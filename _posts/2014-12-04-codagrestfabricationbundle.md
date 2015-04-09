---
layout: post
title: "CodagRestFabricationBundle"
description: "Symfony2 Bundle for RAD of RESTful APIs"
category: publication
tags: [symfony2, rest, architecture]
---
{% include JB/setup %}

[![Build Status](https://travis-ci.org/Codag/RestFabricationBundle.svg?branch=master)](https://travis-ci.org/Codag/RestFabricationBundle)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/Codag/RestFabricationBundle/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/Codag/RestFabricationBundle/?branch=master)
[![Total Downloads](https://poser.pugx.org/codag/restfabrication-bundle/downloads.svg)](https://packagist.org/packages/codag/restfabrication-bundle)
[![Latest Stable Version](https://poser.pugx.org/codag/restfabrication-bundle/v/stable.svg)](https://packagist.org/packages/codag/restfabrication-bundle)
[![Latest Unstable Version](https://poser.pugx.org/codag/restfabrication-bundle/v/unstable.svg)](https://packagist.org/packages/codag/restfabrication-bundle)
[![License](https://poser.pugx.org/codag/restfabrication-bundle/license.svg)](https://packagist.org/packages/codag/restfabrication-bundle)

## Introduction

The FOSRestBundle provides a convenient way to build APIs pretty quick and easy
– which is great. After building a couple of Restful API's using the
[FOSRestBundle](https://github.com/FriendsOfSymfony/FOSRestBundle) one of the
the biggest challenges have always been related to the architectural structure
of the project. Whereas the FOSRestBundle provides great functionalities, it
lacks in providing a scalable workflow. Obviously the latter aspect is
neglected on purpose in order to provide a very flexible bundle to be suitable
in any project. However, as a project grows this can cause troubles and thus, I
not infrequently found myself writing tons duplicate code.

The CodagRestFabricationBundle shall fill this gap and provide a proposal
workflow that should prevent repetitively doing the same tasks from the
beginning on. To do so, the we focus on the following facts.

 - GET/PUT/POST/DELETE are always the same.
 - What changes are resources and data.

## Goals

The main goals of this bundle are to allow a user to build a simple API with
less effort and complexity but also provide predefined workflow that shall
bring scalability.

The key part of this workflow is a simple domain manager and form handler they
will be instantiated for each resource (entity). Those services result in a
workflow that will fit for the most requirements of an application that
provides an API. Particularly this mean:

 - GET: Provide either all elements or a subset defined by an identifier.
 - PUT/POST: Update/Create an element using the Symfony Form Component - particularly a Form Builder.
 - DELETE: Remove an element specified by an identifier.

Obviously, a an application gets more complex, this may not fit for any
requirements. Therefore one can hook in the workflow by extending/overriding
given implementations of the CodagRestFabricationBundle. More concrete, this
means:

 - Extend the domain manager (or form handler) and register it as a the service of the related resource instead of the default domain manager (or form handler).

A more technical description, as well as some examples can be found in the
readme file of the repository itself:
[https://github.com/Codag/RestFabricationBundle/tree/develop](https://github.com/Codag/RestFabricationBundle/tree/develop)

## Demo Application

The following pull request will show a subset of the functionalities provided
by the CodagRestBundle in action:
[https://github.com/gimler/symfony-rest-edition/pull/40](https://github.com/gimler/symfony-rest-edition/pull/40)
