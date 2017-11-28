---
title: "Arduino (C) csatlakozzon az Azure IoT - hárítsa el a |} Microsoft Docs"
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
ms.openlocfilehash: 3bb369da9148300a80e7d351522a4d908e926996
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="efc5d-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="efc5d-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="efc5d-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="efc5d-105">Hardware issues</span></span>
<span data-ttu-id="efc5d-106">A Adafruit lágyított M0 Wi-Fi Arduino táblán gyakori problémák megoldásával kapcsolatos további információkért lásd: a [hivatalos hibaelhárítási lap](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).</span><span class="sxs-lookup"><span data-stu-id="efc5d-106">For information about solving common problems on your Adafruit Feather M0 WiFi Arduino board, see the [official troubleshooting page](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="efc5d-107">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="efc5d-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="efc5d-108">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="efc5d-108">No response during gulp tasks</span></span>
<span data-ttu-id="efc5d-109">Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá a `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="efc5d-109">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="efc5d-110">Állítsa le a jelenlegi gulp feladatok használatával próbál `Ctrl + C`, és futtassa a következő parancsot a konzolablakban tekintse meg a hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="efc5d-110">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="efc5d-111">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="efc5d-111">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

<span data-ttu-id="efc5d-112">Vagy felveheti `--listen` kimeneti eszköz naplóadatok a soros port megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="efc5d-112">Or you can add `--listen` to open serial port to output device log information.</span></span>

```bash
gulp --listen
``` 

### <a name="npm-issues"></a><span data-ttu-id="efc5d-113">NPM problémák</span><span class="sxs-lookup"><span data-stu-id="efc5d-113">NPM issues</span></span>
<span data-ttu-id="efc5d-114">Próbálja meg frissíteni az NPM-csomagot a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="efc5d-114">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="efc5d-115">Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="efc5d-115">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="efc5d-116">Azure-CLI problémák</span><span class="sxs-lookup"><span data-stu-id="efc5d-116">Azure-CLI issues</span></span>
<span data-ttu-id="efc5d-117">Az Azure parancssori felület (CLI) preview build.</span><span class="sxs-lookup"><span data-stu-id="efc5d-117">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="efc5d-118">Keresse meg a megoldást a [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) megoldások keresése.</span><span class="sxs-lookup"><span data-stu-id="efc5d-118">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="efc5d-119">Próbálja meg az Azure-cli parancsok nem a várt módon működik legújabb verziójára történő frissítés.</span><span class="sxs-lookup"><span data-stu-id="efc5d-119">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="efc5d-120">Ha az eszköz hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a a **problémák** a GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="efc5d-120">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="efc5d-121">A gyakori problémák elhárításához tekintse meg a [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="efc5d-121">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="efc5d-122">Ha megfelel a "Egy olyan verzióra, amely eleget tesz a követelmény nem található", futtassa a következő parancsot a pip frissítése a legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="efc5d-122">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="efc5d-123">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="efc5d-123">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="efc5d-124">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="efc5d-124">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="efc5d-125">Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="efc5d-125">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="efc5d-126">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="efc5d-126">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="efc5d-127">Néhány **pip** a korábbi telepítés csomagok létrehozásakor a gyökérszintű, amely azt eredményezi, az engedély hiba.</span><span class="sxs-lookup"><span data-stu-id="efc5d-127">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="efc5d-128">A megoldás, hogy távolítsa el azokat a legfelső szintű telepített csomagokat.</span><span class="sxs-lookup"><span data-stu-id="efc5d-128">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="efc5d-129">Ez a feladat befejezéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="efc5d-129">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="efc5d-130">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="efc5d-130">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="efc5d-131">Csomagok létrehozása a legfelső szintű:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="efc5d-131">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="efc5d-132">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="efc5d-132">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="efc5d-133">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="efc5d-133">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="efc5d-134">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="efc5d-134">Azure IoT Hub issues</span></span>
<span data-ttu-id="efc5d-135">Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és meg kell-e olyan eszköz, amely az IoT hub csatlakozó eszközök kezelésére, és próbálkozzon az alábbi eszközöket:</span><span class="sxs-lookup"><span data-stu-id="efc5d-135">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="efc5d-136">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="efc5d-136">Device Explorer</span></span>
<span data-ttu-id="efc5d-137">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT hub csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="efc5d-137">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="efc5d-138">A következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="efc5d-138">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="efc5d-139">*Eszköz Identitáskezelés* szeretnék telepíteni és kezelni az IoT hub regisztrált eszközökre.</span><span class="sxs-lookup"><span data-stu-id="efc5d-139">*Device identity management* to provision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="efc5d-140">*Eszköz-felhő kap* , figyelheti az IoT hub az eszközről küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="efc5d-140">*Receive device-to-cloud* so you can monitor messages sent from your device to your IoT hub.</span></span>
* <span data-ttu-id="efc5d-141">*Felhő eszközre küldése* úgy küldhet üzeneteket az eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="efc5d-141">*Send cloud-to-device* so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="efc5d-142">Konfigurálja a `IoT hub connection string` ebből az eszközből készülék képességeinek használatához.</span><span class="sxs-lookup"><span data-stu-id="efc5d-142">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="efc5d-143">Az IoT-központ Explorer</span><span class="sxs-lookup"><span data-stu-id="efc5d-143">IoT hub Explorer</span></span>
<span data-ttu-id="efc5d-144">[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközügyfeleken kezelésére.</span><span class="sxs-lookup"><span data-stu-id="efc5d-144">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="efc5d-145">Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="efc5d-145">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>


<span data-ttu-id="efc5d-146">Telepítse a legújabb (előzetes) verzióját az IOT hubbal-explorer eszköz, a következő parancsot a parancssori környezetben:</span><span class="sxs-lookup"><span data-stu-id="efc5d-146">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="efc5d-147">A következő paranccsal minden IOT hubbal-explorer parancsot és paramétereket kapcsolatos további segítség:</span><span class="sxs-lookup"><span data-stu-id="efc5d-147">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="efc5d-148">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="efc5d-148">Azure portal</span></span>
<span data-ttu-id="efc5d-149">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="efc5d-149">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="efc5d-150">Érdemes lehet használni a [Azure-portálon](../azure-portal-overview.md) kiépítéséhez, kezeléséhez, és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="efc5d-150">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="efc5d-151">Az Azure storage-problémák</span><span class="sxs-lookup"><span data-stu-id="efc5d-151">Azure storage issues</span></span>
<span data-ttu-id="efc5d-152">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft, amelyek segítségével a Windows, a macOS és a Linux Azure Storage-adatokkal dolgozni.</span><span class="sxs-lookup"><span data-stu-id="efc5d-152">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="efc5d-153">Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="efc5d-153">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="efc5d-154">Az eszköz segítségével az Azure Storage problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="efc5d-154">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md