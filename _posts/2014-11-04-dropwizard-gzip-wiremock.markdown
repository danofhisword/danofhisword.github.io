---
layout: post
title:  "DropwizardAppRule, Wiremock and GZIP"
date:   2014-11-04 15:20:00
categories: test dev dropwizard
---

I had a bit of challenge when using WireMock to do some stubbed testing in my DropWizard app. 

This was because wiremock didn't support GZIP and the Jersey client had it on by default.
I found my requests weren't matching up when using .withRequestBody()...

The result being the same as described [here][dwissue], "com.github.tomakehurst.wiremock.common.Log4jNotifier: URL /foobar is match, but body is not"

I got around this in my test by having the stubbed test run without GZIP on the default DropWizard Jersey Client
As I was using a DropwizardAppRule, I used config override as below ... 

{% highlight java %}
 
ConfigOverride.config("defaultHttpClient.gzipEnabled", "false")
ConfigOverride.config("defaultHttpClient.gzipEnabledForRequests", "false")

{% endhighlight %}

You can see my post on [Config Overrides][dwconfigoverride] for more info on using overrides in general. 

[dwconfigoverride]: http://danofhisword.com/dev/dropwizard/2014/09/25/dropwizard-testing-config-overrides.html
[dwissue]: https://github.com/tomakehurst/wiremock/issues/106