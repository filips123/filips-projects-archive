---
title: iOS Frameworks with both 64 bit and 32 bit ARCHs
layout: post
date:
---

I was inspired a great deal by [Jeff Verkoeyen's iOS Framework](https://github.com/jverkoey/iOS-Framework) guide, however there were a few things the broke when apple released the new 64 bit archetectures. Primarily the fact that we now have 32 bit and 64 bit simulator archetectures. Unfortunatly xcode does not give you any way to specify the archs you want for a simulator build, and I have been unable to figure out how to coax a fat binary build when I am compiling for the simulator. I was able to get around this with `xcodebuild` and a few environment variables. I also wanted to keep all the frame work building steps together instead of spreading them across different targets.

At the end of this guide you will have a single aggregate target that will build your library and compile it into a `.framework`. The original targets will just build a binary `.a` as normal.

## 1 Create a normal Cocoa Touch Static Library

### 1.1 Make a new one

If necessary, just make a new library project.

This will just make a normal `.a` binary library with headers. We will add a target that is going to convert it into a proper `framework`

![Framework New](/images/framework-new.png)

### 1.2 Framework Header

You will need to create a main import header for the framework. This is used by the consumers of your library, not the library itself.

For a example if I had a framework called "MyFramework", I might have a `MyFramework/MyFramework` header that would look like this:

```objective-c
#import <Foundation/Foundation.h>
#import <MyFramework/MyFramework.h>
```

I just create that header and add it to my project.

### 1.3 Publishing Headers

First you need to add a Copy Headers build phase. This can be done by going to the menu bar and clicking:

* Editor
* Add Build Phase
* Add Copy Headers Build Phase

You should see something like this when you are done:

![Framework Header Target Membership](/images/framework-cp.png)

To expose your libraries functionality you need to publish the headers you want visable. This is done with XCode's "Target Membership."

To set the membership mash ⌘-⌥-0 and check and uncheck the Target Membership and set it to be "project"

![Framework Header Target Membership](/images/framework-header.png)

**NOTE**: You will have to do this any time you want a header to be included in your `.framework`.

### 1.4 Disable Code Striping

Click on the project name, build settings, choose "combined", and then update the following settings:

* Dead Code Stripping: No
* Strip Debug Symbols During Copy: No
* Strip Style: Non-Global Symbols

## 2 Framework Target

### Create a new aggregate target

From the menu bar add a new target:

* File
* New
* Target...
  * Other
  * Aggregate

![Framework Target](/images/framework-agg.png)

Like Jeff, I prefer to call this target "Framework".

### Add Dependencies

Now Click on the project and edit the "Build Settings" where you can add your library as a dependency for the framework.

![Framework Deps](/images/framework-deps.png)

Now you will see it listed under your project.

![Framework Targets](/images/framework-targets.png)


### Compile The Framework

Now add the build phase that will run the script to do the compilation and file creation.

On the menu bar select:

* Editor
* Add Build Phase
* Add Run Script Build Phase

Now you have two options, you can either paste the script into the Xcode project settings or just use it to call your script. I prefer the latter because then git can track and merge your build script like normal code.

![Framework Build](/images/framework-build.png)

Simply paste a call to your script and adjust the path as is necessary.

```bash
set -e
${PROJECT_DIR}/${PROJECT_NAME}/Scripts/framework-compile
```

Here's the real magic, the script that does all the heavy lifing. This is the one you will want to put in a `Scripts/framework-compile` file and add that to your project.

A couple of notes about this script

* I used ruby, mostly because it got a bit messy handling edge cases in bash. However this uses the system ruby included in Mac OS X with no dependencies at all. It should just work. It even avoids any ruby version managers that may be installed.
* This will recompile all the targets to make sure the required archetectures are included. This is a bit of a bigger hammer solution, but seems less fragile.
* Assumes that `$PROJECT\_NAME` is the same name you use for the library name and framework name. If not just edit the script.

```ruby

```


Hopefully this will
