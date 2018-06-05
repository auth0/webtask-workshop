# Custom Programming Models

Earlier you learned about the three standard programming models that Webtasks support. In addition, you can create custom programming models in order to tailor the authoring experience in a domain specific way. This is particularly useful when building applications which can be customized by users via Webtask, such as when your app is powered by [Auth0 Extend](https://auth0.com/extend).

As an example, let's imagine a CRM system that fires a webhook when a lead is created with a payload similar to the following:

```json
{
  "name": "julietapano",
  "email": "julitapanoe@example.com",
  "value": "1500"
}
```

Using the standard programming model you could write a Webtask like this which handles the creation hook....

```javascript
module.exports = (ctx,cb) => {
  var lead = ctx.body;
  var slack = require("slack-notify")(slackUrl);
  slack.alert(`Lead created ${lead.name}`);
  cb();
}
```

With a custom programming model you could tweak this code. For example you can elevate the body so it is first class on the function parameters. We can also expose secrets as a global so that the slack module can be initialized once when the task is instantiated rather than every request.

```javascript
var slackUrl = module.exports.secrets.slack_url;
var slack = require("slack-notify")(slackUrl);

module.exports = (lead,cb)=> {
  slack.alert(`Lead created ${lead.name}`);   
  cb();
}
```

And this is just the beginning; you could go further and offer a completely different model using ES6 classes.

```javascript
var slackUrl = module.exports.secrets.slack_url;
var slack = require("slack-notify")(slackUrl);

module.exports = class LeadHandler {
  onCreate(lead, cb) {
    slack.alert(`Lead created ${lead.name}`);   
    cb();
  }
}
```

Your programming model doesn't even have to be JavaScript. Imagine authoring a task as a Pug file ?!?!?

```
head
  title Pug Micropage
body
    h1 I am a Pug Micropage
    - for (var x = 0; x < 10; x++)
      li I was generated through code
```

## Compilers

Custom programming models are possible through the use of *Webtask Compilers*. A Compiler is a function which takes an incoming source file and transforms it into a function that Webtasks can call. In the previous section you learned about Middleware. In addition to handling cross-cutting concerns like Authentication, you can also use Middleware to perform compilation. The `webtaskContext` has a `compiler` param.

Here is a the compiler that will handle passing in the lead as a first class param:

```javascript
const jsonParser = require('body-parser').json();

function createZeroCRMMiddleware() {
  return (req, res, next) => {
    const ctx = req.webtaskContext;
    const compiler = ctx.compiler;
      
    return compiler.nodejsCompiler(compiler.script, (error, webtaskFn) => {
      if (error) return next(error);
      jsonParser(req,res,(parseError) => {
        if (parseError)
          return next(parseError);
        webtaskFn.secrets = ctx.secrets;  
        return webtaskFn(req.body, (error, result) => {
          if (error) return next(error);

          res.writeHead(200, {
            'content-type': 'application/json',
          });
          res.end(JSON.stringify(result, null, 2));
        });
      });
    });
  };
}

module.exports = createZeroCRMMiddleware;
```

This is what the code does:

* Requires the JSON body parser. When you write a compiler, you have to own JSON parsing.
* Grabs the compiler object off of the Webtask Context.
* Calls the `nodejsCompiler` function passing in the script (the source file), and a callback. `webtaskFn` is the JavaScript function authored in the new programming model. In this case the source is JavaScript so we can pass it directly to the compiler.
* If compilation fails, then the middleware `next` callback is called.
* If compilation succeeds, then it parses the JSON body.
* Similarly, if the JSON parsing fails, the `next` callback is invoked passing the error.
* Secrets are attached to the function (this allows `module.exports.secrets` to work).
* Next, `webtaskFn` is invoked (the user's function) passing in the request body (the new signature) and a callback.
* If the function returns an error, then it is returned.
* If no error has ocurred, then we return the JSON result by ending the response.


 
