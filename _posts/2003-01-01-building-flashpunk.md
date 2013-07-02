---
layout: tutorial
title: Building FlashPunk
category: advanced
---

Although it is possible to compile FlashPunk projects against the FlashPunk library source, it is much simpler to include a precompiled FlashPunk `.swc` file. The most recent stable release of FlashPunk can always be found at [useflashpunk.net][home]; however, it may be helpful to build your own version of the library.

## Contents

<ul class="nav nav-pills nav-stacked">
	<li><a href="#install-exportswc">1. Install ExportSWC</a></li>
	<li><a href="#compile-flashpunk-source">2. Compile FlashPunk Source</a></li>
</ul>

<h2 id="install-exportswc">Install ExportSWC</h2>

<p class="alert alert-info">This tutorial is intended for use with FlashDevelop. It is possible to compile FlashPunk without FlashDevelop, but this guide does not cover those cases.</p>

To simplify the compilation process, you'll be using the FlashDevelop plugin, [ExportSWC][]. Once you've downloaded it from the project page on SourceForge, you can open the .fdz and FlashDevelop should recognize it and ask you to install it. If not, open FlashDevelop and drop the .fdz onto an open window. Once you've installed it, you'll need to restart FlashDevelop.

You should see a new icon on your toolbar with the tooltip `Build SWC/MXI`. If you do, you're ready to compile.

<h2 id="compile-flashpunk-source">Compile FlashPunk Source</h2>

At this point, you'll need to create a new FlashDevelop project using the ActionScript 3 project template "Blank Project." Save this project wherever you'd like, but I'd recommend it be somewhere that you'll remember.

Next, you'll want to copy the entire `/net` folder from whatever fork of FlashPunk you'd like to compile against into the new blank project's root. Viewing the project in the project browser in FlashDevelop should result in a tree that looks something like the following:

    <Project Name>
    - net
      |
      - flashpunk
        |
        + debug
        + graphics
        + masks
        ...
        | Tweener.as
        | World.as

Once the source is in place, click the drop down on the ExportSWC icon and select __Configure__. The release build of FlashPunk only has "Integrate ASDoc" checked, but feel free to set whatever settings you'd like. Make note of your output location, or change it if you want and save your configuration.

Finally, click the ExportSWC button. After a bit of work, assuming you used the default output location, you'll find the compiled `.swc` at `.\bin\<Project Name>.swc`. If you made any changes that caused the build to fail, you should fix them and compile again.

[home]: http://useflashpunk.net/
[exportswc]: http://sourceforge.net/projects/exportswc/
