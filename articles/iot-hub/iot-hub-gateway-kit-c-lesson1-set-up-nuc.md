---
title: "SensorTag eszköz & Azure IoT átjáró - lecke 1: Intel NUC beállítása |} Microsoft Docs"
description: "Az IoT-átjáró érzékelő és az Azure IoT Hub toocollect érzékelő adatokat közötti Intel NUC toowork beállítani, és elküldi a tooIoT központ."
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
ms.openlocfilehash: 7c3ab3b014713c7facb86b8e8622d70e60a960e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="52c39-104">Az IoT átjáróként Intel NUC beállítása</span><span class="sxs-lookup"><span data-stu-id="52c39-104">Set up Intel NUC as an IoT gateway</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a><span data-ttu-id="52c39-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="52c39-105">What you will do</span></span>

- <span data-ttu-id="52c39-106">Az IoT-átjáró Intel NUC állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="52c39-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="52c39-107">Hello Intel NUC hello Azure IoT biztonsági csomag telepítését.</span><span class="sxs-lookup"><span data-stu-id="52c39-107">Install hello Azure IoT Edge package on hello Intel NUC.</span></span>
- <span data-ttu-id="52c39-108">Futtassa a "hello_world" mintaalkalmazás hello Intel NUC tooverify hello átjáró funkciót.</span><span class="sxs-lookup"><span data-stu-id="52c39-108">Run a "hello_world" sample application on hello Intel NUC tooverify hello gateway functionality.</span></span>

  > <span data-ttu-id="52c39-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="52c39-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="52c39-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="52c39-110">What you will learn</span></span>

<span data-ttu-id="52c39-111">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="52c39-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="52c39-112">Hogyan tooconnect Intel NUC a perifériaeszközökbe.</span><span class="sxs-lookup"><span data-stu-id="52c39-112">How tooconnect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="52c39-113">Hogyan tooinstall és frissítés szükséges hello csomagok Intel NUC használatával hello intelligens Package Manager.</span><span class="sxs-lookup"><span data-stu-id="52c39-113">How tooinstall and update hello required packages on Intel NUC using hello Smart Package Manager.</span></span>
- <span data-ttu-id="52c39-114">Hogyan toorun hello "hello_world" mintát alkalmazás tooverify hello átjáró funkciót.</span><span class="sxs-lookup"><span data-stu-id="52c39-114">How toorun hello "hello_world" sample application tooverify hello gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="52c39-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="52c39-115">What you need</span></span>

