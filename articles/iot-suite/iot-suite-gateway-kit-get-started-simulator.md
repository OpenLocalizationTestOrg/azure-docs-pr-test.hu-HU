---
title: "Csatlakozás Azure IoT Suite használatával az Intel NUC átjáró |} Microsoft Docs"
description: "A Microsoft IoT kereskedelmi átjáró Kit és a távoli figyelési előkonfigurált megoldást használni. Az Azure IoT peremhálózati átjáró használatával kapcsolódni a távoli felügyeleti megoldás, szimulált telemetriai adatokat küldeni a felhőben és a megoldás irányítópultja metódusokra válaszolni."
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
ms.openlocfilehash: 9ed57d3c23e2adbd42c054f33c8ed46e3d6c9792
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a><span data-ttu-id="c0c53-104">Csatlakozzon a távoli figyelési előkonfigurált megoldást az Azure IoT peremhálózati átjárót, és szimulált telemetriai adatokat küldhet</span><span class="sxs-lookup"><span data-stu-id="c0c53-104">Connect your Azure IoT Edge gateway to the remote monitoring preconfigured solution and send simulated telemetry</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="c0c53-105">Az oktatóanyag bemutatja, ezzel szimulálva a hőmérséklet és a páratartalom adatokat küldeni a távoli felügyeleti előkonfigurált megoldás Azure IoT Edge használata.</span><span class="sxs-lookup"><span data-stu-id="c0c53-105">This tutorial shows you how to use Azure IoT Edge to simulate temperature and humidity data to send to the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="c0c53-106">Az oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="c0c53-106">The tutorial uses:</span></span>

- <span data-ttu-id="c0c53-107">Az Azure IoT peremhálózati minta átjáró végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="c0c53-107">Azure IoT Edge to implement a sample gateway.</span></span>
- <span data-ttu-id="c0c53-108">Az IoT Suite távoli megfigyelési előre konfigurált megoldás, a felhő alapú háttér.</span><span class="sxs-lookup"><span data-stu-id="c0c53-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="c0c53-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c0c53-109">Overview</span></span>

<span data-ttu-id="c0c53-110">Ebben az oktatóanyagban végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c0c53-110">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="c0c53-111">Telepítse a távoli felügyeleti előkonfigurált megoldás egy példányát az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c0c53-111">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="c0c53-112">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c0c53-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="c0c53-113">Az Intel NUC átjáróeszköz beállítása a számítógép és a távoli felügyeleti megoldás folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="c0c53-113">Set up your Intel NUC gateway device to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="c0c53-114">Konfigurálja az IoT peremhálózati átjáró, amely megtalálható a megoldás irányítópultja szimulált telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="c0c53-114">Configure the IoT Edge gateway to send simulated telemetry that you can view on the solution dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="c0c53-115">A távoli felügyeleti megoldás látja el az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="c0c53-115">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="c0c53-116">A központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="c0c53-116">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="c0c53-117">Szükségtelen Azure felhasználási díjak elkerülése az előre konfigurált megoldást a következő azureiotsuite.com a példányának törlése után vele.</span><span class="sxs-lookup"><span data-stu-id="c0c53-117">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="c0c53-118">Ha újra kell az előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="c0c53-118">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="c0c53-119">További információ a felhasználás csökkentése a távoli felügyeleti megoldás futtatása közben: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="c0c53-119">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

<span data-ttu-id="c0c53-120">Ismételje meg az előző lépéseket egy második eszköz, például egy eszköz azonosítójával **device02**.</span><span class="sxs-lookup"><span data-stu-id="c0c53-120">Repeat the previous steps to add a second device using a Device ID such as **device02**.</span></span> <span data-ttu-id="c0c53-121">A minta elküldi az adatokat a távoli felügyeleti megoldás az átjáró két szimulált eszközökről.</span><span class="sxs-lookup"><span data-stu-id="c0c53-121">The sample sends data from two simulated devices in the gateway to the remote monitoring solution.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a><span data-ttu-id="c0c53-122">Az egyéni IoT peremhálózati modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0c53-122">Build the custom IoT Edge module</span></span>

<span data-ttu-id="c0c53-123">Most már lefordíthatja az egyéni IoT peremhálózati modul, amely lehetővé teszi, hogy az átjáró üzeneteket küldhet a távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="c0c53-123">You can now build the custom IoT Edge module that enables the gateway to send messages to the remote monitoring solution.</span></span> <span data-ttu-id="c0c53-124">Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="c0c53-124">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="c0c53-125">Töltse le az egyéni IoT peremhálózati modulok forráskódja a Githubról a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="c0c53-125">Download the source code for the custom IoT Edge modules from GitHub using the following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="c0c53-126">Hozni az egyéni IoT biztonsági modult, a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="c0c53-126">Build the custom IoT Edge module using the following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="c0c53-127">A build script a build mappában helyezi el a libsimulator.so egyéni IoT peremhálózati modul.</span><span class="sxs-lookup"><span data-stu-id="c0c53-127">The build script places the libsimulator.so custom IoT Edge module in the build folder.</span></span>

## <a name="configure-and-run-the-iot-edge-gateway"></a><span data-ttu-id="c0c53-128">Konfigurálja és futtassa az IoT-átjáró</span><span class="sxs-lookup"><span data-stu-id="c0c53-128">Configure and run the IoT Edge gateway</span></span>

