## <a name="prepare-your-raspberry-pi"></a>Készítse elő a Raspberry Pi

### <a name="install-raspbian"></a>Raspbian telepítése

Ha ez hello első alkalommal használja a málna Pi, meg kell tooinstall hello Raspbian operációs rendszer NOOBS SD-kártyán hello hello Kit része. Hello [málna Pi szoftver útmutató] [ lnk-install-raspbian] ismerteti, hogyan tooinstall a málna Pi operációs rendszerének. Ez az oktatóanyag feltételezi, hogy telepítette a málna Pi hello Raspbian operációs rendszer.

> [!NOTE]
> hello található hello SD-kártyát [a Microsoft Azure IoT Starter Kit málna Pi 3] [ lnk-starter-kits] már rendelkezik telepített NOOBS. Rendszerindító hello málna Pi a kártyáról, és válassza ki a tooinstall hello Raspbian operációs rendszer.

### <a name="set-up-hello-hardware"></a>Hello hardver beállítása

Ez az oktatóanyag használja hello szereplő hello BME280 érzékelő [a Microsoft Azure IoT Starter Kit málna Pi 3] [ lnk-starter-kits] toogenerate telemetriai adatokat. Hello málna Pi dolgozza fel a hello megoldás irányítópultról metódushívási egy LED tooindicate alkalmaz.

hello be táblán hello összetevők a következők:

- Piros LED
- 220 ohmos ellenállás (piros, a piros, a barna.)
- BME280 érzékelő

hello következő ábra azt mutatja, hogyan tooconnect a hardver:

![Málna Pi hardver beállítása][img-connection-diagram]

hello következő táblázat összefoglalja hello málna Pi toohello-összetevőket hello breadboard hello kapcsolatokat:

| Raspberry Pi            | Breadboard             |Szín         |
| ----------------------- | ---------------------- | ------------- |
| GND (14 PIN-kód)            | LED - nálói PIN-kódot (18A)      | Lila          |
| GPCLK0 (PIN-kód 7)          | Ellenállás [25]         | Narancssárga          |
| SPI_CE0 (PIN-kód 24)        | CS (39A)               | Kék          |
| SPI_SCLK (PIN-kód 23)       | SCK (36A)              | Sárga        |
| SPI_MISO (21 PIN-kód)       | SDO (37A)              | Fehér         |
| SPI_MOSI (PIN-kód 19)       | SDI (38A)              | Zöld         |
| GND (PIN-kód 6)             | GND (35A)              | Fekete         |
| 3.3 V (1. Pin)           | 3Vo (34A)              | Piros           |

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

### <a name="enable-spi"></a>SPI engedélyezése

Hello mintaalkalmazás futtatása előtt engedélyeznie kell a hello soros perifériák Interface (SPI) buszhoz hello málna Pi. hello málna Pi hello SPI bus keresztül kommunikál a hello BME280 érzékelő eszköz. Használja a következő parancs tooedit hello konfigurációs fájl hello:

```sh
sudo nano /boot/config.txt
```

Hello sor keresése:

`#dtparam=spi=on`

- toouncomment hello sor törlése hello `#` hello indításkor.
- Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).
- tooenable SPI, indítsa újra a hello málna Pi. Hello Terminálszolgáltatások újraindítás megszakad, újra Ha hello málna Pi újraindul a toosign van szüksége:

  ```sh
  sudo reboot
  ```


[img-connection-diagram]: media/iot-suite-raspberry-pi-kit-prepare-pi/rpi2_remote_monitoring.png

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/