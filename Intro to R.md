<code>R</code> is a powerful programming and statistical language. It is popular among scientists and, increasingly, journalists. The core program, known as "base R", does a variety of jobs well. But R's true power comes from a staggering number -- somewhere between 10,000 and 25,000 -- of tools or <code>packages</code> that seemingly can do everything short of baking a pizza. The most popular of these is a suite of packages for data manipulation and visualization developed by Hadley Wickham, known as the <code>tidyverse</code>. 

R owes much of its popularity to its price. As open-source software it is, by definition, free. So is the most widely used development environment for R, <code>R Studio</code>. So are the vast majority of R packages. The tradeoff is the learning curve. R is complex. You won't learn it in a few hours or days. Unless you have a superb memory or spend most of your time working with R, you will probably need to keep a manual nearby. It is not Excel. But Excel cannot do a tenth of what R can do.

This introductory class will give you a small idea of R's power. But first here are some rules of the R road:

First, you must install R and R Studio. Both are available for free online and are easily installed. (If you're attending this class at NICAR and taking this class on an IRE laptop, no worries -- the crack IRE staff has already have been installed both programs.)

Second, you must install whatever packages you desire. Most are available at [CRAN](cran.r-project.org0), the Comprehensive R Archive Network. Installation is a two-step process. If you were to install the tidyverse, for example, you would enter the following commands at the console; the remarks to the right are comments, separated by hash signs which the computer ignores.

install.packages("tidyverse")   # command to install package

This would install the package in your computer's hard drive. But in order to use it in a session, you would need to activate it:

library(tidyverse)    # notice that the package name is not in quote marks

And now a couple of short cuts that will save your wrists:

In R we name things. We use an assignment operator to give them names. The assignment operator looks like this: <code><-</code>
That's right, a left-pointing arrow followed by a minus sign. But there's a shortcut! On a Mac it's Option plus the minus sign (Option-). On the PC it's Alt plus the minus sign (Alt-). 

Another shortcut that will come in handy is the pipe separator: <code>%/%</code>  For that use Command-Shift-M on the Mac or Control-Shift-M on the PC.

