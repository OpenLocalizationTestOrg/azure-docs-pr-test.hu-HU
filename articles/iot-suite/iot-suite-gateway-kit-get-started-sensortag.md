---
title: "egy átjáró tooAzure használatával az Intel NUC IoT Suite aaaConnect |} Microsoft Docs"
description: "Hello Microsoft IoT kereskedelmi átjáró Kit és hello távoli figyelési előkonfigurált megoldást használni. Használjon hello Azure IoT peremhálózati átjáró tooenable egy SensorTag eszköz tooconnect toohello távoli felügyeleti megoldás, telemetriai toohello felhő küldése és meghívni a hello megoldás irányítópultja toomethods válaszol."
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
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 6f98ee3c1e2311a8644da9d72d40e671e7cbcf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a><span data-ttu-id="1f0ff-104">Csatlakozzon a távoli felügyeleti előkonfigurált megoldás Azure IoT peremhálózati átjáró toohello és telemetriai adatokat küldhet egy SensorTag a</span><span class="sxs-lookup"><span data-stu-id="1f0ff-104">Connect your Azure IoT Edge gateway toohello remote monitoring preconfigured solution and send telemetry from a SensorTag</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="1f0ff-105">Az oktatóanyag bemutatja, hogyan toouse Azure IoT peremhálózati toosend hőmérséklet és a páratartalom adatait SensorTag toohello távoli eszközfigyelésre előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-105">This tutorial shows you how toouse Azure IoT Edge toosend temperature and humidity data from SensorTag device toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="1f0ff-106">hello SensorTag toohello Intel NUC átjáró Bluetooth használatával csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-106">hello SensorTag connects toohello Intel NUC gateway using Bluetooth.</span></span> <span data-ttu-id="1f0ff-107">hello oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-107">hello tutorial uses:</span></span>

- <span data-ttu-id="1f0ff-108">Az Azure IoT peremhálózati tooimplement minta átjáró.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-108">Azure IoT Edge tooimplement a sample gateway.</span></span>
- <span data-ttu-id="1f0ff-109">mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-109">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="1f0ff-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1f0ff-110">Overview</span></span>

<span data-ttu-id="1f0ff-111">Az oktatóanyag befejezése hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-111">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="1f0ff-112">Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-112">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="1f0ff-113">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-113">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="1f0ff-114">Az Intel NUC átjáró eszköz toocommunicate a számítógép és a távoli felügyeleti megoldás hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-114">Set up your Intel NUC gateway device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="1f0ff-115">Az Intel NUC átjáró tooreceive telemetriai beállítása az SensorTag eszközökről, és elküldi a toohello távoli figyelési irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-115">Set up your Intel NUC gateway tooreceive telemetry from a SensorTag device and send it toohello remote monitoring dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

<span data-ttu-id="1f0ff-116">[Texas eszközök BLA SensorTag][lnk-sensortag].</span><span class="sxs-lookup"><span data-stu-id="1f0ff-116">[Texas Instruments BLE SensorTag][lnk-sensortag].</span></span> <span data-ttu-id="1f0ff-117">Ez az oktatóanyag telemetriai adatokat lekérdezi hello SensorTag eszköz Bluetooth-on keresztül.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-117">This tutorial retrieves telemetry data over Bluetooth from hello SensorTag device.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="1f0ff-118">távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-118">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="1f0ff-119">hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-119">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="1f0ff-120">tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-120">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="1f0ff-121">Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-121">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="1f0ff-122">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="1f0ff-122">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a><span data-ttu-id="1f0ff-123">Bluetooth-kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1f0ff-123">Configure Bluetooth connectivity</span></span>

<span data-ttu-id="1f0ff-124">Bluetooth szolgáltatást hello Intel NUC tooenable hello SensorTag eszköz tooconnect, telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-124">Configure Bluetooth on hello Intel NUC tooenable hello SensorTag device tooconnect and send telemetry.</span></span>

