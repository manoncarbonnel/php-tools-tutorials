# Prophecy

Prophecy is a highly opinionated yet very powerful and flexible PHP object mocking framework that works with PHPUnit.
Though initially it was created to fulfil phpspec2 needs, it is flexible enough to be used inside any testing framework out there with minimal effort.

Tests should be written according to the AAA (Arrange-Act-Assert) pattern.

[See full documentation](https://github.com/phpspec/prophecy)

**Summary**

- [Prerequisites](#prerequisites)
- [Install](#install)
- [Usage](#usage)
    - [Write your tests](#write-your-tests)
        - [Dummies](#dummies)
        - [Stubs](#stubs)
        - [Promises](#promises)
        - [Mocks](#mocks)
        - [Spies](#spies)
    - [Running the tests](#running-the-tests)
    
## Prerequisites

To install those tools, you will need:
- PHP library >= 7.2
    - [local](https://www.php.net/downloads.php)
    - from a Docker container
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)
- [PHPUnit](../PHPUnit/README.md)

## Install

``` shell
composer req --dev phpspec/prophecy
```

Composer will install PHP Code Sniffer in `vendor/bin`.

## Usage

### Write your tests

You wan now write your tests. Example:

```php
<?php

class UserTest extends PHPUnit\Framework\TestCase
{
    private $prophet;

    public function testPasswordHashing()
    {
        $hasher = $this->prophet->prophesize('App\Security\Hasher');
        $user   = new App\Entity\User($hasher->reveal());

        $hasher->generateHash($user, 'qwerty')->willReturn('hashed_pass');

        $user->setPassword('qwerty');

        $this->assertEquals('hashed_pass', $user->getPassword());
    }

    protected function setUp()
    {
        $this->prophet = new \Prophecy\Prophet;
    }

    protected function tearDown()
    {
        $this->prophet->checkPredictions();
    }
}
```

First of, create an `object prophecy` :

```php
$prophet = new Prophecy\Prophet;
$prophecy = $prophet->prophesize();

// optionnal : describe your prophecy object
$prophecy->willExtend('stdClass');
$prophecy->willImplement('SessionHandlerInterface');
```

#### Dummies

```php
$dummy = $prophecy->reveal();
```

#### Stubs

```php
$prophecy->read('123')->willReturn('value');
```

#### Promises

```php
$prophecy->read('123')->will(new Prophecy\Promise\ReturnPromise(array('value')));
```

#### Mocks

```php
$entityManager->flush()->shouldBeCalled();
$entityManager->flush()->should(new Prophecy\Prediction\CallPrediction());
$prophet->checkPredictions();
```

#### Spies

```php
$em = $prophet->prophesize('Doctrine\ORM\EntityManager');

$controller->createUser($em->reveal());

$em->flush()->shouldHaveBeenCalled();
```

### Running the tests

See [PHPUnit README.md](../PHPUnit/README.md#running-the-tests)
