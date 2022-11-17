## Project 2 Markdown

Create a new VM with a Ubuntu image
Set Max disk size to 20GB
Confirm rest of the defaults

Login with chosen username and password

Open a terminal

Download Docker
'''sudo apt update'''
'''sudo apt install docker.io'''

Update packages 
'''sudo apt-get update'''

Install needed packages
''sudo apt-get install ca-certificates curl gnupg lsb-release
'''

Make a directory to store the GPG key
'''sudo mkdir -p /etc/apt/keyrings'''

Add the key
'''curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
'''

Set and update the Docker repo
'''echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
'''
'''sudo apt-get update'''

Install the last packages
'''sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
'''

Verify the installation process 
'''sudo docker run hello-world'''

*The output should look like this*

