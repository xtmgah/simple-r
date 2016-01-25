
---

# NAME #

r - a simple-r command line R environment



---

# SYNOPSIS #

r _[options](options.md)_ _filename_

r _[options](options.md)_ _[command](command.md)_ _filename_



---

# DESCRIPTION #


## Overview ##

The program is a simple wrapper to use R functions on a single text file from the command line; useful for quick data plotting or statistical analysis. The program generates and runs R script that: (a) loads up data from the file as a single data frame assuming default parameters (b) runs R functions, described by preset options or passed directly to the script.


## Normal Usage ##

There are two ways to use it (which can be combined): (1) execute one of the preset options (2) run directly a R command

r _[options](options.md)_ _filename_

See OPTIONS for details on the command line switches supported.

r _[command](command.md)_ _filename_

It can be any command supported by R, which takes a single data frame as an argument. The script will just add the name of the data frame to it. For example, for the 'summary' command the script will execute '`summary(d)`'.

The _filename_ can be any file present on the file system or _-_, to denote STDIN. This way, `r` can be used as part of the Unix pipeline. See EXAMPLES for more details.



---

# OPTIONS #

This script currently supports the following command line switches:

### **-k** _columns_ ###
> Columns that should be taken into account (using R convention).
> Execute R line: `d <- d[,columns]`.
### **-a** ###
> Attach the dataset to the environment.
> Execute R line: `attach(d)`.
### **-r** ###
> Force the first column to have the row names. Execute R line in `read.table`: `row.names=1`.
### **-c** ###
> Force the first row to have the column names. Execute R line in `read.table`: `header=T`.
### **-e** _command_ ###
> Execute the specified R command after loading the dataset.
### **-s** _separator_ ###
> Input record separator (default: white space)
### **-p** ###
> Plot the dataset and save it as a pdf file.
> Execute R line: `pdf('plot.pdf'); plot(d); dev.off()`.
### **-o** _file_ ###
> Output file name for the plotted PDF file (default: plot.pdf).
### **-d** ###
> Display plotted dataset (using acroread or evince).
### **-v** ###
> Enable verbose output (goes to STDERR (i.e. 2nd file descriptor).
### **-h** ###
> Print this helpful help message



---

# EXAMPLES #


## Dataset ##

### All the examples use the famous iris dataset generated from R using: ###
`write.table(iris, "iris.txt")`

```
tomek@localhost:~$ head -n3 iris.txt 
  "Sepal.Length" "Sepal.Width" "Petal.Length" "Petal.Width" "Species" 
  "1" 5.1 3.5 1.4 0.2 "setosa" 
  "2" 4.9 3 1.4 0.2 "setosa" 
  "3" 4.7 3.2 1.3 0.2 "setosa" 
```


## Examples ##

### Simple summary of the dataset: ###
> `r summary iris.txt`

It was produced by `r` by generating and executing the following R code:
```
d <- read.table('iris.txt'); 
summary(d)
```

### The same as above, but using three different types of Linux redirection commands (pipe, standard input, named pipe): ###
> (1) `cat iris.txt | r summary -`

> (2) `r summary - < iris.txt`

> (3) `r summary <(cat iris.txt)`
> 
### Summary of the first two columns of the dataset, while showing the executed R script: ###
> `r -v -k 1:2 summary iris.txt`

### Compute correlation coefficient between 1,3 and 4th column of the dataset: ###
> `r -k 'c(1,3,4)' cor iris.txt`

### Plot and display the entire data as well as only first two columns: ###
> `r -dp iris.txt`
> `r -dp -k 1:2 iris.txt`

### Find the line of best fit between Sepal.Length and Petal.Length: ###
> `r -ae 'lm(Sepal.Length ~ Petal.Length)' iris.txt
> `r -e 'attach(d); lm(Sepal.Length ~ Petal.Length)' iris.txt`

### Execute a t-test between the two datasets: ###
> `r -ae 't.test(Sepal.Length, Petal.Length)' iris.txt`

### Extract the first two columns of the dataset and save it as a new file: ###
> `r -k 1:2 write.table iris.txt > new.txt`

### Develop a script to show summary of the dataset, pass it to `R` and execute it, while discarding the `r` output. Tested under Bash. ###
> `r -v summary iris.txt 3>&1 1>&2 2>&3 2>/dev/null | R`



---

# SEE ALSO #

gnuplot(1) perl(1) R(1)


---

# AUTHOR #

T. Religa



---

# VERSION #

```
  0.1
```