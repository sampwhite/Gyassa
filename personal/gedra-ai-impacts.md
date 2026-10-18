---
layout: default
title: Gedra, and Why AI Makes Open Foundations the Product
---

# Gedra, and Why AI Makes Open Foundations the Product

*Oct 18, 2026*

Recently I have been working on the new Kotlin version of the old DynamicRuntime open-source project. As readers of
earlier posts ([Dynamic Runtime Schema](dynamic-runtime-schema.html) and
[Dynamic Runtime Project](dynamic-runtime-project.html)) may know, DynamicRuntime is a Java application, created by
Gyassa Software, that tried to inject schema, runtime variance, and self-description into an application. For the new
Kotlin version, we decided to focus on forms and workflows, which raised the question of what to call the data we stored
in the application. We had seen this question come up before with Jira. Jira is an application much like ours: it
handles form data entry with workflows. We noticed that everybody we knew tended to call the issues they filed "Jiras",
and we wanted something similar. In a conversation with Claude Opus, we asked for ideas for a name of our own, one we
would use internally when naming source code files and as the name of the main objects stored in the application. Claude
noted that our company's name, Gyassa Software, starts with the letter "G", and that the abbreviation for
"KotlinDynamicRuntime" is "KDR". We experimented with joining these with vowels and saying the combinations aloud, and
the one we liked best was "Gedra". That was how Gedra was born, and it became the official name of every stored record
(forms, users, workflow data, and files) in the application.

When working on the new Kotlin version, we chose to have various Claude models (most recently Opus 5.5 and Fable 5.1) do
the work, with us planning features, reviewing the code, and doing QA. Like many others, we were astounded by the
capabilities of current AIs. It became clear that AIs were changing how people viewed software. Software no longer had
to be something where you gathered a group to define requirements, outlined implementation goals, and developed in
two-week sprints. One person, by themselves, could create a disposable application in a few days that did the job and
then move on. It seemed to me that the era of large monolithic software applications that charged expensive licensing
fees was coming to an end, at least for applications built for the particular needs of a working group inside a company.

But there was a problem with this new approach. To explain it, let us look at what people call "one-shot"
implementations: solutions implemented by AI, with no shared software foundation, from a few simple prompts and
minimal revisions by the original developer. Though they were impressive, they usually skipped many aspects of what
makes a product mature. Here are some examples, focusing on web applications meant to be deployed into some type of
compute cloud, such as AWS.

* The one-shots were usually lightweight on security. They did not start from a framework that had been actively
  tested for security issues.
* They did not address the problems of importing or exporting data. In particular, they did not come with APIs with
  well-defined schemas.
* There was minimal infrastructure for history and logging, so any bugs that arose were hard to analyze.
* There were no mechanisms to discover and deal with bad data.
* There was no meta-application where a future AI could use configuration and plugins to extend behavior without
  drastically revising the original application.
* They had no plan for scaling up. In particular, they tended to have suboptimal database schemas and query patterns,
  and no internal infrastructure for caching.
* They were not friendly to the problems of deployment and deployment management. For example, they might not provide
  a health endpoint that could be used to see whether a particular node was functioning correctly.
* They did not have nightly jobs to groom data, create reports, and sync with other applications.
* User provisioning and permission granting tended to be simplistic and inflexible.

Developers would encounter these problems and then spend much time with their AIs solving each one. None of these
problems had anything to do with the features they wanted to implement; they were the overhead required for a
stable, deployable application.

So we asked ourselves: what if this did not have to be the case? What if we could provide a starting base for
developing a type of application common in companies? One of the most common types is form submission and workflow
processing, and we have been working on just such applications for the last few decades. So maybe we could create a
foundation for a typical application for forms and combine it with all the ideas we used when creating the original
DynamicRuntime application. If you read [my prior post](kotlin-dynamic-runtime-project.html), as
[revised by Claude Opus](kdr-project-ai.html), you will see a description of some of the design ideas we wanted to
bring to the new application.

KotlinDynamicRuntime addresses many of the issues listed above. Most of the features described do not have a
well-defined scope for completeness, so we will likely still be working on them for some time. The idea is that anything
we add is done once and not each time you create a new application. As an example, once we start yearly penetration
tests on the core code base, all applications built on this code base will share in the trust it provides. We already
have a well-defined meta-application model, a core idea inherited from the original DynamicRuntime application. And we
have a general mechanism for table caching, and nightly jobs that calculate workflow states and reports. We have
endpoints to inspect health and application state and have written functionality focused on multi-node deployments. As
for the rest, we are actively working on them, so expect their status to change.

The idea of producing a closed-source solution, the typical approach in the past, is no longer viable. I expect that
as companies get in the habit of creating their own solutions using AI, they will no longer be content unless they
own the entire solution. They will see no reason to be limited by licensing restrictions.

In KotlinDynamicRuntime, we are trying to provide a complete and self-contained implementation and minimize our use of
third-party libraries where practical: a web server, a database driver, and React for the browser. This gives any AI
working with the code base a complete reference set that it can code against and cover with unit tests. We also write
code to replace some common libraries because doing so makes it easier to inject custom functionality in a direct and
straightforward fashion. If you do not like something about a particular third-party library, the answer is no longer
"deal with it as best you can." Now you can have an AI write just the code you need. Need a JSON parser? Have the AI
write one. Want to use Markdown to help build your frontend UI? Have the AI build you a Markdown renderer. We have
noticed one other thing: AIs seem particularly good at writing code to replace well-known library functionality used in
many projects by others.

So I see our work as the future of software development. Developers will create self-contained code bases that
others can use as a launching point for their own custom solutions, and these solutions will be free. The most
common way to make money will be getting hired for short-lived projects, writing custom-fit solutions based on
expertise with one of these self-contained code bases. For us, that code base is KotlinDynamicRuntime and Gedra.

If you want to see KotlinDynamicRuntime and Gedra for yourself, there are a few places to start. The reference
instance at [dynamicruntime.org](https://dynamicruntime.org) runs the software live, with sample clients and a
conference demo, and you can sign in and look around (its data is cleared from time to time). The
[conference demo walkthrough](https://github.com/sampwhite/KotlinDynamicRuntime/tree/main/docs/demo-client) explains
what the demo shows, workflow by workflow. The source code and its documentation are on
[GitHub](https://github.com/sampwhite/KotlinDynamicRuntime) under the MIT license, and the commit history shows how
the application was built, issue by issue.
