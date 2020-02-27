<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/35/Tux.svg/869px-Tux.svg.png" alt="linux" width="150"
align="right" >

# How to sign Kernel Modules

## _VirtualBox_
_If your system is using EFI Secure Boot you may need to sign the
kernel modules (vboxdrv, vboxnetflt, vboxnetadp, vboxpci) before you can load
them._

___

Fedora: **_(rpm base)_**

1. Install mokutil

```shell
    sudo dnf upgrade -y
    sudo dnf install modutil
```

2. Generate the signature file. You can later delete this files, if you want to.

```shell
    openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=VirtualBox/"
```

3. Add it to the Kernel.

```shell
    sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxdrv)
```

4. Register \
**NOTE**! Running the following command will ask you for  a password. Put the one you want. You will use this password only one time during the next reboot.
```shell
    sudo mokutil --import MOK.der
```

5. Reboot your machine to enter the MOK manager EFI utility.
    * A blue screen will appear
    ___
    * **SELECT**: Enroll MOK

    ![Select Enroll MOK](https://camo.githubusercontent.com/b21a46cd05e4f730f3243885e9bf48dd1ac8b9f4/68747470733a2f2f736f75726365776172652e6f72672f73797374656d7461702f77696b692f536563757265426f6f743f616374696f6e3d41747461636846696c6526646f3d676574267461726765743d53637265656e73686f745f6b766d2d726177686964652d36342d756566692d315f323031342d30322d32375f31345f30305f31335f63726f702e706e67)

    ___
    * **SELECT**: Continue

    ![Select Continue](https://camo.githubusercontent.com/bac4ce06716e65944934a342d68f5458e4e68380/68747470733a2f2f736f75726365776172652e6f72672f73797374656d7461702f77696b692f536563757265426f6f743f616374696f6e3d41747461636846696c6526646f3d676574267461726765743d53637265656e73686f745f6b766d2d726177686964652d36342d756566692d315f323031342d30322d32375f31345f30305f33355f63726f702e706e67)
    
    ___
    * **SELECT**: Yes

    ![Select Yes](https://camo.githubusercontent.com/b81d4cb38374e4f6f4c599812402b25987e6bea0/68747470733a2f2f736f75726365776172652e6f72672f73797374656d7461702f77696b692f536563757265426f6f743f616374696f6e3d41747461636846696c6526646f3d676574267461726765743d53637265656e73686f745f6b766d2d726177686964652d36342d756566692d315f323031342d30322d32375f31345f30305f34345f63726f702e706e67)

    ___
    * **ENTER**: The password from earlier

    ![Enter the password from earlier](https://camo.githubusercontent.com/f15a0972dfc2d101711d2d584125f14b52e87057/68747470733a2f2f736f75726365776172652e6f72672f73797374656d7461702f77696b692f536563757265426f6f743f616374696f6e3d41747461636846696c6526646f3d676574267461726765743d53637265656e73686f745f6b766d2d726177686964652d36342d756566692d315f323031342d30322d32375f31345f30305f35335f63726f702e706e67)

    ___
    * **SELECT** OK 
    
    ![Select OK](https://camo.githubusercontent.com/3e9aea5d9fbbce69a8ac599d9ceb4bbe9bfbaaeb/68747470733a2f2f736f75726365776172652e6f72672f73797374656d7461702f77696b692f536563757265426f6f743f616374696f6e3d41747461636846696c6526646f3d676574267461726765743d53637265656e73686f745f6b766d2d726177686964652d36342d756566692d315f323031342d30322d32375f31345f30315f30365f63726f702e706e67)

    Hmm... all done. Now start using VirtualBox. :wink:
