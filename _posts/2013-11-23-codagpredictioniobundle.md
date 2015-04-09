---
layout: post
title: "CodagPredictionIOBundle"
description: ""
category: publication
tags: [symfony2, predictionio]
---
{% include JB/setup %}


[![Build Status](https://travis-ci.org/Codag/PredictionIOBundle.png?branch=master)](https://travis-ci.org/Codag/PredictionIOBundle)
[![Total Downloads](https://poser.pugx.org/codag/predictionio-bundle/downloads.png)](https://packagist.org/packages/codag/predictionio-bundle)
[![Latest Stable Version](https://poser.pugx.org/codag/predictionio-bundle/v/stable.png)](https://packagist.org/packages/codag/predictionio-bundle)

## Introduction

This bundle provides an [PredictionIO](http://prediction.io/) integration for
your Symfony2 Project. In effect, it is just a wrapper for the
PredictionIO-PHP-SDK and will support all methods provided in the SDK.
Fortunately, in Symfony2 this can be done using the [dependency injection
component](http://symfony.com/doc/current/components/dependency_injection/introduction.html)
whereas parameters are configurable in separate config/parameter files.

### Sandbox

For further implementation examples please see also our [Sandbox](https://github.com/Codag/PredictionIOBundle-Sandbox).

## Usage

This bundle provides the service `codag.predictionio`

{% highlight php %}
<?php
$client = $this->get('codag.predictionio')->getClient();
{% endhighlight %}

## Source

This is an open source project and can be found under the following links:

* [Github](https://github.com/Codag/PredictionIOBundle)
* [Packagist](https://packagist.org/packages/codag/predictionio-bundle)
