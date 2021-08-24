---
layout: post
title: "Setting up my Mac for climate research"
date: 2016-08-16
comments: false
categories: software research
---

{{< figure src="wind_art.png" title="Surface wind vectors from a simulation of the ancient Martian climate.">}}

**Updated 2016-10-07** Thanks to [@michaelaye](https://twitter.com/michaelaye) for useful comments and corrections.

I use a Mac computer for most of my climate research, since the Mac OS X operating system provides me computational foundation I need to develop and run planetary climate models. I am not a fanatic follower of Apple and I will use Windows machines when the task demands it, e.g. CAD design on Solidworks or mapping on ArcGIS. For me, a computer OS is just another tool, like Fortran, Python, a spectrometer, or a soldering iron. I have a toolbox and [I put the tools in for the job](http://youtu.be/d8Oe9SteE3M "I'm a weapons man. (Ronin)"). The trick is setting up the tools right.

Since I am using the Unix underpinnings of Mac OS X, my setup requires a number of steps that the average Mac owner does not need in order to be productive. Most of these additional steps involve installing and configuring software for writing my modeling and analysis code. This is essential for my research. The rest of the additional steps are there just to make my life easier.

Once you dive into the Unix engine under the hood, you are no longer working with Mac OS X software installers. Instead, you are often in the realm of package managers, compiling your own code, and customizing the paths and configurations. Not being a computer scientist, I was intimidated at first. Fortunately, a number of people posted their own experiences in setting up their own systems[^1].  Over time I strung together disparate instructions and suggestions that resulted in a working system for me. In the spirit of [paying things forward](http://en.wikipedia.org/wiki/Pay_it_forward "Paying it forward (Wikipedia)"), I am providing this description of my setup[^2] in case it might be useful to another scientist out there facing the same problems that I already faced.


## Taming BASH by using a dotfiles system
I use a number of machines at work and home, with a roughly 50/50 split between Macs and Linux machines. I like to have the Unix environment set up the same on all of the machines. Ideally, this means using a bash shell with a custom prompt, colored ```ls``` output, and all of my standard aliases in place. To manage my environments across multiple computers, both Mac and Linux, I have created a ['dotfiles' system](http://dotfiles.github.io/ "Google does dotfiles") using a simple script and GitHub. This method is based on [Michael Smalley's dotfiles](http://blog.smalleycreative.com/tutorials/using-git-and-github-to-manage-your-dotfiles/) setup. I built on his script and setup to create my own dotfiles system. 

The code in [my repository](https://github.com/soto97/dotfiles "soto97's dotfiles") organizes my various dotfiles, including ```.bashrc```, ```.bash_profile```, ```.vimrc```, and others. The repository is cloned into the home directory of any of my machines such that the path is ```~/dotfiles/```. The ```makesymlinks.sh``` setup script creates symlinks of the dotfiles from the home directory to the files in ```~/dotfiles/```. The setup script is smart enough to back up my existing dotfiles into a ```~/dotfiles_old/``` directory thus giving me a means of reversing any changes. By hosting [the code](https://github.com/soto97/dotfiles "soto97's dotfiles") on GitHub, I can clone and setup this system on any Unix based machine that I work on. The code is "smart enough" to setup environments for different flavors of Unix (Linux or Mac OS X). Right now, my dotfiles only supports a bash environment, but a quick Google search will find other versions for csh, zsh, and other shells.

I use [iTerm2](http://www.iterm2.com/ "iTerm2") for my Mac terminal. After years of fighting with terminal color schemes, I have settled on a scheme created and used by many software engineers: [Solarized](http://ethanschoonover.com/solarized "Solarize"). I am a bit indifferent to the specific colors, but the scheme overall works really well and gives me two consistent and easy to apply colors schemes, one light color scheme and one dark color scheme. Also, the color scheme is available to a number of other programs. For example, I used [Solarized for my vim apps](http://ethanschoonover.com/solarized/vim-colors-solarized "Solarized colorscheme for vim") as well. 

## XCode
Alright, it is time to get started on configuring the Mac for scientific research. First, we need to be sure that we have XCode installed.  XCode provides a number of tools that a scientific programmer will likely not need, but the Command Line tools included in XCode are critical for scientific programming. So, if you don't already have XCode, get it from the App Store.

Once you have installed XCode from the App Store, then you need to install the command line developer tools. Using the command line, enter:

```bash
xcode-select --install
```

This will generate a pop-up message asking to install the command line developer tools. Go ahead and install. Once that is successfully done you will then have a number of command line tools that we will be using throughout the rest of this setup.

**Update**: According to Michale Aye ([@michaelaye](https://twitter.com/michaelaye)) you no longer need the entire XCode software to get the commmand line tools:
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/soto97">@soto97</a> nice blog post for setting up mac for research.Couple of things: 1/n Since 10.9 xcode-select cmd can B run w/out xcode installed.</p>&mdash; Michael Aye (@michaelaye) <a href="https://twitter.com/michaelaye/status/783822768722673664">October 6, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/soto97">@soto97</a> 2/n it will just download the cmd-line tools then. And the link to GrADS is dead.And give the conda-forge channel a try, it has</p>&mdash; Michael Aye (@michaelaye) <a href="https://twitter.com/michaelaye/status/783822974663000064">October 6, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## Install X11
Mac OS X no longer comes with a pre-installed X-Window manager for use with the terminal and command line tools. Therefore, you need to be sure you have X11/XQuartz installed. Visit http://xquartz.macosforge.org/trac/wiki and download and install the most recent version. Just follow the instructions at the XQuartz site. You might need to fix the symlink it makes by entering the following command in the terminal:

```bash
ln -s /opt/X11 /usr/X11
```


## Package Manager: Homebrew
To install and manage many of my tools I use [Homebrew](http://brew.sh "Homebrew"). There are other package managers for OS X, including [MacPorts](http://www.macports.org/) and [Fink](http://www.finkproject.org/), but I have found Homebrew to be the most usable and useful. Needs and preferences will vary. 

### A fresh installation of Homebrew
To install Homebrew from scratch, run the following command:

```bash
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

This will both download and install the Homebrew software. After installing, run 'brew doctor' to insure that everything was installed correctly. If everything is working well, then you can start installing packages. For example, I install HDF5, NetCF, ack, and the Silver Searcher (ag), among others. The [Homebrew website](http://brew.sh "Homebrew") provides details on how to use Homebrew. As well, typing ```man brew``` at the command line will bring up the manual page for Homebrew.

### Updating Homebrew from a previous version of OS X
Since I actually had Homebrew installed for on a previous version of Mac OS X, I took the following steps to make sure everything was still working properly.

I started with the command:

```bash
brew list
```
which told me what packages I have installed. Many of these packages were installed in support of others, but I generally know which ones I intentionally installed. For these, I tried running each package. If the command worked, then I was all set and left things along. If a particular package did not run, then I needed to remove it and reinstall, using the following commands:

```bash
brew remove <package>
```

```bash
brew install <package>
```

These instructions are based on step 5 from [myobie's Gist](https://gist.github.com/myobie/1860902).


### Installing NetCDF Operators (NCO) using homebrew-science
The baseic Homebrew database does not include formulas for all of the scientific software that I need. Instead, we need to use an additional Homebrew database, ['homebrew-science'](https://github.com/Homebrew/homebrew-science). From [homebrew-science](https://github.com/Homebrew/homebrew-science) we have instructions for installing software from this alternative database. First, we need to tell brew to use this alternative database. This is done by 'tapping' the database. The command to do this for homebrew-science is:

```bash
brew tap homebrew/science 
```

Now that homebrew-science is 'tapped' we can start install software from that database. The command is similar to any Homebrew install command:

```bash
brew install <formula>.
```

If the formula conflicts with one from the [master database](https://github.com/Homebrew/homebrew) or another tap, you can install with this version of the install command:

```bash
brew install homebrew/science/<formula>.
```

You can also install via URL:

```bash
brew install https://raw.github.com/Homebrew/homebrew-science/master/<formula>.rb
```

To get the NetCDF Operators, I then entered the following command:

```bash
brew install nco
```

That's it. I now have NetCDF Operators like ```ncks``` and ```nccat``` installed along with NCView (```brew install ncview```) for viewing NetCDF files. Since most climate models output the simulations results as NetCDF files, I am now ready to inspect the climate simulation output of almost any model.

### Installing GrADS using homebrew-science

The [GrADS](http://opengrads.org) tool is useful for plotting climate data and can read in NetCDF files. Though I primarily use Python for plotting, GrADS has its place in my scientific workflow. In order to get GrADS, we will need to access an alternative Homebrew database. Similar to homebrew-science we need to tap homebrew-binary to get GrADS:

```bash
brew tap homebrew/binary
```

I want a copy of GrADS, so I type at the command line

```bash
brew install grads
```

If the formula conflicts with one from mxcl/master or another tap, you can

```bash
brew install homebrew/binary/<formula>
```

You can also install via URL:

```bash
brew install https://raw.github.com/Homebrew/homebrew-binary/master/<formula>.rb
```

Again, that's all there is to it. Now I have a copy of GrADS on my machine.


## Installing the Anaconda Python System
To keep things simple, I use the [Anaconda](https://www.continuum.io) system for installing Python, SciPy, Numpy, Matplotlib, and almost any other Python package[^3]. You can [download](https://www.continuum.io/downloads) either a commmand line or package installer. Once you run the installer, double check that you have a working version of Anaconda by typing:

```bash
conda --version
```

I installed the Anaconda system on top of my previous Python installations. This worked smoothly. The only thing I had to do was fix the PATH and PYTHONPATH variables. The Anaconda installer tried to do this automatically, but failed since I use a dotfiles system. Therefore, I had to manually remove old Python paths in my PATH and my PYTHONPATH. As well, I added the Anaconda path by adding the following line:

```bash
export PATH="/Users/user_name/anaconda/bin:$PATH"
```
Once this was successfully done, I had NumPy, SciPy, Matplotlib, and other packages automatically installed. I need to add a few more packages, like xarray, Cartopy, and netcdf4. This was easily done by typing at the command line:

```bash
conda install <package-name>
```

Cartopy required a slightly different command, since it's part of the SciTools suite:
```bash
conda install <package-name>
conda install -c scitools cartopy
```

Anaconda installed any dependencies and handled any conflicts. In this way, Anaconda is like Homebrew or pip[^4], but for Python. Right now, it looks like any package I might need will likely be available through th Anaconda system.

*One quick note:* I chose to install the complete Anaconda system[^5], which is free. The complete system automatically installs various standard packages, like NumPy, Matplotlib, and Jupyter. You can also chose to install [Miniconda](http://conda.pydata.org/miniconda.html), a strip-down version that does not automatically install packages. With Miniconda you get more control of what packages are installed on your system.



## In conclusion . . .
At this point, my Mac is ready for some scientific heavy lifting. I can compile scientific code, inspect and analyze climate model output, and manage my data with the tools that I have now installed and configured. Of course, this means that the fun has only begun, because it's time to [do some science!](https://twitter.com/search?q=%23doingascience&src=hash)

[^1]: For example, see Hacker Codex's instructions for [configuring Mavericks](http://hackercodex.com/guide/mac-osx-mavericks-10.9-configuration/ "Configuring Mac OS X Mavericks 10.9") and [installing Homebrew Python and other tools](http://hackercodex.com/guide/python-development-environment-on-mac-osx/ "Python Development Environment on Mac OS X"). As well, both [Nathan](https://gist.github.com/myobie/1860902) and [Lowin Data](http://www.lowindata.com/2013/installing-scientific-python-on-mac-os-x/) have good instructions for various configurations. Finally, Damian Irving discuss [installing software for climate research](https://drclimate.wordpress.com/2014/10/30/software-installation-explained/). His [rave reviews](https://drclimate.wordpress.com/2016/04/13/keeping-up-with-continuum/) of [Anaconda](https://www.continuum.io) were the final straw to convince me to switch to using Anaconda to manage my Python environment. 

[^2]: These instructions apply to Mac OS X Yosemite and El Capitan. These instructions may or may not work with earlier versions of Mac OS X.
 
[^3]: I previously used a more [customized method]({% post_url 2014-01-22-setting-up-my-mac-for-scientific-research %}) for installing Python and the various packages. I originally did that because older Python systems like Enthought (now Canopy) did not play well with my Fortran compiled NetCDF and other tools. With the introduction of  [Anaconda](https://www.continuum.io), these previous problems no longer exist. Thus, I have been able to completely streamline my Python software management by using  [Anaconda](https://www.continuum.io). Damian Irving provides a succinct argument for [using Anaconda](https://drclimate.wordpress.com/2016/04/13/keeping-up-with-continuum/). On top of this, I can use Anaconda's environment management to switch between Python 2 and Python 3. This flexibility has allowed me to finally make the switch to Python 3 as my primary version of Python for scientific programming.

[^5]: I've been told that my terminology is a little off when it comes to [Anaconda](https://twitter.com/michaelaye/status/783824037973635072) vs [conda](https://twitter.com/michaelaye/status/783824209474576384). [@michaelaye](https://twitter.com/michaelaye) is right, but it doesn't affect geting things up and running. The instructions that I followed and that I shared here get me a Python installation that works and the flexibility to have multiple environments. Once again,  I have a toolbox and [I put the tools in for the job](http://youtu.be/d8Oe9SteE3M "I'm a weapons man. (Ronin)"). I do not care how the tools are made, as long as they work.

[^4]: pip was installed as part of the  [Anaconda](https://www.continuum.io) installation.


