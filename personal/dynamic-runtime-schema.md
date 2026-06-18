---
layout: default
title: Dynamic Runtime Schema
---

# Dynamic Runtime Schema

*Nov 14 2018*

**Prelude**

Before I launch into a discussion of the this topic, I wish to talk about a couple of frustrations I have with trying to
find prior art. If you google the combination of words *dynamic*, *runtime*, and *schema* you do not find discussions
about the general topic of dynamically building schema definitions during the runtime of an application. Instead you
find odds and ends where somebody may be adding a few custom metadata fields or doing something with schema that just
happens to be dynamic and the word *runtime* just happens to show up in the content. Other words I have tried are words
such as *builder*, *definition*, and *designer* with likewise little profit. I have targeted code repositories such as
*github* and the most I have found is graphql like solutions. I have a similar problem with *data-driven*. To me an
example of data-driven is when you take a graphics solution that has classes Point, Line, Triangle, Rectangle, Square,
etc. and you turn all the classes into a single class called Polygon with a simple list of points and constraint
criteria on the points. But with big data becoming such a popular subject, data-driven has become to mean the processing
of data without reference to implementation details.

Also when you search for the term schema you also get ambiguity. For example in many of my searches, schema seems to be
equated with configuration or XML document definitions. It does not have anything to do with web application artifacts
such as endpoints or database tables. We take a moment to define what we mean by the word *schema*. In our case, schema
is the structural definitions of all aspects of your application. For endpoints, this includes the endpoint path, the
security rules being applied, the allowable inputs, and the expected outputs. It also includes any additional tags,
classification, and descriptions. For database tables this not only includes a definition of the database table, it also
includes the definition of the primary key, indexes, foreign key relationships, friendly field labels, and descriptions.

**Overview**

If you have read the prior articles in this blog on runtime data differencing, one of the obvious themes is that code
should be turned into data whenever it is reasonable to do so. When you take this approach to doing coding and apply it
to schema, you find yourself at odds with popular practice. Popular practice is to define schema at compile time. You
use reflection to dig through the definition of classes and pull in all the annotations. This is static schema where the
entire design of endpoints or tables are defined directly as hardwired artifacts in the code itself. Currently the most
dominant version of this approach for Java is Spring Boot and Hibernate. The approach is popular because it has a quick
on-ramp to getting a working solution and it, at least initially, can eliminate a lot of boilerplate code. I should say
that there are many projects where using compile time schema makes a lot of sense. However, I wish to focus on projects
where such an approach does **not** make sense, since from my google searching, there is little discussion on this
point.

**Why Runtime Schema**

One of the motivating examples for using runtime (or dynamic) schema is when you want to allow a client to design
aspects of the application using a browser. This can include creating (or augmenting) database tables, defining indexes,
and writing simple scripts for logic execution. A classic example of this is a general purpose tool for designing simple
form applications. The general theme is that underlying engine that provides this functionality has no compile time
understanding of the database tables, or at least part of the database tables.

However, there are other reasons for why you might want runtime schema. For example, many database applications follow
common design patterns and you might want to separate the definition of those patterns from the other parts of your
schema definition. For example, database tables tend to have a counter for a primary key and they have dates for
tracking when the row was created and last updated. Other common patterns can show up. There may be columns to identify
the consumer who owns the data, a transaction identifier to identify the last transaction that acted on the data, error
report fields, state transition fields, and so on. And lastly (and sometimes critically), there may be a column whose
data is designed to shard the database table. All of these columns may then have standard indexes applied to them and
there may be predictable query patterns as well. I call such fields that are part of the general common table design
patterns, **protocol** fields. They are the price you pay in order to store the data you actually care about. In an
application that uses runtime schema builders, the definitions of tables would be a core set of data fields, some custom
indexes, and a list of references to the common design patterns. This definition would not have direct references to any
of the protocol fields. The code would instead pull in the protocol fields from the referenced common design patterns.
Also the code would build some preliminary queries focused on the protocol fields and supply the general logic for how
the protocol fields are implemented.

A similar argument can be made about endpoints. For example, there are commonly fields in the response data that
describe how long the request took and how many rows were returned. Many of the endpoints may apply identical security
rules. The endpoints may follow similar patterns for paging through result sets. There also maybe common approaches to
error handling. So just like database tables, endpoint schema have their own *protocol* fields with exactly the same
types of issues with abstracting the handling of the general endpoint logic from the specifics of a particular endpoint.

**Consequences and Perks**

