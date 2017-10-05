---
title: "Csatlakozás egy málna Pi Azure IoT Suite Node.js használatával valós érzékelők |} Microsoft Docs"
description: "Használja a Microsoft Azure IoT Starter Kit a Raspberry pi 3 és az Azure IoT Suite. A málna Pi kapcsolódni a távoli felügyeleti megoldás használata Node.js telemetriai adatokat küldhet az érzékelők a felhőhöz, és a megoldás irányítópultja metódusokra válaszol."
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
ms.openlocfilehash: 91546157cc8eabf68706391ce706038d8dc5f82d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a><span data-ttu-id="02cb7-104">A málna Pi 3 kapcsolódni a távoli felügyeleti megoldás és telemetriai adatokat küldhet egy Node.js segítségével valós érzékelő</span><span class="sxs-lookup"><span data-stu-id="02cb7-104">Connect your Raspberry Pi 3 to the remote monitoring solution and send telemetry from a real sensor using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="02cb7-105">Az oktatóanyag bemutatja, hogyan használhatja a Microsoft Azure IoT Starter Kit málna Pi 3 fejlesztéséhez a hőmérséklet és a páratartalom olvasó, amely képes kommunikálni a felhőben.</span><span class="sxs-lookup"><span data-stu-id="02cb7-105">This tutorial shows you how to use the Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to develop a temperature and humidity reader that can communicate with the cloud.</span></span> <span data-ttu-id="02cb7-106">Az oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="02cb7-106">The tutorial uses:</span></span>

- <span data-ttu-id="02cb7-107">Raspbian operációs rendszer, a Node.js programozási nyelv, és a Microsoft Azure IoT SDK for Node.js egy minta eszköz végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="02cb7-107">Raspbian OS, the Node.js programming language, and the Microsoft Azure IoT SDK for Node.js to implement a sample device.</span></span>
- <span data-ttu-id="02cb7-108">Az IoT Suite távoli megfigyelési előre konfigurált megoldás, a felhő alapú háttér.</span><span class="sxs-lookup"><span data-stu-id="02cb7-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="02cb7-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="02cb7-109">Overview</span></span>

<span data-ttu-id="02cb7-110">Ebben az oktatóanyagban végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="02cb7-110">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="02cb7-111">Telepítse a távoli felügyeleti előkonfigurált megoldás egy példányát az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="02cb7-111">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="02cb7-112">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="02cb7-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="02cb7-113">Állítsa be az eszköz és a érzékelők kommunikáljanak a számítógép és a távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="02cb7-113">Set up your device and sensors to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="02cb7-114">Frissítse a mintakódot eszköz csatlakozni a távoli felügyeleti megoldás, és, amely megtalálható a megoldás irányítópultja telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="02cb7-114">Update the sample device code to connect to the remote monitoring solution, and send telemetry that you can view on the solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="02cb7-115">A távoli felügyeleti megoldás látja el az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="02cb7-115">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="02cb7-116">A központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="02cb7-116">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="02cb7-117">Szükségtelen Azure felhasználási díjak elkerülése az előre konfigurált megoldást a következő azureiotsuite.com a példányának törlése után vele.</span><span class="sxs-lookup"><span data-stu-id="02cb7-117">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="02cb7-118">Ha újra kell az előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="02cb7-118">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="02cb7-119">További információ a felhasználás csökkentése a távoli felügyeleti megoldás futtatása közben: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="02cb7-119">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="02cb7-120">Töltse le és konfigurálja a minta</span><span class="sxs-lookup"><span data-stu-id="02cb7-120">Download and configure the sample</span></span>

<span data-ttu-id="02cb7-121">Most töltse le és konfigurálja a távoli felügyeleti ügyfélalkalmazás a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="02cb7-121">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="02cb7-122">A Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="02cb7-122">Install Node.js</span></span>

<span data-ttu-id="02cb7-123">Node.js telepíthető a Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="02cb7-123">Install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="02cb7-124">Az IoT-SDK for Node.js 0.11.5 Node.js vagy újabb verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="02cb7-124">The IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="02cb7-125">A következő lépések bemutatják a Node.js v6.10.2 telepíthető a málna Pi:</span><span class="sxs-lookup"><span data-stu-id="02cb7-125">The following steps show you how to install Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="02cb7-126">A málna Pi frissítéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="02cb7-126">Use the following command to update your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="02cb7-127">Az alábbi parancs segítségével töltse le a Node.js bináris fájlok a málna Pi:</span><span class="sxs-lookup"><span data-stu-id="02cb7-127">Use the following command to download the Node.js binaries to your Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="02cb7-128">A következő paranccsal telepítse a bináris fájlok:</span><span class="sxs-lookup"><span data-stu-id="02cb7-128">Use the following command to install the binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="02cb7-129">A következő paranccsal ellenőrizheti a Node.js v6.10.2 sikeresen telepítette:</span><span class="sxs-lookup"><span data-stu-id="02cb7-129">Use the following command to verify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a><span data-ttu-id="02cb7-130">A tárolóhelyekkel klónozása</span><span class="sxs-lookup"><span data-stu-id="02cb7-130">Clone the repositories</span></span>

<span data-ttu-id="02cb7-131">Ha még nem tette meg, klónozza a szükséges adattárak a Pi a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="02cb7-131">If you haven't already done so, clone the required repositories by running the following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="02cb7-132">Frissítés az eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="02cb7-132">Update the device connection string</span></span>

<span data-ttu-id="02cb7-133">Nyissa meg a minta a fájlt a **nano** szerkesztő a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="02cb7-133">Open the sample source file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="02cb7-134">Keresse meg a sor:</span><span class="sxs-lookup"><span data-stu-id="02cb7-134">Find the line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="02cb7-135">Cserélje le a helyőrző értékeket az eszköz és az IoT-központ adatokat létrehozott és mentett Ez az oktatóanyag elején.</span><span class="sxs-lookup"><span data-stu-id="02cb7-135">Replace the placeholder values with the device and IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="02cb7-136">Menti a módosításokat (**Ctrl-O**, **Enter**), és zárja be a szerkesztőt (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="02cb7-136">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="02cb7-137">A minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="02cb7-137">Run the sample</span></span>

<span data-ttu-id="02cb7-138">A következő parancsokat a csomagokat a minta telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="02cb7-138">Run the following commands to install the prerequisite packages for the sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic
npm install
```

<span data-ttu-id="02cb7-139">A minta program most futtathatja a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="02cb7-139">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="02cb7-140">Adja meg a parancsot:</span><span class="sxs-lookup"><span data-stu-id="02cb7-140">Enter the command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="02cb7-141">A következő minta kimenet látható egy példa a kimeneti látja a málna Pi a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="02cb7-141">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

<span data-ttu-id="02cb7-143">Nyomja le az **Ctrl-C** kilép a programból bármikor.</span><span class="sxs-lookup"><span data-stu-id="02cb7-143">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="02cb7-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02cb7-144">Next steps</span></span>

<span data-ttu-id="02cb7-145">Látogasson el a [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="02cb7-145">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
