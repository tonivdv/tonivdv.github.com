---
title: Read from Excel in ASP
author: van Rumste Kenneth
layout: post
permalink: /2009/10/14/read-from-excel-in-asp/
syntaxhighlighter_encoded:
  - 1
dsq_thread_id:
  - 3828085688
categories:
  - ASP
  - General
tags:
  - ASP
  - excel
  - read
  - vb
---
I always had a hard time finding correct documentation on the old ASP (active server pages) language and I needed it one more time in the last few days to create a script that read from an excel file.

As I believe that it might be handy for a handful of people around the world (is there actually anybody else still developing in ASP these days?), I hereby share my little piece of code with you guys.

Any suggestions or comments are always welcome.

In my example we read from an excel file that has 1 small table that has 1000 lines and columns from A until G.  
The first row contains all the column names.

<pre class="brush: vb; title: ; notranslate" title="">'initialize variables
Dim objConn, strSQL
Dim x

Set objConn = Server.CreateObject("ADODB.Connection")
objConn.Open "DRIVER={Microsoft Excel Driver (*.xls)}; IMEX=1; HDR=NO; "&_
 "Excel 8.0; DBQ=" & Server.MapPath("filename.xls") & "; "
strSQL = "SELECT * FROM A1:G1000"

Response.Write("&lt;table border=""1""&gt;")
Response.Write("&lt;tr&gt;")
'write all columnNames
For x=0 To objRS.Fields.Count-1
 Response.Write("&lt;th&gt;" & objRS.Fields(x).Name & "&lt;/th&gt;")
Next
Response.Write("&lt;/tr&gt;")

Do Until objRS.EOF

' write as much columns as there are in your excel file
Response.Write("&lt;td&gt;" & objRS.Fields(0).Value & "&lt;/td&gt;")
Response.Write("&lt;td&gt;" & objRS.Fields(1).Value & "&lt;/td&gt;")
Response.Write("&lt;td&gt;" & objRS.Fields(2).Value & "&lt;/td&gt;")
Response.Write("&lt;td&gt;" & objRS.Fields(3).Value & "&lt;/td&gt;")
Response.Write("&lt;td&gt;" & objRS.Fields(4).Value & "&lt;/td&gt;")
Response.Write("&lt;td&gt;" & objRS.Fields(5).Value & "&lt;/td&gt;")
Response.Write("&lt;td&gt;" & objRS.Fields(6).Value & "&lt;/td&gt;")

objRS.Close

Response.Write("&lt;/table&gt;")

Set objRS=Nothing

</pre>