---
title: "Contracts"
date: 2020-07-12T15:21:02+02:00
draft: false
type: "docs"
icon: "ti-medall"
description: "Code contracts for building compatible pipelines in the middleware"
---

Code contracts
---

### Introduction

Code contracts are a set of abstraction interfaces made to help you build compatible code between all your pipelines, using common semantics.

### Installation

Contracts are provided as separate packages, so you can install only the ones your projects really need:

```shell script
composer require php-etl/discovery-contracts
composer require php-etl/lambda-contracts
composer require php-etl/mapping-contracts
composer require php-etl/messenger-contracts
composer require php-etl/pipeline-contracts
```

### Usage

The abstractions in those packages are useful to achieve loose coupling and interoperability.
By using the provided interfaces as type hints, you are able to build any implementations that match the contracts.
It could be a native Middleware component, or another package provided by the PHP community at large.

### Design principles

* Contracts are split by domain, each into their own sub-namespaces
* Contracts are small and consistent sets of PHP interfaces