x240
====
Arch Linux setup for a new Lenovo ThinkPad X240.

Windows 7 is pre-installed using MBR. Laptop supports UEFI, dual booting to Windows will requires a Windows bootloader change.

Following the [Arch Laptop wiki](https://wiki.archlinux.org/index.php/Laptop).

# BIOS Updates
BIOS updates are really worthwhile, I jumped from version 2.19 to 2.29, which fixed a slew of annoying bugs (failure to
suspend, the backlight oddness discussed below, etc).

# Configuration
## Backlight Keys
As of Kernel 3.18 and BIOS 2.29:
The backlight keys will work in the kernel if you set the kernel parameter `video.use_native_backlight=0`.
However, note that this will give you two interfaces under /sys/class/backlight, so some userland programs may get
confused.
The steplevel is small, 1% per keypress, so patience is required.

`xbacklight` works well with this configuration when you're using X.

(TODO: Work out why the intel_backlight device doesn't pickup brightness keypresses, but acpi_backlight0 does...)

### Older information
Previously to a BIOS update, ThinkPads hit [Kernel bug #51231](https://bugzilla.kernel.org/show_bug.cgi?id=51231),
where the BIOS tries to work around Windows 8 hardware compatibility. The number of backlight brightness steps is not as
reported by the BIOS, meaning that userland daemons get confused, and keypresses appear to take no action for a single
press (but multiple are required).
The recommended solution is to set the kernel parameter:
```
video.use_native_backlight=1
```
However, this causes backlight keypresses to be ignored by the Kernel.

(Note that video.use_native_backlight=1 is the default since kernel 3.16)


The alternative workaround of disabling the Windows 8 ACPI compatibility flag seems to be a better option from this
point of view. However, this results in two backlight interfaces being present in /sys/class/backlight/, so care may be
required with XOrg configuration.
```
acpi_osi="!Windows 2012"
```
Kernel parameters are set using your favourite bootmanager. (For grub, in GRUB_CMDLINE_LINUX_DEFAULT of
/etc/default/grub (use appropriate escaping for quotes), and then rebuilt using `grub-mkconfig -o /boot/grub/grub.cfg`)



# Links
* [Debian on Lenovo Thinkpad X240 - Gabriel Saldana](http://blog.gabrielsaldana.org/debian-on-lenovo-thinkpad-x240/)
* https://github.com/leoluk/thinkpad-stuff/wiki/Haswell-ThinkPad-problems
* http://www.splitbrain.org/blog/2014-03/08-lenovo_thinkpad_x240_xubuntu (lspci output)
* http://www.reddit.com/r/thinkpad/
* http://www.linlap.com/lenovo_thinkpad_x240
* https://cowthink.org/running-linux-on-thinkpad-t440s-haswell-ultrabook/ (backlight, clickpad)
* http://www.thinkwiki.org/wiki/Category:X240
* http://mydevelopedworld.wordpress.com/2013/11/30/how-to-configure-new-lenovo-x240-touchpad-on-ubuntu-13-10/
* https://wiki.archlinux.org/index.php/Touchpad_Synaptics

