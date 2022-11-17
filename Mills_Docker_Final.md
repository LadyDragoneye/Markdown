## Project 2 Markdown

Create a new VM with a Ubuntu image
Set Max disk size to 20GB
Confirm rest of the defaults

Login with chosen username and password

Open a terminal

Update Packages
```sudo apt update```

Update packages 
```sudo apt-get update```

Install needed packages
```sudo apt-get install ca-certificates curl gnupg lsb-release```

Make a directory to store the GPG key
```sudo mkdir -p /etc/apt/keyrings```

Add the key
```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg```

Set and update the Docker repo
```echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null```
```sudo apt-get update```

Install the last packages
```sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin```

Verify the installation process 
```sudo docker run hello-world```

*The output should look like this*
<img width="403" alt="image" src="https://user-images.githubusercontent.com/71207177/202337638-734fccee-9acd-4eeb-adf6-c617d68f57fe.png">



Run the get_docker.sh script
```curl -fsSL https://get.docker.com -o get-docker.sh```
```sudo sh get-docker.sh```

Downloading Docker Compose Manually
```DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}```
```mkdir -p $DOCKER_CONFIG/cli-plugins```
```curl -SL https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose```

Add execution permissions 
``chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose``

Test Docker Compose version
```docker compose version```

*Output should look like this:*
``Docker Compose version v2.12.2``

## Creating a docker-compose.yml code block

Create a Project Directory 
```mkdir composetest```
```cd composetest```

Import the following test code into a file named app.py

```
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```
Import the following text into a file named requirements.txt
```
flask
redis
```

Create a Dockerfile
```nano Dockerfile```
Paste this code:

```
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```

Finally, define the docker-compose.yml file and add the following text
```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

Start the application
```sudo docker compose up```

Open a browser in the VM and go to the following link
``http://localhost:8000/``

*This is what the page should look like*
<img width="960" alt="image" src="https://user-images.githubusercontent.com/71207177/202337475-5cd6220c-a0ac-4165-b292-5305b86e0d20.png">


Enter Ctrl+C to end the application


https://docs.docker.com/engine/install/ubuntu/#install-docker-engine
https://docs.docker.com/compose/install/linux/#install-the-plugin-manually
https://docs.docker.com/compose/gettingstarted/
