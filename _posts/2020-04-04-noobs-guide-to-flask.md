---
layout: post
title: A noob's 5 minute guide to web development in Flask
date: 2020-04-04 00:00:00 +0100
categories: [Programming, Python, Flask]
tags: [Programming, Python, Flask, Web Development]
---

Recently at work, I was asked to help develop a website that would be used internally within our company as a means of storing, querying and managing data in a centralized database. This website would be an alternative to the current mode of operation which essentially involved consistently sending excel files to one another as a means of sharing information. Because our team has a strong programming background in Python, we decided to implement this tool using [Flask](https://flask.palletsprojects.com/en/1.1.x/). When undertaking this project, I thought that a quick technical overview of the architecture surrounding web development in Flask would have been super useful to a noob like me. Hence, I decided to share my quick 5 minute overview of how to do web development in Flask. For a more detailed overview of how to do this, I refer you to Miguel Grinberg's excellent [mega-tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) in using Flask. 

# Why not Django?

Those who have heard of Flask, have probably also heard of [Django](https://www.djangoproject.com/) which is an alternative web development tool in Python. They broadly are able to perform the same task bu have very different underlying functionalities. The best way I have heard of their differences summarised is that Django is like a car while Flask is like a bike. The former has a lot of scaffolding in place to ensure quick web development whereas the other has more flexibility and relies on more 3rd party apps to perform certain functions. In this project we desired flexibility above all else, but we probably could have done this in Django as well.

# Ingredients

In order to achieve our web development goals, we will be using the following resources.

* Flask (our Python web development tool)
* A SQL database (where our data is stored)
* SQLAlchemy (a way to read data into our Python environment)
* HTML, CSS and Javascript (our trio of web languages to create pages and user interactivity)
* Bootstrap (a library of CSS code that we can use to make our website more sophisticated)
* Jinja (a web template engine that helps Python communicate with HTML)
* WTForms (python way of getting user input into the website)

 I will explain in more detail each one does and how it contributes to the overall system architecture later.

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