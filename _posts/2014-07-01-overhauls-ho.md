---
layout: post
title:  "Overhauls, ho!"
date:   2014-07-01 17:57:00
categories: kancolletool development
---

If anyone wondered what's going on with KCT (or worse, thought it dead), the simple answer is: overhauls!

The codebase had a lot of old stuff in it that got in the way more than it really helped.  
Qt's build tool qmake works wonders for simple projects, but that description hasn't fit KanColleTool for a very long time now - it had several custom build scripts, a ton of hacks to make qmake do what we want, and the build process broke all the time, especially on Windows.

Getting new versions out should be as simple as running the script that updated the version number, hitting Build on two different machines and poking our Linux maintainer.  
In reality, getting a new build out on OSX and Windows alone usually took me around one to two hours of work.

For the next version, I'm converting everything to use [CMake](http://cmake.org/). CMake is an amazing tool, that can be made to swallow nearly any project structure, spit out build files for anything from GNU Make to Visual Studio, and build external dependencies while it's at it.

Oh, and it can spit out platform-specific installers and archives too.

In addition, the next version will have:

- **Properly working translation submission**  
  The current version has trouble with blacklisting, which floods the translator with junk data to the point where translating anything becomes impossible. Oops.
  
  The next version will let us blacklist things remotely, without having to push client updates.
  
- **Fully working notifications**  
  Several notifications in the current version just don't work, mainly because the current notification system is kind of a mess.
  
- **Growl notifications**  
  The next version will optionally let you use [Growl](http://growl.info/) or [Growl for Windows](http://growlforwindows.com/) for notifications.
  
  For Windows users, this is a huge improvement over the tray bubbles that are all the system offers natively\*, and you should definitely try it out.  
  For Mac users, using Growl vs Notification Center is just a matter of taste, and they both work almost identically.
  
  This would have been impossible with the old qmake-based build system, by the way.
  
  <small>\* Yes, Windows 8 has proper notifications, but non-Metro applications can't use them. Thanks, Microsoft.</small>