- <span data-ttu-id="52c39-116">Az Intel NUC Kit DE3815TYKE hello Intel IoT átjáró termékcsomag (szél folyó Linux * 7.0.0.13) előtelepítése mellett.</span><span class="sxs-lookup"><span data-stu-id="52c39-116">An Intel NUC Kit DE3815TYKE with hello Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span> <span data-ttu-id="52c39-117">[Kattintson ide toopurchase Groove IoT kereskedelmi átjáró Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span><span class="sxs-lookup"><span data-stu-id="52c39-117">[Click here toopurchase Grove IoT Commercial Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span></span>
- <span data-ttu-id="52c39-118">Kábel.</span><span class="sxs-lookup"><span data-stu-id="52c39-118">An Ethernet cable.</span></span>
- <span data-ttu-id="52c39-119">A billentyűzet.</span><span class="sxs-lookup"><span data-stu-id="52c39-119">A keyboard.</span></span>
- <span data-ttu-id="52c39-120">Egy HDMI vagy VGA kábel.</span><span class="sxs-lookup"><span data-stu-id="52c39-120">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="52c39-121">Egy figyelő HDMI vagy VGA-port.</span><span class="sxs-lookup"><span data-stu-id="52c39-121">A monitor with an HDMI or VGA port.</span></span>
- <span data-ttu-id="52c39-122">Választható lehetőség: [Texas eszközök érzékelő címke (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span><span class="sxs-lookup"><span data-stu-id="52c39-122">Optional: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span></span>

![Átjáró csomag](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a><span data-ttu-id="52c39-124">Csatlakozás Intel NUC hello perifériák</span><span class="sxs-lookup"><span data-stu-id="52c39-124">Connect Intel NUC with hello peripherals</span></span>

<span data-ttu-id="52c39-125">az alábbi képen hello Intel NUC különböző perifériák kapcsolódnak példája:</span><span class="sxs-lookup"><span data-stu-id="52c39-125">hello image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="52c39-126">Csatlakoztatott tooa billentyűzet.</span><span class="sxs-lookup"><span data-stu-id="52c39-126">Connected tooa keyboard.</span></span>
2. <span data-ttu-id="52c39-127">Csatlakoztatott tooa figyelő egy VGA vagy HDMI kábellel.</span><span class="sxs-lookup"><span data-stu-id="52c39-127">Connected tooa monitor with a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="52c39-128">Vezetékes Ethernet kábel hálózathoz csatlakoztatott tooa.</span><span class="sxs-lookup"><span data-stu-id="52c39-128">Connected tooa wired network with an Ethernet cable.</span></span>
4. <span data-ttu-id="52c39-129">Energiagazdálkodási kábellel csatlakoztatott tooa tápegység.</span><span class="sxs-lookup"><span data-stu-id="52c39-129">Connected tooa power supply with a power cable.</span></span>

![Intel NUC tooperipherals csatlakoztatva](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="52c39-131">Toohello Intel NUC rendszer csatlakoztatja a gazdaszámítógép keresztül Secure Shell (SSH)</span><span class="sxs-lookup"><span data-stu-id="52c39-131">Connect toohello Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="52c39-132">Billentyűzet és egy figyelő tooget hello IP-címet az Intel NUC eszköz lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="52c39-132">You will need a keyboard and a monitor tooget hello IP address of your Intel NUC device.</span></span> <span data-ttu-id="52c39-133">Ha már ismeri a hello IP-címet, ugorjon előre toostep 3 ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="52c39-133">If you already know hello IP address, you can skip ahead toostep 3 in this section.</span></span>

1. <span data-ttu-id="52c39-134">Kapcsolja be hello Intel NUC hello power gomb megnyomásával, majd jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="52c39-134">Turn on hello Intel NUC by pressing hello power button and then log in.</span></span>

   <span data-ttu-id="52c39-135">hello alapértelmezett felhasználónév és jelszó `root`.</span><span class="sxs-lookup"><span data-stu-id="52c39-135">hello default user name and password are both `root`.</span></span>

       > Hit hello enter key on your keyboard if you see either of hello following errors when you boot: 'A TPM error (7) occurred attempting tooread a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. <span data-ttu-id="52c39-136">Hello Intel NUC hello IP-címének lekéréséhez futtassa a hello `ifconfig` hello Intel NUC eszközön parancsot.</span><span class="sxs-lookup"><span data-stu-id="52c39-136">Get hello IP address of hello Intel NUC by running hello `ifconfig` command on hello Intel NUC device.</span></span>

   <span data-ttu-id="52c39-137">Itt található példa hello parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="52c39-137">Here is an example of hello command output.</span></span>

   ![Intel NUC IP megjelenítő ifconfig kimeneti](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="52c39-139">Ebben a példában hello érték, amely a következő `inet addr:` hello IP-cím szükséges, amikor kapcsolódik az Intel NUC toohello el gazdagépről.</span><span class="sxs-lookup"><span data-stu-id="52c39-139">In this example, hello value that follows `inet addr:` is hello IP address that you need when connect toohello Intel NUC from a host computer.</span></span>

3. <span data-ttu-id="52c39-140">A fogadó számítógép tooconnect tooIntel NUC az SSH-ügyfél következő hello egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="52c39-140">Use one of hello following SSH clients from your host computer tooconnect tooIntel NUC.</span></span>

    - <span data-ttu-id="52c39-141">[A puTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="52c39-141">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="52c39-142">hello beépített SSH-ügyfél Ubuntu vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="52c39-142">hello built-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="52c39-143">Hatékony és hatékony toooperate egy állomásról egy Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="52c39-143">It is more efficient and productive toooperate an Intel NUC from a host computer.</span></span> <span data-ttu-id="52c39-144">Akkor lesz szüksége hello Intel NUC IP-cím, felhasználói név és jelszó tooconnect tooit keresztül egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="52c39-144">You'll need hello Intel NUC's IP address, user name and password tooconnect tooit via an SSH client.</span></span> <span data-ttu-id="52c39-145">Íme egy példa, egy SSH-ügyfél által az macOS.</span><span class="sxs-lookup"><span data-stu-id="52c39-145">Here is an example that uses an SSH client on macOS.</span></span>
   <span data-ttu-id="52c39-146">![MacOS futó SSH-ügyfél](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="52c39-146">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-hello-azure-iot-edge-package"></a><span data-ttu-id="52c39-147">Hello Azure IoT biztonsági csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="52c39-147">Install hello Azure IoT Edge package</span></span>

<span data-ttu-id="52c39-148">hello Azure IoT biztonsági csomag hello előre lefordított bináris fájljait IoT széle és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="52c39-148">hello Azure IoT Edge package contains hello pre-compiled binaries of IoT Edge and its dependencies.</span></span> <span data-ttu-id="52c39-149">A bináris fájljait a Azure IoT él, hello Azure IoT SDK és megfelelő eszközök hello.</span><span class="sxs-lookup"><span data-stu-id="52c39-149">These binaries are Azure IoT Edge, hello Azure IoT SDK and hello corresponding tools.</span></span> <span data-ttu-id="52c39-150">hello csomag is tartalmaz egy "hello_world" mintaalkalmazás használt toovalidate hello átjáró funkciót.</span><span class="sxs-lookup"><span data-stu-id="52c39-150">hello package also contains a "hello_world" sample application is used toovalidate hello gateway functionality.</span></span> <span data-ttu-id="52c39-151">Az IoT-Edge része hello core hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="52c39-151">IoT Edge is hello core part of hello gateway.</span></span> 

<span data-ttu-id="52c39-152">Hajtsa végre az alábbi lépéseket tooinstall hello csomag.</span><span class="sxs-lookup"><span data-stu-id="52c39-152">Follow these steps tooinstall hello package.</span></span>

1. <span data-ttu-id="52c39-153">Hello IoT felhő tárház hozzáadása a következő parancsokat egy terminálablakot hello futtatásával:</span><span class="sxs-lookup"><span data-stu-id="52c39-153">Add hello IoT Cloud repository by running hello following commands in a terminal window:</span></span>

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > <span data-ttu-id="52c39-154">Adja meg, "y", ha akkor too'Include kéri, ezt a csatornát? "</span><span class="sxs-lookup"><span data-stu-id="52c39-154">Enter 'y', when it prompts you too'Include this channel?'</span></span>
   
   <span data-ttu-id="52c39-155">Ha megjelenik egy `import read failed(-1)` hiba, a következő parancsok tooresolve hello probléma használata hello:</span><span class="sxs-lookup"><span data-stu-id="52c39-155">If you receive an `import read failed(-1)` error, use hello following commands tooresolve hello issue:</span></span>
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   <span data-ttu-id="52c39-156">Hello `rpm` importálja hello rpm kulcs parancsot.</span><span class="sxs-lookup"><span data-stu-id="52c39-156">hello `rpm` command imports hello rpm key.</span></span> <span data-ttu-id="52c39-157">Hello `smart channel` parancs hozzáadja hello rpm csatorna toohello intelligens Package Manager.</span><span class="sxs-lookup"><span data-stu-id="52c39-157">hello `smart channel` command adds hello rpm channel toohello Smart Package Manager.</span></span> <span data-ttu-id="52c39-158">Hello futtatása előtt `smart update` parancsban, látni fogja, például egy kimeneti alatt.</span><span class="sxs-lookup"><span data-stu-id="52c39-158">Before you run hello `smart update` command, you will see an output like below.</span></span>

   ![rpm és intelligens csatorna parancs kimenete](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. <span data-ttu-id="52c39-160">Hello intelligens frissítés parancs hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="52c39-160">Execute hello smart update command:</span></span>

   ```bash
   smart update
   ```

3. <span data-ttu-id="52c39-161">Hello Azure IoT-átjáró csomag telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="52c39-161">Install hello Azure IoT Gateway package by running hello following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="52c39-162">`packagegroup-cloud-azure`hello csomag hello neve van.</span><span class="sxs-lookup"><span data-stu-id="52c39-162">`packagegroup-cloud-azure` is hello name of hello package.</span></span> <span data-ttu-id="52c39-163">Hello `smart install` parancs használt tooinstall hello csomag.</span><span class="sxs-lookup"><span data-stu-id="52c39-163">hello `smart install` command is used tooinstall hello package.</span></span>

    > <span data-ttu-id="52c39-164">Futtatási hello következő parancsot, ha ezt a hibaüzenetet látja: "nyilvános kulcs nem érhető el"</span><span class="sxs-lookup"><span data-stu-id="52c39-164">Run hello following command if you see this error: 'public key not available'</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > <span data-ttu-id="52c39-165">Indítsa újra a hello Intel NUC, ha ezt a hibaüzenetet látja: "nincs csomag biztosítja a fejlesztői linux util"</span><span class="sxs-lookup"><span data-stu-id="52c39-165">Reboot hello Intel NUC if you see this error: 'no package provides util-linux-dev'</span></span>

   <span data-ttu-id="52c39-166">Hello csomag telepítése után Intel NUC készen toofunction átjáróként.</span><span class="sxs-lookup"><span data-stu-id="52c39-166">After hello package is installed, Intel NUC is ready toofunction as a gateway.</span></span>

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="52c39-167">Hello Azure IoT peremhálózati "hello_world" mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="52c39-167">Run hello Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="52c39-168">a következő mintaalkalmazás hello hoz létre az átjáró egy `hello_world.json` fájlt, és hello Azure IoT peremhálózati architektúra toolog egy hello world tooa üzenetfájlt (log.txt) alapvető összetevőinek 5 másodpercentként használja.</span><span class="sxs-lookup"><span data-stu-id="52c39-168">hello following sample application creates a gateway from a `hello_world.json` file and uses hello fundamental components of Azure IoT Edge architecture toolog a hello world message tooa file (log.txt) every 5 seconds.</span></span>

<span data-ttu-id="52c39-169">Hello Hello World PéldaAlkalmazás futtathatja a következő hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="52c39-169">You can run hello Hello World sample by executing hello following commands:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="52c39-170">Hello Hello World alkalmazás futtatása néhány percet, és majd nyomja le az Enter kulcs toostop hello segítségével azt.</span><span class="sxs-lookup"><span data-stu-id="52c39-170">Let hello Hello World application run for a few minutes and then hit hello Enter key toostop it.</span></span>
<span data-ttu-id="52c39-171">![alkalmazás kimenete](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span><span class="sxs-lookup"><span data-stu-id="52c39-171">![application output](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span></span>

> <span data-ttu-id="52c39-172">A "argumentum érvénytelen handle(NULL)" hibák után le az ENTER billentyűt figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="52c39-172">You can ignore any 'invalid argument handle(NULL)' errors that appear after you hit Enter.</span></span>

<span data-ttu-id="52c39-173">Adott hello átjáró sikeresen lefutott, amelyik most már a hello_world mappában hello log.txt fájl megnyitásával ellenőrizheti ![log.txt directory megtekintése](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span><span class="sxs-lookup"><span data-stu-id="52c39-173">You can verify that hello gateway ran successfully by opening hello log.txt file that is now in your hello_world folder ![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span></span>

<span data-ttu-id="52c39-174">Nyissa meg a következő parancs hello segítségével log.txt:</span><span class="sxs-lookup"><span data-stu-id="52c39-174">Open log.txt using hello following command:</span></span>

```bash
vim log.txt
```

<span data-ttu-id="52c39-175">Ekkor megjelenik hello tartalmát log.txt, ez az üdvözlő naplózási üzenetek 5 másodpercentként hello átjáró Hello World modul által írt egy formázott JSON-kimenetét.</span><span class="sxs-lookup"><span data-stu-id="52c39-175">You will then see hello contents of log.txt, which will be a JSON formatted output of hello logging messages that were written every 5 seconds by hello gateway Hello World module.</span></span>
<span data-ttu-id="52c39-176">![log.txt directory megtekintése](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span><span class="sxs-lookup"><span data-stu-id="52c39-176">![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span></span>

<span data-ttu-id="52c39-177">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="52c39-177">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="52c39-178">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="52c39-178">Summary</span></span>

<span data-ttu-id="52c39-179">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="52c39-179">Congratulations!</span></span> <span data-ttu-id="52c39-180">Intel NUC átjáróként beállításának befejezése után.</span><span class="sxs-lookup"><span data-stu-id="52c39-180">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="52c39-181">Most már készen áll a következő lecke tooset toohello a fogadó számítógép toomove Azure IoT Hub létrehozása, és regisztrálja az Azure IoT Hub logikai eszközt.</span><span class="sxs-lookup"><span data-stu-id="52c39-181">Now you're ready toomove on toohello next lesson tooset up your host computer, create an Azure IoT Hub and register your Azure IoT Hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52c39-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="52c39-182">Next steps</span></span>
[<span data-ttu-id="52c39-183">Az IoT-átjáró tooconnect egy eszköz tooAzure IoT Hub használata</span><span class="sxs-lookup"><span data-stu-id="52c39-183">Use an IoT gateway tooconnect a device tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

