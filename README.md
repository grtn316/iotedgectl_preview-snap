# Azure IoT Edge v2 (preview) snap

This snap was created due to the lack of options to install onto ubuntu core. This runtime has dependencies for python and docker. Python is packaged as a dependency inside of the snap but you will need to install the docker snap before hand.

## Getting Started

To get started, you will need to install the docker snap and then build this snap. On the target machine:

snap install docker

To compile the snap on your build machine, navigate to the root folder and type: <b>snapcraft</b> and press enter. Once the build is completed, you will see a snap in the same directory called: <b>iotedgectl_preview_amd64.snap</b>

Thats it! Now the snap is ready to be published to your private snap repo or scp'd to another machine:

## Deployment

To ubuntu repo:
snapcraft login
snapcraft register mypythonsnap
snapcraft push --release=edge iotedgectl_preview_amd64*.snap

To another machine (in the same folder as the snap):
sudo scp -P 22 iotedgectl_preview_amd64.snap user@ipaddress:/home/userdirectory

### Prerequisites

On the taraget machine you will need to have docker installed by running this command in the terminal:

sudo snap install docker

<b>NOTE</b>
Since snaps are read only, you will need to create a directory for the snap to write to. In the .yaml file we granted access to several interfaces:
<b>plugs: [network, docker-executables, docker-cli, privileged, home]</b>

This will allow us to communicate with docker and also read/write in the users home directory. Great documentation here: https://docs.snapcraft.io/reference/interfaces

To create the necessary directory run the following commands in the terminal:
sudo cd $HOME
mkdir iotedge

### Installing and Running

Navigate back to the directory that you scp'd the .snap to (/home/userdirectory) and install the snap:
snap install iotedgectl_preview_amd64.snap --devmode --dangerous

Configure the runtime (note the directory parameters):
iotedgectl setup --connection-string "connstringhere" --auto-cert-gen-force-no-passwords --edge-config-dir $HOME/iotedge --edge-home-dir $HOME/iotedge

iotedgectl start

and your up and running!

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
