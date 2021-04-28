---

title: Working on Remote Server
linktitle: Working on Remote Server
toc: true
date: "2021-01-01"
draft: false
type: post

tags:
 - tutorial
 - economics
 - programming

---



In this page I will share tips on how to set up a remote server and deploy your code there.



## Setup

In order to start working on a remote server you need

- the server
- local shell
- SSH installed

SSH, or Secure Shell, is a protocol designed to transfer data between a client and a server (two computers basically) over an untrusted network.

The way SSH works is it encrypts the connection using a pair of keys and the server, which is the computer you would connect to, is usually waiting for an SSH connection on Port 22.

SSH is normally installed by default. To check if you have SSH installed, open the terminal and write `ssh`. You should receive a message that looks like this

```shell
usage: ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]
[-D [bind_address:]port] [-E log_file] [-e escape_char]
[-F configfile] [-I pkcs11] [-i identity_file]
[-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address] [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
[user@]hostname [command]
```

If SSH is not installed, you can install it using the following commands.

```shell
sudo apt-get install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

Now that you have installed SSH, we are ready to setup a remote connection.

From the computer you want to access remotey, generate the public key.

```shell
ssh-keygen -t rsa
```

You will be asked for a location. If you decide to enter one manually then that will be the pair’s location, if you leave the default one it will be inside the `.ssh` hidden folder in your home directory.

Now you will be prompted for a password. If you enter one you will be asked for it every time you use the key, this works for added security. If you don’t want a password just press enter and continue without one.

Two files were created. One file ends with the ‘.pub’ extension and the other one doesn’t. The file that ends with ‘.pub’ is your public key. This key needs to be in the computer you want to connect to (the server) inside a file called `authorized_keys` . You can accomplish this with the following command:

```shell
ssh-copy-id username@ip
```

For example in my case to send the key to my computer it would be:

```shell
ssh-copy-id sergiop@132.132.132.132
```

If you have MacOS there’s a chance you don’t have ssh-copy-id installed, in that case you can install it using

```shell
brew install ssh-copy-id
```

If you haven't installed `brew`, you can install it by following [this guide](https://brew.sh/).



## Connect

To permanently add the SSH key, you can use the follwing command

```shell
ssh-add directory\key.pem
```

Lastly, to connect, just type the following command.

```
ssh username@ip
```

Where `username` is the server name and `ip` is the public IP adress, e.g. 132.132.132.132.

If your server is not public, you will not be able to access it. 

If your server is password protected, you will be prompted to insert a password when you connect. If not, you should protect it with a password.



## Managing screens

While you are connected to the remote terminal, any disturbance to your connection will interrupt the code. In order to avoid that, you want to create separate screens. This will allow your code to run remotely undisturbed, irrespectively of your connection.

First, you need to install `screen`.

```shell
brew install screen
```

To create a new screen, just type

```shell
screen
```

Now you can lunch your code. 

After that, you want to detach from that screen so that the code can run remotely undisturbed.

```
screen -d
```

Another option is to use `ctrl+a` followed by `ctrl+d`. This will detach the screen without the need to type anythin in the terminal, in case the terminal is busy (most likely).

To list the current active screens type

```
screen -ls
```

If you want to check at any time that your code is running, without re-attaching to the screen, you can just type

```shell
top
```

which is the general command to check active processes. To exit, use `ctrl+z`, which generally terminates processes in the terminal.

To reattach to your screen, type

```
screen -r
```

In case you have multiple screens (you can check with `screen -ls`), you can reattach to a specific one by typing

```shell
screen -r 12345
```

where `12345` is the id of the screen.

To kill a screen, type

```
screen -XS 12345 quit
```

where again `12345` is the id of the screen.



## Bonus: coding in Python

If you are coding in Python, [PyCharm](https://www.jetbrains.com/pycharm/) is one of the best IDEs. Among many features, it offers the possibility to set a remote compiler for your pthon console and to sync input and output files automatically.

First, you need to have setup a remote SSH connection following the steps above. Importantly, you need to have added the public key to your machine using the `ssh-add` command, as explained above.

Then open Pytharm and go to `interpreter settings`.

Click `add` and select `SSH interpreter`.

Insert the server `host` (IP address, e.g. 132.132.132.132) and `username`.

Next, select the remote interpreter. If you are using a python version taht is not default, browse to the preferred python installation folder. Also, check the box for `execute code giving this interpreter with root privileges via sudo`. You can also select which remote folder to sync with your local project. I recommend also to have the last option checked: `automatically sync project files to the server`.





## Sources

- [How To Setup And Use SSH For Remote Connections](https://medium.com/@SergioPietri/how-to-setup-and-use-ssh-for-remote-connections-e86556d804dd)