# Troubleshooting #

### Get the latest version of JavaPNS ###

If you are experiencing issues with JavaPNS, first make sure that you are using the latest public release.  This is especially important for users of version 1.6 or earlier, as tons of fixes and enhancements have been made since then.  The latest public release can be downloaded from the **[Downloads](http://code.google.com/p/javapns/downloads/list)** page.  Users migrating from version 1.x to 2.x will need to review the entire documentation wiki to learn about the significant changes the library went through between these two milestones.

If the issues you are experiencing have been fixed (according to an issue status) after the last public release, you will need to get the most recent build from SVN.  Be aware that nightly builds might not be as stable as the public release because they have not been tested as much.  The latest nightly build can be downloaded from **[SVN](http://code.google.com/p/javapns/source/browse/trunk)**.


<br />

### Find answers in the documentation ###

The **[wiki](http://code.google.com/p/javapns/w/list)** contains a lot of useful information to get started and to solve common issues.  The **[javadoc](http://javapns.googlecode.com/svn/trunk/doc/javadoc/index.html)** also contains a lot of useful information about using the library.

<br />

### Try the command line test tools ###

Before getting too deep into programming with the library, you might want to try out the command line test tools included with the library.  See the **[Command line tools](CommandLineTools.md)** wiki page for more information.

<br />

### Study sample code and test classes ###

A few wiki pages provide sample code to get you started _(see [Basic push notification](http://code.google.com/p/javapns/wiki/PushNotificationBasic) for example)_ with javapns.  You might also like to study the code in classes from the  [javapns.test](http://code.google.com/p/javapns/source/browse/#svn%2Fbranches%2Fjavapns2%2Fsrc%2Fjavapns%2Ftest)  package.

<br />

### Enable logging ###
#### Library logging ####

javapns uses Log4J for logging. To enable logging quickly, add the following code:
```

import org.apache.log4j.*;```
and before using javapns in your code:
```

try {
BasicConfigurator.configure();
} catch (Exception e) {
}```
If you are already using log4j, you can enable logging for javapns by adding the following line to your `log4j.properties` file:
```

log4j.logger.javapns=debug```

#### Java SSL logging ####
To troubleshoot certificate or SSL issues, add the following to your java VM arguments:
```
-Djavax.net.debug=all```

<br />

### Double-check your certificates ###
Most problems for first-time users revolve around the required SSL certificate.
  * make absolutely sure you follow precisely the procedure to generate and export your certificate
  * verify that the certificate you are providing to javapns (inside a keystore file) was generated for the same Apple service you are using (sandbox or production) - see the [Preparing certificates](PreparingCertificates.md) page

**Important**: you will not get any error if you try to push a notification through an APNS server that does not match your certificate (sandbox or production), either server-side or application-side, but your notification will never make it to your mobile application.  You **must** be very careful that the certificate provided to JavaPNS matches the one currently built into your mobile application, **and** that it matches the APNS server you are choosing to use (sandbox or production).  Any failure to match these three will result in missing notifications.

<br />

### Check the date and time ###
If the date and/or time is not set properly on your device or on your server, notifications might not be displayed on the device.

<br />

### Examine pushed notifications ###
Examine the list of `PushedNotification` objects returned by push methods.  Once you have a list of PushedNotification, you might like to print it on the screen using the following line to get useful details about each push notification attempt:
```
NotificationTest.printPushedNotifications(yourList);```
You can also find out if one `PushedNotification` was successful by calling:
```
boolean success = yourPushedNotification.isSuccessful();```

You should also invoke `javapns.Push.feedback(...)` periodically to get a list of devices Apple believes to be inactive.

**Refer to the [Managing push errors](ManagingPushErrors.md) wiki page to learn how to deal with errors.**


<br />

### Check prior issues ###
If you have a specific problem with push notifications, search the project's [Issues](http://code.google.com/p/javapns/issues/list) database.  When searching or browsing issues, make sure you select "All issues" in the Search pop-up menu.

<br />

### Report an issues ###
If you are still experiencing a problem after following all of the above, add a new issue to the project's [Issues](http://code.google.com/p/javapns/issues/list) database.  Please provide as much details as possible, including Java stack traces and Java code.  If your issue report is incomplete or not specific enough, it might take a lot more time for developers to help you out than if you provide a full and detailed report.


<br />