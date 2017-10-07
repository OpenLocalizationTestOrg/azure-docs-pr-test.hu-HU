---
title: "az Azure IoT Hub (.NET/csomópont) aaaDevice vezérlőprogram-frissítés |} Microsoft Docs"
description: "Hogyan toouse kezelés az Azure IoT Hub tooinitiate egy eszköz belső vezérlőprogram frissítése. Hello Azure IoT-eszközök SDK a Node.js tooimplement a szimulált eszköz alkalmazásának és hello Azure IoT szolgáltatás SDK .NET tooimplement service-alkalmazást, amely elindítja a hello belső vezérlőprogram frissítési használja."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/17/2017
ms.author: juanpere
ms.openlocfilehash: d48899651bcd5d67e577c971be0f9453c8f21592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-netnode"></a>Használja az eszköz felügyeleti tooinitiate eszköz vezérlőprogram-frissítés (.NET/csomópont)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Bevezetés
A hello [Ismerkedés az eszközkezelés] [ lnk-dm-getstarted] az oktatóanyagban megtudhatta, hogyan toouse hello [eszköz iker] [ lnk-devtwin] és [közvetlen módszerek] [ lnk-c2dmethod] primitívek tooremotely eszköz újraindul. A oktatóanyag használja az ugyanazon az IoT-központ primitívek hello, és bemutatja, hogyan toodo egy-végpontok szimulált vezérlőprogram-frissítés.  Ebben a mintában hello belső vezérlőprogram-frissítés végrehajtása során használatos hello [málna Pi eszköz megvalósítási minta][lnk-rpi-implementation].

Ez az oktatóanyag a következőket mutatja be:

* Hozzon létre egy .NET-Konzolalkalmazás, amely behívja hello firmwareUpdate közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.
* Hozzon létre egy szimulált eszköz alkalmazást, amely egy **firmwareUpdate** közvetlen módszer. Ez a módszer indít el egy többlépcsős folyamat, amely toodownload hello belső vezérlőprogram kép vár, hello belső vezérlőprogram lemezképet letölti és végül a hello belső vezérlőprogram kép vonatkozik. Minden egyes hello frissítés szakaszában hello eszköz által használt hello jelentett tulajdonságok tooreport folyamatban van.

Ez az oktatóanyag végén hello hogy egy Node.js Konzolalkalmazás eszköz és a .NET (C#) háttér-Konzolalkalmazás:

**dmpatterns_fwupdate_service.js**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban jeleníti meg a hello válasz, és rendszeres időközönként (minden 500ms) frissítése megjeleníti hello jelentett tulajdonságok.

**TriggerFWUpdate**, tooyour IoT-központ köti össze a korábban létrehozott hello eszközidentitás firmwareUpdate közvetlen metódus kap, egy több államot folyamat toosimulate keresztül futtat, a belső vezérlőprogram frissítési többek között: hello lemezkép letöltése, vár hello új lemezképet, és végül alkalmazása hello lemezkép letöltése.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* NODE.js-verzió 0.12.x vagy újabb verzióját, <br/>  [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

Hajtsa végre a hello [Ismerkedés az eszközkezelés](iot-hub-csharp-node-device-management-get-started.md) cikk toocreate az IoT hub és az IoT-központ kapcsolati karakterláncot.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a>Indítás, közvetlen metódussal hello eszközön távoli vezérlőprogram-frissítés
Ebben a szakaszban egy .NET-Konzolalkalmazás (használatával C#), amely indít el egy távoli belső vezérlőprogram frissítése egy eszközön hoz létre. hello alkalmazása használja-e a közvetlen módszer tooinitiate hello frissítése és által használt eszköz iker lekérdezések tooperiodically hello aktív belső vezérlőprogram frissítése hello állapotának beolvasása.

1. A Visual Studio, a Visual C# klasszikus Windows asztal projekt toohello aktuális megoldás hozzáadása hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **TriggerFWUpdate**.

    ![Új Visual C# Windows klasszikus asztalialkalmazás-projekt][img-createapp]

1. A Megoldáskezelőben kattintson a jobb gombbal hello **TriggerFWUpdate** projektre, és kattintson a **NuGet-csomagok kezelése...** .
1. A hello **NuGet-Csomagkezelő** ablakban válassza ki **Tallózás**, keressen **microsoft.azure.devices**, jelölje be **telepítése** tooinstall Hello **Microsoft.Azure.Devices** csomagot, majd fogadja el hello használati feltételeket. Ez az eljárás tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK] [ lnk-nuget-service-sdk] NuGet csomag és annak függőségeit.

    ![NuGet Package Manager (NuGet-csomagkezelő) ablak][img-servicenuget]
1. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. Adja hozzá a következő mezők toohello hello **Program** osztály. Cserélje le a helyőrző értékeket több hello IoT-központ kapcsolati karakterlánc hello központ hello előző szakaszban létrehozott hello és hello az eszköz azonosítója.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. Adja hozzá a következő metódus toohello hello **Program** osztály:
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. Adja hozzá a következő metódus toohello hello **Program** osztály:

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

1. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
1. A Solution Explorer hello, nyissa meg a hello **állítsa be indítási projektek...**  , és győződjön meg arról, hogy hello **művelet** a **TriggerFWUpdate** projekt **Start**.

1. Hello megoldás felépítéséhez.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása
Most már áll készen toorun hello alkalmazásokat.

1. Parancssorba hello hello **manageddevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. A Visual Studióban, kattintson a jobb gombbal a hello **TriggerFWUpdate** projectRun toohello C# konzolalkalmazást, jelölje be **Debug** és **Start új példány**.

3. Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.

    ![Belső vezérlőprogram frissítése sikeres volt][img-fwupdate]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy közvetlen módszer tootrigger távoli használt belső vezérlőprogram frissítése egy eszközön, és a használt hello jelentett tulajdonságok toofollow hello hello belső vezérlőprogram frissítése folyamatban van.

toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png
[img-fwupdate]: media/iot-hub-csharp-node-firmware-update/fwupdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/