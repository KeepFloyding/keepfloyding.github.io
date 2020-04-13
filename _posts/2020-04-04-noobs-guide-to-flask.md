---
layout: post
title: A Quick Guide to Web Development in Flask
date: 2020-04-04 00:00:00 +0100
categories: [Programming, Python, Web Development, Flask]
tags: [Programming, Python, Flask, Web Development]
seo:
  date_modified: 2020-04-11 18:00:39 +0100
---

Deploying machine learning shouldn't be hard, and there are many python tools that you can use to bring your models into production. [Flask](https://flask.palletsprojects.com/en/1.1.x/) is a web framework that is incredibly flexible and powerful, and that you can use to accomplish this task. However, the plethora of tools that are out there to manage the different functionalities of your web app can make the whole process confusing. This blog post aims to present a quick holistic overview of how to use Flask amongst many others to ensure successful and stress-free (almost) web development. For a more detailed overview of how to do this, I refer you to Miguel Grinberg's excellent [mega-tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) in using Flask. 

# What tools do I need?

Here is a list of essential tools that you will need to use to build your web app. 

* **Python** and **venv** (Python programming language within a virtual environment)
* **Flask** (a Python web development tool)
* **Jinja** (a web template engine that helps Python communicate with HTML)
* **HTML, CSS and Javascript** (a trio of web languages to create web pages)
* **Bootstrap** (a library of CSS code that can be used to make the web app more sophisticated)
* **SQL** database 
* **SQLAlchemy** (a way to read data from the database into the Python environment)
* **WTForms** (python way of getting user input into the website)

