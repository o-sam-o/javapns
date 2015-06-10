
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

<pre><code> // Setup up a simple message<br>
!PayLoad aPayload = new !PayLoad();<br>
aPayload.addBadge( 1 );<br>
<br>
// Get !PushNotification Instance<br>
!PushNotificationManager pushManager = !PushNotificationManager.getInstance();<br>
<br>
// Link iPhone's UDID (64-char device token) to a stringName<br>
pushManager.addDevice("iPhone", iPhoneId );<br>
<br>
// Get iPhone client<br>
Device client = pushManager.getDevice( "iPhone" );<br>
<br>
// Initialize connection<br>
pushManager.initializeConnection( host, port, certificate, passwd, SSLConnectionHelper.KEYSTORE_TYPE_PKCS12 );<br>
<br>
// Send message<br>
pushManager.sendNotification( client, aPayload );<br>
<br>
// close connection<br>
pushManager.stopConnection();<br>
pushManager.removeDevice( "iPhone" );</code></pre>

The created payload looks like:<br>
<br>
<pre><code>{"aps":{"badge":1}}</code></pre>