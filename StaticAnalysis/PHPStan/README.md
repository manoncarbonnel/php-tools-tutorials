# PHP Stan - PHP Static Analysis Tool

<img src="https://i.imgur.com/MOt7taM.png" alt="PHPStan" width="300" height="300">

[Voir la documentation complète](https://github.com/phpstan/phpstan)

PHPStan se concentre sur la recherche d’erreurs dans le code sans l’exécuter.
Il capture des classes entières de bugs avant même que d'écrire des tests unitaires.
Cela rapproche PHP des langages compilés en ce sens que l’exactitude de chaque ligne du code peut être vérifiée avant d’exécuter la ligne réelle.

PHPStan fonctionne mieux avec un code moderne orienté objet. Plus le code est fortement typé, plus cela fournit d'informations à PHPStan.

Le code correctement annoté et en caractères typés (propriétés de classe, arguments de fonction et de méthode, types de retour) aide non seulement les outils d'analyse statiques, mais également les autres personnes travaillant avec le code pour le comprendre.

**Sommaire**

- [Pré-requis](#pre-requis)
- [Installation](#installation)
    - [Extentions](#extensions)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
- [Lancement](#lancement)

## Pré-requis

Pour installer ces outils, vous aurez besoin de :
- La librairie PHP >= 7.1
    - soit en local votre machine à l'aide d'un [exécutable](https://www.php.net/downloads.php)
    - soit celui d'un conteneur Docker
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

Pour utilisation avec l'IDE Jetbrains [PhpStorm](https://www.jetbrains.com/phpstorm/)
- [NEON support](https://plugins.jetbrains.com/plugin/7060-neon-support) Nette Object Notation plugin

## Installation

``` shell
composer global require phpstan/phpstan
```

Composer installera l'exécutable de PHPStan dans `vendor/bin`.

### Extensions

PHP Stan supporte des extensions (support de frameworks, règles d'analyse).
Liste complète sur la (documentation officielle)[https://github.com/phpstan/phpstan#extensibility]

Il en a par exemple, de très utiles pour travailler avec Symfony.

Exemple d'installation en local sur une machine de dev :

``` shell
composer global require phpstan/phpstan-symfony
composer global require pepakriz/phpstan-exception-rules
composer global require phpstan/phpstan-strict-rules
composer global require phpstan/phpstan-deprecation-rules
composer global require phpstan/phpstan-phpunit
composer global require jangregor/phpstan-prophecy
```

## Configuration

La configuration de PHP Stan se fait dans le fichier `phpstan.neon.dist`

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

En installant les outils en global sur une machine de dev, il est possible de créer un [external tool](https://www.jetbrains.com/help/phpstorm/settings-tools-external-tools.html) dans PHPStorm

Dans `Settings>Tools>External Tools`, créer un nouvel outil avec comme settings :

| Clé | Valeur |
| ------ | ------ |
| Program | C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpstan.bat |
| Arguments | analyse -vvv --level 7 -c phpstan.neon $ProjectFileDir$ |
| Working directory | $ProjectFileDir$ |

## Lancement

``` shell
vendor/bin/phpstan analyse -vvv --level 7 -c phpstan.neon $ProjectFileDir$
```