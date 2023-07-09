bandit6: P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

This time we are using the `find` command again

**NOTE TO SELF: MOVE MANUAL TO BANDIT 5 WHEN FINISHED**

The man command - pulls up the `man`ual for another command. This will contain
all the options, command-line flags, behaviors, etc. that will be relevant for
running a given command.

`$ man find`

We can use `/` while in the man view screen to search for a keyword. Within
the `man` page for find, we need to find:
- A filter for searching by user that owns file (/user)
- A filter for searching by group that owns file (/group)

The `/` directory on Linux represents the very top-most directory on the
filesystem. By passing it as an argument to `find`, it will allow us to search
the entire filesystem.

`$ find / -group bandit6 -user bandit7 -size 33c`

We get a very large quantity of access denied errors that are making it
difficult for us to locate the actual outfut for the command.

On Linux we have 3 streams for information when working in a terminal:
- Standard input represents what we type into the terminal
- Standard output represents the regular output of a command
- Standard error represents errors the command outputs

The access denied errors are all going to the standard error stream

We can use `>` on Linux to take all the output from standard output or
standard error and output it to some file.

`$ mktemp`
`<COPY output of mktemp; it represents the name of a temporary file>`
`$ ls -a > <PASTE output of mktemp>`
`$ cat <PASTE output of mktemp; should output the hidden files to the screen>`

We can use `2>` to redirect the output from standard error instead.

`$ ls -a dir`
`<see an error, because dir does not exist>`
`$ ls -a dir > <PASTE output of mktemp>`
`<see an error, because dir still does not exist>`
`$ ls -a dir 2> <PASTE output of mktemp>`
`<no error should appear>`
`$ cat <PASTE output of mktemp>`
`<see error on screen, because that error was redirected into that file>`

There is a special file on Linux called `/dev/null` that discards all the output
that we send to it.

`$ ls -a > /dev/null`
`$ ls -a dir 2> /dev/null`
`$ cat /dev/null`

So, we can send the standard error output of `file` to `/dev/null`, and it
will get rid of all the errors.

`$ find / -group bandit6 -user bandit7 -size 33c 2> /dev/null`

Now that we have a clean output consisting of a single matching file, we can
use two tricks to reduce the amount of typing we have to do.

The `!!` command on Linux will always re-run the last command we ran in a
terminal.

`$ !!`
`<last command>`
`<output from last command>`

In bash, we can use the `$()` syntax to take the output of a command, and put it
into another command. For example, `cat $(ls)` would take the output of `ls` and
it would output the contents of every one of those files.

We can rerun the find command and do this:

`$ find / -group bandit6 -user bandit7 -size 33c 2> /dev/null`
`$ cat $(!!)`

This will take the last command, run it, (done by the `!!`) and take its
output and paste that into the cat command (done by the `$()`) to get the
output and save some keystrokes.

These tricks are not something you *need* to know, but they become really
helpful when you're using the shell all the time.

bandit7: z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

The `grep` command goes through a text file, and looks for a given word. Then,
it outputs the line it found the word on.

`$ grep millionth data.txt`

bandit8: TESKZC0XvTetK0S9xNwm25STk5iWrBvP

In order to find the unique lines in a file, the `uniq` command exists.

`$ man uniq`

Look for the -u flag, explain what that does.

Also, look at the note at the very bottom of the screen.

> Note: 'uniq' does not detect repeated lines unless they are adjacent.
> You may want to sort the input first, or use 'sort -u' without 'uniq'.

This means we have to move all the identical strings next to each other. As
the manpage proposes, sorting the file would do that.

`$ sort data.txt`

Now we need to take the output of that command, and send it to uniq. The pipe
character `|` allows us to take the output of one command, and redirect it to
another.

`$ sort data.txt | uniq -u`

bandit9: EN632PlfYiZbn3PhVK3XOGSlNInNE00t

This is a large binary file with a handful of strings in it. Looking through
it by hand is pointless.

`less data.txt`

The `strings` command goes through a binary file, and it looks for ASCII
strings over a certain length.

`$ strings data.txt`

Now we just need to look for a string that has an `=` character. We can send
the output to `grep` in order to filter that out.

`$ strings data.txt | grep =`

bandit10: G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

`base64` is a way for us to take a piece of data, and turn it into this weird
encoded string. It has some applications, but is mostly an obscure technical
concept you'll run into every once in a while.

`$ file data.txt`

As far as identifying it goes, base64 looks just like a piece of random text.
Pay attention to the following:
- Only upper/lowercase alphabetic characters and equal signs.
- No numbers or other special symbols.
- Will often have equal signs at the end.

