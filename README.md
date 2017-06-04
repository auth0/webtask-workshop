# Introduction to Webtask

## Webtask
Webtask is a platform for easily creating a serverless HTTP Endpoint / Webhook. Webtask executes node.js code in a secure and safe manner. There's no servers to deploy, no tools you have to install, just code. 

* Every Webtask is an HTTP endpoint
* Simple Node.js programming model
* High fidelity of HTTP, Webtasks can even return HTML
* 1000+ node modules available out of the box, support for any node module with package.json
* Rich browser based editing experience
* Middleware 
* Multi-tenant
* Extremely low latency

## How does it differ from other Serverless offerings.

* Designed from scratch for web-based / user interactions rather than plumbing between backend services.
* Specialized for Node.js
* Low configuration
* Highly customizable

## Hello Webtask

Let's jump in and create your first Webtask. Open your browser to [https://webtask.io/make](https://webtask.io/make)

This will give you a prompt to log in. Choose any of the credentials listed.

<a href="https://webtask.io/images/docs/editor/wt-editor-login.png" target="_blank"><img src="https://webtask.io/images/docs/editor/wt-editor-login.png" width="50%"/></a>

After you have logged in, you'll be taken to the Webtask `Create New` screen.

<a href="https://webtask.io/images/docs/editor/wt-editor-create.png" target="_blank"><img src="https://webtask.io/images/docs/editor/wt-editor-create.png" width="50%"/></a>

From here you can choose the type of Webtask to create. For now just choose "Webtask". You'll then be prompted for a name for your Webtask, enter `wt1` and click `Save`. Once you do you'll be taken right to the Webtask editor with a starter Webtask.

<a href="https://webtask.io/images/docs/editor/wt-editor-newtask.png" target="_blank"><img src="https://webtask.io/images/docs/editor/wt-editor-newtask.png" width="50%"/></a>

This Webtask outputs a JSON object with a `hello` property and a value of either Anonymous or the `name` query string value. Notice the two params of the function. `ctx` is the Webtask Context object. We'll come back to this later. The second param is `cb` which is the callback. The callback accepts two params `error` and `body` and must be called when the task completes execution, in order to return some data and a resposne code.

Go run it. Click on the `play` button <img src="https://cloud.githubusercontent.com/assets/141124/26714892/55023a52-4728-11e7-9026-03d9f6bcd2d3.png"/> which will bring up the runner. Now click `Run`. You'll see you Webtask is instantly executed and the message `{"hello":"Anonymous"}` is returned in the Runner window.

<img src="https://cloud.githubusercontent.com/assets/141124/26714955/a0a7f1ae-4728-11e7-96e1-d5b3d71c401d.png"/>

Now click on the Gear icon in the upper right of the runner.

Click on URL Params(0) and you will get an area to enter query string key/value pairs. Put the parameter `name` and then your name for the value

<img src="https://cloud.githubusercontent.com/assets/141124/26715131/52a3e296-4729-11e7-90db-616318770e29.png"/>

Then run it again. You'll see that the name is outputted.

<img src="https://cloud.githubusercontent.com/assets/141124/26715198/9a4e089c-4729-11e7-8c88-f9fc9139c79d.png"/>

Notice also the realtime log viewer that show each time the task is executed and how the long the execution takes.

<img src="https://cloud.githubusercontent.com/assets/141124/26715348/40428aa2-472a-11e7-84e5-0d1fbde9b282.png"/>


## Calling a Webtask from the browser
Each Webtask you create is automatically an HTTP endpoint. There's no special configuration, as soon as you create it, it is available over HTTP.

Let's try this out. If you look in the editor, you'll see a url with a copy/paste button. Press it and it will copy your URL to the clipboard.

<img src="https://cloud.githubusercontent.com/assets/141124/26715476/bb686abc-472a-11e7-96d1-c0f9be65396a.png"/>

Now open up a separate browser tab and paste that URL in your address bar and hit `return`. You'll see your Webtask return the anonymous result.

