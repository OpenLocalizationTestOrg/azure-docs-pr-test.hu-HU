---
title: "Csatlakozás az Azure IoT Suite Raspberry Pi |} Microsoft Docs"
description: "Az oktatóanyag segítséget nyújtanak a málna Pi 3 és az IoT Suite távoli felügyeleti megoldás a Microsoft Azure IoT Starter Kit használata Node.js vagy a C használatával. A következőképpen választhatja ki egy oktatóanyag, amely szimulálja telemetriai adatokat használó valós érzékelők vagy, amely lehetővé teszi, hogy a távoli belső vezérlőprogram-frissítésekre."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: eaa6a21a08bd9068b5335a8167f54c2aa387e0e5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-microsoft-azure-iot-raspberry-pi-3-starter-kit-to-the-remote-monitoring-solution"></a><span data-ttu-id="3caaf-104">A Microsoft Azure IoT málna Pi 3 Starter Kit kapcsolódni a távoli felügyeleti megoldás</span><span class="sxs-lookup"><span data-stu-id="3caaf-104">Connect your Microsoft Azure IoT Raspberry Pi 3 Starter Kit to the remote monitoring solution</span></span>

<span data-ttu-id="3caaf-105">Az oktatóanyagok ebben a szakaszban megtudhatja, hogyan málna Pi 3 eszköz csatlakozni a távoli felügyeleti megoldást nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="3caaf-105">The tutorials in this section help you learn how to connect a Raspberry Pi 3 device to the remote monitoring solution.</span></span> <span data-ttu-id="3caaf-106">Válassza ki az előnyben részesített programozási nyelv megfelelő oktatóanyag és a hogy van-e a használhatók a málna Pi a hardverét.</span><span class="sxs-lookup"><span data-stu-id="3caaf-106">Choose the tutorial appropriate to your preferred programming language and the whether you have the sensor hardware available to use with your Raspberry Pi.</span></span>

## <a name="the-tutorials"></a><span data-ttu-id="3caaf-107">Az oktatóanyagok</span><span class="sxs-lookup"><span data-stu-id="3caaf-107">The tutorials</span></span>

| <span data-ttu-id="3caaf-108">Oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="3caaf-108">Tutorial</span></span> | <span data-ttu-id="3caaf-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="3caaf-109">Notes</span></span> | <span data-ttu-id="3caaf-110">Nyelvek</span><span class="sxs-lookup"><span data-stu-id="3caaf-110">Languages</span></span> |
| -------- | ----- | --------- |
| <span data-ttu-id="3caaf-111">Szimulált telemetriai (alapszintű)</span><span class="sxs-lookup"><span data-stu-id="3caaf-111">Simulated telemetry (Basic)</span></span>| <span data-ttu-id="3caaf-112">Érzékelőadatok szimulálja.</span><span class="sxs-lookup"><span data-stu-id="3caaf-112">Simulates sensor data.</span></span> <span data-ttu-id="3caaf-113">Önálló málna Pi használja.</span><span class="sxs-lookup"><span data-stu-id="3caaf-113">Uses a standalone Raspberry Pi.</span></span> | <span data-ttu-id="3caaf-114">[C][lnk-c-simulator], [Node.js][lnk-node-simulator]</span><span class="sxs-lookup"><span data-stu-id="3caaf-114">[C][lnk-c-simulator], [Node.js][lnk-node-simulator]</span></span> |
| <span data-ttu-id="3caaf-115">Valós érzékelő (közepes)</span><span class="sxs-lookup"><span data-stu-id="3caaf-115">Real sensor (Intermediate)</span></span> | <span data-ttu-id="3caaf-116">Csatlakozik a málna Pi BME280 érzékelő adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="3caaf-116">Uses data from a BME280 sensor connected to your Raspberry Pi.</span></span> | <span data-ttu-id="3caaf-117">[C][lnk-c-basic], [Node.js][lnk-node-basic]</span><span class="sxs-lookup"><span data-stu-id="3caaf-117">[C][lnk-c-basic], [Node.js][lnk-node-basic]</span></span> |
| <span data-ttu-id="3caaf-118">Valósítja meg a belső vezérlőprogram-frissítés (speciális)</span><span class="sxs-lookup"><span data-stu-id="3caaf-118">Implement firmware update (Advanced)</span></span>| <span data-ttu-id="3caaf-119">Csatlakozik a málna Pi BME280 érzékelő adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="3caaf-119">Uses data from a BME280 sensor connected to your Raspberry Pi.</span></span> <span data-ttu-id="3caaf-120">Lehetővé teszi, hogy a málna Pi távoli belső vezérlőprogram-frissítések.</span><span class="sxs-lookup"><span data-stu-id="3caaf-120">Enables remote firmware updates on your Raspberry Pi.</span></span> | <span data-ttu-id="3caaf-121">[C][lnk-c-advanced], [Node.js][lnk-node-advanced]</span><span class="sxs-lookup"><span data-stu-id="3caaf-121">[C][lnk-c-advanced], [Node.js][lnk-node-advanced]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3caaf-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3caaf-122">Next steps</span></span>

<span data-ttu-id="3caaf-123">Látogasson el a [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="3caaf-123">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[lnk-node-simulator]: iot-suite-raspberry-pi-kit-node-get-started-simulator.md
[lnk-node-basic]: iot-suite-raspberry-pi-kit-node-get-started-basic.md
[lnk-node-advanced]: iot-suite-raspberry-pi-kit-node-get-started-advanced.md
[lnk-c-simulator]: iot-suite-raspberry-pi-kit-c-get-started-simulator.md
[lnk-c-basic]: iot-suite-raspberry-pi-kit-c-get-started-basic.md
[lnk-c-advanced]: iot-suite-raspberry-pi-kit-c-get-started-advanced.md