## <a name="install-hello-prerequisites"></a>Hello előfeltételek telepítése

Ebben az oktatóanyagban hello lépések azt feltételezik, Ubuntu Linux futtatja.

Nyisson meg egy kezelőfelületet, és futtassa a következő parancsok tooinstall hello csomagokat hello:

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

A hello rendszerhéjában futtassa a következő parancs tooclone hello Azure IoT peremhálózati GitHub tárház tooyour helyi számítógép hello:

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-toobuild-hello-sample"></a>Hogyan toobuild hello minta

Most már lefordíthatja hello IoT peremhálózati futásidejű és minták a helyi számítógépen:

1. Nyisson meg egy rendszerhéjat.

1. Keresse meg a helyi példányát hello toohello gyökérmappáját **iot-edge** tárházba.

1. A build parancsfájl futtatása a következőképpen:

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

A parancsfájl a a **cmake** segédprogram toocreate egy mappa neve a **build** hello gyökérmappában található azon a helyi másolat készítése a **iot-edge** tárház és egy makefile készítése. hello parancsfájl majd összeállít hello megoldás egység tesztek és a záró tooend tesztek kihagyása. Ha szeretné, hogy toobuild, és hello egység tesztek futtatása, vegye fel a hello `--run-unittests` paraméter. Ha szeretné, hogy toobuild, és hello end tooend tesztek futtatása, vegye fel a hello `--run-e2e-tests`.

> [!NOTE]
> Minden alkalommal, amikor hello **build.sh** parancsfájl, törli és állítja helyre hello **build** hello gyökérmappájában lévő mappának hello helyi példány **iot-edge** tárházba.
