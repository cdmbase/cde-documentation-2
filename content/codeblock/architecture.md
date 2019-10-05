---
title: "Architecture Page"
metaTitle: "Syntax Highlighting is the meta title tag for this page"
metaDescription: "This is the meta description for this page"
---

CDECode is an extensible platform to develop full-fledged multi-language Cloud IDE-like products with state-of-the-art web technologies.

# Architecture Overview 
This section describes the overall architecture of CDECode.

CDECode is designed to work as a native desktop application as well as in the context of a browser and a remote server. To support both situations with a single source, CDECode runs in two separate processes. Those processes are called frontend and backend respectively, and they communicate through JSON-RPC messages over WebSockets or REST APIs over HTTP. In the case of Electron, the backend, as well as the frontend, run locally, while in a remote context the backend would run on a remote host.

Both the frontend and backend processes have their dependency injection (DI) container (see below) to which extensions can contribute.

## Frontend 
The frontend process represents the client and renders the UI. In the browser, it simply runs in the rendering loop, while in Electron it runs in an Electron Window, which basically is a browser with additional Electron and Node.js APIs. Therefore, any frontend code may assume browser as a platform but not Node.js.

The startup of the frontend process will first load the DI modules of all contributing extensions before it obtains an instance of `FrontendApplication` and call `start()` on it.

## Backend 
The backend process runs on Node.js. We use express as the HTTP server. It may not use any code that requires a browser (DOM API) as the platform.

The startup of the backend application will first load the DI modules of all contributing extensions before it obtains an instance of `BackendApplication` and calls `start(portNumber)` on it.

By default the backend's express server also serves the code for the frontend.

## Separation by Platform 
In an extension's top folder we have an additional layer of folders to separate by platform:
* The `common` folder contains code that doesn't depend on any runtime.
* The `browser` folder contains code requiring a modern browser as a platform (DOM API).
* The `electron-browser` folder contains frontend code that requires DOM API as well as Electron renderer-process specific APIs.
* The `node` folder contains (backend) code requiring Node.js as a platform.
* The `node-electron` folder contains (backend) code specific for Electron.

## See Also 
For a high level overview of CDECode's Architecture, see this document:
[Multi-Language IDE Implemented in JS - Scope and Architecture] (https://docs.google.com/document/d/1aodR1LJEF_zu7xBis2MjpHRyv7JKJzW7EWI9XRYCt48/edit)

