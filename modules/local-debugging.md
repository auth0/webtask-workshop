# Local Debugging

In previous modules, you saw how you can locally serve your task, providing secrets, storage and 3rd party modules. Wouldn't it be great if you could step through debug? It turns out you can, even using breakpoints and watches. You'll see how now!

In addition to the `wt serve` command, there is the `wt debug` command.

## Debugging with Devtool

The simplest and quickest way to debug is using the stand alone Electron-based debugger Devtool. Devtool is great because you can install it right from npm. Let's see how you can use it to debug the task. 

- Execute the command, `npm install -g devtool`.
- Execute the command below.
 
```bash
wt debug -d=devtool --parse-body wt6.js --secrets-file .secrets --storage-file .storage
```

Devtool will popup. Once it opens, you can drill down into the file explorer on the left to find your task. 

- Click on the **Sources** tab.
- In the left hand treeview, locate **wt6.js**.
- Click the **wt6.js** file.
- In the main editor view, set a breakpoint by clicking in the margin near line 6.

![Set Breakpoint](https://cloud.githubusercontent.com/assets/141124/26760129/f7145520-48c5-11e7-8e19-0928df592264.gif)

Now that your breakpoint is set, you can test it out.

Run the previous curl command that you ran before:

```bash
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

![Debugging with Devtool](https://cloud.githubusercontent.com/assets/141124/26760204/70ca74b6-48c7-11e7-8684-92a389cd6794.gif)

Go stop the task from being served and Devtool will automatically close. 

## Debugging with Visual Studio Code

If you run `wt debug` without specifying `-d`, then wt-cli will launch standard node debugging allowing you to attach with an IDE like VS Code or Webstorm.

Let's use Visual Studio Code to debug wt6.

- Execute the following command.

```bash
wt debug --parse-body wt6.js --secrets-file .secrets --storage-file .storage
```
- Open Visual Studio Code.
- Open the folder where your task lives within Code.
- Open **wt6.js** and add a breakpoint at line 6.
- Press the **Debug** button.
- Select **Add Configuration** via the Drop Down.
- Select **Node.js: Attach**.
- Ensure your `launch.json` looks like the example below.
- Save the `launch.json`.
- In the Dropdown, select **Attach**
- Press the **Play** button.


```json
{
    "version": "0.2.0",
    "configurations": [
    {
        "type": "node",
        "request": "attach",
        "name": "Attach",
        "port": 9229
    }
    ]
}
```
Repeat the previous curl from the terminal. You will see your breakpoint is hit and you can debug!

![Debugging in Visual Studio Code](https://cloud.githubusercontent.com/assets/141124/26760389/94c0d3ac-48cb-11e7-9a03-4e89241e2be1.gif)

## Summary

You have just learned two methods of debugging your webtask code. Now you an track down those issues that popup that make no sense, but stepping through execution of your webtask line by line. Very helpfull, indeed!

Next, you will learn about securing your webtask using [Middleware](middleware.md).