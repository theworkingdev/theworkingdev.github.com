---
title: Rip youtube videos from the command line
layout: post
categories: linux
permalink: /2012/02/rip-youtube-vids-from-command-line.html
date:   2018-08-01 
---

You can rip YouTube videos from the command line in Linux and OSX. There could be ways to do this in Windows via Cygwin, but I did not try that.

In Debian Linux, or any other distribution that uses the apt or aptitude package manager, you can get the youtube-dl program by pulling them from the repositories. Run the following command on a terminal window

```bash
sudo apt-get update && sudo apt-get install youtube-dl
youtube-dl <youtube vid URL>

```

It's also available on macOS. If you use brew, you can get it with this

```bash
brew update
brew install youtube-dl

youtube-dl <youtube vid URL>
```

I think you can also do this in Windows with Cygwin, but I haven't tried that.
 