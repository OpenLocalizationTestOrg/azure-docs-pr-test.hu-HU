---
title: "Azure IoT peremhálózati (Windows) rendelkező eszköz aaaSimulate |} Microsoft Docs"
description: "Hogyan toouse Azure IoT peremhálózati a Windows toocreate a szimulált eszköz, amely elküldi az Azure IoT peremhálózati átjáró tooan IoT-központ keresztül telemetriai."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: ddbe85eb956e9934e80e2e80e09f77b24cf54856
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-toosend-device-to-cloud-messages-with-a-simulated-device-windows"></a><span data-ttu-id="29865-103">Azure IoT peremhálózati toosend eszközről a felhőbe üzenetek használata a szimulált eszköz (Windows)</span><span class="sxs-lookup"><span data-stu-id="29865-103">Use Azure IoT Edge toosend device-to-cloud messages with a simulated device (Windows)</span></span>

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="29865-104">Hogyan toorun hello minta</span><span class="sxs-lookup"><span data-stu-id="29865-104">How toorun hello sample</span></span>

<span data-ttu-id="29865-105">Hello **build.cmd** parancsfájlt hoz létre a kimeneti hello **build** hello helyi példánya mappájában **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="29865-105">hello **build.cmd** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="29865-106">A kimeneti hello a mintában használt négy IoT peremhálózati modulok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="29865-106">This output includes hello four IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="29865-107">build script helyek hello a:</span><span class="sxs-lookup"><span data-stu-id="29865-107">hello build script places the:</span></span>

* <span data-ttu-id="29865-108">**Logger.dll** a hello **build\\modulok\\naplózó\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="29865-108">**logger.dll** in hello **build\\modules\\logger\\Debug** folder.</span></span>
* <span data-ttu-id="29865-109">**iothub.dll** a hello **build\\modulok\\IOT hubbal\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="29865-109">**iothub.dll** in hello **build\\modules\\iothub\\Debug** folder.</span></span>
* <span data-ttu-id="29865-110">**identitás\_map.dll** a hello **build\\modulok\\identitymap\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="29865-110">**identity\_map.dll** in hello **build\\modules\\identitymap\\Debug** folder.</span></span>
* <span data-ttu-id="29865-111">**Szimulált\_device.dll** a hello **build\\modulok\\szimulált\_eszköz\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="29865-111">**simulated\_device.dll** in hello **build\\modules\\simulated\_device\\Debug** folder.</span></span>

<span data-ttu-id="29865-112">Az elérési utak használata hello **modul elérési útján** látható módon hello beállításokat JSON-fájlt a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="29865-112">Use these paths for hello **module path** values as shown in hello following JSON settings file:</span></span>

<span data-ttu-id="29865-113">Szimulált hello\_eszköz\_felhő\_feltöltése\_minta tart hello elérési tooa JSON-konfigurációs fájl parancssori argumentumként.</span><span class="sxs-lookup"><span data-stu-id="29865-113">hello simulated\_device\_cloud\_upload\_sample process takes hello path tooa JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="29865-114">hello alábbi példa JSON-fájl valósul meg a következő SDK-tárházban hello **minták\\szimulált\_eszköz\_felhő\_feltöltése\_minta\\src\\ Szimulált\_eszköz\_felhő\_feltöltése\_minta\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="29865-114">hello following example JSON file is provided in hello SDK repository at **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_win.json**.</span></span> <span data-ttu-id="29865-115">A konfigurációs fájl működik, kivéve, ha módosítja a hello build script tooplace hello IoT peremhálózati modulok, vagy végrehajtható fájlok alapértelmezettől eltérő helyeket a mintát.</span><span class="sxs-lookup"><span data-stu-id="29865-115">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="29865-116">hello modul elérési útvonalai hello szimulált ahol relatív toohello directory\_eszköz\_felhő\_feltöltése\_sample.exe helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="29865-116">hello module paths are relative toohello directory where hello simulated\_device\_cloud\_upload\_sample.exe is located.</span></span> <span data-ttu-id="29865-117">hello minta JSON-konfigurációs fájlt toowriting too'deviceCloudUploadGatewaylog.log alapértelmezés szerint "az aktuális munkakönyvtárban a.</span><span class="sxs-lookup"><span data-stu-id="29865-117">hello sample JSON configuration file defaults toowriting too'deviceCloudUploadGatewaylog.log' in your current working directory.</span></span>

