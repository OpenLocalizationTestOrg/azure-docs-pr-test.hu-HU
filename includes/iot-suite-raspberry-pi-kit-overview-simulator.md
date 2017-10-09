## <a name="overview"></a><span data-ttu-id="aed0b-101">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="aed0b-101">Overview</span></span>

<span data-ttu-id="aed0b-102">Az oktatóanyag befejezése hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="aed0b-102">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="aed0b-103">Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése.</span><span class="sxs-lookup"><span data-stu-id="aed0b-103">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="aed0b-104">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="aed0b-104">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="aed0b-105">Az eszköz toocommunicate a számítógép és a távoli felügyeleti megoldás hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="aed0b-105">Set up your device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="aed0b-106">Hello eszköz kód tooconnect toohello távoli figyelési megoldást frissítése, és, hogy a hello megoldás irányítópulton megtekintheti szimulált telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="aed0b-106">Update hello sample device code tooconnect toohello remote monitoring solution, and send simulated telemetry that you can view on hello solution dashboard.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aed0b-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="aed0b-107">Prerequisites</span></span>

<span data-ttu-id="aed0b-108">toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="aed0b-108">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="aed0b-109">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="aed0b-109">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="aed0b-110">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="aed0b-110">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="aed0b-111">Szükséges szoftverek</span><span class="sxs-lookup"><span data-stu-id="aed0b-111">Required software</span></span>

<span data-ttu-id="aed0b-112">SSH-ügyfél az Ön tooremotely hozzáférés hello parancs sor a hello málna Pi asztali gépen tooenable kell.</span><span class="sxs-lookup"><span data-stu-id="aed0b-112">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="aed0b-113">A Windows tartalmaz egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="aed0b-113">Windows does not include an SSH client.</span></span> <span data-ttu-id="aed0b-114">Azt javasoljuk, [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="aed0b-114">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="aed0b-115">A legtöbb Linux terjesztéseket, a Mac OS parancssori segédprogram hello a SSH közé</span><span class="sxs-lookup"><span data-stu-id="aed0b-115">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="aed0b-116">További információkért lásd: [SSH használatával Linux és Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="aed0b-116">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="aed0b-117">Szükséges hardver</span><span class="sxs-lookup"><span data-stu-id="aed0b-117">Required hardware</span></span>

<span data-ttu-id="aed0b-118">Egy asztali számítógépen tooenable meg tooconnect távolról toohello parancs sor a hello málna Pi.</span><span class="sxs-lookup"><span data-stu-id="aed0b-118">A desktop computer tooenable you tooconnect remotely toohello command line on hello Raspberry Pi.</span></span>

<span data-ttu-id="aed0b-119">[Microsoft IoT Starter Kit málna Pi 3] [ lnk-starter-kits] vagy ezzel egyenértékű összetevőket.</span><span class="sxs-lookup"><span data-stu-id="aed0b-119">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="aed0b-120">Ez az oktatóanyag használja a következő elemek hello Kit hello:</span><span class="sxs-lookup"><span data-stu-id="aed0b-120">This tutorial uses hello following items from hello kit:</span></span>

- <span data-ttu-id="aed0b-121">Raspberry Pi 3</span><span class="sxs-lookup"><span data-stu-id="aed0b-121">Raspberry Pi 3</span></span>
- <span data-ttu-id="aed0b-122">(A NOOBS) MicroSD-kártyán</span><span class="sxs-lookup"><span data-stu-id="aed0b-122">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="aed0b-123">A USB Mini-kábellel</span><span class="sxs-lookup"><span data-stu-id="aed0b-123">A USB Mini cable</span></span>
- <span data-ttu-id="aed0b-124">Kábel</span><span class="sxs-lookup"><span data-stu-id="aed0b-124">An Ethernet cable</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/