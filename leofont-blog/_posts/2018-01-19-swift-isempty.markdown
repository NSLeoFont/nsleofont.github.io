---
layout: post
title:  "Swift - isEmpty VS count"
date:   2018-01-19 10:22:33 +0100
categories: Swift 
---
Sometimes when doing code reviews I find code where there's a check for emptiness using the count property. 
In Swift, if your collection doesn't conform to RandomAccessCollection, using count will have lineal complexity O(n) while if you use the isEmpty property you'll have constant complexity O(1). 

For big collections on a mobile app this may mean a difference in performance and it would be a capital sin on a server, where data collections are significantly bigger! 

{% highlight swift %}
let myArray = [ // An array with zillions of data ]

if myArray.count == 0 { // etc } 
// O(n) Complexity will be lineal to the array size!

if myArray.isEmpty { // etc }    
// O(1) Complexity will be constant indepentdently of the array size!

{% endhighlight %}

If your app does multiple emptiness checks this may have a real impact on performance!