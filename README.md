# keyboard-fix-apple
Fix for users of mechanical keyboards that use Apple logic to work with their hardware.

I bought a MOD004 a while back. Unfortunately for me, I run Arch Linux with an encrypted partition (LUKS). The first time I rebooted my PC with this Akko keyboard attached, the entire keyboard went dead. No lights, no response from keys. Dead. After some reading, I found a cloud software driver and after securing a Windows PC, I was able to load the cloud driver and bring the keyboard back to life. I thought that was a win, and it was over. Next time I rebooted with this board plugged in, it completely wiped my settings, which I had set using the Windows PC. With it plugged into the Windows or a Mac PC, the keyboard worked as expected, no settings lost, light pattern the same. Not so with my Linux PCs, not one, but all. If the keyboard was plugged in during a reboot or shutdown, the keyboard not only lost all of it's settings, it was unusable until Xorg loaded, thus I was both unable to choose my boot partition, but I was also unable to unlock my LUKS partition unless I grabbed another keyboard and plugged it in.

At damn near $200 for a keyboard, without switches and keycaps, at minimum one would expect the keyboard to survive a reboot, no?

Well, I sent the keyboard in, explained to support the issue, and they said they'd forward it to their engineers and they would get back to me shortly. This continued for weeks, then months. I did research and found that some of the logic chips used in some mechanical keyboards, have specific logic for Apple hardware, which forces them to communicate slightly differently. Most manufacturers correct this via firmware, as it literally only requires about 32 bytes of code in order for it to communicate properly, but unfortunately, some don't.

I found that Keychron keyboards had the same issue, so I purchased one and delved into it. After many hours, I was able to pinpoint the exact issue(s). For me it was actually a combination of factors, due to using LUKS to encrypt my primary partition. Once I found the issue, the fix came shortly after. I corrected the order in which modules load in mkinitcpio.conf ensuring that the keyboard was ordered before the encrypt module and the apple_hid module needed to be added to hooks so that that tiny bit of code that works with Apple hardware, ensures proper functioning at boot time on a Linux machine.
