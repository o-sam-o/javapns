# Feedback Service #

Querying the Feedback Service:
```

import javapns.*;

public class FeedbackTest {

public static void main(String[] args) {

List<Device> inactiveDevices = Push.feedback("keystore.p12", "keystore_password", false);
/* remove inactive devices from your own list of devices */

}
}```

The `feedback` method's first parameter is a reference to your keystore;  it can be either a `java.io.File`, a `java.lang.String` (path to your keystore file), a `java.io.InputStream` or a `byte[]` array.  The second parameter is your keystore's password.  The third parameter selects the production (`true`) or sandbox (`false`) service.

The Feedback Service is one of two error reporting systems involved in push notification.  The other one is error-response packets.  You **must not** rely exclusively on the Feedback Service to remove invalid devices;  you **must** also use error responses while pushing notifications (see [Managing errors](ManagingPushErrors.md)) to detect different notification errors.

<br />

### More detailed example ###
A more detailed and functional example can be seen in the library's source code.  See class `javapns.test.FeedbackTest`.

<br />

## Important notes ##

  1. **Feedback Service** vs **Error-response packets**:  Both systems are designed to report problems with a particular token or notification.  The error-response packets provide immediate error reports when pushing notifications, while the feedback service provides on-demand error reports.  Although Apple's documentation is quite vague about the complementarity or redundancy of the two systems, hands-on experience suggests that they are designed to detect and report different errors, and that you **must** handle errors from both sources.  <br /><br />The error-response packets are returned for obvious and **immediately identifiable** issues such as for invalid tokens, while the feedback service lists devices which are **eventually** considered inactive because of repeated failed notification attempts over some period of time.  Consequently, you must deal with problematic device tokens in two separate occasions:  when an error-response packet is returned for a given token, and when you query the feedback service for inactive devices.<br /><br />You should not assume that the feedback service will list a device if that device's token has previously been targeted by an error-response packet.  If an error-response packet indicates that a device token is invalid, you should delete it immediately from your notification lists, and you most definitely should not wait for that token to be listed by the feedback service because it will most likely never be listed since you received an error-response packet for it.<br /><br /><br />
  1. **Delays involving the Feedback service**: Since Apple does not specify precisely when a device will end up on the Feedback list (ie how many failed push must occur or how much time must elapse before it is marked as inactive), it is not possible to predict reliably when a device will get listed by Apple's Feedback Service. The whole purpose of having an asynchronous Feedback Service to detect inactive devices is because Apple has a lot of factors to consider before listing a device as inactive...  If a notification can't be pushed, it can be because of network issues, device issues, bad reception issues, configuration issues, etc.  but if any of these problems arise, it does not necessarily mean that the device is permanently inactive...  it may simply be a temporary situation.<br /><br />
  1. **Sandbox Feedback Service not listing device after app is removed**:  iOS needs to inform the feedback service when a notification-enabled application is uninstalled, so that the device can be listed instantly by that service. HOWEVER, from various bits and pieces of information around the web, we learn that apparently there needs to remain at least one other notification-enabled application on the device so that iOS can inform the Feedback Service of the uninstallation. If the application you uninstall was the last notification-enabled app, iOS apparently cannot tell the Feedback Service about the uninstallation, and your device doesn't get listed right away (although it most likely will after some number of failed pushes and some time has elapsed).  This information suggests that not only do you need to have at least one other notification-enabled app on your device so that your own app uninstallation will be broadcasted, but that other app needs to be configured to talk to the SAME Feedback service (sandbox or production).<br /><br />