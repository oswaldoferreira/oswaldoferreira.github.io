---
layout: post
title: "First steps on VIM"
modified: 2015-10-25 00:44:28 -0200
category: []
tags: [vim]
image:
  feature: texture-feature-X2.jpg
  credit:
  creditlink:
comments: true
published: true
---

# Hey,


So you’re planning to starting using vim but doesn’t want to afford the initial learning curve. You don’t want to let your productivity go down on your daily basis work. I’ll tell you something, there’s not a magic recipe, or even a secret way of learning it without actually struggling around with it for at least one week. But don’t feel bad already, c’mon put a smile on that face and let’s just DO IT.

Take that vim guru in action to warm you up:

![](http://33.media.tumblr.com/ec4935a645d0003a128a1f7cb12ca377/tumblr_inline_nsu3wtXBIv1raprkq_500.gif)


You probably already read tons of posts talking about [why you should use vim](http://yehudakatz.com/2010/07/29/everyone-who-tried-to-convince-me-to-use-vim-was-wrong/) instead IDE’s or “ready for action” editors out there, so I’ll focus on the VIM survivability path.

I’ve been using only vim for just 4 months for now and I can say that i’m feeling twice (or even more) as productive compared to my [Atom](https://medium.com/@mkozlows/why-atom-cant-replace-vim-433852f4b4d1) days.

_Everything started on `vim tutor`._

If you already not tried out `vimtutor`, take 20 minutes and do it by typing `vimtutor` (ahá!) on your terminal. It's a simple tutorial that I recommend going through for at least 3 times.

There's also funnier ways to put that crazy keystrokes on your muscle memory:

[Vim Adventures](http://vim-adventures.com) - It’s a game which you need to move a character by typing vim keystrokes H, J, K, L.

[Open Vim](http://www.openvim.com/) - An interactive terminal that guides you through vim basics.

If you’re more of a visual person like me, you’ll really like the [upcase basic vim course](https://upcase.com/onramp-to-vim). They talk about everything you’ll need to know for this first week experience and much more.

## My most used commands

VIM has a [unique and efficient movement way](http://www.viemu.com/vi-vim-cheat-sheet.gif). Knowing the essence of it will make you a master. But since you are just starting I would recommend playing around with those commands below.

**Moving**

`w` - Jumps between words to the right.

`b` - Jumps between words going back.

`gg` - Goes to the top of file.

`G` - Goes to bottom of file.

`<C-d>` - Jump’s around 12 lines down (great move faster inside a file).

`<C-u>` - Jump’s around 12 lines up.

**Editing**

`o` - Moves a line down and brings you to insert mode.

`O` - Moves a line up and brings you to inset mode.

`ciw` - Deletes word on your cursor and brings you to insert mode.

`diw` - Deletes word on your cursor and stays on normal mode.

`dd` - Deletes current line and stays on normal mode.

`x` - Deletes a character and stays on normal mode.

`r-character` - Replaces a character and stays on normal mode.

`A` - Goes to end of the last word on the line and brings you to insert mode.

`I` - Goes to the start of first word and brings you to insert mode.

A practice that keep’s me learning more commands is the [use of cheat-sheet’s](https://docs.google.com/spreadsheets/d/1fo8jYbau-e9ZDb_WEGFcKvIMhFhyUY4YENrM40ADZOg/edit?usp=sharing). You can remove
that command of the cheat-sheet when it’s usage is already recurrent to you.


## Basic setup

People that use vim will usually say things like:

- Don’t use direction keys.
- Don’t waste time on insert mode.
- Don’t use folder tree plugins.
- Don’t over-repeat key presses.
- Don't do this or that.

Don’t bother yourself with those “rules” just yet. Just don’t. Make yourself comfortable with the editor and let the usage tell you what you should improve on your workflow.

I’ve created a [gist with vimrc configurations](https://gist.github.com/oswaldoferreira/e1e798d65757713b1757) that I personally felt comfortable to start with.

### Core plugins for a beginner

- [FZF](https://github.com/junegunn/fzf) - A really fast and simple fuzzy finder.
- [ag](https://github.com/rking/ag.vim) - Grep like text searching.
- [NERDtree](https://github.com/scrooloose/nerdtree) - A folder-tree plugin. It’s not a must as you can always use the fuzzy finder.


## Let the battle begin


So let’s say it’s your first official day on your work using vim.

You can shamelessly keep both editor’s open and switch to the old one when you just get stuck. We both know that’s not
a good practice, but it can really help pointing your inabilities on vim. What about using that editor
switching cue to just take a note with a reason like “Need to learn how to use tabs”, or
“Need to learn how to search and replace words”.

You can get all those cue points notes and dig [stackoverflow](http://stackoverflow.com/), [vimcasts](http://stackoverflow.com/), or just google it on your free time.

That practice generated some kind of a guideline for me, and it's a _guideline based on usage_.

After a sweaty week I started to feel the shiny reward and weeks later I almost couldn't see those old switching cue points between editors.

![](https://media.giphy.com/media/87xihBthJ1DkA/giphy.gif)


