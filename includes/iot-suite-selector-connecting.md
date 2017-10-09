> [!div class="op_single_selector"]
> * [<span data-ttu-id="218d9-101">C Windowson</span><span class="sxs-lookup"><span data-stu-id="218d9-101">C on Windows</span></span>](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [<span data-ttu-id="218d9-102">C Linuxon</span><span class="sxs-lookup"><span data-stu-id="218d9-102">C on Linux</span></span>](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [<span data-ttu-id="218d9-103">Node.js</span><span class="sxs-lookup"><span data-stu-id="218d9-103">Node.js</span></span>](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a><span data-ttu-id="218d9-104">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="218d9-104">Scenario overview</span></span>
<span data-ttu-id="218d9-105">Ebben a forgatókönyvben a következő telemetriai toohello távoli megfigyelési hello küldő eszköz létrehozása [előre konfigurált megoldás][lnk-what-are-preconfig-solutions]:</span><span class="sxs-lookup"><span data-stu-id="218d9-105">In this scenario, you create a device that sends hello following telemetry toohello remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="218d9-106">Külső hőmérséklet</span><span class="sxs-lookup"><span data-stu-id="218d9-106">External temperature</span></span>
* <span data-ttu-id="218d9-107">Belső hőmérséklet</span><span class="sxs-lookup"><span data-stu-id="218d9-107">Internal temperature</span></span>
* <span data-ttu-id="218d9-108">Páratartalom</span><span class="sxs-lookup"><span data-stu-id="218d9-108">Humidity</span></span>

<span data-ttu-id="218d9-109">Az egyszerűség kedvéért hello kód hello eszközön mintaértékek állít elő, de javasoljuk, tooextend hello minta valós érzékelők tooyour eszköz csatlakoztatása és valós telemetriai adatok küldését.</span><span class="sxs-lookup"><span data-stu-id="218d9-109">For simplicity, hello code on hello device generates sample values, but we encourage you tooextend hello sample by connecting real sensors tooyour device and sending real telemetry.</span></span>

<span data-ttu-id="218d9-110">hello eszköz is, képes toorespond toomethods hello megoldás irányítópultja meghívni, és tulajdonságértékeket hello megoldás irányítópultjának beállítása szükséges.</span><span class="sxs-lookup"><span data-stu-id="218d9-110">hello device is also able toorespond toomethods invoked from hello solution dashboard and desired property values set in hello solution dashboard.</span></span>

<span data-ttu-id="218d9-111">toocomplete ebben az oktatóanyagban egy aktív Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="218d9-111">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="218d9-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="218d9-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="218d9-113">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="218d9-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="218d9-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="218d9-114">Before you start</span></span>
<span data-ttu-id="218d9-115">Mielőtt bármilyen kódot írna az eszközhöz, ki kell építenie az előre konfigurált távoli figyelési megoldást, és meg kell adnia egy új egyéni eszközt ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="218d9-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="218d9-116">Az előre konfigurált távoli figyelési megoldás kiépítése</span><span class="sxs-lookup"><span data-stu-id="218d9-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="218d9-117">Ebben az oktatóanyagban létrehozhat hello eszköz küldi hello adatok tooan példányának [távoli megfigyelési] [ lnk-remote-monitoring] előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="218d9-117">hello device you create in this tutorial sends data tooan instance of hello [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="218d9-118">Ha még nem már kiépített hello távoli felügyeleti előkonfigurált megoldás az Azure-fiókjába, használja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="218d9-118">If you haven't already provisioned hello remote monitoring preconfigured solution in your Azure account, use hello following steps:</span></span>

1. <span data-ttu-id="218d9-119">A hello <https://www.azureiotsuite.com/> kattintson  **+**  toocreate megoldást.</span><span class="sxs-lookup"><span data-stu-id="218d9-119">On hello <https://www.azureiotsuite.com/> page, click **+** toocreate a solution.</span></span>
2. <span data-ttu-id="218d9-120">Kattintson a **válasszon** a hello **távoli megfigyelési** panel toocreate a megoldás.</span><span class="sxs-lookup"><span data-stu-id="218d9-120">Click **Select** on hello **Remote monitoring** panel toocreate your solution.</span></span>
3. <span data-ttu-id="218d9-121">A hello **létrehozása távoli felügyeleti megoldás** lapján adjon meg egy **megoldásnév** az Ön által választott, válassza ki a hello **régió** szeretné, hogy toodeploy, és válassza ki a hello Azure előfizetés toowant toouse.</span><span class="sxs-lookup"><span data-stu-id="218d9-121">On hello **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select hello **Region** you want toodeploy to, and select hello Azure subscription toowant toouse.</span></span> <span data-ttu-id="218d9-122">Ezután kattintson a **Megoldás létrehozása** parancsra.</span><span class="sxs-lookup"><span data-stu-id="218d9-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="218d9-123">Várjon, amíg a kiépítési folyamat hello befejeződik.</span><span class="sxs-lookup"><span data-stu-id="218d9-123">Wait until hello provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="218d9-124">előre konfigurált hello megoldások számlázható Azure-szolgáltatásokat használja.</span><span class="sxs-lookup"><span data-stu-id="218d9-124">hello preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="218d9-125">Ne feledje tooremove hello előkonfigurált megoldás az előfizetésből, amikor elkészült, azt tooavoid a szükségtelen díjakat.</span><span class="sxs-lookup"><span data-stu-id="218d9-125">Be sure tooremove hello preconfigured solution from your subscription when you are done with it tooavoid any unnecessary charges.</span></span> <span data-ttu-id="218d9-126">Teljesen eltávolíthatja a előkonfigurált megoldás az előfizetésből hello felkeresésével <https://www.azureiotsuite.com/> lap.</span><span class="sxs-lookup"><span data-stu-id="218d9-126">You can completely remove a preconfigured solution from your subscription by visiting hello <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="218d9-127">Ha létesítésének folyamatát kell használnia a távoli felügyeleti megoldás hello hello befejeződik, kattintson a **indítása** tooopen hello megoldás irányítópultja a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="218d9-127">When hello provisioning process for hello remote monitoring solution finishes, click **Launch** tooopen hello solution dashboard in your browser.</span></span>

![A megoldás irányítópultja][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a><span data-ttu-id="218d9-129">Az eszköz a távoli felügyeleti megoldás hello kiépítése</span><span class="sxs-lookup"><span data-stu-id="218d9-129">Provision your device in hello remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="218d9-130">Ha már kiépített egy eszközt a megoldásában, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="218d9-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="218d9-131">Hello ügyfélalkalmazás létrehozásakor tooknow hello eszköz hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="218d9-131">You need tooknow hello device credentials when you create hello client application.</span></span>
> 
> 

<span data-ttu-id="218d9-132">Eszköz tooconnect előre konfigurált toohello megoldás, azt kell azonosítani magát tooIoT Hub érvényes hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="218d9-132">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="218d9-133">Hello megoldás irányítópultja hello eszközhitelesítő adatok kérhetnek le.</span><span class="sxs-lookup"><span data-stu-id="218d9-133">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="218d9-134">Hello eszközhitelesítő adatok szerepeljenek az ügyfélalkalmazás az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="218d9-134">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="218d9-135">tooadd eszköz tooyour távoli figyelési megoldás, a következő teljes hello hello megoldás irányítópultja lépései:</span><span class="sxs-lookup"><span data-stu-id="218d9-135">tooadd a device tooyour remote monitoring solution, complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="218d9-136">Hello bal alsó sarkában hello irányítópultot, kattintson **hozzáad egy eszközt**.</span><span class="sxs-lookup"><span data-stu-id="218d9-136">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>
   
   ![Eszköz hozzáadása][1]
2. <span data-ttu-id="218d9-138">A hello **egyéni eszköz** panelen, kattintson a **új hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="218d9-138">In hello **Custom Device** panel, click **Add new**.</span></span>
   
   ![Egyéni eszköz hozzáadása][2]
3. <span data-ttu-id="218d9-140">Válassza a **Meghatározom a saját eszközazonosítómat** elemet.</span><span class="sxs-lookup"><span data-stu-id="218d9-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="218d9-141">Adja meg például az Eszközazonosítót **mydevice**, kattintson a **ellenőrizze azonosító** tooverify ugyanez a neve már nincs használatban, és kattintson a **létrehozása** tooprovision hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="218d9-141">Enter a Device ID such as **mydevice**, click **Check ID** tooverify that name isn't already in use, and then click **Create** tooprovision hello device.</span></span>
   
   ![Eszközazonosító hozzáadása][3]
4. <span data-ttu-id="218d9-143">Megjegyzés: hello eszköz tétele (Eszközazonosító IoT Hub állomásnév és eszközkulcs) hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="218d9-143">Make a note hello device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="218d9-144">Az ügyfélalkalmazás kell ezen értékek tooconnect toohello távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="218d9-144">Your client application needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="218d9-145">Ezután kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="218d9-145">Then click **Done**.</span></span>
   
    ![Eszköz hitelesítő adatainak megtekintése][4]
5. <span data-ttu-id="218d9-147">Jelölje ki az eszköz hello megoldás irányítópultjának hello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="218d9-147">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="218d9-148">Ezt követően a hello **eszközadatok** panelen, kattintson a **eszköz engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="218d9-148">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="218d9-149">hello az eszköz állapota ezután **futtató**.</span><span class="sxs-lookup"><span data-stu-id="218d9-149">hello status of your device is now **Running**.</span></span> <span data-ttu-id="218d9-150">Mostantól telemetriai adatok fogadhatók az eszközről és metódusok hello eszközön hello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="218d9-150">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/