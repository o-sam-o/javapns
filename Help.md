
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

<h1>Overview</h1>

Post a new Issue in the <code>Issues</code> tab,<br>
leave as much detail as possible (including code snippets!).<br>
<br>
As issues are solved, the wiki will be expanded.<br>
<br>
<h1>Common Errors</h1>
<pre><code>!DerInputStream.getLength(): lengthTag=127, too big.</code></pre>

Reason: This is most likely caused by the lack of having the Apple cert loaded in your keystore.<br>
<br>
Solution for version pre-1.6: Import keystore manually as per ImportAppleCert or upgrade to version 1.6.