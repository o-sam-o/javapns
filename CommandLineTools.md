# Command line tools #

**JavaPNS** includes command line tools that can be used to push test notifications and query the Feedback Service from a console or terminal (no Java programming required), making it easier to test notifications in your mobile application.

The following command line examples assume you have downloaded the latest JavaPNS distribution archive which includes the JavaPNS library and all the [required third-party libraries](http://code.google.com/p/javapns/wiki/GeneralRequirements), that you have properly generated the [required keystore file](http://code.google.com/p/javapns/wiki/PreparingCertificates), and that you will be running the command lines from the **`My project`** directory.

```

*My project/*
JavaPNS_2.1.jar
keystore.p12
lib/
bcprov-jdk15-146.jar
log4j-1.2.15.jar```


<br />


### Pushing a test notification ###
You can push a simple "Hello World!" alert notification to a single device from the command line using one of the following commands (_make sure you specify a path and a password to YOUR keystore, as well as a valid device token_):

To push a notification through the **sandbox** service:```
java -cp "JavaPNS_2.1.jar:lib/bcprov-jdk15-146.jar:lib/log4j-1.2.15.jar" javapns.test.NotificationTest keystore.p12 yourPassword 2ed202ac08ea9033665d853a3dc8bc4c5e98f7c6cf8d55910df290567037dcc4 sandbox```

To push a notification through the **production** service:```
java -cp "JavaPNS_2.1.jar:lib/bcprov-jdk15-146.jar:lib/log4j-1.2.15.jar" javapns.test.NotificationTest keystore.p12 yourPassword 2ed202ac08ea9033665d853a3dc8bc4c5e98f7c6cf8d55910df290567037dcc4 production```

<br />
To push a complex test payload instead, add the word "complex" after the commands above:```
java -cp "JavaPNS_2.1.jar:lib/bcprov-jdk15-146.jar:lib/log4j-1.2.15.jar" javapns.test.NotificationTest keystore.p12 yourPassword 2ed202ac08ea9033665d853a3dc8bc4c5e98f7c6cf8d55910df290567037dcc4 sandbox complex```

<br />

### Querying the Feedback Service ###
You can query the Feedback Service from the command line using one of the following commands (_make sure you specify a path and a password to YOUR keystore_):

To query the **sandbox** Feedback Service:```
java -cp "JavaPNS_2.1.jar:lib/bcprov-jdk15-146.jar:lib/log4j-1.2.15.jar" javapns.test.FeedbackTest keystore.p12 yourPassword sandbox```

To query the **production** Feedback Service:```
java -cp "JavaPNS_2.1.jar:lib/bcprov-jdk15-146.jar:lib/log4j-1.2.15.jar" javapns.test.FeedbackTest keystore.p12 yourPassword production```