---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 1: NUC beállítása |} Microsoft Docs"
description: "Az IoT-átjáró érzékelő és az Azure IoT Hub toocollect érzékelő adatokat közötti Intel NUC toowork beállítani, és elküldi a tooIoT központ."
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
ms.openlocfilehash: c2146c7cd8ca54adbd0af279364ec8484be1b45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="66aac-104">Az IoT átjáróként Intel NUC beállítása</span><span class="sxs-lookup"><span data-stu-id="66aac-104">Set up Intel NUC as an IoT gateway</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="66aac-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="66aac-105">What you will do</span></span>

- <span data-ttu-id="66aac-106">Az IoT-átjáró Intel NUC állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="66aac-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="66aac-107">Intel NUC hello Azure IoT biztonsági csomag telepítését.</span><span class="sxs-lookup"><span data-stu-id="66aac-107">Install hello Azure IoT Edge package on Intel NUC.</span></span>
- <span data-ttu-id="66aac-108">Futtassa a "hello_world" mintaalkalmazás Intel NUC tooverify hello átjáró funkciót.</span><span class="sxs-lookup"><span data-stu-id="66aac-108">Run a "hello_world" sample application on Intel NUC tooverify hello gateway functionality.</span></span>
<span data-ttu-id="66aac-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="66aac-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="66aac-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="66aac-110">What you will learn</span></span>

<span data-ttu-id="66aac-111">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="66aac-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="66aac-112">Hogyan tooconnect Intel NUC a perifériaeszközökbe.</span><span class="sxs-lookup"><span data-stu-id="66aac-112">How tooconnect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="66aac-113">Hogyan tooinstall és frissítés szükséges hello csomagok Intel NUC használatával hello intelligens Package Manager.</span><span class="sxs-lookup"><span data-stu-id="66aac-113">How tooinstall and update hello required packages on Intel NUC using hello Smart Package Manager.</span></span>
- <span data-ttu-id="66aac-114">Hogyan toorun hello "hello_world" mintát alkalmazás tooverify hello átjáró funkciót.</span><span class="sxs-lookup"><span data-stu-id="66aac-114">How toorun hello "hello_world" sample application tooverify hello gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="66aac-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="66aac-115">What you need</span></span>

- <span data-ttu-id="66aac-116">Az Intel NUC Kit DE3815TYKE hello Intel IoT átjáró termékcsomag (szél folyó Linux * 7.0.0.13) előtelepítése mellett.</span><span class="sxs-lookup"><span data-stu-id="66aac-116">An Intel NUC Kit DE3815TYKE with hello Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span>
- <span data-ttu-id="66aac-117">Kábel.</span><span class="sxs-lookup"><span data-stu-id="66aac-117">An Ethernet cable.</span></span>
- <span data-ttu-id="66aac-118">A billentyűzet.</span><span class="sxs-lookup"><span data-stu-id="66aac-118">A keyboard.</span></span>
- <span data-ttu-id="66aac-119">Egy HDMI vagy VGA kábel.</span><span class="sxs-lookup"><span data-stu-id="66aac-119">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="66aac-120">Egy figyelő HDMI vagy VGA-port.</span><span class="sxs-lookup"><span data-stu-id="66aac-120">A monitor with an HDMI or VGA port.</span></span>

