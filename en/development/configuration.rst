Configuration
#############

Configuring a CakePHP application is a piece of cake. After you
have installed CakePHP, creating a basic web application requires
only that you setup a database configuration.

There are, however, other optional configuration steps you can take
in order to take advantage of CakePHP flexible architecture. You
can easily add to the functionality inherited from the CakePHP
core, configure additional/different URL mappings (routes), and
define additional/different inflections.

.. index:: database.php, database.php.default

Database Configuration
======================

CakePHP expects database configuration details to be in a file at
``app/config/database.php``. An example database configuration file can
be found at ``app/config/database.php.default``. A finished
configuration should look something like this::

    <?php
    var $default = array(
        'datasource'  => 'Database/Mysql',
        'persistent'  => false,
        'host'        => 'localhost',
        'login'       => 'cakephpuser',
        'password'    => 'c4k3roxx!',
        'database'    => 'my_cakephp_project',
        'prefix'      => ''
    );

The $default connection array is used unless another connection is
specified by the $useDbConfig property in a model. For example, if
my application has an additional legacy database in addition to the
default one, I could use it in my models by creating a new $legacy
database connection array similar to the $default array, and by
setting var $useDbConfig = ‘legacy’; in the appropriate models.

Fill out the key/value pairs in the configuration array to best
suit your needs.

Key
    Value
datasource
    The name of the datasource this configuration array is for.
    Examples: Database/Mysql, Database/Postgres, Database/Sqlite.  
    You can use :term:`plugin syntax` to indicate plugin datasource to use.
persistent
    Whether or not to use a persistent connection to the database.
host
    The database server’s hostname (or IP address).
login
    The username for the account.
password
    The password for the account.
database
    The name of the database for this connection to use.
prefix (*optional*)
    The string that prefixes every table name in the database. If your
    tables don’t have prefixes, set this to an empty string.
port (*optional*)
    The TCP port or Unix socket used to connect to the server.
encoding
    Indicates the character set to use when sending SQL statements to
    the server. This defaults to the database's default encoding for
    all databases other than DB2. If you wish to use UTF-8 encoding
    with mysql/mysqli connections you must use 'utf8' without the
    hyphen.
schema
    Used in PostgreSQL database setups to specify which schema to use.
datasource
    non-DBO datasource to use, e.g. 'ldap', 'twitter'
    
.. note::

    The prefix setting is for tables, **not** models. For example, if
    you create a join table for your Apple and Flavor models, you name
    it prefix\_apples\_flavors (**not**
    prefix\_apples\_prefix\_flavors), and set your prefix setting to
    'prefix\_'.

At this point, you might want to take a look at the
:doc:`/getting-started/cakephp-conventions`. The correct
naming for your tables (and the addition of some columns) can score
you some free functionality and help you avoid configuration. For
example, if you name your database table big\_boxes, your model
BigBox, your controller BigBoxesController, everything just works
together automatically. By convention, use underscores, lower case,
and plural forms for your database table names - for example:
bakers, pastry\_stores, and savory\_cakes.

Additional Class Paths
======================

It’s occasionally useful to be able to share MVC classes between
applications on the same system. If you want the same controller in
both applications, you can use CakePHP’s bootstrap.php to bring
these additional classes into view.

In bootstrap.php, define some specially-named variables to make
CakePHP aware of other places to look for MVC classes:

::

    <?php
    App::build(array(
        'plugins' => array('/full/path/to/plugins/', '/next/full/path/to/plugins/'),
        'Model' =>  array('/full/path/to/models/', '/next/full/path/to/models/'),
        'View' => array('/full/path/to/views/', '/next/full/path/to/views/'),
        'Controller' => array('/full/path/to/controllers/', '/next/full/path/to/controllers/'),
        'Model/Datasource' => array('/full/path/to/datasources/', '/next/full/path/to/datasources/'),
        'Model/Behavior' => array('/full/path/to/behaviors/', '/next/full/path/to/behaviors/'),
        'Contorller/Component' => array('/full/path/to/components/', '/next/full/path/to/components/'),
        'View/Helper' => array('/full/path/to/helpers/', '/next/full/path/to/helpers/'),
        'vendors' => array('/full/path/to/vendors/', '/next/full/path/to/vendors/'),
        'Console/Command' => array('/full/path/to/shells/', '/next/full/path/to/shells/'),
        'locales' => array('/full/path/to/locale/', '/next/full/path/to/locale/'),
        'libs' => array('/full/path/to/libs/', '/next/full/path/to/libs/')
    ));

