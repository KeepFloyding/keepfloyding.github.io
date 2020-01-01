---
layout: post
title:  "Numpy + Cython = Speed"
date:   2019-06-19 07:51:21 +0100
categories: [Optimisation, Cython]
tags : ['Cython', 'Python', 'Optimisation']
---

# TL;DR

* Using Cython and Numpy can make your code much faster
* For our test piece (not too dissimilar from a case-study at my workplace) we got several magnitudes an increase in the execution speed (summary of the times is given below). 
* Times:
	* Original Code: 12.75 s 
	* Cythonised: 9.23 s
	* Numpy: 0.63 s
	* Numpy and cythonised: 0.63 s

# Full Article

At work, I was recently given a python code that was developed by a third party and asked to make it faster. The code in question solved an optimisation problem that was essentially selecting the best parameters that would minimise the root mean square error of simulated data from an engineering model with actual measured data from a physical system. 

The problem in this case, was that for our chosen system, it would take the code several days to run on a standard machine. So before we tried to brute force it by using more compuational power, I wanted to see what we could do to speed it up by tweaking the code. Our chosen strategy? Remove all for loops and replace with numpy array operations wherever possible and convert it to Cython. 

For those who haven't heard of it before, Cython is essentially a manner of getting your python code to run with C-like performance with a minimum of tweaking. It goes hand-in-hand with numpy where the combination of array operations and C compiling can speed your code up by several orders of magnitude. I'll take you through step-by-step of how we went through the problem and got our code 10x faster. 

After a brief analysis of the code, it seemed that the bottleneck was due to the lenghty process that it took to generate the predictions from the engineering model. For illustration purposes, I'll use some samply code to show what we did:

{% highlight python %}
import numpy as np

# A complicated calculation that we need to perform several times
def complex_calculation(x):
	y = x[0]**3/x[1]**5 + (x[2]*x[3])**0.5

# Original unoptimised code
def original_code(data):
	for item in data:
		y = complex_calculation(item)


# Setting seed for random variables
np.random.seed(1)

# Creating array of random variables
data = np.random.rand(10000000,4)

# Running the code
original_code(data)

{% endhighlight %}

Given that the size of the input data array was very big, it took a long to execute the code. Running the code above 10 times gives an average time of 12.75 s +- 0.02 s. The quickest way I tried to make the code faster was just to convert it directly to Cython. All this involved simply putting the original_code function and the complex_calculation function in a seperate file with a .pyx extension and have a setup.py file as the one given below. 

{% highlight python %}
from distutils.core import setup
from Cython.Build import cythonize
import numpy

setup(
    ext_modules = cythonize("[FILE_NAME].pyx"),
    include_dirs=[numpy.get_include()]
)
{% endhighlight %}

The Cython code can then be compiled, and called as a package.

{% highlight sh %}
python setup.py build_ext --inplace
{% endhighlight %}

Running the cython code speeds things up to around 9 s. Just from looking at the code above, you can see some serious bottlenecks. For scientific computing, the use of for loops is generally discouraged due slow performance. As such, let's see how the time compares when we convert the for loops to numpy array operations. For this case all we had to do was change the functions to accept numpy arrays:

{% highlight python %}
# A complicated calculation that we need to perform several times
def numpied_complex_calculation(x):
	y = x[:,0]**3/x[:,1]**5 + (x[:,2]*x[:,3])**0.5
	#return(y)

# Numpied code
def numpied_code(data):
	y = numpied_complex_calculation(data)

{% endhighlight %}

This alone causes a speedup of over a magnitude to 0.6 s. Next is naturally combining this with Cython to get an even faster performance. In this example, there are significant improvements in the computation time. However, I refer you to an excellent online guide for ways on how Cython can speed up your numpy operations by specifying the variable type <https://cython.readthedocs.io/en/latest/src/tutorial/numpy.html>:

{% highlight python %}
import numpy as np
cimport numpy as np

DTYPE = np.int

ctypedef np.int_t DTYPE_t

cimport cython
@cython.boundscheck(False) # turn off bounds-checking for entire function
@cython.wraparound(False)  # turn off negative index wrapping for entire function

def complex_calculation(np.ndarray[double, ndim=1] x):
	x[:,0]**3/x[:,1]**5 + (x[:,2]*x[:,3])**0.5

# Cythonized code
def cythonized_code(np.ndarray[double, ndim=2] data):
	complex_calculation(data)

{% endhighlight %}

Hopefully now, you can see that being smarter with your code, and Cythonising can bring real improvements in execution time. 







