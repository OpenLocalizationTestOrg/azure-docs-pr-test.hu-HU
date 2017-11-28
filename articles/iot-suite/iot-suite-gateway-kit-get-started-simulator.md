---
title: "egy átjáró tooAzure használatával az Intel NUC IoT Suite aaaConnect |} Microsoft Docs"
description: "Hello Microsoft IoT kereskedelmi átjáró Kit és hello távoli figyelési előkonfigurált megoldást használni. Használjon hello Azure IoT peremhálózati átjáró tooconnect toohello távoli felügyeleti megoldás, szimulált telemetriai toohello felhő küldhet, és válaszolhat a toomethods hello megoldás irányítópultja meghívni."
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
ms.openlocfilehash: 46b545fc21b054191c8f78ace20fc628f839a819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a><span data-ttu-id="91ccb-104">Csatlakozzon a távoli felügyeleti előkonfigurált megoldás Azure IoT peremhálózati átjáró toohello és szimulált telemetriai adatokat küldhet</span><span class="sxs-lookup"><span data-stu-id="91ccb-104">Connect your Azure IoT Edge gateway toohello remote monitoring preconfigured solution and send simulated telemetry</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="91ccb-105">Az oktatóanyag bemutatja, hogyan toouse Azure IoT peremhálózati toosimulate hőmérséklet és a páratartalom adatok toosend toohello távoli felügyelet előre megoldás.</span><span class="sxs-lookup"><span data-stu-id="91ccb-105">This tutorial shows you how toouse Azure IoT Edge toosimulate temperature and humidity data toosend toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="91ccb-106">hello oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="91ccb-106">hello tutorial uses:</span></span>

- <span data-ttu-id="91ccb-107">Az Azure IoT peremhálózati tooimplement minta átjáró.</span><span class="sxs-lookup"><span data-stu-id="91ccb-107">Azure IoT Edge tooimplement a sample gateway.</span></span>
- <span data-ttu-id="91ccb-108">mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="91ccb-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="91ccb-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="91ccb-109">Overview</span></span>

<span data-ttu-id="91ccb-110">Az oktatóanyag befejezése hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="91ccb-110">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="91ccb-111">Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése.</span><span class="sxs-lookup"><span data-stu-id="91ccb-111">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="91ccb-112">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="91ccb-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="91ccb-113">Az Intel NUC átjáró eszköz toocommunicate a számítógép és a távoli felügyeleti megoldás hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="91ccb-113">Set up your Intel NUC gateway device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="91ccb-114">Konfigurálja a hello IoT peremhálózati átjáró toosend szimulált telemetriai adatokból, hogy a hello megoldás irányítópultja tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="91ccb-114">Configure hello IoT Edge gateway toosend simulated telemetry that you can view on hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="91ccb-115">távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="91ccb-115">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="91ccb-116">hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="91ccb-116">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="91ccb-117">tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után.</span><span class="sxs-lookup"><span data-stu-id="91ccb-117">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="91ccb-118">Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="91ccb-118">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="91ccb-119">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="91ccb-119">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

<span data-ttu-id="91ccb-120">Ismételje meg a hello előző lépéseket tooadd egy második eszköz, például egy eszköz azonosítójával **device02**.</span><span class="sxs-lookup"><span data-stu-id="91ccb-120">Repeat hello previous steps tooadd a second device using a Device ID such as **device02**.</span></span> <span data-ttu-id="91ccb-121">hello minta adatot küld a két szimulált eszköz hello átjáró toohello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="91ccb-121">hello sample sends data from two simulated devices in hello gateway toohello remote monitoring solution.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a><span data-ttu-id="91ccb-122">Hello egyéni IoT peremhálózati modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="91ccb-122">Build hello custom IoT Edge module</span></span>

<span data-ttu-id="91ccb-123">Most már lefordíthatja hello egyéni IoT peremhálózati modul, amely lehetővé teszi a hello átjáró toosend üzenetek toohello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="91ccb-123">You can now build hello custom IoT Edge module that enables hello gateway toosend messages toohello remote monitoring solution.</span></span> <span data-ttu-id="91ccb-124">Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="91ccb-124">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="91ccb-125">Hello egyéni IoT peremhálózati modulok hello forráskódja letölthető GitHub hello a következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="91ccb-125">Download hello source code for hello custom IoT Edge modules from GitHub using hello following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="91ccb-126">Build hello hello a következő parancsok használatával egyéni IoT peremhálózati modult:</span><span class="sxs-lookup"><span data-stu-id="91ccb-126">Build hello custom IoT Edge module using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="91ccb-127">hello build script hello libsimulator.so egyéni IoT peremhálózati modul hello build mappába helyezi.</span><span class="sxs-lookup"><span data-stu-id="91ccb-127">hello build script places hello libsimulator.so custom IoT Edge module in hello build folder.</span></span>

## <a name="configure-and-run-hello-iot-edge-gateway"></a><span data-ttu-id="91ccb-128">Konfigurálására és futtatására hello IoT peremhálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="91ccb-128">Configure and run hello IoT Edge gateway</span></span>