### <a name="find-hello-mac-address-of-hello-sensortag"></a><span data-ttu-id="1f0ff-125">A hello SensorTag hello MAC-cím keresése</span><span class="sxs-lookup"><span data-stu-id="1f0ff-125">Find hello MAC address of hello SensorTag</span></span>

1. <span data-ttu-id="1f0ff-126">Hello Intel NUC hello rendszerhéjat futtassa a következő parancs toounblock hello Bluetooth szolgáltatás hello:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-126">In hello shell on hello Intel NUC, run hello following command toounblock hello Bluetooth service:</span></span>

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. <span data-ttu-id="1f0ff-127">Futtatási hello következő parancsok toostart hello Bluetooth Intel NUC hello szolgáltatást, és írja be a hello Bluetooth rendszerhéj:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-127">Run hello following commands toostart hello Bluetooth service on hello Intel NUC and enter hello Bluetooth shell:</span></span>

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="1f0ff-128">Futtassa a következő parancs toopower hello Bluetooth tartományvezérlőn hello:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-128">Run hello following command toopower on hello Bluetooth controller:</span></span>

    ```bash
    power on
    ```

    <span data-ttu-id="1f0ff-129">Ha hello, tartományvezérlői, megjelenik egy üzenet **power módosítása a sikeres**.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-129">When hello controller is on, you see a message **Changing power on succeeded**.</span></span>

1. <span data-ttu-id="1f0ff-130">Futtassa a következő parancs tooscan a Bluetooth-eszközök közelben hello:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-130">Run hello following command tooscan for nearby Bluetooth devices:</span></span>

    ```bash
    scan on
    ```

1. <span data-ttu-id="1f0ff-131">Nyomja le az hello power gombra a hello SensorTag toomake felderíthető azt.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-131">Press hello power button on hello SensorTag toomake it discoverable.</span></span> <span data-ttu-id="1f0ff-132">zöld hello LED tokenkódot.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-132">hello green LED flashes.</span></span>

1. <span data-ttu-id="1f0ff-133">Amikor megjelenik egy üzenet hello rendszerhéj adott hello vezérlő talált hello SensorTag, jegyezze fel a hello hello eszköz MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-133">When you see a message in hello shell that hello controller has discovered hello SensorTag, make a note of hello MAC address of hello device.</span></span> <span data-ttu-id="1f0ff-134">hello MAC-cím a következőképpen néz **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-134">hello MAC address looks like **A0:E6:F8:B5:F6:00**.</span></span> <span data-ttu-id="1f0ff-135">Hello MAC-cím hello oktatóanyag későbbi részében szüksége, hello átjáró konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-135">You need hello MAC address later in hello tutorial when you configure hello gateway.</span></span>

1. <span data-ttu-id="1f0ff-136">Futtassa a következő parancs tooturn ki Bluetooth ellenőrzését hello:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-136">Run hello following command tooturn off Bluetooth scanning:</span></span>

    ```bash
    scan off
    ```

1. <span data-ttu-id="1f0ff-137">Futtassa a következő parancs tooverify, hogy csatlakozhasson toohello SensorTag eszköz hello:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-137">Run hello following command tooverify that you can connect toohello SensorTag device:</span></span>

    ```bash
    connect <SensorTag MAC address>
    ```

    <span data-ttu-id="1f0ff-138">Ha a csatlakozás sikeres, a hello rendszerhéj hello üzenetet jelenít meg **sikeres kapcsolat** majd kinyomtatja hello SensorTag eszközére vonatkozó információt.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-138">If you connect successfully, hello shell shows hello message **Connection successful** and prints information about hello SensorTag device.</span></span> <span data-ttu-id="1f0ff-139">Ha nem sikerül, ellenőrizze a hello SensorTag továbbra is be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-139">If you cannot connect, check hello SensorTag is still powered on.</span></span>

