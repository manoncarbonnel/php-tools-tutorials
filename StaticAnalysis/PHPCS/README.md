# PHP Code Sniffer

[Voir la documentation complète](https://github.com/squizlabs/PHP_CodeSniffer)

PHP Code Sniffer est un ensemble de deux scripts PHP; le script `phpcs` principal qui segmente les fichiers PHP, JavaScript et CSS pour détecter les violations d'une norme de codage définie, et un second script `phpcbf` pour corriger automatiquement les violations des standards de codage.

PHP CodeSniffer est un outil de développement essentiel qui garantit que le code reste propre et cohérent. 
Le standard de codage utilisé par PHP CodeSniffer est le standard de codage PEAR.

**Sommaire**

- [Pré-requis](#pre-requis)
- [Installation](#installation)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
- [Lancement](#lancement)

## Pré-requis

Pour installer ces outils, vous aurez besoin de :
- La librairie PHP >= 5.4
    - soit en local votre machine à l'aide d'un [exécutable](https://www.php.net/downloads.php)
    - soit celui d'un conteneur Docker
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

## Installation

``` shell
composer global require squizlabs/php_codesniffer
```

Composer installera l'exécutable de PHP Code Sniffer dans `vendor/bin`.

## Configuration

### PhpStorm

Voir la [documentation officielle](https://www.jetbrains.com/help/phpstorm/using-php-code-sniffer.html) pour intégrer PHP Code Sniffer à l'IDE Jetbrains PhpStorm.

#### Quality tools

Dans `Settings > Languages & Frameworks > PHP > Quality Tools > Code Sniffer > Local`

Indiquer *PHP Code Sniffer path* : `C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpcs.bat`

#### Editor inspections configuration

Dans `Settings > Editor > Inspection`

Selectionner : `PHP > Quality Tools`
Cocher `PHP Code Sniffer validation`

#### External tool

En installant les outils en global sur une machine de dev, il est possible de créer un [external tool](https://www.jetbrains.com/help/phpstorm/settings-tools-external-tools.html) dans PHPStorm

Dans `Settings > Tools > External Tools`, créer un nouvel outil avec comme settings :

| Clé | Valeur |
| ------ | ------ |
| Program | C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpcs.bat |
| Arguments | -s --report=code --report-file=$ProjectFileDir$\var\codesniffer $ProjectFileDir$\src
| Working directory | $ProjectFileDir$ |

## Lancement

``` shell
vendor/bin/phpcs -s --report=code --report-file=$ProjectFileDir$\var\codesniffer $ProjectFileDir$\src
```

Il est également possible de préciser le standard à utiliser. Exemple avec le PSR-2 :

``` shell
vendor/bin/phpcs -s --standard=PSR2 --report=code --report-file=$ProjectFileDir$\var\codesniffer $ProjectFileDir$\src
```