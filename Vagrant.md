# About the Vagrant build

## Description
Current the Vagrant file simply spins up an Ubuntu instance, deploys a dev build of gitpitch, and opens it up on local port 9000. Note that building your own gitpitch server is entirely optional. Gitpitch.com hosts this service [free for public repos](https://gitpitch.com/pricing).

_Disclaimer: This Vagrant project has been used to play around with gitpitch locally, not for active gitpitch development (it may work fine, I just haven't tried yet)._

## Instructions
You must first have Vagrant installed.  If you don't know what this is, [go find out](https://www.vagrantup.com), install it, and come back here when you are done.

Before proceeding, please read the last section on this page (About Oracle Java License).

This Vagrant script has been tested using a virtualbox provisioner but it specifies the _bento/ubuntu-16.04_ vm, which should work on most vagrant provisioners.

Steps:
1. Clone this project, navigate to it from the command line, and type: `vagrant up`. Then wait until it is done building (may take several minutes).
2. SSH into the gitpitch vm using: `vagrant ssh`
3. Once SSH'd in, navigate to the gitpitch project: `cd /opt/gitpitch`
4. Run the gitpitch project by typing: `/opt/sbt/bin/sbt run`

Now gitpitch should be running. You can minimize your command console window but don't close it.  You should be able to test it by visiting:
```
http://localhost:9000/gitpitch/kitchen-sink
```
Note that the first project you load in will be slow at first, as there are still some items that need to get compiled. Additional projects should load faster.  Refer to the [gitpitch wiki](https://github.com/gitpitch/gitpitch/wiki) for further instructions including creating your own slide shows.

## About Oracle Java 8 License
Gitpitch runs on Java 8, so the Vagrant script installs it on the virtual machine as part of the build process. Before running this project you must agree to the [Oracle Java 8 Licensing terms](http://www.oracle.com/technetwork/java/javase/overview/javase8speclicense-2158700.html).
