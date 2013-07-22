---
layout: post
title: "CodagAlchemyApiBundle"
description: ""
category: publication
tags: [symfony2, alchemyapi]
---
{% include JB/setup %}

## Introduction

[![Build Status](https://travis-ci.org/Codag/AlchemyApiBundle.png?branch=master)](https://travis-ci.org/Codag/AlchemyApiBundle)
[![Total Downloads](https://poser.pugx.org/codag/alchemyapi-bundle/downloads.png)](https://packagist.org/packages/codag/alchemyapi-bundle)
[![Latest Stable Version](https://poser.pugx.org/codag/alchemyapi-bundle/v/stable.png)](https://packagist.org/packages/codag/alchemyapi-bundle)

The CodagAlchemyApiBundle provides an [AlchemyAPI](http://www.alchemyapi.com/) integration for your Symfony2 Project.

### Note

> *The inital deployment of this bundle provides an unimpressive amount of functionality regarding to the powerful api of AlchemyAPI. The given implementation was needed for a project at Codag which was one of the reasons I decided to launch this bundle – next to the fact that there was no other implementation available and I'm always glad to contribute to the community. I've investigated some time in thinking about structure, scalability as well as usage of this bundle. Now it's time to extend the functionality of this bundle and I hope there are Symfony developers they already can use this bundle and will contribute if further implementation was neccessary.*

## Structure

The structure of this bundle is leant to the structure of the AlchemyAPI RESTful API. This will bring more complexity in the usage of this bundle but will provide scalability.

There are three steps needed to grant a standardised way of using a specific AlchemyAPI method. 

1. Call the **service** (AlchemyApi.php)
2. Call the specific **method** (Method/$MethodName)
3. Call the specific **type** of the selected method (Method/$MethodName$Type.php)

At this point the method specific parameters can be set to proceed the final api call.

### Types

AlchemyAPI provides three types of service. The HTML API allows request based on raw HTML, the Text API which will accept Text (string) and the URL API which will proceed text processing for the given URL. 

All of the three mentioned will do the same processing in the end but accept different types of input. AlchemyAPI split the three types in different url prefixes within their RESTful API:

- HTML API: html/
- Text API: text/
- Web API: url/

To follow this priciple this bundle contains an abstract class to each of this type in the `/Services/Types/` folder:

- HtmlApi.php
- TextApi.php
- WebApi.php

Each of these classes extend from the HttpClient and implement the AlchemyAPIInterface.

> *I was thinking about a super class for the three types (e.g. BaseApi.php). At the moment I think it's not necessary and would be an overhead of complexity. But if in the process of building this bundle a super class would make sense, this should be discussed.*


### Methods

AlchemyAPI provides the below listed methods they can be used by the api:

* Author Extraction
* Concept Tagging
* Content Scraping
* Entity Extraction
* Keyword Extraction
* Language Detection
* Microformats Parsing
* RSS/ATOM
* Relation Extraction
* Sentiment Analysis
* Text Categorization
* Text Extraction

In this bundle each method has it's independent base class which is an adapter between the service and the method. This adapter defines the method class which responsible for a specific type. This adapter as well as the actual method are located in `/Services/Methods/`. 


### Client

The client is responsible for handling the api calls and is located in `/Services/`. By default this bundle provides the following Client:

- HTTPClient.php

	This client is based on the "kriswallsmith/buzz" lightweight library for issuing HTTP requests. In this bundle the specific type defines the default handling of the client. If custom handling is needed this can be overriden within a method.

## Usage

Getting familiar with the configuation and installation of this bundle is described within the official [bundle readme](https://github.com/Codag/AlchemyApiBundle) on Github.

## Source

This is an open source project and can be found under the following links:

* [Github](https://github.com/Codag/AlchemyApiBundle)
* [Packagist](https://packagist.org/packages/codag/alchemyapi-bundle)
