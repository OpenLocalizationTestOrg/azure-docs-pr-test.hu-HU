---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 3: sablon-üzembehelyezés |} Microsoft Docs"
description: "hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adattárolás hello felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 11aaad4935681d8b3d338779eec1b19d77cb11e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="8eac2-104">Az Azure-függvény app és az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8eac2-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="8eac2-105">[Az Azure Functions](../../articles/azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="8eac2-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="8eac2-106">Egy Azure függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8eac2-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="8eac2-107">Rendszer mit</span><span class="sxs-lookup"><span data-stu-id="8eac2-107">What will you do</span></span>
<span data-ttu-id="8eac2-108">Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="8eac2-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="8eac2-109">hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat.</span><span class="sxs-lookup"><span data-stu-id="8eac2-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span> <span data-ttu-id="8eac2-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="8eac2-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="8eac2-111">Mi ismerkedhet meg</span><span class="sxs-lookup"><span data-stu-id="8eac2-111">What will you learn</span></span>
<span data-ttu-id="8eac2-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="8eac2-112">In this article, you will learn:</span></span>
* <span data-ttu-id="8eac2-113">Hogyan toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="8eac2-113">How toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="8eac2-114">Hogyan toouse az Azure app tooprocess IoT hub üzenetek működéséhez, és beírhatók tooa tábla Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="8eac2-114">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="8eac2-115">Mire van szüksége</span><span class="sxs-lookup"><span data-stu-id="8eac2-115">What do you need</span></span>
* <span data-ttu-id="8eac2-116">Sikeresen végrehajtotta:</span><span class="sxs-lookup"><span data-stu-id="8eac2-116">You must have successfully completed:</span></span>
- [<span data-ttu-id="8eac2-117">Ismerkedjen meg a málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="8eac2-117">Get started with your Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-get-started.md)
- [<span data-ttu-id="8eac2-118">Az Azure IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="8eac2-118">Create your Azure IoT hub</span></span>](iot-hub-raspberry-pi-kit-c-get-started.md)

## <a name="open-hello-sample-app"></a><span data-ttu-id="8eac2-119">Nyissa meg hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="8eac2-119">Open hello sample app</span></span>
<span data-ttu-id="8eac2-120">Nyissa meg a Visual Studio Code hello mintaprojektet hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="8eac2-120">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure_c.png)

* <span data-ttu-id="8eac2-122">Hello `main.c` hello fájlban `app` almappa is hello kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="8eac2-122">hello `main.c` file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="8eac2-123">A forrásfájl hello kód toosend küldené üzenet 20 alkalommal tooyour IoT hub és villogási hello LED minden üzenet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8eac2-123">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="8eac2-124">Hello `arm-template.json` fájl hello Azure Resource Manager-sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="8eac2-124">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="8eac2-125">Hello `arm-template-param.json` hello konfigurációs fájl hello Azure Resource Manager-sablon által használt.</span><span class="sxs-lookup"><span data-stu-id="8eac2-125">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="8eac2-126">Hello `ReceiveDeviceMessages` hello Azure függvény hello Node.js kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8eac2-126">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="8eac2-127">Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="8eac2-127">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="8eac2-128">Frissítés hello `arm-template-param.json` fájlt a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8eac2-128">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Az Azure Resource Manager sablon paraméterek](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para_c.png)

* <span data-ttu-id="8eac2-130">Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a regisztrált málna Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="8eac2-130">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span></span>
* <span data-ttu-id="8eac2-131">Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal.</span><span class="sxs-lookup"><span data-stu-id="8eac2-131">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="8eac2-132">hello előtag biztosítja az erőforrásnév hello globálisan egyedi tooavoid ütközés.</span><span class="sxs-lookup"><span data-stu-id="8eac2-132">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="8eac2-133">Ne használjon kötőjel vagy kezdeti hello előtag a számot.</span><span class="sxs-lookup"><span data-stu-id="8eac2-133">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="8eac2-134">Miután frissítette hello `arm-template-param.json` fájlt, hello erőforrások tooAzure telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="8eac2-134">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="8eac2-135">Tart körülbelül öt perc toocreate ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="8eac2-135">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="8eac2-136">Hello erőforrás létrehozása folyamatban van, míg a következő cikk toohello áthelyezheti.</span><span class="sxs-lookup"><span data-stu-id="8eac2-136">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="8eac2-137">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="8eac2-137">Summary</span></span>
<span data-ttu-id="8eac2-138">Létrehozta az Azure-függvény app tooprocess IoT hub-üzeneteket és az Azure-tárolóra toostore ezeket az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="8eac2-138">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="8eac2-139">Most már telepítheti és Pi hello minta toosend eszközről a felhőbe üzenetek futtatnak.</span><span class="sxs-lookup"><span data-stu-id="8eac2-139">You can now deploy and run hello sample toosend device-to-cloud messages on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8eac2-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8eac2-140">Next steps</span></span>
[<span data-ttu-id="8eac2-141">Futtassa egy minta alkalmazás toosend eszközről a felhőbe üzenetek</span><span class="sxs-lookup"><span data-stu-id="8eac2-141">Run a sample application toosend device-to-cloud messages</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md)

