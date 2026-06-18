---
layout: default
title: Thoughts on Programming Languages
---

# Thoughts on Programming Languages

*Sep 03 2017*

Over my many years (actually decades now) of my time as a programmer, one of the favorite discussion among programmers
is on the relative merits of various programming languages. In this particular post, I thought I would write a
persistent record of my thoughts on the matter.

Before we start, let me begin with a list of languages in which I have written a piece of code of some significance. I
will give them in the order that I encountered them. I will exclude from this list languages that have a very particular
use, such as SQL or Bash, even if such languages have grown up to have capabilities outside their original mandate. The
languages are Basic, Fortran, Assembly, C,  Pascal, C++, Perl, Java, Javascript, Smalltalk, Groovy, Scala, and Python. I
have encountered Lisp in a text book setting and have only written Lisp in such a setting. Also, in my usage of
Javascript I did quite a bit of work in HTML and CSS, the natural domain for Javascript

## Static vs Dynamic

To start, I classify languages into two camps, dynamic and static. In a dynamic language, the compiler does little to
help you if you write a method call with the wrong parameters, or refer to an attribute of an object that does not exist
or is not of the expected type. Typically dynamic languages work well on relatively small projects with limited
complexity. They compile and launch quickly and usually focus on syntax that lends to quick and convenient
implementations of common solutions to a lot of standard programming problems. Perl, Javascript, Smalltalk, Lisp,
Groovy, and Python are in this camp. Although with Groovy, if you use a clever IDE (Integrated Development Environment)
and write straightforward code, the IDE can tell you if your method calls are correct and your references to attributes
are accurate. This is because Groovy is a convenience extension on top of Java and Java is a static language.

For me, the most painful thing that can happen in a dynamic language is when you have to refactor the code. For example,
if a method is to take a list of a strings, instead of a single string. Generally as large projects grow and mature,
they will go through multiple occurrences of refactoring. A refactoring can be quite complex with class definitions
being split up into multiple classes, entire function bodies reimplemented using a completely different approach. I am
far from alone in my view of dynamic languages and it is why static languages tend to dominate in large projects and why
there are extensions to Javascript, such as TypeScript, that layer a static language on top of a dynamic language.

In a static language you have a significant compile step before code can be launched and run. The static languages that
are in my list are Basic (though there are dynamic variants), C, C++, Fortran, and Scala. Static languages also usually
oblige the programmer to write more overhead for any code construct. Classes, methods, and attributes have to be more
fully declared and there are usually fewer convenience features to minimize the number of lines of code that need to be
written. For me, the thing that causes me the most pain in static languages is the thing that gives its virtue, the time
it takes to compile the code. Given two different static languages, I will tend to prefer the one with a faster compile
time, even if it means that programming in the language is more laborious. Two good examples of this are C vs C++ and
Java vs Scala. In C, it is quite difficult to write object oriented programming and in Java it is hard to write multi-
threaded programming that glues together or composes function blocks. In C++, if you limit your use of more complex
constructions, such as multi-inheritance of multi-parameterized types, the compile time expense is not that much
greater. As for Scala, I feel it to be an unusable language based solely on its compile time, though I have never seen
or written in a better language when judged solely on the merits of the syntax of a language.

But C++ and Scala come with more hidden costs. Both of them tend towards code that creates many temporary small object
allocations and deallocations from memory (the "heap" in Java). And many times, when analyzing the code, it can be
difficult to predict the run time speed of a particular block of code and there are many hidden traps. For example, the
basic collection type in Scala is the read-only linked list. If you prepend to the list, the cost is just the creation
of one new object and a couple of attribute assignments. However, if you append to the linked list, you have to clone
the entire list before the append operation can occur, which can rapidly get very expensive. Also, both C++ and Scala
allow for aggressive code obfuscation, making it difficult to quickly parse and comprehend code written by another
developer. For example, they can give new meanings to operator symbols, have invisible or hidden implicit parameters
and, in the case of Scala, seemingly shift the meaning and interpretation of method calls and operators so that they are
more like a rules engine than a procedural language. Or to put it another way, with the greater complexity of constructs
that both languages allow, it is much easier to write baffling code.

