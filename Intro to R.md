<code>R</code> is a powerful programming and statistical language. It is popular among scientists and, increasingly, journalists. The core program, known as "base R", does a variety of jobs well. But R's true power comes from a staggering number -- somewhere between 10,000 and 25,000 -- of tools or <code>packages</code> that seemingly can do everything short of baking a pizza. The most popular of these is a suite of packages for data manipulation and visualization developed by Hadley Wickham, known as the <code>tidyverse</code>. 

R owes much of its popularity to its price. As open-source software it is, by definition, free. So is the most widely used development environment for R, <code>R Studio</code>. So are the vast majority of R packages. The tradeoff is the learning curve. R is complex. You won't learn it in a few hours or days. Unless you have a superb memory or spend most of your time working with R, you will probably need to keep a manual nearby. It is not Excel. But Excel cannot do a tenth of what R can do.

This introductory class will give you a small idea of R's power. But first here are some rules of the R road:

First, you must install R and R Studio. Both are available for free online and are easily installed. (If you're attending this class at NICAR and taking this class on an IRE laptop, no worries -- the crack IRE staff has already installed both programs.)

Second, you must install whatever packages you desire. Most are available at [CRAN](cran.r-project.org0), the Comprehensive R Archive Network. Installation is a two-step process. If you were to install the tidyverse, for example, you would enter the following commands at the console; the remarks to the right are comments, separated by hash signs which the computer ignores.

    > install.packages("tidyverse")     # command to install package

This would install the package in your computer's hard drive. But in order to use it in a session, you would need to activate it:

    > library(tidyverse)          # notice that the package name is not in quote marks

And now a couple of short cuts that will save your wrists:

In R we name things. We use an assignment operator to give them names. The assignment operator looks like this: <code><-</code>
That's right, a left-pointing arrow followed by a minus sign. But there's a shortcut! On a Mac it's Option plus the minus sign (Option-). On the PC it's Alt plus the minus sign (Alt-). 

Another shortcut that will come in handy is the pipe separator: <code>%>%</code>  For that use Command-Shift-M on the Mac or Control-Shift-M on the PC.

Now -- on with the lesson! The IRE staff has already installed a few packages and some data on our computers, but we'll need to load them into R to work with them. 

    > library(tidyverse)
    > library(readxl)

The readxl package is part of the tidyverse suite but must be separately loaded. Next we'll import American Community Survey data from 2016 listing the number of native and foreign-born persons in each ZIP code in Cook County, Illinois. In R, tables like this are known as "data frames". We'll name this one "Immigrants" using the assignment operator (<-) that we introduced earlier. Remember the shortcut for assignment operators: Option- on a Mac, Alt- on a PC. 

The ever-helpful staff at IRE put all the data files somewhere on your laptops. I promise to find out where before class. The command below is a placeholder.

    > Immigrants <- read_excel("ACS_16_5YR_B05012.xlsx")
    > View(Immigrants)

The command View() with a capital-V displays a portion of the data frame in the upper left pane. 

The View is nice, but we're missing something crucial -- the percentage of immigrants in each tract. Getting that percentage is a breeze in R: so easy that we'll do it first and then explain how we did it.

    > Immigrants <- Immigrants %>% 
      +    mutate(ForeignPer = ForeignBorn / Total)

First "Immigrants <- Immigrants" means that we changed the Immigrants data frame and assigned the change back to itself. We used the pipe operator (%>%) as a way of saying "and then" to go to the next line. And then, we said, mutate, or change. Here's what we changed: We created a new column, ForeignPer, and we set it equal to the value of ForeignBorn divided by Total. So reading backwards, we divided ForeignBorn by Total, assigned that to the new variable ForeignPer, and changed the existing data frame Immigrants to include this new variable. See? I told you it was easier to do than to explain!

Let's explore the data frame. One of the most useful R functions is str(), which stands for structure. Run str(Immigrants) and see what you get.

Now let's take a closer look at the variable we just created, ForeignPer. We'll begin by using the function summary().

        > summary(Immigrants$ForeignPer)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
0.00000 0.06324 0.16310 0.19260 0.30390 0.69640       4 

The summary() function provides the minimum, maximum, mean, median, first and third quartiles plus the number of NA (not available) values.


