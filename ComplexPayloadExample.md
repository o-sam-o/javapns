
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>


<pre><code>// Or create a complex !PayLoad with a custom alert<br>
!PayLoad complexPayLoad = new !PayLoad();<br>
!PayLoadCustomAlert customAlert = new !PayLoadCustomAlert();<br>
// You can use addBody to add simple message, but we'll use<br>
// a more complex alert message so let's comment it<br>
// customAlert.addBody("My alert message");<br>
customAlert.addActionLocKey("Open App");<br>
customAlert.addLocKey("javapns rocks %@ %@%@");<br>
!ArrayList parameters = !new ArrayList();<br>
parameters.add("Test1");<br>
parameters.add("Test");<br>
parameters.add(2);<br>
customAlert.addLocArgs(parameters);<br>
complexPayLoad.addCustomAlert(customAlert);<br>
complexPayLoad.addBadge(45);<br>
complexPayLoad.addSound("default");<br>
complexPayLoad.addCustomDictionary("acme", "foo");<br>
complexPayLoad.addCustomDictionary("acme2", 42);<br>
!ArrayList values = new !ArrayList();<br>
values.add("value1");<br>
values.add(2);<br>
complexPayLoad.addCustomDictionary("acme3", values);<br>
Device client = !PushNotificationManager.getInstance().getDevice("my_iPhone");<br>
!PushNotificationManager.getInstance().initializeConnection("gateway.sandbox.push.apple.com", 2195, "C:/temp/myCertificate.p12", "p@ssw0rd", SSLConnectionHelper.KEYSTORE_TYPE_PKCS12);<br>
!PushNotificationManager.getInstance().sendNotification(client, complexPayLoad);</code></pre>

The resulted payload will look like<br>
<br>
<pre><code>{"aps":{"sound":"default","alert":{"loc-args":["Test1","Test",2],"action-loc-key":"Open App","loc-key":"javapns rocks %@ %@%@"},"badge":45},"acme3":["value1",2],"acme2":42,"acme":"foo"}</code></pre>

<img src='http://img38.imageshack.us/img38/9242/javapns.jpg' />