`cat data.txt`

On Linux, we have the `base64` command to work with base64.

`$ man base64`

Let's find a flag that can decode an input:

`$ base64 -d data.txt`

bandit11: 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
We are using the Caesar cipher here. We take every letter in the string, and
we "rotate" it by a certain number of positions. For example:

a1 => b
a2 => c
a3 => d
z1 => a
z2 => b
c1 => d
a13 => n

and so on. The way we can implement this for a single key is fairly simple:
create a simple database of translations for each letter. That database will
have the letter that is 13 letters away from itself. For example:

a => n
b => o
c => p

We have a command called `tr`. It takes two arrays: one array of characters to
look for, and a second one of characters to translate whatever it finds in the
first array to.

```bash
tr -t abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ \
      nopqrstuvwxyzabcdefghijklmNOPQRSTUVWXYZABCDEFGHIJKLM
```

bandit12: JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
cat data.txt
man xxd
xxd -r data.txt > data
file data
unzip/bzip2 -d/tar xf repeatedly

bandit13: wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw

For this level, you have to use the SSH private key to SSH into the bandit14
account. This is straightforward: you just have to connect to `localhost` (which
is the server) and use the `-i` command line switch for SSH.

ssh -p 2220 -i <key file name> bandit14@localhost

bandit14: fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq

You need to establish a TCP connection to the server and send your password over
that. The `ncat` utility is perfectly suited for this.

`ncat` is a program used for writing information directly over TCP (and UDP). If
you don't know what that is, you should, so go read up on networking. It works
sort of like `cat`: you specify the host and port, and you can just send
information (from a file or from stdin) to that port.

There are two ways you can approach this.

The manual way is to simply run `ncat localhost 30000`. This will connect to the
server on port 30000. Then, paste the password in and press enter. This will
send the password to the TCP socket. You will receive the password back.

The quicker way to do this is to use cat (or echo) to print the password, and
pipe it to ncat. `cat /etc/bandit_pass/bandit14 | ncat localhost 30000`.

bandit15: jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

This level is similar to the last one, except this time the port also uses TLS
encryption. The page conveniently lists `openssl` and `s_client` as suggested
commands. The `openssl` library is commonly used on Linux as a front end for
cryptographic functions. The `openssl` command provides a command called
`s_client`, which is like `ncat` from the previous example except it allows you
to communicate with SSL encryption.

`openssl -quiet -connect localhost:30001`

bandit16: JQttfApK4SeyHwDlI9SXGR50qclOAil1

So there are two ways to solve this: the easy way and the fun way.

The easy way is to run `nmap -sV -p 31000-32000`. This will list off five ports,
four of which will be listed as echo. The last one (31790 in my case) will be
listed as ssl/unknown. That one is the one you're looking for.

The fun way is to run a loop that will iterate through every port and send the
password to it over SSL. Then, see what response you get.

And the response you get is an RSA private key. You can use `mktemp` to create a
temporary file, copy the key in there, and feed the file to SSH.

`ssh -i <file> -p 2220 bandit17@localhost`

You can then get the password using

`cat /etc/bandit_pass/bandit17`

bandit17: VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e

You can use the `diff` utility to compare the two files.

`diff passwords.old passwords.new`

bandit18: hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg

This level familiarizes you with the `.bashrc` file. It is a special file that
you can put in your home directory. It is a shell script. Whenever you start an
interactive shell, this shell script will be run. An SSH session counts as an
interactive shell, and so this file is run whenever you connect over SSH.
However, someone set the `.bashrc` to log you out as soon as you try to log in,
so you can't simply SSH into the account.

The way you bypass that is by realizing that an only an interactive SSH session
is a login shell. If you simply get SSH to run a one-off command, it probably
won't open a whole interactive shell. Checking the SSH man page, it looks like
there's a positional argument to pass the command for it to run.

`ssh -p 2220 bandit18@bandit.labs.overthewire.org 'cat readme'`

bandit19: awhqfNnAbc1naukrpqDYcF95h7HoMTrC

This lab requires you to use a feature called setUID. This is a special
permission on Linux. Normally, when you run a command, that command will run as
your own user. i.e. if you're logged in as user `john` and you run `ls` will run
under the user `john`. When you add this permission to a file, every time you
run that file as a program is will run as the file's owner, rather than
yourself. That is, for example, how `su` works: it's just a program owned by
root and which has the setUID permission which opens a new shell.

In this case, we're given a file with the setUID bit. Running it without
arguments reveals that it will run whatever command you pass to it as an
argument as another user. Let's try that:

