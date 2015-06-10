
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

<h2>General suggestions</h2>

Create a standalone java app that tests your configuration.<br>
<br>
This allows testing the certificate and sending to a known iPhoneID.<br>
<br>
See WorkingExample<br>
<br>
See WorkingFeedbackExample<br>
<br>
<h2>Internal logging</h2>

More logging can be enabled by adding the following to your log4j.properties:<br>
<pre><code> ### log package activity<br>
log4j.logger.com.javapns=debug</code></pre>

<h2>SSL Certificate issues</h2>

Enable SSL logging in java by adding the following to your java VM arguments:<br>
<pre><code>-Djavax.net.debug=all</code></pre>