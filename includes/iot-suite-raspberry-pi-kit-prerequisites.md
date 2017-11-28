## <a name="prerequisites"></a><span data-ttu-id="e62f0-101">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e62f0-101">Prerequisites</span></span>

<span data-ttu-id="e62f0-102">Az oktatóanyag elvégzéséhez aktív Azure-előfizetésre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="e62f0-102">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="e62f0-103">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="e62f0-103">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="e62f0-104">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="e62f0-104">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="e62f0-105">Szükséges szoftverek</span><span class="sxs-lookup"><span data-stu-id="e62f0-105">Required software</span></span>

<span data-ttu-id="e62f0-106">SSH-ügyfél van szüksége az asztali gépen ahhoz, hogy a parancssor a málna Pi a érheti el távolról.</span><span class="sxs-lookup"><span data-stu-id="e62f0-106">You need SSH client on your desktop machine to enable you to remotely access the command line on the Raspberry Pi.</span></span>

- <span data-ttu-id="e62f0-107">A Windows tartalmaz egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="e62f0-107">Windows does not include an SSH client.</span></span> <span data-ttu-id="e62f0-108">Azt javasoljuk, [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="e62f0-108">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="e62f0-109">A legtöbb Linux terjesztéseket, a Mac OS közé tartoznak az SSH parancssori segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="e62f0-109">Most Linux distributions and Mac OS include the command-line SSH utility.</span></span> <span data-ttu-id="e62f0-110">További információkért lásd: [SSH használatával Linux és Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="e62f0-110">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="e62f0-111">Szükséges hardver</span><span class="sxs-lookup"><span data-stu-id="e62f0-111">Required hardware</span></span>

<span data-ttu-id="e62f0-112">Ahhoz, hogy a parancssor a málna Pi a távoli csatlakozás egy asztali számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e62f0-112">A desktop computer to enable you to connect remotely to the command line on the Raspberry Pi.</span></span>

<span data-ttu-id="e62f0-113">[Microsoft IoT Starter Kit málna Pi 3] [ lnk-starter-kits] vagy ezzel egyenértékű összetevőket.</span><span class="sxs-lookup"><span data-stu-id="e62f0-113">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="e62f0-114">Ez az oktatóanyag a csomagot a következő elemeket használja:</span><span class="sxs-lookup"><span data-stu-id="e62f0-114">This tutorial uses the following items from the kit:</span></span>

- <span data-ttu-id="e62f0-115">Raspberry Pi 3</span><span class="sxs-lookup"><span data-stu-id="e62f0-115">Raspberry Pi 3</span></span>
- <span data-ttu-id="e62f0-116">(A NOOBS) MicroSD-kártyán</span><span class="sxs-lookup"><span data-stu-id="e62f0-116">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="e62f0-117">A USB Mini-kábellel</span><span class="sxs-lookup"><span data-stu-id="e62f0-117">A USB Mini cable</span></span>
- <span data-ttu-id="e62f0-118">Kábel</span><span class="sxs-lookup"><span data-stu-id="e62f0-118">An Ethernet cable</span></span>
- <span data-ttu-id="e62f0-119">BME280 érzékelő</span><span class="sxs-lookup"><span data-stu-id="e62f0-119">BME280 sensor</span></span>
- <span data-ttu-id="e62f0-120">Breadboard</span><span class="sxs-lookup"><span data-stu-id="e62f0-120">Breadboard</span></span>
- <span data-ttu-id="e62f0-121">Átkötés fenyegetéseknek</span><span class="sxs-lookup"><span data-stu-id="e62f0-121">Jumper wires</span></span>
- <span data-ttu-id="e62f0-122">Ellenállás</span><span class="sxs-lookup"><span data-stu-id="e62f0-122">Resistors</span></span>
- <span data-ttu-id="e62f0-123">LED</span><span class="sxs-lookup"><span data-stu-id="e62f0-123">LEDs</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/