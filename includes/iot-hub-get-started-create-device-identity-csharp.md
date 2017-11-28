## <a name="create-a-device-identity"></a><span data-ttu-id="d0373-101">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0373-101">Create a device identity</span></span>
<span data-ttu-id="d0373-102">Ebben a szakaszban egy .NET-konzolalkalmazást fog létrehozni, amely egy eszközidentitást hoz létre az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="d0373-102">In this section, you create a .NET console app that creates a device identity in the identity registry in your IoT hub.</span></span> <span data-ttu-id="d0373-103">Egy eszköz csak akkor tud csatlakozni az IoT Hubhoz, ha be van jegyezve az identitásjegyzékbe.</span><span class="sxs-lookup"><span data-stu-id="d0373-103">A device cannot connect to IoT hub unless it has an entry in the identity registry.</span></span> <span data-ttu-id="d0373-104">További információkért lásd az [IoT Hub fejlesztői útmutatójának][lnk-devguide-identity] „Identitásjegyzék” című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="d0373-104">For more information, see the "Identity registry" section of the [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="d0373-105">A konzolalkalmazás egy egyedi eszközazonosítót állít elő a futtatásakor, valamint egy kulcsot, amellyel az eszköz azonosítani tudja magát, amikor az eszközről a felhőbe irányuló üzeneteket küld az IoT Hubnak.</span><span class="sxs-lookup"><span data-stu-id="d0373-105">When you run this console app, it generates a unique device ID and key that your device can use to identify itself when it sends device-to-cloud messages to IoT Hub.</span></span> <span data-ttu-id="d0373-106">Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="d0373-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="d0373-107">A Visual Studióban adjon hozzá egy Visual C# Windows klasszikus asztalialkalmazás-projektet az új megoldáshoz a **Console App (.NET Framework)** (Konzolalkalmazás (.NET-keretrendszer)) projektsablonnal.</span><span class="sxs-lookup"><span data-stu-id="d0373-107">In Visual Studio, add a Visual C# Windows Classic Desktop project to a new solution by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="d0373-108">A Microsoft .NET-keretrendszer 4.5.1-es vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="d0373-108">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="d0373-109">Adja a projektnek a **CreateDeviceIdentity** nevet, a megoldásnak pedig az **IoTHubGetStarted** nevet.</span><span class="sxs-lookup"><span data-stu-id="d0373-109">Name the project **CreateDeviceIdentity** and name the solution **IoTHubGetStarted**.</span></span>
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10]
2. <span data-ttu-id="d0373-111">A Solution Explorerben (Megoldáskezelőben) kattintson a jobb gombbal az **CreateDeviceIdentity** projektre, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="d0373-111">In Solution Explorer, right-click the **CreateDeviceIdentity** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="d0373-112">A **NuGet Package Manager** (NuGet-csomagkezelő) ablakban válassza a **Browse** (Tallózás) lehetőséget, keresse meg a **microsoft.azure.devices** csomagot, válassza a **Install** (Telepítés) lehetőséget a **Microsoft.Azure.Devices** csomag telepítéséhez, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="d0373-112">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="d0373-113">Ez az eljárás letölti és telepíti az [Azure IoT Service SDK][lnk-nuget-service-sdk] (Azure IoT szolgáltatás SDK) NuGet-csomagot és annak függőségeit, valamint hozzáad egy rá mutató hivatkozást is.</span><span class="sxs-lookup"><span data-stu-id="d0373-113">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][11]
4. <span data-ttu-id="d0373-115">Adja hozzá a következő `using` utasításokat a **Program.cs** fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="d0373-115">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. <span data-ttu-id="d0373-116">Adja hozzá a **Program** osztályhoz a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="d0373-116">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="d0373-117">A helyőrző értékét cserélje le az előző szakaszban létrehozott IoT Hub kapcsolati karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="d0373-117">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="d0373-118">Adja hozzá a **Program** osztályhoz a következő módszert:</span><span class="sxs-lookup"><span data-stu-id="d0373-118">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="d0373-119">Ez a metódus egy eszközidentitást hoz létre a **myFirstDevice** azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="d0373-119">This method creates a device identity with ID **myFirstDevice**.</span></span> <span data-ttu-id="d0373-120">(Ha ez az eszköz már létezik az identitásjegyzékben, a kód egyszerűen lekéri a meglévő eszközinformációkat.) Az alkalmazás ezután megjeleníti az identitáshoz tartozó elsődleges kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d0373-120">(If that device ID already exists in the identity registry, the code simply retrieves the existing device information.) The app then displays the primary key for that identity.</span></span> <span data-ttu-id="d0373-121">Ezt a kulcsot a szimulált eszközalkalmazásban használja az IoT Hubhoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="d0373-121">You use this key in the simulated device app to connect to your IoT hub.</span></span>
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. <span data-ttu-id="d0373-122">Végül adja a következő sorokat a **Main** metódushoz:</span><span class="sxs-lookup"><span data-stu-id="d0373-122">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="d0373-123">Futtassa az alkalmazást, és jegyezze fel az eszköz kulcsát.</span><span class="sxs-lookup"><span data-stu-id="d0373-123">Run this application, and make a note of the device key.</span></span>
   
    ![Az alkalmazás által előállított eszközkulcs][12]

> [!NOTE]
> <span data-ttu-id="d0373-125">Az IoT Hub-identitásjegyzék csak az IoT Hub biztonságos elérésének biztosításához tárolja az eszközidentitásokat.</span><span class="sxs-lookup"><span data-stu-id="d0373-125">The IoT Hub identity registry only stores device identities to enable secure access to the IoT hub.</span></span> <span data-ttu-id="d0373-126">Az eszközazonosítókat és kulcsokat biztonsági hitelesítő adatokként tárolja, valamint tartalmaz egy engedélyezve/letiltva jelzőt, amellyel letilthatja egy adott eszköz hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="d0373-126">It stores device IDs and keys to use as security credentials, and an enabled/disabled flag that you can use to disable access for an individual device.</span></span> <span data-ttu-id="d0373-127">Ha az alkalmazásnak más eszközspecifikus metaadatokat kell tárolnia, egy alkalmazásspecifikus tárolót kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d0373-127">If your application needs to store other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="d0373-128">További információkért lásd az [Azure IoT Hub fejlesztői útmutatóját][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="d0373-128">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
