---
title: "Csatlakoztassa a Node.js használatával |} Microsoft Docs"
description: "Eszköz csatlakoztatása az Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek olyan alkalmazással Node.js nyelven írt ismerteti."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 6459b6196eb7f4a083b67e5a421bcc0d51d39e5c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a><span data-ttu-id="a3687-103">Csatlakoztassa az eszközt a távoli felügyeleti előkonfigurált megoldás (Node.js)</span><span class="sxs-lookup"><span data-stu-id="a3687-103">Connect your device to the remote monitoring preconfigured solution (Node.js)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a><span data-ttu-id="a3687-104">Node.js sample megoldás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3687-104">Create a node.js sample solution</span></span>

<span data-ttu-id="a3687-105">Győződjön meg arról, hogy Node.js verziót 0.11.5, vagy később a fejlesztői gépen telepítve.</span><span class="sxs-lookup"><span data-stu-id="a3687-105">Ensure that Node.js version 0.11.5 or later is installed on your development machine.</span></span> <span data-ttu-id="a3687-106">Futtathat `node --version` verziójának a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="a3687-106">You can run `node --version` at the command line to check the version.</span></span>

1. <span data-ttu-id="a3687-107">Hozzon létre egy nevű **RemoteMonitoring** a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="a3687-107">Create a folder called **RemoteMonitoring** on your development machine.</span></span> <span data-ttu-id="a3687-108">Keresse meg a mappát a parancssori környezetben.</span><span class="sxs-lookup"><span data-stu-id="a3687-108">Navigate to this folder in your command-line environment.</span></span>

1. <span data-ttu-id="a3687-109">Futtassa az alábbi parancsokat a végre kell hajtania a mintaalkalmazás letöltése és telepítése a csomagok:</span><span class="sxs-lookup"><span data-stu-id="a3687-109">Run the following commands to download and install the packages you need to complete the sample app:</span></span>

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="a3687-110">Az a **RemoteMonitoring** mappa, hozzon létre egy nevű fájlt **remote_monitoring.js**.</span><span class="sxs-lookup"><span data-stu-id="a3687-110">In the **RemoteMonitoring** folder, create a file called **remote_monitoring.js**.</span></span> <span data-ttu-id="a3687-111">Nyissa meg ezt a fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a3687-111">Open this file in a text editor.</span></span>

1. <span data-ttu-id="a3687-112">Az a **remote_monitoring.js** fájlt, adja hozzá a következő `require` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="a3687-112">In the **remote_monitoring.js** file, add the following `require` statements:</span></span>

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. <span data-ttu-id="a3687-113">Adja hozzá a következő változódeklarációkat az `require` utasítások után.</span><span class="sxs-lookup"><span data-stu-id="a3687-113">Add the following variable declarations after the `require` statements.</span></span> <span data-ttu-id="a3687-114">Cserélje le a [Device Id] és a [Device Key] helyőrzőértékeket az eszközhöz tartozó értékekre a távoli figyelési megoldás irányítópultja alapján.</span><span class="sxs-lookup"><span data-stu-id="a3687-114">Replace the placeholder values [Device Id] and [Device Key] with values you noted for your device in the remote monitoring solution dashboard.</span></span> <span data-ttu-id="a3687-115">Cserélje le az [IoTHub Name] értéket a megoldás irányítópultján található IoT Hub gazdanévre.</span><span class="sxs-lookup"><span data-stu-id="a3687-115">Use the IoT Hub Hostname from the solution dashboard to replace [IoTHub Name].</span></span> <span data-ttu-id="a3687-116">Ha például az IoT Hub gazdaneve **contoso.azure-devices.net**, cserélje le az [IoTHub Name] helyőrzőt a **contoso** értékre:</span><span class="sxs-lookup"><span data-stu-id="a3687-116">For example, if your IoT Hub Hostname is **contoso.azure-devices.net**, replace [IoTHub Name] with **contoso**:</span></span>

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. <span data-ttu-id="a3687-117">Adja hozzá a következő változók néhány alapvető telemetriai adatok meghatározásához:</span><span class="sxs-lookup"><span data-stu-id="a3687-117">Add the following variables to define some base telemetry data:</span></span>

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. <span data-ttu-id="a3687-118">Adja hozzá a következő segítő függvény nyomtatni a művelet eredménye:</span><span class="sxs-lookup"><span data-stu-id="a3687-118">Add the following helper function to print operation results:</span></span>

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. <span data-ttu-id="a3687-119">Adja hozzá a következő segítő függvény használatával a telemetriai adatok értékek ügyfélfuttatási:</span><span class="sxs-lookup"><span data-stu-id="a3687-119">Add the following helper function to use to randomize the telemetry values:</span></span>

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. <span data-ttu-id="a3687-120">Adja hozzá a következő definícióját a **deviceinfo információja** objektum indítási küld az eszköz:</span><span class="sxs-lookup"><span data-stu-id="a3687-120">Add the following definition for the **DeviceInfo** object the device sends on startup:</span></span>

    ```nodejs
    var deviceMetaData = {
        'ObjectType': 'DeviceInfo',
        'IsSimulatedDevice': 0,
        'Version': '1.0',
        'DeviceProperties': {
            'DeviceID': deviceId,
            'HubEnabledState': 1
        }
    };
    ```

