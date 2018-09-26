# SHNOPs
Process, analyze, and visualize complex data sets with Simple Houdini Number OPerators.

![Logo](https://github.com/profbetis/SHNOPs/blob/meta/shnops_banner.svg)

## The Gist
There are lots of programs out there that can take in data sets and spit out some charts. Some look prettier than others, and some have neat tricks. What they usually don't have is the real power to wrangle your data into stunning and enlightening visualizations.

SHNOPs is built to work completely inside SideFX's program Houdini which offers powerful and exposed abstract data concepts on its geometry. Many of its native tools are parallelized which leads to fast processing of large data sets (for those familiar, almost every tool is built in VEX and not Python!)

## The Git
Because SHNOPs is hosted publicly on GitHub, the community is free to view, suggest, and help build a better toolset.

## Installing
If you've installed Houdini tools before, you're probably tired of seeing this, but I'll put it here for good measure.
1. **Go** to the folder where you want SHNOPs to live. I personally recommend a general tool folder for all your Houdini tools.
2. **Clone** or copy the repo. 
    - If you just want to get up and running and don't have Git installed, just download [the latest zip](https://github.com/profbetis/SHNOPs/archive/master.zip) and unzip it here.
    - If you want to use git, copy the HTTPS link (https://github.com/profbetis/SHNOPs.git) or the SSH one (git@github.com:profbetis/SHNOPs.git), and `git clone` it here.
3. **Edit** your Houdini environment file. We're going to use this file to tell Houdini to automatically look for all the OTLs in the SHNOPs folder so that any updates will be automatic (on program startup).
    - On Windows, this file is by default found at `C:\Users\<Username>\Documents\HoudiniX.X\houdini.env` (X.X being the version of Houdini, like 16.5, without the build number)
    - For Macs, it's found at `~/Library/Preferences/houdini/X.X/houdini.env`
    - Linux, `~/houdiniX.X/houdini.env`. (but if you're using Houdini on a Linux machine, you probably already knew that)
    1. Open this file with your text editor of choice.
    2. Pick a line above `HOUDINI_OTLSCAN_PATH` and add the line `SHNOPS = "/where/you/put/SHNOPs"`, obviously replacing that with the actual location of the SHNOPs folder.
    3. In the previously mentioned `HOUDINI_OTLSCAN_PATH` line, insert/append `$SHNOPS/otls` appropriately.

## Getting Started
Assuming everything went smoothly with the installation, we should be ready to wrangle some data sets.
1. **Create** a geometry object in good ol' `/obj/`. I typically go with the Geo object, but some extremists go with the Box. SHNOPs are SOPs, so they will always live inside of a geometry object, instead of it being one itself.
2. **Make** a _SHNOPs Generate Data_ node. Unless you have a CSV file on hand, this is probably the quickest way to get started.
    1. Add a field and name it whatever you like (`val1` is a pretty strong name though, don't stray too far)
    2. Check the Geometry Spreadsheet and **_BAM_** you should already have a hundred data points with random values from 0 to 1. I know, it's so exciting and we're just getting started.
3. **Plop down** a _SHNOPs Transform_ node. While it should be illegal to manipulate data sets, if I don't provide the tools, someone else will.
    1. Click the little dropdown triangle for the `Field` parameter. It should auto-populate with any acceptable attribute that can be used by the node, but select our trusty `val1` (Unless you thought you came up with a better name?? The gall...)
    2. Head over to the `Remap` tab and change the fitting to "Polar Normalize". This will remap our field's values to be between -1 and 1 instead. Nifty!
4. **Craft** a _SHNOPs Integrate_ node and append it to the tree. Look at that handiwork! This node is an Analysis node. Analysis nodes compute new data from existing fields.
    1. Again, select your field of choice from the dropdown list, or if you like feeling like a computer genius, type it in yourself.
    2. If you check the Geometry Spreadsheet again (which honestly you should just keep open for the forseeable future) you'll see there's a new field with the name of your old one plus a little something extra. Your values should resemble something of a noise function. That should be the integral of that field.
    3. For a bunch of random numbers, this is pretty useless (although interesting). In a real-life scenario, this tool would be good for accumulating a bunch of transaction values and seeing how your finances are getting worse and worse over time. Thanks SHNOPs!
5. **Spin up** a little standard _Enumerate_ node. "What, no SHNOPs node?" Nope, no need! Houdini's native nodes work seamlessly with SHNOPs nodes. We'll be using this to provide some sort of context between data points (in the geometry spreadsheet, it's easy to see that they're sequential but to SHNOPs there's nothing to say what order they're in).
    1. Change the group type to `Points` (SHNOPs only deals with points)
6. **Conjure** the final node in the tutorial, the _SHNOPs Euclidian Plot_ node. Yeah it's a scary name but guess what it's actually extremely familiar if you've ever used a 3D program. The plotting nodes take your data and put them into space, and even get back some of that delicious geometry we haven't seen in so so long.
    1. In the `X Axis` tab, load the field up with `index` (or whatever your little deviant mind decided to call it) from the Enumerate node. You should notice your data points have a position now! But only on one axis... not to fear! The next step will surely assuage your unease.
    2. Head to the `Y Axis` tab. Select our integrated field from the drop-down.
    3. **_BAM_** (this ain't a firing range!) a cool noisy curve should be present in your viewport. Because the plot nodes come initialized with normalization enabled, you shouldn't have to do any globe-trotting to find your data.
7. **Extra Credit**. If you wanna see something cool, go back to the Generate Data node, and drag the `Data Points` slider. It's logarithmic so be gentle!

**_Phew!!_** That was a lot. Thanks for sticking with me! This tutorial was more to get a sense of how the nodes work together, rather than a real-life scenario. For that, check out the Examples inside!
