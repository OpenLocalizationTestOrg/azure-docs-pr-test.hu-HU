---
title: "egy eszköz Node.js segítségével aaaConnect |} Microsoft Docs"
description: "Ismerteti, hogyan tooconnect egy eszköz toohello Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek Node.js nyelven írt alkalmazás segítségével."
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
ms.openlocfilehash: 80bf2b70f15f539bfce4f135d533c46dd2b3f5a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-nodejs"></a><span data-ttu-id="a7abb-103">Csatlakozzon a távoli felügyeleti előkonfigurált megoldás (Node.js) eszköz toohello</span><span class="sxs-lookup"><span data-stu-id="a7abb-103">Connect your device toohello remote monitoring preconfigured solution (Node.js)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a><span data-ttu-id="a7abb-104">Node.js sample megoldás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7abb-104">Create a node.js sample solution</span></span>

<span data-ttu-id="a7abb-105">Győződjön meg arról, hogy Node.js verziót 0.11.5, vagy később a fejlesztői gépen telepítve.</span><span class="sxs-lookup"><span data-stu-id="a7abb-105">Ensure that Node.js version 0.11.5 or later is installed on your development machine.</span></span> <span data-ttu-id="a7abb-106">Futtathat `node --version` hello parancssori toocheck hello verziójú.</span><span class="sxs-lookup"><span data-stu-id="a7abb-106">You can run `node --version` at hello command line toocheck hello version.</span></span>

1. <span data-ttu-id="a7abb-107">Hozzon létre egy nevű **RemoteMonitoring** a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="a7abb-107">Create a folder called **RemoteMonitoring** on your development machine.</span></span> <span data-ttu-id="a7abb-108">Keresse meg a parancssori környezetben toothis mappát.</span><span class="sxs-lookup"><span data-stu-id="a7abb-108">Navigate toothis folder in your command-line environment.</span></span>

1. <span data-ttu-id="a7abb-109">A következő futtatási hello parancsok toodownload és a telepítés hello csomagok toocomplete hello mintaalkalmazás van szüksége:</span><span class="sxs-lookup"><span data-stu-id="a7abb-109">Run hello following commands toodownload and install hello packages you need toocomplete hello sample app:</span></span>

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="a7abb-110">A hello **RemoteMonitoring** mappa, hozzon létre egy nevű fájlt **remote_monitoring.js**.</span><span class="sxs-lookup"><span data-stu-id="a7abb-110">In hello **RemoteMonitoring** folder, create a file called **remote_monitoring.js**.</span></span> <span data-ttu-id="a7abb-111">Nyissa meg ezt a fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a7abb-111">Open this file in a text editor.</span></span>

1. <span data-ttu-id="a7abb-112">A hello **remote_monitoring.js** fájlt, adja hozzá a következő hello `require` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="a7abb-112">In hello **remote_monitoring.js** file, add hello following `require` statements:</span></span>

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. <span data-ttu-id="a7abb-113">Adja hozzá a következő változók deklarációja után hello hello `require` utasításokat.</span><span class="sxs-lookup"><span data-stu-id="a7abb-113">Add hello following variable declarations after hello `require` statements.</span></span> <span data-ttu-id="a7abb-114">Cserélje le a hello helyőrző értékeket [eszközazonosító] és [eszközkulcs] az eszköz a távoli felügyeleti megoldás irányítópultja hello feljegyzett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="a7abb-114">Replace hello placeholder values [Device Id] and [Device Key] with values you noted for your device in hello remote monitoring solution dashboard.</span></span> <span data-ttu-id="a7abb-115">Hello megoldás irányítópult tooreplace [IOT hubbal Name] az IoT Hub állomásnév hello használata.</span><span class="sxs-lookup"><span data-stu-id="a7abb-115">Use hello IoT Hub Hostname from hello solution dashboard tooreplace [IoTHub Name].</span></span> <span data-ttu-id="a7abb-116">Ha például az IoT Hub gazdaneve **contoso.azure-devices.net**, cserélje le az [IoTHub Name] helyőrzőt a **contoso** értékre:</span><span class="sxs-lookup"><span data-stu-id="a7abb-116">For example, if your IoT Hub Hostname is **contoso.azure-devices.net**, replace [IoTHub Name] with **contoso**:</span></span>

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. <span data-ttu-id="a7abb-117">Adja hozzá a következő változók toodefine hello néhány alapvető telemetriai adatokat:</span><span class="sxs-lookup"><span data-stu-id="a7abb-117">Add hello following variables toodefine some base telemetry data:</span></span>

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. <span data-ttu-id="a7abb-118">Adja hozzá a következő segítő függvény tooprint művelet eredményeit hello:</span><span class="sxs-lookup"><span data-stu-id="a7abb-118">Add hello following helper function tooprint operation results:</span></span>

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. <span data-ttu-id="a7abb-119">Adja hozzá a következő segítő függvény toouse toorandomize hello telemetriai értékek hello:</span><span class="sxs-lookup"><span data-stu-id="a7abb-119">Add hello following helper function toouse toorandomize hello telemetry values:</span></span>

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. <span data-ttu-id="a7abb-120">Adja hozzá a következő hello definíciója hello **deviceinfo információja** objektum hello eszköz küld indításakor:</span><span class="sxs-lookup"><span data-stu-id="a7abb-120">Add hello following definition for hello **DeviceInfo** object hello device sends on startup:</span></span>

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

