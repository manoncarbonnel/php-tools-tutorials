# PHP Mess Detector

[Voir la documentation complète](https://github.com/phpmd/phpmd)

PHP Mess Detector se concentre sur la complexité et la recherche d’erreurs dans le code sans l’exécuter.

**Sommaire**

- [Pré-requis](#pre-requis)
- [Installation](#installation)
- [Configuration](#configuration)
    - [PhpStorm](#phpstorm)
      - [Quality tools](#quality-tools)
      - [Editor inspections configuration](#editor-inspections-configuration)
      - [External tool](#external-tool)
- [Lancement](#lancement)

## Pré-requis

Pour installer ces outils, vous aurez besoin de :
- La librairie PHP
    - soit en local votre machine à l'aide d'un [exécutable](https://www.php.net/downloads.php)
    - soit celui d'un conteneur Docker
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

Cet outil est [nativement compatibles](https://www.jetbrains.com/help/phpstorm/php-code-quality-tools.html) avec un IDE :
- [PhpStorm](https://www.jetbrains.com/phpstorm/)

## Installation

``` shell
composer global require phpmd/phpmd
```

Composer installera l'exécutable de PHP Mess Detector dans `vendor/bin`.

## Configuration

La configuration de PHP MD se fait dans le fichier `phpmd_ruleset.xml`

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

Voir la [documentation officielle](https://www.jetbrains.com/help/phpstorm/using-php-mess-detector.html) pour intégrer PHP Mess Detector à l'IDE Jetbrains PhpStorm.

#### Quality tools

Dans `Settings > Languages & Frameworks > PHP > Quality Tools > Mess Detector > Local`

Indiquer *PHP Mess Detector path* : `C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpmd.bat`

#### Editor inspections configuration

Dans `Settings > Editor > Inspection`

Selectionner : `PHP > Quality Tools`
Cocher `PHP Mess Detector validation`

#### External tool

En installant les outils en global sur une machine de dev, il est possible de créer un [external tool](https://www.jetbrains.com/help/phpstorm/settings-tools-external-tools.html) dans PHPStorm

Dans `Settings > Tools > External Tools`, créer un nouvel outil avec comme settings :

| Clé | Valeur |
| ------ | ------ |
| Program | C:\Users\username\AppData\Roaming\Composer\vendor\bin\phpmd.bat |
| Arguments | $ProjectFileDir$/src text phpmd_ruleset.xml --exclude bin,features,tests,var,vendor |
| Working directory | $ProjectFileDir$ |

## Lancement

``` shell
vendor/bin/phpmd $ProjectFileDir$/src text phpmd_ruleset.xml --exclude bin,features,tests,var,vendor
```