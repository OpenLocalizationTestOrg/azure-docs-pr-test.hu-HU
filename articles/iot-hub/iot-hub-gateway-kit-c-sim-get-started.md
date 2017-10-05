---
title: "Szimulált eszköz & Azure IoT átjáró – első lépések |} Microsoft Docs"
description: "Az IoT-átjáró Starter Kit első lépései az Azure IoT hub létrehozása és csatlakozás az IoT hub-átjáró"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub, az iot-átjáró, az eszközök internetes hálózata, iot eszközkészlet első lépések"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0c110b8b-bee4-4aec-a18a-dfc292aa17a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 916fa40d9ac857dfa72197b40c232834593d3891
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a><span data-ttu-id="95cea-104">A szimulált eszköz Ismerkedés az IoT-átjáró Starter Kit</span><span class="sxs-lookup"><span data-stu-id="95cea-104">Get started with IoT Gateway Starter Kit with a simulated device</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="95cea-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="95cea-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="95cea-106">Szimulált eszköz</span><span class="sxs-lookup"><span data-stu-id="95cea-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="95cea-107">Ebben az oktatóanyagban kezdődik, majd a használatának alapjait tanulási [IoT átjáró Starter Kit](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="95cea-107">In this tutorial, you begin by learning the basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="95cea-108">Dolgozni fog az Intel NUC szél folyó Linux operációs rendszert futtató.</span><span class="sxs-lookup"><span data-stu-id="95cea-108">You will be working with Intel NUC that's running Wind River Linux.</span></span> <span data-ttu-id="95cea-109">Megtudhatja, hogyan seamleesly csatlakozzon a felhőbe az eszközök Azure IoT Hub használatával.</span><span class="sxs-lookup"><span data-stu-id="95cea-109">You will learn how to seamleesly connect your devices to the cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="95cea-110">**Még nem rendelkezik a csomag?:** kattintson [Itt](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="95cea-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="95cea-111">1. lecke: A NUC konfigurálása</span><span class="sxs-lookup"><span data-stu-id="95cea-111">Lesson 1: Configure your NUC</span></span>
![Lesson1 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

<span data-ttu-id="95cea-113">Ebben a leckében Intel NUC (következő egység a Informatika) Azure IoT átjáróként Kit beállítása, telepítse az Azure IoT biztonsági csomag NUC, és futtassa a mintaalkalmazást átjáró működésének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="95cea-113">In this lesson, you set up Intel NUC (Next Unit of Computing) in the Kit as an Azure IoT gateway, install the Azure IoT Edge package on NUC, and run a sample app to verify the gateway functionality.</span></span>

<span data-ttu-id="95cea-114">*Oktatóanyag áttekintésének várható időtartama: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="95cea-114">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="95cea-115">Ugrás a [Intel NUC IoT átjáróként beállítása](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="95cea-115">Go to [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="95cea-116">2. lecke: Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="95cea-116">Lesson 2: Create your IoT Hub</span></span>
![Lesson2 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

<span data-ttu-id="95cea-118">Ez a lecke az eszközök és szoftverek telepítéséhez a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="95cea-118">In this lesson, you install the tools and software on your host computer.</span></span> <span data-ttu-id="95cea-119">Ezután a ingyenes Azure-fiók létrehozásához, kiépíteni az Azure IoT hub és az IoT hub létrehozása az első eszköz.</span><span class="sxs-lookup"><span data-stu-id="95cea-119">Then you create your free Azure account, provision your Azure IoT hub and create your first device in the IoT hub.</span></span>

<span data-ttu-id="95cea-120">Teljes lecke 1 Ez a lecke megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="95cea-120">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-the-tools"></a><span data-ttu-id="95cea-121">Eszközök letöltése</span><span class="sxs-lookup"><span data-stu-id="95cea-121">Get the tools</span></span>
<span data-ttu-id="95cea-122">Az eszközök és a szoftver telepítése a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="95cea-122">Install the tools and software on your host computer.</span></span>

<span data-ttu-id="95cea-123">*Oktatóanyag áttekintésének várható időtartama: 20 perc*</span><span class="sxs-lookup"><span data-stu-id="95cea-123">*Estimated time to complete: 20 minutes*</span></span>

<span data-ttu-id="95cea-124">Ugrás a [eszközök](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="95cea-124">Go to [Get the tools](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="95cea-125">Létrehoz egy IoT-központot, és regisztrálja az eszközt</span><span class="sxs-lookup"><span data-stu-id="95cea-125">Create an IoT hub and register your device</span></span>
<span data-ttu-id="95cea-126">Az erőforráscsoport létrehozásához, telepítéséhez az első Azure IoT hub és az első eszköz hozzáadása az IoT hub, az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="95cea-126">Create your resource group, provision your first Azure IoT hub, and add your first device to the IoT hub using the Azure CLI.</span></span>

<span data-ttu-id="95cea-127">*Oktatóanyag áttekintésének várható időtartama: 10 perc*</span><span class="sxs-lookup"><span data-stu-id="95cea-127">*Estimated time to complete: 10 minutes*</span></span>

<span data-ttu-id="95cea-128">Ugrás a [létrehoz egy IoT-központot, és regisztrálja az eszközt](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="95cea-128">Go to [Create an IoT hub and register your device](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-the-simulated-device-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="95cea-129">3. lecke: Üzeneteket fogadjon a szimulált eszköz, és üzeneteket beolvasni az IoT hub</span><span class="sxs-lookup"><span data-stu-id="95cea-129">Lesson 3: Receive messages from the simulated device and read messages from your IoT hub</span></span>
<span data-ttu-id="95cea-130">Ez a lecke parancsfájlok használandó automatizálhatja a konfigurációs és a szimulált eszköz alkalmazások futtatásához az átjáró található.</span><span class="sxs-lookup"><span data-stu-id="95cea-130">In this lesson, you will use scripts to automate the configuration and execution of a simulated device app in your gateway.</span></span> <span data-ttu-id="95cea-131">A szimulált eszköz alkalmazásának minta hőmérséklet adatokat állít elő, és elküldi az IoT hub modul.</span><span class="sxs-lookup"><span data-stu-id="95cea-131">The simulated device app generates sample temperature data and sends it to an IoT hub module.</span></span> <span data-ttu-id="95cea-132">Az IoT hub-modul a fogadott adatok csomagokat, és elküldi az IoT hub a megadott Azure IoT peremhálózati átjáró keretrendszeren keresztül.</span><span class="sxs-lookup"><span data-stu-id="95cea-132">The IoT hub module packages the data received and sends it to your IoT hub through the gateway framework provided in Azure IoT Edge.</span></span>

![Diagram. a végpont 3. rész](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a><span data-ttu-id="95cea-134">Konfigurálja és futtassa a szimulált eszköz</span><span class="sxs-lookup"><span data-stu-id="95cea-134">Configure and run a simulated device</span></span>
<span data-ttu-id="95cea-135">Készítse elő a minta-kódokat.</span><span class="sxs-lookup"><span data-stu-id="95cea-135">Prepare the sample codes.</span></span> <span data-ttu-id="95cea-136">Ezután konfigurálja, és futtassa a szimulált eszköz mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="95cea-136">Then configure and run the simulated device sample application.</span></span>

<span data-ttu-id="95cea-137">*Oktatóanyag áttekintésének várható időtartama: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="95cea-137">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="95cea-138">Ugrás a [beállítása és futtatása a szimulált eszköz](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span><span class="sxs-lookup"><span data-stu-id="95cea-138">Go to [Configure and run a simulated device](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="95cea-139">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="95cea-139">Read messages from your IoT hub</span></span>
<span data-ttu-id="95cea-140">Futtassa a mintakódot az üzenetek olvasásakor az IoT hub a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="95cea-140">Run a sample code on your host computer to read the messages from your IoT hub.</span></span>

<span data-ttu-id="95cea-141">*Oktatóanyag áttekintésének várható időtartama: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="95cea-141">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="95cea-142">Ugrás a [üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="95cea-142">Go to [Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-to-azure-table-storage"></a><span data-ttu-id="95cea-143">4. lecke: Üzenetek mentése az Azure Table Storage szolgáltatásba</span><span class="sxs-lookup"><span data-stu-id="95cea-143">Lesson 4: Save messages to Azure Table storage</span></span>
<span data-ttu-id="95cea-144">Hozzon létre egy Azure függvény alkalmazást, amely a bejövő üzenetek lekérése az IoT hub, és írja őket az Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="95cea-144">Create an Azure function app that gets incoming messages from your IoT hub and writes them to Azure Table storage.</span></span>

![Rész 4 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="95cea-146">Egy Azure függvény app és az Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="95cea-146">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="95cea-147">Azure Resource Manager-sablonok segítségével hozzon létre egy Azure függvény alkalmazást és egy Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="95cea-147">Use an Azure Resource Manager template to create an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="95cea-148">*Oktatóanyag áttekintésének várható időtartama: 10 perc*</span><span class="sxs-lookup"><span data-stu-id="95cea-148">*Estimated time to complete: 10 minutes*</span></span>

<span data-ttu-id="95cea-149">Ugrás a [egy Azure függvény app és az Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="95cea-149">Go to [Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="95cea-150">Azure Table storage-ban tárolt üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="95cea-150">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="95cea-151">Az átjáró-a-felhőbe küldött üzeneteket, az Azure Table storage írás figyelése</span><span class="sxs-lookup"><span data-stu-id="95cea-151">Monitor the gateway-to-cloud messages as they are written to Azure Table storage.</span></span>

<span data-ttu-id="95cea-152">*Oktatóanyag áttekintésének várható időtartama: 5 perc*</span><span class="sxs-lookup"><span data-stu-id="95cea-152">*Estimated time to complete: 5 minutes*</span></span>

<span data-ttu-id="95cea-153">Ugrás a [Azure Table storage-ban tárolt üzenetek olvasása](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="95cea-153">Go to [Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="95cea-154">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="95cea-154">Troubleshooting</span></span>
<span data-ttu-id="95cea-155">Ha bármilyen probléma során használatáról, keressen megoldásokat a [hibaelhárítás](iot-hub-gateway-kit-c-sim-troubleshooting.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="95cea-155">If you have any problems during the lessons, look for solutions in the [Troubleshooting](iot-hub-gateway-kit-c-sim-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="95cea-156">További részletek</span><span class="sxs-lookup"><span data-stu-id="95cea-156">Explore more</span></span>
<span data-ttu-id="95cea-157">Látogasson el a [Intel IoT-átjáró csomag fejlesztői zóna](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) további.</span><span class="sxs-lookup"><span data-stu-id="95cea-157">Visit the [Intel IoT Gateway Kit developer zone](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) to learn more.</span></span>