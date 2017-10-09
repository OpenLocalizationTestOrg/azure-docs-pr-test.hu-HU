## <a name="create-a-device-identity"></a><span data-ttu-id="f438b-101">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f438b-101">Create a device identity</span></span>
<span data-ttu-id="f438b-102">Ebben a szakaszban egy .NET-Konzolalkalmazás, amely létrehoz egy eszközidentitás hello identitásjegyzékhez a az IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f438b-102">In this section, you create a .NET console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="f438b-103">Egy eszköz nem lehet kapcsolódni a tooIoT hub, kivéve, ha azt egy bejegyzéssel rendelkezik hello identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="f438b-103">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="f438b-104">További információ a hello hello "Identitásjegyzékhez" című szakaszában talál [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="f438b-104">For more information, see hello "Identity registry" section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="f438b-105">A konzol alkalmazás futtatásakor egy eszköz egyedi Azonosítót hoz létre, és az, hogy az eszköz használhatja tooidentify magát, eszköz-felhő küld a kulcs üzenetek tooIoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f438b-105">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span> <span data-ttu-id="f438b-106">Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="f438b-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="f438b-107">A Visual Studio, a Visual C# klasszikus Windows asztal projekt tooa új megoldás hozzáadása hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="f438b-107">In Visual Studio, add a Visual C# Windows Classic Desktop project tooa new solution by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="f438b-108">Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f438b-108">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="f438b-109">Név hello projekt **CreateDeviceIdentity** és name hello megoldás **IoTHubGetStarted**.</span><span class="sxs-lookup"><span data-stu-id="f438b-109">Name hello project **CreateDeviceIdentity** and name hello solution **IoTHubGetStarted**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10]
2. <span data-ttu-id="f438b-111">A Megoldáskezelőben kattintson a jobb gombbal hello **CreateDeviceIdentity** projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="f438b-111">In Solution Explorer, right-click hello **CreateDeviceIdentity** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="f438b-112">A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="f438b-112">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="f438b-113">Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="f438b-113">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][11]
4. <span data-ttu-id="f438b-115">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="f438b-115">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. <span data-ttu-id="f438b-116">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="f438b-116">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="f438b-117">Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="f438b-117">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="f438b-118">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="f438b-118">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }
   
    <span data-ttu-id="f438b-119">Ez a metódus egy eszközidentitást hoz létre a **myFirstDevice** azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="f438b-119">This method creates a device identity with ID **myFirstDevice**.</span></span> <span data-ttu-id="f438b-120">(Ha az eszköz azonosító már létezik hello identitásjegyzékhez, hello kód egyszerűen hello meglévő eszköz adatainak beolvasása.) hello app majd hello elsődleges kulcs az identitásukat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f438b-120">(If that device ID already exists in hello identity registry, hello code simply retrieves hello existing device information.) hello app then displays hello primary key for that identity.</span></span> <span data-ttu-id="f438b-121">Ez a kulcs hello szimulált eszköz, alkalmazás tooconnect tooyour IoT-központ használható.</span><span class="sxs-lookup"><span data-stu-id="f438b-121">You use this key in hello simulated device app tooconnect tooyour IoT hub.</span></span>
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. <span data-ttu-id="f438b-122">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="f438b-122">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="f438b-123">Az alkalmazás futtatásához, és jegyezze fel a hello eszközkulcs.</span><span class="sxs-lookup"><span data-stu-id="f438b-123">Run this application, and make a note of hello device key.</span></span>
   
    ![Hello alkalmazás által létrehozott eszközkulcs][12]

> [!NOTE]
> <span data-ttu-id="f438b-125">az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja.</span><span class="sxs-lookup"><span data-stu-id="f438b-125">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="f438b-126">Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, valamint az engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol.</span><span class="sxs-lookup"><span data-stu-id="f438b-126">It stores device IDs and keys toouse as security credentials, and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="f438b-127">Ha az alkalmazásnak toostore más eszközre vonatkozó metaadatok, akkor az alkalmazás-specifikus tárolási használjon.</span><span class="sxs-lookup"><span data-stu-id="f438b-127">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="f438b-128">További információkért lásd az [Azure IoT Hub fejlesztői útmutatóját][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="f438b-128">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
