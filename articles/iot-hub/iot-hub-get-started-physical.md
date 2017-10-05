---
title: "Ismerkedés az Azure IoT-központ fizikai eszközök csatlakozás |} Microsoft Docs"
description: "Útmutató: Azure IoT-központ fizikai eszközöket és a modulok csatlakozni. Az eszközök küldhet az IoT-központ és az IoT-központ telemetriai figyelheti és az eszközök kezeléséhez."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: "az Azure iot hub oktatóanyag"
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: f4128b6b049aa876e170c56dcf2e40720644dc3d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a><span data-ttu-id="e381f-105">Az Azure IoT-központ fizikai eszközök oktatóanyagok az első lépései</span><span class="sxs-lookup"><span data-stu-id="e381f-105">Azure IoT Hub get started with physical devices tutorials</span></span>

<span data-ttu-id="e381f-106">Ezek az oktatóanyagok bemutatják a Azure IoT-központ és az eszköz SDK-k.</span><span class="sxs-lookup"><span data-stu-id="e381f-106">These tutorials introduce you to Azure IoT Hub and the device SDKs.</span></span> <span data-ttu-id="e381f-107">Az oktatóanyagok általános IoT-forgatókönyvek esetén az IoT-központ funkcióinak bemutatása foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="e381f-107">The tutorials cover common IoT scenarios to demonstrate the capabilities of IoT Hub.</span></span> <span data-ttu-id="e381f-108">Az oktatóanyagok bemutatják, hogyan kombinálhatók az IoT-központ más Azure-szolgáltatások és eszközök nagyobb teljesítményű az IoT-megoldások létrehozásához is.</span><span class="sxs-lookup"><span data-stu-id="e381f-108">The tutorials also illustrate how to combine IoT Hub with other Azure services and tools to build more powerful IoT solutions.</span></span> <span data-ttu-id="e381f-109">A következő táblázatban található az oktatóanyagok bemutatják a fizikai az IoT-eszközök létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e381f-109">The tutorials listed in the following table show you how to create physical IoT devices.</span></span>

| <span data-ttu-id="e381f-110">IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="e381f-110">IoT device</span></span>                       | <span data-ttu-id="e381f-111">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="e381f-111">Programming language</span></span> |
|---------------------------------|----------------------|
| <span data-ttu-id="e381f-112">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="e381f-112">Raspberry Pi</span></span>                    | <span data-ttu-id="e381f-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="e381f-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="e381f-114">Az IoT-DevKit</span><span class="sxs-lookup"><span data-stu-id="e381f-114">IoT DevKit</span></span>                      | <span data-ttu-id="e381f-115">[A VSCode Arduino][DevKit]</span><span class="sxs-lookup"><span data-stu-id="e381f-115">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="e381f-116">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="e381f-116">Intel Edison</span></span>                    | <span data-ttu-id="e381f-117">[NODE.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="e381f-117">[Node.js][Ed_Nd], [C][Ed_C]</span></span>           |
| <span data-ttu-id="e381f-118">Adafruit lágyított HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="e381f-118">Adafruit Feather HUZZAH ESP8266</span></span> | <span data-ttu-id="e381f-119">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="e381f-119">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="e381f-120">Sparkfun ESP8266 dolog fejlesztői</span><span class="sxs-lookup"><span data-stu-id="e381f-120">Sparkfun ESP8266 Thing Dev</span></span>      | <span data-ttu-id="e381f-121">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="e381f-121">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="e381f-122">Adafruit lágyított M0</span><span class="sxs-lookup"><span data-stu-id="e381f-122">Adafruit Feather M0</span></span>             | <span data-ttu-id="e381f-123">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="e381f-123">[Arduino][M0_Ard]</span></span>              |

<span data-ttu-id="e381f-124">Emellett az egy IoT peremhálózati átjáró segítségével engedélyezheti az eszközök csatlakoztatása az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e381f-124">In addition, you can use an IoT Edge gateway to enable devices to connect to your IoT hub.</span></span>

| <span data-ttu-id="e381f-125">Átjáróeszköz</span><span class="sxs-lookup"><span data-stu-id="e381f-125">Gateway device</span></span>               | <span data-ttu-id="e381f-126">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="e381f-126">Programming language</span></span> | <span data-ttu-id="e381f-127">Platform</span><span class="sxs-lookup"><span data-stu-id="e381f-127">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="e381f-128">Intel NUC (modell DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="e381f-128">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="e381f-129">C#</span><span class="sxs-lookup"><span data-stu-id="e381f-129">C</span></span>                    | <span data-ttu-id="e381f-130">[Szél folyó Linux][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="e381f-130">[Wind River Linux][NUC_Lnx]</span></span> |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]


[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
