---
title: "egy málna Pi tooAzure IoT Suite aaaConnect valós érzékelők Node.js használatával |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. Node.js tooconnect használata a távoli felügyeleti megoldás málna Pi toohello, telemetriai adatokat küldhet az érzékelők toohello felhőalapú és meghívni a hello megoldás irányítópultja toomethods válaszol."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 7ffb4a7a8c04b424a1f29170f4739d89f39a2429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a><span data-ttu-id="34f8e-104">Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello és telemetriai adatokat küldhet egy Node.js segítségével valós érzékelő</span><span class="sxs-lookup"><span data-stu-id="34f8e-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send telemetry from a real sensor using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="34f8e-105">Az oktatóanyag bemutatja, hogyan toouse hello Microsoft Azure IoT Starter Kit málna Pi 3 toodevelop a hőmérséklet és a páratartalom olvasó, amely képes kommunikálni a hello felhő.</span><span class="sxs-lookup"><span data-stu-id="34f8e-105">This tutorial shows you how toouse hello Microsoft Azure IoT Starter Kit for Raspberry Pi 3 toodevelop a temperature and humidity reader that can communicate with hello cloud.</span></span> <span data-ttu-id="34f8e-106">hello oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="34f8e-106">hello tutorial uses:</span></span>

- <span data-ttu-id="34f8e-107">Raspbian operációs rendszer, hello Node.js programozási nyelv, és a Microsoft Azure IoT SDK hello Node.js tooimplement egy minta eszköz.</span><span class="sxs-lookup"><span data-stu-id="34f8e-107">Raspbian OS, hello Node.js programming language, and hello Microsoft Azure IoT SDK for Node.js tooimplement a sample device.</span></span>
- <span data-ttu-id="34f8e-108">mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="34f8e-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="34f8e-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="34f8e-109">Overview</span></span>

<span data-ttu-id="34f8e-110">Az oktatóanyag befejezése hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="34f8e-110">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="34f8e-111">Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése.</span><span class="sxs-lookup"><span data-stu-id="34f8e-111">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="34f8e-112">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="34f8e-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="34f8e-113">Állítsa be a számítógép és a hello az eszköz és az érzékelők toocommunicate távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="34f8e-113">Set up your device and sensors toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="34f8e-114">Hello eszköz kód tooconnect toohello távoli figyelési megoldást frissítése, és megtekintheti a hello megoldás irányítópultja telemetriát.</span><span class="sxs-lookup"><span data-stu-id="34f8e-114">Update hello sample device code tooconnect toohello remote monitoring solution, and send telemetry that you can view on hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="34f8e-115">távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="34f8e-115">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="34f8e-116">hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="34f8e-116">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="34f8e-117">tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után.</span><span class="sxs-lookup"><span data-stu-id="34f8e-117">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="34f8e-118">Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="34f8e-118">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="34f8e-119">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="34f8e-119">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="34f8e-120">Töltse le és konfigurálja a hello minta</span><span class="sxs-lookup"><span data-stu-id="34f8e-120">Download and configure hello sample</span></span>

<span data-ttu-id="34f8e-121">Most töltse le, és a málna Pi hello távoli figyelési ügyfélalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="34f8e-121">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="34f8e-122">A Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="34f8e-122">Install Node.js</span></span>

<span data-ttu-id="34f8e-123">Node.js telepíthető a Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="34f8e-123">Install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="34f8e-124">hello IoT SDK for Node.js 0.11.5 Node.js vagy újabb verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="34f8e-124">hello IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="34f8e-125">hello következő lépések bemutatják, hogyan tooinstall Node.js v6.10.2 a málna Pi meg:</span><span class="sxs-lookup"><span data-stu-id="34f8e-125">hello following steps show you how tooinstall Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="34f8e-126">A következő parancs tooupdate hello a málna telepítővel:</span><span class="sxs-lookup"><span data-stu-id="34f8e-126">Use hello following command tooupdate your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="34f8e-127">A következő parancs toodownload hello Node.js bináris tooyour málna Pi hello használata:</span><span class="sxs-lookup"><span data-stu-id="34f8e-127">Use hello following command toodownload hello Node.js binaries tooyour Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="34f8e-128">A következő parancs tooinstall hello bináris hello használata:</span><span class="sxs-lookup"><span data-stu-id="34f8e-128">Use hello following command tooinstall hello binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="34f8e-129">A következő parancs tooverify Node.js v6.10.2 sikeresen telepített hello használata:</span><span class="sxs-lookup"><span data-stu-id="34f8e-129">Use hello following command tooverify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a><span data-ttu-id="34f8e-130">Klónozás hello adattárak</span><span class="sxs-lookup"><span data-stu-id="34f8e-130">Clone hello repositories</span></span>

<span data-ttu-id="34f8e-131">Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello parancsok követően a Pi:</span><span class="sxs-lookup"><span data-stu-id="34f8e-131">If you haven't already done so, clone hello required repositories by running hello following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="34f8e-132">Hello eszköz kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="34f8e-132">Update hello device connection string</span></span>

<span data-ttu-id="34f8e-133">Nyissa meg hello minta forrásfájl hello **nano** szerkesztő hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="34f8e-133">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="34f8e-134">Hello sor keresése:</span><span class="sxs-lookup"><span data-stu-id="34f8e-134">Find hello line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="34f8e-135">Cserélje le hello helyőrző értékeket hello eszköz- és IoT-központ adatokat létrehozott és mentett ebben az oktatóanyagban hello elején.</span><span class="sxs-lookup"><span data-stu-id="34f8e-135">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="34f8e-136">Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="34f8e-136">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="run-hello-sample"></a><span data-ttu-id="34f8e-137">Hello minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="34f8e-137">Run hello sample</span></span>

<span data-ttu-id="34f8e-138">Futtatási hello következő parancsok hello minta tooinstall hello csomagokat:</span><span class="sxs-lookup"><span data-stu-id="34f8e-138">Run hello following commands tooinstall hello prerequisite packages for hello sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic
npm install
```

<span data-ttu-id="34f8e-139">Hello minta program hello málna Pi most futtathatja.</span><span class="sxs-lookup"><span data-stu-id="34f8e-139">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="34f8e-140">Adja meg a hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="34f8e-140">Enter hello command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="34f8e-141">hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:</span><span class="sxs-lookup"><span data-stu-id="34f8e-141">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

<span data-ttu-id="34f8e-143">Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.</span><span class="sxs-lookup"><span data-stu-id="34f8e-143">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="34f8e-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34f8e-144">Next steps</span></span>

<span data-ttu-id="34f8e-145">A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="34f8e-145">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
