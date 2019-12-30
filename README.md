# mbp_efi_external

## Windows 10 (Tested on MBP 13.2)

<details>
<summary>Show Installation</summary>

### Installation
**[1]** **Download** latest bootcamp drivers

![bootcamp drivers](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/windows/drivers.png)

**[2]** **Create a Virtual Machine** (VirtualBox, Parallels, ...) or **use an existent Windows**

**[3]** Get [Rufus](https://rufus.ie "Rufus") (free) or [WinToUSB](https://www.easyuefi.com/wintousb/ "WinToUSB") (paid, free if you use Windows 10 HOME)

**[4a]** **[Using Rufus]** Select your **usb drive**, **Windows 10 iso** and a **GPT partition scheme**

![rufus example windows to go](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/windows/rufus/windows-to-go-15.png)

**[4.1b]** **[Using WinToUSB]** Select your **usb drive** and **GPT for UEFI**

![wintousb gpt 4 uefi](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/windows/wintousb/partitionscheme.png)

**[4.2b]** **[Using WinToUSB]** Check if it is all ok and select **legacy** installation mode

![wintousb legacy installation](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/windows/wintousb/legacy.png)

**[5]** When it is finished, restart your system with your usb plugged and alt key pressed

**[6]** Maybe your keyboard and trackpad won't work untill bootcamp drivers are installed

</details>

<details>
<summary>Show Boot EFI Logo</summary>

### Boot EFI Logo

**[1]** **Download** [windows.png](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/EFI_ICONS/windows.png "Windows logo") or draw your own

**[2]** Rename **windows.png** to **.VolumeIcon.icns** and place the icon in the root of your efi partition

**The icon will be in the root of your EFI partition**

</details>

## Debian Testing (Tested on MBP 13.2) / Ubuntu 18.04.3 LTS

<details>
<summary>Show Installation</summary>

### Installation
**[1]** **Create an EFI Virtual Machine** (VirtualBox, Parallels, ...)

**Virtualbox**

![vbox](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/vboxefi.png)

**Parallels**

![parallels](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/parallelsefi.png)

**[2]** Run and **install your Debian on your external usb drive** (not the internal)

**[3]** **Force UEFI installation**

![force_uefi](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/force_uefi.png)

**[4]** Choose **install grub on the external drive**

**[5]** Restart and choose: **Advanced options...** -> **Debian... (Recovery Mode)**

![grub](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/grub.png)

**[6]** Type your password and then
```
dpkg-reconfigure grub-efi-amd64
```

**[7]** When prompted if *force extra installation to the EFI removable media path* **Choose YES**

![efi](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/efi.png)

**[8]** When prompted if the NVRAM should be updated **Choose NO**

</details>

<details>
<summary>Show Keyboard and Touchbar</summary>

### Keyboard and Touchbar

#### DKMS module (Debian & co)

**[1A]** As root, do the following (all MacBook's and MacBook Pro's except MacBook8,1 (2015)):
```
echo -e "\n# applespi\napplespi\nspi_pxa2xx_platform\nintel_lpss_pci" >> /etc/initramfs-tools/modules
```

**[1B]** If you're on a MacBook8,1 (2015):
```
echo -e "\n# applespi\napplespi\nspi_pxa2xx_platform\nspi_pxa2xx_pci" >> /etc/initramfs-tools/modules
```

**[2]** For all Macbook's and Macbook Pro's:

```
apt install dkms git
git clone https://github.com/roadrunner2/macbook12-spi-driver.git /usr/src/applespi-0.1
dkms install -m applespi -v 0.1
```

**If dkms doesn't work try /sbin/dkms**

**If you can't clone latest https://github.com/roadrunner2/macbook12-spi-driver.git, try: https://github.com/manuMatnez/macbook12-spi-driver.git**

#### Akmods module (RPM Fusion / Red Hat & co)

You can build the akmod package from this repository:

https://pagure.io/fedora-macbook12-spi-driver-kmod

Or use this [copr repository](https://copr.fedorainfracloud.org/coprs/meeuw/macbook12-spi-driver-kmod/):
```
dnf copr enable meeuw/macbook12-spi-driver-kmod

dnf install macbook12-spi-driver-kmod
```
</details>

<details>
<summary>Show WI-FI</summary>

### WI-FI

**[1]** **Download** [brcmfmac43602-pcie.txt](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/linux_wifi/brcmfmac43602-pcie.txt "brcmfmac43602-pcie")

**[2]** Open and edit **brcmfmac43602-pcie.txt**, you will see: macaddr=**xx:xx:xx:xx:xx:xx**. For usage You have to replace it with the macaddress of your device

**[3]** Save **brcmfmac43602-pcie.txt** and move it to **/lib/firmware/brcm**

</details>

<details>
<summary>Show Sound</summary>

### Sound

**ubuntu / debian package install**  
```
apt install wget make gcc linux-headers-generic
```

**fedora package install**
```
dnf install wget make gcc kernel-devel
```

**build driver**  
```
git clone https://github.com/leifliddy/snd_hda_macbookpro.git  
cd snd_hda_macbookpro/
./install.cirrus.driver.sh
reboot
```

**If you can't clone https://github.com/leifliddy/snd_hda_macbookpro.git, try: https://github.com/manuMatnez/snd_hda_macbookpro.git**

</details>

<details>
<summary>Show Sensors</summary>

### Sensors

**ubuntu / debian package install**  
```
apt install lm-sensors
```

**fedora package install**
```
dnf install lm_sensors
```

**Execute**

```
sensors-detect
```

</details>

<details>
<summary>Show Boot EFI Logo</summary>

### Boot EFI Logo

**[1]** **Download** [linux_debian.png](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/EFI_ICONS/linux_debian.png "Debian logo") or draw your own

**[2]** Rename **linux_debian.png** to **.VolumeIcon.icns** and place the icon here: **/boot/efi/.VolumeIcon.icns**

**The icon will be in your EFI partition**

</details>
