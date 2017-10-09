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
ms.openlocfilehash: cc7add6273e66aadeca9a4915a44f5edf61a0a59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="22a73-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="22a73-104">Troubleshooting</span></span>

## <a name="hardware-issues"></a><span data-ttu-id="22a73-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="22a73-105">Hardware issues</span></span>

### <a name="ti-sensortag-cannot-be-connected"></a><span data-ttu-id="22a73-106">Nem lehet csatlakozni a TI SensorTag</span><span class="sxs-lookup"><span data-stu-id="22a73-106">TI SensorTag cannot be connected</span></span>

<span data-ttu-id="22a73-107">tootroubleshoot SensorTag kapcsolódási problémák, használja a hello [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span><span class="sxs-lookup"><span data-stu-id="22a73-107">tootroubleshoot SensorTag connectivity issues, use hello [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span></span>

### <a name="have-an-issue-with-intel-nuc"></a><span data-ttu-id="22a73-108">Probléma lehet az Intel NUC</span><span class="sxs-lookup"><span data-stu-id="22a73-108">Have an issue with Intel NUC</span></span>

<span data-ttu-id="22a73-109">tootroubleshoot rendszerindítási problémák, tekintse meg a túl[Intel NUC nem rendszerindító hibaelhárítási](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span><span class="sxs-lookup"><span data-stu-id="22a73-109">tootroubleshoot boot issues, refer too[troubleshooting No Boot Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span></span>

<span data-ttu-id="22a73-110">tootroubleshoot operációs rendszerrel kapcsolatos problémák, tekintse meg a túl[Intel NUC operációs rendszer hibaelhárítási](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span><span class="sxs-lookup"><span data-stu-id="22a73-110">tootroubleshoot operating system issues, refer too[troubleshooting Operating System Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span></span>

<span data-ttu-id="22a73-111">tootroubleshoot más olyan problémák, tekintse meg a túl[villogni és az Intel NUC-e sípoló kódokat](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span><span class="sxs-lookup"><span data-stu-id="22a73-111">tootroubleshoot other issues, refer too[Blink Codes and Beep Codes for Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="22a73-112">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="22a73-112">Node.js package issues</span></span>

### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="22a73-113">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="22a73-113">No response during gulp tasks</span></span>

<span data-ttu-id="22a73-114">Ha a futó feladatok gulp problémákat tapasztal, adhat hozzá hello `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="22a73-114">If you encounter problems in running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="22a73-115">Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="22a73-115">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="22a73-116">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="22a73-116">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="22a73-117">Eszköz-felderítési problémák</span><span class="sxs-lookup"><span data-stu-id="22a73-117">Device discovery issues</span></span>

<span data-ttu-id="22a73-118">A Súgó hello gyakori problémák elhárításában `discover-sensortag` paranccsal, ellenőrizze a hello [wiki lapján](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span><span class="sxs-lookup"><span data-stu-id="22a73-118">For help in troubleshooting common problems with hello `discover-sensortag` command, check hello [wiki page](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="22a73-119">npm problémák</span><span class="sxs-lookup"><span data-stu-id="22a73-119">npm issues</span></span>

<span data-ttu-id="22a73-120">Próbálja meg tooupdate az npm csomag hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="22a73-120">Try tooupdate your npm package by running hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="22a73-121">Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span><span class="sxs-lookup"><span data-stu-id="22a73-121">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="22a73-122">Távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="22a73-122">Remote Debugging</span></span>
> <span data-ttu-id="22a73-123">Alább utasításokat úgy van kialakítva, ebben az oktatóanyagban használt node.js parancsfájlokat hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="22a73-123">Below instructions are meant for debugging node.js scripts used in this tutorial.</span></span>
### <a name="run-hello-sample-application-in-debug-mode"></a><span data-ttu-id="22a73-124">Hibakeresési módban hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="22a73-124">Run hello sample application in debug mode</span></span>

<span data-ttu-id="22a73-125">Futtassa a mintaalkalmazást hello hibakeresési módban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="22a73-125">Run hello sample application in debug mode by running hello following command:</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="22a73-126">Amikor készen áll a hello hibakeresési motor, láthatja `Debugger listening on port 5858` a konzol kimeneti hello.</span><span class="sxs-lookup"><span data-stu-id="22a73-126">When hello debug engine is ready, you should see `Debugger listening on port 5858` in hello console output.</span></span>

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a><span data-ttu-id="22a73-127">Visual Studio Code tooconnect toohello távoli eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="22a73-127">Configure Visual Studio Code tooconnect toohello remote device</span></span>

1. <span data-ttu-id="22a73-128">Nyissa meg hello **Debug** a hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="22a73-128">Open hello **Debug** panel on hello left side.</span></span>
2. <span data-ttu-id="22a73-129">Kattintson a zöld hello **Start Debugging** (F5) gombra.</span><span class="sxs-lookup"><span data-stu-id="22a73-129">Click hello green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="22a73-130">A Visual Studio Code megnyílik egy `launch.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="22a73-130">Visual Studio Code opens a `launch.json` file.</span></span>
3. <span data-ttu-id="22a73-131">Frissítés hello `launch.json` hello tartalom a következő fájl.</span><span class="sxs-lookup"><span data-stu-id="22a73-131">Update hello `launch.json` file with hello following content.</span></span> <span data-ttu-id="22a73-132">Cserélje le `[device hostname or IP address]` hello tényleges IP cím vagy állomásnév eszköznévvel.</span><span class="sxs-lookup"><span data-stu-id="22a73-132">Replace `[device hostname or IP address]` with hello actual device IP address or host name.</span></span>

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

### <a name="attach-toohello-remote-application"></a><span data-ttu-id="22a73-134">Toohello távoli alkalmazás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="22a73-134">Attach toohello remote application</span></span>

<span data-ttu-id="22a73-135">Kattintson a zöld hello **Start Debugging** (F5) gomb toostart hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="22a73-135">Click hello green **Start Debugging** (F5) button toostart debugging.</span></span>

<span data-ttu-id="22a73-136">Olvasási [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn hello hibakereső többet.</span><span class="sxs-lookup"><span data-stu-id="22a73-136">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn more about hello debugger.</span></span>

![Hibakeresési BLA-minta](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="22a73-138">Az Azure CLI-problémák</span><span class="sxs-lookup"><span data-stu-id="22a73-138">Azure CLI issues</span></span>

<span data-ttu-id="22a73-139">az Azure parancssori felület (CLI) hello preview build.</span><span class="sxs-lookup"><span data-stu-id="22a73-139">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="22a73-140">tooseek megoldások hello használható [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="22a73-140">tooseek solutions, you can use hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="22a73-141">Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="22a73-141">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="22a73-142">Segítséget a gyakori problémák elhárításához, ellenőrizze a hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="22a73-142">For help in troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="22a73-143">Ha megfelel a "Nem található olyan verzióra, amely eleget tesz a hello követelmény", adjon futtatási hello következő parancsot a tooupgrade pip toolastest verziója.</span><span class="sxs-lookup"><span data-stu-id="22a73-143">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="22a73-144">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="22a73-144">Python installation issues</span></span>

### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="22a73-145">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="22a73-145">Legacy installation issues (macOS)</span></span>

<span data-ttu-id="22a73-146">A pip telepítése, amikor egy engedély hibát vált ki, ha régebbi csomagok vannak telepítve a **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="22a73-146">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="22a73-147">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="22a73-147">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="22a73-148">A korábbi telepítés egyes pip csomagok hello engedély hibát okoz a legfelső szintű lettek létrehozva.</span><span class="sxs-lookup"><span data-stu-id="22a73-148">Some pip packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="22a73-149">megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van.</span><span class="sxs-lookup"><span data-stu-id="22a73-149">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="22a73-150">Ez a feladat használható a következő lépéseket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="22a73-150">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="22a73-151">Nyissa meg túl`/usr/local/lib/python2.7/site-packages`</span><span class="sxs-lookup"><span data-stu-id="22a73-151">Go too`/usr/local/lib/python2.7/site-packages`</span></span>
2. <span data-ttu-id="22a73-152">Legfelső szintű által létrehozott csomagok:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="22a73-152">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="22a73-153">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="22a73-153">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="22a73-154">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="22a73-154">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="22a73-155">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="22a73-155">Azure IoT Hub issues</span></span>

<span data-ttu-id="22a73-156">Ha sikeresen korábban létesített a hello Azure CLI Azure IoT hubot, és egy eszköz toomanage hello eszközök tooyour IoT-központ csatlakozó van szüksége, próbálkozzon a következő eszközök hello.</span><span class="sxs-lookup"><span data-stu-id="22a73-156">If you've successfully provisioned your Azure IoT hub with hello Azure CLI, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="22a73-157">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="22a73-157">Device Explorer</span></span>

<span data-ttu-id="22a73-158">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT-központ tooyour csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="22a73-158">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="22a73-159">Hello következő kommunikál [IoT-központok végpontjai](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span><span class="sxs-lookup"><span data-stu-id="22a73-159">It communicates with hello following [IoT Hub endpoints](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span></span>

- <span data-ttu-id="22a73-160">Eszköz identity management tooprovision és az IoT hub regisztrált eszközök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="22a73-160">Device identity management tooprovision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="22a73-161">Eszköz-felhő, megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="22a73-161">Receive device-to-cloud so you can monitor messages sent from your device tooyour IoT hub.</span></span>
- <span data-ttu-id="22a73-162">Úgy is küldhet üzenetet tooyour eszközök az IoT hub a felhőből eszközre küldése</span><span class="sxs-lookup"><span data-stu-id="22a73-162">Send cloud-to-device so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="22a73-163">Konfigurálja az IoT hub kapcsolati karakterlánc az eszköz toouse belül minden képességet.</span><span class="sxs-lookup"><span data-stu-id="22a73-163">Configure your IoT hub connection string within this tool toouse all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="22a73-164">IOT hubbal-explorer</span><span class="sxs-lookup"><span data-stu-id="22a73-164">iothub-explorer</span></span>

<span data-ttu-id="22a73-165">[IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközügyfeleitől.</span><span class="sxs-lookup"><span data-stu-id="22a73-165">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="22a73-166">Hello eszköz toomanage hello eszközeiket használják a hello identitásjegyzékhez, figyelheti az eszköz a felhőbe küldött üzeneteket, és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="22a73-166">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="22a73-167">tooinstall hello legújabb (előzetes) verzióját hello IOT hubbal-explorer eszköz, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="22a73-167">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="22a73-168">További segítséget tooget összes hello az IOT hubbal-explorer parancsot és paramétereket, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="22a73-168">tooget additional help about all hello iothub-explorer commands and their parameters, run hello following command:</span></span>

```bash
iothub-explorer help
```

### <a name="hello-azure-portal"></a><span data-ttu-id="22a73-169">hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="22a73-169">hello Azure portal</span></span>

<span data-ttu-id="22a73-170">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="22a73-170">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="22a73-171">Érdemes lehet toouse hello [Azure-portálon](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp kiépítését, kezelése és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="22a73-171">You might also want toouse hello [Azure portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="22a73-172">Az Azure tárolási problémák</span><span class="sxs-lookup"><span data-stu-id="22a73-172">Azure Storage issues</span></span>

<span data-ttu-id="22a73-173">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com/) egy különálló alkalmazás, a Microsoft használható toowork macOS, Linux és a Windows Azure Storage-adatokkal.</span><span class="sxs-lookup"><span data-stu-id="22a73-173">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="22a73-174">Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="22a73-174">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="22a73-175">Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.</span><span class="sxs-lookup"><span data-stu-id="22a73-175">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>
