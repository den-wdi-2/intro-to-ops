# Introduction to Operations 

## Why is this important?

Troubleshooting in production often requires you to view logs on a remote machine. You may also be asked to 
make small "hotfixes" to a running machine to fix a critical bug.
 
## Objectives

*By the end of this lab, developers will be able to:*
* __Create__ an SSH key
* __Log in__ to a remote server 
* __Copy files__ to a remote server
* __Review__ a file using tail

## Introduction

Servers are just computers. Webservers are often Linux-based machines and once you have access you can run 
any of the commands you're used to running on your local machine. 

Once an app gets into production, you often need to find out something from the machine where it's running,
either looking at local logs or even making small changes and restarting the system.

## SSH 

The most common way to access a webserver is to use `ssh` or secure shell. `ssh` uses the same technology as 
SSL/HTTPS to enable you to access the server over an encrypted connection. 

Let's start with a quick hello world.  Open up your terminal and connect to my extra laptop with:

`ssh wdi-devs@10.62.107.93`

We will tell you the password out loud rather than put it in this repo.  (Security!  Keep that in mind for the future--never put passwords in a publicly accessible place.  Bad people (and robots) are everywhere.)

Type that into your terminal and...wait, did anything even change?  It sure did.  Notice that our Terminal is now logged into `Katherines-MacBook-Air`.  Poor Katherine, she's getting hacked.

Enter the `Documents` directory and leave her a friendly note.  Something like, 

`"Bwahahaha, I have access to your computer.  Consider yourself lucky I'm not Angelina Jolie from Hackers."`

Save this note into a file like `<your_name>.txt`, then make sure your file is there.  Watch as her folder blows up with all the h4x0rs in this class.  

Then `exit` to land safely back in your Terminal.

Want to set this up for your computer so you can log in from other machines on your network at work or home?  [Follow these directions](https://support.apple.com/kb/PH18726?locale=en_US).  Be very careful, though.  Whenever you open access to a machine, you should remember to follow these rules:

1. Have some sort of authentication.  Above, this is your username and password, but there are other methods (like the one below).
2. Be sure that all the people who could *potentially* reach your computer are trusted.  In the example above, we are trusting all the people on our WeWork network, but the more restricted you can be to specific addresses or networks, the better.

### ssh public keys

When possible, it is better to not share usernames or passwords at all.  One way to avoid that is to share public SSH keys that contain neither usernames or passwords in plain text.  Remember installFest?  [We totally did that](https://help.github.com/articles/generating-an-ssh-key/) during [installFest](https://github.com/den-wdi-2/installFest/blob/master/mac-dev-tools.md).

<!--Raise your hand if you realized that's what you were doing.  -->

Once you have a public key, you can log into a machine with the same format as above: 

```bash
ssh <user-name>@<host>
```

### ssh private keys

For AWS, we'll use something slightly different. AWS adds its own private key when a server is created. 
This private key is what we need to log into the server. To use the private key we'll need a slighlty different form:

```bash
ssh -i file.pem ec2-user@aws-host
```

The ``-i`` tells ssh to use the file listed after as the key.

For this lesson, we will be sharing a private key, for the sake of convenience.  Usually, you would get your own private key and **never share it**.  The instructor will share the key, called `WDI_Ops_Zeb.pem`.  Put it in your `~/.ssh` folder.

One thing that you'll need to do is to make sure that the key file is read only.

How do you change the file so that only you read the file MyCertificate.pem?
<details>
``chmod 400 MyCertificate.pem``
</details>

The `ec2-user` is, well, `ec2-user`.

The `aws-host` is `ec2-35-167-168-17.us-west-2.compute.amazonaws.com`.

>**Note:** Like all files in your Terminal, you need to use either absolute path (`~/.ssh/WDI_Ops_Zeb.pem`) or relative path (move to `~/.ssh` and use `WDI_Ops_Zeb.pem`) in the command above.

When prompted to connect, type `yes`.

__ssh gotcha__

ssh logs you in as a specifc user. Sometimes there's a single user for a server, sometimes you log in as 
yourself. This is often done for logging and permissions. In particular, you may only have read only 
permissions so you might get some new errors that say you can't run a command because you don't have permissions.

If you can't Google your way out of an issue like this, see if someone on your project can help you.

#### Independent practice
Log into the AWS instance the instructor posted in Slack, and find the ``carmen.sandiego`` file.  Once you find it, raise your hand, and an instructor will check your answer.

## SCP 
Since production servers are stripped down machines sometimes you need to transfer files. The easiest way to 
do this is to use ``scp``.

The basic format is:
```bash
scp <local_file(s)> <user>@<host>:<remote_local>
```

For our AWS server we'll need 
```bash
scp -i <certificate> <local_file(s)> <user>@<host>:<remote_local>
```

You can also reverse the system
```bash
scp -i <certificate> <user>@<host>:<remote_local> <local_file(s)> 
```

#### Independent practice
Create a directory with a name of your GitHub username. Inside that directory put a copy of your headshot. Copy this directory onto the AWS instance inside the `class` directory.

## Tail
When you're looking at production logs you can use the ``tail`` command.

#### Indpendent Practice 
Use tail on the log file in the `app` folder to find a message for GA students.

## Licensing
All content is licensed under a CC­BY­NC­SA 4.0 license.
All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.
