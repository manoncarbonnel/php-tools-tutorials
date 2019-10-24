# PROJECT NAME
![pipeline](https://github.com/<projetc_path>/badges/develop/pipeline.svg)
![coverage](https://github.com/<projetc_path>/badges/develop/coverage.svg)

*Project description*

**Table of Contents**

- [Getting Started](#getting-started)
- [Prerequisites](#prerequisites)
- [Installing](#installing)
  - [Configuration](#configuration)
  - [Docker compose usage](#docker-compose-usage)
  - [Dependencies install](#dependencies-install)
  - [Data import](#data-import)
  - [Source files](#source-files)
- [Environnement](#environnement)
  - [Code quality](#code-quality)
  - [PHP commands usage](#php-commands-usage)
- [Running the tests](#running-the-tests)
  - [Unit testing](#unit-testing)
  - [End to end tests testing](#end-to-end-tests-testing)
- [Delivery](#delivery)
- [Deployment](#deployment)
  - [Test environment](#test-environment)
  - [Production environment](#production-environment)
- [Built With](#built-with)
- [Versioning](#versioning)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.
See deployment for notes on how to deploy the project on a live system.
You can plug any of this tools to your favourite IDE.

## Prerequisites

You need to have the following softwares installed on your local machine :
- [Docker for windows](https://docs.docker.com/docker-for-windows/)
- [Git](https://git-scm.com/)
- Windows PowerShell

## Installing

Clone project using ssh (you will need to set a SSH key in your Gitlab profile) 
```Powershell
git clone git@github.com
```

### Configuration

- Each `./Dockerfile` contains a basic configuration for developing with [Symfony](https://symfony.com) 4 framework and deploying in a production environment.
- The file `.env` allows to set the name of the Docker project's stack as well as basic PHP image version for Docker
- The file `.env.dev.local` must be created (and git ignored) to fill in empty parameters from the `/.env` file 

### Docker compose usage

- Run / init : `docker-compose up`
- Stop : `docker-compose stop`
- Destroy : `docker-compose down`
- Cleanup : `docker-compose rm -vsf`
- Reset :  `docker system prune -af --volumes`

### Dependencies install

When starting the `backend` container, [Composer](https://getcomposer.org/) will download and install dependencies in `./vendor` directory

### Data import

We use *name and link to the SGBDR documentation* for this project.

*Explain how to import data when you start working on this project*

### Source files

*Explain where your source code is located*

## Environnement

### Code quality

We use code quality tools which you can plug in your favourite IDE :

*List which code quality tools are used and mandatory in this project*
*For each tool : name, link to documentation and path to the associated config file*

## PHP commands usage

Use this *pattern*
```PowerShell
docker-compose exec backend php <entrypoint> <command> <args>
```

## Running the tests

### Unit testing

Launch tests with associated config file in `` *Path to the phpunit.xml.dist file*

```PowerShell
docker-compose exec backend php bin/phpunit --configuration phpunit.xml.dist tests/
```

### End to end tests testing

Launch tests with associated config file in `` *path to the behat.yml.dist file*

```PowerShell
docker-compose exec backend vendor/behat/behat/bin/behat --config behat.yml.dist features/
```

## Delivery

*Explain how do you deliver the project to the client*

## Deployment

### Test environment

*Explain how do you deploy in test environment*

### Production environment

*Explain how do you deploy in production environment*

## Built With

*List which code and test frameworks and libraries are used in this project*
*For each one : name and link to documentation*

![Symfony](https://symfony.com/images/logos/header-logo.svg)

- [Symfony](https://symfony.com) & [API Platform](https://symfony.com) - PHP Framework
- [MySQL](https://www.mysql.com/) - SGBDR
- [PHP Unit](https://phpunit.readthedocs.io/) & [Prophecy](https://github.com/phpspec/prophecy) - Unit tests framework
- [Behat](https://behat.org/) - Functionnal tests framework
- [Composer](https://getcomposer.org/) - Dependency management
- [Docker](https://www.docker.com/) - Virtualization and container management

## Versioning

We use [Git](https://git-scm.com/) and [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) with [conventional commits](https://www.conventionalcommits.org/) for versioning.
