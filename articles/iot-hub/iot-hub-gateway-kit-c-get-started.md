---
title: "SensorTag eszköz & Azure IoT átjáró – első lépések |} Microsoft Docs"
description: "Az IoT-átjáró Starter Kit első lépései az Azure IoT hub létrehozása és SensorTag és az IoT hub-átjáró"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub, az iot-átjáró, az eszközök internetes hálózata, iot eszközkészlet első lépések"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 56d05f4e-f2c1-4b22-8701-f01e14deead6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 624bdc7877d5048da08897f868272fd8e8f3f7b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a><span data-ttu-id="7e498-104">Az IoT átjáró Starter Kit egy SensorTag az első lépései</span><span class="sxs-lookup"><span data-stu-id="7e498-104">Get started with IoT Gateway Starter Kit with a SensorTag</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7e498-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="7e498-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="7e498-106">Szimulált eszköz</span><span class="sxs-lookup"><span data-stu-id="7e498-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="7e498-107">Ebben az oktatóanyagban kezdődik, majd a használatának alapjait tanulási [IoT átjáró Starter Kit](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="7e498-107">In this tutorial, you begin by learning the basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="7e498-108">Az Intel NUC szél folyó Linux operációs rendszert futtató működik, és a [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span><span class="sxs-lookup"><span data-stu-id="7e498-108">You will be working with Intel NUC that's running Wind River Linux and the [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span></span> <span data-ttu-id="7e498-109">Megtudhatja, hogyan seamleesly csatlakozzon a felhőbe az eszközök Azure IoT Hub használatával.</span><span class="sxs-lookup"><span data-stu-id="7e498-109">You will learn how to seamleesly connect your devices to the cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="7e498-110">**Még nem rendelkezik a csomag?:** kattintson [Itt](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="7e498-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="7e498-111">**Nem rendelkezik egy SensorTag?:** [indítsa el a szimulált eszköz](iot-hub-gateway-kit-c-sim-get-started.md) vagy [egy SensorTag megvásárlása](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span><span class="sxs-lookup"><span data-stu-id="7e498-111">**Don't have a SensorTag?:** [Start with a simulated device](iot-hub-gateway-kit-c-sim-get-started.md) or [buy a SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="7e498-112">1. lecke: A NUC konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e498-112">Lesson 1: Configure your NUC</span></span>
![Lesson1 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

<span data-ttu-id="7e498-114">Ebben a leckében Intel NUC (következő egység a Informatika) Azure IoT átjáróként Kit beállítása, telepítse az Azure IoT biztonsági csomag NUC, és futtassa a mintaalkalmazást átjáró működésének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="7e498-114">In this lesson, you set up Intel NUC (Next Unit of Computing) in the Kit as an Azure IoT gateway, install the Azure IoT Edge package on NUC, and run a sample app to verify the gateway functionality.</span></span>

<span data-ttu-id="7e498-115">*Oktatóanyag áttekintésének várható időtartama: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="7e498-115">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="7e498-116">Ugrás a [Intel NUC IoT átjáróként beállítása](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="7e498-116">Go to [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="7e498-117">2. lecke: Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e498-117">Lesson 2: Create your IoT Hub</span></span>
![Lesson2 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

<span data-ttu-id="7e498-119">Ez a lecke az eszközök és szoftverek telepítéséhez a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="7e498-119">In this lesson, you install the tools and software on your host computer.</span></span> <span data-ttu-id="7e498-120">Ezután a ingyenes Azure-fiók létrehozásához, kiépíteni az Azure IoT hub és az IoT hub létrehozása az első eszköz.</span><span class="sxs-lookup"><span data-stu-id="7e498-120">Then you create your free Azure account, provision your Azure IoT hub and create your first device in the IoT hub.</span></span>

<span data-ttu-id="7e498-121">Teljes lecke 1 Ez a lecke megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="7e498-121">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-the-tools"></a><span data-ttu-id="7e498-122">Eszközök letöltése</span><span class="sxs-lookup"><span data-stu-id="7e498-122">Get the tools</span></span>
<span data-ttu-id="7e498-123">Az eszközök és a szoftver telepítése a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="7e498-123">Install the tools and software on your host computer.</span></span>

<span data-ttu-id="7e498-124">*Oktatóanyag áttekintésének várható időtartama: 20 perc*</span><span class="sxs-lookup"><span data-stu-id="7e498-124">*Estimated time to complete: 20 minutes*</span></span>

<span data-ttu-id="7e498-125">Ugrás a [eszközök](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="7e498-125">Go to [Get the tools](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="7e498-126">Létrehoz egy IoT-központot, és regisztrálja az eszközt</span><span class="sxs-lookup"><span data-stu-id="7e498-126">Create an IoT hub and register your device</span></span>
<span data-ttu-id="7e498-127">Az erőforráscsoport létrehozásához, telepítéséhez az első Azure IoT hub és az első eszköz hozzáadása az IoT hub, az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="7e498-127">Create your resource group, provision your first Azure IoT hub, and add your first device to the IoT hub using the Azure CLI.</span></span>

<span data-ttu-id="7e498-128">*Oktatóanyag áttekintésének várható időtartama: 10 perc*</span><span class="sxs-lookup"><span data-stu-id="7e498-128">*Estimated time to complete: 10 minutes*</span></span>

<span data-ttu-id="7e498-129">Ugrás a [létrehoz egy IoT-központot, és regisztrálja az eszközt](iot-hub-gateway-kit-c-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="7e498-129">Go to [Create an IoT hub and register your device](iot-hub-gateway-kit-c-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="7e498-130">3. lecke: SensorTag érkező üzenetek fogadására és üzenetek olvasni az IoT hub</span><span class="sxs-lookup"><span data-stu-id="7e498-130">Lesson 3: Receive messages from SensorTag and read messages from your IoT hub</span></span>
<span data-ttu-id="7e498-131">Ez a lecke használandó automatizálására Gedélyezése mintaalkalmazás végrehajtásának és a konfigurációs az átjáró a parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="7e498-131">In this lesson, you will use scripts to automate the configuration and execution of a BLE sample application in your gateway.</span></span> <span data-ttu-id="7e498-132">Az ilyen alkalmazások modulok aggregátum- vagy átalakítási adatok gyűjteménye, parancsok, vagy műveletet hajt végre tetszőleges számú kapcsolódó feladat.</span><span class="sxs-lookup"><span data-stu-id="7e498-132">Such applications use a collection of modules to aggregate and transform data, process commands, or perform any number of related tasks.</span></span> <span data-ttu-id="7e498-133">Modulok egy üzenet közvetítőn keresztül kommunikáljanak egymással.</span><span class="sxs-lookup"><span data-stu-id="7e498-133">Modules communicate with one another via a message broker.</span></span> <span data-ttu-id="7e498-134">A mintaalkalmazás BLA modul, ezért az IoT hub modul rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7e498-134">The sample application has a BLE module and an IoT hub module.</span></span> <span data-ttu-id="7e498-135">A táblázat modul adatokat fogad az int SensorTag.</span><span class="sxs-lookup"><span data-stu-id="7e498-135">The BLE module receives data from BLE SensorTag.</span></span> <span data-ttu-id="7e498-136">Az IoT hub-modul a fogadott adatok csomagokat, és elküldi az IoT hub a megadott Azure IoT peremhálózati átjáró keretrendszeren keresztül.</span><span class="sxs-lookup"><span data-stu-id="7e498-136">The IoT hub module packages the data received and sends it to your IoT hub through the gateway framework provided in Azure IoT Edge.</span></span>

![Diagram. a végpont 3. rész](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-the-ble-sample-app"></a><span data-ttu-id="7e498-138">Konfigurálja és BLA mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="7e498-138">Configure and run the BLE sample app</span></span>
<span data-ttu-id="7e498-139">Összekapcsolhatók a SensorTag és az átjárót.</span><span class="sxs-lookup"><span data-stu-id="7e498-139">Set up the connectivity between SensorTag and your gateway.</span></span> <span data-ttu-id="7e498-140">Majd befejezni a konfigurálást, és futtassa a BLA mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7e498-140">Then finish the configuration and run the BLE sample application.</span></span>

<span data-ttu-id="7e498-141">*Oktatóanyag áttekintésének várható időtartama: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="7e498-141">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="7e498-142">Ugrás a [BLA mintaalkalmazás futtatása és konfigurálása](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span><span class="sxs-lookup"><span data-stu-id="7e498-142">Go to [Configure and run the BLE sample app](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="7e498-143">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="7e498-143">Read messages from your IoT hub</span></span>
<span data-ttu-id="7e498-144">Futtassa a mintakódot az IoT hub üzenetek olvasásakor a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="7e498-144">Run sample code on your host computer to read messages from your IoT hub.</span></span>

<span data-ttu-id="7e498-145">*Oktatóanyag áttekintésének várható időtartama: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="7e498-145">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="7e498-146">Ugrás a [üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="7e498-146">Go to [Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-to-azure-table-storage"></a><span data-ttu-id="7e498-147">4. lecke: Üzenetek mentése az Azure Table Storage szolgáltatásba</span><span class="sxs-lookup"><span data-stu-id="7e498-147">Lesson 4: Save messages to Azure Table storage</span></span>
<span data-ttu-id="7e498-148">Hozzon létre egy Azure függvény alkalmazást, amely a bejövő üzenetek lekérése az IoT hub, és írja őket az Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="7e498-148">Create an Azure function app that gets incoming messages from your IoT hub and writes them to Azure Table storage.</span></span>

![Rész 4 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="7e498-150">Egy Azure függvény app és az Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e498-150">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="7e498-151">Azure Resource Manager-sablonok segítségével hozzon létre egy Azure függvény alkalmazást és egy Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="7e498-151">Use an Azure Resource Manager template to create an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="7e498-152">*Oktatóanyag áttekintésének várható időtartama: 10 perc*</span><span class="sxs-lookup"><span data-stu-id="7e498-152">*Estimated time to complete: 10 minutes*</span></span>

<span data-ttu-id="7e498-153">Ugrás a [egy Azure függvény app és az Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="7e498-153">Go to [Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="7e498-154">Azure Table storage-ban tárolt üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="7e498-154">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="7e498-155">Az átjáró-a-felhőbe küldött üzeneteket, az Azure Table storage írás figyelése</span><span class="sxs-lookup"><span data-stu-id="7e498-155">Monitor the gateway-to-cloud messages as they are written to Azure Table storage.</span></span>

<span data-ttu-id="7e498-156">*Oktatóanyag áttekintésének várható időtartama: 5 perc*</span><span class="sxs-lookup"><span data-stu-id="7e498-156">*Estimated time to complete: 5 minutes*</span></span>

<span data-ttu-id="7e498-157">Ugrás a [Azure Table storage-ban tárolt üzenetek olvasása](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="7e498-157">Go to [Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7e498-158">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="7e498-158">Troubleshooting</span></span>
<span data-ttu-id="7e498-159">Ha bármilyen probléma során használatáról, keressen megoldásokat a [hibaelhárítás](iot-hub-gateway-kit-c-troubleshooting.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7e498-159">If you have any problems during the lessons, look for solutions in the [Troubleshooting](iot-hub-gateway-kit-c-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="7e498-160">További részletek</span><span class="sxs-lookup"><span data-stu-id="7e498-160">Explore more</span></span>
<span data-ttu-id="7e498-161">Látogasson el a [Intel IoT-átjáró csomag fejlesztői zóna](http://software.intel.com/iot/microsoft-azure) további.</span><span class="sxs-lookup"><span data-stu-id="7e498-161">Visit the [Intel IoT Gateway Kit developer zone](http://software.intel.com/iot/microsoft-azure) to learn more.</span></span>