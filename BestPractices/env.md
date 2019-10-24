# Env files

**Table of Contents**

- [Getting Started](#getting-started)
- [Env configuration example](#env-dev-configuration-example)
- [Env dev configuration example](#env-dev-configuration-example)

## Getting Started

In all environments, the following files are loaded if they exist, the later taking precedence over the former:

- * .env                contains default values for the environment variables needed by the app
- * .env.local          uncommitted file with local overrides
- * .env.$APP_ENV       committed environment-specific defaults
- * .env.$APP_ENV.local uncommitted environment-specific overrides

Real environment variables win over *.env* files.

**DO NOT DEFINE PRODUCTION SECRETS IN THIS FILE NOR IN ANY OTHER COMMITTED FILES.**

Run `composer dump-env prod` to compile *.env* files for production use (requires symfony/flex >=1.2).

[https://symfony.com/doc/current/best_practices/configuration.html#infrastructure-related-configuration](https://symfony.com/doc/current/best_practices/configuration.html#infrastructure-related-configuration)

## Env configuration example

``` .env
###> symfony/framework-bundle ###
APP_ENV=prod
APP_SECRET=d1dc839b067d8d12673a5460a6f511d1
APP_DEBUG=0
#TRUSTED_PROXIES=127.0.0.1,127.0.0.2
#TRUSTED_HOSTS='^localhost|example\.com$'
###< symfony/framework-bundle ###

###> nelmio/cors-bundle ###
CORS_ALLOW_ORIGIN=^https?://localhost(:[0-9]+)?$
###< nelmio/cors-bundle ###

###> doctrine/doctrine-bundle ###
# Format described at http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/configuration.html#connecting-using-a-url
# For an SQLite database, use: "sqlite:///%kernel.project_dir%/var/data.db"
# Configure your db driver and server_version in config/packages/doctrine.yaml
DATABASE_URL=mysql://root:root@database:3306/defaultschemaname
###< doctrine/doctrine-bundle ###
```

## Env dev configuration example

``` .env
APP_ENV=dev
APP_SECRET='s$cretf0rt3st'
APP_DEBUG=1
PHP_IDE_CONFIG=serverName=localhost
```
