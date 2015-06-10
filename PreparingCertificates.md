# Preparing APNS certificates #

#### IMPORTANT: ####
**iPhone development certificate != iPhone push notification service certificate**
> There are a several iPhone certificates such as :
    * development
    * push server
    * game
When you create your SSL push service Certificate (Apple), please ensure that Enable for Apple Push Notification service is checked.

<br />

## Generate an APNS certificate ##

**[CLICK HERE for step-by-step instructions](https://developer.apple.com/library/mac/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ProvisioningDevelopment.html)**

After you have followed Apple's instructions, you will end up with a single ".p12" file containing your certificate and private key.  That file (referred to as a "keystore"), along with the file's password you configured, is what you need to provide to JavaPNS whenever you want to push notifications.



<br />

## Useful notes about certificates ##

  1. **Blank passwords**: blank or null passwords are in violation of the PKCS12 specifications.  Further more, the Java platform's built-in PKCS12 implementation throws exceptions when trying to load a keystore with no password.  Consequently, the JavaPNS library will throw an InvalidKeystorePasswordException if you try to use a blank or null password.  See comment #15 in [issue #38](https://code.google.com/p/javapns/issues/detail?id=#38) for more information.<br /><br />
  1. **Keystore content** (_two certificates in one file_):  do not put certificates for both sandbox and production servers into the same keystore file.  Java's SSL system automatically uses the first certificate in the file, so obviously one server or the other will never work, depending on the order of certificates in the file.  You must produce an independent keystore file for each server.<br /><br />
  1. **Up-to-date keystore**:  You MUST re-generate your keystore file from scratch whenever you make changes to your certificates or any other aspect of your provisioning profile at Apple.  If you do not, you will get errors like "Invalid certificate chain".