# Sending personalized push notifications

We can send push notifications to a specific user or list of users. This happens through the `touchsurgery.comms.push` module. The custom notification must be defined in `touchsurgery.comms.push.notifications`.

## Defining a push notification

Here is an example showing how to define a push notification:

    from touchsurgery.comms import push

    class ViewCountReached(push.Notification):
        title = 'Your procedure is helping people'
        body = 'Wow, {{name}}! Your proceudre has been viewed {{count}} times.'
        deep_link = 'https://a-deep-link.com/?procedure=my-procedure'
        defaults = {'name': 'Doctor', 'count': 'many'}

- **title:** Title text of the notification. `{{variable}}` elements can be added, and are filled at the time of send.
- **body:** Body text of the notification. `{{variable}}` elements can be added, and are filled at the time of send.
- **deep_link:** Optional. If present, the notification links to the specified URL. If absent, the notification opens the app.
- **defaults:** Provide defaults for `{{variable}}` elements if the dictionary returns a blank field.

## Setting a trigger for your push notification

You can set any kind of event to trigger your push notification to send. Continuing with our `ViewCountReached` example, we might decide to send it during a view count calculation, when the view count has met a threshold value.

    from touchsurgery.comms.push import notifications

    ...

    for procedure in procedures:
        count = calculate_view_count(procedure)
        if count > procedure.next_threshold():
            notifications.ViewCountReached().send(
            	[
            	    {
            	    	'user': procedure.author,
            	    	'variables': {
            	    		'count': count,
            	    		'name': procedure.author.first_name
            	    	}
            	    }
            	],
            	deep_link = get_procedure_deep_link(procedure)
            )

The `send` method queries the dictionary to get the user, the `{{variable}}` elements, and the optional deep link.

Text substitution is performed on fields marked as `{{variable}}` if the variable term matches one of the key/value pairs passed into the `send` method.

If `deep_link` is present, tapping the notification calls the specified URL. If `deep_link`is absent, tapping the notification opens the app.

Each push notification allows a single deep link only. If you need a notification to go to a different URL, you must define another push notification.

## The Notification class

This is how the `Notification` class is built.

    class touchsurgery.comms.push.Notification

      body = ''
      deep_link: str | None = None
      defaults: dict = {}
      send(payloads, deep_link=None, send_immediately=True, expires_after=None)
      title = ''

There is a subclass, `OneOffNotificaton`, that is intended for one-off push notifications.

    class touchsurgery.comms.push.OneOffNotification(title, body, deep_link=None, defaults=None)

      send(payloads, deep_link=None, send_immediately=True, expires_after=None)

## How this works

We use Amazon Pinpoint as the push notification backend.

The messaging services we use are:

- **APNS:** Apple Push Notification Service. For iOS devices.
- **FCM:** Firebase Cloud Messaging. For Android devices.

## Deleting unused endpoints

Whenever a user installs a Touch Surgery app, their endpoint is registered and associated with their Touch Surgery UUID. Since we have different apps on different platforms, it is possible for a single user to have multiple endpoints registered.

If we want to find the currently active endpoint for a given user, we must query the Pinpoint API for the endpoints of all apps. Then, we merge the results and send a notification to all of them.

Amazon Pinpoint does not automatically remove expired or inactive endpoints. This allows us to check which notifications failed to send to identify outdated or inactive endpoints. Unused endpoints should be deleted asynchronously.
