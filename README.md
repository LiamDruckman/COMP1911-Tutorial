# C Cheatsheet

Note: This cheatsheet has more of a focus on learning C content (read this top to bottom as it goes step by step). It also has a fairly strong focus on the C content learnt in COMP1911 with a few extended topics such as linked lists (which will probably not appear in the final exam) as well as some linux shell commands to help along the way.

## Table of Contents

- [C Cheatsheet](#c-cheatsheet)
  - [Table of Contents](#table-of-contents)
  - [Linux/Shell Commands](#linuxshell-commands)
    - [Navigation](#navigation)
    - [Manipulating Files and Directories](#manipulating-files-and-directories)
  - [Data Types](#data-types)
    - [Int](#int)
    - [Char](#char)
    - [Double](#double)
    - [Boolean](#boolean)
  - [Control Structures](#control-structures)
    - [If Blocks](#if-blocks)
    - [While Loops](#while-loops)
    - [For Loops](#for-loops)

## Linux/Shell Commands

Linux is an operating system developed by Linus Torvalds (you can probably tell where the name came from) and places a much greater emphasis on using command line tools to perform operations. If you're primarily a mac user, you may be in luck as both Linux and MacOS are "Unix-like" and share a lot of similarities. For Windows users however, it may be a slightly larger leap to get used to it. But regardless, once you've got a few basic commands under your belt, you'll find Linux is not nearly as mystifying as it once seemed.

### Navigation

Linux uses a tree-like file system where everything is inside of a specific folder (called directories). This includes normal files and other subdirectories, allowing you to organise all your data into separate directories much like in other operating systems.

In Linux the highest level of directory is called the root directory which has the path "/". This denotes the top of the file system and all other directories and files are contained within it.

The one we are most concerned with, however, is your home directory. This is the directory that you will start in when you open up your terminal window. It has a full path name starting from the root directory along the lines of "/import/adams/3/z5309229" but it is also assigned a special name: "~".

Additionally, the command prompt in the terminal usually ends with a ``$`` or ``%`` to show where you write your commands so I will indicate that a command is being written in the terminal by starting the line with ``$``. Any output will be written straight afterwards without the ``$``.

Let's say you open up your terminal into your home directory and you want to know what files and directories are here. You would use the "ls" command and receive output something along these lines:

```bash
$ ls
COMP1911  Documents  Music     Practice  Templates
Desktop   Downloads  Pictures  Public    Videos
```

This command (short for "list") shows all non-hidden files and directories in the current directory. In this case they all happen to be directories so let's change to the COMP1911 directory and run "ls" again.

```bash
$ cd COMP1911
$ ls
lab01  tute01
```

As you can see, we can use the "cd" command to change directories (I'm sure you can figure out what it stands for). So inside the COMP1911 directory we only have two files.

Now, if we want to go back up one directory we can use the "cd" command again. But this time the name of the directory isn't so obvious. We call the directory one level above the parent directory and it has a special name in Linux: "..". In the same vein we refer to our current directory using ".".

To demonstrate this we'll introduce another command called "pwd" which stands for print working directory. This gives us the path of the directory we're currently in.

```bash
# Show our current directory
$ pwd
/import/adams/3/z5309229/COMP1911

# We'll now try to change to our current directory
$ cd .
$ pwd
/import/adams/3/z5309229/COMP1911

# Now let's change to the parent directory
$ cd ..
$ pwd
/import/adams/3/z5309229
```

If you can recall, this is our home directory that we started in and maybe it would be nice for us to get to here quickly because it's probably got a lot of important things in it. So instead of having to navigate up and down directories one at a time, we can supply a full or relative path to the "cd" command and it will take us straight to that directory.

```bash
# Show our current directory
$ pwd
/import/adams/3/z5309229/COMP1911/lab01

# Let's get back to our home directory, remembering the special name for it
$ cd ~
$ pwd
/import/adams/3/z5309229

# Let's go back there now and I'll show you another cool trick
$ cd COMP1911/lab01
$ pwd
/import/adams/3/z5309229/COMP1911/lab01

# Now if we don't give anything to "cd" it defaults to our home directory
$ cd
$ pwd
/import/adams/3/z5309229
```

Now you can navigate a Linux file system! Great Job!!

### Manipulating Files and Directories

Now let's see if we can get on to making some of our own files and directories.

The first thing we'd probably like to know is how to make our own directories. We can do this using the "mkdir" command.

```bash
# Let's have a look where we are and what's in our directory
$ pwd
/import/adams/3/z5309229/COMP1911
$ ls
lab01  tute01

# Now let's make our new directory
$ mkdir tute02
$ ls
lab01  tute01  tute02
```

Now let's go into our new tute02 directory and create a new file. There are a couple ways we can do this; the pure command line way would be using the "touch" command which creates an empty file, otherwise most likely you'll just use your favourite editor to create it.

```bash
# Let's check where we are and move into our new directory
$ pwd
/import/adams/3/z5309229/COMP1911
$ cd tute02

# There's nothing in here yet
$ ls
# Let's create the file
$ touch q1.c
$ ls
q1.c
```

Now if we want to delete the file we just created we can use the "rm" command. Please be very careful with this command. I repeat, **BE VERY CAREFUL**. Once you delete a file or directory in Linux it cannot be recovered and you will lose anything you had there. With that said, let's see it working.

```bash
$ ls
q1.c
# Remove the file
$ rm q1.c
$ ls

# Now we'll make the file again with an editor
$ gedit q1.c &
# Assume we've now saved the file in the editor
$ ls
q1.c
```

Lastly, let's go through an even scarier version of "rm". This one is for removing directories and carries with it the same warning, but when you delete a directory all subdirectories are deleted too. So once again, please be very careful using this.

So say we wanted to remove our tute02 directory we can do it as follows:

```bash
$ cd ..
$ ls
lab01  tute01  tute02

# Adding "-r" after rm makes the command recursive. This means that it
# goes into the directory and deletes all of its files and
# subdirectories first before deleting the directory itself
$ rm -r tute02
$ ls
lab01  tute01
```

Now you know how to create and remove files and directories. Let's get programming!

## Data Types

### Int

This is the probably the most commonly used data type in any language, and that includes C. It simply stores numbers with the only constraint being that they have to be whole numbers (or integers, hence the name). They can be either positive or negative and we can declare and int type variable as follows:
```C
int number;
```

However, this leaves the variable uninitialised (without any value assigned to it) which can be problematic if we try to use it. This can cause issues in our programs so we try to always give it a default value of some sort, usually 0 but that is simply convention and your needs may change.
```C
int number = 0;
```
### Char

This is the next most common data type in C, and again, as the name suggests, it stores characters. It can be any one of the characters defined by the [American Standard Code for Information Interchange](https://www.ascii-code.com/) (ASCII). If you ever want to have a look at it just type "ascii" in your terminal.

We declare a char in the same way we declare an int but we would usually take a different default value which varies greatly:
```C
char character = 'a';
```

One thing you'll notice is that we put single quotes around the 'a' character. The use of specifically single quotes instead of double quotes is important: this serves two purposes. First of all this ensures that the program knows 'a' is not a variable name, and second it performs a special operation where the character 'a' is converted to the corresponding ASCII value from the table linked above. So the real value assigned to character is 97.

This gives you a clue about what the char type stores. It's actually just an integer that gets converted to a character using the ASCII table. So why do we use ints and chars separately then if they store the same things? Well they're not actually exactly the same, the int type has 4 bytes or 32 bits of space allocated to it, whereas the char type only has 1 byte or 8 bits. This is to be more space efficient as we only need 1 byte to fit all the different ASCII values (this is by design) but ints can store much, much larger numbers which is quite useful for us.

### Double

### Boolean

## Control Structures

### If Blocks

### While Loops

### For Loops