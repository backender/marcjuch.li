---
layout: post
title: "How to use validation groups in Symfony"
description: "Example of Symfony2 validation groups using an order form example."
category: blog
tags: [symfony2, form, validation]
---
{% include JB/setup %}

In this post I'm going to show how to use validation groups in symfony with the example of an order form.

The validation groups allow to specify which validation assertions (as groups) should be used when you are validating a model.

## Problem
A form should offer the possibility to use separate billing and shipping adresses. Therefore we use a **checkbox**. If the checkbox is selected, it means the user would like to order to a separate shipping adress. So we have to determine which fields of our form should be validated during the form handling process.

![Mockup](/uploads/2013-04-21-validation-groups_mockup.png)

## Solution

Basically we make the following steps:

- **Group** validation contstraints for the shipping related form fields together
- **Determine** which validation contraints are applied, depending on the checkbox value in the submitted form
- **Copy** data from non-shipping fields to shipping fields if checkbox is not selected *(this step is optional)*

### Model

The below listed Symfony2 entity with annotations (provided with [SensioFrameworkExtraBundle](http://symfony.com/doc/2.1/bundles/SensioFrameworkExtraBundle/index.html)) and assertions which describes the following model:

- Book (from a fictive Book Entity)
- Lastname
- Firstname
- Street

But also the possibility for a separate shipping adress:

- UseShipping (this is the checkbox to let the user choose a separate shipping adress)
- Lastname 
- Firstname
- Street

The important thing is to add the *validation group* to the *@Assert* argument of the specific **shipping** fields:

{% highlight php %}
* @Assert\NotBlank(groups={"separateShipping"})
{% endhighlight %}

Now we have created a validation group that includes the specific shipping fields and can now go over and determine which group should apply to the validation process.

<br />
Additionally, if the checkbox was **not** selected, the data of the regular fields are copied to the related shipping field. To call this method [doctrine event listeners](http://docs.doctrine-project.org/en/2.0.x/reference/events.html#implementing-event-listeners) are used.

{% highlight php %}
<?php
...
/**
 * If no separate shipping address is selected,
 * data from personal fields will be mapped to shipping fields.
 * 
 */
public function setShippingData()
{
    if(!$this->useShipping)
    {
        $this->firstNameShip = $this->firstName;
        $this->lastNameShip = $this->lastName;
        $this->streetShip = $this->street;
    }
}
{% endhighlight %}

*This makes sense if automated shipping stickers are generated at a later date.*
<br /><br />

Our example entity looks as follows:

{% highlight php %}
<?php
...
use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Validator\Constraints as Assert;

/**
 * Subscription
 *
 * @ORM\Table(name="subscriber")
 * @ORM\Entity
 */
class Subscriber
{
    /**
     * @var integer
     *
     * @ORM\Column(name="id", type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    private $id;

    /**
     * @ORM\ManyToOne(targetEntity="Book", inversedBy="Subscriber")
     * @ORM\JoinColumn(name="book", referencedColumnName="id")
     */
    private $book;

    /**
     * @var boolean
     *
     * @ORM\Column(name="use_shipping", type="boolean", nullable=true)
     */
    private $useShipping = false;

    /**
     * @var string
     *
     * @ORM\Column(name="first_name", type="string", length=255)
     * @Assert\NotBlank(message="Name missing!")
     */
    private $firstName;

    /**
     * @var string
     *
     * @ORM\Column(name="first_name_ship", type="string", length=255)
     * @Assert\NotBlank(message="Name missing!", groups={"separateShipping"})
     */
    private $firstNameShip;

    /**
     * @var string
     *
     * @ORM\Column(name="last_name", type="string", length=255)
     * @Assert\NotBlank(message="Name missing!")
     */
    private $lastName;

    /**
     * @var string
     *
     * @ORM\Column(name="last_name_ship", type="string", length=255)
     * @Assert\NotBlank(message="Name missing!", groups={"separateShipping"})
     */
    private $lastNameShip;

    /**
     * @var string
     *
     * @ORM\Column(name="street", type="string", length=255)
     * @Assert\NotBlank(message="Adress missing!")
     */
    private $street;

    /**
     * @var string
     *
     * @ORM\Column(name="street_ship", type="string", length=255)
     * @Assert\NotBlank(message="Adress missing!", groups={"separateShipping"})
     */
    private $streetShip;

    /**
     * @ORM\PrePersist
     */
    public function prePersist()
    {
        self::setShippingData();
    }

    /**
     * @ORM\PreUpdate
     */
    public function preUpdate()
    {
        self::setShippingData();
    }

    /**
     * If no separate shipping address is selected,
     * data from personal fields will be mapped to shipping fields.
     * 
     */
    public function setShippingData()
    {
        if(!$this->useShipping)
        {
            $this->firstNameShip = $this->firstName;
            $this->lastNameShip = $this->lastName;
            $this->streetShip = $this->street;
        }
    }
}
{% endhighlight %}


### Form

In the absctract type the method **setDefaultOptions** offers to determine which validation group should be applied during the form handling process. 

Using an anonymous function we can check if the checkbox ($useShipping) was selected in the submitted form data. As return value we provide the validation groups (we defined in the model before).

{% highlight php %}
<?php
...
public function setDefaultOptions(OptionsResolverInterface $resolver)
{
    $resolver->setDefaults(array(
        'csrf_protection' => true,
        'csrf_field_name' => '_token',
        'validation_groups' => function(FormInterface $form){
            $data = $form->getData();
            if($data->getUseShipping() == true)
                return array('Default', 'separateShipping');
            else
                return array('Default');
        }
    ));
}
{% endhighlight %}

<br />
Our example form type looks as follows:

{% highlight php %}
<?php
...
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolverInterface;
use Symfony\Component\Form\FormInterface;

class AboType extends AbstractType
{
    /**
     * {@inheritdoc}
     */
    public function buildForm(FormBuilderInterface $formBuilder, array $options)
    {

        $formBuilder
            ->add('book', 'entity', array(
                'class'  => 'AcmeDemoBundle:Book',
            ))
            ->add('firstName')
            ->add('lastName')
            ->add('street')
            ->add('useShipping')
            ->add('firstNameShip')
            ->add('lastNameShip')
            ->add('streetShip')
        ;
    }

    /**
     * {@inheritdoc}
     */
    public function getName()
    {
        return 'blogpost_validation_group_example';
    }

    /**
     * {@inheritdoc}
     */
    public function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        $resolver->setDefaults(array(
            'csrf_protection' => true,
            'csrf_field_name' => '_token',
            'validation_groups' => function(FormInterface $form){
                $data = $form->getData();
                if($data->getUseShipping() == true)
                    return array('Default', 'separateShipping');
                else
                    return array('Default');
            }
        ));
    }
}
{% endhighlight %}

## Conclusion

The described two steps let us handle form validation for an order form with a separated shipping adress.

## References
[http://symfony.com/doc/2.1/book/validation.html#validation-groups](http://symfony.com/doc/2.1/book/validation.html#validation-groups)