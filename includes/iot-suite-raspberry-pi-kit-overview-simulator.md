## <a name="overview"></a><span data-ttu-id="d071b-101">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d071b-101">Overview</span></span>

<span data-ttu-id="d071b-102">Ebben az oktatóanyagban végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d071b-102">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="d071b-103">Telepítse a távoli felügyeleti előkonfigurált megoldás egy példányát az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="d071b-103">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="d071b-104">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d071b-104">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="d071b-105">Konfigurálja az eszközt, a számítógép és a távoli felügyeleti megoldás folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="d071b-105">Set up your device to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="d071b-106">Frissítse a mintakódot eszköz csatlakozni a távoli felügyeleti megoldás, és, amely megtalálható a megoldás irányítópultja szimulált telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="d071b-106">Update the sample device code to connect to the remote monitoring solution, and send simulated telemetry that you can view on the solution dashboard.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d071b-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d071b-107">Prerequisites</span></span>

<span data-ttu-id="d071b-108">Az oktatóanyag elvégzéséhez aktív Azure-előfizetésre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="d071b-108">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="d071b-109">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="d071b-109">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d071b-110">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d071b-110">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="d071b-111">Szükséges szoftverek</span><span class="sxs-lookup"><span data-stu-id="d071b-111">Required software</span></span>

<span data-ttu-id="d071b-112">SSH-ügyfél van szüksége az asztali gépen ahhoz, hogy a parancssor a málna Pi a érheti el távolról.</span><span class="sxs-lookup"><span data-stu-id="d071b-112">You need SSH client on your desktop machine to enable you to remotely access the command line on the Raspberry Pi.</span></span>

- <span data-ttu-id="d071b-113">A Windows tartalmaz egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="d071b-113">Windows does not include an SSH client.</span></span> <span data-ttu-id="d071b-114">Azt javasoljuk, [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="d071b-114">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="d071b-115">A legtöbb Linux terjesztéseket, a Mac OS közé tartoznak az SSH parancssori segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="d071b-115">Most Linux distributions and Mac OS include the command-line SSH utility.</span></span> <span data-ttu-id="d071b-116">További információkért lásd: [SSH használatával Linux és Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="d071b-116">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="d071b-117">Szükséges hardver</span><span class="sxs-lookup"><span data-stu-id="d071b-117">Required hardware</span></span>

<span data-ttu-id="d071b-118">Ahhoz, hogy a parancssor a málna Pi a távoli csatlakozás egy asztali számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d071b-118">A desktop computer to enable you to connect remotely to the command line on the Raspberry Pi.</span></span>

<span data-ttu-id="d071b-119">[Microsoft IoT Starter Kit málna Pi 3] [ lnk-starter-kits] vagy ezzel egyenértékű összetevőket.</span><span class="sxs-lookup"><span data-stu-id="d071b-119">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="d071b-120">Ez az oktatóanyag a csomagot a következő elemeket használja:</span><span class="sxs-lookup"><span data-stu-id="d071b-120">This tutorial uses the following items from the kit:</span></span>

- <span data-ttu-id="d071b-121">Raspberry Pi 3</span><span class="sxs-lookup"><span data-stu-id="d071b-121">Raspberry Pi 3</span></span>
- <span data-ttu-id="d071b-122">(A NOOBS) MicroSD-kártyán</span><span class="sxs-lookup"><span data-stu-id="d071b-122">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="d071b-123">A USB Mini-kábellel</span><span class="sxs-lookup"><span data-stu-id="d071b-123">A USB Mini cable</span></span>
- <span data-ttu-id="d071b-124">Kábel</span><span class="sxs-lookup"><span data-stu-id="d071b-124">An Ethernet cable</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/