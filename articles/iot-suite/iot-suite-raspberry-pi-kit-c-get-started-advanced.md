---
title: "Csatlakozás egy málna Pi Azure IoT Suite C használatával támogatja a belső vezérlőprogram-frissítésekre |} Microsoft Docs"
description: "Használja a Microsoft Azure IoT Starter Kit a Raspberry pi 3 és az Azure IoT Suite. A málna Pi kapcsolódni a távoli felügyeleti megoldás használatát C telemetriai adatokat küldhet az érzékelők a felhőbe, és hajtsa végre a távoli vezérlőprogram-frissítés."
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
ms.openlocfilehash: f36f6512bb30e4b109b1bd1c3cdab10300f4edc9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a><span data-ttu-id="c28e0-104">A málna Pi 3 kapcsolódni a távoli felügyeleti megoldás, és engedélyezze a távoli belső vezérlőprogram-frissítésekre C használatával</span><span class="sxs-lookup"><span data-stu-id="c28e0-104">Connect your Raspberry Pi 3 to the remote monitoring solution and enable remote firmware updates using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="c28e0-105">Az oktatóanyag bemutatja, hogyan használható a Microsoft Azure IoT Starter Kit málna Pi 3:</span><span class="sxs-lookup"><span data-stu-id="c28e0-105">This tutorial shows you how to use the Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to:</span></span>

* <span data-ttu-id="c28e0-106">Fejlesztés a hőmérséklet és a páratartalom olvasó, amely képes kommunikálni a felhőben.</span><span class="sxs-lookup"><span data-stu-id="c28e0-106">Develop a temperature and humidity reader that can communicate with the cloud.</span></span>
* <span data-ttu-id="c28e0-107">Engedélyezi, és végre a távoli belső vezérlőprogram frissítése a frissítés az ügyfélalkalmazás a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="c28e0-107">Enable and perform a remote firmware update to update the client application on the Raspberry Pi.</span></span>

<span data-ttu-id="c28e0-108">Az oktatóprogram:</span><span class="sxs-lookup"><span data-stu-id="c28e0-108">The tutorial uses:</span></span>

* <span data-ttu-id="c28e0-109">Raspbian operációs rendszer, a C programozási nyelv, és a Microsoft Azure IoT SDK C-hez egy minta eszköz végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="c28e0-109">Raspbian OS, the C programming language, and the Microsoft Azure IoT SDK for C to implement a sample device.</span></span>
* <span data-ttu-id="c28e0-110">Az IoT Suite távoli megfigyelési előre konfigurált megoldás, a felhő alapú háttér.</span><span class="sxs-lookup"><span data-stu-id="c28e0-110">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="c28e0-111">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c28e0-111">Overview</span></span>

<span data-ttu-id="c28e0-112">Ebben az oktatóanyagban végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c28e0-112">In this tutorial, you complete the following steps:</span></span>

* <span data-ttu-id="c28e0-113">Telepítse a távoli felügyeleti előkonfigurált megoldás egy példányát az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c28e0-113">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="c28e0-114">Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c28e0-114">This step automatically deploys and configures multiple Azure services.</span></span>
* <span data-ttu-id="c28e0-115">Állítsa be az eszköz és a érzékelők kommunikáljanak a számítógép és a távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="c28e0-115">Set up your device and sensors to communicate with your computer and the remote monitoring solution.</span></span>
* <span data-ttu-id="c28e0-116">Frissítse a mintakódot eszköz csatlakozni a távoli felügyeleti megoldás, és, amely megtalálható a megoldás irányítópultja telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="c28e0-116">Update the sample device code to connect to the remote monitoring solution, and send telemetry that you can view on the solution dashboard.</span></span>
* <span data-ttu-id="c28e0-117">A mintakód eszköz használatával frissítheti az ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c28e0-117">Use the sample device code to update the client application.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="c28e0-118">A távoli felügyeleti megoldás látja el az Azure-előfizetéshez az Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="c28e0-118">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="c28e0-119">A központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra.</span><span class="sxs-lookup"><span data-stu-id="c28e0-119">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="c28e0-120">Szükségtelen Azure felhasználási díjak elkerülése az előre konfigurált megoldást a következő azureiotsuite.com a példányának törlése után vele.</span><span class="sxs-lookup"><span data-stu-id="c28e0-120">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="c28e0-121">Ha újra kell az előkonfigurált megoldás, egyszerűen létrehozhatja azt.</span><span class="sxs-lookup"><span data-stu-id="c28e0-121">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="c28e0-122">További információ a felhasználás csökkentése a távoli felügyeleti megoldás futtatása közben: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="c28e0-122">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="c28e0-123">Töltse le és konfigurálja a minta</span><span class="sxs-lookup"><span data-stu-id="c28e0-123">Download and configure the sample</span></span>

