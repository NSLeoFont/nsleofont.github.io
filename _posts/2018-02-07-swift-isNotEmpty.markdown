---
layout: post
title:  "Swift - isNotEmpty "
date:   2018-02-07 10:22:33 +0100
categories: Swift 
---
As I already discussed on a previous post (see [https://nsleofont.github.io/blog/2018/01/19/swift-isempty](https://nsleofont.github.io/blog/2018/01/19/swift-isempty) ) if we need to check if one of our collections is empty, Swift provides a very easy and convenient way to do it using the *isEmpty* property.

However there's no nice way to check if a collection is *NOT empty* and we end up writing ugly code like:

{% highlight swift %}

// Having the negation exclamation mark on first position
// is counterintuitive.

if !collection.isEmpty  (...)  

// Gets worst if you have to force unwrap an optional...

if !collection!.isEmpty (...) // Ouch!

{% endhighlight %}


How handy it would be to be able to do something like: ***collection.isNotEmpty*** wouldn't it?

This small extension will do exactly that, wrapping the ugliness behind the scenes and allowing your code to be shiny and easier to read.

{% highlight swift %}

extension Array {
    
    public var isNotEmpty: Bool {
        
        return !self.isEmpty
    }
}

{% endhighlight %}


From now you'll be able to write *beautiful* code :P:

{% highlight swift %}

if collection.isNotEmpty  (...) // Much better!!!

{% endhighlight %}


Happy coding!!! :)

