---
layout: post
title: "Using Google Custom Search with Symfony"
description: "Using Google Custom Search with Symfony"
category: blog
tags: [symfony2, google, custom search, api]
---
{% include JB/setup %}

In this blog post I'd like to show an example of using Google CSE within your Symfony application. 

Therefore we implement a search functinoality on top of a **magazine archive** (already given in this tutorial) - of course you can replicate this with any other kind of material. The only important thing is a **public** detail view of every magazine with it's **unique URL**. in this post we use: {% highlight bash %} http://example.com/detail/123 {% endhighlight %} *(123 means the unique magazine number)*

**Clarification:**

This is a big limitation but as the title already mentions this should only display an example of how to use Google CSE. In our case this was an efficient way to offer a search functionality, since a separate serach engine (e.g. apache solr) was an overkill.

## Parameters

First of all we define the neccessary parameters within the app/config/parameters.yml file. In this case we need:

* `google_cse_key`: YOURKEY 
  
	The key can be found in Google API Console: https://code.google.com/apis/console/ 

* `google_cse_cx`: YOURCX

	The cx id can be found here: https://www.google.com/cse/manage/all (it's contained in the url of your specific search engine).
	
## Index

So that Google knows what to search when we send a request, we have to configure cse and set an index. This can be done within the CSE management console (https://www.google.com/cse/manage/all). In this example we set the index to: 

{% highlight bash %}
http://example.com/archiv/detail/*
{% endhighlight %}

This means that the **whole detail page of ever magazine** will be indexed. Now all the content on this pages will be recoginzed by our custom search functionality - which is great. No configuration has to be done and the whole page is indexed.

## Form Type (Search Field)

Using Symfonys form compenent, the form type is used to build our form which is nothing more than a simple **input field**. Therefore the name of the field is **q** and it's type is **text**. 

As an optional argument we set **required** to **false**, since the user does not have to put any text in the search field if he doesn't want to.

{% highlight php %}
<?php
namespace Backender\SerachExampleBundle\Form\Type;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;

class ArchivSearchType extends AbstractType
{
    public function buildForm(FormBuilderInterface $formBuilder, array $options)
    {
        $formBuilder
            ->add('q', 'text', array(
                'required'  => false,
            ))
        ;
    }

    public function getName()
    {
        return 'archive_search';
    }
}

{% endhighlight %}

## Repository

Now it's time to define the logic to use Google as the search engine.
At this point a dependency is required to handle http requests. This can be installed using composer.

{% highlight bash %}
composer require kriswallsmith/buzz
composer update kriswallsmith/buzz
{% endhighlight %}


Next a Repository Class needs to be defined in our entity.

{% highlight php %}
<?php
namespace Backender\SearchExampleBundle\Entity;

use Doctrine\ORM\Mapping as ORM;

* @ORM\Entity(repositoryClass="Backender\SearchExampleBundle\Entity\MagazineRepository")

// entity stuff, not part of this tutorial

{% endhighlight %}

### Custom function

Let's define a function called **findByGoogle**. In general this function will go through three steps:

**1. Send request to google with search string and further arguments**

{% highlight php %}
<?php
$curl = new Curl();
$browser = new Browser($curl);
$url = 'https://www.googleapis.com/customsearch/v1';
$url = $url.'?key='.$options['key'].'&cx='.$options['cx'].'&q='.$options['q'].'&alt='.$options['alt'];
$response = $browser->get($url)->getContent();
$response = json_decode($response);
{% endhighlight %}

The previously installed Buzz library will be used. A request to https://www.googleapis.com/customsearch/v1 will be sent with the following url parameters:

- key, the google cse key
- cx, the google cx id
- q, which means the search string
- alt, defines the response format

Then a GET request will be submitted and it's response will be decoded to JSON.

**2. Extract magazine number from response**

{% highlight php %}
<?php
$searchMagazines = array();
foreach($response->items as $item){
    array_push($searchMagazines, substr($item->link, strrpos($item->link, '/') + 1));
}
{% endhighlight %}

The only necessary thing from Google's response is the received **url**. The url contains our unique magazine number. In the code example, for every result the part behind the last "/" (slash) will be extracted and stored in "searchMagazines" (array). In our case this is exactly the magazine number.

**3. Get internal objects using the magazine numbers**

{% highlight php %}
<?php
$magazines = $this->getEntityManager()->createQueryBuilder();
$magazines->select('m')
	->from('Backender\SearchExampleBundle\Entity\Magazine', 'm')
	->where('m.enabled = 1')
	->andWhere($magazines->expr()->in('m.magazineNumber', ':m_array'))
	->orderBy('m.magazineNumber', 'DESC')
	->setParameter('m_array', $searchMagazines)
	;
return $magazines;

{% endhighlight %}

Using the query builder and the extracted magazine numbers the internal objects can be found within a select statement.

And that's it - using a simple request including the search string we have our wanted magazines.

**See the full file here**

{% highlight php %}
<?php
namespace Backender\SearchExampleBundle\Entity;

use Doctrine\ORM\EntityRepository;
use Buzz\Browser, Buzz\Client\Curl;

class MagazineRepository extends EntityRepository
{
	public function findByGoogle($options){

        // PART 1
        $curl = new Curl();
        $browser = new Browser($curl);
        $url = 'https://www.googleapis.com/customsearch/v1';
        $url = $url.'?key='.$options['key'].'&cx='.$options['cx'].'&q='.$options['q'].'&alt='.$options['alt'];
        $response = $browser->get($url)->getContent();
        $response = json_decode($response);

        if(!empty($response->items)){
			
            // PART 2
            $searchMagazines = array();
            foreach($response->items as $item){
                array_push($searchMagazines, substr($item->link, strrpos($item->link, '/') + 1));
            }
			
            // PART 3
            $magazines = $this->getEntityManager()->createQueryBuilder();
            $magazines->select('m')
                ->from('Backender\SearchExampleBundle\Entity\Magazine', 'm')
                ->where('m.enabled = 1')
                ->andWhere($magazines->expr()->in('m.magazineNumber', ':m_array'))
                ->orderBy('m.magazineNumber', 'DESC')
                ->setParameter('m_array', $searchMagazines)
            ;
            return $magazines;

        } else {
            return null;
        }
    }
    
}

{% endhighlight %}


## Controller

The controller part is pretty simple, since the "heavy" stuff is alredy done in the repository class.

**1. Bind the form**

{% highlight php %}
$form = $this->createForm(new ArchivSearchType());
$form->bind($request);
$formData = $form->getData();
$query = $formData['q'];
{% endhighlight %}

First of all we need to bind the form to check if the user sent a serach request.

**2. Call findByGoogle**

{% highlight php %}
$options = array(
	'key' => $this->container->getParameter('google_cse_key'),
	'cx' => $this->container->getParameter('google_cse_cx'),
	'q' => urlencode($query),
	'alt' => 'json',
);
{% endhighlight %}

If a request was sent to the controller with a valid value for q (remember this is the search string), the previously created method "findByGoogle" will be called.

**3. Return the results**

{% highlight php %}
if($magazinesByGoogle !== null) {
	$magazines = $magazinesByGoogle->getQuery()->getResult();;
	return array(
		'magazines' => $magazines,
	);

} else {
	//no results
	return $this->render('SearchExampleBundle:Archiv:search_empty.html.twig');
}        }
{% endhighlight %}

