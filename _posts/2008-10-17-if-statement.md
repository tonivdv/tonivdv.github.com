---
title: if statement
author: kennethvr
layout: post
deprecated: true
permalink: /2008/10/17/if-statement/
categories:
  - General
---
<p class="MsoNormal">
  <span lang="EN-US">I always seem to forget how the short if statement is working in PHP so here it is for a future reference.</span>
</p>

<p class="MsoNormal">
  <span lang="EN-US">Normally you should write it like this in code </span>
</p>

<p class="MsoNormal">
  <pre class="brush: php; title: ; notranslate" title="">if(a == b){ echo “a”; }else{echo “b”;}</pre>
</p>

<p class="MsoNormal">
  <span lang="EN-US">There is also a possibility to write this much faster like so:</span>
</p>

<p class="MsoNormal">
  <pre class="brush: php; title: ; notranslate" title="">(a == b) ? echo “a” : echo “b”; </pre>
</p>

<p class="MsoNormal">
  <span lang="EN-US">Isn’t that a lot easier. </span>
</p>

<p class="MsoNormal">
  <span lang="EN-US">To be completely correct don’t use {} in the template files. You might rather use something like this:</span>
</p>

<p class="MsoNormal">
  <pre class="brush: php; title: ; notranslate" title="">if(a==b):
echo “a”;
else:
echo “b”;
endif;</pre>
  
  <p class="MsoNormal">
    <span lang="EN-US">Same for foreach where you will use foreach(): and endforeach;</span>
  </p>