---
title: "egy málna Pi tooAzure IoT Suite aaaConnect C használata valós érzékelők |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. C tooconnect használata a távoli felügyeleti megoldás málna Pi toohello, telemetriai adatokat küldhet az érzékelők toohello felhőalapú és meghívni a hello megoldás irányítópultja toomethods válaszol."
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
ms.openlocfilehash: 7dac55ae5fde4c6f8e3ea6a7debf9a6822dc07ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a><span data-ttu-id="5dc95-104">Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello és telemetriai adatokat küldhet egy C használatával valós érzékelő</span><span class="sxs-lookup"><span data-stu-id="5dc95-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send telemetry from a real sensor using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="5dc95-105">Az oktatóanyag bemutatja, hogyan toouse hello Microsoft Azure IoT Starter Kit málna Pi 3 toodevelop a hőmérséklet és a páratartalom olvasó, amely képes kommunikálni a hello felhő.</span><span class="sxs-lookup"><span data-stu-id="5dc95-105">This tutorial shows you how toouse hello Microsoft Azure IoT Starter Kit for Raspberry Pi 3 toodevelop a temperature and humidity reader that can communicate with hello cloud.</span></span> <span data-ttu-id="5dc95-106">hello oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="5dc95-106">hello tutorial uses:</span></span>

- <span data-ttu-id="5dc95-107">Az operációs rendszer Raspbian, hello C programozási nyelv, és C tooimplement egy minta eszközt a Microsoft Azure IoT SDK hello.</span><span class="sxs-lookup"><span data-stu-id="5dc95-107">Raspbian OS, hello C programming language, and hello Microsoft Azure IoT SDK for C tooimplement a sample device.</span></span>
- <span data-ttu-id="5dc95-108">mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="5dc95-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="5dc95-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5dc95-109">Overview</span></span>

<span data-ttu-id="5dc95-110">Az oktatóanyag befejezése hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5dc95-110">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="5dc95-111">Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése.</span><span class="sxs-lookup"><span data-stu-id="5dc95-111">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="5dc95-112">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="5dc95-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="5dc95-113">Állítsa be a számítógép és a hello az eszköz és az érzékelők toocommunicate távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="5dc95-113">Set up your device and sensors toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="5dc95-114">Hello eszköz kód tooconnect toohello távoli figyelési megoldást frissítése, és megtekintheti a hello megoldás irányítópultja telemetriát.</span><span class="sxs-lookup"><span data-stu-id="5dc95-114">Update hello sample device code tooconnect toohello remote monitoring solution, and send telemetry that you can view on hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="5dc95-115">távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="5dc95-115">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="5dc95-116">hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="5dc95-116">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="5dc95-117">tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után.</span><span class="sxs-lookup"><span data-stu-id="5dc95-117">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="5dc95-118">Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="5dc95-118">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="5dc95-119">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="5dc95-119">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="5dc95-120">Töltse le és konfigurálja a hello minta</span><span class="sxs-lookup"><span data-stu-id="5dc95-120">Download and configure hello sample</span></span>

<span data-ttu-id="5dc95-121">Most töltse le, és a málna Pi hello távoli figyelési ügyfélalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5dc95-121">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-hello-repositories"></a><span data-ttu-id="5dc95-122">Klónozás hello adattárak</span><span class="sxs-lookup"><span data-stu-id="5dc95-122">Clone hello repositories</span></span>

<span data-ttu-id="5dc95-123">Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello követően egy terminált a parancsok a Pi:</span><span class="sxs-lookup"><span data-stu-id="5dc95-123">If you haven't already done so, clone hello required repositories by running hello following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="5dc95-124">Hello eszköz kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="5dc95-124">Update hello device connection string</span></span>

<span data-ttu-id="5dc95-125">Nyissa meg hello minta forrásfájl hello **nano** szerkesztő hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="5dc95-125">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
```

<span data-ttu-id="5dc95-126">Keresse meg az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="5dc95-126">Locate hello following lines:</span></span>

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

<span data-ttu-id="5dc95-127">Cserélje le hello helyőrző értékeket hello eszköz- és IoT-központ adatokat létrehozott és mentett ebben az oktatóanyagban hello elején.</span><span class="sxs-lookup"><span data-stu-id="5dc95-127">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="5dc95-128">Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="5dc95-128">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="build-hello-sample"></a><span data-ttu-id="5dc95-129">Hello minta</span><span class="sxs-lookup"><span data-stu-id="5dc95-129">Build hello sample</span></span>

<span data-ttu-id="5dc95-130">A Microsoft Azure IoT eszköz SDK hello hello csomagokat telepítése c hello hello málna Pi követően egy terminál-parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="5dc95-130">Install hello prerequisite packages for hello Microsoft Azure IoT Device SDK for C by running hello following commands in a terminal on hello Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="5dc95-131">Most már lefordíthatja frissítése hello megoldást a hello málna Pi:</span><span class="sxs-lookup"><span data-stu-id="5dc95-131">You can now build hello updated sample solution on hello Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

<span data-ttu-id="5dc95-132">Hello minta program hello málna Pi most futtathatja.</span><span class="sxs-lookup"><span data-stu-id="5dc95-132">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="5dc95-133">Adja meg a hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="5dc95-133">Enter hello command:</span></span>

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

<span data-ttu-id="5dc95-134">hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:</span><span class="sxs-lookup"><span data-stu-id="5dc95-134">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

<span data-ttu-id="5dc95-136">Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.</span><span class="sxs-lookup"><span data-stu-id="5dc95-136">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="5dc95-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5dc95-137">Next steps</span></span>

<span data-ttu-id="5dc95-138">A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="5dc95-138">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
