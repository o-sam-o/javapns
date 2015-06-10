# Basic Push Notification #

#### Pushing a simple notification to a single device ####
```

import javapns.Push;

public class PushTest {

public static void main(String[] args) {

Push.alert("Hello World!", "keystore.p12", "keystore_password", false, "Your token");

}
}```

The `alert` method's first parameter is the alert message itself.  The second parameter is a reference to your keystore (more information below).  The third parameter is your keystore's password.  The fourth parameter selects the production (`true`) or sandbox (`false`) service.  The fifth parameter is a device token (64-bytes) to push the alert to.

<br />


#### The `Push` class ####

The `javapns.Push` class, used in the previous example, is JavaPNS's center piece.  This is the class that most developers will ever use to push notifications.  It provides methods for quickly pushing alerts, badges, sounds, Newsstand notifications, and custom / complex payloads.

Here are the methods for pushing simple notifications:

  1. `alert (message, keystore, password, production, devices)`_: push a simple alert message_
  1. `badge (badge, keystore, password, production, devices)`_: push a badge number_
  1. `sound (sound, keystore, password, production, devices)`_: push a sound name_
  1. `combined (message, badge, sound, keystore, password, production, devices)`_: push a alert+badge+sound notification_
  1. `contentAvailable (keystore, password, production, devices)`_: notify Apple's Newsstand application_
  1. `test (keystore, password, production, devices)`_: push useful debug information_

<br />

If you create custom payloads yourself, you can use the following methods to push them:
  1. `payload (payload, keystore, password, production, devices)`_: push a single payload to devices_
  1. `payload (payload, keystore, password, production, numberOfThreads, devices)`_: use the built-in multithreaded transmission engine to push a single payload to lots of devices using n threads_
  1. `payloads (keystore, password, production, payloadDevicePairs)`_: push payloads to paired devices_
  1. `payloads (keystore, password, production, numberOfThreads, payloadDevicePairs)`_: use the built-in multithreaded transmission engine to push payloads to paired devices_

<br />

The `Push` class also provides the following methods which are described on other wiki pages:
  1. `feedback (keystore, password, production)`_: see [Managing Push Errors](ManagingPushErrors.md) for more information_
  1. `queue (keystore, password, production, numberOfThreads)`_: see [Advanced Push Notification](PushNotificationAdvanced.md) for more information_



<br />


---

##### Common parameters #####

Most methods in the `Push` class have some parameters in common.  Here are some details about these parameters:

  * **`Object keystore`**: a reference to a keystore file, or the actual keystore content.  See [Preparing certificates](PreparingCertificates.md) for more information about how to create a keystore.  You can pass the following objects to this parameter:
    * `java.io.File`: a direct pointer to your keystore file
    * `java.lang.String`: a path to your local keystore file
    * `java.io.InputStream`: a stream providing the bytes from a keystore
    * `byte[]`: the actual bytes of a keystore
    * `java.security.KeyStore`: an actual loaded keystore<br /><br />
  * **`String password`**: the password you used when creating your keystore.<br /><br />
  * **`boolean production`**: indicate which APNS service you want to use (sandbox or production).  Pass `true` to use the production service, or `false` to use the sandbox service.<br /><br />
  * **`Object devices`**: a list of device tokens to push your notification to.  Note that you should always pass a list of device tokens if you have one instead of creating a loop and invoking a method repetitively with each token.  This is because Apple strongly encourages users to send multiple notifications on a single connection, instead of opening and closing connections for each notification.  Passing your list of device tokens directly to JavaPNS will allow the library to optimize the connection usage.    You can pass the following objects to this parameter:<br /><br />
    * `java.lang.String[]` : an array of device tokens
    * `java.util.List<String>`: a list of device tokens
    * `java.lang.String`: a single device token
    * `javapns.devices.Device[]`: an array of device objects
    * `java.util.List<Device>`: a list of device objects
    * `javapns.devices.Device`: a single device object<br /><br />` `<br />
  * **`Object payloadDevicePairs`**: a list of paired of payloads and tokens to push.    You can pass the following objects to this parameter:<br /><br />
    * `javapns.notification.PayloadPerDevice[]`: an array of PayloadPerDevice objects
    * `java.util.List<PayloadPerDevice>`: a list of PayloadPerDevice objects
    * `javapns.notification.PayloadPerDevice`: a single PayloadPerDevice object<br /><br />


<br />

<br />


---

##### Returned objects and exceptions #####

The Apple Push Notification System is deceptively tricky to use, in that it has both strict and vague requirements, errors, and behaviors.  The JavaPNS library makes all possible efforts to handle these things in a user-friendly way.  When using the `Push` class, errors are communicated to you in a few different ways:

  1. the list of `PushedNotification` objects returned by most methods, which encapsulate details and results about each notification attempt the library actually made, based on the parameters you provided (see [Managing Push Errors](ManagingPushErrors.md) for more information);<br /><br />
  1. the critical exceptions thrown by most methods, which are usually related to communication or keystore issues;<br /><br />
  1. the Feedback Service which can be queried manually (see [Managing Push Errors](ManagingPushErrors.md) for more information).


<br /><br />

#### Frequently Asked Questions ####

  1. **How to send a different payload for each device?**: if each device should receive a different payload (_even slightly different_), you will need to create a `PayloadPerDevice` object for each device along with the payload it should receive.  You can then use the `Push.payloads(...)` methods to push your payloads.  Example:  ```
List<PayloadPerDevice> payloadDevicePairs = new Vector<PayloadPerDevice>();
payloadDevicePairs.add(new PayloadPerDevice(PushNotificationPayload.alert("Hello World 1!"), myDevice1));
payloadDevicePairs.add(new PayloadPerDevice(PushNotificationPayload.alert("Hello World 2!"), myDevice2));
payloadDevicePairs.add(new PayloadPerDevice(PushNotificationPayload.alert("Hello World 3!"), myDevice3));

Push.payloads("keystore.p12", "keystore_password", false, payloadDevicePairs);```<br />
  1. **How to improve performance?**: If you want to push multiple notifications, **avoid** calling `Push` class methods in a loop.  Most methods in the `Push` class will accept lists of multiple devices and/or payloads, and you should use those whenever possible.  Internally, JavaPNS will optimize its use of sockets and other operations whenever you provide a list of notifications;  those optimizations are not possible if you invoke a `Push` class method repeatedly for each notification you want to push.<br />
  1. **Why is there a 5-second delay after pushing a notification?**: this delay applies only when JavaPNS needs to close a socket, which happens after it has finished sending the notifications you pushed.  It was discovered after a lot of trial and error that some error response packets can be missed if a connection is closed too early, most likely because APNS experiences longer processing delays from time to time.  To make sure that all APNS response packets are received, the 5-second delay was found to provide the most safety against loosing packets.  If you are getting 5-second delays after **each** push, this most likely means that you are not using JavaPNS's more efficient ways of sending notifications, such as calling the `Push` class with you entire list of notifications instead of sending your notifications one by one (see comment above).<br /><br />


#### More information ####

To learn more about JavaPNS, you can take a look at the source code for the `javapns.test.NotificationTest` class.  This class contains examples for various push notification scenarios.

Refer to the [javadoc](Documentation.md) for more information about using the library, or see the [Advanced Push Notification](PushNotificationAdvanced.md) wiki page.