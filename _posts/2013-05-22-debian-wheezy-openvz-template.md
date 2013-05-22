---
layout: post
title: "Debian Wheezy OpenVZ Template"
description: "Ready to use Debian Wheezy OpenVZ Template "
category: publication
tags: [openvz, debian]
---
{% include JB/setup %}

## Introduction

This is an [OpenVZ](http://openvz.org/Main_Page) Template from Debian 7.0 (Wheezy) 64-Bit Version. (*Debian 7.0.0 was released Saturday, 4th May 2013*). 

The template is based on a debootstrap installation from the official [Debian Repository](ftp://ftp.de.debian.org/debian/). 

## Install

To install this template you can download and save it into the cache directory of your private OpenVZ host server.

{% highlight bash %}
cd /var/lib/vz/template/cache
wget http://marcjuch.li/uploads/debian-7.0-amd64-minimal.tar.gz
{% endhighlight %}

## Launch

Create a new virtual machine using `vzctl create` where VEID is a random Server ID.

{% highlight bash %}
vzctl create VEID --ostemplate debian-7.0-amd64-minimal
{% endhighlight %}

Then `start` and `enter` the virtual machine where VEID is the selected Server ID before.
{% highlight bash %}
vzctl start VEID
vzctl enter VEID
{% endhighlight %}

<br />

*Alternatively the virtual machine can be created within the OpenVZ Web Panel (if installed). The template will be listed in the Installed OS Tempaltes section.*

## Note:
 
There is a bug reported for vzctl due to incompatibility with newer versions of bash. So the command `vzctl enter` will hang. See bug report: [https://bugzilla.openvz.org/show_bug.cgi?id=1812](https://bugzilla.openvz.org/show_bug.cgi?id=1812)

To resolve this bug install the latest version of vzctl to be find here: [http://openvz.org/Download/vzctl/](http://openvz.org/Download/vzctl/)