---
title: "Intel Edison (C) csatlakozni az Azure IoT - hibaelhárítása |} Microsoft Docs"
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
ms.openlocfilehash: dd6338ad29e0bb858c33e5bb24b8f41d3c22575a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="e5fef-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="e5fef-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="e5fef-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="e5fef-105">Hardware issues</span></span>
<span data-ttu-id="e5fef-106">Intel Edison gyakori problémáinak megoldásával kapcsolatban további információkért lásd: a [hivatalos hibaelhárítási lap](https://software.intel.com/en-us/node/637974).</span><span class="sxs-lookup"><span data-stu-id="e5fef-106">For information about solving common problems on Intel Edison, see the [official troubleshooting page](https://software.intel.com/en-us/node/637974).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="e5fef-107">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="e5fef-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="e5fef-108">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="e5fef-108">No response during gulp tasks</span></span>
<span data-ttu-id="e5fef-109">Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá a `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e5fef-109">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="e5fef-110">Állítsa le a jelenlegi gulp feladatok használatával próbál `Ctrl + C`, és futtassa a következő parancsot a konzolablakban tekintse meg a hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="e5fef-110">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="e5fef-111">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="e5fef-111">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="npm-issues"></a><span data-ttu-id="e5fef-112">NPM problémák</span><span class="sxs-lookup"><span data-stu-id="e5fef-112">NPM issues</span></span>
<span data-ttu-id="e5fef-113">Próbálja meg frissíteni az NPM-csomagot a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e5fef-113">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="e5fef-114">Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="e5fef-114">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="e5fef-115">Azure-CLI problémák</span><span class="sxs-lookup"><span data-stu-id="e5fef-115">Azure-CLI issues</span></span>
<span data-ttu-id="e5fef-116">Az Azure parancssori felület (CLI) preview build.</span><span class="sxs-lookup"><span data-stu-id="e5fef-116">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="e5fef-117">Keresse meg a megoldást a [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) megoldások keresése.</span><span class="sxs-lookup"><span data-stu-id="e5fef-117">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="e5fef-118">Próbálja meg az Azure-cli parancsok nem a várt módon működik legújabb verziójára történő frissítés.</span><span class="sxs-lookup"><span data-stu-id="e5fef-118">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="e5fef-119">Ha az eszköz hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a a **problémák** a GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="e5fef-119">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="e5fef-120">A gyakori problémák elhárításához tekintse meg a [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="e5fef-120">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="e5fef-121">Ha megfelel a "Egy olyan verzióra, amely eleget tesz a követelmény nem található", futtassa a következő parancsot a pip frissítése a legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="e5fef-121">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="e5fef-122">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="e5fef-122">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="e5fef-123">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="e5fef-123">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="e5fef-124">Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="e5fef-124">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="e5fef-125">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="e5fef-125">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="e5fef-126">Néhány **pip** a korábbi telepítés csomagok létrehozásakor a gyökérszintű, amely azt eredményezi, az engedély hiba.</span><span class="sxs-lookup"><span data-stu-id="e5fef-126">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="e5fef-127">A megoldás, hogy távolítsa el azokat a legfelső szintű telepített csomagokat.</span><span class="sxs-lookup"><span data-stu-id="e5fef-127">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="e5fef-128">Ez a feladat befejezéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e5fef-128">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="e5fef-129">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="e5fef-129">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="e5fef-130">Csomagok létrehozása a legfelső szintű:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="e5fef-130">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="e5fef-131">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="e5fef-131">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="e5fef-132">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="e5fef-132">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="e5fef-133">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="e5fef-133">Azure IoT Hub issues</span></span>
<span data-ttu-id="e5fef-134">Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és meg kell-e olyan eszköz, amely az IoT hub csatlakozó eszközök kezelésére, és próbálkozzon az alábbi eszközöket:</span><span class="sxs-lookup"><span data-stu-id="e5fef-134">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="e5fef-135">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="e5fef-135">Device Explorer</span></span>
<span data-ttu-id="e5fef-136">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT hub csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="e5fef-136">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="e5fef-137">A következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="e5fef-137">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

- <span data-ttu-id="e5fef-138">_Eszköz Identitáskezelés_ szeretnék telepíteni és kezelni az IoT hub regisztrált eszközökre.</span><span class="sxs-lookup"><span data-stu-id="e5fef-138">_Device identity management_ to provision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="e5fef-139">_Eszköz-felhő kap_ , figyelheti az IoT hub az eszközről küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="e5fef-139">_Receive device-to-cloud_ so you can monitor messages sent from your device to your IoT hub.</span></span>
- <span data-ttu-id="e5fef-140">_Felhő eszközre küldése_ úgy küldhet üzeneteket az eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="e5fef-140">_Send cloud-to-device_ so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="e5fef-141">Konfigurálja a `IoT hub connection string` ebből az eszközből készülék képességeinek használatához.</span><span class="sxs-lookup"><span data-stu-id="e5fef-141">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="e5fef-142">Az IoT-központ Explorer</span><span class="sxs-lookup"><span data-stu-id="e5fef-142">IoT hub Explorer</span></span>
<span data-ttu-id="e5fef-143">[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközügyfeleken kezelésére.</span><span class="sxs-lookup"><span data-stu-id="e5fef-143">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="e5fef-144">Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="e5fef-144">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="e5fef-145">Telepítse a legújabb (előzetes) verzióját az IOT hubbal-explorer eszköz, a következő parancsot a parancssori környezetben:</span><span class="sxs-lookup"><span data-stu-id="e5fef-145">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="e5fef-146">A következő paranccsal minden IOT hubbal-explorer parancsot és paramétereket kapcsolatos további segítség:</span><span class="sxs-lookup"><span data-stu-id="e5fef-146">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="e5fef-147">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e5fef-147">Azure portal</span></span>
<span data-ttu-id="e5fef-148">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="e5fef-148">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="e5fef-149">Érdemes lehet használni a [Azure-portálon](../azure-portal-overview.md) kiépítéséhez, kezeléséhez, és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="e5fef-149">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="e5fef-150">Az Azure storage-problémák</span><span class="sxs-lookup"><span data-stu-id="e5fef-150">Azure storage issues</span></span>
<span data-ttu-id="e5fef-151">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, amelyekkel dolgozni Microsoft [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) Windows, a macOS és a Linux adatok.</span><span class="sxs-lookup"><span data-stu-id="e5fef-151">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="e5fef-152">Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e5fef-152">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="e5fef-153">Az eszköz segítségével az Azure Storage problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="e5fef-153">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5fef-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5fef-154">Next steps</span></span>
<span data-ttu-id="e5fef-155">Ezen a lapon csak Intel Edison Kit leggyakoribb problémákat foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="e5fef-155">This page only includes the most common problems of Intel Edison kit.</span></span> <span data-ttu-id="e5fef-156">Alsó megjegyzések további hibaelhárítási jelentés problémákat is hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="e5fef-156">You can also leave bottom comments to report issues for further troubleshooting.</span></span>

<span data-ttu-id="e5fef-157">Lépjen vissza a [Intel Edison (C) az első lépései](iot-hub-intel-edison-kit-c-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="e5fef-157">Go back to [Get started with Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-c-edison-getting-started