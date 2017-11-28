---
title: "Egy eszköz szimulálása Azure IoT oldala (Windows) |} Microsoft Docs"
description: "Megtudhatja, hogyan lehet Windows Azure IoT peremhálózati használatával létrehozni a szimulált eszköz által küldött telemetriai adatokat az IoT-központ az Azure IoT peremhálózati átjárón keresztül."
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
ms.openlocfilehash: e7eb2931993daf3f0aecbd4a43d27ebd5adc10b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a><span data-ttu-id="2b21c-103">Segítségével Azure IoT peremhálózati eszköz-felhő üzenetek a szimulált eszköz (Windows)</span><span class="sxs-lookup"><span data-stu-id="2b21c-103">Use Azure IoT Edge to send device-to-cloud messages with a simulated device (Windows)</span></span>

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="2b21c-104">A minta futtatása</span><span class="sxs-lookup"><span data-stu-id="2b21c-104">How to run the sample</span></span>

<span data-ttu-id="2b21c-105">A **build.cmd** parancsfájlt hoz létre a kimenetét a **build** mappa a helyi példánya a **iot-edge** tárház.</span><span class="sxs-lookup"><span data-stu-id="2b21c-105">The **build.cmd** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="2b21c-106">A kimenetet a mintában használt négy IoT peremhálózati modulok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2b21c-106">This output includes the four IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="2b21c-107">A build script helyek a:</span><span class="sxs-lookup"><span data-stu-id="2b21c-107">The build script places the:</span></span>

* <span data-ttu-id="2b21c-108">**Logger.dll** a a **build\\modulok\\naplózó\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="2b21c-108">**logger.dll** in the **build\\modules\\logger\\Debug** folder.</span></span>
* <span data-ttu-id="2b21c-109">**iothub.dll** a a **build\\modulok\\IOT hubbal\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="2b21c-109">**iothub.dll** in the **build\\modules\\iothub\\Debug** folder.</span></span>
* <span data-ttu-id="2b21c-110">**identitás\_map.dll** a a **build\\modulok\\identitymap\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="2b21c-110">**identity\_map.dll** in the **build\\modules\\identitymap\\Debug** folder.</span></span>
* <span data-ttu-id="2b21c-111">**Szimulált\_device.dll** a a **build\\modulok\\szimulált\_eszköz\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="2b21c-111">**simulated\_device.dll** in the **build\\modules\\simulated\_device\\Debug** folder.</span></span>

<span data-ttu-id="2b21c-112">Az elérési utak használata a **modul elérési útján** értékeket az alábbi JSON fájlban látható módon:</span><span class="sxs-lookup"><span data-stu-id="2b21c-112">Use these paths for the **module path** values as shown in the following JSON settings file:</span></span>

<span data-ttu-id="2b21c-113">A szimulált\_eszköz\_felhő\_feltöltése\_minta tart az elérési út egy JSON-konfigurációs fájl parancssori argumentumként.</span><span class="sxs-lookup"><span data-stu-id="2b21c-113">The simulated\_device\_cloud\_upload\_sample process takes the path to a JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="2b21c-114">A következő példa JSON-fájl megtalálható a következő SDK tárház **minták\\szimulált\_eszköz\_felhő\_feltöltése\_minta\\src\\ Szimulált\_eszköz\_felhő\_feltöltése\_minta\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="2b21c-114">The following example JSON file is provided in the SDK repository at **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_win.json**.</span></span> <span data-ttu-id="2b21c-115">A konfigurációs fájl, kivéve, ha módosítja a build parancsfájlt helyezze el az IoT peremhálózati modulok vagy a minta végrehajtható fájlok alapértelmezettől eltérő helyeket működik.</span><span class="sxs-lookup"><span data-stu-id="2b21c-115">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="2b21c-116">A modul elérési útvonalai képest a könyvtár ahol a szimulált\_eszköz\_felhő\_feltöltése\_sample.exe helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="2b21c-116">The module paths are relative to the directory where the simulated\_device\_cloud\_upload\_sample.exe is located.</span></span> <span data-ttu-id="2b21c-117">A minta JSON-konfigurációs fájl az alapértelmezett az aktuális munkakönyvtárban a "deviceCloudUploadGatewaylog.log" írásakor.</span><span class="sxs-lookup"><span data-stu-id="2b21c-117">The sample JSON configuration file defaults to writing to 'deviceCloudUploadGatewaylog.log' in your current working directory.</span></span>

