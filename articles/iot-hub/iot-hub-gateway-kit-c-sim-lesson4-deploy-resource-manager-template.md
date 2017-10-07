---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 4: üzenetek menthetők |} Microsoft Docs"
description: "Intel NUC tooyour IoT hubról üzenetek menthetők, írja őket tooAzure a Table storage, és majd hello felhőből olvashatja őket."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adattárolás hello felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
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
ms.openlocfilehash: 230f2708b62b89c6eed2e238efefc1c4da86e373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a><span data-ttu-id="874fb-104">Azure-függvényalkalmazás és -tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="874fb-104">Create an Azure function app and storage account</span></span>

<span data-ttu-id="874fb-105">Az Azure Functions megoldással egyszerűen futtathatók _funkciók_ (kis kódrészletek) hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="874fb-105">Azure Functions is a solution for easily running _functions_ (small pieces of code) in hello cloud.</span></span> <span data-ttu-id="874fb-106">Egy Azure függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="874fb-106">An Azure function app hosts hello execution of your functions in Azure.</span></span> 

## <a name="what-you-will-do"></a><span data-ttu-id="874fb-107">Mit fog</span><span class="sxs-lookup"><span data-stu-id="874fb-107">What you will do</span></span>

- <span data-ttu-id="874fb-108">Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="874fb-108">Use an Azure Resource Manager template toocreate an Azure function app and an Azure storage account.</span></span> <span data-ttu-id="874fb-109">hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat.</span><span class="sxs-lookup"><span data-stu-id="874fb-109">hello Azure function app listens tooAzure IoT hub events, processes incoming messages, and writes them tooAzure Table storage.</span></span>

<span data-ttu-id="874fb-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="874fb-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>


## <a name="what-you-will-learn"></a><span data-ttu-id="874fb-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="874fb-111">What you will learn</span></span>

<span data-ttu-id="874fb-112">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="874fb-112">In this lesson, you will learn:</span></span>

- <span data-ttu-id="874fb-113">Hogyan toouse Azure Resource Manager toodeploy Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="874fb-113">How toouse Azure Resource Manager toodeploy Azure resources.</span></span>
- <span data-ttu-id="874fb-114">Hogyan toouse az Azure app tooprocess IoT-központ üzenetek működéséhez, és beírhatók tooa tábla Azure Table storage-ban.</span><span class="sxs-lookup"><span data-stu-id="874fb-114">How toouse an Azure function app tooprocess IoT Hub messages and write them tooa table in Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="874fb-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="874fb-115">What you need</span></span>

<span data-ttu-id="874fb-116">Sikeresen végrehajtotta hello előző megszerzett:</span><span class="sxs-lookup"><span data-stu-id="874fb-116">You must have successfully completed hello previous lessons:</span></span>

- [<span data-ttu-id="874fb-117">1. lecke: Állítsa be az Intel NUC IoT átjáróként</span><span class="sxs-lookup"><span data-stu-id="874fb-117">Lesson 1: Set up your Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)
- [<span data-ttu-id="874fb-118">2. lecke: Felkészülés a gazdaszámítógép és Azure IoT-központ</span><span class="sxs-lookup"><span data-stu-id="874fb-118">Lesson 2: Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
- [<span data-ttu-id="874fb-119">3. lecke: Hello szimulált eszköz érkező üzenetek fogadására, és üzeneteket beolvasni az IoT hub</span><span class="sxs-lookup"><span data-stu-id="874fb-119">Lesson 3: Receive messages from hello simulated device and read messages from your IoT hub</span></span>](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

## <a name="open-a-sample-app"></a><span data-ttu-id="874fb-120">Nyissa meg a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="874fb-120">Open a sample app</span></span>

<span data-ttu-id="874fb-121">Nyissa meg tooyour `iot-hub-c-intel-nuc-gateway-getting-started` tárház mappa, inicializálási hello konfigurációs fájlokat és majd nyitott hello minta projektre a Visual Studio Code hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="874fb-121">Go tooyour `iot-hub-c-intel-nuc-gateway-getting-started` repo folder, initialize hello configuration files, and then open hello sample project in Visual Studio Code by running hello following command:</span></span>

```bash
cd Lesson4
npm install
gulp init
code .
```

![Tárház szerkezete](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- <span data-ttu-id="874fb-123">Hello `arm-template.json` fájl hello Azure Resource Manager-sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="874fb-123">hello `arm-template.json` file is hello Azure Resource Manager template that contains an Azure function app and an Azure storage account.</span></span>
- <span data-ttu-id="874fb-124">Hello `arm-template-param.json` hello konfigurációs fájl hello Azure Resource Manager-sablon által használt.</span><span class="sxs-lookup"><span data-stu-id="874fb-124">hello `arm-template-param.json` file is hello configuration file used by hello Azure Resource Manager template.</span></span>
- <span data-ttu-id="874fb-125">Hello `ReceiveDeviceMessages` hello Azure függvény hello Node.js kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="874fb-125">hello `ReceiveDeviceMessages` subfolder contains hello Node.js code for hello Azure function.</span></span>

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a><span data-ttu-id="874fb-126">Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="874fb-126">Configure Azure Resource Manager templates and create resources in Azure</span></span>

<span data-ttu-id="874fb-127">Frissítés hello `arm-template-param.json` fájlt a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="874fb-127">Update hello `arm-template-param.json` file in Visual Studio Code.</span></span>

![ARM-sablon json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- <span data-ttu-id="874fb-129">Cserélje le `[your IoT Hub name]` rendelkező `{my hub name}` , amelyet a 2.</span><span class="sxs-lookup"><span data-stu-id="874fb-129">Replace `[your IoT Hub name]` with `{my hub name}` that you specified in Lesson 2.</span></span>

<span data-ttu-id="874fb-130">Miután frissítette hello `arm-template-param.json` fájlt, hello erőforrások tooAzure telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="874fb-130">After you update hello `arm-template-param.json` file, deploy hello resources tooAzure by running hello following command:</span></span>

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

<span data-ttu-id="874fb-131">Használjon `iot-gateway` hello értékeként `{resource group name}` nem módosítása hello érték a 2.</span><span class="sxs-lookup"><span data-stu-id="874fb-131">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="summary"></a><span data-ttu-id="874fb-132">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="874fb-132">Summary</span></span>

<span data-ttu-id="874fb-133">Létrehozta az Azure-függvény app tooprocess IoT hub-üzeneteket és az Azure-tárolóra toostore ezeket az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="874fb-133">You've created your Azure function app tooprocess IoT hub messages and an Azure storage account toostore these messages.</span></span> <span data-ttu-id="874fb-134">Elolvashatja az átjáró tooyour IoT hub által küldött állapotüzenetek.</span><span class="sxs-lookup"><span data-stu-id="874fb-134">You can now read messages that are sent by your gateway tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="874fb-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="874fb-135">Next steps</span></span>
<span data-ttu-id="874fb-136">[Az Azure Storage megőrzött olvasható üzenetek](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="874fb-136">[Read messages persisted in Azure Storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>
