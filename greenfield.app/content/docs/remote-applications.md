+++
description = "Things to know when running remote applications"
title = "Remote Applications"
date = "2019-07-26T11:42:02+0100"
draft = false
weight = 300
bref="Greenfield remote applications are a bit different from existing solutions like VNC or RDP as Greenfield does not stream the entire desktop screen to your browser"
+++

The Greenfield application end-point server live encodes each individual application to an h264 stream which is send to the browser using a dedicated websocket connection. On reception, the h264 stream is decoded and drawn directly into it's own HTML5 canvas. As a result, the screen you see in the browser is actually composed of nothing more than ordinary browser DOM elements.

Native wayland applications can connect to the in-browser compositor by talking to a local application endpoint server. This application endpoint server presents itself as a locally running native wayland compositor while in reality it relays \(nearly\) all messages between the browser compositor & the native application. The in-browser compositor is not limited to handling a single application endpoint. Any number of endpoints can establish a connection. As a consequence **different wayland applications can run on different physical hosts while being connected to the same compositor.**

The in-browser compositor and the local application endpoint server use a direct websocket connection per native application. As such there is no delaying or interfering intermediate party involved. This is made possible by the fact that websocket connections are not bound to the same origin policy, unlike ordinary http connections.

The core Wayland protocol is implemented as well as the _stable_ XDG shell protocol. As such it is possible to run Wayland applications with a compatible widget toolkit.

Supported toolkits are:

* GTK+ 3.22.30 or later \(tested\)
* Qt 5.11 or later \(untested\)
* Any toolkit that supports Wayland using XDG shell or Wl shell protocol.

## Adding & Launching remote applications

When opening Greenfield in the browser, there is a grid icon in the top right corner. Clicking this icon opens the application launch menu. At the bottom of this menu is a round button with a plus icon on it. This button allows to add and associate applications to your account.
When clicking this button a popup appears asking to upload an application link file.

There are several demo application link items already defined in https://github.com/udevbe/greenfield/tree/master/compositor/public/store. An example remote application is https://github.com/udevbe/greenfield/blob/master/compositor/public/store/remote-gtk3-demo/link.json .

This one is configured for the gtk3 demo application but that's not a problem as we can adjust the application end-point server to launch a different binary.

Now on the application endpoint server side we can edit the config file https://github.com/udevbe/greenfield/blob/master/app-endpoint-server/config.json5 . At the bottom you see an "apps" section. This section defines the applications that are allowed to be started by the application endpoint. You can edit the remote-gtk3-demo entry and change the value 
bin: "/usr/bin/gtk3-demo" 
to (for example)
bin: "/usr/bin/gnome-terminal"

That's it. You should now be able to launch a gnome terminal.

If you want to play around, you can also change the id,  name, description & icon if you want. If you start editing, make sure the id of the link.json file matches the id found in "apps" in the config.json5, as this id is how the compositor tells the application endpoint server which application to launch when you press an application icon.



