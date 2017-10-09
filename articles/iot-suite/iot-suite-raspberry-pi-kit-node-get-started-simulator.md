---
title: "egy málna Pi tooAzure IoT Suite aaaConnect szimulált telemetriai Node.js használatával |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. Node.js tooconnect használata a távoli felügyeleti megoldás málna Pi toohello szimulált telemetriai toohello felhő küldése és meghívni a hello megoldás irányítópultja toomethods válaszol."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: f65eeaa6e83fd89cdedae8fa8386a3e9ef8417d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a><span data-ttu-id="4e16a-104">Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello, és a Node.js segítségével szimulált telemetriai adatokat küldhet</span><span class="sxs-lookup"><span data-stu-id="4e16a-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send simulated telemetry using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="4e16a-105">Az oktatóanyag bemutatja, hogyan toouse hello málna Pi 3 toosimulate hőmérséklet és a páratartalom adatok toosend toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="4e16a-105">This tutorial shows you how toouse hello Raspberry Pi 3 toosimulate temperature and humidity data toosend toohello cloud.</span></span> <span data-ttu-id="4e16a-106">hello oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="4e16a-106">hello tutorial uses:</span></span>

- <span data-ttu-id="4e16a-107">Raspbian operációs rendszer, hello Node.js programozási nyelv, és a Microsoft Azure IoT SDK hello Node.js tooimplement egy minta eszköz.</span><span class="sxs-lookup"><span data-stu-id="4e16a-107">Raspbian OS, hello Node.js programming language, and hello Microsoft Azure IoT SDK for Node.js tooimplement a sample device.</span></span>
- <span data-ttu-id="4e16a-108">mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="4e16a-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="4e16a-109">távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="4e16a-109">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="4e16a-110">hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="4e16a-110">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="4e16a-111">tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után.</span><span class="sxs-lookup"><span data-stu-id="4e16a-111">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="4e16a-112">Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="4e16a-112">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="4e16a-113">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="4e16a-113">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="4e16a-114">Töltse le és konfigurálja a hello minta</span><span class="sxs-lookup"><span data-stu-id="4e16a-114">Download and configure hello sample</span></span>

<span data-ttu-id="4e16a-115">Most töltse le, és a málna Pi hello távoli figyelési ügyfélalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4e16a-115">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="4e16a-116">A Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="4e16a-116">Install Node.js</span></span>

<span data-ttu-id="4e16a-117">Ha még nem tette meg, telepítse a málna Pi Node.js.</span><span class="sxs-lookup"><span data-stu-id="4e16a-117">If you haven't done so already, install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="4e16a-118">hello IoT SDK for Node.js 0.11.5 Node.js vagy újabb verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="4e16a-118">hello IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="4e16a-119">hello következő lépések bemutatják, hogyan tooinstall Node.js v6.10.2 a málna Pi meg:</span><span class="sxs-lookup"><span data-stu-id="4e16a-119">hello following steps show you how tooinstall Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="4e16a-120">A következő parancs tooupdate hello a málna telepítővel:</span><span class="sxs-lookup"><span data-stu-id="4e16a-120">Use hello following command tooupdate your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="4e16a-121">A következő parancs toodownload hello Node.js bináris tooyour málna Pi hello használata:</span><span class="sxs-lookup"><span data-stu-id="4e16a-121">Use hello following command toodownload hello Node.js binaries tooyour Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="4e16a-122">A következő parancs tooinstall hello bináris hello használata:</span><span class="sxs-lookup"><span data-stu-id="4e16a-122">Use hello following command tooinstall hello binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="4e16a-123">A következő parancs tooverify Node.js v6.10.2 sikeresen telepített hello használata:</span><span class="sxs-lookup"><span data-stu-id="4e16a-123">Use hello following command tooverify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a><span data-ttu-id="4e16a-124">Klónozás hello adattárak</span><span class="sxs-lookup"><span data-stu-id="4e16a-124">Clone hello repositories</span></span>

<span data-ttu-id="4e16a-125">Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello követően egy terminált a parancsok a Pi:</span><span class="sxs-lookup"><span data-stu-id="4e16a-125">If you haven't already done so, clone hello required repositories by running hello following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="4e16a-126">Hello eszköz kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="4e16a-126">Update hello device connection string</span></span>

<span data-ttu-id="4e16a-127">Nyissa meg hello minta forrásfájl hello **nano** szerkesztő hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="4e16a-127">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

<span data-ttu-id="4e16a-128">Hello sor keresése:</span><span class="sxs-lookup"><span data-stu-id="4e16a-128">Find hello line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="4e16a-129">Cserélje le hello helyőrző értékeket hello eszköz- és IoT-központ adatokat létrehozott és mentett ebben az oktatóanyagban hello elején.</span><span class="sxs-lookup"><span data-stu-id="4e16a-129">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="4e16a-130">Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="4e16a-130">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="run-hello-sample"></a><span data-ttu-id="4e16a-131">Hello minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="4e16a-131">Run hello sample</span></span>

<span data-ttu-id="4e16a-132">Futtatási hello következő parancsok hello minta tooinstall hello csomagokat:</span><span class="sxs-lookup"><span data-stu-id="4e16a-132">Run hello following commands tooinstall hello prerequisite packages for hello sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

<span data-ttu-id="4e16a-133">Hello minta program hello málna Pi most futtathatja.</span><span class="sxs-lookup"><span data-stu-id="4e16a-133">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="4e16a-134">Adja meg a hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="4e16a-134">Enter hello command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

<span data-ttu-id="4e16a-135">hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:</span><span class="sxs-lookup"><span data-stu-id="4e16a-135">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

<span data-ttu-id="4e16a-137">Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.</span><span class="sxs-lookup"><span data-stu-id="4e16a-137">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a><span data-ttu-id="4e16a-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4e16a-138">Next steps</span></span>

<span data-ttu-id="4e16a-139">A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="4e16a-139">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
