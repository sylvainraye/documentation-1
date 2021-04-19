---
title: "Sylius"
date: 2021-02-17T10:13:54+01:00
draft: false
type: "plugins"
logo: "sylius"
description: "Connect your Sylius e-commerce through pipelines"
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Goal

The packages listed below aims at the integration of [Sylius](https://www.sylius.com) into the [ETL Pipeline](../../components/pipeline) 
and [Satellite](../../components/satellite) stacks.

The tools built for Sylius is composed of ETL capacities and data processing functions to be used in
[Fast Map](../../components/fast-map).

### The Sylius plugin for ETL Pipeline

This plugin will enable Sylius connectivity to the [ETL Pipeline](../../components/pipeline), in order to
read and write from and to Akeneo PIM. This integration is compatible with both
[Akeneo Enterprise Edition client](https://github.com/akeneo/api-php-client-ee)
and the [Akeneo Community Edition client](https://github.com/akeneo/api-php-client).

[See detailed documentation](plugin)

### Configuration format

#### Building an extractor

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  pipeline:
    steps:
    - sylius:
        extractor:
          type: products
          method: all
          search:
            - { field: enabled, operator: '=', value: true }
            - { field: completeness, operator: '>', value: 70, scope: ecommerce }
            - { field: completeness, operator: '<', value: 85, scope: ecommerce }
            - { field: categories, operator: IN, value: winter_collection }
            - { field: family, operator: IN, value: [camcorders, digital_cameras] }
        logger:
          type: 'stderr'
        client:
          api_url: 'https://demo.sylius.com'
          client_id: '2_5a3jtcvwi8w0cwk88w04ogkcks00o4wowwgc8gg4w0cow4wsc8'
          secret: '4ww9l30ij2m8wsw8w04sgw4wgkwc8gss0sgc8cc0o0goo4wkso'
          username: 'demo_9573'
          password: 516f3e3e5
{{< /tab >}}

{{< /tabs >}}

#### Building a loader

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  pipeline:
    steps:
    - sylius:
        loader:
          type: products
          method: create
        logger:
          type: 'stderr'
        client:
          api_url: 'https://demo.sylius.com'
          client_id: '2_5a3jtcvwi8w0cwk88w04ogkcks00o4wowwgc8gg4w0cow4wsc8'
          secret: '4ww9l30ij2m8wsw8w04sgw4wgkwc8gss0sgc8cc0o0goo4wkso'
          username: 'demo_9573'
          password: 516f3e3e5
{{< /tab >}}

{{< /tabs >}}
