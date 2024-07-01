# DPI - compileMethod

## Automation

This is now automated and is really easy.

1. Make sure that everything is saved.
1. Run
    ```bash
    ./dpi-compileMethod
    ```
1. If you are not running it as root, it will ask for your password to escalate privileges to root.
1. Once done, it will restart the user interface (lipstick). This takes several seconds.
1. If you don't get the expected results, reboot the phone.

## Original instructions

From [here](https://forum.sailfishos.org/t/ui-themer-missing-from-openrepos/2457/62).


I did some experimentation to get DPI changes working on 3.4 (I’m on the very latest 3.4.0.24), and I’ve discovered a way to make it work since dconf write no longer seems to work, even with the vendor locks disabled:

First, make sure you have the sailfish graphics packages installed:

devel-su pkcon install --allow-reinstall -y sailfish-content-graphics-default-z1.0-base sailfish-content-graphics-default-z1.25-base sailfish-content-graphics-default-z1.5-base sailfish-content-graphics-default-z1.75-base sailfish-content-graphics-default-z2.0-base sailfish-content-graphics-closed-z1.0 sailfish-content-graphics-closed-z1.25 sailfish-content-graphics-closed-z1.5 sailfish-content-graphics-closed-z1.75 sailfish-content-graphics-closed-z2.0

Make your edits, for example, I edited /etc/dconf/db/vendor.d/silica-configs.txt and set it to the following:

[desktop/sailfish/silica]
theme_pixel_ratio=1.25
theme_icon_subdir='z1.25'

After saving, I used dconf compile to recompile the keys directory into a binary, like so:

dconf compile /etc/dconf/db/vendor.new /etc/dconf/db/vendor.d

Then, backup the old vendor file:

devel-su
cd /etc/dconf/db
mv vendor{,.bak}

Then move your new vendor file into place:

mv vendor.new vendor

After rebooting, your DPI will be set, and your icons will show up since you have the graphics packages installed from earlier.

I just tested it on two XA2 Ultras, one fresh flashed, and one OTA’ed.

I hope this helps someone, at least. Let me know if you run into any issues.