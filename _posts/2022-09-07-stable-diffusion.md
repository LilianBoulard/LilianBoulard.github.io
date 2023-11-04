---
layout: post
title: Self-hosting Stable Diffusion on Windows
date: 2022-09-07 17:13:00-0100
description: This very short guide will show you how to run Stable Diffusion on Windows, and create images easily with a command-line interface.
tags: stable-diffusion self-hosting windows github
categories: 
related_posts: false
---

> This blog post was written at a time before AUTOMATIC1111's webui. Today, it is a much better choice.
> However, there are some use cases for using Stable Diffusion this way, so this post stays 😃
{: .block-warning }

This very short guide will show you how to run [Stable Diffusion](https://stability.ai/blog/stable-diffusion-announcement) on Windows, and create images easily with a command-line interface.

> The hardware specification provided by Stability AI still apply:
> as of writing these lines, that means you’ll need a graphics card with at least 8GB of VRAM.
{: .block-tip }

## Spin up the docker image

First, install [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/).

Next, check the [Replicate page](https://replicate.com/stability-ai/stable-diffusion) and use the command indicated in *Run on your own computer* to run a docker container with Stable Diffusion.

For reference, as of writing these lines, the command is

```
docker run -d -p 5000:5000 --gpus=all r8.im/stability-ai/stable-diffusion@sha256:a9758cbfbd5f3c2094457d996681af52552901775aa2d6dd0b17fd15df959bef
```

## Use with the Python script to generate images

I’ve written a simple Python script to interface with the horrible Windows commands.

To use it, install Python, and copy [the script](https://github.com/LilianBoulard/Stable-Diffusion-Windows/blob/main/stable_diffusion.py) to your local machine.

Next, install the script requirements:

```
python -m pip install requests
```

Finally, run the script with the desired arguments:

```
python stable_diffusion.py -h
```

Example of a prompt:

```
python stable_diffusion.py --prompt "a cat on a car"
```

<div class="row">
    <div class="col-sm-1 mt-5 mt-md-0"></div>
    <div class="col-sm-4 mt-5 mt-md-0">
        {% include figure.html path="assets/img/0-acatonacar.png" title="a cat on a car" class="img-fluid" %}
    </div>
    <div class="col-sm-1 mt-5 mt-md-0"></div>
</div>

## Bonus: disable NSFW filter

> This is specific to version 1.4. This might not work for future/previous versions.
{: .block-tip }

The NSFW filter will turn the image to solid black and display a warning if it is triggered.

In order to disable this behavior, open a prompt in the container

```
docker exec -it <container_name> /bin/sh
```

Next, install a text editor, such as `nano`

```
apt update && apt install nano
```

use it to modify the file `/src/predict.py`

```
nano /src/predict.py
```

Inside nano, use `Ctrl + W` to search for `output = self.pipe`.

Right above, you’ll want to add these lines: 

```python
def dummy_checker(images, *args, **kwargs):
    return images, [False] * len(images)
self.pipe.safety_checker = dummy_checker
```

You might also want to stop the safety model from even loading, which will save some VRAM (not tackled here).

Restart the container, and you should be good to go !

# Final thoughts

After testing thoroughly Dall-E 2, Midjourney and Stable Diffusion, here are my thoughts:

- Dall-E and Stable Diffusion offer similar style of results, though Dall-E tends to be more in-line with what is being prompted from a standard user perspective (which is to be expected given that OpenAI is state of the art).
- Stable Diffusion has the big advantage of being free if you omit the fact that you need a 8GB+ graphics card (and ignore the electricity bill, that is). Self-hosted is very cool nonetheless. 
However, its results are not very impressive to me, compared to the mind-blowing results of Midjourney.
  - Update: the [Two Minute Papers video](https://youtu.be/nVhmFski3vg) really shows what can be done if you really dive into the code (which might not be optimal with the setup described in this post), and it’s absolutely mind-blowing !
    From a researcher / artist perspective, this is an absolutely amazing tool !
- [Midjourney is extremely good at what it does](https://www.midjourney.com/showcase/), even more since version 4 (which at the time of writing this is in test phase).

So, if you have a good graphics card, try Stable Diffusion, it’s self-hosted, it doesn’t have quotas, you can do extremely advanced things with it: it’s very cool!

You might also want to try experimenting with Dall-E 2 (though you’ll need to be patient, given that access is controlled by a waitlist).

Finally, my personal favorite is Midjourney, which you can try right now with 25 free images. Even after using these, the lowest tier is currently at 10 bucks for 200 GPU minutes, which is pretty cheap.

If you want to use Midjourney, I'd also recommend the [Midjourney prompt builder](https://promptomania.com/midjourney-prompt-builder/)!

# References

[GitHub - LilianBoulard/Stable-Diffusion-Windows: Interface for self-hosting Stable Diffusion on Windows](https://github.com/LilianBoulard/Stable-Diffusion-Windows)

[Replicate](https://replicate.com/stability-ai/stable-diffusion)

[Disable Hugging Face NSFW filter in three step](https://www.reddit.com/r/StableDiffusion/comments/wxba44/disable_hugging_face_nsfw_filter_in_three_step/)

[promptoMANIA:: Midjourney prompt builder](https://promptomania.com/midjourney-prompt-builder/)

[Stable Diffusion: DALL-E 2 For Free, For Everyone!](https://youtu.be/nVhmFski3vg)
