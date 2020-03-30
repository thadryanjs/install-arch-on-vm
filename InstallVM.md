# How to install Arch Linux on VirtualBox 

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_loadScreen.png)

There have been several walkthroughs on installing [Arch Linux](https://wiki.archlinux.org/index.php/Arch_Linux) on VMs, like [this one](https://www.howtoforge.com/tutorial/install-arch-linux-on-virtualbox/) for example. However, I noticed there have been some changes that call for a few updates, so I thought I'd post a procedure that included them. As of the date of this post, this guide will produce a working system from the ground up. I also added some tweaks to get the most out of the VM, creating and adding users, and installing a desktop environment.

## Pros and Cons or Arch

- TL;DNR: Arch is great is you want a well-documented, minimal system that you can customize, has up-to-date software, and will teach you about Linux. It's not great if you want something up and running quickly with out-of-the-box features or something that doesn't update often.

The benefits and downsides or Arch have been debated extensively (ad nauseam?) all over the internet. This is my take on summarizing them in as briefly as possible while still giving a good enough overview that people can choose for themselves (this is purely from a practical standpoint and doesn't get into more [philosphical stuff](https://www.reddit.com/r/linux/comments/5n069y/why_do_people_not_like_systemd/) like ```systemd```).

### Pros

- Minimal - Arch installs a minimal system without even so much as a desktop environment and not a lot of stuff to slow your system down. The user doesn't have the hassle of tracking down extra software and figuring if it's bloat or important.

- Customizable -  This minimal approach means you can install whatever you want. In particular, you get to choose a desktop without depending on that has been made into a flavor/edition/spin. 

-  Well-documented - The much-hyped [Arch Wiki](https://wiki.archlinux.org/) is, in fact, great.

- Transparent - It's often said Arch is good if you want to learn more about GNU/Linux and how it works. Arch is helpful for this in my experience - I learned a lot tinkering under the hood.

- Current - Arch is a rolling release distro know for getting updates quickly.

### Cons

- (Relatively) Complex to install - Arch has a more involved installation process that other distros; the minimal system doesn't come without a trade-off. It's less geared to getting a system setup quickly and easily than other distros.

- Requires some maintenance - every now and then (a handful of times a year maybe) a package will requires you to manually run a command or two to update properly. They'll be noted on the front page of the Arch website, but if you let them build up, things can get weird. In theory, this increases the chances that some update will screw up your machine and require you too boot into a live medium and fix it.

## Procedure

Head over to the [Arch Linux download page](https://www.archlinux.org/download/) and either torrent yourself an ISO or download one from one of the mirrors.

### The VirtualBox Part

VirtualBox can be downloaded [here](https://www.virtualbox.org/wiki/Downloads). For Windows/Mac people, select the installer from the list. When on Linux I prefer to go with my distro-specific package manager. 

#### Setting up a virtual machine

There are a few steps to the VM setup but they're simple and the defaults will work in most cases. Once you've got VirtualBox running, click "New" to get things started.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_1.png)

Give your machine a snappy name! VirtualBox will detect what system it is if it starts with "Arch". If it doesn't, you can select it from the dropdown. Choose "Arch Linux (64-bit)".

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_2.png)

VBox will ask what you want for memory size. I usually double this just to make sure the machine isn't sluggish, then click "Next".

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_3.png)

We want to create a disk, not load an existing one. Just click "Create".

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_4.png)

We can accept the default drive type and click "Next".

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_5.png)

I use dynamically allocated by default, and it will work fine for our purposes.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_6.png)

I put the size up to 24 GB in case we want to build out the system and try it for a daily driver for a while or something. It's just a VM, so [it's no big deal!](https://i.kym-cdn.com/entries/icons/original/000/021/311/free.jpg)

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_7.png)

We will now see the machine on the VBox menu on the left side of the screen.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_8.png)

Before we go ahead and start it and select our ISO, a few little tweaks make the VBox experience a lot more pleasant and fast. Click "Settings". You'll see a menu like this:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_7b.png)

Navigate to "System".

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_7c.png)

Choose the "Processor" tab, and increase the number of CPUs the VM is allowed to use (assuming you have more than one on the host machine). Then move over to "Display" on the menu on the left side of the screen.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_7d.png)

I like to increase the video to max as well. Exit the "Settings" menu by clicking "Ok". We're ready to fire up the machine and connect it to our Arch ISO. In the top menu click the green "Start" button.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_9.png)

VBox will now ask you where your bootable media is. Click the folder icon. You'll see a screen like this:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_10.png)

