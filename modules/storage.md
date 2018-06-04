# Storage

Sometimes webtasks need to persist state. Auth0 Webtasks include a built in [storage API](https://webtask.io/docs/storage) that you can use within your tasks. You can persist and retreive a single JSON object in the store that is <= 500KB in size. Storage also supports concurrency, to prevent loss of data. Its use is primarly to maintain lightweight and transient state. To access storage you use the `storage` object on `context`. 

For the slack example, you can imagine using storage to keep a counter of issues created for each repo. This is a good fit as the number of repos should be relatively small.

You'll change the task to persist a counter for each repo, and you'll add logic to allow retrieving the stats.

## Updating Webtask to Use Storage from the Editor

First you'll add the code, and then we'll review the new parts.

- Open your browser to [https://webtask.io/edit/wt6](https://webtask.io/edit/wt6).
  - Alternatively, you can use `wt edit wt6` from the command line.
- Modify the webtask code with the following logic.
- Click `save`.

```javascript
module.exports = function(ctx, cb) {
  var slack = require("slack-notify")(ctx.secrets.SLACK_URL);
  var body = ctx.body;
  var attempts;
  
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
    ctx.storage.get(function(error, data){
      if (data === undefined) {
        data={};
      }
      var repoName = body.repository.full_name
      data[repoName] === undefined ? data[repoName] = 1 : data[repoName]++;
      attempts = 3;
      ctx.storage.set(data, function(error) {
        setStorage(error, data);
      });
    });
  }
  
  function setStorage(error,data) {
    if(error) {
      if (error.code == 409 && attempts--) {
        data.counter = Math.max(data.counter, error.conflict.counter) + 1;
        return ctx.storage.set(data, setStorage);
      }
      else {
        return cb(error);
      }
    }
    cb();
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

## Testing Storage Implementation

Let's test this functionality out!

- Open your browser to [https://webtask.io/edit/wt6](https://webtask.io/edit/wt6).
  - Alternatively, you can use `wt edit wt6` from the command line.
- Open the **Logs** panel.
- Open a new tab in your browser and navigate to your repository.
- Add a new issue.
- Verify "issue created" is output to the logs console.
- Click the wrench icon and select "Storage".

You should now see a json object containing a property named based on your repository with a value of **1**. You can also fetch the values stored in storage via the API directly.

### Fetching Storage values using curl

Try executing this command from bash:

```bash
curl https://webtask.it.auth0.com/api/webtask/$(wt inspect wt6 --output json | jq .ten -r)/wt6/data \
 -H "Authorization: Bearer $(wt profile get --field token)"
```

This command uses curl to hit the API endpoint for your container to fetch your stored data. It also uses the wt-cli to retrieve your container name and profile token to authorize the request.

## Summary

You have just learned how to use the Storage api to persist small bits of state between Webtask executions. While not intended to be durable long term data persistence, it is useful to gather small bits of data across multiple executions of your webtasks.

Next up you will learn about the built in [Programming Models](programming-models.md).
 
