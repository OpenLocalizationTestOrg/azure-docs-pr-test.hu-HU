---
title: "Az Azure IoT Hub - első lépések az IoT-eszközök felhőhöz történő csatlakoztatásának |} Microsoft Docs"
description: "Útmutató: Azure IoT-központ az IoT-modulok és a starter Kit csatlakozni. Az eszközök küldhet az IoT-központ és az IoT-központ telemetriai figyelheti és az eszközök kezeléséhez."
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
ms.openlocfilehash: 45016e6383761ffe78f13ccef1112ab3d9753498
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a><span data-ttu-id="20497-105">Az Azure IoT Hub get oktatóanyagok indítása</span><span class="sxs-lookup"><span data-stu-id="20497-105">Azure IoT Hub get started tutorials</span></span>

<span data-ttu-id="20497-106">Azure IoT-központ és az Azure IoT-eszközök SDK-k segítségével az eszközök internetes hálózatát (IoT) megoldások:</span><span class="sxs-lookup"><span data-stu-id="20497-106">You can use Azure IoT Hub and the Azure IoT device SDKs to build Internet of Things (IoT) solutions:</span></span>

* <span data-ttu-id="20497-107">Az Azure IoT-központot egy olyan teljes körűen felügyelt szolgáltatás a felhőben, amelyek biztonságosan csatlakozik, figyeli, és kezeli az IoT-eszközök.</span><span class="sxs-lookup"><span data-stu-id="20497-107">Azure IoT Hub is a fully managed service in the cloud that securely connects, monitors, and manages your IoT devices.</span></span> <span data-ttu-id="20497-108">Az Azure IoT eszközoldali SDK-k segítségével valósítja meg az IoT-eszközök.</span><span class="sxs-lookup"><span data-stu-id="20497-108">Use the Azure IoT Device SDKs to implement your IoT devices.</span></span>
* <span data-ttu-id="20497-109">Az IoT-átjáró összetettebb IoT-forgatókönyvek esetén használható.</span><span class="sxs-lookup"><span data-stu-id="20497-109">Use an IoT gateway in more complex IoT scenarios.</span></span> <span data-ttu-id="20497-110">Például, ha szeretné például az örökölt eszközök, a sávszélességgel kapcsolatos költségek, a biztonsági és adatvédelmi szabályzatok vagy a biztonsági adatok feldolgozása tényezőket kell figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="20497-110">For example, where you need to consider factors such as legacy devices, bandwidth costs, security and privacy policies, or edge data processing.</span></span> <span data-ttu-id="20497-111">Ezekben az esetekben a Azure IoT peremhálózati átjáró, amely az eszköz csatlakozik az IoT hub végrehajtásához használhatja.</span><span class="sxs-lookup"><span data-stu-id="20497-111">In these scenarios, you use Azure IoT Edge to implement a gateway that connects devices to your IoT hub.</span></span>

## <a name="what-the-tutorials-cover"></a><span data-ttu-id="20497-112">Az oktatóanyagok vonatkozik</span><span class="sxs-lookup"><span data-stu-id="20497-112">What the tutorials cover</span></span>

<span data-ttu-id="20497-113">Ezek az oktatóanyagok bemutatják a Azure IoT-központ és az eszköz SDK-k.</span><span class="sxs-lookup"><span data-stu-id="20497-113">These tutorials introduce you to Azure IoT Hub and the device SDKs.</span></span> <span data-ttu-id="20497-114">Az oktatóanyagok általános IoT-forgatókönyvek esetén az IoT-központ funkcióinak bemutatása foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="20497-114">The tutorials cover common IoT scenarios to demonstrate the capabilities of IoT Hub.</span></span> <span data-ttu-id="20497-115">Az oktatóanyagok bemutatják, hogyan kombinálhatók az IoT-központ más Azure-szolgáltatások és eszközök nagyobb teljesítményű az IoT-megoldások létrehozásához is.</span><span class="sxs-lookup"><span data-stu-id="20497-115">The tutorials also illustrate how to combine IoT Hub with other Azure services and tools to build more powerful IoT solutions.</span></span> <span data-ttu-id="20497-116">Az oktatóanyagok választhatja ki, vagy a szimulált, vagy a valós az IoT-eszközök használatához.</span><span class="sxs-lookup"><span data-stu-id="20497-116">In the tutorials, you can choose to use either simulated or real IoT devices.</span></span> <span data-ttu-id="20497-117">Továbbá megismerheti az átjáró használatával engedélyezhető az eszközök csatlakoztatása az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="20497-117">In addition, you can learn how to use a gateway to enable devices to connect to your IoT hub.</span></span>

