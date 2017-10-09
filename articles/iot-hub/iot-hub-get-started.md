---
title: "Az Azure IoT Hub - első lépések csatlakoztatása az IoT-eszközök toohello felhő |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect az IoT-modulok és a starter Kit tooAzure IoT-központot. Az eszközök küldhet telemetriai tooIoT központ és az IoT-központ figyelheti és az eszközök kezeléséhez."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: "az Azure iot hub oktatóanyag"
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 6dc956308009091532019ff84aec881f042f0104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a><span data-ttu-id="e9340-105">Az Azure IoT Hub get oktatóanyagok indítása</span><span class="sxs-lookup"><span data-stu-id="e9340-105">Azure IoT Hub get started tutorials</span></span>

<span data-ttu-id="e9340-106">Azure IoT Hub és hello Azure IoT eszköz SDK-k toobuild az eszközök internetes hálózatát (IoT) megoldások is használhatja:</span><span class="sxs-lookup"><span data-stu-id="e9340-106">You can use Azure IoT Hub and hello Azure IoT device SDKs toobuild Internet of Things (IoT) solutions:</span></span>

* <span data-ttu-id="e9340-107">Az Azure IoT-központot egy olyan teljes körűen felügyelt szolgáltatás hello felhőbe, amely biztonságosan csatlakozik, figyeli, és kezeli az IoT-eszközök.</span><span class="sxs-lookup"><span data-stu-id="e9340-107">Azure IoT Hub is a fully managed service in hello cloud that securely connects, monitors, and manages your IoT devices.</span></span> <span data-ttu-id="e9340-108">Hello Azure IoT eszközoldali SDK-k tooimplement az IoT-eszközök használata.</span><span class="sxs-lookup"><span data-stu-id="e9340-108">Use hello Azure IoT Device SDKs tooimplement your IoT devices.</span></span>
* <span data-ttu-id="e9340-109">Az IoT-átjáró összetettebb IoT-forgatókönyvek esetén használható.</span><span class="sxs-lookup"><span data-stu-id="e9340-109">Use an IoT gateway in more complex IoT scenarios.</span></span> <span data-ttu-id="e9340-110">Például, ha szüksége tooconsider tényezőkkel, például az örökölt eszközök, a sávszélességgel kapcsolatos költségek, a biztonsági és adatvédelmi szabályzatok vagy a biztonsági adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="e9340-110">For example, where you need tooconsider factors such as legacy devices, bandwidth costs, security and privacy policies, or edge data processing.</span></span> <span data-ttu-id="e9340-111">Ezek a forgatókönyvek használhatja az Azure IoT peremhálózati tooimplement eszközök tooyour IoT-központ összekapcsoló átjáró.</span><span class="sxs-lookup"><span data-stu-id="e9340-111">In these scenarios, you use Azure IoT Edge tooimplement a gateway that connects devices tooyour IoT hub.</span></span>

## <a name="what-hello-tutorials-cover"></a><span data-ttu-id="e9340-112">Hello oktatóanyagok vonatkozik</span><span class="sxs-lookup"><span data-stu-id="e9340-112">What hello tutorials cover</span></span>

<span data-ttu-id="e9340-113">Ezek az oktatóanyagok tooAzure IoT Hub és hello eszköz SDK-k bevezetése.</span><span class="sxs-lookup"><span data-stu-id="e9340-113">These tutorials introduce you tooAzure IoT Hub and hello device SDKs.</span></span> <span data-ttu-id="e9340-114">hello oktatóanyagok általános IoT forgatókönyvek toodemonstrate hello képességeket IoT hub foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="e9340-114">hello tutorials cover common IoT scenarios toodemonstrate hello capabilities of IoT Hub.</span></span> <span data-ttu-id="e9340-115">hello oktatóanyagokat is bemutatják, hogyan-toocombine más Azure IoT hubot szolgáltatásokat és eszközöket több toobuild hatékony IoT-megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="e9340-115">hello tutorials also illustrate how toocombine IoT Hub with other Azure services and tools toobuild more powerful IoT solutions.</span></span> <span data-ttu-id="e9340-116">Hello oktatóanyagok válassza toouse szimulált vagy valós az IoT-eszközök.</span><span class="sxs-lookup"><span data-stu-id="e9340-116">In hello tutorials, you can choose toouse either simulated or real IoT devices.</span></span> <span data-ttu-id="e9340-117">Emellett áttekintheti, hogyan toouse egy átjáró tooenable eszközök tooconnect tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="e9340-117">In addition, you can learn how toouse a gateway tooenable devices tooconnect tooyour IoT hub.</span></span>

