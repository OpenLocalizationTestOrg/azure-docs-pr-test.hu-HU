---
title: "Az Azure IoT - lecke 3 Connect Arduino (C): sablon-üzembehelyezés |} Microsoft Docs"
description: "Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok tárolása a felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
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
ms.openlocfilehash: be6105927645ae2ec56f6885c61dbcb6faf5b11f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="ac93d-104">Az Azure-függvény app és az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac93d-104">Create an Azure function app and Azure storage account</span></span>
<span data-ttu-id="ac93d-105">[Az Azure Functions](../../articles/azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) a felhőben.</span><span class="sxs-lookup"><span data-stu-id="ac93d-105">[Azure Functions](../../articles/azure-functions/functions-overview.md) is a solution for easily running *functions* (small pieces of code) in the cloud.</span></span> <span data-ttu-id="ac93d-106">Egy Azure függvényalkalmazás biztosítja a a függvények végrehajtásához szükséges az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ac93d-106">An Azure function app hosts the execution of your functions in Azure.</span></span>

## <a name="what-will-you-do"></a><span data-ttu-id="ac93d-107">Rendszer mit</span><span class="sxs-lookup"><span data-stu-id="ac93d-107">What will you do</span></span>
<span data-ttu-id="ac93d-108">Az Azure Resource Manager-sablon használatával hozzon létre egy Azure függvény alkalmazást és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="ac93d-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="ac93d-109">Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="ac93d-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span>

<span data-ttu-id="ac93d-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [lap a Adafruit lágyított M0 Wi-Fi Arduino kártya hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="ac93d-110">If you have any problems, look for solutions on the [troubleshooting page for your Adafruit Feather M0 WiFi Arduino board](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-will-you-learn"></a><span data-ttu-id="ac93d-111">Mi ismerkedhet meg</span><span class="sxs-lookup"><span data-stu-id="ac93d-111">What will you learn</span></span>
<span data-ttu-id="ac93d-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="ac93d-112">In this article, you will learn:</span></span>
* <span data-ttu-id="ac93d-113">Használata [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) központi telepítése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ac93d-113">How to use [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) to deploy Azure resources.</span></span>
* <span data-ttu-id="ac93d-114">Hogyan használható az Azure-függvény alkalmazás IoT hub üzenetek feldolgozásához, és az Azure Table storage egy táblához.</span><span class="sxs-lookup"><span data-stu-id="ac93d-114">How to use an Azure function app to process IoT hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="ac93d-115">Mire van szüksége</span><span class="sxs-lookup"><span data-stu-id="ac93d-115">What do you need</span></span>
<span data-ttu-id="ac93d-116">Sikeresen végrehajtotta:</span><span class="sxs-lookup"><span data-stu-id="ac93d-116">You must have successfully completed:</span></span>
- <span data-ttu-id="ac93d-117">[Ismerkedjen meg a Arduino tábla][get-started]</span><span class="sxs-lookup"><span data-stu-id="ac93d-117">[Get started with your Arduino board][get-started]</span></span>
- <span data-ttu-id="ac93d-118">[Az Azure IoT hub létrehozása][create-iot-hub]</span><span class="sxs-lookup"><span data-stu-id="ac93d-118">[Create your Azure IoT hub][create-iot-hub]</span></span>

## <a name="open-the-sample-app"></a><span data-ttu-id="ac93d-119">Nyissa meg a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="ac93d-119">Open the sample app</span></span>
<span data-ttu-id="ac93d-120">Nyissa meg a minta-projektet a Visual Studio Code a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ac93d-120">Open the sample project in Visual Studio Code by running the following commands:</span></span>

```bash
cd Lesson3
code .
```

![Tárház szerkezete][repo-structure]

