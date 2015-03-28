---
layout: post
title:  "Creating Alias in Bash Shell in OSX"
date:   2015-03-27 11:30:21
categories: jekyll update
---

# Creating Alias

For the second time I had to reconfigure my work environment from ground up.
Literally resetting my mac to factory settings. And every time I had to do a
little bit of research to remind myself how to get my favorite git alias working
again. So I decided to do a little tutorial on it, after spending sometime
getting it setup. (for myself)

  • Launch your Terminal. I use [Quisilver](http://qsapp.com/), so can either
  use ```Ctr + Space + type<Iterm>``` or the following hotkey/trigger  ```Ctr + Option + Cmd + t```


  • ```"cd ~/"``` to your home directory.


  • The .bash_profile file lives in your home directory, make sure you have it by entering ```ls -la``` or ``` la```(works for me).
   - it should look similar to this.


  ![Imgur](http://i.imgur.com/tDlYiOX.png)

  • In case you don't have .bash_profile file, type ```nano .bash_profile```.

  • In ```nano .bash_profile```

  - You should see something similar to this (mind my aliases)

 ![Imgur](http://i.imgur.com/W4mf1IK.png)

  • After the file is created and you see that on your screen, you are ready to enter some aliases.

   - ```alias alias_name="command_to_run```

  •  Make sure you have no space between the equal sign.

  •  When you're done ```Ctr + o``` will write the changes to the file.

  •  ```Ctr + x``` will exit you out of nano.

  •  In order for the changes to take effect run ```source ~/.bash_profile```.


 All done.
