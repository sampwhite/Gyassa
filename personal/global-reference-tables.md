---
layout: default
title: Global Reference Tables
---

# Global Reference Tables

*Mar 19 2012*

This article is about whether certain types of data should be in version control systems and treated as source
code or whether they should be in database tables and treated as data. Let us say you are writing a web
application which will allow users to input recipes for food. As part of your web application you decide that you
need a list of the different types of spices that can be put on food. This table and its related tables would not
only list the spices, but the degree of hotness, a little snippet of the history of the spice, the plant that it
comes from, methods of extraction from the plant, general popularity of the spice, the typical cost of the spice,
the regional areas that favor the spice, the amount normally used for spicing a standard size dish, and tips for
usage in creating dishes. The entries in this table and its companion children tables would be referenced by
recipes, by users reporting about the usage of spices in message boards, functionality in the web site that
determine the hotness of the dish based on the amount of various spices in the dish, recommendations of spices for
different types of dishes, lists of spices for different regions in the world and so on. In other words, the table
and its associated children tables are an example of a Global Reference Table. I use the singular when referencing
a global reference table because the children tables are in some sense owned or subsumed by the root table.

The problem is this. Where is this global reference table stored? The developers writing the application don't know
much about spices and do not believe that have the expertise to create an authoritative list of spices, so they
know they need input from experts who have knowledge on spices. The first solution is to put the spice list into
database tables and create a user interface to allow administrators or trusted users (the so-called experts) to add
new spice entries and all the associated information to a spice. The second is to hardwire the choices into tables
in a source code file (preferably RDD formatted tables).

The natural urge is to go with the first approach and put the data into database tables because it allows the
developers to punt on the general problem of coming up with the list. I believe this is wrong. As counter-intuitive
as it might sound, I believe it is better to hardwire the data into source code controlled text files.

Source code control systems (such as 'git') give you more than you might first think. For example, suppose you
wanted to add some new spices to support Vietnamese specialties which you were going to feature on your food web
site with a new Vietnamese recipe designer mobile app. You would like the new spices and the new functionality for
Vietnamese specialties to go out at the same time on your web site. If both were in source code, it would happen
naturally as part of your deployment process. Source code versioning systems give you natural large scale
synchronized distributed transactions on the behavior of your web site. In sophisticated software development
environments, the whole build and deployment process leverages this feature of a source code control system to
create targeted builds and deployments that control precisely what you want to have appear in your application.

On the other hand, if the spices were in a database that could be changed by administrators and privileges users,
then you would lose the ability to have simple synchronized deployment of new features. Also, if you have demo,
test, development, and staging versions of your application which used their own databases, every new spice that
had associated code functionality would need to have controlled propagation to all these environments. For example,
both the Component Evolution and the Staging to Production problems that create such pain for most mature
applications would require quite complex additions to your application to solve. In addition, you could get into
serious trouble if some of the administrators and/or trusted users added new spices that anticipated the changes
coming from the development group. In that case, you would have colliding definitions and difficulties resolving
conflicts. That is another thing version control systems do very well – resolving conflicts from multiple
developers.

Suppose you buy the argument that the spice tables should go into a version controlled text file that is bundled
with the deployed application. What if you want to let administrators or trusted users to add new spices or modify
existing entries? My answer is to turn these contributors into developers. Not all version controlled development
activity needs to be done using a text editor or involve writing logical behavior. This is well understood for the
artists who create the images and styling of your web pages. I believe a similar approach should be used for
controlling the content of your global reference tables.

There is no reason why a web interface that would normally target a database table cannot have proposed
modifications captured into a version controlled 'diff file'. You compare the table as it is in source code and the
proposed new table as desired by a contributor and capture the difference, but unlike with regular file
differencing, you capture differences at the table cell level and not by comparing lines of text. When a user saves
their changes, they are saved into a 'diff' file and committed to a source code control system automatically by the
web server that displays the page. Using the RDD model, the 'diff files' would be runtime overlays of the existing
global reference data. Generally, a group of administrators or trusted users would have a common diff file that they
would be editing so that you would not have a proliferation of such files. In some cases, if you wanted to isolate
certain changes from the main branch of changes, you could have some administrators and trusted users creating a
separate new diff file that was overlaid on top of both the base source file and all the other active diff files.
If the contributors wanted to have a coordination of propagating the changes in multiple different global reference
tables, they could put diff files being applied to different tables into a single component.

If after a certain period of time, the changes by the administrators and trusted users gained wide spread
acceptance, the core group of developers could roll the diff files into the main source file for the reference
data, much in the same way they would merge in the changes from a offshoot branch of their main development tree.
If a bundle of diffs happened to be captured into a single component, it would make the merging process simpler and
easier to understand and control.

In a sophisticated version of this solution, the creators of these 'diff' files could view and analyze the history
of the changes in the data much as they would in a Wiki site where a user can view the history and changes in the
content of a Wiki page. In theory, they would even be free to write suggestions for proposed changes or write
comments about the current table content. A very sophisticated solution might let the users edit the tables using
an Excel spreadsheet that they can download and upload as they desire just as it is common for users to copy the
contents of their Word documents into the edit areas of a Wiki page when editing online page content. Admittedly it
would be a major effort to create this functionality, because the files themselves would still be simple text files
under the control of standard developer oriented version control system such as 'git'. Mapping this functionality
to a Wiki page metaphor would require some real labor.
