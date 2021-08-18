# ConGen 2021: Brief introduction to command line and BASH

This session is meant to provide a broad overview of the command line interface, with a focus on the bash shell language. The lesson is slightly modified from last year's version taught by Dr. Amanda Stahlke (https://gist.github.com/Astahlke), and is also based on publically available workshops from www.datacarpentry.org, . 

## Goals
By the end of this session, you should be familiar with the following. 
- Navigating different directories on your system (`pwd`, `cd`, `ls`)
- assessing available disk space and usage (`df`, `du`, `top`, `htop`)
- viewing, copying, moving files (`head`, `tail`, `more`, `less`, `cat`, `rm`, `mv`, `mkdir`)
- wildcards and `grep`
- Assessing and changing file permissions
- Moving/downloading data (`wget`, `curl`, `scp`)

If all of this is old news to you, then feel free to skip it and tune in for next week's more advanced workshop on redirection, automation, one-liners, scripts, and project organization. 

**What is the shell? Why use command-line programs?**

## Log on to the remote R-Studio server 
Url, username, and password have been provided by ConGen Organizers. 

Check out the source panel for scripting, the conosole/terminal, environment and history pane, and the files, viewer, packages, and help pane. You can customize this if you like.  

## Natigation: Here we go! 
Let's orient ourselves to this environment.  Click the termminal tab in the console/terminal/jobs pane, in the lower left for deafult confirguration. 

The command prompt begins with your username @ ip-adress and a colon (:), then a tilde (~) for your home ***directory*** and a $ indicating the beginning of a shell prompt. A directory is like a folder in your files finder.

Print your working directory with `pwd`, our first ***command***. This command returns where you are in the structure of the server.

```{bash}
user1@ip-<###>-<##>-<##>-<###>:~$ pwd
```

Returns for me: 
```{bash}
/home/user1
```
The forwardslash (/) at the beginning indicates the ***root*** directory. That's the top-level of the server and everything lives below that. This is the first ***path*** we'll see. This is an ***absolute path*** which is like a complete address, in this case starting from the root. A ***relative path*** starts from your current directory. An absolute path is similar to having the GPS coordinates for a destination, whereas a relative path is similar to getting directions to a destination based on where you currently are. I often start troubleshooting bioinformatic issues by checking paths. 

I'll use the $ to indicate the beginning of a command.  What else is in your home directory? Use `ls` to list the contents of your home directory. 

```{bash}
$ ls
```

Returns for me: 
```{bash}
Desktop    Public     Videos                    instructor_materials  user1.data
Documents  R          Zip_Folder_64_bit_191125  neestimator
Downloads  README     denovo                    process_radtags.log
Music      Shiny      easypop                   temp
Pictures   Templates  info                      thinclient_drives
```

RStudio has nice color encoding to help us identify different types of contents. Default setting has light blue for directories, black for files, and red for executables. 

Let's list one of those directories using the ls command and an ***argument***, instructor_materials. This is the basic format of shell programming, like many other languages. 

### A few tips as we get going here: 
If you start typing "ins", and hit ***\<TAB>*** the computer will auto-complete for instructor_materials. If there are multiple matches, it will only auto-complete as far as the matching part. If you hit ***\<TAB> \<TAB>***, it will list the possible matches.  USE TAB-COMPLETE!! This will reduce mistakes and make you more efficient. 

You can scroll-up in your command history with the up- and down-arrow keys. 

```{bash}
$ ls instructor_materials 
```

Returns: 
```{bash}
Amanda_Stahlke   Craig_Primmer  Joanna_Kelley  Rena_Schweizer  Steve_Mussmann
Brenna_Forester  Eric_Anderson  Marty_Kardos   Rob_Waterhouse
Chris_Funk       Gregg_Thomas   Mike_Miller    Robin_Waples
```

Arguments often have **flags** to modify the execution of a command. Single dash ***-*** have single-character options. Double-dash ***--*** have multi-character. Which flags can you use to modify the ls command? How do you find out? 

Most programs in shell have a manual (also known as documentation) associated which you can access with the command `man`. 

```{bash}
$ man ls 
```
You can use the space bar to scroll through the man page, and can press ***q*** to quit. 

Let's sort the instructor materials by the most recently modified with the `-t` flag. What else do you use often or could be useful? 

```{bash}
$ ls instructor_materials -t
```

What do the -h and -l flags provide? Note they can be strung together here with the single dash. 

```{bash}
$ ls instructor_materials -lth
```

When in doubt with many programs, useful documentation is often provided with --help. 

```{bash}
$ ls --help
```

Let's change directories with `cd` , into my instructor materials. From your home directory, the relative path is instructor_materials/Amanda_Stahlke/. What is in this directoy? 

```{bash}
$ cd instructor_materials/Amanda_Stahlke/
```

A few very useful ***special characters***
~ represents your home directory.
. represents your current directory. 
.. represents the directory up one level. 
* is a wildcard and represents one or more characters. 

```{bash}
$ ls .
```

