---
layout: page
title: prompt-forge
description: An advanced system for industrial-scale prompt generation
importance: 2
category: fun
---

Having a prompt gallery for txt2img models (i.e. Stable Diffusion) has always pained me: having a wordpad on the side and copy-pasting parts is *very* annoying in the long run.
And on top of that, it doesn't allow for much flexibility: do you want a variation with different types of lighting? Well, you have to tune it manually, wait for the generation to finish, tune again... it sucks.

Searching for existing solutions, I stumbled upon [Dynamic Prompts](https://github.com/adieyal/sd-dynamic-prompts), which has a good presence, but is not exactly fit for what I was trying to accomplish, and lacked a few features I've grown to love from [AUTOMATIC1111's](https://github.com/AUTOMATIC1111/stable-diffusion-webui/) `prompts from file` script, such as setting the resolution, inference steps or sampler directly from the prompt.

With my initial use case in mind, I started working on this system, which was originally a script that would've generated prompts in a file, which could then be fed to `prompts from file`.
However, since I'm using a webui hosted on [my own cloud](https://lilian.boulard.fr/blog/2023/my-cloud/), it's quite annoying to have to deal with files outside the webapp. So I wrote a script for the webui, which seemlessly integrates prompt-forge.

In terms of features, as of the time of writing these lines, prompt-forge has its own simple syntax for the candidates (the possible prompts parts), supports setting a range of webui settings from the config file without actually touching the UI (so cool!), and has a couple of modes: random, which generates random prompts from the configuration, and exhaustive, which generates them all (and trust me, it grows fast!).

Under the hood, there has been a lot of work on the parser, the graph construction, and the probability distributions (which is all customizable!).

Today it serves me well, but I notice some limitations, especially regarding the configuration format (i.e. using a single file).
At first, I thought this was a great idea, especially for versioning, but the more I use it, the more I find the Dynamic Prompts idea of using sub-files for specific properties interesting.
If I ever get limited by that, it seems smarter to implement prompt-forge's features into Dynamic Prompts directly.

Hope you enjoyed the read! Find the project [on GitHub](https://github.com/LilianBoulard/prompt-forge), feedback is very welcome 😃
