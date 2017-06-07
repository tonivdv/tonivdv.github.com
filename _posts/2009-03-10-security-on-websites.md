---
title: Security on websites
author: kennethvr
layout: post
deprecated: true
permalink: /2009/03/10/security-on-websites/
categories:
  - General
  - Javascript
  - Jquery
  - Languages
  - php
tags:
  - .access
  - encryption
  - robot.txt
  - security
---
Sometimes it’s interesting to protect your web content from the world, and there are different ways (or a combination) to do this:

**Create a robot.txt file**

This file is uploaded into the root folder of a website and makes sure that specific folders shouldn’t be crawled and indexed by search engines. Most of the time this is for images, administration section, documents, etc…

Create the file using notepad and type this:

> User-agent: *  
> Disallow: /images/  
> Disallow: /cgi-bin/  
> Disallow: /documents/  
> Disallow: /administration/

This means that for all search bots (\*) these 4 folders will not be crawled. It‘s possible to set these folders for each individual search robot by replacing the \* by the name of the robot (ex. Googlebot, MSNbot, …)

**Encryption**

When using a password, be sure to use encryption.  
I guess it could be interesting to write a whole article on this issue, because it’s a bit complex, and I’ll try to do that in the next few days. But if this topic is interesting for you be sure to take a look at MD5

  * Info: <a href="http://en.wikipedia.org/wiki/MD5" target="_blank">http://en.wikipedia.org/wiki/MD5</a>
  * PHP: <a href="http://be2.php.net/md5" target="_blank">http://be2.php.net/md5</a>
  * ASP: <a href="http://www.freevbcode.com/ShowCode.Asp?ID=2366" target="_blank">http://www.freevbcode.com/ShowCode.Asp?ID=2366</a>

**.htaccess**

An image or document often gets a direct link, created by users and placed in their favorites. That isn’t a big problem when we are talking about an unimportant document or a small image. But when dozens of websites start linking to this document, it’s possible to reach the maximum of your bandwidth in no time. And it certainly may not happen when we are talking about confidential documents.  
To restrict these documents from the outer world, we can use the .htaccess file.  
This file affects only the directory where it has been placed in and all its subdirectories.

> So if you have a directory  
> /security  
> and 2 folders in it  
> /security/docs  
> /security/images

In this case, put the .access file in the /security if you want all the directories to be secured, or only in the docs folder if this is the only one you want to be secure.  
A good hint is to try to use the fewest number of .access file because redundancies can cause infinite loops.  
Preventing hot linking is the most important event the .access file will prevent. Instead of getting the file they are hot linking they can get another image like an angry man or a message that this document is not available.  
This is how an .htaccess file is build up:

> RewriteEngine on  
> RewriteCond %{HTTP_REFERER} !^$  
> RewriteCond %{HTTP_REFERER} !^http://(www\.)?mydomain.com/.*$ [NC]  
> RewriteRule \.(gif|jpg)$ &#8211; [F]

The first two rules have to be kept as they are now; the third rule however needs to be adjusted so it indicates your specific URL.  
The fourth line is set up to deny the use of types you like to be denied, you can add as many types as you like using the pipe (|) separator.  
To show another file when they want to download a .gif or .jpg, change the fourth line to something like this:

> RewriteRule \.(gif|jpg)$ http://www.mydomain.com/angryman.gif [R,L]

These adjustments should implement some security in to your website. Greets.