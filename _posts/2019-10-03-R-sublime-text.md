---
layout: post
title: Running R in Sublime Text 3
date: 2019-10-03 05:20:13 +0100
categories: [Programming, R, IDE, Sublime Text]
tags: [Programming, R, IDE, Sublime Text]
seo:
  date_modified: 2020-01-06 10:20:35 +0200
---

# TL;DR

* You can run R inside the Sublime Text 3 IDE
* All you need in addition to an installation of R and Sublime Text 3 are the SendCode and Terminus packages (both installed via Command Palette tool).
* To get it to work, just do the following:
	* Open up Sublime
	* Run SendCode: Choose Program from Command Palette
	* Select Terminus as the program
	* Run Terminus: Open Default Shell ...
	* Inside Terminus command window, just run R (by accessing the R.exe executable).
	* Create R script
	* Run lines of code with CTRL+Enter or build entire code with CTRL+B

# Main Article

I use Sublime Text as my IDE of choice for all things not Python. It has a clean interface, it is easy to configure and is reasonably ligthweight. So when I had to develop some code in R for my work, I wanted to use this IDE to run blocks of code, build entire scripts and easily commit them to a shared repo with Git. Searching online, I found that using a combination of 2 packages (SendCode and Terminus) was the best way forward to developing R in Sublime Text 3. 

SendCode, as its name suggests, simply sends code to a terminal, and Terminus is an intergrated, cross-platform terminal that you can have in a view tab. The end result is an R script and console side-by-side where you can run the entire or highlighted bits of your R code in the terminal (as below). 

![Alt Text](https://keepfloyding.github.io/images/R_subtext.gif)


Configuring this setup is super easy, and only requires you to install SendCode and Terminus via the Command Palette in Sublime Text. Before you start, make sure you have your R installation done and that you know how to access the executable. Typically, I add the location to the R executable to environment variables (in Windows) or to the PATH variable in Linux so that all I have to do is type in 'R' in the terminal.  

To get it running, just do the following steps:
* Open up Sublime
* Run SendCode: Choose Program from Command Palette
* Select Terminus as the program
* Run Terminus: Open Default Shell ...
* Inside Terminus command window, just run R (by accessing the R.exe executable).
* Create R script
* Run lines of code with CTRL+Enter or build entire code with CTRL+B

Enjoy!


