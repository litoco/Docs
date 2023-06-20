# Docker with python from the [official docs](https://docs.docker.com/language/python/)

## Definitions:
#### 1. Docker container:
It is an OS process (that runs on a host machine like your personal laptop or desktop) and lets us run our application. It has its own networking, file-system and process trees. For more details [read from the official doc](https://docs.docker.com/get-started/overview/#containers).

## Step 1: Create an image of your app
### Create a python flask application
  We will create a simple python flask server application and run it inside a docker container.\
  In order for the app to work you should have python installed in your laptop or PC.
  
  Start with creating a new directory to keep all our development files and lets call it `python-docker`. This directory will have two files `app.py` (containing the source code of flask server) and `requirement.txt` (containing the python dependencies that we need to run the application).
  
  <details>
    <summary>app.py</summary>
    
    ```
  from flask import Flask
  app = Flask(__name__)
  
  @app.route('/')
  def hello_world():
      return 'Hello, Docker!'
    ```
  </details>
  
  
  <details>
    <summary>requirement.txt</summary>
    
    ```
  # This file is used by pip to install required python packages
  # Usage: pip install -r requirements.txt
  
  # Flask Framework
  Flask==1.0.2
    ```
  </details>
  
  To test the application issue the command: `$ python3 -m flask run`\
  And in your browser go to url `http://localhost:5000`\
  On successful execution of the above steps we should get a message log like: `127.0.0.1 - - [...] "GET / HTTP/1.1" 200` on the terminal.

### Create a docker image for the python application
  
  
## Step 2: Test the image inside a docker container

## Step 3: Add database to a docker container

## Step 4: TBD

## Step 5: TBD
