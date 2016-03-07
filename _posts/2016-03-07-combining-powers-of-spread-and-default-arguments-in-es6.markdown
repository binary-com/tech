---
layout: post
title: "Combining Powers of Spread and Default Arguments in ES6"
modified:
categories: 
description:
tags: []
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2016-03-07T13:35:25+08:00
---

I am working on a unfamiliar library, with tons of configurations, I wanted to create an domain-specific javascript
abstractions so that other developers can use the complicated library without worrying too much of those options.

Throughout the development process, I need to change the my abstractions frequently, as we do not have very specific
requirements upfront.

As you know, refactoring Javascript is ***no fun***, for example

##Case 1 - Simple functions without default arguments##

Change this:

{% highlight javascript %}
const buyStuff = (productName, quantity, location) => { // do something };
{% endhighlight %}

To

{% highlight javascript %}
const buyStuff = (productName, useCreditCard, quantity, location) => { // do something };
{% endhighlight %}

will break all the caller(buyer) at ***Runtime !!!*** or ~~unit-test~~ which you do not have.

Luckily, what I did is obviously wrong, you should refactor them into

{% highlight javascript %}
const doSomething = (productName, quantity, location, useCreditCard = false) => { // do something };
{% endhighlight %}

So now the old caller code will still work (at least in this context), this is much safer.

##Case 2 - Functions with default arguments##

Things get uglier when you have default arguments, consider the below scenario

{% highlight javascript %}
const brewCoffee = (type, duration = '5 mins', volume = '20ml') => { // brew ur coffee }
{% endhighlight %}

Then suddenly you find out you need to provide a boolean argument `extraStrong`, because average coffee are not for
programmers  =)

So you will have 2 choices

###Method 1 - add the extra arguments to the end###

{% highlight javascript %}
const brewCoffee = (type, duration = '5 mins', volume = '20ml', extraStrong = true) => { // brew ur coffee }
{% endhighlight %}

Now non-programmers will hate you, because they need to change their orignally cute and simple


{% highlight javascript %}
brewCoffee('expresso)
{% endhighlight %}

to

{% highlight javascript %}
brewCoffee('expresso', '5 mins', '20ml', false)
{% endhighlight %}

This need to be change in **Every single caller!!!**

###Method 2 - add the extra arguments right in front of params with defaults###

{% highlight javascript %}
const brewCoffee = (type, extraStrong = true, duration = '5 mins', volume = '20ml') => { // brew ur coffee }
{% endhighlight %}

Now you annoy people which liked to have a mugful of coffee, originally they have

{% highlight javascript %}
brewCoffee('latte', duration = '10 mins', volume = '500ml');
{% endhighlight %}

now they need to change it to

{% highlight javascript %}
brewCoffee('latte', false, duration = '10 mins', volume = '500ml');
{% endhighlight %}

again, **everywhere** the code is used


##ES6 to the Rescue !!##

In ES6, there's a [pattern extraction / destructuring](https://github.com/lukehoban/es6features#destructuring)

which can be combined with defaults parameter to create flexible + highly usable function interface
An example

{% highlight javascript %}
const {foo: x = 'halo', bar: y} = {bar: 'world'}
// x = 'halo'
// y = 'world'
{% endhighlight %}

we can use this to create function like this

{% highlight javascript %}
const brewCoffee = ({type, duration: duration = '5 mins', volume: volume = '300ml'} = {}) => { // do stuff }
{% endhighlight %}

The key advantage is now **parameter's order does not matter**, a huge win!!
You pass in an object and if the key does not exist, it will use the defaults, if you need to support more arguments,
just add in, it does not breaks anyone's code.

And whoever needs to use that parameter are required to change.

Note: remember to have {} as default for the argument, else it will throw error in runtime !
