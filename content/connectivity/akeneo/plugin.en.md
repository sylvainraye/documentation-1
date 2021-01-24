---
title: "Akeneo plugin"
date: 2021-01-24T09:23:54+01:00
lastmod: 2021-01-24T09:23:54+01:00
weight: ""
draft: false
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Configuration format

#### Building an extractor

```yaml
akeneo:
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
```

#### Building a loader

```yaml
akeneo:
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
```

### Usage

This library will build for you either an extractor or a loader, compatible with the Akeneo API.

You can use the following PHP script to test and print the result of your configuration.

```php
<?php

require __DIR__ . '/../vendor/autoload.php';

use Kiboko\Component\ETL\Flow\Akeneo;
use PhpParser\Node;
use PhpParser\PrettyPrinter;
use Symfony\Component\Console;
use Symfony\Component\Yaml;

$input = new Console\Input\ArgvInput($argv);
$output = new Console\Output\ConsoleOutput();

class DefaultCommand extends Console\Command\Command
{
    protected static $defaultName = 'test';

    protected function configure()
    {
        $this->addArgument('file', Console\Input\InputArgument::REQUIRED);
    }

    protected function execute(Console\Input\InputInterface $input, Console\Output\OutputInterface $output)
    {
        $factory = new Akeneo\Service();

        $style = new Console\Style\SymfonyStyle(
            $input,
            $output,
        );

        $config = Yaml\Yaml::parse(input: file_get_contents($input->getArgument('file')));

        $style->section('Validation');
        $style->writeln($factory->validate($config) ? '<info>ok</info>' : '<error>failed</error>');
        $style->section('Normalized Config');
        $style->writeln(\json_encode($config = $factory->normalize($config), JSON_PRETTY_PRINT));
        $style->section('Generated code');
        $style->writeln((new PrettyPrinter\Standard())->prettyPrintFile([
            new Node\Stmt\Return_($factory->compile($config)->getNode()),
        ]));

        return 0;
    }
}

(new Console\Application())
    ->add(new DefaultCommand())
    ->run($input, $output)
;
```

See also

* [php-etl/pipeline](../pipeline/)
* [php-etl/fast-map](../fast-map)
* [php-etl/akeneo-expression-language](../akeneo-expression-language)
