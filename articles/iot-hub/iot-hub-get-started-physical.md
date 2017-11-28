---
title: "Ismerkedés az IoT-központ fizikai eszközök tooAzure kapcsolódás |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect fizikai eszközöket és a modulok tooAzure IoT-központot. Az eszközök küldhet telemetriai tooIoT központ és az IoT-központ figyelheti és az eszközök kezeléséhez."
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
ms.openlocfilehash: 47ce289c438b2f495d499d724c38ddc4b3307425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a><span data-ttu-id="aca62-105">Az Azure IoT-központ fizikai eszközök oktatóanyagok az első lépései</span><span class="sxs-lookup"><span data-stu-id="aca62-105">Azure IoT Hub get started with physical devices tutorials</span></span>

<span data-ttu-id="aca62-106">Ezek az oktatóanyagok tooAzure IoT Hub és hello eszköz SDK-k bevezetése.</span><span class="sxs-lookup"><span data-stu-id="aca62-106">These tutorials introduce you tooAzure IoT Hub and hello device SDKs.</span></span> <span data-ttu-id="aca62-107">hello oktatóanyagok általános IoT forgatókönyvek toodemonstrate hello képességeket IoT hub foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="aca62-107">hello tutorials cover common IoT scenarios toodemonstrate hello capabilities of IoT Hub.</span></span> <span data-ttu-id="aca62-108">hello oktatóanyagokat is bemutatják, hogyan-toocombine más Azure IoT hubot szolgáltatásokat és eszközöket több toobuild hatékony IoT-megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="aca62-108">hello tutorials also illustrate how toocombine IoT Hub with other Azure services and tools toobuild more powerful IoT solutions.</span></span> <span data-ttu-id="aca62-109">hello felsorolt oktatóanyagok hello következő tábla megjelenítése, hogyan toocreate fizikai az IoT-eszközök.</span><span class="sxs-lookup"><span data-stu-id="aca62-109">hello tutorials listed in hello following table show you how toocreate physical IoT devices.</span></span>

| <span data-ttu-id="aca62-110">IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="aca62-110">IoT device</span></span>                       | <span data-ttu-id="aca62-111">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="aca62-111">Programming language</span></span> |
|---------------------------------|----------------------|
| <span data-ttu-id="aca62-112">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="aca62-112">Raspberry Pi</span></span>                    | <span data-ttu-id="aca62-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="aca62-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="aca62-114">Az IoT-DevKit</span><span class="sxs-lookup"><span data-stu-id="aca62-114">IoT DevKit</span></span>                      | <span data-ttu-id="aca62-115">[A VSCode Arduino][DevKit]</span><span class="sxs-lookup"><span data-stu-id="aca62-115">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="aca62-116">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="aca62-116">Intel Edison</span></span>                    | <span data-ttu-id="aca62-117">[NODE.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="aca62-117">[Node.js][Ed_Nd], [C][Ed_C]</span></span>           |
| <span data-ttu-id="aca62-118">Adafruit lágyított HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="aca62-118">Adafruit Feather HUZZAH ESP8266</span></span> | <span data-ttu-id="aca62-119">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="aca62-119">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="aca62-120">Sparkfun ESP8266 dolog fejlesztői</span><span class="sxs-lookup"><span data-stu-id="aca62-120">Sparkfun ESP8266 Thing Dev</span></span>      | <span data-ttu-id="aca62-121">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="aca62-121">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="aca62-122">Adafruit lágyított M0</span><span class="sxs-lookup"><span data-stu-id="aca62-122">Adafruit Feather M0</span></span>             | <span data-ttu-id="aca62-123">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="aca62-123">[Arduino][M0_Ard]</span></span>              |

<span data-ttu-id="aca62-124">Továbbá az IoT-peremhálózati átjáró tooenable eszközök tooconnect tooyour IoT-központ is használhatja.</span><span class="sxs-lookup"><span data-stu-id="aca62-124">In addition, you can use an IoT Edge gateway tooenable devices tooconnect tooyour IoT hub.</span></span>

| <span data-ttu-id="aca62-125">Átjáróeszköz</span><span class="sxs-lookup"><span data-stu-id="aca62-125">Gateway device</span></span>               | <span data-ttu-id="aca62-126">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="aca62-126">Programming language</span></span> | <span data-ttu-id="aca62-127">Platform</span><span class="sxs-lookup"><span data-stu-id="aca62-127">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="aca62-128">Intel NUC (modell DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="aca62-128">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="aca62-129">C#</span><span class="sxs-lookup"><span data-stu-id="aca62-129">C</span></span>                    | <span data-ttu-id="aca62-130">[Szél folyó Linux][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="aca62-130">[Wind River Linux][NUC_Lnx]</span></span> |

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
