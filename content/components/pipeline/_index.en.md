---
title: "Pipeline"
date: 2020-07-12T15:21:02+02:00
draft: false
type: "component"
logo: "pipeline"
description: "Data stream processing at high rate and low memory consuming"
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Definition

A pipeline is a series of processes, also called steps, that filter or transform data.
The first process takes raw input data, uses it and then
sends the results to the second process, and so on, ending with the final result produced by the last process in progress.

The steps of our pipeline are `extract`, `transform` or `load`.

### Installation

``` 
composer require php-etl/pipeline
```

### Usage

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
satellite:
  pipeline:
    steps:
      - csv:
          extractor:
            file_path: path/to/file/input.csv
            delimiter: ';'
            enclosure: '"'
            escape: '\\'
          logger:
            type: stderr
      - csv:
          loader:
            file_path: path/to/file/output.csv
            delimiter: ','
            enclosure: '"'
            escape: '\\'
{{< /tab >}}

{{< tab name="PHP" codelang="php"  >}}
<?php

use Kiboko\Component\Pipeline\PipelineRunner;
use Kiboko\Component\Pipeline\Pipeline;
use Kiboko\Component\Flow\Csv\Safe\Extractor;
use Kiboko\Component\Flow\Csv\Safe\Loader;

/** @var Psr\Log\LoggerInterface $logger */ 
$runner = new PipelineRunner();
$pipeline = (new Pipeline($runner))
    ->extract((new Extractor('path/to/file/input.csv'))->setLogger($logger))
    ->load(new Loader('path/to/file/output.csv', delimiter: ','))
    ->run();
{{< /tab >}}

{{< /tabs >}}
