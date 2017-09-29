<img src="images/symbol.svg" />
<img src="images/logotype.svg" height="150px" />

# Auth0 Webtask Workshop

Webtask is a platform for easily creating a serverless HTTP Endpoint / Webhook. Webtask executes node.js code in a secure and safe manner. There's no servers to deploy, no tools you have to install, just code. 

* Every Webtask is an HTTP endpoint
* Simple Node.js programming model
* High fidelity of HTTP, Webtasks can even return HTML
* Full access to node modules available on npmjs.com
* Rich browser based editing experience
* Middleware 
* Multi-tenant


## How does it differ from other Serverless offerings?

* Specialized for Node.js
* Low configuration
* Highly customizable
* Extremely low latency
* Designed from scratch for web-based / user interactions rather than plumbing between backend services.
 
## Prerequisites

This workshop assumes you have the following ready to go:

- Familiarity with modern JavaScript 
- A [Github](https://github.com/) account with a public repository
  - Feel free to create a new repository just for this workshop 
- A [Slack](https://slack.com/) account with administrative privileges
  - Feel free to create a new one just for this workshop 
- [Node.js](https://nodejs.org/en/) installed (v6 is perfered, some features currently unstable in v8)
- A Text Editor
  - This workshop shows use of [Visual Studio Code](https://code.visualstudio.com/)

## Getting Started

- Fork the repository.
- Clone your forked repository.
- `npm install -g hads`
- `hads . -o`

Using [hads](https://github.com/sinedied/hads) allows you to navigate around the workshop content, edit it to add your own notes and offers a easy search bar to go back and look something up. 

**Note:** Feel free to edit and send pull requests to update/add more modules.


## Workshop Modules

1. [Hello Webtasks](modules/hello-webtasks.md)
1. [Hello CLI](modules/hello-cli.md)
1. [Programming Models](modules/programming-models.md)
1. [NPM Modules](modules/npm-modules.md)
1. [Secrets](modules/secrets.md)
1. [Storage](modules/storage.md)