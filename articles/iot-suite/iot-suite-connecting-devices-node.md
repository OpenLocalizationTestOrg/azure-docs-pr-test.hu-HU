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
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-nodejs"></a>Csatlakozzon a távoli felügyeleti előkonfigurált megoldás (Node.js) eszköz toohello
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a>Node.js sample megoldás létrehozása

Győződjön meg arról, hogy Node.js verziót 0.11.5, vagy később a fejlesztői gépen telepítve. Futtathat `node --version` hello parancssori toocheck hello verziójú.

1. Hozzon létre egy nevű **RemoteMonitoring** a fejlesztési számítógépén. Keresse meg a parancssori környezetben toothis mappát.

1. A következő futtatási hello parancsok toodownload és a telepítés hello csomagok toocomplete hello mintaalkalmazás van szüksége:

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. A hello **RemoteMonitoring** mappa, hozzon létre egy nevű fájlt **remote_monitoring.js**. Nyissa meg ezt a fájlt egy szövegszerkesztőben.

1. A hello **remote_monitoring.js** fájlt, adja hozzá a következő hello `require` utasításokat:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Adja hozzá a következő változók deklarációja után hello hello `require` utasításokat. Cserélje le a hello helyőrző értékeket [eszközazonosító] és [eszközkulcs] az eszköz a távoli felügyeleti megoldás irányítópultja hello feljegyzett értékekkel. Hello megoldás irányítópult tooreplace [IOT hubbal Name] az IoT Hub állomásnév hello használata. Ha például az IoT Hub gazdaneve **contoso.azure-devices.net**, cserélje le az [IoTHub Name] helyőrzőt a **contoso** értékre:

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Adja hozzá a következő változók toodefine hello néhány alapvető telemetriai adatokat:

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. Adja hozzá a következő segítő függvény tooprint művelet eredményeit hello:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Adja hozzá a következő segítő függvény toouse toorandomize hello telemetriai értékek hello:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Adja hozzá a következő hello definíciója hello **deviceinfo információja** objektum hello eszköz küld indításakor:

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

1. Adja hozzá a következő hello hello eszköz iker definíciója jelentett értékek. Ez a definíció hello hello eszköz támogatja a közvetlen módszerek leírását tartalmazza:

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

1. Adja hozzá a következő függvény toohandle hello hello **újraindítás** közvetlen metódus hívása:

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

1. Adja hozzá a következő függvény toohandle hello hello **InitiateFirmwareUpdate** közvetlen metódus hívása. Ez a közvetlen módszer használja az hello belső vezérlőprogram kép toodownload paraméter toospecify hello helyre, és kezdeményezi aszinkron módon hello hello eszköz belső vezérlőprogram frissítése:

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

1. Adja hozzá a következő kód toocreate egy ügyfél példány hello:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Adja hozzá a következő kódot a hello:

    * Hello kapcsolat megnyitásához.
    * Hello küldése **deviceinfo információja** objektum.
    * A kezelő kívánt tulajdonságok beállítása.
    * Jelentett tulajdonságok küldése.
    * Regisztrálja a kezelők hello közvetlen módszer.
    * Indítsa el a telemetriai adatok küldését.

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

1. Mentse a módosításokat toohello hello **remote_monitoring.js** fájlt.

1. Futtassa a következő parancsot a következő parancssor toolaunch hello mintaalkalmazás hello:
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
