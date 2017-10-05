---
title: "Az Azure IoT - lecke 3 Connect Raspberry pi (C): sablon-üzembehelyezés |} Microsoft Docs"
description: "Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok tárolása a felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
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
ms.openlocfilehash: b8bab31489f2e912b51212cb58cb598a9db9b59d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="38a4d-104">Az Azure-függvény app és az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="38a4d-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="38a4d-105">[Az Azure Functions](../../articles/azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) a felhőben.</span><span class="sxs-lookup"><span data-stu-id="38a4d-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in the cloud.</span></span> <span data-ttu-id="38a4d-106">Egy Azure függvényalkalmazás biztosítja a a függvények végrehajtásához szükséges az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="38a4d-106">An Azure function app hosts the execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="38a4d-107">Rendszer mit</span><span class="sxs-lookup"><span data-stu-id="38a4d-107">What will you do</span></span>
<span data-ttu-id="38a4d-108">Az Azure Resource Manager-sablon használatával hozzon létre egy Azure függvény alkalmazást és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="38a4d-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="38a4d-109">Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="38a4d-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span> <span data-ttu-id="38a4d-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="38a4d-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="38a4d-111">Mi ismerkedhet meg</span><span class="sxs-lookup"><span data-stu-id="38a4d-111">What will you learn</span></span>
<span data-ttu-id="38a4d-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="38a4d-112">In this article, you will learn:</span></span>
* <span data-ttu-id="38a4d-113">Használata [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) központi telepítése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="38a4d-113">How to use [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) to deploy Azure resources.</span></span>
* <span data-ttu-id="38a4d-114">Hogyan használható az Azure-függvény alkalmazás IoT hub üzenetek feldolgozásához, és az Azure Table storage egy táblához.</span><span class="sxs-lookup"><span data-stu-id="38a4d-114">How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="38a4d-115">Mire van szüksége</span><span class="sxs-lookup"><span data-stu-id="38a4d-115">What do you need</span></span>
* <span data-ttu-id="38a4d-116">Sikeresen végrehajtotta:</span><span class="sxs-lookup"><span data-stu-id="38a4d-116">You must have successfully completed:</span></span>
- [<span data-ttu-id="38a4d-117">Ismerkedjen meg a málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="38a4d-117">Get started with your Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-get-started.md)
- [<span data-ttu-id="38a4d-118">Az Azure IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="38a4d-118">Create your Azure IoT hub</span></span>](iot-hub-raspberry-pi-kit-c-get-started.md)

## <a name="open-the-sample-app"></a><span data-ttu-id="38a4d-119">Nyissa meg a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="38a4d-119">Open the sample app</span></span>
<span data-ttu-id="38a4d-120">Nyissa meg a minta-projektet a Visual Studio Code a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="38a4d-120">Open the sample project in Visual Studio Code by running the following commands:</span></span>

```bash
cd Lesson3
code .
```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure_c.png)

* <span data-ttu-id="38a4d-122">A `main.c` fájlt a `app` almappában a kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="38a4d-122">The `main.c` file in the `app` subfolder is the key source file.</span></span> <span data-ttu-id="38a4d-123">A forrásfájl üzenet küldése 20 alkalommal az IoT hub, és minden üzenetet küld a LED villogni kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="38a4d-123">This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.</span></span>
* <span data-ttu-id="38a4d-124">A `arm-template.json` fájl az Azure Resource Manager sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="38a4d-124">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="38a4d-125">A `arm-template-param.json` a konfigurációs fájl az Azure Resource Manager-sablon által használt.</span><span class="sxs-lookup"><span data-stu-id="38a4d-125">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
* <span data-ttu-id="38a4d-126">A `ReceiveDeviceMessages` tartalmazza, a Node.js-kódot az Azure-függvény.</span><span class="sxs-lookup"><span data-stu-id="38a4d-126">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="38a4d-127">Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="38a4d-127">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="38a4d-128">Frissítés a `arm-template-param.json` fájlt a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="38a4d-128">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![Az Azure Resource Manager sablon paraméterek](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para_c.png)

* <span data-ttu-id="38a4d-130">Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a regisztrált málna Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="38a4d-130">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).</span></span>
* <span data-ttu-id="38a4d-131">Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal.</span><span class="sxs-lookup"><span data-stu-id="38a4d-131">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="38a4d-132">Az előtag biztosítja, hogy az erőforrás nevének globálisan egyedi ütközés elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="38a4d-132">The prefix ensures that the resource name is globally unique to avoid conflict.</span></span> <span data-ttu-id="38a4d-133">Ne használjon kötőjel vagy kezdeti az előtag a számot.</span><span class="sxs-lookup"><span data-stu-id="38a4d-133">Do not use a dash or number initial in the prefix.</span></span>

<span data-ttu-id="38a4d-134">Miután frissítette a `arm-template-param.json` fájlt, telepítheti az erőforrásokat az Azure, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="38a4d-134">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="38a4d-135">Ezek az erőforrások létrehozásához körülbelül öt percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="38a4d-135">It takes about five minutes to create these resources.</span></span> <span data-ttu-id="38a4d-136">Amíg az erőforrás létrehozása folyamatban van, továbbléphet a következő cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="38a4d-136">While the resource creation is in progress, you can move on to the next article.</span></span>

## <a name="summary"></a><span data-ttu-id="38a4d-137">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="38a4d-137">Summary</span></span>
<span data-ttu-id="38a4d-138">Az Azure-függvény alkalmazás feldolgozni az IoT hub üzenetek és az Azure-tárfiók ezek az üzenetek tárolásához létrehozott.</span><span class="sxs-lookup"><span data-stu-id="38a4d-138">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="38a4d-139">Most már telepítheti és futtathatja a Pi eszköz a felhőbe küldött üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="38a4d-139">You can now deploy and run the sample to send device-to-cloud messages on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38a4d-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="38a4d-140">Next steps</span></span>
[<span data-ttu-id="38a4d-141">Futtassa a mintaalkalmazást eszközről a felhőbe üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="38a4d-141">Run a sample application to send device-to-cloud messages</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md)

