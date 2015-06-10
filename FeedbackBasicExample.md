
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>


<pre><code> // Get !FeedbackServiceManager Instance<br>
!FeedbackServiceManager feedbackManager = !FeedbackServiceManager.getInstance();<br>
<br>
// Get Devices<br>
!LinkedList&lt;Device&gt; devices = feedbackManager.getDevices( HOST, PORT, certificate, passwd, SSLConnectionHelper.KEYSTORE_TYPE_PKCS12 );<br>
<br>
// Display total devices returned<br>
System.out.println( "Devices returned: " + devices.size() );<br>
<br>
// Iterate over all returned devices and display each<br>
!ListIterator&lt;Device&gt; itr = devices.listIterator();<br>
while ( itr.hasNext() ) {<br>
System.out.println( itr.next() );<br>
}<br>
</code></pre>