---
layout: post
title: Power, flexibility and transparency: OpenFOAM
date: 2020-03-28 00:00:00 +0100
categories: [Engineering, Computational Modelling, CFD]
tags: [Engineering, Computational Modelling, CFD]
---

I love OpenFoam! To those of you who don't know, it is an open source CFD solver that is used widely in academic circles. To me, it sits head and shoulders above every other commercial  CFD software out there. In this article, I talk a little bit about what it is, and why I love it so much?

[](https://upload.wikimedia.org/wikipedia/commons/b/b9/False_color_image_of_the_far_field_of_a_submerged_turbulent_jet.jpg)

# What is OpenFoam?

Most engineers, physicists and mathematicians come across the topic of fluid dynamics during their professional careers. It is a field of study that explores the physics behind the movement of fluids. And despite the simpliticy and conciseness of its governing differential equations (e.g. [Navier-Stokes equations](https://en.wikipedia.org/wiki/Navier%E2%80%93Stokes_existence_and_smoothness)), they are analytically incredibly difficult to solve. That is unless we make some very basic assumptions. In fact, proving the existence and unqiueness of solutions to the Navier-Stokes equations alone is one of the [Millenium problems](https://www.claymath.org/millennium-problems); a set of open problems in Mathematics with a prize money of $1 million. 

So how can you make use of these equations? Well a lot of the time, we solve these differentiatl equations compuationally, often using an iterative approach to guess a solution and then gradually improving on that guess. We call this field Computational Fluid Dynamics (or CFD for short). Now lucky for us, there exist a plethora of different software purveyors that do all the hard work for us; all we need to do is define our problem, press a button, and we can solve jsut about anything that we want. Off the top of my head (and in no particular order), these are the main contendors:

* Ansys CFX
* Ansys Fluent
* COMSOL
* OpenFoam

So what seperates OpenFoam from the rest, and why is is so damn good?

# It's Open Source

Need I say more, things are just so much better when you have the chance to look under the hood and see how something works. Another benefit is that you avoid costly license fees that can deprive you several thousand quid. To make matters worse, they sometimes charge more for more cores (typically over 4)  and if it will be used globally or regionally. None of that silly stuff with OpenFoam, your run wherever, whenever with however many cores you want (I typically did simulations with over 100 cores). 


# It's beautifully written

Now I am not a software engineer, but even I know that the code behind OpenFoam is beautifully written. What do I mean by this? It's concise, structured, easy to edit and lets you access the most important parts immediately (i.e. the physical equations). For instance, if you take a look at SimpleFoam.C, you can see that the whole algorithm is just an iteration over the momentum and pressure equations:	 

`while (simple.loop())
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
    }`

If I wanted to solve for a scalar transport equation to this, I can simply do it as follows: 

`fvScalarMatrix TEqn
(
   fvm::ddt(T)             
 + fvm::div(phi, T)        
 - fvm::laplacian(DT, T) 
);
 
TEqn.solve();`


Because the whole thing is written in C++, it uses object oriented programming to automatically allow for easy discretisation. All this requires of you therefore, is simply to call the right object. 

# Its super extensive
Not only is the code open source and beautiful, it is actually super extensive. There are modules to deal with all types of physics and phenomena, ftom Black-Scholes equation to simulating the flow of magnetic fluids. If you want to write your own code, then that's fine but take a quick look [here](https://www.openfoam.com/documentation/user-guide/standard-libraries.php) at the existing functionality to make your life easier. 


# It has a python interface
Don't like dealing in C++. No worries, there is a python interface that you can use to set up your scenarios and manage your machines ([PyFoam](https://github.com/takaakiaoki/PyFoam)). I myself have not used it extensively, but it seems to be well documented and regularly maintained. This interface comes in handy if you need to couple your CFD simulation with other systems (perhaps a machine learning algorithm). A python interface is a good environment to set up some crosstalk.

# But how do I postprocess? 

Postprocessing results is a way that you retrieve the results from your CFD simulation to analyse the variable of importance. A lot of the commercial CFd tools a post processing tool embedded in the software ( or alternatively use their own postprocessing tool such as CFDPost). For OpenFoam, we use an open source tool called Paraview which can analyse results and run animations of transient simulations. It's not perfect but it is suitable for most of your needs. It also has a python interface to help you automate tedious postprocessing tasks which can come on handy ( although personally I never got it to work during my PHD years.)

# Stop talking, where do I get started?

Currently, a lot of companies choose to use tools such as Ansys Fluent or CFx for their CFD needs (potentially due to the ease of using the user interface). I would strongly argue for the inclusion of OpenFoam due to its sheer power and abilities. Best place to start is simply to clone the latest repo (https://github.com/OpenFOAM) and start running the tutorials. Start with the incompressible flow solvers to get started and then find the solver best suited for your use case and try it out. Good luck!	