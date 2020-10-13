# PHP Mess Detector

[See full documentation](https://github.com/phpmd/phpmd)

PHP Mess Detector focuses on complexity and finding errors in code without executing it.

**Summary**

- [Prerequisites](#prerequisites)
- [Install](#install)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
      - [Quality tools](#quality-tools)
      - [Editor inspections configuration](#editor-inspections-configuration)
      - [External tool](#external-tool)
- [Usage](#usage)

## Prerequisites

To install those tools, you will need:
- PHP library
    - [local](https://www.php.net/downloads.php)
    - from a Docker container
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

This tool is [already built-in](https://www.jetbrains.com/help/phpstorm/php-code-quality-tools.html) an IDE:
- [PhpStorm](https://www.jetbrains.com/phpstorm/)

## Install

``` shell
composer global require phpmd/phpmd
```

Composer will install PHP Mess Detector in `vendor/bin`.

## Configuration

The configuration of PHP MD is located in the file `phpmd_ruleset.xml`

``` xml
<?xml version="1.0"?>
<ruleset name="My first PHPMD rule set"
         xmlns="http://pmd.sf.net/ruleset/1.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://pmd.sf.net/ruleset/1.0.0
                     http://pmd.sf.net/ruleset_xml_schema.xsd"
         xsi:noNamespaceSchemaLocation="
                     http://pmd.sf.net/ruleset_xml_schema.xsd">
    <description>
        Ruleset for PHPMD
    </description>

    <rule ref="rulesets/cleancode.xml">
        <exclude name="StaticAccess" />
    </rule>
    <rule ref="rulesets/codesize.xml"/>
    <rule ref="rulesets/controversial.xml" />
    <rule ref="rulesets/design.xml"/>
    <rule ref="rulesets/naming.xml">
        <exclude name="ShortVariable" />
        <rule ref="rulesets/naming.xml/ShortVariable">
            <properties>
                <property name="minimum exceptions">
                    <value>2</value>
                </property>
            </properties>
        </rule>
    </rule>
    <rule ref="rulesets/unusedcode.xml">
        <exclude name="UnusedFormalParameter" />
    </rule>
</ruleset>
```

### PhpStorm

See [full documentation](https://www.jetbrains.com/help/phpstorm/using-php-mess-detector.html) to integrate PHP Mess Detector into Jetbrains PhpStorm IDE.

#### Quality tools

In `Settings > Languages & Frameworks > PHP > Quality Tools > Mess Detector > Local`

Indicate *PHP Mess Detector path*: `C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpmd.bat`

#### Editor inspections configuration

In `Settings > Editor > Inspection`

Select: `PHP > Quality Tools`

Check `PHP Mess Detector validation`

#### External tool

By installing the tools globally on a development machine, it is possible to create an [external tool](https://www.jetbrains.com/help/phpstorm/settings-tools-external-tools.html) in PHPStorm

In `Settings > Tools > External Tools`, create a new tool with the following settings:

| Key | Value |
| ------ | ------ |
| Program | C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpmd.bat |
| Arguments | $ProjectFileDir$/src text phpmd_ruleset.xml --exclude bin,features,tests,var,vendor |
| Working directory | $ProjectFileDir$ |

## Usage

``` shell
vendor/bin/phpmd $ProjectFileDir$/src text phpmd_ruleset.xml --exclude bin,features,tests,var,vendor
```
