
class: middle, center, inlineimg

# lets talk build systems

@equalsraf

---
class: middle, center, inverse

# "men I have bad news ... all build systems suck"

---

# Software code needs to:

- Be properly commented
- Indentation matters
- Readability is important
- Have unit tests and code coverage
- yada, yada, yada

## ... and when it doesn't you get a bit mad!

---

# Build system recipes need to:

- run and not fail
- that is all

## ... and when it doesn't you're stuck!

---
class: middle, center

# no one looks at build recipes unless they break

---

cmake, GNU make, BSD make, autoconf, automake, nmake, jom, ant, maven, scons, waf, bjam, ninja, vcbuild, msbuild, gyp, qmake, sh/batch/perl/python/ruby/\* scripts, setuptools -- Gradle, premake, Rake, gn -- Sublime, XCODE, Visual Studio, Eclipse project files

I haven't even listed **NIH** derivatives


---

# Can't I just use make for everything?

Maybe, if

- make runs on all platforms where you build
- you don't need to find dependencies (or can assume pkg-config)
- you don't need **compiler/platform checks** - does this system have *termios.h*, does the compiler support *inline*?

This works fine for BSD ports.

---

# And that is why we autoconf/automake

- Need to do lots of checks before you know what to build
  - Compiler version
  - Target platform
  - OS specifics
  - Which header to include to get *stat()*
  - Some targets are optional
- Generate code to abstract all this away
- Then you can call Make

---

# But, but, but ... portability

- Not everyone uses make
- Each system has its own tooling, that make does not understand
- "Meta" Build systems (gyp, CMake) convert recipes from one build system to ... another level of indirection


---

# Things build systems do

- Check you compiler for features
- Check your platform, headers, x86
- Generate code
- Find dependencies
- Download stuff
- Build dependencies
- Run custom commands
- random crazy stuff

---

# Things build systems do

i.e. they need to

1. manage target dependencies
1. understand your compiler/linker tools to some extent
1. allow you to write portable custom extensions

---

# Imperative vs Declarative

Big discussion, just like in programming

- Declarative systems are easier and less error prone
- Imperative tend to be more flexible

...in practice not really

---

# What really matters in a build system ...**SUPPORT**

- Nobody cares about the rest
- chicken and egg problem
- someone has to write recipes to find curses or java or ... and other domain specific tools
- Popular systems (autoconf, cmake, ant, maven) bundle lots of
  recipes for common software

---
class: inverse
background-image: url(onering.jpg)
# one system to rule?

---
# one system to rule ... don't be stupid!

- Build systems have to know your compiler/tools
- This means domain specific problems
- There will always be build systems that are domain specific
  - no "extra" dependencies (self contained)
  - feel natural for that domain


---
class: center, middle
# #Examples
---

# e.g. Neovim

1. [optional] make calls cmake
2. cmake calls autoconf and make and cmake to build external deps (that call make again ...) ... and lua gets called to build lua modules ...
3. make calls make or ninja or whatever

Not as hard as it sounds, the problem are dependencies.

---

# e.g. libuv (Windows)

1. you call vcbuild.bat
2. vcbuild.bat clones gyp from git
3. vcbuild.bat calls gyp to generate msbuild (this needs python2, python 3 is too much)
4. ... and then calls msbuild
5. finally it runs some tests

---

# e.g. Webkit

- Build dependencies: Perl, Python, Ruby, bison, gperf, flex
- LOL there is a script **install-dependencies**
- **$!@%%** Not even gonna try explaining it!

This is improving as CMake gets used more - used to be a lot worse

---

# e.g. Vim

- Ships one Makefile for each platform

Make_bc3.mak  Make_cyg_ming.mak  Make_dvc.mak  Make_manx.mak  Make_morph.mak  Make_sas.mak
Make_bc5.mak  Make_dice.mak      Makefile      Make_ming.mak  Make_mvc.mak    Make_vms.mms
Make_cyg.mak  Make_djg.mak       Make_ivc.mak  Make_mint.mak  Make_os2.mak    Make_w16.mak

- still needs autoconf for dependency detection (in some platforms)
---

# e.g. Python (Windows)

Check PCBuild/readme.txt it just makes you cry


---
class: middle, center, inverse

# chose one that does what you want and sucks less

---

# CMake

- Meta build system, generates Make, Ninja, Codeblocks, Eclipse, KDevelop, Sublime, ...
- The good parts: It is **portable** and has lots of goodies
- Does other things besides C, C++ - just not so well - remember it is all about support
- God only knows what can happen afterwards, you defined a custom command target that
  was turned into a Visual Studio project ...

---

# this is accurate

> I must admit, it was simpler to write cross platform code then to perform cross platform builds!
>
> -- <cite>http://blog.cppcms.com/post/54</cite>
---

# Autotools (autoconf+automake+make)

- Runs on Unix, everyone knows ./configure && make && make install
- Good C/C++
- If that is all you need, great
- (sarcasm) writing configure scripts and m4 is fun

---

# Ant

- Because Java, and your build system needs to understand your compiler remember
- In Android Ant was replaced by Gradle.

---

# Maven 

- Because Ant didn't download and manage 300 dependencies automagically.
- Java is good at messing up dependencies.

---

# gyp

- Comes from Google
- used by V8, Node.js
- files are JSON-ish
- Similar motivation to CMake, meta-generator for ...
- deprecated, replaced by **gn**

---
# Final words

- It is harder to test your build system than your code (and silly too)
- In software we deal with *unreasonable uncertainty* with unit tests and coverage 
- You can do the same for build systems
- The really tricky bits happen
  - When you need to build third-party software that uses other build system
  - Can't assume anything about the systen (a.k.a. Windows)


---
background-image: url(lemons.jpg)

---

# stuff from the Internet

- [io.js - What about gyp?](https://github.com/iojs/io.js/issues/133)
- [Imperative vs Declarative build systems](http://stackoverflow.com/questions/14955597/imperative-vs-declarative-build-systems)
- [CMake sucks, autoconf rules](https://plus.google.com/+LinasVepstas/posts/1FZG3Wx8XTt)
- [autoconf and portable programs: the joke](http://marc.info/?l=openbsd-ports&m=126805847322995&w=2)