All additional path configuration should be done at the top of your application's
bootstrap.php. This will ensure that the paths are available for the rest of your
application.


.. index:: core.php, configuration

Core Configuration
==================

Each application in CakePHP contains a configuration file to 
determine CakePHP's internal behavior.
``app/config/core.php``. This file is a collection of Configure class
variable definitions and constant definitions that determine how
your application behaves. Before we dive into those particular
variables, you’ll need to be familiar with :php:class:`Configure`, CakePHP’s
configuration registry class.

CakePHP Core Configuration
--------------------------

The Configure class is used to manage a set of core CakePHP
configuration variables. These variables can be found in
``app/config/core.php``. Below is a description of each variable and
how it affects your CakePHP application.

debug
    Changes CakePHP debugging output.
    0 = Production mode. No output.
    1 = Show errors and warnings.
    2 = Show errors, warnings, and SQL. [SQL log is only shown when you
    add $this->element('sql\_dump') to your view or layout.]

Error
    Configure the Error handler used to handle errors for your application.  
    By default :php:meth:`ErrorHandler::handleError()` is used.  It will display 
    errors using :php:class:`Debugger`, when debug > 0
    and log errors with :php:class:`CakeLog` when debug = 0.
    
    Sub-keys:
    
    * ``handler`` - callback - The callback to handle errors. You can set this to any 
      callback type, including anonymous functions.
    * ``level`` - int - The level of errors you are interested in capturing.
    * ``trace`` - boolean - Include stack traces for errors in log files.

Exception
    Configure the Exception handler used for uncaught exceptions.  By default, 
    ErrorHandler::handleException() is used. It will display a HTML page for 
    the exception, and while debug > 0, framework errors like 
    Missing Controller will be displayed.  When debug = 0, 
    framework errors will be coerced into generic HTTP errors.
    For more information on Exception handling, see the :doc:`exceptions`
    section.

App.baseUrl
    Un-comment this definition if you **don’t** plan to use Apache’s
    mod\_rewrite with CakePHP. Don’t forget to remove your .htaccess
    files too.
App.encoding
    Define what encoding your application uses.  This encoding
    is used to generate the charset in the layout, and encode entities.
    It should match the encoding values specified for your database.
Routing.prefixes
    Un-comment this definition if you’d like to take advantage of
    CakePHP prefixed routes like admin. Set this variable with an array
    of prefix names of the routes you’d like to use. More on this
    later.
Cache.disable
    When set to true, persistent caching is disabled site-wide.
    This will make all read/writes to :php:class:`Cache` fail.
Cache.check
    If set to true, enables view caching. Enabling is still needed in
    the controllers, but this variable enables the detection of those
    settings.
Session
    Contains an array of settings to use for session configuration. The defaults key is 
    used to define a default preset to use for sessions, any settings declared here will override
    the settings of the default config.
    
    Sub-keys
    
    * ``name`` - The name of the cookie to use. Defaults to 'CAKEPHP'
    * ``timeout`` - The number of minutes you want sessions to live for. 
      This timeout is handled by CakePHP
    * ``cookieTimeout`` - The number of minutes you want session cookies to live for.
    * ``checkAgent`` - Do you want the user agent to be checked when starting sessions? 
      You might want to set the value to false, when dealing with older versions of 
      IE, Chrome Frame or certain web-browsing devices and AJAX
    * ``defaults`` - The default configuration set to use as a basis for your session.
      There are four builtins: php, cake, cache, database.
    * ``handler`` - Can be used to enable a custom session handler. 
      Expects an array of callables, that can be used with `session_save_handler`.  
      Using this option will automatically add `session.save_handler` to the ini array.
    * ``autoRegenerate`` - Enabling this setting, turns on automatic renewal 
      of sessions, and sessionids that change frequently. 
      See :php:attr:`CakeSession::$requestCountdown`.
    * ``ini`` - An associative array of additional ini values to set.
    
    The built in defaults are:

    * 'php' - Uses settings defined in your php.ini.
    * 'cake' - Saves session files in CakePHP's /tmp directory.
    * 'database' - Uses CakePHP's database sessions.
    * 'cache' - Use the Cache class to save sessions.
    
    To define a custom session handler, save it at ``app/libs/session/<name>.php``.
    Make sure the class implements :php:interface:`CakeSessionHandlerInterface` 
    and set Session.handler to <name>

    To use database sessions, run the ``app/config/schema/sessions.php`` schema using
    the cake shell command: ``cake schema create Sessions``

