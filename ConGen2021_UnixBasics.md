# ConGen 2021: Brief introduction to command line and BASH
## Monday, August 30

This session is meant to provide a broad overview of the command line interface, with a focus on the bash shell language. The lesson is slightly modified from last year's version taught by Dr. Amanda Stahlke (https://gist.github.com/Astahlke), and is also based on publically available workshops from www.datacarpentry.org. If you already know how to do all of the actions listed below, then feel free to skip this session and tune in for next week's more advanced workshop on redirection, automation, one-liners, scripts, and project organization. 

## Goals
By the end of this session, you should be familiar with the following. 
- Navigating different directories on your system (`pwd`, `cd`, `ls`)
- Assessing and changing file permissions (`ls -l`, `chmod`)
- Moving/downloading data (`wget`, `curl`, `scp`)
- assessing available disk space and usage (`df`, `du`, `top`, `htop`)
- viewing, copying, moving files (`head`, `tail`, `more`, `less`, `cat`, `rm`, `mv`, `mkdir`)
- wildcards and `grep`

**What is the shell? Why use command-line programs?**

## Log on to the remote R-Studio server 
Url, username, and password have been provided by ConGen Organizers. 

Check out the source panel for scripting, the conosole/terminal, environment and history pane, and the files, viewer, packages, and help pane. You can customize this if you like.  

## Natigation: Here we go! 
Let's orient ourselves to this environment.  Click the terminal tab in the console/terminal/jobs pane, in the lower left for deafult confirguration. 

The command prompt begins with your username @ ip-adress and a colon (:), then a tilde (~) for your home ***directory*** and a $ indicating the beginning of a shell prompt. A directory is like a folder in your files finder.

Print your working directory with `pwd`, our first ***command***. This command returns where you are in the structure of the server.

```{bash}
user1@ip-<###>-<##>-<##>-<###>:~$ pwd
```

Returns for me: 
```{bash}
/home/user10
```
The forwardslash (/) at the beginning indicates the ***root*** directory. That's the top-level of the server and everything lives below that. This is the first ***path*** we'll see. This is an ***absolute path*** which is like a complete address, in this case starting from the root. A ***relative path*** starts from your current directory. An absolute path is similar to having the GPS coordinates for a destination, whereas a relative path is similar to getting directions to a destination based on where you currently are. I often start troubleshooting bioinformatic issues by checking paths. 

What else is in your home directory? Use `ls` to list the contents of your home directory. 

```{bash}
ls
```

Returns for me: 
```{bash}
Desktop    Downloads  Pictures  R          Videos                    augustus  instructor_materials  shared             user10.data
Documents  Music      Public    Templates  Zip_Folder_64_bit_191125  easypop   neestimator           thinclient_drives
```

RStudio has nice color encoding to help us identify different types of contents. Default setting has light blue for directories, black for files, and red for executables. 

Let's list one of those directories using the `ls` command and an ***argument***, instructor_materials. This is the basic format of shell programming, like many other languages. 

### A few tips as we get going here: 
If you start typing "ins", and hit ***\<TAB>*** the computer will auto-complete for instructor_materials. If there are multiple matches, it will only auto-complete as far as the matching part. If you hit ***\<TAB> \<TAB>***, it will list the possible matches.  USE TAB-COMPLETE!! This will reduce mistakes and make you more efficient. 

You can scroll-up in your command history with the up- and down-arrow keys. 

```{bash}
ls instructor_materials 
```

Returns: 
```{bash}
Amanda_Stahlke   Chris_Funk     Eric_Anderson  Joanna_Kelley  Mike_Miller     Rob_Waterhouse  Steve_Mussmann
Brenna_Forester  Craig_Primmer  Gregg_Thomas   Marty_Kardos   Rena_Schweizer  Robin_Waples
```

Arguments often have **flags** to modify the execution of a command. Single dash ***-*** have single-character options. Double-dash ***--*** have multi-character. Which flags can you use to modify the `ls` command? How do you find out? 

Most programs in shell have a manual (also known as documentation) associated which you can access with the command `man`. 

```{bash}
man ls 
```
You can use the space bar to scroll through the man page, and can press ***q*** to quit. 

Let's sort the instructor materials by the most recently modified with the `-t` flag. What else do you use often or could be useful? 

```{bash}
ls instructor_materials -t
```

What do the -h and -l flags provide? Note they can be strung together here with the single dash. 

```{bash}
ls instructor_materials -lth
```

When in doubt with many programs, useful documentation is often provided with --help. 

```{bash}
ls --help
```

Let's change directories with `cd` , into my instructor materials. From your home directory, the relative path is instructor_materials/Rena_Schweizer/. What is in this directoy? 

```{bash}
cd instructor_materials/Rena_Schweizer/
```

A few very useful ***special characters***
- ~ represents your home directory.
- . represents your current directory. 
- .. represents the directory up one level. 
- \* is a wildcard and represents one or more characters. 

```{bash}
ls .
```

```{bash}
ls ..
```

