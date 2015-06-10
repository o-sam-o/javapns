
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
Compile with the following command:<br>
<pre><code>javac -cp "/path/to/required/jars/*" Push.java </code></pre>

Then run with the following command:<br>
<pre><code>java -cp "/path/to/class/file/:/path/to/required/jars/*" Push</code></pre>

<i><b>Note:</b> List of Required Jars is on the <a href='Requirements.md'>Requirements</a> page</i>

For ssl debugging output, run the command:<br>
<pre><code>java -Djavax.net.debug=all -cp "/path/to/class/file/:/path/to/required/jars/*" Push</code></pre>

<h1>Java file</h1>

<pre><code><br>
import javapns.back.!PushNotificationManager;<br>
import javapns.back.SSLConnectionHelper;<br>
import javapns.data.Device;<br>
import javapns.data.!PayLoad;<br>
<br>
public class Push {<br>
// APNs Server Host &amp; port<br>
private static final String HOST = "gateway.sandbox.push.apple.com";<br>
private static final int PORT = 2195;<br>
<br>
// Badge<br>
private static final int BADGE = 66;<br>
<br>
// iPhone's UDID (64-char device token)<br>
private static String iPhoneId = "2ed202ac08ea9...cf8d55910df290567037dcc4";<br>
private static String certificate = "/absolute/path/to/my/certificate";<br>
private static String passwd = "mycertificatepassword";<br>
<br>
public static void main( String[] args ) throws Exception {<br>
<br>
System.out.println( "Setting up Push notification" );<br>
<br>
try {<br>
// Setup up a simple message<br>
!PayLoad aPayload = new !PayLoad();<br>
aPayload.addBadge( BADGE );<br>
System.out.println( "Payload setup successfull." );<br>
<br>
System.out.println ( aPayload );<br>
<br>
// Get !PushNotification Instance<br>
!PushNotificationManager pushManager = !PushNotificationManager.getInstance();<br>
<br>
// Link iPhone's UDID (64-char device token) to a stringName<br>
pushManager.addDevice("iPhone", iPhoneId);<br>
System.out.println( "iPhone UDID taken." );<br>
<br>
System.out.println( "Token: " + pushManager.getDevice( "iPhone" ).getToken() );<br>
<br>
// Get iPhone client<br>
Device client = pushManager.getDevice( "iPhone" );<br>
System.out.println( "Client setup successfull." );<br>
<br>
// Initialize connection<br>
pushManager.initializeConnection( HOST, PORT, certificate, passwd, SSLConnectionHelper.KEYSTORE_TYPE_PKCS12);<br>
System.out.println( "Connection initialized..." );<br>
<br>
// Send message<br>
pushManager.sendNotification( client, aPayload );<br>
System.out.println( "Message sent!" );<br>
<br>
System.out.println( "# of attempts: " + pushManager.getRetryAttempts() );<br>
pushManager.stopConnection();<br>
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
Setting up Push notification<br>
Payload setup successfull.<br>
{"aps":{"badge":66}}<br>
iPhone UDID taken.<br>
Token: 2ed202ac08ea9033665e853a3dc8bc4c5e78f7a6cf8d55910df230567037dcc4<br>
Client setup successfull.<br>
Connection initialized...<br>
Message sent!<br>
# of attempts: 3<br>
done</code></pre>