<span data-ttu-id="2b21c-118">Egy szövegszerkesztőben nyissa meg a fájlt **minták\\szimulált\_eszköz\_felhő\_feltöltése\_minta\\src\\szimulált\_eszköz\_felhő\_feltöltése\_win.json** a helyi példánya a **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="2b21c-118">In a text editor, open the file **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_win.json** in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="2b21c-119">Ez a fájl az IoT-Edge modulok konfigurálja a minta átjáró:</span><span class="sxs-lookup"><span data-stu-id="2b21c-119">This file configures the IoT Edge modules in the sample gateway:</span></span>

* <span data-ttu-id="2b21c-120">A **IOT hubbal** modul csatlakozik az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2b21c-120">The **IoTHub** module connects to your IoT hub.</span></span> <span data-ttu-id="2b21c-121">Konfigurálja az IoT hub adatküldéshez.</span><span class="sxs-lookup"><span data-stu-id="2b21c-121">You configure it to send data to your IoT hub.</span></span> <span data-ttu-id="2b21c-122">Pontosabban, állítsa be a **IoTHubName** értékre az IoT hub nevét, és állítsa a **IoTHubSuffix** egy érték **azure-devices.net**.</span><span class="sxs-lookup"><span data-stu-id="2b21c-122">Specifically, set the **IoTHubName** value to the name of your IoT hub and set the **IoTHubSuffix** value to **azure-devices.net**.</span></span> <span data-ttu-id="2b21c-123">Állítsa be a **átviteli** egyik: **HTTP**, **AMQP**, vagy **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="2b21c-123">Set the **Transport** value to one of: **HTTP**, **AMQP**, or **MQTT**.</span></span> <span data-ttu-id="2b21c-124">Jelenleg csak **HTTP** közösen használja az összes eszközre küldött üzenetek egy TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="2b21c-124">Currently, only **HTTP** shares one TCP connection for all device messages.</span></span> <span data-ttu-id="2b21c-125">Ha az érték **AMQP**, vagy **MQTT**, az átjáró egy külön TCP-kapcsolatot az IoT-központ az egyes eszközök tart fenn.</span><span class="sxs-lookup"><span data-stu-id="2b21c-125">If you set the value to **AMQP**, or **MQTT**, the gateway maintains a separate TCP connection to IoT Hub for each device.</span></span>
* <span data-ttu-id="2b21c-126">A **leképezési** modul szimulált eszköz MAC-címet rendel az IoT Hub eszköz azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="2b21c-126">The **mapping** module maps the MAC addresses of your simulated devices to your IoT Hub device ids.</span></span> <span data-ttu-id="2b21c-127">Győződjön meg arról, hogy **deviceId** megegyeznek a két eszköz az IoT hub, és hogy a hozzáadott azonosítóit a **deviceKey** értékek a kulcsokat, a két eszközök tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="2b21c-127">Make sure that **deviceId** values match the ids of the two devices you added to your IoT hub, and that the **deviceKey** values contain the keys of your two devices.</span></span>
* <span data-ttu-id="2b21c-128">A **BLE1** és **BLE2** modulokra a szimulált eszköz.</span><span class="sxs-lookup"><span data-stu-id="2b21c-128">The **BLE1** and **BLE2** modules are the simulated devices.</span></span> <span data-ttu-id="2b21c-129">Vegye figyelembe, hogyan modul MAC-címek felel meg a címeket a **leképezési** modul.</span><span class="sxs-lookup"><span data-stu-id="2b21c-129">Note how the module MAC addresses match the addresses in the **mapping** module.</span></span>
* <span data-ttu-id="2b21c-130">A **naplózó** modul fájlban naplózza a átjáró tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="2b21c-130">The **Logger** module logs your gateway activity to a file.</span></span>
* <span data-ttu-id="2b21c-131">A **modul elérési útján** az alábbi példában látható módon értékei a könyvtár viszonyítva ahol a szimulált\_eszköz\_felhő\_feltöltése\_sample.exe helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="2b21c-131">The **module path** values shown in the following example are relative to the directory where the simulated\_device\_cloud\_upload\_sample.exe is located.</span></span>
* <span data-ttu-id="2b21c-132">A **hivatkozások** tömb a JSON-fájl alján csatlakozik a **BLE1** és **BLE2** modulok a **leképezési** modul, és a **leképezési** modult a **IOT hubbal** modul.</span><span class="sxs-lookup"><span data-stu-id="2b21c-132">The **links** array at the bottom of the JSON file connects the **BLE1** and **BLE2** modules to the **mapping** module, and the **mapping** module to the **IoTHub** module.</span></span> <span data-ttu-id="2b21c-133">Emellett biztosítja, hogy az összes üzenet által naplózott a **naplózó** modul.</span><span class="sxs-lookup"><span data-stu-id="2b21c-133">It also ensures that all messages are logged by the **Logger** module.</span></span>

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

