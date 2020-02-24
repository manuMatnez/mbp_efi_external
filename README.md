# MBP EFI External: OS Installation

## Windows 10 (Tested on MBP 13.2)

<details>
  <summary><b>Show Installation</b></summary>

### Installation
**[1]** **Download** latest bootcamp drivers

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/windows/drivers.png" width="500" alt="Bootcamp drivers download">

**[2]** **Create a Virtual Machine** (VirtualBox, Parallels, ...) or **use an existent Windows**

**[3]** Get [Rufus](https://rufus.ie "Rufus") (free) or [WinToUSB](https://www.easyuefi.com/wintousb/ "WinToUSB") (paid, free if you use Windows 10 HOME)

**[4a]** **[Using Rufus]** Select your **usb drive**, **Windows 10 iso** and a **GPT partition scheme**

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/windows/rufus/windows-to-go-15.png" width="300" alt="Rufus windows to go example">

**[4.1b]** **[Using WinToUSB]** Select your **usb drive** and **GPT for UEFI**

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/windows/wintousb/partitionscheme.png" width="500" alt="Wintousb gpt 4 uefi">

**[4.2b]** **[Using WinToUSB]** Check if it is all ok and select **legacy** installation mode

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/windows/wintousb/legacy.png" width="500" alt="Wintousb legacy installation">

**[5]** When it is finished, restart your system with your usb plugged and alt key pressed

**[6]** Maybe your keyboard and trackpad won't work untill bootcamp drivers are installed

**[7]** Run regedit.exe as administrator

**[8]** Navigate to **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control** and change the value **PortableOperatingSystem** from **1** to **0**

</details>

<details>
<summary><b>Show Mac Precision Touchpad</b></summary>

### Mac Precision Touchpad

**[1]** **Download the latest RELEASE** [mac-precision-touchpad](https://github.com/imbushuo/mac-precision-touchpad "mac-precision-touchpad")

**[2]** Navigate to **x64\ReleaseSigned**

**[3]** Go to **AmtPtpDeviceUniversalPkg** directory

**[4]** Right click **AmtPtpDevice.inf** and install it

</details>

<details>
<summary><b>Show Boot EFI Logo</b></summary>

### Boot EFI Logo

**[1]** **Download** [windows.png](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/EFI_ICONS/windows.png "Windows logo") or draw your own

**[2]** Rename **windows.png** to **.VolumeIcon.icns** and place the icon in the root of your efi partition

**The icon will be in the root of your EFI partition**

</details>

<hr />

## Debian Testing (Tested on MBP 13.2) / Ubuntu 18.04.3 LTS

### Doesn't Work:
- Suspend/Hibernate (partial working) (doesn't work using external drive)
- Sound (partial working)

<details>
<summary><b>Show Installation</b></summary>

### Installation
**[1]** **Create an EFI Virtual Machine** (VirtualBox, Parallels, ...)

**Virtualbox**

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/vboxefi.png" width="500" alt="Vbox">

**Parallels**

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/parallelsefi.png" width="500" alt="Parallels">

**[2]** Run and **install your Debian on your external usb drive** (not the internal)

**[3]** **Force UEFI installation**

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/force_uefi.png" width="500" alt="Force uefi">

**[4]** Choose **install grub on the external drive**

**[5]** Restart and choose: **Advanced options...** -> **Debian... (Recovery Mode)**

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/grub.png" width="500" alt="Grub">

**[6]** Type your password and then
```
dpkg-reconfigure grub-efi-amd64
```

**[7]** When prompted if *force extra installation to the EFI removable media path* **Choose YES**

<img src="https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/images_tutorial/debian/efi.png" width="500" alt="Efi">

**[8]** When prompted if the NVRAM should be updated **Choose NO**

**[9]** After restarted you can disable suspend and hibernate because they don't work yet using linux from external drive

**Disable**
```
systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

**Enable**
```
systemctl unmask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

</details>

#### FOR MBP >= 13.1

<details>
<summary><b>Show Keyboard and Touchbar</b></summary>

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

**If dkms doesn't work try su -**

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
<summary><b>Show WI-FI</b></summary>

### WI-FI

**[1]** Maybe **Install brcmfmac** [brcmfmac43602-pcie.bin](https://git.kernel.org/cgit/linux/kernel/git/firmware/linux-firmware.git/plain/brcm/brcmfmac43602-pcie.bin "brcmfmac43602-pcie") for MBP 13.2 [Debian wiki BCM43602](https://wiki.debian.org/brcmfmac "debian wiki") by brcmfmac43602-pcie.bin cpying to the folder /lib/firmware/brcm/

**[2]** **Download** [brcmfmac43602-pcie.txt](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/linux_wifi/brcmfmac43602-pcie.txt "brcmfmac43602-pcie")

**[3]** Open and edit **brcmfmac43602-pcie.txt**, you will see: macaddr=**xx:xx:xx:xx:xx:xx**. For usage You have to replace it with the macaddress of your device

**[4]** Save **brcmfmac43602-pcie.txt** and move it to **/lib/firmware/brcm**

</details>

<details>
<summary><b>Show Sound</b></summary>

### Sound

**ubuntu / debian package install**  
```
apt install wget make gcc linux-headers-generic
```

**ubuntu install**  

```
apt install linux-headers-generic
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

</details>

<details>
<summary><b>Show Sensors</b></summary>

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
<summary><b>Show FIX "W: Possible missing firmware /lib/firmware/i915"</b></summary>

### FIX "W: Possible missing firmware /lib/firmware/i915"

[askubuntu]("https://askubuntu.com/questions/811453/w-possible-missing-firmware-for-module-i915-bpo-when-updating-initramfs")

**[1]** **Download** from [git.kernel.org]("https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915") your missing firmwares

**[2]** **Use**

```
cp ~/Downloads/*.bin /lib/firmware/i915/
update-initramfs -u
```

</details>

<details>
<summary><b>Show Boot EFI Logo</b></summary>

### Boot EFI Logo

**[1]** **Download** [linux_debian.png](https://raw.githubusercontent.com/manuMatnez/mbp_efi_external/master/EFI_ICONS/linux_debian.png "Debian logo") or draw your own

**[2]** Rename **linux_debian.png** to **.VolumeIcon.icns** and place the icon in the root of your efi partition

**The icon will be in your EFI partition**

</details>
