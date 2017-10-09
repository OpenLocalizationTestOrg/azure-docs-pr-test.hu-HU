## <a name="install-hello-prerequisites"></a><span data-ttu-id="f38b9-101">Hello előfeltételek telepítése</span><span class="sxs-lookup"><span data-stu-id="f38b9-101">Install hello prerequisites</span></span>

1. <span data-ttu-id="f38b9-102">Telepítés [Visual Studio 2015-öt vagy 2017](https://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="f38b9-102">Install [Visual Studio 2015 or 2017](https://www.visualstudio.com).</span></span> <span data-ttu-id="f38b9-103">Használhatja a hello Community Edition ingyenes, ha teljesülnek hello licencelési.</span><span class="sxs-lookup"><span data-stu-id="f38b9-103">You can use hello free Community Edition if you meet hello licensing requirements.</span></span> <span data-ttu-id="f38b9-104">Lehet, hogy tooinclude Visual C++ és NuGet Package Manager.</span><span class="sxs-lookup"><span data-stu-id="f38b9-104">Be sure tooinclude Visual C++ and NuGet Package Manager.</span></span>

1. <span data-ttu-id="f38b9-105">Telepítés [git](http://www.git-scm.com) és ellenőrizze, hogy git.exe hello parancssorból futtatható.</span><span class="sxs-lookup"><span data-stu-id="f38b9-105">Install [git](http://www.git-scm.com) and make sure you can run git.exe from hello command line.</span></span>

1. <span data-ttu-id="f38b9-106">Telepítés [CMake](https://cmake.org/download/) és ellenőrizze, hogy cmake.exe hello parancssorból futtatható.</span><span class="sxs-lookup"><span data-stu-id="f38b9-106">Install [CMake](https://cmake.org/download/) and make sure you can run cmake.exe from hello command line.</span></span> <span data-ttu-id="f38b9-107">CMake 3.7.2 verzió vagy újabb ajánlott.</span><span class="sxs-lookup"><span data-stu-id="f38b9-107">CMake version 3.7.2 or later is recommended.</span></span> <span data-ttu-id="f38b9-108">Hello **.msi** telepítő a Windows hello a legegyszerűbb lehetőség.</span><span class="sxs-lookup"><span data-stu-id="f38b9-108">hello **.msi** installer is hello easiest option on Windows.</span></span> <span data-ttu-id="f38b9-109">Adja hozzá a CMake toohello elérési ÚTJÁT legalább hello aktuális felhasználó, amikor hello telepítő kéri.</span><span class="sxs-lookup"><span data-stu-id="f38b9-109">Add CMake toohello PATH for at least hello current user when hello installer prompts you.</span></span>

1. <span data-ttu-id="f38b9-110">Telepítés [Python 2.7](https://www.python.org/downloads/release/python-27).</span><span class="sxs-lookup"><span data-stu-id="f38b9-110">Install [Python 2.7](https://www.python.org/downloads/release/python-27).</span></span> <span data-ttu-id="f38b9-111">Mindenképpen adja hozzá a Python tooyour `PATH` környezeti változót **Vezérlőpult -> rendszer -> Speciális rendszerbeállítások -> környezeti változók**.</span><span class="sxs-lookup"><span data-stu-id="f38b9-111">Make sure you add Python tooyour `PATH` environment variable in **Control Panel -> System -> Advanced system settings -> Environment Variables**.</span></span>

1. <span data-ttu-id="f38b9-112">Parancsot egy parancssorba futtassa a következő parancs tooclone hello Azure IoT peremhálózati GitHub tárház tooyour helyi számítógép hello:</span><span class="sxs-lookup"><span data-stu-id="f38b9-112">At a command prompt, run hello following command tooclone hello Azure IoT Edge GitHub repository tooyour local machine:</span></span>

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-toobuild-hello-sample"></a><span data-ttu-id="f38b9-113">Hogyan toobuild hello minta</span><span class="sxs-lookup"><span data-stu-id="f38b9-113">How toobuild hello sample</span></span>

<span data-ttu-id="f38b9-114">Most már lefordíthatja hello IoT peremhálózati futásidejű és minták a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="f38b9-114">You can now build hello IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="f38b9-115">Nyissa meg a **Developer-parancssorból VS 2015** vagy **Developer-parancssorból VS 2017** parancssort.</span><span class="sxs-lookup"><span data-stu-id="f38b9-115">Open a **Developer Command Prompt for VS 2015** or **Developer Command Prompt for VS 2017** command prompt.</span></span>

1. <span data-ttu-id="f38b9-116">Keresse meg a helyi példányát hello toohello gyökérmappáját **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="f38b9-116">Navigate toohello root folder in your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="f38b9-117">A build parancsfájl futtatása a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="f38b9-117">Run the build script as follows:</span></span>

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

<span data-ttu-id="f38b9-118">Ez a parancsfájl létrehoz egy Visual Studio megoldás fájlt, és hello megoldást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f38b9-118">This script creates a Visual Studio solution file and builds hello solution.</span></span> <span data-ttu-id="f38b9-119">Hello hello Visual Studio megoldás található **build** hello helyi példánya mappájában **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="f38b9-119">You can find hello Visual Studio solution in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="f38b9-120">Ha szeretné, hogy toobuild, és hello egység tesztek futtatása, vegye fel a hello `--run-unittests` paraméter.</span><span class="sxs-lookup"><span data-stu-id="f38b9-120">If you want toobuild and run hello unit tests, add hello `--run-unittests` parameter.</span></span> <span data-ttu-id="f38b9-121">Ha szeretné, hogy toobuild, és hello end tooend tesztek futtatása, vegye fel a hello `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="f38b9-121">If you want toobuild and run hello end tooend tests, add hello `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="f38b9-122">Minden alkalommal, amikor hello **build.cmd** parancsfájl, törli és állítja helyre hello **build** hello gyökérmappájában lévő mappának hello helyi példány **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="f38b9-122">Every time you run hello **build.cmd** script, it deletes and then recreates hello **build** folder in hello root folder of your local copy of hello **iot-edge** repository.</span></span>
