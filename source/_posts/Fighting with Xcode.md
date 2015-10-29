# Fighting with Xcode
title:  Fighting with Xcode
date: 2015-10-28 18:00:00
tags:
- Xcode

---

Xcode is hard to work with, especially for those guys come from Visual Studio, [IntelliJ](https://www.jetbrains.com/idea/) or [Eclipse](https://eclipse.org/). Try out the following tips to make your life happier with Xcode.

> **Note**: If you can write an iOS app via [vim](http://www.vim.org/), please skip this post;)

<!--more-->


----------


##  Reveal File in Project Navigator

![Reveal File in Project Navigator](http://i.imgur.com/UJ6SNs8.png)


----------


## Copy Full File Path
Well, sometimes we need the full path to check out a file in [Perforce](https://www.perforce.com/). We can also achieve this goal by dragging the file into the terminal.
![Copy Full File Path](http://i.imgur.com/B5EZhFW.png)


----------


## Eliminate Trailing Spaces
For a better coding styles.
![Eliminate Trailing Spaces](http://i.imgur.com/m56NS5g.png)


----------


## Replace Tabs with Spaces
Don't let TABs ruin your life...
![Replace Tabs with Spaces](http://i.imgur.com/UvNfRVY.png)


----------


## Rulers

![Rulers for 100 characters](http://i.imgur.com/otchG7P.png)


----------


## Plugins
**Alcatraz** is an open-source package manager for Xcode. It let us discover and install plugins, templates and color schemes without the need for manually cloning or copying files.

Check out **Alcatraz** [here](http://alcatraz.io/)!

To install **Alcatraz**:
``` bash
curl -fsSL https://raw.githubusercontent.com/supermarin/Alcatraz/deploy/Scripts/install.sh | sh
```

To delete **Alcatraz**:
``` bash
rm -rf ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins/Alcatraz.xcplugin
```
To remove all cached data:
``` bash
rm -rf ~/Library/Application\ Support/Alcatraz
```
To open **Alcatraz** in Xcode:


![Open Alcatraz in Xcode](http://i.imgur.com/Ke7XG9m.png)


### Highlight Selected Strings

Pick up the [HighlightSelectedString](https://github.com/keepyounger/HighlightSelectedString) plugin in Alcatraz.




### Highlight Current Line

Pick up the [Backlight-for-XCode](https://github.com/limejelly/Backlight-for-XCode) plugin in Alcatraz.




### Bookmarks

Pick up the [XcodeBookmark](https://github.com/nicoster/XcodeBookmark) plugin in Alcatraz.


----------


@(Learning Cards)[Marxico|Legacy Tools]