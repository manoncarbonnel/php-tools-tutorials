![Behat](https://github.com/Behat/logo/raw/master/logo.png)

Behat is a functional (end-to-end) test framework.
Based on [Behaviour Driven Design](https://fr.wikipedia.org/wiki/Programmation_pilot%C3%A9e_par_le_comportement), with tests written in [Gherkin](https://cucumber.io/docs/gherkin/) and contexts (steps) written in PHP.

Tests are written according to the GWT (Given-When-Then) pattern.

Pre written contexts, [Behatch](https://github.com/Behatch/contexts) are available.

**Summary**

- [Prerequisites](#prerequisites)
- [Installing](#installing)
    - [Behat](#behat)
    - [Behat Mink](#behat-mink)
    - [Behat Symfony2 Extension](#behat-symfony2-extension)
    - [Mink BrowserKit Driver](#mink-browserkit-driver)
    - [Behat Mink Extension](#behat-mink-extension)
    - [Behatch Contexts](#behatch-contexts)
- [Usage](#usage)
    - [Write your tests](#write-your-tests)
    - [Data Fixtures](#data-fixtures)
    - [Running the tests](#running-the-tests)
    - [Generate a report](#generate-a-report)
        - [Text report](#text-report)
        - [HTML report](#html-report)

## Prerequisites

To install those tools, you will need:
- PHP library
    - [local](https://www.php.net/downloads.php)
    - from a Docker container
- [Git](https://git-scm.com/)
- [Composer](https://getcomposer.org/)

## Installing

### Behat

Install behat for development environment

``` shell
composer req behat/behat --dev
```

### Behat Mink

[Mink](http://mink.behat.org) is an open source browser emulator for web applications. It is used to simulate the interaction between browser and our REST API application to check if it works correctly when running tests.

``` shell
composer req behat/mink --dev
```

### Behat Symfony2 Extension

*Symfony2Extension* is an integration layer between *Behat 3+* and *Symfony2+* frameworks which provides a complete integration with Symfony2+ bundles structure, an initialized and booted kernel instance for your contexts and additional symfony2 Mink driver, if the Mink Extension is installed (see Installing Behat Mink Extension section below).

``` shell
composer req behat/symfony2-extension --dev
```

Thanks to *Symfony Flex*and its recipes approach, it will automatically create a basic structure, a directory called features in your main project dir with the default *FeatureContext* class and bootstrap directory containing `bootstrap.php` file.

Additionally, an example `demo.feature` feature file will be created inside features directory and Behat configuration file called `behat.yml.dist` in your main project directory.

The example configuration will look like:

``` yml
# behat.yml.dist
default:
    suites:
        default:
            contexts:
                - FeatureContext:
                    kernel: '@kernel'
    extensions:
        Behat\Symfony2Extension:
            kernel:
                bootstrap: features/bootstrap/bootstrap.php
                class: App\Kernel
```

### Mink BrowserKit Driver

*BrowserKitDriver* provides a bridge for the Symfony [BrowserKit](https://symfony.com/components/BrowserKit) component.

*BrowserKit* is a browser emulator provided by the Symfony project.

*BrowserKit* will be used now to emulate out functional tests.

``` shell
composer req behat/mink-browserkit-driver --dev
```

### Behat Mink Extension

*MinkExtension* is an integration layer between *Behat 3+* and *Mink 1.5+* and it provides:

- Additional services for Behat (*Mink*, *Sessions*, *Drivers*).
- *Behat\MinkExtension\Context\MinkAwareContext* which provides Mink instance for your contexts.
- Base *Behat\MinkExtension\Context\MinkContext* context which provides base step definitions and hooks for your contexts or sub-contexts. Or it could be even used as context on its own.

``` shell
composer req behat/mink-extension --dev
```

And adjust the Behat config file:

``` yml
# behat.yml
default:
  suites:
    default:
      contexts:
        - FeatureContext:
            kernel: '@kernel'
        - Behat\MinkExtension\Context\MinkContext
  extensions:
    Behat\Symfony2Extension:
      kernel:
        bootstrap: features/bootstrap/bootstrap.php
        class: App\Kernel
    Behat\MinkExtension:
      base_url: "http://example.com/"
      sessions:
        default:
          symfony2: ~
```

### Behatch Contexts

[Behatch Contexts](https://github.com/Behatch/contexts) is a package with most commonly used steps. It is an extension which provides, among others, a Behat context such as JSON, XML, REST, debug etc.
It requires Mink and Mink Extension to be installed.

``` shell
composer req behatch/contexts --dev
```

In `behat.yml.dist`file enable installed Behatch Contexts. In this case, `behatch:context:json` and `behatch:context:rest` context will be used to test JSON REST API. Also, `Behatch\Extension` extension should be activated:

``` yml
# behat.yml
default:
  suites:
    default:
      contexts:
        - FeatureContext:
            kernel: '@kernel'
        - Behat\MinkExtension\Context\MinkContext
        - behatch:context:json
        - behatch:context:rest
  extensions:
    Behat\Symfony2Extension:
      kernel:
        bootstrap: features/bootstrap/bootstrap.php
        class: App\Kernel
    Behat\MinkExtension:
      base_url: "http://example.com/"
      sessions:
        default:
          symfony2: ~
    Behatch\Extension: ~
```

## Usage

### Write your tests

You wan now write your features. Example:

``` gherkin
# features/books.feature
Feature: Books feature
  Scenario: Adding a new book
    When I add "Content-Type" header equal to "application/json"
    And I add "Accept" header equal to "application/json"
    And I send a "POST" request to "/api/books/" with body:
    """
    {
      "title": "King",
      "author": "T. M. Frazier",
      "enabled": true
    }
    """
    Then the response status code should be 201
    And the response should be in JSON
    And the header "Content-Type" should be equal to "application/json"
    And the JSON nodes should contain:
      | title                   | King              |
      | author                  | T. M. Frazier     |
    And the JSON node "enabled" should be true
```

### Data fixtures

To run end-to-end tests of an app, you might need reliable and clean data in your database.

For this purpose, you can use [Doctrine fixtures bundle](https://symfony.com/doc/current/bundles/DoctrineFixturesBundle) to load required data, when running a scenario or a suite, using a [hook](https://docs.behat.org/en/v2.5/guides/3.hooks.html) (in our example *BeforeScenario*).

``` shell
composer require --dev doctrine/doctrine-fixtures-bundle
```

Write a loader following the official [documentation](https://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html#writing-fixtures):

``` php
// src/DataFixtures/AppFixtures.php
namespace App\DataFixtures;

use App\Entity\Product;
use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Common\Persistence\ObjectManager;

class AppFixtures extends Fixture
{
    public function load(ObjectManager $manager)
    {
        // create 20 products! Bam!
        for ($i = 0; $i < 20; $i++) {
            $product = new Product();
            $product->setName('product '.$i);
            $product->setPrice(mt_rand(10, 100));
            $manager->persist($product);
        }

        $manager->flush();
    }
}
```

Update file `features/bootstrap/FeatureContext.php`:

``` php
// features/bootstrap/FeatureContext.php

use App\DataFixtures\AppFixtures;
use Behat\Behat\Context\Context;
use Behat\Behat\Hook\Scope\BeforeScenarioScope;
use Doctrine\Common\DataFixtures\Executor\ORMExecutor;
use Doctrine\Common\DataFixtures\Loader;
use Doctrine\Common\DataFixtures\Purger\ORMPurger;
use Doctrine\ORM\Tools\SchemaTool;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpKernel\KernelInterface;

class FeatureContext
{
    /**
     * @var KernelInterface
     */
    private $kernel;

    /**
    * @var EntityManagerInterface
    */
    protected $entityManager;

    /**
     * FeatureContext constructor.
     *
     * @param KernelInterface $kernel
     */
    public function __construct(KernelInterface $kernel)
    {
        $this->kernel = $kernel;
        $this->entityManager = $kernel->getContainer()->get('doctrine.orm.default_entity_manager');

        // Recreate database before running tests
        $schemaTool = new SchemaTool($this->entityManager);
        $schemaTool->dropDatabase();
        $metadata = $this->entityManager->getMetadataFactory()->getAllMetadata();
        $schemaTool->createSchema($metadata);
    }

    /**
     * @BeforeScenario
     *                
     * @param BeforeScenarioScope $scope
     */
    public function loadDataFixtures(BeforeScenarioScope $scope): void
    {
        // load data
    }
}
```

### Running the tests

**Using a local PHP**

``` shell
php vendor/behat/behat/bin/behat --config behat.yml.dist features/
```

**Using Docker**

```PowerShell
docker-compose exec backend vendor/behat/behat/bin/behat --config behat.yml.dist features/
```

### Generate a report

#### Text report

It is possible to generate a text report by adding those parameters to the command `--format pretty --out report.txt`

**Using local PHP**

``` shell
php vendor/behat/behat/bin/behat --format pretty --out report.txt --config behat.yml.dist features/
```

**Using Docker**

```PowerShell
docker-compose exec backend vendor/behat/behat/bin/behat --format pretty --out report.txt --config behat.yml.dist features/
```

http://behat.org/en/latest/user_guide/command_line_tool/formatting.html

#### HTML report

It is possible to generate a report with [BehatHtmlFormatterPlugin](https://github.com/dutchiexl/BehatHtmlFormatterPlugin) plugin

``` shell
composer require --dev emuse/behat-html-formatter
```

Edit configuration:

``` yml
# behat.yml.dist
default:
  suites:
    default:
       contexts:
          - emuse\BehatHTMLFormatter\Context\ScreenshotContext:
               screenshotDir: build/html/behat/assets/screenshots
    ... # All your awesome suites come here
  formatters:
    html:
      output_path: %paths.base%/build/html/behat

  extensions:
    emuse\BehatHTMLFormatter\BehatHTMLFormatterExtension:
      name: html
      renderer: Twig,Behat2
      file_name: index
      print_args: true
      print_outp: true
      loop_break: true
```

Report example:

![Rapport](https://camo.githubusercontent.com/6e52f9aa08aa2b597928724f22644169b58aab09/687474703a2f2f692e696d6775722e636f6d2f6f307a437169422e706e67)
