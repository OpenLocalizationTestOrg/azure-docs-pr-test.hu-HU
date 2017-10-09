## <a name="create-a-device-identity"></a><span data-ttu-id="fa5e5-101">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa5e5-101">Create a device identity</span></span>

<span data-ttu-id="fa5e5-102">Ebben a szakaszban használhatja az hello [Azure-portálon] [ lnk-azure-portal] toocreate hello identitás rendszerleíró adatbázis az IoT hub egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-102">In this section, you use hello [Azure portal][lnk-azure-portal] toocreate a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="fa5e5-103">Egy eszköz nem lehet kapcsolódni a tooIoT hub, kivéve, ha azt egy bejegyzéssel rendelkezik hello identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-103">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="fa5e5-104">További információ a hello hello "Identitásjegyzékhez" című szakaszában talál [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="fa5e5-104">For more information, see hello "Identity registry" section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="fa5e5-105">Hello **eszköz Explorer** hello a portál segítségével hozzon létre egy eszköz egyedi azonosítója és kulcsa, hogy az eszköz használhatja magát tooidentify tooIoT Hub kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-105">hello **Device Explorer** in hello portal helps you generate a unique device ID and key that your device can use tooidentify itself when it connects tooIoT Hub.</span></span> <span data-ttu-id="fa5e5-106">Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="fa5e5-107">Győződjön meg arról, hogy be van jelentkezve toohello [Azure-portálon][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="fa5e5-107">Make sure you are signed in toohello [Azure portal][lnk-azure-portal].</span></span>

1. <span data-ttu-id="fa5e5-108">Hello Ugrósávon, kattintson **összes erőforrás** és az IoT hub erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-108">In hello Jumpbar, click **All resources** and find your IoT hub resource.</span></span>

    ![Keresse meg a tooyour Iot-központ][img-find-iothub]

1. <span data-ttu-id="fa5e5-110">Ha az IoT hub erőforrás már meg van nyitva, kattintson a hello **eszköz Explorer** eszközzel, és kattintson a **Hozzáadás** hello tetején.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-110">When your IoT hub resource is opened, click hello **Device Explorer** tool, and then click **Add** at hello top.</span></span> <span data-ttu-id="fa5e5-111">Adja meg, mint az új eszköz hello nevét **myDeviceId**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-111">Provide hello name for your new device, such as **myDeviceId**, and click **Save**.</span></span>

    ![Az eszközazonosító létrehozása a portálon][img-create-device]

   <span data-ttu-id="fa5e5-113">Ezzel létrehoz egy új eszközidentitás az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-113">This creates a new device identity for your IoT hub.</span></span>

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. <span data-ttu-id="fa5e5-114">A hello **eszköz Explorer**tartozó eszközök listája, kattintson az újonnan létrehozott hello eszközre, és jegyezze fel a hello **kapcsolati karakterlánc---elsődleges kulcs**.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-114">In hello **Device Explorer**'s device list, click hello newly created device and make note of hello **Connection string---primary key**.</span></span> 

    ![Eszköz kapcsolati karakterlánc][img-connection-string]

> [!NOTE]
> <span data-ttu-id="fa5e5-116">az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-116">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="fa5e5-117">Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, valamint az engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-117">It stores device IDs and keys toouse as security credentials, and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="fa5e5-118">Ha az alkalmazásnak toostore más eszközre vonatkozó metaadatok, akkor az alkalmazás-specifikus tárolási használjon.</span><span class="sxs-lookup"><span data-stu-id="fa5e5-118">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="fa5e5-119">További információkért lásd az [Azure IoT Hub fejlesztői útmutatóját][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="fa5e5-119">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

