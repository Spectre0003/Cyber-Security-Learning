# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# Note: This document contains only the most fundamental Linux commands and their syntax. Many commands have additional uses and options that may be covered in later sections.

# What is Linux?

Linux is a free, open source operating system family built around the Linux kernel known for its flexibility. It offers a high level of control over the system architecture when compared with other operating systems like Windows or MacOS.
Linux is used by many because of its performance, customizability, stability and security. Commonly, it is used extensively in Security, Hosting, Development and Education.

# Basic Linux Commands

Linux Terminal is the Command Line Interface for the Linux Systems that utilizes system commands - which can be used to do powerful things - to interact with the operating system.
Some basic commands are given as follows:

## echo
A simple command which literally 'echoes' whatever is written afterward. Normally used together with other commands or in scripts to print on the terminal.
Syntax: $echo [String]

## whoami
A very simple command that tells which user is currently accessing the system.
Syntax: $whoami

## ls
This command is used to list all the files and directories in the current directory/folder. This command can be modified further by specifying path or by specifying whether to access hidden files and more.
Syntax: $ls

## cd
This command is used to change the current directory. It can utilise both absolute path or relative path based on the inputs. To use this, one needs to only specify the path of the directory to go to after the command. Typing '..' after this command tells the system to go back one directory.
Syntax: $cd [directory_path]

## pwd
This command is used to show the present working directory of the user. Whatever commands that are typed in the terminal using relative pathing occur inside the directory given as an output for this command.
Syntax: $pwd

## cat
This command is used to concatenate or read files. Typing the name of a file causes the output of the file to show up on the terminal. It can also be used to concatenate two or more files and then show the resulting output in the terminal. 
Syntax: $cat file1.txt              [Contents of file1 will be shown on the terminal]
        $cat file1.txt file2.txt    [Contents of file1 and file2 will be shown on the terminal]
        $cat file1.txt > file2.txt  [The contents of file1 will be copied to file2]
        $cat file1.txt >> file2.txt [The contents of file1 will be appended to file2]

## find
This command is used to pattern match any file names based on given conditions. It uses shell-style wildcards together with options to enhance search capabilities.
Syntax: $find [-option] [pattern] 

## grep
This powerful command is used to find patterns inside files or given text. It also utilizes regex expressions and is one of the most used commands.
Syntax: $grep [-option] [pattern] [file/(s)]

## wc
This command is used to count lines, characters, words and bytes from a file or text of your choice.
Syntax: $wc [-option] [file]

## man
This is the help command. It is an alias for 'manual'. It provides detailed descriptions, usage and syntax for any command typed after.
Syntax $man [command]

## touch
This command is used to initialize a file. It creates an empty file in the current working directory.
Syntax: $touch [filename]

## mkdir
This command is used to create a new empty directory in the present working directory.
Syntax: $mkdir [directory_name]

## cp
This command copies files and directories from one location to another.
Syntax: $cp [source] [destination]

## mv
This command can move files and directories from a given location to another. It can also be used for renaming.
Syntax: $mv [filename] [destination]

## rm
This command is used to remove a file or a directory (delete it). The option [-r] is used to recursively delete the directories and their contents.
Syntax: $rm [filename]
        $rm -r [directory_name]

## su
This command is used to switch between two users. Note that you may require the root password for the same.
Syntax: $su [username]

## chmod
This command is used to change the permissions that a file has.
Syntax: $chmod [permission] [filename]

## ps
This command shows current active processes running on the system.
Syntax: $ps

## kill
This command is used to terminate processes using their process id.
Syntax: $kill [process_id]

## systemctl
This command is used to manage system services using systemd.
Syntax: $systemctl [option]

## sudo
This command is used to execute any other command with the privileges of another user, typically a superuser with higher privileges. Note that the user password will be required to do the same.
Syntax: $sudo [commmand]

# Additional Symbols

Linux commands may also require some additional symbols which enhance the commands.

## &
This symbol is used with any command to make it run in the background.

## &&
This symbols allows for the sequential execution of two different commands. Note that the second command only runs once the first command is successfully executed.

## >
This symbol lets one redirect the output of one command.

## >>
This symbol lets one redirect the output of a command and append it.

## |
This symbol allows the output of one command to pass as input of another command.

## ;
This symbol allows one to run multiple commands regardless of success.

## *
This symbol acts as a wildcard for pattern matching expressions. It means 'zero or more characters'