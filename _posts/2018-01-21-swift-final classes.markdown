---
layout: post
title:  "Swift - final class"
date:   2018-01-22 10:22:33 +0100
categories: Swift Tip
---
Sometimes when designing/implementing classes, properties and methods in Swift we forget *small details* that can make a difference in terms of performance. Small details matter!

We usually write our classes and really don't think so much in inheritance. Maybe the User class will be suclassed, maybe not. Who cares. We just write *class User* and move forward with our code (and our life). 

But the fact is that forgetting about adding access modifiers to our methods and properties, or forgetting to mark our classes as final (unless we need to subclass them), apart from being considered a bad practice from a OOP perspective, has a real performance impact in Swift!

If we don't pay attention to this detail, we'll be giving the Swift runtime an extra amount of work to determine which method or property has to access on each class (direct all/indirect access) via *Dynamic Dispatch*. 


{% highlight swift %}
class Salesman {
   
    var name = "Jesse" // Uses Dynamic Dispatch
    final var surname = "Pinkman" // No Dynamic Dispatch. Marked as final
    
    // Uses Dynamic Dispatch
    func sayHello() {
        print("Hello \(name)")
    }
    
    // No Dynamic Dispatch. Private avoids subclassing.
    private func sayGoodBye() {
        print("Bye \(surname)")
    }
}

// No Dynamic Dispatch. Everything is final as the class is marked as so
final class Researcher {
  
    var name = "Walter"
    var surname = "White"
    
    func sayHello() {
        print("Hello \(name)")
    }
    
    private func sayGoodBye() {
        print("Bye \(surname)")
    }

}
{% endhighlight %}

The Swift compiler can't know if a given class marked with default access modifier (internal) will be overriden anywhere else outside its module **unless** *Whole Module Optimization* is enabled in Xcode. So a good check is to make sure that we have this setting on. 
We should have into consideration that if we have Public methods, properties or classes even though we have *Whole Module Optimization* on, the compiler won't be able to disable Dynamic Dispatch since it won't know if they'll be overridden somewhere else.

So my advise would be that even if we now that the *Whole Module Optimization* may be a safety net (unless we are using Public) it's a good practice to mark classes that won't be subclassed as final and use access modifiers. We'll help the runtime and **especially** our future colleagues who'll have to read our code six months later and will have a much clearer vision of our intent when implementing things... ;)




