---
title: "egy málna Pi tooAzure aaaConnect IoT Suite segítségével C toosupport belső vezérlőprogram frissítése |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. C tooconnect használata a távoli felügyeleti megoldás málna Pi toohello, telemetriai adatokat küldhet az érzékelők toohello felhő, és hajtsa végre a távoli vezérlőprogram-frissítés."
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
ms.openlocfilehash: 36d39c6d754ddb025fd3f6b74d7795ed907b754c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a><span data-ttu-id="9f9ca-104">Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello, és engedélyezze a távoli belső vezérlőprogram-frissítésekre C használatával</span><span class="sxs-lookup"><span data-stu-id="9f9ca-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and enable remote firmware updates using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="9f9ca-105">Az oktatóanyag bemutatja, hogyan toouse hello Microsoft Azure IoT Starter Kit málna Pi 3:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-105">This tutorial shows you how toouse hello Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to:</span></span>

* <span data-ttu-id="9f9ca-106">A hőmérséklet és a páratartalom olvasó, amely képes kommunikálni a hello felhő fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-106">Develop a temperature and humidity reader that can communicate with hello cloud.</span></span>
* <span data-ttu-id="9f9ca-107">Engedélyezi, és végezze el a távoli belső vezérlőprogram frissítési tooupdate hello ügyfélalkalmazás hello málna Pi.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-107">Enable and perform a remote firmware update tooupdate hello client application on hello Raspberry Pi.</span></span>

<span data-ttu-id="9f9ca-108">hello oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-108">hello tutorial uses:</span></span>

* <span data-ttu-id="9f9ca-109">Az operációs rendszer Raspbian, hello C programozási nyelv, és C tooimplement egy minta eszközt a Microsoft Azure IoT SDK hello.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-109">Raspbian OS, hello C programming language, and hello Microsoft Azure IoT SDK for C tooimplement a sample device.</span></span>
* <span data-ttu-id="9f9ca-110">mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-110">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="9f9ca-111">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9f9ca-111">Overview</span></span>

<span data-ttu-id="9f9ca-112">Az oktatóanyag befejezése hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-112">In this tutorial, you complete hello following steps:</span></span>

* <span data-ttu-id="9f9ca-113">Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-113">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="9f9ca-114">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-114">This step automatically deploys and configures multiple Azure services.</span></span>
* <span data-ttu-id="9f9ca-115">Állítsa be a számítógép és a hello az eszköz és az érzékelők toocommunicate távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-115">Set up your device and sensors toocommunicate with your computer and hello remote monitoring solution.</span></span>
* <span data-ttu-id="9f9ca-116">Hello eszköz kód tooconnect toohello távoli figyelési megoldást frissítése, és megtekintheti a hello megoldás irányítópultja telemetriát.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-116">Update hello sample device code tooconnect toohello remote monitoring solution, and send telemetry that you can view on hello solution dashboard.</span></span>
* <span data-ttu-id="9f9ca-117">Hello minta eszköz kód tooupdate hello-ügyfélalkalmazás használata.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-117">Use hello sample device code tooupdate hello client application.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="9f9ca-118">távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-118">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="9f9ca-119">hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-119">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="9f9ca-120">tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-120">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="9f9ca-121">Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-121">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="9f9ca-122">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="9f9ca-122">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="9f9ca-123">Töltse le és konfigurálja a hello minta</span><span class="sxs-lookup"><span data-stu-id="9f9ca-123">Download and configure hello sample</span></span>

<span data-ttu-id="9f9ca-124">Most töltse le, és a málna Pi hello távoli figyelési ügyfélalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-124">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-hello-repositories"></a><span data-ttu-id="9f9ca-125">Klónozás hello adattárak</span><span class="sxs-lookup"><span data-stu-id="9f9ca-125">Clone hello repositories</span></span>

<span data-ttu-id="9f9ca-126">Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello parancsok követően a Pi:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-126">If you haven't done so already, clone hello required repositories by running hello following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="9f9ca-127">Hello eszköz kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="9f9ca-127">Update hello device connection string</span></span>

<span data-ttu-id="9f9ca-128">Nyissa meg hello minta konfigurációs fájlt a hello **nano** szerkesztő hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-128">Open hello sample configuration file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

<span data-ttu-id="9f9ca-129">Cserélje le hello helyőrző értékeket hello azonosító és az IoT-központ eszközadatokat létrehozott és mentett ebben az oktatóanyagban hello elején.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-129">Replace hello placeholder values with hello device ID and IoT Hub information you created and saved at hello start of this tutorial.</span></span>

<span data-ttu-id="9f9ca-130">Amikor elkészült, hello hello deviceinfo információja fájl tartalmát a következő példa hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-130">When you are done, hello contents of hello deviceinfo file should look like hello following example:</span></span>

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

<span data-ttu-id="9f9ca-131">Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="9f9ca-131">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="build-hello-sample"></a><span data-ttu-id="9f9ca-132">Hello minta</span><span class="sxs-lookup"><span data-stu-id="9f9ca-132">Build hello sample</span></span>

<span data-ttu-id="9f9ca-133">Ha még nem tette meg, telepítse hello csomagokat a Microsoft Azure IoT eszköz SDK hello C hello hello málna Pi követően egy terminál-parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-133">If you have not already done so, install hello prerequisite packages for hello Microsoft Azure IoT Device SDK for C by running hello following commands in a terminal on hello Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="9f9ca-134">Most már lefordíthatja hello megoldást a hello málna Pi:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-134">You can now build hello sample solution on hello Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

