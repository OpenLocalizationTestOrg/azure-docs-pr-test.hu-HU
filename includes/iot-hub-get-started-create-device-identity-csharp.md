## <a name="create-a-device-identity"></a>Eszközidentitás létrehozása
Ebben a szakaszban egy .NET-Konzolalkalmazás, amely létrehoz egy eszközidentitás hello identitásjegyzékhez a az IoT hub létrehozása. Egy eszköz nem lehet kapcsolódni a tooIoT hub, kivéve, ha azt egy bejegyzéssel rendelkezik hello identitásjegyzékhez. További információ a hello hello "Identitásjegyzékhez" című szakaszában talál [IoT Hub fejlesztői útmutató][lnk-devguide-identity]. A konzol alkalmazás futtatásakor egy eszköz egyedi Azonosítót hoz létre, és az, hogy az eszköz használhatja tooidentify magát, eszköz-felhő küld a kulcs üzenetek tooIoT Hub. Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt tooa új megoldás hozzáadása hello segítségével **Konzolalkalmazás (.NET-keretrendszer)** projektsablon. Győződjön meg arról, hogy hello .NET-keretrendszer 4.5.1 vagy újabb. Név hello projekt **CreateDeviceIdentity** és name hello megoldás **IoTHubGetStarted**.
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][10]
2. A Megoldáskezelőben kattintson a jobb gombbal hello **CreateDeviceIdentity** projektre, és kattintson a **NuGet-csomagok kezelése**.
3. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][11]
4. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. Adja hozzá a következő metódus toohello hello **Program** osztály:
   
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
   
    Ez a metódus egy eszközidentitást hoz létre a **myFirstDevice** azonosítóval. (Ha az eszköz azonosító már létezik hello identitásjegyzékhez, hello kód egyszerűen hello meglévő eszköz adatainak beolvasása.) hello app majd hello elsődleges kulcs az identitásukat jeleníti meg. Ez a kulcs hello szimulált eszköz, alkalmazás tooconnect tooyour IoT-központ használható.
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. Az alkalmazás futtatásához, és jegyezze fel a hello eszközkulcs.
   
    ![Hello alkalmazás által létrehozott eszközkulcs][12]

> [!NOTE]
> az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja. Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, valamint az engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol. Ha az alkalmazásnak toostore más eszközre vonatkozó metaadatok, akkor az alkalmazás-specifikus tárolási használjon. További információkért lásd az [Azure IoT Hub fejlesztői útmutatóját][lnk-devguide-identity].
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
