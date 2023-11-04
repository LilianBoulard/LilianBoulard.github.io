---
layout: page
title: prompt-forge
description: An advanced system for industrial-scale prompt generation
img: assets/img/12.jpg
importance: 2
category: fun
---

Having a prompt gallery for txt2img models (i.e. Stable Diffusion) has always pained me: a wordpad on the side, copy-pasting prompt parts is very annoying in the long run.
And on top of that, it doesn't allow for much flexibility: you want a variation with different types of lighting? Well, have to tune it manually, wait for the generation to finish, tune again... it sucks.

Researching on the topic a little bit, I stumbled upon [Dynamic Prompts](https://github.com/adieyal/sd-dynamic-prompts), which has some advantages, but is not exactly fit for what I was trying to accomplish, and lacked a few features I've grown to love from [AUTOMATIC1111's](https://github.com/AUTOMATIC1111/stable-diffusion-webui/) `prompts from file` script, such as setting the resolution, steps or sampler directly from the prompt.

With my initial use case in mind, I started working on this system, which was originally a script which would've generated prompts in a file, then fit for `prompts from file`, but since I'm using a webui hosted on my own cloud, it's quite annoying to have to deal with scripts and files output of the web client. Considering that, I wrote a simple interface for the webui.

In terms of features, as of the time of writing these lines, prompt-forge has its own simple syntax for the candidates (the possible prompts parts), supports setting a range of values in the webui from the configuration without touching the UI (so cool!), and has a couple of modes: random, which generates random prompts from the configuration, and exhaustive, which generates all (and trust me, it grows fast!).

Under the hood, there is a lot of work on the parser, the graph construction, and the probability distributions (which is all customizable!).

Today it serves me well, but I know there are some limitations, especially on the configuration format (using a single file). At first, I thought this was a great idea, especially for versioning, but the more I use it, the more I find the Dynamic Prompts idea of using sub-files for specific properties interesting.
If I ever get limited by that, it seems smarter to implement prompt-forge's features into Dynamic Prompts directly.

Hope you enjoyed the read! [Find the project on GitHub](https://github.com/LilianBoulard/prompt-forge), feedback is very welcome 😃
