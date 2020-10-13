# PHP Stan - PHP Static Analysis Tool

<img src="https://i.imgur.com/MOt7taM.png" alt="PHPStan" width="300" height="300">

[See full documentation](https://github.com/phpstan/phpstan)

PHPStan focuses on finding errors in your code without actually running it.
It catches whole classes of bugs even before you write tests for the code.
It moves PHP closer to compiled languages in the sense that the correctness of each line of the code can be checked before you run the actual line.

PHPStan works best with modern object-oriented code. The more strongly-typed your code is, the more information you give PHPStan to work with.

Properly annotated and typehinted code (class properties, function and method arguments, return types) helps not only static analysis tools but also other people that work with the code to understand it.

**Summary**

- [Prerequisites](#prerequisites)
- [Install](#install)
    - [Extensions](#extensions)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
- [Usage](#usage)

## Prerequisites

To install those tools, you will need:
- PHP library >= 7.1
    - [local](https://www.php.net/downloads.php)
    - from a Docker container
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

For usage with Jetbrains [PhpStorm](https://www.jetbrains.com/phpstorm/) IDE :
- [NEON support](https://plugins.jetbrains.com/plugin/7060-neon-support) Nette Object Notation plugin

## Install

``` shell
composer global require phpstan/phpstan
```

Composer will install PHPStan in `vendor/bin`.

### Extensions

PHP Stan supports extensions (frameworks support, analysis rules).
Full list on [official documentation](https://github.com/phpstan/phpstan#extensibility)

For example, there are some very useful ones for working with Symfony.

Example of a local installation on a development machine:
``` shell
composer global require phpstan/phpstan-symfony
composer global require pepakriz/phpstan-exception-rules
composer global require phpstan/phpstan-strict-rules
composer global require phpstan/phpstan-deprecation-rules
composer global require phpstan/phpstan-phpunit
composer global require jangregor/phpstan-prophecy
```

## Configuration

The configuration of PHP MD is located in the file `phpstan.neon.dist`

``` neon
includes:
  - %rootDir%\..\phpstan-symfony\extension.neon
  - %rootDir%\..\..\pepakriz\phpstan-exception-rules\extension.neon
  - %rootDir%\..\phpstan-strict-rules\rules.neon
  - %rootDir%\..\phpstan-deprecation-rules\rules.neon
  - %rootDir%\..\phpstan-phpunit\extension.neon
  - %rootDir%\..\..\jangregor\phpstan-prophecy\src\extension.neon

parameters:
  autoload_directories:
    - features/
  autoload_files:
    - bin/.phpunit/phpunit-6.5/vendor/autoload.php
  symfony:
    container_xml_path: 'var/cache/dev/srcApp_KernelDevDebugContainer.xml'
  level: 7
  exceptionRules:
    reportUnusedCatchesOfUncheckedExceptions: false
    ignoreDescriptiveUncheckedExceptions: false
  excludes_analyse:
    - config
    - public
    - tests
    - src/Kernel.php
    - src/Migrations
    - var
    - vendor
```

### PhpStorm

By installing the tools globally on a development machine, it is possible to create an [external tool](https://www.jetbrains.com/help/phpstorm/settings-tools-external-tools.html) in PHPStorm

In `Settings>Tools>External Tools`, create a new tool with the following settings:

| Key | Value |
| ------ | ------ |
| Program | C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpstan.bat |
| Arguments | analyse -vvv --level 7 -c phpstan.neon $ProjectFileDir$ |
| Working directory | $ProjectFileDir$ |

## Usage

``` shell
vendor/bin/phpstan analyse -vvv --level 7 -c phpstan.neon $ProjectFileDir$
```
