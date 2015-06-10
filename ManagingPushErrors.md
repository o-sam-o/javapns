# Managing push errors #

After sending push notifications, you will need to manage errors.  Errors can occur if you try to push a notification to an invalid token, if your mobile application is removed from a device, and many more other possibilities.  Managing errors allows you to cleanup your lists of devices so you can stop trying to push notifications to invalid or inactive devices. Apple's documentation states that they monitor providers for failure to cleanup their lists of devices, and that they can take action if the situation is not fixed promptly.  It is therefore important to manage push notification errors properly.

The Apple Push Notification System uses two different error reporting systems which work differently and report different kinds of errors.  The first system — _error-response packets_ — is involved while pushing notifications.  When you push notifications, you immediately get access to these responses (if any) through the `PushedNotification` objects returned by push methods.  The second system is the _Feedback Service_, which you must call periodically to get a list of devices that Apple has found to be inactive. You **must not** rely exclusively on either of these systems to remove invalid devices;  you **must** handle errors from **both** systems, as one will not list devices reported by the other.  In addition, the JavaPNS library itself can report errors of its own, and you need to pay attention to that as well.

<br />
## Handling errors (exceptions and response packets) ##

First and foremost, the library will throw critical exceptions if any occur.  This means that you need to catch them in order to deal with them.  Critical exceptions include problems with your keystore (such as a wrong password or a bad file format) and problems communicating with Apple servers.  Critical exceptions **do not include** however problems with individual messages, payloads or devices;  these exceptions are not thrown, but rather attached to their related `PushedNotification` objects (see below).

If no critical exception occurs, all push methods in the `javapns.Push` class return a list of `javapns.notification.PushedNotification` objects.  A `PushedNotification` object is created for each individual message (ie one per payload per device) the library pushes toward Apple servers, and encapsulates useful details about each attempt.

To find out if a push was successfully sent to Apple and that Apple did not return any error-response packet, simply invoke the `pushedNotification.isSuccessful()` method.  A notification might **not** be successful if any of these conditions occur:

  * the library rejected the token you provided because of obvious specs violations _(ex: token not 64-bytes long, etc.)_
  * the library rejected the payload you provided because of obvious specs violations _(ex: payload too large, etc.)_
  * a connection error occurred and the library was not able to communicate with Apple servers
  * an error occurred with your certificate or keystore _(ex: wrong password, invalid keystore format, etc.)_
  * a valid error-response packet was received from Apple servers
  * and many other possible errors...

Beyond the simplistic `isSuccessful()` result, you will probably want to know what the actual problem was.  To do so, invoke `pushedNotification.getException()` to get an exception describing why the notification was not successful.  Printing the exception to the console (`exception.printStackTrace()`) will probably help you understand what went wrong very quickly. _Note that exceptions are **not** thrown, they are only stored in the `PushedNotification` object and you need to get them manually using `getException()`; it is designed this way so that a single problematic device or notification will not block an entire notification operation (when pushing to multiple devices)._

If the error was reported by Apple servers through an error-response packet, you can get access to the actual response packet by invoking `pushedNotification.getResponse()`.

Managing errors after pushing a simple notification to multiple devices:
```

import javapns.*;
import javapns.notification.*;

public class PushTest {

public static void main(String[] args) {

String[] devices = {"MyToken111111111111111111111111111111111111111111111111111111111",
"MyToken222222222222222222222222222222222222222222222222222222222"};

try {

List<PushedNotification> notifications = Push.alert("Hello World!",
"keystore.p12", "keystore_password", false, devices);

for (PushedNotification notification : notifications) {
if (notification.isSuccessful()) {
/* Apple accepted the notification and should deliver it */ 
System.out.println("Push notification sent successfully to: " +
notification.getDevice().getToken());
/* Still need to query the Feedback Service regularly */ 
} else {
String invalidToken = notification.getDevice().getToken();
/* Add code here to remove invalidToken from your database */ 

/* Find out more about what the problem was */ 
Exception theProblem = notification.getException();
theProblem.printStackTrace();

/* If the problem was an error-response packet returned by Apple, get it */ 
ResponsePacket theErrorResponse = notification.getResponse();
if (theErrorResponse != null) {
System.out.println(theErrorResponse.getMessage());
}
}
}

} catch (KeystoreException e) {
/* A critical problem occurred while trying to use your keystore */ 
e.printStackTrace();

} catch (CommunicationException e) {
/* A critical communication error occurred while trying to contact Apple servers */ 
e.printStackTrace();
}
}
}```

<br />

## Working with the Feedback Service ##
JavaPNS includes everything you need to query the Feedback Service for inactive devices.  See the **[Feedback Service](FeedbackService.md)** page for information and examples on how to use that service.