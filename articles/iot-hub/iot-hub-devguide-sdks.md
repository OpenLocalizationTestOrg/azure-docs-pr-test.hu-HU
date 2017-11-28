---
title: aaaUnderstand hello Azure IoT SDK |} Microsoft Docs
description: "Fejlesztői útmutató - információk és hivatkozások toohello különböző Azure IoT eszköz és a szolgáltatás SDK-k használható toobuild eszköz és háttér-alkalmazások."
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
ms.openlocfilehash: e319451ca97876666e1c011ee0e1a73d0a0f1ed1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-azure-iot-sdks"></a><span data-ttu-id="8ed6c-103">Megismeréséhez és használatához Azure IoT SDK-k</span><span class="sxs-lookup"><span data-stu-id="8ed6c-103">Understand and use Azure IoT SDKs</span></span>

<span data-ttu-id="8ed6c-104">Az IoT-központ való munkához SDK három kategóriába sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="8ed6c-104">There are three categories of SDK for working with IoT Hub:</span></span>

* <span data-ttu-id="8ed6c-105">**Eszközoldali SDK-k** lehetővé teszik az IoT-eszközök futó toobuild alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-105">**Device SDKs** enable you toobuild apps that run on your IoT devices.</span></span> <span data-ttu-id="8ed6c-106">Ezek az alkalmazások telemetriai tooyour IoT-központ küldeni, és opcionálisan az IoT hub-üzeneteket fogadjon.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-106">These apps send telemetry tooyour IoT hub, and optionally receive messages from your IoT hub.</span></span>

* <span data-ttu-id="8ed6c-107">**Szolgáltatás SDK-k** meg toomanage engedélyezése az IoT hub, és opcionálisan tooyour az IoT-eszközök az üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-107">**Service SDKs** enable you toomanage your IoT hub, and optionally send messages tooyour IoT devices.</span></span>

* <span data-ttu-id="8ed6c-108">**Az Azure IoT peremhálózati** lehetővé teszi a toobuild átjárók tooenable eszközök hello támogatott protokollok, amelyek nem használnak, vagy ha tooprocess üzenetek hello oldal van szükség.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-108">**Azure IoT Edge** enables you toobuild gateways tooenable devices that don't use one of hello supported protocols, or when you need tooprocess messages on hello edge.</span></span>

<span data-ttu-id="8ed6c-109">SDK-k vannak megadva toosupport több programozási nyelv.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-109">SDKs are provided toosupport multiple programming languages.</span></span>

## <a name="azure-iot-device-sdks"></a><span data-ttu-id="8ed6c-110">Az Azure SDK-k IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="8ed6c-110">Azure IoT device SDKs</span></span>

<span data-ttu-id="8ed6c-111">Microsoft Azure IoT eszközoldali SDK-k hello kódot tartalmaznak, amely elősegíti a épület eszközök és alkalmazások, amelyek tooand Azure IoT-központ szolgáltatás által kezelt.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-111">hello Microsoft Azure IoT device SDKs contain code that facilitates building devices and applications that connect tooand are managed by Azure IoT Hub services.</span></span>

<span data-ttu-id="8ed6c-112">hello következő Azure IoT eszközoldali SDK-k a Githubról elérhető toodownload:</span><span class="sxs-lookup"><span data-stu-id="8ed6c-112">hello following Azure IoT device SDKs are available toodownload from GitHub:</span></span>