## <a name="set-up-your-device"></a><span data-ttu-id="e9340-118">Az eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="e9340-118">Set up your device</span></span>

<span data-ttu-id="e9340-119">Csatlakozás egy IoT eszköz vagy az átjáró tooAzure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="e9340-119">Connect an IoT device or gateway tooAzure IoT Hub.</span></span> <span data-ttu-id="e9340-120">A szimulált vagy fizikai eszköz tooget lépések közül választhat:</span><span class="sxs-lookup"><span data-stu-id="e9340-120">You can choose a physical or simulated device tooget started:</span></span>

| <span data-ttu-id="e9340-121">IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="e9340-121">IoT device</span></span>                       | <span data-ttu-id="e9340-122">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="e9340-122">Programming language</span></span> |
|----------------------------------|----------------------|
| <span data-ttu-id="e9340-123">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="e9340-123">Raspberry Pi</span></span>                     | <span data-ttu-id="e9340-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="e9340-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="e9340-125">Az IoT-DevKit</span><span class="sxs-lookup"><span data-stu-id="e9340-125">IoT DevKit</span></span>                       | <span data-ttu-id="e9340-126">[A VSCode Arduino][DevKit]</span><span class="sxs-lookup"><span data-stu-id="e9340-126">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="e9340-127">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="e9340-127">Intel Edison</span></span>                     | <span data-ttu-id="e9340-128">[NODE.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="e9340-128">[Node.js][Ed_Nd], [C][Ed_C]</span></span>    |
| <span data-ttu-id="e9340-129">Adafruit lágyított HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="e9340-129">Adafruit Feather HUZZAH ESP8266</span></span>  | <span data-ttu-id="e9340-130">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="e9340-130">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="e9340-131">Sparkfun ESP8266 dolog fejlesztői</span><span class="sxs-lookup"><span data-stu-id="e9340-131">Sparkfun ESP8266 Thing Dev</span></span>       | <span data-ttu-id="e9340-132">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="e9340-132">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="e9340-133">Adafruit lágyított M0</span><span class="sxs-lookup"><span data-stu-id="e9340-133">Adafruit Feather M0</span></span>              | <span data-ttu-id="e9340-134">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="e9340-134">[Arduino][M0_Ard]</span></span>              |
| <span data-ttu-id="e9340-135">Szimulált eszköz számítógépen</span><span class="sxs-lookup"><span data-stu-id="e9340-135">Simulated device on PC</span></span>           | <span data-ttu-id="e9340-136">[.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span><span class="sxs-lookup"><span data-stu-id="e9340-136">[.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span></span> |
| <span data-ttu-id="e9340-137">Online eszköz szimulátor</span><span class="sxs-lookup"><span data-stu-id="e9340-137">Online device simulator</span></span>         | <span data-ttu-id="e9340-138">[Raspberry Pi (Node.js)][Ol_Sim]</span><span class="sxs-lookup"><span data-stu-id="e9340-138">[Raspberry Pi (Node.js)][Ol_Sim]</span></span> |

<span data-ttu-id="e9340-139">Továbbá az IoT-peremhálózati átjáró tooenable eszközök tooconnect tooyour IoT-központ is használhatja:</span><span class="sxs-lookup"><span data-stu-id="e9340-139">In addition, you can use an IoT Edge gateway tooenable devices tooconnect tooyour IoT hub:</span></span>

| <span data-ttu-id="e9340-140">Átjáróeszköz</span><span class="sxs-lookup"><span data-stu-id="e9340-140">Gateway device</span></span>               | <span data-ttu-id="e9340-141">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="e9340-141">Programming language</span></span> | <span data-ttu-id="e9340-142">Platform</span><span class="sxs-lookup"><span data-stu-id="e9340-142">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="e9340-143">Intel NUC (modell DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="e9340-143">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="e9340-144">C#</span><span class="sxs-lookup"><span data-stu-id="e9340-144">C</span></span>                    | <span data-ttu-id="e9340-145">[Szél folyó Linux][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="e9340-145">[Wind River Linux][NUC_Lnx]</span></span> |
| <span data-ttu-id="e9340-146">Szimulált átjáró</span><span class="sxs-lookup"><span data-stu-id="e9340-146">Simulated gateway</span></span>            | <span data-ttu-id="e9340-147">C#</span><span class="sxs-lookup"><span data-stu-id="e9340-147">C</span></span>                    | <span data-ttu-id="e9340-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span><span class="sxs-lookup"><span data-stu-id="e9340-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span></span> |

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
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
