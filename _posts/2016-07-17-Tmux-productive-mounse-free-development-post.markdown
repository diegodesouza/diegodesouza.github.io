---
layout: post
title:  "Tmux Notes"
date:   2016-07-17 11:30:21
categories: jekyll update
---

### Tmux - Productive Mouse-Free Development

-

#### Instructions

To keep it simple, we'll follow these conventions.

* `PREFIX` means the default `CTRL-b` here, though I have remapped my `PREFIX` to `CTRL-s` because I find it better to reach (also because most of my colleagues already use these key bindings). I hear some people have mapped their `Caps-lock-a` as their `PREFIX`. Use your discretion here. There are no rules. 

* `CTRL-s` means "press the CTRL and s keys simultaneously"

* `CTRL-R` means the same thing, except that you'll need to use the `SHIFT` key to produce the capital "R". Just notice the capital letters.

* `CTRL-s d` means "press the CTRL and s keys simultaneously, then release,
and then press d"

* `$ tmux new-session` the dollar sign represents the prompt from the shell session.·

## Basics

| Abrv   | Full Name     |
| -------|---------------|
| new    | new-session   |
| -s     | session       |
| -t     | target        |
| a / at | attach        |
| ls     | list sessions |
| -h     | horizontal    |
| -v     | vertical      |
| -d     | detach        |

#### Creating Named Sessions

`$ tmux new -s [name_of_session]`

#### Attaching From Sessions
 
 `$ tmux attach -t [name_of_existing_session]`

#### Detaching From Sessions

* `CTRL-s` `d`, or
* `PREFIX` `d`
  * NOTE: *The session you just detached from, is still running in the background,
meaning you can still re-attach to it later.*

#### Killing Sessions

`$ tmux kill-session -t [name_of_session]`

## Window Management (Tabs)

#### Creating and naming a window with a new session

 `$ tmux new -s [name_of_session] -n [name_of_window]`

  * The `-n` flag tells tmux to name the first window whatever you pass in the `name_of_window`.

#### Creating a window from an existing session

  `PREFIX` `c` 
        
NOTE: You can then rename your newly created window by using *`PREFIX` `,`*
  
#### Tmux Command Line Mode

  `PREFIX` `:`
  
  then in the prompt we should be able to enter
  
  `new-window -n [name_of_window]`
  
   * Except that this is rather verbose and long to create a named window (tab).

#### Switching Windows

`PREFIX` `[1..9]`

#### Displaying a list of windows

`PREFIX` `w`

#### Closing a window with a prompt

`PREFIX` `&`

#### Splitting windows

  * Horizontally (will stack one pane on top of each other)
  
  `PREFIX` `-` (hyphen)
  
  * Vertically (will put one pane next to each other)
  
  `PREFIX` `\`
  
  NOTE: when we split afollowed window, it is said we have created a *pane*. 

#### Turning a Pane into a Window

  * After splitting a window you might want to have a specific pane as a window instead.

  `PREFIX` `!`

#### Turning a Window into a Pane

  * To reverse the above, we need to do a little bit of work. Essentially we'll be moving a pane from one session to another. We specify the source window and pane, followed by the target window and pane. If we leave the target off, the current focused window becomes the target.

  Ex: We create a new Tmux session with two windows.
  
  ```
  $ tmux new-session -s panes -n first -d
  $ tmux new-window -t panes -n second
  $ tmux attach -t panes
  ```

  Now, to move the first window into a pane in the second window, we press
`PREFIX` `:` to enter Command mode, and type this:
`join-pane -s 1`
This means “Take window 1 and join it to the current window,” since we didn’t specify a target.

#### Resizing split windows

 * Smaller steps
 
   `SHIFT-LEFT` `SHIFT-RIGHT` `SHIFT-UP` `SHIFT-DOWN`

 * Bigger steps
 
   `CTRL-LEFT` `CTRL-RIGHT` `CTRL-UP` `CTRL-DOWN`

# Settings

-

* Reloads .tmux.conf

`PREFIX` `r`

## Creating Custom Scripts

* Based on the knowledge we have above, we can start a script with some tux commands.
Let's create a file called `~/development`. 
  * `touch ~/development`
  
  In it we start with:
  
  `tmux new-session -s development -n editor -d`
  
  * **Recap**: we're creating a new named session called `development` a new *window* called `editor` and immediately detaching from this new session with the `-d` flag.
  
  
  * In Tmux we can send commands to other sessions with `send-keys`. This is going to come handy when running script like the ones we're creating. Here's an example.

  Ex: `tmux send-keys -t development 'cd ~/devproject' C-m` - cd into devproject directory.
  
  Ex: `tmux send-keys -t development 'vim' C-m` - opens vim editor.
  
  **NOTE**: `C-m` is equivalent to pressing `ENTER`. Notice how the commands `cd ~/devproject` and `vim` are in quotes as well.  
  
  Ex: `tmux split-window -v -t development` - splits a window vertically.
  
  Ex: `tmux split-window -v development main-horizontal` - splits the main window horizontally.
  
  * We can target another window as well with this formatting. `[session]:[window].[pane]`.
  
  Ex: `tmux send-keys -t development:1.2 'cd ~/devproject' C-m` cd into devproject directory in session 1 window 2.
  
The script looks like something like this, with extra stuff. The sky is the limit here. 

```
tmux new-session -s development -n editor -d
tmux send-keys -t development 'cd ~/devproject' C-m
tmux send-keys -t development 'vim' C-m
tmux split-window -v -t development
tmux select-layout -t development main-horizontal
tmux send-keys -t development:1.2 'cd ~/devproject' C-m
tmux new-window -n console -t development
tmux send-keys -t development:2 'cd ~/devproject' C-m
tmux select-window -t development:1
tmux attach -t development
```

# Copy Mode 

-

* In order to be able to copy text in Tmux we need to enter *copy mode*. 

`PREFIX` `[` puts us in that mode. Though out of the box the commands aren't very intuitive. So after a little googling, I got it working the *vim* way. 

In my `.tmux.conf` file, i placed the following settings. 

```
# Use vim keybindings in copy mode
setw -g mode-keys vi

# Setup 'v' to begin selection as in Vim
bind-key -t vi-copy v begin-selection
bind-key -t vi-copy y copy-pipe "reattach-to-user-namespace pbcopy"

# Update default binding of `Enter` to also use copy-pipe
unbind -t vi-copy Enter
bind-key -t vi-copy Enter copy-pipe "reattach-to-user-namespace pbcopy"
```
and in my `.vimrc` I added 


```
" be able to copy from vim within tmux
set clipboard=unnamed
```

I encourage you to google these settings and dig deeper as the information is so beyond my needs at the moment.


# Buffers

-

* Displays the content of our paste buffer.

`$ tmux show-buffer`

* Displays a list of buffers we have used.

`$ tmux list-buffers`

* Saves buffer into a file

`$ tmux capture-pane && tmux save-buffer buffer.txt`

* Choose a buffer from a list

`$ tmux choose-buffer`

* Scrolls up one page

`Ctrl-b`

* Scrolls down one page

`Ctrl-f`

* Inherits some *Vim* commands like `/`, `?`, `g`, `G`, `f`, `F`, etc...

NOTE: although there are a lot more you can do with buffers, this is as far as I've gotten to using them. Feel free to dig deeper. 