<span data-ttu-id="91ccb-129">Hello IoT peremhálózati átjáró toosend szimulált telemetriai tooyour távoli figyelési irányítópult mostantól konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="91ccb-129">You can now configure hello IoT Edge gateway toosend simulated telemetry tooyour remote monitoring dashboard.</span></span> <span data-ttu-id="91ccb-130">Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="91ccb-130">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="91ccb-131">Ebben az oktatóanyagban hello szabvány használata `vi` hello Intel NUC a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="91ccb-131">In this tutorial, you use hello standard `vi` text editor on hello Intel NUC.</span></span> <span data-ttu-id="91ccb-132">Ha még nem használta `vi` , el kell végeznie egy bevezető oktatóanyag, például a [Unix - hello vi szerkesztő oktatóanyag] [ lnk-vi-tutorial] toofamiliarize saját kezűleg a szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="91ccb-132">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - hello vi Editor Tutorial][lnk-vi-tutorial] toofamiliarize yourself with this editor.</span></span> <span data-ttu-id="91ccb-133">Másik lehetőségként telepítése könnyebben hello [nano](https://www.nano-editor.org/) hello paranccsal szerkesztő `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="91ccb-133">Alternatively, you can install hello more user-friendly [nano](https://www.nano-editor.org/) editor using hello command `smart install nano -y`.</span></span>

<span data-ttu-id="91ccb-134">Nyissa meg hello minta konfigurációs fájlt a hello **vi** szerkesztő hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="91ccb-134">Open hello sample configuration file in hello **vi** editor using hello following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
```

<span data-ttu-id="91ccb-135">Keresse meg az alábbi hello konfigurációban hello IOT hubbal modul hello:</span><span class="sxs-lookup"><span data-stu-id="91ccb-135">Locate hello following lines in hello configuration for hello IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="91ccb-136">Cserélje le a hello helyőrző értékeket az IoT-központ adatokat létrehozott és mentett hello hello elejére ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="91ccb-136">Replace hello placeholder values with hello IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="91ccb-137">hello érték IoTHubName a következőhöz hasonló **yourrmsolution37e08**, és hello IoTSuffix értéke általában **azure-devices.net**.</span><span class="sxs-lookup"><span data-stu-id="91ccb-137">hello value for IoTHubName looks like **yourrmsolution37e08**, and hello value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="91ccb-138">Keresse meg az alábbi hello konfigurációban a hello hozzárendelési module hello:</span><span class="sxs-lookup"><span data-stu-id="91ccb-138">Locate hello following lines in hello configuration for hello mapping module:</span></span>

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

<span data-ttu-id="91ccb-139">Cserélje le a hello **deviceID** és **deviceKey** helyőrzőt hello azonosítók és kulcsok hello távoli figyelési megoldást korábban létrehozott hello két eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="91ccb-139">Replace hello **deviceID** and **deviceKey** placeholders with hello IDs and keys for hello two devices you created in hello remote monitoring solution previously.</span></span>

<span data-ttu-id="91ccb-140">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="91ccb-140">Save your changes.</span></span>

<span data-ttu-id="91ccb-141">Most futtathatja hello IoT peremhálózati átjáró használatával hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="91ccb-141">You can now run hello IoT Edge gateway using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

<span data-ttu-id="91ccb-142">hello átjáró hello Intel NUC kezdődik, és elküldi a szimulált telemetriai toohello távoli figyelési megoldást:</span><span class="sxs-lookup"><span data-stu-id="91ccb-142">hello gateway starts on hello Intel NUC and sends simulated telemetry toohello remote monitoring solution:</span></span>

![Az IoT-átjárónak állít elő, szimulált telemetriai adat][img-simulated telemetry]

<span data-ttu-id="91ccb-144">Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.</span><span class="sxs-lookup"><span data-stu-id="91ccb-144">Press **Ctrl-C** tooexit hello program at any time.</span></span>

## <a name="view-hello-telemetry"></a><span data-ttu-id="91ccb-145">Nézet hello telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="91ccb-145">View hello telemetry</span></span>

<span data-ttu-id="91ccb-146">hello IoT peremhálózati átjáró most küld szimulált telemetriai toohello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="91ccb-146">hello IoT Edge gateway is now sending simulated telemetry toohello remote monitoring solution.</span></span> <span data-ttu-id="91ccb-147">Hello telemetriai hello megoldás irányítópultján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="91ccb-147">You can view hello telemetry on hello solution dashboard.</span></span>

- <span data-ttu-id="91ccb-148">Keresse meg a toohello megoldás irányítópultja.</span><span class="sxs-lookup"><span data-stu-id="91ccb-148">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="91ccb-149">Válasszon ki egy konfigurált hello hello átjáró két hello eszközök **eszköz tooView** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="91ccb-149">Select one of hello two devices you configured in hello gateway in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="91ccb-150">hello telemetriai hello átjáró eszközökről hello irányítópulton jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="91ccb-150">hello telemetry from hello gateway devices displays on hello dashboard.</span></span>

![A szimulált hello átjáró eszközökről telemetriai megjelenítése][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="91ccb-152">Ha nem adja meg hello távoli felügyeleti megoldás az Azure-fiókjával fut, a hello futásakor kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="91ccb-152">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="91ccb-153">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="91ccb-153">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="91ccb-154">Ha befejezte, használja előre konfigurált hello megoldás törlése az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="91ccb-154">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91ccb-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91ccb-155">Next steps</span></span>

<span data-ttu-id="91ccb-156">A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="91ccb-156">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started