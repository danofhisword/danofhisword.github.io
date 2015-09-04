---
layout: post
title:  "X-HTTP-Method-Override in Dropwizard 0.8"
date:   2015-09-04 17:30:00
categories: reading 
---

X-HTTP-Method-Override might be handly when you want to use PATCH in an environment where the client or network only support POST.

Hanselman talks about it [here][Hanselman]

This can be achieved in Dropwizard 0.8.2 using Jersey HttpMethodOverrideFilter as follows:

{% highlight java %}
 
 	@Override
    public void run(YourConfiguration configuration, Environment environment) {
        ...
        environment.jersey().register(new HttpMethodOverrideFilter()); // support X-HTTP-Method-Override
        ...
    }

{% endhighlight %}

Here's a sample Jersey client using X-HTTP-Method-Override and POST with JSON patch:

{% highlight java %}

 	 new JerseyClientBuilder().build()
 	 	.target("http://localhost:8080/patchable/1")
        .request(MediaType.APPLICATION_JSON)
        .header("X-HTTP-Method-Override", "PATCH")
        .post(Entity.json("[{\"op\":\"remove\",\"path\":\"/foo\"}]"));

{% endhighlight %}

[Hanselman]:http://www.hanselman.com/blog/HTTPPUTOrDELETENotAllowedUseXHTTPMethodOverrideForYourRESTServiceWithASPNETWebAPI.aspx
