# archinstallguide

NOTE: text within () are additional info, text within <> are for you to decide

fdisk -l (list partitions)

fdisk /dev/sda (select drive, typically sda unless you have multiple)

o (wipes drive)

n (new partition)

p (set as primary)

1 (set partition number)

(set sectors to default if you're using entire drive)

a (set as active partition)

w (writes to drive)

mkfs.ext4 /dev/sda1 (sets filesystem to ext4)

ip a (check if you have an ip address)

dhcpcd (request ip from DHCP server)


if you have no wifi:

iw dev (find wireless interface name) 
(or)
lspci -v (shift + pageup/pagedown to scroll)
dmesg | grep <interface_name>

iw dev <interface_name> link

ip link set <interface_name> up

wpa_supplicant -B n180211 wext -i <interface_name> -e <(wpa_passphrase "SSID" "WIFI_PASSWORD")

dhcpcd <interface_name>


mount /dev/sda1 /mnt (replace sda1 with drive name)

mkdir /mnt/home

mount /dev/sda1 /mnt/home


pacstrap -i /mnt base (installs base of filesystem)

genfstab -U -p /mnt >> /mnt/etc/fstab

arch-chroot /mnt


pacman -S grub-bios linux-headers wpa_supplicant wireless_tools

nano /etc/locale.gen (uncomment en_US.UTF8 : ctrl+o ctrl+x to save and exit)

locale-gen

ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime

hwclock --systohcloca

passwd (set your root password)

grub-install --target=i386-pc --recheck /dev/sda (replace sda with your drive name, do NOT add a number after your drive name or the system will not be bootable)

cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo

grub-mkconfig -o /boot/grub/grub.cfg

exit

umount /mnt/home

umount /mnt

reboot


(Login, username: root, pass: whatever you set it as)

localectl set-locale LANG="en_US.UTF-8"


(check for wifi connection)

ip a

(if no wifi, follow previous steps for getting wifi)


pacman -Sy wicd wireless_tools wpa_supplicant wpa_actiond dialog xorg-server xorg-xinit xorg-apps mesa


(to install 32 bit packages, follow this step, please keep in mind that 32-bit support is going away soon)

nano /etc/pacman.conf (uncomment multilib : ctrl+o ctrl+x to save and exit)


(To install video drivers follow this next step)


pacman -Sy xf86-video-intel lib32-intel-dri lib32-mesa lib32-libgl (Intel graphics)

https://wiki.archlinux.org/index.php/NVIDIA (NVIDIA graphics)

https://wiki.archlinux.org/index.php/VirtualBox (VB graphics)

https://wiki.archlinux.org/index.php/ATI (AMD graphics)


pacman -Sy sudo

visudo (uncomment wheel all = all command, press x to delete and :wq to save and exit)

useradd -m -G wheel -s /bin/bash <username>

passwd <username>

hostnamectl set-hostname <name of computer, will display in terminal as username@hostname>


(at this point you set your login manager, I would suggest using lightdm or gdm, but find one that you like)

pacman -Sy lightdm lightdm-gtk-greeter

sudo systemctl enable lightdm

(OR)

pacman -Sy gdm

sudo systemctl enable gdm


sudo pacman -S terminator i3-wm thunar (look up all of these packages and choose which ones you want, terminator can be swapped out for xterm or gnome-terminal for exmaple and i3 can be changed to any desktop/wm you like)


sudo pacman -S net-tools

#(sudo systemctl start Network Manager)

#(sudo systemctl enable Network Manager)


(at this point, login by using sudo systemctl start <loginmanger>)


You should now have Arch linux installed and should begin installing basic software that you will be using.

sudo pacman -S a52dec faac faad2 flac jasper lame libdca libdv libmad libmpeg2 libtheora libvorbis libxv wavpack x264 xvidcoregstreamer0.10-plugins (this will download many codecs that you will need)

sudo pacman -S pulseaudio pulseaudio-alsa firefox flashplugin vlc unzip unrar p7zip deluge gimp xfburn (example of software that you can start off with, more can be found at https://aur.archlinux.org/)


I also HIGHLY advise using yaourt. To start, go to https://aur.archlinux.org/packages/yaourt/ and click "Download snapshot" to the right under "Package Actions"

then in terminal go to where the tar is saved and type "tar -xvf <file_name>" then change directory into the folder made for yaourt and type "makepkg -si"

once yaourt is installed you can install programs in the Arch user repository by typing "yaourt -S <package_name>"

Without yaourt you would have to do the same proccess that you did to install yaourt on any package you got off of the Arch user repository (if it is tagged with AUR, otherwise you use pacman).
