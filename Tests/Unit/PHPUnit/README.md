![PHPUnit](https://phpunit.de/img/phpunit.png)

PHPUnit is a programmer-oriented testing framework for PHP.
It is an instance of the [xUnit](https://xunit.net/) architecture for unit testing frameworks.

Tests should be written according to the AAA (Arrange-Act-Assert) pattern.

[See full documentation](https://phpunit.readthedocs.io/)

**Summary**

- [Prerequisites](#prerequisites)
- [Install](#install)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
- [Usage](#usage)
    - [Write your tests](#write-your-tests)
    - [Data Fixtures](#data-fixtures)
    - [Running the tests](#running-the-tests)
    - [Generate a report](#generate-a-report)
        - [Text report](#text-report)
        - [HTML report](#html-report)
    
## Prerequisites

To install those tools, you will need:
- PHP library
    - [local](https://www.php.net/downloads.php)
    - from a Docker container
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

## Install

``` shell
composer req --dev phpunit/phpunit
```

Composer will install PHP Code Sniffer in `vendor/bin`.

## Configuration

### PhpStorm

See [full documentation](https://www.jetbrains.com/help/phpstorm/using-phpunit-framework.html) to integrate PHPUnit into Jetbrains PhpStorm IDE.

#### Test frameworks

In `Settings > Languages & Frameworks > PHP > Test frameworks` add a new PHPUnit (local or remote depending on your environment)

Choose the PHPUnit library installation type

## Usage

### Write your tests

You wan now write your tests. Example:

```php
<?php
use PHPUnit\Framework\TestCase;

class StackTest extends TestCase
{
    public function testPushAndPop()
    {
        $stack = [];
        $this->assertSame(0, count($stack));

        array_push($stack, 'foo');
        $this->assertSame('foo', $stack[count($stack)-1]);
        $this->assertSame(1, count($stack));

        $this->assertSame('foo', array_pop($stack));
        $this->assertSame(0, count($stack));
    }
}
```

### Data fixtures

To run tests of an app, you might need reliable and clean data in your database.

[See documentation](https://phpunit.readthedocs.io/en/latest/fixtures.html)

The PHPUnit `setUp()` and `tearDown()` template methods are run once for each test method (and on fresh instances) of the test case class.

In addition, the `setUpBeforeClass()` and `tearDownAfterClass()` template methods are called before the first test of the test case class is run and after the last test of the test case class is run, respectively.

### Running the tests

**Using a local PHP**

``` shell
php vendor/bin/phpunit <directory>
```

**Using Docker**

```PowerShell
docker-compose exec backend vendor/bin/phpunit <directory>
```

### Generate a report

#### Text report

It is possible to generate a text report by adding those parameters to the command `--coverage-text`

#### HTML report

It is possible to generate a text report by adding those parameters to the command `--coverage-html`
