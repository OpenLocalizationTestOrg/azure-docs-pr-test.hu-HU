---
title: "SensorTag eszköz & Azure IoT átjáró - lecke 1: Intel NUC beállítása |} Microsoft Docs"
description: "Intel NUC beállítása érzékelő és az Azure IoT Hub érzékelő adatokat gyűjteni, és elküldi a IoT-központ közötti IoT átjáróként működik."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-átjárón intel nuc nuc számítógép, DE3815TYKE"
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1a3a92ab8d08c6ed6f047208217c46022027157e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="a1366-104">Az IoT átjáróként Intel NUC beállítása</span><span class="sxs-lookup"><span data-stu-id="a1366-104">Set up Intel NUC as an IoT gateway</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a><span data-ttu-id="a1366-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a1366-105">What you will do</span></span>

- <span data-ttu-id="a1366-106">Az IoT-átjáró Intel NUC állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="a1366-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="a1366-107">Az Intel NUC Azure IoT peremhálózati telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1366-107">Install the Azure IoT Edge package on the Intel NUC.</span></span>
- <span data-ttu-id="a1366-108">Futtassa a "hello_world" mintaalkalmazás az Intel NUC átjáró működésének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1366-108">Run a "hello_world" sample application on the Intel NUC to verify the gateway functionality.</span></span>

  > <span data-ttu-id="a1366-109">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a1366-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a1366-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a1366-110">What you will learn</span></span>

<span data-ttu-id="a1366-111">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="a1366-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="a1366-112">Hogyan perifériák Intel NUC kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="a1366-112">How to connect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="a1366-113">Hogyan kell telepíteni, és frissítse a szükséges csomagokat a Intel NUC az intelligens Package Manager segítségével.</span><span class="sxs-lookup"><span data-stu-id="a1366-113">How to install and update the required packages on Intel NUC using the Smart Package Manager.</span></span>
- <span data-ttu-id="a1366-114">Hogyan futtassa a "hello_world" mintaalkalmazást átjáró működésének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1366-114">How to run the "hello_world" sample application to verify the gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a1366-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a1366-115">What you need</span></span>

