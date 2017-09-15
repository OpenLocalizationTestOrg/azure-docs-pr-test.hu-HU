## <a name="build-iot-edge"></a><span data-ttu-id="ae65b-101">Az IoT-Edge összeállítása</span><span class="sxs-lookup"><span data-stu-id="ae65b-101">Build IoT Edge</span></span>

<span data-ttu-id="ae65b-102">Ez az oktatóanyag IoT peremhálózati az egyéni modulok kommunikálni a távoli figyelési előkonfigurált megoldást használja.</span><span class="sxs-lookup"><span data-stu-id="ae65b-102">This tutorial uses custom IoT Edge modules to communicate with the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="ae65b-103">Ezért kell felépítenie az IoT él modulok egyéni forrás kódból.</span><span class="sxs-lookup"><span data-stu-id="ae65b-103">Therefore, you need to build the IoT Edge modules from custom source code.</span></span> <span data-ttu-id="ae65b-104">Az alábbi szakaszok azt ismertetik, hogyan IoT peremhálózati telepítéséhez, és hozni az egyéni IoT peremhálózati modult.</span><span class="sxs-lookup"><span data-stu-id="ae65b-104">The following sections describe how to install IoT Edge and build the custom IoT Edge module.</span></span>

### <a name="install-iot-edge"></a><span data-ttu-id="ae65b-105">Az IoT-Edge telepítése</span><span class="sxs-lookup"><span data-stu-id="ae65b-105">Install IoT Edge</span></span>

<span data-ttu-id="ae65b-106">Az Intel NUC előre lefordított IoT peremhálózati szoftver telepítése a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="ae65b-106">The following steps describe how to install the pre-compiled IoT Edge software on the Intel NUC:</span></span>

1. <span data-ttu-id="ae65b-107">Adja meg a szükséges intelligens csomag tárházak találhatók az Intel NUC a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ae65b-107">Configure the required smart package repositories by running the following commands on the Intel NUC:</span></span>

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    <span data-ttu-id="ae65b-108">Adja meg `y` Ha a parancs felszólítja, hogy **tartalmazzák ezt a csatornát?**.</span><span class="sxs-lookup"><span data-stu-id="ae65b-108">Enter `y` when the command prompts you to **Include this channel?**.</span></span>

1. <span data-ttu-id="ae65b-109">Az intelligens Csomagkezelő frissítése a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ae65b-109">Update the smart package manager by running the following command:</span></span>

    ```bash
    smart update
    ```

1. <span data-ttu-id="ae65b-110">Telepítse az Azure IoT biztonsági csomagot a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ae65b-110">Install the Azure IoT Edge package by running the following command:</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. <span data-ttu-id="ae65b-111">Ellenőrizze a telepítést a "Hello world" példával futtatásával.</span><span class="sxs-lookup"><span data-stu-id="ae65b-111">Verify the installation by running the "Hello world" sample.</span></span> <span data-ttu-id="ae65b-112">Ez a minta hello world üzenet log.txT fájlba írja öt másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="ae65b-112">This sample writes a hello world message to the log.txT file every five seconds.</span></span> <span data-ttu-id="ae65b-113">Az alábbi parancsokat futtassa a "Hello world" példával:</span><span class="sxs-lookup"><span data-stu-id="ae65b-113">The following commands run the "Hello world" sample:</span></span>

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    <span data-ttu-id="ae65b-114">Figyelmen kívül hagyása **érvénytelen argumentum** üzenetek a minta leállításakor.</span><span class="sxs-lookup"><span data-stu-id="ae65b-114">Ignore any **invalid argument** messages when you stop the sample.</span></span>

    <span data-ttu-id="ae65b-115">A következő paranccsal tekintheti meg a naplófájl tartalmát:</span><span class="sxs-lookup"><span data-stu-id="ae65b-115">Use the following command to view the contents of the log file:</span></span>

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a><span data-ttu-id="ae65b-116">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="ae65b-116">Troubleshooting</span></span>

<span data-ttu-id="ae65b-117">Ha hibaüzenetet kap, a "nincs csomag biztosítja a fejlesztői linux util", próbálkozzon az Intel NUC újraindul.</span><span class="sxs-lookup"><span data-stu-id="ae65b-117">If you receive the error "No package provides util-linux-dev", try rebooting the Intel NUC.</span></span>