`./bandit20-do cat /etc/bandit_pass/bandit20`

bandit20: VxCazJaVykI6W36BkBU0mJTCM8rR95XT

This is an interesting one. We have a program within the current folder that
will establish a TCP connection to any port you specify. It will wait to receive
a password (the password for `bandit20`) from that port. If it receives the
correct password, it will give you the password for `bandit21`.

So first things first, we need to open a server on some port. We can use `ncat`
for that: the `-l` option opens a server on a port of your choosing. But now
that our server is running, we have a problem: our terminal is busy and we can't
run the binary.

Two ways to make this work: the fun way or the funny way. The funny way is you
open a new terminal, ssh into the bandit again, and run the SUID binary from
there. You can also use tmux and open a new session. Let's focus on the fun way
though.

The fun way is to do this from a single terminal without exiting, and the
concepts behind that are important to learn. To get access to the terminal, we
need to background the `ncat` server. There are two ways to do that: we can
either append `&` to the command (`ncat -l 2700 &`) which will start the process
in the background straight away. You can also run `ncat` regularly, and then
press `Ctrl + Z`. This will suspend the process, bringing you back to the
terminal. From there, you can run `bg` and it will continue the process in the
background.

Now that we have the server running in the background, let's run
`./suconnect 2700`. This has occupied our terminal again; we can press
`Ctrl + z` to suspend it and `bg` to send it into the background.

Now we need to get back to our `ncat` server, as we are expected to send the
password through it. First, type `jobs` to see the jobs that are tied to this
terminal. `ncat` should be job number one. Now run `fg 1` to bring ncat to the
foreground again. Paste the password and press enter.

Note that we could also have saved ourselves a bit of work by `cat`ing the
password right into `ncat` when we opened the server.

bandit21: NvEJF7oVjkddltPSrdKEFOllh9V1IBcq

This level involves looking at a cron job and figuring out what it does. Now, although it's not strictly necessary to solve our problem (yet), I will begin by explaining what exactly cron is.

`cron` is a daemon that is commonly used on UNIX-based operating systems as a task scheduler. It works fairly simply: you give it a job to do (a shell script) and a schedule when that job should be executed. For example, you can get it to run a full system upgrade every day at midnight. Or you might get it to run a backup on the 1st and 15th days of every month. Or you might have it disable a firewall rule every Wednesday at 2:30 pm, and then enable it again every Friday at midnight. This is a very useful tool, as it allows you to repeatedly run simple, repetitive tasks without having to do them manually.

As the problem tells us, we need to check `/etc/cron.d`. There we see a file called `cronjob_bandit22`. `cat`ing the file, we see that it calls a script located at `/usr/bin/cronjob_bandit22.sh`. `cat`ing that file, we see that it outputs the password to a file in the `/tmp` directory. `cat` that file and we get the password.

bandit22: WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff

Another cronjob. There's a file called `cronjob_bandit23`. `cat`ing the file, we see that it calls a script located at `/usr/bin/cronjob_bandit23.sh`. This time, the script is more complex. Let's break it down.

