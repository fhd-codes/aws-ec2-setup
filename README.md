# Create an AWS-EC2 instance and install Node.js and Python
It is assumed that you have properly signed up to an AWS account and you are ready to use it.

> The purpose of this file is to consolidate all the resources that are needed while working with AWS EC2 instance. Initial reference taken from [A Monk in Cloud](https://www.youtube.com/@amonkincloud). While difficulties while installation has also been mentioned with its solution. _Online resources have been referenced also_

## Creating an EC2 instance
1- Signup for an Amazon AWS account

2- Create an EC2 instance. Ref: [link_1](https://www.youtube.com/watch?v=BOIBZURNEIg)

3- Connect to that EC2 instance. Ref: [link_2](https://www.youtube.com/watch?v=GnRl6ETwHVE)

*Now you are inside the EC2 instance (server computer)*

## Installing Node in the EC2 instance
1- To become a root user, type this in the EC2 terminal
```
$ sudo su -
```

2- Install nvm and activate it
```
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
$ . ~/.nvm/nvm.sh
$ nvm --version
```
> If you see the version number, it means that nvm has been installed.

3- Install node and check its version
```
$ nvm install node
$ node --version
```
> If you get a node version, it means that node has successfully been installed.


If you get an error after running **node --version** command, it means node has not been installed. 

Write this line in the terminal (if it is not installed with the above steps)

```
$ nvm install 16
```
> This command is installing node version 16x as Amazon Linux 2 doesnot supports current LTS release of Node.js. 
[Reference](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html#:~:text=of%20Node.js.-,Warning,-Amazon%20Linux%202)

> Write *_node --version_* and you will get the node version that is installed in your EC2 instance.

If you get a message that "node: /lib64/libm.so.6: version `GLIBC_2.27' not found (required by node)", run the following commands

```
$ sudo apt-get remove nodejs
$ nvm install 16.15.1
```

> *Note* that this will only install node in the current session of EC2. If you close the terminal and run again, you have to install node again.
> To install it permanantely, you have to create *Amazon Machine Image (AMI)* from that instance.


## Installing Python and PIP on the EC2 intance

Check if the python is currently installed or not?
```
$ python --version
```
If python 2.7 or later version is not installed, use this command
```
$ sudo yum install python37
```
> Check python version again. Probably, the python version will still be 2.7x. This is because the default python version is set to 2.7x
Now we need to update the default python version

### Installing Python3.x from *amazon-linux-extras repository*

1- Write the followwing lines in the terminal
```
$ sudo yum install -y amazon-linux-extras
$ which amazon-linux-extras
$ amazon-linux-extras | grep -i python
```

> For the next commands, make sure that you select the Python version that you get after running the above line
2- To enable the latest available python version
```
$ sudo amazon-linux-extras enable python3.8
$ sudo yum install python3.8
```

### Setting newly installed Python as default

```
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8
```

If priority is required use:
```
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1
```

Confirm the new setting.
```
$ sudo update-alternatives --list | grep python
$ python --version
```
> Now you have set the Python 3.x version as default

[Reference](https://techviewleo.com/how-to-install-python-on-amazon-linux/)


## Installing PIP and EB CLI


1- Download the installation script
```
$ curl -O https://bootstrap.pypa.io/get-pip.py
```

2- Run the script with Python.
```
$ python3 get-pip.py --user
```

3- Add the executable path, ~/.local/bin, to your PATH variable.

```
$ export PATH=~/.local/bin:$PATH
$ source ~/.bash_profile
```

4- Verify the pip installation
```
$ pip version
```

5- Use pip to install the EB CLI.
```
$ pip install awsebcli --upgrade --user
$ eb --version
```

[Reference](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install-linux.html)


> Now you are all set. You have made an AWS EC2 instance and successfully installed Node.js and Python in it. Now you can deploy Node and Python apps on this server.

## Creating Virtual Environments for Python Project

Head to your project directory and write these lines to make and activate venv 
```
$ python3 -m venv env
$ source ./env/bin/activate
```
> if you see (env) at the extreme left side of your terminal line, your venv has been activated in this project

## Solution of some errors/

### Error while using "yum"
> It is because yum still uses Python2x version. But in the above steps, we have updated the default python to Python3x version. So we have to specify the Python version in yum files

If you get Keyboard Interrupt error while using "yum" command, do [this](https://itecnote.com/tecnote/python-yum-crashed-with-keyboard-interrupt-error/)

If you get an error while installing a package from "yum", do [this](https://unix.stackexchange.com/questions/524552/upgraded-python-and-now-i-cant-run-yum-upgrade)



## Installing Git and adding SSH key

1- Write these commands in the terminal to install Git and check the version
```
$ sudo yum install git
$ git --version
```
> If you see a version number, it means that git has successfully been installed.



2- Go to the Github [documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys) and follow the instructions step-by-step.
> Make sure that you install rsa and use based SSH key as it is supported by Github.

## Installing opencv-python

For installing opencv in the desktop environment, we use "__pip install opencv-python__", but this does not work on the server environment.
For installing opencv on server, use the following command:
```
$ pip install opencv-python-headless 
```



*JazakAllah!*










