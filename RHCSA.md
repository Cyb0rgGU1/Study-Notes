yh   # RHCSA 
## _RedHat Certified Systems Administrator_

**Lab Enviornment**
* All student computer systems have a standard user account, **student**, which has the password **55TurnK3y**. The root password on all student systems is _**redhat**_.

|Machine name | IP address | Role |
| ----- | ----- | ----- |
| bastion.lab.example.com | 172.25.250.254 |Gateway system to connect the student private network to the classroom server (must always be running)
| classroom.example.com | 172.25.254.254 | Server that hosts the required classroom materials |
| workstation.lab.example.com | 172.25.250.9 | Graphical workstation for student use |
|servera.lab.example.com |172.25.250.10 | Managed server "A" |
|serverb.lab.example.com |172.25.250.11 |Managed server "B" |

Log into `http://rol.redhat.com/` to access self-paced courses. 

## Overview
* **Control:** See what the code does and improve it.
* **Training:** Learn from real-world code and develop more applications that are useful.
* **Security:** Inspect sensitive code, and fix it even without the original developers' help.
* **Stability:** Rely on code that can survive the loss of the original developer.

- Import a HTML file and watch it magically convert to Markdown
- Drag and drop images (requires your Dropbox account be linked)
- Import and save files from GitHub, Dropbox, Google Drive and One Drive
- Drag and drop markdown and HTML files into Dillinger
- Export documents as Markdown, HTML and PDF

## Ecosystem 

**Fedora:** Project that produces and releases free Linux-based OS; prioritizes innovation and excellence above long-term stability. Source of innovation for the entire Enterprise Linux ecosystem. 

**EPEL:** _Extra Packages for Enterprise Linux_ builds and maintains community-supported package repos; aligns with major RHEL releases. An additional repo for package maintainers to build against CentOS Steam. 

**CentOS Stream:** Patches are submitted to the CentOS Stream are integrated faster; continious integration and delivery distribution, with tested and stable nightly builds. 

**RHEL:** _Redhat Enterprise Linux_ is production-ready, commercially supported distribution. Builds RHEL major releases directly from the CentOS Steam CD project. 

**RHEL for Edge:** Image-based variant of RHEL, with different deployment mechanisms. Uses a tool called **Image Builder** that provides the ability to create OSs. Edge features include secure management, scaling capabilities, including zero-touch provisioning, system health visibility, and quick security remeditaion from a single interface. 

**RHEL CoreOS:** (_RHELOS_) not a stand-alone operating system but built from RHEl components then released, upgraded, and managed as part of **RedHat OpenShift Container Platform** (_RHOCP_) for cloud-native apps. Fundamentally an image based container host. 

**Redhat Universal Base Image:** (_UBI_) freely distributed derivation of RHEL; designed to be a foundation for cloud-native and web application use cases within containers. Packages sourced from secure RHEL channels. Developers can focus their efforts on their application in the container image. Has a set of base images such as _python, ruby, node.js, httpd, or nginx_. UBI also has RPM repos. 

**Redhat Enterprise Linux Continuous Development:** Fedora rawhide is the CD enviornment for regular cadence of public fedora releases.

> A linux distribution is an installable operating system that is constructed from a Linux kernel and that supports user programs and libraries. 

---
## CLI
**_BASH:_** 
* GNU Bourne-Again Shell (_bash_) is the default user shell
* Shell prompts: 
```
[user@hostname ~]$ 
```
$ = Reveals that the shell is running as a regular user 

```
[root@hostname]#
```
\# = Reveals that the shell is running as the superuser. 

> The bash shell is _conceptually similar_ to the Microsoft Windows **cmd.exe** command-line interpreter. However, _bash_ has a sophisticated scripting language, and is more similar to **Windows PowerShell.**

> If you omit the user argument, then the `su` command assumes that the user is `root`. If the `su` command is followed by a single dash `-`, then it starts a **child login shell**. Without the dash, the `su` command creates a **non-login child** shell that *matches the user's current environment*.

To type more than one cmd on a single line, use the semicolon `;` as a command seperator. 

```
[root@hostname]$ command 1; command2
command 1 output
command 2 output
```

