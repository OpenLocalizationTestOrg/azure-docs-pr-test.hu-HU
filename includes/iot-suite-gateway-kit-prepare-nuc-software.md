## <a name="build-iot-edge"></a><span data-ttu-id="34bc3-101">Az IoT-Edge összeállítása</span><span class="sxs-lookup"><span data-stu-id="34bc3-101">Build IoT Edge</span></span>

<span data-ttu-id="34bc3-102">Ez az oktatóanyag a távoli felügyeleti előkonfigurált megoldás hello egyéni IoT peremhálózati modulok toocommunicate használja.</span><span class="sxs-lookup"><span data-stu-id="34bc3-102">This tutorial uses custom IoT Edge modules toocommunicate with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="34bc3-103">Ezért toobuild hello IoT peremhálózati modulok egyéni forrás kódból van szüksége.</span><span class="sxs-lookup"><span data-stu-id="34bc3-103">Therefore, you need toobuild hello IoT Edge modules from custom source code.</span></span> <span data-ttu-id="34bc3-104">hello a következő szakaszok ismertetik, hogyan tooinstall IoT peremhálózati és -buildek hello egyéni IoT peremhálózati modult.</span><span class="sxs-lookup"><span data-stu-id="34bc3-104">hello following sections describe how tooinstall IoT Edge and build hello custom IoT Edge module.</span></span>

### <a name="install-iot-edge"></a><span data-ttu-id="34bc3-105">Az IoT-Edge telepítése</span><span class="sxs-lookup"><span data-stu-id="34bc3-105">Install IoT Edge</span></span>

<span data-ttu-id="34bc3-106">hello alábbi lépések bemutatják, hogyan tooinstall hello előre összeállított hello Intel NUC IoT peremhálózati szoftver:</span><span class="sxs-lookup"><span data-stu-id="34bc3-106">hello following steps describe how tooinstall hello pre-compiled IoT Edge software on hello Intel NUC:</span></span>

1. <span data-ttu-id="34bc3-107">Adja meg a szükséges hello intelligens csomag adattárak futtassa a következő parancsokat a hello Intel NUC hello:</span><span class="sxs-lookup"><span data-stu-id="34bc3-107">Configure hello required smart package repositories by running hello following commands on hello Intel NUC:</span></span>

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    <span data-ttu-id="34bc3-108">Adja meg `y` amikor hello parancs kéri túl**tartalmazzák ezt a csatornát?**.</span><span class="sxs-lookup"><span data-stu-id="34bc3-108">Enter `y` when hello command prompts you too**Include this channel?**.</span></span>

1. <span data-ttu-id="34bc3-109">Frissítés hello intelligens Csomagkezelő hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="34bc3-109">Update hello smart package manager by running hello following command:</span></span>

    ```bash
    smart update
    ```

1. <span data-ttu-id="34bc3-110">Hello Azure IoT biztonsági csomag telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="34bc3-110">Install hello Azure IoT Edge package by running hello following command:</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. <span data-ttu-id="34bc3-111">Futó hello "Hello world" minta hello telepítésének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="34bc3-111">Verify hello installation by running hello "Hello world" sample.</span></span> <span data-ttu-id="34bc3-112">Ez a minta egy hello world toohello log.txT üzenetfájlt öt másodpercenként írási műveletek.</span><span class="sxs-lookup"><span data-stu-id="34bc3-112">This sample writes a hello world message toohello log.txT file every five seconds.</span></span> <span data-ttu-id="34bc3-113">hello alábbi parancsok futtatnak hello "Hello world" példával:</span><span class="sxs-lookup"><span data-stu-id="34bc3-113">hello following commands run hello "Hello world" sample:</span></span>

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    <span data-ttu-id="34bc3-114">Figyelmen kívül hagyása **érvénytelen argumentum** üzenetek hello minta leállításakor.</span><span class="sxs-lookup"><span data-stu-id="34bc3-114">Ignore any **invalid argument** messages when you stop hello sample.</span></span>

    <span data-ttu-id="34bc3-115">A következő parancs tooview hello hello naplófájl tartalmát hello használata:</span><span class="sxs-lookup"><span data-stu-id="34bc3-115">Use hello following command tooview hello contents of hello log file:</span></span>

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a><span data-ttu-id="34bc3-116">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="34bc3-116">Troubleshooting</span></span>

<span data-ttu-id="34bc3-117">Ha hello hibaüzenet "nincs csomag util-linux-fejlesztői", akkor próbálja meg újraindítani az Intel NUC hello.</span><span class="sxs-lookup"><span data-stu-id="34bc3-117">If you receive hello error "No package provides util-linux-dev", try rebooting hello Intel NUC.</span></span>
