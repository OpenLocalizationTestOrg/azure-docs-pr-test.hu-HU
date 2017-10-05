---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 4: üzenetek menthetők |} Microsoft Docs"
description: "Intel NUC üzenetek mentéséhez az IoT hub, Azure Table Storage írja őket, és majd olvashatja őket a felhőből."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok tárolása a felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: ffed0c2e-b092-40e1-9113-8196ec057d67
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c7fc47b07acede28ffe790debca7e38521726011
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a><span data-ttu-id="b818e-104">Azure-függvényalkalmazás és -tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b818e-104">Create an Azure function app and storage account</span></span>

<span data-ttu-id="b818e-105">Az Azure Functions megoldással egyszerűen futtathatók _funkciók_ (kis kódrészletek) a felhőben.</span><span class="sxs-lookup"><span data-stu-id="b818e-105">Azure Functions is a solution for easily running _functions_ (small pieces of code) in the cloud.</span></span> <span data-ttu-id="b818e-106">Egy Azure függvényalkalmazás biztosítja a a függvények végrehajtásához szükséges az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b818e-106">An Azure function app hosts the execution of your functions in Azure.</span></span> 

## <a name="what-you-will-do"></a><span data-ttu-id="b818e-107">Mit fog</span><span class="sxs-lookup"><span data-stu-id="b818e-107">What you will do</span></span>

- <span data-ttu-id="b818e-108">Az Azure Resource Manager-sablon használatával hozzon létre egy Azure függvény alkalmazást és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="b818e-108">Use an Azure Resource Manager template to create an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="b818e-109">Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="b818e-109">The Azure function app listens to Azure IoT hub events, processes incoming messages, and writes them to Azure Table storage.</span></span>

<span data-ttu-id="b818e-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="b818e-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>


## <a name="what-you-will-learn"></a><span data-ttu-id="b818e-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="b818e-111">What you will learn</span></span>

<span data-ttu-id="b818e-112">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="b818e-112">In this lesson, you will learn:</span></span>

- <span data-ttu-id="b818e-113">Hogyan használható az Azure Resource Manager központi telepítése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b818e-113">How to use Azure Resource Manager to deploy Azure resources.</span></span>
- <span data-ttu-id="b818e-114">Hogyan használható az Azure-függvény alkalmazás IoT-központ üzenetek feldolgozásához, és az Azure Table storage egy táblához.</span><span class="sxs-lookup"><span data-stu-id="b818e-114">How to use an Azure function app to process IoT Hub messages and write them to a table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b818e-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="b818e-115">What you need</span></span>

<span data-ttu-id="b818e-116">Sikeresen végrehajtotta a korábbi rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="b818e-116">You must have successfully completed the previous lessons:</span></span>

- [<span data-ttu-id="b818e-117">1. lecke: Állítsa be az Intel NUC IoT átjáróként</span><span class="sxs-lookup"><span data-stu-id="b818e-117">Lesson 1: Set up your Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)
- [<span data-ttu-id="b818e-118">2. lecke: Felkészülés a gazdaszámítógép és Azure IoT-központ</span><span class="sxs-lookup"><span data-stu-id="b818e-118">Lesson 2: Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
- [<span data-ttu-id="b818e-119">3. lecke: Üzeneteket fogadjon a szimulált eszköz, és üzeneteket beolvasni az IoT hub</span><span class="sxs-lookup"><span data-stu-id="b818e-119">Lesson 3: Receive messages from the simulated device and read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

## <a name="open-a-sample-app"></a><span data-ttu-id="b818e-120">Nyissa meg a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="b818e-120">Open a sample app</span></span>

<span data-ttu-id="b818e-121">Lépjen a `iot-hub-c-intel-nuc-gateway-getting-started` tárház mappa inicializálni a konfigurációs fájlokat, és ezután nyissa meg a minta projektet a Visual Studio Code a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b818e-121">Go to your `iot-hub-c-intel-nuc-gateway-getting-started` repo folder, initialize the configuration files, and then open the sample project in Visual Studio Code by running the following command:</span></span>

```bash
cd Lesson4
npm install
gulp init
code .
```

![Tárház szerkezete](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- <span data-ttu-id="b818e-123">A `arm-template.json` fájl az Azure Resource Manager sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="b818e-123">The `arm-template.json` file is the Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
- <span data-ttu-id="b818e-124">A `arm-template-param.json` a konfigurációs fájl az Azure Resource Manager-sablon által használt.</span><span class="sxs-lookup"><span data-stu-id="b818e-124">The `arm-template-param.json` file is the configuration file used by the Azure Resource Manager template.</span></span>
- <span data-ttu-id="b818e-125">A `ReceiveDeviceMessages` tartalmazza, a Node.js-kódot az Azure-függvény.</span><span class="sxs-lookup"><span data-stu-id="b818e-125">The `ReceiveDeviceMessages` subfolder contains the Node.js code for the Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="b818e-126">Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b818e-126">Configure Azure Resource Manager templates and create resources in Azure</span></span>

<span data-ttu-id="b818e-127">Frissítés a `arm-template-param.json` fájlt a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b818e-127">Update the `arm-template-param.json` file in Visual Studio Code.</span></span>

![ARM-sablon json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- <span data-ttu-id="b818e-129">Cserélje le `[your IoT Hub name]` rendelkező `{my hub name}` , amelyet a 2.</span><span class="sxs-lookup"><span data-stu-id="b818e-129">Replace `[your IoT Hub name]` with `{my hub name}` that you specified in Lesson 2.</span></span>

<span data-ttu-id="b818e-130">Miután frissítette a `arm-template-param.json` fájlt, telepítheti az erőforrásokat az Azure, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b818e-130">After you update the `arm-template-param.json` file, deploy the resources to Azure by running the following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

<span data-ttu-id="b818e-131">Használjon `iot-gateway` értékeként `{resource group name}` Ha nem módosítja az érték a 2.</span><span class="sxs-lookup"><span data-stu-id="b818e-131">Use `iot-gateway` as the value of `{resource group name}` if you didn't change the value in Lesson 2.</span></span>

## <a name="summary"></a><span data-ttu-id="b818e-132">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="b818e-132">Summary</span></span>

<span data-ttu-id="b818e-133">Az Azure-függvény alkalmazás feldolgozni az IoT hub üzenetek és az Azure-tárfiók ezek az üzenetek tárolásához létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b818e-133">You've created your Azure function app to process IoT hub messages and an Azure storage account to store these messages.</span></span> <span data-ttu-id="b818e-134">Elolvashatja az átjárót az IoT hub által küldött állapotüzenetek.</span><span class="sxs-lookup"><span data-stu-id="b818e-134">You can now read messages that are sent by your gateway to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b818e-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b818e-135">Next steps</span></span>
<span data-ttu-id="b818e-136">[Az Azure Storage megőrzött olvasható üzenetek](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="b818e-136">[Read messages persisted in Azure Storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>
