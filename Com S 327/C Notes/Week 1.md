Prof Jim Lathrop

# Version Control Systems (VCS)

Question: How do you maintain different versions of software that are developing?
- You save "snapshots" of files, eq. proj_V1.java, proj_V2.java, etc

Suppose you are exploring different design options - array based vs linked list for example.
- stack-array-working.c
- stack-list-working.c
- master file of stack.c, eventually dump code in there

What are the problems with this?
- disorganized directories
- error prone

If you have a lot of source files, what could cause a lot of problems?
- Different versions of files may need to be synchronized for different things

One possible solution - throw all that shit in a folder
- Store each snapshot in a separate folder
Issues with this
- Inefficient use of storage, unchanged files can be copied
- Still error prone
- Hard to make retroactive changes to old versions that may have very different files
	- Will you copy and paste a file for every single version?
	- What if they have changes specific to them? Now you have to manually enter the change

You could write scripts to automate this, but its very complicated

Why would you do this if it already exists?

### Version Control Systems Consists of:
- Some sort of database that efficiently stores past snapshots. 
	- This is called a **Repository**
	- This works for all types of projects
- A set of utilities (programs or scripts to manage the snapshots)

### How They Usually Work
- Can be on a local machine or remote server
- You have what's called a local working copy of some snapshot
	- This is often a directory of/or subdirectories on your local machine
- Every so often (or when appropriate) you can take a snapshot and "check in" all files under version control in your working directory
- You can recover previous versions of a snapshot by "checking out" 
	- makes a copy all files in the working directory as they were when the snapshot was taken

#### Branching
Development does not need to be linear
If we want to explore other implementation options or make disruptive changes, you may want to create a "branch"

VCS will let you migrate changes between branches

Remote Repositories (or local)
You can have multiple working copies
- On different machines
- with different developers
- working on a different version

But what could happen?
Alice edits stack.c and changes line 273
Bob edits stack.c and changes lines 273 and 274

What happens?
If edits are far apart - it will probably work.
Otherwise it will mark the file as **conflicted.**

**Commands**
`git clone <url>` - copies the git url
`git status` - tells you what has changed in the repo
`git add` - add to the staging area, anything in the staging area can be committed
`git commit` - make a snapshot in your local repo
`git push` - sends the content of the staging area to the remote repository
`git pull` - fetches changes from server and merge into local repository
`git checkout <which>` switch to a different snapshot
`git diff` - look at differences between versions



### Unix And Shells
Today unix can (and does) resemble other operating systems with a GUI interface
We will focus on the command line interface used in a shell

**What is a shell?**
- A program that allows the user to interact with the OS via a keyboard
- Contains several built in programs or functions to perform specific tasks
- Allows user to execute other programs that are not built in on the OS "inside the shell"

Many shells will work with *bash* here
- A shell must parse and execute commands
	- External commands and shell are mostly written in c
- Shells usually run interactively, but you can make shell scripts (run commands in file)

Interactive Shell
Note that additional arguments that changes the behavior of a command is often called a **switch**
- users get a prompt
- shit goes after the prompt, commonly $ or current working directory
- we will use `>`
- Commands by convention are 
	- `> cmd ar1 arg2 ... arg n`
- A few critical commands:
	- `man` - display the manual pages
		- while manual page to display
		- many options to the argument
		- `man man` displays the manual manual
		- Useful sections:
			- Section 1 - general commands like man
			- Section 3 - C library function - java docs but for C
			- Section 2 - system calls
- Directories (folders in windows)
	- shells have a concept of the *Working Directory* - the directory you (the shell) are in right now
		- If you type a command like `ls` which prints the content of the directory, it will only display what is in the *working* directory
		- Commands that operate on files will use the working directory unless a file path is specified
	- `pwd` - print working directory
		- Usually is relative to have a directory
		- Separator in file path is / ( \\ on windows) (bc its stupid)
	- `cd` - change working directory
		- with no arguments, cd will take you back to home directory
		- `cd <path>` will take you to path, either relative or absolute
	- `ls` - list files / directories
		- with no parameter, lists all files in the current working directory
		- with a parameter, it lists all files in the given path
	- `mkdir`
		- make directory in CWD or 
	- `rmdir`
		- remove directory, either CWD or 
	- `cp <src> <dst>` 
		- copy files from src to dst
	- `mv <rename>`
		- Remove files and put them in a destination directory
		- its like cut and paste
	- `rm`
		- Remove
	- `cat <f1>, <f2>`
		- concatenate, essentially print
	- `less`, `more` 
		- display file with pagination
	- `ssh`
		- secure shell


A note about paths -  absolute paths start with a "/" and go all the way from the user and give complete path, starting at the root directory, where as relative paths do not start with a "/" and is relative to the working directory

Relative paths: . means current directory, `./mydir` $\equiv$ `cd mydir`
Another special symbol is `~/file`, where `~` $\equiv$ home directory 

In Unix and Windows, a process is a name for an executing program
- The OS will set aside memory for the process
- The process cannot read/write/execute (r/w/x) outside of the allocated memory

Each process has its own set of open files, including 3 special files
(0) Standard Input (stdio) (stdin)
	The default place to read from
(1) Standard Output (stdio) (stdout)
	The default place to write to
(2) Standard Error (stdio) (stderr)
	The default place to write error messages to

#### Redirection
For *any* command or program, you can send the output to a file (redirect to a file) using:
`> cmd args ... > outfile`

You can append to an output file using
`> cmd args ... >> outfile`

For any command or program you can read input from a file (as if the user typed it)
`> cmd args... <`

For any program or command, you can link them together with pipes. That is, you can send the output of one program to the input of another.
`> cmdA | cmdB`

Both these programs are tun at the same time

