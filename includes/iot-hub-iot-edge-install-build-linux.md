## <a name="install-hello-prerequisites"></a><span data-ttu-id="cd83c-101">Hello előfeltételek telepítése</span><span class="sxs-lookup"><span data-stu-id="cd83c-101">Install hello prerequisites</span></span>

<span data-ttu-id="cd83c-102">Ebben az oktatóanyagban hello lépések azt feltételezik, Ubuntu Linux futtatja.</span><span class="sxs-lookup"><span data-stu-id="cd83c-102">hello steps in this tutorial assume you are running Ubuntu Linux.</span></span>

<span data-ttu-id="cd83c-103">Nyisson meg egy kezelőfelületet, és futtassa a következő parancsok tooinstall hello csomagokat hello:</span><span class="sxs-lookup"><span data-stu-id="cd83c-103">Open a shell and run hello following commands tooinstall hello prerequisite packages:</span></span>

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

<span data-ttu-id="cd83c-104">A hello rendszerhéjában futtassa a következő parancs tooclone hello Azure IoT peremhálózati GitHub tárház tooyour helyi számítógép hello:</span><span class="sxs-lookup"><span data-stu-id="cd83c-104">In hello shell, run hello following command tooclone hello Azure IoT Edge GitHub repository tooyour local machine:</span></span>

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-toobuild-hello-sample"></a><span data-ttu-id="cd83c-105">Hogyan toobuild hello minta</span><span class="sxs-lookup"><span data-stu-id="cd83c-105">How toobuild hello sample</span></span>

<span data-ttu-id="cd83c-106">Most már lefordíthatja hello IoT peremhálózati futásidejű és minták a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="cd83c-106">You can now build hello IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="cd83c-107">Nyisson meg egy rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="cd83c-107">Open a shell.</span></span>

1. <span data-ttu-id="cd83c-108">Keresse meg a helyi példányát hello toohello gyökérmappáját **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="cd83c-108">Navigate toohello root folder in your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="cd83c-109">A build parancsfájl futtatása a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="cd83c-109">Run the build script as follows:</span></span>

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

<span data-ttu-id="cd83c-110">A parancsfájl a a **cmake** segédprogram toocreate egy mappa neve a **build** hello gyökérmappában található azon a helyi másolat készítése a **iot-edge** tárház és egy makefile készítése.</span><span class="sxs-lookup"><span data-stu-id="cd83c-110">This script uses the **cmake** utility toocreate a folder called **build** in hello root folder of your local copy of the **iot-edge** repository and generate a makefile.</span></span> <span data-ttu-id="cd83c-111">hello parancsfájl majd összeállít hello megoldás egység tesztek és a záró tooend tesztek kihagyása.</span><span class="sxs-lookup"><span data-stu-id="cd83c-111">hello script then builds hello solution, skipping unit tests and end tooend tests.</span></span> <span data-ttu-id="cd83c-112">Ha szeretné, hogy toobuild, és hello egység tesztek futtatása, vegye fel a hello `--run-unittests` paraméter.</span><span class="sxs-lookup"><span data-stu-id="cd83c-112">If you want toobuild and run hello unit tests, add hello `--run-unittests` parameter.</span></span> <span data-ttu-id="cd83c-113">Ha szeretné, hogy toobuild, és hello end tooend tesztek futtatása, vegye fel a hello `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="cd83c-113">If you want toobuild and run hello end tooend tests, add hello `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="cd83c-114">Minden alkalommal, amikor hello **build.sh** parancsfájl, törli és állítja helyre hello **build** hello gyökérmappájában lévő mappának hello helyi példány **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="cd83c-114">Every time you run hello **build.sh** script, it deletes and then recreates hello **build** folder in hello root folder of your local copy of hello **iot-edge** repository.</span></span>
