## <a name="install-the-prerequisites"></a><span data-ttu-id="256ce-101">Az előfeltételek telepítése</span><span class="sxs-lookup"><span data-stu-id="256ce-101">Install the prerequisites</span></span>

<span data-ttu-id="256ce-102">A jelen oktatóanyagban szereplő lépések azt feltételezik, Ubuntu Linux futtatja.</span><span class="sxs-lookup"><span data-stu-id="256ce-102">The steps in this tutorial assume you are running Ubuntu Linux.</span></span>

<span data-ttu-id="256ce-103">Nyisson meg egy kezelőfelületet, és az előfeltételként szükséges csomagok telepítése a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="256ce-103">Open a shell and run the following commands to install the prerequisite packages:</span></span>

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

<span data-ttu-id="256ce-104">A rendszerhéj, az Azure IoT peremhálózati GitHub-tárházban, a helyi számítógép klónozása a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="256ce-104">In the shell, run the following command to clone the Azure IoT Edge GitHub repository to your local machine:</span></span>

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-to-build-the-sample"></a><span data-ttu-id="256ce-105">A minta létrehozása</span><span class="sxs-lookup"><span data-stu-id="256ce-105">How to build the sample</span></span>

<span data-ttu-id="256ce-106">Most már lefordíthatja az IoT él futásidejű és minták a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="256ce-106">You can now build the IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="256ce-107">Nyisson meg egy rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="256ce-107">Open a shell.</span></span>

1. <span data-ttu-id="256ce-108">Az **iot-edge** adattár helyi példányában lépjen a gyökérmappába.</span><span class="sxs-lookup"><span data-stu-id="256ce-108">Navigate to the root folder in your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="256ce-109">A build parancsfájl futtatása a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="256ce-109">Run the build script as follows:</span></span>

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

<span data-ttu-id="256ce-110">Ez a szkript a **cmake** segédprogramot használja a **build** nevű mappa létrehozásához az **iot-edge** adattár helyi példányának gyökérmappájában, valamint a makefile előállításához.</span><span class="sxs-lookup"><span data-stu-id="256ce-110">This script uses the **cmake** utility to create a folder called **build** in the root folder of your local copy of the **iot-edge** repository and generate a makefile.</span></span> <span data-ttu-id="256ce-111">Ezt követően a szkript felépíti a megoldást, és kihagyja az egységteszteket és a teljes körű teszteket.</span><span class="sxs-lookup"><span data-stu-id="256ce-111">The script then builds the solution, skipping unit tests and end to end tests.</span></span> <span data-ttu-id="256ce-112">Build a egység tesztek futtatása, és adja hozzá szeretné a `--run-unittests` paraméter.</span><span class="sxs-lookup"><span data-stu-id="256ce-112">If you want to build and run the unit tests, add the `--run-unittests` parameter.</span></span> <span data-ttu-id="256ce-113">Ha szeretne build és a végpontok közötti tesztek, vegye fel a `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="256ce-113">If you want to build and run the end to end tests, add the `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="256ce-114">A **build.sh** szkript minden futtatásakor törli, majd újra létrehozza a **build** mappát az **iot-edge** adattár helyi példányának gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="256ce-114">Every time you run the **build.sh** script, it deletes and then recreates the **build** folder in the root folder of your local copy of the **iot-edge** repository.</span></span>