
---

```
 *IMPORTANT*:
This page is *deprecated* and therefore contains outdated and/or invalid information.
Please go back to the [http://code.google.com/p/javapns/ Project Home] to get access to the latest version and information.```

---

<br>

<h1>javapns 2.0</h1>
<h2>Requirements</h2>

<ul><li>Java 1.5<br>
</li><li>javapns 2.0: <a href='http://code.google.com/p/javapns/downloads/list'>javapns_2.0_Beta_5.jar</a>
</li><li>Open-source libraries:<br>
<ul><li><a href='http://commons.apache.org/downloads/download_lang.cgi'>commons-lang-2.4.jar</a>
</li><li><a href='http://commons.apache.org/downloads/download_io.cgi'>commons-io-1.4.jar</a>
</li><li><a href='http://www.bouncycastle.org/latest_releases.html'>bcprov-jdk15-146.jar</a>
</li><li><a href='http://logging.apache.org/log4j/1.2/download.html'>log4j-1.2.15.jar</a>
</li></ul></li><li>SSL certificate provided by Apple + private key, exported as a PKCS12 keystore file<br>
</li><li>Device Token ID  <i>(example: 2ed202ac08ea9033665d853a3dc8bc4c5e98f7c6cf8d55910df290567037dcc4)</i>
</li><li>iPhone app</li></ul>

<b>IMPORTANT</b>: If you are upgrading from javapns 1.x, please read the <b>Upgrading from javapns 1.x</b> section below.<br>
<br>
<br />

<h2>Usage</h2>

<b>Download javapns 2.0 from the Downloads page, or checkout the <a href='http://code.google.com/p/javapns/source/browse/#svn%2Fbranches%2Fjavapns2'>javapns2</a> branch and build it using the included Ant script.</b>

javapns 2.0 is designed so it can be plugged into existing projects and use their own data storage systems for devices and applications. However, the library includes its own basic implementations of these objects, so you can get started in no time.<br>
<br>
<br>
<h3>Basic</h3>
Pushing a simple notification:<br>
<pre><code><br>
import javapns.*;<br>
<br>
public class PushTest {<br>
<br>
public static void main(String[] args) {<br>
<br>
Push.alert("Hello World!", "keystore.p12", "keystore_password", false, "Your token");<br>
<br>
}<br>
}</code></pre>

To push to multiple devices:<br>
<pre><code><br>
import javapns.*;<br>
<br>
public class PushTest {<br>
<br>
public static void main(String[] args) {<br>
String[] devices = {"token 1", "token 2"};<br>
Push.alert("Hello World!", "keystore.p12", "keystore_password", false, devices);<br>
}<br>
}</code></pre>

All push methods in the <code>javapns.Push</code> class return a list of <code>javapns.notification.PushedNotification</code> objects.  A <code>PushedNotification</code> object is created for each individual message (ie one per payload per device) the library pushes toward Apple servers, and encapsulates useful details about each attempt.  To find out if a push was successfully sent to Apple and that Apple did not return any error-response packet, simply invoke the <code>PushedNotification.isSuccessful()</code> method.  More advanced users can get the actual error-response packet (if any was received) by invoking <code>PushedNotification.getResponse()</code>.<br>
<br>
<br />

<h3>Persisted with JPA</h3>
<ul><li>Implement <code>javapns.devices.Device</code> as a POJO in your project<br>
</li><li>Implement <code>javapns.devices.DeviceFactory </code> to hookup to your own DAO, or invoke JPA directly:<br>
<pre><code><br>
import java.util.*;<br>
import javax.persistence.*;<br>
import javax.persistence.criteria.*;<br>
import javapns.devices.*;<br>
import javapns.devices.exceptions.*;<br>
<br>
public class DeviceFactoryImpl implements DeviceFactory {<br>
<br>
private EntityManager entityManager; // Your EntityManager instance<br>
<br>
public Device addDevice(String id, String token) throws Exception {<br>
Device device = new DeviceImpl(id, token); // Your implementation of Device<br>
entityManager.persist(device);<br>
return device;<br>
}<br>
<br>
public Device getDevice(String id) throws UnknownDeviceException, NullIdException {<br>
CriteriaBuilder qb = entityManager.getCriteriaBuilder();<br>
CriteriaQuery&lt;Device&gt; query = qb.createQuery(Device.class);<br>
Root&lt;Device&gt; device = query.from(Device.class);<br>
query.where(qb.equal(device.get("id"), id));<br>
List&lt;Device&gt; result = entityManager.createQuery(query).getResultList();<br>
return result.size() &gt;= 1 ? result.get(0) : null;<br>
}<br>
<br>
public void removeDevice(String id) throws UnknownDeviceException, NullIdException {<br>
entityManager.remove(getDevice(id));<br>
}<br>
}</code></pre>
</li><li>Instantiate your <code>DeviceFactoryImpl</code> (can be instantiated by Spring)<br>
</li><li>Instantiate a <code>javapns.notification.PushNotificationManager</code> and invoke <code>setDeviceFactory(your DeviceFactoryImpl)</code> <i>(PushNotificationManager can be instantiated and DeviceFactoryImpl injected by Spring)</i>
</li><li>Instantiate a <code>javapns.feedback.FeedbackServiceManager</code> and invoke <code>setDeviceFactory(your DeviceFactoryImpl)</code> <i>(FeedbackServiceManager can be instantiated and DeviceFactoryImpl injected by Spring)</i>
</li><li>From your code, use the <code>PushNotificationManager</code> and <code>FeedbackServiceManager</code> <i>(both can be injected by Spring)</i> as documented<br>
</li><li>You can also implement your own <code>AppleNotificationServer</code> and <code>AppleFeedbackServer</code> so that connection configuration to Apple servers is stored in JPA as well</li></ul>


