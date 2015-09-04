---
layout: post
title:  "Dropwizard Jersey Client 2.21 Http PATCH"
date:   2015-09-04 17:30:00
categories: dev 
---

Small trick when using HTTP PATCH on Jersey HTTP client ...
You may have to flick the switch  HttpUrlConnectorProvider.SET_METHOD_WORKAROUND to true 

I found solution [here][blog] 

Beware the [documentation statement][docs]:

"NOTE: Enabling this property may cause security related warnings/errors and it may break when other JDK implementation is used. Use only when you know what you are doing"

{% highlight java %}

 	  return client
                .target("http://localhost:8080/patchable/1")
                .request(MediaType.APPLICATION_JSON)
                .property(HttpUrlConnectorProvider.SET_METHOD_WORKAROUND, true)
                .method("PATCH", Entity.json(json));

{% endhighlight %}

[docs]:https://jersey.java.net/apidocs/2.11/jersey/org/glassfish/jersey/client/HttpUrlConnectorProvider.html#SET_METHOD_WORKAROUND
[blog]:http://jersey.576304.n2.nabble.com/Sending-a-PATCH-request-from-JAX-RS-client-td7583240.html