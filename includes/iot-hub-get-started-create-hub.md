## <a name="create-an-iot-hub"></a><span data-ttu-id="89b96-101">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="89b96-101">Create an IoT hub</span></span>
<span data-ttu-id="89b96-102">A szimulált eszköz alkalmazás tooconnect, hogy az IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="89b96-102">Create an IoT hub for your simulated device app tooconnect to.</span></span> <span data-ttu-id="89b96-103">hello következő lépések bemutatják, hogyan toocomplete ennek a feladatnak a által használatával hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="89b96-103">hello following steps show you how toocomplete this task by using hello Azure portal.</span></span>

1. <span data-ttu-id="89b96-104">Jelentkezzen be toohello [Azure-portálon][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="89b96-104">Sign in toohello [Azure portal][lnk-portal].</span></span>
1. <span data-ttu-id="89b96-105">Hello Ugrósávon, kattintson **új** > **az eszközök internetes hálózatát** > **IoT-központ**.</span><span class="sxs-lookup"><span data-stu-id="89b96-105">In hello Jumpbar, click **New** > **Internet of Things** > **IoT Hub**.</span></span>
   
    ![Azure Portal – ugrósáv][1]
1. <span data-ttu-id="89b96-107">A hello **IoT-központ** panelen válassza ki az IoT hub hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="89b96-107">In hello **IoT hub** blade, choose hello configuration for your IoT hub.</span></span>
   
    ![IoT Hub panel][2]
   
   1. <span data-ttu-id="89b96-109">A hello **neve** mezőbe írja be az IoT hub nevét.</span><span class="sxs-lookup"><span data-stu-id="89b96-109">In hello **Name** box, enter a name for your IoT hub.</span></span> <span data-ttu-id="89b96-110">Ha hello **neve** érvényes és elérhető, egy zöld pipa jelenik meg hello **neve** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="89b96-110">If hello **Name** is valid and available, a green check mark appears in hello **Name** box.</span></span>
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. <span data-ttu-id="89b96-111">Válasszon ki egy [tarifacsomagot és méretet][lnk-pricing].</span><span class="sxs-lookup"><span data-stu-id="89b96-111">Select a [pricing and scale tier][lnk-pricing].</span></span> <span data-ttu-id="89b96-112">Az oktatóanyag teljesítéséhez nem kell egy konkrét csomagot kiválasztani.</span><span class="sxs-lookup"><span data-stu-id="89b96-112">This tutorial does not require a specific tier.</span></span> <span data-ttu-id="89b96-113">Ebben az oktatóanyagban hello ingyenes F1 csomagot használja.</span><span class="sxs-lookup"><span data-stu-id="89b96-113">For this tutorial, use hello free F1 tier.</span></span>
   1. <span data-ttu-id="89b96-114">Az **Erőforráscsoport** mezőben hozzon létre egy erőforráscsoportot, vagy válasszon ki egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="89b96-114">In **Resource group**, either create a resource group, or select an existing one.</span></span> <span data-ttu-id="89b96-115">További információkért lásd: [erőforrás segítségével csoportosítja toomanage az Azure-erőforrások][lnk-resource-groups].</span><span class="sxs-lookup"><span data-stu-id="89b96-115">For more information, see [Using resource groups toomanage your Azure resources][lnk-resource-groups].</span></span>
   1. <span data-ttu-id="89b96-116">A **hely**, válassza ki hello hely toohost az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="89b96-116">In **Location**, select hello location toohost your IoT hub.</span></span> <span data-ttu-id="89b96-117">A jelen oktatóanyag esetében válassza az Önhöz legközelebb eső helyet.</span><span class="sxs-lookup"><span data-stu-id="89b96-117">For this tutorial, choose your nearest location.</span></span>
1. <span data-ttu-id="89b96-118">Az IoT Hub konfigurációs beállításainak kiválasztása után kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="89b96-118">When you have chosen your IoT hub configuration options, click **Create**.</span></span>  <span data-ttu-id="89b96-119">Az IoT hub az Azure toocreate néhány percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="89b96-119">It can take a few minutes for Azure toocreate your IoT hub.</span></span> <span data-ttu-id="89b96-120">toocheck hello állapotát, figyelheti a Kezdőpulton hello vagy hello értesítések panelen hello előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="89b96-120">toocheck hello status, you can monitor hello progress on hello Startboard or in hello Notifications panel.</span></span>
   
    ![Az új IoT Hub állapota][3]
1. <span data-ttu-id="89b96-122">Ha hello IoT hub létrehozása sikeres, kattintson a hello új csempe az IoT hub hello Azure portál tooopen hello panelen hello új IoT hub.</span><span class="sxs-lookup"><span data-stu-id="89b96-122">When hello IoT hub has been created successfully, click hello new tile for your IoT hub in hello Azure portal tooopen hello blade for hello new IoT hub.</span></span> <span data-ttu-id="89b96-123">Jegyezze fel a hello **állomásnév**, és kattintson a **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="89b96-123">Make a note of hello **Hostname**, and then click **Shared access policies**.</span></span>
   
    ![Új IoT Hub panel][4]
1. <span data-ttu-id="89b96-125">A hello **megosztott elérési házirendek** panelen kattintson hello **iothubowner** házirend, majd másolja ki és jegyezze fel az IoT-központ kapcsolati karakterlánc hello hello a **iothubowner** panelen.</span><span class="sxs-lookup"><span data-stu-id="89b96-125">In hello **Shared access policies** blade, click hello **iothubowner** policy, and then copy and make note of hello IoT Hub connection string in hello **iothubowner** blade.</span></span> <span data-ttu-id="89b96-126">További információkért lásd: [hozzáférés-vezérlés] [ lnk-access-control] a hello "IoT Hub developer guide."</span><span class="sxs-lookup"><span data-stu-id="89b96-126">For more information, see [Access control][lnk-access-control] in hello "IoT Hub developer guide."</span></span>
   
    ![Megosztott hozzáférési házirend panel][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
