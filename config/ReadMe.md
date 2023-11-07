## Config files

### config.buildinfo
If you wish to compile OpenWrt yourself you can use the config.buildinfo file to create the configuration used for this release.
Copy it into your clone of OpenWrt source code, renaming it to `.config` and then run `make difconfig` to expand it to a full config file.
You can then make the project immediately or use `make menuconfig` to look at and change the configuration.

### distfeeds.conf
distfeeds.conf contains the package feed urls used by the install script
