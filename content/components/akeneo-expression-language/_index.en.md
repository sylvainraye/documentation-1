---
title: "Akeneo Expression Language"
date: 2021-01-21T15:44:27+01:00
lastmod: 2021-01-21T15:44:27+01:00
weight: ""
draft: true
---

### Definition
This library implements functions for manupulating Akeneo API data in ExpressionLanguage.

Its uses the Symfony ExpressionLanguage component.

### Installation
```
composer require php-etl/akeneo-expression-language
```

### Usage
```php
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
```
`
`
