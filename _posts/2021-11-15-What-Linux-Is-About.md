---
layout: post
title: What Linux is about?
---

Dig into the kernel's architecture.

This article covers:

1. [Introduction](#introduction)
2. [Talking with the Linux kernel](#talking-with-the-linux-kernel)
    * [Files and what we can do with them](#files-and-what-we-can-do-with-them)
    * [Syscalls: the bridge between user and kernel](#between-user-and-the-kernel)
    * [Using syscalls in C](#using-syscalls-in-c)

## Introduction
When we hear about Linux our thoughts are probably somewhere between convoluted command line interface and that cute penguin mascotte. In general, Linux is seen as something strange and awkward. In reality it is even worse and
you are going to know why in a second.

Firstly, imagine who is responsible for providing a cute kitties from the internet cable to your browser.
You are right- some software. The software responsible for doing all the magic with bits, bytes and streams tangled in the hardware is the operating system and- in more depth- device drivers.

Each part of the computer, whatever it is: a GPU, hard drive or floppy disk drive needs a specific piece of software which knows all its behaviors, needs and issues. In technical terms- a device driver is a piece of software which runs in the operating system and controls the specified hardware in means of sending the data back and forth, handling interrupts and providing a simple, yet reliable interface for the user.

**Note:** The term *user* used in this tutorial does not indicate a person who uses software, but rather a computer engineer who uses the device drivers in his programs.

## Talking with the Linux kernel

Let's recall how we can read or write files from command line. For writing the data into file we can use **echo** followed by some data and stream redirection like that:

```console
$ echo "some data" > my_file.txt
```

To read previously written text file **cat** command can be used:

```console
$ cat my_file.txt
```

### Files and what we can do with them

We got used to utilize some certain types of files, for example pdf, text or binary files and think about them in means of some data saved in non-volatile storage (most often on the hard drive of our PC or in the cloud). The term **file** in Unix and Unix-like systems (which Linux belongs to) is more broad than that.

**Note:** Files represent every kinds of resources, whatever these resources are, and give an ability to use them.

For example **text files** represent text (a resource) encoded with some type of encoding (the most often utf-8) which reside somewhere in the storage. We don't have to know where these files are saved, actual sector nor address of the memory occupied by the data is not needed for us to read or write (ability to use them) the data to the text file. The kernel knows where specific files are saved and how to access them on a certain types of storage.

### Between user and the kernel

After getting deeper into reading and writing a text file you can wonder how **cat** or **echo** know the way to read or write the text file. Do these programs know all quirks of hard drive and all filesystems? I won't surprise you with the answer: they simply don't. So, what they do to put the data back and forth the text file residing in the storage? They use the special interface given to them by the kernel. This interface allows user's programs to access any resources they want in an unified way. This interface is called **System Calls**.

There are many system calls which can be used to communicate with the kernel. Some of them are simple and most probably you have met them while writing some programs in any programming language. Let's memorize how **open** syscall can utilized to open a file in C programming language.

### Using syscalls in C

The snippet below presents the usage of **open** function which is accessible after including *fcntl.h* header.

```C
#include <fcntl.h>

int main(int nargs, char*args[])
{
    int file_descriptor = open(FILENAME, O_WRONLY | O_CREAT);

    // Every open file must be closed
    close(file_descriptor);

    return 0;
}

```

It seems that code does nothing more than opening and closing the file with name specified as FILENAME macro (which can be supplied during the compilation process). In reality, the **open** function, which implements **OPEN** syscall performs this system call and takes an unique **file descriptor** from the kernel. Moreover, if the file does not exist, it is created as a result of calling this function with **O_CREAT** flag supplied as an argument. Calling this function results in instructing some part of kernel to talk with the storage. Let's recall the *Introduction* part of this article and guess what piece of software is responsible for knowing how to talk with the hardware. Yes- device drivers. System call is issued in the conext of some file- let's say a text file. In this case, kernel instructs a driver responsible for talking with the storage to provide a data written in some area marked with the filename supplied by the user.

**Note:** System call is an interface to exchange the data with the kernel. System call must be called on certain file. The file is bonded with the driver inside kernel which receives data from the user transferred via system calls.