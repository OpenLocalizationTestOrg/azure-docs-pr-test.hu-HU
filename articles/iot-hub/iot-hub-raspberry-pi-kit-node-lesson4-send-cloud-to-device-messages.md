---
featureFlags: usabilla
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 4: Felhő eszközre |} Microsoft Docs"
description: "hello mintaalkalmazás Pi futtatja, és figyeli a bejövő üzenetek az IoT hub. Egy új gulp feladatot az IoT hub tooblink hello LED üzenetek tooPi küldi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "felhő toodevice, üzenet a felhőből"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6ae6539e-1163-4490-8d72-fdf7803e3054
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d69ded4e30c27378481ab2a4fb9c5b73be7bd44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-sample-application-tooreceive-cloud-to-device-messages"></a><span data-ttu-id="a1c9a-105">Futtassa a minta alkalmazás tooreceive felhő eszközre köszönőüzenetei</span><span class="sxs-lookup"><span data-stu-id="a1c9a-105">Run hello sample application tooreceive cloud-to-device messages</span></span>
<span data-ttu-id="a1c9a-106">Ebben a cikkben a málna Pi 3 minta alkalmazást telepít központilag.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-106">In this article, you deploy a sample application on Raspberry Pi 3.</span></span> <span data-ttu-id="a1c9a-107">hello mintaalkalmazást az IoT hub bejövő üzeneteit figyeli.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-107">hello sample application monitors incoming messages from your IoT hub.</span></span> <span data-ttu-id="a1c9a-108">Akkor is futtassa gulp feladat a számítógép toosend üzenetek tooPi az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-108">You also run a gulp task on your computer toosend messages tooPi from your IoT hub.</span></span> <span data-ttu-id="a1c9a-109">Hello mintaalkalmazás hello üzeneteket fogad, ha azt villogjon-hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-109">When hello sample application receives hello messages, it blinks hello LED.</span></span> <span data-ttu-id="a1c9a-110">Ha bármilyen problémába ütközik, a keresési hello megoldások [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a1c9a-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a1c9a-111">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a1c9a-111">What you will do</span></span>
* <span data-ttu-id="a1c9a-112">Csatlakozás hello minta alkalmazás tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-112">Connect hello sample application tooyour IoT hub.</span></span>
* <span data-ttu-id="a1c9a-113">Regisztrálhat és futtathat hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-113">Deploy and run hello sample application.</span></span>
* <span data-ttu-id="a1c9a-114">Az IoT hub tooPi tooblink hello LED küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-114">Send messages from your IoT hub tooPi tooblink hello LED.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a1c9a-115">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a1c9a-115">What you will learn</span></span>
<span data-ttu-id="a1c9a-116">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a1c9a-116">In this article, you will learn:</span></span>
* <span data-ttu-id="a1c9a-117">Hogyan a toomonitor bejövő az IoT hub érkező üzenetek</span><span class="sxs-lookup"><span data-stu-id="a1c9a-117">How toomonitor incoming messages from your IoT hub</span></span>
* <span data-ttu-id="a1c9a-118">Hogyan toosend felhő eszközt az IoT hub tooPi érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-118">How toosend cloud-to-device messages from your IoT hub tooPi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a1c9a-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a1c9a-119">What you need</span></span>
* <span data-ttu-id="a1c9a-120">Raspberry Pi 3, állítsa be a használatra.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-120">Raspberry Pi 3, set up for use.</span></span> <span data-ttu-id="a1c9a-121">Hogyan tooset fel a Pi: toolearn [állítsa be az eszközt](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).</span><span class="sxs-lookup"><span data-stu-id="a1c9a-121">toolearn how tooset up Pi, see [Configure your device](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).</span></span>
* <span data-ttu-id="a1c9a-122">Az IoT-központ, amely az Azure-előfizetése jön létre.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-122">An IoT hub that is created in your Azure subscription.</span></span> <span data-ttu-id="a1c9a-123">toolearn hogyan toocreate az IoT hub, lásd: [az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="a1c9a-123">toolearn how toocreate your IoT hub, see [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span></span>

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a><span data-ttu-id="a1c9a-124">Csatlakozás hello minta alkalmazás tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="a1c9a-124">Connect hello sample application tooyour IoT hub</span></span>
1. <span data-ttu-id="a1c9a-125">Győződjön meg arról, hogy Ön hello tárház mappában `iot-hub-node-raspberrypi-getting-started`.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-125">Make sure that you're in hello repo folder `iot-hub-node-raspberrypi-getting-started`.</span></span> <span data-ttu-id="a1c9a-126">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1c9a-126">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>
   
   ```bash
   cd Lesson4
   code .
   ```
   
   <span data-ttu-id="a1c9a-127">Értesítés hello `app.js` hello fájlban `app` almappájában.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-127">Notice hello `app.js` file in hello `app` subfolder.</span></span> <span data-ttu-id="a1c9a-128">Hello `app.js` hello kód toomonitor bejövő üzenetek hello IoT hubról tartalmazó hello forrás-fájl.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-128">hello `app.js` file is hello key source file that contains hello code toomonitor incoming messages from hello IoT hub.</span></span> <span data-ttu-id="a1c9a-129">Hello `blinkLED` függvény villogjon hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-129">hello `blinkLED` function blinks hello LED.</span></span>
   
   ![Hello mintaalkalmazást a tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)
2. <span data-ttu-id="a1c9a-131">Hello konfigurációs fájl inicializálása hello a következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="a1c9a-131">Initialize hello configuration file by using hello following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```
   
   <span data-ttu-id="a1c9a-132">Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) ezen a számítógépen az összes hello konfiguráció örökölt, kihagyhatja, központi telepítéséhez és futtatásához hello mintaalkalmazás toohello feladatánál.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-132">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) on this computer, all hello configurations are inherited, so you can skip toohello task of deploying and running hello sample application.</span></span> <span data-ttu-id="a1c9a-133">Ha elvégezte a hello lépéseket [Azure függvény alkalmazás és a storage-fiók létrehozása](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) egy másik számítógépen kell tooreplace hello helyőrzőt hello `config-raspberrypi.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-133">If you completed hello steps in [Create an Azure function app and storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) on a different computer, you need tooreplace hello placeholders in hello `config-raspberrypi.json` file.</span></span> <span data-ttu-id="a1c9a-134">Hello `config-raspberrypi.json` fájl van-e a kezdőmappa hello almappája.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-134">hello `config-raspberrypi.json` file is in hello subfolder of your home folder.</span></span>
   
   ![Hello config-raspberrypi.json fájl tartalma](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* <span data-ttu-id="a1c9a-136">Cserélje le **[eszköz állomásnév vagy IP-címe]** Pi vagy hello hello futtatásával kapott állomásnév hello IP-címmel `devdisco list --eth` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-136">Replace **[device hostname or IP address]** with hello IP address of Pi or hello host name that you get by running hello `devdisco list --eth` command.</span></span>
* <span data-ttu-id="a1c9a-137">Cserélje le **[IoT eszköz kapcsolati karakterlánc]** hello eszköz kapcsolati karakterlánccal hello futtatásával kapott `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-137">Replace **[IoT device connection string]** with hello device connection string that you get by running hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` command.</span></span>
* <span data-ttu-id="a1c9a-138">Cserélje le **[IoT hub kapcsolati karakterlánc]** az IoT hub kapcsolati karakterláncot kapott hello futtatásával hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-138">Replace **[IoT hub connection string]** with hello IoT hub connection string that you get by running hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` command.</span></span>

> [!NOTE]
> <span data-ttu-id="a1c9a-139">Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-139">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="a1c9a-140">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="a1c9a-140">Deploy and run hello sample application</span></span>
<span data-ttu-id="a1c9a-141">Központi telepítése, és futtassa a mintaalkalmazást hello Pi hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1c9a-141">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

<span data-ttu-id="a1c9a-142">hello parancs hello minta alkalmazás tooPi telepíti.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-142">hello command deploys hello sample application tooPi.</span></span> <span data-ttu-id="a1c9a-143">Ezt követően azt futtatja hello alkalmazás Pi és a gazdagép egy külön feladat számítógép toosend 20 villogási üzenetek tooPi az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-143">Then, it runs hello application on Pi and a separate task on your host computer toosend 20 blink messages tooPi from your IoT hub.</span></span>

<span data-ttu-id="a1c9a-144">Hello mintaalkalmazás futtatása után kezdődik toomessages az IoT-központ figyel.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-144">After hello sample application runs, it starts listening toomessages from your IoT hub.</span></span> <span data-ttu-id="a1c9a-145">Eközben hello gulp feladat több "villogni" üzenetet küldi az IoT hub tooPi.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-145">Meanwhile, hello gulp task sends several "blink" messages from your IoT hub tooPi.</span></span> <span data-ttu-id="a1c9a-146">Minden egyes villogási üzenet, amely megkapja a Pi, hello mintaalkalmazás meghívja a hello `blinkLED` függvény tooblink hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-146">For each blink message that Pi receives, hello sample application calls hello `blinkLED` function tooblink hello LED.</span></span>

<span data-ttu-id="a1c9a-147">Feladat küldése 20 üzenetet az IoT hub tooPi a gulp LED hello villogási hello mint két másodpercenként kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-147">You should see hello LED blink every two seconds as hello gulp task sends 20 messages from your IoT hub tooPi.</span></span> <span data-ttu-id="a1c9a-148">hello utolsó egyik "stop" üzenet, amely közli hello alkalmazás toostop futtatása.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-148">hello last one is a "stop" message that tells hello application toostop running.</span></span>

![A mintaalkalmazás parancs gulp és üzenetek villogni](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="summary"></a><span data-ttu-id="a1c9a-150">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a1c9a-150">Summary</span></span>
<span data-ttu-id="a1c9a-151">Az IoT hub tooPi tooblink hello LED a sikeresen elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-151">You’ve successfully sent messages from your IoT hub tooPi tooblink hello LED.</span></span> <span data-ttu-id="a1c9a-152">hello következő feladat nem kötelező: hello be- és kikapcsolását hello LED viselkedésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="a1c9a-152">hello next task is optional: change hello on and off behavior of hello LED.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1c9a-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1c9a-153">Next steps</span></span>
[<span data-ttu-id="a1c9a-154">Hello be- és kikapcsolását hello LED viselkedésének módosítása</span><span class="sxs-lookup"><span data-stu-id="a1c9a-154">Change hello on and off behavior of hello LED</span></span>](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)

