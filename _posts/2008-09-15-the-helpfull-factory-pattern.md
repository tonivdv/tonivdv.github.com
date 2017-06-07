---
title: The helpful design patterns
author: kennethvr
deprecated: true
layout: post
permalink: /2008/09/15/the-helpfull-factory-pattern/
categories:
  - php
tags:
  - constructor
  - factory
  - oop
  - pattern
  - php
---
The use of patterns in code being a good enhancement isn&#8217;t big news. But I discovered them recently and am very positive about it. It can, when understood, hugely increase your readability and is a nice problem solver for complicated and extendible coding.

Before creating a design pattern, you have to create it on paper. Think about what pattern you can use before implementing one. There are different patterns, every one of them has specific needs and specifications.

For example:

* Observer Pattern  
* Decorator Pattern  
* Factory Pattern  
* Command Pattern  
* etc&#8230;

In our case we needed to build a good nice 5-step wizard that is perfect to use simple pattern structure.

For us, it made the code very easy to understand and realy clean. We created one controller that gets the correct action when initialized , and it initializes the correct class (step) by itself.

<pre class="brush: php; title: ; notranslate" title="">$controller = new StepController();
$controller -&gt;getInstance($action);</pre>

stepcontroller.class.php

public function getInstance($currentAction = null) {

<pre class="brush: php; title: ; notranslate" title="">$stepAction = null;

switch($currentAction) {
case "step1" :
$stepAction = new step1Action();
break;
case "step2" :
$stepAction = new step2Action();
break
}

return $stepAction-&gt;execute();
}</pre>

All the action classes have a execute function on their own and implement an interface that contains the execute function.

By using this kind of pattern it is very easy for us to add or remove steps in our little wizard. Every step has its own action and is really easy to maintain.  
As you can see (and this is only one little example) the patterns have great advantages, the only challenge is to know what pattern to be used best in your specific situation.

Have fun with it.