Security.level
    The level of CakePHP security. The session timeout time defined in
    'Session.timeout' is multiplied according to the settings here.
    Valid values:
    'high' = x 10
    'medium' = x 100
    'low' = x 300
    'high' and 'medium' also enable
    `session.referer\_check <http://www.php.net/manual/en/session.configuration.php#ini.session.referer-check>`_
    CakePHP session IDs are also regenerated between requests if
    'Security.level' is set to 'high'.
Security.salt
    A random string used in security hashing.
Security.cipherSeed
    A random numeric string (digits only) used to encrypt/decrypt
    strings.
Asset.timestamp
    Appends a timestamp which is last modified time of the particular
    file at the end of asset files urls (CSS, JavaScript, Image) when
    using proper helpers.
    Valid values:
    (bool) false - Doesn't do anything (default)
    (bool) true - Appends the timestamp when debug > 0
    (string) 'force' - Appends the timestamp when debug >= 0
Acl.classname, Acl.database
    Constants used for CakePHP’s Access Control List functionality. See
    the Access Control Lists chapter for more information.

.. note::
    Cache configuration is also found in core.php — We’ll be covering
    that later on, so stay tuned.

The :php:class:`Configure` class can be used to read and write core
configuration settings on the fly. This can be especially handy if
you want to turn the debug setting on for a limited section of
logic in your application, for instance.

Configuration Constants
-----------------------

While most configuration options are handled by Configure, there
are a few constants that CakePHP uses during runtime.

.. php:const:: LOG_ERROR

    Error constant. Used for differentiating error logging and
    debugging. Currently PHP supports LOG\_DEBUG.

Core Cache Configuration
------------------------

CakePHP uses two cache configurations internally.  ``_cake_model_`` and ``_cake_core_``.
``_cake_core_`` is used to store file paths, and object locations.  ``_cake_model_`` is
used to store schema descriptions, and source listings for datasources.  Using a fast
cache storage like APC or Memcached is recommended for these configurations, as
they are read on every request.  By default both of these configurations expire every
10 seconds when debug is greater than 0.

As with all cached data stored in :php:class:`Cache` you can clear data using
:php:meth:`Cache::clear()`.


Configure Class
===============

.. php:class:: Configure

Despite few things needing to be configured in CakePHP, it’s
sometimes useful to have your own configuration rules for your
application. In the past you may have defined custom configuration
values by defining variable or constants in some files. Doing so
forces you to include that configuration file every time you needed
to use those values.

CakePHP’s new Configure class can be used to store and retrieve
application or runtime specific values. Be careful, this class
allows you to store anything in it, then use it in any other part
of your code: a sure temptation to break the MVC pattern CakePHP
was designed for. The main goal of Configure class is to keep
centralized variables that can be shared between many objects.
Remember to try to live by "convention over configuration" and you
won't end up breaking the MVC structure we’ve set in place.

This class can be called from
anywhere within your application, in a static context::

    <?php Configure::read('debug'); ?>

.. php:staticmethod:: write($key, $value)
    
    :param string $key: The key to write, can use be a :term:`dot notation` value.
    :param mixed $value: The value to store.

    Use ``write()`` to store data in the application’s configuration::

        <?php
        Configure::write('Company.name','Pizza, Inc.');
        Configure::write('Company.slogan','Pizza for your body and soul');

    .. note::

        The :term:`dot notation` used in the ``$key`` parameter can be used to
        organize your configuration settings into logical groups.

    The above example could also be written in a single call::

        <?php
        Configure::write(
            'Company',array('name'=>'Pizza, Inc.','slogan'=>'Pizza for your body and soul')
        );

    You can use ``Configure::write('debug', $int)`` to switch between
    debug and production modes on the fly. This is especially handy for
    AMF or SOAP interactions where debugging information can cause
    parsing problems.

.. php:staticmethod:: read($key = null)

    :param string $key: The key to read, can use be a :term:`dot notation` value

    Used to read configuration data from the application. Defaults to
    CakePHP’s important debug value. If a key is supplied, the data is
    returned. Using our examples from write() above, we can read that
    data back::
        
        <?php
        Configure::read('Company.name');    //yields: 'Pizza, Inc.'
        Configure::read('Company.slogan');  //yields: 'Pizza for your body and soul'
     
        Configure::read('Company');
     
        //yields: 
        array('name' => 'Pizza, Inc.', 'slogan' => 'Pizza for your body and soul');
    
    If $key is left null, all values in Configure will be returned.

.. php:staticmethod:: delete($key)

    :param string $key: The key to delete, can use be a :term:`dot notation` value

    Used to delete information from the application’s configuration::

        <?php
        Configure::delete('Company.name');

