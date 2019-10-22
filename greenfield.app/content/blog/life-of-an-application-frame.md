+++
date = "2019-10-21T15:49:15+02:00"
title = "The life and performance of an application frame."
tags = ["fps","latency", "roundtrip", "performance"]
categories = ["core"]
description = "About FPS, inputs and round-trips"
draft = false
weight = 30
+++

To understand the life and performance of an application frame inside Greenfield, it's important to understand the difference
between a plain video streaming solution, and the design used by Greenfield.

### A Simple Video Streaming Solution

If we are to define the maximum FPS as: "maximum number of frames an application can send to the compositor without the display pipeline clogging", The pipeline basically looks like this:
<p align="center">
<b>server side</b>  
client application renders frame  
↓  
encode frame to h264  
↓  
network transfer frame  
↓  
<b>browser side</b>  
decode h264 frame  
↓  
display on screen  
</p>

With this setup you should be able to reach 30 to 60 FPS with a full HD (1920x1080) application using a reasonable fast 
desktop PC, depending mostly on the encoding and decoding speed.

To start rendering the next frame we simply need to know what part of these steps is the slowest (in Greenfield that's 
decoding.) and push images through the pipeline at that interval. This will also automatically be your FPS.

However this is quite a useless definition as it tells you nothing about the input reaction time, that is the time it 
takes to update the application based on user input, which is heavily dependent on the network latency. There's also 
the requirement in the Wayland protocol (which Greenfield uses internally), where an application will not start 
rendering the next frame until the compositor explicitly tells the application it's OK to do so. In other words, 
the application will always wait for the compositor before it will start rendering the next frame.

### Greenfield Solution

So if we are to update our maximum FPS definition to  "time it takes from the compositor telling the application to render until the screen is updated", things look a bit different as suddenly there's a full network roundtrip between each frame being displayed
<p align="center">
<b>browser side</b>  
compositor want to render next frame  
↓  
network call  
↓  
<b>server side</b>  
client application renders frame  
↓  
encode frame to h264  
↓  
network transfer frame  
↓  
<b>browser side</b>  
decode h264 frame  
↓  
display on screen  
↓  
compositor want to render next frame  
</p>

The maximum FPS is now defined by the time it takes for a single frame to travel through this pipeline. This is the slowest but also the most reliable way to render frames and is how it's currently implemented in Greenfield. Quick tests on a hard-ware encoding accelerated server with a reasonable fast client, with 30ms round-trip latency gives about 15 FPS on full HD (1920x1080).

If you're smart, you could optimize this pipeline by predicting how long each step will take, and based on that tell the application to start rendering *before* the compositor wants to render the next frame, hoping that by the time the frame reaches the compositor to be displayed, the compositor has just asked for the next frame. 
In the perfect scenario this can bring the FPS count to what we saw in the first pipeline, however this is near impossible as each step varies in time quite significantly based on the size of the next frame and the type of H264 frame (key frame vs delta frame).

So with that said, the best way to currently make the pipeline faster is simply in the encoding step, which is can be  and decoding (6) as the current encoder is entirely done in software (CPU), which can take 15-50ms. Offloading that to a HW encoder (GPU) can bring that down to 15ms.
