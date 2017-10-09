---
title: "az eszközök internetes hálózatát (IoT Suite) aaaAzure megoldások |} Microsoft Docs"
description: "Egy minta IoT-megoldásarchitektúra és toodevices, kapcsolódására áttekintése hello Azure IoT-központ szolgáltatás, Azure IoT eszközoldali SDK-k, Azure IoT szolgáltatás SDK-k és más Azure-szolgáltatásokkal."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 2d934e3f988c530de6a242869c021712d2aa1576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a><span data-ttu-id="59654-103">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59654-103">Next steps</span></span>

<span data-ttu-id="59654-104">Az Azure IoT Hub olyan Azure-szolgáltatás, amely biztonságos és megbízható kétirányú kommunikációt tesz lehetővé a megoldás háttérrendszere és több millió eszköz között.</span><span class="sxs-lookup"><span data-stu-id="59654-104">Azure IoT Hub is an Azure service that enables secure and reliable bi-directional communications between your solution back end and millions of devices.</span></span> <span data-ttu-id="59654-105">Ez lehetővé teszi, hogy a hello megoldás háttérrendszeréhez:</span><span class="sxs-lookup"><span data-stu-id="59654-105">It enables hello solution back end to:</span></span>

* <span data-ttu-id="59654-106">Az eszközök telemetriai adatainak lekérése távolról.</span><span class="sxs-lookup"><span data-stu-id="59654-106">Receive telemetry at scale from your devices.</span></span>
* <span data-ttu-id="59654-107">Az eszközök tooa adatfolyam eseményfeldolgozóhoz útvonal adatait.</span><span class="sxs-lookup"><span data-stu-id="59654-107">Route data from your devices tooa stream event processor.</span></span>
* <span data-ttu-id="59654-108">Fájlfeltöltés az eszközökről.</span><span class="sxs-lookup"><span data-stu-id="59654-108">Receive file uploads from devices.</span></span>
* <span data-ttu-id="59654-109">Felhő-eszközre küldött üzenetek küldése toospecific eszközök.</span><span class="sxs-lookup"><span data-stu-id="59654-109">Send cloud-to-device messages toospecific devices.</span></span>

<span data-ttu-id="59654-110">Az IoT-központ tooimplement saját megoldás háttérrendszere is használhatja.</span><span class="sxs-lookup"><span data-stu-id="59654-110">You can use IoT Hub tooimplement your own solution back end.</span></span> <span data-ttu-id="59654-111">Az IoT-központ tartalmazza továbbá egy identitás használt beállításjegyzék tooprovision eszközök, a hitelesítő adatokat és a jogok tooconnect toohello IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="59654-111">In addition, IoT Hub includes an identity registry used tooprovision devices, their security credentials, and their rights tooconnect toohello IoT hub.</span></span> <span data-ttu-id="59654-112">toolearn IoT Hub kapcsolatos további információkért lásd: [Mi az az IoT-központ?] [lnk-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="59654-112">toolearn more about IoT Hub, see [What is IoT Hub?][lnk-iot-hub].</span></span>

<span data-ttu-id="59654-113">toolearn hogyan Azure IoT Hub szabványalapú kezelés lehetővé teszi, az Ön tooremotely kezelése, konfigurálása és az eszközök frissítését lásd [IoT-központ az eszközkezelés áttekintése][lnk-device-management].</span><span class="sxs-lookup"><span data-stu-id="59654-113">toolearn how Azure IoT Hub enables standards-based device management for you tooremotely manage, configure, and update your devices, see [Overview of device management with IoT Hub][lnk-device-management].</span></span>

<span data-ttu-id="59654-114">tooimplement ügyfélalkalmazások számos különböző eszközplatformok hardverek és operációs rendszereket, hello Azure IoT-eszközök SDK-k is használhatja.</span><span class="sxs-lookup"><span data-stu-id="59654-114">tooimplement client applications on a wide variety of device hardware platforms and operating systems, you can use hello Azure IoT device SDKs.</span></span> <span data-ttu-id="59654-115">hello eszköz SDK-k, amelyek egyszerűbbé teszik a telemetriai adatok tooan IoT hub és fogadás felhő eszközre üzenetek küldése szalagtárak tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="59654-115">hello device SDKs include libraries that facilitate sending telemetry tooan IoT hub and receiving cloud-to-device messages.</span></span> <span data-ttu-id="59654-116">Ha hello eszközzel SDK-k, az IoT hubbal választhat több hálózati protokollok toocommunicate.</span><span class="sxs-lookup"><span data-stu-id="59654-116">When you use hello device SDKs, you can choose from several network protocols toocommunicate with IoT Hub.</span></span> <span data-ttu-id="59654-117">toolearn több, tekintse meg a hello [SDK-k eszközére vonatkozó információt][lnk-device-sdks].</span><span class="sxs-lookup"><span data-stu-id="59654-117">toolearn more, see hello [information about device SDKs][lnk-device-sdks].</span></span>

<span data-ttu-id="59654-118">Néhány kódot és az egyes minták futtatása elindult tooget lásd: hello [Ismerkedés az IoT-központ] [ lnk-getstarted] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="59654-118">tooget started writing some code and running some samples, see hello [Get started with IoT Hub][lnk-getstarted] tutorial.</span></span>

<span data-ttu-id="59654-119">Az [Azure IoT Suite][lnk-iot-suite] is hasznos lehet, amely egy előre konfigurált megoldásokat tartalmazó gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="59654-119">You may also be interested in [Azure IoT Suite][lnk-iot-suite], which is a collection of preconfigured solutions.</span></span> <span data-ttu-id="59654-120">Az IoT Suite lehetővé teszi, hogy a tooget, gyorsan, és a méret IoT projektek tooaddress általános IoT-forgatókönyvek esetén – például távoli figyelés, az eszközök kezelése és a prediktív karbantartási.</span><span class="sxs-lookup"><span data-stu-id="59654-120">IoT Suite enables you tooget started quickly and scale IoT projects tooaddress common IoT scenarios--such as remote monitoring, asset management, and predictive maintenance.</span></span>

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