<span data-ttu-id="c28e0-124">Most töltse le és konfigurálja a távoli felügyeleti ügyfélalkalmazás a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="c28e0-124">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-the-repositories"></a><span data-ttu-id="c28e0-125">A tárolóhelyekkel klónozása</span><span class="sxs-lookup"><span data-stu-id="c28e0-125">Clone the repositories</span></span>

<span data-ttu-id="c28e0-126">Ha még nem tette meg, klónozza a szükséges adattárak a Pi a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c28e0-126">If you haven't done so already, clone the required repositories by running the following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="c28e0-127">Frissítés az eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c28e0-127">Update the device connection string</span></span>

<span data-ttu-id="c28e0-128">Nyissa meg a minta konfigurációs fájlt a **nano** szerkesztő a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c28e0-128">Open the sample configuration file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

<span data-ttu-id="c28e0-129">Cserélje le a helyőrző értékeket az ID és az IoT-központ eszközinformáció létrehozott és mentett Ez az oktatóanyag elején.</span><span class="sxs-lookup"><span data-stu-id="c28e0-129">Replace the placeholder values with the device ID and IoT Hub information you created and saved at the start of this tutorial.</span></span>

<span data-ttu-id="c28e0-130">Amikor elkészült, a deviceinfo információja fájl tartalma az alábbihoz hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="c28e0-130">When you are done, the contents of the deviceinfo file should look like the following example:</span></span>

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

