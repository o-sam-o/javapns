# Requirements #

# #1:  Understand the basics first... #

**JavaPNS** is a library that facilitates communication with the **Apple Push Notification System** (APNS).  Your very first step in working with APNS should be to read Apple's documentation about that technology.  Most questions from first-time users are already answered in the APNS manual.  Refer to the [Documentation](Documentation.md) page to find links to Apple documentation.

You also need to be familiar with the basics of your particular execution environment.  For example, if you choose to use JavaPNS in a web application that will be running inside Apache Tomcat, you first need to get a little familiar with Tomcat to understand where to put Java libraries so your web application can use them.

<br />

# #2:  Java Runtime Environment #

**JavaPNS** requires Java 1.5 or later.

```
 *IMPORTANT*:
One of JavaPNS's former dependencies (BouncyCastle) is reportedly not compatible with the Google App Engine. Please upgrade to JavaPNS 2.3 Beta 3 or later to use JavaPNS on Google App Engine.```

```
 *ABOUT JAVA 7*:
Some users have reported problems creating SSL connections while using Java 7 (in general, not just with JavaPNS).
Until this issue is completely resolved, you should try to run JavaPNS with Java 5 or 6 if you experience
SSL-related connection issues.```

First-time users that have not yet read Apple's documentation on APNS often wonder if a web server is required to use JavaPNS.  There is no such requirement.  JavaPNS can be used within a web application (in a web server), a desktop application, or any other program that meets your needs.  The APNS documentation helps a lot in understanding how programs will use the technology.

<br />

# #3:  JavaPNS library #

**JavaPNS** is distributed as a standard `.jar` Java library which you will need to download.

The latest public release can be downloaded from the **[Downloads](http://code.google.com/p/javapns/downloads/list)** page.

The latest nightly build _(may not be production-ready)_ can be downloaded from [SVN](http://code.google.com/p/javapns/source/browse/trunk).

<br />

# #4:    Java dependencies #

A few required open-source libraries will need to be included in the classpath to use JavaPNS:

  * **[log4j-1.2.15.jar](http://logging.apache.org/log4j/1.2/download.html)**

Where to ultimately put those libraries depends directly on your particular environment.  If you do not know where to put them, you need to get familiar with the installation basics of your environment (ex: Tomcat) before going any further.


<br />

# #5:   SSL Certificate (from Apple) #
The SSL Certificate is created by Apple from the App ID.  This step is the biggest source of problems for first-time users.  SSL certificates need to be generated very carefully, and any deviation from the procedure usually results in unexpected results.  See the **[Preparing certificates](PreparingCertificates.md)** page for all the details.

<br />

# #6:   Device Token #
A Device Token is provided by the iOS API with the following code:

`(void)application:(UIApplication )application didRegisterForRemoteNotificationsWithDeviceToken:(NSData)deviceToken`

Example:
> 2ed202ac08ea9033665d853a3dc8bc4c5e98f7c6cf8d55910df290567037dcc4

**Important note about Device token vs Device ID or UDID**: tokens and UDIDs should not be confused...  **the library uses 64-byte tokens exclusively**, which is how Apple designed the Push Notification System.  You cannot push to a device using its device ID;  your mobile app MUST get a valid device token from Apple and use that token to push notifications to that device.  Note that the id used by JavaPNS's DeviceFactory is a local reference only and is not related to the actual device UDID;  we recommend you use the device token as the id to make sure that all local devices have unique local identifiers.  Typically, a mobile app retrieves a token from Apple and registers it with a provider (your server);  the provider can then provide the token it received to the JavaPNS library to push notifications.

<br />
<br />

# #7:    iOS App #
Badges and alerts will appear if the App is not running and the App is registered with the `PushNotificationService`.
All other notifications need to be handled by the App while the App is running.

Note: JavaPNS works with APNS only!  It is not designed to work with Android or other platforms.