.. php:staticmethod:: version()

    Returns the CakePHP version for the current application.

.. php:staticmethod:: config($name, $reader)

    :param string $name: The name of the reader being attached.
    :param ConfigReaderInterface $reader: The reader instance being attached.
    
    Attach a configuration reader to Configure.  Attached readers can
    then be used to load configuration files. See :ref:`loading-configuration-files`
    for more information on how to read configuration files.

.. php:staticmethod:: configured($name = null)
    
    :param string $name: The name of the reader to check, if null
        a list of all attached readers will be returned.
    
    Either check that a reader with a given name is attached, or get
    the list of attached readers.

.. php:staticmethod:: drop($name)

    Drops a connected reader object.

.. _loading-configuration-files:

Loading configuration files
===========================

CakePHP comes with two built-in configuration file readers.  
:php:class:`PhpReader` is able to read PHP config files, in the same 
format that Configure has historically read.  :php:class:`IniReader` is
able to read ini config files.  See the `PHP documentation <http://php.net/parse_ini_file>`_ 
for more information on the specifics of ini files. 
To use a core config reader, you'll need to attach it to Configure 
using :php:meth:`Configure::config()`::

    <?php
	App::uses('PhpReader', 'Configure');
	// Read config files from app/config
	Configure::config('default', new PhpReader());
	
	// Read config files from another path.
	Configure::config('default', new PhpReader('/path/to/your/config/files/'));

You can have multiple readers attached to Configure, each reading
different kinds of configuration files, or reading from 
different types of sources.  You can interact with attached readers 
using a few other methods on Configure. To see check which reader 
aliases are attached you can use :php:meth:`Configure::configured()`::

    <?php
	// Get the array of aliases for attached readers.
	Configure::configured()
	
	// Check if a specific reader is attached
	Configure::configured('default');

You can also remove attached readers.  ``Configure::drop('default')``
would remove the default reader alias. Any future attempts to load configuration 
files with that reader would fail.

.. php:staticmethod:: load($key, $config = 'default', $merge = true)
    
    :param string $key: The identifier of the configuration file to load.
    :param string $config: The alias of the configured reader.
    :param boolean $merge: Whether or not the contents of the read file
        should be merged, or overwrite the existing values.

Once you've attached a config reader to Configure you can load configuration files::

    <?php
	// Load my_file.php using the 'default' reader object.
	Configure::load('my_file', 'default');

Loaded configuration files merge their data with the existing runtime configuration 
in Configure.  This allows you to overwrite and add new values 
into the existing runtime configuration. By setting ``$merge`` to true, values
will not ever overwrite the existing configuration.

Storing runtime configuration
-----------------------------

.. php:staticmethod:: store($name, $cacheConfig = 'default', $data = null)

    :param string $name: The storage key for the cache file.
    :param string $cacheConfig: The name of the cache configuration to store the
        configuration data with.
    :param mixed $data: Either the data to store, or leave null to store all data
        in Configure.

You can also store runtime configuration values for use in a future request.  
Since configure only remembers values for the current request, you will 
need to store any modified configuration information if you want to 
use it in subsequent requests::

    <?php
	// Store the current configuration in the 'user_1234' key in the 'default' cache.
	Configure::store('user_1234', 'default');

Stored configuration data is persisted in the :php:class:`Cache` class. This allows 
you to store Configuration information in any storage engine that :php:class:`Cache` can talk to.

Restoring runtime configuration
-------------------------------

.. php:staticmethod:: restore($name, $cacheConfig = 'default')

    :param string $name: The storage key to load.
    :param string $cacheConfig: The cache configuration to load the data from.

Once you've stored runtime configuration, you'll probably need to restore it 
so you can access it again.  ``Configure::restore()`` does exactly that::

    <?php
	// restore runtime configuration from the cache.
	Configure::restore('user_1234', 'default');

When restoring configuration information its important to restore it with
the same key, and cache configuration as was used to store it.  Restored 
information is merged on top of the existing runtime configuration.

Creating your own Configuration readers
=======================================

Since configuration readers are an extensible part of CakePHP, 
you can create configuration readers in your application and plugins.  
Configuration readers need to implement the :php:interface:`ConfigReaderInterface`.  
This interface defines a read method, as the only required method. 
If you really like XML files, you could create a simple Xml config 
reader for you application::

    <?php
	// in app/Lib/Config/XmlReader.php
	App::uses('Xml', 'Utility');
	class XmlReader implements ConfigReaderInterface {
		function __construct($path = CONFIGS) {
			$this->_path = $path;
		}
		
		function read($key) {
			$xml = Xml::build($this->_path . $key . '.xml');
			return Xml::toArray($xml);
		}
	}

