# Azure IoT Edge v2 (preview) snap

This snap was created due to the lack of options to install onto ubuntu core. This runtime has dependencies for python and docker. Python is packaged as a dependency inside of the snap but you will need to install the docker snap before hand.

## Getting Started

To get started, you will need to install the docker snap and then build this snap. On the target machine:</br>

<b>snap install docker</b>

To compile the snap on your build machine, navigate to the root folder and type: <b>snapcraft</b> and press enter. Once the build is completed, you will see a snap in the same directory called: <b>iotedgectl_preview_amd64.snap</b>

Thats it! Now the snap is ready to be published to your private snap repo or scp'd to another machine:

## Deployment

To ubuntu repo: </br></br>
<b>snapcraft login</b> </br>
<b>snapcraft register mysnapname</b> </br>
<b>snapcraft push --release=edge iotedgectl_preview_amd64*.snap</b> </br>

To another machine (in the same folder as the snap): </br></br>
<b>sudo scp -P 22 iotedgectl_preview_amd64.snap user@ipaddress:/home/userdirectory</b> </br>

### Prerequisites

On the taraget machine you will need to have docker installed by running this command in the terminal:

<b>sudo snap install docker</b>

<b>NOTE:</b>
Since snaps are read only, you will need to create a directory for the snap to write to. In the <b>.yaml</b> file we granted access to several interfaces:</br>
<b>plugs: [network, docker-executables, docker-cli, privileged, home]</b>

This will allow us to communicate with docker and also read/write in the users home directory. Great documentation here: https://docs.snapcraft.io/reference/interfaces

To create the necessary directory run the following commands in the terminal:</br></br>
<b>sudo su<b></br>
 <b>cd $HOME</b></br>
<b>mkdir iotedge</b></br>

### Installing and Running

Setup your device in the IoT Hub (documentation can be found here: https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux)

Navigate back to the directory that you scp'd the .snap to (/home/userdirectory) and install the snap:</br></br>
<b>snap install iotedgectl_preview_amd64.snap --devmode --dangerous</b></br>

Configure the runtime (note the directory parameters):</br></br>
<b>iotedgectl setup --connection-string "connstringhere" --auto-cert-gen-force-no-passwords --edge-config-dir $HOME/iotedge --edge-home-dir $HOME/iotedge</b></br></br>

<b>iotedgectl start</b>

and your up and running!

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