1. <span data-ttu-id="1f0ff-140">Most hello SensorTag válassza le, és lépjen ki a hello Bluetooth rendszerhéj hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-140">You can now disconnect from hello SensorTag and exit hello Bluetooth shell by running hello following commands:</span></span>

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a><span data-ttu-id="1f0ff-141">Hello egyéni IoT peremhálózati modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f0ff-141">Build hello custom IoT Edge module</span></span>

<span data-ttu-id="1f0ff-142">Most már lefordíthatja hello egyéni IoT peremhálózati modul, amely lehetővé teszi a hello átjáró toosend üzenetek toohello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-142">You can now build hello custom IoT Edge module that enables hello gateway toosend messages toohello remote monitoring solution.</span></span> <span data-ttu-id="1f0ff-143">Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="1f0ff-143">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="1f0ff-144">Hello egyéni IoT peremhálózati modulok hello forráskódja letölthető GitHub hello a következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-144">Download hello source code for hello custom IoT Edge modules from GitHub using hello following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="1f0ff-145">Build hello hello a következő parancsok használatával egyéni IoT peremhálózati modult:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-145">Build hello custom IoT Edge module using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="1f0ff-146">hello build script hello libsensor2remotemonitoring.so egyéni IoT peremhálózati modul hello build mappába helyezi.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-146">hello build script places hello libsensor2remotemonitoring.so custom IoT Edge module in hello build folder.</span></span>

## <a name="configure-and-run-hello-iot-edge-gateway"></a><span data-ttu-id="1f0ff-147">Konfigurálására és futtatására hello IoT peremhálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="1f0ff-147">Configure and run hello IoT Edge gateway</span></span>