As a quick side note, I should point out the [Django](https://www.djangoproject.com/) is a potential alternative web development tool in Python. Flask and Django are broadly able to perform the same task but have very different underlying functionalities. I have heard that if Flask was a bike, then Django is like a car. The latter has a lot of scaffolding in place to ensure quick web development whereas the other has more flexibility and relies on more 3rd party apps to perform certain functions. More on this discussion can be found [here](https://medium.com/@SteelKiwiDev/flask-vs-django-how-to-understand-whether-you-need-a-hammer-or-a-toolbox-39b8b3a2e4a5).

# System Architecture

Each tool listed above communicates with each other as shown in the image below. The web app is served locally at `localhost:5000` using Flask. Python is the backend of the web page and performs 2 vital roles: it communicates with the SQL database using SQLAlchemy (performing both read and write functions) and it renders the html templates using Jinja. The webpages are defined using typical web technologies such as HTML, CSS and Javascript and are enriched stylistically by making use of the awesome Bootstrap library. 

![](/images/flask_architecture.png)

To get a better appreciation of how each tool performs its role, let's look at some sample code. Before we get started, install Python 3.7 or above and define your virtual environment with `venv`. It is essentially a handy way of keeping your Python environments separate from one another when developing different software projects. To create and load your environment (if using Windows, you can remove source function), you do the following:
{% highlight bash %}
python -m venv my_python_environment
source my_python_environment/bin/activate
{% endhighlight	 %}

Then you can use the `pip` package manager to install any packages that you might need. A handy way of doing this is to use a requirements.txt file that lists all the packages and versions that you require. You then install them in your environment with 

`pip install -r requirements.txt`

For the purposes of this tutorial, you can copy and paste the following in a `requirements.txt`
file:

* Flask
* SQLAlchemy
* other stuff


## Flask 

Right, let's get started with a simple Flask app. Typically, it would have the following folder hierarchy: 

![](/images/tree_snap.png)

There will be an app folder that contains the main code. Outside of it there will a python file that will initiate the code in that app folder. So say you can create a python file with the name of your app (`my_flask_app.py`) and it would have the following line: `from app import app`. i.e. from the app folder import the class called app. 

Now as such, inside the app folder there will be an `__init__.py` script that will instantiate the app object. Inside this script we will simply import the flask package and create the app. 

{% highlight python %}
from flask import Flask

app = Flask(__name__)
{% endhighlight %}


The variable `__name` will be automatically assigned. All well and good, now how do we create webpages? The index page (and really all flask webpages) can be created as follows (in the same `__init__.py` script :

{% highlight python %}
@app.route('/home')
def hello_world():
	return "hello world"
{% endhighlight %}

The function is simply a function which returns the string 'hello world'. The command above it refers to something called a decorator which simply provides additional functionality to our function. In this case, the app.route function adds a url to our app class that then executes the hello world function. 

Now to test whether this will run, you go to the same directory as your master script (`my_flask_app.py`) and run the following commands. To start running the Flask app; simply do the following commands (on windows, you use SET instead of export)"" 

{% highlight bash %}
export FLASK_APP=my_flask_app.py
flask run
{% endhighlight %}

Now access localhost:5000 on your web browser and if everything has gone well, you should get a simple html page that shows 'hello world'. 

## Flask extensibility with Blueprints

Now the beauty of Flask lies in its easy extensibility. Let's say we wanted to create a submodule that will showcase a different part of our website. We simply create a folder inside the `app` folder that has its own `__init__.py` with the following content:

{% highlight python %}
from flask import Blueprint

mod1 = Blueprint("mod1",__name__)

@mod1.route('/')
def print_hello():
	return 'hello to mod1'
{% endhighlight %}

Here we define an index page in mod1 that will return another string which will say 'welcome to mod1'.

Now we just need to change our `app/__init__.py` file to the following:

{% highlight python %}
from flask import Flask

app = Flask(__name__)

from app.mod1 import mod1 
app.register_blueprint(mod1, url_prefix='/mod1')

@app.route('/')
def hello_world():
	
	return 'hello world!'
{% endhighlight %}

Now if we go `localhost:5000/mod1`, we should see a simple web page that has the string `hello to mod1`. In this way, you can tag on submodules to your Flask app as you see fit. 

## HTML rendering and Jinja2

To add more rich detail to your html pages you can do a couple of things. You can choose to return an html string from one of your functions and it will be automatically rendered. So for instance, the `hello_world()` function can be changed to:

{%highlight python%}
@app.route('/')
def hello_world():
	return '''
	<body> 
	<h1> HTML Heading</h1>
	<p>HTML paragraph</p>
	<a href='https:\\www.google.com'>Link to google</a>
	</body>
	'''
{%endhighlight%}

However, embedding HTML code into python code can quickly get messy so a better alernative is simply to generate a seperate file and then redirect python to render using the `render_template` function from Flask. Note that we have to save our index.html under a folder called `templates`. 

{%highlight python%}
from Flask import render_template

@app.route('/')
def hello_world():
	return render_template('index.html')
{%endhighlight%}

Now for a really super cool functionality of Flask is the use of Jinja to actually embed python code in html. And its syntax is acutally quite easy to use. Say for instance that we have a variable in python that we want to pass into html and change the html page. In this case, say that sometimes I want the web page to say either 'hello' or 'good morning' depending on what time of day it is. In this case, I can just create a variable that takes a string after going through some python logic as follows. We then pass that variable as an additional input into the `render_template` function. 

{%highlight python%}
from flask import render_template
import time

@app.route('/')
def hello_world():

	t = time.localtime()
	current_time = time.strftime("%H:%M:%S", t)

	return render_template('index.html', current_time=current_time)
{%endhighlight%}

We can now call that variable in html by using \{\{`python code`\}\} such as the following:
{%highlight python%}
<body> 
	<h1> Hello world</h1>
	<p>The time is currently {{current_time}}</p>
	<a href='https:\\www.google.com'>Link to google</a>
	</body>
{%endhighlight%}

Pretty cool right? But what about for and if loops? Their syntax is a little different with a {%for n in range(10)%} and {%endfor%} in between any html code you want to loop through and a {%if loop%} and {%endif%} for any if statments. So say we want to print the time 10 times, we can just do the following:

{%highlight html%}
<body> 
	<h1> Hello world</h1>
	{%for n in range(10) %}
	<p>The time is currently {{current_time}}</p>
	{%endfor%}
	<a href='https:\\www.google.com'>Link to google</a>
</body>
{%endhighlight%}


## Managing databases

Odds are that you will want your app to read, write and edit data in a database. This can easily be done for SQL databases using the `SQLAlchemy` package. To get started, simply set up a SQLite database by downloading an opensource db browser like this [one](https://sqlitebrowser.org/), click on create a database insert the following SQL command to create a table of users (with some sample data). 

{%highlight SQL%}
CREATE TABLE "my_users" (
	"id"	INTEGER NOT NULL UNIQUE,
	"name"	TEXT,
	PRIMARY KEY("id")
);

INSERT INTO my_users (name)
VALUES ('Henry'),
		('Alex'),
		('Richard');

{%endhighlight%}

Now you can query your database in Flask using SQL alchemy like so. Add the following code to the `__init__.py` file:

{%highlight python%}
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:////my_database.db'
db = SQLAlchemy(app)


class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(120), unique=True, nullable=False)

{%endhighlight%}

And so now  you can query the users in your database as follows:

{%highlight python%}
from app.models import User

users = User.query.all()

for user in Users:
	print(user.name)
{%endhighlight%}

Now with all the tools at your disposal, you can directly embed your database information into your html pages. 

## Other tools

There are a whole lot of other tools at your disposal that you can use to jazz up your website. These include things such as:

* user forms (Flask-WTF)
* Bootstrap to make the website pretty (Flask-Bootstrap4)
* Login forms to create easy user authentication
* Flask-Migrate for database initalisation

Amongst a whole lot of other tools. Make sure to check them out when you are extending the functionality of your website. 

# Further reading

So this is my rough guide to using Flask. My hope is that the take home lesson in this blog is an overview of the architeture around web development in Flask. To close, I present to you some resources for going in a bit deeper. Thanks for reading and happy developing; some links below to help you get started:

* [https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)
* [https://flask.palletsprojects.com/en/1.1.x/](https://flask.palletsprojects.com/en/1.1.x/)
* [https://getbootstrap.com/docs/4.4/getting-started/introduction](https://getbootstrap.com/docs/4.4/getting-started/introduction)

