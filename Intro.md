
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>


JavaPNS is a Java Library used to send <a href='http://developer.apple.com/iphone/library/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CommunicatingWIthAPS/CommunicatingWIthAPS.html'>Apple Notification Service</a> (APNS) messages to iPhone Apps<br>
<br>
This Java library provides a way to convert your notification to the “raw” (binary) data expected by the APNS and send using an asynchronous streaming socket over TLS (or SSL).<br>
<br>
<h2>Index</h2>
<ul><li><a href='Requirements.md'>Requirements</a>
</li><li><a href='GetAPNSCertificate.md'>GetAPNSCertificate</a></li></ul>

<ul><li><a href='Tips.md'>Tips</a>
</li><li><a href='Help.md'>Help</a></li></ul>

You may establish multiple, parallel connections to the same gateway or to multiple gateway instances. But it is suggested that you retain your connection if sending many notifications. Although there is no hard limit to the number of gateway connections a provider can make, you should limit them to no more than 15. (APNs may consider connections that are rapidly and repeatedly established and torn down as a denial-of-service attack.)<br>
<br>
<h2>Sending Push Notifications</h2>
<ul><li><a href='BasicExample.md'>BasicExample</a>
</li><li><a href='WorkingExample.md'>WorkingExample</a>
</li><li><a href='ComplexPayloadExample.md'>ComplexPayloadExample</a>
</li><li><a href='Proxy.md'>Proxy</a></li></ul>

You should regularly connect with the feedback service and fetch the current list of those devices that have reported failed-delivery attempts. The Java library parses the "raw" (binary) data-stream from the server and returns a list of iPhone tokens. The feedback server clears this list when it is sent... so once fetched, it cannot be re-fetched.<br>
<br>
<h2>Getting device tokens where the notification delivery has failed</h2>
<ul><li><a href='FeedbackBasicExample.md'>FeedbackBasicExample</a>
</li><li><a href='WorkingFeedbackExample.md'>WorkingFeedbackExample</a></li></ul>

<h2>Limitations</h2>
<ul><li>Will not work on Google App Engine - try Urban Airship instead - @alhalayqa</li></ul>

<h2>Old Documentation</h2>
<ul><li><a href='How2UseJavapns.md'>How2UseJavapns</a>