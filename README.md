# Patched Mesa for Gaming (Arch)
A patched version of mesa tracking the version of 
[Bazzite](https://github.com/ublue-os/bazzite/tree/main/spec_files/mesa),
provided as an Arch package, based on the upstream mesa PKGBUILD.
It includes valve patches which improve frametime stability.

The packages are named as `handheld-*` to avoid auto-updating by pacman.
You may use the suffix when uninstalling as `sudo pacman -R handheld-*` and pressing
tab.

# How to install
```bash
git clone https://github.com/hhd-dev/patched-mesa patched-mesa
cd patched-mesa
updpkgsums; time makepkg -s -f --skippgpcheck

# You might need to press q for `makepkg` to continue

sudo pacman -U *.tar.zst
```