If you go down this path, one thing immediately happens. You cannot define Java classes (for the remainder of this
discussion we assume the language is Java, but a similar argument can be made for any language) that reference all the
fields in the database table. The classes can only define fields that are relevant to that class. In many cases, this
means the objects must hold a reference to a general Map of data holding all the row data with only partial extraction
into actual declared fields. I call this pattern *light-weight* data contracts. The Java classes only know a portion of
the rules being applied to the data, they do not have knowledge of all the possible fields or values in the data. The
classes that participate in the data flow logic only alter portions of the data as it flows through code. Different
areas of the code focus on different aspects of the data with different areas of the code isolated logically from each
other. Also, the Java classes may reference various fields by aliases and not by the actual names used in the database.
This allows the code to apply common patterns to the data by pulling in data through indirect references.

There are some other perks of defining schema in data separately from the code. It is easier to write archiving or
migration logic, especially if you put rules for doing archiving and migration into the metadata of the schema. You can
also publish the schema as a *dictionary* and write automated inspectors that validate the correctness and completeness
of the schema. The schema can also help in automated tests where random data generation is required. The schema would
define what would be reasonable random data.

**How to do Schema**

So suppose you accept the idea that you have to define your schema compactly in data. The next question is how best to
do this. One approach is to define the schema in data files, such as *JSON* files. This can be useful if you wish a
client to publish new changes to the schema. But there is an alternate approach that gives you some of the advantages of
compiled schema but still allows for runtime schema definition. This approach uses builders written in Java (or whatever
the primary language is for your application). The field names and all other literal strings used in the schema
definitions are defined as static string constants in appropriate Java classes. Functions are used to optimally build
schemas eliminating as much redundancy as possible. The advantage of using static declared literal strings is that when
you write code to extract the fields from data, you can use the same constants as lookup keys. There is one other
advantage of using code. You can use Java Document links between implementation code and schema. One of the
disadvantages turning schema into pure data is that a programmer viewing the code may not easily find the schema driving
the behavior of the code. When both the functional behaviors and the schema are defined in code, then it is possible to
put in easy to follow links between the code to data and the links can be clickable links in an IDE such as IntelliJ.

**Expanding Schema Scope**

There is already some movement towards a more dynamic view of schema. The most recent example is *graphql*. However,
graphql has a very limited view of its schema model. There are many things missing that would be useful to have it
schema. In fact, many schema solutions do not consider that there is a lot of associated information that can be bound
to schema definitions.

There are many examples of such data. The schema could define restricted choice lists for some of its fields, labels for
the fields, descriptions of various parts of the schema, bindings to internationalization keys, constraint rules, and so
on. But this only the start. Schema can be a seed on which you accrete other aspects of your application definition. For
example, if you allow for extended metadata on the types and fields in your schema, you can use the metadata for
specifying how the fields or types should be handled by the code, allowing the code to be more "data-driven". For
example, there might be a metadata field that says the field holds the author of the document and because it is the
author field, if the acting user is the author, then the user has greater privileges to edit fields in a document. Some
date fields may have metadata that describes the rules for how they are populated. Generally the metadata would start
getting defined once common patterns were discovered in the code and the code was abstracted to run against data inputs
with the data inputs being put into the metadata. As you discover more opportunities for abstraction and capturing
common code patterns, more and more data could get attached to your schema.

**Implementation Issues**

Ok, suppose you are sold on the arguments above. What is actually involved in defining runtime schema? The answer is
that you have to use runtime code and data to define the types of constructs you would normally define in compiled code.
For example, enums would turn into lists of declared string constants. Fields would be objects with a field name, the
type that defines it, whether it is required, the constraints to apply, descriptions, and any other relevant metadata.
But this can rapidly get trickier. For example, what if you wish to support parameterized types or generics? This would
mean the type of the field would be a generic that would not be defined until a parent type gave the field a type
definition. Another possibility is creating unions of types where if one type redundantly defines a field, the
definition that is earliest in the list of types being joined would win (or some type of merging rule would be applied).
And if you allow generic types and unions of types, then it is possible that you might not give a generic type a
definition in an endpoint. Instead the definition of the type would be in the data itself. This would allow for true
dynamic types. The definition in this case could be a composition on already defined types. An example might be
"List&lt;Items|Prices&gt;" with the pipe symbol indicating a union of the types Items and Prices. This string would
occur in the JSON data and it would be used to dynamically determine the type to apply to the data.

**When Dynamic Schema**

Now, one objection that might be made to runtime schema is that creating application solutions this way is harder and
takes more time upfront before you get a working application. That objection has validity, but if the code is created by
multiple teams of developers over many years, gets refactored many times, and changes dramatically to fit evolving
requirements, the advantages of using runtime schema can be huge. It is when the application becomes a large complex but
still growing and evolving construct that the power of data driven abstractions can really be felt. The different
abstractions can be tested separately and changes to design usually occur in fewer places and with far fewer side-
effects. Also, over time such applications start being much smaller in code size than the traditional sprawling
*compile-only* implementations.
