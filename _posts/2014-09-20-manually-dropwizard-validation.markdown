---
layout: post
title:  "Going Manual on Dropwizard Validation"
date:   2014-09-18 17:06:35
categories: dev dropwizard
---

Dropwizard resources support some great declarative [validation][dwvalidation] with the @Valid annotation. 

However, Dropwizard does marshall the errors out to a response object for you and hides some of the underlying details whilst also string formatting a little.
See [ConstraintViolationExceptionMapper][dwConstraintViolationExceptionMapper] and [ConstraintViolations][dwConstraintViolations] format method.

If the default Dropwizard behaviour meets your needs (often the case) you can stop reading now!

For my scenario, I wanted to keep the nice declaritive validation, but get a bit more insight into the error and also control of the response. 

A sample of triggering validation manually is below ... (you should probably inject the ValidatorFactory etc into where you need it).

Jump into your debugger and inspect the violations objects returned ... of interest is usually the properties that violated, and their associated messages. 

{% highlight java %}
		ValidatorFactory factory = Validation.buildDefaultValidatorFactory();	
        Validator validator = factory.getValidator();
        Set<ConstraintViolation<SomeObjectToValidateType>> violations = validator.validate(someInstanceToValidate);
        assert(violations.size() > 1);
{% endhighlight %}

You can usually build on that to create a custom response as you need, either manually rolling a Response object or by creating exceptions and plumbing that into the response pipeline with some [error handling][dwerrorhandling]

Like with anything, if you decide to go it alone, instead of using the standard built in Dropwizard functionality, make sure you have a good reason!

[dwvalidation]:      http://dropwizard.io/manual/core.html#validation
[dwerrorhandling]: https://dropwizard.github.io/dropwizard/manual/core.html#error-handling
[dwConstraintViolationExceptionMapper]: https://github.com/dropwizard/dropwizard/blob/master/dropwizard-jersey/src/main/java/io/dropwizard/jersey/validation/ConstraintViolationExceptionMapper.java
[dwConstraintViolations]: https://github.com/dropwizard/dropwizard/blob/2655ac703a0f6ddb4eae933a74dae3d12fde287f/dropwizard-validation/src/main/java/io/dropwizard/validation/ConstraintViolations.java
