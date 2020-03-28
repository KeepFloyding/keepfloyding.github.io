---
layout: post
title: The power, flexibility and transparency of OpenFOAM
date: 2020-03-28 00:00:00 +0100
categories: [Engineering, Computational Modelling, CFD]
tags: [Engineering, Computational Modelling, CFD]
seo:
  date_modified: 2020-03-28 16:19:08 +0000
---

I love OpenFoam! To those of you who don't know, it is an open source CFD solver that is used widely in academic circles. To me, it sits head and shoulders above every other commercial  CFD software out there. In this article, I talk a little bit about what it is, and why I love it so much.

![](https://www.esrtechnology.com/images/Oil_and_gas/openfoam1.PNG)

# What is OpenFoam?

Most engineers, physicists and mathematicians come across the topic of fluid dynamics during their professional careers. It is a field of study that explores the physics behind the movement of fluids. And despite the simplicity and conciseness of its governing differential equations (e.g. [Navier-Stokes equations](https://en.wikipedia.org/wiki/Navier%E2%80%93Stokes_existence_and_smoothness)), they are analytically incredibly difficult to solve. That is unless we make some very basic assumptions. In fact, proving the existence and uniqueness of solutions to the Navier-Stokes equations alone is one of the [Millennium problems](https://www.claymath.org/millennium-problems); a set of open problems in Mathematics with a prize money of $1 million. 

So how can you make use of these equations? Well a lot of the time, we solve these differential equations computationally, often using an iterative approach to guess a solution and then gradually improving on that guess. We call this field Computational Fluid Dynamics (or CFD for short). Now lucky for us, there exist a plethora of different software purveyors that do all the hard work; all we need to do is define our problem, press a button, and we can solve just about anything that we want. The image below outlines the jungle of available CFD solvers that are out there: 
![](https://static1.squarespace.com/static/53eacd17e4b0588f78eb723c/57d014bcf5e231cca98a9f0a/5b9923e10e2e72e7bb560929/1580746771470/Consultants+and+CFD+software.png?format=1500w)

So what separates OpenFoam from the rest, and why do I think that it's so damn good?

# It's open source

Need I say more, things are just so much better when you have the chance to look under the hood and see how something works. Another benefit is that you avoid costly license fees that can seriously hurt your pockets. Back in my research days, a lot of us were using Ansys CFX, and would typically fight over the licenses that were available. To make matters worse, some CFD software providers charge you more if you want to run your simulation over more than 4 cores. None of that silly stuff with OpenFoam, you can run whenever you want with however many cores you want (I typically did simulations with over 100 cores). 

![](https://external-preview.redd.it/iWgQYjbOPoYdyGzHM9FdoCvBayKSWktdqweZokDtcKs.png?auto=webp&s=08ae5592957df52152182390d84dbd03ef49dea7)

# It's beautifully written

I am not a software engineer, but even I can tell that the code behind OpenFoam is beautifully written. What do I mean by this? It's concise, structured, easy to edit and lets you access the most important parts immediately (i.e. the physical equations). For instance, each solver is typically just made up of a handful of different scripts (each of which is typically less than 200 lines long). For instance, if you take a quick look at `SimpleFoam.C`, you can see what the main underlying algorithm is (after all it's just an iteration over the momentum and pressure equations):	 

{% highlight C %}
while (simple.loop())
    {
        Info<< "Time = " << runTime.timeName() << nl << endl;

        // --- Pressure-velocity SIMPLE corrector
        {
            #include "UEqn.H"
            #include "pEqn.H"
        }

        laminarTransport.correct();
        turbulence->correct();

        runTime.write();

        runTime.printExecutionTime(Info);
    }
{% endhighlight %}

I can't overstate the benefits in having structured code that is easy to read. First of all, it helps the user to understand the main underlying process behind a given solver without getting bogged down in the details. This avoids what I call black-box syndrome where you inherently trust a piece of software you don't really understand. Secondly, it allows you to then edit the code to suit the use-case that YOU want to simulate. For instance, if we wanted to solve an additional scalar transport equation, we can simply add the following to the above script: 

{% highlight C %}
fvScalarMatrix TEqn
(
   fvm::ddt(T)             
 + fvm::div(phi, T)        
 - fvm::laplacian(DT, T) 
);
 
TEqn.solve();
{% endhighlight %}

Because the whole thing is written in C++, it uses object oriented programming to ensure that you can make any modifications without having to change several lines of code in different files. For instance, by calling these objects, things like the discretisation of your equation, and the way that it is being solved, is something that will automatically be taken care of for you. All you need to do, is simply call the right object. 

# It's super extensive
Not only is the code open source and beautiful, it is actually super extensive. There are modules to deal with all types of physics and phenomena, from Black-Scholes equation to simulating the flow of magnetic fluids. If you want to write your own code, then that's fine but take a quick look [here](https://www.openfoam.com/documentation/user-guide/standard-libraries.php) at the existing functionality to make your life easier. 


# It has a python interface
Don't like dealing in C++. No worries, there is a python interface that you can use to set up your scenarios and manage your machines ([PyFoam](https://github.com/takaakiaoki/PyFoam)). I myself have not used it extensively, but it seems to be well documented and regularly maintained. This interface comes in handy if you need to couple your CFD simulation with other systems (perhaps a machine learning algorithm). A python interface is a good environment to set up some crosstalk.

# And its disadvantages?

There are 2 main touted disadvantages leveled against OpenFoam. One is an argument that is typically leveled against FOSS software: it is not rigorous enough. How can we trust a software that is managed by a group of enthusiasts that develop it in their spare time against those who are paid to explicitly develop it? Well first, I would argue that a piece of software that is developed by hundreds of contributors that are passionate about the subject (each providing their knowledge) is as reliable (if not more) than anything developed by a commercial entity. Secondly, the implementation of CFD is not exactly a secret; there are plenty of books out there that detail on which algorithms should be used to solve for fluid flow. Thirdly, OpenFoam is quite widely used, especially in academia and therefore has many use-cases to back up its [authenticity](https://www.researchgate.net/publication/261876529_Evaluation_of_OpenFOAM_in_Academic_Research_and_Industrial_Applications).

Another reason people might wish to avoid OpenFoam is that it is comparatively more complicated to use than commercial variants. Currently, a lot of companies choose to use tools such as Ansys Fluent or CFX for their CFD needs due to the ease in using the user interface to setup simulations. OpenFoam has no such GUI, and has to be accessed via the command line. Despite this, I would argue that OpenFoam is not that much harder to use. Sure you need some command line knowledge, and yes, it will take you longer to learn how to use OpenFoam than other CFD software, but at the end of the day you will succeed in learning how to use it. And when you reach that point, you will be better off in understanding the fundamentals behind CFD, and you will have the sheer power and flexibility of OpenFoam to back up your simulations. 


# Enough talk, where do I get started?

 Best place to start is simply to clone the latest [repo](https://github.com/OpenFOAM) and start running the tutorials. Start with the incompressible flow solvers and then find the solver best suited for your use case and try it out. Use forums and online material to guide you along the way. Good luck!	