```{bash}
$ ls ..
```

You can chain these together:

```{bash}
$ ls ../..
```

```{bash}
$ ls ~
```

```{bash}
$ ls ~
```

If you execute `cd` without any arguments, it will take you back home.

```{bash}
$ cd
```

## Let's start by downloading a practice data set. 

Yay! Your first sequencing run is done and you've received an email from the sequencing facility that your data are ready. Now what?? Depending on the facility, you may use `ftp`, `wget`, or `curl` to download the data. Today, we'll use `wget` (World Wide Web get). 

```{bash}
$ wget https://www.dropbox.com/s/koz5nss7sb28buq/ME_0616_8_S6_L005_R1_sub.fastq.gz
$ wget https://www.dropbox.com/s/ru06zndjxktcib4/ME_0616_8_S6_L005_R2_sub.fastq.gz
```

These commands will download a set of forward and reverse reads from a deer mouse exome. You should always try to look at the data, even if you process the bulk of it with a program. Take a look at one of these files. 

```{bash}
$ cat ME_0616_8_S6_L005_R1_sub.fastq.gz
```

AH! Too much data and it looks garbled, too. Hit **Ctl+c (^c)** to quit a running process or abort a task. I use this more often than I care to admit. You can see that the file is of type "fastq.gz" where the ".gz" indicates the file has been compressed. File compression can save huge amounts of space! For today, though, let's uncompress the file. 

```{bash}
$ gzcat ME_0616_8_S6_L005_R1_sub.fastq.gz > ME_0616_8_S6_L005_R1_sub.fastq
```
Let's use our familiar `ls` command with some additional options, to see the difference in file size. Again, the 'l' stands for 'long' format, which means more detailed information is provided for each file. The 'h' means 'human-readable' file sizes, and 't' sorts by date modified. Don't forget you can always use `man ls` to see all the detailed options. 

```{bash}
$ ls -lht
```
This long version of the directory gives us quite a bit of information! 
- Column 1 provides information if the content is a directory ('d'), file ('-'), or a link ('l'). The next 9 characters provide information on the file permission, with 3 characters for the Owner, the next 3 for the Group owner, and the last 3 for everyone else. Each set of 3 characters provides information on whether members of that group can read it ('r'), write to it ('w'), or execute it ('x'). 
- Column 2 tells us about how many links are to this file.
- Column 3 tells us about who is the owner of the file/directory.
- Column 4 tells us about who is the group owner of the file/directory.
- Column 5 tells us about the size of the file/directory in bytes unit (with the `-h` flag makes it in Byte, Kilobyte, Megabyte, Gigabyte, Terabyte and Petabyte)
- Column 6 provides the abbreviated month, day-of-month file was last modified, hour file last modified, minute file last modified. 
- Column 7 is the file or directory path name. 
   
At first look, we can see that the compressed version of our fastq file is about 1/4 the size of the uncompressed version. One of the first things I do when I get new data is I make a backup of it that is write protected. Let's do that now. 

```{bash}
$ mkdir raw_data
$ cp ME_0616_8_S6_L005_R*.gz raw_data
$ cd raw_data
$ ls -l
```
These commands make a copy of the data in a new directory, then list the permissions for the files. What are the current permissions for the owner of the file? 

We can then modify the permissions of files using the command `chmod` and flags to add or remove read, write, or execute ability. Our goal for now is to change permissions on this file so that you (the owner) no longer have write permissions. We can do this using the chmod (change mode) command and subtracting (-) the write permission -w.

```{bash}
$ chmod -w ME_0616_8_S6_L005_R*.gz
```
We can use our `ls` again to check that we've changed the permissions. 

```{bash}
$ ls -l
```

And, we can prove to ourselves that we have modified the permissions by trying to delete the file using `rm`

```{bash}
$ rm ME_0616_8_S6_L005_R*.gz
```

The output should ask if you actually want to remove the write-protected file. You should answer with an 'n' If you say yes, you will remove the file forever! Using the command `rmdir` will delete directories. 


Let's look at the first few lines of the file with the command `head`. 

```{bash}
$ head ME_0616_8_S6_L005_R1_sub.fastq
```

You can do the same with `tail` for the end of the file. Both commands have an option `-n` for the number of lines. 

```{bash}
$ tail -n 4 ME_0616_8_S6_L005_R1_sub.fastq
```

We can identify RADseq reads, which all have the cut-site ‘ATGCAG’ with command **grep** which is super (!!) useful.

```{bash}
$ zcat instructor_materials/Amanda_Stahlke/Hands-on_Data/PHMC002B_S366_L007_R1_001.fastq.gz | head -n 200 | grep ATGCAG 
```

It's useful to know about the fastqc file encoding: https://en.wikipedia.org/wiki/FASTQ_format. Fastqc uses these data to generate a really useful report. 

# Fastqc
I almost always run Fastqc first when I get a new data set. I check for the expected number of reads, read length, overall quality, and duplicate rate. 


Check out fastqc options with the --help option.