1. <span data-ttu-id="a7abb-121">Adja hozzá a következő hello hello eszköz iker definíciója jelentett értékek.</span><span class="sxs-lookup"><span data-stu-id="a7abb-121">Add hello following definition for hello device twin reported values.</span></span> <span data-ttu-id="a7abb-122">Ez a definíció hello hello eszköz támogatja a közvetlen módszerek leírását tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a7abb-122">This definition includes descriptions of hello direct methods hello device supports:</span></span>

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
            "Reboot": "Reboot hello device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI toospecifiy hello URI of hello firmware file"
        },
    }
    ```

1. <span data-ttu-id="a7abb-123">Adja hozzá a következő függvény toohandle hello hello **újraindítás** közvetlen metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="a7abb-123">Add hello following function toohandle hello **Reboot** direct method call:</span></span>

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete hello response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. <span data-ttu-id="a7abb-124">Adja hozzá a következő függvény toohandle hello hello **InitiateFirmwareUpdate** közvetlen metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="a7abb-124">Add hello following function toohandle hello **InitiateFirmwareUpdate** direct method call.</span></span> <span data-ttu-id="a7abb-125">Ez a közvetlen módszer használja az hello belső vezérlőprogram kép toodownload paraméter toospecify hello helyre, és kezdeményezi aszinkron módon hello hello eszköz belső vezérlőprogram frissítése:</span><span class="sxs-lookup"><span data-stu-id="a7abb-125">This direct method uses a parameter toospecify hello location of hello firmware image toodownload, and initiates hello firmware update on hello device asynchronously:</span></span>

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete hello response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here tooperform hello firmware update asynchronously
    }
    ```

1. <span data-ttu-id="a7abb-126">Adja hozzá a következő kód toocreate egy ügyfél példány hello:</span><span class="sxs-lookup"><span data-stu-id="a7abb-126">Add hello following code toocreate a client instance:</span></span>

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="a7abb-127">Adja hozzá a következő kódot a hello:</span><span class="sxs-lookup"><span data-stu-id="a7abb-127">Add hello following code to:</span></span>

    * <span data-ttu-id="a7abb-128">Hello kapcsolat megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a7abb-128">Open hello connection.</span></span>
    * <span data-ttu-id="a7abb-129">Hello küldése **deviceinfo információja** objektum.</span><span class="sxs-lookup"><span data-stu-id="a7abb-129">Send hello **DeviceInfo** object.</span></span>
    * <span data-ttu-id="a7abb-130">A kezelő kívánt tulajdonságok beállítása.</span><span class="sxs-lookup"><span data-stu-id="a7abb-130">Set up a handler for desired properties.</span></span>
    * <span data-ttu-id="a7abb-131">Jelentett tulajdonságok küldése.</span><span class="sxs-lookup"><span data-stu-id="a7abb-131">Send reported properties.</span></span>
    * <span data-ttu-id="a7abb-132">Regisztrálja a kezelők hello közvetlen módszer.</span><span class="sxs-lookup"><span data-stu-id="a7abb-132">Register handlers for hello direct methods.</span></span>
    * <span data-ttu-id="a7abb-133">Indítsa el a telemetriai adatok küldését.</span><span class="sxs-lookup"><span data-stu-id="a7abb-133">Start sending telemetry.</span></span>

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

1. <span data-ttu-id="a7abb-134">Mentse a módosításokat toohello hello **remote_monitoring.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="a7abb-134">Save hello changes toohello **remote_monitoring.js** file.</span></span>

1. <span data-ttu-id="a7abb-135">Futtassa a következő parancsot a következő parancssor toolaunch hello mintaalkalmazás hello:</span><span class="sxs-lookup"><span data-stu-id="a7abb-135">Run hello following command at a command prompt toolaunch hello sample application:</span></span>
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
