---
layout: default
title: Standard Environment Runtimes
parent: App Engine
grand_parent: Google Cloud Platform
nav_order: 1
description: The App Engine standard environment has two generations of runtime environments. This page lists all the available runtime values.
---

# App Engine Standard Environment Runtimes

The following options can be specified as values for `runtime` property in App Engine application configuration file (`app.yaml`):

## First generation

- `go111`
- `java` (see [Note on Java 6 support](#note-on-java-6-support))
- `java7` (see [Note on Java 7 support](#note-on-java-7-support))
- `java8`
- `php` (see [Note on PHP 5.4 support](#note-on-php-54-support))
- `php55`
- `python27`

### Note on PHP 5.4 support

PHP 5.4 applications are prevented from being deployed to Google App Engine from any version of the SDK, including older ones. If you need to continue to deploy PHP 5.4 applications for compatibility reasons, you can request that your application be whitelisted for PHP 5.4 deployment by visiting [http://goo.gl/qjKEuk].

### Note on Java 6 support

Java 6 applications are prevented from being deployed to Google App Engine from any version of the SDK, including older ones. If you need to continue to deploy Java 6 applications for compatibility reasons, you can request that your application be whitelisted for Java 6 deployment by visiting [http://goo.gl/ycffXq].

### Note on Java 7 support

Java 7 applications are prevented from being deployed to Google App Engine from any version of the SDK, including older ones. If you need to continue to deploy Java 7 applications for compatibility reasons, you can request that your application be whitelisted for Java 7 deployment by visiting [https://goo.gl/forms/qxeXfigRMccwAvoj1].

## Second generation

- `go112`
- `go113`
- `go114`
- `go115`
- `java11`
- `nodejs10`
- `nodejs12`
- `nodejs14`
- `php72`
- `php73`
- `php74`
- `python37`
- `python38`
- `python39`
- `ruby25`
- `ruby26`
- `ruby27`

## Source

- [App Engine Standard Environment Runtimes](https://cloud.google.com/appengine/docs/standard/runtimes)
