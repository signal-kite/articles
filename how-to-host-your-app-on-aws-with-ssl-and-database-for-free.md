# How to host your database with SSL and database for free for at least one year

## Why AWS
Amazon Web Service is a powerful platform that allows doing a lot of things. Literally speaking, you can do everything beginning with simple things like hosting and ending with super-complex machine learning modelling. Moreover, most of their services have:
- free trials
- 12 month free tier
- forever free offers
The full list of services with their free options can be found [here](https://aws.amazon.com/free/).

Unfortunately, there are flaws too: 
- To use many of services and get most of them you need to be proficient in the service's topics
- many services have confusing UI/UX and a million of options
- some of options can easily turn a free service into a crazy-cost one.

So, this article is dedicated to hosting an applictations with AWS. When you start a new startup you don't want to pay a lot for the app that you are not sure to be with in a year. Then, why wouldn't use an AWS to host the app, connect to your domain name, provide SSL, alongside with a relational database?

### Why host an app on AWS Elastic Beanstalk?
1. It has a great free tier valid for 12 montsh.
2. It's not as hard as running your app, say, on DigitalOcean
3. You can easily attach your own domain name and SSL.

### Some flaws
1. As a tradition, the UI/UX is not simple and intuitive but this article should guide you through all the obstacles and show all the tricks.
2. It may be suitable only for apps that can be easily run on Linux (like Node.js or Python/Flask). We tried to run a .Net app but didn't find a way to install the additional libraries that we needed.

> **Note**
> Don't forget to check [the Amazon program for startups](https://aws.amazon.com/activate/founders/) for additional funding and discounts!

> **Warning**
> We provide some solutions for Windows OS, please check the reference for other operation systems.

> **Note**
> As a sample application, we will create an app written in Python (Flask).

Before we start, let's define the list of task we would like to accomplish:
- [ ] Create an application (in terms of AWS) and deploy it
- [ ] Attach our own domain name and provide SSL
- [ ] Create the database and connect to it (we will work with PostgreSQL)

#### Prerequisites
You need the following tools and programs to be installed:
1. IDE. We use [MS VS Code](https://code.visualstudio.com/)
2. Python. At the moment, it's version the 3.8 but you can [download a fresher version](https://www.python.org/downloads/)
3. [AWS EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html).


### Create and deploy an app on AWS Elastic Beanstalk (EB)
#### Install and run your app locally
1. **Create a folder and add the code there.** The very minimal application in Flask may have as few as just [5 lines of code](https://flask.palletsprojects.com/en/2.2.x/quickstart/#a-minimal-application). You can copy this code into the _application.py_ file and put it into a root folder.
2. **Create a virtual environment** If you don't know what is a Python virtuall environment you can read [this nice guide](https://thebiogirls.com/index.php/2020/01/18/how-to-set-up-a-python-virtual-environment/).
Basically in your root folder you run the following command:
```
python -m venv venv
```
This is a command for Windows, for other OS it may look like this:

```
python3 -m venv venv
```
3. **Select an interpreter** In your VS Code, click *View -> Command Palette -> Python: Select Interpreter* and then enter into the prompt field:
```
.\venv\Scripts\python.exe
```
5. 