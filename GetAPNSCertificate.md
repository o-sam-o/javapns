
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

<h1>APNS Certificates</h1>

<h2>Notes</h2>
<b>iPhone development certificate != iPhone push notification service certificate</b>
<blockquote>There are a several iPhone certificates such as :<br>
<ul><li>development<br>
</li><li>push server<br>
</li><li>game<br>
When you create your SSL push service Certificate (Apple), please ensure that Enable for Apple Push Notification service is checked.</li></ul></blockquote>

<h1>Generate APNS Certificate</h1>
Step by step Instructions: <a href='http://appnotify.com/developer/getting-started-push'>http://appnotify.com/developer/getting-started-push</a>

(from: <a href='http://www.developers-life.com/apple-push-notification.html'>http://www.developers-life.com/apple-push-notification.html</a>)<br>
<br>
<ul><li>Each iPhone App requires it's own Certificate.<br>
</li><li>Only users with <code>Team Agent</code> access create a certificate<br>
</li><li>You need to create an App ID without <code>.*</code> in the iPhone developer Portal. An App ID without <code>.*</code> means its unique and works only for a single application</li></ul>

<h2>Log into the iPhone Developer Program Portal</h2>
<ol><li>Click on <code>Certificates</code> on the left menu<br>
</li><li>Click on the <code>How To</code> tab<br>
</li><li>Scroll down to the options at the bottom of the page</li></ol>

<h2>Certificate request</h2>
<ol><li>Generate a certificate signing request from your Mac's <a href='http://en.wikipedia.org/wiki/Keychain_%28Mac_OS%29'>KeyChain</a> and save to disk<br>
</li><li>Upload the <code>CertificateSigningRequest.certSigningRequest</code> to the Program Portal<br>
</li><li>Wait for the generation of cert (about 1 min). Download the certificate (aps_developer_identity.cer) from the Program Portal<br>
</li><li>Keep (or rename them if you want) these 2 files in a safe place. You might need the <code>CertificateSigningRequest.certSigningRequest</code> file to request a production cert in the future or to renew it again.</li></ol>

<h2>Export Certificate from Keychain</h2>
<ol><li>Import the <code>aps_developer_identity.cer</code> into <a href='http://en.wikipedia.org/wiki/Keychain_%28Mac_OS%29'>KeyChain</a>.<br>
</li><li>Select both certificate <b>and</b> private key (associated to the application you wish to use to send notifications)<br>
</li><li>Right click, and select <code>Export 2 elements</code>, give a name (for example : myCertificate.p12) and password (for example : p@ssw0rd) and then export as p12.</li></ol>
