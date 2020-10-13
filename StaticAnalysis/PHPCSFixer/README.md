# PHP Coding Standards Fixer

[See full documentation](https://github.com/FriendsOfPHP/PHP-CS-Fixer)

The PHP Coding Standards Fixer (PHP CS Fixer) tool fixes your code to follow standards; whether you want to follow PHP coding standards as defined in the PSR-1, PSR-2, etc., or other community driven ones like the Symfony one. You can also define your (team's) style through configuration.

It can modernize your code (like converting the pow function to the ** operator on PHP 5.6) and (micro) optimize it.

If you are already using a linter to identify coding standards problems in your code, you know that fixing them by hand is tedious, especially on large projects. This tool does not only detect them, but also fixes them for you.

**Summary**

- [Prerequisites](#prerequisites)
- [Install](#install)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
- [Usage](#usage)

## Prerequisites

To install those tools, you will need:
- PHP library >= 5.6
    - [local](https://www.php.net/downloads.php)
    - from a Docker container
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

## Install

``` shell
composer global require friendsofphp/php-cs-fixer
```

Composer will install PHPCS Fixer in `vendor/bin`.

## Configuration

The configuration of PHPCS Fixer is located in the file `.php_cs.dist`

[A complete list of rules with dynamic example](https://mlocati.github.io/php-cs-fixer-configurator/#version:2.15) was made by the community but is also available on the [official documentation](https://github.com/FriendsOfPHP/PHP-CS-Fixer#usage).

``` php
<?php

declare(strict_types=1);

$finder = PhpCsFixer\Finder::create()
    ->in(['src'])
    ->exclude([
        'Entity',
        'Migrations'
    ])
    ->notName('Kernel.php')
;

return PhpCsFixer\Config::create()
    ->setUsingCache(false)
    ->setRiskyAllowed(true)
    ->setRules([
        '@DoctrineAnnotation' => true,
        '@PHP71Migration' => true,
        '@PHP71Migration:risky' => true,
        '@PHPUnit60Migration:risky' => true,
        '@PSR1' => true,
        '@PSR2' => true,
        '@Symfony' => true,
        '@PhpCsFixer' => true,
        '@Symfony:risky' => true,
        'align_multiline_comment' => [
            'comment_type' => 'phpdocs_only',
        ],
        'array_indentation' => true,
        'array_syntax' => [
            'syntax' => 'short',
        ],
        'braces' => [
            'allow_single_line_closure' => true,
        ],
        'compact_nullable_typehint' => true,
        'concat_space' => ['spacing' => 'one'],
        'doctrine_annotation_array_assignment' => [
            'operator' => '=',
        ],
        'doctrine_annotation_spaces' => [
            'after_array_assignments_equals' => false,
            'before_array_assignments_equals' => false,
        ],
        'no_extra_blank_lines' => [
            'tokens' => [
                'break',
                'continue',
                'curly_brace_block',
                'extra',
                'parenthesis_brace_block',
                'return',
                'square_brace_block',
                'throw',
                'use',
            ],
        ],
        'no_useless_else' => true,
        'no_useless_return' => true,
        'ordered_imports' => [
            'importsOrder' => [
                'class',
                'function',
                'const',
            ],
            'sortAlgorithm' => 'alpha',
        ],
        'php_unit_method_casing' => [
            'case' => 'camel_case',
        ],
        'phpdoc_order' => true,
        'phpdoc_separation' => true,
        'phpdoc_trim_consecutive_blank_line_separation' => true,
        'phpdoc_types_order' => ['null_adjustment' => 'always_last'],
        'strict_comparison' => true,
        'strict_param' => true,
        'void_return' => true,
        'yoda_style' => true
    ])
    ->setFinder($finder)
;

```

### PhpStorm

See [full documentation](https://www.jetbrains.com/help/phpstorm/using-php-cs-fixer.html) to integrate PHP Code Sniffer into Jetbrains PhpStorm IDE.

#### Quality tools

In `Settings > Languages & Frameworks > PHP > Quality Tools > PHP CS Fixer > Local`

Indicate *PHP CS Fixer path*: `C:\Users\username\AppData\Roaming\Composer\vendor\bin\php-cs-fixer.bat`

#### Editor inspections configuration

In `Settings > Editor > Inspection`

Select: `PHP > Quality Tools`

Check `PHP CS Fixer validation`

#### External tool

By installing the tools globally on a development machine, it is possible to create an [external tool](https://www.jetbrains.com/help/phpstorm/settings-tools-external-tools.html) in PHPStorm

In `Settings>Tools>External Tools`, create a new tool with the following settings:

| Key | Value |
| ------ | ------ |
| Program | C:\Users\username\AppData\Roaming\Composer\vendor\bin\php-cs-fixer.bat |
| Arguments | --verbose --config=$ProjectFileDir$\src\.php_cs.dist fix |
| Working directory | $ProjectFileDir$ |

## Usage

``` shell
vendor/bin/php-cs-fixer --verbose --config=$ProjectFileDir$\src\.php_cs.dist fix
```
