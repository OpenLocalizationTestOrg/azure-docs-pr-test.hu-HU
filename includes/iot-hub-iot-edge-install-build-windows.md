## <a name="install-hello-prerequisites"></a>Hello előfeltételek telepítése

1. Telepítés [Visual Studio 2015-öt vagy 2017](https://www.visualstudio.com). Használhatja a hello Community Edition ingyenes, ha teljesülnek hello licencelési. Lehet, hogy tooinclude Visual C++ és NuGet Package Manager.

1. Telepítés [git](http://www.git-scm.com) és ellenőrizze, hogy git.exe hello parancssorból futtatható.

1. Telepítés [CMake](https://cmake.org/download/) és ellenőrizze, hogy cmake.exe hello parancssorból futtatható. CMake 3.7.2 verzió vagy újabb ajánlott. Hello **.msi** telepítő a Windows hello a legegyszerűbb lehetőség. Adja hozzá a CMake toohello elérési ÚTJÁT legalább hello aktuális felhasználó, amikor hello telepítő kéri.

1. Telepítés [Python 2.7](https://www.python.org/downloads/release/python-27). Mindenképpen adja hozzá a Python tooyour `PATH` környezeti változót **Vezérlőpult -> rendszer -> Speciális rendszerbeállítások -> környezeti változók**.

1. Parancsot egy parancssorba futtassa a következő parancs tooclone hello Azure IoT peremhálózati GitHub tárház tooyour helyi számítógép hello:

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-toobuild-hello-sample"></a>Hogyan toobuild hello minta

Most már lefordíthatja hello IoT peremhálózati futásidejű és minták a helyi számítógépen:

1. Nyissa meg a **Developer-parancssorból VS 2015** vagy **Developer-parancssorból VS 2017** parancssort.

1. Keresse meg a helyi példányát hello toohello gyökérmappáját **iot-edge** tárházba.

1. A build parancsfájl futtatása a következőképpen:

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

Ez a parancsfájl létrehoz egy Visual Studio megoldás fájlt, és hello megoldást hoz létre. Hello hello Visual Studio megoldás található **build** hello helyi példánya mappájában **iot-edge** tárházba. Ha szeretné, hogy toobuild, és hello egység tesztek futtatása, vegye fel a hello `--run-unittests` paraméter. Ha szeretné, hogy toobuild, és hello end tooend tesztek futtatása, vegye fel a hello `--run-e2e-tests`.

> [!NOTE]
> Minden alkalommal, amikor hello **build.cmd** parancsfájl, törli és állítja helyre hello **build** hello gyökérmappájában lévő mappának hello helyi példány **iot-edge** tárházba.
