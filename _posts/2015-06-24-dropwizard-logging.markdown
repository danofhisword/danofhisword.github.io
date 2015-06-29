---
layout: post
title:  "Dropwizard Logging Inbound/Outbound Http Request Bodies"
date:   2015-06-29 17:30:00
categories: dev dropwizard
---

For ease of debugging and testing, I needed to support logging all inbound and outbound http requests and responses including the body from my service built with DropWizard 0.8.1 app. 

I got this happening as follows, using a LoggingFilters with Jersey.

Note, the "true" parameter in code below, this causes response and request body to be logged ...

Everything comes out as INFO level so make sure you're config.yaml is set to a suitable log level to see it.

Hope it helps!

{% highlight java %}
 
/* snip */

  // Logging outbound request/response 
  Client client = new JerseyClientBuilder(environment)
      .build("myclient") 
      .register(new LoggingFilter(Logger.getLogger("OutboundRequestResponse"), true));

/* snip */
      
  // Logging inbound request/response 
  environment.jersey().register(new LoggingFilter(Logger.getLogger("InboundRequestResponse"), true));

/* snip */

{% endhighlight %}

