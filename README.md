### An opening statement
Having an arm64 arch on your vm can lead to several issues and difficulties while trying to run steamcmd or host steam based dedicated game server. 

Within this tutorial i condensed and used information from here:
* [How to install box86/64 on arm64](https://forum.armbian.com/topic/19526-how-to-install-box86-box64-wine32-wine64-winetricks-on-arm64/)
* [SteamCMD Wiki Page](https://developer.valvesoftware.com/wiki/SteamCMD)

Every guide for steamcmd on arm uses docker and i don't want that so after some reaserch i actually did not only install steamcmd, i created and successfully started project zomboid server for my friends.
## How to guide
For testing i used my own ARM64 Oracle Cloud Free Tier VM with Ubuntu 22.04 Jammy
### Installing box86/64
[Box86 Website](https://box86.org/)

For short **box86** allows us to run **x86 arch** apps on the other architectures such as **ARM** in our case

Firstly we need to install packages needed for box86 and add armhf to our system
```sh
sudo dpkg --add-architecture armhf
sudo apt-get update
```
```sh
sudo apt-get install git cmake cabextract gcc-arm-linux-gnueabihf libc6-dev-armhf-cross
```

To install both versions: 86 & 64 run those commands in order:
```sh
git clone https://github.com/ptitSeb/box86
cd box86
mkdir build
cd build
cmake .. -DARM_DYNAREC=ON -DCMAKE_BUILD_TYPE=RelWithDebInfo
make -j3
sudo make install
sudo systemctl restart systemd-binfmt
```
```sh
git clone https://github.com/ptitSeb/box64
cd box64
mkdir build
cd build 
cmake .. -DARM_DYNAREC=ON -DCMAKE_BUILD_TYPE=RelWithDebInfo
make -j3
sudo make install
sudo systemctl restart systemd-binfmt
```
This will clone repo > build app > install it to your system so you can use it, needed version of box will be used automaticlly when you will try to open x86/x64 app
### Installing SteamCMD & server files
Create and open folder in which steamcmd should be installed:
```sh
mkdir ~/steamcmd
cd ~/steamcmd
```
Next step is downloading & opening steamcmd, after this steamcmd will autoupdate itself
```sh
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -
./steamcmd.sh
```
After successfull installation type `quit` in the steam shell, after that you can finally install game files of the desired server
```sh
cd ~/steamcmd
./steamcmd.sh +force_install_dir <DIR> +@sSteamCmdForcePlatformType linux +login anonymous +app_update <APP_ID> validate +quit
```
Replace **DIR** and **APP_ID** with directory of your choice and steam app id of the game you want to create server of

### Further steps
Depending on what game you choosed refer to its wiki for more information such as:
* Firewall ports to open
* Server startup file

And so on...

Thank you for using this guide and see you later! 😊
