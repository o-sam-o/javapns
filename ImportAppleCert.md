
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>


<h1>Overview</h1>
To get the keystore working... (In Version 1.6 and later.. .this is NOT REQUIRED!)<br>
<br>
<i><b>Note:</b> This may need tweaking to work properly...</i>

<h2>Instructions</h2>

Download IntallCert:<br>
<br>
<a href='http://blogs.sun.com/andreas/resource/InstallCert.java'>http://blogs.sun.com/andreas/resource/InstallCert.java</a>

Compile it:<br>
<pre><code>javac !InstallCert.java</code></pre>

Run it:<br>
<pre><code>java !InstallCert gateway.sandbox.push.apple.com:2195</code></pre>

then... when asked to select something...enter:<br>
<pre><code>1</code></pre>

Rename it:<br>
<pre><code>mv jssecacerts apple.keystore</code></pre>

change the password:<br>
<pre><code>keytool -storepasswd -keystore apple.keystore -new 'your_cert_passwd' -storepasswd changeit</code></pre>

Put the apple.keystore where the java code is expecting it.<br>
(see your java error logs)