---
title: "Satellite"
date: 2020-07-12T15:21:02+02:00
draft: false
type: "docs"
icon: "ti-cloud"
description: "Generated micro-services for data stream processing in the cloud"

---

Definition of a Satellite
---

A satellite is a micro-service running in the cloud, packaged as a docker image.
It can be deployed in any Docker infrastructure, including Kubernetes clusters.

There is 3 types of runtimes, depending on your needs:
 * `http-api`: the micro-service will be operating an API, on which several URL routes can be registered
 * `http-hook`: the micro-service will be operating an API on a single URL route
 * `pipeline`: the micro-service will be operating a data pipeline, runned in the backend that can be executed as a cron job

Building your own satellite
---

We will build your satellite micro-service in a few steps. In this example, we will use the `pipeline` runtime.

First of all, let's declare the base image on which we want to buyild our micro-service. 

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  image: kiboko/php:7.4-fpm
{{< /tab >}}

{{< /tabs >}}

Here, we chose to use the `kiboko/php:7.4-fpm` base image on which our code will be executed.
You could use any docker image of your choice, however you will need to have a PHP runtime 
available, in a compatible version.

Next, as a second step, we need to declare the dependencies our microservice will have.
We will require them through composer, and we need to declare the dependencies.

{{< tabs name="basic_with_composer" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
#...
  composer:
    require:
      - "php-etl/pipeline:@dev"
      - "php-etl/fast-map:@dev"
{{< /tab >}}

{{< /tabs >}}

Now that we have a fully-functioning environment prepared for our satellite, we will declare 
the way we want our pipeline to handle our data flows.

Configuration format
---

### The `image` field

In this field you will be able to specify the base docker image on which the
satellite will be built. We recommend either the official PHP images based on
Alpine Linux, the ones in `kiboko/php` or any of your custom brewed image.

### The `composer` section

This section makes you able to specify some parameters that will be transmitted
to composer at build time.

### The `runtime` section 


#### The `http-api` runtime
#### The `http-hook` runtime
#### The `pipeline` runtime

### Examples

{{< tabs name="tab_with_code" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  image: kiboko/php:7.4-fpm
  composer:
#    from-local: true
    require:
      - "psr/http-message:^1.0@dev"
      - "psr/http-factory:^1.0@dev"
      - "psr/http-server-handler:^1.0@dev"
      - "middlewares/uuid:dev-master"
      - "middlewares/base-path:dev-master"
      - "middlewares/request-handler:dev-master"
      - "middlewares/fast-route:dev-master"
      - "nyholm/psr7:^1.0@dev"
      - "nyholm/psr7-server:dev-master"
      - "laminas/laminas-httphandlerrunner:1.2.x-dev"
  runtime:
    type: http-api
    path: /foo
    routes:
      - path: /hello
        function: hello.php
      - path: /events/products
        function: events/products.php
{{< /tab >}}

{{< tab name="PHP" codelang="php"  >}}
satellite:
  image: kiboko/php:7.4-fpm
  composer:
#    from-local: true
    require:
      - "psr/http-message:^1.0@dev"
      - "psr/http-factory:^1.0@dev"
      - "psr/http-server-handler:^1.0@dev"
      - "middlewares/uuid:dev-master"
      - "middlewares/base-path:dev-master"
      - "middlewares/request-handler:dev-master"
      - "middlewares/fast-route:dev-master"
      - "nyholm/psr7:^1.0@dev"
      - "nyholm/psr7-server:dev-master"
      - "laminas/laminas-httphandlerrunner:1.2.x-dev"
  runtime:
    type: http-api
    path: /foo
    routes:
      - path: /hello
        function: hello.php
      - path: /events/products
        function: events/products.php
{{< /tab >}}

{{< /tabs >}}

```yaml
satellite:
  image: kiboko/php:7.4-fpm
  composer:
#    from-local: true
    require:
      - "psr/http-message:^1.0@dev"
      - "psr/http-factory:^1.0@dev"
      - "psr/http-server-handler:^1.0@dev"
      - "middlewares/uuid:dev-master"
      - "middlewares/base-path:dev-master"
      - "middlewares/request-handler:dev-master"
      - "middlewares/fast-route:dev-master"
      - "nyholm/psr7:^1.0@dev"
      - "nyholm/psr7-server:dev-master"
      - "laminas/laminas-httphandlerrunner:1.2.x-dev"
  runtime:
    type: http-hook
    path: /bar/hello
    function: hello.php
```

```yaml
satellite:
  image: kiboko/php:7.4-cli
  composer:
#    from-local: true
    autoload:
      psr-4:
        "Pipeline\\": ""
    require:
      - "php-etl/pipeline:@dev"
      - "php-etl/fast-map:@dev"
  runtime:
    type: pipeline
    steps:
    - extract: Pipeline\FooExtractor
    - transform: Pipeline\FastMapTransformer
      array:
      - field: '[sku]'
        copy: '[sku]'
      - field: '[title]'
        expression: 'input["sku"] ~" | "~ input["name"]'
      - field: '[name]'
        copy: '[name]'
      - field: '[staticValue]'
        constant: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur mollis efficitur justo, id facilisis elit venenatis et. Sed fermentum posuere convallis. Phasellus lectus neque, bibendum sit amet enim imperdiet, dignissim blandit nisi. Donec nec neque nisi. Vivamus luctus facilisis nibh id rhoncus. Vestibulum eget facilisis tortor. Etiam at cursus enim, vitae mollis ex. Proin at velit at erat bibendum ultricies. Duis ut velit malesuada, placerat nisl a, ultrices tortor.'
      - field: '[descriptions]'
        class: 'Pipeline\\Usage'
        expression: 'input'
        object:
        - field: 'usage'
          expression: '"Usage: " ~ input["usage"]'
        - field: 'warning'
          copy: '[warning]'
        - field: 'notice'
          copy: '[notice]'
    - load: Pipeline\BarLoader
```
