---
layout: post
title: How to speed up your vim startup time
category: posts
---

My go-to editor for pretty much anything is vim. And like any serious vim user
I use numerous plugins to extend and improve the editing experience. Natually
there's a cost to that, namely an increase in vim startup time. Compared to
almost any GUI application vim starts up almost instantaneously. Still I
noticed that slight delay and it had been bothering me for a while until I
finally got round to investigate and tweak.

Thankfully vim makes profiling the startup time really convenient by providing
a `--startuptime` flag to write timings for loading your `.vimrc` and plugins
to a file, which looks something like this:

    times in msec
     clock   self+sourced   self:  sourced script
     clock   elapsed:              other lines

    000.007  000.007: --- VIM STARTING ---
    002.399  002.392: Allocated generic buffers
    002.468  000.069: locale set
    004.946  002.478: GUI prepared
    004.954  000.008: clipboard setup
    004.967  000.013: window checked
    016.181  011.214: inits 1
    016.187  000.006: parsing arguments
    016.188  000.001: expanding arguments
    016.212  000.024: shell init
    017.230  001.018: Termcap init
    018.203  000.973: inits 2
    018.390  000.187: init highlight
    022.090  002.745  002.745: sourcing /usr/share/vim/vim74/debian.vim
    026.679  000.296  000.296: sourcing /usr/share/vim/vim74/syntax/syncolor.vim
    026.805  001.004  000.708: sourcing /usr/share/vim/vim74/syntax/synload.vim
    ...

The interesting measurements are those concerned with sourcing files, so focus
your attention on the 3rd column to see where time is spent. Short of trimming
down your vimrc (mine takes about 18ms to load by itself) and cutting down on
the number of plugins, the plugin manager can also make a difference. I used
[pathogen] for a long time, before switching to [Vundle] (because it's easier
to have your plugin manager handle Git repositories rather than having to
manually add them as submodules to your dotfile repository or similar). Then I
came across [vim-plug] and was intrigued by its [on-demand loading
feature](https://github.com/junegunn/vim-plug#on-demand-loading-of-plugins).
That allows loading plugins for specific file types or only on the first
invocation of a certain command. The latter is particularly useful for plugins
you don't need to have active all the time. In my case this was particularly
useful for [NERDtree] and [DokuVimKi], which both take a significant time to
load. Looking carefully at the startup time output I also noticed I was
sourcing filetype plugins twice! The offending line in my vimrc was quickly
found, which shaved off some additional milliseconds.

**TL;DR** Switching to [vim-plug], getting rid of some unnecessary plugins,
loading others only on demand and uncluttering my vimrc I managed to cut down
my vim startup by more than half, from close to 250ms to only about 120ms.

[pathogen]: https://github.com/tpope/vim-pathogen
[Vundle]: https://github.com/gmarik/vundle
[vim-plug]: https://github.com/junegunn/vim-plug
[NERDtree]: https://github.com/scrooloose/nerdtree
[DokuVimKi]: https://github.com/kynan/dokuvimki
