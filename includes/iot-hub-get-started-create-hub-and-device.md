## <a name="create-an-iot-hub"></a><span data-ttu-id="d9849-101">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9849-101">Create an IoT hub</span></span>

1. <span data-ttu-id="d9849-102">A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **az eszközök internetes hálózatát** > **IoT-központ**.</span><span class="sxs-lookup"><span data-stu-id="d9849-102">In hello [Azure portal](https://portal.azure.com/), click **New** > **Internet of Things** > **IoT Hub**.</span></span>

   ![Hello Azure-portálon az IoT hub létrehozása](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. <span data-ttu-id="d9849-104">A hello **IoT-központ** panelen adja meg a következő adatokat az IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="d9849-104">In hello **IoT hub** pane, enter hello following information for your IoT hub:</span></span>

     <span data-ttu-id="d9849-105">**Név**: Adja meg az IoT hub hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d9849-105">**Name**: Enter hello name of your IoT hub.</span></span> <span data-ttu-id="d9849-106">Ha hello név érvényes, egy zöld pipa jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d9849-106">If hello name you enter is valid, a green check mark appears.</span></span>

     <span data-ttu-id="d9849-107">**Tarifa- és méretcsomag**: Select hello **F1 - szabad** réteg.</span><span class="sxs-lookup"><span data-stu-id="d9849-107">**Pricing and scale tier**: Select hello **F1 - Free** tier.</span></span> <span data-ttu-id="d9849-108">Ebben a demóban elegendő ezt a beállítást megadni.</span><span class="sxs-lookup"><span data-stu-id="d9849-108">This option is sufficient for this demo.</span></span> <span data-ttu-id="d9849-109">További információkért lásd: hello [árazás és méretcsomag](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="d9849-109">For more information, see hello [Pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

     <span data-ttu-id="d9849-110">**Erőforráscsoport**: létrehoz egy erőforrás csoport toohost hello IoT-központot, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="d9849-110">**Resource group**: Create a resource group toohost hello IoT hub or use an existing one.</span></span> <span data-ttu-id="d9849-111">További információkért lásd: [használata erőforráscsoportokat toomanage az Azure-erőforrások](../articles/azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d9849-111">For more information, see [Use resource groups toomanage your Azure resources](../articles/azure-resource-manager/resource-group-portal.md).</span></span>

     <span data-ttu-id="d9849-112">**Hely**: válassza ki a legközelebbi helyen tooyou hello IoT-központ létrehozási helyének hello.</span><span class="sxs-lookup"><span data-stu-id="d9849-112">**Location**: Select hello closest location tooyou where hello IoT hub is created.</span></span>

     <span data-ttu-id="d9849-113">**PIN-kód toodashboard**: válassza ezt a beállítást, mennyire egyszerű a hozzáférés tooyour IoT hub hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="d9849-113">**Pin toodashboard**: Select this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![Adjon meg információt toocreate az IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. <span data-ttu-id="d9849-115">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d9849-115">Click **Create**.</span></span> <span data-ttu-id="d9849-116">Az IoT hub eltarthat néhány percig toocreate.</span><span class="sxs-lookup"><span data-stu-id="d9849-116">Your IoT hub might take a few minutes toocreate.</span></span> <span data-ttu-id="d9849-117">Láthatja, hogy a folyamat állapotát a hello **értesítések** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="d9849-117">You can see progress in hello **Notifications** pane.</span></span>

   ![Az IoT Hub folyamatértesítéseinek megtekintése](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. <span data-ttu-id="d9849-119">Az IoT hub létrehozása után kattintson rá a hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="d9849-119">After your IoT hub is created, click it on hello dashboard.</span></span> <span data-ttu-id="d9849-120">Jegyezze fel a hello **állomásnév**, és kattintson a **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="d9849-120">Make a note of hello **Hostname**, and then click **Shared access policies**.</span></span>

   ![Az IoT hub hello nevének beolvasása](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. <span data-ttu-id="d9849-122">A hello **megosztott elérési házirendek** ablaktáblán hello kattintson **iothubowner** házirend, és ezután másolása és jegyezze fel az hello **kapcsolati karakterlánc** az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d9849-122">In hello **Shared access policies** pane, click hello **iothubowner** policy, and then copy and make a note of hello **Connection string** of your IoT hub.</span></span> <span data-ttu-id="d9849-123">További információkért lásd: [vezérlő hozzáférés tooIoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).</span><span class="sxs-lookup"><span data-stu-id="d9849-123">For more information, see [Control access tooIoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).</span></span>

> [!NOTE] 
<span data-ttu-id="d9849-124">Ennek a beállítási oktatóanyagnak az elvégzéséhez szüksége lesz erre az iothubowner kapcsolati karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="d9849-124">You will not need this iothubowner connection string for this set-up tutorial.</span></span> <span data-ttu-id="d9849-125">Azonban szükség lehet az egyes hello oktatóanyagok a különböző IoT-forgatókönyvek esetén a telepítés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="d9849-125">However, you may need it for some of hello tutorials on different IoT scenarios after you complete this set-up.</span></span>

   ![Az IoT Hub kapcsolati karakterláncának beszerzése](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-hello-iot-hub-for-your-device"></a><span data-ttu-id="d9849-127">Eszköz regisztrálása az IoT-központ hello az eszközhöz</span><span class="sxs-lookup"><span data-stu-id="d9849-127">Register a device in hello IoT hub for your device</span></span>

1. <span data-ttu-id="d9849-128">A hello [Azure-portálon](https://portal.azure.com/), nyissa meg az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d9849-128">In hello [Azure portal](https://portal.azure.com/), open your IoT hub.</span></span>

2. <span data-ttu-id="d9849-129">Kattintson a **Device Explorer** elemre.</span><span class="sxs-lookup"><span data-stu-id="d9849-129">Click **Device Explorer**.</span></span>
3. <span data-ttu-id="d9849-130">Hello eszköz Explorer ablaktáblában kattintson **Hozzáadás** tooadd eszköz tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="d9849-130">In hello Device Explorer pane, click **Add** tooadd a device tooyour IoT hub.</span></span> <span data-ttu-id="d9849-131">Majd hello a következő:</span><span class="sxs-lookup"><span data-stu-id="d9849-131">Then do hello following:</span></span>

   <span data-ttu-id="d9849-132">**Eszközazonosító**: Adja meg az új eszköz hello hello Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d9849-132">**Device ID**: Enter hello ID of hello new device.</span></span> <span data-ttu-id="d9849-133">Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="d9849-133">Device IDs are case sensitive.</span></span>

   <span data-ttu-id="d9849-134">**Hitelesítés típusa**: Válassza a **Szimmetrikus kulcs** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d9849-134">**Authentication Type**: Select **Symmetric Key**.</span></span>

   <span data-ttu-id="d9849-135">**Kulcsok automatikus létrehozása**: Jelölje be ezt a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d9849-135">**Auto Generate Keys**: Select this check box.</span></span>

   <span data-ttu-id="d9849-136">**Eszköz tooIoT központi csatlakozás**: kattintson a **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="d9849-136">**Connect device tooIoT Hub**: Click **Enable**.</span></span>

   ![Hozzáad egy eszközt a hello eszköz Explorer, az IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. <span data-ttu-id="d9849-138">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d9849-138">Click **Save**.</span></span>
5. <span data-ttu-id="d9849-139">Hello eszköz létrehozását követően nyissa meg hello eszköz hello **eszköz Explorer** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="d9849-139">After hello device is created, open hello device in hello **Device Explorer** pane.</span></span>
6. <span data-ttu-id="d9849-140">Jegyezze fel az elsődleges kulcs hello hello kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d9849-140">Make a note of hello primary key of hello connection string.</span></span>

   ![Hello eszköz kapcsolati karakterlánc beolvasása](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
