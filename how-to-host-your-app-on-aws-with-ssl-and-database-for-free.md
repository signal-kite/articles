WORKINPROGRESS
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
3. [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
4. [AWS EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html).


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
![image](https://user-images.githubusercontent.com/125164513/226763279-a6378b84-8821-4eec-a4b8-892a538bb36f.png)

4. **Activate the virtual environment** In your VS Code terminal, run the command to activate the virtual environment.
For Windows, run:

```
venv\Scripts\activate.bat
```

For Mac/Linix, run:

```
source venv/bin/activate
```
5. **Install the Python packages** To run the Flask app, we need to install Flask itself. To do it, run the following command:
```
pip install flask
```
> **Note**
> Usually, the Python package manager PIP installs automatically but if it didn't happen please refer to [the official documentation](https://pip.pypa.io/en/stable/installation/).

6. **Run your app** To make sure the app works normally, run it:
```
flask run
```
> **Note**
> In a case of some issues, please refer to the [Flask Quick Start](https://flask.palletsprojects.com/en/1.1.x/quickstart/).

#### Set up the Amazon account
1. **Create an account** Firstly, go to the AWS console and create [a new account](https://portal.aws.amazon.com/billing/signup#/start/email). You will need to verify your email, enter a credit card (even if you will only free tier), and verify your phone number.
2. **Sign in the AWS console** Now you can sign in. Please note, that verification of your new account may take some time, up to several hours. It looks like you can sign in but can't do anything in the console.
3. **Copy your credentials** Inside the AWS console, click your username (in the top right corner) and select _Seciruty credentials_. 
![image](https://user-images.githubusercontent.com/125164513/226777317-6f56f140-b690-4f29-b39b-ff3f14e437ca.png)
Scroll a bit and click the _Create access key_ button:
![image](https://user-images.githubusercontent.com/125164513/226777655-6c7ffa25-8cb9-4d5e-948d-42caf6e33278.png)
You will see the warning titled "Root user access keys are not recommended", click the checkbox and then the _Create access key_ button.
In the next window copy both access id and secret key. You also can download the keys in a file.
4. **AWS EB Configuring** In your

 
