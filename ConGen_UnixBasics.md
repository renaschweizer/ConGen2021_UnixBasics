# ConGen 2023: Brief introduction to command lines in Unix
## Monday, August 21

This session is meant to provide a broad overview of the command line interface, with a focus on performing basic commands in Unix. The lesson is slightly modified from last year's version taught by Dr. Amanda Stahlke (https://gist.github.com/Astahlke), and is also based on publically available workshops from www.datacarpentry.org. If you already know how to do all of the actions listed below under "Goals", then feel free to skip this session and tune in for next week's more advanced workshop on redirection, automation, one-liners, scripts, and project organization. If this is all new to you, don't worry! You'll have plenty of exposure to bioinformatics during ConGen, and the more practice you get, the more comfortable it will all become. 

## Goals
By the end of this session, you should be familiar with the following. 
- Command lines, how to implement them, and why they're useful
- Navigating different directories on your system (`pwd`, `cd`, `ls`)
- Assessing and changing file permissions (`ls -l`, `chmod`)
- Moving/downloading data (`wget`, `curl`, `scp`)
- assessing available disk space and usage (`df`, `du`, `top`, `htop`)
- viewing, copying, moving files (`head`, `tail`, `more`, `less`, `cat`, `rm`, `mv`, `mkdir`)

To achieve the above goals, we will download some raw sequence data, make backups, and analyze the quality of the data. 

## Some basics

1. What is the **command line**? Essentially, a "command" is a string of text that, through the use of a command-line interpreter, tells the computer what to do. The command line connects the user to a computer operating system, which can be on your own physical computer, or which can be on a remote server many miles away. 
2. The **Unix shell** is one such command-line interpreter and is usually run within a terminal window.  
3. Why use command-line programs? They are extremely versatile and simple, and can be used to manipulate files or analyze data that you would otherwise not be able to do using software such as Excel. As we'll learn about in the bioinformatics session next week, commands can be pieced together in a very powerful way, and then can be automated across hundreds or thousands of samples. Altogether, this makes your life easier and your science more reproducible! Finally, many sequencing projects require powerful computational clusters that are only accessible via a command-line interpreter. 

## Log on to the remote R-Studio server 
Sign in to https://handson.congen1.com/. Url, username, and password have been provided by ConGen Organizers. 

Check out the source panel for scripting, the conosole/terminal, environment and history pane, and the files, viewer, packages, and help pane. You can customize this if you like.  

<img width="1673" alt="Screen Shot 2021-08-29 at 11 44 30 AM" src="https://user-images.githubusercontent.com/10552484/131260953-acb91b54-1d3b-4041-b534-9925a8ee9166.png">


## Natigation: Here we go! 
Let's orient ourselves to this environment.  Click the terminal tab in the console/terminal/jobs pane, in the lower left for default configuration. 

The command prompt begins with your username @ ip-adress and a colon (:), then a tilde (~) for your home ***directory*** and a $ indicating the beginning of a shell prompt. A **directory** is like a folder in your files finder. If you think about your Home folder on your own computer, you likely have several folders, many that contain subfolders or files. You could access them within Finder (on a Mac) by double clicking various folders to open them, or, you can access those same folders and files from the Terminal window using the **command line**.

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
Angel_Rivera-Colon  Brenna_Forester  Eric_Anderson  Jisca_Huisman  Mike_Miller    Rena_Schweizer  Steve_Mussmann
Arun_Sethuraman     Chris_Funk       Gregg_Thomas   Marty_Kardos   Norah_Saarman  Robin_Waples
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
ls instructor_materials/ -lth
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

