# VirtualBox guest additions for Archlinux guest systems

## step 1: install the Guest additions

For VirtualBox Guest utilities with X support

`$ pacman -S virtualbox-guest-utils xf86-video-vmware`

Choose virtualbox-guest-modules-arch of the you are prompted to

## step 2: Set screen resolution

You will need to on the host machine go to the installation path of VBox and find `VBoxManage.exe`
by default it should be located at `C:\Program Files\Oracle\VirtualBox`, 
if VBox is installed in another directory find `<path/to/dir/>`

Open CMD and cd to to that location

```$ cd C:\Program Files\Oracle\VirtualBox```

or

```$ cd <path/to/dir/>```

When you are there execute the following command to add a custom resolution 

```$ VBoxManage setaxtradata "Arch" "customevideomode" "1080x1920x24"```

Arch is the name of your Virtual machine

### step 2.1: Set kernel parameter

Add the desired resolution to the kernel parameters ex `video=resolution`

reboot the VM and press "e" when the menu shows
locate the line 

```linux /boot/vmlinuz-linux root=UUID=978e3e81-8048-4ae1-8a06-aa727458e8ff quiet splash```

And to the end add video=1080x1920

To make the changes persist after reboot
Edit the file `/etc/default/grub` on the line

```GRUB_CMDLINE_LINUX_DEFAULT```

And append the the same options as in the kernel parameter

Then regenerate `grub.cfg` with

```$ grub-mkconfig -o /boot/grub/grub.cfg```

## step 3: Load the VirtualBox kernel modules

type:
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

`$ VBoxClient-all`

