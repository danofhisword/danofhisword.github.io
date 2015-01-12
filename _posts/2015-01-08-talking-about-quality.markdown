---
layout: post
title:  "Talking About Quality?"
date:   2015-1-8 14:30:00
categories: agile lean team culture quality
---

Over time I've been thinking about "quality" a lot and what it means in lean and agile teams.

The thing is, quality means different things to different people;<br/>

If you're thinking "product" or lean startup, quality might be a factor of <i>"features users love and not wasting time building stuff no-one wants"</i>.
If you're code focused, quality might be be a factor of <i>"being able to sustainably deliver working software at a rapid rate"</i>.

Nobody wants to fall into failure demand fixing up nasty code or dud features all the time ... you can decide what quality means to you.

My experience is, expecially where culture is still maturing, miss-alignment can lead to people, product and technical problems.

The neat thing is, smart people who care and talk (and listen!) about quality find a shared understanding quickly. 
Even better, if handled the right way, quality values become culture.

One of the actions I've been introducing to new teams (as they form) is a "Quality Values" discussion. 
During Iteration Zero we take a moment to talk about what the team believes is quality and the things we value.
I say "values" and not "standards", "rules" and "process" ... rules were meant to be broken right? :)

We then take discussion outcomes and use practices which support our values.
As an ongoing checkpoint we revist the values in retros, to check if the our values or practices need realignment.  
Taking an hour to workshop at the start and 5 minutes in retros can pay back many times over.
Once the practices become culture we often don't even need to revisit anymore.

<h2>In Practice </h2>

I've experienced a team agree that commit test automation was a big Quality Value for them (helping code design quality).
The team revisited it at first retro and found it was low (and the code design reflected that).

Everyone agreed the Quality Value was still important, and took steps to adjust practices and test coverage was almost 100% next retro and refactored code design quality was higher.We adjusted our practices and that became part of our team culture. Test automation continued to remain high, it was just a given and it worked for us. 

The rapid ability to change the codebase due to the design quality was the commercial payback of the Quality Values. 


<h2>How to Get There - Workshop It!</h2> 

Is quality even a priority? Decide that first. 
All "stakeholders" in the project/team/product should agree if it is a priority. 
That should include the folk who are paying the bills and will own the product etc. 

A set of Success Sliders can help here... I find that when it's made visible, few people put Quality down the bottom, unless there is <u>purposeful reason</u> to do so. 

<img src="/images/SuccessSliders.png" height="200px">

{image borrowed from [Craig Smith's The Speed to Cool: Agile Testing & Building Quality In][CraigSmith]}

Next, share your values ... what do you mean when you put quality up the top?

Anyone that knows me knows I'm big on physical sticky notes! 
They are real, fun and fast (it parrallelizes the process of getting everyones ideas).

So maybe start off with a question; <i>"so we agree quality is priority. What do you believe is quality and what do you think it achieves"</i>.
Then have people busy themselves in a timebox sticking notes on the wall.
Then someone can organizes the notes into themes or common groupings. 
Just talk them through, agree, disagree, debate, talk, listen, come out with a manageable set of ideas that represents your values. 

You can then do something as simple as keep these on your team space wall, nice and visible and tick vote them in retro if going poorly.
Or if you like the electronic ways (which I don't), add them in your wiki or some list you can revisit.

Put them on the wall, allow people to talk about your Quality Values, let people know your values (especially new team members).
Keep practicing your values, adapt your values if you need. 

Some of the stuff that's come up for my teams includes more technical things like: 

<ul>
    <li>We'll make sure keeping builds green is a priority</li>
    <li>We'll keep things simple, simplest thing that works</li>
    <li>We'll make sure our product failing doesn't cause others to fail</li>
    <li>We'll make sure our product performs under load</li>
    <li>We'll automate as much acceptance testing as we can</li>
    <li>We'll embrace refactoring our code when it's valuable</li>
    <li>We'll ship small batches often</li>
</ul>

and people things like: 

<ul>
    <li>We'll actively engage with others when we need guidance</li>
    <li>We'll show empathy to those who follow us, with monitoring and alerting, correct logging/exception handling and supporting materials</li>
    <li>We'll actively leverage pairing and "over the shoulder" review with peers</li>
</ul>

and product things like:  

<ul>
    <li>We'll make sure our product works across browsers X, Y and Z</li>
    <li>We'll make sure our UI's are intuitive and simple for users</li>
    <li>We'll test and use data to make decisions about what's important to users</li>
</ul>

I've found a list of 15-20 still manageable on a working basis, or distil it back to a smaller set of values to focus on at any time as the team matures. 
Communicate them at a high level even, maybe you believe in "Technical Quality, People Quality, Product Quality" ... whatever works, <b>the discussion is the important bit</b>.

Some values may be more specific, some may be pretty abstract.
I don't think it matters, as long as you all understand what you mean and it's making things better.
Is unit testing your value, or is the code design quality your value... whatever, make it better, move on.

So that's my experience in one practice which has helped team quality. 

I'd love to hear about what others are doing to support quality in what they do, and whether you've tried similar practices. Did they help or hinder your team? 

Jokes about the quality of my writing or spelling will be deleted ;)

[CraigSmith]: http://www.slideshare.net/smithcdau/the-speed-to-cool-agile-testing-building-quality-in-39135501