<span data-ttu-id="c0c53-129">Most már konfigurálhatja az IoT peremhálózati átjáró szimulált telemetriai adatokat küldeni a távoli figyelési irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="c0c53-129">You can now configure the IoT Edge gateway to send simulated telemetry to your remote monitoring dashboard.</span></span> <span data-ttu-id="c0c53-130">Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="c0c53-130">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="c0c53-131">Ebben az oktatóanyagban a használja, a standard `vi` az Intel NUC a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="c0c53-131">In this tutorial, you use the standard `vi` text editor on the Intel NUC.</span></span> <span data-ttu-id="c0c53-132">Ha még nem használta `vi` , el kell végeznie egy bevezető oktatóanyag, például a [Unix - a vi szerkesztő oktatóanyag] [ lnk-vi-tutorial] való ismerkedésre a szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="c0c53-132">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - The vi Editor Tutorial][lnk-vi-tutorial] to familiarize yourself with this editor.</span></span> <span data-ttu-id="c0c53-133">Másik lehetőségként telepítheti a könnyebben [nano](https://www.nano-editor.org/) a paranccsal szerkesztő `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="c0c53-133">Alternatively, you can install the more user-friendly [nano](https://www.nano-editor.org/) editor using the command `smart install nano -y`.</span></span>

<span data-ttu-id="c0c53-134">Nyissa meg a minta konfigurációs fájlt a **vi** szerkesztő a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c0c53-134">Open the sample configuration file in the **vi** editor using the following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
```

<span data-ttu-id="c0c53-135">Az IOT hubbal modul konfigurációjában keresse meg a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="c0c53-135">Locate the following lines in the configuration for the IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="c0c53-136">Cserélje le a helyőrző értékeket az IoT-központ adatokat létrehozott és mentett Ez az oktatóanyag elején.</span><span class="sxs-lookup"><span data-stu-id="c0c53-136">Replace the placeholder values with the IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="c0c53-137">IoTHubName értékét a következőképpen néz **yourrmsolution37e08**, és IoTSuffix értéke általában **azure-devices.net**.</span><span class="sxs-lookup"><span data-stu-id="c0c53-137">The value for IoTHubName looks like **yourrmsolution37e08**, and the value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="c0c53-138">A hozzárendelési modul konfigurációjában keresse meg a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="c0c53-138">Locate the following lines in the configuration for the mapping module:</span></span>

```json
args": [
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

<span data-ttu-id="c0c53-139">Cserélje le a **deviceID** és **deviceKey** helyőrzőt azonosítóit és a két eszköz a távoli felügyeleti megoldás korábban létrehozott kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="c0c53-139">Replace the **deviceID** and **deviceKey** placeholders with the IDs and keys for the two devices you created in the remote monitoring solution previously.</span></span>

<span data-ttu-id="c0c53-140">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="c0c53-140">Save your changes.</span></span>

<span data-ttu-id="c0c53-141">Most már futtathatja az IoT peremhálózati átjáró a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="c0c53-141">You can now run the IoT Edge gateway using the following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

<span data-ttu-id="c0c53-142">Az átjáró elindítja az Intel NUC és szimulált telemetriai adatokat küld a távoli figyelési megoldást:</span><span class="sxs-lookup"><span data-stu-id="c0c53-142">The gateway starts on the Intel NUC and sends simulated telemetry to the remote monitoring solution:</span></span>

![Az IoT-átjárónak állít elő, szimulált telemetriai adat][img-simulated telemetry]

<span data-ttu-id="c0c53-144">Nyomja le az **Ctrl-C** kilép a programból bármikor.</span><span class="sxs-lookup"><span data-stu-id="c0c53-144">Press **Ctrl-C** to exit the program at any time.</span></span>

## <a name="view-the-telemetry"></a><span data-ttu-id="c0c53-145">A telemetriai adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="c0c53-145">View the telemetry</span></span>

<span data-ttu-id="c0c53-146">Az IoT-átjárónak szimulált telemetriai most küld a távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="c0c53-146">The IoT Edge gateway is now sending simulated telemetry to the remote monitoring solution.</span></span> <span data-ttu-id="c0c53-147">A telemetriai adatokat a megoldás irányítópultján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c0c53-147">You can view the telemetry on the solution dashboard.</span></span>

- <span data-ttu-id="c0c53-148">Nyissa meg a megoldás irányítópultja.</span><span class="sxs-lookup"><span data-stu-id="c0c53-148">Navigate to the solution dashboard.</span></span>
- <span data-ttu-id="c0c53-149">Válassza ki a két eszköz, az átjárót konfigurált a **nézet eszköz** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="c0c53-149">Select one of the two devices you configured in the gateway in the **Device to View** dropdown.</span></span>
- <span data-ttu-id="c0c53-150">Az átjáró eszközökről telemetriai adatok az irányítópulton jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c0c53-150">The telemetry from the gateway devices displays on the dashboard.</span></span>

![A szimulált átjáró eszközök telemetriai adatok megjelenítéséhez][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="c0c53-152">Ha nem adja meg a távoli figyelési megoldást igényelnek fut az Azure-fiókjával, a futtatásakor a kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="c0c53-152">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="c0c53-153">További információ a felhasználás csökkentése a távoli felügyeleti megoldás futtatása közben: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="c0c53-153">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="c0c53-154">Ha befejezte, használja az előkonfigurált megoldás törlése az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c0c53-154">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0c53-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0c53-155">Next steps</span></span>

<span data-ttu-id="c0c53-156">Látogasson el a [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="c0c53-156">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started