This leads to me to another belief, that I hold strongly though I rarely achieve this in practice, is that the less
clever you can make yourself appear when solving a problem, the better the code. When I was in mathematics, the best
proofs to a problem, and the ones that were a sign of true genius were elegant, but in retrospect blindingly obvious and
simple proofs. The type of proof that made you feel like an idiot for not thinking of it. As an example of this in
programming, I give the mark up language HTML. In its first incarnation, it was a ridiculously simple language and
compared to other markup languages (such as postscript or other SGML syntaxes) it was primitive and simplistic. It
looked like something any programmer with a bit of smarts could have invented. But in the end HTML won out mostly
because of its simplicity. Today, nobody would define page content using anything other than HTML. And in some cases,
HTML itself is considered too complex and wiki and markdown SGML languages have been developed as being even friendlier
and easier to use than HTML.

The problem with C++ and Scala is that they offer seductive ways to write clever code. It is easy to rationalize using
complex constructs and libraries to solve relatively simple programming problems. For example in Scala, a simple multi-
threaded application will be turned into a complicated messaging application using Akka. Such complex applications can
be difficult to troubleshoot when they fail and difficult to comprehend and analyze when trying to extend or alter
behavior. But such implementations keep appearing. Code of this type tends to give the programmer that wrote the code an
air of smarts and sophistication, a fatal lure. A lure I have felt strongly myself.

## Virtual Machine

The other defining difference between languages is whether they run on a virtual machine or not. A virtual machine can
prevent memory access violations from crashing your program and provide automatic garbage collection for objects
instantiated from the heap. Also, languages written on virtual machines usually have a greater ability to interrogate
their own runtime state. For example, if an exception is thrown, it is easy to get at the entire method call stack of
the code that caused the error without having to crash the program. Also, virtual machines make it much easier to find
and dynamically load new code into the runtime and make it available for programmatic usage, especially when compared to
the complexities of using dynamically loadable modules (DLLs on Windows, SOs on other operating systems) when not using
a virtual machine.

All the dynamic languages that I have mentioned are implemented on such virtual machines. The only static languages
(among the ones that I have mentioned) that run on such virtual machines are Java and Scala (which is written on top of
Java). The main competitor to Java is C# (and Google's Go is a recent competitor as well), but I have limited
familiarity with C#, so I will not say much about it. However, I believe C# to be as worthy or more worthy a language
than Java, but my recent programming career has been focused on linux deploys where C# is not a natural fit.

For me, programming without the safety belt of an underlying virtual machine is madness and should only be contemplated
if the requirements of your project demand it, for example if extracting every bit of performance out of the hardware is
vital. At the time Java first came out, I was so sold on its virtues that I immediately started doing all my programming
in the language only a few months after the very first version (Java 1.02) was available.

## Tool Chains and Libraries

However, there are other considerations when it comes to adopting a particular language. One of them is the number of
readily available libraries and the tool chains (such as IDEs) that are built on top of the language. Currently, I
believe Python and Java to be the clear winners here, even when compared to Microsoft languages. For example, if you
wish to programmatically control robots, you are most likely to find Python or Java to dominate the languages used for
the libraries that interface with the robots. Python has the additional win of being pre-installed on almost on all
machines not running Microsoft Windows and is becoming the programming language of choice for academic education in
programming.

But before declaring Java and Python winners, we should take a moment to look at Javascript. Javascript has one huge
advantage over the other languages that cannot be ignored. It is the natural language of web pages, and because of this
it also has a strong tool chain built on it and many libraries that can be readily adopted into any project. Also, a
large percentage of programmers are familiar and comfortable with Javascript and so naturally tend to desire to use it
for other projects besides presenting web pages. Javascript provides one other huge benefit. If you are writing a web
application and you write both the server and client in Javascript, you can write end to end unit tests that are
executed within a single virtual machine running the Javascript. This is because it is easy to to instantiate and
manipulate simulated browsers inside a Javascript program, even if the Javascript program runs entirely as a server side
solution. Such an advantage is not to be dismissed lightly.

