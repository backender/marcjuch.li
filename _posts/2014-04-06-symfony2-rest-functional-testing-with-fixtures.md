---
layout: post
title: "Symfony REST API functional testing with fixtures"
description: "Using Google Custom Search with Symfony"
category: blog
tags: [symfony2, REST, datafixtures, api, testing]
---
{% include JB/setup %}

Starting a new Symfony2 project these days will often end up in providing a RESTful API. Therefore one of the most important parts in delivering a stable API is testing. Some research in this area makes clear that all API methods should be tested functinally.

<br>

## Entity / Data Fixtures

One of the first steps of a Symfony2 project is to create all the entites according to the given domain model. Everybody who created a couple of entites with some relationships among themselves will confirm that there were issues to be solved at the time when some data should have been entered. This is just one reason why to setup up data fixtures using the `DoctrineFixturesBundle`.

The bundle is very easy to setup and will help to make your schema more solid and allows to generate useful test data. More information here: [http://symfony.com/doc/current/bundles/DoctrineFixturesBundle](http://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html)

Let's assume we have the entity `member` with the following columns:
	
- acronym
- firstName
- lastName


We therefore create the following data fixture class:

{% highlight php %}
<?php
namespace Acme\MemberBundle\DataFixtures\ORM;

use Doctrine\Common\DataFixtures\AbstractFixture;
use Doctrine\Common\DataFixtures\OrderedFixtureInterface;
use Doctrine\Common\Persistence\ObjectManager;
use Acme\MemberBundle\Entity\Member;

class LoadMemberData extends AbstractFixture implements OrderedFixtureInterface
{
	static public $members = array();

    /**
     * {@inheritDoc}
     */
    public function load(ObjectManager $manager)
    {
        $member1 = new Member();
        $member1->setAcronym('LKY');
        $member1->setFirstName('Lucky');
        $member1->setLastName('Luke');
        
        $member2 = new Member();
        $member2->setAcronym('JWN');
        $member2->setFirstName('John');
        $member2->setLastName('Wayne');
       
        $manager->persist($member1);
        $manager->persist($member2);

        $manager->flush();

        $this->addReference('member-1', $member1);
        $this->addReference('member-2', $member2);

        self::$members = array($member1, $member2);
    }
}
{% endhighlight %}

<br>

## Controller

I won't deep into controller actions in this article since there are some very nice tutorials out there:

- [REST APIs with Symfony2: The Right Way](http://williamdurand.fr/2012/08/02/rest-apis-with-symfony2-the-right-way/)
- [Symfony2 REST API: the best way](http://welcometothebundle.com/symfony2-rest-api-the-best-2013-way/)

Let's just assume we are in a TDD environment and are supposed to implement the following API methods:

	GET 	/api/memebers		member_all	Returns a list of all members
	GET 	/api/memebers/{id}	member_get	Resturn a member specified by id

<br>

## Functional Tests

Now since we have our test data provided by the data fixtures, we want to use this data in our tests as well. We therefore are going to take advantage of the `LiipFunctionalTestBundle`. For more information about the bundle [here](https://github.com/liip/LiipFunctionalTestBundle)

As you can see in the test class below we are now able to call `$this->loadFixtures()` which takes an array of fixtures as an argument.

{% highlight php %}
<?php
namespace Acme\MemberBundle\Tests\Controller;

use Liip\FunctionalTestBundle\Test\WebTestCase;

class MemberControllerTest extends WebTestCase {

    static public $expected = array(
        '{"members":{"id":1,"acronym":"LKY","firstName":"Lucky","lastName":"Luke"}}',
        '{"members":{"id":1,"acronym":"JWN","firstName":"John","lastName":"Wayne"}}',
    );

    public function setUp(){
        $this->client = static::createClient();
    }

    protected function assertJsonResponse($response, $statusCode = 200) {
            $this->assertEquals(
                $statusCode, $response->getStatusCode(),
                $response->getContent()
            );
            $this->assertTrue(
                $response->headers->contains('Content-Type', 'application/json'),
                $response->headers
            );
    }

    public function testAllAction() {
        $fixtures = array('Acme\MemeberBundle\DataFixtures\ORM\LoadMemberData');
        $this->loadFixtures($fixtures);
        $members = LoadMemberData::$members;
        $limit = 2;

        for($i=0; $i<$limit; $i++) {
            $route =  $this->getUrl('member_all', array('id' => $members[$i]->getId(), '_format' => 'json'));

            $this->client->request('GET', $route, array('ACCEPT' => 'application/json'));
            $response = $this->client->getResponse();
            $content = $response->getContent();

            $this->assertJsonResponse($response, 200);
            $this->assertEquals($expected[$i], $content);
        }
    }


    public function testGetAction() {
        $fixtures = array('Acme\MemeberBundle\DataFixtures\ORM\LoadMemberData');
        $this->loadFixtures($fixtures);
        $members = LoadMemberData::$members;

        $route =  $this->getUrl('member_get', array('id' => $members[0]->getId(), '_format' => 'json'));

        $this->client->request('GET', $route, array('ACCEPT' => 'application/json'));
        $response = $this->client->getResponse();
        $content = $response->getContent();

        $this->assertJsonResponse($response, 200);
        $this->assertEquals($expected[0] , $content);
    }
    
}
{% endhighlight %}
