This is mostly taken from [Shogan's blog](https://www.shogan.co.uk/how-tos/wsl2-gui-x-server-using-vcxsrv/) which also covers PulseAudio.

## Prerequisites
- You'll need admin privileges in Windows to install a software.
- Around 1.5 GB of data will be downloaded so be mindful if you are on a limited data connection. 

## Steps
- Firstly, update your packages using `sudo apt update` and `sudo apt dist-upgrade`. 
- On Ubuntu shell install Xfce using `sudo apt install xfce4 xfce4-goodies`. On Kali linux it can be installed using `sudo apt install kali-desktop-xfce`.
- Xfce would be paused in mid-install. You’ll be prompted about which display manager to use. This is up to you, I chose lightdm.
- While Xfce is installing, download and install VcXsrv Windows X Server either [from Sourceforge](https://sourceforge.net/projects/vcxsrv/) or [from here](https://www.shogan.co.uk/wp-content/uploads/WSL-VcXsrv.zip). I had to use the latter becaue download speed from the former was slow for me. If you are using the latter the installer is named `vcxsrv-64.1.20.8.1.installer.exe`, rest of the files are unnecessary for us.
- Once VcXsrv is installed go to its installed location (which most probably is `C:\Program Files\VcXsrv`), right-click xlaunch.exe and go to Compatibility, click on Change high DPI settings and choose Override high DPI scaling behavior. Ensure Application is in the dropdown. 
- Open Windows Defender Firewall with Advanced Security and add an inbound rule for VcXsrv with the following properties:
  - Type: Program
  - Program path: %ProgramFiles%\VcXsrv\vcxsrv.exe
  - Allow the connection
  - Profile: Domain, Private
  - Name it whatever you like
- Once everything is installed, open a bash shell (bash, fish, doesn't matter) and type `export DISPLAY=:0` and `export LIBGL_ALWAYS_INDIRECT=1`. Now open VcXsrv (named as XLaunch) on Windows, choose Display number as 0, click Next, keep Start no client as selected, click Next, make sure Native opengl is ticked, click Next, and then on Finish. Its icon will now appear on sytem tray.
- We have specified options with same values on both OS in previous step. This is needed.
- Now you can open aopen any program to verify it. For example, I can find correct location of Sublime Merge using `which -a smerge` and launch it.
- Onivim 2, on the other hand opens but shows black screen. To fix that, open system tray on Windows, right click on VcXsrv icon and close it. Open a bash shell and enter `export LIBGL_ALWAYS_INDIRECT=0` and Launch VcXsrv again but this time on third screen untick Native opengl. Try running the app again.

## Notes
- There is this [other method on the same lines](https://skeptric.com/wsl2-xserver/). 
- There is a good [explanation on `LIBGL_ALWAYS_INDIRECT` var on StackOverflow](https://superuser.com/a/1487558).
- For that black screen fix, they suggest installing `mesa-utils` on Ubuntu but it worked for me without it. See the [related GitHub issue](https://github.com/microsoft/WSL/issues/2855). There is [another issue](https://github.com/microsoft/WSL/issues/3757) on the same.
- There is also a [post from Microsoft on GUI/WSL2](https://techcommunity.microsoft.com/t5/windows-dev-appconsult/running-wsl-gui-apps-on-windows-10/ba-p/1493242).