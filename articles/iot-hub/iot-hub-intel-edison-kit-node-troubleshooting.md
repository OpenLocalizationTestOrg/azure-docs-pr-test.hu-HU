---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - 4. lecke: hibaelhárítása |} Microsoft Docs"
description: "Hibaelhárítás lap Intel Edison Node.js-élmény"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino hibaelhárítása"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: f11c5521-8a36-44c0-baad-f5ec26ab4618
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d54dd4a13ed87152fb6c039f38a5ffe8b4470df9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="6734f-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="6734f-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="6734f-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="6734f-105">Hardware issues</span></span>
<span data-ttu-id="6734f-106">Intel Edison gyakori problémáinak megoldásával kapcsolatban további információkért lásd: a [hivatalos hibaelhárítási lap](https://software.intel.com/en-us/node/637974).</span><span class="sxs-lookup"><span data-stu-id="6734f-106">For information about solving common problems on Intel Edison, see the [official troubleshooting page](https://software.intel.com/en-us/node/637974).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="6734f-107">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="6734f-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="6734f-108">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="6734f-108">No response during gulp tasks</span></span>
<span data-ttu-id="6734f-109">Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá a `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6734f-109">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="6734f-110">Állítsa le a jelenlegi gulp feladatok használatával próbál `Ctrl + C`, és futtassa a következő parancsot a konzolablakban tekintse meg a hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="6734f-110">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="6734f-111">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="6734f-111">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="npm-issues"></a><span data-ttu-id="6734f-112">NPM problémák</span><span class="sxs-lookup"><span data-stu-id="6734f-112">NPM issues</span></span>
<span data-ttu-id="6734f-113">Próbálja meg frissíteni az NPM-csomagot a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6734f-113">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="6734f-114">Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="6734f-114">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="6734f-115">Távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="6734f-115">Remote debugging</span></span>

### <a name="run-the-sample-application-in-debug-mode"></a><span data-ttu-id="6734f-116">Futtassa a mintaalkalmazást hibakeresési módban</span><span class="sxs-lookup"><span data-stu-id="6734f-116">Run the sample application in debug mode</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="6734f-117">Ha készen áll a hibakeresési motor, kell tudni ```Debugger listening on port 5858``` a konzol kimeneti a.</span><span class="sxs-lookup"><span data-stu-id="6734f-117">Once the debug engine is ready, you should be able to see ```Debugger listening on port 5858``` from the console output.</span></span>

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a><span data-ttu-id="6734f-118">Visual STUDIO Code kapcsolódni a távoli eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6734f-118">Configure VS Code to connect to the remote device</span></span>

<span data-ttu-id="6734f-119">Nyissa meg a **Debug** bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="6734f-119">Open the **Debug** panel from the left side.</span></span>

<span data-ttu-id="6734f-120">Kattintson a zöld **Start Debugging** (F5) gombra.</span><span class="sxs-lookup"><span data-stu-id="6734f-120">Click the green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="6734f-121">Visual STUDIO Code nyitna a **launch.json** fájl, amelyet frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="6734f-121">VS Code would open a **launch.json** file, which you need to update.</span></span>

<span data-ttu-id="6734f-122">Frissítés a **launch.json** fájlt a következő tartalmat, hogy lecseréli `[device hostname or IP address]` a tényleges eszköz IP-címet vagy állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="6734f-122">Update the **launch.json** file with the following content, replace `[device hostname or IP address]` with the actual device IP address or hostname.</span></span>  

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

![Távoli hibakeresési konfigurálása](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a><span data-ttu-id="6734f-124">A távoli alkalmazás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="6734f-124">Attach to the remote application</span></span>

<span data-ttu-id="6734f-125">Kattintson a zöld **Start Debugging** (F5) gombra, és kihasználhatják a hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="6734f-125">Click the green **Start Debugging** (F5) button and enjoy debugging.</span></span>

<span data-ttu-id="6734f-126">Elolvashatja [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) további információt a hibakereső.</span><span class="sxs-lookup"><span data-stu-id="6734f-126">You can read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) to learn more about the debugger.</span></span>

![Távoli, interaktív hibakeresés](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="6734f-128">Azure-CLI problémák</span><span class="sxs-lookup"><span data-stu-id="6734f-128">Azure-CLI issues</span></span>
<span data-ttu-id="6734f-129">Az Azure parancssori felület (CLI) preview build.</span><span class="sxs-lookup"><span data-stu-id="6734f-129">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="6734f-130">Keresse meg a megoldást a [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) megoldások keresése.</span><span class="sxs-lookup"><span data-stu-id="6734f-130">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="6734f-131">Próbálja meg az Azure-cli parancsok nem a várt módon működik legújabb verziójára történő frissítés.</span><span class="sxs-lookup"><span data-stu-id="6734f-131">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="6734f-132">Ha az eszköz hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a a **problémák** a GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="6734f-132">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="6734f-133">A gyakori problémák elhárításához tekintse meg a [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="6734f-133">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="6734f-134">Ha megfelel a "Egy olyan verzióra, amely eleget tesz a követelmény nem található", futtassa a következő parancsot a pip frissítése a legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="6734f-134">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="6734f-135">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="6734f-135">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="6734f-136">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="6734f-136">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="6734f-137">Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="6734f-137">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="6734f-138">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="6734f-138">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="6734f-139">Néhány **pip** a korábbi telepítés csomagok létrehozásakor a gyökérszintű, amely azt eredményezi, az engedély hiba.</span><span class="sxs-lookup"><span data-stu-id="6734f-139">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="6734f-140">A megoldás, hogy távolítsa el azokat a legfelső szintű telepített csomagokat.</span><span class="sxs-lookup"><span data-stu-id="6734f-140">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="6734f-141">Ez a feladat befejezéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6734f-141">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="6734f-142">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="6734f-142">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="6734f-143">Csomagok létrehozása a legfelső szintű:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="6734f-143">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="6734f-144">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="6734f-144">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="6734f-145">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="6734f-145">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="6734f-146">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="6734f-146">Azure IoT Hub issues</span></span>
<span data-ttu-id="6734f-147">Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és meg kell-e olyan eszköz, amely az IoT hub csatlakozó eszközök kezelésére, és próbálkozzon az alábbi eszközöket:</span><span class="sxs-lookup"><span data-stu-id="6734f-147">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="6734f-148">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="6734f-148">Device Explorer</span></span>
<span data-ttu-id="6734f-149">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT hub csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="6734f-149">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="6734f-150">A következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="6734f-150">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

- <span data-ttu-id="6734f-151">_Eszköz Identitáskezelés_ szeretnék telepíteni és kezelni az IoT hub regisztrált eszközökre.</span><span class="sxs-lookup"><span data-stu-id="6734f-151">_Device identity management_ to provision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="6734f-152">_Eszköz-felhő kap_ , figyelheti az IoT hub az eszközről küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="6734f-152">_Receive device-to-cloud_ so you can monitor messages sent from your device to your IoT hub.</span></span>
- <span data-ttu-id="6734f-153">_Felhő eszközre küldése_ úgy küldhet üzeneteket az eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="6734f-153">_Send cloud-to-device_ so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="6734f-154">Konfigurálja a `IoT hub connection string` ebből az eszközből készülék képességeinek használatához.</span><span class="sxs-lookup"><span data-stu-id="6734f-154">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="6734f-155">Az IoT-központ Explorer</span><span class="sxs-lookup"><span data-stu-id="6734f-155">IoT hub Explorer</span></span>
<span data-ttu-id="6734f-156">[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközügyfeleken kezelésére.</span><span class="sxs-lookup"><span data-stu-id="6734f-156">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="6734f-157">Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="6734f-157">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="6734f-158">Telepítse a legújabb (előzetes) verzióját az IOT hubbal-explorer eszköz, a következő parancsot a parancssori környezetben:</span><span class="sxs-lookup"><span data-stu-id="6734f-158">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="6734f-159">A következő paranccsal minden IOT hubbal-explorer parancsot és paramétereket kapcsolatos további segítség:</span><span class="sxs-lookup"><span data-stu-id="6734f-159">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="6734f-160">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6734f-160">Azure portal</span></span>
<span data-ttu-id="6734f-161">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="6734f-161">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="6734f-162">Érdemes lehet használni a [Azure-portálon](../azure-portal-overview.md) kiépítéséhez, kezeléséhez, és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="6734f-162">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="6734f-163">Az Azure storage-problémák</span><span class="sxs-lookup"><span data-stu-id="6734f-163">Azure storage issues</span></span>
<span data-ttu-id="6734f-164">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, amelyekkel dolgozni Microsoft [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) Windows, a macOS és a Linux adatok.</span><span class="sxs-lookup"><span data-stu-id="6734f-164">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="6734f-165">Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="6734f-165">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="6734f-166">Az eszköz segítségével az Azure Storage problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="6734f-166">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6734f-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6734f-167">Next steps</span></span>
<span data-ttu-id="6734f-168">Ezen a lapon csak Intel Edison Kit leggyakoribb problémákat foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="6734f-168">This page only includes the most common problems of Intel Edison kit.</span></span> <span data-ttu-id="6734f-169">Alsó megjegyzések további hibaelhárítási jelentés problémákat is hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="6734f-169">You can also leave bottom comments to report issues for further troubleshooting.</span></span>

<span data-ttu-id="6734f-170">Lépjen vissza a [Intel Edison (Node.js) az első lépései](iot-hub-intel-edison-kit-node-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="6734f-170">Go back to [Get started with Intel Edison (Node.js)](iot-hub-intel-edison-kit-node-get-started.md)</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-node-edison-getting-started