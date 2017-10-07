---
title: "aaaUse Azure IoT Hub iker tulajdonságai (.NET/.NET) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooconfigure eszközök. Hello Azure IoT-eszközök SDK a .NET tooimplement a szimulált eszköz alkalmazásának és hello Azure IoT szolgáltatás SDK .NET tooimplement egy szolgáltatás-alkalmazást, amely módosítja a használatával egy eszközt a két eszköz konfigurációs használja."
services: iot-hub
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dkshir
ms.openlocfilehash: 486436d29abfd5158c253adc5abf5935e0e1fdba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices"></a>Használja a kívánt tulajdonságokkal tooconfigure eszközök
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Ez az oktatóanyag végén hello hogy két .NET konzol alkalmazások:

* **SimulateDeviceConfiguration**, a szimulált eszköz alkalmazás, amely megvárja-e a szükséges konfiguráció frissítése a jelentést készít egy szimulált konfigurációs frissítési folyamat állapotának hello.
* **SetDesiredConfigurationAndQuery**, egy háttér-alkalmazást, amely hello szükséges konfigurációs egy eszközön, és lekérdezések hello konfigurációs frissítési folyamat.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.
> 
> 

toocomplete ebben az oktatóanyagban hello a következőkre lesz szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].

Ha követte hello [Ismerkedés az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már rendelkezik egy IoT-központot, és egy eszközidentitás nevű **myDeviceId**. Ebben az esetben kihagyhatja toohello [létrehozás hello szimulált eszköz alkalmazásának] [ lnk-how-to-configure-createapp] szakasz.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a>Hello szimulált eszköz alkalmazás létrehozása
Ebben a szakaszban egy .NET-Konzolalkalmazás, amely a tooyour hub, létrehozhat **myDeviceId**megvárja-e a szükséges konfiguráció frissítése a, majd jelentést készít a frissítések a szimulált hello konfigurációs frissítési folyamat.

1. A Visual Studio, hozzon létre egy új Visual C# klasszikus Windows asztal projektet hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **SimulateDeviceConfiguration**.
   
    ![Új Visual C# klasszikus Windows-eszköz alkalmazás][img-createdeviceapp]

1. A Megoldáskezelőben kattintson a jobb gombbal hello **SimulateDeviceConfiguration** projektre, és kattintson a **NuGet-csomagok kezelése...** .
1. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás** keresse meg a **microsoft.azure.devices.client**. Válassza ki **telepítése** tooinstall hello **Microsoft.Azure.Devices.Client** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT-eszközök SDK] [ lnk-nuget-client-sdk] NuGet csomag és annak függőségeit.
   
    ![NuGet-Csomagkezelő ablak ügyfélalkalmazás][img-clientnuget]
1. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrző értékét lecserélheti egy hello eszköz kapcsolati karakterláncot, amely az előző szakaszban hello feljegyzett.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. Adja hozzá a következő metódus toohello hello **Program** osztály:
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting toohub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    Hello **ügyfél** vezérlőnek minden hello módszerek toointeract van szüksége az eszköz twins hello eszközről. a fent látható kód hello inicializál hello **ügyfél** objektumot, és lekéri hello eszköz iker a **myDeviceId**.

1. Adja hozzá a következő metódus toohello hello **Program** osztály. Ez a módszer hello kezdőértéket telemetriai beállítja hello helyi eszközön, és majd frissítések hello eszköz iker.

        public static async void InitTelemetry()
        {
            try
            {
                Console.WriteLine("Report initial telemetry config:");
                TwinCollection telemetryConfig = new TwinCollection();
                
                telemetryConfig["configId"] = "0";
                telemetryConfig["sendFrequency"] = "24h";
                reportedProperties["telemetryConfig"] = telemetryConfig;
                Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Adja hozzá a következő metódus toohello hello **Program** osztály. Ez egy visszahívást, amely észleli a változást az *szükséges tulajdonságok* a hello eszköz iker.

        private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            try
            {
                Console.WriteLine("Desired property change:");
                Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                var currentTelemetryConfig = reportedProperties["telemetryConfig"];
                var desiredTelemetryConfig = desiredProperties["telemetryConfig"];

                if ((desiredTelemetryConfig != null) && (desiredTelemetryConfig["configId"] != currentTelemetryConfig["configId"]))
                {
                    Console.WriteLine("\nInitiating config change");
                    currentTelemetryConfig["status"] = "Pending";
                    currentTelemetryConfig["pendingConfig"] = desiredTelemetryConfig;

                    await Client.UpdateReportedPropertiesAsync(reportedProperties);

                    CompleteConfigChange();
                }
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    A metódus frissítések hello jelentett hello helyi eszköz a két objektum hello konfiguráció tulajdonságainak túl kérelem és a készletek hello állapotának frissítése**függőben lévő**, majd a frissítések hello eszköz iker hello szolgáltatásban. Miután sikeresen frissített hello eszköz iker, hello metódus meghívásával befejezné hello konfiguráció módosításakor `CompleteConfigChange` ismertetett hello következő pontra.

1. Adja hozzá a következő metódus toohello hello **Program** osztály. Ez a módszer szimulálja egy eszköz alaphelyzetbe állítása, majd a frissítések helyi jelentett tulajdonságok hello állapotának beállításakor túl hello**sikeres** és eltávolítja hello **pendingConfig** elemet. Majd frissíti a hello eszköz iker hello szolgáltatásban. 

        public static async void CompleteConfigChange()
        {
            try
            {
                var currentTelemetryConfig = reportedProperties["telemetryConfig"];

                Console.WriteLine("\nSimulating device reset");
                await Task.Delay(30000); 

                Console.WriteLine("\nCompleting config change");
                currentTelemetryConfig["configId"] = currentTelemetryConfig["pendingConfig"]["configId"];
                currentTelemetryConfig["sendFrequency"] = currentTelemetryConfig["pendingConfig"]["sendFrequency"];
                currentTelemetryConfig["status"] = "Success";
                currentTelemetryConfig["pendingConfig"] = null;

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
                Console.WriteLine("Config change complete \nPress any key tooexit.");
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:

        try
        {
            InitClient();
            InitTelemetry();

            Console.WriteLine("Wait for desired telemetry...");
            Client.SetDesiredPropertyUpdateCallback(OnDesiredPropertyChanged, null).Wait();
            Console.ReadKey();
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }

   > [!NOTE]
   > Ez az oktatóanyag nem szimulálása egyidejű keresni minden olyan esetben. Néhány konfigurációs frissítési folyamat előfordulhat, hogy képes tooaccommodate módosításainak konfigurációjához hello frissítés futása közben, előfordulhat, hogy rendelkeznek őket, és néhány sikerült utasítsa el azokat a hibaállapotot tooqueue. Győződjön meg arról, hogy tooconsider hello kívánt viselkedés, a konfigurációs folyamat, és adja hozzá a megfelelő logika hello hello konfigurációváltozás kezdeményezése előtt.
   > 
   > 
1. Hello megoldás kiépítését, majd futtassa az hello eszközalkalmazás a Visual Studio eszközből kattintva **F5**. Jelzi, hogy a szimulált eszköz hello eszköz iker beolvasása, hello telemetriai beállítása, és várakozik a kívánt tulajdonság módosításának köszönőüzenetei megtekintheti a hello kimeneti konzolon. Folyamatosan futó hello alkalmazást.

## <a name="create-hello-service-app"></a>Hello service-alkalmazás létrehozása
Ez a szakasz során létrehoz egy .NET-Konzolalkalmazás, hogy a frissítések hello *szükségeskonfiguráció-tulajdonságok* a hello eszköz iker társított **myDeviceId** új telemetria-konfigurációs objektum. Ezután hello eszköz twins hello IoT-központ tárolt lekérdezi és hello hello különbségének szükséges, és a jelentett hello eszköz konfigurációját jeleníti meg.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **SetDesiredConfigurationAndQuery**.
   
    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]
1. A Megoldáskezelőben kattintson a jobb gombbal hello **SetDesiredConfigurationAndQuery** projektre, és kattintson a **NuGet-csomagok kezelése...** .
1. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.
   
    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. Adja hozzá a következő mezők toohello hello **Program** osztály. Hello helyőrző értékét lecserélheti egy hello hello hub hello előző szakaszban létrehozott IoT-központ kapcsolati karakterláncot.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Adja hozzá a következő metódus toohello hello **Program** osztály:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }
   
    Hello **beállításjegyzék** vezérlőnek minden hello módszerek szükséges toointeract az eszköz twins hello szolgáltatásból. Ez a kód inicializálja hello **beállításjegyzék** objektum beolvassa az eszköz iker hello **myDeviceId**, majd frissíti a kívánt tulajdonságát egy új telemetriai configuration objektummal.
    Ezt követően hello eszköz twins tárolt hello IoT-központ 10 másodpercenként kérdezi le, és megrendelése hello szükséges, és telemetriai konfigurációk jelentett. Tekintse meg a toohello [IoT-központ lekérdezési nyelv] [ lnk-query] toolearn hogyan toogenerate gazdag jelent az eszközön.
   
   > [!IMPORTANT]
   > Ez az alkalmazás lekérdezi az IoT-központ 10 másodpercenként szemléltetési célokat szolgál. Használja több eszközt, és nem toodetect módosítások toogenerate felhasználók számára is elérhető jelentések lekérdezi. Ha a megoldás a valós idejű értesítések eszköz események van szüksége, [iker értesítések][lnk-twin-notifications].
   > 
   > 
1. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **SetDesiredConfigurationAndQuery** projekt **Start**. Hello megoldás felépítéséhez.
1. A **SimulateDeviceConfiguration** alkalmazást futtató eszköznek, futtatási hello service-alkalmazást a Visual Studio használatával **F5**. Megtekintheti az hello jelentett konfigurációs módosítást a **függőben lévő** túl**sikeres** hello új aktív a Küldés 24 óra helyett öt perces gyakoriságot.

 ![Eszköz sikeresen konfigurálva][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Nincs késleltetést mentése tooa perc közötti hello eszköz jelentés művelet és hello lekérdezés eredménye. Ez a tooenable hello lekérdezés infrastruktúra toowork nagyon nagy méretekben. egy egyetlen eszközt iker tooretrieve konzisztens nézeteinek hello használata **getDeviceTwin** metódus a hello **beállításjegyzék** osztály.
   > 
   > 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy szabványoskonfiguráció mint beállítása *szükséges tulajdonságok* hello megoldásból háttér, és egy eszköz alkalmazás toodetect, módosítása és annak állapotát a jelentett hello reporting többlépéses frissítési folyamat szimulálása megírt tulajdonságok.

A következő erőforrások toolearn hogyan használja hello számára:

* telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ] [ lnk-iothub-getstarted] oktatóanyagban
* ütemezhet, vagy hajtsa végre műveleteket a eszközök nagy mennyiségű, lásd: hello [ütemezés és a szórásos feladatok] [ lnk-schedule-jobs] oktatóanyag.
* interaktív (például bekapcsolásával a felhasználó által felügyelt alkalmazásból ventilátor), eszközeinek vezérléséhez a hello [közvetlen módszerekkel] [ lnk-methods-tutorial] oktatóanyag.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/devicesdknuget.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app
