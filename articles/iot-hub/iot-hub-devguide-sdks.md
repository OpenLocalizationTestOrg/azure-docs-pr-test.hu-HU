---
title: "Az Azure IoT SDK-k ismertetése |} Microsoft Docs"
description: "Fejlesztői útmutató - információk és hivatkozások a különböző Azure IoT eszköz és a szolgáltatás SDK-k, amelyek segítségével eszközön futó alkalmazások és a háttér-alkalmazásokat hozhat létre."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bcbf4b9633f58293edb19aeb33dec6602ac4ec8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="understand-and-use-azure-iot-sdks"></a><span data-ttu-id="27940-103">Megismeréséhez és használatához Azure IoT SDK-k</span><span class="sxs-lookup"><span data-stu-id="27940-103">Understand and use Azure IoT SDKs</span></span>

<span data-ttu-id="27940-104">Az IoT-központ való munkához SDK három kategóriába sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="27940-104">There are three categories of SDK for working with IoT Hub:</span></span>

* <span data-ttu-id="27940-105">**Eszközoldali SDK-k** lehetővé teszik az IoT-eszközök futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="27940-105">**Device SDKs** enable you to build apps that run on your IoT devices.</span></span> <span data-ttu-id="27940-106">Ezek az alkalmazások telemetriai adatokat küldeni az IoT hub, és opcionálisan az IoT hub-üzeneteket fogadjon.</span><span class="sxs-lookup"><span data-stu-id="27940-106">These apps send telemetry to your IoT hub, and optionally receive messages from your IoT hub.</span></span>

* <span data-ttu-id="27940-107">**Szolgáltatás SDK-k** lehetővé teszik az IoT hub kezelését, és opcionálisan az üzenetek küldése az IoT-eszközök.</span><span class="sxs-lookup"><span data-stu-id="27940-107">**Service SDKs** enable you to manage your IoT hub, and optionally send messages to your IoT devices.</span></span>

* <span data-ttu-id="27940-108">**Az Azure IoT peremhálózati** ahhoz, hogy az eszközök, amelyek a támogatott protokollok, vagy nem kell tennie a peremhálózaton lévő üzenetek feldolgozásához átjárók létrehozását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="27940-108">**Azure IoT Edge** enables you to build gateways to enable devices that don't use one of the supported protocols, or when you need to process messages on the edge.</span></span>

<span data-ttu-id="27940-109">SDK-k több programozási nyelv támogatása biztosított.</span><span class="sxs-lookup"><span data-stu-id="27940-109">SDKs are provided to support multiple programming languages.</span></span>

## <a name="azure-iot-device-sdks"></a><span data-ttu-id="27940-110">Az Azure SDK-k IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="27940-110">Azure IoT device SDKs</span></span>

<span data-ttu-id="27940-111">A Microsoft Azure IoT-eszközök SDK-k, amely elősegíti a épület eszközök és alkalmazások és szolgáltatások Azure IoT-központ által kezelt-kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="27940-111">The Microsoft Azure IoT device SDKs contain code that facilitates building devices and applications that connect to and are managed by Azure IoT Hub services.</span></span>

<span data-ttu-id="27940-112">A következő Azure IoT-eszközök SDK-k is letölthetők a Githubról:</span><span class="sxs-lookup"><span data-stu-id="27940-112">The following Azure IoT device SDKs are available to download from GitHub:</span></span>

