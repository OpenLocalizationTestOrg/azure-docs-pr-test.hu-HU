---
title: "Egy eszköz szimulálása Azure IoT oldala (Linux) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT peremhálózati Linux által küldött telemetriai adatokat az IoT-központ az IoT-Edge átjárón keresztül szimulált eszköz létrehozásához."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: 5349960373ae6815862c5f79a69dd6d5d9d624ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-linux"></a><span data-ttu-id="0ab64-103">Segítségével Azure IoT peremhálózati eszköz-felhő üzenetek a szimulált eszköz (Linux)</span><span class="sxs-lookup"><span data-stu-id="0ab64-103">Use Azure IoT Edge to send device-to-cloud messages with a simulated device (Linux)</span></span>

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="0ab64-104">A minta futtatása</span><span class="sxs-lookup"><span data-stu-id="0ab64-104">How to run the sample</span></span>

<span data-ttu-id="0ab64-105">A **build.sh** szkript a kimenetét a **build** mappában hozza létre, az **iot-edge** adattár helyi példányában.</span><span class="sxs-lookup"><span data-stu-id="0ab64-105">The **build.sh** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="0ab64-106">A kimenetet a mintában használt négy IoT peremhálózati modulok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0ab64-106">This output includes the four IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="0ab64-107">A build script helyek a:</span><span class="sxs-lookup"><span data-stu-id="0ab64-107">The build script places the:</span></span>

* <span data-ttu-id="0ab64-108">**liblogger.so** a a **build/modulok/naplózó** mappát.</span><span class="sxs-lookup"><span data-stu-id="0ab64-108">**liblogger.so** in the **build/modules/logger** folder.</span></span>
* <span data-ttu-id="0ab64-109">**libiothub.so** a a **build/modulok/IOT hubbal** mappát.</span><span class="sxs-lookup"><span data-stu-id="0ab64-109">**libiothub.so** in the **build/modules/iothub** folder.</span></span>
* <span data-ttu-id="0ab64-110">**lib\_identitás\_map.so** a a **build/modulok/identitymap** mappa.</span><span class="sxs-lookup"><span data-stu-id="0ab64-110">**lib\_identity\_map.so** in the **build/modules/identitymap** folder.</span></span>
* <span data-ttu-id="0ab64-111">**libsimulated\_device.so** a a **build, modulok vagy szimulált\_eszköz** mappát.</span><span class="sxs-lookup"><span data-stu-id="0ab64-111">**libsimulated\_device.so** in the **build/modules/simulated\_device** folder.</span></span>

<span data-ttu-id="0ab64-112">Az elérési utak használata a **modul elérési útján** értékeket az alábbi JSON fájlban látható módon:</span><span class="sxs-lookup"><span data-stu-id="0ab64-112">Use these paths for the **module path** values as shown in the following JSON settings file:</span></span>

<span data-ttu-id="0ab64-113">A szimulált\_eszköz\_felhő\_feltöltése\_minta tart az elérési út egy JSON-konfigurációs fájl parancssori argumentumként.</span><span class="sxs-lookup"><span data-stu-id="0ab64-113">The simulated\_device\_cloud\_upload\_sample process takes the path to a JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="0ab64-114">A következő példa JSON-fájl megtalálható a következő SDK tárház **minták\\szimulált\_eszköz\_felhő\_feltöltése\_minta\\src\\ Szimulált\_eszköz\_felhő\_feltöltése\_minta\_lin.json**.</span><span class="sxs-lookup"><span data-stu-id="0ab64-114">The following example JSON file is provided in the SDK repository at **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_lin.json**.</span></span> <span data-ttu-id="0ab64-115">A konfigurációs fájl, kivéve, ha módosítja a build parancsfájlt helyezze el az IoT peremhálózati modulok vagy a minta végrehajtható fájlok alapértelmezettől eltérő helyeket működik.</span><span class="sxs-lookup"><span data-stu-id="0ab64-115">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="0ab64-116">A modul elérési útvonalai képest a könyvtárat az futtató a szimulált\_eszköz\_felhő\_feltöltése\_minta végrehajtható fájl, nem a könyvtárat, amelyben a végrehajtható fájl található.</span><span class="sxs-lookup"><span data-stu-id="0ab64-116">The module paths are relative to the directory from where you run the simulated\_device\_cloud\_upload\_sample executable, not the directory where the executable is located.</span></span> <span data-ttu-id="0ab64-117">A minta JSON-konfigurációs fájl az alapértelmezett az aktuális munkakönyvtárban a "deviceCloudUploadGatewaylog.log" írásakor.</span><span class="sxs-lookup"><span data-stu-id="0ab64-117">The sample JSON configuration file defaults to writing to 'deviceCloudUploadGatewaylog.log' in your current working directory.</span></span>

