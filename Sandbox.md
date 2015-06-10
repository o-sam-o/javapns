# Useful notes about Apple's sandbox #

  1. **Sandbox-specific certificate**: the SSL certificate required for communicating with the sandbox servers is NOT THE SAME as the one required for the production servers.<br /><br />
  1. **Sandbox Feedback Service not listing device after app is removed**:  see [Feedback Service](FeedbackService.md)<br /><br />
  1. **Moving from sandbox to production**: once your notifications are working with the sandbox, you will naturally switch to Apple's production server.  A couple of things to watch out for when transitioning though:
    * device tokens are not the same for sandbox and production, you need new tokens when moving to production
    * certificates are also different (see [Certificates](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ProvisioningDevelopment/ProvisioningDevelopment.html))
    * some users have reported issues which might be solved by the procedure described on [this page](http://www.techjini.com/blog/how-we-fixed-production-push-notifications-not-working-while-sandbox-works/)