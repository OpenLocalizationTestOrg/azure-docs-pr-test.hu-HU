---
title: "Raspberry Pi (C) csatlakozzon az Azure IoT - hárítsa el a |} Microsoft Docs"
description: "A Pi Node.js málna élmény hibaelhárítási lap"
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
ms.openlocfilehash: f9058068972ddbb674d3cd159948835dc88c4451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="da9a1-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="da9a1-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="da9a1-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="da9a1-105">Hardware issues</span></span>
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a><span data-ttu-id="da9a1-106">Az alkalmazás futtatása is, de a LED nem villogó.</span><span class="sxs-lookup"><span data-stu-id="da9a1-106">The application runs well but the LED is not blinking</span></span>
<span data-ttu-id="da9a1-107">A probléma mindig a hardver áramkör kapcsolatra vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="da9a1-107">This issue is always related to hardware circuit connectivity.</span></span> <span data-ttu-id="da9a1-108">Az alábbi lépések segítségével azonosíthatja a problémákat:</span><span class="sxs-lookup"><span data-stu-id="da9a1-108">Use the following steps to identify problems:</span></span>

1. <span data-ttu-id="da9a1-109">Ellenőrizze, hogy úgy döntött, hogy a helyes **GPIO** a táblán.</span><span class="sxs-lookup"><span data-stu-id="da9a1-109">Check that you chose the correct **GPIO** on your board.</span></span> <span data-ttu-id="da9a1-110">A két portokat kell **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**.</span><span class="sxs-lookup"><span data-stu-id="da9a1-110">The two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="da9a1-111">Ellenőrizze, hogy helyesen-e a LED polaritásának.</span><span class="sxs-lookup"><span data-stu-id="da9a1-111">Check that the polarity of your LED is correct.</span></span> <span data-ttu-id="da9a1-112">A hosszabb engedélyezi kell jeleznie a **pozitív**, anód PIN-kódot.</span><span class="sxs-lookup"><span data-stu-id="da9a1-112">The longer leg should indicate the **positive**, anode pin.</span></span>
3. <span data-ttu-id="da9a1-113">Használja a **3.3V PIN-kód** és **GND PIN-kód** málna a Pi a 3.</span><span class="sxs-lookup"><span data-stu-id="da9a1-113">Use the **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="da9a1-114">A tartományvezérlő power Pi kezelje.</span><span class="sxs-lookup"><span data-stu-id="da9a1-114">Treat Pi as the DC power.</span></span> <span data-ttu-id="da9a1-115">Ellenőrizze, hogy jól működik a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="da9a1-115">Check that the LED works fine.</span></span>

