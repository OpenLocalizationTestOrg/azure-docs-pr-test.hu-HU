---
title: "egy málna Pi tooAzure IoT Suite aaaConnect C használatával szimulált telemetriai |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. C tooconnect használata a távoli felügyeleti megoldás málna Pi toohello szimulált telemetriai toohello felhő küldése és meghívni a hello megoldás irányítópultja toomethods válaszol."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 3c4fa43b381385d1a7896cada34aa3aa0b8e5fb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-c"></a><span data-ttu-id="a77c7-104">Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello, és a C használatával szimulált telemetriai adatokat küldhet</span><span class="sxs-lookup"><span data-stu-id="a77c7-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send simulated telemetry using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="a77c7-105">Az oktatóanyag bemutatja, hogyan toouse hello málna Pi 3 toosimulate hőmérséklet és a páratartalom adatok toosend toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="a77c7-105">This tutorial shows you how toouse hello Raspberry Pi 3 toosimulate temperature and humidity data toosend toohello cloud.</span></span> <span data-ttu-id="a77c7-106">hello oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="a77c7-106">hello tutorial uses:</span></span>

- <span data-ttu-id="a77c7-107">Az operációs rendszer Raspbian, hello C programozási nyelv, és C tooimplement egy minta eszközt a Microsoft Azure IoT SDK hello.</span><span class="sxs-lookup"><span data-stu-id="a77c7-107">Raspbian OS, hello C programming language, and hello Microsoft Azure IoT SDK for C tooimplement a sample device.</span></span>
- <span data-ttu-id="a77c7-108">mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="a77c7-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="a77c7-109">távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a77c7-109">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="a77c7-110">hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="a77c7-110">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="a77c7-111">tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után.</span><span class="sxs-lookup"><span data-stu-id="a77c7-111">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="a77c7-112">Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="a77c7-112">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="a77c7-113">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="a77c7-113">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="a77c7-114">Töltse le és konfigurálja a hello minta</span><span class="sxs-lookup"><span data-stu-id="a77c7-114">Download and configure hello sample</span></span>

<span data-ttu-id="a77c7-115">Most töltse le, és a málna Pi hello távoli figyelési ügyfélalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a77c7-115">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-hello-repositories"></a><span data-ttu-id="a77c7-116">Klónozás hello adattárak</span><span class="sxs-lookup"><span data-stu-id="a77c7-116">Clone hello repositories</span></span>

<span data-ttu-id="a77c7-117">Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello követően egy terminált a parancsok a Pi:</span><span class="sxs-lookup"><span data-stu-id="a77c7-117">If you haven't already done so, clone hello required repositories by running hello following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="a77c7-118">Hello eszköz kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="a77c7-118">Update hello device connection string</span></span>

<span data-ttu-id="a77c7-119">Nyissa meg hello minta forrásfájl hello **nano** szerkesztő hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="a77c7-119">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/remote_monitoring/remote_monitoring.c
```

<span data-ttu-id="a77c7-120">Keresse meg az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="a77c7-120">Locate hello following lines:</span></span>

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

<span data-ttu-id="a77c7-121">Cserélje le hello helyőrző értékeket hello eszköz- és IoT-központ adatokat létrehozott és mentett ebben az oktatóanyagban hello elején.</span><span class="sxs-lookup"><span data-stu-id="a77c7-121">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="a77c7-122">Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="a77c7-122">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="build-hello-sample"></a><span data-ttu-id="a77c7-123">Hello minta</span><span class="sxs-lookup"><span data-stu-id="a77c7-123">Build hello sample</span></span>

<span data-ttu-id="a77c7-124">A Microsoft Azure IoT eszköz SDK hello hello csomagokat telepítése c hello hello málna Pi követően egy terminál-parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a77c7-124">Install hello prerequisite packages for hello Microsoft Azure IoT Device SDK for C by running hello following commands in a terminal on hello Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="a77c7-125">Most már lefordíthatja frissítése hello megoldást a hello málna Pi:</span><span class="sxs-lookup"><span data-stu-id="a77c7-125">You can now build hello updated sample solution on hello Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
```

<span data-ttu-id="a77c7-126">Hello minta program hello málna Pi most futtathatja.</span><span class="sxs-lookup"><span data-stu-id="a77c7-126">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="a77c7-127">Adja meg a hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="a77c7-127">Enter hello command:</span></span>

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

<span data-ttu-id="a77c7-128">hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:</span><span class="sxs-lookup"><span data-stu-id="a77c7-128">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

<span data-ttu-id="a77c7-130">Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.</span><span class="sxs-lookup"><span data-stu-id="a77c7-130">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a><span data-ttu-id="a77c7-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a77c7-131">Next steps</span></span>

<span data-ttu-id="a77c7-132">A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="a77c7-132">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-simulator/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