<span data-ttu-id="0ab64-118">Egy szövegszerkesztőben nyissa meg a fájlt **minták/szimulált\_eszköz\_felhő\_feltöltése\_minta/src/szimulált\_eszköz\_felhő\_feltöltése\_lin.json** a helyi példánya a **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="0ab64-118">In a text editor, open the file **samples/simulated\_device\_cloud\_upload\_sample/src/simulated\_device\_cloud\_upload\_lin.json** in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="0ab64-119">Ez a fájl az IoT-Edge modulok konfigurálja a minta átjáró:</span><span class="sxs-lookup"><span data-stu-id="0ab64-119">This file configures the IoT Edge modules in the sample gateway:</span></span>

* <span data-ttu-id="0ab64-120">A **IOT hubbal** modul csatlakozik az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0ab64-120">The **IoTHub** module connects to your IoT hub.</span></span> <span data-ttu-id="0ab64-121">Konfigurálja az IoT hub adatküldéshez.</span><span class="sxs-lookup"><span data-stu-id="0ab64-121">You configure it to send data to your IoT hub.</span></span> <span data-ttu-id="0ab64-122">Pontosabban, állítsa be a **IoTHubName** értékre az IoT hub nevét, és állítsa a **IoTHubSuffix** egy érték **azure-devices.net**.</span><span class="sxs-lookup"><span data-stu-id="0ab64-122">Specifically, set the **IoTHubName** value to the name of your IoT hub and set the **IoTHubSuffix** value to **azure-devices.net**.</span></span> <span data-ttu-id="0ab64-123">Állítsa be a **átviteli** egyik: **HTTP**, **AMQP**, vagy **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="0ab64-123">Set the **Transport** value to one of: **HTTP**, **AMQP**, or **MQTT**.</span></span> <span data-ttu-id="0ab64-124">Jelenleg csak **HTTP** közösen használja az összes eszközre küldött üzenetek egy TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="0ab64-124">Currently, only **HTTP** shares one TCP connection for all device messages.</span></span> <span data-ttu-id="0ab64-125">Ha az érték **AMQP**, vagy **MQTT**, az átjáró egy külön TCP-kapcsolatot az IoT-központ az egyes eszközök tart fenn.</span><span class="sxs-lookup"><span data-stu-id="0ab64-125">If you set the value to **AMQP**, or **MQTT**, the gateway maintains a separate TCP connection to IoT Hub for each device.</span></span>
* <span data-ttu-id="0ab64-126">A **leképezési** modul szimulált eszköz MAC-címet rendel az IoT Hub eszköz azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="0ab64-126">The **mapping** module maps the MAC addresses of your simulated devices to your IoT Hub device ids.</span></span> <span data-ttu-id="0ab64-127">Győződjön meg arról, hogy **deviceId** megegyeznek a két eszköz az IoT hub, és hogy a hozzáadott azonosítóit a **deviceKey** értékek a kulcsokat, a két eszközök tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="0ab64-127">Make sure that **deviceId** values match the ids of the two devices you added to your IoT hub, and that the **deviceKey** values contain the keys of your two devices.</span></span>
* <span data-ttu-id="0ab64-128">A **BLE1** és **BLE2** modulokra a szimulált eszköz.</span><span class="sxs-lookup"><span data-stu-id="0ab64-128">The **BLE1** and **BLE2** modules are the simulated devices.</span></span> <span data-ttu-id="0ab64-129">Vegye figyelembe, hogyan a MAC-cím egyezik-e a címeket a **leképezési** modul.</span><span class="sxs-lookup"><span data-stu-id="0ab64-129">Note how their MAC addresses match the addresses in the **mapping** module.</span></span>
* <span data-ttu-id="0ab64-130">A **naplózó** modul fájlban naplózza a átjáró tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="0ab64-130">The **Logger** module logs your gateway activity to a file.</span></span>
* <span data-ttu-id="0ab64-131">A **modul elérési útján** értékek, a példában látható módon azt feltételezik, hogy futtasson-e a mintát a **build** mappa a helyi példánya a **iot-edge** tárház.</span><span class="sxs-lookup"><span data-stu-id="0ab64-131">The **module path** values shown in the example assume that you run the sample from the **build** folder in your local copy of the **iot-edge** repository.</span></span>
* <span data-ttu-id="0ab64-132">A **hivatkozások** tömb a JSON-fájl alján csatlakozik a **BLE1** és **BLE2** modulok a **leképezési** modul, és a **leképezési** modult a **IOT hubbal** modul.</span><span class="sxs-lookup"><span data-stu-id="0ab64-132">The **links** array at the bottom of the JSON file connects the **BLE1** and **BLE2** modules to the **mapping** module, and the **mapping** module to the **IoTHub** module.</span></span> <span data-ttu-id="0ab64-133">Emellett biztosítja, hogy az összes üzenet által naplózott a **naplózó** modul.</span><span class="sxs-lookup"><span data-stu-id="0ab64-133">It also ensures that all messages are logged by the **Logger** module.</span></span>

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
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
              "module.path": "./modules/identitymap/libidentity_map.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
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
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

