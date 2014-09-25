---
layout: post
title:  "Dropwizard Integrated Testing and ConfigOverride"
date:   2014-09-25 17:06:35
categories: dev dropwizard
---

Dropwizard supports integrated testing of via [DropwizardAppRule][dwintegratedtesting].

When you spin up the app, you might want to use your default config.yml file and just override some of the values, specific for your test scenario. 

You can use DropwizardAppRule and [io.dropwizard.testing.junit.ConfigOverride][dwConfigOverride] to achieve this. 

Say you have a config yml like:

{% highlight java %}

server:
    applicationConnectors:
       - type: https
         port: 481
    adminConnectors:
       - type: http
         port: 482

someother:
    magicConfig: somevalue

{% endhighlight %}

You can overide some of the configs like following, where I use a base config.yml file, then override a port and set another config value: 

{% highlight java %}
 
   @ClassRule
    public static final DropwizardAppRule<AuthinatorConfig> RULE =
    
            new DropwizardAppRule<>(AuthinatorService.class, "/path/to/your/config.yml",
                    ConfigOverride.config("server.applicationConnectors[0].port", "443"),           
                    ConfigOverride.config("someother.magicConfig", "someothervalue")
                    );

{% endhighlight %}

You'll note that some config nodes are collections (like server.applicationConnectors above), and you have to index reference them... 

I didn't find a good way to figure this out, other than using my debugger and seeing what was and wasn't a collection as the DropwizardAppRule started up. 
There's probably a sensible reason here for what is or isn't a collection, but I didn't think that hard about it :) 

Anyways, I couldn't find much info online about using ConfigOverides, so after a bit of source code digging this was what I came up with, hope it helps!

[dwintegratedtesting]: http://dropwizard.io/manual/testing.html#integrated-testing
[dwConfigOverride]: https://github.com/dropwizard/dropwizard/blob/master/dropwizard-testing/src/main/java/io/dropwizard/testing/junit/ConfigOverride.java
