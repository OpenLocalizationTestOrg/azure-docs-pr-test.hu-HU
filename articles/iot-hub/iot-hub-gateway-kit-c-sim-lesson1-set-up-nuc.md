---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 1: NUC beállítása |} Microsoft Docs"
description: "Intel NUC beállítása érzékelő és az Azure IoT Hub érzékelő adatokat gyűjteni, és elküldi a IoT-központ közötti IoT átjáróként működik."
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: "az IOT-átjárón intel nuc nuc számítógép, DE3815TYKE"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f41d6b2e-9b00-40df-90eb-17d824bea883
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b87974be9570f7d03fe84ae0a1d1fa7e346ff189
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="4cafe-104">Az IoT átjáróként Intel NUC beállítása</span><span class="sxs-lookup"><span data-stu-id="4cafe-104">Set up Intel NUC as an IoT gateway</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="4cafe-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="4cafe-105">What you will do</span></span>

- <span data-ttu-id="4cafe-106">Az IoT-átjáró Intel NUC állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="4cafe-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="4cafe-107">Telepítse az Azure IoT biztonsági csomag Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4cafe-107">Install the Azure IoT Edge package on Intel NUC.</span></span>
- <span data-ttu-id="4cafe-108">Futtassa a "hello_world" mintaalkalmazás Intel NUC átjáró működésének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="4cafe-108">Run a "hello_world" sample application on Intel NUC to verify the gateway functionality.</span></span>
<span data-ttu-id="4cafe-109">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="4cafe-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4cafe-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="4cafe-110">What you will learn</span></span>

<span data-ttu-id="4cafe-111">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="4cafe-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="4cafe-112">Hogyan perifériák Intel NUC kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="4cafe-112">How to connect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="4cafe-113">Hogyan kell telepíteni, és frissítse a szükséges csomagokat a Intel NUC az intelligens Package Manager segítségével.</span><span class="sxs-lookup"><span data-stu-id="4cafe-113">How to install and update the required packages on Intel NUC using the Smart Package Manager.</span></span>
- <span data-ttu-id="4cafe-114">Hogyan futtassa a "hello_world" mintaalkalmazást átjáró működésének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="4cafe-114">How to run the "hello_world" sample application to verify the gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4cafe-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="4cafe-115">What you need</span></span>

- <span data-ttu-id="4cafe-116">Az Intel NUC Kit DE3815TYKE a Intel IoT átjáró termékcsomag (szél folyó Linux * 7.0.0.13) előtelepítése mellett.</span><span class="sxs-lookup"><span data-stu-id="4cafe-116">An Intel NUC Kit DE3815TYKE with the Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span>
- <span data-ttu-id="4cafe-117">Kábel.</span><span class="sxs-lookup"><span data-stu-id="4cafe-117">An Ethernet cable.</span></span>
- <span data-ttu-id="4cafe-118">A billentyűzet.</span><span class="sxs-lookup"><span data-stu-id="4cafe-118">A keyboard.</span></span>
- <span data-ttu-id="4cafe-119">Egy HDMI vagy VGA kábel.</span><span class="sxs-lookup"><span data-stu-id="4cafe-119">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="4cafe-120">Egy figyelő HDMI vagy VGA-port.</span><span class="sxs-lookup"><span data-stu-id="4cafe-120">A monitor with an HDMI or VGA port.</span></span>

