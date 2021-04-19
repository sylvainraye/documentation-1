---
title: "Spreadsheet"
date: 2021-03-30T15:50:22+02:00
draft: false
type: "plugins"
description: "Read and write Excel or OpenDocument files"
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Definition
This package aims at integrating the Excel or the Opendocument reader and writer into the
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
When you write a yaml configuration file, by default the extraction starts at line 0, but
you can also choose at which line you want your extraction to start with the `skip_lines` option

Regarding the `logger:` option, it can be set or not. If it's not set, then `logger:` will be set to `\Psr\Log\NullLogger`.

#### Extract an Excel file
Reads `input.xlsx` with logs error in system's [stderr](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)).

```yaml
spreadsheet:
  extractor:
    file_path: 'input.xlsx'
    excel:
      sheet: 'sheet2'
      skip_line: 1
  logger:
    type: stderr
```
#### Load an OpenDocument file

Writes `output.ods` without logs error.

```yaml
spreadsheet:
  loader:
    file_path: 'output.ods'
    open_document:
      sheet: 'sheet2'
```