1. <span data-ttu-id="a3687-121">Adja hozzá a következő definícióját az eszköz iker jelentett értékek.</span><span class="sxs-lookup"><span data-stu-id="a3687-121">Add the following definition for the device twin reported values.</span></span> <span data-ttu-id="a3687-122">Ez a definíció tartalmazza az eszköz támogatja a közvetlen módszerek:</span><span class="sxs-lookup"><span data-stu-id="a3687-122">This definition includes descriptions of the direct methods the device supports:</span></span>

    ```nodejs
    var reportedProperties = {
        "Device": {
            "DeviceState": "normal",
            "Location": {
                "Latitude": 47.642877,
                "Longitude": -122.125497
            }
        },
        "Config": {
            "TemperatureMeanValue": 56.7,
            "TelemetryInterval": 45
        },
        "System": {
            "Manufacturer": "Contoso Inc.",
            "FirmwareVersion": "2.22",
            "InstalledRAM": "8 MB",
            "ModelNumber": "DB-14",
            "Platform": "Plat 9.75",
            "Processor": "i3-9",
            "SerialNumber": "SER99"
        },
        "Location": {
            "Latitude": 47.642877,
            "Longitude": -122.125497
        },
        "SupportedMethods": {
            "Reboot": "Reboot the device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI to specifiy the URI of the firmware file"
        },
    }
    ```

1. <span data-ttu-id="a3687-123">Adja hozzá a következő függvény kezelni a **újraindítás** közvetlen metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="a3687-123">Add the following function to handle the **Reboot** direct method call:</span></span>

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete the response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. <span data-ttu-id="a3687-124">Adja hozzá a következő függvény kezelni a **InitiateFirmwareUpdate** közvetlen metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="a3687-124">Add the following function to handle the **InitiateFirmwareUpdate** direct method call.</span></span> <span data-ttu-id="a3687-125">Ez a közvetlen módszer egy paraméter használatával adja meg a belső vezérlőprogram kép helyét, és kezdeményezi a belső vezérlőprogram frissítése az eszközön aszinkron módon:</span><span class="sxs-lookup"><span data-stu-id="a3687-125">This direct method uses a parameter to specify the location of the firmware image to download, and initiates the firmware update on the device asynchronously:</span></span>

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete the response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here to perform the firmware update asynchronously
    }
    ```

1. <span data-ttu-id="a3687-126">Adja hozzá az ügyfél-példány létrehozása a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="a3687-126">Add the following code to create a client instance:</span></span>

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="a3687-127">Adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="a3687-127">Add the following code to:</span></span>

    * <span data-ttu-id="a3687-128">Nyissa meg a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a3687-128">Open the connection.</span></span>
    * <span data-ttu-id="a3687-129">Küldjön a **deviceinfo információja** objektum.</span><span class="sxs-lookup"><span data-stu-id="a3687-129">Send the **DeviceInfo** object.</span></span>
    * <span data-ttu-id="a3687-130">A kezelő kívánt tulajdonságok beállítása.</span><span class="sxs-lookup"><span data-stu-id="a3687-130">Set up a handler for desired properties.</span></span>
    * <span data-ttu-id="a3687-131">Jelentett tulajdonságok küldése.</span><span class="sxs-lookup"><span data-stu-id="a3687-131">Send reported properties.</span></span>
    * <span data-ttu-id="a3687-132">A közvetlen módszer kezelők regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="a3687-132">Register handlers for the direct methods.</span></span>
    * <span data-ttu-id="a3687-133">Indítsa el a telemetriai adatok küldését.</span><span class="sxs-lookup"><span data-stu-id="a3687-133">Start sending telemetry.</span></span>

    ```nodejs
    client.open(function (err) {
        if (err) {
            printErrorFor('open')(err);
        } else {
            console.log('Sending device metadata:\n' + JSON.stringify(deviceMetaData));
            client.sendEvent(new Message(JSON.stringify(deviceMetaData)), printErrorFor('send metadata'));

            // Create device twin
            client.getTwin(function(err, twin) {
                if (err) {
                    console.error('Could not get device twin');
                } else {
                    console.log('Device twin created');

                    twin.on('properties.desired', function(delta) {
                        console.log('Received new desired properties:');
                        console.log(JSON.stringify(delta));
                    });

                    // Send reported properties
                    twin.properties.reported.update(reportedProperties, function(err) {
                        if (err) throw err;
                        console.log('twin state reported');
                    });

                    // Register handlers for direct methods
                    client.onDeviceMethod('Reboot', onReboot);
                    client.onDeviceMethod('InitiateFirmwareUpdate', onInitiateFirmwareUpdate);
                }
            });

            // Start sending telemetry
            var sendInterval = setInterval(function () {
                temperature += generateRandomIncrement();
                externalTemperature += generateRandomIncrement();
                humidity += generateRandomIncrement();

                var data = JSON.stringify({
                    'DeviceID': deviceId,
                    'Temperature': temperature,
                    'Humidity': humidity,
                    'ExternalTemperature': externalTemperature
                });

                console.log('Sending device event data:\n' + data);
                client.sendEvent(new Message(data), printErrorFor('send event'));
            }, 5000);

            client.on('error', function (err) {
                printErrorFor('client')(err);
                if (sendInterval) clearInterval(sendInterval);
                client.close(printErrorFor('client.close'));
            });
        }
    });
    ```

1. <span data-ttu-id="a3687-134">A módosítások mentése a **remote_monitoring.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="a3687-134">Save the changes to the **remote_monitoring.js** file.</span></span>

1. <span data-ttu-id="a3687-135">A következő parancsot egy parancssorból indítsa el a mintaalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="a3687-135">Run the following command at a command prompt to launch the sample application:</span></span>
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
