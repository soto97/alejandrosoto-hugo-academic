---
layout: post
title: "Setting up my Mac for scientific research"
date: 2014-01-22
comments: false
categories: ["software", "research"]
---

{{< figure src="/images/light_dots.png" >}}

*UPDATE: I have changed how I [setup my Python environment]({{< relref "../setting-up-my-mac-for-climate-research/index.md" >}}). These instructions are no longer up to date and may not work on newer versions of the Mac OS.*

I use a Mac computer for most of my research, with the exception of running large climate models, which is usually done on clusters that are built from Linux machines. For most of my work, the Mac OS X operating system provides me computational foundation I need to develop and run planetary climate models. I am not a fanatic follower of Apple and I will use Windows machines when the task demands it, e.g. CAD design on Solidworks or mapping on ArcGIS. For me, a computer OS is just another tool, like Fortran, Python, a spectrometer, or a soldering iron. I have a toolbox and [I put the tools in for the job](http://youtu.be/d8Oe9SteE3M "I'm a weapons man. (Ronin)"). The trick is setting up the tools right.

Since I am using the Unix underpinnings of Mac OS X, my setup requires a number of steps that the average Mac owner does not need in order to be productive. Most of these additional steps involve installing and configuring software for writing my modeling and analysis code. This is essential for my research. The rest of the additional steps are there just to make my life easier.

Once you dive into the Unix engine under the hood, you are no longer working with Mac OS X software installers. Instead, you are often in the realm of package managers, compiling your own code, and customizing the paths and configurations. Not being a computer scientist, I was intimidated at first. Fortunately, a number of people posted their own experiences in setting up their own systems[^1].  Over time I strung together disparate instructions and suggestions that resulted in a working system for me. In the spirit of [paying things forward](http://en.wikipedia.org/wiki/Pay_it_forward "Paying it forward (Wikipedia)"), I am providing this description of my setup[^2] in case it might be useful to another scientist out there facing the same problems that I already faced.

<!-- more -->

## Wrangling BASH preferences: my new dotfiles system
I use a number of machines at work and home, with a roughly 50/50 split between Macs and Linux machines. I like to have the Unix environment set up the same on all of the machines. Ideally, this means using a bash shell with a custom prompt, colored ls output, and all of my standard aliases in place. Historically, I have gone through a tedious process with every new machine in which I manually recreate the set up that I already have on my other machines. With my most recent new computer, I have adopted a more systematic and automated way of maintaining syncing my environments on different machines: I have created a ['dotfiles' system](http://dotfiles.github.io/ "Google does dotfiles") using a simple script and GitHub.

This method is based on Michael Smalley's dotfiles setup, which he described at his blog. I built on his script and setup to create my own dotfiles system. The code in [my repository](https://github.com/soto97/dotfiles "soto97's dotfiles") organizes my various dotfiles, including ```.bashrc```, ```.bash_profile```, ```.vimrc```, and others. The repository is cloned into the home directory of any of my machines such that the path is ```~/dotfiles/```. The ```makesymlinks.sh``` setup script creates symlinks of the dotfiles from the home directory to the files in ```~/dotfiles/```. The setup script is smart enough to back up my existing dotfiles into a ```~/dotfiles_old/``` directory thus giving me a means of reversing any changes. By hosting [the code](https://github.com/soto97/dotfiles "soto97's dotfiles") on GitHub, I can clone and setup this system on any Unix based machine that I work on. Right now, the files are designed to be universal, but eventually I will add some smarts to the system so that I can have some customizations setup for different flavors of Unix (Linux or Mac OS X) and possibly for different shells (csh, tcsh, zsh, etc.).

## iTerm and Solarized
I use [iTerm2](http://www.iterm2.com/ "iTerm2") for my Mac terminal. After years of fighting with terminal color schemes, I have settled on a scheme created and used by many software engineers: [Solarized](http://ethanschoonover.com/solarized "Solarize"). I am a bit indifferent to the specific colors, but the scheme overall works really well and gives me two consistent and easy to apply colors schemes, one light color scheme and one dark color scheme. Also, the color scheme is available to a number of other programs. For example, I used [Solarized for my vim apps](http://ethanschoonover.com/solarized/vim-colors-solarized "Solarized colorscheme for vim") as well. 

## XCode
Alright, it is time to get started on configuring Mac Mavericks for scientific research. First, we need to be sure that we have XCode installed.  XCode provides a number of tools that a scientific programmer will likely not need, but the Command Line tools included in XCode are critical for scientific programming. So, if you don't already have XCode, get it from the App Store.

Once you have installed XCode from the App Store, then you need to install the command line developer tools. Using the command line, enter:

```bash
xcode-select --install
```

This will generate a pop-up message asking to install the command line developer tools. Go ahead and install. Once that is successfully done you will then have a number of command line tools that we will be using throughout the rest of this setup.


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
Since I actually had Homebrew installed for Mountain Lion before my upgrade to Mavericks, I took the following steps to make sure everything was still working properly.


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
brew install cdo
```

That's it. I now have NetCDF Operators like ncks and nccat installed along with NCView for viewing NetCDF files. Since most climate models output the simulations results as NetCDF files, I am not ready to inspect the climate simulation output of almost any moedl.

### Installing GrADS using homebrew-science

The [GrADS](http://www.iges.org/grads/) tool is useful for plotting climate data and can read in NetCDF files. Though I primarily use Python for plotting, GrADS has its place in my scientific workflow. In order to get GrADS, we will need to access an alternative Homebrew database. Similar to homebrew-science we need to tap homebrew-binary to get GrADS:

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


### Installing Homebrew Python[^4] 
Plenty of arguments are given on the web for not merely using Apple's installation of Python (e.g.  [Hacker Codex's discussion](http://hackercodex.com/guide/python-development-environment-on-mac-osx/ )), but for me, it's mainly because I want all of my packages to play well together within Homebrew.

Getting Python 2.7.6 installed is pretty straightforward. The command is:

```bash
brew install python --with-brewed-openssl
```

This will also install package management tools like pip, which we'll need later. For my scientific work, I only need Python 2.7.x. Most scientific and mathematical packages have not yet moved to Python 3.x.

## Installing the SciPy Superpack
Although the [Enthought](https://www.enthought.com/) Python distribution provides an all-in-one, turnkey solution to getting SciPy and matplotlib installed, EPD does not play well with Homebrew, my preferred package manager on the Mac. Therefore I am trying a different route, namely the [SciPy Superpack](http://fonnesbeck.github.io/ScipySuperpack).

Now it was time to install the SciPy Superpack, developed by [Chris Fonnesbeck](http://stronginference.com/ ) . I used his very simple instructions to get this code installed. First, there was a curl command:

```bash
curl -o install_superpack.sh https://raw.github.com/fonnesbeck/ScipySuperpack/master/install_superpack.sh
```

Then, I move the script to my bin directory, at ~/bin/ . I then ran the script by typing:

```bash
sh install_superpack.sh
```

Once this script was done, I had installed Numpy, SciPy, Matplotlib, iPython, Pandas, Statsmodels, Scikit-Learn, and PyMC.

## Just another pretty interface: qtconsole

I decided to take everyone's (on the internets) suggestion and install qtconsole to provide an [aesthetically pleasing interface](http://ipython.org/ipython-doc/dev/interactive/qtconsole.html) for iPython[^3]. This was the trickiest step yet. First I had to install the Qt software. Unfortunately, the newest version, 5.0, comes packaged with quite a bit of stuff (e.g. the "Creator") that I do not want. I just want the console. So I went to https://qt-project.org/downloads and downloaded the Qt Library for version 4.8. Once I ran the installer, the basic Qt software was in place.

Next I downloaded the [PySide libraries](http://pyside.markus-ullmann.de/pyside-1.1.0-qt47-py27apple.pkg) and used the package installer to install the libraries. Just follow that link and the PySide libraries should start downloading. Then, using pip[^5], I installed pygments through the command:

```bash
pip install pygments
```

Finally, I installed pyqt by typing:

```bash
brew install pyqt
```

Once I did all of this, I was able to verify that I had a working qtconsole by executing the following commands:

```bash
ipython qtconsole --pylab=inline
plot(randn(500),rand(500),'o',alpha=0.2)
```

The last command produces the following output:
{% include image.html img='/images/qtconsole_screenshot.png' %}

So now I have a pretty iPython console with inline plotting.


## NetCDF4: the only way to read and write climate data
For my climate modeling work I need the netCDF-python package installed. Fortunately, because of the [Python Package Index (PyPi)](https://pypi.python.org/pypi) and the pip command, this is one of the easiest steps in the installation process.

Here's the command:

```bash
pip install netCDF4
```

And that's it. This set up works with the Homebrew Python, HDF5, and NetCDF4 as well as with the SciPy Superpack.


## In conclusion . . .
At this point, I have a Mac that is ready for some scientific heavy lifting. I can compile scientific code, inspect and analyze climate model output, and manage my data with the tools that I have now installed and configured. Of course, this means that the fun has only begun, because it's time to [do some science!](https://twitter.com/search?q=%23doingascience&src=hash)

[^1]: For example, see Hacker Codex's instructions for [configuring Mavericks](http://hackercodex.com/guide/mac-osx-mavericks-10.9-configuration/ "Configuring Mac OS X Mavericks 10.9") and [installing Homebrew Python and other tools](http://hackercodex.com/guide/python-development-environment-on-mac-osx/ "Python Development Environment on Mac OS X"). As well, both [Nathan](https://gist.github.com/myobie/1860902) and [Lowin Data](http://www.lowindata.com/2013/installing-scientific-python-on-mac-os-x/) have good instructions for various configurations. 

[^2]: These instructions apply to Mac OS X Mavericks. These instructions may or may not work with earlier versions of Mac OS X.

[^3]: See the articles ["Installing python, numpy, scipy, matplotlib, and ipython on Lion"](http://www.thisisthegreenroom.com/2011/installing-python-numpy-scipy-matplotlib-and-ipython-on-lion/ "installing-python-numpy-scipy-matplotlib-and-ipython-on-lion") and ["Installing the IPython qtconsole in Mac OS X"](https://github.com/sympy/sympy/wiki/Installing-the-IPython-qtconsole-in-Mac-OS-X "Installing-the-IPython-qtconsole-in-Mac-OS-X") and ["Setting up Mountain Lion"](http://sergeykarayev.com/work/2012-08-08/setting-up-mountain-lion/ "setting-up-mountain-lion"). This is more than an aesthetic upgrade; the ```qtconsole``` provides additional functionality when working with iPython. The [iPython website](http://ipython.org/ipython-doc/dev/interactive/qtconsole.html) points out that: " This is a very lightweight widget that largely feels like a terminal, but provides a number of enhancements only possible in a GUI, such as inline figures, proper multiline editing with syntax highlighting, graphical calltips, and much more."

[^4]: Instructions based on [Hacker Codex's instructions](http://hackercodex.com/guide/python-development-environment-on-mac-osx/ ) and [Lowin Data's instructions](http://www.lowindata.com/2013/installing-scientific-python-on-mac-os-x/ ).

[^5]: pip was installed as part of the Homebrew Python installation.


