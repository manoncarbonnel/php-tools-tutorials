# Gitignore

Always regroup and frame the ignored files by library/framework.

``` .ignore
###> JetBrains IDE ###
.idea/
###< JetBrains IDE ###

###> symfony/framework-bundle ###
/.env.local
/.env.local.php
/.env.*.local
/public/bundles/
/public/doc/
/var/log/
/var/cache/
/vendor/
###< symfony/framework-bundle ###

###> symfony/phpunit-bridge ###
.phpunit
/phpunit.xml
###< symfony/phpunit-bridge ###

###> symfony/web-server-bundle ###
/.web-server-pid
###< symfony/web-server-bundle ###

###> behat/symfony2-extension ###
behat.yml
###< behat/symfony2-extension ###

###> phpunit ###
.phpunit.result.cache
###< phpunit ###

###> custom static analysis config ###
.php_cs
phpstan.neon
###< custom static analysis config ###

###> custom static analysis cache ###
/.php_cs.cache
###< custom static analysis cache ###
```