<br />

<h3>From the command line</h3>
<h4>Push a test notification</h4>
To push a simple "Hello World!" alert notification to a single device from the command line, use the library's built-in command-line tool (<i>make sure you specify a path and a password to YOUR keystore, as well as a valid device token</i>):<br>
<br>
Push to sandbox:<pre><code>java -cp "javapns_2.0_Beta_5.jar:lib/bcprov-jdk15-146.jar:lib/commons-io-1.4.jar:lib/commons-lang-2.4.jar:lib/log4j-1.2.15.jar" javapns.test.NotificationTest keystore.p12 yourPassword 2ed202ac08ea9033665d853a3dc8bc4c5e98f7c6cf8d55910df290567037dcc4 sandbox</code></pre>

Push to production:<pre><code>java -cp "javapns_2.0_Beta_5.jar:lib/bcprov-jdk15-146.jar:lib/commons-io-1.4.jar:lib/commons-lang-2.4.jar:lib/log4j-1.2.15.jar" javapns.test.NotificationTest keystore.p12 yourPassword 2ed202ac08ea9033665d853a3dc8bc4c5e98f7c6cf8d55910df290567037dcc4 production</code></pre>

To push a complex test payload instead, add the word "complex" after the commands above.<br>
<br>
<br />

<h4>Query the Feedback Service</h4>
To query the Feedback Service from the command line, use the library's built-in command-line tool (<i>make sure you specify a path and a password to YOUR keystore</i>):<br>
<br>
Feedback from sandbox:<pre><code>java -cp "javapns_2.0_Beta_5.jar:lib/bcprov-jdk15-146.jar:lib/commons-io-1.4.jar:lib/commons-lang-2.4.jar:lib/log4j-1.2.15.jar" javapns.test.FeedbackTest keystore.p12 yourPassword sandbox</code></pre>

Feedback from production:<pre><code>java -cp "javapns_2.0_Beta_5.jar:lib/bcprov-jdk15-146.jar:lib/commons-io-1.4.jar:lib/commons-lang-2.4.jar:lib/log4j-1.2.15.jar" javapns.test.FeedbackTest keystore.p12 yourPassword production</code></pre>


<br />
<hr />
<br />

<h2>Documentation</h2>
<ul><li><b>User documentation</b>:  please see the wiki<br>
</li><li><b>Javadoc</b>:  full javadoc is available in SVN as a .jar file (<a href='http://code.google.com/p/javapns/source/browse/branches/javapns2/javapns_2.0_Beta_5_javadoc.jar'>direct link</a>)</li></ul>

<i>Please note that user documentation is still under development.  The best and most up-to-date source of information about how the library works and can be used is most likely the javadoc.  Please refer to the javadoc for more information about library usage.<br></i>


<br />

<h2>Enabling logging</h2>
javapns uses Log4J for logging. To enable logging quickly, add the following code:<br>
<pre><code><br>
import org.apache.log4j.*;<br>
</code></pre>
and before using javapns in your code:<br>
<pre><code><br>
try {<br>
BasicConfigurator.configure();<br>
} catch (Exception e) {<br>
}</code></pre>

<br />

