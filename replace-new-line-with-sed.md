[How can I replace a newline (\\n) using sed?](/questions/1251999/how-can-i-replace-a-newline-n-using-sed)
==========================================================================================================

[Ask Question](/questions/ask)

Asked 11 years, 1 month ago

Active [2 months ago](?lastactivity "2020-07-16 01:07:36Z")

Viewed 1.7m times

This question shows research effort; it is useful and clear

1406

This question does not show any research effort; it is unclear or not useful

498

Bookmark this question.

[](/posts/1251999/timeline)

Show activity on this post.

How can I replace a newline ("`\n`") with a space ("") using the `sed` command?

I unsuccessfully tried:

```
sed 's#\n# #g' file
sed 's#^$# #g' file

```

How do I fix it?

[sed](/questions/tagged/sed "show questions tagged 'sed'")

[edited Aug 12 '19 at 15:44](/posts/1251999/revisions "show all edits to this post")


![](https://www.gravatar.com/avatar/0385d5a1c27a6707139e7dc7ad29b6d6?s=32&d=identicon&r=PG&f=1)

[agc](/users/6136214/agc)

6,475 1 gold badge 23 silver badges 41 bronze badges

asked Aug 9 '09 at 19:10

![](https://www.gravatar.com/avatar/8b9388a7e480b3eb53b992ce8683729d?s=32&d=identicon&r=PG&f=1)

[hhh](/users/164148/hhh)

41.5k 52 gold badges 141 silver badges 241 bronze badges

*   30
    
    `tr` is only the right tool for the job if replace a single character for a single character, while the example above shows replace newline with a space.. So in the above example, tr could work.. But would be limiting later on. – [Angry 84](/users/1438265/angry-84 "2,545 reputation") [Dec 31 '15 at 2:47](#comment56822763_1251999)
    
*   10
    
    `tr` in the right tool for the job because the questioner wanted to replace each newline with a space, as shown in his example. The replacement of newlines is uniquely arcane for `sed` but easily done by `tr`. This is a common question. Performing regex replacements is not done by `tr` but by `sed`, which would be the right tool... for a different question. – [Mike S](/users/3768749/mike-s "960 reputation") [Dec 28 '16 at 15:01](#comment69930516_1251999)
    
*   3
    
    "tr" can also just delete the newline ` tr -d '\\n' ` however you may also like to delete returns to be more universal ` tr -d '\\012\\015' `. – [anthony](/users/701532/anthony "7,155 reputation") [Feb 27 '17 at 23:44](#comment72134777_1251999)
    
*   2
    
    WARNING: "tr" acts differently with regards to a character ranges between Linux and older Solaris machines (EG sol5.8). EG: ` tr -d 'a-z' ` and ` tr -d '\[a-z\]' `. For that I recommend you use "sed" which doesn't have that difference. – [anthony](/users/701532/anthony "7,155 reputation") [Feb 27 '17 at 23:45](#comment72134797_1251999)
    
*   2
    
    @MikeS Thanks for the answer. Follow `tr '\012' ' '` with an `echo`. Otherwise the last linefeed in the file is deleted, too. `tr '\012' ' ' < filename; echo`does the trick. – [Bernie Reiter](/users/2700897/bernie-reiter "76 reputation") [Dec 28 '17 at 23:30](#comment83000548_1251999)

42 Answers 42
=============


## This answer is useful

# 1555

Use this solution with GNU `sed`:

```
sed ':a;N;$!ba;s/\n/ /g' file

```

This will read the whole file in a loop, then replaces the newline(s) with a space.

Explanation:

1.  Create a label via `:a`.
2.  Append the current and next line to the pattern space via `N`.
3.  If we are before the last line, branch to the created label `$!ba` (`$!` means not to do it on the last line as there should be one final newline).
4.  Finally the substitution replaces every newline with a space on the pattern space (which is the whole file).

Here is cross-platform compatible syntax which works with BSD and OS X's `sed` (as per [@Benjie comment](https://stackoverflow.com/questions/1251999/how-can-i-replace-a-newline-n-using-sed?page=1&tab=votes#comment9175314_1252191)):

```
sed -e ':a' -e 'N' -e '$!ba' -e 's/\n/ /g' file

```

As you can see, using `sed` for this otherwise simple problem is problematic. For a simpler and adequate solution see [this answer](https://stackoverflow.com/a/1252010/1638350).

[edited Sep 23 '18 at 17:56](/posts/1252191/revisions "show all edits to this post")

![](https://i.stack.imgur.com/qk9vC.jpg?s=32&g=1)

[jpp](/users/9209546/jpp)

124k 23 gold badges 154 silver badges 204 bronze badges

answered Aug 9 '09 at 20:26

![](https://www.gravatar.com/avatar/b9d1981ac223df907b7ef285c3d92031?s=32&d=identicon&r=PG)

[Zsolt Botykai](/users/11621/zsolt-botykai)

44.3k 11 gold badges 80 silver badges 101 bronze badges

*   110
    
    You can run this cross-platform (i.e. on Mac OS X) by separately executing the commands rather than separating with semi-colons: `sed -e ':a' -e 'N' -e '$!ba' -e 's/\n/ /g'` – [Benjie](/users/141284/benjie "6,765 reputation") [Sep 27 '11 at 11:16](#comment9175314_1252191)
    
*   It seems not to remove the last \\n ? – [Pierre-Gilles Levallois](/users/2162529/pierre-gilles-levallois "3,527 reputation") [Aug 4 at 10:55](#comment111836771_1252191)

## This answer is useful

# 1751

`sed` is intended to be used on line-based input. Although it can do what you need.

* * *

A better option here is to use the `tr` command as follows:

```
tr '\n' ' ' < input_filename

```

or remove the newline characters entirely:

```
tr -d '\n' < input.txt > output.txt

```

or if you have the GNU version (with its long options)

```
tr --delete '\n' < input.txt > output.txt

```

[edited Sep 3 '19 at 1:35](/posts/1252010/revisions "show all edits to this post")

![](https://www.gravatar.com/avatar/c1386d0e6a382d1b5cbb44652736cb25?s=32&d=identicon&r=PG)

[Ayman Salah](/users/1993396/ayman-salah)

819 9 silver badges 30 bronze badges

answered Aug 9 '09 at 19:16

![](https://www.gravatar.com/avatar/79ed928ca8b1ab9a637720bf0fb20732?s=32&d=identicon&r=PG&f=1)

[dmckee --- ex-moderator kitten](/users/2509/dmckee-ex-moderator-kitten)dmckee --- ex-moderator kitten

87.6k 23 gold badges 127 silver badges 219 bronze badges

*   I cannot understand why sed cannot do it. Please, clarify to use different tool. – [Léo Léopold Hertz 준영](/users/54964/l%c3%a9o-l%c3%a9opold-hertz-%ec%a4%80%ec%98%81 "113,793 reputation") [Aug 9 '09 at 19:18](#comment1078863_1252010)
    
*   90
    
    Sed is line-based therefore it is hard for it to grasp newlines. – [Alexander Gladysh](/users/6236/alexander-gladysh "32,174 reputation") [Aug 9 '09 at 19:22](#comment1078869_1252010)
    
*   1
    
    Alexander: Does "stream editor" mean line-based? Perhaps, the name is confusing. – [Léo Léopold Hertz 준영](/users/54964/l%c3%a9o-l%c3%a9opold-hertz-%ec%a4%80%ec%98%81 "113,793 reputation") [Aug 9 '09 at 19:26](#comment1078879_1252010)
    
*   194
    
    sed works on a "stream" of input, but it comprehends it in newline delimited chunks. It is a unix tool, which means it does one thing very well. The one thing is "work on a file line-wise". Making it do something else will be hard, and risks being buggy. The moral of the story is: choose the right tool. A great many of your questions seem to take the form "How can I make this tool do something it was never meant to do?" Those questions are interesting, but if they come up in the course of solving a real problem, you're probably doing it wrong. – [dmckee --- ex-moderator kitten](/users/2509/dmckee-ex-moderator-kitten "87,574 reputation") [Aug 9 '09 at 19:39](#comment1078907_1252010)
    

## This answer is useful

# 514

### Fast answer

```
sed ':a;N;$!ba;s/\n/ /g' file 
```

1.  **:a** _create a label 'a'_
2.  **N** _append the next line to the pattern space_
3.  **$!** _if not the last line_, **ba** _branch (go to) label 'a'_
4.  **s** _substitute_, **/\\n/** _regex for new line_, **/ /** _by a space_, **/g** _global match (as many times as it can)_

sed will loop through step 1 to 3 until it reach the last line, getting all lines fit in the pattern space where sed will substitute all \\n characters

* * *

### Alternatives

All alternatives, unlike _sed_ will not need to reach the last line to begin the process

with **bash**, slow

```
while read line; do printf "%s" "$line "; done < file 
```

with **perl**, _sed_-like speed

```
perl -p -e 's/\n/ /' file 
```

with **tr**, faster than _sed_, can replace by one character only

```
tr '\n' ' ' < file 
```

with **paste**, _tr_-like speed, can replace by one character only

```
paste -s -d ' ' file 
```

with **awk**, _tr_-like speed

```
awk 1 ORS=' ' file 
```

Other alternative like _"echo $(< file)"_ is slow, works only on small files and needs to process the whole file to begin the process.

* * *

### Long answer from the [sed FAQ 5.10](http://sed.sourceforge.net/sedfaq5.html#s5.10)

5.10. Why can't I match or delete a newline using the \\n escape  
sequence? Why can't I match 2 or more lines using \\n?

The \\n will never match the newline at the end-of-line because the  
newline is always stripped off before the line is placed into the  
pattern space. To get 2 or more lines into the pattern space, use  
the 'N' command or something similar (such as 'H;...;g;').

Sed works like this: sed reads one line at a time, chops off the  
terminating newline, puts what is left into the pattern space where  
the sed script can address or change it, and when the pattern space  
is printed, appends a newline to stdout (or to a file). If the  
pattern space is entirely or partially deleted with 'd' or 'D', the  
newline is _not_ added in such cases. Thus, scripts like

```
 sed 's/\n//' file       # to delete newlines from each line 
  sed 's/\n/foo\n/' file  # to add a word to the end of each line 
```

will NEVER work, because the trailing newline is removed _before_  
the line is put into the pattern space. To perform the above tasks,  
use one of these scripts instead:

```
 tr -d '\n' < file              # use tr to delete newlines 
  sed ':a;N;$!ba;s/\n//g' file   # GNU sed to delete newlines 
  sed 's/$/ foo/' file           # add "foo" to end of each line 
```

Since versions of sed other than GNU sed have limits to the size of  
the pattern buffer, the Unix 'tr' utility is to be preferred here.  
If the last line of the file contains a newline, GNU sed will add  
that newline to the output but delete all others, whereas tr will  
delete all newlines.

To match a block of two or more lines, there are 3 basic choices:  
(1) use the 'N' command to add the Next line to the pattern space;  
(2) use the 'H' command at least twice to append the current line  
to the Hold space, and then retrieve the lines from the hold space  
with x, g, or G; or (3) use address ranges (see section 3.3, above)  
to match lines between two specified addresses.

Choices (1) and (2) will put an \\n into the pattern space, where it  
can be addressed as desired ('s/ABC\\nXYZ/alphabet/g'). One example  
of using 'N' to delete a block of lines appears in section 4.13  
("How do I delete a block of _specific_ consecutive lines?"). This  
example can be modified by changing the delete command to something  
else, like 'p' (print), 'i' (insert), 'c' (change), 'a' (append),  
or 's' (substitute).

Choice (3) will not put an \\n into the pattern space, but it _does_  
match a block of consecutive lines, so it may be that you don't  
even need the \\n to find what you're looking for. Since GNU sed  
version 3.02.80 now supports this syntax:

```
 sed '/start/,+4d'  # to delete "start" plus the next 4 lines, 
```

in addition to the traditional '/from here/,/to there/{...}' range  
addresses, it may be possible to avoid the use of \\n entirely.

[edited Jun 20 at 9:12](/posts/7697604/revisions "show all edits to this post")

![](https://www.gravatar.com/avatar/a007be5a61f6aa8f3e85ae2fc18dd66e?s=32&d=identicon&r=PG)

[Community](/users/-1/community)♦

1 1 silver badge

answered Oct 8 '11 at 14:55

![](https://www.gravatar.com/avatar/3b9b57b48fe54f012eab5a455b0c7aa7?s=32&d=identicon&r=PG)

[hdorio](/users/111785/hdorio)

10.9k 5 gold badges 30 silver badges 32 bronze badges

*   Thanks for the paste command. I didn't know that one. I have a file with some URLs and I wanted to open them all at once in Chrome browser. So I did (on Mac OS X): `paste -s -d ' ' url.txt | xargs open` Works great. – [xlttj](/users/449436/xlttj "1,048 reputation") [Jan 7 '12 at 14:36](#comment10932259_7697604)
    
*   @xl-t you should use `xargs -a url.txt open` instead – [hdorio](/users/111785/hdorio "10,944 reputation") [Feb 9 '12 at 23:28](#comment11609926_7697604)
    
*   Good answer. There is a slight problem in the bash alternative. It will fail on lines which are interpreted as switches to echo. This is why I never use echo to print variable data. Use `printf '%s' "$line "` instead. – [Pianosaurus](/users/44680/pianosaurus "4,559 reputation") [Jun 28 '12 at 17:03](#comment14783678_7697604)
    
*   8
    
    `tr` was a great idea, and your overall coverage makes for a top-quality answer. – [New Alexandria](/users/263858/new-alexandria "6,066 reputation") [Jan 6 '13 at 17:57](#comment19660629_7697604)
    
*   1
    
    +1 for using ([standard utility](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/paste.html)) `paste`... and all the others! – [Totor](/users/1176782/totor "737 reputation") [Mar 15 '13 at 11:10](#comment21824255_7697604)
    
*   Does not work with colored output. I tried everything on this Q&A and nothing worked when the output is ansi colored. Tested on Ubuntu 13.04 – [Leo Gallucci](/users/511069/leo-gallucci "14,635 reputation") [Dec 9 '13 at 22:51](#comment30611913_7697604)
    
*   1
    
    @elgalu try this [unix.stackexchange.com/questions/4527/…](http://unix.stackexchange.com/questions/4527/program-that-passes-stdin-to-stdout-with-color-codes-stripped "program that passes stdin to stdout with color codes stripped") – [hdorio](/users/111785/hdorio "10,944 reputation") [Dec 11 '13 at 9:36](#comment30669787_7697604)
    
*   4
    
    The best part about this answer is that the "long answer" explains exactly how and why the command works. – [pdwalker](/users/363631/pdwalker "763 reputation") [May 2 '14 at 11:20](#comment35902193_7697604)
    
*   4
    
    This may be the most helpful of the thousands of answers I've read on stackexchange. I need to match multiple characters across lines. No previous sed examples covered multi-lines and tr can't handle multiple character matching. Perl looks good, but isn't working as I expect. I'd vote this answer up several times if I could. – [mightypile](/users/2846766/mightypile "5,665 reputation") [Jun 29 '15 at 23:37](#comment50266825_7697604)
    
*   I needed to replace with multiple characters, and I really enjoyed the depth and organization of this answer. My sample command was this one: `awk 1 ORS='\\\n'`. Read what the `1` means from Thor's answer, the true condition of the awk program simply causing each line of input to be printed. – [Pysis](/users/1091943/pysis "972 reputation") [Nov 15 '17 at 14:17](#comment81569686_7697604)

## This answer is useful

# 227

A shorter awk alternative:

```
awk 1 ORS=' '

```

### Explanation

An awk program is built up of rules which consist of conditional code-blocks, i.e.:

```
condition { code-block }

```

If the code-block is omitted, the default is used: `{ print $0 }`. Thus, the `1` is interpreted as a true condition and `print $0` is executed for each line.

When `awk` reads the input it splits it into records based on the value of `RS` (Record Separator), which by default is a newline, thus `awk` will by default parse the input line-wise. The splitting also involves stripping off `RS` from the input record.

Now, when printing a record, `ORS` (Output Record Separator) is appended to it, default is again a newline. So by changing `ORS` to a space all newlines are changed to spaces.

[edited Oct 2 '18 at 13:00](/posts/14853319/revisions "show all edits to this post")

answered Feb 13 '13 at 12:12

![](https://www.gravatar.com/avatar/4a22763d295c9d45db0dd0c692bb4051?s=32&d=identicon&r=PG)

[Thor](/users/1331399/thor)

36.5k 8 gold badges 98 silver badges 111 bronze badges

*   5
    
    I like a lot this simple solution, which is much more readable, than others – [Fedir RYKHTIK](/users/634275/fedir-rykhtik "8,704 reputation") [Jul 30 '13 at 13:29](#comment26229057_14853319)
    
*   9
    
    If it makes more sense, this could effectively be written as: `awk 'BEGIN { ORS=" " } { print $0 } END { print "\n"} ' file.txt` (adding an ending newline just to illustrate begin/end); the "1" evaluates to `true` (process the line) and `print` (print the line). A conditional could also be added to this expression, e.g., only working on lines matching a pattern: `awk 'BEGIN { ORS=" " } /pattern/ { print $0 } END { print "\n"} '` – [michael](/users/127971/michael "6,279 reputation") [Sep 19 '14 at 1:22](#comment40583093_14853319)
    
*   2
    
    You can do it more simle: `code` awk 'ORS=" "' file.txt `code` – [Udi](/users/2294644/udi "117 reputation") [Mar 28 '18 at 15:52](#comment86085101_14853319)
    
*   When using awk like this then, unfortunately, the last line feed in the file is deleted, too. See Patrick Dark answer above about using 'tr' in a subshell like ` cat file | echo $(tr "\\012" " ") ` which does the trick. Nifty. – [Bernie Reiter](/users/2700897/bernie-reiter "76 reputation") [Mar 28 '19 at 11:54](#comment97514103_14853319)
    

## This answer is useful

# 151

gnu sed has an option `-z` for null separated records (lines). You can just call:

```
sed -z 's/\n/ /g'

```

[edited Mar 16 '18 at 10:05](/posts/30049447/revisions "show all edits to this post")

answered May 5 '15 at 9:43

![](https://www.gravatar.com/avatar/b1bf3f42fc00dd825eda62f1df4fa304?s=32&d=identicon&r=PG)

[JJoao](/users/2991627/jjoao)

3,503 1 gold badge 13 silver badges 19 bronze badges

*   5
    
    Even if the input does contain nulls, they will be preserved (as record delimiters). – [Toby Speight](/users/4850040/toby-speight "22,126 reputation") [Feb 16 '16 at 17:42](#comment58576675_30049447)
    
*   6
    
    Won't this load the whole input if there're no nulls? In this case processing a multi-gigabyte file may be crashy. – [Ruslan](/users/673852/ruslan "13,484 reputation") [Mar 4 '16 at 9:43](#comment59255210_30049447)
    
*   4
    
    @Ruslan, yes it loads the whole input. This solution is not a good idea for multi-gigabyte files. – [JJoao](/users/2991627/jjoao "3,503 reputation") [Mar 4 '16 at 9:50](#comment59255486_30049447)
    
*   11
    
    This is seriously the **best** answer. The other expressions are too contorted to remember. @JJoao You can use it with `-u, --unbuffered`. The `man` mage states: "load minimal amounts of data from the input files and flush the output buffers more often". – [not2qubit](/users/1147688/not2qubit "8,697 reputation") [Feb 21 '18 at 21:10](#comment84837423_30049447)
    
*   1
    
    @Ruslan If you have a multi-gigabyte textfile, you don't want to use `sed` anyway, even in line-based mode, as `sed` is annoying slow on large input. – [vog](/users/19163/vog "16,055 reputation") [Nov 26 '19 at 23:19](#comment104360910_30049447)
    

## This answer is useful

# 87

The [Perl](http://en.wikipedia.org/wiki/Perl) version works the way you expected.

```
perl -i -p -e 's/\n//' file

```

As pointed out in the comments, it's worth noting that this edits in place. `-i.bak` will give you a backup of the original file before the replacement in case your [regular expression](http://en.wikipedia.org/wiki/Regular_expression) isn't as smart as you thought.

[edited Mar 31 '15 at 8:09](/posts/1252020/revisions "show all edits to this post")

![](https://i.stack.imgur.com/RIZKi.png?s=32&g=1)

[Peter Mortensen](/users/63550/peter-mortensen)

26.5k 21 gold badges 92 silver badges 122 bronze badges

answered Aug 9 '09 at 19:25

![](https://www.gravatar.com/avatar/42d46e3315293e801832c87d54c96eb3?s=32&d=identicon&r=PG)

[ire\_and\_curses](/users/143804/ire-and-curses)

62.5k 22 gold badges 109 silver badges 135 bronze badges

*   23
    
    Please at least mention that `-i` without a suffix makes _no backup_. `-i.bak` protects you from an easy, ugly mistake (say, forgetting to type `-p` and zeroing out the file). – [Telemachus](/users/26702/telemachus "18,136 reputation") [Aug 9 '09 at 20:31](#comment1079000_1252020)
    
*   6
    
    @Telemachus: It's a fair point, but it can be argued either way. The main reason I didn't mention it is that the sed example in the OP's question doesn't make backups, so it seems superfluous here. The other reason is that I've never actually used the backup functionality (I find automatic backups annoying, actually), so I always forget it's there. The third reason is it makes my command line four characters longer. For better or worse (probably worse), I'm a compulsive minimalist; I just prefer brevity. I realise you don't agree. I will try my best to remember to warn about backups in future. – [ire\_and\_curses](/users/143804/ire-and-curses "62,537 reputation") [Aug 9 '09 at 20:53](#comment1079069_1252020)
    
*   6
    
    @Ire\_and\_curses: Actually, you just made a damn good argument for ignoring me. That is, you have reasons for your choices, and whether or not I agree with the choices, I certainly respect that. I'm not sure entirely why, but I've been on a tear about this particular thing lately (the `-i` flag in Perl without a suffix). I'm sure I'll find something else to obsess about soon enough. :) – [Telemachus](/users/26702/telemachus "18,136 reputation") [Aug 9 '09 at 21:36](#comment1079203_1252020)
    
*   It's really unfortunate that this doesn't work with stdin by specifying `-` for filename. Is there a way to do that? That's my go-to way to not worry about modifying a file is using a pipeline that starts with cat. – [Steven Lu](/users/340947/steven-lu "34,859 reputation") [Apr 10 '19 at 2:57](#comment97903447_1252020)
    
*   @StevenLu Perl will read from STDIN by default if no filenames are provided. So you could do e.g. `perl -i -p -e 's/\n//' < infile > outfile` – [ire\_and\_curses](/users/143804/ire-and-curses "62,537 reputation") [Apr 10 '19 at 15:47](#comment97926554_1252020)
    
## This answer is useful

# 47

Who needs `sed`? Here is the `bash` way:

```
cat test.txt |  while read line; do echo -n "$line "; done

```

[edited Aug 8 '14 at 9:27](/posts/3458034/revisions "show all edits to this post")

![](https://i.stack.imgur.com/2NIGA.png?s=32&g=1)

[xav](/users/1149528/xav)

4,566 7 gold badges 39 silver badges 54 bronze badges

answered Aug 11 '10 at 12:07

![](https://i.stack.imgur.com/dIVbe.jpg?s=32&g=1)

[commonpike](/users/95733/commonpike)

7,457 1 gold badge 53 silver badges 53 bronze badges

*   3
    
    Upvote, I normally used the top answer, but when piping /dev/urandom through it, sed won't print until EOF, and ^C is no EOF. This solution prints every time it sees a newline. Exactly what I needed! Thanks! – [Vasiliy Sharapov](/users/399257/vasiliy-sharapov "909 reputation") [Feb 9 '11 at 17:34](#comment5519157_3458034)
    
*   1
    
    then why not: echo -n \`cat days.txt\` [From this post](http://linux.dsplabs.com.au/rmnl-remove-new-line-characters-tr-awk-perl-sed-c-cpp-bash-python-xargs-ghc-ghci-haskell-sam-ssam-p65/) – [Tony](/users/328237/tony "788 reputation") [Sep 26 '12 at 23:51](#comment17002301_3458034)
    
*   9
    
    @Tony because backticks are deprecated and the cat is redundant ;-) Use: echo $(<days.txt) – [seumasmac](/users/1455936/seumasmac "1,149 reputation") [Feb 7 '13 at 16:03](#comment20651287_3458034)
    
*   11
    
    Without even using `cat`: `while read line; do echo -n "$line "; done < test.txt`. Might be useful if a sub-shell is a problem. – [Carlo Cannas](/users/3023877/carlo-cannas "2,981 reputation") [Jan 20 '14 at 17:42](#comment31996416_3458034)
    
*   5
    
    `echo $(<file)` squeezes **all** whitespace to a single space, not just newlines: this goes beyond what the OP is asking. – [glenn jackman](/users/7552/glenn-jackman "195,142 reputation") [Mar 18 '15 at 18:46](#comment46486240_3458034)
    
## This answer is useful

# 28

In order to replace all newlines with spaces using awk, without reading the whole file into memory:

```
awk '{printf "%s ", $0}' inputfile

```

If you want a final newline:

```
awk '{printf "%s ", $0} END {printf "\n"}' inputfile

```

You can use a character other than space:

```
awk '{printf "%s|", $0} END {printf "\n"}' inputfile

```
answered Mar 30 '12 at 6:41

![](https://i.stack.imgur.com/S1BbQ.png?s=32&g=1)

[Paused until further notice.](/users/26428/paused-until-further-notice)

286k 81 gold badges 340 silver badges 409 bronze badges

*   `END{ print ""}` is a shorter alternative for a trailing newline. – [Isaac](/users/8017719/isaac "5,121 reputation") [Dec 15 '19 at 23:41](#comment104894015_9938125)


## This answer is useful

# 22


```
tr '\n' ' ' 

```

is the command.

Simple and easy to use.

[edited May 1 '14 at 13:54](/posts/23047065/revisions "show all edits to this post")

![](https://i.stack.imgur.com/eKpAk.png?s=32&g=1)

[fedorqui 'SO stop harming'](/users/1983854/fedorqui-so-stop-harming)

212k 73 gold badges 430 silver badges 485 bronze badges

answered Apr 13 '14 at 19:01

![](https://www.gravatar.com/avatar/f4f30c54d08ae8991e7f16f8943ba086?s=32&d=identicon&r=PG)

[Dheeraj R](/users/1804511/dheeraj-r)

630 8 silver badges 17 bronze badges

*   16
    
    or simply `tr -d '\n'` if you don't want to add a space – [spuder](/users/1626687/spuder "12,846 reputation") [Oct 15 '14 at 23:26](#comment41439993_23047065)


## This answer is useful

# 21

Three things.

1.  `tr` (or `cat`, etc.) is absolutely not needed. (GNU) `sed` and (GNU) `awk`, when combined, can do 99.9% of any text processing you need.
    
2.  stream != line based. `ed` is a line-based editor. `sed` is not. See [sed lecture](http://web.eecs.utk.edu/~cs300/Sed/lecture.html) for more information on the difference. Most people confuse `sed` to be line-based because it is, by default, not very greedy in its pattern matching for SIMPLE matches - for instance, when doing pattern searching and replacing by one or two characters, it by default only replaces on the first match it finds (unless specified otherwise by the global command). There would not even be a global command if it were line-based rather than STREAM-based, because it would evaluate only lines at a time. Try running `ed`; you'll notice the difference. `ed` is pretty useful if you want to iterate over specific lines (such as in a for-loop), but most of the times you'll just want `sed`.
    
3.  That being said,
    
    ```
    sed -e '{:q;N;s/\n/ /g;t q}' file
    
    ```
    
    works just fine in GNU `sed` version 4.2.1. The above command will replace all newlines with spaces. It's ugly and a bit cumbersome to type in, but it works just fine. The `{}`'s can be left out, as they're only included for sanity reasons.
    

[edited Jul 1 '15 at 15:15](/posts/5847965/revisions "show all edits to this post")

![](https://www.gravatar.com/avatar/c0d41500d324e993d127170ec6274580?s=32&d=identicon&r=PG)

[7ochem](/users/1306684/7ochem)

1,945 1 gold badge 26 silver badges 35 bronze badges

answered May 1 '11 at 11:04


![](https://i.stack.imgur.com/V7tVj.jpg?s=32&g=1)

[brent saner](/users/733214/brent-saner)

418 1 gold badge 5 silver badges 8 bronze badges

*   5
    
    As a person who only knows enough `sed` to do basic stuff, I have to say it's more than about what you _can_ do with `sed` but rather how easy it is to understand what is going on. I have a very hard time working with `sed` so I would prefer a simpler command when I can use it. – [Nate](/users/761771/nate "11,298 reputation") [Mar 4 '14 at 19:15](#comment33667326_5847965)
    
*   Using `t q` as conditional jump this works with a pattern like `s/\n / /` (to join all lines which begin with a space) without reading the whole file into memory. Handy when transforming multi megabyte files. – [textshell](/users/4973666/textshell "908 reputation") [Jun 26 '15 at 11:17](#comment50162303_5847965)
    
*   The article you've linked does not reflect what you are saying – [hek2mgl](/users/171318/hek2mgl "126,231 reputation") [Mar 29 '16 at 22:33](#comment60217518_5847965)
    
*   1
    
    This is almost 800 times slower than the accepted answer on large input. This is due to running substitute for every line on increasingly larger input. – [Thor](/users/1331399/thor "36,502 reputation") [Nov 3 '16 at 20:17](#comment68071108_5847965)

## This answer is useful

# 13

The answer with the :a label ...

[How can I replace a newline (\\n) using sed?](https://stackoverflow.com/questions/1251999/sed-how-can-i-replace-a-newline-n/1252191#1252191)

... does not work in freebsd 7.2 on the command line:

( echo foo ; echo bar ) | sed ':a;N;$!ba;s/\\n/ /g'
sed: 1: ":a;N;$!ba;s/\\n/ /g": unused label 'a;N;$!ba;s/\\n/ /g'
foo
bar

But does if you put the sed script in a file or use -e to "build" the sed script...

\> (echo foo; echo bar) | sed -e :a -e N -e '$!ba' -e 's/\\n/ /g'
foo bar

or ...

```
> cat > x.sed << eof
:a
N
$!ba
s/\n/ /g
eof

> (echo foo; echo bar) | sed -f x.sed
foo bar

```

Maybe the sed in OS X is similar.

[edited May 23 '17 at 12:18](/posts/1474512/revisions "show all edits to this post")

![](https://www.gravatar.com/avatar/a007be5a61f6aa8f3e85ae2fc18dd66e?s=32&d=identicon&r=PG)

[Community](/users/-1/community)♦

1 1 silver badge

answered Sep 24 '09 at 22:26

![](https://www.gravatar.com/avatar/f53f42a7048e675d4b24a3a2bc8f5acc?s=32&d=identicon&r=PG)

[Juan](/users/178750/juan)

615 5 silver badges 19 bronze badges

*   The series of -e arguments worked for me on windows using MKS! Thanks! – [JamesG](/users/54449/jamesg "750 reputation") [Sep 7 '11 at 19:26](#comment8851892_1474512)

## This answer is useful

# 13

You can use [xargs](https://en.wikipedia.org/wiki/Xargs):

```
seq 10 | xargs

```

or

```
seq 10 | xargs echo -n

```

[edited Mar 31 '15 at 16:38](/posts/23704686/revisions "show all edits to this post")

![](https://i.stack.imgur.com/RIZKi.png?s=32&g=1)

[Peter Mortensen](/users/63550/peter-mortensen)

26.5k 21 gold badges 92 silver badges 122 bronze badges

answered May 16 '14 at 21:18

![](https://www.gravatar.com/avatar/2ea812af0f6e951eeebf4157c62ab9bb?s=32&d=identicon&r=PG)

[Vytenis Bivainis](/users/815741/vytenis-bivainis)

1,872 13 silver badges 21 bronze badges

*   Work for me: `xargs < file.txt` – [Udi](/users/2294644/udi "117 reputation") [Mar 28 '18 at 16:31](#comment86086734_23704686)
    
## This answer is useful

# 12

Easy-to-understand Solution
===========================

I had this problem. The kicker was that I needed the solution to work on BSD's (Mac OS X) and GNU's (Linux and [Cygwin](http://en.wikipedia.org/wiki/Cygwin)) `sed` and `tr`:

```
$ echo 'foo
bar
baz


foo2
bar2
baz2' \
| tr '\n' '\000' \
| sed 's:\x00\x00.*:\n:g' \
| tr '\000' '\n'

```

Output:

```
foo
bar
baz

```

(has trailing newline)

**It works on Linux, OS X, and BSD** \- even without [UTF-8](http://en.wikipedia.org/wiki/UTF-8) support or with a crappy terminal.

1.  Use `tr` to swap the newline with another character.
    
    `NULL` (`\000` or `\x00`) is nice because it doesn't need UTF-8 support and it's not likely to be used.
    
2.  Use `sed` to match the `NULL`
    
3.  Use `tr` to swap back extra newlines if you need them
    

[edited Mar 31 '15 at 16:33](/posts/8997314/revisions "show all edits to this post")

![](https://i.stack.imgur.com/RIZKi.png?s=32&g=1)

[Peter Mortensen](/users/63550/peter-mortensen)

26.5k 21 gold badges 92 silver badges 122 bronze badges

answered Jan 25 '12 at 2:52

![](https://www.gravatar.com/avatar/0a8b345ddcfc5401f578c850442f1e1b?s=32&d=identicon&r=PG)

[coolaj86](/users/151312/coolaj86)

60.2k 14 gold badges 83 silver badges 101 bronze badges

*   1
    
    A subtle note on nomenclature: the character `\000` is commonly referred to as `NUL` (one L), and `NULL` is generally used when talking about a zero-_pointer_ (in C/C++). – [sqweek](/users/1976730/sqweek "1,021 reputation") [Feb 29 '16 at 8:59](#comment59067705_8997314)
    

[add a comment](# "Use comments to ask for more information or suggest improvements. Avoid comments like “+1” or “thanks”.")  | [](# "expand to show all comments on this post")

This answer is useful

9

This answer is not useful

[](/posts/1252180/timeline)

Show activity on this post.

I'm not an expert, but I guess in `sed` you'd first need to append the next line into the pattern space, bij using "`N`". From the section "Multiline Pattern Space" in "Advanced sed Commands" of the book [sed & awk](http://oreilly.com/catalog/9781565922259/) (Dale Dougherty and Arnold Robbins; O'Reilly 1997; page 107 in [the preview](http://oreilly.com/catalog/9781565922259/preview.html)):

> The multiline Next (N) command creates a multiline pattern space by reading a new line of input and appending it to the contents of the pattern space. The original contents of pattern space and the new input line are separated by a newline. The embedded newline character can be matched in patterns by the escape sequence "\\n". In a multiline pattern space, the metacharacter "^" matches the very first character of the pattern space, and not the character(s) following any embedded newline(s). Similarly, "$" matches only the final newline in the pattern space, and not any embedded newline(s). After the Next command is executed, control is then passed to subsequent commands in the script.

From `man sed`:

> \[2addr\]N
> 
> Append the next line of input to the pattern space, using an embedded newline character to separate the appended material from the original contents. Note that the current line number changes.

I've [used this](https://stackoverflow.com/questions/1102986/most-powerful-examples-of-unix-commands-or-scripts-every-programmer-should-know/1103426#1103426) to search (multiple) badly formatted log files, in which the search string may be found on an "orphaned" next line.

[edited Jun 20 at 9:12](/posts/1252180/revisions "show all edits to this post")

![](https://www.gravatar.com/avatar/a007be5a61f6aa8f3e85ae2fc18dd66e?s=32&d=identicon&r=PG)

[Community](/users/-1/community)♦

111 silver badge

answered Aug 9 '09 at 20:23

![](https://i.stack.imgur.com/f4Ipf.png?s=32&g=1)

[Arjan](/users/84237/arjan)

19.4k 9 gold badges 55 silver badges 66 bronze badges

## This answer is useful

# 9

Why didn't I find a simple solution with `awk`?

```
awk '{printf $0}' file

```

`printf` will print the every line without newlines, if you want to separate the original lines with a space or other:

```
awk '{printf $0 " "}' file

```

answered Mar 15 at 20:24

![](https://i.stack.imgur.com/CEQho.png?s=32&g=1)

[Itachi](/users/1677041/itachi)

4,415 2 gold badges 28 silver badges 60 bronze badges

*   `echo "1\n2\n3" | awk '{printf $0}'`, this works for me. @edi9999 – [Itachi](/users/1677041/itachi "4,415 reputation") [Mar 19 at 9:39](#comment107490840_60697302)
    
*   this was the only approach that worked for me within git bash for windows – [Plato](/users/1380669/plato "9,536 reputation") [May 15 at 0:00](#comment109327660_60697302)
    
## This answer is useful

# 7

I used a hybrid approach to get around the newline thing by using tr to replace newlines with tabs, then replacing tabs with whatever I want. In this case, "  
" since I'm trying to generate HTML breaks.

```
echo -e "a\nb\nc\n" |tr '\n' '\t' | sed 's/\t/ <br> /g'`

```

[edited Mar 31 '15 at 16:33](/posts/9674117/revisions "show all edits to this post")

![](https://i.stack.imgur.com/RIZKi.png?s=32&g=1)

[Peter Mortensen](/users/63550/peter-mortensen)

26.5k 21 gold badges 92 silver badges 122 bronze badges

answered Mar 12 '12 at 20:13

![](https://www.gravatar.com/avatar/184820f25264be6b06f11cbaffa65a38?s=32&d=identicon&r=PG)

[rfengr](/users/1265035/rfengr)

79 1 silver badge 1 bronze badge

This answer is useful

6

In response to the "tr" solution above, on Windows (probably using the Gnuwin32 version of tr), the proposed solution:

```
tr '\n' ' ' < input

```

was not working for me, it'd either error or actually replace the \\n w/ '' for some reason.

Using another feature of tr, the "delete" option -d did work though:

```
tr -d '\n' < input

```

or '\\r\\n' instead of '\\n'

answered Feb 25 '11 at 22:54

![](https://www.gravatar.com/avatar/dcafe95f214879ea02e64a4faaac87b2?s=32&d=identicon&r=PG)

[John Lawler](/users/634998/john-lawler)

61 1 silver badge 1 bronze badge

*   3
    
    On Windows, you probably need to use `tr "\n" " " < input`. The Windows shell (cmd.exe) doesn't treat the apostrophe as a quoting character. – [Keith Thompson](/users/827263/keith-thompson "220,773 reputation") [Aug 24 '11 at 14:25](#comment8615329_5123598)
    
*   No, in Windows 10 Ubuntu subsystem, you need to use `tr "\n\r" " " < input.txt > output.txt` – [user1491819](/users/1491819/user1491819 "1,650 reputation") [Sep 29 '17 at 3:55](#comment79917757_5123598)
    
*   This works on Windows 10 using Gnuwin32: `cat SourceFile.txt | tr --delete '\r\n' > OutputFile.txt` . Or, instead of Gnuwin32, use Gow (Gnu on Windows), [github.com/bmatzelle/gow/wiki](https://github.com/bmatzelle/gow/wiki) – [Alchemistmatt](/users/1179467/alchemistmatt "301 reputation") [Jun 13 '18 at 23:15](#comment88698510_5123598)

## This answer is useful

# 5

Bullet-proof solution. Binary-data-safe and POSIX-compliant, but slow.
----------------------------------------------------------------------

[POSIX sed](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/sed.html) requires input according to the [POSIX text file](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_397) and [POSIX line](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_206) definitions, so NULL-bytes and too long lines are not allowed and each line must end with a newline (including the last line). This makes it hard to use sed for processing arbitrary input data.

The following solution avoids sed and instead converts the input bytes to octal codes and then to bytes again, but intercepts octal code 012 (newline) and outputs the replacement string in place of it. As far as I can tell the solution is POSIX-compliant, so it should work on a wide variety of platforms.

```
od -A n -t o1 -v | tr ' \t' '\n\n' | grep . |
  while read x; do [ "0$x" -eq 012 ] && printf '<br>\n' || printf "\\$x"; done

```

POSIX reference documentation: [sh](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/sh.html), [shell command language](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html), [od](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/od.html), [tr](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/tr.html), [grep](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/grep.html), [read](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/read.html), [\[](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/test.html), [printf](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/printf.html).

Both `read`, `[`, and `printf` are built-ins in at least bash, but that is probably not guaranteed by POSIX, so on some platforms it could be that each input byte will start one or more new processes, which will slow things down. Even in bash this solution only reaches about 50 kB/s, so it's not suited for large files.

Tested on Ubuntu (bash, dash, and busybox), FreeBSD, and OpenBSD.

answered Apr 19 '14 at 6:15

![](https://www.gravatar.com/avatar/c52492ecf9231e5e2d8b5febb69b4371?s=32&d=identicon&r=PG)

[Håkon A. Hjortland](/users/1443699/h%c3%a5kon-a-hjortland)

441 5 silver badges 4 bronze badges

## This answer is useful

# 5

In some situations maybe you can change `RS` to some other string or character. This way, \\n is available for sub/gsub:

```
$ gawk 'BEGIN {RS="dn" } {gsub("\n"," ") ;print $0 }' file

```

The power of shell scripting is that if you do not know how to do it in one way you can do it in another way. And many times you have more things to take into account than make a complex solution on a simple problem.

Regarding the thing that gawk is slow... and reads the file into memory, I do not know this, but to me gawk seems to work with one line at the time and is very very fast (not that fast as some of the others, but the time to write and test also counts).

I process MB and even GB of data, and the only limit I found is line size.

[edited Jan 5 '16 at 8:31](/posts/7819090/revisions "show all edits to this post")

![](https://i.stack.imgur.com/UaQAA.jpg?s=32&g=1)

[slayedbylucifer](/users/1251660/slayedbylucifer)

20.1k 15 gold badges 82 silver badges 116 bronze badges

answered Oct 19 '11 at 9:20

![](https://www.gravatar.com/avatar/7112729a10f1ec814a3c28f825fc843b?s=32&d=identicon&r=PG)

[mor](/users/1002806/mor)

51 1 silver badge 1 bronze badge

## This answer is useful

# 5

If you are unfortunate enough to have to deal with windows line endings you need to remove the `\r` and the `\n`

```
tr '\r\n' ' ' < $input > $output

```

[edited Jul 16 at 1:07](/posts/55602356/revisions "show all edits to this post")

answered Apr 9 '19 at 22:47

![](https://i.stack.imgur.com/XNDKw.jpg?s=32&g=1)

[StevenWernerCS](/users/3390659/stevenwernercs)

458 5 silver badges 11 bronze badges

*   This replaces `[` with a space, and `\r` with a space, and `\n` with a space, and `]` with a space. `tr -d '\r\n' <file` would remove any `\r` or `\n` characters, but that is also not what is being asked. `tr -d '\r' <file` will remove any `\r` characters (regardless of whether they are adjacent to `\n`) which is probably closer to being useful as well as quite possibly correct for the OP's need (still assuming your `tr` understands this backslash notation). – [tripleee](/users/874188/tripleee "123,745 reputation") [Dec 29 '19 at 10:39](#comment105208337_55602356)
    
*   Thanks, fixed it. just don't put \[\], and tr does respect \\n & \\r as new line and returns. are there systems where tr doesn't? – [StevenWernerCS](/users/3390659/stevenwernercs "458 reputation") [Jul 16 at 1:08](#comment111278217_55602356)
    
*   They ase oretty ubiquitous these days, but I think I can remember systems where they didn't work (dinosaurs like HP-UX and AIX and Irix maybe?) – [tripleee](/users/874188/tripleee "123,745 reputation") [Jul 16 at 5:48](#comment111281750_55602356)
    

## This answer is useful

# 4

You could use `xargs` — it will replace `\n` with a space by default.

However, it would have problems if your input has any case of an `unterminated quote`, e.g. if the quote signs on a given line don't match.


answered Mar 28 '13 at 15:06

![](https://www.gravatar.com/avatar/1ab0e19086582d27abe75b0635f57f43?s=32&d=identicon&r=PG)

[cnst](/users/1122270/cnst)

20.2k 2 gold badges 69 silver badges 102 bronze badges

*   xargs also handles the last line nicely: – [AAAfarmclub](/users/712616/aaafarmclub "1,558 reputation") [Aug 22 '15 at 21:37](#comment52209601_15685295)
    

## This answer is useful

# 4

Finds and replaces using allowing \\n

```
sed -ie -z 's/Marker\n/# Marker Comment\nMarker\n/g' myfile.txt

```

answered Jul 17 '18 at 7:33


![](https://www.gravatar.com/avatar/5933943497cf3dc80cfce2634025a726?s=32&d=identicon&r=PG&f=1)


[Proximo](/users/111624/proximo)

4,173 8 gold badges 38 silver badges 55 bronze badges


## This answer is useful

# 3

On Mac OS X (using FreeBSD sed):

```
# replace each newline with a space
printf "a\nb\nc\nd\ne\nf" | sed -E -e :a -e '$!N; s/\n/ /g; ta'
printf "a\nb\nc\nd\ne\nf" | sed -E -e :a -e '$!N; s/\n/ /g' -e ta

```

answered Jul 28 '10 at 10:44

![](https://www.gravatar.com/avatar/df22200e12d539174dcff9334697460b?s=32&d=identicon&r=PG)


[bashfu](/users/404397/bashfu)

249 1 gold badge 3 silver badges 2 bronze badges

## This answer is useful

# 3

To remove empty lines:

```
sed -n "s/^$//;t;p;"

```

answered Jun 6 '11 at 14:42


![](https://www.gravatar.com/avatar/15fbc7eac0e190a249d7495cc9cdf156?s=32&d=identicon&r=PG)

[kralyk](/users/786102/kralyk)

3,486 1 gold badge 26 silver badges 28 bronze badges

*   This is for GNU Sed. In normal Sed, this gives `sed: 1: "s/^$//;t;p;": undefined label ';p;'`. – [Léo Léopold Hertz 준영](/users/54964/l%c3%a9o-l%c3%a9opold-hertz-%ec%a4%80%ec%98%81 "113,793 reputation") [Jun 8 '15 at 20:29](#comment49494520_6253727)

## This answer is useful

# 3

Using Awk:

```
awk "BEGIN { o=\"\" }  { o=o \" \" \$0 }  END { print o; }"

```

answered Jun 6 '11 at 15:15


![](https://www.gravatar.com/avatar/15fbc7eac0e190a249d7495cc9cdf156?s=32&d=identicon&r=PG)


[kralyk](/users/786102/kralyk)

3,486 1 gold badge 26 silver badges 28 bronze badges

*   2
You don't need to escape the quotation marks and dollar sign if you change the outer ones to single quotes. The letter "o" is usually considered a bad choice as a variable name since it can be confused with the digit "0". You also don't need to initialize your variable, it defaults to a null string. However, if you don't want an extraneous leading space: `awk '{s = s sp $0; sp = " "} END {print s}'`. However, see my answer for a way to use awk without reading the whole file into memory. – [Paused until further notice.](/users/26428/paused-until-further-notice "286,259 reputation") [Mar 30 '12 at 6:34](#comment12687926_6254168)
    
*   **Please** check out [Thor's answer](http://stackoverflow.com/a/14853319/2451238) instead. It is way more efficient, readable and just _better_ by all means to compared this approach (even though this _would_ work)! – [mschilli](/users/2451238/mschilli "1,780 reputation") [Sep 2 '13 at 17:03](#comment27336426_6254168)
    
*   Dude, I get it. No need to rub it in my face :-) Thor's answer is way above on the page anyway (which is right), so what do you care? – [kralyk](/users/786102/kralyk "3,486 reputation") [Sep 3 '13 at 13:28](#comment27363299_6254168)
    

## This answer is useful

# 3

A solution I particularly like is to append all the file in the hold space and replace all newlines at the end of file:

```
$ (echo foo; echo bar) | sed -n 'H;${x;s/\n//g;p;}'
foobar

```

However, someone said me the hold space can be finite in some sed implementations.


answered Jul 13 '11 at 14:35


![](https://www.gravatar.com/avatar/8cf3090cda1c829f3f8df0a3796fc60c?s=32&d=identicon&r=PG)


[brandizzi](/users/287976/brandizzi)brandizzi

22.6k 5 gold badges 90 silver badges 137 bronze badges

*   1
the replacement with an empty string in your answer conceals the fact that always using H to append to the hold space means that the hold space will start with a newline. To avoid this, you need to use `1h;2,$H;${x;s/\n/x/g;p}` – [Jeff](/users/1397947/jeff "1,528 reputation") [Jul 9 '16 at 6:34](#comment63976849_6680626)


## This answer is useful

# 3

Replace newlines with any string, and replace the last newline too
------------------------------------------------------------------

The pure `tr` solutions can only replace with a single character, and the pure `sed` solutions don't replace the last newline of the input. The following solution fixes these problems, and seems to be safe for binary data (even with a UTF-8 locale):

```
printf '1\n2\n3\n' |
  sed 's/%/%p/g;s/@/%a/g' | tr '\n' @ | sed 's/@/<br>/g;s/%a/@/g;s/%p/%/g'

```

Result:

```
1<br>2<br>3<br>

```

answered Feb 26 '13 at 14:15

![](https://www.gravatar.com/avatar/c52492ecf9231e5e2d8b5febb69b4371?s=32&d=identicon&r=PG)


[Håkon A. Hjortland](/users/1443699/h%c3%a5kon-a-hjortland)

441 5 silver badges 4 bronze badges

*   This is bad because it will produce unwanted output on any input containing `@` – [Steven Lu](/users/340947/steven-lu "34,859 reputation") [Jun 22 '13 at 17:38](#comment25005231_15091304)
    
*   @StevenLu: No, `@` in the input is OK. It gets escaped to `%a` and back again. The solution might not be completely POSIX compliant, though (NULL-bytes not allowed so not good for binary data, and all lines must end with newline so the `tr` output is not really valid). – [Håkon A. Hjortland](/users/1443699/h%c3%a5kon-a-hjortland "441 reputation") [Jun 27 '13 at 0:48](#comment25144152_15091304)
    
*   Ah. I see you've fixed it up. Kinda convoluted for what should be a simple operation, but good work. – [Steven Lu](/users/340947/steven-lu "34,859 reputation") [Jun 27 '13 at 2:48](#comment25145797_15091304)
    
## This answer is useful

# 3

It is **sed** that introduces the new-lines after "normal" substitution. First, it trims the new-line char, then it processes according to your instructions, then it introduces a new-line.

Using **sed** you can replace "the end" of a line (not the new-line char) after being trimmed, with a string of your choice, for each input line; but, **sed** will output different lines. For example, suppose you wanted to replace the "end of line" with "===" (more general than a replacing with a single space):

```
PROMPT~$ cat <<EOF |sed 's/$/===/g'
first line
second line
3rd line
EOF

first line===
second line===
3rd line===
PROMPT~$

```

To replace the new-line char with the string, you can, inefficiently though, use **tr** , as pointed before, to replace the newline-chars with a "special char" and then use **sed** to replace that special char with the string you want.

For example:

```
PROMPT~$ cat <<EOF | tr '\n' $'\x01'|sed -e 's/\x01/===/g'
first line
second line
3rd line
EOF

first line===second line===3rd line===PROMPT~$

```

[edited Apr 16 '14 at 5:47](/posts/23098268/revisions "show all edits to this post")

answered Apr 16 '14 at 3:06

![](https://www.gravatar.com/avatar/64cf3c314f8b0da9bdeb5ad41d29fc29?s=32&d=identicon&r=PG)


[Robert Vila](/users/3529240/robert-vila)

201 2 silver badges 2 bronze badges

## This answer is useful

# 3

You can use this method also

```
sed 'x;G;1!h;s/\n/ /g;$!d'

```

Explanation

```
x   - which is used to exchange the data from both space (pattern and hold).
G   - which is used to append the data from hold space to pattern space.
h   - which is used to copy the pattern space to hold space.
1!h - During first line won't copy pattern space to hold space due to \n is
      available in pattern space.
$!d - Clear the pattern space every time before getting next line until the
      last line.

```

**Flow:**  
When the first line get from the input, exchange is made, so 1 goes to hold space and \\n comes to pattern space, then appending the hold space to pattern space, and then substitution is performed and deleted the pattern space.  
During the second line exchange is made, 2 goes to hold space and 1 comes to pattern space, then `G` append the hold space into the pattern space, then `h` copy the pattern to it and substitution is made and deleted. This operation is continued until eof is reached then print exact result.  

[edited Jul 18 '14 at 19:26](/posts/24818721/revisions "show all edits to this post")

![](https://i.stack.imgur.com/0VZ1V.png?s=32&g=1)

[Léo Léopold Hertz 준영](/users/54964/l%c3%a9o-l%c3%a9opold-hertz-%ec%a4%80%ec%98%81)

114k 154 154 gold badges 410 410 silver badges 644 644 bronze badges

answered Jul 18 '14 at 6:44

![](https://i.stack.imgur.com/fMYyR.jpg?s=32&g=1)

[Kalanidhi](/users/2869795/kalanidhi)

3,982 18 silver badges 39 bronze badges

*   However, be warned that `echo 'Y' | sed 'x;G;1!h;s/\n/X/g;$!d'` results in `XY`. – [Spooky](/users/1159643/spooky "2,799 reputation") [Mar 13 '15 at 1:41](#comment46289674_24818721)


## This answer is useful

# 3

Another _GNU_ `sed` method, almost the same as [_Zsolt Botykai_'s answer](https://stackoverflow.com/a/1252191/6136214), but this uses `sed`'s less-frequently used `y` (_transliterate_) command, which saves _one byte_ of code (the trailing `g`):

```
sed ':a;N;$!ba;y/\n/ /'

```

One would hope `y` would run faster than `s`, (perhaps at `tr` speeds, 20x faster), but in _GNU sed v4.2.2_ `y` is about **4%** slower than `s`.

* * *

More portable _BSD_ `sed` version:

```
sed -e ':a' -e 'N;$!ba' -e 'y/\n/ /'

```

[edited Sep 13 '18 at 13:39](/posts/38164949/revisions "show all edits to this post")

answered Jul 2 '16 at 22:22

![](https://www.gravatar.com/avatar/0385d5a1c27a6707139e7dc7ad29b6d6?s=32&d=identicon&r=PG&f=1)

[agc](/users/6136214/agc)

6,475 1 gold badge 23 silver badges 41 bronze badges

*   2
    
    With BSD sed `y` is ca 15% faster. See [this answer](http://stackoverflow.com/a/1474512/1331399) for a working example. – [Thor](/users/1331399/thor "36,502 reputation") [Nov 3 '16 at 19:01](#comment68068711_38164949)
    
*   Also, with BSD sed commands need to terminate after a label, so `sed -e ':a' -e 'N;$!ba' -e 'y/\n/ /'` would be the way to go. – [ghoti](/users/1072112/ghoti "39,840 reputation") [Sep 13 '18 at 4:26](#comment91560469_38164949)
    




## This answer is useful

# 2

@OP, if you want to replace newlines in a file, you can just use dos2unix (or unix2dox)

```
dos2unix yourfile yourfile

```

![](https://www.gravatar.com/avatar/c2618d986361c695497c1a875ea8da01?s=32&d=identicon&r=PG)

[ghostdog74](/users/131527/ghostdog74)ghostdog74

269k 48 gold badges 233 silver badges 323 bronze badges

*   Can you add some more information to your answer (a link may or may not be sufficient)? E.g. where is dos2unix to be had? Is it a stand-alone program? Is it part of a bigger package? What platforms does it run on (only Unix? only Windows?) – [Peter Mortensen](/users/63550/peter-mortensen "26,498 reputation") [Mar 31 '15 at 8:11](#comment46910807_1252714)
    
*   2
    
    This only gets rid of \\r -- not \\n. I use sed all the time for this... _sed -i -e 's/\\r$//' foo_ is more robust than these tools for me. – [Michael Back](/users/3745821/michael-back "1,709 reputation") [Nov 26 '15 at 4:13](#comment55621574_1252714)
    

## This answer is useful

# 1


```
sed '1h;1!H;$!d
     x;s/\n/ /g' YourFile

```

This does not work for huge files (buffer limit), but it is very efficient if there is enough memory to hold the file. (Correction `H`-\> `1h;1!H` after the good remark of @hilojack )

Another version that change new line while reading (more cpu, less memory)

```
 sed ':loop
 $! N
 s/\n/ /
 t loop' YourFile

```

[edited Nov 3 '16 at 19:33](/posts/19907901/revisions "show all edits to this post")

![](https://www.gravatar.com/avatar/4a22763d295c9d45db0dd0c692bb4051?s=32&d=identicon&r=PG)

[Thor](/users/1331399/thor)

36.5k 8 gold badges 98 silver badges 111 bronze badges

answered Nov 11 '13 at 13:44

![](https://i.stack.imgur.com/l4VVG.jpg?s=32&g=1)

[NeronLeVelu](/users/2885763/neronlevelu)

9,202 1 gold badge 20 silver badges 40 bronze badges

*   It is not efficient as the example `sed ':a;N;$!ba;s/\n/ /g'`. On the other hand the output has an extra space character `<space>` – [ahuigo](/users/2140757/ahuigo "1,655 reputation") [Jun 19 '15 at 9:59](#comment49905391_19907901)
    
*   The loop version is similar to [this answer](http://stackoverflow.com/a/5847965/1331399) and as I noted in the comments there, it is ca 800 times slower. – [Thor](/users/1331399/thor "36,502 reputation") [Nov 3 '16 at 19:14](#comment68069123_19907901)
    
## This answer is useful

# 0

You can also use the [Standard Text Editor](https://www.gnu.org/fun/jokes/ed-msg.en.html):

```
printf '%s\n%s\n%s\n' '%s/$/ /' '%j' 'w' | ed -s file

```

Note: this saves the result back to `file`.

As with `sed`, this solution suffers from having to load the whole file into memory first.

answered Nov 3 '16 at 19:32

![](https://www.gravatar.com/avatar/4a22763d295c9d45db0dd0c692bb4051?s=32&d=identicon&r=PG)

[Thor](/users/1331399/thor)

36.5k 8 gold badges 98 silver badges 111 bronze badges

## This answer is useful

# 0

I posted this answer because I have tried with most of the `sed` commend example provided above which does not work for me in my Unix box and giving me error message `Label too long: {:q;N;s/\n/ /g;t q}`. Finally I made my requirement and hence shared here which works in all Unix/Linux environment:-

```
line=$(while read line; do echo -n "$line "; done < yoursourcefile.txt)
echo $line |sed 's/ //g' > sortedoutput.txt

```

The first line will remove all the new line from file `yoursourcefile.txt` and will produce a single line. And second `sed` command will remove all the spaces from it.

[edited Mar 22 '18 at 22:20](/posts/49366462/revisions "show all edits to this post")

answered Mar 19 '18 at 15:20

![](https://i.stack.imgur.com/sN3eq.png?s=32&g=1)

[Abhijit Pritam Dutta](/users/8405835/abhijit-pritam-dutta)

4,829 2 gold badges 6 silver badges 16 bronze badges

*   The "Label too long" probably means you are on BSD `sed` and trying to use GNU `sed` syntax. Try adding a newline after the label, as suggested in another answer. – [tripleee](/users/874188/tripleee "123,757 reputation") [Dec 29 '19 at 11:27](#comment105208863_49366462)
    
## This answer is useful

# 0

This might work for you (GNU sed):

```
sed 'H;$!d;x;:a;s/^((.).*)\2/\1 /;ta;s/.//' file

```

The `H` command prepends a newline to the pattern space and then appends the result to the hold space. The normal flow of sed is to remove the following newline from each line, thus this will introduce a newline to the start of the hold space and the replicate the remainder of the file. Once the file has been slurped into the hold space, swap the hold space with the patten space and then use pattern matching to replace all original newlines with spaces. Finally, remove the introduced newline.

This has the advantage of never actually entering a newline string within the sed commands.

answered May 19 '18 at 10:32

![](https://www.gravatar.com/avatar/526ea226101e199d029049dda22a4e58?s=32&d=identicon&r=PG)

[potong](/users/967492/potong)

43.6k 6 gold badges 38 silver badges 70 bronze badges

## This answer is useful

# 0

I came here to learn how to **delete** all blank lines in a file. To do that

```
 sed "/^\s*$/ d" fileName

```

answered Jan 18 at 14:17

![](https://www.gravatar.com/avatar/f287ad521ca7fb6ee7dc9b90787b02df?s=32&d=identicon&r=PG)

[Daniel Katz](/users/2158784/daniel-katz)

1,354 3 gold badges 14 silver badges 24 bronze badges

*   But that's another question and problem, [stackoverflow.com/questions/16414410/…](https://stackoverflow.com/questions/16414410/delete-empty-lines-using-sed "delete empty lines using sed") no? – [Davide Fiocco](/users/4240413/davide-fiocco "2,848 reputation") [May 28 at 12:06](#comment109769711_59801447)
    
## This answer is useful

# -1

This is really simple... I really get irritated when I found the solution. There was just one more back slash missing. This is it:

```
sed -i "s/\\\\\n//g" filename

```

[edited Mar 31 '15 at 16:27](/posts/7176800/revisions "show all edits to this post")

![](https://i.stack.imgur.com/RIZKi.png?s=32&g=1)

[Peter Mortensen](/users/63550/peter-mortensen)

26.5k 21 gold badges 92 silver badges 122 bronze badges

answered Aug 24 '11 at 14:06

![](https://www.gravatar.com/avatar/9b82a539cb09e68b041ecdc57d6b3a89?s=32&d=identicon&r=PG)

[Sabby](/users/909787/sabby)

9

## This answer is useful

# -2

Here is `sed` without buffers (good for real time output).  
Example: replacing `\n` with `<br/>` break in HTML

```
echo -e "1\n2\n3" | sed 's/.*$/&<br\/>/'

```

[edited Aug 8 '14 at 9:29](/posts/7059825/revisions "show all edits to this post")

![](https://i.stack.imgur.com/2NIGA.png?s=32&g=1)

[xav](/users/1149528/xav)

4,566 7 gold badges 39 silver badges 54 bronze badges

answered Aug 14 '11 at 21:23

![](https://www.gravatar.com/avatar/c6e269eac3a6b83f1e6f98b9e7f0b973?s=32&d=identicon&r=PG)

[Tiago](/users/894257/tiago)

17

## This answer is useful

# -2

The following is a lot simpler than most answers. Also, it is working:

```
echo `sed -e 's/$/\ |\ /g' file`

```

[edited Mar 31 '15 at 16:34](/posts/10622862/revisions "show all edits to this post")

![](https://i.stack.imgur.com/RIZKi.png?s=32&g=1)

[Peter Mortensen](/users/63550/peter-mortensen)

26.5k 21 gold badges 92 silver badges 122 bronze badges

answered May 16 '12 at 16:35

![](https://www.gravatar.com/avatar/93314b647d41429b2649356db220ddac?s=32&d=identicon&r=PG)

[THE Sorcerer](/users/1399161/the-sorcerer)

13 2 bronze badges

*   8
    
    This isn't working, since it doesn't _replace_ the newline, but prepend some text to each line. – [leemes](/users/592323/leemes "40,882 reputation") [Jun 14 '12 at 1:48](#comment14415451_10622862)
    

## This answer is useful

# -3


```
sed -i ':a;N;$!ba;s/\n/,/g' test.txt

tr "\n" <file name>

```

answered Jan 18 '16 at 8:56

![](https://graph.facebook.com/1610917883/picture?type=large)

[Poo](/users/4401898/poo)

643 6 silver badges 15 bronze badges

## This answer is useful

# -7

Try this:

```
echo "a,b"|sed 's/,/\r\n/'

```

[edited Nov 19 '11 at 22:33](/posts/4440428/revisions "show all edits to this post")

![](https://i.stack.imgur.com/ugRaY.jpg?s=32&g=1)

[animuson](/users/246246/animuson)♦

49k 23 gold badges 127 silver badges 139 bronze badges

answered Dec 14 '10 at 14:53

![](https://www.gravatar.com/avatar/fb69610a353ccfebef97e549ddb537aa?s=32&d=identicon&r=PG)

[Leandro](/users/542104/leandro)

3

## This answer is useful

# -7

In the sed replacement part, type backslash, hit enter to go to the second line, then end it with /g':

```
sed 's/>/\
/g'

[root@localhost ~]# echo "1st</first>2nd</second>3rd</third>" | sed 's/>/\
> /g'
1st</first
2nd</second
3rd</third

[root@localhost ~]#

```
[edited Mar 31 '15 at 16:34](/posts/12462891/revisions "show all edits to this post")

![](https://i.stack.imgur.com/RIZKi.png?s=32&g=1)

[Peter Mortensen](/users/63550/peter-mortensen)

26.5k 21 gold badges 92 silver badges 122 bronze badges

answered Sep 17 '12 at 15:54

![](https://www.gravatar.com/avatar/11fe1206a0e398ab89bb153044e30bd7?s=32&d=identicon&r=PG)

[blah](/users/1678082/blah)

3 1 bronze badge

*   One thing to note though is that for portability, sometimes using a new line in the script (not \\n) is necessary. With some trickery you could also insert a literal `\r`. I mean inserting the real characters into the script file, not escapes. – [akostadinov](/users/520567/akostadinov "14,096 reputation") [Jun 5 '14 at 6:41](#comment37086124_12462891)
    





