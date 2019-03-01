---
title: "Video Game Design with Rx Programming"
date: 2019-02-28T17:59:27-06:00
draft: true
---

In this workshop, you are going to implement a barebones game based on Atari's
Centipede almost from scratch using RxProgramming and ClojureScript.

Motivation
---
Creating a game from scratch is a hard task. However, with modern tools, it's
a lot more easy than when games like centipede where released. Also, working
on different problems related with game engines can teach a lot about the 
internals of modern video games and consoles.

In particular, the events and message system can be hard to master even with 
modern engines like Unity3D or Unreal Engine. More often than not we find 
ourselves implementing collisions or events that involve more than two 
objects and its action might cancel each other. Think on a mutual desctruction
when a missile hits an enemy, which one sends the destroy message to the other
one?, which object updates the score? Do the message bubbles up or is lost when
one object is gone?, Are they on different threads?, Do they have to sync with 
shared memory?, etc. All this questions can be modeled using RxProgramming
with ease without much thinking on concurrency.

While there are implementation of RxProgramming libraries for [multiple 
languages](http://reactivex.io/languages.html), Clojure (and ClojureScript) Async is an interesting option.
While not promoted as RxProgramming, the streams of clojure async allow for
simple handling of concurrency and event composing thanks to [channels](https://clojure.org/news/2013/06/28/clojure-clore-async-channels) and
[transducers](https://clojure.org/reference/transducers). Also, you can find that learning Clojure might be a fun
task if you come from a background of classic imperative programming like 
Python or C/C++.


The tools
---
We will be using ClojureScript and pixi.js. For that we need to have intalled
the following:

+ A Java JDK
+ Node.js

Also, while not required, I recommend to install the following:

+ Visual Studio Code with:
  + Calva
  + Rainbow Brackets
+ Yarn
+ Git

You can download all source code at [http://github.com/juancgalan/RxPixiTutorial]

Java Installation
---
Our first requirement is the JVM. Because ClojureScript uses google closure for
transpiling into Javascript, we need to install a JDK. All examples must work 
fine with either JDK8 or JDK11. I recommend to install OpenJDK when possible using
a package manager.

+ Windows: Use [scoop](https://scoop.sh) with the [java bucket](https://github.com/se35710/scoop-java) or install the hotspot
JVM from the [Oracle's site](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
+ Macos: I'm fan of [Homebrew](https://brew.sh) but for java I install it directly
from Oracle's site](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
+ Linux: Use the package manager from your distro to intall OpenJDK

Nodejs
---
As second requirement, we need nodejs for running our examples. 

+ Windows: Use [scoop](https://scoop.sh) or install the last version from the [Node's Site](https://nodejs.org/es/)
+ Macos: Again, use [Homebrew](https://brew.sh) or the [Node's Site](https://nodejs.org/es/)
+ Linux: Use the package manager from your distro to intall nodejs

All examples where compiled and run using the last nodejs version 11.10.1, I'm not aware
if the LST version can run them, so please install the latest version.

Yarn
---
I like to use yarn for managing node packages, you can install it using the 
suggested package managers or go to the [yarn's site](https://yarnpkg.com/). However, all examples 
must work using npm.

The Editor
---
While all ClojureScript editors should work, I designed all examples using [VsCode](https://code.visualstudio.com/) with
the following plugins:
+ [Calva](https://marketplace.visualstudio.com/items?itemName=cospaia.clojure4vscode)
+ [Rainbow Brackets](https://marketplace.visualstudio.com/items?itemName=2gua.rainbow-brackets)

I also recommend installing [Google Chrome Canary](https://www.google.com/intl/es-419_ALL/chrome/canary/) with the [Devtools Plugin](https://chrome.google.com/webstore/detail/dirac-devtools/kbkdngfljkchidcjpnfcgcokkbhlkogi) for
extra development tools. However, Canary is not required for running the examples.

Base Project
---
All source code is available at the [github repo](https://github.com/juancgalan/RxPixiTutorial).
Inside you can find the `base` folder that contains a barebone project with the
required config files. All steps of the workshop are tagged as following:


Each tag contains the answer for each step of the workshop, I strongly recommend
to first try to follow along before jumping into each commit. I also recommend 
to review the commit history at each tag for checking all source code edits in
detail.

To start with the workshop, please download the master branch and copy the 
`base` folder. After the copy is done, move into the new folder and run the 
following command if you have yarn installed

```
yarn install
```

Or with npm

```
npm install
```

This will intall all JavaScript dependencies. Later on the workshop, you will
need to install the ClojureScript dependencies, for that you have two options: From
the command line run

```
yarn watch
```

This command will run a daemon that will update ClojureScript dependencies, 
compile and hot reload new source code. It will also run a local server at
[http://localhost:8000] where you can see the generated code.

If you are using Visual Studio Code you can run a task using Ctrl+Shift+P and
choosing *run task* from the command palette. Then choose to run the watch 
task without output scanning and you are free to go. Additionally, if you 
installed the Calva Plugin, you can run *Calva Reconnect* from the command
palette also. When the Calva plugin connects, it will enable intellisense for
ClojureScript and start two terminal instances for interacting with live code 
using the Clojure REPL and the ClojureScript REPL.

Agenda
--- 