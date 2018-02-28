# About the Vagrant build

## Purpose
The goal of this Vagrant file is to provide a solution whereby gitpitch presentations can be developed and tested without a public repo.

Current the Vagrant file simply spins up an Ubuntu instance, deploys a dev build of gitea and gitpitch with a custom app config. Note that building your own gitpitch server is entirely optional. Gitpitch.com hosts this service [free for public repos](https://gitpitch.com/pricing). The version of gitpitch running in the Vagrant box is configured to work with the local gitea instance and will not work with public repos without modifications... but you can use the [public-facing gitpitch.com](https://gitpitch.com) to do that.

_Disclaimer: This Vagrant project has been used to play around with gitpitch locally, not for active gitpitch development (it may work fine, I just haven't tried yet)._

## Instructions
You must first have Vagrant installed.  If you don't know what this is, [go find out](https://www.vagrantup.com), install it, and come back here when you are done.

Before proceeding, please read the last section on this page (About Oracle Java License).

This Vagrant script has been tested using a virtualbox provisioner but it specifies the _bento/ubuntu-16.04_ vm, which should work on most vagrant provisioners.

### Basic Steps:
1. Clone this project, navigate to it from the command line, and type: `vagrant up`. Then wait until it is done building (may take several minutes).
2. Setup gitea (http://localhost:3000). Just select SQLite DB and review the other options in case you want to change anything else.
3. Register a new gitea user and log in with it. Create a repo to test with and include a PITCHME.md file as per the gitpitch instructions.
4. Test gitpitch by visiting http://localhost:9000/YOUR_GITEA_USERNAME/YOUR_REPO. The first time you do this it may take a long time since some stuff needs to compile.

### Creating presentations
Refer to the [gitpitch wiki](https://github.com/gitpitch/gitpitch/wiki) for further instructions including creating your own slide shows.

### Mirroring from a Private Github Repo
You can mirror a private github repo into gitea. To do this, you will first need to create a personal access token in GitHub (Settings / Developer settings / Personal access tokens) and check the repo checkbox when you do. Keep the generated token handy until you create the mirror in gitea.

In gitea, press the Plus button and create a New Migration. Use your git repo clone URL (https works best here). Open up the Authorization section and supply your github username. For the password, use the access token you generated.  Check the Migration Type box. Don't make it a private repo or you won't be able to access it in gitpitch.

By default gitea will sync the repo every 8 hours. The shortest time you can set this to is 10 minutes but in most cases that is not practical either.  The best options is to manually sync at anytime from the gitea repo's Settings screen.

### Exporting from gitea
Remember gitea is a fully functional git repo.  The https urls can be used to `git clone` from you Vagrant host (i.e. your laptop) and then you can push to an alternate (e.g. github) upstream using the usual git commands for doing so.

_Note: Going in the other direction (from the Vagrant host to gitea on the Vagrant guest) is trickier but should be possible. A ssh config pointing to localhost port 2222 and an appropriate SSH key may work (has not been tested)._

### Restarting
The Vagrantfile starts up gitea and gitpitch in screen sessions, both running as root (yes, I know this is not ideal but as we are binding to localhost from the Vagrant host side, the exposure is minimal).  If you need to restart Vagrant for any reason, you will need to restart the gitea and gitpitch processes.  The easiest way to do this is to `vagrant ssh` and then copy/paste the following commands:

```
cd /opt/gitea
sudo screen -dm -S gitea ./gitea web
cd /opt/gitpitch
sudo screen -dm -S gitpitch /opt/sbt/bin/sbt run
 ```


## About Oracle Java 8 License
Gitpitch runs on Java 8, so the Vagrant script installs it on the virtual machine as part of the build process. Before running this project you must agree to the [Oracle Java 8 Licensing terms](http://www.oracle.com/technetwork/java/javase/overview/javase8speclicense-2158700.html).
