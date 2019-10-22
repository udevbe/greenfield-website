+++
description = "Setup a test environment with Docker."
title = "Quick-start Guide"
date = "2019-07-26T11:42:02+0100"
draft = false
weight = 1
bref="Quickly get started using a local environment in Docker"
+++

In a Linux terminal run:

- `git clone https://github.com/udevbe/greenfield.git`
- `cd greenfield/environments/local`
- `docker-compose up`

This will start 3 containers.

- An app-endpoint-server, has the gtk3-demo-application as launchable application.
- A dummy X server, required by the gstreamer encoder from the app-endpoint-server to run OpengGL commands. Not used for anything else.
- An nginx server, has ssl termination and uses a self-signed localhost certificate so a secure websocket connection can be set up.

Your browser will, by default, reject the secure websocket connection as it uses a self-signed certificate. 
You can however force your browser to accept the certificate.

- In Firefox, go to https://localhost and simply follow the dialogue and accept the certificate. You should now get a `502 bad gateway` which means
your browser can communicate. This is fine as the app-endpoint-server only handles websocket requests, hence you get a `5xx error`.
Simply close the tab, the certificate has now been permanently accepted.
- In Chrome there is no dialogue button. Go to `chrome://flags/#allow-insecure-localhost` and enable `Allow invalid certificates for resources loaded from localhost.`

To add an application to your account:

- Go to https://preview.greenfield.app 
- Click the top right raster icon. 
- Click the + icon. 
- Click the cloud icon and select the [remote-gtk3-demo](https://github.com/udevbe/greenfield/blob/master/compositor/public/store/remote-gtk3-demo/link.json)
link file.
