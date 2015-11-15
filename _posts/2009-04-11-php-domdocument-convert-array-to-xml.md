---
title: 'PHP DOMDocument: Convert Array to Xml'
author: Van de Voorde Toni
layout: post
permalink: /2009/04/11/php-domdocument-convert-array-to-xml/
syntaxhighlighter_encoded:
  - 1
dsq_thread_id:
  - 3828085609
categories:
  - General
  - php
tags:
  - DOMDocument
  - php
  - xml
---
<img src="http://www.devexp.eu/wp-content/uploads/2009/04/elephant-elephant-php-logo1.png" alt="php" title="php" width="146" height="110" class="alignright size-full wp-image-611" />Recently I start working on an export module to invoice applications. One of them uses a very simple xml structure (simple nodes without attributes etc.) and therefore I wanted to create this xml also in a very simple way: from an array.

I decided the use DOM, and not e.g. SimpleXML, because I need to validate my xml with a schema.

After a quick search on the internet I found several snippets that could do the trick until I had to set the same element tag name on the same level:

<pre class="brush: xml; title: ; notranslate" title="">&lt;nodes&gt;
	&lt;node&gt;text&lt;/node&gt;
	&lt;node&gt;text&lt;/node&gt;
&lt;/nodes&gt;
</pre>

The snippets I found use the index of an array to create the name of the element and the value for the text part. Only everyone knows that you cannot set 2 keys with the same name in an array. 

So what could we do to improve this ?

<!--more-->

  
I changed a snippet so it can generate the example above. The array should now look like this:

<pre class="brush: php; title: ; notranslate" title="">array (
	“nodes” =&gt; array (
		“node” =&gt; array (
			0 =&gt; “text”
			1 =&gt; “text”
		)
	)
)
</pre>

Since you cannot set integers as element tags, it checks if the index is an integer and if it is it loops the array to recreate the same element tag name with the new value.

I added the function in a class which extends the DOMDocument class ([XmlDomConstruct][1]):

<pre class="brush: php; title: ; notranslate" title="">&lt;?php
/**
 * Extends the DOMDocument to implement personal (utility) methods.
 *
 * @author Toni Van de Voorde
 */
class XmlDomConstruct extends DOMDocument {

	/**
	 * Constructs elements and texts from an array or string.
	 * The array can contain an element's name in the index part
	 * and an element's text in the value part.
	 * 
	 * It can also creates an xml with the same element tagName on the same 
	 * level.
	 * 
	 * ex:
	 * &lt;nodes&gt;
	 *   &lt;node&gt;text&lt;/node&gt;
	 *   &lt;node&gt;
	 *     &lt;field&gt;hello&lt;/field&gt;
	 *     &lt;field&gt;world&lt;/field&gt;
	 *   &lt;/node&gt;
	 * &lt;/nodes&gt;
	 * 
	 * Array should then look like:
	 * 
	 * Array (
	 *   "nodes" =&gt; Array (
	 *     "node" =&gt; Array (
	 *       0 =&gt; "text"
	 *       1 =&gt; Array (
	 *         "field" =&gt; Array (
	 *           0 =&gt; "hello"
	 *           1 =&gt; "world"
	 *         )
	 *       )
	 *     )
	 *   )
	 * )
	 *
	 * @param mixed $mixed An array or string.
	 * 
	 * @param DOMElement[optional] $domElement Then element
	 * from where the array will be construct to.
	 * 
	 */
	public function fromMixed($mixed, DOMElement $domElement = null) {

		$domElement = is_null($domElement) ? $this : $domElement;

		if (is_array($mixed)) {
			foreach( $mixed as $index =&gt; $mixedElement ) {

				if ( is_int($index) ) {
					if ( $index == 0 ) {
						$node = $domElement;
					} else {
						$node = $this-&gt;createElement($domElement-&gt;tagName);
						$domElement-&gt;parentNode-&gt;appendChild($node);
					}
				} 
				
				else {
					$node = $this-&gt;createElement($index);
					$domElement-&gt;appendChild($node);
				}
				
				$this-&gt;fromMixed($mixedElement, $node);
				
			}
		} else {
			$domElement-&gt;appendChild($this-&gt;createTextNode($mixed));
		}
		
	}
	
}
?&gt;
</pre>

And here is a very simple example on how you can use this class:

<pre class="brush: php; title: ; notranslate" title="">$array = array(
  "nodes" =&gt; array(
    "node" =&gt; array(
      0 =&gt; "text",
      1 =&gt; "text"
)));

$this-&gt;dom = new XmlDomConstruct('1.0', 'utf-8');
$this-&gt;dom-&gt;fromMixed($array);

echo $this-&gt;dom-&gt;saveXML();
</pre>

Isn&#8217;t that wonderful? You don&#8217;t even have to know how to construct xmls with the DOMDocument object. Of course this will only work for simple xmls.

Maybe, if I have the time, I’ll try to add the possibility to insert attributes to elements.

 [1]: http://www.devexp.eu/wp-content/uploads/2009/04/xmldomconstructclassphp.gz