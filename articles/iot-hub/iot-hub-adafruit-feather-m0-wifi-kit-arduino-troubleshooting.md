---
title: "Az IoT - Connect Arduino (C) tooAzure hibaelhárítása |} Microsoft Docs"
description: "Hibaelhárítás lap Adafruit lágyított M0 Wi-Fi Arduino élmény"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino hibaelhárítása"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: fdcc56ff-4420-463c-8a0e-5a1d215a874f
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ed793041ffa1887dbe73067f7c48d2417e982866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="13661-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="13661-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="13661-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="13661-105">Hardware issues</span></span>
<span data-ttu-id="13661-106">A Adafruit lágyított M0 Wi-Fi Arduino táblán gyakori problémák megoldásával kapcsolatos további információkért lásd: hello [hivatalos hibaelhárítási lap](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).</span><span class="sxs-lookup"><span data-stu-id="13661-106">For information about solving common problems on your Adafruit Feather M0 WiFi Arduino board, see hello [official troubleshooting page](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="13661-107">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="13661-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="13661-108">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="13661-108">No response during gulp tasks</span></span>
<span data-ttu-id="13661-109">Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá hello `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="13661-109">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="13661-110">Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="13661-110">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="13661-111">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="13661-111">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

<span data-ttu-id="13661-112">Vagy felveheti `--listen` tooopen soros port toooutput eszköz szereplő információkat.</span><span class="sxs-lookup"><span data-stu-id="13661-112">Or you can add `--listen` tooopen serial port toooutput device log information.</span></span>

```bash
gulp --listen
``` 

### <a name="npm-issues"></a><span data-ttu-id="13661-113">NPM problémák</span><span class="sxs-lookup"><span data-stu-id="13661-113">NPM issues</span></span>
<span data-ttu-id="13661-114">Próbálja meg tooupdate az NPM csomag hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="13661-114">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="13661-115">Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="13661-115">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="13661-116">Azure-CLI problémák</span><span class="sxs-lookup"><span data-stu-id="13661-116">Azure-CLI issues</span></span>
<span data-ttu-id="13661-117">az Azure parancssori felület (CLI) hello preview build.</span><span class="sxs-lookup"><span data-stu-id="13661-117">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="13661-118">Keressen megoldást a hello [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="13661-118">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="13661-119">Amikor parancsok nem a várt módon működik, próbálja meg tooupgrade Azure-cli toolatest verziója.</span><span class="sxs-lookup"><span data-stu-id="13661-119">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="13661-120">Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="13661-120">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="13661-121">A gyakori problémák elhárításához tekintse meg hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="13661-121">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="13661-122">Ha megfelel a "Nem található olyan verzióra, amely eleget tesz a hello követelmény", adjon futtatási hello következő parancsot a tooupgrade pip toolastest verziója.</span><span class="sxs-lookup"><span data-stu-id="13661-122">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="13661-123">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="13661-123">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="13661-124">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="13661-124">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="13661-125">Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="13661-125">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="13661-126">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="13661-126">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="13661-127">Néhány **pip** a korábbi telepítés csomagok hello engedély hibát okoz a legfelső szintű hozott létre.</span><span class="sxs-lookup"><span data-stu-id="13661-127">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="13661-128">megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van.</span><span class="sxs-lookup"><span data-stu-id="13661-128">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="13661-129">Ez a feladat használható a következő lépéseket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="13661-129">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="13661-130">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="13661-130">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="13661-131">Csomagok létrehozása a legfelső szintű:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="13661-131">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="13661-132">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="13661-132">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="13661-133">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="13661-133">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="13661-134">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="13661-134">Azure IoT Hub issues</span></span>
<span data-ttu-id="13661-135">Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és egy eszköz toomanage hello eszközök tooyour IoT-központ, a következő eszközök próbálja hello csatlakozó van szüksége:</span><span class="sxs-lookup"><span data-stu-id="13661-135">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="13661-136">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="13661-136">Device Explorer</span></span>
<span data-ttu-id="13661-137">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT-központ tooyour csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="13661-137">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="13661-138">Hello következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="13661-138">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="13661-139">*Eszköz Identitáskezelés* tooprovision és az IoT hub regisztrált eszközök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="13661-139">*Device identity management* tooprovision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="13661-140">*Eszköz-felhő kap* , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="13661-140">*Receive device-to-cloud* so you can monitor messages sent from your device tooyour IoT hub.</span></span>
* <span data-ttu-id="13661-141">*Felhő eszközre küldése* úgy küldhet üzeneteket tooyour eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="13661-141">*Send cloud-to-device* so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="13661-142">Konfigurálja a `IoT hub connection string` belül az eszköz toouse annak képességeit.</span><span class="sxs-lookup"><span data-stu-id="13661-142">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="13661-143">Az IoT-központ Explorer</span><span class="sxs-lookup"><span data-stu-id="13661-143">IoT hub Explorer</span></span>
<span data-ttu-id="13661-144">[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközügyfeleitől.</span><span class="sxs-lookup"><span data-stu-id="13661-144">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="13661-145">Hello eszköz toomanage hello eszközeiket használják a hello identitásjegyzékhez, figyelheti az eszköz a felhőbe küldött üzeneteket, és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="13661-145">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>


<span data-ttu-id="13661-146">tooinstall hello hello IOT hubbal-explorer eszköz, futtassa a következő parancsot a parancssori környezetben hello legújabb (előzetes) verzióját:</span><span class="sxs-lookup"><span data-stu-id="13661-146">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="13661-147">A következő parancs tooget további súgó összes hello IOT hubbal-explorer parancsot és paramétereket hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="13661-147">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="13661-148">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="13661-148">Azure portal</span></span>
<span data-ttu-id="13661-149">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="13661-149">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="13661-150">Érdemes lehet toouse hello [Azure-portálon](../azure-portal-overview.md) toohelp kiépítését, kezelése és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="13661-150">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="13661-151">Az Azure storage-problémák</span><span class="sxs-lookup"><span data-stu-id="13661-151">Azure storage issues</span></span>
<span data-ttu-id="13661-152">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft használható toowork macOS, Linux és a Windows Azure Storage-adatokkal.</span><span class="sxs-lookup"><span data-stu-id="13661-152">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="13661-153">Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="13661-153">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="13661-154">Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.</span><span class="sxs-lookup"><span data-stu-id="13661-154">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md