You can chain these together:

```{bash}
ls ../..
```

```{bash}
ls ~
```

If you execute `cd` without any arguments, it will take you back home.

```{bash}
cd
```

Let's prepare to download and view some data by changing to our user directory folder (mine is user10.data). 
```{bash}
cd user10.data/
```

## Let's start by downloading a practice data set. 

Yay! Your first sequencing run is done and you've received an email from the sequencing facility that your data are ready. Now what?? Depending on the facility, you may use `ftp`, `wget`, or `curl` to download the data. Today, we'll use `wget` (World Wide Web get). 

```{bash}
wget https://www.dropbox.com/s/l6iewp5f0c3gxr3/S144_L006_R1_sub.fastq.gz
wget https://www.dropbox.com/s/9hc6n8zsmc5raj7/S144_L006_R2_sub.fastq.gz
```

These commands will download a set of forward (R1) and reverse (R2) reads from a deer mouse exome. You should always try to look at the data, even if you process the bulk of it with a program. Take a look at one of these files. 

```{bash}
cat S144_L006_R1_sub.fastq.gz
```

AH! Too much data and it looks garbled, too. Hit **Ctl+c (^c)** to quit a running process or abort a task. I use this more often than I care to admit. You can see that the file is of type "fastq.gz" where the ".gz" indicates the file has been compressed. File compression can save huge amounts of space! For today, though, let's uncompress the files. We can also use the `clear` command to clear the current screen.  

```{bash}
clear
gunzip -c S144_L006_R1_sub.fastq.gz > S144_L006_R1_sub.fastq
gunzip -c S144_L006_R2_sub.fastq.gz > S144_L006_R2_sub.fastq

```
Let's use our familiar `ls` command with some additional options, to see the difference in file size. Again, the 'l' stands for 'long' format, which means more detailed information is provided for each file. The 'h' means 'human-readable' file sizes, and 't' sorts by date modified. Don't forget you can always use `man ls` to see all the detailed options. 

```{bash}
ls -lht
```
This long version of the directory gives us quite a bit of information! 
- Column 1 provides information if the content is a directory ('d'), file ('-'), or a link ('l'). The next 9 characters provide information on the file permission, with 3 characters for the Owner, the next 3 for the Group owner, and the last 3 for everyone else. Each set of 3 characters provides information on whether members of that group can read it ('r'), write to it ('w'), or execute it ('x'). 
- Column 2 tells us about how many links are to this file.
- Column 3 tells us about who is the owner of the file/directory.
- Column 4 tells us about who is the group owner of the file/directory.
- Column 5 tells us about the size of the file/directory in bytes unit (with the `-h` flag makes it in Byte, Kilobyte, Megabyte, Gigabyte, Terabyte and Petabyte)
- Column 6 provides the abbreviated month, day-of-month file was last modified, hour file last modified, minute file last modified. 
- Column 7 is the file or directory path name. 
   
At first look, we can see that the compressed version of our fastq file is about 1/4 the size of the uncompressed version. 

## Modifying permissions and backing up your raw data 

One of the first things I do when I get new data is I make a backup of it that is write protected. Let's do that now using the `mv` command. Depending on how it is used, `mv` can either rename a file or move a file to somewhere else.  

```{bash}
mkdir data_backup
mv S144_L006_R*_sub.fastq.gz data_backup
cd data_backup
ls -l
```
These commands make a copy of the data in a new directory called "data_backup", then list the permissions for the files. What are the current permissions for the owner of the file? 

We can then modify the permissions of files using the command `chmod` and flags to add or remove read, write, or execute ability. Our goal for now is to change permissions on this file so that you (the owner) no longer have write permissions. We can do this using the chmod (change mode) command and subtracting (-) the write permission -w.

```{bash}
chmod -w S144_L006_R*_sub.fastq.gz
```
We can use our `ls` again to check that we've changed the permissions. 

```{bash}
ls -l
```

And, we can prove to ourselves that we have modified the permissions by trying to delete the files using `rm`

```{bash}
rm S144_L006_R*_sub.fastq.gz
```

The output should ask if you actually want to remove the write-protected files. You should answer with an 'n'. If you say yes, you will remove the file forever! Using the command `rmdir` will delete directories. 

As you create files and directories, remember: 
* File names that start with a period are hidden. You can view them with **ls -a**
* Bash is case-sensitive. file1.txt and File1.txt are different. Be consistent. 
* Do not (!!) embed spaces in file names. Use file1 or File_1 or file-1 or SnakeCase. I prefer underscores because R interprets - as subtraction, and I prefer starting filenames with lower case so I don't have to type in the upper case letter each time.  

## What sort of resources do I have available on a computer?

Often, we run analyses on a variety of computers, including our own laptop, a shared lab server, or an institution-wide cluster. Before I start a new project or set of analyses, I like to see how resources are available for me to download data, generate files, run multiple jobs, etc. 

To assess how much free disk space is available, you can use the `df` (display free disk space) command. 

