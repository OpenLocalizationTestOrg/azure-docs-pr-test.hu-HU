---
title: "aaaGet Azure IoT Hub kezelés (csomópont) használatába |} Microsoft Docs"
description: "Hogyan toouse IoT Hub eszköz felügyeleti tooinitiate egy távoli eszköz újraindul. Node.js tooimplement a szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és a szolgáltatás alkalmazást, amely meghívja a közvetlen módszer hello hello Azure IoT SDK-t használja."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: juanpere
ms.openlocfilehash: 5dd1878e71231850fb95f4170b823f1e86c3ee83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-node"></a><span data-ttu-id="03b1f-104">Ismerkedés az Eszközfelügyelet (csomópont)</span><span class="sxs-lookup"><span data-stu-id="03b1f-104">Get started with device management (Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="03b1f-105">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="03b1f-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="03b1f-106">Az Azure portál toocreate az IoT-központ hello használata, és az IoT hub hozzon létre egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="03b1f-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="03b1f-107">Létrehoz egy szimulált eszköz alkalmazást, amely tartalmazza a közvetlen módszer, amely az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="03b1f-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="03b1f-108">Közvetlen módszerek hello felhőből hívják.</span><span class="sxs-lookup"><span data-stu-id="03b1f-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="03b1f-109">Hozzon létre egy Node.js-Konzolalkalmazás, amely behívja hello újraindítás közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="03b1f-109">Create a Node.js console app that calls hello reboot direct method in hello simulated device app through your IoT hub.</span></span>

<span data-ttu-id="03b1f-110">Ez az oktatóanyag végén hello két Node.js konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="03b1f-110">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="03b1f-111">**dmpatterns_getstarted_device.js**, amely kapcsolódik tooyour IoT-központ hello eszközidentitás, korábban létrehozott kap egy újraindítás közvetlen módszer, fizikai újraindítás szimulálja, és hello idő az utolsó újraindítás hello jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="03b1f-111">**dmpatterns_getstarted_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports hello time for hello last reboot.</span></span>

<span data-ttu-id="03b1f-112">**dmpatterns_getstarted_service.js**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban jeleníti meg a hello választ, és megjeleníti hello frissítése jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="03b1f-112">**dmpatterns_getstarted_service.js**, which calls a direct method in hello simulated device app, displays hello response, and displays hello updated reported properties.</span></span>

<span data-ttu-id="03b1f-113">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="03b1f-113">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="03b1f-114">NODE.js-verzió 0.12.x vagy újabb verzióját,</span><span class="sxs-lookup"><span data-stu-id="03b1f-114">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="03b1f-115">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="03b1f-115">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="03b1f-116">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="03b1f-116">An active Azure account.</span></span> <span data-ttu-id="03b1f-117">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="03b1f-117">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="03b1f-118">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="03b1f-118">Create a simulated device app</span></span>
<span data-ttu-id="03b1f-119">Ez a szakasz tartalma</span><span class="sxs-lookup"><span data-stu-id="03b1f-119">In this section, you will</span></span>

* <span data-ttu-id="03b1f-120">Hozzon létre egy Node.js-Konzolalkalmazás, amely válaszol a hello felhő által meghívott tooa közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="03b1f-120">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="03b1f-121">A szimulált eszköz újraindítását eseményindító</span><span class="sxs-lookup"><span data-stu-id="03b1f-121">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="03b1f-122">Használjon hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök, és ha azok utolsó újraindítása</span><span class="sxs-lookup"><span data-stu-id="03b1f-122">Use hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted</span></span>

