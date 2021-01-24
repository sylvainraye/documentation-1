---
title: "CSV Plugin"
date: 2021-01-19T10:02:02+02:00
draft: false
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
```yaml
csv:
    extractor:
        file_path: input.csv
        delimiter: ';'
        enclosure: '"'
        escape: '\\'
```

#### Writing to a file
```yaml
csv:
    loader:
        file_path: output.csv
        delimiter: ','
        enclosure: '"'
        escape: '\\'
```

#### Writing logs error in system's stderr
```yaml
csv:
    logger:
        type: stderr
```

`Your plugin configuration must not contain the config of the loader and the extractor in the same file.`
