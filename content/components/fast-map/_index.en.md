---
title: "Fast Map"
date: 2020-07-12T15:21:02+02:00
draft: false
type: "docs"
icon: "ti-package"
description: "Data transformation and serialization, with compiled and static mappers"
---

{{< feature-state for_mw_version="0.1" state="alpha" >}}

### Definition of a Mapping

A mapping is an operation that associates each element of a source of data into 
a new format of the same data. By this operation, it is possible to transform
the way the data is represented.

### The Abstract Mapping Tree

The _abstract mapping tree_ is a representation of the mapping to apply to your
data, in order to transform its representation. It is an internal representation
build for a computer to understand how it will have to change the data representation
during the mapping operation.

### Components included

#### The `php-etl/fast-map` component

This component is in charge of representing the Abstract Mapping Tree, and transforming
it into compiled PHP code. This compiled PHP code is the actual code that will do the 
mapping, in an optimized way.
 
####`php-etl/fast-map-config`
