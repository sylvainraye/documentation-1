---
title: "JSON Schema"
date: 2021-03-30T15:55:00+02:00
draft: false
type: "plugins"
description: "Read and write JSON-LD files"
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
The `logger:` option can be set or not. If it's not set, then `logger:` will be set to `\Psr\Log\NullLogger`.

#### Extract a json file
Reads `input.jsonld` with logs error in system's [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)).
```yaml
json:
  extractor:
    file_path: 'input.jsonld'
  logger:
    type: stderr
```

#### Load a json file
Writes `output.jsonld` with logs error in system's [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)).
```yaml
json:
  loader:
    file_path: 'output.jsonld'
  logger:
    type: stderr
```
