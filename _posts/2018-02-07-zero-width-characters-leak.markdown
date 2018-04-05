---
layout: post
title:  "Zero Width Characters - Information leak "
date:   2018-04-05 10:22:33 +0100
categories: Security 
---

Companies spend usually millions trying to secure their most valuable asset: Their information (or let me rephrase, their data processed in a way that provides value to the company and their customers). 

Being their goal avoiding leakage of information, corporate laptops are locked (with all the inconvenience this causes to users!), user permissions reduced to the minimum, access to external resources is blocked, Internet navigation filtered, network packets are analysed…

But did you know that you can leak information and bypass all of this million costing technologies without spending a single $ by using zero width characters in your mails or documents? As you may imagine, zero width characters are not visible (that’s the “zero width” part) but they can be present in a document, mail, etc.

So, it’s easy to tamper any text or document using a simple technique like encoding your text to be leaked in a binary representation and then using a couple of zero width characters to represent the corresponding 0 and 1. You just need to insert this “invisible” zero width characters binary representation in your text or document and “voilà”…you are leaking your company next secret project into an innocent mail you are sending to your old granny.

The same technique can be used in the opposite direction by companies to fingerprint documents with invisible messages and reveal where they were leaked from, so be careful the next time you just copy & paste something confidential if your intentions are not honest. 

Disclaimer: You should never leak private information as you may face serious legal consequences. This text and link are for educational purposes only. :)

If you want to read a very interesting article regarding this subject (with examples, source code and even a live demo) please check:

[https://medium.com/@umpox/be-careful-what-you-copy-invisibly-inserting-usernames-into-text-with-zero-width-characters-18b4e6f17b66]