```{bash}
df -h
```
Since most or all of us are using the AWS setup for ConGen, a lot of these directories may be unfamiliar. On my lab server, it looks more like this ## INSERT PHOTO ##

To check how much space a single directory takes up (say, your /user directory which you may have been told to keep under a certain size), you can use `du` (display disk usage statistics), 

```{bash}
du -ha 
```
This will display the file size in human readable format (-h) for all files (-a) within the current directory, with a total at the bottom. If you only want to display the usage for a particular directory and not all of its subdirectories, you can do so, too. 

```{bash}
du -hd1 [directory name]
```
This will provide the file sizes of subdirectories to a depth only one below the current one (-d1). 

Some genomic software is very memory intensive, or for java programs, we can tell java how much memory to use, so it would be helpful to know what we have available on a computer. One way we can do this is with the `free` command. 

```{bash}
free -mh
```
This will tell us, in human-readable format, the total installed memory (i.e. total RAM), the used memory (memory in use by the operating system/processes), and free (any memory not in use). The "available" value is what you might want to pay attention to, since it provides an estimate of how much memory is available for starting new applications, without swapping. Read more about interpreting the output of `free` here: https://stackoverflow.com/questions/6345020/what-is-the-difference-between-buffer-and-cache-memory-in-linux

Before we submit jobs, we also might want to know the status of our system, and what processes are running, so that we can decide how many jobs we can submit simultaneously. We can see what is currently running on a computer using either `top` or `htop`. `top` is the default on most linux systems, but `htop` is more straightforward to interpet. Read more about them here: https://www.unixtutorial.org/commands/top and https://htop.dev/

Note that if your server has a queueing system, such as qsub, your job submission process will be different, and the queueing software will help manage CPU and memory usage. 

## Viewing files

Let's put our raw fastq files in a directory to stay organized. 

```{bash}
mkdir raw_fastq
mv *.fastq raw_fastq
```

Let's look at the first few lines of one of our fastq files with the command `head`. 

```{bash}
head raw_fastq/S144_L006_R1_sub.fastq
```

You can do the same with `tail` for the end of the file. Both commands have an option `-n` for the number of lines. 

```{bash}
tail -n 4 raw_fastq/S144_L006_R1_sub.fastq
```

Just a side note that you can make your cursor jump around the command line by using some handy shortcuts, e.g. 
- **Ctl+a (^a)** moves your cursor to the beginning of the line
- **Ctl+e (^e)** moves your cursor to the end of the line
- **Ctl+w (^w)** deletes the word to the left of your cursor
- **Esc+d** deletes the word to the left of your cursor
- **Ctl+u (^u)** deletes everything to the left of your cursor
- **Ctl+k (^k)** deletes everything to the right of your cursor

It's useful to know about the fastqc file encoding: https://en.wikipedia.org/wiki/FASTQ_format. Fastqc uses these data to generate a really useful report. 

Let's check how many reads we have in each file using `wc` (word count). 

```{bash}
wc -l raw_fastq/S144_L006_R*_sub.fastq
```
How many reads are there?

<details>
	There are 4 million lines in each file. Given that there are 4 lines per 1 sequence read in a .fastq file, there are 1 million reads in each file. 
</details>
					
Given how large these files are, it is not useful to use `cat` to try to look at them. We can view parts of the file using `more` or `less`. Like the manual pages, we can use the space bar to scroll, and the 'q' to quit. Can you tell what the difference is between the two commands? 
		
```{bash}
more raw_fastq/S144_L006_R1_sub.fastq
less raw_fastq/S144_L006_R1_sub.fastq		
```
		
# Fastqc
I almost always run Fastqc first when I get a new data set. I check for the expected number of reads, read length, overall quality, and duplicate rate. 

Check out fastqc options with the --help option.

```{bash}
fastqc --help
```

Make a directory to capture the output of fastqc. 

```{bash}
mkdir quality_metrics
```

Use the wildcard \*.fastq to call both R1 and R2.  

```{bash}
ls raw_fastq/*.fastq
```

Returns: 
```{bash}
raw_fastq/S144_L006_R1_sub.fastq
raw_fastq/S144_L006_R2_sub.fastq
```

## Run FastQC

```{bash}
fastqc raw_fastq/*.fastq -o quality_metrics/
```

While this is running, look through the provided 'good' and 'bad' report examples. 

http://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html
http://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html

More on common fastqc red-flags: https://www.dna-ghost.com/single-post/2017/09/01/How-to-interpret-FASTQC-results 

When complete, check out the output. Use Rstudio to open the html report in your web browser.

```{bash}
ls quality_metrics/
```

Hopefully today's lesson has helped you feel more comfortable working from the command line in UNIX. The more you practice, the easier and more fluid it will be!

# Other resources

https://wikis.utexas.edu/display/CoreNGSTools/Linux+fundamentals

https://astrobiomike.github.io/unix/

http://linuxcommand.org/index.php

https://datacarpentry.org/shell-genomics/

https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424