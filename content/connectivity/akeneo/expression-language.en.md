---
title: "Akeneo expression language functions"
date: 2021-01-24T09:23:54+01:00
draft: false
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Goal

This library implements functions for manipulating Akeneo API data through the
[Symfony Expression Language](https://symfony.com/doc/current/components/expression_language.html).

### Installation

```
composer require php-etl/akeneo-expression-language
```

### Usage

{{< tabs name="basic_definition" >}}

{{< tab name="YAML" codelang="yaml"  >}}
- fastmap:
    map:
    - field: '[title]'
      expression: 'filter(input["title"], scope("print", "mobile", "web"), first())'
{{< /tab >}}

{{< tab name="PHP" codelang="php"  >}}
<?php

use Symfony\Component\ExpressionLanguage\ExpressionLanguage;
use Kiboko\Component\ExpressionLanguage\Akeneo\AkeneoFilterProvider;

$input = [
    [
        'locale' => 'en_US',
        'scope' => 'mobile',
        'data' => 'Lorem ipsum dolor sit amet',
    ],
    [
        'locale' => 'fr_CA',
        'scope' => 'web',
        'data' => 'Lorem ipsum dolor sit amet',
    ],
    [
        'locale' => 'fr_CA',
        'scope' => 'marketplace',
        'data' => 'Lorem ipsum dolor sit amet',
    ],
    [
        'locale' => 'fr_FR',
        'scope' => 'print',
        'data' => 'Lorem ipsum dolor sit amet',
    ],
    [
        'locale' => 'en_GB',
        'scope' => 'mobile',
        'data' => 'Lorem ipsum dolor sit amet',
    ],
];
$expression = 'filter(input, scope("print", "mobile", "web"), first())';

$interpreter = new ExpressionLanguage(null, [new AkeneoFilterProvider()]);
$interpreter->evaluate($expression, ['input' => $input]);
{{< /tab >}}

{{< /tabs >}}