![Átjáró csomag](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a><span data-ttu-id="4cafe-122">Csatlakozás a perifériák Intel NUC</span><span class="sxs-lookup"><span data-stu-id="4cafe-122">Connect Intel NUC with the peripherals</span></span>

<span data-ttu-id="4cafe-123">Az alábbi képen látható egy példa Intel NUC különböző perifériák kapcsolódnak:</span><span class="sxs-lookup"><span data-stu-id="4cafe-123">The image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="4cafe-124">A billentyűzet csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="4cafe-124">Connected to a keyboard.</span></span>
2. <span data-ttu-id="4cafe-125">Csatlakozik a figyelő egy VGA vagy HDMI kábellel.</span><span class="sxs-lookup"><span data-stu-id="4cafe-125">Connected to the monitor by a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="4cafe-126">Kábel vezetékes hálózathoz csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="4cafe-126">Connected to a wired network by an Ethernet cable.</span></span>
4. <span data-ttu-id="4cafe-127">A tápegység power kábellel csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="4cafe-127">Connected to the power supply with a power cable.</span></span>

![Intel NUC perifériák csatlakozik.](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="4cafe-129">A gazdaszámítógép keresztül Secure Shell (SSH) az Intel NUC rendszerhez való csatlakozás</span><span class="sxs-lookup"><span data-stu-id="4cafe-129">Connect to the Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="4cafe-130">Itt szüksége billentyűzet és figyelheti az IP-címet a NUC eszköz eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4cafe-130">Here you need keyboard and monitor to get the IP address of your NUC device.</span></span> <span data-ttu-id="4cafe-131">Ha már ismeri az IP-cím, kihagyhatja, hogy ez a szakasz a 3. lépés.</span><span class="sxs-lookup"><span data-stu-id="4cafe-131">If you already know the IP address, you can skip to step 3 in this section.</span></span>

1. <span data-ttu-id="4cafe-132">Intel NUC bekapcsolása a főkapcsoló lenyomásával, és jelentkezzen be a rendszer.</span><span class="sxs-lookup"><span data-stu-id="4cafe-132">Turn on Intel NUC by pressing the Power button and log in the system.</span></span>

   <span data-ttu-id="4cafe-133">Az alapértelmezett felhasználónév és jelszó mindkét `root`.</span><span class="sxs-lookup"><span data-stu-id="4cafe-133">The default user name and password are both `root`.</span></span>

2. <span data-ttu-id="4cafe-134">NUC IP-címének lekéréséhez futtassa a `ifconfig` parancsot.</span><span class="sxs-lookup"><span data-stu-id="4cafe-134">Get the IP address of NUC by running the `ifconfig` command.</span></span> <span data-ttu-id="4cafe-135">Ez a lépés a NUC eszközön történik.</span><span class="sxs-lookup"><span data-stu-id="4cafe-135">This step is done on the NUC device.</span></span>

   <span data-ttu-id="4cafe-136">Íme egy példa a parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="4cafe-136">Here is an example of the command output.</span></span>

   ![ifconfig kimeneti NUC IP megjelenítése](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="4cafe-138">Ebben a példában az érték, amely a következő `inet addr:` az IP-címe, ha azt tervezi, távolról csatlakozni a számítógép Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4cafe-138">In this example, the value that follows `inet addr:` is the IP address that you need when you plan to connect remotely from a host computer to Intel NUC.</span></span>

3. <span data-ttu-id="4cafe-139">Használja a következő SSH ügyfelek a számítógép Intel NUC való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="4cafe-139">Use one of the following SSH clients from your host machine to connect to Intel NUC.</span></span>

   - <span data-ttu-id="4cafe-140">[A puTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="4cafe-140">[PuTTY](http://www.putty.org/) for Windows.</span></span>
   - <span data-ttu-id="4cafe-141">A beépített SSH-ügyfél Ubuntu vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="4cafe-141">The build-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="4cafe-142">Hatékony, és növeli az való működésre Intel NUC el gazdagépről.</span><span class="sxs-lookup"><span data-stu-id="4cafe-142">It is more efficient and productive to operate on Intel NUC from a host computer.</span></span> <span data-ttu-id="4cafe-143">Van szüksége a az IP cím, felhasználónevet és jelszót a NUC SSH-ügyfél keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="4cafe-143">You need the the IP address, user name and password to connect the NUC via SSH client.</span></span> <span data-ttu-id="4cafe-144">A macOS Ez a példa használata SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="4cafe-144">Here is the example use SSH client on macOS.</span></span>
   <span data-ttu-id="4cafe-145">![MacOS futó SSH-ügyfél](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="4cafe-145">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-the-azure-iot-edge-package"></a><span data-ttu-id="4cafe-146">Telepítse az Azure IoT Edge-csomagot</span><span class="sxs-lookup"><span data-stu-id="4cafe-146">Install the Azure IoT Edge package</span></span>

<span data-ttu-id="4cafe-147">Az Azure IoT biztonsági csomag tartalmazza az előre lefordított bináris fájljait az SDK-t és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="4cafe-147">The Azure IoT Edge package contains the pre-compiled binaries of the SDK and its dependencies.</span></span> <span data-ttu-id="4cafe-148">A bináris fájljait a Azure IoT él, az Azure IoT SDK és a megfelelő eszközök.</span><span class="sxs-lookup"><span data-stu-id="4cafe-148">These binaries are Azure IoT Edge, the Azure IoT SDK and the corresponding tools.</span></span> <span data-ttu-id="4cafe-149">A csomag "hello_world" mintaalkalmazás, amely ellenőrzi az átjárófunkció is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4cafe-149">The package also contains a "hello_world" sample application that is used to validate the gateway functionality.</span></span> <span data-ttu-id="4cafe-150">Az IoT-Edge az átjáró alapvető részét képezi.</span><span class="sxs-lookup"><span data-stu-id="4cafe-150">IoT Edge is the core part of the gateway.</span></span> <span data-ttu-id="4cafe-151">A csomag telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4cafe-151">To install the package, follow these steps:</span></span>

1. <span data-ttu-id="4cafe-152">Az IoT-felhő tárház hozzáadása egy terminál ablakban a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4cafe-152">Add the IoT cloud repository by running the following commands in a terminal window:</span></span>

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   <span data-ttu-id="4cafe-153">A `rpm` parancs importálja a rpm kulcsot.</span><span class="sxs-lookup"><span data-stu-id="4cafe-153">The `rpm` command imports the rpm key.</span></span> <span data-ttu-id="4cafe-154">A `smart channel` parancs hozzáadja a rpm-csatorna a intelligens Csomagkezelő.</span><span class="sxs-lookup"><span data-stu-id="4cafe-154">The `smart channel` command adds the rpm channel to the Smart Package Manager.</span></span> <span data-ttu-id="4cafe-155">Mielőtt újra lefuttatja a `smart update` hasonló kimenetnek parancs, lásd alább.</span><span class="sxs-lookup"><span data-stu-id="4cafe-155">Before you run the `smart update` command, you see an output like below.</span></span>

   ![rpm és intelligens csatorna parancs kimenete](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. <span data-ttu-id="4cafe-157">A csomag telepítése a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4cafe-157">Install the package by running the following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="4cafe-158">`packagegroup-cloud-azure`a csomag neve van.</span><span class="sxs-lookup"><span data-stu-id="4cafe-158">`packagegroup-cloud-azure` is the name of the package.</span></span> <span data-ttu-id="4cafe-159">A `smart install` a parancsnak a csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4cafe-159">The `smart install` command is used to install the package.</span></span>

   <span data-ttu-id="4cafe-160">A csomag telepítése után a Intel NUC várhatóan átjáróként működik.</span><span class="sxs-lookup"><span data-stu-id="4cafe-160">After the package is installed, Intel NUC is expected to work as a gateway.</span></span>

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="4cafe-161">Az Azure IoT peremhálózati "hello_world" mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="4cafe-161">Run the Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="4cafe-162">Ugrás a `azureiotgatewaysdk/samples` és futtassa a "hello_world" minta mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4cafe-162">Go to `azureiotgatewaysdk/samples` and run the sample "hello_world" sample application.</span></span> <span data-ttu-id="4cafe-163">A mintaalkalmazás létrehoz egy átjárót a `hello_world.json` fájlt, és használja az Azure IoT peremhálózati architektúra alapvető összetevői 5 másodpercentként bejelentkezési hello world üzenet fájlba.</span><span class="sxs-lookup"><span data-stu-id="4cafe-163">This sample application creates a gateway from the `hello_world.json` file and uses the fundamental components of the Azure IoT Edge architecture to log a hello world message to a file every 5 seconds.</span></span>

<span data-ttu-id="4cafe-164">A minta "hello_world" mintaalkalmazást is futtathatja a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4cafe-164">You can run the sample "hello_world" sample application by running the following command:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="4cafe-165">A mintaalkalmazást a következő kimenetet hoz létre, ha az átjárófunkció megfelelően működik-e:</span><span class="sxs-lookup"><span data-stu-id="4cafe-165">The sample application produces the following output if the gateway functionality is working correctly:</span></span>

![alkalmazás kimenete](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

<span data-ttu-id="4cafe-167">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="4cafe-167">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="4cafe-168">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4cafe-168">Summary</span></span>

<span data-ttu-id="4cafe-169">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="4cafe-169">Congratulations!</span></span> <span data-ttu-id="4cafe-170">Intel NUC átjáróként beállításának befejezése után.</span><span class="sxs-lookup"><span data-stu-id="4cafe-170">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="4cafe-171">Most már készen áll áthelyezése a következő lecke állítsa be a gazdaszámítógép, Azure IoT hub létrehozása és regisztrálása az Azure IoT hub logikai eszköz.</span><span class="sxs-lookup"><span data-stu-id="4cafe-171">Now you're ready to move on to the next lesson to set up your host computer, create an Azure IoT hub and register your Azure IoT hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cafe-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4cafe-172">Next steps</span></span>
[<span data-ttu-id="4cafe-173">Felkészülés a gazdaszámítógép és Azure IoT-központ</span><span class="sxs-lookup"><span data-stu-id="4cafe-173">Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
