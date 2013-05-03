---
layout: post
title: "Painless navigation between panes in tmux"
date: 2013-05-03 00:48
comments: true
categories: 
---
iTerm has a same splitting panes functionality as tmux. By default you can jump to the pane on a left by using by using a single keystroke *Command+Alt+Left*. However in Tmux, you need to press two keystrokes *Ctrl-B* followed by *Left*. I found that moving between panes is the operation that I do the most and hitting two key strokes instead of one might be annoying.

It turns out we can use tmux `bind` command with -n flag to create a mapping that don't require pressing the extra *Ctrl-B* keystroke. Here is how:

If you're using iTerm2, make sure that "option key acts as +Esc" setting is chosen on Preferences->Profiles->Keys screen
Put following into your ~/.tmux.conf

```
bind -n 'M-Left' select-pane -L
bind -n 'M-Down' select-pane -D
bind -n 'M-Up' select-pane -U
bind -n 'M-Right' select-pane -R
```
Reload your tmux session and voala!

Now you can use *Alt-Left* in tmux to move between panes without pressing *Ctrl-B*

Also, if you're using vim's navigation keys, you might use h, j ,k , l keys in your tmux mappings to be consistent.

```
bind -n M-h select-pane -L
bind -n M-j select-pane -D
bind -n M-k select-pane -U
bind -n M-l select-pane -R
```