## <a name="set-up-your-device"></a><span data-ttu-id="20497-118">Az eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="20497-118">Set up your device</span></span>

<span data-ttu-id="20497-119">Csatlakoztassa az IoT-eszközön vagy Azure IoT Hub-átjárót.</span><span class="sxs-lookup"><span data-stu-id="20497-119">Connect an IoT device or gateway to Azure IoT Hub.</span></span> <span data-ttu-id="20497-120">Első lépésként szimulált vagy fizikai eszköz közül választhat:</span><span class="sxs-lookup"><span data-stu-id="20497-120">You can choose a physical or simulated device to get started:</span></span>

| <span data-ttu-id="20497-121">IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="20497-121">IoT device</span></span>                       | <span data-ttu-id="20497-122">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="20497-122">Programming language</span></span> |
|----------------------------------|----------------------|
| <span data-ttu-id="20497-123">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="20497-123">Raspberry Pi</span></span>                     | <span data-ttu-id="20497-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="20497-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="20497-125">Az IoT-DevKit</span><span class="sxs-lookup"><span data-stu-id="20497-125">IoT DevKit</span></span>                       | <span data-ttu-id="20497-126">[A VSCode Arduino][DevKit]</span><span class="sxs-lookup"><span data-stu-id="20497-126">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="20497-127">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="20497-127">Intel Edison</span></span>                     | <span data-ttu-id="20497-128">[NODE.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="20497-128">[Node.js][Ed_Nd], [C][Ed_C]</span></span>    |
| <span data-ttu-id="20497-129">Adafruit lágyított HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="20497-129">Adafruit Feather HUZZAH ESP8266</span></span>  | <span data-ttu-id="20497-130">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="20497-130">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="20497-131">Sparkfun ESP8266 dolog fejlesztői</span><span class="sxs-lookup"><span data-stu-id="20497-131">Sparkfun ESP8266 Thing Dev</span></span>       | <span data-ttu-id="20497-132">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="20497-132">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="20497-133">Adafruit lágyított M0</span><span class="sxs-lookup"><span data-stu-id="20497-133">Adafruit Feather M0</span></span>              | <span data-ttu-id="20497-134">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="20497-134">[Arduino][M0_Ard]</span></span>              |
| <span data-ttu-id="20497-135">Szimulált eszköz számítógépen</span><span class="sxs-lookup"><span data-stu-id="20497-135">Simulated device on PC</span></span>           | <span data-ttu-id="20497-136">[.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span><span class="sxs-lookup"><span data-stu-id="20497-136">[.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span></span> |
| <span data-ttu-id="20497-137">Online eszköz szimulátor</span><span class="sxs-lookup"><span data-stu-id="20497-137">Online device simulator</span></span>         | <span data-ttu-id="20497-138">[Raspberry Pi (Node.js)][Ol_Sim]</span><span class="sxs-lookup"><span data-stu-id="20497-138">[Raspberry Pi (Node.js)][Ol_Sim]</span></span> |

<span data-ttu-id="20497-139">Emellett az egy IoT peremhálózati átjáró segítségével engedélyezheti az eszközök csatlakoztatása az IoT hub:</span><span class="sxs-lookup"><span data-stu-id="20497-139">In addition, you can use an IoT Edge gateway to enable devices to connect to your IoT hub:</span></span>

| <span data-ttu-id="20497-140">Átjáróeszköz</span><span class="sxs-lookup"><span data-stu-id="20497-140">Gateway device</span></span>               | <span data-ttu-id="20497-141">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="20497-141">Programming language</span></span> | <span data-ttu-id="20497-142">Platform</span><span class="sxs-lookup"><span data-stu-id="20497-142">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="20497-143">Intel NUC (modell DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="20497-143">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="20497-144">C#</span><span class="sxs-lookup"><span data-stu-id="20497-144">C</span></span>                    | <span data-ttu-id="20497-145">[Szél folyó Linux][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="20497-145">[Wind River Linux][NUC_Lnx]</span></span> |
| <span data-ttu-id="20497-146">Szimulált átjáró</span><span class="sxs-lookup"><span data-stu-id="20497-146">Simulated gateway</span></span>            | <span data-ttu-id="20497-147">C#</span><span class="sxs-lookup"><span data-stu-id="20497-147">C</span></span>                    | <span data-ttu-id="20497-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span><span class="sxs-lookup"><span data-stu-id="20497-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span></span> |

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
