---
layout: post
title:  "Sharing and Isolating Steps and State in Cucumber JVM and Groovy"
date:   2014-11-18 16:45:00
categories: test dev cucumber bdd
---

I've been doing a bunch of BDD acceptance tests and automation lately, having some wins and losses and learning new stuff as I go. 
Whilst enjoying working with Cucumber, I had a bit of challenge with sharing and issolation.

I'm not sure I like the solution I arrived at, I need to use it in anger more to see that for myself. 
It's a bit complex and I still don't like the global-ness of the world stuff, could be evolved somewhat. 
I hope it at least gives some ideas to others or helps someone come up with something more elegant!

<b>The problem ...</b>

I was writing a bunch of scenarios for Features and wanted to re-use Steps and related code across various Features.

I was hoping to make it quicker to compose new Scenarios, quicker to modify and maintain by drying up my code a bit.
Cucumber and Groovy can be a bit challenging, the way step files are compiled and the scope of variables between them seemed to cause me the challenges.

I also desired to encapsulate some state and operations so they are not shared, to avoid wierd global state side-effects impacting Scenarios. 

My general desire was:

<ul>
	<li>A JUnit runnable which ran my Features based on a Annotation (e.g. @AcceptanceTest).</li>
    <li>Many features, multiple Feature files (one per Feature), multiple Scenarios within the Feature.</li>
	<li>A dedicated Step file for a Feature, which also contains state for the Features Scenarios.</li>
	<li>A shared Step file and class for the bits I wanted to re-use across Features.</li>
	<li>Independence of tests, state not carried over between scenarios.</li>
	<li>Not too complex/abstract to understand (not sure I got there). Clear hierarchy, not spaghetti.</li>
</ul>

<img src="/images/CucumberSharing.gif" height="300px">

Some of the pains I seemed to encounter:

<ul>
	<li>You can't see variables defined in one step file in another step file</li>
	<li>You can't see variables in global scope (or world scope) within any classes within step files</li>
	<li>Can't have steps with the same "phrase" in multiple feature files, they clash at runtime</li>
	<li>Even using glue to try and separate steps with separate JUnit runnable tests, it seems Cucumber will still by convention find your other steps and they clash</li>
	<li>Mix-ins seem useful, but seem to get themselves in a pickle when running in cucumber, things getting defined in scope you wouldn't expect and lazy re-initialising when you don't expect</li>
	<li>World can only be defined once, you can't redefine it or add to it easily</li>
	<li>I even got in a weird state where the one Step file was being compiled (I think) twice, saying Steps were duplicates of themselves... that was fun.</li>
</ul>

<b>Where I'm at so far ... </b>

I used a combination of World() and Feature specific "State" classes in Step files.

I defined the World once, but I use @Before hooks to refresh the World properties in place.

Note, I also pass the world reference in (delegate) when I create the step state, so I can use properties from the world internally or delegate to it.
This was kind of the discovery for me that led to the pattern I have now for better or worse. 

Here's an example shared steps file:

{% highlight java %}

World(){
    // By delegating World to SharedWorld, we can see it in other step files
    new SharedWorld()
}
 
class SharedWorld {
    MySharedProperty sharedProperty
    // Some shared functions I want to call elsewhere
    def mySharedMethod(){
    }
}
 
Given(~'^I did some shared stuff and checks$') { ->
    // Cool, I can see this stuff, it's in World scope, I'll use it
    mySharedMethod()    
    assert sharedProperty != null
}

{% endhighlight %}

And here is an example feature steps:


{% highlight java %}

MyFeatureTestState featureTestState
 
Before('@MyFeature'){
    // Re-initialise the state before the test
    // Maybe I should re-init the world here too? 
    featureTestState = new MyFeatureTestState(delegate);  // delegate is shared world delegate
}
 
class MyFeatureTestState{
    
    // Pass in the world, so we can reference values from world scope in here
    def world
    MyFeatureTestState(def world){
        this.world = world
    }
 
    MyFeatureProperty myFeatureProperty
 
    def myFeatureMethod(){
        // So you can access world values in here now
        world.sharedProperty = "foo"
        myFeatureProperty = "bar"
    }
}
 
Then(~'^some feature specific step$') { ->
    
    // So I can access my feature state here by name
    featureTestState.myFeatureMethod()
    assert featureTestState.myFeatureProperty != null
 
    // I can also access my shared state as the World
    assert sharedProperty != null
}

{% endhighlight %}

And a feature: 

{% highlight java %}

@MyFeature
Feature: A feature
Scenario: Made of shared and specific steps
Given I did some shared stuff and checks     # a shared step
Then some feature specific step    # a feature specific step

{% endhighlight %}

<b>UPDATE</b>

A collegue of mine also mentioned the use of the Groovy [@Field][GroovyField] annotation on your global fields, so you could see them in other step files. 

Probably something to use at your discretion w.r.t global state.

I need to play with it some more, could be a simpler solution than what I have above!  

In shared script:

@Field
SomeObject myObject
 
In other Groovy scripts:

myObject.doSomething()

[GroovyField]: http://groovy.codehaus.org/gapi/groovy/transform/Field.html