- <span data-ttu-id="a1366-116">Az Intel NUC Kit DE3815TYKE a Intel IoT átjáró termékcsomag (szél folyó Linux * 7.0.0.13) előtelepítése mellett.</span><span class="sxs-lookup"><span data-stu-id="a1366-116">An Intel NUC Kit DE3815TYKE with the Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span> <span data-ttu-id="a1366-117">[Kattintson ide a beszerzési Groove IoT kereskedelmi átjáró Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span><span class="sxs-lookup"><span data-stu-id="a1366-117">[Click here to purchase Grove IoT Commercial Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span></span>
- <span data-ttu-id="a1366-118">Kábel.</span><span class="sxs-lookup"><span data-stu-id="a1366-118">An Ethernet cable.</span></span>
- <span data-ttu-id="a1366-119">A billentyűzet.</span><span class="sxs-lookup"><span data-stu-id="a1366-119">A keyboard.</span></span>
- <span data-ttu-id="a1366-120">Egy HDMI vagy VGA kábel.</span><span class="sxs-lookup"><span data-stu-id="a1366-120">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="a1366-121">Egy figyelő HDMI vagy VGA-port.</span><span class="sxs-lookup"><span data-stu-id="a1366-121">A monitor with an HDMI or VGA port.</span></span>
- <span data-ttu-id="a1366-122">Választható lehetőség: [Texas eszközök érzékelő címke (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span><span class="sxs-lookup"><span data-stu-id="a1366-122">Optional: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span></span>

![Átjáró csomag](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a><span data-ttu-id="a1366-124">Csatlakozás a perifériák Intel NUC</span><span class="sxs-lookup"><span data-stu-id="a1366-124">Connect Intel NUC with the peripherals</span></span>

<span data-ttu-id="a1366-125">Az alábbi képen látható egy példa Intel NUC különböző perifériák kapcsolódnak:</span><span class="sxs-lookup"><span data-stu-id="a1366-125">The image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="a1366-126">A billentyűzet csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="a1366-126">Connected to a keyboard.</span></span>
2. <span data-ttu-id="a1366-127">A figyelő egy VGA vagy HDMI kábellel csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="a1366-127">Connected to a monitor with a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="a1366-128">Ethernet kábel egy vezetékes hálózathoz csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="a1366-128">Connected to a wired network with an Ethernet cable.</span></span>
4. <span data-ttu-id="a1366-129">Egy tápegység power kábellel csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="a1366-129">Connected to a power supply with a power cable.</span></span>

![Intel NUC perifériák csatlakozik.](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="a1366-131">A gazdaszámítógép keresztül Secure Shell (SSH) az Intel NUC rendszerhez való csatlakozás</span><span class="sxs-lookup"><span data-stu-id="a1366-131">Connect to the Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="a1366-132">Szüksége lesz a billentyűzet és egy figyelőt a beolvasása az Intel NUC eszköz IP-címét.</span><span class="sxs-lookup"><span data-stu-id="a1366-132">You will need a keyboard and a monitor to get the IP address of your Intel NUC device.</span></span> <span data-ttu-id="a1366-133">Ha már ismeri az IP-cím, kihagyhatja azokat, amelyek a jelen szakaszban 3. lépésre.</span><span class="sxs-lookup"><span data-stu-id="a1366-133">If you already know the IP address, you can skip ahead to step 3 in this section.</span></span>

1. <span data-ttu-id="a1366-134">Az Intel NUC bekapcsolása a power gombra kattintva, majd jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="a1366-134">Turn on the Intel NUC by pressing the power button and then log in.</span></span>

   <span data-ttu-id="a1366-135">Az alapértelmezett felhasználónév és jelszó mindkét `root`.</span><span class="sxs-lookup"><span data-stu-id="a1366-135">The default user name and password are both `root`.</span></span>

       > Hit the enter key on your keyboard if you see either of the following errors when you boot: 'A TPM error (7) occurred attempting to read a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. <span data-ttu-id="a1366-136">Az Intel NUC IP-címének lekéréséhez futtassa a `ifconfig` parancsot az Intel NUC eszközön.</span><span class="sxs-lookup"><span data-stu-id="a1366-136">Get the IP address of the Intel NUC by running the `ifconfig` command on the Intel NUC device.</span></span>

   <span data-ttu-id="a1366-137">Íme egy példa a parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="a1366-137">Here is an example of the command output.</span></span>

   ![Intel NUC IP megjelenítő ifconfig kimeneti](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="a1366-139">Ebben a példában az érték, amely a következő `inet addr:` van szüksége, amikor csatlakoznak a Intel NUC egy számítógép IP-cím.</span><span class="sxs-lookup"><span data-stu-id="a1366-139">In this example, the value that follows `inet addr:` is the IP address that you need when connect to the Intel NUC from a host computer.</span></span>

3. <span data-ttu-id="a1366-140">A számítógép a következő SSH-ügyfél segítségével Intel NUC csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="a1366-140">Use one of the following SSH clients from your host computer to connect to Intel NUC.</span></span>

    - <span data-ttu-id="a1366-141">[A puTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="a1366-141">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="a1366-142">A beépített SSH-ügyfél Ubuntu vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="a1366-142">The built-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="a1366-143">Hatékony és hatékony működéséhez egy állomásról egy Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a1366-143">It is more efficient and productive to operate an Intel NUC from a host computer.</span></span> <span data-ttu-id="a1366-144">Az Intel NUC IP-cím, a felhasználónév és jelszó egy SSH-ügyfél keresztül kapcsolódni hozzá lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="a1366-144">You'll need the Intel NUC's IP address, user name and password to connect to it via an SSH client.</span></span> <span data-ttu-id="a1366-145">Íme egy példa, egy SSH-ügyfél által az macOS.</span><span class="sxs-lookup"><span data-stu-id="a1366-145">Here is an example that uses an SSH client on macOS.</span></span>
   <span data-ttu-id="a1366-146">![MacOS futó SSH-ügyfél](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="a1366-146">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-the-azure-iot-edge-package"></a><span data-ttu-id="a1366-147">Telepítse az Azure IoT Edge-csomagot</span><span class="sxs-lookup"><span data-stu-id="a1366-147">Install the Azure IoT Edge package</span></span>

<span data-ttu-id="a1366-148">Az Azure IoT biztonsági csomag IoT széle és annak függőségeit a előre lefordított bináris fájljait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a1366-148">The Azure IoT Edge package contains the pre-compiled binaries of IoT Edge and its dependencies.</span></span> <span data-ttu-id="a1366-149">A bináris fájljait a Azure IoT él, az Azure IoT SDK és a megfelelő eszközök.</span><span class="sxs-lookup"><span data-stu-id="a1366-149">These binaries are Azure IoT Edge, the Azure IoT SDK and the corresponding tools.</span></span> <span data-ttu-id="a1366-150">A csomag is tartalmaz egy "hello_world" mintaalkalmazást az átjárófunkció ellenőrzésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="a1366-150">The package also contains a "hello_world" sample application is used to validate the gateway functionality.</span></span> <span data-ttu-id="a1366-151">Az IoT-Edge az átjáró alapvető részét képezi.</span><span class="sxs-lookup"><span data-stu-id="a1366-151">IoT Edge is the core part of the gateway.</span></span> 

<span data-ttu-id="a1366-152">Kövesse az alábbi lépéseket a csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1366-152">Follow these steps to install the package.</span></span>

1. <span data-ttu-id="a1366-153">Adja hozzá a felhőben az IoT-tárház egy terminál ablakban a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1366-153">Add the IoT Cloud repository by running the following commands in a terminal window:</span></span>

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > <span data-ttu-id="a1366-154">Adja meg az "y", amikor a rendszer kérni fogja, hogy az "Include ezt a csatornát?"</span><span class="sxs-lookup"><span data-stu-id="a1366-154">Enter 'y', when it prompts you to 'Include this channel?'</span></span>
   
   <span data-ttu-id="a1366-155">Ha megjelenik egy `import read failed(-1)` hiba, a probléma megoldásához használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="a1366-155">If you receive an `import read failed(-1)` error, use the following commands to resolve the issue:</span></span>
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   <span data-ttu-id="a1366-156">A `rpm` parancs importálja a rpm kulcsot.</span><span class="sxs-lookup"><span data-stu-id="a1366-156">The `rpm` command imports the rpm key.</span></span> <span data-ttu-id="a1366-157">A `smart channel` parancs hozzáadja a rpm-csatorna a intelligens Csomagkezelő.</span><span class="sxs-lookup"><span data-stu-id="a1366-157">The `smart channel` command adds the rpm channel to the Smart Package Manager.</span></span> <span data-ttu-id="a1366-158">Futtatása előtt a `smart update` parancsban, látni fogja, például egy kimeneti alatt.</span><span class="sxs-lookup"><span data-stu-id="a1366-158">Before you run the `smart update` command, you will see an output like below.</span></span>

   ![rpm és intelligens csatorna parancs kimenete](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. <span data-ttu-id="a1366-160">Az intelligens frissítés parancs hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="a1366-160">Execute the smart update command:</span></span>

   ```bash
   smart update
   ```

3. <span data-ttu-id="a1366-161">Az Azure IoT-átjáró telepítéséhez a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1366-161">Install the Azure IoT Gateway package by running the following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="a1366-162">`packagegroup-cloud-azure`a csomag neve van.</span><span class="sxs-lookup"><span data-stu-id="a1366-162">`packagegroup-cloud-azure` is the name of the package.</span></span> <span data-ttu-id="a1366-163">A `smart install` a parancsnak a csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1366-163">The `smart install` command is used to install the package.</span></span>

    > <span data-ttu-id="a1366-164">Futtassa a következő parancsot, ha ezt a hibaüzenetet látja: "nyilvános kulcs nem érhető el"</span><span class="sxs-lookup"><span data-stu-id="a1366-164">Run the following command if you see this error: 'public key not available'</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > <span data-ttu-id="a1366-165">Indítsa újra az Intel NUC, ha ezt a hibaüzenetet látja: "nincs csomag biztosítja a fejlesztői linux util"</span><span class="sxs-lookup"><span data-stu-id="a1366-165">Reboot the Intel NUC if you see this error: 'no package provides util-linux-dev'</span></span>

   <span data-ttu-id="a1366-166">A csomag telepítése után Intel NUC készen áll az átjáró működhet.</span><span class="sxs-lookup"><span data-stu-id="a1366-166">After the package is installed, Intel NUC is ready to function as a gateway.</span></span>

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="a1366-167">Az Azure IoT peremhálózati "hello_world" mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a1366-167">Run the Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="a1366-168">Az alábbi minta-alkalmazást hoz létre az átjáró egy `hello_world.json` fájlt, és az Azure IoT peremhálózati architektúra alapvető összetevői hello world üzenet jelentkezzenek be egy fájl (log.txt) 5 másodpercentként használja.</span><span class="sxs-lookup"><span data-stu-id="a1366-168">The following sample application creates a gateway from a `hello_world.json` file and uses the fundamental components of Azure IoT Edge architecture to log a hello world message to a file (log.txt) every 5 seconds.</span></span>

<span data-ttu-id="a1366-169">A Hello World PéldaAlkalmazás futtathatja a következő parancsok végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="a1366-169">You can run the Hello World sample by executing the following commands:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="a1366-170">Lehetővé teszik a Hello World alkalmazásról futtatása néhány percet, és majd kattintson a állítsa le az Enter billentyűt.</span><span class="sxs-lookup"><span data-stu-id="a1366-170">Let the Hello World application run for a few minutes and then hit the Enter key to stop it.</span></span>
<span data-ttu-id="a1366-171">![alkalmazás kimenete](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span><span class="sxs-lookup"><span data-stu-id="a1366-171">![application output](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span></span>

> <span data-ttu-id="a1366-172">A "argumentum érvénytelen handle(NULL)" hibák után le az ENTER billentyűt figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="a1366-172">You can ignore any 'invalid argument handle(NULL)' errors that appear after you hit Enter.</span></span>

<span data-ttu-id="a1366-173">Ellenőrizheti, hogy az átjáró, amelyik most már a hello_world mappában log.txt fájlt sikeresen lefutott ![log.txt directory megtekintése](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span><span class="sxs-lookup"><span data-stu-id="a1366-173">You can verify that the gateway ran successfully by opening the log.txt file that is now in your hello_world folder ![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span></span>

<span data-ttu-id="a1366-174">Nyissa meg a következő paranccsal log.txt:</span><span class="sxs-lookup"><span data-stu-id="a1366-174">Open log.txt using the following command:</span></span>

```bash
vim log.txt
```

<span data-ttu-id="a1366-175">Ekkor megjelenik a log.txt, ami egy formázott JSON-kimenetét a naplózási üzeneteket, hogy az átjáró Hello World modul 5 másodpercenként írt tartalmát.</span><span class="sxs-lookup"><span data-stu-id="a1366-175">You will then see the contents of log.txt, which will be a JSON formatted output of the logging messages that were written every 5 seconds by the gateway Hello World module.</span></span>
<span data-ttu-id="a1366-176">![log.txt directory megtekintése](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span><span class="sxs-lookup"><span data-stu-id="a1366-176">![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span></span>

<span data-ttu-id="a1366-177">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a1366-177">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="a1366-178">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a1366-178">Summary</span></span>

<span data-ttu-id="a1366-179">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="a1366-179">Congratulations!</span></span> <span data-ttu-id="a1366-180">Intel NUC átjáróként beállításának befejezése után.</span><span class="sxs-lookup"><span data-stu-id="a1366-180">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="a1366-181">Most már készen áll áthelyezése a következő lecke állítsa be a gazdaszámítógép, Azure IoT Hub létrehozása és regisztrálása az Azure IoT Hub logikai eszköz.</span><span class="sxs-lookup"><span data-stu-id="a1366-181">Now you're ready to move on to the next lesson to set up your host computer, create an Azure IoT Hub and register your Azure IoT Hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1366-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1366-182">Next steps</span></span>
[<span data-ttu-id="a1366-183">Egy eszköz csatlakoztatása az Azure IoT Hub egy IoT-átjáró használatával</span><span class="sxs-lookup"><span data-stu-id="a1366-183">Use an IoT gateway to connect a device to Azure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