In your ``app/config/bootstrap.php`` you could attach this reader and use it::

    <?php
	App::uses('XmlReader', 'Lib/Config');
	Configure::config('xml', new XmlReader());
	...
	
	Configure::load('my_xml');

The ``read()`` method of a config reader, must return an array of the configuration information 
that the resource named ``$key`` contains.

.. php:interface:: ConfigReaderInterface

    Defines the interface used by classes that read configuration data and
    store it in :php:class:`Configure`

.. php:method:: read($key)

    :param string $key: The key name or identifier to load.
    
    This method should load/parse the configuration data identified by ``$key``
    and return an array of data in the file.

.. php:exception:: ConfigureException

    Thrown when errors occur when loading/storing/restoring configuration data.
    :php:interface:`ConfigReaderInterface` implementations should throw this
    error when they encounter an error.

Built-in Configuration readers
------------------------------

.. php:class:: PhpReader

    Allows you to read configuration files that are stored as plain PHP files.
    You can read either files from your ``app/configs`` or from plugin configs
    directories by using :term:`plugin syntax`.  Files **must** contain a ``$config``
    variable.  An example configuration file would look like::
    
        <?php
        $config = array(
            'debug' => 0,
            'Security' => array(
                'salt' => 'its-secret'
            ),
            'Exception' => array(
                'handler' => 'ErrorHandler::handleException',
                'renderer' => 'ExceptionRenderer',
                'log' => true
            )
        );
    
    Files without ``$config`` will cause an :php:exc:`ConfigureException`
    
.. php:class:: IniReader

    Allows you to read configuration files that are stored as plain .ini files.
    The ini files must be compatible with php's ``parse_ini_file`` function, and 
    benefit from the following improvements
    
    * dot separated values are expanded into arrays.
    * boolean-ish values like 'on' and 'off' are converted to booleans.
    
    An example ini file would look like::
    
        debug = 0
        
        Security.salt = its-secret
        
        [Exception]
        handler = ErrorHandler::handleException
        renderer = ExceptionRenderer
        log = true
    
    The above ini file, would result in the same end configuration data
    as the PHP example above.  Array structures can be created either
    through dot separated values, or sections.  Sections can contain
    dot separated keys for deeper nesting.

.. _inflection-configuration:

Inflection Configuration
========================

Cake's naming conventions can be really nice - you can name your
database table big\_boxes, your model BigBox, your controller
BigBoxesController, and everything just works together
automatically. The way CakePHP knows how to tie things together is
by *inflecting* the words between their singular and plural forms.

There are occasions (especially for our non-English speaking
friends) where you may run into situations where CakePHP's
inflector (the class that pluralizes, singularizes, camelCases, and
under\_scores) might not work as you'd like. If CakePHP won't
recognize your Foci or Fish, you can tell CakePHP about your
special cases.

Loading custom inflections
--------------------------

You can use :php:meth:`Inflector::rules()` in the file
``app/config/bootstrap.php`` to load custom inflections::

    <?php
    Inflector::rules('singular', array(
        'rules' => array('/^(bil)er$/i' => '\1', '/^(inflec|contribu)tors$/i' => '\1ta'),
        'uninflected' => array('singulars'),
        'irregular' => array('spins' => 'spinor')
    ));

or::

    <?php
    Inflector::rules('plural', array('irregular' => array('phylum' => 'phyla')));

Will merge the supplied rules into the inflection sets defined in
cake/libs/inflector.php, with the added rules taking precedence
over the core rules.

Bootstrapping CakePHP
=====================

If you have any additional configuration needs, use CakePHP’s
bootstrap file, found in app/config/bootstrap.php. This file is
executed just after CakePHP’s core bootstrapping.

This file is ideal for a number of common bootstrapping tasks:

- Defining convenience functions.
- Registering global constants.
- Defining additional model, view, and controller paths.
- Creating cache configurations.
- Configuring inflections.
- Loading configuration files.

Be careful to maintain the MVC software design pattern when you add
things to the bootstrap file: it might be tempting to place
formatting functions there in order to use them in your
controllers.

Resist the urge. You’ll be glad you did later on down the line.

You might also consider placing things in the :php:class:`AppController` class.
This class is a parent class to all of the controllers in your
application. :php:class:`AppController` is a handy place to use controller
callbacks and define methods to be used by all of your
controllers.
