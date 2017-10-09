## <a name="build-iot-edge"></a>Az IoT-Edge összeállítása

Ez az oktatóanyag a távoli felügyeleti előkonfigurált megoldás hello egyéni IoT peremhálózati modulok toocommunicate használja. Ezért toobuild hello IoT peremhálózati modulok egyéni forrás kódból van szüksége. hello a következő szakaszok ismertetik, hogyan tooinstall IoT peremhálózati és -buildek hello egyéni IoT peremhálózati modult.

### <a name="install-iot-edge"></a>Az IoT-Edge telepítése

hello alábbi lépések bemutatják, hogyan tooinstall hello előre összeállított hello Intel NUC IoT peremhálózati szoftver:

1. Adja meg a szükséges hello intelligens csomag adattárak futtassa a következő parancsokat a hello Intel NUC hello:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Adja meg `y` amikor hello parancs kéri túl**tartalmazzák ezt a csatornát?**.

1. Frissítés hello intelligens Csomagkezelő hello a következő parancs futtatásával:

    ```bash
    smart update
    ```

1. Hello Azure IoT biztonsági csomag telepítése hello a következő parancs futtatásával:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. Futó hello "Hello world" minta hello telepítésének ellenőrzése. Ez a minta egy hello world toohello log.txT üzenetfájlt öt másodpercenként írási műveletek. hello alábbi parancsok futtatnak hello "Hello world" példával:

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Figyelmen kívül hagyása **érvénytelen argumentum** üzenetek hello minta leállításakor.

    A következő parancs tooview hello hello naplófájl tartalmát hello használata:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Hibaelhárítás

Ha hello hibaüzenet "nincs csomag util-linux-fejlesztői", akkor próbálja meg újraindítani az Intel NUC hello.
