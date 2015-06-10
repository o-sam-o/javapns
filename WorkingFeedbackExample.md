
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

<h1>Introduction</h1>

Below is a handy test to verify that everything is working.<br>
<br>
<i><b>Note:</b> feedback service seems to only work on the production url.</i>

<i><b>Note:</b> Requires Java 1.6</i>

Compile with the following command:<br>
<pre><code>javac -cp "/path/to/required/jars/*" Feedback.java </code></pre>

Then run with the following command:<br>
<pre><code>java6 -cp "/path/to/class/file/:/path/to/required/jars/*" Feedback</code></pre>

<i><b>Note:</b> List of Required Jars is on the <a href='Requirements.md'>Requirements</a> page</i>

For ssl debugging output, run the command:<br>
<pre><code>java -Djavax.net.debug=all -cp "/path/to/class/file/:/path/to/required/jars/*" Feedback</code></pre>

<h1>Java file</h1>

<pre><code><br>
import java.util.!LinkedList;<br>
import java.util.!ListIterator;<br>
<br>
import javapns.back.!FeedbackServiceManager;<br>
import javapns.back.!SSLConnectionHelper;<br>
import javapns.data.Device;<br>
<br>
public class Feedback {<br>
// APNs Server Host &amp; port<br>
private static final String HOST = "feedback.push.apple.com";<br>
private static final int PORT = 2196;<br>
<br>
private static String certificate = "/absolute/path/to/certificate";<br>
private static String passwd = "certificatePassword";<br>
<br>
public static void main( String[] args ) throws Exception {<br>
<br>
try {<br>
// Get !FeedbackServiceManager Instance<br>
!FeedbackServiceManager feedbackManager = !FeedbackServiceManager.getInstance();<br>
<br>
// Initialize connection<br>
!LinkedList&lt;Device&gt; devices = feedbackManager.getDevices( HOST, PORT, certificate, passwd, SSLConnectionHelper.KEYSTORE_TYPE_PKCS12 );<br>
System.out.println( "Connection initialized..." );<br>
<br>
System.out.println( "Devices returned: " + devices.size() );<br>
<br>
!ListIterator&lt;Device&gt; itr = devices.listIterator();<br>
while ( itr.hasNext() ) {<br>
Device device = itr.next();<br>
System.out.println( "Device: id=[" + device.getId() + " token=[" + device.getToken() + "]" );<br>
}<br>
<br>
System.out.println( "done" );<br>
<br>
} catch (Exception e) {<br>
e.printStackTrace();<br>
}<br>
}<br>
}</code></pre>

<h1>Output</h1>
<pre><code><br>
Connection initialized...<br>
Devices returned: 0<br>
done</code></pre>