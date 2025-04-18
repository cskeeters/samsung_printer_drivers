The purpose of this repository is to enable the installation of drivers for Samsung printer on macOS.

The contents of `SamsungPrinterDrivers.pkg` have been added to this repository along with an `install` and `uninstall` script.  `install` will install the requisite files for your model, once you select the appropriate model using [fzf](https://github.com/junegunn/fzf).

If you are new to FZF, it's not hard.  Keep typing the name of your printer model until there is just one option and then press enter.

# Backstory

The drivers come from `SamsungPrinterDrivers.pkg`, which is located inside of `SamsungPrinterDrivers.dmg`, which is available [here](https://support.apple.com/en-au/106427).  If this pkg file could be installed, that would be it.  However it seems macOS at or newer than Monterey don't allow for installation of this pkg file and the following error is offered:

> This volume does not meet the requirements for this update.

The drivers inside have been reported to work so people have [figured out how](https://discussions.apple.com/thread/253611411?answerId=256951433022&sortBy=rank#256951433022) (and [confirmed on reddit](https://www.reddit.com/r/printers/comments/16nak1x/old_samsung_printer_on_mac_os_13/)) to use [Pacifist](https://www.charlessoft.com/) to extract the drivers and install them.  The idea is to extract the `.gz` of the PPD for your printer and manually install it.

This process didn't work off the bat for me.  I extracted and installed the `.gz` file, added the printer, but when I went to print something it said something about the software not being installed.  After `gunzip`ing the PPD, I saw that there were `.dynlib` files and other files that also needed to be installed.  Once I installed them, I was able to print.

I then realized that the process could be simplified by providing this repository that includes a bash installation script parse through the appropriate PPD file, extract the requisite files and install them all.

The script extracts the model names from the PPD files in each `.gz` file and allows the user to select their model by name.

# Installation

## Brew

Install [Homebrew](https://brew.sh/).

## FZF

Install FZF, the tool that you will select your Samsung printer model.

```sh
brew install fzf
```

## Samsung Drivers (this repo)

```sh
git clone http://github.com/cskeeters/samsung_print_drivers
cd samsung_print_drivers
sudo ./install
```

# Configuring

It appears to be easiest to configure the printer via the CUPS web interface.

If you don't know, macOS uses the *Common UNIX Printing System* (CUPS) to print documents.  It has a web interface that allows for the management and configuration of printers.

The web interface of CUPS is disabled by default, so to use it, we need to enable it first.

```sh
cupsctl WebInterface=yes
```

Then we can use a web browser to access the web interface on our local machine.

<http://127.0.0.1:631/printers>

You can add printers here, see the queue, but more importantly set parameters like defaulting to *High Quality* prints or a higher quality DPI that your printer may support.
