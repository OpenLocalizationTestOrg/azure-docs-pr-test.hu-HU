---
title: "Csatlakozás egy málna Pi Azure IoT Suite Node.js használatával szimulált telemetriai |} Microsoft Docs"
description: "Használja a Microsoft Azure IoT Starter Kit a Raspberry pi 3 és az Azure IoT Suite. A málna Pi kapcsolódni a távoli felügyeleti megoldás használata Node.js szimulált telemetriai adatokat küldhet a felhőhöz, és a megoldás irányítópultja metódusokra válaszol."
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
ms.openlocfilehash: 43fbfe2f2c5fb86e496c4419f9565365cf89830a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a><span data-ttu-id="bda57-104">A málna Pi 3 kapcsolódni a távoli felügyeleti megoldás és a Node.js segítségével szimulált telemetriai adatokat küldhet</span><span class="sxs-lookup"><span data-stu-id="bda57-104">Connect your Raspberry Pi 3 to the remote monitoring solution and send simulated telemetry using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="bda57-105">Az oktatóanyag bemutatja, hogyan használható a málna Pi 3 szimulálása a hőmérséklet és a páratartalom a felhőbe küldött adatok.</span><span class="sxs-lookup"><span data-stu-id="bda57-105">This tutorial shows you how to use the Raspberry Pi 3 to simulate temperature and humidity data to send to the cloud.</span></span> <span data-ttu-id="bda57-106">Az oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="bda57-106">The tutorial uses:</span></span>

- <span data-ttu-id="bda57-107">Raspbian operációs rendszer, a Node.js programozási nyelv, és a Microsoft Azure IoT SDK for Node.js egy minta eszköz végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="bda57-107">Raspbian OS, the Node.js programming language, and the Microsoft Azure IoT SDK for Node.js to implement a sample device.</span></span>
- <span data-ttu-id="bda57-108">Az IoT Suite távoli megfigyelési előre konfigurált megoldás, a felhő alapú háttér.</span><span class="sxs-lookup"><span data-stu-id="bda57-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="bda57-109">A távoli felügyeleti megoldás látja el az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="bda57-109">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="bda57-110">A központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="bda57-110">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="bda57-111">Szükségtelen Azure felhasználási díjak elkerülése az előre konfigurált megoldást a következő azureiotsuite.com a példányának törlése után vele.</span><span class="sxs-lookup"><span data-stu-id="bda57-111">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="bda57-112">Ha újra kell az előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="bda57-112">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="bda57-113">További információ a felhasználás csökkentése a távoli felügyeleti megoldás futtatása közben: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="bda57-113">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="bda57-114">Töltse le és konfigurálja a minta</span><span class="sxs-lookup"><span data-stu-id="bda57-114">Download and configure the sample</span></span>

<span data-ttu-id="bda57-115">Most töltse le és konfigurálja a távoli felügyeleti ügyfélalkalmazás a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="bda57-115">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="bda57-116">A Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="bda57-116">Install Node.js</span></span>

<span data-ttu-id="bda57-117">Ha még nem tette meg, telepítse a málna Pi Node.js.</span><span class="sxs-lookup"><span data-stu-id="bda57-117">If you haven't done so already, install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="bda57-118">Az IoT-SDK for Node.js 0.11.5 Node.js vagy újabb verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="bda57-118">The IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="bda57-119">A következő lépések bemutatják a Node.js v6.10.2 telepíthető a málna Pi:</span><span class="sxs-lookup"><span data-stu-id="bda57-119">The following steps show you how to install Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="bda57-120">A málna Pi frissítéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bda57-120">Use the following command to update your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="bda57-121">Az alábbi parancs segítségével töltse le a Node.js bináris fájlok a málna Pi:</span><span class="sxs-lookup"><span data-stu-id="bda57-121">Use the following command to download the Node.js binaries to your Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="bda57-122">A következő paranccsal telepítse a bináris fájlok:</span><span class="sxs-lookup"><span data-stu-id="bda57-122">Use the following command to install the binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="bda57-123">A következő paranccsal ellenőrizheti a Node.js v6.10.2 sikeresen telepítette:</span><span class="sxs-lookup"><span data-stu-id="bda57-123">Use the following command to verify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a><span data-ttu-id="bda57-124">A tárolóhelyekkel klónozása</span><span class="sxs-lookup"><span data-stu-id="bda57-124">Clone the repositories</span></span>

<span data-ttu-id="bda57-125">Ha még nem tette meg, klónozza a szükséges tárházak találhatók a következő parancsok futtatásával parancsot egy terminálban a Pi:</span><span class="sxs-lookup"><span data-stu-id="bda57-125">If you haven't already done so, clone the required repositories by running the following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="bda57-126">Frissítés az eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bda57-126">Update the device connection string</span></span>

<span data-ttu-id="bda57-127">Nyissa meg a minta a fájlt a **nano** szerkesztő a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bda57-127">Open the sample source file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

<span data-ttu-id="bda57-128">Keresse meg a sor:</span><span class="sxs-lookup"><span data-stu-id="bda57-128">Find the line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="bda57-129">Cserélje le a helyőrző értékeket az eszköz és az IoT-központ adatokat létrehozott és mentett Ez az oktatóanyag elején.</span><span class="sxs-lookup"><span data-stu-id="bda57-129">Replace the placeholder values with the device and IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="bda57-130">Menti a módosításokat (**Ctrl-O**, **Enter**), és zárja be a szerkesztőt (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="bda57-130">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="bda57-131">A minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="bda57-131">Run the sample</span></span>

<span data-ttu-id="bda57-132">A következő parancsokat a csomagokat a minta telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="bda57-132">Run the following commands to install the prerequisite packages for the sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

<span data-ttu-id="bda57-133">A minta program most futtathatja a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="bda57-133">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="bda57-134">Adja meg a parancsot:</span><span class="sxs-lookup"><span data-stu-id="bda57-134">Enter the command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

<span data-ttu-id="bda57-135">A következő minta kimenet látható egy példa a kimeneti látja a málna Pi a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="bda57-135">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

<span data-ttu-id="bda57-137">Nyomja le az **Ctrl-C** kilép a programból bármikor.</span><span class="sxs-lookup"><span data-stu-id="bda57-137">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a><span data-ttu-id="bda57-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bda57-138">Next steps</span></span>

<span data-ttu-id="bda57-139">Látogasson el a [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="bda57-139">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
