# VirtualBox guest additions for Archlinux guest systems on Win host

Lines preceded with "$" should be executed in the terminal

## step 1: install the Guest additions

For VirtualBox Guest utilities with X support,
Install the following packages.

```$ pacman -S virtualbox-guest-utils xf86-video-vmware```

Choose virtualbox-guest-modules-arch of the you are prompted to

## step 2: Set screen resolution


you will need to navigate to the installation path of VirtualBox on the host machine and locate `VBoxManage.exe`. 
By default VirtualBox should be installed at `C:\Program Files\Oracle\VirtualBox`, 
if VBox is installed in another directory use `<path/to/dir/>`

Open CMD and navagate to to that location

```$ cd C:\Program Files\Oracle\VirtualBox```

or

```$ cd <path/to/dir/>```

When you are there execute the following command to add a custom resolution 

```$ VBoxManage setaxtradata "Arch" "customevideomode" "1920x1080x24"```

`Arch` is the name of your Virtual machine

### step 2.1: Set kernel parameter

Add the desired resolution to the kernel parameters ex `video=resolution`

reboot the Virtual machine and press "e" when the menu shows, locate the line 

```linux /boot/vmlinuz-linux root=UUID=978e3e81-8048-4ae1-8a06-aa727458e8ff quiet splash```

At the end add your desired resolution `video=1920x1080`

To make the changes persist after reboot
Edit the file `/etc/default/grub` on the line

```GRUB_CMDLINE_LINUX_DEFAULT```

And append the the same options as in the kernel parameter

Then regenerate `grub.cfg` with

```$ grub-mkconfig -o /boot/grub/grub.cfg```

## step 3: Load the VirtualBox kernel modules

Type to add the VirtualBox modules to the linux kernel
```$ modprobe -a vboxguest vboxsf vboxvideo```

Now its time to launch the vbox service.

```
$ VBoxClient --clipboard
$ VBoxClient --draganddrop
$ VBoxClient --seamless
$ VBoxClient --display
$ VBoxClient --checkhostversion
$ VBoxClient --vmsvga-x11
```

VBoxClient can only be called with one flag at a time, as a shortcut to launch all at once type

```$ VBoxClient-all```

For more information visit the Arch Wiki at (https://wiki.archlinux.org/index.php/VirtualBox#Installation_steps_for_Arch_Linux_guests)