```{bash}
$ fastqc --help
```

As you create things remember: 
* File names that start with a period are hidden. You can view them with **ls -a**
* Bash is case-sensitive. file1.txt and File1.txt are different. Be consistent. 
* Do not (!!) embed spaces in file names. Use file1 or File_1 or file-1 or SnakeCase. I prefer underscores because R interprets - as subtraction. 

Make a directory to capture the output of fastqc. Since this is the first step in my project, I start the name with 1. 

```{bash}
$ mkdir 1fastqc
```

Use the wildcard \*.gz to call both R1 and R2.  

```{bash}
$ ls instructor_materials/Amanda_Stahlke/Hands-on_Data/*gz
```

Returns: 
```{bash}
instructor_materials/Amanda_Stahlke/Hands-on_Data/PHMC002B_S366_L007_R1_001.fastq.gz
instructor_materials/Amanda_Stahlke/Hands-on_Data/PHMC002B_S366_L007_R2_001.fastq.gz
```

## Run FastQC

```{bash}
$ fastqc instructor_materials/Amanda_Stahlke/Hands-on_Data/*gz -o 1fastqc/
```

While this is running (< 10 min), take a break, look through the provided 'good' and 'bad' report examples. 

http://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html
http://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html

More on common fastqc red-flags: https://www.dna-ghost.com/single-post/2017/09/01/How-to-interpret-FASTQC-results 

When complete, check out the output. Use Rstudio to open the html report in your web browser.

```{bash}
$ ls  1fastqc/
```

## Materials below this line could be used in the more advanced workshop!! 


Try opening the zip files. 

```{bash}
$ unzip  1fastqc/PHMC002B_S366_L007_R*
```

This doesn't work. We have to unzip them one at a time. In the future, we'll have many plates-worth of fastqc to summarize in our pipeline, so let's write a for-loop. 

```{bash}
$ for filename in 1fastqc/*.zip
> do
> echo $filename
> done
```
The for-loop begins with the formula **for** <variable> in <group to iterate over>. Here, filename is the variable that will take on each item of the list that expands from /*.zip. 

Then a new line signals the end of the list, and the shell provides the **>** prompt because it's expecting more. The next command **do** proivdes instructions within the loop. Here, we provide the command to **echo** the filename itself. We end the loop with **done**. If you make any mistakes here, you can ^c to reset.  

Let's echo and unzip each file.

```{bash}
$ for filename in 1fastqc/*.zip
> do
> echo $filename
> unzip $filename
> done
```

# Make this an executable script
Open a new shell script from File > New File > Shell Script in Rstudio. Write your for-loop in this script and save it in your home directory with the .sh ("shell") extension. 

Try to run this with the **sh** command and the script as the argument.

```{bash}
$ sh fastqc.sh 
```

This doesn't work because we don't have executing permissions. A note on permissons: http://linuxcommand.org/lc3_lts0090.php. The default reduces the likelihood of accidently executing something. 

```{bash}
$ ls -l fastqc.sh
```

Let's grant ourselves permissions to make it executable by "changing the mode" with **chmod**. Add e**x**ecutable permission with +x. This 

```{bash}
$ chmod +x fastqc.sh
```

Run this script with **sh**. 

```{bash}
$ sh fastqc.sh
```

You've written and run your first shell script!  Let's make a directory for our scripts and move this script over there with **mv**. 

```{bash}
$ mkdir scripts
```

```{bash}
$ mv fastqc.sh scripts/
```

Actually, let's rename it to something more descriptive. 

```{bash}
$ mv scripts/fastqc.sh scripts/unzip_fastqc.sh
```

We could make a copy to test adding more to this.

```{bash}
$ cp scripts/unzip_fastqc.sh scripts/unzip_fastqc_test.sh
```

Or delete that copy. BUT RM IS FOREVER. 

```{bash}
$ rm scripts/unzip_fastqc_test.sh
```

# Update your README with the steps and info for performing fastqc analysis. 

# Challenge
Write a script that automates fastqc analysis of the paired-end reads and reports which fastqs have warning and failed statistics. 

<details>
  <summary>Here's my script.</summary>
	
```{bash}
mkdir -p 1fastqc # this is a comment. -p makes directory and parents directories if don't exist

fastqc instructor_materials/Amanda_Stahlke/Hands-on_Data/PHMC002B_S366_L007_R*_001.fastq.gz \
-o 1fastqc/ # run fastqc on R1 and R2; backslash keeps command on one line 

## heres our loop to unzip the fastqc output
for filename in 1fastqc/*.zip 
do # new line for for-loops
unzip $filename -d 1fastqc/ # unzip and direct the output to the fastqc directory
done

grep FAIL 1fastqc/PHMC002B_S366_L007_R*/summ* > 1fastqc/failed_stats.txt

cat 1fastqc/failed_stats.txt # print the results

```

</details>



# Other resources
https://astrobiomike.github.io/unix/

http://linuxcommand.org/index.php

https://datacarpentry.org/shell-genomics/

https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424