Just return the results to the view. Once you returned you are can treath the magazine objects as you would like to. 

**See the full file here**

{% highlight php %}
<?php
    /**
     * @Route("/search", name="search_example_archiv_search")
     * @Template()
     *
     * @param \Symfony\Component\HttpFoundation\Request $request
     * @return \Symfony\Component\HttpFoundation\Response
     */
    public function searchAction(Request $request)
    {
    	//Part 1
        $form = $this->createForm(new ArchivSearchType());
        $form->bind($request);
        $formData = $form->getData();
        $query = $formData['q'];

        if($query !== null){
            // Part 2
            $options = array(
                'key' => $this->container->getParameter('google_cse_key'),
                'cx' => $this->container->getParameter('google_cse_cx'),
                'q' => urlencode($query),
                'alt' => 'json',
            );

            $magazinesByGoogle = $this->getDoctrine()->getRepository('SearchExampleBundle:Magazine')->findByGoogle($options);
            
            // Part 3
            if($magazinesByGoogle !== null) {
                $magazines = $magazinesByGoogle->getQuery()->getResult();;
                return array(
                    'magazines' => $magazines,
                );

            } else {
                //no results
                return $this->render('SearchExampleBundle:Archiv:search_empty.html.twig');
            }

        } else{
            //not searched, render all or say no results
            return $this->redirect($this->generateUrl('serach_example_archiv_index'));
        }

    }
{% endhighlight %}

## Conclusion

I used this example in a project for one of my customers. I always considered about performance issues by implementing a search functionality this way. But once the implementation was done the results came frighteningly fast. Try it out!