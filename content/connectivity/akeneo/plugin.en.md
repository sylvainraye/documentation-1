---
title: "Akeneo plugin"
date: 2021-01-24T09:23:54+01:00
draft: false
description: "Akeneo API integration in satellites pipelines"
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Configuration format

#### Building an extractor

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  pipeline:
    steps:
    - akeneo:
        enterprise: false
        extractor:
          type: productModel
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
          api_url: 'https://demo.akeneo.com'
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
    - akeneo:
        enterprise: true
        loader:
          type: productModel
          method: upsert
        logger:
          type: 'stderr'
        client:
          api_url: 'https://demo.akeneo.com'
          client_id: '2_5a3jtcvwi8w0cwk88w04ogkcks00o4wowwgc8gg4w0cow4wsc8'
          secret: '4ww9l30ij2m8wsw8w04sgw4wgkwc8gss0sgc8cc0o0goo4wkso'
          username: 'demo_9573'
          password: 516f3e3e5
{{< /tab >}}

{{< /tabs >}}