* <span data-ttu-id="ac93d-122">A `app.ino` fájlt a `app` almappában a kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="ac93d-122">The `app.ino` file in the `app` subfolder is the key source file.</span></span> <span data-ttu-id="ac93d-123">A forrásfájl üzenet küldése 20 alkalommal az IoT hub, és minden üzenetet küld a LED villogni kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ac93d-123">This source file contains the code to send a message 20 times to your IoT hub and blink the LED for each message it sends.</span></span>
* <span data-ttu-id="ac93d-124">A `config.json` szükséges konfigurációs beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ac93d-124">The `config.json` contains required configuration settings.</span></span>
* <span data-ttu-id="ac93d-125">A `arm-template.json` fájl az Azure Resource Manager sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="ac93d-125">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
* <span data-ttu-id="ac93d-126">A `arm-template-param.json` a konfigurációs fájl az Azure Resource Manager-sablon által használt.</span><span class="sxs-lookup"><span data-stu-id="ac93d-126">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
* <span data-ttu-id="ac93d-127">A `ReceiveDeviceMessages` tartalmazza, a Node.js-kódot az Azure-függvény.</span><span class="sxs-lookup"><span data-stu-id="ac93d-127">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="ac93d-128">Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ac93d-128">Configure Azure Resource Manager templates and create resources in Azure</span></span>
<span data-ttu-id="ac93d-129">Frissítés a `arm-template-param.json` fájlt a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ac93d-129">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![Az Azure Resource Manager sablon paraméterek][arm-template-params]

* <span data-ttu-id="ac93d-131">Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a Arduino board regisztrált] [ created-iot-hub-and-registered-arduino-board].</span><span class="sxs-lookup"><span data-stu-id="ac93d-131">Replace **[your IoT Hub name]** with **{my hub name}** that you specified when you [created your IoT hub and registered your Arduino board][created-iot-hub-and-registered-arduino-board].</span></span>
* <span data-ttu-id="ac93d-132">Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal.</span><span class="sxs-lookup"><span data-stu-id="ac93d-132">Replace **[prefix string for new resources]** with any prefix you want.</span></span> <span data-ttu-id="ac93d-133">Az előtag biztosítja, hogy az erőforrás nevének globálisan egyedi ütközés elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="ac93d-133">The prefix ensures that the resource name is globally unique to avoid conflict.</span></span> <span data-ttu-id="ac93d-134">Ne használjon kötőjel vagy kezdeti az előtag a számot.</span><span class="sxs-lookup"><span data-stu-id="ac93d-134">Do not use a dash or number initial in the prefix.</span></span>

<span data-ttu-id="ac93d-135">Miután frissítette a `arm-template-param.json` fájlt, telepítheti az erőforrásokat az Azure, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ac93d-135">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

<span data-ttu-id="ac93d-136">Ezek az erőforrások létrehozásához körülbelül öt percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="ac93d-136">It takes about five minutes to create these resources.</span></span> <span data-ttu-id="ac93d-137">Amíg az erőforrás létrehozása folyamatban van, továbbléphet a következő cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ac93d-137">While the resource creation is in progress, you can move on to the next article.</span></span>

## <a name="summary"></a><span data-ttu-id="ac93d-138">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="ac93d-138">Summary</span></span>
<span data-ttu-id="ac93d-139">Az Azure-függvény alkalmazás feldolgozni az IoT hub üzenetek és az Azure-tárfiók ezek az üzenetek tárolásához létrehozott.</span><span class="sxs-lookup"><span data-stu-id="ac93d-139">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="ac93d-140">Most már telepítheti és futtathatja a Arduino táblán eszköz a felhőbe küldött üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="ac93d-140">You can now deploy and run the sample to send device-to-cloud messages on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac93d-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac93d-141">Next steps</span></span>
<span data-ttu-id="ac93d-142">[Küldjön az eszköz a felhőbe küldött üzeneteket a Arduino indítópanel mintaalkalmazás futtatása][send-device-to-cloud-messages]</span><span class="sxs-lookup"><span data-stu-id="ac93d-142">[Run a sample application to send device-to-cloud messages on your Arduino board][send-device-to-cloud-messages]</span></span>

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md