---
title: "Bare-metal Installation"
date: 2020-07-12T15:21:02+02:00
draft: false
shortDescription: Lorem Ipsum
---

### Contents of this documentation

What we will consider as a *bare-metal* installation, is an installation of at least one component of the middleware in
a vanilla PHP application, without the use of a specific framework.

This process will make use of the *Composer* command all along. Even if Composer is a de-facto standard, please make
sure your application can work with composer.

### Prerequisites

Please check page of [System Requirements](/system-requirements) and ensure your system can work with **Middleware**.
This page will help you install Logstash, ElasticSearch and Kibana in a quick walkthrough.

#### PHP dependencies

We will use the *Symfony Console* and *Monolog* as a base for our application.

In your project directory run the following:

`composer require symfony/console monolog/monolog`

This command will check out the most appropriate versions of both components.

### Prepare your executable

Create a `bin/middleware` file with the execution permission and the following contents:

```php
#!/usr/bin/env php
<?php

require __DIR__ . '/../vendor/autoload.php';

use MyMiddleware\Console\Command;
use Symfony\Component\Console\Application;
use Symfony\Component\Console\Input\ArgvInput;
use Symfony\Component\Console\Output\ConsoleOutput;

$app = new Application('Pipelines Runner');

$app->addCommands([
    new Command\FastMap\CompileCommand(
        Command\FastMap\CompileCommand::$defaultName
    ),

    new Command\Pipeline\CustomersRunCommand(
        Command\Pipeline\CustomersRunCommand::$defaultName
    ),
]);

$app->run(new ArgvInput($argv), new ConsoleOutput());
```

Now that you have your executable, file, we will need to 