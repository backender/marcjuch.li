---
layout: post
title: "Time and allocation profiling in Haskell"
description: "A few things to watch out while profiling (time and allocation) with Haskell and Cabal"
category: blog
tags: [haskell, profiling, cabal]
---
{% include JB/setup %}

This post describes how to enable time and allocation profiling in Haskell
(with cabal). When I first tried so, it took me a while to get this working. It
is basically very straightforward but there are a few things to watch out for,
which I want to uncover now.

<br>

## Configuring Cabal

First of all, to avoid a mess with dependencies (packages) they are not
installed for profiling, one should change the cabal configuration to enable
executable-profiling as well as library-profiling by default. To do so open
the file **~/.cabal/config** and change the following parameters from false
to **true**:

{% highlight bash %}
library-profiling: True
library-profiling: True
{% endhighlight %}

<br>

## Configuring the project

The project you are working at has to be configured for profiling as well.
Therefore use the following cabal command:


{% highlight bash %}
cabal configure --enable-library-profiling --enable-executable-profiling
{% endhighlight %}

After this, the required dependencies can be installed:

{% highlight bash %}
cabal install -p
{% endhighlight %}

*The option -p can be omitted if cabal was configured globally as described before*

<br>

## Configuring executable

While building the executable, several options can be passed along to the
GHC. In this example we refer to the *Time Profiling* chapter from [Real
World Haskell](http://book.realworldhaskell.org/):

{% highlight bash %}
cabal build --ghc-options="-prof -auto-all -caf-all -fforce-recomp"
{% endhighlight %}

 - `-prof` Enables basic time and allocation profiling
 - `-auto-all` Add [cost centres](https://downloads.haskell.org/~ghc/7.0.4/docs/html/users_guide/profiling.html#cost-centres) on all top level functions
 - `-caf-all` Include [constant applicative forms (CAF)](https://wiki.haskell.org/Constant_applicative_form) (one time computed values)
 - `-fforce-recomp` Force full recompilation

Further explanation can be found here: [Profiling and optimization](http://book.realworldhaskell.org/read/profiling-and-optimization.html)


## Run the program

After the executable was built successfully, we now can run the program. By
doing so we can pass options to the runtime-system (RTS) by appending +RTS
-options. In our case of time and allocation profiling we run the program as
follows:

{% highlight bash %}
./dist/build/program/program +RTS -p
{% endhighlight %}

 - `-p` Produces a time profile report and is written to the file **program.prof**
 - More options can be found here: [Time and allocation profiling](https://downloads.haskell.org/~ghc/7.8.2/docs/html/users_guide/prof-time-options.html)

