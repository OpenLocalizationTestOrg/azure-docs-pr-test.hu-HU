---
title: "Szimulált eszköz & Azure IoT átjáró – első lépések |} Microsoft Docs"
description: "Az IoT-átjáró Starter Kit első lépései az Azure IoT hub létrehozása és connect Gateway toohello IoT-központ"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub, iot-átjáró első lépések hello az eszközök internetes hálózata, iot eszközkészlet"
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
ms.openlocfilehash: 1a54a5e5f1c1d9b2e657c9e4448274256e2533f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a><span data-ttu-id="c58bf-104">A szimulált eszköz Ismerkedés az IoT-átjáró Starter Kit</span><span class="sxs-lookup"><span data-stu-id="c58bf-104">Get started with IoT Gateway Starter Kit with a simulated device</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c58bf-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="c58bf-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="c58bf-106">Szimulált eszköz</span><span class="sxs-lookup"><span data-stu-id="c58bf-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="c58bf-107">Ebben az oktatóanyagban kezdődik, majd hello használatának alapjait tanulási [IoT átjáró Starter Kit](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="c58bf-107">In this tutorial, you begin by learning hello basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="c58bf-108">Dolgozni fog az Intel NUC szél folyó Linux operációs rendszert futtató.</span><span class="sxs-lookup"><span data-stu-id="c58bf-108">You will be working with Intel NUC that's running Wind River Linux.</span></span> <span data-ttu-id="c58bf-109">Megtudhatja, hogyan tooseamleesly kapcsolódnak az eszközök toohello felhőalapú Azure IoT Hub használatával.</span><span class="sxs-lookup"><span data-stu-id="c58bf-109">You will learn how tooseamleesly connect your devices toohello cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="c58bf-110">**Még nem rendelkezik a csomag?:** kattintson [Itt](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="c58bf-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="c58bf-111">1. lecke: A NUC konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c58bf-111">Lesson 1: Configure your NUC</span></span>
![Lesson1 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

<span data-ttu-id="c58bf-113">Ez a lecke hello Azure IoT átjáróként Kit Intel NUC (következő egység a Informatika) beállítása, NUC hello Azure IoT biztonsági csomag telepítését, és futtatni egy minta app tooverify hello átjáró funkciót.</span><span class="sxs-lookup"><span data-stu-id="c58bf-113">In this lesson, you set up Intel NUC (Next Unit of Computing) in hello Kit as an Azure IoT gateway, install hello Azure IoT Edge package on NUC, and run a sample app tooverify hello gateway functionality.</span></span>

<span data-ttu-id="c58bf-114">*Becsült idő toocomplete: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="c58bf-114">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="c58bf-115">Nyissa meg túl[Intel NUC IoT átjáróként beállítása](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="c58bf-115">Go too[Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="c58bf-116">2. lecke: Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="c58bf-116">Lesson 2: Create your IoT Hub</span></span>
![Lesson2 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

<span data-ttu-id="c58bf-118">Ez a lecke hello eszközök és szoftverek telepítéséhez a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="c58bf-118">In this lesson, you install hello tools and software on your host computer.</span></span> <span data-ttu-id="c58bf-119">Ezután a ingyenes Azure-fiók létrehozásához, kiépíteni az Azure IoT hub és az első eszköz hello IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c58bf-119">Then you create your free Azure account, provision your Azure IoT hub and create your first device in hello IoT hub.</span></span>

<span data-ttu-id="c58bf-120">Teljes lecke 1 Ez a lecke megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="c58bf-120">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-hello-tools"></a><span data-ttu-id="c58bf-121">Hello eszközök beszerzése</span><span class="sxs-lookup"><span data-stu-id="c58bf-121">Get hello tools</span></span>
<span data-ttu-id="c58bf-122">A gazdaszámítógép hello eszközök és szoftverek telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c58bf-122">Install hello tools and software on your host computer.</span></span>

<span data-ttu-id="c58bf-123">*Becsült idő toocomplete: 20 perc*</span><span class="sxs-lookup"><span data-stu-id="c58bf-123">*Estimated time toocomplete: 20 minutes*</span></span>

<span data-ttu-id="c58bf-124">Nyissa meg túl[hello eszközök beszerzése](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="c58bf-124">Go too[Get hello tools](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="c58bf-125">Létrehoz egy IoT-központot, és regisztrálja az eszközt</span><span class="sxs-lookup"><span data-stu-id="c58bf-125">Create an IoT hub and register your device</span></span>
<span data-ttu-id="c58bf-126">Az erőforráscsoport létrehozása, az első Azure IoT hub kiépíteni, és adja hozzá az első eszköz toohello IoT hub hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="c58bf-126">Create your resource group, provision your first Azure IoT hub, and add your first device toohello IoT hub using hello Azure CLI.</span></span>

<span data-ttu-id="c58bf-127">*Becsült idő toocomplete: 10 perc*</span><span class="sxs-lookup"><span data-stu-id="c58bf-127">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="c58bf-128">Nyissa meg túl[létrehoz egy IoT-központot, és regisztrálja az eszközt](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="c58bf-128">Go too[Create an IoT hub and register your device](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-hello-simulated-device-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="c58bf-129">3. lecke: Hello szimulált eszköz érkező üzenetek fogadására, és üzeneteket beolvasni az IoT hub</span><span class="sxs-lookup"><span data-stu-id="c58bf-129">Lesson 3: Receive messages from hello simulated device and read messages from your IoT hub</span></span>
<span data-ttu-id="c58bf-130">Ez a lecke szüksége lesz egy szimulált eszköz alkalmazásának végrehajtásának és parancsfájlok tooautomate hello konfigurációs az átjárót.</span><span class="sxs-lookup"><span data-stu-id="c58bf-130">In this lesson, you will use scripts tooautomate hello configuration and execution of a simulated device app in your gateway.</span></span> <span data-ttu-id="c58bf-131">hello szimulált eszköz alkalmazásának minta hőmérséklet adatokat állít elő, és elküldi azt tooan IoT hub modul.</span><span class="sxs-lookup"><span data-stu-id="c58bf-131">hello simulated device app generates sample temperature data and sends it tooan IoT hub module.</span></span> <span data-ttu-id="c58bf-132">hello IoT hub modul csomagok hello adatokat fogad, és elküldi azt az Azure IoT Edge tooyour IoT-központ hello átjáró keretrendszere keresztül.</span><span class="sxs-lookup"><span data-stu-id="c58bf-132">hello IoT hub module packages hello data received and sends it tooyour IoT hub through hello gateway framework provided in Azure IoT Edge.</span></span>

![Diagram. a végpont 3. rész](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a><span data-ttu-id="c58bf-134">Konfigurálja és futtassa a szimulált eszköz</span><span class="sxs-lookup"><span data-stu-id="c58bf-134">Configure and run a simulated device</span></span>
<span data-ttu-id="c58bf-135">Készítse elő a hello minta kódokat.</span><span class="sxs-lookup"><span data-stu-id="c58bf-135">Prepare hello sample codes.</span></span> <span data-ttu-id="c58bf-136">Ezután konfigurálja, és hello szimulált eszköz mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="c58bf-136">Then configure and run hello simulated device sample application.</span></span>

<span data-ttu-id="c58bf-137">*Becsült idő toocomplete: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="c58bf-137">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="c58bf-138">Nyissa meg túl[beállítása és futtatása a szimulált eszköz](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span><span class="sxs-lookup"><span data-stu-id="c58bf-138">Go too[Configure and run a simulated device](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="c58bf-139">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="c58bf-139">Read messages from your IoT hub</span></span>
<span data-ttu-id="c58bf-140">Futtassa a mintakód a fogadó számítógép tooread köszönőüzenetei az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c58bf-140">Run a sample code on your host computer tooread hello messages from your IoT hub.</span></span>

<span data-ttu-id="c58bf-141">*Becsült idő toocomplete: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="c58bf-141">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="c58bf-142">Nyissa meg túl[üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="c58bf-142">Go too[Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-tooazure-table-storage"></a><span data-ttu-id="c58bf-143">4. lecke: Üzenetek tooAzure a Table storage mentése</span><span class="sxs-lookup"><span data-stu-id="c58bf-143">Lesson 4: Save messages tooAzure Table storage</span></span>
<span data-ttu-id="c58bf-144">Hozzon létre, hogy a bejövő üzenetek lekérése az IoT hub, majd beírja őket tooAzure a Table storage Azure függvény alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c58bf-144">Create an Azure function app that gets incoming messages from your IoT hub and writes them tooAzure Table storage.</span></span>

![Rész 4 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="c58bf-146">Egy Azure függvény app és az Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c58bf-146">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="c58bf-147">Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="c58bf-147">Use an Azure Resource Manager template toocreate an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="c58bf-148">*Becsült idő toocomplete: 10 perc*</span><span class="sxs-lookup"><span data-stu-id="c58bf-148">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="c58bf-149">Nyissa meg túl[Azure függvény app és Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="c58bf-149">Go too[Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="c58bf-150">Azure Table storage-ban tárolt üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="c58bf-150">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="c58bf-151">Átjáró felhő köszönőüzenetei, az oktatóprogram tooAzure a Table storage figyelése</span><span class="sxs-lookup"><span data-stu-id="c58bf-151">Monitor hello gateway-to-cloud messages as they are written tooAzure Table storage.</span></span>

<span data-ttu-id="c58bf-152">*Becsült idő toocomplete: 5 perc*</span><span class="sxs-lookup"><span data-stu-id="c58bf-152">*Estimated time toocomplete: 5 minutes*</span></span>

<span data-ttu-id="c58bf-153">Nyissa meg túl[Azure Table storage-ban tárolt üzenetek olvasása](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c58bf-153">Go too[Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c58bf-154">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="c58bf-154">Troubleshooting</span></span>
<span data-ttu-id="c58bf-155">Ha bármilyen probléma során hello során tapasztalatokat, keressen megoldásokat hello [hibaelhárítás](iot-hub-gateway-kit-c-sim-troubleshooting.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c58bf-155">If you have any problems during hello lessons, look for solutions in hello [Troubleshooting](iot-hub-gateway-kit-c-sim-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="c58bf-156">További részletek</span><span class="sxs-lookup"><span data-stu-id="c58bf-156">Explore more</span></span>
<span data-ttu-id="c58bf-157">A Microsoft hello [Intel IoT-átjáró csomag fejlesztői zóna](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="c58bf-157">Visit hello [Intel IoT Gateway Kit developer zone](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) toolearn more.</span></span>
