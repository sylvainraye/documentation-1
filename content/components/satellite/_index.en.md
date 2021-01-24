---
title: "Satellite"
date: 2020-07-12T15:21:02+02:00
weight: 1
draft: false
type: "docs"
logo: "lambda"
description: "Generated micro-services for data stream processing in the cloud"
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Definition of a Satellite

A satellite is a micro-service running in the cloud, packaged as a docker image.
It can be deployed in any Docker infrastructure, including Kubernetes clusters.

There is 3 types of runtimes, depending on your needs:
 * `http-api`: the micro-service will be operating an API, on which several URL routes can be registered
 * `http-hook`: the micro-service will be operating an API on a single URL route
 * `pipeline`: the micro-service will be operating a data pipeline, runned in the backend that can be executed as a cron job

### Building your own satellite

We will build your satellite micro-service in a few steps. In this example, we will use the `pipeline` runtime.

First of all, let's declare the base image on which we want to build our micro-service. 

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  image: kiboko/php:7.4-cli
{{< /tab >}}

{{< tab name="PHP" codelang="php"  >}}
<?php

use Kiboko\Component\ETL\Satellite\Adapter\Docker;

$dockerfile = new Docker\Dockerfile(
    new Docker\Dockerfile\From('kiboko/php:7.4-cli'),
    new Docker\Dockerfile\Workdir('/var/www/html/'),
);

$satellite = new Docker\Satellite(
    'foo/satellite:bar',
    $dockerfile,
);
{{< /tab >}}

{{< /tabs >}}

Here, we chose to use the `kiboko/php:7.4-cli` base image on which our code will be executed.
You could use any docker image of your choice, however you will need to have a PHP runtime 
available, in a compatible version: >=7.4 with the CLI SAPI.

Next, as a second step, we need to declare the composer dependencies our microservice will have.
We will require them through composer, with a declarative manner.

{{< tabs name="basic_with_composer" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
#...
  composer:
    require:
      - "php-etl/pipeline:@dev"
      - "php-etl/fast-map:@dev"
{{< /tab >}}

{{< tab name="PHP" codelang="php"  >}}
$dockerfile->push(
    new Docker\PHP\Composer(),
    new Docker\PHP\ComposerInit(),
    new Docker\PHP\ComposerMinimumStability('dev'),
    new Docker\PHP\ComposerRequire('php-etl/pipeline:@dev');
);
{{< /tab >}}

{{< /tabs >}}

Now that we have made our environment prepared for our satellite, we will declare 
the way we want our pipeline to handle our data flows.

We decide we need 

{{< tabs name="dataflows" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
#...
  runtime:
    type: pipeline
    steps:
    - extract: Pipeline\FooExtractor
    - transform: Pipeline\FastMapTransformer
#...
    - load: Pipeline\BarLoader
{{< /tab >}}

{{< tab name="PHP" codelang="php"  >}}
$dockerfile->push(
    new Docker\PHP\Composer(),
    new Docker\PHP\ComposerInit(),
    new Docker\PHP\ComposerMinimumStability('dev'),
    new Docker\PHP\ComposerRequire('php-etl/pipeline:@dev');
);
{{< /tab >}}

{{< /tabs >}}
### Configuration formats

There are 2 ways to declare satellites :
* Use the PHP objects
* Use the [YAML configuration Syntax](yaml-format)
