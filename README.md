# Hackintosh-Dell-7580

## Tested and working with Ventura 13.1

`Opencore Version : 0.9.0`

For Monterey I recommend using: [queensferryme/Hackintosh-Dell-7580](https://github.com/queensferryme/Hackintosh-Dell-7580)
#

| Specifications | Detail                                           |
| ------------------- | ------------------------------------------- |
| Computer model      | Dell 7580 (MX 150)                          |
| Processor           | Intel Core i7-8565u                         |
| Memory              | 16GB 2400MHz DDR4                           |
| Integrated Graphics | Intel UHD Graphics 620                      |

For general installation instructions, please refer to [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/). This project is greatly inspired by [queensferryme/Hackintosh-Dell-7580](https://github.com/queensferryme/Hackintosh-Dell-7580) (which works great with Monterey)

## Working:

- Wifi ✅;
- Bluetooth ✅;
- Trackpad ✅;
- Intel UHD Graphics 620 ✅;
- Webcam ✅;
- Audio ✅;
- Microfone ✅;
- Network (Wired network) ✅;
- All usb and type-c (you are able to use this slot to charge your computer) ✅;
- Keyboard and commands fn, backlight shortcut (Fn + S or Fn + B) ✅;
- HDMI (image and audio) ✅;
- Battery and power display ✅;
- CPU frequency & power & temperature & utilization ✅;

## Not Working:
- MX 150 (Blame Nvidia) 🚫;
- AirDrop, continuity camera, continuity microphone (IPhone) (Need change wireless/bluetooth adapter for a native. example Bcm94360ng) 🚫;

## Notes

### Fix CFG Lock

If you have gone through the [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) carefullly enough, you may find that of all the [BIOS settings](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake.html#intel-bios-settings), disabling `CFG Lock` is particularly frustrating, since no GUI configuring option is provided.

In order to fix this issue, you will need to first:

1. Download `Utils/modGRUBShell.efi` (or from [datasone/grub-mod-setup_var](https://github.com/datasone/grub-mod-setup_var/releases/grub-mod-setup_var)) and move it to `EFI/BOOT`;
2. Restart the computer, press `F2` to go to the BIOS settings and choose to boot with `EFI/BOOT/modGRUBShell.efi`.

Note that normally we boot with `EFI/BOOT/BOOTx64.efi`， you may want to revert to the original state when `CFG Lock` got fixed.

By now you should find yourself in an interactive shell. You can run the following commands:

- `setup_var 0x5C3` to check `CFG Lock`: 0x00 means disabled, 0x01 means enabled;
- `setup_var 0x5C3 0x00` to disable `CFG Lock`;
- `setup_var 0x5C3 0x01` to enable  `CFG Lock` in case you want it back.

Then you are good to go.

### Fix headphone audio distortion

If you experience audio distortion when plugging in your headphone, please

1. Download `Utils/hda-verb` and move it to `/usr/local/bin`;
2. Download the `Utils/Launch.app` and move it to your Application folder in the Finder (or anywhere else you like). Then open the *Users & Groups* pane of System Preferences, click the *Login Items* tab and add *Launch.app* to the list.

Every time you log in from now on, the application will be automatically executed. What this app really does under the hood is to run two simple lines of shell commands (source https://www.elitemacx86.com/threads/fix-audio-distortion-when-using-headphones-on-laptops.185/) that will fix the problem:

```bash
/usr/local/bin/hda-verb 0x19 SET_PIN_WIDGET_CONTROL 0x25
/usr/local/bin/hda-verb 0x21 SET_UNSOLICITED_ENABLE 0x83
```