The first line is what is called the shebang. This is a comment (comments in bash start with `#`) that is always on the first line of a shell script, starts with a `!`, and specifies the shell with which the shell should be executed. This is usually either `#!/bin/sh` (if the script is POSIX-compliant, and doesn't use any bash-specific features) or `#!/bin/bash`. This doesn't really matter in the context of our script, but you should be aware that it's a thing.

Next, the script defines a variable `myname`. Using the `$()` syntax the variable sets itself to the value of the `whoami` command. That command always outputs the name of the user running the script, so `myname` is just the user who runs the script.

The next line is a bit more complex. First, it echoes `I am user $myname`. `$<variable name>` in bash is used to retrieve the value of a variable, which in this case is `myname`. So `echo` is essentially outputting `I am user <username>`. It then takes the output of the echo, and pipes it to `md5sum`. `md5` is an old hashing algorithm, and the command should simply output a long hash corresponding to the output of `echo`. Finally, the output of `md5sum` is piped to `cut`. This isn't important; this simply removes some trailing information that the `md5sum` command adds after the hash itself.

Having created the two variables, the script outputs a debug message and `cat`s the password file `/etc/bandit_pass/$myname` to a file called `/tmp/$mytarget`. Now, you might be tempted to just run the script yourself and view the debug message to easily get the path. This won't work. When you run the script manually, it will be run as the user you are logged in as. Since you are logged in as `bandit22`, the script will create the hash using `bandit22`'s username and will write `bandit22`'s password to it. Therefore, we need to use the file created by the crontab, as that is run under `bandit23`.

We can find the name of the file fairly simply: just rerun the command that create the hash, but rather than using `$(whoami)` emulate the correct output by using `bandit23`. You will get a hash as the output. Prepend `/tmp/` to that hash, and you have the filename with the password.

bandit23: QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G

Another cron problem. This time, the explanation I gave above for what exactly cron is will come in handy, so go reread that if you need to.

`/usr/bin/cronjob_bandit24.sh` contains an even more complex script that time. We once again have a shebang and a declaration of the `myname` variable. Then, the script changes directory to `/var/spool/$myname/foo`, and says (using `echo`) it's about to execute and delete every script in that directory. Then there's a `for` loop. Let's break that down.

The `for` loop's condition is `for i in * .*`. You might recall I mentioned wildcards in one of the earlier levels: those are what this is. The loop will run once for every file it finds in the current directory, writing that file's name to variable `i`.

**NOTE TO SELF: WHERE IS EXPLANATION FOR . AND ..?**

Next, there's an `if` statement. Again, you might recall my explanation regarding the special `.` and `..` directories. The `.*` wildcard will include those two directories, so the script is doing error checking to ensure that nothing is done to them.

Next, it declares a variable called `owner`. That variable is set to the value of `stat --format "%U" ./$i`. The `stat` command outputs information about a file, and the `--format` flag makes it output that information in a certain format. As you might have guessed from the variable name, this simply returns the name of the owner of a file `./$i`. `i` is the iterator variable created by the `for` loop representing the current file. Therefore, for each file in the directory, this line will create a variable called `owner` which will contain that file's owner.

Next, there's another `if` statement. That simply checks that `owner` is `bandit23` (which is our current level). If that condition is satisfied, we run `timeout -s 9 60 ./$i`. A quick look through the manpage for `timeout` indicates that timeout simply runs a command, and kills it if it doesn't exit within the time specified. For our purposes, what matters is it runs `$i`. After that, it deletes `$i` and starts the loop again.

Okay, that was a mouthful, but long story short: every time `/usr/bin/cronjob_bandit24.sh` runs, it executes every single script it finds within `/var/spool/<username>/foo`. What we need to do is put a script within that directory that will somehow get us the password. Let's do that.

First, let's `mktemp` to give us a file to work with. First, we'll need to put a shebang. Simply `#!/bin/bash` will do. We already know we can get the password by `cat`ing from `/etc/bandit_pass/bandit24`, so let's do that as well. Now, we obviously can't have the password output itself to our screen, so we'll need to choose a location (within `/tmp` where we have write access) to write the output of `cat` to first. This location will need to be readable and writable by both `bandit23` (for us to retrieve the password) and by `bandit24` (for the script to successfully write it out). Let's just use our script file for that purpose.

```bash
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/outputfile
```

We need to deploy it now, but before we copy it over we need to set our script file's permissions. `chmod 777 filename` will allow bandit24 to both execute and overwrite it. Now, `cp` that script file over to `/var/spool/bandit24/foo` and wait for the magic to happen. Note that this won't work immediately. Recall that this script is being run from a crontab. cron jobs can only be run, at most, once every minute. This cron job is set to run that often, so you'll need to wait until that happens. You can just wait a minute, or you can `sleep 60 && cat /tmp/<filename>` to have it measure the time out for you.

bandit24: VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar

We need to connect to port 30002 and bruteforce a 4-digit password over TCP. `ncat` can do that, so let's `ncat localhost 30002` and enter 9999 to try it out. Upon connecting we get a message saying that we need to enter both the password and the pincode for each try. Let's keep that in mind.

Now we need some way to feed 0000-9999 into that connection. We can use the `seq` command to generate a sequence of numbers. The `-w` flag allows us to pad each number in the sequence with zeroes, so `seq -w 0 9999` should do the trick.

Next we need some way to connect the sequence of numbers with the password on each line. We can use the `xargs` command for that. `xargs` just runs a command with piped arguments from another command. We can run `seq -w 0 10 | xargs -L1 echo password` and that will prepend `password` to every line of output.

Now we just need to pipe all that to `ncat`. `seq -w 0 9999 | xargs -L1 echo password | ncat localhost 30002` does what we want.

bandit25: p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d

We are given an SSH key to log into bandit26. Once we try to use it, the connection immediately terminates. We cannot simply run commands either (since we don't have a shell), so we'll have to get creative.

First things first, we can check `/etc/passwd` to find out what bandit26's login shell is. It turns out to be `/usr/bin/showtext`. Upon trying to run that program, we get an error message that it can't access a file in our home folder called `text.txt`. This seems to be some sort of custom script, so let's `cat` that file.

This script runs `more` on a file called `text.txt` inside the home folder. `more` is a pager. Nowadays, it's almost never used in favor of the more advanced `less`, but let's try running it the ssh key in our home folder: `more bandit26.sshkey`. But wait: it's not paging. It just outputs the text to the screen like `cat`. If we run `less` on that same file, we get the expected paged output. This is due to a default behavior of `more`: if there's less than a screenful of text to display, it is simply printed to the screen. If we shrink the size of our terminal window we will get the paged output as expected though. Upon shriking our terminal to a sufficiently small size, we are able to successfully log in using the ssh key.

Looking through the manpage, it seems as if `more` is able to call a text editor on the file being viewed. This is triggered by pressing `v`. Upon trying that within the SSH login session, we get the `vi` editor. We can use that to escalate to a shell by running `:terminal /bin/bash`.

Now all we need to do is get the password for bandit27. There's another `setUID` executable inside our home folder, so `./bandit27-do cat /etc/bandit_pass/bandit27` does the trick.

bandit27: YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS

**NOTE TO SELF: EXPLAIN GIT PROPERLY**

We have to use SSH to clone a git repo from the bandit27-git user. First, let's `cd $(mktemp -d)`. Now we need to use `git clone` to clone the repository over SSH. We'll need to craft a URL for that. First, the protocol we're using is `ssh`. The user is `bandit27-git` and the host is `localhost`. The port is `2220` and the file path is `/home/bandit27-git/repo`. When we combine that together, we get `git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo`. Now that the repo has been cloned, `ls` the new folder and `cat README`.

bandit28: AVanL161y9rsbcJIsFHuw35rjaOM19nR

Same story as last time: make a temporary directory and `git clone`. Now, there's nothing inside the repository folder. Upon checking `git log`, there seem to be some commits in the repository's history. We can use `git checkout` to switch to the previous commit. Now if we `cat README.md` we can get the password.

bandit29: tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S

Another git repo. This time, checking through history doesn't help. The README gives the answer away though: `no passwords in production!`. `git branch --all` lists a branch called `dev`. `git checkout dev` gets us to that branch. Output `README.md` to get the password.

bandit30: xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS

Another git repo. No branches or history. However, `git tag` reveals a tag called `secret`. `git show secret` returns the password.

bandit31: OoffzGDlzhAlerFJ2cAiz1D41JW1Mhmt

Another git repo. No branches, tags, or history. The README says we need to push a file called `key.txt` with the content `May I come in?` to remote. `echo May I come in? > key.txt` creates the necessary file. When we try to `git add` it to the repository, `git` says we're not allowed to by the `.gitignore`. We can bypass that either by deleting `.gitignore` or following `git`'s suggestion and using `git add -f`. Finally, we `git commit -am message` to add the commit to the repository, and we push. With the push, we receive a message back containing the password.

bandit32: rmCBvG56y58BXzv98yZGdO7ATVL5dW8y

Upon SSHing into this one, you'll be greeted with a welcome to the "uppercase shell". Any command you try to type here will get uppercased, so it won't work. `Ctrl+d` to exit SSH, and let's go back to `bandit31` for a sec.

**SETUID CAPITALIZATION STANDARDS**

First things first, let's log back into bandit31 for a second. `grep bandit32 /etc/passwd` claims that bandit32's login shell is `/home/bandit32/uppershell`. `ls -l` says that it's a setUID binary owned by bandit33 and with the group bandit32. We don't have read access to it, so it doesn't seem as if we can do anything from here. Cool.

The first thing that comes to mind is `.` is not an alphanumeric character. If we try to input it, it does seem as if it's unaffected. The second thing that comes to mind is that environment variables are spelled in all upper case. Running `$SHELL` seems to be successful, and running $PATH produces a list of lower-case directories. However, it doesn't appear as if we can set any of the environmental variables ourselves. Now we need to find an existing enviroment variable that we can exploit.

I would never have thought of the solution had I not used the `foot` terminal on Linux desktop. See here, it's a non-standard terminal and it regularly breaks if I try to run something like `vim` or `less` on the bandit machines. In order to work around this, I have to set `$TERM` to `xterm-256color` (essentially a generic terminal) before SSHing in. What you can do is you can, on your own machine, set `TERM` to a non-standard value (such as `bash`), and it will carry over to the SSH session. Now you can run $TERM, and you have yourself a regular bash session.