```javascript
{
    hello: "Anonymous"
}
```

Now go modify the URL and add the name param i.e. `?name=Glenn` (using your name). Hit `return` and you'll see as before that your name is returned.

```javascript
{
    hello: "Glenn"
}
```

## Using a Webtask as a Webhook
That URL can now easily be plugged in as a Webhook. You can try that out using one of our favorite Webhook based services, Github.

First modify the code of your Webtask and add a `console.log` statement in the code. This will send output to the log viewer. The code shoud look like the following

```javascript
module.exports = function(ctx, cb) {
  console.log("Webhook invoked");
  cb(null, { hello: ctx.data.name || 'Anonymous' });
};
```

Save the Webtask. Open a new tab to Github and create a new repo, or choose an existing fork / personal repo that you can modify. Go the `settings` page on the repo and then click `Webhooks`

<a href="https://cloud.githubusercontent.com/assets/141124/26735390/ad0835a0-4776-11e7-8dcb-4ceb2e5d96be.png" target="_blank"><img src="https://cloud.githubusercontent.com/assets/141124/26735390/ad0835a0-4776-11e7-8dcb-4ceb2e5d96be.png" width="50%"/></a>

Now go click the `Add webhook` button to create a new Webhook. For the payload URL, paste in the URL of your Webtask which you copied earlier (without any query params). Change the content-type to application/json. For events, put `Send me everything`. Leave all the other values to their defaults. Then click `Add webhook`.

<a href="https://cloud.githubusercontent.com/assets/141124/26736562/898a51ee-477b-11e7-8297-90294b025e8c.png" target="_blank"><img src="https://cloud.githubusercontent.com/assets/141124/26736562/898a51ee-477b-11e7-8297-90294b025e8c.png" width="50%"/><a>

As soon as the Webhook is created, it will get invoked. Go check the log viewer for your Webtask and you should see the "Webhook invoked" message in the console.

<img src="https://cloud.githubusercontent.com/assets/141124/26736848/97815bac-477c-11e7-9db7-a264db858d7e.png"/>

# Simple management in the browser
## Opening an existing task

In the same way that you can create a task from the browser, you can also open an existing task. To do this you use `webtask.io/edit/[task]` as the url. 

