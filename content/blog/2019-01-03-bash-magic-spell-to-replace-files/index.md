---
title: Bash magic spell to replace files
date: '2019-01-03T22:12:03.284Z'
template: 'post'
draft: false
url: '/bash-magic-spell-to-replace-files'
category: 'tips'
tags:
  - 'script'
description: 'How to easily replace files with bash'
---

![Bash Logo](./bash.jpg)

Most recently, at my work project, I assigned myself to a task to replace a png file across the project with a new graphic.
Same file name, different pixels.  
A quick search for the file in VSCode showed around 35 results across different folders.

> But why there are 35 copies of the same png in the project?  
> Good question for which I have no answer 🤔

What a can be done here to complete the task?

1. Manually track all the files in file explorer and replace them. This can take around 15 to 25 minutes depending if you double check copies and how fast you click... boooooring!

2. Or find a bash command to do the work for you.

Challenge accepted!  
What should be done?

- have a reference to file with new data

- find the file by name

- replace found file with new data

- repeat until all files in the folder are changed

After some googling, reading stack overflow and MAN pages, I stitched this line to replace a file with new data:

```bash
find . -print -type f -name apple.PNG -execdir cp /mnt/c/Users/Maciej_Chmura/replace_source/orange.PNG {} \;
```

Lets break it down to know exactly what is happening:

```
find 👉 bash find command
. 👉 current execution context, current folder
-print 👉 log the process
-type f 👉 specify to look for files
-name file_name 👉 specify file name to search
-execdir 👉 execute command from the subdirectory
cp from_file to_file 👉 copy from one file to another
{} 👉 is a placeholder for file that `find` will find
```

Don't stop here, learn more about these commands:  
[find](https://ss64.com/bash/find.html),
[cp](https://ss64.com/bash/cp.html)

With this little line, all files in the current folder and subfolders will be replaced.

After this research, I thought of another use case.  
What if you need to rename the file?  
The command is almost the same:

```bash
find . -print -type f -name apple.PNG -execdir mv {} orange.PNG \;
```

Hopefully, this will save you some time in the future when encountered in a similar case.  
Do you know any magic bash spells?
