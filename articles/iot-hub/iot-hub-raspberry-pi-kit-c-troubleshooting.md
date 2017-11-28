---
title: "Az IoT - Connect Raspberry pi (C) tooAzure hibaelhárítása |} Microsoft Docs"
description: "Hibaelhárítás lap málna Pi Node.js élmény"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT problémák, internet dolgot problémák"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 3653c79b-8a0c-41d4-b0bf-f6b4edb4d233
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4f1ea81dd25d10a39c2939f5ee5f19f6d2ba2b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="22cc3-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="22cc3-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="22cc3-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="22cc3-105">Hardware issues</span></span>
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a><span data-ttu-id="22cc3-106">hello alkalmazás futtatása is jól, de hello LED nem villogó.</span><span class="sxs-lookup"><span data-stu-id="22cc3-106">hello application runs well but hello LED is not blinking</span></span>
<span data-ttu-id="22cc3-107">A probléma mindig kapcsolódó toohello hardver expressroute kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="22cc3-107">This issue is always related toohello hardware circuit connectivity.</span></span> <span data-ttu-id="22cc3-108">A következő lépéseket tooidentify problémák hello használata.</span><span class="sxs-lookup"><span data-stu-id="22cc3-108">Use hello following steps tooidentify problems.</span></span>

1. <span data-ttu-id="22cc3-109">Ellenőrizze, hogy helyes-e hello választott **GPIO** a táblán.</span><span class="sxs-lookup"><span data-stu-id="22cc3-109">Check that you chose hello correct **GPIO** on your board.</span></span> <span data-ttu-id="22cc3-110">a két hello portokat kell **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**.</span><span class="sxs-lookup"><span data-stu-id="22cc3-110">hello two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="22cc3-111">Ellenőrizze, hogy helyes-e a LED hello polaritásának.</span><span class="sxs-lookup"><span data-stu-id="22cc3-111">Check that hello polarity of your LED is correct.</span></span> <span data-ttu-id="22cc3-112">hello hosszabb engedélyezi kell jeleznie hello **pozitív**, anód PIN-kódot.</span><span class="sxs-lookup"><span data-stu-id="22cc3-112">hello longer leg should indicate hello **positive**, anode pin.</span></span>
3. <span data-ttu-id="22cc3-113">Használjon hello **3.3V PIN-kód** és **GND PIN-kód** málna Pi 3.</span><span class="sxs-lookup"><span data-stu-id="22cc3-113">Use hello **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="22cc3-114">A Pi hello DC power kezelje.</span><span class="sxs-lookup"><span data-stu-id="22cc3-114">Treat Pi as hello DC power.</span></span> <span data-ttu-id="22cc3-115">Ellenőrizze, hogy hello LED remekül működik.</span><span class="sxs-lookup"><span data-stu-id="22cc3-115">Check that hello LED works fine.</span></span>

