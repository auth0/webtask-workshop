# Local Execution

Back in the [Hello CLI](hello-cli.md) module, we briefly mentioned that you could run webtasks locally using `wt serve`. That example was straightforward, but it did not rely on 3rd party modules, secrets, or on accessing the storage APIs. In addition to running a task locally, you can also specify secrets and storage using files. You will see how in this section.

## Fetch a Local Copy of WT6

Let's start by getting a local copy of **wt6** so you can run it locally.

- Create a new file in your workshop directory, `touch wt6.js`.
- Open your browser to [https://webtask.io/edit/wt6](https://webtask.io/edit/wt6).
  - Alternatively, you can use `wt edit wt6` from the command line.
- Using your favorite text editor, copy the webtask code to the **wt6.js** file.
- Using **NPM**, install the **slack-notify** module: `npm install slack-notify --save`.

## Local Secrets

To set secrets, you provide a secrets file where each secret is a key/value pair.

- Execute the command `touch .secrets`.
- Open it in your favorite text editor.
- Add a key/value pair for the `SLACK_URL` in the following format.
- Save the file. 

```
SLACK_URL={slack_url}
```

**Note:** Replace the entire token on the right-hand side including curly braces.


## Local Storage

For storage, you provide a text file which acts as the store. You can also pre-populate the file if you want to provide test data in the store.

- Execute the command `touch .storage`.

## Execute with Secrets & Storage

To serve, you additionally specify the secrets and storage file at the command line. Also, you need to pass `--parse-body` to force the body to be parsed.

- Execute the following command.

```bash
wt serve --parse-body wt6.js --secrets-file .secrets --storage-file .storage
```

Now that the server is running, you can send a request. Becuase this task requires a POST with a body, you can use `curl`.

- Open a second terminal window.
- Execute the curl command below.

```bash
curl localhost:8080 -H "content-type: application/json" -d '{
    "action": "opened",
    "repository": {
        "full_name": "Fake Repository"
    },
    "issue": {
        "number": 1,
        "url": "github.com/fake/fake",
        "title": "Fake Issue 1",
        "body": "Fake Body"
    }
}'
```

Check the first terminal; you should see the message "issue created." Also if you check your slack channel, the dummy issue was in fact created!

## Summary

You have just learned how to execute your webtask locally using both **secrets** and **storage**. Local execution is helpful to reduce the feedback cycle while developing. You can work locally and iterate quickly, then publish your webtask when done.

Next, you will learn about a couple different methods for [Local Debugging](local-debugging.md).