<span data-ttu-id="c28e0-131">Menti a módosításokat (**Ctrl-O**, **Enter**), és zárja be a szerkesztőt (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="c28e0-131">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="build-the-sample"></a><span data-ttu-id="c28e0-132">A minta</span><span class="sxs-lookup"><span data-stu-id="c28e0-132">Build the sample</span></span>

<span data-ttu-id="c28e0-133">Ha még nem tette meg, telepítse a csomagokat a Microsoft Azure IoT eszköz SDK C-hez a következő parancsok futtatásával parancsot egy terminálban a málna Pi:</span><span class="sxs-lookup"><span data-stu-id="c28e0-133">If you have not already done so, install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by running the following commands in a terminal on the Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="c28e0-134">A megoldást a málna Pi a most már lefordíthatja:</span><span class="sxs-lookup"><span data-stu-id="c28e0-134">You can now build the sample solution on the Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

<span data-ttu-id="c28e0-135">A minta program most futtathatja a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="c28e0-135">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="c28e0-136">Adja meg a parancsot:</span><span class="sxs-lookup"><span data-stu-id="c28e0-136">Enter the command:</span></span>

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

<span data-ttu-id="c28e0-137">A következő minta kimenet látható egy példa a kimeneti látja a málna Pi a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="c28e0-137">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

<span data-ttu-id="c28e0-139">Nyomja le az **Ctrl-C** kilép a programból bármikor.</span><span class="sxs-lookup"><span data-stu-id="c28e0-139">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. <span data-ttu-id="c28e0-140">A megoldás irányítópulton kattintson **eszközök** és látogasson el a **eszközök** lap.</span><span class="sxs-lookup"><span data-stu-id="c28e0-140">In the solution dashboard, click **Devices** to visit the **Devices** page.</span></span> <span data-ttu-id="c28e0-141">Válassza ki a Raspberry Pi a a **eszközlista**.</span><span class="sxs-lookup"><span data-stu-id="c28e0-141">Select your Raspberry Pi in the **Device List**.</span></span> <span data-ttu-id="c28e0-142">Válassza a **módszerek**:</span><span class="sxs-lookup"><span data-stu-id="c28e0-142">Then choose **Methods**:</span></span>

    ![Az irányítópult eszközök][img-list-devices]

1. <span data-ttu-id="c28e0-144">Az a **metódus meghívása** lapon, válassza ki **InitiateFirmwareUpdate** a a **metódus** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="c28e0-144">On the **Invoke Method** page, choose **InitiateFirmwareUpdate** in the **Method** dropdown.</span></span>

1. <span data-ttu-id="c28e0-145">Az a **FWPackageURI** mezőbe írja be **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**.</span><span class="sxs-lookup"><span data-stu-id="c28e0-145">In the **FWPackageURI** field, enter **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**.</span></span> <span data-ttu-id="c28e0-146">Az archívum 2.0-s verziójának a belső vezérlőprogram végrehajtásának tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c28e0-146">This archive file contains the implementation of version 2.0 of the firmware.</span></span>

1. <span data-ttu-id="c28e0-147">Válasszon **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="c28e0-147">Choose **InvokeMethod**.</span></span> <span data-ttu-id="c28e0-148">Az alkalmazás a málna Pi nyugtázás visszaküldi a megoldás irányítópultja.</span><span class="sxs-lookup"><span data-stu-id="c28e0-148">The app on the Raspberry Pi sends an acknowledgment back to the solution dashboard.</span></span> <span data-ttu-id="c28e0-149">Ezután elindítja a belső vezérlőprogram frissítési folyamat által a belső vezérlőprogram az új verzió letöltése:</span><span class="sxs-lookup"><span data-stu-id="c28e0-149">It then starts the firmware update process by downloading the new version of the firmware:</span></span>

    ![Módszer előzmények megjelenítése][img-method-history]

## <a name="observe-the-firmware-update-process"></a><span data-ttu-id="c28e0-151">Figyelje meg a belső vezérlőprogram frissítése folyamatban</span><span class="sxs-lookup"><span data-stu-id="c28e0-151">Observe the firmware update process</span></span>

<span data-ttu-id="c28e0-152">Figyelheti a folyamatot nem lehet frissíteni, hogy az eszközön, és a megoldás irányítópultjának jelentett tulajdonságait megtekintve belső vezérlőprogram:</span><span class="sxs-lookup"><span data-stu-id="c28e0-152">You can observe the firmware update process as it runs on the device and by viewing the reported properties in the solution dashboard:</span></span>

1. <span data-ttu-id="c28e0-153">A frissítési folyamat előrehaladását a a málna Pi tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="c28e0-153">You can view the progress in of the update process on the Raspberry Pi:</span></span>

    ![Megjeleníti a frissítési folyamatot][img-update-progress]

    > [!NOTE]
    > <span data-ttu-id="c28e0-155">A távoli figyelési alkalmazás csendes újraindul, ha a frissítés befejezése.</span><span class="sxs-lookup"><span data-stu-id="c28e0-155">The remote monitoring app restarts silently when the update completes.</span></span> <span data-ttu-id="c28e0-156">A paranccsal `ps -ef` ellenőrzése, hogy fut-e.</span><span class="sxs-lookup"><span data-stu-id="c28e0-156">Use the command `ps -ef` to verify it is running.</span></span> <span data-ttu-id="c28e0-157">Ha azt szeretné, a folyamat leáll, használja a `kill` parancsot a folyamat azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c28e0-157">If you want to terminate the process, use the `kill` command with the process id.</span></span>

1. <span data-ttu-id="c28e0-158">Megtekintheti a belső vezérlőprogram-frissítés állapota az eszközt, a megoldás portál által jelentett módon.</span><span class="sxs-lookup"><span data-stu-id="c28e0-158">You can view the status of the firmware update, as reported by the device, in the solution portal.</span></span> <span data-ttu-id="c28e0-159">Az alábbi képernyőfelvételen látható állapotát és a frissítési folyamat, és az új belső vezérlőprogram-verziója minden szakaszhoz időtartama:</span><span class="sxs-lookup"><span data-stu-id="c28e0-159">The following screenshot shows the status and duration of each stage of the update process, and the new firmware version:</span></span>

    ![Feladat állapotának megjelenítése][img-job-status]

    <span data-ttu-id="c28e0-161">Ha, lépjen vissza az irányítópulthoz, ellenőrizheti az eszköz továbbra is küld a frissítést követő telemetriai.</span><span class="sxs-lookup"><span data-stu-id="c28e0-161">If you navigate back to the dashboard, you can verify the device is still sending telemetry following the firmware update.</span></span>

> [!WARNING]
> <span data-ttu-id="c28e0-162">Ha nem adja meg a távoli figyelési megoldást igényelnek fut az Azure-fiókjával, a futtatásakor a kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="c28e0-162">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="c28e0-163">További információ a felhasználás csökkentése a távoli felügyeleti megoldás futtatása közben: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="c28e0-163">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="c28e0-164">Ha befejezte, használja az előkonfigurált megoldás törlése az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c28e0-164">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c28e0-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c28e0-165">Next steps</span></span>

<span data-ttu-id="c28e0-166">Látogasson el a [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.</span><span class="sxs-lookup"><span data-stu-id="c28e0-166">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md