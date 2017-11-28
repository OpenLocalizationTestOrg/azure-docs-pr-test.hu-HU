---
title: "A szimulált eszköz & Azure IoT átjáró - hibaelhárítása |} Microsoft Docs"
description: "Intel NUC átjáró hibaelhárítási lap"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT problémák, internet dolgot problémák"
ROBOTS: NOINDEX
ms.assetid: 3ee8f4b0-5799-40a3-8cf0-8d5aa44dbc2b
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: eae4c112accaefa8bd1bf85f7b43badc2f491dfd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="e10f5-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="e10f5-104">Troubleshooting</span></span>

## <a name="hardware-issues"></a><span data-ttu-id="e10f5-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="e10f5-105">Hardware issues</span></span>

### <a name="ti-sensortag-cannot-be-connected"></a><span data-ttu-id="e10f5-106">Nem lehet csatlakozni a TI SensorTag</span><span class="sxs-lookup"><span data-stu-id="e10f5-106">TI SensorTag cannot be connected</span></span>

<span data-ttu-id="e10f5-107">SensorTag kapcsolódási problémák elhárításához használja a [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span><span class="sxs-lookup"><span data-stu-id="e10f5-107">To troubleshoot SensorTag connectivity issues, use the [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span></span>

### <a name="have-an-issue-with-intel-nuc"></a><span data-ttu-id="e10f5-108">Probléma lehet az Intel NUC</span><span class="sxs-lookup"><span data-stu-id="e10f5-108">Have an issue with Intel NUC</span></span>

<span data-ttu-id="e10f5-109">A rendszerindítási problémák megoldásához tekintse meg [Intel NUC nem rendszerindító hibaelhárítási](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span><span class="sxs-lookup"><span data-stu-id="e10f5-109">To troubleshoot boot issues, refer to [troubleshooting No Boot Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span></span>

<span data-ttu-id="e10f5-110">Operációs rendszerrel kapcsolatos problémák megoldásához tekintse meg [Intel NUC operációs rendszer hibaelhárítási](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span><span class="sxs-lookup"><span data-stu-id="e10f5-110">To troubleshoot operating system issues, refer to [troubleshooting Operating System Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span></span>

<span data-ttu-id="e10f5-111">Egyéb problémák elhárításához tekintse meg [villogni és az Intel NUC-e sípoló kódokat](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span><span class="sxs-lookup"><span data-stu-id="e10f5-111">To troubleshoot other issues, refer to [Blink Codes and Beep Codes for Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="e10f5-112">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="e10f5-112">Node.js package issues</span></span>

### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="e10f5-113">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="e10f5-113">No response during gulp tasks</span></span>

<span data-ttu-id="e10f5-114">Ha a futó feladatok gulp problémákat tapasztal, adhat hozzá a `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e10f5-114">If you encounter problems in running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="e10f5-115">Állítsa le a jelenlegi gulp feladatok használatával próbál `Ctrl + C`, és futtassa a következő parancsot a konzolablakban tekintse meg a hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="e10f5-115">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="e10f5-116">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="e10f5-116">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="e10f5-117">Eszköz-felderítési problémák</span><span class="sxs-lookup"><span data-stu-id="e10f5-117">Device discovery issues</span></span>

<span data-ttu-id="e10f5-118">A gyakori problémák elhárításában súgó a `discover-sensortag` parancs, ellenőrizze a [wiki lapján](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span><span class="sxs-lookup"><span data-stu-id="e10f5-118">For help in troubleshooting common problems with the `discover-sensortag` command, check the [wiki page](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="e10f5-119">npm problémák</span><span class="sxs-lookup"><span data-stu-id="e10f5-119">npm issues</span></span>

<span data-ttu-id="e10f5-120">Próbálja meg frissíteni az npm-csomagot a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e10f5-120">Try to update your npm package by running the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="e10f5-121">Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span><span class="sxs-lookup"><span data-stu-id="e10f5-121">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="e10f5-122">Távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="e10f5-122">Remote Debugging</span></span>
> <span data-ttu-id="e10f5-123">Alább utasításokat úgy van kialakítva, ebben az oktatóanyagban használt node.js parancsfájlokat hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="e10f5-123">Below instructions are meant for debugging node.js scripts used in this tutorial.</span></span>
### <a name="run-the-sample-application-in-debug-mode"></a><span data-ttu-id="e10f5-124">Futtassa a mintaalkalmazást hibakeresési módban</span><span class="sxs-lookup"><span data-stu-id="e10f5-124">Run the sample application in debug mode</span></span>

<span data-ttu-id="e10f5-125">Futtassa a mintaalkalmazást hibakeresési módban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e10f5-125">Run the sample application in debug mode by running the following command:</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="e10f5-126">Amikor készen áll a hibakeresési motor, megjelennie `Debugger listening on port 5858` a konzol kimeneti.</span><span class="sxs-lookup"><span data-stu-id="e10f5-126">When the debug engine is ready, you should see `Debugger listening on port 5858` in the console output.</span></span>

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a><span data-ttu-id="e10f5-127">Visual Studio Code kapcsolódni a távoli eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e10f5-127">Configure Visual Studio Code to connect to the remote device</span></span>

1. <span data-ttu-id="e10f5-128">Nyissa meg a **Debug** a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="e10f5-128">Open the **Debug** panel on the left side.</span></span>
2. <span data-ttu-id="e10f5-129">Kattintson a zöld **Start Debugging** (F5) gombra.</span><span class="sxs-lookup"><span data-stu-id="e10f5-129">Click the green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="e10f5-130">A Visual Studio Code megnyílik egy `launch.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="e10f5-130">Visual Studio Code opens a `launch.json` file.</span></span>
3. <span data-ttu-id="e10f5-131">Frissítés a `launch.json` fájlt az alábbi tartalommal.</span><span class="sxs-lookup"><span data-stu-id="e10f5-131">Update the `launch.json` file with the following content.</span></span> <span data-ttu-id="e10f5-132">Cserélje le `[device hostname or IP address]` , a tényleges eszköz IP-címét vagy állomásnevét nevét.</span><span class="sxs-lookup"><span data-stu-id="e10f5-132">Replace `[device hostname or IP address]` with the actual device IP address or host name.</span></span>

   ``` json
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
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Távoli hibakeresési konfigurálása](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a><span data-ttu-id="e10f5-134">A távoli alkalmazás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="e10f5-134">Attach to the remote application</span></span>

<span data-ttu-id="e10f5-135">Kattintson a zöld **Start Debugging** a hibakeresés (F5) gombra.</span><span class="sxs-lookup"><span data-stu-id="e10f5-135">Click the green **Start Debugging** (F5) button to start debugging.</span></span>

<span data-ttu-id="e10f5-136">Olvasási [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) további információt a hibakereső.</span><span class="sxs-lookup"><span data-stu-id="e10f5-136">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) to learn more about the debugger.</span></span>

![Hibakeresési BLA-minta](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="e10f5-138">Az Azure CLI-problémák</span><span class="sxs-lookup"><span data-stu-id="e10f5-138">Azure CLI issues</span></span>

<span data-ttu-id="e10f5-139">Az Azure parancssori felület (CLI) preview build.</span><span class="sxs-lookup"><span data-stu-id="e10f5-139">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="e10f5-140">Megoldásokat keres, használhatja a [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="e10f5-140">To seek solutions, you can use the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="e10f5-141">Ha az eszköz hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a a **problémák** a GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="e10f5-141">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="e10f5-142">A Súgó gyakori problémák elhárításához, tekintse meg a [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="e10f5-142">For help in troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="e10f5-143">Ha megfelel a "Egy olyan verzióra, amely eleget tesz a követelmény nem található", futtassa a következő parancsot a pip frissítése a legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="e10f5-143">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="e10f5-144">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="e10f5-144">Python installation issues</span></span>

### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="e10f5-145">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="e10f5-145">Legacy installation issues (macOS)</span></span>

<span data-ttu-id="e10f5-146">A pip telepítése, amikor egy engedély hibát vált ki, ha régebbi csomagok vannak telepítve a **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="e10f5-146">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="e10f5-147">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="e10f5-147">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="e10f5-148">A korábbi telepítés egyes pip csomagok létrehozásakor a gyökérszintű, amely azt eredményezi, az engedély hiba.</span><span class="sxs-lookup"><span data-stu-id="e10f5-148">Some pip packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="e10f5-149">A megoldás, hogy távolítsa el azokat a legfelső szintű telepített csomagokat.</span><span class="sxs-lookup"><span data-stu-id="e10f5-149">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="e10f5-150">Ez a feladat befejezéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e10f5-150">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="e10f5-151">Nyissa meg a következőt: `/usr/local/lib/python2.7/site-packages`</span><span class="sxs-lookup"><span data-stu-id="e10f5-151">Go to `/usr/local/lib/python2.7/site-packages`</span></span>
2. <span data-ttu-id="e10f5-152">Legfelső szintű által létrehozott csomagok:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="e10f5-152">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="e10f5-153">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="e10f5-153">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="e10f5-154">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="e10f5-154">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="e10f5-155">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="e10f5-155">Azure IoT Hub issues</span></span>

<span data-ttu-id="e10f5-156">Ha már sikeresen létesített az Azure IoT hub, az Azure parancssori felület segítségével, és kezelheti az eszközöket, amelyek csatlakozik, az IoT hub eszköz van szüksége, próbálkozzon a következő eszközök.</span><span class="sxs-lookup"><span data-stu-id="e10f5-156">If you've successfully provisioned your Azure IoT hub with the Azure CLI, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="e10f5-157">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="e10f5-157">Device Explorer</span></span>

<span data-ttu-id="e10f5-158">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT hub csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="e10f5-158">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="e10f5-159">A következő kommunikál [IoT-központok végpontjai](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span><span class="sxs-lookup"><span data-stu-id="e10f5-159">It communicates with the following [IoT Hub endpoints](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span></span>

- <span data-ttu-id="e10f5-160">Az IoT hub regisztrált eszköz Identitáskezelés szeretnék telepíteni és kezelni az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="e10f5-160">Device identity management to provision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="e10f5-161">Eszköz-felhő, figyelheti az IoT hub az eszközről küldött üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="e10f5-161">Receive device-to-cloud so you can monitor messages sent from your device to your IoT hub.</span></span>
- <span data-ttu-id="e10f5-162">Így küldhet üzeneteket az eszközök az IoT hub a felhőből eszközre küldése</span><span class="sxs-lookup"><span data-stu-id="e10f5-162">Send cloud-to-device so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="e10f5-163">Az IoT hub kapcsolati karakterlánc ebből az eszközből készülék képességeinek használatához konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e10f5-163">Configure your IoT hub connection string within this tool to use all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="e10f5-164">IOT hubbal-explorer</span><span class="sxs-lookup"><span data-stu-id="e10f5-164">iothub-explorer</span></span>

<span data-ttu-id="e10f5-165">[IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközügyfeleken kezelésére.</span><span class="sxs-lookup"><span data-stu-id="e10f5-165">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="e10f5-166">Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="e10f5-166">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="e10f5-167">Az IOT hubbal-explorer eszköz legújabb (előzetes) verziójának telepítéséhez futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e10f5-167">To install the latest (prerelease) version of the iothub-explorer tool, run the following command:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="e10f5-168">További segítség minden IOT hubbal-explorer parancsot és paramétereket, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e10f5-168">To get additional help about all the iothub-explorer commands and their parameters, run the following command:</span></span>

```bash
iothub-explorer help
```

### <a name="the-azure-portal"></a><span data-ttu-id="e10f5-169">Az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e10f5-169">The Azure portal</span></span>

<span data-ttu-id="e10f5-170">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="e10f5-170">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="e10f5-171">Érdemes lehet használni a [Azure-portálon](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) kiépítéséhez, kezeléséhez, és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="e10f5-171">You might also want to use the [Azure portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="e10f5-172">Az Azure tárolási problémák</span><span class="sxs-lookup"><span data-stu-id="e10f5-172">Azure Storage issues</span></span>

<span data-ttu-id="e10f5-173">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com/) egy különálló alkalmazás, a Microsoft, amelyek segítségével a Windows, a macOS és a Linux Azure Storage-adatokkal dolgozni.</span><span class="sxs-lookup"><span data-stu-id="e10f5-173">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="e10f5-174">Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e10f5-174">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="e10f5-175">Az eszköz segítségével az Azure Storage problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="e10f5-175">You can use this tool to troubleshoot your Azure Storage issues.</span></span>
