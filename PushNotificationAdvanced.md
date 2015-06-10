# Advanced Push Notification #

### Building payloads ###

**JavaPNS**'s `Push` class provides easy ways of pushing simple notifications (such as alerts, badges, sounds, etc.) which spare you from creating payloads yourself.  As you create more elaborate mobile applications though, you will most likely want to customize your notifications by adding custom properties to payloads you push.  Instead of using the simple one-line push methods in the `Push` class (alert, badge, sound, combined, etc.), you can create a payload first, customize it the way you want, and push it using one of the `Push.payload` methods.

To create a custom payload and push it, see the following code:
```
public void send (List<Device> devices, Object keystore, String password, boolean production) {

/* Build a blank payload to customize */
PushNotificationPayload payload = PushNotificationPayload.complex();

/* Customize the payload */
payload.addAlert("Hello World!");
payload.addCustomDictionary("mykey1", "My Value 1");
payload.addCustomDictionary("mykey2", 2);
// etc.

/* Push your custom payload */
List<PushedNotification> notifications = Push.payload(payload, keystore, password, production, devices);

}```

Refer to the javadoc for [Payload](http://javapns.googlecode.com/svn/trunk/doc/javapns/notification/Payload.html), [PushNotificationPayload](http://javapns.googlecode.com/svn/trunk/doc/javapns/notification/PushNotificationPayload.html) and [NewsstandNotificationPayload](http://javapns.googlecode.com/svn/trunk/doc/javapns/notification/NewsstandNotificationPayload.html) for more information on methods available on a `Payload` object.

<br />

### Sending large amounts of notifications (multi-threading) ###

**JavaPNS** includes tools for streaming large number of notifications safely and efficiently using multiple parallel threads.

To send a notification to hundreds or thousands of devices, use the following code:
```
public void send (List<Device> devices, Object keystore, String password, boolean production) {

/* Prepare a simple payload to push */
PushNotificationPayload payload = PushNotificationPayload.alert("Hello World!");

/* Decide how many threads you want to create and use */
int threads = 30;

/* Start threads, wait for them, and get a list of all pushed notifications */
List<PushedNotification> notifications = Push.payload(payload, keystore, password, production, threads, devices);

}```

Note: the multithread payload method above will return only once all threads have finished pushing notifications. If you do not want to wait for them to complete, run the code above inside a separate Thread (example: `new Thread() {public void run() {...}}.start();`).

<br />

### Creating a push queue (connection pool) ###

**JavaPNS** includes tools for creating push queues (connection pools).  A queue is a group of multi-threaded parallel live connections to APNS which simply wait for you to queue notifications to push.  The queue dispatches new notifications to threads and connections on a rotation basis.

To create a queue (and add a simple notification), use the following code:
```
public void send (String token, Object keystore, String password, boolean production) {

/* Prepare a simple payload to push */
PushNotificationPayload payload = PushNotificationPayload.alert("Hello World!");

/* Decide how many threads you want your queue to use */
int threads = 30;

/* Create the queue */
PushQueue queue = Push.queue(keystore, password, production, threads);

/* Start the queue (all threads and connections and initiated) */
queue.start();

/* Add a notification for the queue to push */
queue.add(payload, token);

}```

Note: if you do not start the queue manually (like above), the queue will start itself automatically on the first call to an `add` method.


<br />

### Configuring a proxy ###
To set a proxy for all connections initiated by JavaPNS, use the following code:
```
pushNotificationManager.setProxy(host, port);```

<br />

### Enhanced notification format ###
The Apple Push Notification System describes an optional enhanced notification format which provides support for various additional options (see [this page](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CommunicatingWIthAPS/CommunicatingWIthAPS.html)).  By default,  JavaPNS uses the enhanced notification format when pushing notifications to Apple servers.

To customize the expiry option provided by the enhanced notification format, you must first get a Payload object and invoke `payload.setExpiry(long)` to customize its value (default is 1 day).  Error-response packets, if any are received, are attached to their related `PushedNotification` objects, allowing you to analyze notification results in context.

If for any reason you wish to switch to the simple notification format, you can do so by invoking `PushNotificationManager.setEnhancedNotificationFormatEnabled(false);`.  When using the simple format, payload identifiers and expiry are ignored, and the error-response packet reader is disabled.  It is generally not recommended to turn off the enhanced notification format because using it provides much better error detection.

<br />

### More control over default settings ###
The methods in `javapns.Push` accept a boolean parameter that tells JavaPNS to use Apple's production servers or not.  If `true`, javapns will use the production services (`gateway.push.apple.com:2195` and `feedback.push.apple.com:2196`); otherwise it will use the sandbox services (`gateway.sandbox.push.apple.com:2195` and `feedback.sandbox.push.apple.com:2196`).

To work around these predefined settings, take a look at the following code:
```
public void send (List<Device> devices, Object keystore, String password, String appleHost, int applePort) {

/* Gather communication details for your custom server */
AppleNotificationServer customServer = new AppleNotificationServerBasicImpl(keystore, password, ConnectionToAppleServer.KEYSTORE_TYPE_PKCS12, appleHost, applePort);

/* Prepare a simple payload to push */
PushNotificationPayload payload = PushNotificationPayload.alert("Hello World!");

/* Create a push notification manager */
PushNotificationManager pushManager = new PushNotificationManager();

/* Initialize the push manager's connection to the custom server */
pushManager.initializeConnection(customServer);

/* Push notifications and get the results */
List<PushedNotification> notifications = pushManager.sendNotifications(payload, devices);

}```


#### Changing default keystore type ####

JavaPNS uses "PKCS12" as the default keystore type.  You can customize this by setting a system property "javapns.communication.keystoreType" to the keystore type you want to use.  That property can be set to "PKCS12", "JKS", or any type supported by the JVM.

<br />


### OTA MDM technology support ###

JavaPNS includes basic support for Apple's Over-The-Air (OTA) MDM technology. The intent of this addition is to make JavaPNS even more useful to enterprise customers, thus improving widespread usage of this library. OTA MDM howerver is not the primary target of JavaPNS (Push Notification is), and consequently mdm support is provided as is, without serious testing. If you are involved in MDM, please feel free to get involved in this project so this area can be enhanced.