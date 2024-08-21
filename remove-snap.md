# [Removing Snapd from Ubuntu](https://linuxconfig.org/uninstalling-snapd-on-ubuntu)

```bash
# list all snaps
snap list

# some snaps are dependent on other snaps, snap will tell you which can be first removed
# in the following dependency order: Notes -, Notes base, Notes snapd)
sudo snap remove firefox # (by dependency order)

# remove the snapd package itself, including configuration files
sudo apt purge snapd
sudo apt-get remove --purge gnome-software-plugin-snap
#sudo apt autoremove --purge snapd gnome-software-plugin-snap

# remove residual directories
rm -rf ~/snap
sudo rm -rf /snap
sudo rm -rf /var/snap
sudo rm -rf /var/lib/snapd
sudo rm -rf /var/cache/snapd/

# prevent snapd to be installed again
sudo apt-mark hold snapd

# verify that, snap will not get automatically reinstalled again via apt
# should fail to install: "E: Unable to correct problems, you have held broken packages"
sudo apt-get install chromium-browser
sudo apt install thunderbird

# reboot system
sudo reboot

# optional: install gnome-software as GUI app store WITHOUT snap plugin
sudo apt install gnome-software --no-install-recommends

# optional: allow snapd to be installed again automatically
#sudo apt-mark unhold snapd
```

- Link: [prevent snapd to be installed automatically again via apt](https://askubuntu.com/questions/1035915/how-to-remove-snap-from-ubuntu)

```bash
# prevent Ubuntu from automatically reinstalling snapd in the future
cat <<EOF | sudo tee /etc/apt/preferences.d/nosnap.pref
# To prevent repository packages from triggering the installation of Snap,
# this file forbids snapd from being installed by APT.
# For more information: https://linuxmint-user-guide.readthedocs.io/en/latest/snap.html

Package: snapd
Pin: release a=*
Pin-Priority: -10
EOF
```

- Link: [create special configuration file for APT, as LinuxMint did to prevent future snap installations](https://askubuntu.com/questions/1345385/how-can-i-stop-apt-from-installing-snap-packages/1345401#1345401)
