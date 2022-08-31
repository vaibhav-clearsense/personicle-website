---
date: "2022-07-15"
title: "How External Applications Can Start Connecting to Personicle"
image: "images/how-external-applications-can-start-connecting-to-personicle.png"
author_info: 
  name: "Zara Ahmed"
  image: "images/author/zara-ahmed.jpeg"
draft: false
---

#### _External Data Connections Repository: Setup and Usage Instructions_

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

This article includes setup and usage instructions for External Data Connections Repository. 

## Prerequisites
-	Python 3.7+
-	Postgresql db
-	Azure event hub for streaming data packets
-	Developer applications for external data applications
        -- Google fit
        -- Fitbit
        -- 	Oura ring

## Role of the application in Personicle Architecture:
<in-progress>

### A. Environment Setup

#### PostgreSQL setup

The external data connections service uses postgresql database for managing user credentials and tokens, therefore we need to install postgresql development libraries before setting up and running the application.

##### For Linux/ WSL (Windows):
```sudo apt-get install libpq-dev python-dev```

##### For Mac:

You can use homebrew to install libpq library:
```brew install libpq```

##### Python environment set up (Mac, Linux and Windows (WSL))
We recommend using Python3.7+ for running the Flask application. You create a python virtual environment for the application as follows:
1.	Follow the instruction listed [here](https://pip.pypa.io/en/stable/installation/) to install pip.
2.	Install virtualenv to manage the virtual environments for your application:
```python3 -m pip install --user virtualenv```
3.	Create a virtual environment: 
```python3 -m venv personicle-env```
4.	Activate the virtual environment for the current session: 
```source personicle-dev/bin/activate```
5.	Install the required packages listed in ```requirements.txt``` using the following command:	```pip install -r requirements.txt```
**Note**: You can deactivate the virutal environment using the command ```deactivate```.

### B. Configuring the Application
The configurations and environment variables required for running the application need to be included in config.ini file in the root directory. This file contains the client secrets, event hub configuration and database urls for the application.

##### Sample file:

```
[HOST]
HOST_ADDRESS=https://localhost:5000

[GOOGLE_FIT]
CLIENT_ID=<Google fit application client id>.apps.googleusercontent.com
CLIENT_SECRET=<Google fit application client secret>
REDIRECT_URL=/google-fit/oauth/access_token
AUTH_URL=https://accounts.google.com/o/oauth2/auth
API_ENDPOINT=https://www.googleapis.com/fitness/v1/
TOKEN_URL=https://oauth2.googleapis.com/token
SECRET_JSON=config_json/client_secret_<Google fit client secret>.apps.googleusercontent.com.json

[FITBIT]
CLIENT_ID=<fitbit application client id>
CLIENT_SECRET=<Fitbit application client secret>
REDIRECT_URL=/fitbit/oauth/access-token/
AUTH_URL=https://www.fitbit.com/oauth2/authorize
REQUEST_URL=https://api.fitbit.com/oauth2/token
API_ENDPOINT=https://api.fitbit.com

[SQLITE]
DATABASE=userDatabase/external_tokens.db

[EVENTHUB]
CONNECTION_STRING=<Event hub connection string for sending events>
DATASTREAM_EVENTHUB_CONNECTION_STRING=<Event hub connection string for sending data streams>
EVENTHUB_NAME=<Event hub name for sending events>
DATASTREAM_EVENTHUB_NAME=<Event hub name for sending data streams>
SCHEMA_REGISTRY_FQNS=<schema registry name space>
SCHEMA_REGISTRY_GROUP=<schema registry group name>

[TASK_QUEUE]
CONNECTION_STRING=<Azure queue connection string for sending data download task requests>
QUEUE_NAME=<Azure storage queue name>
QUEUE_NAME_OURA=<Azure storage queue name for Oura data download requests>
CONNECTION_STRING_OURA=< Azure storage queue connection string for Oura data download requests >

[CREDENTIALS_DATABASE]
USERNAME=<PostgreSQL username>
PASSWORD=<PostgreSQL password>
HOST=<PostgreSQL host>
NAME=<PostgreSQL database name>

[OURA]
CLIENT_ID=<Oura application client id>
CLIENT_SECRET=<Oura application client secret>
REDIRECT_URL=/oura/oauth/access-token/
AUTH_URL=https://cloud.ouraring.com/oauth/authorize
TOKEN_URL=https://api.ouraring.com/oauth/token
```
### C. Running the Application:

You can now run the application (in the virtual environment created in previous steps) using the following command
```
python run.py
```

This will host the flask application on ```port 5000``` on localhost.