<h2>Upgrading from javapns 1.x</h2>
javapns 2.0 has been completely reengineered to be more flexible and adapt to a larger number of projects.  Consequently, many classes have been refactored, replaced by interfaces where possible, and other changes.  Please take some time to look at the javadoc for version 2.0 so you can make the necessary changes in your existing code.  If your use of javapns is somewhat basic, you should look at the new <code>javapns.Push</code> class which is the new preferred and much simpler way of quickly pushing notifications.<br>
<br>
<br>
<br>
<h2>Useful notes</h2>
<ol><li><b>Production vs Sandbox</b>: the methods in <code>javapns.Push</code> accept a boolean parameter that tells javapns to use Apple's production servers or not.  If <code>true</code>, javapns will use the production services (<code>gateway.push.apple.com:2195</code> and <code>feedback.push.apple.com:2196</code>); otherwise it will use the sandbox services (<code>gateway.sandbox.push.apple.com:2195</code> and <code>feedback.sandbox.push.apple.com:2196</code>).<br>
</li><li><b>Pushing to multiple devices</b>: when you have multiple devices to push to, make sure not to call <code>javapns.Push. ...()</code> for each and every token, as this will open and close network connections to Apple servers for each token (which Apple warns not to do).  Instead, provide an array of tokens; this will allow the library to open a connection, push your payload to all devices, and close the connection.  Also, to push notifications to large number of devices, consider using specialized classes in <code>javapns.notification.transmission</code>.<br>
</li><li><b>Detecting push errors</b>:  Examine the list of <code>PushedNotification</code> objects returned by push methods; the <code>PushedNotification.isSuccessful()</code> method helps determining quickly if a notification was successfully sent to Apple. You should also invoke <code>javapns.Push.feedback(...)</code> periodically to get a list of devices you should remove from your notification list.  Please see Apple's documentation on the Feedback Service to understand how push errors are reported.<br>
</li><li><b>Delays involving the Feedback service</b>: Since Apple does not specify precisely when a device will end up on the Feedback list (ie how many failed push must occur or how much time must elapse before it is marked as inactive), I believe that what you are seeing is the normal behaviour of Apple's Feedback Service. We must not forget that the whole purpose of having an asynchronous Feedback Service to detect inactive devices instead of simply receiving error codes while pushing notifications is because Apple has a lot of factors to consider before listing a device as inactive...  If a notification can't be pushed, it can be because of network issues, device issues, bad reception issues, configuration issues, etc.  but if any of these problems arise, it does not necessarily mean that the device is permanently inactive...  it may simply be a temporary situation.<br>
</li><li><b>Sandbox Feedback Service not listing device after app is removed</b>:  iOS needs to inform the feedback service when a notification-enabled application is uninstalled, so that the device can be listed instantly by that service. HOWEVER, from various bits and pieces of information around the web, we learn that apparently there needs to remain at least one other notification-enabled application on the device so that iOS can inform the Feedback Service of the uninstallation. If the application you uninstall was the last notification-enabled app, iOS apparently cannot tell the Feedback Service about the uninstallation, and your device doesn't get listed right away (although it most likely will after some number of failed pushes and some time has elapsed).  This information suggests that not only do you need to have at least one other notification-enabled app on your device so that your own app uninstallation will be broadcasted, but that other app needs to be configured to talk to the SAME Feedback service (sandbox or production).<br>
</li><li><b>Device token vs Device ID or UDID</b>: tokens and UDIDs should not be confused...  <b>the library uses 64-byte tokens exclusively</b>, which is how Apple designed the Push Notification System.  You cannot push to a device using its device ID;  your mobile app MUST get a valid device token from Apple and use that token to push notifications to that device.  Note that the id used by javapns's DeviceFactory is a local reference only and is not related to the actual device UDID;  we recommend you use the device token as the id to make sure that all local devices have unique local identifiers.  Typically, a mobile app retrieves a token from Apple and registers it with a provider (your server);  the provider can then provide the token it received to the javapns library to push notifications.<br>
</li><li><b>MDM support</b>: basic support has been included in javapns 2.0 for Apple's MDM technology.  The intent of this addition is to make javapns even more useful to enterprise customers, thus improving widespread usage of this library.  MDM is not the primary target of javapns (Push Notification is), and consequently mdm support is provided as is, without serious testing.  If you are involved in MDM, please feel free to get involved in this project so this area can be enhanced.<br>
</li><li><b>Moving from sandbox to production</b>: once your notifications are working with the sandbox, you will naturally switch to Apple's production server.  A couple of things to watch out for when transitioning though:<br>
<ul><li>device tokens are not the same for sandbox and production, you need new tokens when moving to production<br>
</li><li>certificates are also different (see <a href='http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ProvisioningDevelopment/ProvisioningDevelopment.html'>Certificates</a>)<br>
</li><li>some users have reported issues which might be solved by the procedure described on <a href='http://www.techjini.com/blog/2009/10/22/how-we-fixed-production-push-notifications-not-working-while-sandbox-works/'>this page</a>
</li></ul></li><li><b>Enhanced notification format</b>: the Apple Push Notification System describes an optional enhanced notification format which provides support for various additional options (see <a href='http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CommunicatingWIthAPS/CommunicatingWIthAPS.html'>this page</a>).  javapns 2.0 always uses the enhanced notification format when pushing notifications to Apple servers.  To customize the expiry option provided by the enhanced notification format, you must first get a Payload object and invoke <code>payload.setExpiry(long)</code> to customize its value (default is 1 day).  Error-response packets, if any are received, are attached to their related <code>PushedNotification</code> objects, allowing you to analyze notification results in context.