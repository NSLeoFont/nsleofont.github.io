---
layout: post
title:  "Cocoapods - Creating a Swift private pod in Github"
date:   2018-01-26 10:22:33 +0100
categories: Cocoapods iOS Tip Swift
---
Sometimes you need a quick way to distribute your *reusable* Swift components (because we all write reusable components, don't we? :P ) in an easy way with our team. We all know how to install 3rd party libraries and dependencies using Cocoapods so there's no need to explain here all the advantages of using a dependency manager. The question is: Why not do the same with our own code? 

There are wonderful and detailed tutorials out there about how to do it, so this is a practical guide to achieve the final result (being able to distribute our libraries from Github) in a quick way as in "We need it for yesterday".

# Let's go!

Let's assume you have some code you want to share (components, classes etc). 

* Make sure your classes (and their related ones) have valid access modifiers in their initializers, properties and methods (e.g. public instead of internal etc). This may be the cause of a lot of errors!

You have to create 2 repos in Github. One will contain your Podspecs (for all your projects) and then you'll have to create a repo for each project you want to share.

* Create a new private repo in Github and name it **ios-podspecs** (or whatever!). Add a README file (which you can later use as a guide of the available Podspecs and link to their repos). Clone it locally.

* Add this private repo to your local cocoapods installation with 
{% highlight bash %}pod repo add ios-podspecs https://yourRepoUrlFromGithub{% endhighlight %}

* Check that everything is fine with:
{% highlight bash %}cd ~/.cocoapods/repos/REPO_NAME
pod repo lint .{% endhighlight %}

* Create the repo for your library. Give it a name. E.g. MyLibrary. Clone it on a local folder.

* Create the Pod project (and an optional Test App and optional Unit Testing!). To do so, type:
{% highlight bash %}pod lib create MyLibrary{% endhighlight %}

* This will open a new project in Xcode. The more important  steps here are **editing the podspec file**(which will contain all the info needed to build it and where you must enter info as the summary, the description, the version, the deployment target, author etc. ) and **adding your classes**. 

* **Editing the podspec**
Here is a sample of some of the important sections as a reference (obviously you'll have to put your own values":
{% highlight bash %}
 (...)
s.name             = 'My Library'
s.version          = '0.1.0'
s.summary          = 'A lovely library'
 
 (...)
s.description      = <<-DESC
A simple lovely library made in Swift
DESC

s.homepage         = 'https://github.com/xxx/MyLibrary'
# s.screenshots     = 'www.example.com/screenshots_1', 'www.example.com/screenshots_2'
s.license          = { :type => 'MIT', :file => 'LICENSE' }
s.author           = { 'Me!' => 'xxx@gmail.com' }
s.source           = { :git => 'https://github.com/xxx/MyLibrary.git', :tag => s.version.to_s }
# s.social_media_url = 'https://twitter.com/Me'

s.platform     = :ios, "10.0"
s.pod_target_xcconfig = { 'SWIFT_VERSION' => '4' }

s.source_files = 'Corker/Classes/**/*' ***
(...)
{% endhighlight %}

One of the most bigger *pain generators* here is *.source_files*. There are zillions of *Stack Overflow* Q&A about it so won't enter into a big detail about it, but basically it specifies where your classes will be in relation to the podspec file in your folder. So it's critical to put this correctly otherwise it won't work! (Will give you a quick tip in a moment on a shortcut if you are in a hurry!)

To check that your podspec file is properly configured you can type the following (you must be in the same folder as the podspec file)
{% highlight bash %}pod lib lint MyLibrary.podspec{% endhighlight %}

If it finds issues it will warn you. If you receive a WARN about not being able to validate the url this is normal since you are using a private repo and you can silence it using the --private modifier.

* **Copying your classes and files**

You can copy all your files inside the *Pod* section /Development Pods/MyLibrary. You'll find a ReplaceMe.swift file there. Many options to be done. Depending on how you copy files here you'll have to adjust your s.source_files in your podspec file. 

**Quick Tip** If you are in a hurry, and don't want to spend zillions of hours adjusting s.source_files, just right click on the Replace Me file -> Show in finder. This will bring you to the classes folder of the project. Copy your classes there and then drag them to the Xcode project...and you won't have to modify the s.source_files at all! :)

If you need to add some demo code or unit tests now is the time. You can add it to the MyLibrary top section and make sure everything builds (CMD+B) and unit tests work (CMD+U). You might have to adjust Target Membership, access modifiers...

**Tip** if you get a unit testing error where the unit tests breaks before starting "Early unexpected exit, operation never finished bootstrapping" just add $(FRAMEWORK_SEARCH_PATHS) to RunPath Search Paths in the Tests Target!

Again, to check that your podspec file is properly configured you can type the following (you must be in the same folder as the podspec file)
{% highlight bash %}pod lib lint MyLibrary.podspec{% endhighlight %}

* Push your library to its repo and TAG it

{% highlight bash %}

// Commit and push as usual
git push -u  

// Then push tags with the version -> Must be the same as the podspec file!
git tag '0.1.0'
git push --tags
{% endhighlight %}

* Push to spec repo

You need to confirm that your spec is correct and then push the pod to your private podspec repo

{% highlight bash %}
pod spec lint MyLibrary.podspec

pod repo push ios-podspecs MyLibrary.podspec
{% endhighlight %}

* **And we are done! Ready to share!**

From this moment if someone else in your team needs to use your wonderful library it's very simple. 

1. They need to add the private repo to their local Cocoapods:

{% highlight bash %}pod repo add ios-podspecs https://UrlOfThePodSpecsRepo{% endhighlight %}

2. They need to specify the source url on their pod file and from there they can use the pod as they'd use any other 3rd party dependency.

{% highlight bash %}
// This would go on top of the pod:

source 'https://github.com/xxxx/ios-podspecs.git'

// And this inside each target you want to use your library:
target 'MyProject' do

  use_frameworks!

  # Pods for MyProject
  pod 'MyLibrary', '~>0.1.0'

  // Etc
{% endhighlight %}

I hope you found this useful. Life is too short to waste it trying to configure things! :)

