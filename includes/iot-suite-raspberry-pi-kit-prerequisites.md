## <a name="prerequisites"></a><span data-ttu-id="83f2a-101">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="83f2a-101">Prerequisites</span></span>

<span data-ttu-id="83f2a-102">toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="83f2a-102">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="83f2a-103">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="83f2a-103">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="83f2a-104">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="83f2a-104">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="83f2a-105">Szükséges szoftverek</span><span class="sxs-lookup"><span data-stu-id="83f2a-105">Required software</span></span>

<span data-ttu-id="83f2a-106">SSH-ügyfél az Ön tooremotely hozzáférés hello parancs sor a hello málna Pi asztali gépen tooenable kell.</span><span class="sxs-lookup"><span data-stu-id="83f2a-106">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="83f2a-107">A Windows tartalmaz egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="83f2a-107">Windows does not include an SSH client.</span></span> <span data-ttu-id="83f2a-108">Azt javasoljuk, [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="83f2a-108">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="83f2a-109">A legtöbb Linux terjesztéseket, a Mac OS parancssori segédprogram hello a SSH közé</span><span class="sxs-lookup"><span data-stu-id="83f2a-109">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="83f2a-110">További információkért lásd: [SSH használatával Linux és Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="83f2a-110">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="83f2a-111">Szükséges hardver</span><span class="sxs-lookup"><span data-stu-id="83f2a-111">Required hardware</span></span>

<span data-ttu-id="83f2a-112">Egy asztali számítógépen tooenable meg tooconnect távolról toohello parancs sor a hello málna Pi.</span><span class="sxs-lookup"><span data-stu-id="83f2a-112">A desktop computer tooenable you tooconnect remotely toohello command line on hello Raspberry Pi.</span></span>

<span data-ttu-id="83f2a-113">[Microsoft IoT Starter Kit málna Pi 3] [ lnk-starter-kits] vagy ezzel egyenértékű összetevőket.</span><span class="sxs-lookup"><span data-stu-id="83f2a-113">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="83f2a-114">Ez az oktatóanyag használja a következő elemek hello Kit hello:</span><span class="sxs-lookup"><span data-stu-id="83f2a-114">This tutorial uses hello following items from hello kit:</span></span>

- <span data-ttu-id="83f2a-115">Raspberry Pi 3</span><span class="sxs-lookup"><span data-stu-id="83f2a-115">Raspberry Pi 3</span></span>
- <span data-ttu-id="83f2a-116">(A NOOBS) MicroSD-kártyán</span><span class="sxs-lookup"><span data-stu-id="83f2a-116">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="83f2a-117">A USB Mini-kábellel</span><span class="sxs-lookup"><span data-stu-id="83f2a-117">A USB Mini cable</span></span>
- <span data-ttu-id="83f2a-118">Kábel</span><span class="sxs-lookup"><span data-stu-id="83f2a-118">An Ethernet cable</span></span>
- <span data-ttu-id="83f2a-119">BME280 érzékelő</span><span class="sxs-lookup"><span data-stu-id="83f2a-119">BME280 sensor</span></span>
- <span data-ttu-id="83f2a-120">Breadboard</span><span class="sxs-lookup"><span data-stu-id="83f2a-120">Breadboard</span></span>
- <span data-ttu-id="83f2a-121">Átkötés fenyegetéseknek</span><span class="sxs-lookup"><span data-stu-id="83f2a-121">Jumper wires</span></span>
- <span data-ttu-id="83f2a-122">Ellenállás</span><span class="sxs-lookup"><span data-stu-id="83f2a-122">Resistors</span></span>
- <span data-ttu-id="83f2a-123">LED</span><span class="sxs-lookup"><span data-stu-id="83f2a-123">LEDs</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/