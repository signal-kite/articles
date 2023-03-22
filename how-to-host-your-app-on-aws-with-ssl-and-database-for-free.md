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
4. **AWS EB Configuring** In your IDE terminal run the command (if you have or will have multiple applications, please jump to p.5):
```
eb init
```
The command starts the configuring process and will ask you for credentials. It also will ask other questions like region (you need to remember the region because the application will be deployed exactly in the given region and will be visible in others).
If you don't want to create SSH now, you can omit this step. (More details can be found [here](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-configuration.html)).
5. **AWS EB Configuring for multiple apps** If you have or plan to have multiple applications, you can try another approach. Open the _C:\Users\username\.aws\config_
 or _~/.aws/config_ file (depending on your operating system) and add the following text:
```
[profile profile-app-1]
aws_access_key_id = XXXXXXXXXX
aws_secret_access_key = XXXXXXXXXXXX

[profile profile-app-2]
aws_access_key_id = XXXXXXXXXX
aws_secret_access_key = XXXXXXXXXXXX
```
The profile name is arbitrary (but should not contain spaces).
Then, when you configure EB, just use one of the profile names:
```
eb init --profile profile-app-2
```
6. **Create an environment** In terms of AWS, here is hierarchy Application -> environment. So, the second step is to create an evnironment. Run the command:
```
eb create
```
Again, it will ask you some question but most of them are pretty straightforward. Answer "_classic_" to the question about the load balancer.
More information can be found in [their documentation on this command](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-create.html).

After it's done you can sign in into the AWS console and make sure your application and environment are deployed successfully. It will provide the link you can check the working app: 
![image](https://user-images.githubusercontent.com/125164513/226986287-99599c84-af62-4d1e-b27b-3b91dd413df5.png)


> **Note**
> If your app's front-end is built with Webpack compile your JavaScript code on your website and deploy the result bundle. We didn't find a way to make AWS EB compile the JavaScript code during deployment. Don't forget to remove the dist folders from your .gitingore file.

> **Note**
> If you use a source control like GitHub please make sure you commit the changes first because AWS EB CLI automatically grabs the latest from the repository. How to automate the process, see the section "Automation".

### Set up SSL
1.**Request the certificate** In AWS console, in the search field, start typing the _certificate_ word, then click the Certificate manager: 
![image](https://user-images.githubusercontent.com/125164513/226989389-7e8fcba1-9cfc-436b-9c3c-042458c35450.png)
Then click the _Request certificate_ button: 
![image](https://user-images.githubusercontent.com/125164513/226991614-c34fe78b-3196-42fe-9b37-57500d1e70fd.png)
Leave the _Request a public certificate_ checkbox checked and click the _Next_ button: 
![image](https://user-images.githubusercontent.com/125164513/226991833-60010f86-d983-4c11-8baf-ec10312dfae7.png)
On the next page, enter the domain name (that you already have bought) you want to be covered by this certificate. If you want several domains to be covered, enter them starting from the wildcard, for example "\*.mydomain.com".

> **Note**
> You enter only those domain names that will be connected to this application. Usually, it may be a website like www.mysite.com or an application app.mysite.com. If you have several apps, you should create separate accounts for them to avoid exceeding the free tier.

So, your page could look something like that: 
![image](https://user-images.githubusercontent.com/125164513/226993158-b29e09e8-a404-4a02-a52c-617e215ac8c3.png)
Leave all the checkboxes untouched and click the _Request_ button.




## Automation

## Troubleshooting
