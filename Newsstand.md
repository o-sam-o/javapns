# Newsstand support #

#### Notifying a single device that new content is available for Newsstand ####
```

import javapns.*;

public class NewsstandTest {

public static void main(String[] args) {

Push.contentAvailable("keystore.p12", "keystore_password", false, "Your token");

}
}```

The `contentAvailable` method's first parameter is a reference to your keystore;  it can be either a `java.io.File`, a `java.lang.String` (path to your keystore file), a `java.io.InputStream` or a `byte[]` array.  The second parameter is your keystore's password.  The third parameter selects the production (`true`) or sandbox (`false`) service.  The fourth parameter is a device token (64-bytes) to notify.

<br />

#### Notifying multiple devices that new content is available for Newsstand ####

If you need to notify multiple devices that new content is available, you should pass your list of device tokens directly to the `contentAvailable` method, instead of creating a loop and invoking the `contentAvailable` method repetitively with each token.  This is because Apple strongly encourages users to send multiple notifications on a single connection, instead of opening and closing connections for each notification.  Passing your list of device tokens directly to JavaPNS will allow the library to optimize the connection usage.

Example:

```

import javapns.*;

public class NewsstandTest {

public static void main(String[] args) {
String[] devices = {"token 1", "token 2"};
Push.contentAvailable("keystore.p12", "keystore_password", false, devices);
}
}```

<br />


#### Managing push errors ####
After pushing notifications, you need to manage errors.  Errors can occur if you try to push a notification to a device that has become inactive, if a device token is invalid, or other kinds of issues.  See the **[Managing Push Errors](ManagingPushErrors.md)** page for information and examples on how to manage errors.