![LED meghatározása](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="22cc3-117">Más hardverekkel kapcsolatos problémák szerepelnek</span><span class="sxs-lookup"><span data-stu-id="22cc3-117">Other hardware issues</span></span>
<span data-ttu-id="22cc3-118">Málna Pi 3 gyakori problémáinak megoldásával kapcsolatban további információkért lásd: hello [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="22cc3-118">For information about solving common problems on Raspberry Pi 3, see hello [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="22cc3-119">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="22cc3-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="22cc3-120">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="22cc3-120">No response during gulp tasks</span></span>
<span data-ttu-id="22cc3-121">Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá hello `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="22cc3-121">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="22cc3-122">Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="22cc3-122">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="22cc3-123">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="22cc3-123">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="22cc3-124">Eszköz-felderítési problémák</span><span class="sxs-lookup"><span data-stu-id="22cc3-124">Device discovery issues</span></span>
<span data-ttu-id="22cc3-125">A Súgó hello gyakori problémák elhárításában `devdisco` paranccsal, ellenőrizze a hello [információs](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="22cc3-125">For help in troubleshooting common problems with hello `devdisco` command, check hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="22cc3-126">NPM problémák</span><span class="sxs-lookup"><span data-stu-id="22cc3-126">NPM issues</span></span>
<span data-ttu-id="22cc3-127">Próbálja meg tooupdate az NPM csomag hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="22cc3-127">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="22cc3-128">Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span><span class="sxs-lookup"><span data-stu-id="22cc3-128">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="22cc3-129">Távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="22cc3-129">Remote debugging</span></span>

<span data-ttu-id="22cc3-130">Távoli hibaelhárítási támogatás a Visual Studio Code C/C++ bővítmény hamarosan elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="22cc3-130">Remote debugging support will be available soon in Visual Studio Code C/C++ Extension.</span></span>

<span data-ttu-id="22cc3-131">Egy meanwhile GDB használhat a kedvenc SSH terminál keresztül:</span><span class="sxs-lookup"><span data-stu-id="22cc3-131">In a meanwhile you can use GDB via your favourite SSH terminal:</span></span>

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a><span data-ttu-id="22cc3-132">Azure-CLI problémák</span><span class="sxs-lookup"><span data-stu-id="22cc3-132">Azure-CLI issues</span></span>
<span data-ttu-id="22cc3-133">az Azure parancssori felület (CLI) hello preview build.</span><span class="sxs-lookup"><span data-stu-id="22cc3-133">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="22cc3-134">Keressen megoldást a hello [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="22cc3-134">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="22cc3-135">Amikor parancsok nem a várt módon működik, próbálja meg tooupgrade Azure-cli toolatest verziója.</span><span class="sxs-lookup"><span data-stu-id="22cc3-135">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="22cc3-136">Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="22cc3-136">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="22cc3-137">A gyakori problémák elhárításához tekintse meg hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="22cc3-137">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="22cc3-138">Ha megfelel a "Nem található olyan verzióra, amely eleget tesz a hello követelmény", adjon futtatási hello következő parancsot a tooupgrade pip toolastest verziója.</span><span class="sxs-lookup"><span data-stu-id="22cc3-138">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="22cc3-139">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="22cc3-139">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="22cc3-140">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="22cc3-140">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="22cc3-141">Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="22cc3-141">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="22cc3-142">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="22cc3-142">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="22cc3-143">Néhány **pip** a korábbi telepítés csomagok hello engedély hibát okoz a legfelső szintű hozott létre.</span><span class="sxs-lookup"><span data-stu-id="22cc3-143">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="22cc3-144">megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van.</span><span class="sxs-lookup"><span data-stu-id="22cc3-144">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="22cc3-145">Ez a feladat használható a következő lépéseket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="22cc3-145">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="22cc3-146">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="22cc3-146">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="22cc3-147">Csomagok létrehozása a legfelső szintű:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="22cc3-147">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="22cc3-148">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="22cc3-148">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="22cc3-149">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="22cc3-149">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="22cc3-150">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="22cc3-150">Azure IoT Hub issues</span></span>
<span data-ttu-id="22cc3-151">Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és egy eszköz toomanage hello eszközök tooyour IoT-központ, a következő eszközök próbálja hello csatlakozó van szüksége:</span><span class="sxs-lookup"><span data-stu-id="22cc3-151">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="22cc3-152">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="22cc3-152">Device Explorer</span></span>
<span data-ttu-id="22cc3-153">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT-központ tooyour csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="22cc3-153">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="22cc3-154">Hello következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="22cc3-154">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="22cc3-155">*Eszköz Identitáskezelés* tooprovision és az IoT hub regisztrált eszközök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="22cc3-155">*Device identity management* tooprovision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="22cc3-156">*Eszköz-felhő kap* , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="22cc3-156">*Receive device-to-cloud* so you can monitor messages sent from your device tooyour IoT hub.</span></span>
* <span data-ttu-id="22cc3-157">*Felhő eszközre küldése* úgy küldhet üzeneteket tooyour eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="22cc3-157">*Send cloud-to-device* so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="22cc3-158">Konfigurálja a `IoT hub connection string` belül az eszköz toouse annak képességeit.</span><span class="sxs-lookup"><span data-stu-id="22cc3-158">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="22cc3-159">Az IoT-központ Explorer</span><span class="sxs-lookup"><span data-stu-id="22cc3-159">IoT hub Explorer</span></span>
<span data-ttu-id="22cc3-160">[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközügyfeleitől.</span><span class="sxs-lookup"><span data-stu-id="22cc3-160">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="22cc3-161">Hello eszköz toomanage hello eszközeiket használják a hello identitásjegyzékhez, figyelheti az eszköz a felhőbe küldött üzeneteket, és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="22cc3-161">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="22cc3-162">tooinstall hello hello IOT hubbal-explorer eszköz, futtassa a következő parancsot a parancssori környezetben hello legújabb (előzetes) verzióját:</span><span class="sxs-lookup"><span data-stu-id="22cc3-162">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```
npm install -g iothub-explorer@latest
```

<span data-ttu-id="22cc3-163">A következő parancs tooget további súgó összes hello IOT hubbal-explorer parancsot és paramétereket hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="22cc3-163">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="22cc3-164">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="22cc3-164">Azure portal</span></span>
<span data-ttu-id="22cc3-165">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="22cc3-165">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="22cc3-166">Érdemes lehet toouse hello [Azure-portálon](../azure-portal-overview.md) toohelp kiépítését, kezelése és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="22cc3-166">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="22cc3-167">Az Azure storage-problémák</span><span class="sxs-lookup"><span data-stu-id="22cc3-167">Azure storage issues</span></span>
<span data-ttu-id="22cc3-168">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft használható toowork macOS, Linux és a Windows Azure Storage-adatokkal.</span><span class="sxs-lookup"><span data-stu-id="22cc3-168">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="22cc3-169">Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="22cc3-169">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="22cc3-170">Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.</span><span class="sxs-lookup"><span data-stu-id="22cc3-170">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>
