---
title: How to use a symfony 1 layout from a different directory
author: tonivdv
layout: post
deprecated: true
permalink: /2011/10/28/how-to-use-a-symfony-1-layout-from-a-different-directory/
shareaholic_disable_share_buttons:
  - 0
shareaholic_disable_open_graph_tags:
  - 0
dsq_thread_id:
  - 3828085699
categories:
  - General
  - Layout
  - php
  - Tips and Tricks
tags:
  - Layout
  - php
  - plugin
  - Symfony
---
[<img src="http://www.devexp.eu/wp-content/uploads/2009/04/symfony.jpg" alt="" title="Symfony" width="127" height="35" class="alignright size-full wp-image-727" />][1]In symfony 1 it is possible to have different layouts for an application. But they all have to be put into the directory &#8216;myproject/apps/frontend/templates/&#8217;. But what if you want to use a layout from another location? 

Assume you make a plugin with a specific layout, it would be nice to load the layout from the plugin directory, and not to have to copy the file to the global directory.

Here is how you can achieve this:

<pre class="brush: php; title: ; notranslate" title="">$template = $this-&gt;getContext()-&gt;getConfiguration()-&gt;getTemplateDir('MODULE', 'LAYOUT_FILE.php');
$this-&gt;setLayout($template . '/LAYOUT_FILE');
</pre>

Let&#8217;s say you have the following:

<pre class="brush: plain; title: ; notranslate" title="">my_project/
  plugins/
    my_plugin/
      modules/
        MyUser/
          actions/
            actions.php
          templates/
            indexSuccess.php
            MyUserLayout.php
</pre>

The actions.php class could be something like this:

<pre class="brush: php; title: ; notranslate" title="">class MyUserAction extends sfActions {

  public function preExecute() {
    $template = $this-&gt;getContext()-&gt;getConfiguration()-&gt;getTemplateDir('MyUser', 'MyUserLayout.php');

    $this-&gt;setLayout($template . '/MyUserLayout');
  }

  public function executeIndex() {

  }
}
</pre>

Have fun!

 [1]: http://www.devexp.eu/wp-content/uploads/2009/04/symfony.jpg