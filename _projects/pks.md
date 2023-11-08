---
layout: page
title: PKS
description: A dynamic port-knocking system, for securing access to servers
importance: 3
category: fun
---

During my Associate's Degree, in a cybersecurity course, I learned about [port-knocking](https://en.wikipedia.org/wiki/Port_knocking), a rather simple idea for securing servers: knock in a predefined pattern, and you're allowed in.
In practice, the server defines a sequence for securing a resource (e.g. the default SSH port, 22), which is shared with authorized clients, and when one of those wants to connect, it will send packets to the server ports following the sequence.
The server constantly listens, and upon detecting a successful attempt, opens up the resource. It then closes it again and the process can repeat.

This type of authentication is called *security by obscurity*. On its own, it's not enough to secure a system, but it has its advantages, notably to avoid port scans.

A major issue immediately evident is its vulnerability to replay attacks: an attacker can simply listen for the hard-coded sequence, and re-use it to open the sensitive resource.

As a fun side-project, I decided to work on a solution of my own, without really looking at already-existing ones.
The obvious idea was to not have a hard-coded sequence, but instead generate a new -- random -- one at each connection.

So that's what I ended up doing: in essence, the program is really simple, it builds on top of [knockd](https://linux.die.net/man/1/knockd) and simply rewrites the configuration with a random sequence on specific events.
At the time of writing this script, I was also very much in love with the [Telegram API](https://core.telegram.org/api), so I ended up opting for a this solution using a bot: the sequence is transmitted to a discussion group, and I added a few commands to make right management simpler: authorized users can generate a new sequence, start/stop the knockd server, etc.

Currently, it's in a stable state, and can *technically* be used in production, although again, this was a project I worked on for fun, so be warned it's not very polished!