<span data-ttu-id="9f9ca-135">Hello minta program hello málna Pi most futtathatja.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-135">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="9f9ca-136">Adja meg a hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-136">Enter hello command:</span></span>

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

<span data-ttu-id="9f9ca-137">hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-137">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

<span data-ttu-id="9f9ca-139">Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-139">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. <span data-ttu-id="9f9ca-140">Hello megoldás irányítópulton kattintson **eszközök** toovisit hello **eszközök** lap.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-140">In hello solution dashboard, click **Devices** toovisit hello **Devices** page.</span></span> <span data-ttu-id="9f9ca-141">Válassza ki a málna Pi hello **eszközlista**.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-141">Select your Raspberry Pi in hello **Device List**.</span></span> <span data-ttu-id="9f9ca-142">Válassza a **módszerek**:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-142">Then choose **Methods**:</span></span>

    ![Az irányítópult eszközök][img-list-devices]

1. <span data-ttu-id="9f9ca-144">A hello **metódus meghívása** lapon, válassza ki **InitiateFirmwareUpdate** a hello **metódus** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-144">On hello **Invoke Method** page, choose **InitiateFirmwareUpdate** in hello **Method** dropdown.</span></span>

1. <span data-ttu-id="9f9ca-145">A hello **FWPackageURI** mezőbe írja be **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-145">In hello **FWPackageURI** field, enter **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**.</span></span> <span data-ttu-id="9f9ca-146">Az archívumfájl hello végrehajtásának hello belső vezérlőprogram 2.0-s verzióját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-146">This archive file contains hello implementation of version 2.0 of hello firmware.</span></span>

1. <span data-ttu-id="9f9ca-147">Válasszon **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-147">Choose **InvokeMethod**.</span></span> <span data-ttu-id="9f9ca-148">hello málna Pi hello alkalmazást egy visszaigazoló hátsó toohello megoldás irányítópultja küld.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-148">hello app on hello Raspberry Pi sends an acknowledgment back toohello solution dashboard.</span></span> <span data-ttu-id="9f9ca-149">Ezután elindítja a letöltés hello új hello belső vezérlőprogram verziójának hello belső vezérlőprogram frissítési folyamat:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-149">It then starts hello firmware update process by downloading hello new version of hello firmware:</span></span>

    ![Módszer előzmények megjelenítése][img-method-history]

## <a name="observe-hello-firmware-update-process"></a><span data-ttu-id="9f9ca-151">Tekintse át az hello belső vezérlőprogram frissítési folyamat</span><span class="sxs-lookup"><span data-stu-id="9f9ca-151">Observe hello firmware update process</span></span>

<span data-ttu-id="9f9ca-152">Figyelheti a hello belső vezérlőprogram frissítési folyamat hello eszközön futó, és hello megtekintésével jelentett hello megoldás irányítópultjának tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-152">You can observe hello firmware update process as it runs on hello device and by viewing hello reported properties in hello solution dashboard:</span></span>

1. <span data-ttu-id="9f9ca-153">Hello előrehaladását a frissítési folyamat hello hello málna Pi tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-153">You can view hello progress in of hello update process on hello Raspberry Pi:</span></span>

    ![Megjeleníti a frissítési folyamatot][img-update-progress]

    > [!NOTE]
    > <span data-ttu-id="9f9ca-155">hello távoli figyelési alkalmazás újraindul csendes hello frissítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-155">hello remote monitoring app restarts silently when hello update completes.</span></span> <span data-ttu-id="9f9ca-156">Hello paranccsal `ps -ef` tooverify fut-e.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-156">Use hello command `ps -ef` tooverify it is running.</span></span> <span data-ttu-id="9f9ca-157">Ha azt szeretné, hogy tooterminate hello folyamat, használja a hello `kill` hello folyamatazonosító parancsot.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-157">If you want tooterminate hello process, use hello `kill` command with hello process id.</span></span>

1. <span data-ttu-id="9f9ca-158">Hello hello belső vezérlőprogram-frissítés állapota a, megtekintheti a hello eszközt, hello megoldás portál által jelentett módon.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-158">You can view hello status of hello firmware update, as reported by hello device, in hello solution portal.</span></span> <span data-ttu-id="9f9ca-159">hello alábbi képernyőfelvételen látható hello állapotát és minden szakaszhoz hello frissítési folyamat, és új belsővezérlőprogram-verziónként hello időtartama:</span><span class="sxs-lookup"><span data-stu-id="9f9ca-159">hello following screenshot shows hello status and duration of each stage of hello update process, and hello new firmware version:</span></span>

    ![Feladat állapotának megjelenítése][img-job-status]

    <span data-ttu-id="9f9ca-161">Lépjen vissza toohello irányítópult, ellenőrizheti a hello eszköz továbbra is küldi hello belső vezérlőprogram frissítése a következő telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-161">If you navigate back toohello dashboard, you can verify hello device is still sending telemetry following hello firmware update.</span></span>

> [!WARNING]
> <span data-ttu-id="9f9ca-162">Ha nem adja meg hello távoli felügyeleti megoldás az Azure-fiókjával fut, a hello futásakor kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-162">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="9f9ca-163">Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="9f9ca-163">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="9f9ca-164">Ha befejezte, használja előre konfigurált hello megoldás törlése az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-164">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f9ca-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9f9ca-165">Next steps</span></span>

<span data-ttu-id="9f9ca-166">A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="9f9ca-166">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md