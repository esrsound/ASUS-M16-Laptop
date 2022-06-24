# Asus M16 Laptop

Kernel patch to support 165Hz:
https://raw.githubusercontent.com/xddxdd/pkgbuild/master/linux-xanmod-lantian/0003-intel-drm-use-max-clock.patch

## How to patch kernel for 165hz support?

Fetch the kernel build files:
`yay -G linux-lts` or linux if you on main arch kernel

`$ cd linux-lts`
type `makepkg -o` to have it fetch and unpack the kernel sources.

`$ cd linux-lts/repos/core-x86_64`


type `makepkg -o` to have it fetch and unpack the kernel sources. 

Import gpg key for kernel-lts or kernel:

`$ gpg --keyserver keys.gnupg.net --recv-keys 38DBBDC86092693E`


Copy linux-lts default config to src/.config
`$ cp config src/.config `
`$ cd src/linux-5.15.49/ `
 
Download patch for 165Hz

`$ wget https://raw.githubusercontent.com/xddxdd/pkgbuild/master/linux-xanmod-lantian/0003-intel-drm-use-max-clock.patch`

Patch the i915 module:

`$ patch -p1 < 0003-intel-drm-use-max-clock.patch`

Run `make menuconfig` and exit  (you will need kernel headers, so install it pacman -S linux-headers linux-lts-headers)

`$ make modules_prepare `
`$ make M=drivers/gpu/drm/i915`

Next, compress your new module:

`$ zstd drivers/gpu/drm/i915/i915.ko`

Copy module to kernel modules dir:

`$ sudo cp drivers/gpu/drm/i915/i915.ko.zst /usr/lib/modules/5.15.49-1-lts/kernel/drivers/gpu/drm/i915`

Reboot!
