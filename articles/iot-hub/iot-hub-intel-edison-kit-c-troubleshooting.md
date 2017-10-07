---
title: "Az IoT - Connect Intel Edison (C) tooAzure hibaelhárítása |} Microsoft Docs"
description: "Hibaelhárítás lap Intel Edison C élmény"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino hibaelhárítása"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: fe20f2fe-490c-4910-82e1-578ed504ae86
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c4a71983e3906cfc5ce7c832cf858852b9bd744a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="b2a92-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="b2a92-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="b2a92-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="b2a92-105">Hardware issues</span></span>
<span data-ttu-id="b2a92-106">Intel Edison gyakori problémáinak megoldásával kapcsolatban további információkért lásd: hello [hivatalos hibaelhárítási lap](https://software.intel.com/en-us/node/637974).</span><span class="sxs-lookup"><span data-stu-id="b2a92-106">For information about solving common problems on Intel Edison, see hello [official troubleshooting page](https://software.intel.com/en-us/node/637974).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="b2a92-107">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="b2a92-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="b2a92-108">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="b2a92-108">No response during gulp tasks</span></span>
<span data-ttu-id="b2a92-109">Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá hello `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b2a92-109">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="b2a92-110">Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="b2a92-110">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="b2a92-111">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="b2a92-111">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="npm-issues"></a><span data-ttu-id="b2a92-112">NPM problémák</span><span class="sxs-lookup"><span data-stu-id="b2a92-112">NPM issues</span></span>
<span data-ttu-id="b2a92-113">Próbálja meg tooupdate az NPM csomag hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b2a92-113">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="b2a92-114">Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="b2a92-114">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="b2a92-115">Azure-CLI problémák</span><span class="sxs-lookup"><span data-stu-id="b2a92-115">Azure-CLI issues</span></span>
<span data-ttu-id="b2a92-116">az Azure parancssori felület (CLI) hello preview build.</span><span class="sxs-lookup"><span data-stu-id="b2a92-116">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="b2a92-117">Keressen megoldást a hello [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="b2a92-117">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="b2a92-118">Amikor parancsok nem a várt módon működik, próbálja meg tooupgrade Azure-cli toolatest verziója.</span><span class="sxs-lookup"><span data-stu-id="b2a92-118">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="b2a92-119">Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="b2a92-119">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="b2a92-120">A gyakori problémák elhárításához tekintse meg hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="b2a92-120">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="b2a92-121">Ha megfelel a "Nem található olyan verzióra, amely eleget tesz a hello követelmény", adjon futtatási hello következő parancsot a tooupgrade pip toolastest verziója.</span><span class="sxs-lookup"><span data-stu-id="b2a92-121">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="b2a92-122">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="b2a92-122">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="b2a92-123">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="b2a92-123">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="b2a92-124">Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="b2a92-124">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="b2a92-125">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="b2a92-125">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="b2a92-126">Néhány **pip** a korábbi telepítés csomagok hello engedély hibát okoz a legfelső szintű hozott létre.</span><span class="sxs-lookup"><span data-stu-id="b2a92-126">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="b2a92-127">megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b2a92-127">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="b2a92-128">Ez a feladat használható a következő lépéseket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="b2a92-128">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="b2a92-129">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="b2a92-129">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="b2a92-130">Csomagok létrehozása a legfelső szintű:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="b2a92-130">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="b2a92-131">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="b2a92-131">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="b2a92-132">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="b2a92-132">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="b2a92-133">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="b2a92-133">Azure IoT Hub issues</span></span>
<span data-ttu-id="b2a92-134">Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és egy eszköz toomanage hello eszközök tooyour IoT-központ, a következő eszközök próbálja hello csatlakozó van szüksége:</span><span class="sxs-lookup"><span data-stu-id="b2a92-134">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="b2a92-135">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="b2a92-135">Device Explorer</span></span>
<span data-ttu-id="b2a92-136">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT-központ tooyour csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b2a92-136">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="b2a92-137">Hello következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="b2a92-137">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

- <span data-ttu-id="b2a92-138">_Eszköz Identitáskezelés_ tooprovision és az IoT hub regisztrált eszközök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b2a92-138">_Device identity management_ tooprovision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="b2a92-139">_Eszköz-felhő kap_ , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="b2a92-139">_Receive device-to-cloud_ so you can monitor messages sent from your device tooyour IoT hub.</span></span>
- <span data-ttu-id="b2a92-140">_Felhő eszközre küldése_ úgy küldhet üzeneteket tooyour eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="b2a92-140">_Send cloud-to-device_ so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="b2a92-141">Konfigurálja a `IoT hub connection string` belül az eszköz toouse annak képességeit.</span><span class="sxs-lookup"><span data-stu-id="b2a92-141">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="b2a92-142">Az IoT-központ Explorer</span><span class="sxs-lookup"><span data-stu-id="b2a92-142">IoT hub Explorer</span></span>
<span data-ttu-id="b2a92-143">[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközügyfeleitől.</span><span class="sxs-lookup"><span data-stu-id="b2a92-143">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="b2a92-144">Hello eszköz toomanage hello eszközeiket használják a hello identitásjegyzékhez, figyelheti az eszköz a felhőbe küldött üzeneteket, és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="b2a92-144">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="b2a92-145">tooinstall hello hello IOT hubbal-explorer eszköz, futtassa a következő parancsot a parancssori környezetben hello legújabb (előzetes) verzióját:</span><span class="sxs-lookup"><span data-stu-id="b2a92-145">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="b2a92-146">A következő parancs tooget további súgó összes hello IOT hubbal-explorer parancsot és paramétereket hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="b2a92-146">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="b2a92-147">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b2a92-147">Azure portal</span></span>
<span data-ttu-id="b2a92-148">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b2a92-148">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="b2a92-149">Érdemes lehet toouse hello [Azure-portálon](../azure-portal-overview.md) toohelp kiépítését, kezelése és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b2a92-149">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="b2a92-150">Az Azure storage-problémák</span><span class="sxs-lookup"><span data-stu-id="b2a92-150">Azure storage issues</span></span>
<span data-ttu-id="b2a92-151">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, melyekkel a toowork Microsoft [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) Windows, a macOS és a Linux adatok.</span><span class="sxs-lookup"><span data-stu-id="b2a92-151">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="b2a92-152">Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b2a92-152">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="b2a92-153">Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.</span><span class="sxs-lookup"><span data-stu-id="b2a92-153">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2a92-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b2a92-154">Next steps</span></span>
<span data-ttu-id="b2a92-155">Ezen a lapon csak a leggyakoribb problémák hello Intel Edison Kit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b2a92-155">This page only includes hello most common problems of Intel Edison kit.</span></span> <span data-ttu-id="b2a92-156">Alsó megjegyzések tooreport problémákkal kapcsolatos további információkat is hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="b2a92-156">You can also leave bottom comments tooreport issues for further troubleshooting.</span></span>

<span data-ttu-id="b2a92-157">Lépjen vissza túl[Intel Edison (C) az első lépései](iot-hub-intel-edison-kit-c-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="b2a92-157">Go back too[Get started with Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-c-edison-getting-started