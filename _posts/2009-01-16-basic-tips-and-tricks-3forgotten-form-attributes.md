---
title: 'basic tips and tricks: 3.Forgotten form attributes'
author: kennethvr
layout: post
permalink: /2009/01/16/basic-tips-and-tricks-3forgotten-form-attributes/
syntaxhighlighter_encoded:
  - 1
categories:
  - css
  - html
  - Tips and Tricks
tags:
  - attributes
  - elements
  - form
---
A lot of people create easy forms but don’t think it trough before designing the page. I listed 5 useful attributes/elements a lot of people intend to forget or simply don’t know why they are using it.

Use **get or post** to send a form

Get will append the form data to the URL that is set in the action attribute of the form and is used when the form is <a title="idempotent" href="http://en.wikipedia.org/wiki/Idempotent" target="_blank">idempotent</a>. For example on search forms.  
Post will not append it to the URL but sent it in the body of the form. It should be used when we are modifying a database or doing a subscription.

What is **enctype**?

This attribute is used to submit the form to the server; it is only needed when using post. Whenever you use a file field in your form you should set the enctype value to multipart/form-data, otherwise it uses the default value application/x-www-form-urlencoded.

Use **tabindex**

This creates an order when going from one field to the next using your tab button. When creating complex forms, this might come in quite handy. It creates the opportunity for the developer to indicate the order the form needs to be completed. Every field in the form gets a ‘tabindex’ value starting from 0 with a maximum of 32767.

Connect a **label to a field**

It’s always handy to have a label for each input field, so you can click on the label and the cursor automatically gets positioned in the input field. The only thing to do is put a ‘for’ attribute in the label and give it the id of the corresponding field as value. Easy and Handy for users

**Fieldset and legend**

This allows you to thematically group your form elements. The ‘legend’ element gives you the opportunity to create a title on each fieldset and has to be put in between the fieldset tags. Use css styling to create a nice design for the fieldset.

Greetings K.