* <span data-ttu-id="8ed6c-113">[Az Azure IoT-eszközök SDK c] [ lnk-c-device-sdk] írt adatok hordozhatóságára és széles körű platformkompatibilitásra ANSI C (C99).</span><span class="sxs-lookup"><span data-stu-id="8ed6c-113">[Azure IoT device SDK for C][lnk-c-device-sdk] written in ANSI C (C99) for portability and broad platform compatibility.</span></span> <span data-ttu-id="8ed6c-114">Nincsenek C, két eszköz ügyféloldali kódtáraknál hello alacsony szintű **iothub_client** és hello **szerializáló**.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-114">There are two device client libraries for C, hello low-level **iothub_client** and hello **serializer**.</span></span>
* <span data-ttu-id="8ed6c-115">[Az Azure IoT-eszközök SDK a .NET-hez][lnk-dotnet-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-115">[Azure IoT device SDK for .NET][lnk-dotnet-device-sdk]</span></span>
* <span data-ttu-id="8ed6c-116">[Az Azure IoT-eszközök SDK Java][lnk-java-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-116">[Azure IoT device SDK for Java][lnk-java-device-sdk]</span></span>
* <span data-ttu-id="8ed6c-117">[Az Azure IoT-eszközök SDK for Node.js][lnk-node-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-117">[Azure IoT device SDK for Node.js][lnk-node-device-sdk]</span></span>
* <span data-ttu-id="8ed6c-118">[Az Azure IoT-eszközök SDK Python-hez][lnk-python-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-118">[Azure IoT device SDK for Python][lnk-python-device-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="8ed6c-119">Lásd: hello információs fájl hello GitHub-adattárak az információt a nyelvi és a platform-specifikus csomag kezelők tooinstall bináris fájljait és a függőségek a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-119">See hello readme files in hello GitHub repositories for information about using language and platform-specific package managers tooinstall binaries and dependencies on your development machine.</span></span>
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a><span data-ttu-id="8ed6c-120">Az operációs rendszer platform és hardver kompatibilitása</span><span class="sxs-lookup"><span data-stu-id="8ed6c-120">OS platform and hardware compatibility</span></span>

<span data-ttu-id="8ed6c-121">Bizonyos hardvereszközök kompatibilisek SDK kapcsolatos további információkért lásd: hello [Azure IoT eszköz katalógus hitelesített][lnk-certified].</span><span class="sxs-lookup"><span data-stu-id="8ed6c-121">For more information about SDK compatibility with specific hardware devices, see hello [Azure Certified for IoT device catalog][lnk-certified].</span></span>

## <a name="azure-iot-service-sdks"></a><span data-ttu-id="8ed6c-122">Az Azure IoT szolgáltatás SDK-k</span><span class="sxs-lookup"><span data-stu-id="8ed6c-122">Azure IoT service SDKs</span></span>

<span data-ttu-id="8ed6c-123">hello Azure IoT szolgáltatás SDK-k alkalmazások közvetlenül kommunikáló IoT Hub toomanage eszközöket és a biztonsági kódot toofacilitate tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-123">hello Azure IoT service SDKs contain code toofacilitate building applications that interact directly with IoT Hub toomanage devices and security.</span></span>

<span data-ttu-id="8ed6c-124">hello következő Azure IoT szolgáltatás SDK-k a Githubról elérhető toodownload:</span><span class="sxs-lookup"><span data-stu-id="8ed6c-124">hello following Azure IoT service SDKs are available toodownload from GitHub:</span></span>

* <span data-ttu-id="8ed6c-125">[Az Azure IoT szolgáltatás SDK a .NET-hez][lnk-dotnet-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-125">[Azure IoT service SDK for .NET][lnk-dotnet-service-sdk]</span></span>
* <span data-ttu-id="8ed6c-126">[Az Azure IoT szolgáltatás SDK for Node.js][lnk-node-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-126">[Azure IoT service SDK for Node.js][lnk-node-service-sdk]</span></span>
* <span data-ttu-id="8ed6c-127">[Azure IoT-szolgáltatás SDK Java][lnk-java-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-127">[Azure IoT service SDK for Java][lnk-java-service-sdk]</span></span>
* <span data-ttu-id="8ed6c-128">[Azure IoT-szolgáltatás SDK Python][lnk-python-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-128">[Azure IoT service SDK for Python][lnk-python-service-sdk]</span></span>
* <span data-ttu-id="8ed6c-129">[Az Azure IoT szolgáltatás SDK C-hez][lnk-c-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-129">[Azure IoT service SDK for C][lnk-c-service-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="8ed6c-130">Lásd: hello információs fájl hello GitHub-adattárak az információt a nyelvi és a platform-specifikus csomag kezelők tooinstall bináris fájljait és a függőségek a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-130">See hello readme files in hello GitHub repositories for information about using language and platform-specific package managers tooinstall binaries and dependencies on your development machine.</span></span>

## <a name="azure-iot-edge"></a><span data-ttu-id="8ed6c-131">Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="8ed6c-131">Azure IoT Edge</span></span>

<span data-ttu-id="8ed6c-132">Az Azure IoT peremhálózati hello infrastruktúra és a modulok toocreate IoT átjáró megoldásait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-132">Azure IoT Edge contains hello infrastructure and modules toocreate IoT gateway solutions.</span></span> <span data-ttu-id="8ed6c-133">Az IoT-Edge toocreate átjárók szabott tooany végpont forgatókönyvet kibővítheti a.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-133">You can extend IoT Edge toocreate gateways tailored tooany end-to-end scenario.</span></span>

<span data-ttu-id="8ed6c-134">Letöltheti a [Azure IoT peremhálózati] [ lnk-iot-edge] a Githubról.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-134">You can download [Azure IoT Edge][lnk-iot-edge] from GitHub.</span></span>

## <a name="online-api-reference-documentation"></a><span data-ttu-id="8ed6c-135">Online API referenciadokumentációt tartalmaz</span><span class="sxs-lookup"><span data-stu-id="8ed6c-135">Online API reference documentation</span></span>

<span data-ttu-id="8ed6c-136">hello alábbi lista hivatkozások tooonline API referenciadokumentációt tartalmaz Azure IoT eszköz, a szolgáltatás és az átjáró szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="8ed6c-136">hello following list contains links tooonline API reference documentation for Azure IoT device, service, and gateway libraries:</span></span>

* <span data-ttu-id="8ed6c-137">[Az eszközök internetes hálózata (IoT) .NET][lnk-dotnet-ref]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-137">[Internet of Things (IoT) .NET][lnk-dotnet-ref]</span></span>
* <span data-ttu-id="8ed6c-138">[Az IoT Hub REST][lnk-rest-ref]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-138">[IoT Hub REST][lnk-rest-ref]</span></span>
* <span data-ttu-id="8ed6c-139">[Az Azure IoT-eszközök SDK C-hez][lnk-c-ref]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-139">[Azure IoT device SDK for C][lnk-c-ref]</span></span>
* <span data-ttu-id="8ed6c-140">[Az Azure IoT-eszközök SDK Java][lnk-java-ref]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-140">[Azure IoT device SDK for Java][lnk-java-ref]</span></span>
* <span data-ttu-id="8ed6c-141">[Azure IoT-szolgáltatás SDK Java][lnk-java-service-ref]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-141">[Azure IoT service SDK for Java][lnk-java-service-ref]</span></span>
* <span data-ttu-id="8ed6c-142">[Az Azure IoT-eszközök SDK for Node.js][lnk-node-ref]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-142">[Azure IoT device SDK for Node.js][lnk-node-ref]</span></span>
* <span data-ttu-id="8ed6c-143">[Az Azure IoT szolgáltatás SDK for Node.js][lnk-node-service-ref]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-143">[Azure IoT service SDK for Node.js][lnk-node-service-ref]</span></span>
* <span data-ttu-id="8ed6c-144">[Az Azure IoT él][lnk-gateway-ref]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-144">[Azure IoT Edge][lnk-gateway-ref]</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ed6c-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ed6c-145">Next steps</span></span>

<span data-ttu-id="8ed6c-146">Az IoT Hub fejlesztői útmutató egyéb témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8ed6c-146">Other reference topics in this IoT Hub developer guide include:</span></span>

* <span data-ttu-id="8ed6c-147">[IoT-központok végpontjai][lnk-devguide-endpoints]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-147">[IoT Hub endpoints][lnk-devguide-endpoints]</span></span>
* <span data-ttu-id="8ed6c-148">[Az IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás][lnk-devguide-query]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-148">[IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query]</span></span>
* <span data-ttu-id="8ed6c-149">[Kvóták és sávszélesség-szabályozás][lnk-devguide-quotas]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-149">[Quotas and throttling][lnk-devguide-quotas]</span></span>
* <span data-ttu-id="8ed6c-150">[Az IoT Hub MQTT támogatása][lnk-devguide-mqtt]</span><span class="sxs-lookup"><span data-stu-id="8ed6c-150">[IoT Hub MQTT support][lnk-devguide-mqtt]</span></span>

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
