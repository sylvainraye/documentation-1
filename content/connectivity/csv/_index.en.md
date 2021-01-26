---
title: "CSV"
date: 2021-01-19T10:02:02+02:00
draft: false
type: "plugins"
logo: "csv"
description: "Read and write CSV files in any format"
---

### Definition

This package aims at integrating the CSV reader and writer into the Pipeline stack.
This plugin works thanks to the `csv-flow` package which itself depends on `flow-spout`.

### Installation

```
composer require php-etl/csv-plugin
```

### Usage

You have to create a yml file which contains all the plugin configuration.

Example of a config file. 

#### Reading from a CSV file

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  pipeline:
    steps:
      - csv:
          extractor:
            file_path: input.csv
            delimiter: ';'
            enclosure: '"'
            escape: '\\'
          logger:
            type: stderr
{{< /tab >}}

{{< /tabs >}}

#### Writing to a CSV file

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  pipeline:
    steps:
    - csv:
        loader:
          file_path: output.csv
          delimiter: ','
          enclosure: '"'
          escape: '\\'
        logger:
          type: stderr
{{< /tab >}}

{{< /tabs >}}