* <span data-ttu-id="27940-113">[Az Azure IoT-eszközök SDK c] [ lnk-c-device-sdk] írt adatok hordozhatóságára és széles körű platformkompatibilitásra ANSI C (C99).</span><span class="sxs-lookup"><span data-stu-id="27940-113">[Azure IoT device SDK for C][lnk-c-device-sdk] written in ANSI C (C99) for portability and broad platform compatibility.</span></span> <span data-ttu-id="27940-114">Nincsenek a két eszköz ügyféloldali kódtáraknál c, az alacsony szintű **iothub_client** és a **szerializáló**.</span><span class="sxs-lookup"><span data-stu-id="27940-114">There are two device client libraries for C, the low-level **iothub_client** and the **serializer**.</span></span>
* <span data-ttu-id="27940-115">[Az Azure IoT-eszközök SDK a .NET-hez][lnk-dotnet-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-115">[Azure IoT device SDK for .NET][lnk-dotnet-device-sdk]</span></span>
* <span data-ttu-id="27940-116">[Az Azure IoT-eszközök SDK Java][lnk-java-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-116">[Azure IoT device SDK for Java][lnk-java-device-sdk]</span></span>
* <span data-ttu-id="27940-117">[Az Azure IoT-eszközök SDK for Node.js][lnk-node-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-117">[Azure IoT device SDK for Node.js][lnk-node-device-sdk]</span></span>
* <span data-ttu-id="27940-118">[Az Azure IoT-eszközök SDK Python-hez][lnk-python-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-118">[Azure IoT device SDK for Python][lnk-python-device-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="27940-119">Tekintse meg a GitHub-adattárak readme fájljaiban találhat használatáról nyelvet és a platform-specifikus csomag kezelők bináris fájljait és a függőségek telepítése a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="27940-119">See the readme files in the GitHub repositories for information about using language and platform-specific package managers to install binaries and dependencies on your development machine.</span></span>
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a><span data-ttu-id="27940-120">Az operációs rendszer platform és hardver kompatibilitása</span><span class="sxs-lookup"><span data-stu-id="27940-120">OS platform and hardware compatibility</span></span>

<span data-ttu-id="27940-121">Bizonyos hardvereszközök kompatibilisek SDK kapcsolatos további információkért tekintse meg a [Azure IoT eszköz katalógus hitelesített][lnk-certified].</span><span class="sxs-lookup"><span data-stu-id="27940-121">For more information about SDK compatibility with specific hardware devices, see the [Azure Certified for IoT device catalog][lnk-certified].</span></span>

## <a name="azure-iot-service-sdks"></a><span data-ttu-id="27940-122">Az Azure IoT szolgáltatás SDK-k</span><span class="sxs-lookup"><span data-stu-id="27940-122">Azure IoT service SDKs</span></span>

<span data-ttu-id="27940-123">Az Azure IoT szolgáltatás SDK-k lehetővé teszi az épület alkalmazásokat közvetlenül az IoT-központ kezelheti az eszközöket és a biztonsági kódot tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="27940-123">The Azure IoT service SDKs contain code to facilitate building applications that interact directly with IoT Hub to manage devices and security.</span></span>

<span data-ttu-id="27940-124">A következő Azure IoT-szolgáltatás SDK-k is letölthetők a Githubról:</span><span class="sxs-lookup"><span data-stu-id="27940-124">The following Azure IoT service SDKs are available to download from GitHub:</span></span>

* <span data-ttu-id="27940-125">[Az Azure IoT szolgáltatás SDK a .NET-hez][lnk-dotnet-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-125">[Azure IoT service SDK for .NET][lnk-dotnet-service-sdk]</span></span>
* <span data-ttu-id="27940-126">[Az Azure IoT szolgáltatás SDK for Node.js][lnk-node-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-126">[Azure IoT service SDK for Node.js][lnk-node-service-sdk]</span></span>
* <span data-ttu-id="27940-127">[Azure IoT-szolgáltatás SDK Java][lnk-java-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-127">[Azure IoT service SDK for Java][lnk-java-service-sdk]</span></span>
* <span data-ttu-id="27940-128">[Azure IoT-szolgáltatás SDK Python][lnk-python-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-128">[Azure IoT service SDK for Python][lnk-python-service-sdk]</span></span>
* <span data-ttu-id="27940-129">[Az Azure IoT szolgáltatás SDK C-hez][lnk-c-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="27940-129">[Azure IoT service SDK for C][lnk-c-service-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="27940-130">Tekintse meg a GitHub-adattárak readme fájljaiban találhat használatáról nyelvet és a platform-specifikus csomag kezelők bináris fájljait és a függőségek telepítése a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="27940-130">See the readme files in the GitHub repositories for information about using language and platform-specific package managers to install binaries and dependencies on your development machine.</span></span>

## <a name="azure-iot-edge"></a><span data-ttu-id="27940-131">Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="27940-131">Azure IoT Edge</span></span>

<span data-ttu-id="27940-132">Azure IoT peremhálózati infrastruktúra és az IoT-átjáró megoldások létrehozásához modulok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="27940-132">Azure IoT Edge contains the infrastructure and modules to create IoT gateway solutions.</span></span> <span data-ttu-id="27940-133">Bővítheti IoT peremhálózati olyan végpont forgatókönyv igazított átjárók létrehozására.</span><span class="sxs-lookup"><span data-stu-id="27940-133">You can extend IoT Edge to create gateways tailored to any end-to-end scenario.</span></span>

<span data-ttu-id="27940-134">Letöltheti a [Azure IoT peremhálózati] [ lnk-iot-edge] a Githubról.</span><span class="sxs-lookup"><span data-stu-id="27940-134">You can download [Azure IoT Edge][lnk-iot-edge] from GitHub.</span></span>

## <a name="online-api-reference-documentation"></a><span data-ttu-id="27940-135">Online API referenciadokumentációt tartalmaz</span><span class="sxs-lookup"><span data-stu-id="27940-135">Online API reference documentation</span></span>

<span data-ttu-id="27940-136">Az alábbi lista tartalmazza az Azure IoT eszköz, a szolgáltatás és az átjáró szalagtárak online API referenciadokumentációt tartalmaz hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="27940-136">The following list contains links to online API reference documentation for Azure IoT device, service, and gateway libraries:</span></span>

* <span data-ttu-id="27940-137">[Az eszközök internetes hálózata (IoT) .NET][lnk-dotnet-ref]</span><span class="sxs-lookup"><span data-stu-id="27940-137">[Internet of Things (IoT) .NET][lnk-dotnet-ref]</span></span>
* <span data-ttu-id="27940-138">[Az IoT Hub REST][lnk-rest-ref]</span><span class="sxs-lookup"><span data-stu-id="27940-138">[IoT Hub REST][lnk-rest-ref]</span></span>
* <span data-ttu-id="27940-139">[Az Azure IoT-eszközök SDK C-hez][lnk-c-ref]</span><span class="sxs-lookup"><span data-stu-id="27940-139">[Azure IoT device SDK for C][lnk-c-ref]</span></span>
* <span data-ttu-id="27940-140">[Az Azure IoT-eszközök SDK Java][lnk-java-ref]</span><span class="sxs-lookup"><span data-stu-id="27940-140">[Azure IoT device SDK for Java][lnk-java-ref]</span></span>
* <span data-ttu-id="27940-141">[Azure IoT-szolgáltatás SDK Java][lnk-java-service-ref]</span><span class="sxs-lookup"><span data-stu-id="27940-141">[Azure IoT service SDK for Java][lnk-java-service-ref]</span></span>
* <span data-ttu-id="27940-142">[Az Azure IoT-eszközök SDK for Node.js][lnk-node-ref]</span><span class="sxs-lookup"><span data-stu-id="27940-142">[Azure IoT device SDK for Node.js][lnk-node-ref]</span></span>
* <span data-ttu-id="27940-143">[Az Azure IoT szolgáltatás SDK for Node.js][lnk-node-service-ref]</span><span class="sxs-lookup"><span data-stu-id="27940-143">[Azure IoT service SDK for Node.js][lnk-node-service-ref]</span></span>
* <span data-ttu-id="27940-144">[Az Azure IoT él][lnk-gateway-ref]</span><span class="sxs-lookup"><span data-stu-id="27940-144">[Azure IoT Edge][lnk-gateway-ref]</span></span>

## <a name="next-steps"></a><span data-ttu-id="27940-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="27940-145">Next steps</span></span>

<span data-ttu-id="27940-146">Az IoT Hub fejlesztői útmutató egyéb témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="27940-146">Other reference topics in this IoT Hub developer guide include:</span></span>

* <span data-ttu-id="27940-147">[IoT-központok végpontjai][lnk-devguide-endpoints]</span><span class="sxs-lookup"><span data-stu-id="27940-147">[IoT Hub endpoints][lnk-devguide-endpoints]</span></span>
* <span data-ttu-id="27940-148">[Az IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás][lnk-devguide-query]</span><span class="sxs-lookup"><span data-stu-id="27940-148">[IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query]</span></span>
* <span data-ttu-id="27940-149">[Kvóták és sávszélesség-szabályozás][lnk-devguide-quotas]</span><span class="sxs-lookup"><span data-stu-id="27940-149">[Quotas and throttling][lnk-devguide-quotas]</span></span>
* <span data-ttu-id="27940-150">[Az IoT Hub MQTT támogatása][lnk-devguide-mqtt]</span><span class="sxs-lookup"><span data-stu-id="27940-150">[IoT Hub MQTT support][lnk-devguide-mqtt]</span></span>

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-c-service-sdk]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_service_client
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/service
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/service
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device
[lnk-python-service-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-iot-edge]: https://github.com/Azure/iot-edge

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth
[lnk-node-service-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-gateway-ref]: http://azure.github.io/iot-edge/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
