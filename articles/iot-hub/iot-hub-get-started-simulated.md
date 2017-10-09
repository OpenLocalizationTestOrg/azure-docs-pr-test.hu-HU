---
title: "Ismerkedés a szimulált eszköz tooAzure IoT-központ csatlakozás |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate szimulált az IoT-eszközök, és csatlakoztassa őket az IoT-központ tooAzure. Az eszközök küldhet telemetriai tooIoT központ és az Iot-központ figyelheti és az eszközök kezeléséhez."
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
ms.date: 06/02/2017
ms.author: dobett
ms.openlocfilehash: 2c9b76477d12c853abd93aa96043417a013daaef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-with-simulated-devices-tutorials"></a><span data-ttu-id="d35f6-105">Az Azure IoT Hub szimulált eszköz oktatóanyagok az első lépései</span><span class="sxs-lookup"><span data-stu-id="d35f6-105">Azure IoT Hub get started with simulated devices tutorials</span></span>

<span data-ttu-id="d35f6-106">Ezek az oktatóanyagok tooAzure IoT Hub és hello eszköz SDK-k bevezetése.</span><span class="sxs-lookup"><span data-stu-id="d35f6-106">These tutorials introduce you tooAzure IoT Hub and hello device SDKs.</span></span> <span data-ttu-id="d35f6-107">hello oktatóanyagok általános IoT forgatókönyvek toodemonstrate hello képességeket IoT hub foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="d35f6-107">hello tutorials cover common IoT scenarios toodemonstrate hello capabilities of IoT Hub.</span></span> <span data-ttu-id="d35f6-108">hello oktatóanyagokat is bemutatják, hogyan-toocombine más Azure IoT hubot szolgáltatásokat és eszközöket több toobuild hatékony IoT-megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d35f6-108">hello tutorials also illustrate how toocombine IoT Hub with other Azure services and tools toobuild more powerful IoT solutions.</span></span> <span data-ttu-id="d35f6-109">hello a következő táblázatban felsorolt hello oktatóprogramok bemutatják, hogyan toocreate szimulált az IoT-eszközök.</span><span class="sxs-lookup"><span data-stu-id="d35f6-109">hello tutorials listed in hello following table show you how toocreate simulated IoT devices.</span></span>

| <span data-ttu-id="d35f6-110">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="d35f6-110">Programming language</span></span> |
|----------------------|
| <span data-ttu-id="d35f6-111">[.NET][Sim_NET]</span><span class="sxs-lookup"><span data-stu-id="d35f6-111">[.NET][Sim_NET]</span></span>      |
| <span data-ttu-id="d35f6-112">[Java][Sim_Jav]</span><span class="sxs-lookup"><span data-stu-id="d35f6-112">[Java][Sim_Jav]</span></span>      |
| <span data-ttu-id="d35f6-113">[NODE.js][Sim_Nd]</span><span class="sxs-lookup"><span data-stu-id="d35f6-113">[Node.js][Sim_Nd]</span></span>    |
| <span data-ttu-id="d35f6-114">[Python][Sim_Pyth]</span><span class="sxs-lookup"><span data-stu-id="d35f6-114">[Python][Sim_Pyth]</span></span>   |

<span data-ttu-id="d35f6-115">Továbbá az IoT-peremhálózati átjáró szimulált tooenable eszközök tooconnect tooyour IoT-központ is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d35f6-115">In addition, you can use an IoT Edge gateway tooenable simulated devices tooconnect tooyour IoT hub.</span></span>

| <span data-ttu-id="d35f6-116">Programozási nyelv</span><span class="sxs-lookup"><span data-stu-id="d35f6-116">Programming language</span></span> | <span data-ttu-id="d35f6-117">Platform</span><span class="sxs-lookup"><span data-stu-id="d35f6-117">Platform</span></span>           |
|----------------------|------------------- |
| <span data-ttu-id="d35f6-118">C#</span><span class="sxs-lookup"><span data-stu-id="d35f6-118">C</span></span>                    | <span data-ttu-id="d35f6-119">[Linux][Sim_Lnx]</span><span class="sxs-lookup"><span data-stu-id="d35f6-119">[Linux][Sim_Lnx]</span></span>   |
| <span data-ttu-id="d35f6-120">C#</span><span class="sxs-lookup"><span data-stu-id="d35f6-120">C</span></span>                    | <span data-ttu-id="d35f6-121">[Windows][Sim_Win]</span><span class="sxs-lookup"><span data-stu-id="d35f6-121">[Windows][Sim_Win]</span></span> |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
