# Middleware

We currently have a webtask that consumes a GitHub issue and formats a message to post into Slack. As we saw using curl, as long as we know the URL of the webtask we can send a correctly formatted issue and have messages show up in Slack. That is fine for development locally, but we probably don’t want an open HTTP endpoint on the internet capable of sending anonymous messages to our Slack!

Middleware is Node.js code you control that executes right before your webtask code does. It allows you to augment the default webtask execution logic without requiring any changes in the code of your webtask.

In this module, we will secure the webtask using simple bearer token authorization, then modify it to use an HMAC Digest singed token authorization all using the same webtask code.

## Bearer Auth Middleware

Securing webtasks with a bearer authentication token is such a typical process, that the webtask team publishes an NPM package that handles it. Let's secure our webtask so that only requests with valid tokens are allowed to use it.

- Open the `.secrets` file.
- Add a new line with the value `wt-auth-secret=mysecretkeytoken`.
- Execute the following command, to create a new webtask.

```bash
wt create wt6.js --name wt7 \
  --secrets-file .secrets \
  --middleware @webtask/bearer-auth-middleware \
  --meta wt-middleware=@webtask/bearer-auth-middleware
```
Now let's test to see if the new webtask is secured using the token value.

- Exeute the following `curl` command.

```bash
curl -X POST $(wt inspect wt7 --output json | jq .webtask_url -r) \
  -H 'content-type: application/json' \
  -d '{}'
```

You should see a **401** response returned rejecting the request.

```json
{
  "message":"Unauthenticated extensibility point",
  "statusCode":401
}
```

Now let's rerun the command supplying the authorization header.

- Execute the following `curl` command.

```bash
curl -X POST $(wt inspect wt7 --output json | jq .webtask_url -r) \
  -H 'authorization: Bearer mysecretkeytoken' \
  -H 'content-type: application/json' \
  -d '{ 
    "action":"opened", 
    "repository":{ 
        "full_name": "testrepo" 
    }, 
    "issue":{ 
        "number":1, 
        "url":"testurl", 
        "title":"test issue 1", 
        "body":"test body" 
    } 
}'
```

Shared key authorization is a reasonably easy to implement authorization method, and it is suitable for many use cases.

## Secure Github Webhook Middleware

Now let's create a new webtask based on the **wt6** code and secure it so that only GitHub can execute it.

- Open the `.secrets` file.
- Add a new line with the value `GH-WEBHOOK-SECRET=mysecretkeytoken`.
- Execute the following command, to create a new webtask.

```bash
wt create wt6.js --name wt8 \
  --secrets-file .secrets \
  --middleware secure-github-webhook \
  --meta wt-middleware=secure-github-webhook
```

Now that the webtask is secured when running on webtask.io, let's update our webhook on GitHub to point to the new webtask and use the secure token.

- Execute the following command.
- Copy the url.
 
```bash
wt inspect wt8 --output json | jq .webtask_url -r
```

- Open [Github](https://github.com).
- Choose the repository we used before.
- Go to the `settings` page.
- Click `Webhooks`.
- Click the `Edit` button next to the previous webhook.
- For the payload URL, paste the URL of the new webtask.
- Add the secret `mysecretkeytoken` to the **Secret** field.
- Click `Update webhook` button.

Finally, add a new issue to your repository and see GitHub securely send a message to Slack.

## Summary

In this module, you have learned how to implement use middleware to change the behavior of your webtask without affecting the source of your tasks. Middleware is a useful way to share cross-cutting concerns among many webtasks.

Next you'll learn how [custom programming models](custom-programming-models.md) can let you create webtasks in entirely new formats.
