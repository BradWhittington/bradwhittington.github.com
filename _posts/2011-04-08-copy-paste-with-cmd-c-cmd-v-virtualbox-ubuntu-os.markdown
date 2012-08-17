---
author: bradwhittington
date: '2011-04-08 12:52:55'
layout: post
slug: copy-paste-with-cmd-c-cmd-v-virtualbox-ubuntu-os
status: publish
title: Copy-paste with ⌘-c and ⌘-v in ubuntu running in virtualbox (on an OSX host)
wordpress_id: '16'
tags: [Development, Django, linux, OSX, Ubuntu, vim, virtualbox]
---

I have a somewhat esoteric setup on my laptop. I use OSX as my
primary operating system, and run one or more
[Ubuntu](http://ubuntu.com) virtual machines in
[Virtualbox](http://virtualbox.org) for development work,
effectively using a VM as a project IDE (I use vim for editing,
Firefox to interact with the current 'build', running a
[Django](http://djangoproject.com) local server on the current code
base). When I am done for the day, I suspend the VM, so that the
next time I want to work my 'IDE' is in **exactly** the same state
I left it in, and it is up and running faster than any other IDE I
have ever used. But I have this issue with copying and pasting.
Gnome / Linux never really figured out how to merge the windows
introduced GUI concept of ctrl-c / ctrl-v for clipboard
manipulation with the 'ctrl-c means stop' world of the terminal.
The current Gnome solution is ctrl-shift-c / ctrl-shift-v, which is
a lot of fingers for a common use-case. Apple created a lovely
solution by side-stepping it a little; on OSX ⌘-c means copy, and
⌘-v means paste. So I want that sugar on everything. Virtualbox
complicates this endeavor because they have the concept of a "Host"
key, defaulted to left-⌘. The Host key helps you escape from the
virtual machine by explicitly toggling modes (it is like caps-lock
for where your keyboard input should go; to the host OSX, or the
guest, Ubuntu). Easily fixed! Go to Virtualbox preferences
(shortcut ⌘,), choose 'input' and set the Host key to another key
(I chose right-⌘), and uncheck 'Auto capture keyboard': 

![Set the Host key to something else, disable 'Auto capture keyboard'](http://bradwhittington.files.wordpress.com/2011/04/screen-shot-2011-04-02-at-12-28-39-pm.png "Virtualbox input preferences")

This has a side benefit of making the left ⌘ behave more sensibly,
I can now ⌘-tab between virtualbox and other applications, where
the action used to be tap ⌘, ⌘-tab to switch input to the host, and
then switch applications. After hours of googling, the best I can
do is change gnome-terminal's preferences for copy/pasting.
Unfortunately, short of recompiling GTK, or using very crufty macro
replay solutions, I can't find a way to change the global ctrl-c /
ctrl-v shortcuts (fielding suggestions now). At least getting rid
of one copy-paste set of shortcuts is a win, and my most common
use-case is satisfied (copying from OSX and pasting in a terminal
in Ubuntu). To change gnome-terminal, choose Edit -\> Keyboard
Shortcuts... 

![Choose Edit -\> Keyboard Preferences...](http://bradwhittington.files.wordpress.com/2011/04/screen-shot-2011-04-02-at-12-35-13-pm.png "Gnome-terminal")](http://bradwhittington.files.wordpress.com/2011/04/screen-shot-2011-04-02-at-12-35-13-pm.png)

Finally, set your edit keys to "Super+c" and "Super+v" (click the
existing shortcut, and hit the new keyboard combo): 

![Set your copy and paste shortcuts to "Super+c" and "Super+v"](http://bradwhittington.files.wordpress.com/2011/04/screen-shot-2011-04-02-at-12-35-33-pm.png "Gnome-terminal keyboard shortcut preferences")](http://bradwhittington.files.wordpress.com/2011/04/screen-shot-2011-04-02-at-12-35-33-pm.png)

All done. Maybe a long post on something so random, but, after
using this setup for almost 2 years, my heart did a little jump for
joy at this triviality. Looking for suggestions how I can change /
alias the global copy paste shortcuts in gnome though...


