---
layout: default
title: Application Configuration
---

# Application Configuration

*Jun 23 2017*

This post (though some time separates this post from the previous) follows a similar theme from prior posts. This time I
want to give my thoughts on the general problem of configuration. And when I say configuration, I am not talking about
simple things like database connections, load balancers, and deployments. Nor am I talking about the type of
configuration usually found in config files consisting of simple name-value pairs providing simple tweaks to the
behavior of a running application. Instead, I am talking about the type of configuration that can be found in tables or
large complex documents.

One mistake, and I believe it to be a mistake, is that in many cases this configuration is supplied through endpoints
and a custom administrative interface is created to call these endpoints. Let me give an example. Suppose your software
has some large accounts which we will call clients. For each client, the client can supply goods to sell, prices of
those goods, rules for allowing discounts, and rules for mechanisms of delivery. You can also imagine that they might
also supply rich content around those goods, such as images, descriptions, and technical specifications. These clients
sell their goods to buyers for corporations. An example of such an application would be a standard B2B (business to
business) commerce website.

A traditional approach to providing this functionality is for the solution provider to create administrative endpoints,
accessible by the client, where the client can provide the content and rules. However, problems begin to arise. For
example, if the client wishes to test their configuration on staging servers before they try it out on the production
server, simple administrative endpoints may make this awkward. Also, if you already have ten or more clients who have
solutions that are similar to the one the client wishes to create, it is difficult for the client to take advantage of
the prior art of other clients. And in many cases, you demoed a sophisticated example implementation which the client
would like to use as a basis for their own implementation. And if you imagine that the client configuration includes
scripts for tweaking behavior, such as algorithms for determining when to offer discounts based on prior behavior of
purchase, the configuration can get quite tricky. Also, if the configuration evolves over time and the client wishes to
do historical analysis of functionality or if the client wishes to have clean change over from one configuration to
another timed at midnight of a particular day, this can get quite tricky using simple endpoints. Administrative
endpoints, as a general rule, do not easily lend themselves to implementing version control in their APIs.

Another approach, and the one I advocate, is to use a bundle of configuration files which have name value pairs, tables,
and script containing all the desired configuration for a client. If the configuration files include mechanisms for
importing other shared file resources or overlaying on other configuration, it is quite possible to create a fairly
compact and flexible implementation. The files can be version controlled and pre-existing art can easily be migrated
into the new configuration files. And with a bit of cleverness, common implementations can be turned into templates and
the templates can be imported (by reference, not by making copy) into a new configuration and then overlaid by
elements of the new configuration.

The usual criticism of this approach is that there is no natural path to a GUI that allows an admin level user to
implement their desired configuration in a particular website. However, with some thought this issue can be resolved. An
administrative interface can be made to build prototype content for configuration bundles and edit the simpler contents
of these files. These files can then be deployed using an admin endpoint to deploy config bundles. However, this approach
has limitations. Usually the administrative interface can only do relatively simple things and more complex
configurations require hand editing of the files. This may appear like a limitation specific to this approach, however
this overlooks the fact that the original simple endpoints approach had similar issues. The truth was that
administrators were handcrafting JSON to call against the admin endpoints and bypassing the admin GUI anyway.

If your configuration consists of config bundles then the contents of those bundles can easily be versioned controlled
by the normal source code control process and can be propagated through staging servers much like changes to source code
is propagated. In fact, that is the essential point. Even though the client is doing the work, they are essentially
creating source code. You may call it configuration, but many times the havoc wreaked by mistakes in this so-called
configuration can be as great or greater than one would get from bugs in source code, especially since configuration
tends not to go through the same unit testing and QA process that source code is forced to go through.

So the truth is that you are essentially letting your clients write source code for your system, and you should only
allow them to do that if you can version control it for them in your source code repositories. I suspect that when you
upload a mobile app to Apple and ask them to approve it that the first thing they do is throw the interior elements of
the app into a version control system so they can make it go through a standard QA approval process. They can also use
such a storage system to look for similarities with apps they know to have caused problems in the past.

However, one issue comes up immediately when you start using deployable configuration bundles, and this is the focus of
this article. When you use endpoints, you can uniquely assign an ID, either a counter or a GUID, to each configuration
element. For example, a widget could be uniquely identified by a column in a database table holding an auto-incrementing
primary key. But when you do the configuration outside the domain of a particular deployment, all aspects of the
configuration must necessarily have **external** unique global keys to uniquely identify the elements in the
configuration. These unique keys must be unique across various deployments and will generally take on a persistent life
of their own. And there can easily be thousands or tens of thousands of such keys for truly large scale client
configurations. For example, if the widget has mechanical joints and the joints are of a particular type and make, you
may need many unique configuration keys to describe all the possibilities.

To recap, you have external configuration files holding large amounts of text content with potentially thousands of
unique keys. How do you deal with this situation? I argue that you create a globally available **identifier-key**
server. This server would hold unique string identifiers, called keys and attached to those keys would be various
metadata. The metadata could be labels, descriptions, associated choice lists whose contents also refer to other keys,
or other useful metadata of all types, and the field names for that metadata would also be keys in your identifier-key
server. In order to create a new external unique string in a configuration file, you would have to register it in the
identifier-key server first and also verify that another such string could not be used in its place. This has the
advantage that when two different clients configure essentially the same widget with the same joints, they would use the
same unique keys in naming the widget and those joints. They would also use the same unique keys in the various choice
lists of types or kinds of widgets or joints.

If a client were to create a new configuration, they would be advised to see if they can find existing unique keys in
the identifier-key server. If they cannot find them, then should first create a configuration bundle with just their
proposed new unique string identifiers and that bundle would have to be validated before the full configuration bundle
could be considered for deployment.

At first, this may seem like a bit of an encumbrance, but if you set up a bit of admin GUI, you can speed the process
along. New unique identifiers are relatively easy to QA and validate and once you have done so, a lot of good things can
come from this. One of the biggest is that it can greatly ease the problem of transferring configurations from one
environment to another. Since every web instance has access to the contents of the identifier-key server, all of them
have a basic underlying understanding of any configuration bundle that might come their way. It can greatly reduce the
broken reference problems that tend to plague complex configurations. It can also greatly ease the process of reuse of
common configuration templates.

However, other good things can occur. With the identifier-key server, you have a good set of search terms to determine
accurately what client is using a particular type of widget with a particular set of joints. You can determine when the
keys were first used, who is actively using them, how often they are used, and all other types of useful data mining
capabilities. In fact, this idea is not that new. It is generally called providing a **data dictionary** for your
application and data dictionaries can be powerful and useful constructs to force everybody to use the same language to
talk about the same things. However, most applications when they create global data dictionaries do not go anywhere far
enough with the idea. A data dictionary gets its true power when it penetrates into every unique key construct, no
matter how internal or obscure.
