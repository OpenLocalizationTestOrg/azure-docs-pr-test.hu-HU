## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="dc27e-101">Készítse elő a Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="dc27e-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="dc27e-102">Raspbian telepítése</span><span class="sxs-lookup"><span data-stu-id="dc27e-102">Install Raspbian</span></span>

<span data-ttu-id="dc27e-103">Ha ez az első alkalommal használja a málna Pi, telepítendő a Raspbian operációs rendszer NOOBS használja a csomag tartalmazza az SD-kártyára.</span><span class="sxs-lookup"><span data-stu-id="dc27e-103">If this is the first time you are using your Raspberry Pi, you need to install the Raspbian operating system using NOOBS on the SD card included in the kit.</span></span> <span data-ttu-id="dc27e-104">A [málna Pi szoftver útmutató] [ lnk-install-raspbian] ismerteti, hogyan lehet a málna Pi operációs rendszer telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="dc27e-104">The [Raspberry Pi Software Guide][lnk-install-raspbian] describes how to install an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="dc27e-105">Ez az oktatóanyag feltételezi, hogy telepítette a Raspbian operációs rendszer a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="dc27e-105">This tutorial assumes you have installed the Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="dc27e-106">Az SD-kártya szerepel a [a Microsoft Azure IoT Starter Kit málna Pi 3] [ lnk-starter-kits] már rendelkezik telepített NOOBS.</span><span class="sxs-lookup"><span data-stu-id="dc27e-106">The SD card included in the [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="dc27e-107">Indítsa el a málna Pi a kártyáról, és a Raspbian operációs rendszer telepítése.</span><span class="sxs-lookup"><span data-stu-id="dc27e-107">You can boot the Raspberry Pi from this card and choose to install the Raspbian OS.</span></span>

<span data-ttu-id="dc27e-108">A hardver-telepítés befejezéséhez kell:</span><span class="sxs-lookup"><span data-stu-id="dc27e-108">To complete the hardware setup, you need to:</span></span>

- <span data-ttu-id="dc27e-109">Csatlakoztassa a málna Pi Kit része tápegység.</span><span class="sxs-lookup"><span data-stu-id="dc27e-109">Connect your Raspberry Pi to the power supply included in the kit.</span></span>
- <span data-ttu-id="dc27e-110">A málna Pi csatlakoznak a hálózathoz, a csomag tartalmazza az Ethernet-kábelen keresztül.</span><span class="sxs-lookup"><span data-stu-id="dc27e-110">Connect your Raspberry Pi to your network using the Ethernet cable included in your kit.</span></span> <span data-ttu-id="dc27e-111">Másik lehetőségként beállíthatja [vezeték nélküli kapcsolat] [ lnk-pi-wireless] a málna pi.</span><span class="sxs-lookup"><span data-stu-id="dc27e-111">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="dc27e-112">Ezzel befejezte a málna pi a hardverbeállításokat.</span><span class="sxs-lookup"><span data-stu-id="dc27e-112">You have now completed the hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-the-terminal"></a><span data-ttu-id="dc27e-113">Bejelentkezhet és elérheti a Terminálszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="dc27e-113">Sign in and access the terminal</span></span>

<span data-ttu-id="dc27e-114">A Terminálszolgáltatások tesztkörnyezetben, a málna Pi eléréséhez két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="dc27e-114">You have two options to access a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="dc27e-115">Ha a figyelő a málna Pi csatlakoztatott és a billentyűzeten, használhatja a Raspbian grafikus felhasználói felület egy terminálablakot eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="dc27e-115">If you have a keyboard and monitor connected to your Raspberry Pi, you can use the Raspbian GUI to access a terminal window.</span></span>

- <span data-ttu-id="dc27e-116">A parancssorban meg az SSH használata a asztali gépen málna Pi eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="dc27e-116">Access the command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-the-gui"></a><span data-ttu-id="dc27e-117">A grafikus felhasználói felületen terminálablakot használni</span><span class="sxs-lookup"><span data-stu-id="dc27e-117">Use a terminal Window in the GUI</span></span>

<span data-ttu-id="dc27e-118">Az alapértelmezett Raspbian a hitelesítő adatok felhasználónév **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="dc27e-118">The default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="dc27e-119">A tálcán a grafikus felhasználói felületén, elindíthatja a **Terminálszolgáltatások** segédprogram használatával egy figyelőt a ikon.</span><span class="sxs-lookup"><span data-stu-id="dc27e-119">In the task bar in the GUI, you can launch the **Terminal** utility using the icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="dc27e-120">SSH bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dc27e-120">Sign in with SSH</span></span>

<span data-ttu-id="dc27e-121">SSH használható parancssori hozzáférést a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="dc27e-121">You can use SSH for command-line access to your Raspberry Pi.</span></span> <span data-ttu-id="dc27e-122">A cikk [SSH (Secure Shell)] [ lnk-pi-ssh] útmutatás a málna Pi SSH konfigurálása, valamint csatlakozhat a [Windows] [ lnk-ssh-windows] vagy [ Linux és Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="dc27e-122">The article [SSH (Secure Shell)][lnk-pi-ssh] describes how to configure SSH on your Raspberry Pi, and how to connect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="dc27e-123">Jelentkezzen be a felhasználónevet **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="dc27e-123">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="dc27e-124">Választható lehetőség: A málna Pi a mappa megosztása</span><span class="sxs-lookup"><span data-stu-id="dc27e-124">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="dc27e-125">Ha szükséges érdemes lehet a málna Pi a mappa megosztása az asztali környezetet.</span><span class="sxs-lookup"><span data-stu-id="dc27e-125">Optionally, you may want to share a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="dc27e-126">A mappa megosztása lehetővé teszi az előnyben részesített asztali szövegszerkesztővel (például [Visual Studio Code](https://code.visualstudio.com/) vagy [Sublime Text](http://www.sublimetext.com/)) a málna Pi használata helyett a fájlok szerkesztésére `nano` vagy `vi`.</span><span class="sxs-lookup"><span data-stu-id="dc27e-126">Sharing a folder enables you to use your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) to edit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="dc27e-127">A Windows mappa megosztásához a málna Pi Samba kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="dc27e-127">To share a folder with Windows, configure a Samba server on the Raspberry Pi.</span></span> <span data-ttu-id="dc27e-128">Másik megoldásként használhatja a beépített [SFTP](https://www.raspberrypi.org/documentation/remote-access/) kiszolgáló az asztalon egy SFTP-ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="dc27e-128">Alternatively, use the built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/