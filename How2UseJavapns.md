
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

<h1>Get your certificate</h1>

If you wanna use javapns on a PC, the first thing you have to do is exporting your certificate and you private key as a p12 file, using <a href='http://en.wikipedia.org/wiki/Keychain_%28Mac_OS%29'>KeyChain</a> on a MAC.<br>
<br>
To do this, just select both certificate and private key (associated to the application you wish to use to send notifications) in <a href='http://en.wikipedia.org/wiki/Keychain_%28Mac_OS%29'>KeyChain</a>, and the right click on one item, and select "Export 2 elements", give a name (for example : <i><b>myCertificate.p12</b></i>) and password (for example : <i><b>p@ssw0rd</b></i>) and then export as p12.<br>
<br>
Once you did this, you can now copy this p12 file on your PC (for example <i><b>C:/temp</b></i>).<br>
<br>
<br>
<h1>Working with javapns</h1>

<h2>Send a simple notification</h2>

Download javapns, and then add it to your java project classpath. To send notifications, you just need this :<br>
<pre><code><br>
PayLoad simplePayLoad = new PayLoad();<br>
simplePayLoad.addAlert("My alert message");<br>
simplePayLoad.addBadge(45);<br>
simplePayLoad.addSound("default");<br>
Device client = PushNotificationManager.getInstance().getDevice("my_iPhone");<br>
PushNotificationManager.getInstance().initializeConnection("gateway.sandbox.push.apple.com", 2195, "C:/temp/myCertificate.p12", "p@ssw0rd", SSLConnectionHelper.KEYSTORE_TYPE_PKCS12);<br>
PushNotificationManager.getInstance().sendNotification(client, simplePayLoad);<br>
</code></pre>

The created payload looks like<br>
<br>
<pre><code><br>
{"aps":{"sound":"default","alert":"My alert message","badge":45}}<br>
</code></pre>

And thats it, enjoy !<br>
<br>
<h2>Send a complex notification</h2>

<pre><code><br>
// Or create a complex PayLoad with a custom alert<br>
PayLoad complexPayLoad = new PayLoad();<br>
PayLoadCustomAlert customAlert = new PayLoadCustomAlert();<br>
// You can use addBody to add simple message, but we'll use<br>
// a more complex alert message so let's comment it<br>
// customAlert.addBody("My alert message");<br>
customAlert.addActionLocKey("Open App");<br>
customAlert.addLocKey("javapns rocks %@ %@%@");<br>
ArrayList parameters = new ArrayList();<br>
parameters.add("Test1");<br>
parameters.add("Test");<br>
parameters.add(2);<br>
customAlert.addLocArgs(parameters);<br>
complexPayLoad.addCustomAlert(customAlert);<br>
complexPayLoad.addBadge(45);<br>
complexPayLoad.addSound("default");<br>
complexPayLoad.addCustomDictionary("acme", "foo");<br>
complexPayLoad.addCustomDictionary("acme2", 42);<br>
ArrayList values = new ArrayList();<br>
values.add("value1");<br>
values.add(2);<br>
complexPayLoad.addCustomDictionary("acme3", values);<br>
Device client = PushNotificationManager.getInstance().getDevice("my_iPhone");<br>
PushNotificationManager.getInstance().initializeConnection("gateway.sandbox.push.apple.com", 2195, "C:/temp/myCertificate.p12", "p@ssw0rd", SSLConnectionHelper.KEYSTORE_TYPE_PKCS12);<br>
PushNotificationManager.getInstance().sendNotification(client, complexPayLoad);<br>
</code></pre>

The resulted payload will look like<br>
<br>
<pre><code><br>
{"aps":{"sound":"default","alert":{"loc-args":["Test1","Test",2],"action-loc-key":"Open App","loc-key":"javapns rocks %@ %@%@"},"badge":45},"acme3":["value1",2],"acme2":42,"acme":"foo"}<br>
</code></pre>
<img src='http://img38.imageshack.us/img38/9242/javapns.jpg' />