Click the "Add" symbol in the left of the menu, navigate to wherever you downloaded your ISO, and select it with "Choose", then click "Start".

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/virtualbox_11.png)

Select the first option, which boots us into a live Arch system. When you boot in, a series of message will flash by and then you'll see something like this:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_00.png)

*Note: If the terminal seems cluttered, pressing ctl-l between commands will clear the screen. If the amount of black space on your screen looks different than mine it's probably just that I've done this.*

Here goes!

### The Arch part

#### Partitioning

This is where we prepare the hard drive for installation. There are many ways to do this, with separate partitions for boot, swap, etc, but for a simple test VM, we're just going to install it all into one partition. We prepare it using the following command.

    cfdisk

This starts a partitioning tool. You'll be given an option to select "label type" by moving the arrows up and down. We're looking for ```dos```.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_0.png)

Next you will see a screen like the one below. You can move left and write with arrow keys to make a selection. Move to "New", and hit "enter" to make a new partition of the virtual hard rive. 

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_gp1.png)

Hit enter to use all 24G.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_gp2.png)

Select "primary" when asked for type (extended partitions allow for sub-dividing space, which isn't a concern since we're using the whole virtual disk for one thing).

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_gp3.png)

Now move to the "Bootable" option on the left side of the screen and hit enter.


![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_gp4.png)

Navigate to "Write" and hit enter to make these changes actually happen. If we were on a real machine, this would be the point of no return where we had erased the underlying system.


![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_gp5.png)

You will have to type "yes" to consent to the operation. The top of the screen should show a partition of 24G on /dev/sda1 with a ```*``` under "Boot".


![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_gp7.png)

Now we can head over to "Quit" and exit the partition editor tool.


![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_gp8.png)

#### File systems

Next, we assign the file system type to the newly partitioned drive partition. ```ext4``` is the default file system for Linux.

    mkfs.ext4 /dev/sda1


![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_2.png)

You will see some output from the results of the command. Now we mount the drive so we can install the base system:

    mount /dev/sda1 /mnt

#### The Base System

Arch uses a utility called ```pacstrap``` utility to install the base Arch system. *If you're looking for an update, this is one place where things are different from previous tutorials.* In previous versions only ```base``` and ```base-devel``` were needed here. Failing to include ```linux``` and ```linux-firmware``` will cause a failure to boot later on because this is the actual Linux kernel and associated content. This step will most likely take several minutes. 

    pacstrap /mnt base base-devel linux linux-firmware

Initially, you'll see:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_5.png)

...and then something like:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_6.png)

This is the normal display of Arch syncing and installing packages, the equivalent of the ```apt update```, etc procedure in Debian based systems. Hit enter to continue when prompted.

When it's done, it will look like this:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_7.png)

We now need to create a file system table. This is a record of our hard drive partitions assigned identifiers for efficient look-up. If you're curious, you can view the help for the command like this:

    genfstab -h

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_8.png)

To use it, and redirect the output to the proper spot on our system, use this command:

    genfstab /mnt >> /mnt/etc/fstab

Now we use the Arch version of ```chroot```. This creates an isolated environment that doesn't have access to the main system in case we biff something (the extra arguments are where to put this environment and what shell to use).

    arch-chroot /mnt /bin/bash

You should notice a change in the terminal prompt:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_11.png)

We will install ```nano```, and simple text-editor, within ```chroot``` so we can do some manual configuration. ```pacman``` is the package manager for Arch. It's fast, simple, and dearly beloved. ```-S``` means "sync", as in "sync these packages with my machine". *This is also a divergence from previous tutorials*.

    pacman -S nano

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_12.png)

Hit enter to accept the default and proceed with the installation. This should be quick.

#### Language

Now we're free to edit some files:

    nano /etc/locale.gen

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_14.png)

We're doing this setup for English language systems, use whatever you're looking for. Basically we're just un-commenting the language locale we want. If you're configuring an English language system, scroll until you find...

*#en_US.UTF-8 UTF-8*

...and un-comment it. 

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_15.png)

Press ```ctr-x``` to exit ```nano```. You'll be asked if you want to save the modified buffer.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_16.png)

Type ```y```, then hit enter when asked for the file name to save it as (it will default to the one we gave it using the ```nano``` command). We can now run the ```locale-gen``` command to generate the locale information.

    locale-gen

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_17.png)

We've generated to locale and made them available. Now we can set it as our choice. Enter the language info in a file called locale.conf.

    nano /etc/locale.conf 

Enter...

*LANG=en_US.UTF-8*

...into this file, and exit ```nano``` as before.


![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_18.png)

#### Time

 Next up is time zones. We can see a list of the selections using this commands.

    ls /usr/share/zoneinfo

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_19.png)