You can also chain them together with a wildcard: 
```{bash}
ls ../R*
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

Yay! Your first sequencing run is done and you've received an email from the sequencing facility that your data are ready. Now what?? Depending on the facility, you may use `ftp`, `wget`, or `curl` to download the data. Today, we'll use `git`. For those of you unfamiliar, git is a software for version control. We won't use most of its capabilities today, so you can learn more [here](https://github.com/goodest-goodlab/good-protocols/tree/main/how-tos) and elsewhere.

```{bash}
git clone https://github.com/renaschweizer/congen-unixbasics.git
```

This command will download a copy of the github repository (or project) containing this tutorial. Let's take a look. 

```{bash}
ls congen-unixbasics
```
We see two markdown files and a data folder. Let's change to the data folder and take a look at our files. 

```{bash}
cd congen-unixbasics/data
ls
```

We see a set of forward (R1) and reverse (R2) reads from a deer mouse exome. You should always try to look at the data, even if you process the bulk of it with a program. Take a look at one of these files. 

```{bash}
cat S144_L006_R1_sub.fastq.gz
```

AH! Too much data and it looks garbled, too. Hit **Ctl+c (^c)** to quit a running process or abort a task. I use this more often than I care to admit. You can see that the file is of type "fastq.gz" where the ".gz" indicates the file has been compressed. File compression can save huge amounts of space! For today, though, let's uncompress the files. We can also use the `clear` command to clear the current screen.  

```{bash}
clear
gunzip -c S144_L006_R1_sub.fastq.gz > S144_L006_R1_sub.fastq
gunzip -c S144_L006_R2_sub.fastq.gz > S144_L006_R2_sub.fastq
```

The '>' redirects the text that would otherwise be printed to the Terminal window (called standard output) into a new text file. We'll learn more about how multiple commands can be pieced together during next week's session. 

Let's use our familiar `ls` command with some additional options, to see the difference in file size between compressed and uncompressed files. Again, the 'l' stands for 'long' format, which means more detailed information is provided for each file. The 'h' means 'human-readable' file sizes, and 't' sorts by date modified. Don't forget you can always use `man ls` to see all the detailed options. 

```{bash}
ls -lht
```
This is what I see on my Terminal window (don't worry if you don't have all the same directories that I have): 

<img width="561" alt="Screen Shot 2021-08-29 at 2 59 50 PM" src="https://user-images.githubusercontent.com/10552484/131265204-5d18cd33-4b6f-4a1f-a196-392ed3866c24.png">

This long version of the directory gives us quite a bit of information! 
- Column 1 provides information if the content is a directory ('d'), file ('-'), or a link ('l'). The next 9 characters provide information on the file permission, with 3 characters for the Owner, the next 3 for the Group owner, and the last 3 for everyone else. Each set of 3 characters provides information on whether members of that group can read it ('r'), write to it ('w'), or execute it ('x'). 
- Column 2 tells us about how many links are to this file.
- Column 3 tells us about who is the owner of the file/directory.
- Column 4 tells us about who is the group owner of the file/directory.
- Column 5 tells us about the size of the file/directory in bytes unit (with the `-h` flag makes it in Byte, Kilobyte, Megabyte, Gigabyte, Terabyte and Petabyte)
- Column 6 provides the abbreviated month, day-of-month file was last modified, hour file last modified, minute file last modified. 
- Column 7 is the file or directory path name. 
   
**Exercise 1: What is the relative size difference between our compressed and uncompressed sequence files?**
<details>	
	By using the `ls-lht` command above, we can see that our compressed files are approximately 1/4 the size of our uncompressed files. Think of how much space that saves over the duration of a sequencing experiment! 
	</details>

## Modifying permissions and backing up your raw data 

One of the first things I do when I get new data is I make a backup of it that is write protected. Let's do that now using the `mv` command. Depending on how it is used, `mv` can either rename a file or move a file to somewhere else.  

```{bash}
mkdir data_backup
mv S144_L006_R*_sub.fastq.gz data_backup
cd data_backup
ls -l
```
These commands make a copy of the data in a new directory called "data_backup", then list the permissions for the files. What are the current permissions for the owner of the file? 

We can then modify the permissions of files using the command `chmod` and flags to add or remove read, write, or execute ability. Our goal for now is to change permissions on this file so that you (the owner) no longer have write permissions. We can do this using the `chmod` (change mode) command and subtracting (-) the write permission -w.

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

Finally, we can use the command `mv` in a different context to rename the files to indicate they are a backup. 

```{bash}
mv S144_L006_R1_sub.fastq.gz S144_L006_R1_sub_backup.fastq.gz
mv S144_L006_R2_sub.fastq.gz S144_L006_R2_sub_backup.fastq.gz
```

Moving forward, as you create files and directories, remember: 
* File names that start with a period are hidden. You can view them with **ls -a**
* Bash is case-sensitive. file1.txt and File1.txt are different. Be consistent. 
* Do not (!!) embed spaces in file names. Use file1 or File_1 or file-1 or SnakeCase. I prefer underscores because R interprets - as subtraction, and I prefer starting filenames with lower case so I don't have to type in the upper case letter each time.  

## What sort of resources do I have available on a computer?

Often, we run analyses on a variety of computers, including our own laptop, a shared lab server, or an institution-wide cluster. Before I start a new project or set of analyses, I like to see how resources are available for me to download data, generate files, run multiple jobs, etc. 

To assess how much free disk space is available, you can use the `df` (display free disk space) command. 

```{bash}
df -h
```
Since most or all of us are using the AWS setup for ConGen, a lot of these directories may be unfamiliar. On my lab server, it looks more like this: 

<img width="555" alt="Screen Shot 2021-08-24 at 10 20 49 AM" src="https://user-images.githubusercontent.com/10552484/130654957-ecd42c05-e6ef-4b68-9b89-9c6e152bb29a.png">

To check how much space a single directory takes up (say, your /user directory which you may have been told to keep under a certain size), you can use `du` (display disk usage statistics), 

```{bash}
du -ha 
```
This will display the file size in human readable format (-h) for all files (-a) within the current directory, with a total at the bottom. If you only want to display the usage for a particular directory and not all of its subdirectories, you can do so, too. 

```{bash}
du -hd1 [directory name]
```
This will provide the file sizes of subdirectories to a depth only one below the current one (-d1). 

**Exercise 2: How much space do our raw data files take up?**
<details>
	You can use the du command for the data_backup directory
	```{bash}
	du -hd1 ../data_backup/
	```
	This specifies that our data_backup directory uses 114 MB of space.
	</details>

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
- **Ctl+u (^u)** deletes everything to the left of your cursor
- **Ctl+k (^k)** deletes everything to the right of your cursor

It's useful to know about the fastqc file encoding: https://en.wikipedia.org/wiki/FASTQ_format. Fastqc uses these data to generate a really useful report. 

Let's check how many reads we have in each file using `wc` (word count). By default, using `wc` on a file gives three columns with the number of lines, the number of words, and the number of characters. We can ask for only the number of lines using the `-l` flag. 

```{bash}
wc -l raw_fastq/S144_L006_R*_sub.fastq
```
**Exercise 3: How many reads are there?**
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

**Exercise 4: What did fastqc create?**

ls -l shows two new files for each fastq file

```{bash}
ls -l quality_metrics/
```

```{bash}
total 1796
-rw-r--r-- 1 user10 user10 592881 Aug 23 10:51 S144_L006_R1_sub_fastqc.html
-rw-r--r-- 1 user10 user10 311361 Aug 23 10:51 S144_L006_R1_sub_fastqc.zip
-rw-r--r-- 1 user10 user10 603773 Aug 23 10:51 S144_L006_R2_sub_fastqc.html
-rw-r--r-- 1 user10 user10 323468 Aug 23 10:51 S144_L006_R2_sub_fastqc.zip
```
	
- The ".html" is the FastQC report, in HTML format.
- The "zip" is a zipped (compressed) directory of FastQC output files.

Let's look at the output. We can't view html reports on the remote server, so you could copy the file back to your own laptop. Within RStudio, however, we are able to open the html files we have generated by selecting the "Files" tab in the bottom right corner of RStudio, then navigating to our 'quality_metrics' directory. Open the html report in your web browser. How do you think the sequencing run went? 

Hopefully today's lesson has helped you feel more comfortable working from the command line in UNIX. The more you practice, the easier and more fluid it will be!

# Other resources

https://wikis.utexas.edu/display/CoreNGSTools/Linux+fundamentals

https://astrobiomike.github.io/unix/

http://linuxcommand.org/index.php

https://datacarpentry.org/shell-genomics/

https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424
