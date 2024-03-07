# Patched Mesa for Gaming (Arch)
A patched version of mesa using Valve's patches that improve frame time stability.
Provided as an Arch package, based on the upstream mesa PKGBUILD.
The current version is mesa 24.

The packages are named as `handheld-*` to avoid auto-updating by pacman.
You may use the suffix when uninstalling as `sudo pacman -R handheld-*` and pressing
tab.

> [!WARNING]  
> Currently on Arch, GDM will not launch with mesa 23, as it expects mesa 24.
> If you get an error saying to contact your system administrator, switch to
> a different terminal and do `sudo pacman -S mesa; sudo systemctl restart gdm`. 
> 
> Then, re-install this mesa if you want. 
> 
> If you get this error in the future, this is how to fix it.

> [!IMPORTANT]
> You might need to press Q for the install to continue.

# How to install
```bash
git clone https://github.com/hhd-dev/patched-mesa patched-mesa
cd patched-mesa
updpkgsums; time makepkg -s -f --skippgpcheck

# You might need to press q for `makepkg` to continue

sudo pacman -U *.tar.zst
```