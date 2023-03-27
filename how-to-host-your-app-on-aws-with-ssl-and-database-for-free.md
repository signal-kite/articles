# How to host your web application with SSL and database for free for at least one year

Photo by <a href="https://unsplash.com/@austindistel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Austin Distel</a> on <a href="https://unsplash.com/photos/mpN7xjKQ_Ns?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## Why AWS
Amazon Web Service is a powerful platform that allows doing a lot of things. Literally speaking, you can do everything beginning with simple things like hosting and ending with super-complex machine learning modeling. Moreover, most of their services have:

- free trials
- 12-month free tier
- forever free offers
The full list of services with their free options can be found [here](https://aws.amazon.com/free/).

Unfortunately, there are flaws too: 

- To use many services and get the most out of them you need to be proficient in the service's topics
- many services have confusing UI/UX and a million options
- some of the options can easily turn a free service into a crazy-cost one.

So, this article is dedicated to hosting applications with AWS. When you start a new startup you don't want to pay a lot for an app that you are not sure to be within a year. Then, why wouldn't use AWS to host the app, connect to your domain name, and provide SSL, alongside with a relational database?

### Why host an app on AWS Elastic Beanstalk?
1. It has a great free tier valid for 12 months.
2. It's not as hard as running your app, say, on DigitalOcean
3. You can easily attach your own domain name and SSL.

### Some flaws
1. Traditionally, the UI/UX is not simple and intuitive but this article should guide you through all the obstacles and show all the tricks.
2. It may be suitable only for apps that can be easily run on Linux (like Node.js or Python/Flask). We tried to run a .Net app but didn't find a way to install the additional libraries that we needed.

<div class="message-box info">
 Don't forget to check [the Amazon program for startups](https://aws.amazon.com/activate/founders/) for additional funding and discounts!
</div>

> **Warning**
> We provide some solutions for Windows OS, please check the reference for other operating systems.

> **Note**
> As a sample application, we will create an app written in Python (Flask).

Before we start, let's define the list of tasks we would like to accomplish:

- [ ] Create an application (in terms of AWS) and deploy it
- [ ] Attach our own domain name and provide SSL
- [ ] Create the database and connect to it (we will work with PostgreSQL)

#### Prerequisites

You need the following tools and programs to be installed:

1. IDE. We use [MS VS Code](https://code.visualstudio.com/)
2. Python. At the moment, it's the version 3.8 but you can [download a fresher version](https://www.python.org/downloads/)
3. [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
4. [AWS EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html).


### Create and deploy an app on AWS Elastic Beanstalk (EB)
#### Install and run your app locally

1. **Create a folder and add the code there.** The very minimal application in Flask may have as few as just [5 lines of code](https://flask.palletsprojects.com/en/2.2.x/quickstart/#a-minimal-application). You can copy this code into the _application.py_ file and put it into a root folder.
2. **Create a virtual environment** If you don't know what is a Python virtual environment you can read [this nice guide](https://thebiogirls.com/index.php/2020/01/18/how-to-set-up-a-python-virtual-environment/).
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

For Mac/Linux, run:

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
> In the case of some issues, please refer to the [Flask Quick Start](https://flask.palletsprojects.com/en/1.1.x/quickstart/).

#### Set up the Amazon account

1. **Create an account** First, go to the AWS console and create [a new account](https://portal.aws.amazon.com/billing/signup#/start/email). You will need to verify your email, enter a credit card (even if you will only free tier), and verify your phone number.
2. **Sign in to the AWS console** Now you can sign in. Please note, that verification of your new account may take some time, up to several hours. It looks like you can sign in but can't do anything in the console.
3. **Copy your credentials** Inside the AWS console, click your username (in the top right corner), and select _Security credentials_. 

![image](https://user-images.githubusercontent.com/125164513/226777317-6f56f140-b690-4f29-b39b-ff3f14e437ca.png)

Scroll a bit and click the _Create access key_ button:

![image](https://user-images.githubusercontent.com/125164513/226777655-6c7ffa25-8cb9-4d5e-948d-42caf6e33278.png)

You will see the warning titled "Root user access keys are not recommended", click the checkbox and then the _Create access key_ button.
In the next window copy both the access id and secret key. You also can download the keys in a file.

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

6. **Create an environment** In terms of AWS, here is the hierarchy Application -> environment. So, the second step is to create an environment. Run the command:

```
eb create
```

Again, it will ask you some questions but most of them are pretty straightforward. Answer "_classic_" to the question about the load balancer.
More information can be found in [their documentation on this command](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-create.html).

After it's done you can sign in to the AWS console and make sure your application and environment are deployed successfully. It will provide the link you can check the working app: 

![image](https://user-images.githubusercontent.com/125164513/226986287-99599c84-af62-4d1e-b27b-3b91dd413df5.png)

7. **Deploy** Next time, when you are ready to deploy your changes, just run this command:

```
eb deploy
```

> **Note**
> If your app's front end is built with Webpack compile your JavaScript code on your website and deploy the resulting bundle. We didn't find a way to make AWS EB compile the JavaScript code during deployment. Don't forget to remove the dist folders from your .gitingore file.

> **Note**
> If you use a source control like GitHub please make sure you commit the changes first because AWS EB CLI automatically grabs the latest from the repository. For how to automate the process, see the section "Automation".

> **Warning**
> Don't forget to set up your environment variables. To add them, click your environment, then the _Configuration_ link in the left menu, and then click the _Edit_ button against the _Software_ pane: 

![image](https://user-images.githubusercontent.com/125164513/227616385-ebc3e434-c9c7-4cc7-a2e6-5b35c3f28b73.png)

To automate this process, we create the Python script (see the last section about automation).

### Set up SSL

1. **Request the certificate** In the AWS console, in the search field, start typing the _certificate_ word, then click the Certificate manager: 

![image](https://user-images.githubusercontent.com/125164513/226989389-7e8fcba1-9cfc-436b-9c3c-042458c35450.png)

Then click the _Request certificate_ button: 

![image](https://user-images.githubusercontent.com/125164513/226991614-c34fe78b-3196-42fe-9b37-57500d1e70fd.png)

Leave the _Request a public certificate_ checkbox checked and click the _Next_ button: 

![image](https://user-images.githubusercontent.com/125164513/226991833-60010f86-d983-4c11-8baf-ec10312dfae7.png)

On the next page, enter the domain name (that you already have bought) you want to be covered by this certificate. If you want several domains to be covered, enter them starting from the wildcard, for example "*.mydomain.com".

> **Note**
> You enter only those domain names that will be connected to this application. Usually, it may be a website like www.mysite.com or an application app.mysite.com. If you have several apps, you should create separate accounts for them to avoid exceeding the free tier.

So, your page could look something like that: 

![image](https://user-images.githubusercontent.com/125164513/226993158-b29e09e8-a404-4a02-a52c-617e215ac8c3.png)

Leave all the checkboxes untouched and click the _Request_ button.

You then will be redirected to the page with certificates. If the page looks empty click the button with the round arrow to update it, then click the link with _Certificate Id_ and you will be redirected again. Scroll a bit down to see the domain list.

2. **Validate your domain ownership** In the previous step, we left the _DNS validation_ checked, so we need to prove we own our domain names. To do that, sign in to the control panel of your hosting provider. In our example, we will use the Namecheap. After you signed in, open the list of your domains, find your domain, and click the _Manage_ button: 

![image](https://user-images.githubusercontent.com/125164513/226995705-a615d298-4004-4e9d-8e1f-59f225d2068c.png)

Then click the _Advanced DNS_ tab, and you will be redirected to the page with all DNS records for your domain. Click the _ADD NEW RECORD_ button: 

![image](https://user-images.githubusercontent.com/125164513/226996194-70330c3f-8e15-458f-bf85-2c0f664cd0cd.png)

and then select _CNAME Record_: 

![image](https://user-images.githubusercontent.com/125164513/226996441-dba5f5b2-ad55-4122-8e17-9278abd8d3d7.png)

Now, come back to the certificate manager, and copy the following values to the corresponding fields in the CNAME record:

- CNAME name --> Host
- CNAME value --> Target

![image](https://user-images.githubusercontent.com/125164513/226997311-17f99233-728e-480d-b113-c5f60edeec2c.png)

> **Note** Do not copy the full **CNAME name**, only the part **before** your domain name as shown in the picture above. For the domain with "www", "app" or any other prefix, copy the prefix but not the domain name.

> **Note** We found the Namecheap DNS record editor a bit weird: when we copy the values right from the AWS Console, the UI gets frozen. So, instead, we copy the values to some plain text editor, then copy the text into the Namecheap record.

So, in this example, you create 2 records, one of them should look like this: 

![image](https://user-images.githubusercontent.com/125164513/226998642-35c7618f-9625-4355-81b0-baab139f1f64.png)

After you added your record, go back to your AWS certificate and refresh the page. You will see it's in pending status: 

![image](https://user-images.githubusercontent.com/125164513/226999177-a6ad9d27-96ed-4f5f-8118-8406464c09c1.png)

Usually, the DNS updating records takes 20-30 minutes but sometimes longer. As soon as the validation process is done, the status will be changed to "Issued" and the domains statuses to "Success": 

![image](https://user-images.githubusercontent.com/125164513/227002269-bca8f068-a882-401c-9978-60e470cc9f16.png)

3. **Add a load balancer** Come back to your EB console. Click your environment, then _Configuration_ link: 

![image](https://user-images.githubusercontent.com/125164513/227392070-e99c822c-d4f8-4d89-a4a6-0f37fa3461f6.png)

Scroll to the _Load balancer_ and click the _Edit_ button on the right. Click the _Add listener_ button. Then add the values as shown in the picture below: 

![image](https://user-images.githubusercontent.com/125164513/227392479-a88b6bd7-f34c-44b0-ba09-0bfd045f9a8f.png)

In the _SSL certificate_ dropdown, select the certificate you created in the previous step.
Click the _Add_ button, you will be redirected back. Scroll the page to the end and click the _Apply_ button.

4. **Update the DNS record** Copy your environment AWS URL. For example, you can copy the link from the left menu from the _Go to environment_ item: 

![image](https://user-images.githubusercontent.com/125164513/227393220-eb9f52f4-afc9-4867-959c-d0141bbcb6ba.png)

Now, open your hosting provider control panel. We will show you how to update the DNS if your hosting provider is Namecheap. For other hostings, please refer to their documentation. Open the list of your domains and click the _Manage_ button against your domain. Then click the _Advanced DNS_ tab. In this tab, find the CNAME record, pointing to your parking page. If you don't have a such record, create a new one by clicking the _ADD NEW RECORD_ button: 

![image](https://user-images.githubusercontent.com/125164513/227393684-2b9aab1e-e92d-4844-9620-eeea4d460df0.png)

You may leave the _Host_ field as is (if you want the URL to start with "www", or change it to "@" (without quotes) if you want to have just a bare domain name - in this case your URL will be like https://mywebsite.com). In the _Value_ field, paste the environment link you copied before.

That's it!

> **Note**
> The changes may take some time, usually 20-30 minutes, sometimes more. To verify your domain with SSL works, just enter in your web browser the domain name with "https" like: "https://mywebsite.com". If after 1-2 hours it's still not working, please check if you've done everything as described, if so, please ask your hosting provider for assistance.

> **Warning**
> Please be aware that SSL and the load balancer can be used for free for 1 year **only for for one application/environment**. If you add one more, even without a load balancer, you can be immediately charged.

## Set up your AWS RDS PostgreSQL database
### Create a special database security group

If your database will be accessed remotely, you will need to create and configure a security group.

1. **Know your environment security group** Open your AWS EB console, click the environment, and then click the _Configuration_ in the left menu. In the _Instances_ pane, click the _Edit_ button, and then scroll to the end to see the list of security groups. You will see something like that: 

![image](https://user-images.githubusercontent.com/125164513/227404563-1f2fd039-ae35-418f-8ea5-c0fb3d8d1f25.png)

Write down the selected group Id.

2. **Create a new security group** In your AWS console, start typing "ec2" in the search field, then select the _EC2_ service: 

![image](https://user-images.githubusercontent.com/125164513/227400233-0474f753-5c36-4659-833a-5fa3c75366ae.png)

Scroll a bit to see the menu on the left and click it: 

![image](https://user-images.githubusercontent.com/125164513/227402761-6289b0d2-b140-4db3-9efc-9926c8905bb3.png)

Click the _Create security group_ button on the top right. Enter your group name, for example, "Database security group", then the description (optional). Click the _Add rule_ button in the _Inbound rules_ pane.

For the new rule:

- In the _Type_ field select "PostgreSQL"
- Leave the _Protocol_, _Port range_, and _Source_ as is
- In the field next to the _Source_ start typing "sg" and select the group accompanied by the environment (that you wrote down in step 1). This will allow your application to access the database and prevents connections from any other places.

If you want to access the database from your computer, add the second rule but in the _Source_ field select the "My IP" value. Your list will look something like that: 

![image](https://user-images.githubusercontent.com/125164513/227404887-01907624-c088-4609-87e1-f4d4737d4992.png)

Click the _Create security group_ button.

2. **Create the database instance** Go to your AWS console and start typing: 

![image](https://user-images.githubusercontent.com/125164513/227399066-f5f04a34-98ed-42ad-a282-adb36a4f0dce.png)

Then click the RDS service, on the following page, click the _Create database_ button. 

Leave the _Choose a database creation method_ pane with the _Standard create_ checked, then select your database and version (or leave it as is).

In the _Templates_ pane select the _Free tier_ radio button and scroll a bit to the _Settings_ pane.

Enter your database name and master user name, then enter and re-enter your password.

> **Note**
> This database name is used for identifying the databases in the list of your databases, this name is also used in the hostname as we can see later.

In the _Instance configuration_ section, you can leave "db.t3.micro".

In the _Storage_ section, leave the storage type as is ("General Purpose SSD (gp2)") but change the allocated storage to 20Gib.

> **Warning**
> This step is very important! We don't know why they set up 200 Gb by default value for a free tier because the maximum storage that they don't charge for is 20Gb, and if you leave it as is (200 Gb) you will be immediately charged even if your data is just as little as 2Kb.

You can leave the _Storage autoscaling_ section as is.

In the _Connectivity_ section, leave the preselected choices as is.

In the _Public access_ select **Yes** and then in the _Existing VPC security groups_ select the security group you created in the previous step.

In the _Additional configuration_ enter the **Initial database name** - it's the name of the database you will use in your connection string.

> **Warning**
> If you omit to enter the database name, it will be set up as "postgres". 

Change or leave other options, then click the _Create database_ button.
To make sure your database was created successfully, open _RTS -> Databases_ and find your database in the list: 

![image](https://user-images.githubusercontent.com/125164513/227603893-e76dd4f9-11e1-4195-9b6c-cb48d557c6ed.png)

It usually takes several minutes to create and run the instance so wait until its status is "Available".

3. **Connect to your database** To connect to your database from your application or any client (we use great free [pgAdmin](https://www.pgadmin.org/)) you need to form the connection string or get its parts. For PostgreSQL, the connection string has a format

```
postgres://YourUserName:YourPassword@YourHostname:5432/YourDatabaseName
```

Use the same parameter to connect to the database from a client. You already know the username, password, and database name. To know the hostname, click the database and copy the endpoint on the _Endpoint & port_ pane: 

![image](https://user-images.githubusercontent.com/125164513/227609754-f50f56f7-acfb-40b6-b202-2b34fbf83e07.png)

## Troubleshooting
In this section, we list the issues we faced but potentially anything can happen. Ask Stackoverflow or ChatGPT for assistance :)

### In your AWS log, there is an error "Invalid requirements.txt on deploying django app to aws beanstalk"
**Solution** To your project's root folder, add the _.ebextensions_ folder and create the file named, say _01_packages.config_ with the following content:

```
packages:
  yum:
    git: []
    postgresql93-devel: []
```

More details are on this issue in [the Stackoverflow post](https://stackoverflow.com/questions/18554666/invalid-requirements-txt-on-deploying-django-app-to-aws-beanstalk).

### Error "Error: pg_config executable not found"
**Solution**

1. uninstall psycopg2, 
2. install psycopg2-binary
3. update requirements
4. push to git
5. redeploy

### Error "ImportError: No module named psycopg2"
**Solution** Try to change the protocol of your database connection string from "postgresql" to "postgresql+psycopg2" so the connection string will look like

```
postgresql+psycopg2://YourUserName:YourPassword@YourHostname:5432/YourDatabaseName
```

## Automation
Here are several scripts that you can use to automate tedious tasks.

### Fast initial setup of the application

For Windows:

```
call python -m venv venv
call venv\Scripts\activate.bat
call python -m pip install --upgrade pip
call pip install -r requirements.txt
call npm install
call npm run dev
call flask run
```

For Unix-based Oss:

```
source venv/bin/activate
python3 -m pip install --upgrade pip
pip install -r requirements.txt
npm install
npm run dev
flask run
```

### Deployment bat
Create a .bat file in the root folder and put the following content inside:

```
call npm run --prefix webapp prod
call git add .
call git commit -m %1
call git push origin master
call eb deploy
```

To run the bat, in your terminal, run the command:

```
bat-file-name "Git message"
```

> **Note** Write the bat file name without the extension.

> **Warning**
> This command is for Windows. For Unix-based OS, please, amend it correspondingly.

### Python script to load the environment variables
You may add and change your env variables manually but for a faster process, we use the following script:

```
def send_env_vars_to_aws():
    import dotenv, os
    from configobj import ConfigObj
    dotenv_file = dotenv.find_dotenv()
    command = ''
    if dotenv_file is not None:
        config = ConfigObj(dotenv_file)     
        vars = map(lambda key:f'{key}="{config[key]}"', config.scalars)
        command = ' '.join(vars)
        os.system(f'eb setenv {command}')
```

> **Note**
> The _dotenv_ and _configobj_ packages should be installed.

Run this script from your terminal.