You can see the options under each region using ``ls`` on one of the folders shown by the above command, ie...

    ls /usr/share/zoneinfo/America

...to see which region you're in:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_timezones.png)

We then set the desired zone to our system localtime. This command creates a "symbolic link" from the zone info data, and associates that link with a new config file called *etc/localtime*. I'm in the northeastern US, so for me it's:

    ln –s /usr/share/zoneinfo/America/New_York /etc/localtime

This should just happen silently.

Next is the "hardware" clock of our VM. Problems here can lead to trouble updating down the road, so it's worth checking that it's synced if you run into issues.

    hwclock --systohc --utc

This command is also silent when successful.

#### Users and Permissions

Now we create a password for the root user.

    passwd # set your root password

Here is what your screen should look like:


![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_21.png)

Practically speaking, I think it's worth making a main user at this point, though it isn't technically required.

    useradd -m -g users -G wheel -s /bin/bash username

The ```-m``` flag creates a ```/home``` directory for the new user. The ```-g``` flag specifies the group to add the user to. The ```-G``` flag refers to auxiliary groups to add the user to. In this case, they're added to the ```wheel``` group, which will let them be a full admin. ```/bin/bash``` specifies what shell the user will have. It's typical to use bash. Don't forget to change ```username``` to the name you want.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_22.png)

Give the user a password:

    passwd username

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_23.png)

Now, we allow our users in the wheel group to use ```sudo```. The ```visudo```   command makes sure the edits to the file are syntactically legit so you don't screw it up with a typo.

    EDITOR=nano visudo

Find and un-comment the following line:

*%wheel ALL=(ALL) ALL*

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_25.png)

#### Hostname & Network

Now to name the machine:

    nano /etc/hostname

Enter the name you want and exit ```nano```.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_26.png)

Now we need to setup the network inside chroot so we can install the bootloader (up until now, we've been getting sweet internet goodness from the host machine, but we're hidden away in ```chroot``` at the moment). We will install a network manager to enable this. *This is a divergence from previous tutorials as well*. Run the following command and hit enter to accept the install.

    pacman -S dhcpcd

We've install the network manager, but we need to turn it on. We can do so using the ```systemctl``` command. 

    systemctl enable dhcpcd

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_27.png)

Now we can install ```grub```, which will allow us to boot into our system. ```os-prober``` is sometimes installed at this step. It detects other operating systems if you dual boot, though that won't be an issue with a VM. It's good to know it exists however.

    pacman –S grub os-prober

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_28.png)

Now that we have ```grub```, we can install to our ```/dev/sda``` drive where is will be able to "see" the rest of our system:

    grub-install /dev/sda

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_new_grb.png)

This command will create a config file for ```grub``` from which we can customize it later if desired. 

    grub-mkconfig –o /boot/grub/grub.cfg

You should see something like this:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_31.png)

This is important: what it says it's detecting is the Linux kernel we installed earlier. This means ```grub``` knows where to look when it's time to boot. We can now exit the ```chroot``` environment:

    exit

We should have a working base system now. To test this, reboot:

    reboot 

You should be back at the boot screen. Now, try booting into the existing OS to see our system. 


![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_33.png)

You should see the ```grub``` login screen. 

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_35.png)

Select "Arch Linux". You'll be asked to log in via terminal. Type your username and password.

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_36.png)

We're good! If you want a desktop, you can choose whichever you like, and set it up. I'm going to use Gnome for this because it's my go-to, but the point of Arch is that you can use whatever you want and configure it however you like. This will usually entail installing the desktop, and, if it's not included in the desktop package, a display manager. First the install (this takes a while - Gnome is full-size desktop):

    sudo pacman -S gnome

You may see the following prompt:

![](http://pythoninthewyld.com/wp-content/uploads/2020/03/install_jack.png)

Accepting the default is fine (it's an audio component use in the Gnome package)

Next, enable the Gnome display manager like this:

    systemctl enable gdm.service

This will allow us to use the desktop after boot. Reboot, and you will be greeted with a the Gnome login screen.

    reboot

You should now have a working VM. I'd recommend checking out the [Arch Wiki](https://wiki.archlinux.org/index.php/VirtualBox) on tweaking the virtual machine for your host OS to allow for fullscreen, shared clipboard, and shared drives.

#### References:

	https://wiki.archlinux.org/index.php/Installation_guide

	https://www.howtoforge.com/tutorial/install-arch-linux-on-virtualbox/

	# now retired apparently
	https://bigdaddylinux.com/easily-install-arch-linux-in-virtualbox/
