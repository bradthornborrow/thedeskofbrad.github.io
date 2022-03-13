+++
title = "Building i3-gaps and i3status from source"
date = 2022-01-30
tags = [ "linux", "raspbian", "i3" ]
+++

Recently, I have become a great fan of __i3-gaps__ as my Window Manager of choice when using Raspbian. Unfortunately, the Raspbian images do not include __i3-gaps__ nor the latest versions of some other i3 utilities.  

To work around this issue, the packages must be built from source, as shown below (pulled together from several other posts found elsewhere and my own troubleshooting).

1. Install __i3-gaps__ build dependencies:  

	```
	sudo apt install meson dh-autoreconf libxcb-keysyms1-dev libpango1.0-dev libxcb-util0-dev xcb libxcb1-dev libxcb-icccm4-dev libyajl-dev libev-dev libxcb-xkb-dev libxcb-cursor-dev libxkbcommon-dev libxcb-xinerama0-dev libxkbcommon-x11-dev libstartup-notification0-dev libxcb-randr0-dev libxcb-xrm0 libxcb-xrm-dev libxcb-shape0 libxcb-shape0-dev
	```
	
2. Download __i3-gaps__ from the __Regolith-Linux__ repository, build and install in `/usr/local`. This is not the primary repo for __i3-gaps__, but it is being actively maintained, so I use it for my builds:  

	```
	git clone https://github.com/regolith-linux/i3-gaps-wm.git
	cd i3-gaps
	mkdir -p build && cd build
	meson --prefix /usr/local
	ninja
	sudo ninja install
	```
	
3. Remove the original i3 installation package:  

	`sudo apt remove i3-wm`
	
4. If using the __LightDM__ session manager, edit `/etc/lightdm/lightdm.conf`, adding `/usr/local/share/xsessions` to the session-directory search path.  

5. Logout and login to start using __i3-gaps__.  

### Build updated i3status from source (optional)

Older versions of the __i3status__ tool can encounter issues when running on __Raspbian__. For example, it can display incorrect memory values. If this occurs, the latest version must be built from source to replace the default __Raspbian__ package.

1. Install __i3status__ build dependencies:  

	```
	sudo apt install libconfuse-dev libyajl-dev libasound2-dev libiw-dev libpulse-dev libnl-genl-3-dev
	```
	
2. Download __i3status__ repository, build and install in `/usr/local`:  

	```
	git clone https://github.com/i3/i3status.git
	cd i3status
	mkdir -p build && cd build
	meson --prefix /usr/local
	ninja
	sudo ninja install
	```
	
3. Remove original __i3status__ installation package:  

	`sudo apt remove i3status`  


### Fix i3 / i3status package dependencies  

1. Lastly, after removing the default i3-wm and/or i3status packages, some dependencies may be broken.  

2. If any i3 dependencies are listed as __no longer required__ when running `sudo apt upgrade`, use this command to mark them as manually installed to avoid removing them in error:  

	```
	sudo apt-mark manual <package_names>
	``` 
	
I drafted this post primarily for my own reference but if you found it helpful, you’re welcome ツ. To contact me, please use the [Contact](/contact) page, or send me a direct message on [Twitter](https://twitter.com/TheDeskofBrad).  

Take care.  
