---
title: "Az IoT - Connect Raspberry pi (C) tooAzure hibaelhárítása |} Microsoft Docs"
description: "Hibaelhárítás lap hello málna Pi Node.js élmény"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT problémák, internet dolgot problémák"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 22cf50dc-8206-42a2-a1fc-f75fa85135fa
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8f0807104550e8e53a132f7741564b33f1db17ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="88dc4-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="88dc4-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="88dc4-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="88dc4-105">Hardware issues</span></span>
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a><span data-ttu-id="88dc4-106">hello alkalmazás futtatása is jól, de hello LED nem villogó.</span><span class="sxs-lookup"><span data-stu-id="88dc4-106">hello application runs well but hello LED is not blinking</span></span>
<span data-ttu-id="88dc4-107">A probléma mindig kapcsolódó toohardware expressroute kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="88dc4-107">This issue is always related toohardware circuit connectivity.</span></span> <span data-ttu-id="88dc4-108">A következő lépéseket tooidentify problémák hello használata:</span><span class="sxs-lookup"><span data-stu-id="88dc4-108">Use hello following steps tooidentify problems:</span></span>

1. <span data-ttu-id="88dc4-109">Ellenőrizze, hogy helyes-e hello választott **GPIO** a táblán.</span><span class="sxs-lookup"><span data-stu-id="88dc4-109">Check that you chose hello correct **GPIO** on your board.</span></span> <span data-ttu-id="88dc4-110">a két hello portokat kell **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**.</span><span class="sxs-lookup"><span data-stu-id="88dc4-110">hello two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="88dc4-111">Ellenőrizze, hogy helyes-e a LED hello polaritásának.</span><span class="sxs-lookup"><span data-stu-id="88dc4-111">Check that hello polarity of your LED is correct.</span></span> <span data-ttu-id="88dc4-112">hello hosszabb engedélyezi kell jeleznie hello **pozitív**, anód PIN-kódot.</span><span class="sxs-lookup"><span data-stu-id="88dc4-112">hello longer leg should indicate hello **positive**, anode pin.</span></span>
3. <span data-ttu-id="88dc4-113">Használjon hello **3.3V PIN-kód** és **GND PIN-kód** málna Pi 3.</span><span class="sxs-lookup"><span data-stu-id="88dc4-113">Use hello **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="88dc4-114">A Pi hello DC power kezelje.</span><span class="sxs-lookup"><span data-stu-id="88dc4-114">Treat Pi as hello DC power.</span></span> <span data-ttu-id="88dc4-115">Ellenőrizze, hogy hello LED remekül működik.</span><span class="sxs-lookup"><span data-stu-id="88dc4-115">Check that hello LED works fine.</span></span>

