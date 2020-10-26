Last updated: 2017–01–01
------------------------

[![](https://miro.medium.com/fit/c/96/96/0*FXmmVheEKikSZQGO.jpg)
](https://medium.com/@manujarvinen?source=post_page-----d478e7b8ffe4--------------------------------)

I don’t usually use much Terminal in Linux, but for some things it’s a very handy tool. Every now and then I read a [**_nice article_**](https://medium.freecodecamp.com/the-command-line-1fdbc692b3bf#.k0ugg0fq2) about using it and so I decided to gather some of the most basic and useful hints into this article. [**_Here’s another good article_**](http://linuxcommand.org/lc3_learning_the_shell.php) about it. (Also [**_this_**](https://help.ubuntu.com/community/UsingTheTerminal) and [**_this_**](http://ss64.com/bash/syntax-keyboard.html), I need to add more of this stuff to this article later)

# Open it

Open terminal with **Ctrl+Alt+T**, or **Super+T** (Windows-key + T) or just by finding it from the menus. In KDE desktop environment the terminal is often called Konsole.

Quit terminal with **Ctrl+Shift+Q**

# Jump in

\# Manual command (super useful, although exhausting):  
**man**

\# See the manual of the manual (h to help, q to quit):  
**man man**

\# See the manual of Nano Command-line text editor, for example:  
**man nano**

\# Display options to use with the man command:  
**man --help  
man -h**

\# Display options to use with Nano:  
**nano -h**

\# Display the path you're currently standing in (**p**rint **w**orking **d**irectory):  
**pwd**

\# For some info about the pwd command:  
**man pwd**


# Navigate

\# Move cursor **b**ack:  
**Ctrl+B** or **Arrow left**

\# Move cursor **f**orward:  
**Ctrl+F**(in some systems) or **Arrow right**

\# Using Ctrl+B and Ctrl+F (and all the others that will follow) are recommended for efficent users. You don't have to lift off your fingers from the typing area of the keyboard. The Ctrl+F doesn't work in xfce4-terminal (at the time of writing this), for example. That's why I like to use gnome-terminal. Install it with: _sudo apt install gnome-terminal_ in ubuntu-based distros. In Xubuntu you can put it as default Terminal from _Preferred Applications_. In _Edit > Preferences > Shortcuts_ there's a nice list of useful extra keyboard shortcuts.



![](https://miro.medium.com/max/2094/1*lbvMn_Wm10THF7vTIzpehg.png)

gnome-terminal


\# Enter:  
**Ctrl+J** or **Enter** \# Scroll command list to previous command:  
**Ctrl+P** or **Arrow up**

\# Scroll next:  
**Ctrl+N** or **Arrow down**

\# Goes **b**ack one word at a time:  
**Alt+B**

\# Moves **f**orward one word at a time(in some systems):  
**Alt + F**

\# - Capitalize the letter your cursor is on, decapitalize all the following letters of the word  
\# - If cursor is between words, capitalize the first letter of the next word  
\# - I don't really know a good use case for this  
**Alt + C**

\# Delete rest of the word or the whole word after the cursor:  
**Alt+D**

\# Jump to insert something to the beginning of what you're typing, press again to go back to almost the same place. Toggles between two positions of the cursor, a bit weird:  
**Ctrl+X+X**

# Change directory

\# Go to your home directory (/home/_\[username\]_), no matter where you are located (all of these commands work):  
**cd**  
**cd ~**  
**cd $HOME  
cd $home**

\# Change directory to /usr/bin:  
**cd /usr/bin**

\# Go to the parent directory (from /usr/bin to /usr/):  
**cd ..**

\# Go back to the previous directory  
**cd -**

\# Scroll through previous commands (very useful):  
**Up  
Down**

\# Quickly jump back and forth over words with cursor (in Mac OS X, _Esc+B_ and _Esc+F, which work in Linux as well):  
**Ctrl+Left  
Ctrl+Right**

\# Quickly jump to the beginning or end of the of the current line (in Mac OS X, Ctrl+A and Ctrl+E):  
**Home  
End**

\# Also:  
**Ctrl+A  
Ctrl+E**

# Clearing text

\# Clear the screen of the text (push the empty lines in front of you to back):  
**Ctrl+L**

\# or  
**clear**

\# However, you can still **_scroll up or down_** with scroll wheel or use **_Shift+PageUp_** and **_Shift+PageDown_** to scroll to the previous commands\# Remove what's on the left of your cursor on the line (use when you want to clear out the command you're currently typing):  
**Ctrl+U**

\# Remove everything in front of your cursor on the line:  
**Ctrl+K**

\# Fast backspace or Erase/cut words/parameters of a command that are separated by space:  
**Ctrl+W** or **Alt+Backspace**

\# Paste back what you removed with Ctrl+U, Ctrl+K or Ctrl+W or Alt+Backspace:  
**Ctrl+Y**

\# These are very useful for navigating while typing a command, if your cursor is in the middle, you can just **Ctrl+E+U** or **Ctrl+A+K** to clear it. If you didn't mean to do that, **Ctrl+Y** brings it back.

\# However, **Ctrl+C** (cancel) is even faster for cancelling the typing of a command, but Ctrl+Y or up arrow doesn't bring that back.

\# **Ctrl+\\** - same as Ctrl+C but stronger (used when terminal doesn't respond. Doesn't work in Finnish keyboard, though :(
    
\# Or, **Alt+Shift+#** comments out the line to be visible for reference.

# Random stuff

\# Somekind of undo, like Ctrl+Y but more accurate (but very unconsistent). Handy to use if you removed characters too much from your command):  
**Ctrl+-** or **Ctrl+7**

\# Backspace:  
**Ctrl+H** or **Ctrl+8** or **Backspace**

\# Enter:  
**Ctrl+J**

\# Deletes characters to the right of cursor. (If no characters, Ctrl+D also logs out and closes terminal):  
**Ctrl+D** or **Delete**

\# Search text from the whole history (even after **reset**):  
**Ctrl+R**

\# This will zombie an application. If you have a process running in a terminal and you want the terminal back but don’t want to kill the application, you can hit Ctrl + z to send the process to the background. To get the process back, type _fg_.  
**\# Ctrl + Z**

\# Search text from the terminal window scrollback history:  
**Ctrl+Shift+F**

\# You can fast spam the screen full of something by:  
**wheee**  
**Ctrl+U**  
**Ctrl+hold Y**  
**Ctrl+U**  
**Ctrl+hold Y**  
**Ctrl+U**  
**Ctrl+hold Y**

\# If your terminal gets stuck, you can usually get back to entering commands by pressing **Enter** and **Ctrl+L,** or just with **Ctrl+C** (cancelling).

\# Reset:   
\# - Clear screen and the scroll history  
\# - The command history stays (arrow keys up and down, Ctrl+R search)  
**reset**

\# Or faster one:  
**tput reset**

\# Clear bash history (clear all the commands in history):  
**history -cw**

\# Or (depending on what Terminal is in use):  
**Alt+T, C**

\# Sometimes you might want to do **Ctrl+L+L** to make a nice little gap (the vertical size of your terminal window) to see where your previous command started to give input. For example:  
**reset  
man -h** \*enter\* \*up-arrow\*  
**man -h  
man -h**

\# And you can't really scroll up and quickly see where you typed the command second or third time. But:  
**reset**  
**man -h**

**Ctrl+L+L  
man -h**

**Ctrl+L+L  
man -h**

\# And scroll up and you can clearly see where your commands begun.

\# Some people like to keep **enter pressed down** for one second to make that gap, which is in some cases a better method (the gap isn't as huge as with the Ctrl+L+L method).

# Copy-pasting with mouse cursor

\# Copy the highlighted text to clipboard:  
**Ctrl+Shift+C**

\# Paste:  
**Ctrl+Shift+V**  
**Shift+Insert**

\# (In elementary OS also Ctrl+C and Ctrl+V work. If text is not highlighted, **Ctrl+C** is the **Cancel** command, like it is normally.)

# Auto-complete

\# Use Tab to autocomplete commands, files and directories you are typing:  
**cd .config/tran** \*Tab\*  -> **cd .config/transmission**

\# Double-tap Tab to reveal, for example, all files/folders in /usr/share/ that start with "gtk" while still being able to continue typing the command:  
**ls /usr/share/gtk** \*double-tap Tab\*

![](https://miro.medium.com/max/1000/1*2VjUed7g5SOH01YSCuU_2g.jpeg)

\# Double-tap Tab to reveal what’s inside the folder /usr/ while still being able to continue typing the command:  
**ls /usr/** \*double-tap Tab\*


# List files

\# List files and directories of the current directory:  
**ls**

\# To list also hidden files:  
**ls -a**

\# To list more info about the files:  
**ls -l**

\# To list files starting with "blen" in the directory you're in:  
**ls blen**

\# To list all the .jpg files in the directory you're in:  
**ls \*.jpg**

\# You can also use it to list the contents of another directory, like so:  
**ls /usr/bin  
ls /usr/bin/blen\***

![](https://miro.medium.com/max/1000/1*auRIBgnQHGS1JrMOFDJb_w.jpeg)

**ls /home/mj/.config/blender  
ls ~/.config/blender  
ls $HOME/.config/blender**

\# Or multiple other directories at the same time:  
**ls ~/.config/blender ~/.config/transmission**

\# Echo is useful for listing stuff as well:  
**echo Pictures\*.txt  
echo /usr/bin/blen\*  
echo /usr/bin/\*ender**

# File manipulation

\# Make a file called file1.txt  
**touch file1.txt  
touch ~/Documents/file1.txt**

\# Insert a text line into the file1.txt:  
**echo apple >> file1.txt**

\# Check quickly out what the file1.txt contains:  
**cat file1.txt**

\# Also works (for files with a lot of information):  
**less file1.txt**

\# (quit by pressing **Q**)

# System info

\# Find the word "apple" inside all .txt files:  
**grep apple \*.txt**

\# Pipe the information from lspci device information list command to grep and find the line that has "NVIDIA" in it. For example, find details about your display card (in case it's NVIDIA):  
**lspci | grep "NVIDIA"**

![](https://miro.medium.com/max/1576/1*z9-J6YhkbHzlTR9sU8MKbw.png)

Finding details about the display card with the command: **lspci | grep “NVIDIA”**

# Text editor

\# Open and edit the file1.txt in the Nano command line text editor:  
**nano file1.txt**

\# Quit by pressing **Ctrl+X**, then **Y** (if you want to save the changes), then **Enter**

# Copy, Move & Rename

Copy
----

\# Copy file1.txt to file2.txt:  
**cp file1.txt file2.txt**

\# Copy file1.txt to ~/Texts:  
**cp file1.txt ~/Texts**

\# Also works:  
**cp file1.txt ~/Texts/**

\# Also works:  
**cp file1.txt ~/Texts/file1.txt**


Move & Rename
-------------

\# Move/Rename file1.txt to file2.txt:  
**mv file1.txt file2.txt**

\# Move file1.txt to ~/Texts/:  
**mv file1.txt ~/Texts/**

\# You can even Move and Rename at the same time:  
**mv file1.txt ~/Texts/file2.txt**

\# More info [**_here_**](http://linuxcommand.org/lc3_lts0050.php)

# Chain commands

\# Chain commands:  
**cp file1.txt ~/Texts/ && mv file1.txt ~/Texts/file2.txt**

\# Or:  
**cp file1.txt ~/Texts/ && mv ~/Texts/file1.txt ~/Texts/file2.txt**

\# Or:  
**cp file1.txt ~/Texts/ && mv ~/Texts/file1.txt file2.txt**

\# Chaining choices [*](http://askubuntu.com/a/539293):  
**A && B**  Run B if A succeeded  
**A; B**    Run A and then B, regardless of success of A  
**A || B**  Run B if A failed

\# Other way to chain commands:  
**sh -s <<EOF** \*Enter\* 

**\> cp ~/test/file.txt ~/test/folder1/** \*Enter\*

**\> cp ~/test/file.txt ~/test/folder2/** \*Enter\*

**\> EOF** \*Enter\*

\# Or:  
**for dest in folder1 folder2; do cp ~/test/file.txt ~/test/"$dest"; done**

# Run in background

**A&**     Run A in background  
**A &**    Run A in background

\# For example:

\# Open Thunar File Manager with root privileges:  
**sudo thunar**

\# You can't use that particular terminal window after doing this, until you close that Thunar window.  
\# If you close the terminal, thunar also closes.  
\# If you close Thunar, you can use that terminal window again.

\# If you don't have Thunar, you can install it in Ubuntu-based systems with the command:  
**sudo apt install thunar**

\# Open Thunar with root privileges in background:  
**sudo thunar&**

\# After that you can use or close the terminal and keep the rooted Thunar running.

\# Remember that if you, for example, copy a picture to your home folder in a rooted Thunar, you can't edit and save them with your regular user unless you modify the file's permissions again.

\# However, for example **blender&** or **gimp&** don't work that way.

\# But, sometimes it's good to open **blender** through a terminal to see its error messages and other information in the terminal window. Especially when hassling with Python. Btw, have you checked out the article about [Simple Python Tips For Artists](https://gooseberry.blender.org/simple-python-tips-for-artists/).

\# Note: it's generally recommended to **not use sudo** for opening graphical applications like Thunar and instead use **gksu** or **gksudo** for that. I'll update this section in the future when I find a very clear and practical example about that. Using **sudo** for that is awesomely fast, though.

# Symbolic (soft) links and Hard links

\# Make a symbolic link from file1.txt to file1\_symbolic\_link.txt  
**ln -s file1.txt file1\_symbolic\_link.txt**

\# When you change the contents of file1.txt the same changes can be seen in file1\_symbolic\_link.txt.

\# Symbolic links are very handy for linking a folder from a different partition to ~/.config/blender, for example. Like explained in [**_here_**](https://medium.com/@manujarvinen/my-method-for-storing-files-49bf71c6edbd)  in the section "Symbolic links".

\# You can also make hard links with the ln command. Hard links can’t be made for directories, though. More info about the difference between symbolic (soft) and hard links can be found in [**_here_**](http://askubuntu.com/a/801191)**_._**

# System monitor

\# Terminal system monitor (quit with **q** or **Ctrl+C**):  
**top**

\# Better system monitor (colors, supports mouse):  
**htop**

\# In Ubuntu-based systems install it by:  
**sudo apt install htop**

\# Uptime, how long your computer has been on:  
**uptime**

# Calendar

\# Calendar:  
**cal**

\# How it works:  
**man cal**

# Disk usage

\# Get the disk usage of the Home folder and /usr/bin:  
**du -sh ~ /usr/bin**

\# Get the disk usage of the 15 largest folders in /storage/Dropbox/:  
**du -h /storage/Dropbox/ | sort -h | tail -n 15**

# Find

\# Find files that have in their name the word "workroom" from your hard drive, in this case, the whole system ( / ):  
**sudo find / -iname \*workroom\***

\# Same, but find both "workroom" and "weakroom", for example:  
**sudo find / -iname \*w??kroom\***

\# Same, but for folders. The iname means that it doesn't matter if the letters are upper-case or lower-case:  
**sudo find / -type d -iname \*workroom\***

\# Find a file from the folder you’re currently standing in:  
**sudo find . -iname \*workroom\***

\# Find a file that's _exactly_ Workroom\_hour\_list, with only W being a capital letter, nothing else:  
**sudo find / -name Workroom\_hour\_list**

\# Find all the .txt files:  
**sudo find / -iname \*.txt**

\# Find all the files:  
**sudo find / -iname \*.\***

\# If the list you're finding is too long and keeps on growing, cancel it with **Ctrl+C**. Or go up and down the list with **Shift+PgUp** and **Shift+PgDn**

# Misc

\# All of these execute Gimp if Gimp is installed:  
**exec /usr/bin/gimp  
exec gimp  
gimp**

\# Can be launched from **Alt+F2** as well:  
**gimp**

\# If you have downloaded a Blender from builder.blender.org and are standing in the folder: ~/Downloads/blender-2.78-96f28f9-linux-glibc219-x86_64/blender, you can launch that particular Blender by:  
**exec ./blender**

\# or:  
**exec ~/Downloads/blender-2.78-96f28f9-linux-glibc219-x86_64/blender**

\# This also works without the exec:  
**~/Downloads/blender-2.78-96f28f9-linux-glibc219-x86_64/blender**

\# Display the path of an installed application:  
**which gimp**

![](https://miro.medium.com/max/990/1*Zl1UHPQf55-aZi_4Xu642Q.png)

Using the command **which** to get a path to the installation location of an application

\# Show your user ID:  
**id -un**

\# Also works:  
**echo $USER**

# Render with Blender via Terminal

Flags [*](http://www.blender.org/manual/render/workflows/command_line.html)
---------------------------------------------------------------------------

**-b** or **--background** = Render in background

**-o \[PATH\]** or **--render-output \[PATH\]** = Render path and file name

**-y** or **--enable-autoexec** = Enables the automatic Python script execution

**-F PNG** or **--render-format PNG** = render in format PNG

**-F EXR** or **--render-format EXR** = render in format EXR

**-x \[boolean (1** or **0** (true or false))**\]** or **--use-extension \[boolean\]** = add the file extension to the end of the file

**-a** or **--render-anim** = Render frames from start to end (inclusive)

\# More can be found with the command blender --help

Render image sequences
----------------------

**~/Downloads/Blender_2.78/blender** -b **/work/blends/test1.blend** -o **/work/renders/test1/frame_** -F PNG -y -x 1 -a


Render a single frame
---------------------

**~/Downloads/Blender_2.78/blender** -b **/work/blends/test1.blend** -o **/work/renders/test1/frame_#####** -F PNG -x 1 -f 651

Render multiple non-sequenced single frames
-------------------------------------------

\# Render frames 2335, 2337, 2401 and 2385:

**~/Downloads/Blender_2.78/blender** --background 

**/work/blends/test1.blend** --render-output 

**/work/renders/test1/#####**  --render-format EXR --enable-autoexec -f 2335 -f 2337 -f 2401 -f 2385

# Make a .GIF from a .mp4 video

**ffmpeg -y -i “Goblin Turntable.mp4” -vf fps=10,scale=1024:-1:flags=lanczos,palettegen palette.png**

**ffmpeg -y -i “Goblin Turntable.mp4” -i palette.png -filter_complex “select=not(mod(n\\\\,3)),setpts=N/((30000/1001)\*TB),fps=5,scale=1024:-1:flags=lanczos\[x\];\[x\]\[1:v\]paletteuse” goblin.gif**

