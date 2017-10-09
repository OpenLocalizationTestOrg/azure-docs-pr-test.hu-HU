---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 3: sablon-üzembehelyezés |} Microsoft Docs"
description: "hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "adattárolás hello felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6c58de85-c5c4-4989-bb5e-08c45c549966
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b6c0a9530cb80e3f78c0e96037f6f3942b602aea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="e6e5a-104">Az Azure-függvény app és az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e6e5a-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="e6e5a-105">[Az Azure Functions](../azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-105">[Azure Functions](../azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="e6e5a-106">Egy Azure függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-106">An Azure function app hosts hello execution of your functions in Azure.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e6e5a-107">Mit fog</span><span class="sxs-lookup"><span data-stu-id="e6e5a-107">What you will do</span></span>
<span data-ttu-id="e6e5a-108">Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="e6e5a-109">hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span> <span data-ttu-id="e6e5a-110">Ha bármilyen problémába ütközik, a keresési hello megoldások [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e6e5a-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e6e5a-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="e6e5a-111">What you will learn</span></span>
<span data-ttu-id="e6e5a-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="e6e5a-112">In this article, you will learn:</span></span>

* <span data-ttu-id="e6e5a-113">Hogyan toouse [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toodeploy Azure erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-113">How toouse [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toodeploy Azure resources.</span></span>
* <span data-ttu-id="e6e5a-114">Hogyan toouse az Azure app tooprocess IoT hub üzenetek működéséhez, és beírhatók tooa tábla Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-114">How toouse an Azure function app tooprocess IoT hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e6e5a-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="e6e5a-115">What you need</span></span>
<span data-ttu-id="e6e5a-116">Sikeresen végrehajtotta:</span><span class="sxs-lookup"><span data-stu-id="e6e5a-116">You must have successfully completed:</span></span>
* [<span data-ttu-id="e6e5a-117">Ismerkedés a málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="e6e5a-117">Get started with Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-get-started.md)
* [<span data-ttu-id="e6e5a-118">Az Azure IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="e6e5a-118">Create your Azure IoT hub</span></span>](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="open-hello-sample-app"></a><span data-ttu-id="e6e5a-119">Nyissa meg hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="e6e5a-119">Open hello sample app</span></span>
<span data-ttu-id="e6e5a-120">Nyissa meg a Visual Studio Code hello mintaprojektet hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e6e5a-120">Open hello sample project in Visual Studio Code by running hello following commands:</span></span>

```bash
cd Lesson3
code .
```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* <span data-ttu-id="e6e5a-122">Hello `app.js` hello fájlban `app` almappa is hello kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-122">hello `app.js` file in hello `app` subfolder is hello key source file.</span></span> <span data-ttu-id="e6e5a-123">A forrásfájl hello kód toosend küldené üzenet 20 alkalommal tooyour IoT hub és villogási hello LED minden üzenet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-123">This source file contains hello code toosend a message 20 times tooyour IoT hub and blink hello LED for each message it sends.</span></span>
* <span data-ttu-id="e6e5a-124">Hello `arm-template.json` fájl hello Azure Resource Manager-sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-124">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="e6e5a-125">Hello `arm-template-param.json` hello konfigurációs fájl hello Azure Resource Manager-sablon által használt.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-125">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
* <span data-ttu-id="e6e5a-126">Hello `ReceiveDeviceMessages` hello Azure függvény hello Node.js kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-126">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="e6e5a-127">Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e6e5a-127">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="e6e5a-128">Frissítés hello `arm-template-param.json` fájlt a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-128">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![Az Azure Resource Manager sablon paraméterek](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* <span data-ttu-id="e6e5a-130">Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a regisztrált málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="e6e5a-130">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).</span></span>
* <span data-ttu-id="e6e5a-131">Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-131">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="e6e5a-132">hello előtag biztosítja az erőforrásnév hello globálisan egyedi tooavoid ütközés.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-132">hello prefix ensures that hello resource name is globally unique tooavoid conflict.</span></span> <span data-ttu-id="e6e5a-133">Ne használjon kötőjel vagy kezdeti hello előtag a számot.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-133">Do not use a dash or number initial in hello prefix.</span></span>

<span data-ttu-id="e6e5a-134">Miután frissítette hello `arm-template-param.json` fájlt, hello erőforrások tooAzure telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e6e5a-134">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="e6e5a-135">Tart körülbelül öt perc toocreate ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-135">It takes about five minutes toocreate these resources.</span></span> <span data-ttu-id="e6e5a-136">Hello erőforrás létrehozása folyamatban van, míg a következő cikk toohello áthelyezheti.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-136">While hello resource creation is in progress, you can move on toohello next article.</span></span>

## <a name="summary"></a><span data-ttu-id="e6e5a-137">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e6e5a-137">Summary</span></span>
<span data-ttu-id="e6e5a-138">Létrehozta az Azure-függvény app tooprocess IoT hub-üzeneteket és az Azure-tárolóra toostore ezeket az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-138">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="e6e5a-139">Most már telepítheti és Pi hello minta toosend eszközről a felhőbe üzenetek futtatnak.</span><span class="sxs-lookup"><span data-stu-id="e6e5a-139">You can now deploy and run hello sample toosend device-to-cloud messages on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6e5a-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6e5a-140">Next steps</span></span>
[<span data-ttu-id="e6e5a-141">Futtassa egy minta alkalmazás toosend eszköz a felhőbe küldött üzeneteket málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="e6e5a-141">Run a sample application toosend device-to-cloud messages on Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

