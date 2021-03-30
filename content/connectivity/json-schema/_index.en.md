---
title: "JSON Schema"
date: 2021-03-30T15:55:00+02:00
draft: false
type: "plugins"
description: "Data transformation and serialization, with compiled and static mappers"
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Definition
This package aims at integrating the json extractor and loader into the
[Pipeline](https://github.com/php-etl/pipeline) stack.

### Principles
The tools in this library will produce executable PHP sources, using an intermediate _Abstract Syntax Tree_ from
[nikic/php-parser](https://github.com/nikic/PHP-Parser). This intermediate format helps you combine
the code produced by this library with other packages from [Middleware](https://github.com/php-etl).

### Installation
```
composer require php-etl/json-plugin
```

### Usage
Reads `input.jsonld`, writes `output.jsonld`, logs error in system's [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)).
```yaml
csv:
  extractor:
    file_path: 'input.jsonld'
#  loader:
#    file_path: 'output.jsonld'
  logger:
    type: stderr
```
