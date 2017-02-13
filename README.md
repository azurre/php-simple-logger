SimpleLog
=====================

### Powerful PSR-3 logging. So easy, it's simple!

SimpleLog is a powerful PSR-3 logger for PHP that is simple to use.

Simplicity is achieved by providing great defaults. No options to configure! Yet flexible enough to meet most logging needs.
And if your application's logging needs expand beyond what SimpleLog provides, since it implements PSR-3, you can drop in
another great PSR-3 logger like MonoLog in its place when the time comes with minimal changes.

It is actively under development with development (0.y.z) releases.

Features
--------

 * Power and Simplicity
 * PSR-3 logger interface
 * Multiple log level severities
 * Log channels
 * Process ID logging
 * Custom log messages
 * Custom contextual data
 * Exception logging

Setup
-----

 Add the library to your `composer.json` file in your project:

```javascript
{
  "require": {
      "markrogoyski/simplelog": "0.*"
  }
}
```

Use [composer](http://getcomposer.org) to install the library:

```bash
$ php composer.phar install
```

Composer will install SimpleLog inside your vendor folder. Then you can add the following to your
.php files to use the library with Autoloading.

```php
require_once(__DIR__ . '/vendor/autoload.php');
```

### Minimum Requirements
 * PHP 7

Usage
-----

### Simple 20-Second Getting-Started Tutorial
```php
$logfile = '/path/to/logfile.log';
$channel = 'events';
$logger  = new SimpleLog\Logger($logfile, $channel);

$logger->info('This is going to be logged as informational.');
```

That's it! Your application is logging!

### Extended Example
```php
$logfile = '/var/log/events.log';
$channel = 'billing';
$logger  = new SimpleLog\Logger($logfile, $channel);

$logger->info('Begin process that usually fails.', ['process' => 'invoicing', 'user' => $user]);

try {
    invoiceUser($user); // This usually fails
} catch (\Exception $e) {
    $logger->error('Billing failure.', ['process' => 'invoicing', 'user' => $user, 'exception' => $e]);
}
```

Logger output
```
2017-02-13 00:35:55.426630  [info]  [billing] [pid:17415] Begin process that usually fails. {"process":"invoicing","user":"bob"}  {}
2017-02-13 00:35:55.430071  [error] [billing] [pid:17415] Billing failure.  {"process":"invoicing","user":"bob"}  {"message":"Could not process invoice.","code":0,"file":"/path/to/app.php","line":20,"trace":[{"file":"/path/to/app.php","line":13,"function":"invoiceUser","args":["mark"]}]}
```

### Log Output
Log lines have the following format:
```
YYYY-MM-DD HH:mm:SS.uuuuuu [loglevel] [channel]  [pid:##] Log content {"Optional":"JSON Contextual Support Data"} {"Optional":"Exception Data"}
```

Log lines are easily readable and parsable. Log lines are always on a single line. Fields are tab seprated.

### Log Levels

SimpleLog has eight log level severities based on [PSR Log Levels](http://www.php-fig.org/psr/psr-3/#psrlogloglevel).

```php
$logger->debug('Detailed information about the application run.');
$logger->info('Informational messages about the application run.');
$logger->notice('Normal but significant events.');
$logger->warning('Information that something potentially bad has occured.');
$logger->error('Runtime error that should be monitored.');
$logger->critical('A service is unavailable or unresponsive.');
$logger->alert('The entire site is down.');
$logger->emergency('The Web site is on fire.');
```

By default all log levels are logged. The minimum log level can be changed in two ways:
 * Optional constructor parameter
 * Setter method at any time

```php
use Psr\Log\LogLevel;

// Optional constructor Parameter (Only error and above are logged [error, critical, alert, emergency])
$logger = new SimpleLog\Logger($logfile, $channel, LogLevel::ERROR);

// Setter method (Only warning and above are logged)
$logger->setLogLevel(LogLevel::WARNING);
```

### Contextual data
SimpleLog enables logging best practices to have general-use log messages with contextual support data to give context to the message.

The second argument to a log message is an associative array of key-value pairs that will log as a JSON string, serving as the contextual support data to the log message.

```php
// 
$log->info('Web request initiated', ['method' => 'GET', 'endpoint' => 'user/account', 'queryParameters' => 'id=1234']);

$log->warning('Free space is below safe threshold.', ['volume' => '/var/log', 'availablePercent' => 4]);
```

Unit Tests
----------

```bash
$ cd tests
$ phpunit
```

Standards
---------

SimpleLog conforms to the following standards:

 * PSR-3 - Logger Interface (http://www.php-fig.org/psr/psr-3/)

License
-------

SimpleLog is licensed under the MIT License.