<span data-ttu-id="1f0ff-148">Most már a SensorTag eszköz tooyour távoli figyelési irányítópult konfigurálhatja hello IoT peremhálózati átjáró toosend telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-148">You can now configure hello IoT Edge gateway toosend telemetry from your SensorTag device tooyour remote monitoring dashboard.</span></span> <span data-ttu-id="1f0ff-149">Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="1f0ff-149">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="1f0ff-150">Ebben az oktatóanyagban hello szabvány használata `vi` hello Intel NUC a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-150">In this tutorial, you use hello standard `vi` text editor on hello Intel NUC.</span></span> <span data-ttu-id="1f0ff-151">Ha még nem használta `vi` , el kell végeznie egy bevezető oktatóanyag, például a [Unix - hello vi szerkesztő oktatóanyag] [ lnk-vi-tutorial] toofamiliarize saját kezűleg a szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-151">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - hello vi Editor Tutorial][lnk-vi-tutorial] toofamiliarize yourself with this editor.</span></span> <span data-ttu-id="1f0ff-152">Másik lehetőségként telepítése könnyebben hello [nano](https://www.nano-editor.org/) hello paranccsal szerkesztő `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-152">Alternatively, you can install hello more user-friendly [nano](https://www.nano-editor.org/) editor using hello command `smart install nano -y`.</span></span>

<span data-ttu-id="1f0ff-153">Nyissa meg hello minta konfigurációs fájlt a hello **vi** szerkesztő hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-153">Open hello sample configuration file in hello **vi** editor using hello following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

<span data-ttu-id="1f0ff-154">Keresse meg az alábbi hello konfigurációban hello IOT hubbal modul hello:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-154">Locate hello following lines in hello configuration for hello IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="1f0ff-155">Cserélje le a hello helyőrző értékeket az IoT-központ adatokat létrehozott és mentett hello hello elejére ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-155">Replace hello placeholder values with hello IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="1f0ff-156">hello érték IoTHubName a következőhöz hasonló **yourrmsolution37e08**, és hello IoTSuffix értéke általában **azure-devices.net**.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-156">hello value for IoTHubName looks like **yourrmsolution37e08**, and hello value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="1f0ff-157">Keresse meg az alábbi hello konfigurációban a hello hozzárendelési module hello:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-157">Locate hello following lines in hello configuration for hello mapping module:</span></span>

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

<span data-ttu-id="1f0ff-158">Cserélje le a hello **macAddress** helyőrzőt hello a korábban feljegyzett SensorTag MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-158">Replace hello **macAddress** placeholder with hello MAC address of your SensorTag you noted previously.</span></span> <span data-ttu-id="1f0ff-159">Cserélje le a hello **deviceID** és **deviceKey** helyőrzőt hello azonosítók és kulcsok hello távoli figyelési megoldást korábban létrehozott hello két eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-159">Replace hello **deviceID** and **deviceKey** placeholders with hello IDs and keys for hello two devices you created in hello remote monitoring solution previously.</span></span>

<span data-ttu-id="1f0ff-160">Keresse meg az alábbi hello konfigurációban hello SensorTag modul hello:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-160">Locate hello following lines in hello configuration for hello SensorTag module:</span></span>

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

<span data-ttu-id="1f0ff-161">Cserélje le a hello **eszköz\_mac\_cím** helyőrzőt hello a korábban feljegyzett SensorTag MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-161">Replace hello **device\_mac\_address** placeholder  with hello MAC address of your SensorTag you noted previously.</span></span>

<span data-ttu-id="1f0ff-162">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-162">Save your changes.</span></span>

<span data-ttu-id="1f0ff-163">Most futtathatja hello átjáró hello a következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-163">You can now run hello gateway using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

<span data-ttu-id="1f0ff-164">hello IoT peremhálózati átjáró hello Intel NUC kezdődik, és telemetriai küldi hello SensorTag toohello távoli figyelési megoldást:</span><span class="sxs-lookup"><span data-stu-id="1f0ff-164">hello IoT Edge gateway starts on hello Intel NUC and sends telemetry from hello SensorTag toohello remote monitoring solution:</span></span>

![Az IoT-átjárónak telemetriai küldi hello SensorTag][img-telemetry]

<span data-ttu-id="1f0ff-166">Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-166">Press **Ctrl-C** tooexit hello program at any time.</span></span>

## <a name="view-hello-telemetry"></a><span data-ttu-id="1f0ff-167">Nézet hello telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="1f0ff-167">View hello telemetry</span></span>

<span data-ttu-id="1f0ff-168">hello átjáró most hello SensorTag toohello távoli felügyeleti megoldás a telemetriai adatokat küld.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-168">hello gateway is now sending telemetry from hello SensorTag device toohello remote monitoring solution.</span></span> <span data-ttu-id="1f0ff-169">Hello telemetriai hello megoldás irányítópultján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-169">You can view hello telemetry on hello solution dashboard.</span></span> <span data-ttu-id="1f0ff-170">Parancsok tooyour SensorTag eszköz hello átjárón keresztül hello megoldás irányítópultról is küldhet.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-170">You can also send commands tooyour SensorTag device through hello gateway from hello solution dashboard.</span></span>

- <span data-ttu-id="1f0ff-171">Keresse meg a toohello megoldás irányítópultja.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-171">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="1f0ff-172">Jelölje be hello eszköz hello SensorTag hello a jelölő hello átjárót konfigurált **eszköz tooView** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-172">Select hello device you configured in hello gateway that represents hello SensorTag in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="1f0ff-173">hello telemetriai hello SensorTag eszközről hello irányítópulton jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-173">hello telemetry from hello SensorTag device displays on hello dashboard.</span></span>

![Hello SensorTag eszközökről telemetriai adatainak megjelenítése][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="1f0ff-175">Ha nem adja meg hello távoli felügyeleti megoldás az Azure-fiókjával fut, a hello futásakor kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-175">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="1f0ff-176">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="1f0ff-176">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="1f0ff-177">Ha befejezte, használja előre konfigurált hello megoldás törlése az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-177">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1f0ff-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f0ff-178">Next steps</span></span>

<span data-ttu-id="1f0ff-179">A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="1f0ff-179">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started