![Átjáró csomag](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a><span data-ttu-id="66aac-122">Csatlakozás Intel NUC hello perifériák</span><span class="sxs-lookup"><span data-stu-id="66aac-122">Connect Intel NUC with hello peripherals</span></span>

<span data-ttu-id="66aac-123">az alábbi képen hello Intel NUC különböző perifériák kapcsolódnak példája:</span><span class="sxs-lookup"><span data-stu-id="66aac-123">hello image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="66aac-124">Csatlakoztatott tooa billentyűzet.</span><span class="sxs-lookup"><span data-stu-id="66aac-124">Connected tooa keyboard.</span></span>
2. <span data-ttu-id="66aac-125">Toohello figyelő által a VGA vagy HDMI kábellel csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="66aac-125">Connected toohello monitor by a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="66aac-126">Csatlakoztatott tooa hálózati vezetékes Ethernet-kábellel.</span><span class="sxs-lookup"><span data-stu-id="66aac-126">Connected tooa wired network by an Ethernet cable.</span></span>
4. <span data-ttu-id="66aac-127">Energiagazdálkodási kábellel csatlakoztatott toohello tápegység.</span><span class="sxs-lookup"><span data-stu-id="66aac-127">Connected toohello power supply with a power cable.</span></span>

![Intel NUC tooperipherals csatlakoztatva](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="66aac-129">Toohello Intel NUC rendszer csatlakoztatja a gazdaszámítógép keresztül Secure Shell (SSH)</span><span class="sxs-lookup"><span data-stu-id="66aac-129">Connect toohello Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="66aac-130">Itt billentyűzet és monitor tooget hello IP-címet a NUC eszköz van szüksége.</span><span class="sxs-lookup"><span data-stu-id="66aac-130">Here you need keyboard and monitor tooget hello IP address of your NUC device.</span></span> <span data-ttu-id="66aac-131">Ha már ismeri a hello IP-címet, kihagyhatja a toostep 3 ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="66aac-131">If you already know hello IP address, you can skip toostep 3 in this section.</span></span>

1. <span data-ttu-id="66aac-132">Kapcsolja be a Intel NUC hello rendszer hello Főkapcsoló és a naplófájlok lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="66aac-132">Turn on Intel NUC by pressing hello Power button and log in hello system.</span></span>

   <span data-ttu-id="66aac-133">hello alapértelmezett felhasználónév és jelszó `root`.</span><span class="sxs-lookup"><span data-stu-id="66aac-133">hello default user name and password are both `root`.</span></span>

2. <span data-ttu-id="66aac-134">Hello IP-címe NUC lekéréséhez futtassa a hello `ifconfig` parancsot.</span><span class="sxs-lookup"><span data-stu-id="66aac-134">Get hello IP address of NUC by running hello `ifconfig` command.</span></span> <span data-ttu-id="66aac-135">Ez a lépés hello NUC eszközön történik.</span><span class="sxs-lookup"><span data-stu-id="66aac-135">This step is done on hello NUC device.</span></span>

   <span data-ttu-id="66aac-136">Itt található példa hello parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="66aac-136">Here is an example of hello command output.</span></span>

   ![ifconfig kimeneti NUC IP megjelenítése](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="66aac-138">Ebben a példában hello érték, amely a következő `inet addr:` hello IP-cím, ha azt tervezi, hogy a gazdagép számítógép tooIntel NUC távolról a tooconnect.</span><span class="sxs-lookup"><span data-stu-id="66aac-138">In this example, hello value that follows `inet addr:` is hello IP address that you need when you plan tooconnect remotely from a host computer tooIntel NUC.</span></span>

3. <span data-ttu-id="66aac-139">SSH-ügyfél követően a gazdagép gép tooconnect tooIntel NUC hello egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="66aac-139">Use one of hello following SSH clients from your host machine tooconnect tooIntel NUC.</span></span>

   - <span data-ttu-id="66aac-140">[A puTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="66aac-140">[PuTTY](http://www.putty.org/) for Windows.</span></span>
   - <span data-ttu-id="66aac-141">hello beépített SSH-ügyfél Ubuntu vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="66aac-141">hello build-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="66aac-142">Egy számítógép az Intel NUC hatékonyabb és hatékony toooperate.</span><span class="sxs-lookup"><span data-stu-id="66aac-142">It is more efficient and productive toooperate on Intel NUC from a host computer.</span></span> <span data-ttu-id="66aac-143">Hello hello IP-cím, a felhasználónevet és jelszót kell tooconnect hello NUC SSH-ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="66aac-143">You need hello hello IP address, user name and password tooconnect hello NUC via SSH client.</span></span> <span data-ttu-id="66aac-144">Itt nincs macOS hello példa használata SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="66aac-144">Here is hello example use SSH client on macOS.</span></span>
   <span data-ttu-id="66aac-145">![MacOS futó SSH-ügyfél](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="66aac-145">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-hello-azure-iot-edge-package"></a><span data-ttu-id="66aac-146">Hello Azure IoT biztonsági csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="66aac-146">Install hello Azure IoT Edge package</span></span>

<span data-ttu-id="66aac-147">hello Azure IoT biztonsági csomag hello hello SDK előre lefordított bináris fájljait és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="66aac-147">hello Azure IoT Edge package contains hello pre-compiled binaries of hello SDK and its dependencies.</span></span> <span data-ttu-id="66aac-148">A bináris fájljait a Azure IoT él, hello Azure IoT SDK és megfelelő eszközök hello.</span><span class="sxs-lookup"><span data-stu-id="66aac-148">These binaries are Azure IoT Edge, hello Azure IoT SDK and hello corresponding tools.</span></span> <span data-ttu-id="66aac-149">hello csomag "hello_world" mintaalkalmazás, amely használt toovalidate hello átjáró funkciókat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="66aac-149">hello package also contains a "hello_world" sample application that is used toovalidate hello gateway functionality.</span></span> <span data-ttu-id="66aac-150">Az IoT-Edge része hello core hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="66aac-150">IoT Edge is hello core part of hello gateway.</span></span> <span data-ttu-id="66aac-151">tooinstall hello csomag, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="66aac-151">tooinstall hello package, follow these steps:</span></span>

1. <span data-ttu-id="66aac-152">Vegyen fel hello IoT felhőalapú-tárházat hello követő egy terminálablakot parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="66aac-152">Add hello IoT cloud repository by running hello following commands in a terminal window:</span></span>

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   <span data-ttu-id="66aac-153">Hello `rpm` importálja hello rpm kulcs parancsot.</span><span class="sxs-lookup"><span data-stu-id="66aac-153">hello `rpm` command imports hello rpm key.</span></span> <span data-ttu-id="66aac-154">Hello `smart channel` parancs hozzáadja hello rpm csatorna toohello intelligens Package Manager.</span><span class="sxs-lookup"><span data-stu-id="66aac-154">hello `smart channel` command adds hello rpm channel toohello Smart Package Manager.</span></span> <span data-ttu-id="66aac-155">Hello futtatása előtt `smart update` hasonló kimenetnek parancs, lásd alább.</span><span class="sxs-lookup"><span data-stu-id="66aac-155">Before you run hello `smart update` command, you see an output like below.</span></span>

   ![rpm és intelligens csatorna parancs kimenete](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. <span data-ttu-id="66aac-157">Hello telepítéséhez hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="66aac-157">Install hello package by running hello following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="66aac-158">`packagegroup-cloud-azure`hello csomag hello neve van.</span><span class="sxs-lookup"><span data-stu-id="66aac-158">`packagegroup-cloud-azure` is hello name of hello package.</span></span> <span data-ttu-id="66aac-159">Hello `smart install` parancs használt tooinstall hello csomag.</span><span class="sxs-lookup"><span data-stu-id="66aac-159">hello `smart install` command is used tooinstall hello package.</span></span>

   <span data-ttu-id="66aac-160">Hello csomag telepítése után Intel NUC várt toowork átjáróként.</span><span class="sxs-lookup"><span data-stu-id="66aac-160">After hello package is installed, Intel NUC is expected toowork as a gateway.</span></span>

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="66aac-161">Hello Azure IoT peremhálózati "hello_world" mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="66aac-161">Run hello Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="66aac-162">Nyissa meg túl`azureiotgatewaysdk/samples` és hello minta "hello_world" mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="66aac-162">Go too`azureiotgatewaysdk/samples` and run hello sample "hello_world" sample application.</span></span> <span data-ttu-id="66aac-163">A mintaalkalmazás létrehoz egy átjáró hello `hello_world.json` fájlt, és alapvető összetevői hello hello Azure IoT peremhálózati architektúra toolog egy hello world tooa üzenetfájlt 5 másodpercentként használja.</span><span class="sxs-lookup"><span data-stu-id="66aac-163">This sample application creates a gateway from hello `hello_world.json` file and uses hello fundamental components of hello Azure IoT Edge architecture toolog a hello world message tooa file every 5 seconds.</span></span>

<span data-ttu-id="66aac-164">Futtathatja a minta "hello_world" hello mintaalkalmazás hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="66aac-164">You can run hello sample "hello_world" sample application by running hello following command:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="66aac-165">hello mintaalkalmazás hoz létre, ha hello átjárófunkció megfelelően működik-e a kimeneti hello következő:</span><span class="sxs-lookup"><span data-stu-id="66aac-165">hello sample application produces hello following output if hello gateway functionality is working correctly:</span></span>

![alkalmazás kimenete](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

<span data-ttu-id="66aac-167">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="66aac-167">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="66aac-168">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="66aac-168">Summary</span></span>

<span data-ttu-id="66aac-169">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="66aac-169">Congratulations!</span></span> <span data-ttu-id="66aac-170">Intel NUC átjáróként beállításának befejezése után.</span><span class="sxs-lookup"><span data-stu-id="66aac-170">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="66aac-171">Most már készen áll a következő lecke tooset toohello a fogadó számítógép toomove Azure IoT hub létrehozása, és regisztrálja az Azure IoT hub logikai eszközt.</span><span class="sxs-lookup"><span data-stu-id="66aac-171">Now you're ready toomove on toohello next lesson tooset up your host computer, create an Azure IoT hub and register your Azure IoT hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66aac-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66aac-172">Next steps</span></span>
[<span data-ttu-id="66aac-173">Felkészülés a gazdaszámítógép és Azure IoT-központ</span><span class="sxs-lookup"><span data-stu-id="66aac-173">Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
