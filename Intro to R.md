<code>R</code> is a powerful programming and statistical language. It is popular among scientists and, increasingly, journalists. The core program, known as "base R", does a variety of jobs well. But R's true power comes from a staggering number -- somewhere between 10,000 and 25,000 -- of tools or <code>packages</code> that seemingly can do everything short of baking a pizza. The most popular of these is a suite of packages for data manipulation and visualization developed by Hadley Wickham, known as the <code>tidyverse</code>. 

R owes much of its popularity to its price. As open-source software R is, by definition, free. So is the most widely used development environment for R, <code>R Studio</code>. So are the vast majority of R packages. The tradeoff is the learning curve. R is complex. You won't learn it in a few hours or days. Unless you have a superb memory or spend most of your time working with R, you will probably need to keep a manual nearby. It is not Excel. But Excel cannot do a tenth of what R can do.

This introductory class will give you a small idea of R's power. But before we get started here are some rules of the R road:

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

The readxl package is part of the tidyverse suite but must be separately loaded. Next we'll import American Community Survey data from 2016 listing the number of native and foreign-born persons in each census tract in Cook County, Illinois. In R, tables like this are known as "data frames". We'll name this one "Immigrants" using the assignment operator (<-) that we introduced earlier. Remember the shortcut for assignment operators: Option- on a Mac, Alt- on a PC. 

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

The summary() function provides the minimum, maximum, mean, median, first and third quartiles plus the number of NA (not available) values. Notice also that we use the $-sign to connect data frames and columns.

You can see that there are big gaps from the first quartile (25th percentile) to the median (50th percentile) to the 3rd quartile (75th percentile). We can explore these with the quantile() function.

        > quantile(Immigrants$ForeignPer, c(0.1, 0.2, 0.3, 0.4,0.5, 0.6, 0.7, 0.8, 0.9), na.rm=TRUE)
       10%        20%        30%        40%        50%        60%        70%        80% 
        0.01815276 0.04795991 0.08415216 0.12211798 0.16310981 0.21134868 0.27006931 0.34238683 
       90% 
        0.41473971 


This takes a little explaining. First let's deal with that c(0.1, 0.2 ...). The c() stands for concatenate or combine; it means apply a formula to everything within the parentheses. In this case we want the quantile of ForeignPer for all these digits. Next, outside the first set of parentheses we've got "na.rm=TRUE"; this is here just in case we have any NA's in the mix. And we know that we've got 4 NA's because they showed up in the summary. NA's play havoc with statistics; they cannot, repeat cannot, be treated the same as a zero. You must explicitly tell R to ignore them. That is exactly what the command "na.rm=TRUE" does.

Finally, let's visualize this with a histogram. R packages such as ggplot2 offer dazzling graphics. But you can get a good idea of what's going on with the simple tools built into base R.

        > hist(Immigrants$ForeignPer)

![](https://github.com/roncampbell/NICAR2018/blob/images/ImmigrantPer.png?raw=true)

It looks like immigrants are concentrated. Some tracts have a lot of them while some have very few. Let's find out if immigrants tend to live together. Another way of putting that is by asking a question: Do most immigrants live in tracts with a high percentage of immigrants?

To get the answer, we'll write a short script in R. In the upper left corner of R Studio click on the little white sheet with a green + sign. The first item in the menu is the one we want: R Script. And here's what we enter.

        library(tidyverse)                  # load packages you might need
        HighImmigrant <- Immigrants %>%     # create new data frame based on existing data
         filter(ForeignPer >= .3039) %>%   # filter to get the 75th percentile
         summarize(
          Tracts = n(),
          USBorn = sum(Native),
          Immigrant = sum(ForeignBorn),
          Everyone = sum(Total),
          ImmigPer = (Immigrant / Everyone)
         )
The results: Of 1,320 Chicago area tracts, 311 have immigrant populations at or above the 75th percentile. Those tracts are home to 598,924 foreign-born residents, 54 percent of the county total. So yes, most immigrants in Cook County live near other immigrants.         

Now let's look at income in Cook County. We'll do that by importing census data on median household income by tract.

        > View(Income)

When we imported the Immigrants data frame earlier, we used the str() function to look at the structure. If you do that for Income, you'll get a surprise -- R imported the MedianHHInc column as a string or character. We need to change that to a number so we can make calculations. 

        > Income$MedianHHInc <- as.numeric(Income$MedianHHInc)
        Warning message:
        NAs introduced by coercion
 
The as.numeric() function changes a string to a number. We assign the column to the Income data frame to make the change permanent. The warning message tells us that there are some NA's (not available values) in the column. With that, let's see how incomes vary among Cook County tracts.

> hist(Income$MedianHHInc)
![]()



