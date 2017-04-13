---
title: Mantis in Eclipse with Mylyn
author: kennethvr
layout: post
permalink: /2009/05/20/mantis-in-eclipse-with-mylyn/
syntaxhighlighter_encoded:
  - 1
dsq_thread_id:
  - 3828085656
categories:
  - General
  - Languages
  - php
tags:
  - eclipse
  - mantis
  - mylyn
  - php
  - websvn
---
This is a post especially for those who use Mantis as a bug tracker software and Eclipse as development software. For our PHP and JAVA development we do all the programming in Eclipse and wouldn&#8217;t it be easy if Eclipse recognized the bugs we are working? Wouldn&#8217;t it be easy if we could just get an automatic link between mantis and the commits we do in subversion (svn).

Well, praise the lord, &#8217;cause there is an open source project out there that does all these things called <a href="http://www.eclipse.org/mylyn/" target="_blank">Mylyn</a>.

> Mylyn is a task-focused interface for Eclipse that reduces information overload and makes multi-tasking easy. It does this by making tasks a first class part of Eclipse, and integrating rich and offline editing for repositories such as Bugzilla, Trac, and JIRA. Once your tasks are integrated, Mylyn monitors your work activity to identify relevant information, and uses this task context to focus the user interface on the task-at-hand. This puts the information you need at your fingertips and improves productivity by reducing searching, scrolling, and navigation. By making task context explicit Mylyn also facilitates multitasking, planning, reusing past efforts, and sharing expertise. 

<img class="alignright wp-image-786" title="tasklist-splash-31" src="http://www.devexp.eu/wp-content/uploads/2009/05/tasklist-splash-31.png" alt="tasklist-splash-31" width="282" height="489" />  
After a quick install and doing the configuration in Eclipse the interface is installed in Eclipse and ready to work with. And it is great! Easy overviews of all the bugs sorted by filter as you personally prefer, great layout to create and manage the bugs, etc.

It communicates quite nice with mantis, it could be a bit faster but let&#8217;s not fall over that tiny thing. The cool thing though is when working for a specific bug, all changes will be committed on those files with a reference to that specific bug. This makes it very easy to get see what files where changed for a specific bug. 

For example when you commit changed files you will be asked to comment your committed files. Mylyn links your files directly to the current bug you are working on and puts this in the comment area: &#8220;fixed issue #178: Change user name to full name&#8221; . The number of the issue and the state are first mentioned and the title completes your comment.

In the screenshot you can see how a bug can be inserted or edited in a easy layout and inside the eclipse environment. Click on the image or <a href="http://www.devexp.eu/wp-content/uploads/2009/05/mylyn-31-screenshot.png" target="_blank">here</a> to see a larger image  
<a href="http://www.devexp.eu/wp-content/uploads/2009/05/mylyn-31-screenshot.png" target="_blank"><img class="size-full wp-image-785" title="mylyn-31-screenshot" src="http://www.devexp.eu/wp-content/uploads/2009/05/mylyn-31-screenshot.png" alt="mylyn-31-screenshot" width="550" border="0" /></a>

My colleague Toni also installed <a href="http://www.websvn.info/" target="_blank">WebSVN </a>that gives us a super overview on all those changes and files committed in subversion.

If you use Mantis and Eclipse and SVN I highly recommend that you use these two utilities.