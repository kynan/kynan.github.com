---
layout: post
title: Plotting Christmas gift tags using a Raspberry Pi and resin.io
category: posts
---

On the first weekend of December, the fine folks from [resin.io] put on an
[Xmas IoT Hackathon] at the [London Fab Lab]. The Fab Lab is a maker space for
digital fabrication and rapid prototyping. They host classes and events (like
hackathons) and members can use a variety of tools for wood and metal working
as well as 3D printers and a laser cutter, which you can use e.g. to cut a
gingerbread house like this one (made from real gingerbread!):

![Gingerbread house](https://lh5.googleusercontent.com/-jkHO2VE6F-A/VN9LWuFjZCI/AAAAAAAAzAs/teC6cQLf-4E/w636-h477-no/IMG_20141206_170241.jpg)

Appropriate to the occassion, the resin folks brought boxes full of hardware:

![Hardware](https://lh5.googleusercontent.com/-ftbgn7eVD6w/VN9LWsBisZI/AAAAAAAAzAs/Un0M0opMXso/w636-h477-no/IMG_20141206_114759.jpg)

There was therefore no shortage of Raspberry Pis and all kinds of accessoires.
Teams were formed and a range of exciting hacks started taking shape. I teamed
up with [Mark], a good friend and hackathon buddy, who had brought along a
Graphtec Silhouette Portrait cutter/plotter which we were intending to hack:

![Graphtec Silhouette Portrait](https://lh5.googleusercontent.com/-T2KvqyUBock/VN9LWo6hOmI/AAAAAAAAzAs/Qpz08KntBcQ/w636-h477-no/IMG_20141206_130622.jpg)

You'd normally use this to cut out shapes previously created with their
proprietary software Silhouette Studio. Of course that's only the intended
use. Replacing the knife by a silver sharpie turns the Silhouette into an
electronic circuit plotter - for very simple circuits. Here's a
proof-of-concept circuit used to power an LCD with a button cell:

![Circuit](https://lh3.googleusercontent.com/-6e_iEbRmYyM/VN9LWkiLmKI/AAAAAAAAzAs/MHIT7syrkM4/w636-h848-no/IMG_20141206_133959.jpg)

Our plan was to the Silhouette into a Christmas gift tag plotter, powered by a
Raspberry Pi. Using Silhouette Studio to drive the plotter was of course no
option. We had to find a pure command line, open source solution to generate
the template and feed it to the plotter which also needed to run on the ARM
processor of the Pi. After some searching and experimenting we found a Python
based driver, which after [a little tweaking] was able to digest a postscript
file and get it plotted. Here are some experimental results:

![Experiments](https://lh3.googleusercontent.com/-OJp-PtnPV_Y/VN9LWhxeCkI/AAAAAAAAzAs/o-yyfmbTGVM/w636-h477-no/IMG_20141206_200249.jpg)

We generated the gift tags using [paper.js]: The user puts in five names of
friends to create tags for and we place the names, framed each by two
automatically generated snowflakes, on a canvas and have [paper.js] save the
canvas as SVG. Now there were only two pieces missing: one was converting from
svg to postscript, which [inkscape] happily does for us, even on the command
line. The final piece was then a simple [node.js] server to put everything
together: A form to put the names in, which when submitted generates an SVG
string, which is POSTed to the server, where it is saved to a file, converted
to postscript via inkscape and then fed to the plotter. Job done! Well,
almost, we still had to deploy it to the Pi.

Fortunately [resin.io], a deployment service for IoT devices, makes that
rather simple: you create an application on the [resin.io] dashboard, download
the base image for your device, flash it on the SD card, pop it into the Pi
and boot it. After a few minutes the device comes online at the dashboard.
When you then push to the Git repository associated with the project, your new
code is deployed on the Pi, [heroku]-style, but cooler (because of the [blue
unicorn] and cause it run on your device!).

The device is provisioned via [docker] and [resin.io] provide a base container
(based on [Raspbian] wheezy) that comes with node.js preinstalled. Python, git
and inkscape are easily installed via the `apt` package manager and we simply
`git clone` the graphtec driver. The container automatically runs `npm
install` and when successful `npm start` to run our server. So far, so
straightforward, but there were two small issues to figure out: one was
installing the font we were using for the names and refreshing the font cache
and the other was detecting the printer, which required starting `udevd`
manually.  Having that sorted, our first test was finally sucessful:

![Gift tags](https://lh3.googleusercontent.com/-evUBXakjqkA/VJIf2-ifOUI/AAAAAAAAvqE/js-4JPhAVOU/w636-h848-no/IMG_20141218_002904.jpg)

We had a really great weekend at the Fab Lab and even made the 2nd prize with
our hack. Thanks to the [resin.io] folks for putting it on and hopefully until
the next one!

[resin.io]: http://resin.io
[Xmas IoT Hackathon]: http://fablablondon.org/events/xmas-iot-hackathon-with-resin-io/
[London Fab Lab]: http://fablablondon.org
[Mark]: https://twitter.com/M6_D6
[a little tweaking]: https://github.com/kynan/graphtecprint
[paper.js]: http://paperjs.org
[inkscape]: http://inkscape.org
[node.js]: http://nodejs.org
[heroku]: http://heroku.com
[blue unicorn]: http://docs.resin.io/img/screenshots/git_pushed.png
[docker]: http://docker.com
[Raspbian]: http://raspbian.org
