---
title: "Connect Arduino (C) tooAzure IoT - lecke 3: sablon-üzembehelyezés |} Microsoft Docs"
description: "hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adattárolás hello felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9c8f4cd1-9511-4601-ad7e-51761a986753
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 6a84a6d3c5263a85c8997cf69fe446d73ab7a5fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="d0146-104">Az Azure-függvény app és az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0146-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="d0146-105">[Az Azure Functions](../../articles/azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="d0146-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="d0146-106">Egy Azure függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d0146-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="d0146-107">Rendszer mit</span><span class="sxs-lookup"><span data-stu-id="d0146-107">What will you do</span></span>
<span data-ttu-id="d0146-108">Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d0146-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="d0146-109">hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat.</span><span class="sxs-lookup"><span data-stu-id="d0146-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span>

<span data-ttu-id="d0146-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [lap a Adafruit lágyított M0 Wi-Fi Arduino kártya hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="d0146-110">If you have any problems, look for solutions on hello [troubleshooting page for your Adafruit Feather M0 WiFi Arduino board](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="d0146-111">Mi ismerkedhet meg</span><span class="sxs-lookup"><span data-stu-id="d0146-111">What will you learn</span></span>
<span data-ttu-id="d0146-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="d0146-112">In this article, you will learn:</span></span>
* <span data-ttu-id="d0146-113">Hogyan toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d0146-113">How toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="d0146-114">Hogyan toouse az Azure app tooprocess IoT hub üzenetek működéséhez, és beírhatók tooa tábla Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="d0146-114">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="d0146-115">Mire van szüksége</span><span class="sxs-lookup"><span data-stu-id="d0146-115">What do you need</span></span>
<span data-ttu-id="d0146-116">Sikeresen végrehajtotta:</span><span class="sxs-lookup"><span data-stu-id="d0146-116">You must have successfully completed:</span></span>
- <span data-ttu-id="d0146-117">[Ismerkedjen meg a Arduino tábla][get-started]</span><span class="sxs-lookup"><span data-stu-id="d0146-117">[Get started with your Arduino board][get-started]</span></span>
- <span data-ttu-id="d0146-118">[Az Azure IoT hub létrehozása][create-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="d0146-118">[Create your Azure IoT hub][create-iot-hub]</span></span>

## <a name="open-hello-sample-app"></a><span data-ttu-id="d0146-119">Nyissa meg hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="d0146-119">Open hello sample app</span></span>
<span data-ttu-id="d0146-120">Nyissa meg a Visual Studio Code hello mintaprojektet hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d0146-120">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Tárház szerkezete][repo-structure]

* <span data-ttu-id="d0146-122">Hello `app.ino` hello fájlban `app` almappa is hello kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="d0146-122">hello `app.ino` file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="d0146-123">A forrásfájl hello kód toosend küldené üzenet 20 alkalommal tooyour IoT hub és villogási hello LED minden üzenet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d0146-123">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="d0146-124">Hello `config.json` szükséges konfigurációs beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d0146-124">hello `config.json` contains required configuration settings.</span></span>
* <span data-ttu-id="d0146-125">Hello `arm-template.json` fájl hello Azure Resource Manager-sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="d0146-125">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="d0146-126">Hello `arm-template-param.json` hello konfigurációs fájl hello Azure Resource Manager-sablon által használt.</span><span class="sxs-lookup"><span data-stu-id="d0146-126">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="d0146-127">Hello `ReceiveDeviceMessages` hello Azure függvény hello Node.js kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d0146-127">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="d0146-128">Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="d0146-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="d0146-129">Frissítés hello `arm-template-param.json` fájlt a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d0146-129">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Az Azure Resource Manager sablon paraméterek][arm-template-params]

* <span data-ttu-id="d0146-131">Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a Arduino board regisztrált] [ created-iot-hub-and-registered-arduino-board].</span><span class="sxs-lookup"><span data-stu-id="d0146-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered your Arduino board][created-iot-hub-and-registered-arduino-board].</span></span>
* <span data-ttu-id="d0146-132">Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal.</span><span class="sxs-lookup"><span data-stu-id="d0146-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="d0146-133">hello előtag biztosítja az erőforrásnév hello globálisan egyedi tooavoid ütközés.</span><span class="sxs-lookup"><span data-stu-id="d0146-133">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="d0146-134">Ne használjon kötőjel vagy kezdeti hello előtag a számot.</span><span class="sxs-lookup"><span data-stu-id="d0146-134">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="d0146-135">Miután frissítette hello `arm-template-param.json` fájlt, hello erőforrások tooAzure telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d0146-135">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="d0146-136">Tart körülbelül öt perc toocreate ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d0146-136">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="d0146-137">Hello erőforrás létrehozása folyamatban van, míg a következő cikk toohello áthelyezheti.</span><span class="sxs-lookup"><span data-stu-id="d0146-137">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="d0146-138">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d0146-138">Summary</span></span>
<span data-ttu-id="d0146-139">Létrehozta az Azure-függvény app tooprocess IoT hub-üzeneteket és az Azure-tárolóra toostore ezeket az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="d0146-139">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="d0146-140">Most már telepítheti és üdvözlő minta toosend eszközről a felhőbe üzenetek futtassa a Arduino táblán.</span><span class="sxs-lookup"><span data-stu-id="d0146-140">You can now deploy and run hello sample toosend device-to-cloud messages on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0146-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d0146-141">Next steps</span></span>
<span data-ttu-id="d0146-142">[A Arduino táblán az eszköz a felhőbe küldött üzeneteket egy minta alkalmazás toosend futtatása][send-device-to-cloud-messages]</span><span class="sxs-lookup"><span data-stu-id="d0146-142">[Run a sample application toosend device-to-cloud messages on your Arduino board][send-device-to-cloud-messages]</span></span>

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md