<span data-ttu-id="2b21c-134">A konfigurációs fájl módosításainak mentése.</span><span class="sxs-lookup"><span data-stu-id="2b21c-134">Save the changes you made to the configuration file.</span></span>

<span data-ttu-id="2b21c-135">A minta futtatásához:</span><span class="sxs-lookup"><span data-stu-id="2b21c-135">To run the sample:</span></span>

1. <span data-ttu-id="2b21c-136">A parancssorban navigáljon a **build** mappa a helyi példánya a **iot-edge** tárház.</span><span class="sxs-lookup"><span data-stu-id="2b21c-136">At a command prompt, navigate to the **build** folder in your local copy of the **iot-edge** repository.</span></span>
2. <span data-ttu-id="2b21c-137">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b21c-137">Run the following command:</span></span>
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. <span data-ttu-id="2b21c-138">Használhatja a [eszköz explorer] [ lnk-device-explorer] vagy [IOT hubbal-explorer] [ lnk-iothub-explorer] az üzeneteket, amelyek az IoT-központ fogadja az átjáró figyelésére.</span><span class="sxs-lookup"><span data-stu-id="2b21c-138">You can use the [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool to monitor the messages that IoT hub receives from the gateway.</span></span> <span data-ttu-id="2b21c-139">Például az IOT hubbal-Explorerben figyelheti eszköz-a-felhőbe küldött üzeneteket a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b21c-139">For example, using iothub-explorer you can monitor device-to-cloud messages using the following command:</span></span>

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a><span data-ttu-id="2b21c-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b21c-140">Next steps</span></span>

<span data-ttu-id="2b21c-141">IoT peremhálózati tájékozottabbak kapnak, és néhány kódpéldák kísérletezhet, látogasson el az alábbi fejlesztői oktatóanyagok és erőforrások:</span><span class="sxs-lookup"><span data-stu-id="2b21c-141">To gain a more advanced understanding of IoT Edge and experiment with some code examples, visit the following developer tutorials and resources:</span></span>

* <span data-ttu-id="2b21c-142">[Eszköz-felhő üzeneteket küldeni egy fizikai eszközről IoT oldala][lnk-physical-device]</span><span class="sxs-lookup"><span data-stu-id="2b21c-142">[Send device-to-cloud messages from a physical device with IoT Edge][lnk-physical-device]</span></span>
* <span data-ttu-id="2b21c-143">[Az Azure IoT él][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="2b21c-143">[Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="2b21c-144">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="2b21c-144">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="2b21c-145">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="2b21c-145">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="2b21c-146">[Az IoT-megoldásból az alapoktól biztonságos mentése][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="2b21c-146">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md