![LED meghatározása](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="88dc4-117">Más hardverekkel kapcsolatos problémák szerepelnek</span><span class="sxs-lookup"><span data-stu-id="88dc4-117">Other hardware issues</span></span>
<span data-ttu-id="88dc4-118">Málna Pi 3 gyakori problémáinak megoldásával kapcsolatban további információkért lásd: hello [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="88dc4-118">For information about solving common problems on Raspberry Pi 3, see hello [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="88dc4-119">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="88dc4-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="88dc4-120">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="88dc4-120">No response during gulp tasks</span></span>
<span data-ttu-id="88dc4-121">Ha a futó feladatok gulp problémákat tapasztal, adhat hozzá hello `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="88dc4-121">If you encounter problems in running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="88dc4-122">Próbálja tooterminate aktuális gulp feladatokat a Ctrl + C használatával, és futtassa a következő parancsot a konzol ablakot toosee hibakeresési üzeneteket hello.</span><span class="sxs-lookup"><span data-stu-id="88dc4-122">Try tooterminate current gulp tasks by using Ctrl + C, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="88dc4-123">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="88dc4-123">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="88dc4-124">Eszköz-felderítési problémák</span><span class="sxs-lookup"><span data-stu-id="88dc4-124">Device discovery issues</span></span>
<span data-ttu-id="88dc4-125">A Súgó hello gyakori problémák elhárításában `devdisco` paranccsal, ellenőrizze a hello [információs](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="88dc4-125">For help in troubleshooting common problems with hello `devdisco` command, check hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="88dc4-126">npm problémák</span><span class="sxs-lookup"><span data-stu-id="88dc4-126">npm issues</span></span>
<span data-ttu-id="88dc4-127">Próbálja meg tooupdate az npm csomag hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="88dc4-127">Try tooupdate your npm package by using hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="88dc4-128">Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span><span class="sxs-lookup"><span data-stu-id="88dc4-128">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="88dc4-129">Távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="88dc4-129">Remote debugging</span></span>
### <a name="run-hello-sample-application-in-debug-mode"></a><span data-ttu-id="88dc4-130">Hibakeresési módban hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="88dc4-130">Run hello sample application in debug mode</span></span>
```bash
gulp run --debug
```

<span data-ttu-id="88dc4-131">Amikor készen áll a hello hibakeresési motor, láthatja ```Debugger listening on port 5858``` a konzol kimeneti hello.</span><span class="sxs-lookup"><span data-stu-id="88dc4-131">When hello debug engine is ready, you should see ```Debugger listening on port 5858``` in hello console output.</span></span>

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a><span data-ttu-id="88dc4-132">Visual Studio Code tooconnect toohello távoli eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="88dc4-132">Configure Visual Studio Code tooconnect toohello remote device</span></span>
1. <span data-ttu-id="88dc4-133">Nyissa meg hello **Debug** a hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="88dc4-133">Open hello **Debug** panel on hello left side.</span></span>
2. <span data-ttu-id="88dc4-134">Kattintson a zöld hello **Start Debugging** (F5) gombra.</span><span class="sxs-lookup"><span data-stu-id="88dc4-134">Click hello green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="88dc4-135">A Visual Studio Code launch.json fájlt nyit meg.</span><span class="sxs-lookup"><span data-stu-id="88dc4-135">Visual Studio Code opens a launch.json file.</span></span>
3. <span data-ttu-id="88dc4-136">A következő tartalmat hello hello launch.json fájl frissítése.</span><span class="sxs-lookup"><span data-stu-id="88dc4-136">Update hello launch.json file with hello following content.</span></span> <span data-ttu-id="88dc4-137">Cserélje le `[device hostname or IP address]` hello tényleges IP cím vagy állomásnév eszköznévvel.</span><span class="sxs-lookup"><span data-stu-id="88dc4-137">Replace `[device hostname or IP address]` with hello actual device IP address or host name.</span></span>

> [!NOTE]
> <span data-ttu-id="88dc4-138">toolearn Visual Studio hibakeresési hello kapcsolatos további információkért tekintse meg a túl[hibakeresést a Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span><span class="sxs-lookup"><span data-stu-id="88dc4-138">toolearn more about hello Visual Studio Debugging, please refer too[Debugging in Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span></span>


```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Távoli hibakeresési konfigurálása](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a><span data-ttu-id="88dc4-140">Toohello távoli alkalmazás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="88dc4-140">Attach toohello remote application</span></span>
<span data-ttu-id="88dc4-141">Kattintson a zöld hello **Start Debugging** (F5) gomb toostart hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="88dc4-141">Click hello green **Start Debugging** (F5) button toostart debugging.</span></span>

<span data-ttu-id="88dc4-142">Olvasási [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn hello hibakereső többet.</span><span class="sxs-lookup"><span data-stu-id="88dc4-142">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn more about hello debugger.</span></span>

![Távoli, interaktív hibakeresés](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="88dc4-144">Az Azure CLI-problémák</span><span class="sxs-lookup"><span data-stu-id="88dc4-144">Azure CLI issues</span></span>
<span data-ttu-id="88dc4-145">az Azure parancssori felület (CLI) hello preview build.</span><span class="sxs-lookup"><span data-stu-id="88dc4-145">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="88dc4-146">tooseek megoldások hello használható [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="88dc4-146">tooseek solutions, you can use hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="88dc4-147">Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="88dc4-147">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="88dc4-148">Segítséget a gyakori problémák elhárításához, ellenőrizze a hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="88dc4-148">For help in troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

## <a name="python-installation-issues"></a><span data-ttu-id="88dc4-149">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="88dc4-149">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="88dc4-150">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="88dc4-150">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="88dc4-151">A pip telepítése, amikor egy engedély hibát vált ki, ha régebbi csomagok vannak telepítve a **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="88dc4-151">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="88dc4-152">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="88dc4-152">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="88dc4-153">A korábbi telepítés egyes pip csomagok hello engedély hibát okoz a legfelső szintű lettek létrehozva.</span><span class="sxs-lookup"><span data-stu-id="88dc4-153">Some pip packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="88dc4-154">megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van.</span><span class="sxs-lookup"><span data-stu-id="88dc4-154">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="88dc4-155">Ez a feladat használható a következő lépéseket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="88dc4-155">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="88dc4-156">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="88dc4-156">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="88dc4-157">Legfelső szintű által létrehozott csomagok:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="88dc4-157">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="88dc4-158">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="88dc4-158">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="88dc4-159">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="88dc4-159">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="88dc4-160">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="88dc4-160">Azure IoT Hub issues</span></span>
<span data-ttu-id="88dc4-161">Ha sikeresen korábban létesített az Azure IoT hub Azure parancssori felület segítségével, és egy eszköz toomanage hello eszközök tooyour IoT-központ csatlakozó van szüksége, próbálkozzon a következő eszközök hello.</span><span class="sxs-lookup"><span data-stu-id="88dc4-161">If you've successfully provisioned your Azure IoT hub with Azure CLI, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="88dc4-162">Eszköz explorer</span><span class="sxs-lookup"><span data-stu-id="88dc4-162">Device explorer</span></span>
<span data-ttu-id="88dc4-163">Hello [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eszköz a Windows helyi számítógépen fut, és az Azure IoT-központ tooyour csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="88dc4-163">hello [Device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) tool runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="88dc4-164">Hello következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="88dc4-164">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>


* <span data-ttu-id="88dc4-165">*Eszköz Identitáskezelés* tooprovision és az IoT hub regisztrált eszközök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="88dc4-165">*Device identity management* tooprovision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="88dc4-166">*Eszköz-felhő kap* , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="88dc4-166">*Receive device-to-cloud* so you can monitor messages sent from your device tooyour IoT hub.</span></span>
* <span data-ttu-id="88dc4-167">*Felhő eszközre küldése* úgy küldhet üzeneteket tooyour eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="88dc4-167">*Send cloud-to-device* so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="88dc4-168">Konfigurálja az IoT hub kapcsolati karakterlánc az eszköz toouse belül minden képességet.</span><span class="sxs-lookup"><span data-stu-id="88dc4-168">Configure your IoT hub connection string within this tool toouse all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="88dc4-169">IOT hubbal-explorer</span><span class="sxs-lookup"><span data-stu-id="88dc4-169">iothub-explorer</span></span>
<span data-ttu-id="88dc4-170">[IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközök.</span><span class="sxs-lookup"><span data-stu-id="88dc4-170">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage devices.</span></span> <span data-ttu-id="88dc4-171">A hello identitásjegyzékhez hello eszköz toomanage hello eszközöket használnak, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő-eszközre küldött üzenetek küldése.</span><span class="sxs-lookup"><span data-stu-id="88dc4-171">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device messages.</span></span>

<span data-ttu-id="88dc4-172">tooinstall hello hello IOT hubbal-explorer eszköz, futtassa a következő parancsot a parancssori környezetben hello legújabb (előzetes) verzióját:</span><span class="sxs-lookup"><span data-stu-id="88dc4-172">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="88dc4-173">A következő parancs tooget további súgó összes hello IOT hubbal-explorer parancsot és paramétereket hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="88dc4-173">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="88dc4-174">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="88dc4-174">Azure portal</span></span>
<span data-ttu-id="88dc4-175">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="88dc4-175">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="88dc4-176">Érdemes lehet toouse hello [Azure-portálon](../azure-portal-overview.md) toohelp kiépítését, kezelése és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="88dc4-176">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="88dc4-177">Az Azure tárolási problémák</span><span class="sxs-lookup"><span data-stu-id="88dc4-177">Azure Storage issues</span></span>
<span data-ttu-id="88dc4-178">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft használható toowork macOS, Linux és a Windows Azure Storage-adatokkal.</span><span class="sxs-lookup"><span data-stu-id="88dc4-178">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="88dc4-179">Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="88dc4-179">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="88dc4-180">Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.</span><span class="sxs-lookup"><span data-stu-id="88dc4-180">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

