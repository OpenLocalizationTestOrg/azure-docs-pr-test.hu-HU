## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="afbd3-101">Nézet hello megoldás irányítópultja</span><span class="sxs-lookup"><span data-stu-id="afbd3-101">View hello solution dashboard</span></span>

<span data-ttu-id="afbd3-102">hello megoldás irányítópultja toomanage telepített hello megoldás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="afbd3-102">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="afbd3-103">Például telemetriai adatainak megtekintése, eszközöket, és lehetőség módszerek meghívására.</span><span class="sxs-lookup"><span data-stu-id="afbd3-103">For example, you can view telemetry, add devices, and invoke methods.</span></span>

1. <span data-ttu-id="afbd3-104">Hello kiépítés befejeztével, miután az előkonfigurált megoldás hello csempe jelzi **készen**, válassza a **indítsa el** tooopen a távoli felügyeleti megoldás portál új lapon.</span><span class="sxs-lookup"><span data-stu-id="afbd3-104">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your remote monitoring solution portal in a new tab.</span></span>

    ![Indítsa el az előre konfigurált hello megoldás][img-launch-solution]

1. <span data-ttu-id="afbd3-106">Alapértelmezés szerint hello megoldás portal mutatja hello *irányítópult*.</span><span class="sxs-lookup"><span data-stu-id="afbd3-106">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="afbd3-107">Navigálhat tooother területek hello megoldás portál hello bal oldalán található hello hello menü segítségével.</span><span class="sxs-lookup"><span data-stu-id="afbd3-107">You can navigate tooother areas of hello solution portal using hello menu on hello left-hand side of hello page.</span></span>

    ![Az előre konfigurált távoli figyelési megoldás irányítópultja][img-menu]

## <a name="add-a-device"></a><span data-ttu-id="afbd3-109">Eszköz hozzáadása</span><span class="sxs-lookup"><span data-stu-id="afbd3-109">Add a device</span></span>

<span data-ttu-id="afbd3-110">Eszköz tooconnect előre konfigurált toohello megoldás, azt kell azonosítani magát tooIoT Hub érvényes hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="afbd3-110">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="afbd3-111">Hello megoldás irányítópultja hello eszközhitelesítő adatok kérhetnek le.</span><span class="sxs-lookup"><span data-stu-id="afbd3-111">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="afbd3-112">Hello eszközhitelesítő adatok szerepeljenek az ügyfélalkalmazás az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="afbd3-112">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="afbd3-113">Ha még nem tette meg, adjon hozzá egy egyéni tooyour távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="afbd3-113">If you haven't already done so, add a custom device tooyour remote monitoring solution.</span></span> <span data-ttu-id="afbd3-114">Teljes hello hello megoldás irányítópultja lépései:</span><span class="sxs-lookup"><span data-stu-id="afbd3-114">Complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="afbd3-115">Hello bal alsó sarkában hello irányítópultot, kattintson **hozzáad egy eszközt**.</span><span class="sxs-lookup"><span data-stu-id="afbd3-115">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>

   ![Eszköz hozzáadása][1]

1. <span data-ttu-id="afbd3-117">A hello **egyéni eszköz** panelen, kattintson a **új hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="afbd3-117">In hello **Custom Device** panel, click **Add new**.</span></span>

   ![Egyéni eszköz hozzáadása][2]

1. <span data-ttu-id="afbd3-119">Válassza a **Meghatározom a saját eszközazonosítómat** elemet.</span><span class="sxs-lookup"><span data-stu-id="afbd3-119">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="afbd3-120">Adja meg például az Eszközazonosítót **rasppi**, kattintson a **ellenőrizze azonosító** tooverify hello neve már nem használta a megoldásban, és kattintson **létrehozása** tooprovision hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="afbd3-120">Enter a Device ID such as **rasppi**, click **Check ID** tooverify you haven't already used hello name in your solution, and then click **Create** tooprovision hello device.</span></span>

   ![Eszközazonosító hozzáadása][3]

1. <span data-ttu-id="afbd3-122">Megjegyzés: hello eszköz hitelesítő adatok tétele (**Eszközazonosító**, **IoT Hub állomásnév**, és **eszközkulcs**).</span><span class="sxs-lookup"><span data-stu-id="afbd3-122">Make a note hello device credentials (**Device ID**, **IoT Hub Hostname**, and **Device Key**).</span></span> <span data-ttu-id="afbd3-123">Az ügyfélalkalmazás hello málna Pi kell ezen értékek tooconnect toohello távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="afbd3-123">Your client application on hello Raspberry Pi needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="afbd3-124">Ezután kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="afbd3-124">Then click **Done**.</span></span>

    ![Eszköz hitelesítő adatainak megtekintése][4]

1. <span data-ttu-id="afbd3-126">Jelölje ki az eszköz hello megoldás irányítópultjának hello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="afbd3-126">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="afbd3-127">Ezt követően a hello **eszközadatok** panelen, kattintson a **eszköz engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="afbd3-127">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="afbd3-128">hello az eszköz állapota ezután **futtató**.</span><span class="sxs-lookup"><span data-stu-id="afbd3-128">hello status of your device is now **Running**.</span></span> <span data-ttu-id="afbd3-129">Mostantól telemetriai adatok fogadhatók az eszközről és metódusok hello eszközön hello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="afbd3-129">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-launch-solution]: media/iot-suite-raspberry-pi-kit-view-solution/launch.png
[img-menu]: media/iot-suite-raspberry-pi-kit-view-solution/menu.png
[1]: media/iot-suite-raspberry-pi-kit-view-solution/suite0.png
[2]: media/iot-suite-raspberry-pi-kit-view-solution/suite1.png
[3]: media/iot-suite-raspberry-pi-kit-view-solution/suite2.png
[4]: media/iot-suite-raspberry-pi-kit-view-solution/suite3.png