To open the task you created before, use this url: [https://webtask.io/edit/wt1](https://webtask.io/edit/wt1). This will bring you right into the editor

## Listing tasks
You can also list tasks. To do this press `<CMD> + p` (Windows Key on Windows) which will display a list of tasks. 

## Filtering tasks
On the list is displayed you can type into the search bar to filter.

## Deleting tasks
From the list you can delete tasks by clicking on the trash icon next to each task.

You've just seen the basics of using the Webtask editor to create your first Webtask. You've then seen how to invoke the Webtask from the runner, in the browser, and then as a Github Webhook. Wasn't that easy? This is just scratching the surface. Now you'll learn how to use the CLI.

# The wt-cli
In the first section you authored a Webtask via the browser. Webtask also offers wt-cli to create, edit, and manage your webtasks from the command line. One advantage of the CLI is you can create tasks using your favorite text editor / IDE and then upload them. The CLI also allows you to run tasks locally and even debug them! 

## Installing the CLI
Head over to [https//webtask.io/cli](https://webtask.io/cli) and follow the instuctions to install and test the wt-cli. You will receive a code via email which you will need to enter into the CLI in order to activate it.

### Common CLI commands
The CLI will let you do a number of things. Here are the most common commands:

* ls - List Webtasks. `wt ls`
* create - Creates a new webtask based on an existing file. The prefix for the file is used as the Webtask name by default. `wt create wt1.js`
* edit - Edits an existing Webtask in the Webtask editor. If no args are passed, then it will create a new Webtask in the editor. `wt edit wt1`
* mv - Renames a Webtask. `wt mv wt1 wt2`
* logs - View realtime logs `wt logs`
* serve - Runs a Webtask locally `wt serve wt1.js`
* debug - Debugs a Webtask locally.
* profile - Manage Webtask profiles

### Using the CLI
Now you'll do some basic things with the cli.

#### List
First list out your webtasks.

```
wt ls
```

If you just created your account today, you should see 2 tasks listed, the one we created the in the browser `wt1` and the second one you created in the cli.


#### Edit
Let's edit the first one.

```
wt edit wt1
```

This should open up the editor to view your Webtask.

```
gbmac:workshop glennblock$ wt edit wt1
Attempting to open the following url in your browser:

https://webtask.it.auth0.com/edit/wt-glenn-block-gmail-com-0#/...

If the webtask editor does not automatically open, please copy this address and paste it into your browser.
gbmac:workshop glennblock$
```

#### Serve
Next you'll create a task locally. Using your favorite editor, let's create a new task file `wt2.js`. Put the same task that you used earlier:

```javascript
module.exports = function(ctx, cb) {
  console.log("Webhook invoked");
  cb(null, { hello: ctx.data.name || 'Anonymous' });
};
```

You can run the task locally.

```
wt serve wt2.js
```

You'll see a message indicating your task is running.

```
gbmac:workshop glennblock$ wt serve wt2.js
Your webtask is now listening for IPv4 traffic on 127.0.0.1:8080
```

Open your browser to `localhost:8080`. You'll see your task has executed and you got back the response

```javascript
{
    hello: "Anonymous"
}
```

#### Create
You can now upload it to the cloud.

```
wt create wt2.js.
```

This will immediately create your task and share the URL.

```
gbmac:workshop glennblock$ wt create wt2.js
Webtask created

You can access your webtask at the following url:

https://wt-glenn-block-gmail-com-0.run.webtask.io/wt2.js
```

#### Logs
Before invoking your task, you can start viewing realtime logs from the cli.

```
wt logs
```

This will output to show you that the stream is connected and waiting for output.

```
gbmac:workshop glennblock$ wt logs
[20:42:49.638Z]  INFO wt: connected to streaming logs (container=wt-glenn-block-gmail-com-0)
```

Now go copy the task URL and open it in the browser. When you do you will see the realtime logs are displayed.

```
[20:45:04.443Z]  INFO wt: new webtask request 1496436304360.977826
[20:45:04.483Z]  INFO wt: Webhook invoked
[20:45:04.483Z]  INFO wt: finished webtask request 1496436304360.977826 with HTTP 200 in 44ms
```

# Deeper dive into Webtasks
So far you've created Webtasks that write to the console, access query strings, and return simple payloads. That's fun, but not that useful in the real world. We'll now go deeper into what you can do with Webtask.

## Accessing the payload
The first Webtask you created `wt1` was wired up to a Github Webhook. When the Webhook fires the message `Webhook invoked` is outputted to the console from the Webtask. To really do anything useful, the Webtask needs to access the payload Github sends which gives details about the invocation. 

You already saw how using `ctx.data` you were able to access querys string params. Webtask also lets you access the body of the request. In you code, you can use `ctx.body` to get the body. `body` will either be a JSON object if the payload is JSON and content-type is 'application/json' OR it will be the raw payload.

Go and edit your first task using the cli. `wt edit wt1.js`. Modify the code so it outputs the body to the console, instead of a static message. To do this you're going to use the built in `util` node module. Webtask also supports 3rd party modules which we'll discuss later. You'll use the `util` module to output the JSON object that Github sends.

```javascript
var util = require('util');
module.exports = function(ctx, cb) {
  console.log(util.inspect(ctx.body, {depth:null}));
  cb();
};
```

Now go back to your github repo and make a change either editing a file, adding an issue, etc. As soon as you do, you should see the Webhook is invoked and the Github payload will appear in the console window.

<a href="https://cloud.githubusercontent.com/assets/141124/26745585/fd3aa7dc-479f-11e7-8405-c55c2d0454f8.png" target="_blank"><img src="https://cloud.githubusercontent.com/assets/141124/26745585/fd3aa7dc-479f-11e7-8405-c55c2d0454f8.png" width="70%"/></a>

## Secrets
A very common use case for Webtask is to be a bridge between another service. For example you might want to send a notification to a Slack channel whenever an issue is filed in a Github repo. Using Webtask provides a real easy way to handle this kind of logic. 

In order to send to Slack though, you will need to provide an SLACK URL. This URL is a secret, similar to an API key that would use to talk to a service like Twilio, or it may be connection string information to connect to a Database. Generally you don't want this kind of information sitting in the code. For one thing it is a security risk to have keys loosely exposed in text. Another is it makes the code hard to reuse and test.

Webtask lets you store this kind of information separately from the code in a secure manner using [Secrets](https://webtask.io/docs/editor/secrets). Each Webtask can have one or more secrets with are then accessible off of the `secrets` param of the context object. Secrets are useful for more than just secure keys and connection strings, you can use them for general configuration as well.

You'll now see how you can use secrets to connecting your Webtask to Slack. Before you move forward the first thing you need is an incoming Slack URL. If one is not provided to you, you can create one in any Slack group that you are an admin following these [instructions](https://my.slack.com/services/new/incoming-webhook/]. Once you have the URL, copy it to the clipboard.

<img src="secrets"/>

Now you'll go add a Secret. In the editor (editing wt1) you can create Secrets using the Secrets panel. Click on the `Key` icon and then select `Secrets`. The Secrets panel will be displayed. Add a new secret called `SLACK_URL` and then place the URL you copied to the clipbard as the value. Click `Save` to save your secret.

As mentioned earlier, secrets can be accessed of the Context object using the name. To access the `SLACK_URL` you can use the code `ctx.secrets.SLACK_URL`.

To put everything together you now need to add logic to your task to send to Slack whenever there is an issue. For the actual sending to Slack you're going to take advantage of a 3rd party node module, `slack-notify`. Webtask has over 1000 modules available out of the box without any configuration, which you can just `require`. `slack-notify` is one such modules. We'll cover much more about Module later including how you can access ANY npm module.

Below is the updated code to send to Slack.


```javascript
module.exports = function(ctx, cb) {
  var slack = require("slack-notify")(ctx.secrets.SLACK_URL);
  var body = ctx.body;
  if (body.issue && body.action === "opened") {
    var issue = body.issue;

    var text='*New Issue*\n\n' + 
             `Repository: ${body.repository.full_name}\n` +
             `Number: ${issue.number}\n` +
             `Url: ${issue.url}\n` +
             `Title: ${issue.title}\n\n` +
             `${issue.body}`;

    slack.send({text:text, username: "webtask-bot", icon_emoji: ":robot_face:"});   
  }
  cb();
};
```

Here is what this code is doing:

* Requiring the `slack-notify` module and passing the `SLACK_URL` to the constructor.
* Checking if the notification is for a new issue that has been opened.
* If it is a new issue, creating a tailored Slack Message to send to the channel.
* Sending the message.

Now go create an issue in your repo. As soon as you do, you should see a Slack message similar to the following.

<img src="https://cloud.githubusercontent.com/assets/141124/26757499/fc58ed9c-4871-11e7-8261-ef83e2809c90.png" width="50%"/>

As you can see `Secrets` are really easy to use, and they keep your code more secure and easier to maintain. 

## Storage
Sometimes Webtasks need to persist state. Webtask includes a built in [storage API](https://webtask.io/docs/storage) that you can use within your tasks. You can persist and retreive a single JSON object in the store that is <= 500KB in size. Storage also supports concurrency, to prevent loss of data. It's use is primarly to maintain lightweight and transient state. To access storage you use the `storage` object on the `context`. 

For the slack example, you can imagine using storage to keep a counter of issues created for each repo. This is a good fit as the number of repos should be relatively small.

You'll change the task to persist a counter for each repo, and you'll add logic to allow retrieving the stats.

First you'll add the code, and then we'll review the new parts.

```javascript
module.exports = function(ctx, cb) {
  var slack = require("slack-notify")(ctx.secrets.SLACK_URL);
  var body = ctx.body;

  if (ctx.data.showstats === "true") {
    return getStats();
  }
  else if (body.issue && body.action === "opened") {
    console.log("issue created");
    var issue = body.issue;

    var text='*New Issue*\n\n' + 
             `Repository: ${body.repository.full_name}\n` +
             `Number: ${issue.number}\n` +
             `Url: ${issue.url}\n` +
             `Title: ${issue.title}\n\n` +
             `${issue.body}`;
             
    slack.send({text:text, username: "webtask-bot", icon_emoji: ":robot_face:"}); 
    incrementCounter();
  }

  function incrementCounter() {
    var attempts = 3;
    tryIncrementCounter(attempts);
  }
  function tryIncrementCounter(attempsLeft, error) {
    if (attempsLeft <= 0) {
      return cb(error || "no more attemps");
    }

    ctx.storage.get(function(error, data) {
      if (error) {
        // treat read errors as transient and retriable
        return tryIncrementCounter(error, attempsLeft - 1);
      }

      if (data === undefined) {
        data = {};
      }

      var repoName = body.repository.full_name
      data[repoName] === undefined ? data[repoName] = 1 : data[repoName]++;
      ctx.storage.set(data, function(error) {
        if (error) {
          if (error.code === 409) {
            return tryIncrementCounter(error, attempsLeft - 1);
          } else {
            return cb(error);
          }
        }
        cb();
      });
    });
  }

  function getStats() {
    ctx.storage.get(function(error,data){
      cb(null, data); 
    });
  }
}; 
```

Now to what the new code does:

* When the request is received, if the query string value of "getstats" is true, then any collected stats will be returned. The `get` function of Storage retrieves the persisted data object. If data has not previously been set then it will return undefined.
* After the Slack event is created:
 * The `get` function will be used to retrieve the data.
 * If no data was stored, then data will be initialized as an empty JSON object.
 * On the data object, the value for the repo name key will be incremented by 1. If it was previously undefined, then it will be initialized to 1.
 * The data object will be persisted using the `set` function on Storage.
 * If there is a 409 conflict (meaning another instance of the task updated storage AFTER this instance read the data), then it will resolve the conflict by choosing the greatest number between the current value and the conflicting value. It will then add 1 and try again.
 * After 3 total attempts to resolve it will return an error.

## HTTP fidelity and accessing the raw request and response
The callback object on Webtask allows you to return a body, whether it be a string or a JSON payload. This is useful for many scenarios, but sometimes you want to go further. You may need to access or set headers such as content-type or cache headers, or check for an API key. Each Webtask is an HTTP endpoint, and you can access the raw Node.js request and response objects. This will be especially useful for alternate programmng models, which we'll learn about later.

To do this, you use a different function signature for your task. 

```javascript
module.exports = function(context, req, res) {
  
}
```

* `req` and `res` are the raw Node.js request and response objects.
* `context.body` will not be populated by default. This is useful for advanced cases like chunked data. It is possible to force body to be populated.

Now you have access to the raw request. Let's see how you can return a simple HTML page.

Create a new webtask: `wt edit`. Once the editor opens, this time select `Pick a Template`. In the search bar type `Full` and select `Full HTTP control`. Type `wt3` for the name and click `Enter`.

This will create a task like the following:

```javascript
module.exports = function (context, req, res) {
  res.writeHead(200, { 'Content-Type': 'text/html '});
  res.end('<h1>Hello, world!</h1>');
};
```

* This task sets the Content-Type header to `text/html`.
* Returns a simple HTML response.

**Note:** Notice there is no callback object. As you have access to the raw response, you can call res.end, to end the response. 

Save the task and copy the Webtask URL to the clipboard as you did earlier. Now paste that into your browser.

<img src="https://cloud.githubusercontent.com/assets/141124/26758169/2c6ba84e-488b-11e7-9c21-8f99b6a3e884.png" width="50%"/>

As you can see, you now have a Webtask that returns an HTML page.

Using the raw request and response opens up a number of possiblities, you can write micro-apis, and you can return other kinds of rich content like a PDF or a graphic. You can even build `Express` tasks, something which you'll see later.

# Modules
When you need to go beyond the built in node.js functionality, Webtask has an answer, npm modules. It offers two ways to get access to node nodules so you can enrich the capabilities of your Webtasks.

## Using pre-installed modules
Webtask includes 1000+ modules for you to easily just use in you Webtask. To check if a module is pre-installed, you can search [here](https://tehsis.github.io/webtaskio-canirequire). In the earlier exercise we used the `slack-notify` module to send a notification to Slack. You can see that module is installed [here](https://tehsis.github.io/webtaskio-canirequire/#slack-notify). 

Accessing a pre-installed is really simple, you just require it. i.e. if you want to access `twilio` you just do `require('twilio')` and so on.

### Multiple versions
Some built-in modules have multiple versions. For example the `react` module has 2 [versions](https://tehsis.github.io/webtaskio-canirequire/#react). In the case of multiple versions, you can specify the version in the `require` statement i.e. `require('react@15.4.1')` will pull in that specific version.

## Including modules that are not pre-installed
Up until recently you were generally limited to set of pre-installed modules. We've now added support for any NPM module in the NPM registry. 

To include modules from the registry, you use a `package.json`, the idiomatic way of including modules in Node.js. When you create a task that has a package.json adjacent to it in the file-system, `wt-cli` will automatically ensure those modules are installed. Webtask keeps several caches of a module and all its dependencies, thus after the initial install all subsequent installs will be dramatically faster.

From a `require` standpoint, you do not specify any version when you require, if the module was specific via package.json.

Try this out yourself. Create a new task (wt4.js) locally. Then create a package.json using `npm init` and specifying the task name (wt4) for the name. Modify the package.json and include a module / version in the dependencies that is not pre-installed. You can verify using the [canirequire](https://tehsis.github.io/webtaskio-canirequire) tool mentioned earlier. Save your package.json. It should like something like the following:

```javascript
{
  "name": "wt4",
  "version": "1.0.0",
  "description": "",
  "main": "wt4.js",
  "dependencies": {
    "cheerio": "^1.0.0-rc.1"
  }
}
```

Now create the task using `wt create wt4.js`. You should see output similar to the following indicating the module is being installed.

```
gbmac:workshop glennblock$ wt create wt4.js
* Hint: A package.json file has been detected adjacent to your webtask. Ensuring that all dependencies from that file are avialable on the platform. This may take a few minutes for new versions of modules so please be patient.
* Hint: If you would like to opt-out from this behaviour, pass in the --ignore-package-json flag.
Resolving 1 module...
Provisioning 1 module...
cheerio@1.0.0-rc.1 is available
Webtask created

You can access your webtask at the following url:

https://wt-glenn-block-gmail-com-0.run.webtask.io/wt4
```

If the module installation fails, then an error will be reported.

## Handling of ranges and freezing dependencies
If you are famliar with `package.json`, then you know it supports ranges for dependencies. Webtask will honor these ranges, but it will freeze the dependencies at the time of install. Thus subsequent updates to the task will not cause new versions of the modules to be installed. If however you modify the package.json, then new versions will get installed.

# Local Execution and debugging
## Serving 
Earlier when you created your second task `wt2`, you saw how you were able to run the task locally using `wt serve`. That example was very simple, but it did not rely on 3rd party modules, secrets, or on accessing the storage APIs. In addition to running a task locally, you can also specify secrets and storage using JSON files. You'll see how in this section.

First do the following steps.

* Open up `wt1` in the editor: `wt edit wt1`. 
* Copy the contents of the task to a local wt1.js. 
* Install locally the slack-notify module: `npm install slack-notify`. 

### Secrets & Storage
To set secrets, you can provide a secrets file where each secret is a key/value pair. For storage you can provide a text file which will act as the store. You can also pre-populate the text file if you want to provide test data in the store.

First create a new text file called `secrets`. Add the contents below substituting {slack_url} with the SLACK URL you used in the task.

```text
SLACK_URL={slack_url}
```

Now create an empty text file for storage: 

```text
touch storage
```

### Executing with Secrets & Storage
To serve, you'll additionally specify the secrets and storage file at the command line. Also you'll need to pass --parse-body to force the body to be parsed.

```text
wt serve --parse-body wt1.js --secrets-file secrets --storage-file storage
```

Now that the server is running, you can send a request. Becuase this task requires a POST with a body, you can use `curl`

Open a second terminal window and paste the `curl` command below directly.

```
curl localhost:8080 -H "content-type: application/json" -d '{ 
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

Check the first terminal, you should see the message `issue created`. Also if you check your slack channel, you'll see the dummy issue was in fact created!

<img src="https://cloud.githubusercontent.com/assets/141124/26760014/1b8f655a-48c3-11e7-9976-3bf5dd985796.png" width="50%"/>

## Debugging
In the past section, you saw how you can locally serve your task, providing secrets, storage and 3rd party modules. Wouldn't it be great if you could step through debug? It turns out you can, even using breakpoints and watches. You'll see how now!

In addition to the `wt serve` command, there is a `wt debug` command. There are 2 ways you can debug.

### Devtool
There's a convenient stand alone Electron-based debugger called Devtool, which you can install right from npm. Let's see how you can use it to debug the task. First install Devtool from npm: `npm install -g devtool`. 

Once devtool is installed you can launch `wt-cli` telling it to use it to debug. Here is the command to debug our task.

```
wt debug -d=devtool --parse-body wt1.js --secrets-file secrets --storage-file storage
```

Devtool will popup. Once it opens, you can drill down into the file explorer on the left to find your task. Then double click on it and you can add a breakpoint as is shown below.

<a href="https://cloud.githubusercontent.com/assets/141124/26760129/f7145520-48c5-11e7-8e19-0928df592264.gif" target="_blank"><img src="https://cloud.githubusercontent.com/assets/141124/26760129/f7145520-48c5-11e7-8e19-0928df592264.gif" width="50%"/></a>

Now that your breakpoint is set, you can test it out.

Run the previous `curl` command that you ran before:

```
curl localhost:8080 -H "content-type: application/json" -d '{ 
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

You'll see that the debugger will break on the breakpoint you set previously. You can then step through debug, inspect variables and everything!

<a href="https://cloud.githubusercontent.com/assets/141124/26760204/70ca74b6-48c7-11e7-8684-92a389cd6794.gif" target="_blank"><img src="https://cloud.githubusercontent.com/assets/141124/26760204/70ca74b6-48c7-11e7-8684-92a389cd6794.gif" width="50%"/></a>

Go stop the task from being served and Devtool will automatically close. Now let's see how you can debug using Visual Studio Code.

### VS Code
If you run `wt debug` without specifying `-d`, then `wt-cli` will launch standard node debugging allowing you to attach with an IDE like VS Code or Webstorm. 

Here's the steps to debug with VS Code.

First serve the task using wt-cli without specifying `-d`

```
wt debug --parse-body wt1.js --secrets-file secrets --storage-file storage
```

Once the task is running do the following steps.

* Open VS Code 
* Open the folder where your task lives within VS Code.
* Open wt1.js and add a breakpoint.
* Press the `Debug` button.
* Select `Add Configuration` via the Drop Down. 
* Select `Node.js: Attach`.
* Save the Launch.json 
* In the Dropdown select "Attach"
* Press the `Play` button.

Repeat the previous curl from the terminal. You will see your breakpoint is hit and you can debug!

<a href="https://cloud.githubusercontent.com/assets/141124/26760389/94c0d3ac-48cb-11e7-9a03-4e89241e2be1.gif" target="_blank"><img src="https://cloud.githubusercontent.com/assets/141124/26760389/94c0d3ac-48cb-11e7-9a03-4e89241e2be1.gif" width="50%"/></a>
