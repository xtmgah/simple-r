![http://simple-r.googlecode.com/svn/wiki/simpleRfull.jpg](http://simple-r.googlecode.com/svn/wiki/simpleRfull.jpg)

The program is a simple wrapper to use [R functions](http://www.r-project.org/) on a single text file from the command line; useful for quick data plotting or statistical analysis. The program generates and runs R script that: (a) loads up data from the file as a single data frame assuming default parameters (b) runs `R` functions, described by preset options or passed directly to the script.

It should be helpful for anybody who is using a lot of Linux command line for statistical analysis.

## Download ##

[Checkout the source using git](https://code.google.com/p/simple-r/source/checkout) or just download the [simple-r file](https://simple-r.googlecode.com/git/r). It is a single Perl script named `r`. Put it in your `bin/` directory, change file permissions to make it executable (`chmod 755 r`) and enjoy simple-r life. You will need [R environment](http://www.r-project.org/) and [Perl](http://www.perl.org/) to be present in your path to achieve it.

If your life does not get any simple-r, [fill out an issue](https://code.google.com/p/simple-r/issues/list) or [e-mail me](mailto:rtomek@outlook.com) so that we can work on simple-r-ing.

## Examples ##
All the examples use the famous [iris dataset](http://en.wikipedia.org/wiki/Iris_flower_data_set) generated from `R` using: `write.table(iris, "iris.txt")` command:
```
user@localhost:~$ head -n3 iris.txt
"Sepal.Length" "Sepal.Width" "Petal.Length" "Petal.Width" "Species"
"1" 5.1 3.5 1.4 0.2 "setosa"
"2" 4.9 3 1.4 0.2 "setosa"
```

**Simple-r summary of the dataset:
> `r summary iris.txt`**

Will generate:
```
  Sepal.Length    Sepal.Width     Petal.Length    Petal.Width   
 Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100  
 1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300  
 Median :5.800   Median :3.000   Median :4.350   Median :1.300  
 Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199  
 3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800  
 Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500  
       Species  
 setosa    :50  
 versicolor:50  
 virginica :50  
```

**The same as above, but using three different types of Linux redirection commands (pipe, standard input, named pipe):
  1. `cat iris.txt | r summary -`
  1. `r summary - < iris.txt`
  1. `r summary <(cat iris.txt)`**

**Compute correlation coefficient between 1,3 and 4th column of the dataset:
> `r -k 'c(1,3,4)' cor iris.txt`**

**Plot and display the entire data as well as only first two columns:
> `r -dp iris.txt`**

> `r -dp -k 1:2 iris.txt`

**Find the line of best fit between Sepal.Length and Petal.Length:
> `r -ae 'lm(Sepal.Length ~ Petal.Length)' iris.txt`**

> `r -e 'attach(d); lm(Sepal.Length ~ Petal.Length)' iris.txt`

**Execute a t-test between the two datasets:
> `r -ae 't.test(Sepal.Length, Petal.Length)' iris.txt`**