<span data-ttu-id="29865-118">Egy szövegszerkesztőben nyissa meg a hello fájlt **minták\\szimulált\_eszköz\_felhő\_feltöltése\_minta\\src\\szimulált\_eszköz \_felhő\_feltöltése\_win.json** hello helyi példánya a **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="29865-118">In a text editor, open hello file **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_win.json** in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="29865-119">Ez a fájl hello IoT peremhálózati modulok hello minta átjáró konfigurálja:</span><span class="sxs-lookup"><span data-stu-id="29865-119">This file configures hello IoT Edge modules in hello sample gateway:</span></span>

* <span data-ttu-id="29865-120">Hello **IOT hubbal** modul tooyour IoT-központ csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="29865-120">hello **IoTHub** module connects tooyour IoT hub.</span></span> <span data-ttu-id="29865-121">Konfigurálja toosend adatok tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="29865-121">You configure it toosend data tooyour IoT hub.</span></span> <span data-ttu-id="29865-122">Pontosabban, a set hello **IoTHubName** az IoT hub nevét toohello értékét, és állítsa be a hello **IoTHubSuffix** érték túl**azure-devices.net**.</span><span class="sxs-lookup"><span data-stu-id="29865-122">Specifically, set hello **IoTHubName** value toohello name of your IoT hub and set hello **IoTHubSuffix** value too**azure-devices.net**.</span></span> <span data-ttu-id="29865-123">Set hello **átviteli** az érték tooone: **HTTP**, **AMQP**, vagy **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="29865-123">Set hello **Transport** value tooone of: **HTTP**, **AMQP**, or **MQTT**.</span></span> <span data-ttu-id="29865-124">Jelenleg csak **HTTP** közösen használja az összes eszközre küldött üzenetek egy TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="29865-124">Currently, only **HTTP** shares one TCP connection for all device messages.</span></span> <span data-ttu-id="29865-125">Ha a hello érték túl**AMQP**, vagy **MQTT**, hello átjáró egy külön TCP kapcsolat tooIoT Hub az egyes eszközök tart fenn.</span><span class="sxs-lookup"><span data-stu-id="29865-125">If you set hello value too**AMQP**, or **MQTT**, hello gateway maintains a separate TCP connection tooIoT Hub for each device.</span></span>
* <span data-ttu-id="29865-126">Hello **leképezési** modul leképezi a szimulált eszköz tooyour IoT-központ eszközazonosítókat hello MAC-címeit.</span><span class="sxs-lookup"><span data-stu-id="29865-126">hello **mapping** module maps hello MAC addresses of your simulated devices tooyour IoT Hub device ids.</span></span> <span data-ttu-id="29865-127">Győződjön meg arról, hogy **deviceId** értékek egyeznek hello azonosítói hello két tooyour IoT-központot, és adott hello hozzáadott eszközök **deviceKey** értékeket tartalmaz, a két eszközök hello kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="29865-127">Make sure that **deviceId** values match hello ids of hello two devices you added tooyour IoT hub, and that hello **deviceKey** values contain hello keys of your two devices.</span></span>
* <span data-ttu-id="29865-128">Hello **BLE1** és **BLE2** modulokra szimulált hello eszközök.</span><span class="sxs-lookup"><span data-stu-id="29865-128">hello **BLE1** and **BLE2** modules are hello simulated devices.</span></span> <span data-ttu-id="29865-129">Vegye figyelembe, hogyan hello modul MAC-címek felel meg a hello hello címek **leképezési** modul.</span><span class="sxs-lookup"><span data-stu-id="29865-129">Note how hello module MAC addresses match hello addresses in hello **mapping** module.</span></span>
* <span data-ttu-id="29865-130">Hello **naplózó** modul naplózza az átjáró tevékenység tooa fájlt.</span><span class="sxs-lookup"><span data-stu-id="29865-130">hello **Logger** module logs your gateway activity tooa file.</span></span>
* <span data-ttu-id="29865-131">Hello **modul elérési útján** hello a következő példában látható értékek a következők hello szimulált ahol relatív toohello directory\_eszköz\_felhő\_feltöltése\_sample.exe helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="29865-131">hello **module path** values shown in hello following example are relative toohello directory where hello simulated\_device\_cloud\_upload\_sample.exe is located.</span></span>
* <span data-ttu-id="29865-132">Hello **hivatkozások** hello JSON-fájl hello alján tömb csatlakozik hello **BLE1** és **BLE2** modulok toohello **leképezési** modul, és hello **leképezési** modul toohello **IOT hubbal** modul.</span><span class="sxs-lookup"><span data-stu-id="29865-132">hello **links** array at hello bottom of hello JSON file connects hello **BLE1** and **BLE2** modules toohello **mapping** module, and hello **mapping** module toohello **IoTHub** module.</span></span> <span data-ttu-id="29865-133">Emellett biztosítja, hogy az összes üzenet hello által naplózott **naplózó** modul.</span><span class="sxs-lookup"><span data-stu-id="29865-133">It also ensures that all messages are logged by hello **Logger** module.</span></span>

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01"
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02"
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

