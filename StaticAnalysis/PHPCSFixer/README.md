# PHP Coding Standards Fixer

[Voir la documentation complète](https://github.com/FriendsOfPHP/PHP-CS-Fixer)

L'outil PHP Coding Standards Fixer (PHP CS Fixer) corrige le code pour qu'il respecte des standards; que ce soient les standards de codage PHP telles que définies dans le PSR-1, le PSR-2, etc., ou d’autres règles telles que celles de Symfony, gérées par la communauté. Toutes les règles sont personnalisables.

Il peut moderniser le code et l’optimiser.

Si vous utilisez déjà un linter pour identifier les problèmes de standards de codage dans votre code, vous savez que les résoudre à la main est fastidieux, en particulier pour les grands projets. Cet outil ne les détecte pas seulement, mais les corrige également pour vous.

**Sommaire**

- [Pré-requis](#pre-requis)
- [Installation](#installation)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
- [Lancement](#lancement)

## Pré-requis

Pour installer ces outils, vous aurez besoin de :
- La librairie PHP >= 5.6
    - soit en local votre machine à l'aide d'un [exécutable](https://www.php.net/downloads.php)
    - soit celui d'un conteneur Docker
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

## Installation

``` shell
composer global require friendsofphp/php-cs-fixer
```

Composer installera l'exécutable de PHPCS Fixer dans `vendor/bin`.

## Configuration

La configuration de PHPCS Fixer se fait dans le fichier `.php_cs.dist`

[Une liste complète des règles avec exemple dynamiques](https://mlocati.github.io/php-cs-fixer-configurator/#version:2.15) a été faite par la communauté mais est aussi disponible sur la [documentation officielle](https://github.com/FriendsOfPHP/PHP-CS-Fixer#usage).

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

Voir la [documentation officielle](https://www.jetbrains.com/help/phpstorm/using-php-cs-fixer.html) pour intégrer PHP Coding Standards Fixer à l'IDE Jetbrains PhpStorm.

#### Quality tools

Dans `Settings > Languages & Frameworks > PHP > Quality Tools > PHP CS Fixer > Local`

Indiquer *PHP CS Fixer path* : `C:\Users\username\AppData\Roaming\Composer\vendor\bin\php-cs-fixer.bat`

#### Editor inspections configuration

Dans `Settings > Editor > Inspection`

Selectionner : `PHP > Quality Tools`
Cocher `PHP CS Fixer validation`

#### External tool

En installant les outils en global sur une machine de dev, il est possible de créer un [external tool](https://www.jetbrains.com/help/phpstorm/settings-tools-external-tools.html) dans PHPStorm

Dans `Settings>Tools>External Tools`, créer un nouvel outil avec comme settings :

| Clé | Valeur |
| ------ | ------ |
| Program | C:\Users\username\AppData\Roaming\Composer\vendor\bin\php-cs-fixer.bat |
| Arguments | --verbose --config=$ProjectFileDir$\src\.php_cs.dist fix |
| Working directory | $ProjectFileDir$ |

## Lancement

``` shell
vendor/bin/php-cs-fixer --verbose --config=$ProjectFileDir$\src\.php_cs.dist fix
```