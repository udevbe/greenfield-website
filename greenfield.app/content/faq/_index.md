+++
draft= false
title = "FAQ"
description = "Thou ask and thou shalt receive"
+++

## What is Greenfield?
Greenfield is in a essence an fully fledged Linux Wayland compositor running entirely in a user's browser. It can use WebSockets to interface
with remotely running applications, or it can use WebWorkers to run applications directly in a user's browser.

## How is Greenfield different compared to existing solutions like VNC or RDP?
See [about remote applications](http://localhost:1313/docs/remote-applications/).

## What are the current limitations of Greenfield?
- Greenfield currently only supports Linux Wayland applications, as well as rudimentary JavaScript web applications running in 
the user's browser. See [here](http://localhost:1313/docs/web-applications/) for more information about web applications.

- Sound is currently not supported for remote applications. The main reason being that it's low priority and as such it's simply not implemented yet.

- Remote gaming is possible in theory, however there has been no effort made to optimize gaming scenarios.

## What are the future possibilities of Greenfield?
- A complete high level widget toolkit to develop applications running entirely in a user's browser, with an accompanying web app store to distribute them.
In fact, there's already a [widget toolkit](https://flutter.dev/web) that's ticking all the checkboxes.

- Full remote Linux XWayland support is also on the roadmap. This should provide stable access from your browser to over 99% of all available Linux applications.

- Another options is... Windows application support! Although not currently on the roadmap, it is a technical possibility.

- As for Mac, who knows perhaps one day?


## What Does the Future Hold for Greenfield? What’s the plan?
The goal is provide a user with all their data and applications, without being locked to any platform, device or location.

## Can I run Greenfield privately/self-hosted?
You can! The source is available [here](https://github.com/udevbe/greenfield). Be warned though, it's still very much
BETA.

## What license does Greenfield use?
Greenfield is licensed under [AGPL-3.0](https://opensource.org/licenses/AGPL-3.0). We realise this is quite a restrictive
license, and as such future stable releases will include a commercial license
for use with closed source environments.