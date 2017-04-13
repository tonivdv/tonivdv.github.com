---
title: How to force symfony colors on windows with PuttyCyg?
author: tonivdv
layout: post
permalink: /2009/09/22/how-to-force-symfony-colors-on-windows-with-cygwinputtycyg/
dsq_thread_id:
  - 3828085670
categories:
  - Framework
  - General
  - php
  - Symfony
tags:
  - framework
  - php
  - Symfony
  - tips
---
Those of you who’re developing with symfony under windows will have noticed that, when running tasks in the command prompt, no colors are used. This is because the windows command prompt isn’t compatible with the color notation.  
Most of you also have [cygwin][1] installed (shame on you if you didn’t :p). But even if you run the tasks through “[PuttyCyg][2]”, which is fully compatible with the color notation, you will not benefit from the colors. 

Why?

<!--more-->

  
The problem resides in the symfony class “sfAnsiColorFormatter” in the method “supportsColors($stream)”:

<pre class="brush: php; title: ; notranslate" title="">/**
   * Returns true if the stream supports colorization.
   *
   * Colorization is disabled if not supported by the stream:
   *
   *  -  windows
   *  -  non tty consoles
   *
   * @param mixed $stream A stream
   *
   * @return Boolean true if the stream supports colorization, false otherwise
   */
  public function supportsColors($stream)
  {
    return DIRECTORY_SEPARATOR != '\\' && function_exists('posix_isatty') && @posix_isatty($stream);
  }
</pre>

The method supportsColors($stream) decides whether or not colors are supported. And as you can see, one of the checks is the directory separator which in our case will always return false because we are on a windows operating system. So even if you run tasks through puttycyg the directory separator will remain the same.

**Possibility 1**

If you always work through the puttycyg you could simply set the method to always return true, but in a command prompt it will look like this:  
<figure id="attachment_946" style="width: 300px;" class="wp-caption aligncenter">[<img src="http://www.devexp.eu/wp-content/uploads/2009/09/command_output-300x236.png" alt="Dos Command Output" title="Dos Command Output" width="300" height="236" class="size-medium wp-image-946" />][3]<figcaption class="wp-caption-text">Dos Command Output</figcaption></figure>

**Possibility 2**

Detect if cygwin is used. This way when you use the dos command the output will work and when cygwin is used you&#8217;ll have the colors :).

I checked the $\_SERVER array to find something that could help me to distinguish the cygwin prompt with the dos prompt, and I found that the &#8216;PWD&#8217; key is only available on *nix shells. And when I print the value of $\_SERVER[&#8216;PWD&#8217;] in the cygwin prompt it gives me &#8220;/cygdrive/f/sandbox/adlogix/branch-3.2/frontend&#8221;. 

Knowing that, here is how we can change the supportsColors method:

<pre class="brush: php; title: ; notranslate" title="">/**
   * Returns true if the stream supports colorization.
   *
   * Colorization is disabled if not supported by the stream:
   *
   *  -  windows
   *  -  non tty consoles
   *
   * @param mixed $stream A stream
   *
   * @return Boolean true if the stream supports colorization, false otherwise
   */
  public function supportsColors($stream)
  {
    $supported = DIRECTORY_SEPARATOR != '\\' && function_exists('posix_isatty') && @posix_isatty($stream);
    
    return $supported ? true : !is_bool(strpos(@$_SERVER['PWD'], "/cygdrive"));
  }
</pre>

What I did, is still using the check symfony used, but when it returns false I do a second check to see if the $_SERVER[&#8216;PWD&#8217;] exists and that it contains the String &#8220;/cygdrive&#8221;.

Now when I run the taks through cygwin I get the colors and when I run it in dos it displays correctly.  
<figure id="attachment_950" style="width: 300px;" class="wp-caption aligncenter">[<img src="http://www.devexp.eu/wp-content/uploads/2009/09/puttycyg-versus-dos-300x187.png" alt="PuttyCyg versus Dos" title="PuttyCyg versus Dos" width="300" height="187" class="size-medium wp-image-950" />][4]<figcaption class="wp-caption-text">PuttyCyg versus Dos</figcaption></figure>

I only tested it on my machine, so if you have troubles or a better way to do it, please let me know :).

**Update:** This is only tested with symfony 1.2 and as from sf 1.3 there will be an option &#8211;color to force the colors.

**Update:** For some reason it does not work in the cygwin bash shell. When I set the message manually (echo -e &#8220;\033[31mHello World\033[0m&#8221;) in the command the colors appear, but through symfony not. I suspect that the cygwin bash shell miss interprets the returns of php, but I have no idea why it does work on puttyCyg (which only launches the cygwin shell) &#8230; probably some startup configuration of the bash ?!

 [1]: http://www.cygwin.com/
 [2]: http://code.google.com/p/puttycyg/
 [3]: http://www.devexp.eu/wp-content/uploads/2009/09/command_output.png
 [4]: http://www.devexp.eu/wp-content/uploads/2009/09/puttycyg-versus-dos.png