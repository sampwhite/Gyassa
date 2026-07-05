---
layout: default
title: Kotlin, KMP, JSON Schema, SDUI, and Claude
---

# Kotlin, KMP, JSON Schema, SDUI, and Claude

*Jul 5, 2026*

## Overview

In [Thoughts on Programming Languages](thoughts-on-programming-languages.html) I laid out my view on what 
makes a good programming language and my survey of what was currently out there. I mentioned Kotlin but dismissed
it because of maturity. Well, it is nine years later, so I think a re-evaluation is in order. Though I did not mention
it at that time, JSON Schema was also an up-and-coming idea.  However, I must admit that I thought little of it
back then. To me, it was yet another likely failed attempt to formalize contracts between third parties.
I can still remember the days of CORBA Interface Definition Language (IDL) and XML Schema, each of which had their
moments of popularity. Both of which have essentially failed for what I believe to be good reasons.
In particular, the contracts for Dates and collections tended to be weak and imprecise. Also, they
had little support for conditional constructs. But JSON Schema has fixed all of these issues. And also recently,
JetBrains has introduced Kotlin Multiplatform (KMP). It is an attempt to write a Kotlin-based solution for
everything, starting with the frontend part of an application. Finally, we have recently seen the birth of really
usable AI with Claude Opus 4.8, and later iterations show special promise. I wish to address all of these together.

First, Kotlin has matured into a really usable language without the religions that infect languages like Scala.
Its focus is on being practical, and it has almost the same interoperability with Java that Groovy does.
For example, it does not default to using read-only custom versions of core collection classes like Scala.
Kotlin also has a nice configuration builder syntax, giving you the opportunity to create statically compilable
config files matching capabilities on Groovy and Scala. And a few years ago JetBrains released an improved
version of the Kotlin compiler which is good enough to only marginally fall behind Java. That means an
IntelliJ project with thousands or more source code files is practical. I admit that at this point Kotlin
is not just my favorite language, but it is so by a reasonable margin when compared to all other languages.

One of the main perks of JavaScript or TypeScript is that they can be used in both a server side and client
side implementation. It allows you to share code across the two layers. JetBrains introduced KMP a few years
ago to directly address this issue, allowing a user to use the same Gradle project to build both backend and
frontend and to transpile Kotlin code into TypeScript classes with JavaScript implementations. There is a
promise that this might go further. For example, most infrastructure environments consist of a lot of shell
scripts and small bits of executable code written in Python or Go. Might it not also be possible to write
this type of infrastructure code in Kotlin? Lately it looks like there is an attempt to do exactly that.

JSON Schema in its 2017 draft made an incredible jump forward. It now fully supports conditional constructs, 
and with the 2020-12 draft it now supports a syntax that robustly supports modern approaches to schema. Along
with this, there is a growing initiative to have the server entirely control the presentation of the frontend.
This approach is called Server-Driven UI (SDUI), and it grows naturally along with the new richness of
JSON schema. JSON schema can not only define the contracts between frontend and backend, it can give hints to how
the frontend should render when displaying content or editing content.

And of course, the seismic recent change is AIs that can now code better than humans. To be more specific,
in my recent experience with Claude Opus 4.8, I believe it writes code better than humans for the following reasons. 
First, its naming is better with the names likely to suggest the memes in the code better than the names I invent. Here
the advantage of being crowdsourced is really clear. Second, it tends to find simpler, less awkward ways to
do implementations that do recursion. For example, it did a good job in writing an elegant JSON schema
parser and validator from scratch using fewer lines of code than I would have written. Again, code like this
is constantly being written, so having a large memory of similar implementations gives it a decisive advantage.
Third, it does a better job of writing unit tests that give good coverage. Here its ability to not get bored
is its major advantage. I should mention I do not believe Claude Sonnet beats humans as decisively, and I will only 
use Claude Opus 4.8 and probably Claude Fable sometime soon. Since AI is so quickly moving, this
particular paragraph is likely to be out of date fairly soon.

## Proposal

My proposal is to ride this growing wave of Kotlin, KMP, JSON Schema, SDUI, and Claude. In particular, I wish to 
rewrite the [DynamicRuntime](dynamic-runtime-project.html) code base using all these new ideas. The DynamicRuntime 
project defined its own schema, so in the proposed new version of the code we would replace that with JSON schema. 
And of course we would write all the code in Kotlin. And Claude will write most of the code with humans only 
contributing outlines or small pieces of utility code. To see the initial creation, go to 
[https://github.com/sampwhite/KotlinDynamicRuntime](https://github.com/sampwhite/KotlinDynamicRuntime). In what 
follows I make additional comments about elements of this proposal.

So one of the latent promises in a schema definition syntax such as JSON schema, is that you can extend its 
use beyond endpoint and data contracts. In particular, you can extend it with additional metadata to 
inform a frontend how to present and create editors for data. And one of the core differentiation principles in JSON 
schema is that you can define your own keys, and JSON Schema even supports a way to give documentation 
on custom keys. So you can address concerns such as layout placements, styles, usage of dialog popups, and on and on. 
You can also define extensions that define table style presentations with rules for cell presentation vs. 
the view of a popup editor. If you combine this with a JSON data mechanism to define layouts including 
references to Markdown text for descriptions or extended text, you can essentially write a full description 
of the frontend.

However, from this proposal there is one large missing piece, and that is a full custom Abstract Syntax Tree (AST)
interpreter that allows the backend to inject dynamic code behaviors from the backend into the frontend. 
This would mimic the old style JavaScript sections in HTML pages. This area does not look that mature,  
and so I want to write a completely new syntax specific to this problem domain that has a Kotlin bias. 
We would write a parser in Kotlin and get to use it in the backend as well as the frontend. And we would
like to somehow get IntelliJ to validate the code, especially its references to declared constants, 
object attributes, and methods.

There are some things I would like this new script language to do. First, it can do “if conditionals” and 
iterate on collections. Second, it can refer to attributes of Kotlin objects that were transpiled to 
JavaScript/TypeScript. Third, it can help guide elements of frontend presentation such as formatting, enable/disabled 
states of buttons, placement of error warnings, and so on. I admit this is a big unknown for me as to 
where this might actually go and will probably be driven by whatever current needs the frontend has. 
And there will likely be a constant tension between augmenting understandings of new options defined 
in the JSON rendering data vs. trying to code behaviors using a custom code syntax.

In the most mature version of this "following the wave" proposal, a fully simulated version of a frontend rendering would 
be written in the backend allowing the backend to write unit tests against a subset of frontend behavior. 
For example, it could verify that the correct endpoint was called when the frontend loaded a table or 
that the correct sort order was applied. Of course, it would not be able to verify that the display 
made any sense to a user, but the backend could still validate many of the logical behaviors. One of 
the issues with full integration test engines such as Playwright is that they are quite slow since 
they are doing full frontend browser rendering. Also, since they are executing in both frontend and backend, 
it is challenging to use a debugger to check up on behavior.

I do want to make one comment about this. I am fairly certain that I would not be able to implement 
this project without the assistance of AI. For example, my days of writing completely new custom languages 
are likely behind me, but AIs seem quite good at this.
