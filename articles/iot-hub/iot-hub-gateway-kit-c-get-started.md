---
title: "SensorTag eszköz & Azure IoT átjáró – első lépések |} Microsoft Docs"
description: "Az IoT-átjáró Starter Kit első lépései az Azure IoT hub létrehozása és csatlakozás SensorTag és átjáró toohello IoT-központ"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub, iot-átjáró első lépések hello az eszközök internetes hálózata, iot eszközkészlet"
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
ms.openlocfilehash: d8e4057e7774e43c069dd3f2f2e03f098c1ac844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a><span data-ttu-id="1c499-104">Az IoT átjáró Starter Kit egy SensorTag az első lépései</span><span class="sxs-lookup"><span data-stu-id="1c499-104">Get started with IoT Gateway Starter Kit with a SensorTag</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1c499-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="1c499-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="1c499-106">Szimulált eszköz</span><span class="sxs-lookup"><span data-stu-id="1c499-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="1c499-107">Ebben az oktatóanyagban kezdődik, majd hello használatának alapjait tanulási [IoT átjáró Starter Kit](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="1c499-107">In this tutorial, you begin by learning hello basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="1c499-108">Az Intel NUC szél folyó Linux és hello futtató működik [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span><span class="sxs-lookup"><span data-stu-id="1c499-108">You will be working with Intel NUC that's running Wind River Linux and hello [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span></span> <span data-ttu-id="1c499-109">Megtudhatja, hogyan tooseamleesly kapcsolódnak az eszközök toohello felhőalapú Azure IoT Hub használatával.</span><span class="sxs-lookup"><span data-stu-id="1c499-109">You will learn how tooseamleesly connect your devices toohello cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="1c499-110">**Még nem rendelkezik a csomag?:** kattintson [Itt](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="1c499-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="1c499-111">**Nem rendelkezik egy SensorTag?:** [indítsa el a szimulált eszköz](iot-hub-gateway-kit-c-sim-get-started.md) vagy [egy SensorTag megvásárlása](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span><span class="sxs-lookup"><span data-stu-id="1c499-111">**Don't have a SensorTag?:** [Start with a simulated device](iot-hub-gateway-kit-c-sim-get-started.md) or [buy a SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="1c499-112">1. lecke: A NUC konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c499-112">Lesson 1: Configure your NUC</span></span>
![Lesson1 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

<span data-ttu-id="1c499-114">Ez a lecke hello Azure IoT átjáróként Kit Intel NUC (következő egység a Informatika) beállítása, NUC hello Azure IoT biztonsági csomag telepítését, és futtatni egy minta app tooverify hello átjáró funkciót.</span><span class="sxs-lookup"><span data-stu-id="1c499-114">In this lesson, you set up Intel NUC (Next Unit of Computing) in hello Kit as an Azure IoT gateway, install hello Azure IoT Edge package on NUC, and run a sample app tooverify hello gateway functionality.</span></span>

<span data-ttu-id="1c499-115">*Becsült idő toocomplete: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="1c499-115">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="1c499-116">Nyissa meg túl[Intel NUC IoT átjáróként beállítása](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="1c499-116">Go too[Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="1c499-117">2. lecke: Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c499-117">Lesson 2: Create your IoT Hub</span></span>
![Lesson2 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

<span data-ttu-id="1c499-119">Ez a lecke hello eszközök és szoftverek telepítéséhez a gazdaszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="1c499-119">In this lesson, you install hello tools and software on your host computer.</span></span> <span data-ttu-id="1c499-120">Ezután a ingyenes Azure-fiók létrehozásához, kiépíteni az Azure IoT hub és az első eszköz hello IoT hub létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1c499-120">Then you create your free Azure account, provision your Azure IoT hub and create your first device in hello IoT hub.</span></span>

<span data-ttu-id="1c499-121">Teljes lecke 1 Ez a lecke megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="1c499-121">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-hello-tools"></a><span data-ttu-id="1c499-122">Hello eszközök beszerzése</span><span class="sxs-lookup"><span data-stu-id="1c499-122">Get hello tools</span></span>
<span data-ttu-id="1c499-123">A gazdaszámítógép hello eszközök és szoftverek telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1c499-123">Install hello tools and software on your host computer.</span></span>

<span data-ttu-id="1c499-124">*Becsült idő toocomplete: 20 perc*</span><span class="sxs-lookup"><span data-stu-id="1c499-124">*Estimated time toocomplete: 20 minutes*</span></span>

<span data-ttu-id="1c499-125">Nyissa meg túl[hello eszközök beszerzése](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="1c499-125">Go too[Get hello tools](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="1c499-126">Létrehoz egy IoT-központot, és regisztrálja az eszközt</span><span class="sxs-lookup"><span data-stu-id="1c499-126">Create an IoT hub and register your device</span></span>
<span data-ttu-id="1c499-127">Az erőforráscsoport létrehozása, az első Azure IoT hub kiépíteni, és adja hozzá az első eszköz toohello IoT hub hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="1c499-127">Create your resource group, provision your first Azure IoT hub, and add your first device toohello IoT hub using hello Azure CLI.</span></span>

<span data-ttu-id="1c499-128">*Becsült idő toocomplete: 10 perc*</span><span class="sxs-lookup"><span data-stu-id="1c499-128">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="1c499-129">Nyissa meg túl[létrehoz egy IoT-központot, és regisztrálja az eszközt](iot-hub-gateway-kit-c-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="1c499-129">Go too[Create an IoT hub and register your device](iot-hub-gateway-kit-c-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="1c499-130">3. lecke: SensorTag érkező üzenetek fogadására és üzenetek olvasni az IoT hub</span><span class="sxs-lookup"><span data-stu-id="1c499-130">Lesson 3: Receive messages from SensorTag and read messages from your IoT hub</span></span>
<span data-ttu-id="1c499-131">Ez a lecke szüksége lesz BLA mintaalkalmazás végrehajtásának és parancsfájlok tooautomate hello konfigurációs az átjárót.</span><span class="sxs-lookup"><span data-stu-id="1c499-131">In this lesson, you will use scripts tooautomate hello configuration and execution of a BLE sample application in your gateway.</span></span> <span data-ttu-id="1c499-132">Az ilyen alkalmazások modulok tooaggregate és átalakítási adatok gyűjteménye, parancsok, vagy műveletet hajt végre tetszőleges számú kapcsolódó feladat.</span><span class="sxs-lookup"><span data-stu-id="1c499-132">Such applications use a collection of modules tooaggregate and transform data, process commands, or perform any number of related tasks.</span></span> <span data-ttu-id="1c499-133">Modulok egy üzenet közvetítőn keresztül kommunikáljanak egymással.</span><span class="sxs-lookup"><span data-stu-id="1c499-133">Modules communicate with one another via a message broker.</span></span> <span data-ttu-id="1c499-134">hello mintaalkalmazás BLA modul, ezért az IoT hub modul rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1c499-134">hello sample application has a BLE module and an IoT hub module.</span></span> <span data-ttu-id="1c499-135">hello BLA modul adatokat fogad az int SensorTag.</span><span class="sxs-lookup"><span data-stu-id="1c499-135">hello BLE module receives data from BLE SensorTag.</span></span> <span data-ttu-id="1c499-136">hello IoT hub modul csomagok hello adatokat fogad, és elküldi azt az Azure IoT Edge tooyour IoT-központ hello átjáró keretrendszere keresztül.</span><span class="sxs-lookup"><span data-stu-id="1c499-136">hello IoT hub module packages hello data received and sends it tooyour IoT hub through hello gateway framework provided in Azure IoT Edge.</span></span>

![Diagram. a végpont 3. rész](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-hello-ble-sample-app"></a><span data-ttu-id="1c499-138">Konfigurálja és hello BLA mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="1c499-138">Configure and run hello BLE sample app</span></span>
<span data-ttu-id="1c499-139">Összekapcsolhatók hello SensorTag és az átjárót.</span><span class="sxs-lookup"><span data-stu-id="1c499-139">Set up hello connectivity between SensorTag and your gateway.</span></span> <span data-ttu-id="1c499-140">Majd hello konfigurálás befejezése, és futtassa hello BLA mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1c499-140">Then finish hello configuration and run hello BLE sample application.</span></span>

<span data-ttu-id="1c499-141">*Becsült idő toocomplete: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="1c499-141">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="1c499-142">Nyissa meg túl[mintaalkalmazás futtatása hello BLA és konfigurálása](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span><span class="sxs-lookup"><span data-stu-id="1c499-142">Go too[Configure and run hello BLE sample app](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="1c499-143">Az IoT hub olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="1c499-143">Read messages from your IoT hub</span></span>
<span data-ttu-id="1c499-144">Futtassa mintakód a gazdagépen futó számítógép tooread üzenetet az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1c499-144">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="1c499-145">*Becsült idő toocomplete: 15 perc*</span><span class="sxs-lookup"><span data-stu-id="1c499-145">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="1c499-146">Nyissa meg túl[üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="1c499-146">Go too[Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-tooazure-table-storage"></a><span data-ttu-id="1c499-147">4. lecke: Üzenetek tooAzure a Table storage mentése</span><span class="sxs-lookup"><span data-stu-id="1c499-147">Lesson 4: Save messages tooAzure Table storage</span></span>
<span data-ttu-id="1c499-148">Hozzon létre, hogy a bejövő üzenetek lekérése az IoT hub, majd beírja őket tooAzure a Table storage Azure függvény alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="1c499-148">Create an Azure function app that gets incoming messages from your IoT hub and writes them tooAzure Table storage.</span></span>

![Rész 4 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="1c499-150">Egy Azure függvény app és az Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c499-150">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="1c499-151">Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="1c499-151">Use an Azure Resource Manager template toocreate an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="1c499-152">*Becsült idő toocomplete: 10 perc*</span><span class="sxs-lookup"><span data-stu-id="1c499-152">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="1c499-153">Nyissa meg túl[Azure függvény app és Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="1c499-153">Go too[Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="1c499-154">Azure Table storage-ban tárolt üzenetek olvasása</span><span class="sxs-lookup"><span data-stu-id="1c499-154">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="1c499-155">Átjáró felhő köszönőüzenetei, az oktatóprogram tooAzure a Table storage figyelése</span><span class="sxs-lookup"><span data-stu-id="1c499-155">Monitor hello gateway-to-cloud messages as they are written tooAzure Table storage.</span></span>

<span data-ttu-id="1c499-156">*Becsült idő toocomplete: 5 perc*</span><span class="sxs-lookup"><span data-stu-id="1c499-156">*Estimated time toocomplete: 5 minutes*</span></span>

<span data-ttu-id="1c499-157">Nyissa meg túl[Azure Table storage-ban tárolt üzenetek olvasása](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="1c499-157">Go too[Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1c499-158">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="1c499-158">Troubleshooting</span></span>
<span data-ttu-id="1c499-159">Ha bármilyen probléma során hello során tapasztalatokat, keressen megoldásokat hello [hibaelhárítás](iot-hub-gateway-kit-c-troubleshooting.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1c499-159">If you have any problems during hello lessons, look for solutions in hello [Troubleshooting](iot-hub-gateway-kit-c-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="1c499-160">További részletek</span><span class="sxs-lookup"><span data-stu-id="1c499-160">Explore more</span></span>
<span data-ttu-id="1c499-161">A Microsoft hello [Intel IoT-átjáró csomag fejlesztői zóna](http://software.intel.com/iot/microsoft-azure) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="1c499-161">Visit hello [Intel IoT Gateway Kit developer zone](http://software.intel.com/iot/microsoft-azure) toolearn more.</span></span>
