
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

<h2>Java Libraries</h2>

<h3>Java 1.6 Version</h3>
Requires:<br>
<ul><li><a href='http://commons.apache.org/downloads/download_lang.cgi'>commons-lang-2.4.jar</a>
</li><li><a href='http://commons.apache.org/downloads/download_io.cgi'>commons-io-1.4.jar</a>
</li><li><a href='http://www.bouncycastle.org/latest_releases.html'>bcprov-jdk16-145.jar</a>
</li><li><a href='http://logging.apache.org/log4j/1.2/download.html'>log4j-1.2.15.jar</a></li></ul>

<h3>Java 1.5 Version</h3>
Requires:<br>
<ul><li><a href='http://commons.apache.org/downloads/download_lang.cgi'>commons-lang-2.4.jar</a>
</li><li><a href='http://commons.apache.org/downloads/download_io.cgi'>commons-io-1.4.jar</a>
</li><li><a href='http://www.bouncycastle.org/latest_releases.html'>bcprov-jdk15-145.jar</a>
</li><li><a href='http://logging.apache.org/log4j/1.2/download.html'>log4j-1.2.15.jar</a></li></ul>

<h2>SSL Certificate (from Apple)</h2>
The SSL Certificate is created by Apple from the App ID.<br>
(See certificate <a href='GetAPNSCertificate.md'>GetAPNSCertificate</a>)<br>
<br>
<h2>iPhone Token ID</h2>
Token ID is provided by the iPhone API with the following code:<br>
<br>
<code>(void)application:(UIApplication )application didRegisterForRemoteNotificationsWithDeviceToken:(NSData)deviceToken</code>

Example:<br>
<blockquote>2ed202ac08ea9033665d853a3dc8bc4c5e98f7c6cf8d55910df290567037dcc4</blockquote>

<h2>iPhone App</h2>
Badges will appear if the App is not running and the App is registered with the <code>PushNotificationService</code>.<br>
<br>
All other notifications need to be handled by the App while the App is running.