<span data-ttu-id="0ab64-134">A konfigurációs fájl módosításainak mentése.</span><span class="sxs-lookup"><span data-stu-id="0ab64-134">Save the changes you made to the configuration file.</span></span>

<span data-ttu-id="0ab64-135">A minta futtatásához:</span><span class="sxs-lookup"><span data-stu-id="0ab64-135">To run the sample:</span></span>

1. <span data-ttu-id="0ab64-136">A rendszerhéj navigáljon a **létrehozása, az iot-edge** mappát.</span><span class="sxs-lookup"><span data-stu-id="0ab64-136">In your shell, navigate to the **iot-edge/build** folder.</span></span>
2. <span data-ttu-id="0ab64-137">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="0ab64-137">Run the following command:</span></span>
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. <span data-ttu-id="0ab64-138">Használhatja a [eszköz explorer] [ lnk-device-explorer] vagy [IOT hubbal-explorer] [ lnk-iothub-explorer] az üzeneteket, amelyek az IoT-központ fogadja az átjáró figyelésére.</span><span class="sxs-lookup"><span data-stu-id="0ab64-138">You can use the [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool to monitor the messages that IoT hub receives from the gateway.</span></span> <span data-ttu-id="0ab64-139">Például az IOT hubbal-Explorerben figyelheti eszköz-a-felhőbe küldött üzeneteket a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0ab64-139">For example, using iothub-explorer you can monitor device-to-cloud messages using the following command:</span></span>

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a><span data-ttu-id="0ab64-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0ab64-140">Next steps</span></span>

<span data-ttu-id="0ab64-141">Azure IoT peremhálózati tájékozottabbak kapnak, és néhány kódpéldák kísérletezhet, látogasson el az alábbi fejlesztői oktatóanyagok és erőforrások:</span><span class="sxs-lookup"><span data-stu-id="0ab64-141">To gain a more advanced understanding of Azure IoT Edge and experiment with some code examples, visit the following developer tutorials and resources:</span></span>

* <span data-ttu-id="0ab64-142">[Eszköz-felhő üzeneteket küldeni egy fizikai eszközről Azure IoT oldala][lnk-physical-device]</span><span class="sxs-lookup"><span data-stu-id="0ab64-142">[Send device-to-cloud messages from a physical device with Azure IoT Edge][lnk-physical-device]</span></span>
* <span data-ttu-id="0ab64-143">[Az Azure IoT él][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="0ab64-143">[Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="0ab64-144">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="0ab64-144">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="0ab64-145">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="0ab64-145">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="0ab64-146">[Az IoT-megoldásból az alapoktól biztonságos mentése][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="0ab64-146">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
