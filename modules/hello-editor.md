# Hello Editor

Let's jump in and create your first webtask. 

## Authenticate

- Open your browser to [https://webtask.io/make](https://webtask.io/make)
- This will give you a prompt to log in. 
- Choose any of the credentials listed.

![wt editor login](../images/wt-editor-login.png)

After you have logged in, you'll be taken to the **Auth0 Webtask Editor**.

## Create a Webtask

- Click the `Create a new one` link. 
- Select the `Create empty` option in the **Create New** dialog.
- Enter `wt1` for the webtask name.
- Click `Save`. 

Once you do you'll be taken right to the editor with a starter webtask.

![Create new](../images/wt-editor-create-new.gif)

This webtask outputs a JSON object with a `hello` property and a value of either Anonymous or the `name` query string value.

Notice the two params of the function. `context` is the webtask Context object. We'll come back to this later. The second param is `cb` which is the callback. The callback accepts two params `error` and `body` and **must** be called when the task completes execution, in order to return some data and a response code.

## Run the Webtask

- Click the `play` icon which will bring up the runner. 
- Click the little gear in the uppper right, then the blue `Run` button. 
 
![Run](../images/wt-editor-run.gif)

See that webtask is instantly executed and (as of 10/17/2017 ) the following is returned in the Runner response frame.

``` 
{
  "code": 500,
  "error": "Script generated an unhandled synchronous exception.",
  "details": "TypeError: Cannot read property 'name' of undefined",
  "name": "TypeError",
  "message": "Cannot read property 'name' of undefined",
  "stack": "TypeError: Cannot read property 'name' of undefined\n    at module.exports (/data/io/58fa2909141e408eb0e57579a9cfea56/webtask.js:2:33)\n    at Async.series.Request.get.Async.series.Async.forEachOf.createError.code (/data/sandbox/lib/sandbox.js:808:33)\n    at /data/sandbox/node_modules/async/dist/async.js:3853:24\n    at replenish (/data/sandbox/node_modules/async/dist/async.js:946:17)\n    at iterateeCallback (/data/sandbox/node_modules/async/dist/async.js:931:17)\n    at /data/sandbox/node_modules/async/dist/async.js:906:16\n    at /data/sandbox/node_modules/async/dist/async.js:3858:13\n    at /data/sandbox/lib/sandbox.js:882:24\n    at invokeCallback (/data/sandbox/node_modules/raw-body/index.js:224:16)\n    at done (/data/sandbox/node_modules/raw-body/index.js:213:7)"
}
```
Doing a little debugging, change line 2 in the editor:

__from__: `cb(null, { hello: context.body.name || 'Anonymous' });
`

__to__: `cb(null, { hello: context || 'Anonymous' });
`

Click the save to disk icon or `ctrl+s`, once saved the `gear` icon and the blue `run` button.

In the Runner window response frame see the contents of `context`:

```
{
  "hello": {
    "meta": {
      "wt-editor": "https://cdn.auth0.com/webtask-editor/editors/1/function-editor.js",
      "wt-editor-icon": "wt-icon-717"
    },
    "storage": {
      "timeout": 10000,
      "token": "eyJhbGciOiJIUzI1NiIsImtpZCI6IjIifQ.eyJqdGkiOiI2NTQzNzUxOTlkMjg0YzIzODQ3Y2M3NTJiMjE4NDIyNyIsImlhdCI6MTUwODI3MjExMSwiY2EiOlsiOTAwNzMzNGRiMDhjNGQ2M2E0MTNjZGFmM2YzYjYxNGMiLCI1ZmUxMDZmZjY3MDE0YWEwYWFlNGFmMGI0ZWFlY2Q2MiJdLCJkZCI6MCwidGVuIjoid3QtYzE3NWY3NDJmMzk2MzcwZGQzNzM4NDg0ZjQ5OWUwYWUtMCIsImp0biI6Ind0MSIsInBiIjoyLCJ1cmwiOiJ3ZWJ0YXNrOi8vbG9jYWxob3N0L2FwaS9kYXRhL2NvZGUvd3QtYzE3NWY3NDJmMzk2MzcwZGQzNzM4NDg0ZjQ5OWUwYWUtMCUyRnd0MSJ9.wfEZLPQOqtwolrDeL90ISmMKyt36SQvWSLGEqQbelo0"
    },
    "data": {},
    "params": {},
    "query": {},
    "secrets": {},
    "headers": {
      "host": "wt-c175f742f396370dd3738484f499e0ae-0.run.webtask.io",
      "accept": "application/json, text/plain, */*",
      "accept-encoding": "gzip, deflate, br",
      "accept-language": "en-US,en;q=0.5",
      "dnt": "1",
      "origin": "https://webtask.io",
      "referer": "https://webtask.io/make",
      "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:56.0) Gecko/20100101 Firefox/56.0",
      "x-forwarded-for": "131.191.55.208,::ffff:172.31.4.35",
      "x-forwarded-port": "443,8721",
      "x-forwarded-proto": "https,http",
      "accept-version": "2.0.0",
      "x-wt-params": "eyJyZXFfaWQiOiIxNTA4MjcyMjY1NjMzLjkyNzc3IiwiY29udGFpbmVyIjoid3QtYzE3NWY3NDJmMzk2MzcwZGQzNzM4NDg0ZjQ5OWUwYWUtMCIsInJlc29sdmVkX21vZHVsZXMiOm51bGwsInVybF9mb3JtYXQiOjMsInVybCI6IndlYnRhc2s6Ly9sb2NhbGhvc3QvYXBpL2RhdGEvY29kZS93dC1jMTc1Zjc0MmYzOTYzNzBkZDM3Mzg0ODRmNDk5ZTBhZS0wJTJGd3QxIiwicGIiOjIsImp0aSI6IjY1NDM3NTE5OWQyODRjMjM4NDdjYzc1MmIyMTg0MjI3IiwianRuIjoid3QxIiwidG9rZW4iOiJleUpoYkdjaU9pSklVekkxTmlJc0ltdHBaQ0k2SWpJaWZRLmV5SnFkR2tpT2lJMk5UUXpOelV4T1Rsa01qZzBZekl6T0RRM1kyTTNOVEppTWpFNE5ESXlOeUlzSW1saGRDSTZNVFV3T0RJM01qRXhNU3dpWTJFaU9sc2lPVEF3TnpNek5HUmlNRGhqTkdRMk0yRTBNVE5qWkdGbU0yWXpZall4TkdNaUxDSTFabVV4TURabVpqWTNNREUwWVdFd1lXRmxOR0ZtTUdJMFpXRmxZMlEyTWlKZExDSmtaQ0k2TUN3aWRHVnVJam9pZDNRdFl6RTNOV1kzTkRKbU16azJNemN3WkdRek56TTRORGcwWmpRNU9XVXdZV1V0TUNJc0ltcDBiaUk2SW5kME1TSXNJbkJpSWpveUxDSjFjbXdpT2lKM1pXSjBZWE5yT2k4dmJHOWpZV3hvYjNOMEwyRndhUzlrWVhSaEwyTnZaR1V2ZDNRdFl6RTNOV1kzTkRKbU16azJNemN3WkdRek56TTRORGcwWmpRNU9XVXdZV1V0TUNVeVJuZDBNU0o5LndmRVpMUFFPcXR3b2xyRGVMOTBJU21NS3l0MzZTUXZXU0xHRXFRYmVsbzAifQ==",
      "connection": "close"
    },
    "token": "eyJhbGciOiJIUzI1NiIsImtpZCI6IjIifQ.eyJqdGkiOiI2NTQzNzUxOTlkMjg0YzIzODQ3Y2M3NTJiMjE4NDIyNyIsImlhdCI6MTUwODI3MjExMSwiY2EiOlsiOTAwNzMzNGRiMDhjNGQ2M2E0MTNjZGFmM2YzYjYxNGMiLCI1ZmUxMDZmZjY3MDE0YWEwYWFlNGFmMGI0ZWFlY2Q2MiJdLCJkZCI6MCwidGVuIjoid3QtYzE3NWY3NDJmMzk2MzcwZGQzNzM4NDg0ZjQ5OWUwYWUtMCIsImp0biI6Ind0MSIsInBiIjoyLCJ1cmwiOiJ3ZWJ0YXNrOi8vbG9jYWxob3N0L2FwaS9kYXRhL2NvZGUvd3QtYzE3NWY3NDJmMzk2MzcwZGQzNzM4NDg0ZjQ5OWUwYWUtMCUyRnd0MSJ9.wfEZLPQOqtwolrDeL90ISmMKyt36SQvWSLGEqQbelo0",
    "id": "1508272265633.92777"
  }
}
```


 - Click on the Gear icon in the upper right of the runner.
 - Click on URL Params(0) and you will get an area to enter query string key/value pairs. 
 - Put the parameter `name` and then your name for the value.
 - Click `Run`.

 In the response section see inside `context`:
```
"data": {
      "name": "nd"
    }
```
Now the properties are known to query for in `context`, make the following change to line 2 
in your editor:

__from__: `cb(null, { hello: context || 'Anonymous' });
`

__to__: `cb(null, { hello: context.data.name || 'Anonymous' });
`

Click the save to disk icon or `ctrl+s`, once saved the `gear` icon and the blue `run` button.


![Run](../images/wt-editor-run2.gif)

You'll see that the name is outputted `{"hello":"Your Name"}`.

## View Logs

- Click on the `logs` button which will bring up the log viewer.
- Click on the Gear icon in the upper right hand corner of the runner.
- Click `Run`.

![View logs](../images/wt-editor-logs.gif)

Notice the realtime log viewer shows each time the task is executed and how the long the execution takes.

## Write Logs

- Check the tick box in the logs viewer labed `autoscroll`.
- Modify the code to write the `data` to the console using `console.log`.
- Click on the `save` button which will save your code changes.
- Click on the Gear icon in the upper right hand corner of the runner.
- Click `Run`.

Your code should look like this:

```javascript
module.exports = function(context, cb) {
  console.log(context.data); 
  cb(null, { hello: context.data.name || 'Anonymous' });
};
```

![Write logs](../images/wt-editor-write-logs.gif)


## Calling a Webtask from the Browser

Each Auth0 Webtasks you create is automatically an HTTP endpoint. There's no special configuration, as soon as you create it, it is available over HTTP.

Let's try this out. 

If you look in the editor footer, you'll see a url with a copy/paste button. 

- Click it and it will copy your URL to the clipboard.
- Hit `Ctrl-T` or `CMD-T` to open a separate browser tab.
- Paste the Url in your address bar and hit `enter`.

![Write logs](../images/wt-browser-run.gif)

You'll see your webtask return the anonymous result.

```javascript
{
    hello: "Anonymous"
}
```

- Modify the URL and add the name param i.e. `?name=Your Name` (using your name). 
- Hit `enter`. 

![Write logs](../images/wt-browser-run2.gif)
 
You'll see as before that your name is returned.

```javascript
{
    hello: "Your Name"
}
```

## Using a Webtask as a Webhook

That URL can now easily be plugged in as a Webhook. You can try that out using one of our favorite Webhook based services, [Github](https://github.com).

- Modify the code of your webtask and change the `console.log` statement to write `Webhook Invoked`.
- Click on the `save` button.

Your code should look like this:

```javascript
module.exports = function(ctx, cb) {
  console.log("Webhook Invoked");
  cb(null, { hello: ctx.data.name || 'Anonymous' });
};
```

- Open [Github](https://github.com).
- Create a new repository, or choose an existing one that you can modify.
- Go to the `settings` page.
- Click `Webhooks`.
- Click the `Add wehook` button.
- For the payload URL, paste the URL of your webtask.
- For Content type, select `application/json`.
- For events, select `Send me everything`.
- Leave all other values as defaults.
- Click `Add webhook` button.

![Add webhook](../images/github-add-webhook.gif)

As soon as the Webhook is created, it will get invoked. Go check the log viewer for your webtask and you should see the "Webhook Invoked" message in the console.

![Add webhook](../images/wt-editor-logs-webhook.png)

# Simple Management in the Browser

## Opening an Existing Task

In the same way that you can create a task from the browser, you can also open an existing task. To do this you use `webtask.io/edit/[task]` as the url. 

To open the task you created before, use this url: [https://webtask.io/edit/wt1](https://webtask.io/edit/wt1). This will bring you right into the editor.

## Listing Tasks

- Press `<CMD> + p` (Windows Key on Windows) which will display a list of tasks. 
- On the list displayed type `wt1` into the search bar to filter.
- Hit `enter` to open the wt1 task.
 
![List webhook](../images/wt-editor-list.gif)

## Deleting tasks

- On the left hand navigation, Click the `explore` icon which will display a list of all tasks.
- Click the `trash` icon next to the task you want to destroy.
- Click the `yes` button to confirm.

![List webhook](../images/wt-editor-delete.gif)

## Summary

You've just seen the basics of using the Auth0 Webtask Editor to create your first webtask. You've then seen how to invoke the webtask from the runner, in the browser, and then as a Github webhook. Wasn't that easy? This is just scratching the surface. 

Now you'll learn how to use the [CLI](hello-cli.md).