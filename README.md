
# RoboPeak/DFRobot 2.8" USB TFT Driver

This repository is made for Linux 6.12, I don't know if it work on older version.  
only knows 6.x has changed for framebuffer things and this source can work on it.

As you know, the old repository(pimoroni or robopeak) can't work anymore.  
so I modified and cleaned pimoroni's version using AI, now it can work on 6.12.

According to subsequent confirmation, this version of code will only work until 6.12.  
6.13+ will change things again, need to fixed for work.  
I might do it later...

Manufacturer's repository  
https://github.com/robopeak/rpusbdisp  
Retailer's repository  
https://github.com/pimoroni/rp_usbdisplay  
AI(Manus)  
https://manus.im/


I tested on Raspberry Pi Zero W (armv6l + Linux 6.12.62)  
and screen work, but touchscreen doest test yet.  
also tested on Radxa A7A (aarch64 + 5.15.147) as screen display  
(but only this time for this old kernel)


## Build and Install

0.  Clone and enter to building path：
    ```bash
    git clone https://github.com/sunglee42/rp_usbdisplay_linux6x.git
    cd rp_usbdisplay_linux6x/dkms-rp_usbdisplay-2.0
    ```
1.  Copy this path to `/usr/src/rp_usbdisplay-2.0`：
    ```bash
    sudo cp -r . /usr/src/rp_usbdisplay-2.0
    ```
2.  Using DKMS Add、Build and Install：
    ```bash
    sudo dkms add -m rp_usbdisplay -v 2.0
    sudo dkms build -m rp_usbdisplay -v 2.0
    sudo dkms install -m rp_usbdisplay -v 2.0
    ```

## Simple Test
will load driver module and display somethings on screen...  

1. Load the module just build (should be black screen after this)
```bash
sudo depmod
modprobe rp_usbdisplay
```
2. Check which framebuffer using (normally it will be fb1, so we do fb1 here)
```bash
cat /proc/fb | grep rpusbdisp-fb
```
3. Load the noise screen
```bash
cat /dev/urandom > /dev/fb1
```
4. Load Pimoroni logo
```bash
zcat shoplogo.fb.gz > /dev/fb1
```

  
Alternative methods (specify screen size: 320x240, 16 bit)
- noise screen
```bash
sudo dd if=/dev/urandom of=/dev/fb1 bs=153600 count=1
```
-  black screen (clean)
```bash
sudo dd if=/dev/zero of=/dev/fb1 bs=153600 count=1
```

## Extra
- Set dkms driver auto load on boot
```bash
echo rp_usbdisplay | sudo tee -a /etc/modules
```
  
- TODO   
> Xorg or Console things...  
> TODO will try on Linux 6.17 with x86 architecture  

- [X] Module compiled (Linux 6.17, x86)
- [X] Module install and load
- [X] Screen Work well
- [ ] Work with Xorg
- [ ] Work with Console
