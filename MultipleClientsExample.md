
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

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
private static String certificate = "/absolute/path/to/my/certificate";<br>
private static String passwd = "mycertificatepassword";<br>
<br>
public static void main( List&lt;User&gt; ) throws Exception {<br>
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
// Loop thru our users and add the tokens to the pushManager<br>
// User is an object, that returns 'username' and 'token' both as Strings<br>
while ( User user ) {<br>
<br>
pushManager.addDevice( user.getUsername(), user.getToken() );<br>
System.out.println( "iPhone UDID taken." );<br>
<br>
System.out.println( "Token: " + pushManager.getDevice( user.getUsername() ).getToken() );<br>
}<br>
<br>
// Initialize connection<br>
pushManager.initializeConnection( HOST, PORT, certificate, passwd, SSLConnectionHelper.KEYSTORE_TYPE_PKCS12);<br>
System.out.println( "Connection initialized..." );<br>
<br>
// Send messages<br>
while( User user ) {<br>
<br>
System.out.println( "Token: " + pushManager.getDevice( user.getUsername() ).getToken() );<br>
<br>
Device client = pushManager.getDevice( user.getUsername() );<br>
pushManager.sendNotification( client, aPayload );<br>
System.out.println( "Message sent!" );<br>
}<br>
System.out.println( "All Messages sent!" );<br>
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