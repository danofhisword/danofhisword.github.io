---
layout: post
title:  "Dropwizard Validation and ResourceTestRule"
date:   2014-09-25 17:06:35
categories: dev dropwizard
---

Dropwizard support testing of Resources via [ResourceTestRules][dwresourcetesting].

If you're using Dropwizard [Validation][dwvalidation] and you want to test the 422 responses and error messages using ResourceTestRule, you might try something like ...

{% highlight java %}
 
  // setup you ResourceTestRule

  @ClassRule
  public static final ResourceTestRule resources = ResourceTestRule.builder()
            .addResource(new TestWebResource())           
            .build();

  // ... and in your actual test you might do 

  final ClientResponse response = resources
                .client()
                .resource("/test/something")
                .accept(MediaType.APPLICATION_JSON)
                .type(MediaType.APPLICATION_JSON)
                .post(ClientResponse.class, someInvalidRequest);
        
  // Expecting a 422 ... 
  assert(response.getStatus() == 422);

{% endhighlight %}

But instead you might get a RuntimeException something like... 

{% highlight java %}
ERROR [2014-09-25 14:32:31,234] com.sun.jersey.spi.container.ContainerResponse: The RuntimeException could not be mapped to a response, re-throwing to the HTTP container
! javax.validation.ConstraintViolationException: The request entity had the following errors:
! at io.dropwizard.jersey.jackson.JacksonMessageBodyProvider.validate(JacksonMessageBodyProvider.java:95) ~[dropwizard-jersey-0.7.1.jar:0.7.1]
{% endhighlight %}

The trick is you, you need to wire up a provider to handle the Exception as per Dropwizard's normal pipeline.

You can do that as follows, adding the ConstraintViolationExceptionMapper using "addProvider":

{% highlight java %}
	@ClassRule
    public static final ResourceTestRule resources = ResourceTestRule.builder()
            .addResource(new TestWebResource())
            .addProvider(ConstraintViolationExceptionMapper.class)
            .build();
{% endhighlight %}

Now, you should see your normal 422 responses and error messages come out when using ResourceTestRule! 

[dwresourcetesting]: http://dropwizard.io/manual/testing.html#testing-resources
[dwvalidation]: http://dropwizard.io/manual/core.html#validation