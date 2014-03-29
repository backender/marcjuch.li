---
layout: post
title: "CodagPredictionIOBundle"
description: ""
category: publication
tags: [symfony2, predictionio]
---
{% include JB/setup %}

## Introduction

[![Build Status](https://travis-ci.org/Codag/PredictionIOBundle.png?branch=master)](https://travis-ci.org/Codag/PredictionIOBundle)
[![Total Downloads](https://poser.pugx.org/codag/predictionio-bundle/downloads.png)](https://packagist.org/packages/codag/predictionio-bundle)
[![Latest Stable Version](https://poser.pugx.org/codag/predictionio-bundle/v/stable.png)](https://packagist.org/packages/codag/predictionio-bundle)

This bundle provides an [PredictionIO](http://prediction.io/) integration for your Symfony2 Project.

### Note

For further implementation examples please see also our [Sandbox](https://github.com/Codag/PredictionIOBundle-Sandbox).

## Structure

This Bundle is a wrapper for the [PredictionIO-PHP-SDK](https://github.com/PredictionIO/PredictionIO-PHP-SDK) and will support all methods provided in the SDK.

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