![LED meghatározása](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="da9a1-117">Más hardverekkel kapcsolatos problémák szerepelnek</span><span class="sxs-lookup"><span data-stu-id="da9a1-117">Other hardware issues</span></span>
<span data-ttu-id="da9a1-118">Málna Pi 3 gyakori problémáinak megoldásával kapcsolatban további információkért lásd: a [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="da9a1-118">For information about solving common problems on Raspberry Pi 3, see the [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="da9a1-119">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="da9a1-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="da9a1-120">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="da9a1-120">No response during gulp tasks</span></span>
<span data-ttu-id="da9a1-121">Ha a futó feladatok gulp problémákat tapasztal, adhat hozzá a `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="da9a1-121">If you encounter problems in running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="da9a1-122">Próbálja meg a Ctrl + C aktuális gulp feladatok befejezéséhez, és futtassa a következő parancsot a konzolablakban, hogy hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="da9a1-122">Try to terminate current gulp tasks by using Ctrl + C, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="da9a1-123">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="da9a1-123">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="da9a1-124">Eszköz-felderítési problémák</span><span class="sxs-lookup"><span data-stu-id="da9a1-124">Device discovery issues</span></span>
<span data-ttu-id="da9a1-125">A gyakori problémák elhárításában súgó a `devdisco` parancs, ellenőrizze a [információs](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="da9a1-125">For help in troubleshooting common problems with the `devdisco` command, check the [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="da9a1-126">npm problémák</span><span class="sxs-lookup"><span data-stu-id="da9a1-126">npm issues</span></span>
<span data-ttu-id="da9a1-127">Próbálja meg frissíteni az npm-csomagot a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="da9a1-127">Try to update your npm package by using the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="da9a1-128">Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span><span class="sxs-lookup"><span data-stu-id="da9a1-128">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="da9a1-129">Távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="da9a1-129">Remote debugging</span></span>
### <a name="run-the-sample-application-in-debug-mode"></a><span data-ttu-id="da9a1-130">Futtassa a mintaalkalmazást hibakeresési módban</span><span class="sxs-lookup"><span data-stu-id="da9a1-130">Run the sample application in debug mode</span></span>
```bash
gulp run --debug
```

<span data-ttu-id="da9a1-131">Amikor készen áll a hibakeresési motor, megjelennie ```Debugger listening on port 5858``` a konzol kimeneti.</span><span class="sxs-lookup"><span data-stu-id="da9a1-131">When the debug engine is ready, you should see ```Debugger listening on port 5858``` in the console output.</span></span>

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a><span data-ttu-id="da9a1-132">Visual Studio Code kapcsolódni a távoli eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="da9a1-132">Configure Visual Studio Code to connect to the remote device</span></span>
1. <span data-ttu-id="da9a1-133">Nyissa meg a **Debug** a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="da9a1-133">Open the **Debug** panel on the left side.</span></span>
2. <span data-ttu-id="da9a1-134">Kattintson a zöld **Start Debugging** (F5) gombra.</span><span class="sxs-lookup"><span data-stu-id="da9a1-134">Click the green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="da9a1-135">A Visual Studio Code launch.json fájlt nyit meg.</span><span class="sxs-lookup"><span data-stu-id="da9a1-135">Visual Studio Code opens a launch.json file.</span></span>
3. <span data-ttu-id="da9a1-136">Frissítse a launch.json fájlt az alábbi tartalommal.</span><span class="sxs-lookup"><span data-stu-id="da9a1-136">Update the launch.json file with the following content.</span></span> <span data-ttu-id="da9a1-137">Cserélje le `[device hostname or IP address]` , a tényleges eszköz IP-címét vagy állomásnevét nevét.</span><span class="sxs-lookup"><span data-stu-id="da9a1-137">Replace `[device hostname or IP address]` with the actual device IP address or host name.</span></span>

> [!NOTE]
> <span data-ttu-id="da9a1-138">További információt a Visual Studio hibakeresési módját, tekintse meg [hibakeresést a Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span><span class="sxs-lookup"><span data-stu-id="da9a1-138">To learn more about the Visual Studio Debugging, please refer to [Debugging in Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span></span>


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

### <a name="attach-to-the-remote-application"></a><span data-ttu-id="da9a1-140">A távoli alkalmazás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="da9a1-140">Attach to the remote application</span></span>
<span data-ttu-id="da9a1-141">Kattintson a zöld **Start Debugging** a hibakeresés (F5) gombra.</span><span class="sxs-lookup"><span data-stu-id="da9a1-141">Click the green **Start Debugging** (F5) button to start debugging.</span></span>

<span data-ttu-id="da9a1-142">Olvasási [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) további információt a hibakereső.</span><span class="sxs-lookup"><span data-stu-id="da9a1-142">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) to learn more about the debugger.</span></span>

![Távoli, interaktív hibakeresés](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="da9a1-144">Az Azure CLI-problémák</span><span class="sxs-lookup"><span data-stu-id="da9a1-144">Azure CLI issues</span></span>
<span data-ttu-id="da9a1-145">Az Azure parancssori felület (CLI) preview build.</span><span class="sxs-lookup"><span data-stu-id="da9a1-145">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="da9a1-146">Megoldásokat keres, használhatja a [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="da9a1-146">To seek solutions, you can use the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="da9a1-147">Ha az eszköz hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a a **problémák** a GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="da9a1-147">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="da9a1-148">A Súgó gyakori problémák elhárításához, tekintse meg a [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="da9a1-148">For help in troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

## <a name="python-installation-issues"></a><span data-ttu-id="da9a1-149">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="da9a1-149">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="da9a1-150">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="da9a1-150">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="da9a1-151">A pip telepítése, amikor egy engedély hibát vált ki, ha régebbi csomagok vannak telepítve a **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="da9a1-151">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="da9a1-152">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="da9a1-152">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="da9a1-153">A korábbi telepítés egyes pip csomagok létrehozásakor a gyökérszintű, amely azt eredményezi, az engedély hiba.</span><span class="sxs-lookup"><span data-stu-id="da9a1-153">Some pip packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="da9a1-154">A megoldás, hogy távolítsa el azokat a legfelső szintű telepített csomagokat.</span><span class="sxs-lookup"><span data-stu-id="da9a1-154">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="da9a1-155">Ez a feladat befejezéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="da9a1-155">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="da9a1-156">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="da9a1-156">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="da9a1-157">Legfelső szintű által létrehozott csomagok:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="da9a1-157">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="da9a1-158">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="da9a1-158">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="da9a1-159">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="da9a1-159">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="da9a1-160">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="da9a1-160">Azure IoT Hub issues</span></span>
<span data-ttu-id="da9a1-161">Ha sikeresen korábban létesített az Azure IoT hub Azure parancssori felület segítségével, és kezelheti az eszközöket, amelyek csatlakozik, az IoT hub eszköz van szüksége, próbálkozzon a következő eszközök.</span><span class="sxs-lookup"><span data-stu-id="da9a1-161">If you've successfully provisioned your Azure IoT hub with Azure CLI, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="da9a1-162">Eszköz explorer</span><span class="sxs-lookup"><span data-stu-id="da9a1-162">Device explorer</span></span>
<span data-ttu-id="da9a1-163">A [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eszköz a Windows helyi számítógépen fut, és az Azure IoT hub csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="da9a1-163">The [Device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) tool runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="da9a1-164">A következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="da9a1-164">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>


* <span data-ttu-id="da9a1-165">*Eszköz Identitáskezelés* szeretnék telepíteni és kezelni az IoT hub regisztrált eszközökre.</span><span class="sxs-lookup"><span data-stu-id="da9a1-165">*Device identity management* to provision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="da9a1-166">*Eszköz-felhő kap* , figyelheti az IoT hub az eszközről küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="da9a1-166">*Receive device-to-cloud* so you can monitor messages sent from your device to your IoT hub.</span></span>
* <span data-ttu-id="da9a1-167">*Felhő eszközre küldése* úgy küldhet üzeneteket az eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="da9a1-167">*Send cloud-to-device* so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="da9a1-168">Az IoT hub kapcsolati karakterlánc ebből az eszközből készülék képességeinek használatához konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="da9a1-168">Configure your IoT hub connection string within this tool to use all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="da9a1-169">IOT hubbal-explorer</span><span class="sxs-lookup"><span data-stu-id="da9a1-169">iothub-explorer</span></span>
<span data-ttu-id="da9a1-170">[IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközök kezelésére.</span><span class="sxs-lookup"><span data-stu-id="da9a1-170">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage devices.</span></span> <span data-ttu-id="da9a1-171">Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő-eszközre küldött üzenetek küldése.</span><span class="sxs-lookup"><span data-stu-id="da9a1-171">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device messages.</span></span>

<span data-ttu-id="da9a1-172">Telepítse a legújabb (előzetes) verzióját az IOT hubbal-explorer eszköz, a következő parancsot a parancssori környezetben:</span><span class="sxs-lookup"><span data-stu-id="da9a1-172">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="da9a1-173">A következő paranccsal minden IOT hubbal-explorer parancsot és paramétereket kapcsolatos további segítség:</span><span class="sxs-lookup"><span data-stu-id="da9a1-173">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="da9a1-174">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="da9a1-174">Azure portal</span></span>
<span data-ttu-id="da9a1-175">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="da9a1-175">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="da9a1-176">Érdemes lehet használni a [Azure-portálon](../azure-portal-overview.md) kiépítéséhez, kezeléséhez, és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="da9a1-176">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="da9a1-177">Az Azure tárolási problémák</span><span class="sxs-lookup"><span data-stu-id="da9a1-177">Azure Storage issues</span></span>
<span data-ttu-id="da9a1-178">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft, amelyek segítségével a Windows, a macOS és a Linux Azure Storage-adatokkal dolgozni.</span><span class="sxs-lookup"><span data-stu-id="da9a1-178">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="da9a1-179">Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="da9a1-179">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="da9a1-180">Az eszköz segítségével az Azure Storage problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="da9a1-180">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

