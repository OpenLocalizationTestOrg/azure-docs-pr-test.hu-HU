---
title: "az Azure IoT Hub (csomópont) aaaDevice belső vezérlőprogram frissítése |} Microsoft Docs"
description: "Hogyan toouse kezelés az Azure IoT Hub tooinitiate egy eszköz belső vezérlőprogram frissítése. A szimulált eszköz alkalmazásának és a service-alkalmazást, amely elindítja a hello belső vezérlőprogram frissítési hello Azure IoT SDK-k használata Node.js tooimplement."
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
ms.date: 02/06/2017
ms.author: juanpere
ms.openlocfilehash: 99d4b369e7aba334bf713e0c657e6e5d227fb691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-nodenode"></a><span data-ttu-id="b84c2-104">Használja az eszköz felügyeleti tooinitiate eszköz vezérlőprogram-frissítés (csomópontok /)</span><span class="sxs-lookup"><span data-stu-id="b84c2-104">Use device management tooinitiate a device firmware update (Node/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="b84c2-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b84c2-105">Introduction</span></span>
<span data-ttu-id="b84c2-106">A hello [Ismerkedés az eszközkezelés] [ lnk-dm-getstarted] az oktatóanyagban megtudhatta, hogyan toouse hello [eszköz iker] [ lnk-devtwin] és [közvetlen módszerek] [ lnk-c2dmethod] primitívek tooremotely eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="b84c2-106">In hello [Get started with device management][lnk-dm-getstarted] tutorial, you saw how toouse hello [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives tooremotely reboot a device.</span></span> <span data-ttu-id="b84c2-107">A oktatóanyag használja az ugyanazon az IoT-központ primitívek hello és nyújt útmutatást, és bemutatja, hogyan toodo egy-végpontok szimulált vezérlőprogram-frissítés.</span><span class="sxs-lookup"><span data-stu-id="b84c2-107">This tutorial uses hello same IoT Hub primitives and provides guidance and shows you how toodo an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="b84c2-108">Ebben a mintában hello Intel Edison eszköz minta hello belső vezérlőprogram frissítési végrehajtására szolgál.</span><span class="sxs-lookup"><span data-stu-id="b84c2-108">This pattern is used in hello firmware update implementation for hello Intel Edison device sample.</span></span>

<span data-ttu-id="b84c2-109">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="b84c2-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="b84c2-110">Hozzon létre egy Node.js-Konzolalkalmazás, amely behívja hello firmwareUpdate közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b84c2-110">Create a Node.js console app that calls hello firmwareUpdate direct method in hello simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="b84c2-111">Hozzon létre egy szimulált eszköz alkalmazást, amely egy **firmwareUpdate** közvetlen módszer.</span><span class="sxs-lookup"><span data-stu-id="b84c2-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="b84c2-112">Ez a módszer indít el egy többlépcsős folyamat, amely toodownload hello belső vezérlőprogram kép vár, hello belső vezérlőprogram lemezképet letölti és végül a hello belső vezérlőprogram kép vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b84c2-112">This method initiates a multi-stage process that waits toodownload hello firmware image, downloads hello firmware image, and finally applies hello firmware image.</span></span> <span data-ttu-id="b84c2-113">Minden egyes hello frissítés szakaszában hello eszköz által használt hello jelentett tulajdonságok tooreport folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="b84c2-113">During each stage of hello update, hello device uses hello reported properties tooreport on progress.</span></span>

<span data-ttu-id="b84c2-114">Ez az oktatóanyag végén hello két Node.js konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="b84c2-114">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="b84c2-115">**dmpatterns_fwupdate_service.js**, amely meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban jeleníti meg a hello válasz, és rendszeres időközönként (minden 500ms) frissítése megjeleníti hello jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="b84c2-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in hello simulated device app, displays hello response, and periodically (every 500ms) displays hello updated reported properties.</span></span>

<span data-ttu-id="b84c2-116">**dmpatterns_fwupdate_device.js**, tooyour IoT-központ köti össze a korábban létrehozott hello eszközidentitás firmwareUpdate közvetlen metódus kap, egy több államot folyamat toosimulate keresztül futtat, a belső vezérlőprogram frissítési többek között: hello vár Kép letöltése, hello új lemezkép letöltése, és végül a hello kép telepítése.</span><span class="sxs-lookup"><span data-stu-id="b84c2-116">**dmpatterns_fwupdate_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process toosimulate a firmware update including: waiting for hello image download, downloading hello new image, and finally applying hello image.</span></span>

<span data-ttu-id="b84c2-117">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b84c2-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="b84c2-118">NODE.js-verzió 0.12.x vagy újabb verzióját,</span><span class="sxs-lookup"><span data-stu-id="b84c2-118">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="b84c2-119">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Node.js ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="b84c2-119">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="b84c2-120">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="b84c2-120">An active Azure account.</span></span> <span data-ttu-id="b84c2-121">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="b84c2-121">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="b84c2-122">Hajtsa végre a hello [Ismerkedés az eszközkezelés](iot-hub-node-node-device-management-get-started.md) cikk toocreate az IoT hub és az IoT-központ kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="b84c2-122">Follow hello [Get started with device management](iot-hub-node-node-device-management-get-started.md) article toocreate your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a><span data-ttu-id="b84c2-123">Indítás, közvetlen metódussal hello eszközön távoli vezérlőprogram-frissítés</span><span class="sxs-lookup"><span data-stu-id="b84c2-123">Trigger a remote firmware update on hello device using a direct method</span></span>
<span data-ttu-id="b84c2-124">Ebben a szakaszban egy eszköz távoli vezérlőprogram-frissítés kezdeményező Node.js-Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b84c2-124">In this section, you create a Node.js console app that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="b84c2-125">hello alkalmazása használja-e a közvetlen módszer tooinitiate hello frissítése és által használt eszköz iker lekérdezések tooperiodically hello aktív belső vezérlőprogram frissítése hello állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b84c2-125">hello app uses a direct method tooinitiate hello update and uses device twin queries tooperiodically get hello status of hello active firmware update.</span></span>

1. <span data-ttu-id="b84c2-126">Hozzon létre egy üres nevű **triggerfwupdateondevice**.</span><span class="sxs-lookup"><span data-stu-id="b84c2-126">Create an empty folder called **triggerfwupdateondevice**.</span></span>  <span data-ttu-id="b84c2-127">A hello **triggerfwupdateondevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="b84c2-127">In hello **triggerfwupdateondevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="b84c2-128">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="b84c2-128">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="b84c2-129">A parancssorban hello **triggerfwupdateondevice** mappa, futtassa a következő parancs tooinstall hello hello **azure-iot-központ** és **azure-iot-eszközök – mqtt** eszköz SDK-csomagot:</span><span class="sxs-lookup"><span data-stu-id="b84c2-129">At your command prompt in hello **triggerfwupdateondevice** folder, run hello following command tooinstall hello **azure-iot-hub** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="b84c2-130">Egy szövegszerkesztő használatával hozzon létre egy **dmpatterns_getstarted_service.js** hello fájlban **triggerfwupdateondevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="b84c2-130">Using a text editor, create a **dmpatterns_getstarted_service.js** file in hello **triggerfwupdateondevice** folder.</span></span>
4. <span data-ttu-id="b84c2-131">Adja hozzá a hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_getstarted_service.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="b84c2-131">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="b84c2-132">Adja hozzá a következő változók deklarációja hello, és cserélje le a hello helyőrző értékeket:</span><span class="sxs-lookup"><span data-stu-id="b84c2-132">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. <span data-ttu-id="b84c2-133">Adja hozzá a következő hello toofind funkciót, és hello firmwareUpdate hello értékének megjelenítésére jelentett tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b84c2-133">Add hello following function toofind and display hello value of hello firmwareUpdate reported property.</span></span>
   
    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```
7. <span data-ttu-id="b84c2-134">Adja hozzá a következő függvény tooinvoke hello firmwareUpdate metódus tooreboot hello céleszköz hello:</span><span class="sxs-lookup"><span data-stu-id="b84c2-134">Add hello following function tooinvoke hello firmwareUpdate method tooreboot hello target device:</span></span>
   
    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
   
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
   
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
   
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start hello firmware update on hello device: ' + err.message)
        } 
      });
    };
    ```
8. <span data-ttu-id="b84c2-135">Végezetül Hozzáadás hello alábbi toocode toostart hello belső vezérlőprogram frissítési sorrend működik, és rendszeres időközönként megjelenjen hello jelentett tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="b84c2-135">Finally, Add hello following function toocode toostart hello firmware update sequence and start periodically showing hello reported properties:</span></span>
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. <span data-ttu-id="b84c2-136">Mentse és zárja be a hello **dmpatterns_fwupdate_service.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="b84c2-136">Save and close hello **dmpatterns_fwupdate_service.js** file.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a><span data-ttu-id="b84c2-137">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="b84c2-137">Run hello apps</span></span>
<span data-ttu-id="b84c2-138">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="b84c2-138">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="b84c2-139">Parancssorba hello hello **manageddevice** mappa, futtassa a következő parancs toobegin figyeli a hello újraindítás közvetlen módszer hello.</span><span class="sxs-lookup"><span data-stu-id="b84c2-139">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="b84c2-140">Parancssorba hello hello **triggerfwupdateondevice** mappa, futtassa a következő parancs tootrigger hello távoli hello újraindul, és lekérdezi a hello eszköz iker toofind hello utolsó indítsa újra a számítógépet idő.</span><span class="sxs-lookup"><span data-stu-id="b84c2-140">At hello command prompt in hello **triggerfwupdateondevice** folder, run hello following command tootrigger hello remote reboot and query for hello device twin toofind hello last reboot time.</span></span>
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. <span data-ttu-id="b84c2-141">Hello eszköz válasz toohello közvetlen módszer hello konzolon láthatja.</span><span class="sxs-lookup"><span data-stu-id="b84c2-141">You see hello device response toohello direct method in hello console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b84c2-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b84c2-142">Next steps</span></span>
<span data-ttu-id="b84c2-143">Ebben az oktatóanyagban egy közvetlen módszer tootrigger távoli használt belső vezérlőprogram frissítése egy eszközön, és a használt hello jelentett tulajdonságok toofollow hello hello belső vezérlőprogram frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="b84c2-143">In this tutorial, you used a direct method tootrigger a remote firmware update on a device and used hello reported properties toofollow hello progress of hello firmware update.</span></span>

<span data-ttu-id="b84c2-144">toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="b84c2-144">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
