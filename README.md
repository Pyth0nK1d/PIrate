
# PIrate, the automatic auditor
<p align="center">
  <img height=150 src="/images/PIrate-icon-nobackground.png" />
</p>

# Table of contents
1. [Introduction](#introduction)
2. [Legal disclaimer](<#legal-disclaimer>)
3. [What is this tool about?](<#what-is-this-tool-about>)
4. [What is an "action"?](<#what-is-an-action>)<br>
	4.1. [What is a “startup action”?](<#what-is-a-startup-action>)<br>
	4.2. [What is a “recurring action”?](<#what-is-a-recurring-action>)<br>
	4.3. [What is an “exception action”?](<#what-is-an-exception-action>)<br>
5. [Available options](<#available-options>)<br>
	5.1. [Add or remove an action](<#add-or-remove-an-action>)<br>
	5.2. [Show configured actions by type](<#Show-configured-actions-by-type>)<br>
	5.3. [Export or import all actions](<#Export-or-import-all-actions>)<br>
7. [Installation](<#installation>)
8. [Configuration](<#configuration>)
9. [Enter Raspberry Pi](<#enter-raspberry-pi>)
10. [Recommendations and tips](<#recommendations-and-tips>)
11. [Future of this tool](<#Future-of-this-tool>)



# Introduction

<strong><i>This is Redface.<br><br>
Born to be the captain of an entire fleet, <br>
commanding his tripulation and logging his misdeeds. <br>
A reliable corsair that will fit your needs <br>
across your govern to rule the seven seas.
</i></strong>


This little poem is a great summary about what this tool is about. On the following sections you will discover the why, what and how's of this tool.

So, please, read the documentation thoroughly if you want to fully understand its purpose and resolve any technical doubts that may arise.

# Legal disclaimer

Note that this tool may use offensive tools that can potentially harm targeted systems if misused. For that means, **you (the end user of this tool)** are solely responsible for any damage to targeted systems.

Once clarified, as a recommendation: **Always ensure you target the systems you own or that you have permission by the actual owner.**

# What is this tool about?

This tool consists on providing companies and/or individuals with technical knowledge of a system that will be responsible for carrying out an "organized and limited pentest" via checks configured and/or programmed by the user (aka "actions") to detect possible vulnerabilities or misconfigurations generated over time foreseeable by the user itself.

Originally, this tool was intended to be installed and used on a system like a Raspberry Pi only. But, due to using a Raspberry Pi may be a hard requirement and not everyone may be forced to use this kind of hardware (even that it is very handy sometimes), its developed on terms of flexibility, so the user can install this tool in any Linux environment that requires it, with as minimal dependencies as possible.


# What is an "action"?

Within this tool, the main elements that are responsible for performing the corresponding operations are called "actions". These "actions" **consist of executable elements (such as a command or a script) which will be executed by the tool itself**. In addition, they will consequently generate a log of the result obtained by the execution with its corresponding "timestamp". In this way, it is possible to keep track of the executions and their corresponding results in an orderly manner and divided by the different "actions" defined in the tool. These "actions" are divided into three distinct types: **startup action, recurring action and exception action**.
## What is a “startup action”?

A "startup action" consists of **an "action" configured to run at system startup**. In this case, the "action" is configured in the system as a cron of type "@reboot" in order to ensure that this occurs. Here's an outline of how a "startup action" is set up to understand how it works in a more visual way.

<p align="center">
  <img src="/images/Startup-Action.png" />
</p>

## What is a “recurring action”?

A "recurring action" consists of **an "action" with a similar function to the "startup action" but with the flexibility to run in an interval defined at the time of its configuration**. Below is an outline of how a "recurring action" is configured to understand how it works in a more visual way.

<p align="center">
  <img src="/images/Recurring-Action.png" />
</p>

## What is an “exception action”?

An "exception action" consists of **an "action" configured for the user to execute it manually**. This type of action is made for **cases in which the user must have control of the number of executions** of a particular command or script and that the logging of these actions is effective, since it should not be executed too many times (for example, to **avoid jeopardizing the availability of a particular asset**, **the use of an "intrusive" command if used too often**...). Here's an outline of how an "exception action" is set up to understand how it works in a more visual way.

<p align="center">
  <img src="/images/Exception-Action.png" />
</p>

# Available options

Within the tool there some available options with which the user can manage the actions. This section indicates what these options are about.

## Add or remove an action

Actions can be added or deleted through the tool. When creating an action, you must select the type of action you want to create (**"startup", "recurring" or "exception"**). Then, depending on the type of action that is defined, some extra information is requested to properly formalize and record the action that is to be created. All actions, regardless of their type, are recorded in a SQLite database in order to keep track of them and be able to manage them more easily. The information requested is the following:

- **The alias of the action (mandatory)**: It consists of a name assigned by the user that identifies the action to be executed. Therefore, **the alias entered must be unique**. In case of entering an existing one, it will request it again.

- **The action command (mandatory)**: This is the command that this action will execute. It is divided into two subtypes:

	- **Single command**: Any instruction that a user could execute on console (including instructions that take advantage of previously defined environment variables or previously defined executables in the **$PATH** variable).

	- **Scripts**: Any file with a series of previously defined instructions and grouped in a logical and structured way (such as, for example, a ".sh" file, a ".py" file with the "python3" instruction in front of it, any file with the corresponding "shebang" defined in the first line, etc...). This way, you will be able to execute any fancy tool, just make sure it is in non-interactive mode.

- **The execution interval (required, only for recurring actions)**: This is the interactive definition of which interval the action will be executed. In the case of a startup action, the interval is set to "@reboot" by default and is not prompted to the user. The definition of this field is accompanied by a creation of a "cron" that is automatically enabled once the action has been created.

- **The description of the action (optional)**: It consists of giving the user a section to give more detail about the action to be performed, in the event that the alias is not going to be sufficiently descriptive, to be able to quickly know what the action consists of without having to interpret the operation of the command/script defined for it.

On the other hand, to delete a particular action, you must select the type of action to be deleted (**"startup", "recurring" or "exception"**). Next, select the alias of the action to be deleted from the list of available actions. Once this is done, the application will ask if you want to delete the logs generated by this action or not. **In the case of indicating that they want to keep them, the application will delete the action, but the logs will remain intact in the folder where they were generated**. Otherwise, they will be deleted at the same time as the action. Any action that has a "cron" defined will be deleted after the action is deleted, the same happening with the database log.

## Show configured actions by type

Within the main options, there is the possibility to show which actions are configured divided by the existing types:

- Startup actions
- Recurring actions
- Exception actions

Through these, you will be able to check the details of the actions created for each of the types, without having to interact with them.

## Export or import all actions

In the case of wanting to manage several actions at the same time, there are two other options within the application: export and import. 

The export option allows you to make a copy of the current state of the application (similar to a snapshot), which will obtain the status of the actions and the corresponding logs at the time of making the copy. This option makes a full copy of the following:

- The SQLite database in its current state
- The cronjobs configured at that time
- The application's existing logs (both of existing and maintained actions).

This option will generate a ZIP file with all this content and which can be transferred to another system in which you want to import this same state. 

On the other hand, the import option is a DESTRUCTIVE option. When importing, a compatible ZIP file (previously obtained from an export operation) is requested. Before requesting the archive, it will repeatedly warn that any information previously entered in the tool will be overwritten by the content of the ZIP provided. Therefore, if it is necessary to make a copy of it, it is advisable to do so before proceeding using the export option previously described.

# Installation

How to install the tool

IMPORTANT NOTE: **The tool is intended to be installed, configured and used as the "root" user.** This may change on a future update, but in the meantime is the convention to ensure every action defined gets executed properly. 

Tested environments: Kali Linux (Rolling 2024.2), Kali Linux (Rolling 2024.2 for Raspberry Pi)

### Using a DEB file

It is provided an already packaged version of the tool on a DEB file, which can be found and downloaded from the "Releases" section.

To install the tool using the downloaded DEB file just run the following:

```
dpkg -i PIrate.deb
```


### I don't trust the DEB file. How to create my own?

Easy. Clone this repo and access its root folder (the location where it is cloned). There you should find all the repo content, including the "PIrate" folder which is in fact the contents of the DEB file (**source code of the tool and main package information**). Then, run the following command to create your own DEB file:

```
dpkg-deb --root-owner-group --build PIrate
```

This will generate the file "PIrate.deb", which can be installed with the following command:

```
dpkg -i PIrate.deb
```

### I'm a Windows user, what now?

In case of running it on a Windows system directly (not tested), just get a way of running Bash. You can set WSL to achive this: https://learn.microsoft.com/en-us/windows/wsl/install.

Another alternative way for Windows is setting any Linux Distro VM to install the application and run it from there. You will be required to set the proper network configuration to make it visible on the network segment you require to run it. Once you have it, go to sections **Installation** and **Configuration** in order to getting it set up.

# Configuration

The configuration to make it work.
### Dependencies

Make sure the system has at least the following dependencies:

- SQLite 3
- zip/unzip
- Swaks (not hard requirement, it will depend on what you will be using. Required if you want to run the examples included whithout not so much customization.)

### Path fixes

The tool won't run if a path fixing is not done previously. This is because the dependencies and the main file of the tool are stored inside a subdirectory. This means that it is required to add this subdirectory to the path to make it work.

First you will have to identify what is the default shell used by "root" user. For that, you can just open a root shell and type:

```
echo $SHELL
```

Depending on the shell configured by default, you will have to edit the corresponding RC file. This is to make persistent the modifications made.

```
BASH -> /root/.bashrc
ZSH -> /root/.zshrc
and so on...
```

Then, modify the corresponding file and add the following at the end of it:

```
# Added to get access to PIrate app
export PATH=/bin/PIrate:$PATH
```

Once you restart the shell, you may find that $PATH variable is set properly:

```
echo $PATH
```

So now you may be able to run the tool just calling it.

```
PIrate
```


<p align="center">
  <img src="/images/PIrate-menu.png" />
</p>

### Edit configuration file

The default values on the configuration file (**/etc/PIrate/PIrate.conf**) will make work the basic operations of the tool to make it run properly, so my recommendation is to let them configured this way (at least the ones related to locations and paths).

There is a section that you may modify before going on with its use. This section is the SMTP related information, set for the use cases provided to be able to send notifications to the administrator (make sure to configure your real SMTP server or any testing one to check it is working first). Just open the file and edit them with the proper information if you want to run the example use cases provided.

```
...
### Email configuration
# Name or IP of SMTP server
MAIL_SERVER=<SEVER_IP_HERE>
# Port of SMTP server
MAIL_PORT=<SERVICE_PORT_HERE>
# Email authentication username
MAIL_USERNAME=<USERNAME_FOR_AUTH_HERE>
# Email authentication password
MAIL_PASSWORD=<PASSWORD_FOR_AUTH_HERE>
# FROM Email
MAIL_FROM=<EMAIL_FROM_HERE>
# TO Email
MAIL_TO=<EMAIL_TO_HERE>
...
```

If you change the configuration when the tool is running, make sure to exit and run it back again so it loads the up-to-date changes.
### Configuration of my own scripts

There is a folder with the use case scripts I talked about earlier (**/bin/PIrate/scripts**). This folder serves two purposes:
1. Keep everything organized
2. Ensure the resources are accesible **only for "root"**

When creating an action, if you desire using your own tool (an script for example), it is not requried to include it on that folder. But, ensure you know its absolute path, or its relative path if is accessible via $PATH.

Also, **if you don't include this script to the folder**, at least make sure it has proper permissions before going on. Execute the following commands to do so:

```
chmod 700 /my/fancy/script
chown root:root /my/fancy/script
```

As a general recommendation on this point: If the script executed is not crafted by you, please understand what it does before running it, as you are granting that script root privileges.

### Email templates

Under folder **/bin/PIrate/templates**, you may found the email templates used by the scripts provided in **/bin/PIrate/scripts**. You may use this templates to adapt them to your needs or create new ones. 

This will depend on your needs (as the functionality of sending an email is managed by the script you craft, as you can see on the examples provided), but my recommendation is to store any newly crafted templates on that folder, to fit the two purposes specified on the previous section.
# Enter Raspberry Pi

Ok, at this point you might be thinking, where is the sweet stuff? How make this work on a Raspberry Pi?

Well, the very first idea I had was to provide you with an ARM image of Kali Linux with the tool already installed so you can go and try it directly. But, I encountered that even as a new release, GitHub has capped the file uploads to 2GB, making me unable to upload this version here.
<p align="center">
  <img src="/images/error-space-limit-github.png" />
</p>

Some things I tried so far:

- Try to compress the image in other ways, with different tools. Sadly, it will result on a file not less than 2.8 GB.
- I checked the option to use GitHub LFS, but it is an expensive option to host a single file there.

So, sadly, I will not be able to provide this image on a burn-and-go basis :(

Anyways, there exists options to make you work with it in a no direct manner.

### Manual setup

This is the easy one, which in plain terms is to transfer the "PIrate.deb" file to your device (a Raspberry Pi which may have already all the configuration and tools required on it) and go through sections "**Installation**" and "**Configuration**" to finish setting it up. In this case, if you require wireless connection, check "**WiFi setup**" subsection below before proceeding.

As the case I tested, you may get a Kali Linux ARM image compatible for Raspberry Pi in the following link: https://www.kali.org/get-kali/#kali-arm

### Automatic setup

This section is for folks that may require to set this on a larger scale, requiring also a bit of customization inside the image, and of course compatible with Raspberry Pi. Next will be established the process to compile a custom ARM image of Kali Linux by yourself.

First it is required to establish an environment where it is possible to compile this type of image, as indicated on the [official Kali Linux page](https://www.kali.org/docs/development/custom-raspberry-pi-image/). **The "root" user is used to perform this procedure, as it is a requirement indicated by the page itself in a note** (extracted from kali.org).

<p align="center">
  <img src="/images/ARM-01-root-required-for-installation.png" />
</p>

To make the process as smooth and functional as possible, follow the advice given by the [documentation](https://www.kali.org/docs/development/kali-linux-arm-chroot/) and set up a pre-generated Kali Linux virtual machine to prepare the environment. To do this, you can download and start one from the [official download page](https://www.kali.org/get-kali/#kali-virtual-machines).

<p align="center">
  <img src="/images/ARM-02-Kali-Linux-required.png" />
</p>

Since it indicates that the machine must be updated, this is done with the commands "**apt update**" and "**apt upgrade**". The version used is the latest version existing at the time of writing this instructions. In this case, **Kali Rolling 2024.2**.

```
apt update && apt upgrade
```

At this point, you may proceed to follow the [instructions](https://www.kali.org/docs/development/arm-cross-compilation-environment/) in order to install the necessary dependencies to be able to cross-compile ARM on this machine. It is an **essential requirement** to be able to generate your own and compatible images for Raspberry Pi.

When attempting to perform the installation of dependencies required to complete the first step, it results in one of the dependencies failing.

```
apt install -y git-core gnupg flex bison gperf libesd0-dev build-essential zip curl libncurses5-dev zlib1g-dev gcc-multilib g++-multilib

...

Error: Unable to locate package libesd0-dev
```

After some [research](https://askubuntu.com/questions/1082722/unable-to-locate-package-libesd0-dev-on-ubuntu-18-04#1082739), it turns out to be a deprecated package that is not found by default in the defined Kali sources. Therefore, those recommended to download this package from **Ubuntu (Xenial)** sources are included since they do seem to have it. The following sources are included in the **/etc/apt/sources.list** file:

```
deb http://us.archive.ubuntu.com/ubuntu/ xenial main universe
deb-src http://us.archive.ubuntu.com/ubuntu/ xenial main universe
```

However, after executing "**apt update**" to download the resources from the new sources specified, it returns an error because it **does not find the public keys associated** with the new sources included.

```
apt update

...

Warning: GPG error: ... NO_PUBKEY 40976EAF437D05B5 NO_PUBKEY 3B4FE6ACC0B21F32
```

However, after [importing the keys](https://askubuntu.com/questions/13065/how-do-i-fix-the-gpg-error-no-pubkey) , it is possible to download the required resources. It may arise in the process some errors, but this is to indicate that the package that is intended to be downloaded is deprecated.

```
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 40976EAF437D05B5

apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32

apt update
```

After doing this, the package is available to be installed. Therefore, it is **installed individually**.

```
apt-get install libesd0-dev
```

This process does not report any errors, so the rest of the dependencies required for cross-compilation can be installed.

```
apt install -y git-core gnupg flex bison gperf libesd0-dev build-essential zip curl libncurses5-dev zlib1g-dev gcc-multilib g++-multilib
```

This process, on the other hand, does report a new error, consisting of the inability to replace a file of a specific package and that it already exists in another dependency, not being able to resolve it.

```
dpkg: ... trying to overwrite 'usr/lib/x86_64-linux-gnu/pkgconfig/tic.pc' ...
Error: Sub-process /usr/bin/dpkg returned an error code (1)
```

Since this package is in an unresolved dependency and it seems that it is from an earlier version, the installation is carried out **indicating to forcefully overwrite** of the file. You can see that by doing this, instead of trying to resolve this dependency, it deletes this package that is apparently not being used.

```
apt -o Dpkg::Options::="--force-overwrite" install -y git-core gnupg flex bison gperf libesd0-dev build-essential zip curl libncurses5-dev zlib1g-dev gcc-multilib g++-multilib
```

At this point, include the "i386" architecture and get the associated packages. However, when trying to install the package indicated by the instructions (**ia32-libs**), it returns an error indicating that it is replaced by another one called **lib32z1**.

```
dpkg --add-architecture i386
apt update
apt install -y ia32-libs

No installation candidate for 'ia32-libs'
The following packages replace it:
    lib32z1
```

To solve this, this new dependency is installed as a substitution.

```
apt install -y lib32z1
```

Next, you must install the ARM Linaro compiler from its [GitLab](https://gitlab.com/kalilinux/packages/gcc-arm-eabi-linaro-4-6-2).

```
cd /root/
mkdir -p arm-stuff/kernel/toolchains/
cd arm-stuff/kernel/toolchains/
git clone https://gitlab.com/kalilinux/packages/gcc-arm-eabi-linaro-4-6-2.git
```

Finally, you define the architecture and the compiler in a variable to be used. This way, the compiler is ready to be used.

```
export ARCH=arm
export CROSS_COMPILE=/root/arm-stuff/kernel/toolchains/gcc-arm-eabi-linaro-4.6.2/bin/arm-eabi-
```

At this point, there are several ways to prepare and generate an ARM image. The simplest and the one that will be used in this case is to **take advantage of existing _scripts_ of different ARM images** to be able to be automatically generated and that offer the flexibility of being able to modify the specific image before being created. These scripts are in the official Kali repository on [GitLab](https://gitlab.com/kalilinux/build-scripts/kali-arm). Therefore, we proceed to clone this repository, include necessary resources of the "i386" architecture as indicated in the [instructions](https://www.kali.org/docs/development/kali-linux-arm-chroot/#real-world-custom-kali-linux-builds-for-arm-devices) and install the dependencies that these images require. However, one of the "i386" dependencies appears not to be found in the specified resources.

```
cd /root/
git clone https://gitlab.com/kalilinux/build-scripts/kali-arm.git
dpkg --add-architecture i386
apt update
apt install -y deboostrap qemu-user-static device-tree-compiler lzma lzop u-boot-tools libncurses5:i386 pixz

Error: Unable to locate package libncurses5:i386
```

After some [research](https://stackoverflow.com/questions/60824700/how-can-i-install-python-3-9-on-a-linux-ubuntu-terminal), found that this type of dependency is sometimes not directly referenced by name, but is associated by references from others. Therefore, it is advised to install the "**software-properties-common**" package which, when installed, should include this dependency that is needed.

```
apt install software-properties-common
```

At this point, the rest of the dependencies required for the images to be compiled are installed. The installation is successful.

```
apt install -y deboostrap qemu-user-static device-tree-compiler lzma lzop u-boot-tools pixz
```

Now, all requirements are met and it is possible to generate ARM images with the scripts provided by Kali. To do this, it is necessary to choose the appropriate script for the ARM system to use, since there are internal dependencies that can vary depending on the system and version. Given that in this case a Raspberry Pi 3 Model B is going to be used for testing, the correct script would be the so-called "**raspberry-pi-64-bit.sh**" which uses the "**raspberry-pi.sh**" script as a base to generate ARM images for versions 2/3/4/400, being able to specify its model and its architecture among other parameters. The latter _script_ is the one that will be used to include extra elements in the image before generating it as specified in a later section.

When executing the script "**raspberry-pi-64-bit.sh**" to try to generate a base image to test its operation, it returns an error indicating that it is missing a dependency called "**systemd-nspawn**" and that the script "**common.d/build_deps.sh**" must be executed in order to install the dependencies required to build the image.

```
cd /root/kali-arm
./raspberry-pi-64-bit.sh
...
[i] Error: missing systemd-nspawn
[i] Please run ./common.d/build_deps.sh
```

This is because **the generation of images is done in a virtualized environment within the system** which is generated and deleted in the compilation process itself, and which requires pre-installing, mounting and allocating a series of resources for the image to be generated. If this were not done, it would collide with the machine's own programs, dependencies, and resources. For this reason, it is also indicated beforehand that the machine must have a high allocation of resources (RAM, hard disk...), so that the allocation of resources in the compilation process is sufficient to carry it out properly. Therefore, as indicated in the previous illustration, the dependencies are installed with the indicated script.

```
/root/kali-arm/common.d/build_deps.sh
```

Now, before going on with the generation process, you may require to include your custom stuff to the image, like PIrate tool. The easiest way I found to do so is setting an HTTP server with Python and insert instructions on the base script to download the resources from there and then install them. Remember the installation process is taking place in a virtualized environment and shouldn't have access to the actual filesystem, that's why it is done this way. To make this, first set the Python HTTP server on the folder you will host the resources from (i.e. PIrate.deb on this example).

```
python3 -m http.server 80
```

Then edit the base file named "**raspberry-pi.sh**" from the official Kali repository on [GitLab](https://gitlab.com/kalilinux/build-scripts/kali-arm) cloned earlier and include these code on lines 40-41, after the instructions that install the kernel dependencies.

```
# Downloading and installing "PIrate" package
eatmydata wget -O /tmp/PIrate.deb http://<KALI_VM_IP>/PIrate.deb && eatmydata dpkg -i /tmp/PIrate.deb && eatmydata rm /tmp/PIrate.deb
```

If you require a wireless connection to be automatically configured, check "**WiFi setup**" subsection below before proceeding.

Once finished, you may run the script "raspberry-pi-64-bit.sh". The image generation process proceeds without any errors.

```
cd /root/kali-arm
./raspberry-pi-64-bit.sh
```

After waiting some time, the image ends up being successfully generated in a folder called "**images**", ready to be recorded on a Raspberry Pi-compatible media. It may take a while, so let it running in the background while doing other stuff. The generation may end up in roughly 3 hours.

```
File output: /root/kali-arm/images/kali-linux-2024.2-raspberry-pi-xfce-arm64.img.xz
Final time: 3 hours 21 minutes 3 seconds
```


### WiFi setup

This section will tell you how to configure the wireless connection on your Raspberry Pi.

Device and model tested: Raspberry Pi 3 Model B

In this repo, you will find a folder named "wifi-conf" which contains two files:
- **wlan0**: Its the file that configures the wireless interface. You should change this filename and the references to "wlan0" in its content to adapt it to your devices wireless interface name. It was left like this as an example.
- **wpa_supplicant.conf**: The configuration that will use the wireless interface to connect to the wireless network. You will require to specify the SSID and its password (the template is for a WPA2 Personal, but you may adapt it to your needs).

Once you have those files ready, depending on the approach you will have to do one thing or another.

#### Manually

Transfer these files to the Raspberry Pi and run the following commands to set them on their proper places (you may require root privileges):

```
mv /location/of/wlan0 /etc/network/interfaces.d/wlan0
mv /location/of/wpa_supplicant.conf /etc/wpa_supplicant/wpa_supplicant.conf
```

Once done, restart the system. Once the system has started back up, it should have connection with the wireless network and an IP address assigned by the DHCP server.

```
(opens a shell)
ifconfig

(IP address assigned to wireless network interface? Good!)
```

#### Automatically

Before proceeding and running the generation script "raspberry-pi-64-bit.sh", you should also make available this files on the Python HTTP server set previously and add the following lines just below the "PIrate" tool ones on script "**raspberry-pi.sh**".

```
# Downloading and configuring WiFi automatic connection (if you don't require it, comment out these two lines)

eatmydata wget -O /tmp/wlan0 http://<KALI_VM_IP>/wifi-conf/wlan0 && eatmydata mv /tmp/wlan0 /etc/network/interfaces.d/wlan0

eatmydata wget -O /tmp/wpa_supplicant.conf http://<KALI_VM_IP>/wifi-conf/wpa_supplicant.conf && eatmydata mv /tmp/wpa_supplicant.conf /etc/wpa_supplicant/wpa_supplicant.conf
```

Once done, you are set to run the script "**raspberry-pi-64-bit.sh**" to generate the image, including automatically also the wireless interface configuration.


# Recommendations and tips

- As already specified on some sections, **make use of the root user to manage and execute this tool**. For security purposes, **all the elements associated to this tool are owned strictly by this user**. I've been drawing an approach to avoid this on a future update, as I had short time to release this first version. In the meantime, please, do it this way.

- If you struggle with single commands when configuring them on an action (having errors or the action creation seems not to be working), try creating a Bash script file, include it there and then set this script as the action command. Of course, if this is the case go through the subsection "**Configuration of my own scripts**", to set the proper permissions on this new script.

- Also remember that the "export" functionality is only related to **actions**, meaning that it won't back up any new scripts or templates included.

- You may follow the same steps to define the administrator notification through email on new scripts and email templates the same way as shown on the provided examples or do it on your own way, it's up to you. As a troubleshooting tip, remember to check that your SMTP server configuration specified on the tool configuration file is correct by sending a test email to verify that it is working before proceeding to test the whole custom script.

- If you have new ideas to expand this tool to your own, you can by writing new code and extending the configuration file if needed. Then, generating the proper package as shown previously so you can install your most up-to-date version. Also, if you think those ideas are great for new updates (useful on general terms and not business-specific ones), make sure to contact me through GitHub, so I consider them on for the next updates.

# Future of this tool

As already specified, this tool will have some improvements and added new features on the near future. Some things will be deprecated and others will be extended.

The most critical issues that I already have them documented will be fixed on the next update.

Redface, this sassy pirate has been spending all the hard-earned gold on rum and now its having serious trouble on getting back to his ship in order to continue with his adventures.

But, during the development of this tool, some new ideas came up to my mind. Probably a pal for this pirate may appear on a future. He heard of a name while taking a shot of rum in the pub; Blueface.