1. <span data-ttu-id="03b1f-123">Hozzon létre egy **manageddevice** nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="03b1f-123">Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="03b1f-124">A hello **manageddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="03b1f-124">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="03b1f-125">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="03b1f-125">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="03b1f-126">A parancssorban hello **manageddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt**csomag:</span><span class="sxs-lookup"><span data-stu-id="03b1f-126">At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="03b1f-127">Egy szövegszerkesztő használatával hozzon létre egy **dmpatterns_getstarted_device.js** hello fájlban **manageddevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="03b1f-127">Using a text editor, create a **dmpatterns_getstarted_device.js** file in hello **manageddevice** folder.</span></span>
4. <span data-ttu-id="03b1f-128">Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_getstarted_device.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="03b1f-128">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="03b1f-129">Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány.</span><span class="sxs-lookup"><span data-stu-id="03b1f-129">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  <span data-ttu-id="03b1f-130">Hello kapcsolati karakterláncot cserélje le a eszköz kapcsolati karakterláncát.</span><span class="sxs-lookup"><span data-stu-id="03b1f-130">Replace hello connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="03b1f-131">Adja hozzá a következő függvény tooimplement hello közvetlen módszer hello eszközön hello</span><span class="sxs-lookup"><span data-stu-id="03b1f-131">Add hello following function tooimplement hello direct method on hello device</span></span>
   
    ```
    var onReboot = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report hello reboot before hello physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. <span data-ttu-id="03b1f-132">Nyissa meg a hello kapcsolat tooyour IoT-központ, és indítsa el a hello közvetlen módszer figyelő:</span><span class="sxs-lookup"><span data-stu-id="03b1f-132">Open hello connection tooyour IoT hub and start hello direct method listener:</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. <span data-ttu-id="03b1f-133">Mentse és zárja be a hello **dmpatterns_getstarted_device.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="03b1f-133">Save and close hello **dmpatterns_getstarted_device.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="03b1f-134">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="03b1f-134">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="03b1f-135">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="03b1f-135">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="03b1f-136">Eseményindító közvetlen metódussal hello eszközön távoli újraindítás</span><span class="sxs-lookup"><span data-stu-id="03b1f-136">Trigger a remote reboot on hello device using a direct method</span></span>
<span data-ttu-id="03b1f-137">Ebben a szakaszban egy Node.js-Konzolalkalmazás közvetlen metódussal eszköz távoli újraindítást kezdeményezett hoz létre.</span><span class="sxs-lookup"><span data-stu-id="03b1f-137">In this section, you create a Node.js console app that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="03b1f-138">hello alkalmazás eszköz iker lekérdezések toodiscover hello legutóbbi újraindítás használ, az eszköznek.</span><span class="sxs-lookup"><span data-stu-id="03b1f-138">hello app uses device twin queries toodiscover hello last reboot time for that device.</span></span>

1. <span data-ttu-id="03b1f-139">Hozzon létre egy üres nevű **triggerrebootondevice**.</span><span class="sxs-lookup"><span data-stu-id="03b1f-139">Create an empty folder called **triggerrebootondevice**.</span></span>  <span data-ttu-id="03b1f-140">A hello **triggerrebootondevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="03b1f-140">In hello **triggerrebootondevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="03b1f-141">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="03b1f-141">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="03b1f-142">A parancssorban hello **triggerrebootondevice** mappa, futtassa a következő parancs tooinstall hello hello **azure-IOT hubbal** eszköz SDK-csomagot és **azure-iot-eszközök – mqtt** csomag:</span><span class="sxs-lookup"><span data-stu-id="03b1f-142">At your command prompt in hello **triggerrebootondevice** folder, run hello following command tooinstall hello **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="03b1f-143">Egy szövegszerkesztő használatával hozzon létre egy **dmpatterns_getstarted_service.js** hello fájlban **triggerrebootondevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="03b1f-143">Using a text editor, create a **dmpatterns_getstarted_service.js** file in hello **triggerrebootondevice** folder.</span></span>
4. <span data-ttu-id="03b1f-144">Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_getstarted_service.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="03b1f-144">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="03b1f-145">Adja hozzá a következő változók deklarációja hello, és cserélje le a hello helyőrző értékeket:</span><span class="sxs-lookup"><span data-stu-id="03b1f-145">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. <span data-ttu-id="03b1f-146">Adja hozzá a következő függvény tooinvoke hello eszköz metódus tooreboot hello céleszköz hello:</span><span class="sxs-lookup"><span data-stu-id="03b1f-146">Add hello following function tooinvoke hello device method tooreboot hello target device:</span></span>
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked hello device tooreboot.");  
            }
        });
    };
    ```
7. <span data-ttu-id="03b1f-147">Adja hozzá a hello következő tooquery hello eszköz funkciót, és a legutóbbi újraindítás hello beolvasása:</span><span class="sxs-lookup"><span data-stu-id="03b1f-147">Add hello following function tooquery for hello device and get hello last reboot time:</span></span>
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device tooreport last reboot time.');
        });
    };
    ```
8. <span data-ttu-id="03b1f-148">Adja hozzá a következő kód toocall hello funkciók hello kiváltó hello indítsa újra a közvetlen módszer és hello a lekérdezés utolsó újraindítás időpontja:</span><span class="sxs-lookup"><span data-stu-id="03b1f-148">Add hello following code toocall hello functions that trigger hello reboot direct method and query for hello last reboot time:</span></span>
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. <span data-ttu-id="03b1f-149">Mentse és zárja be a hello **dmpatterns_getstarted_service.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="03b1f-149">Save and close hello **dmpatterns_getstarted_service.js** file.</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="03b1f-150">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="03b1f-150">Run hello apps</span></span>
<span data-ttu-id="03b1f-151">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="03b1f-151">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="03b1f-152">Parancssorba hello hello **manageddevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.</span><span class="sxs-lookup"><span data-stu-id="03b1f-152">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="03b1f-153">Parancssorba hello hello **triggerrebootondevice** mappa, futtassa a következő parancs tootrigger hello távoli hello újraindul, és lekérdezi a hello eszköz iker toofind hello utolsó indítsa újra a számítógépet idő.</span><span class="sxs-lookup"><span data-stu-id="03b1f-153">At hello command prompt in hello **triggerrebootondevice** folder, run hello following command tootrigger hello remote reboot and query for hello device twin toofind hello last reboot time.</span></span>
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. <span data-ttu-id="03b1f-154">Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.</span><span class="sxs-lookup"><span data-stu-id="03b1f-154">You see hello device response toohello direct method in hello console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
