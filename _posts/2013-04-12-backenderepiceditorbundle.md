---
layout: post
title: "BackenderEpicEditorBundle"
description: "EpicEditor Bundle for Symfony"
category: publication
tags: [symfony2, epiceditor]
---
{% include JB/setup %}

This bundle provides a EpicEditor integration for your Symfony2 Project. It simply adds the form field type `epiceditor` to the Form Component.

For more information about EpicEditor see: http://oscargodson.github.com/EpicEditor/
## Installation

1. Add BackenderEpiceditorBundle to your composer.json
2. Enable the bundle
3. Install bundle assets
4. Configure the bundle (optional)
5. Add the editor to a form

### Step 1: Add BackenderEpiceditorBundle to your composer.json
{% highlight js %}
{
    "require": {
        "Backender/epiceditor-bundle": "*"
    }
}
{% endhighlight %}

Update your project dependencies:

{% highlight bash %}
php composer.phar update Backender/epiceditor-bundle
{% endhighlight %}

### Step 2: Enable the bundle
{% highlight php %}
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Backender\EpiceditorBundle\BackenderEpiceditorBundle(),
    );
}
{% endhighlight %}

### Step 3: Install bundle assets
{% highlight bash %}
$ php ./app/console assets:install web --symlink
{% endhighlight %}

--symlink is optional

### Step 4: Configure the bundle (optional)

For a full configuration dump use:
{% highlight bash %}
$ php ./app/console config:dump-reference BackenderEpiceditorBundle
{% endhighlight %}

An example configuration:

{% highlight yaml %}
backender_epiceditor:  
    class:                Backender\EpiceditorBundle\Form\Type\EpiceditorType 
    container:            epiceditor 
    basepath:             /web/bundles/backenderepiceditor 
    clientSideStorage:    true 
    localStorageName:     epiceditor 
    parser:               marked 
    focusOnLoad:          false 
    file:                 
        name:                 epiceditor 
        defaultContent:       
        autoSave:             100 
    theme:                
        base:                 /themes/base/epiceditor.css 
        preview:              /themes/preview/github.css 
        editor:               /themes/editor/epic-dark.css 
    shortcut:             
        modifier:             18 
        fullscreen:           70 
        preview:              80 
        edit:                 79 
{% endhighlight %}

Or even overwrite the configuration within a FormBuilder (see Step 5).

### Step 5: Add the editor to a form

Example form:

{% highlight php %}
<?php

$form = $this->createFormBuilder($post)
    ->add('content', 'epiceditor', array(
            'container'             => 'epiceditor',
            'basepath'              => '/~marc/blog/web/bundles/backenderepiceditor',
            'clientSideStorage'     => true,
            'localStorageName'      => 'epiceditor',
            'parser'                => 'marked',
            'focusOnLoad'           => false,
            'file'                  => array(
                'name'              => 'epiceditor',
                'defaultContent'    => '',
                'autoSave'          => 100
            ),
            'theme'                 => array(
                'base'              => '/themes/base/epiceditor.css',
                'preview'           => '/themes/preview/github.css',
                'editor'            => '/themes/editor/epic-dark.css'
            ),
            'shortcut'              => array(
                'modifier'          => 18,
                'fullscreen'        => 70,
                'preview'           => 80,
                'edit'              => 79
            )
        ))
        ->getForm()
;
{% endhighlight %}

**Note:** All parameters from config.yml can be overwritten in a form (*excluding `class`*).

##Contribute

If the bundle doesn't allow you to customize an option, I invite you to fork the project, create a feature branch, and send a pull request.

To ensure a consistent code base, you should make sure the code follows
the [Coding Standards](http://symfony.com/doc/2.1/contributing/code/standards.html).


##License

This bundle is under the MIT license. See the complete license [here](https://github.com/backender/BackenderEpiceditorBundle/blob/master/LICENSE).

## Next Steps

- Testing is coming soon