# SQLMap: The Basics
Learn about SQL injection and exploit this vulnerability through the SQLMap tool.

## Content:
<br>Task 1: Introduction
<br>Task 2: SQL Injection Vulnerability
<br>Task 3: Automated SQL Injection Tool
<br>Task 4: Practical Exercise
<br>[To note](#to-note)

## To note
<ul>
<li>[GET parameter] http://sqlmaptesting.thm/search?cat=1<br>URL uses GET parameters to retrieve data because parameters are in URL.</li>
<li>For POST: Data is hidden in the request body and URL looks clean.</li>
<li>[path/ route aprameter] http://sqlmaptesting.thm/search/cat=1: <code>cat=1</code> is part of the URL path and isn't the query parameter. Both URLs are still HTTP GET requests. Former is more common in old/ classic apps, while latter is for modern frameworks.</li>
</ul>