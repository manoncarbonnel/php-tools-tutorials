# PHP Code Sniffer

[See full documentation](https://github.com/squizlabs/PHP_CodeSniffer)

PHP_CodeSniffer is a set of two PHP scripts; the main `phpcs` script that tokenizes PHP, JavaScript and CSS files to detect violations of a defined coding standard, and a second `phpcbf` script to automatically correct coding standard violations.

PHP_CodeSniffer is an essential development tool that ensures your code remains clean and consistent.

**Summary**

- [Prerequisites](#prerequisites)
- [Install](#install)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
- [Usage](#usage)

## Prerequisites

To install those tools, you will need:
- PHP library >= 5.4
    - [local](https://www.php.net/downloads.php)
    - from a Docker container
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

## Install

``` shell
composer global require squizlabs/php_codesniffer
```

Composer will install PHP Code Sniffer in `vendor/bin`.

## Configuration

### PhpStorm

See [full documentation](https://www.jetbrains.com/help/phpstorm/using-php-code-sniffer.html) to integrate PHP Code Sniffer into Jetbrains PhpStorm IDE.

#### Quality tools

In `Settings > Languages & Frameworks > PHP > Quality Tools > Code Sniffer > Local`

Indicate *PHP Code Sniffer path*: `C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpcs.bat`

#### Editor inspections configuration

In `Settings > Editor > Inspection`

Select: `PHP > Quality Tools`

Check `PHP Code Sniffer validation`

#### External tool

By installing the tools globally on a development machine, it is possible to create an [external tool](https://www.jetbrains.com/help/phpstorm/settings-tools-external-tools.html) in PHPStorm

In `Settings > Tools > External Tools`, create a new tool with the following settings:

| Key | Value |
| ------ | ------ |
| Program | C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpcs.bat |
| Arguments | -s --report=code --report-file=$ProjectFileDir$\var\codesniffer $ProjectFileDir$\src
| Working directory | $ProjectFileDir$ |

## Usage

``` shell
vendor/bin/phpcs -s --report=code --report-file=$ProjectFileDir$\var\codesniffer $ProjectFileDir$\src
```

It is also possible to specify the standard to be used. Example with the PSR-2:

``` shell
vendor/bin/phpcs -s --standard=PSR2 --report=code --report-file=$ProjectFileDir$\var\codesniffer $ProjectFileDir$\src
```
