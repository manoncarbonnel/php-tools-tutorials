# PHP Metrics

![Standard report](https://github.com/phpmetrics/PhpMetrics/raw/master/doc/overview.png)

[See full documentation](https://github.com/phpmetrics/PhpMetrics)

PhpMetrics provides metrics about PHP project and classes, with beautiful and readable HTML report.

**Summary**

- [Prerequisites](#prerequisites)
- [Install](#install)
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
composer global require phpmetrics/phpmetrics
```

Composer will install PHP Metrics in `vendor/bin`.

### PhpStorm

The [PhpMetrics plugin](https://plugins.jetbrains.com/plugin/7500-phpmetrics) for the Jetbrains PhpStorm IDE allows you to launch the code analysis directly in the IDE.

## Usage

``` shell
vendor/bin/phpmetrics --report-html=myreport src
```

**with PhpStorm plugin**

Select `Code > Run PhpMetrics on selected folder` this will open an html report in a new tab of the default browser.
