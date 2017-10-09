---
title: "Csatlakozás Intel Edison (csomópont) tooAzure IoT - lecke 4: hibaelhárítása |} Microsoft Docs"
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
ms.openlocfilehash: 9502300f7f372255572b49bea45dd3e1fdaeeb37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="bfc72-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="bfc72-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="bfc72-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="bfc72-105">Hardware issues</span></span>
<span data-ttu-id="bfc72-106">Intel Edison gyakori problémáinak megoldásával kapcsolatban további információkért lásd: hello [hivatalos hibaelhárítási lap](https://software.intel.com/en-us/node/637974).</span><span class="sxs-lookup"><span data-stu-id="bfc72-106">For information about solving common problems on Intel Edison, see hello [official troubleshooting page](https://software.intel.com/en-us/node/637974).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="bfc72-107">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="bfc72-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="bfc72-108">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="bfc72-108">No response during gulp tasks</span></span>
<span data-ttu-id="bfc72-109">Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá hello `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bfc72-109">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="bfc72-110">Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="bfc72-110">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="bfc72-111">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="bfc72-111">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="npm-issues"></a><span data-ttu-id="bfc72-112">NPM problémák</span><span class="sxs-lookup"><span data-stu-id="bfc72-112">NPM issues</span></span>
<span data-ttu-id="bfc72-113">Próbálja meg tooupdate az NPM csomag hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bfc72-113">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="bfc72-114">Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="bfc72-114">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="bfc72-115">Távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="bfc72-115">Remote debugging</span></span>

### <a name="run-hello-sample-application-in-debug-mode"></a><span data-ttu-id="bfc72-116">Hibakeresési módban hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="bfc72-116">Run hello sample application in debug mode</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="bfc72-117">Ha hello hibakeresési motor készen áll, meg kell tudni toosee ```Debugger listening on port 5858``` hello konzol kimenetből.</span><span class="sxs-lookup"><span data-stu-id="bfc72-117">Once hello debug engine is ready, you should be able toosee ```Debugger listening on port 5858``` from hello console output.</span></span>

### <a name="configure-vs-code-tooconnect-toohello-remote-device"></a><span data-ttu-id="bfc72-118">Visual STUDIO Code tooconnect toohello távoli eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bfc72-118">Configure VS Code tooconnect toohello remote device</span></span>

<span data-ttu-id="bfc72-119">Nyissa meg hello **Debug** a hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="bfc72-119">Open hello **Debug** panel from hello left side.</span></span>

<span data-ttu-id="bfc72-120">Kattintson a zöld hello **Start Debugging** (F5) gombra.</span><span class="sxs-lookup"><span data-stu-id="bfc72-120">Click hello green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="bfc72-121">Visual STUDIO Code nyitna a **launch.json** fájlt, amely tooupdate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="bfc72-121">VS Code would open a **launch.json** file, which you need tooupdate.</span></span>

<span data-ttu-id="bfc72-122">Frissítés hello **launch.json** tartalom a következő hello fájlt, hogy lecseréli `[device hostname or IP address]` hello tényleges eszköz IP-címet vagy állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="bfc72-122">Update hello **launch.json** file with hello following content, replace `[device hostname or IP address]` with hello actual device IP address or hostname.</span></span>  

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

### <a name="attach-toohello-remote-application"></a><span data-ttu-id="bfc72-124">Toohello távoli alkalmazás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="bfc72-124">Attach toohello remote application</span></span>

<span data-ttu-id="bfc72-125">Kattintson a zöld hello **Start Debugging** (F5) gombra, és kihasználhatják a hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="bfc72-125">Click hello green **Start Debugging** (F5) button and enjoy debugging.</span></span>

<span data-ttu-id="bfc72-126">Elolvashatja [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn hello hibakereső többet.</span><span class="sxs-lookup"><span data-stu-id="bfc72-126">You can read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn more about hello debugger.</span></span>

![Távoli, interaktív hibakeresés](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="bfc72-128">Azure-CLI problémák</span><span class="sxs-lookup"><span data-stu-id="bfc72-128">Azure-CLI issues</span></span>
<span data-ttu-id="bfc72-129">az Azure parancssori felület (CLI) hello preview build.</span><span class="sxs-lookup"><span data-stu-id="bfc72-129">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="bfc72-130">Keressen megoldást a hello [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="bfc72-130">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="bfc72-131">Amikor parancsok nem a várt módon működik, próbálja meg tooupgrade Azure-cli toolatest verziója.</span><span class="sxs-lookup"><span data-stu-id="bfc72-131">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="bfc72-132">Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="bfc72-132">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="bfc72-133">A gyakori problémák elhárításához tekintse meg hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="bfc72-133">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="bfc72-134">Ha megfelel a "Nem található olyan verzióra, amely eleget tesz a hello követelmény", adjon futtatási hello következő parancsot a tooupgrade pip toolastest verziója.</span><span class="sxs-lookup"><span data-stu-id="bfc72-134">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="bfc72-135">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="bfc72-135">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="bfc72-136">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="bfc72-136">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="bfc72-137">Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="bfc72-137">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="bfc72-138">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="bfc72-138">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="bfc72-139">Néhány **pip** a korábbi telepítés csomagok hello engedély hibát okoz a legfelső szintű hozott létre.</span><span class="sxs-lookup"><span data-stu-id="bfc72-139">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="bfc72-140">megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van.</span><span class="sxs-lookup"><span data-stu-id="bfc72-140">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="bfc72-141">Ez a feladat használható a következő lépéseket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="bfc72-141">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="bfc72-142">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="bfc72-142">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="bfc72-143">Csomagok létrehozása a legfelső szintű:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="bfc72-143">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="bfc72-144">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="bfc72-144">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="bfc72-145">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="bfc72-145">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="bfc72-146">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="bfc72-146">Azure IoT Hub issues</span></span>
<span data-ttu-id="bfc72-147">Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és egy eszköz toomanage hello eszközök tooyour IoT-központ, a következő eszközök próbálja hello csatlakozó van szüksége:</span><span class="sxs-lookup"><span data-stu-id="bfc72-147">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="bfc72-148">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="bfc72-148">Device Explorer</span></span>
<span data-ttu-id="bfc72-149">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT-központ tooyour csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="bfc72-149">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="bfc72-150">Hello következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="bfc72-150">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

- <span data-ttu-id="bfc72-151">_Eszköz Identitáskezelés_ tooprovision és az IoT hub regisztrált eszközök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="bfc72-151">_Device identity management_ tooprovision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="bfc72-152">_Eszköz-felhő kap_ , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="bfc72-152">_Receive device-to-cloud_ so you can monitor messages sent from your device tooyour IoT hub.</span></span>
- <span data-ttu-id="bfc72-153">_Felhő eszközre küldése_ úgy küldhet üzeneteket tooyour eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="bfc72-153">_Send cloud-to-device_ so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="bfc72-154">Konfigurálja a `IoT hub connection string` belül az eszköz toouse annak képességeit.</span><span class="sxs-lookup"><span data-stu-id="bfc72-154">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="bfc72-155">Az IoT-központ Explorer</span><span class="sxs-lookup"><span data-stu-id="bfc72-155">IoT hub Explorer</span></span>
<span data-ttu-id="bfc72-156">[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközügyfeleitől.</span><span class="sxs-lookup"><span data-stu-id="bfc72-156">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="bfc72-157">Hello eszköz toomanage hello eszközeiket használják a hello identitásjegyzékhez, figyelheti az eszköz a felhőbe küldött üzeneteket, és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="bfc72-157">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="bfc72-158">tooinstall hello hello IOT hubbal-explorer eszköz, futtassa a következő parancsot a parancssori környezetben hello legújabb (előzetes) verzióját:</span><span class="sxs-lookup"><span data-stu-id="bfc72-158">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="bfc72-159">A következő parancs tooget további súgó összes hello IOT hubbal-explorer parancsot és paramétereket hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="bfc72-159">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="bfc72-160">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bfc72-160">Azure portal</span></span>
<span data-ttu-id="bfc72-161">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="bfc72-161">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="bfc72-162">Érdemes lehet toouse hello [Azure-portálon](../azure-portal-overview.md) toohelp kiépítését, kezelése és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="bfc72-162">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="bfc72-163">Az Azure storage-problémák</span><span class="sxs-lookup"><span data-stu-id="bfc72-163">Azure storage issues</span></span>
<span data-ttu-id="bfc72-164">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, melyekkel a toowork Microsoft [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) Windows, a macOS és a Linux adatok.</span><span class="sxs-lookup"><span data-stu-id="bfc72-164">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="bfc72-165">Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="bfc72-165">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="bfc72-166">Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.</span><span class="sxs-lookup"><span data-stu-id="bfc72-166">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bfc72-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bfc72-167">Next steps</span></span>
<span data-ttu-id="bfc72-168">Ezen a lapon csak a leggyakoribb problémák hello Intel Edison Kit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bfc72-168">This page only includes hello most common problems of Intel Edison kit.</span></span> <span data-ttu-id="bfc72-169">Alsó megjegyzések tooreport problémákkal kapcsolatos további információkat is hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="bfc72-169">You can also leave bottom comments tooreport issues for further troubleshooting.</span></span>

<span data-ttu-id="bfc72-170">Lépjen vissza túl[Intel Edison (Node.js) az első lépései](iot-hub-intel-edison-kit-node-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="bfc72-170">Go back too[Get started with Intel Edison (Node.js)](iot-hub-intel-edison-kit-node-get-started.md)</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-node-edison-getting-started