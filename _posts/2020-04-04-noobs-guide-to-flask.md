---
layout: post
title: A 5 minute guide to web development in Flask
date: 2020-04-04 00:00:00 +0100
categories: [Programming, Python, Web Development, Flask]
tags: [Programming, Python, Flask, Web Development]
---

Recently at work, I was asked to help develop a website that would be used internally within our company as a means of storing, querying and managing data in a centralized database. This website would be an alternative to the current mode of operation which involved simply sending excel files to one another as a means of sharing information. Because our team has a strong programming background in Python, we decided to implement this tool using [Flask](https://flask.palletsprojects.com/en/1.1.x/). In this blog, I give a quick 5 min holistic overview of how to use this tool amongst many many others for successful web development. 

# What tools do I need?

Before we get started, I will present a quick primer on the essential tools that you will need to use alongside Flask. The main ones include:

* **venv** (a python package to create virtual environments)
* **Flask** (our Python web development tool)
* A **SQL** database (where our data is stored)
* **SQLAlchemy** (a way to read data into our Python environment)
* **HTML, CSS and Javascript** (our trio of web languages to create pages and user interactivity)
* **Bootstrap** (a library of CSS code that we can use to make our website more sophisticated)
* **Jinja** (a web template engine that helps Python communicate with HTML)
* **WTForms** (python way of getting user input into the website)

As a quick side note, I should point out the [Django](https://www.djangoproject.com/) is a potential alternative web development tool in Python. Flask and Django are broadly able to perform the same task but have very different underlying functionalities. I have heard that if Flask was a bike, then Django is like a car. The latter has a lot of scaffolding in place to ensure quick web development whereas the other has more flexibility and relies on more 3rd party apps to perform certain functions. In this project we desired flexibility above all else, but we probably could have done this in Django as well.

For a more detailed overview of how to do this, I refer you to Miguel Grinberg's excellent [mega-tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) in using Flask. 

# Functionality

Let's look at each tool separately to see the role that it performs. 

## Creating a virtual environment with **venv**

Every Python user should be familiar with using this tool. It is essentially a handy way of keeping your Python environments separate from one another when developing different software projects. To create and load your environment (in Linux), you can do the following:

`python -m venv my_python_environment`

`source my_python_environment/bin/activate`

Then you can use the `pip` package manager to install any packages that you might need. A handy way of doing this is to use a requirements.txt file that lists all the packages and versions that you require. You then install them in your environment with 

`pip install -r requirements.txt`

If you already have an environment that you have been using, and you want to have the same packages in another virtual environment, then you can simply get your requirements.txt file with 

`pip freeze > requirements.txt`

## Alembic and Flask db

These tools are all about creating the database that will talk with your Flask application. As far as I know there are 2 main ways that you can go around this. You can simply create your SQL database separately and then talk to it with your SQL application or you can build it from your python Flask scripts with Alembic and Flask db.

Minimal example here on how to get started...

## Flask 

Flask is made up of many different packages, but the key ones that you will probably need are the following:

* `Flask==1.1.2`
* `Flask-Bootstrap4==4.0.2`
* `Flask-Login==0.5.0`
* `Flask-Migrate==2.5.3`
* `Flask-Moment==0.9.0`
* `Flask-SQLAlchemy==2.4.1`
* `Flask-WTF==0.14.3`

A minimal flask environment will typically look something like this. You will have an app folder that contains the main code. Outside of it there will a python file that will initiate the code in that app folder. Inside the app folder there will be an __init__.py script that will instantiate the app object and several folders (with their own __init__.py files) that will refer to the submodules of your website (one for user authentication, another for main, etc). So a simple hello world example would have the following folder structure:

[insert image here]

To start running the Flask app; simply do the following commands (again on Linux), and access localhost:500 on your web browser. 
`
export FLASK_APP=manage.py
flask run
`


# Recipe

* Step 1: Pick your database (SQLite, PostgreSQL, MySQL, ... or a non-relational database structure)
* Step 2: Pick your database communicator. For reading SQL into Python, I struggle to find any alternatives to using SQLAlchemy. It's a great tool for querying and writing data to your SQL database. For instance, here is some sample code below:

`
Sample SQL alchemy code here
`

* Step 3: Getting a basic Flask application to run. Now this I probably found to be the least intuitive part since there needs to be a basic struture to how you organise your flask environment. Generally, you would organise it as follows:
	* App folder
		* templates
			* subfolders (jinja templates)
		* subfolders (such as auth or users)
			* Python code
		* index.html
		* main.py
	* manage.py (code to initiate your Flask App)

So for instance, on a very simple level your manage.py folder would look something like this

`
manage.py script
`

And your main.py script would contain the following:

`
main.py
`
* Step 4:  Now even less intuitive, getting Flask to communicate with SQLalchemy. This involves creating classes in a seperate models.py script that gets imported into whatever python script thats needed. Then if you want to query the database, you pass that class into the db.request. What is the point of doing this rather than having a simple SQL request? Best answer that I can think of is that you can make it quite versatile and create additional methods to your classes and also do this handy thing called backreferencing which is an elegant way of doing table joins. Then you can just call the property of that user in your python code; pretty neat. 
* Step 5: Getting Flask to display your page. Really you need to do 2 things: set a link to point to; and then either you render a template or you redirect to a specific url. I don't know the difference between the 2 approaches... really. 
* Step 6: Writing HTML code (shudder). A very painful part of the process since I regard HTML code as one of the least legible and understandable codes to write and read. Hopefully with templates you don't need to worry too much about this part, but to make it pretty; you need Bootstrap.
* Step 7: Bootstrap; as far as I know, is just a library of CSS code that you can you use to make your website look awesome. Most of the time; I use this reference and literaly copy and paste bits of code that I need to do stuff like drop-down menus and cards (for the win btw). 
* Step 8: Organising your Flask code with blueprints... A way to structure different parts of your website...
* Step 9:Forms; ways to edit information and get user input. 


# Overall architecture

So this is my rough guide to using Flask. No doubt over the next few weeks I'll get that much wiser in learning how to use it better. Here is an architecture overview about how all the different tools communicate with one another. 

![](https://devopedia.org/images/article/140/9072.1547744489.png)

I very well might come back and update this article. Thanks for reading and happy developing; some links below to help you get started.

* https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world
* https://flask.palletsprojects.com/en/1.1.x/
* https://getbootstrap.com/docs/4.4/getting-started/introduction/