### Common CMDS
| CMD | What it do |
| --- | ---- |
| date | Displays the current date and time
| passwd | changes password **(if no option, changes current users pass)**
| file | displays the type of file
| cat | view contents of a file
| less | displays one page of a file at a time; you can scroll 
| head | displays beginning of file **(displays 10 lines unless used with `-n`)**
| tail | displays end of file **(displays 10 lines unless used with `-n`)**
| wc | counts lines, words, and characters **( `-l`, `-w`,`-c` lines, words, characters)**
| history | displays a list of previously executed cmds **(can use `!string` to rerun cmd in history)
| pwd | print working directory
| ls | list files
| touch | updates the time stamp of a file, creates empty files
| whereis | locate the binary, source, and manual pages |

> Can use `\` escape character to build out a long command

```
[user@host ~]$ head -n 3 \
/usr/share/dict/word \
/usr/share/dict/linux.words
```
### Editing the CMD Line

| Shortcut | Description |
| --- | --- |
|Ctrl+A	| Jump to the beginning of the command line.
|Ctrl+E	| Jump to the end of the command line.
|Ctrl+U	| Clear from the cursor to the beginning of the command line.
|Ctrl+K	| Clear from the cursor to the end of the command line.
|Ctrl+LeftArrow	| Jump to the beginning of the previous word on the command line.
|Ctrl+RightArrow	| Jump to the end of the next word on the command line.
|Ctrl+R	| Search the history list of commands for a pattern.

**Basics:**
* Command is the name of the program that is being ran. 
* Terminal is the text-based interface to enter comands into. 
* Virtual consoles can be ran on separate terminals which support an independent login session.
> **CTRL + ALT** & a Function key switches between the consoles.
    The first virtual console, which is called **_tty1_**. Five additional text login prompts are available on virtual consoles two **_(tty2)_** through six **_(tty6)._**

## Links 
-  Multiple file names can point to the same file, which create links

_**Hard links**_
- Every file that is created starts with a hard link
- Cannot tell the difference between the new hard link and the original name of the file. 

To tell if a file has multiple hard links:
```
[user@host ~]$ ls -l 
```
It's also possible to point a hard link to an already existing file. 
```
[user@host ~]$ ln newfile.txt /tmp/newfile-link2.txt
```

To verify if two files are hard linked, you need to use the file's inode number. If the numbers are the same, the files are hardlinks that point to the same data file content. 

```
[user@host ~]$ ls -il newfile.txt /tmp/newfile-link2.txt
```

Even if the original is deleted, you are able to still access the data from an existing hard link. Data is delted from the storage only when the last hardlink is deleted. 

> Limitations: You can only use hard links with regular files; cannot use it with a directory or special file. You also can only use hard links if both files are on the same file system. 

_**Soft/Symbolic links**_
- A soft link is not a regular file but a special type of file that points to an existing file or directory. 
- Symbolic links can link two files on different file systems
- Symbolic links can point to a directory or special file - not just to a regular file. 

```
[user@host ~]$ ln -s /home/user/newfile-link2.txt /tmp/newfile-symlink.txt
```
To indicate if a file is a symbolic link is by the first character after running the `ls -l` cmd. 

```
[user@host ~]$ ls -l newfile-link2.txt /tmp/newfile-symlink.txt
lrwxrwxrwx. 1 user user 11 Mar 11 20:59 /tmp/newfile-symlink.txt -> /home/user/newfile-link2.txt
```
When the original file is deleted, the symbolic link still points to the file but the target is gone. 
> A symbolic link that points to a missing file is called a **_dangling symbolic link_**.

>> One side-effect of the **dangling symbolic link** is that if you create a file with the same name as the deleted file then the symbolic link is no longer **"dangling"** and points to the new file. **Hard links do not work in this way.** If you delete a hard link and then use normal tools (rather than ln) to create a file with the same name, then the new file is not linked to the old file.

**Hard links points a name to data.**

**Symbolic links points a name to another name.**

## Pattern Matching

| Pattern |	Matches |
|---|---| 
| *	| Any string of zero or more characters
| ?	|Any single character
[abc…​]	|Any one character in the enclosed class (between the square brackets)
[!abc…​]|	Any one character not in the enclosed class
[^abc…​]|	Any one character not in the enclosed class
[[:alpha:]]	|Any alphabetic character
[[:lower:]]	|Any lowercase character
[[:upper:]]	|Any uppercase character
[[:alnum:]]	|Any alphabetic character or digit
[[:punct:]]	|Any printable character that is not a space or alphanumeric
[[:digit:]]	|Any single digit from 0 to 9
[[:space:]]	|Any single white space character, which might include tabs, newlines, carriage returns, form feeds, or spaces

---
## MAN Pages

```
[user@host ~]$ man <topic> 
```
There are different sessions to the man page that you can specifically call out to in your command:

```
[user@host ~]$ man 5 <topic> 
```

|1	|User commands	|Both executable and shell programs |
| --- | --- | --- |
2	|System calls	|Kernel routines that are invoked from user space
3	|Library functions	|Provided by program libraries
4	|Special files	|Such as device files
5	|File formats	|For many configuration files and structures
6	|Games and screensavers	|Historical section for amusing programs
7	|Conventions, standards, and miscellaneous	|Protocols and file systems
8	|System administration and privileged commands	|Maintenance tasks
9	|Linux kernel API	|Internal kernel calls
---

> Popular topics are 1, 5, and 8. 

### Navigating MAN

```
[user@host ~]$ man -k <topic> 
```
`-k` command option searches for the keyword in the full-text page. not only in the titles and description. 

> Keyword searches must be ran as `root`.

## Create/View/Edit Files 

### Standard Input, output, error

![N|Solid](https://rol.redhat.com/rol/static/static_file_cache/rh124-9.0/edit/descriptor-overview.svg)

> Running program is a _process_ and reads **input** while spitting **output**. A _process_ uses numbered channels called **file descriptors** to get **input**; all _process_ start with at least **3** file descriptors. 

|File Descriptors | Channel | Channel Name | Usage |
| --- | --- | ---- | --- |
| Input | 0 | `stdin` | Read only 
| Output | 1 | `stdout` | Write only 
| Error | 2 |  `stderr` | Write only 
| Other | 3+ | `filename` | R/W 

You can redirect a process `stdout` to supress the process output into a file to avoid it popping up on a terminal. If you redirect to a file that doesn't exist, it'll create it or if it does then overwrite it. 

#### **To discard the output of a process, you can redirect to `/dev/null` that discards channel output.**

## Redirection

```
[user@host ~]$ output.log 2>&1
```
This sequence redirects `stdout` to the **output.log** file then redirects `stderr` messages to the same place. 

```
[user@host ~]$ 2>&1 output.log 
```
This sequence redirects in the opposite order, redirecting `stderr` to the default place for `stdout` then redirects `stdout` to the **output.log**. 

#### Other wants to append a file 
```
[user@host ~]$ echo "something interesting to put here" >> /tmp/extrafile.txt
```
Append a line to the existing file.
```
[user@host ~]$ find /etc -name passwd > /tmp/output 2> /dev/null 
```
Saves process output to `/tmp/output` file and discards error messages into `/dev/null`.

### Pipelines

A **pipeline** is a sequence of multiple commands seperated by `|` which connects the `stdout` of the first command to the `stdin` of the next command. 
```
[user@host ~]$ ls -l /usr/bin | less
```
#### **When combining redirects (`>`) with a pipeline `|`, the shell sets up the pipeline first, then redirects the input/output.**

The `tee` command overcomes this limitation. 

`Tee` copies the input to the output and also redirects the standard output to the files that are given. 

>If you imagine data as water that flows through a pipeline, then you can visualize `tee` as a "T" joint in the pipe that directs output in two directions.

![N|Solid](https://rol.redhat.com/rol/static/static_file_cache/rh124-9.0/edit/pipe-tee.svg)

```
[user@host ~]$ ls -l | tee /tmp/saved-output | less
```
 Redirects the output of the `ls` command to the `/tmp/saved-output` file and passes it to the `less` command, so it is displayed on the terminal one screen at a time.
```
[user@host ~]$ ls -t | head -n 10 | tee /tmp/ten-last-changed-files
```
If you use the `tee` command at the end of a pipeline, then the terminal shows the output of the commands in the pipeline and saves it to a file at the same time.
```
[user@host ~]$ ls -l | tee -a /tmp/append-files
```
Use the `tee` command `-a` option to append the content to a file instead of overwriting it.

> You can redirect standard error through a pipeline, but you cannot use the merging redirection operators `(&> and &>>)`.

