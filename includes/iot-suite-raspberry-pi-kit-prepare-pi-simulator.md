## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="9ceab-101">Készítse elő a Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="9ceab-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="9ceab-102">Raspbian telepítése</span><span class="sxs-lookup"><span data-stu-id="9ceab-102">Install Raspbian</span></span>

<span data-ttu-id="9ceab-103">Ha ez hello első alkalommal használja a málna Pi, meg kell tooinstall hello Raspbian operációs rendszer NOOBS SD-kártyán hello hello Kit része.</span><span class="sxs-lookup"><span data-stu-id="9ceab-103">If this is hello first time you are using your Raspberry Pi, you need tooinstall hello Raspbian operating system using NOOBS on hello SD card included in hello kit.</span></span> <span data-ttu-id="9ceab-104">Hello [málna Pi szoftver útmutató] [ lnk-install-raspbian] ismerteti, hogyan tooinstall a málna Pi operációs rendszerének.</span><span class="sxs-lookup"><span data-stu-id="9ceab-104">hello [Raspberry Pi Software Guide][lnk-install-raspbian] describes how tooinstall an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="9ceab-105">Ez az oktatóanyag feltételezi, hogy telepítette a málna Pi hello Raspbian operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="9ceab-105">This tutorial assumes you have installed hello Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="9ceab-106">hello található hello SD-kártyát [a Microsoft Azure IoT Starter Kit málna Pi 3] [ lnk-starter-kits] már rendelkezik telepített NOOBS.</span><span class="sxs-lookup"><span data-stu-id="9ceab-106">hello SD card included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="9ceab-107">Rendszerindító hello málna Pi a kártyáról, és válassza ki a tooinstall hello Raspbian operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="9ceab-107">You can boot hello Raspberry Pi from this card and choose tooinstall hello Raspbian OS.</span></span>

<span data-ttu-id="9ceab-108">toocomplete hello hardverbeállításokat, kell:</span><span class="sxs-lookup"><span data-stu-id="9ceab-108">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="9ceab-109">Csatlakoztassa a málna Pi toohello tápegység hello Kit része.</span><span class="sxs-lookup"><span data-stu-id="9ceab-109">Connect your Raspberry Pi toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="9ceab-110">Csatlakoztassa a málna Pi tooyour hálózati kábellel hello Ethernet az Kit része.</span><span class="sxs-lookup"><span data-stu-id="9ceab-110">Connect your Raspberry Pi tooyour network using hello Ethernet cable included in your kit.</span></span> <span data-ttu-id="9ceab-111">Másik lehetőségként beállíthatja [vezeték nélküli kapcsolat] [ lnk-pi-wireless] a málna pi.</span><span class="sxs-lookup"><span data-stu-id="9ceab-111">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="9ceab-112">Ezzel befejezte a málna pi hello hardverbeállításokat.</span><span class="sxs-lookup"><span data-stu-id="9ceab-112">You have now completed hello hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="9ceab-113">Bejelentkezhetnek és elérhetik a hello Terminálszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9ceab-113">Sign in and access hello terminal</span></span>

<span data-ttu-id="9ceab-114">Két beállítások tooaccess terminál környezettel rendelkezik a málna Pi meg:</span><span class="sxs-lookup"><span data-stu-id="9ceab-114">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="9ceab-115">Ha a billentyűzet és csatlakoztatott tooyour málna Pi figyelése, használhatja a hello Raspbian grafikus felhasználói Felülettel tooaccess egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="9ceab-115">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

- <span data-ttu-id="9ceab-116">Hozzáférés hello parancs vonalát. az SSH használata a asztali gépen málna Pi.</span><span class="sxs-lookup"><span data-stu-id="9ceab-116">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="9ceab-117">A grafikus felhasználói Felülettel hello terminálablakot használni</span><span class="sxs-lookup"><span data-stu-id="9ceab-117">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="9ceab-118">hello alapértelmezett Raspbian a hitelesítő adatok felhasználónév **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="9ceab-118">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="9ceab-119">Hello tálcán hello grafikus felhasználói Felülettel, elindíthatja a hello **Terminálszolgáltatások** segédprogram használatával egy figyelő hello ikon.</span><span class="sxs-lookup"><span data-stu-id="9ceab-119">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="9ceab-120">SSH bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9ceab-120">Sign in with SSH</span></span>

<span data-ttu-id="9ceab-121">Parancssori hozzáférést tooyour málna Pi SSH használható.</span><span class="sxs-lookup"><span data-stu-id="9ceab-121">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="9ceab-122">hello cikk [SSH (Secure Shell)] [ lnk-pi-ssh] ismerteti, hogyan tooconfigure a málna Pi az SSH és hogyan tooconnect a [Windows] [ lnk-ssh-windows] vagy [Linux és Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="9ceab-122">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="9ceab-123">Jelentkezzen be a felhasználónevet **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="9ceab-123">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="9ceab-124">Választható lehetőség: A málna Pi a mappa megosztása</span><span class="sxs-lookup"><span data-stu-id="9ceab-124">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="9ceab-125">Ha szükséges érdemes lehet tooshare egy mappát a málna Pi a az asztali környezetet.</span><span class="sxs-lookup"><span data-stu-id="9ceab-125">Optionally, you may want tooshare a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="9ceab-126">A mappa megosztása lehetővé teszi, hogy Ön toouse az előnyben részesített asztali szövegszerkesztőben (például [Visual Studio Code](https://code.visualstudio.com/) vagy [Sublime Text](http://www.sublimetext.com/)) használata helyett a málna Pi tooedit fájlok `nano` vagy `vi`.</span><span class="sxs-lookup"><span data-stu-id="9ceab-126">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="9ceab-127">tooshare egy mappát a Windows hello málna Pi Samba kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9ceab-127">tooshare a folder with Windows, configure a Samba server on hello Raspberry Pi.</span></span> <span data-ttu-id="9ceab-128">Másik megoldásként használhatja a hello beépített [SFTP](https://www.raspberrypi.org/documentation/remote-access/) kiszolgáló az asztalon egy SFTP-ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="9ceab-128">Alternatively, use hello built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/