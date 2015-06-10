**JavaPNS** is a Java Library used to send [Apple Notification Service](http://developer.apple.com/iphone/library/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CommunicatingWIthAPS/CommunicatingWIthAPS.html) (APNS) messages to iOS Apps

This Java library provides a way to convert your notification to the “raw” (binary) data expected by the APNS and send using an asynchronous streaming socket over TLS (or SSL).  Essentially, you can send push notifications using a single line of code, or benefit from the library's flexible architecture to develop more elaborate push notification-based systems.

## Index ##
  * [Documentation](Documentation.md)
  * [Requirements](GeneralRequirements.md)
  * [Preparing certificates](PreparingCertificates.md)

  * [Troubleshooting, solving issues, etc.](Troubleshooting.md)
  * [Command line test tools](CommandLineTools.md)
  * [Useful information about the sandbox](Sandbox.md)


<br />

## Sending Push Notifications ##
  * [Basic push notification](PushNotificationBasic.md)
  * [Advanced push notification](PushNotificationAdvanced.md)

You should regularly connect with the feedback service and fetch the current list of those devices that have reported failed-delivery attempts. The Java library parses the "raw" (binary) data-stream from the server and returns a list of iPhone tokens. The feedback server clears this list when it is sent... so once fetched, it cannot be re-fetched.

<br />

## Managing delivery errors ##
  * **[Managing push errors](ManagingPushErrors.md)**:
    * [Handling errors](ManagingPushErrors#Handling_errors_(exceptions_and_response_packets).md)
    * [Working with the Feedback Service](FeedbackService.md)

<br />

## Upgrading from javapns 1.x ##
JavaPNS 2 has been completely reengineered to be more flexible and adapt to a larger number of projects.  Consequently, many classes have been refactored, replaced by interfaces where possible, and other changes.  Please take some time to look at the javadoc for version 2 so you can make the necessary changes in your existing code.  If your use of JavaPNS is somewhat basic, you should look at the new `javapns.Push` class which is the new preferred and much simpler way of quickly pushing notifications.