---
title: "Spreadsheet"
date: 2021-03-30T15:50:22+02:00
draft: false
type: "plugins"
description: "Data transformation and serialization, with compiled and static mappers"
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Definition
This package aims at integrating the Spreadsheet or the Opendocument reader and writer into the
[Pipeline](https://github.com/php-etl/pipeline) stack.

### Principles
The tools in this library will produce executable PHP sources, using an intermediate _Abstract Syntax Tree_ from
[nikic/php-parser](https://github.com/nikic/PHP-Parser). This intermediate format helps you combine
the code produced by this library with other packages from [Middleware](https://github.com/php-etl).

### Installation
```
composer require php-etl/spreadsheet-plugin
```

### Usage

#### Example for Excel
Reads `input.xlsx`, writes `output.xlsx`, logs error in system's [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)).
```yaml
csv:
  extractor:
    file_path: 'input.xlsx'
    excel:
      sheet: 'sheet2'
      skip_line: 1
#  loader:
#    file_path: 'output.xlsx'
#    excel:
#      sheet: 'sheet2'
  logger:
    type: stderr
```

#### Example for OpenDocument
Reads `input.ods`, writes `output.ods`, logs error in system's [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)).
```yaml
csv:
  extractor:
    file_path: 'input.ods'
    open_document:
      sheet: 'sheet2'
      skip_line: 1
#  loader:
#    file_path: 'output.ods'
#    open_document:
#      sheet: 'sheet2'
  logger:
    type: stderr
