## <a name="prepare-your-raspberry-pi"></a>Készítse elő a Raspberry Pi

### <a name="install-raspbian"></a>Raspbian telepítése

Ha ez hello első alkalommal használja a málna Pi, meg kell tooinstall hello Raspbian operációs rendszer NOOBS SD-kártyán hello hello Kit része. Hello [málna Pi szoftver útmutató] [ lnk-install-raspbian] ismerteti, hogyan tooinstall a málna Pi operációs rendszerének. Ez az oktatóanyag feltételezi, hogy telepítette a málna Pi hello Raspbian operációs rendszer.

> [!NOTE]
> hello található hello SD-kártyát [a Microsoft Azure IoT Starter Kit málna Pi 3] [ lnk-starter-kits] már rendelkezik telepített NOOBS. Rendszerindító hello málna Pi a kártyáról, és válassza ki a tooinstall hello Raspbian operációs rendszer.

toocomplete hello hardverbeállításokat, kell:

- Csatlakoztassa a málna Pi toohello tápegység hello Kit része.
- Csatlakoztassa a málna Pi tooyour hálózati kábellel hello Ethernet az Kit része. Másik lehetőségként beállíthatja [vezeték nélküli kapcsolat] [ lnk-pi-wireless] a málna pi.

Ezzel befejezte a málna pi hello hardverbeállításokat.

### <a name="sign-in-and-access-hello-terminal"></a>Bejelentkezhetnek és elérhetik a hello Terminálszolgáltatások

Két beállítások tooaccess terminál környezettel rendelkezik a málna Pi meg:

- Ha a billentyűzet és csatlakoztatott tooyour málna Pi figyelése, használhatja a hello Raspbian grafikus felhasználói Felülettel tooaccess egy terminálablakot.

- Hozzáférés hello parancs vonalát. az SSH használata a asztali gépen málna Pi.

#### <a name="use-a-terminal-window-in-hello-gui"></a>A grafikus felhasználói Felülettel hello terminálablakot használni

hello alapértelmezett Raspbian a hitelesítő adatok felhasználónév **pi** és a jelszó **málna**. Hello tálcán hello grafikus felhasználói Felülettel, elindíthatja a hello **Terminálszolgáltatások** segédprogram használatával egy figyelő hello ikon.

#### <a name="sign-in-with-ssh"></a>SSH bejelentkezés

Parancssori hozzáférést tooyour málna Pi SSH használható. hello cikk [SSH (Secure Shell)] [ lnk-pi-ssh] ismerteti, hogyan tooconfigure a málna Pi az SSH és hogyan tooconnect a [Windows] [ lnk-ssh-windows] vagy [Linux és Mac OS][lnk-ssh-linux].

Jelentkezzen be a felhasználónevet **pi** és a jelszó **málna**.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>Választható lehetőség: A málna Pi a mappa megosztása

Ha szükséges érdemes lehet tooshare egy mappát a málna Pi a az asztali környezetet. A mappa megosztása lehetővé teszi, hogy Ön toouse az előnyben részesített asztali szövegszerkesztőben (például [Visual Studio Code](https://code.visualstudio.com/) vagy [Sublime Text](http://www.sublimetext.com/)) használata helyett a málna Pi tooedit fájlok `nano` vagy `vi`.

tooshare egy mappát a Windows hello málna Pi Samba kiszolgáló konfigurálása. Másik megoldásként használhatja a hello beépített [SFTP](https://www.raspberrypi.org/documentation/remote-access/) kiszolgáló az asztalon egy SFTP-ügyféllel.

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/