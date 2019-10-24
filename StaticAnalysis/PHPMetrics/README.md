# PHP Metrics

![Standard report](https://github.com/phpmetrics/PhpMetrics/raw/master/doc/overview.png)

[Voir la documentation complète](https://github.com/phpmetrics/PhpMetrics)

PhpMetrics fournit des métriques sur le projet et les classes PHP, avec un rapport HTML stylisé et lisible.

**Sommaire**

- [Pré-requis](#pre-requis)
- [Installation](#installation)
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
composer global require phpmetrics/phpmetrics
```

Composer installera l'exécutable de PHP Metrics dans `vendor/bin`.

### PhpStorm

Le plugin [PhpMetrics](https://plugins.jetbrains.com/plugin/7500-phpmetrics) pour l'IDE Jetbrains PhpStorm permet de lancer l'analyse du code directement dans l'IDE.

## Lancement

``` shell
vendor/bin/phpmetrics --report-html=myreport src
```

**avec le plugin PhpStorm**

Dans le menu `Code > Run PhpMetrics on selected folder` cela ouvrire un rapport html dans un nouvel onglet du navigateur par défaut.