So, in the end, I believe there are only four real competitors that should be given serious consideration for operating
system agnostic languages. They are Python, Javascript, Java, and Groovy. Groovy is only on the list because dynamic
languages have their place as facilitators, assemblers, and testers, even in large projects, and Groovy is highly
compatible with Java. It is why it is the underlying language for Gradle, a ubiquitous build tool, especially in Android
phone development. When Go or Kotlin (Kotlin is another language written on top of Java) reach a certain maturity, they
may also be given serious consideration as well.

## Java

Over time, Java has become so dominant that it is beginning to pick up a chorus of detractors. Its syntax, when compared
to a language like Scala, looks primitive and clunky and Java now has an aura of being an archaic language from a more
simple and ignorant era. But even today, Java has strengths not matched by most other languages written on a VM. For
example, it is so close to native machine instructions when manipulating arrays of characters, it is possible to build
new languages on top of Java that can be compiled and executed with reasonable efficiency. This is something you do not
see with Python and Javascript. In general, Java makes a real attempt to use programming constructs that have efficient
virtual machine implementations. This allows an experienced programmer to predict with some accuracy the overall speed
of execution of a block of code written in Java.

Java also is a winner against other static languages written on a virtual machine when you look at compile speed. Its
simpler syntax and restrictions on locations of files dictated by package membership make it possible to write compilers
that can compile a lot of code very quickly. But more than that, if you change code in a particular Java file, the
compiler can correctly figure out which other Java files need to be compiled because of the change. Neither Scala or
Kotlin do a good job on this, many times recompiling a lot of code even after fairly trivial changes, even if the
trivial changes appear to only change the interior logic of method and do not change the signatures of classes or
methods. As I have said before, compile speed is one of the primary measuring sticks I use when judging a language, so I
see this as a clear and decisive win for Java. TypeScript, the static language extension to Javascript, has similar
problems. A javascript language that takes only a few seconds to compile and launch can take minutes to compile when
written in TypeScript.

Another reason why Java has gotten a bad reputation is that some of the frameworks written for creating web applications
have been painful to use. For example, traditional application server frameworks implementing the full J2EE stack have
built up their own set of arcane constructs and have become so burdened with unnecessary complexity that even launching
a "hello world" application can take minutes. One of the wins that newer languages provide is that they get to start
with simpler and cleaner frameworks for developing applications, one of the main reasons why "node.js", a Javascript
server side implementation, has become so popular.

But this is a self-inflicted wound done to Java. It is possible to write simpler and more straightforward server side
applications in Java. I have done this multiple times and none of the applications I have worked on take more than 60
seconds to compile from scratch and take less than 30 seconds to start up, and some of these applications are fairly
sophisticated and complex with hundreds of thousands of lines of code. One of them had over a 100 man years of effort
put into it when it reached its most mature state.

So for those who propose a replacement for Java, your new language should have the following characteristics. It must be
static with the compiler doing a lot of the work in validating your code, something Scala does extraordinarily well. For
example, Scala eliminates code constructs that can cause null pointer exceptions, the bane of Java. The compiler cannot
add more than 50% to the overall time of compiling the code when compared to Java, and it must be similarly clever in
limiting the amount of compiling done when a few lines of code are changed. When writing code in the language, the
execution speed of the implementation should be at least somewhat predictable. To put it another way, the language
cannot be too conceptually removed from the underlying hardware that actually executes your program. The language can be
a bit more complex than Java, but not too much so and it should not cater to those who like to write super short method
names or use symbols as their method names. In this respect, I am quite impressed with Python which, more than any other
language I have encountered, makes it easy to write simple easily comprehensible solutions to simple problems. So,
assuming you can make the language static with quick compiles, a Python like language with its heavy emphasis on style,
with some of the elegant syntax from Scala (especially for declaring function types) is my best candidate for a language
that replaces Java.
