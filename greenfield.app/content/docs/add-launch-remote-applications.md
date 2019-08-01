+++
description = "How to add and launch an application"
title = "Application Walkthrough"
date = "2019-07-26T11:42:02+0100"
draft = false
weight = 1
bref="This simple walkthrough explains how you can link, run and edit applications to your account"
+++

To associate an application to your account, we need to tell Greenfield about the application type, location and other necessary information. This information is grouped in what's called an "application link file".
There are several demo application link files already defined in the [repository](https://github.com/udevbe/greenfield/tree/master/compositor/public/store).

Let's go over the remote gtk3-demo-application link [file]((https://github.com/udevbe/greenfield/blob/master/compositor/public/store/remote-gtk3-demo/link.json)).
This one is configured for gtk3-demo-application but that's not a problem as we can cheat the application endpoint server into launching a different binary.
```json
{
  "id": "remote-gtk3-demo",
  "title": "Remote GTK3 Demo",
  "icon": "http://localhost:8080/store/remote-gtk3-demo/icon.svg",
  "type": "remote",
  "url": "ws://localhost:8081"
}
```
Les's go over it:

- `id`: A unique application id. This can be anything but has to be unique for the remote application end-point defined in `url`.
- `title`: A human readable title of the application. This text will be displayed in the application launch menu.
- `icon`: The application icon. This icon will be displayed above the `title` in the application launch menu.
- `type`: The type of application. Currently only "remote" and "web" are valid.
- `url`: The remote websocket url where the application end-point server can be reached. Only "ws" or "wss" protocol is valid.

To link an application to your account we need to upload this information. This can be done in the application launch menu.

- Press the grid icon in the top right corner.
- Press the round button with a plus icon on it.
- A popup appears asking you to upload an application link file.
- Select the gtk3-demo-application link file and upload it.

An application icon has now appeared in your application launch menu. Clicking this icon will make Greenfield connect
to the url defined in the link file. This url must be a websocket url, and Greenfield expects an application end-point server to
be listening at the other side. Let's have a look at the application end-point server.

On the application endpoint server side, applications are currently configured in the main config [file](https://github.com/udevbe/greenfield/blob/master/app-endpoint-server/config.json5).
```json5
{
    // List of apps that can be started from the browser, based on the 'appId' send
    // from the browser. The 'appId' is then matched with a key value in 'apps'.
    apps: {
      'remote-gtk3-demo': {
        // The full path of the executable binary. Can also be a symlink.
        bin: "/usr/bin/gtk3-demo",
        // Arguments passed to the binary
        args: [],
      }
    }
}
```
At the bottom of the main config file, we see an "apps" section (shown above). This section defines the applications that are allowed 
to be started by the application endpoint. Here the `id` from the application link file is used to find the matching entry "remote-gtk3-demo".
We can edit the remote-gtk3-demo entry and change the value

bin: "/usr/bin/gtk3-demo" 

to (for example)

bin: "/usr/bin/gnome-terminal"

That's it. You should now be able to launch any binary.

If you want to play around, you can also change the id, name, description & icon of the link file.