<span data-ttu-id="29865-134">Mentés hello módosításokat végzett toohello konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="29865-134">Save hello changes you made toohello configuration file.</span></span>

<span data-ttu-id="29865-135">toorun hello minta:</span><span class="sxs-lookup"><span data-stu-id="29865-135">toorun hello sample:</span></span>

1. <span data-ttu-id="29865-136">A parancssorban lépjen a toohello **build** hello helyi példánya mappájában **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="29865-136">At a command prompt, navigate toohello **build** folder in your local copy of hello **iot-edge** repository.</span></span>
2. <span data-ttu-id="29865-137">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="29865-137">Run hello following command:</span></span>
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. <span data-ttu-id="29865-138">Használhatja a hello [eszköz explorer] [ lnk-device-explorer] vagy [IOT hubbal-explorer] [ lnk-iothub-explorer] IoT-központ fogadja hello toomonitor köszönőüzenetei eszköz átjáró.</span><span class="sxs-lookup"><span data-stu-id="29865-138">You can use hello [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool toomonitor hello messages that IoT hub receives from hello gateway.</span></span> <span data-ttu-id="29865-139">Például az IOT hubbal-Explorerben figyelheti eszközről a felhőbe üzenetek hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="29865-139">For example, using iothub-explorer you can monitor device-to-cloud messages using hello following command:</span></span>

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a><span data-ttu-id="29865-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29865-140">Next steps</span></span>

<span data-ttu-id="29865-141">toogain tájékozottabbak IoT peremhálózati és kísérlet az egyes kódpéldák hello következő látogasson el fejlesztői oktatóanyagok és erőforrások:</span><span class="sxs-lookup"><span data-stu-id="29865-141">toogain a more advanced understanding of IoT Edge and experiment with some code examples, visit hello following developer tutorials and resources:</span></span>

* <span data-ttu-id="29865-142">[Eszköz-felhő üzeneteket küldeni egy fizikai eszközről IoT oldala][lnk-physical-device]</span><span class="sxs-lookup"><span data-stu-id="29865-142">[Send device-to-cloud messages from a physical device with IoT Edge][lnk-physical-device]</span></span>
* <span data-ttu-id="29865-143">[Az Azure IoT él][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="29865-143">[Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="29865-144">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="29865-144">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="29865-145">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="29865-145">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="29865-146">[Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="29865-146">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md