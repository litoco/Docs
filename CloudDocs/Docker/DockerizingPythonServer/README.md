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

  Now that we have our application running successfully we can go ahead and create a Dockerfile to package our server inside an image. 

  To create an `image` we need to create a new file inside current directory (python-docker directory) and call it `Dockerfile`. Following will be the content of this file:
  ```
FROM python

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

CMD ["python3", "-m" , "flask", "run", "--host=0.0.0.0"]
  ```
  * The first line `FROM python` will pull the latest python image (that has all the tools and packages needed to run a Python application) from the dockerhub
  * The second line `WORKDIR /app` tells docker to create a working directory inside the image that will be treated as the default directory
  * The third line `COPY requirements.txt requirements.txt` copies the mentioned file from local repository to the docker image
  * The fourth line `RUN pip3 install -r requirements.txt` run the package installtion commands in the image to install the packages mentioned in `requirements.txt` file
  * The fifth line `COPY . .` copies all content of current local directory to images `/app` directory. 
  
  ***It is interesting to observe that we first copied `requirement.txt` installed the packages mentioned in it and then copied the source code along with all the other files with current command. This is because docker caches the same steps. So since we would not change the `requirement.txt` contents with the same frequency as that of the source code we will not need to download the dependecies again and again. This will save us time in building images fast and also internet data.***

  * The last line of the dockerfile `CMD ["python3", "-m" , "flask", "run", "--host=0.0.0.0"]` tells docker to run the provided command when the image is in execution.\
  Note: *`--host=0.0.0.0`* helps in making the application visible from outside of the container in which it is executed.

  We now have our `Dockerfile` created. To create an image from this we use the following command (inside of the local machine terminal):\
  `docker build --tag python-docker .`\
  This command will take the Dockerfile from the current directory(specified by .) and build an image from it with name `python-docker`.
  
  We can view the images created by above command by (issuing the following command inside of the local machine terminal):\
  `docker images`
  
## Step 2: Test the image inside a docker container

## Step 3: Add database to a docker container

## Step 4: TBD

## Step 5: TBD
