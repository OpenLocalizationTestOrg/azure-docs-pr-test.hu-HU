---
title: "Raspberry Pi (C) csatlakozzon az Azure IoT - hárítsa el a |} Microsoft Docs"
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
ms.openlocfilehash: 828669db23fa8d608029134fbe364033456d935a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="214c3-104">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="214c3-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="214c3-105">Hardverproblémák</span><span class="sxs-lookup"><span data-stu-id="214c3-105">Hardware issues</span></span>
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a><span data-ttu-id="214c3-106">Az alkalmazás futtatása is, de a LED nem villogó.</span><span class="sxs-lookup"><span data-stu-id="214c3-106">The application runs well but the LED is not blinking</span></span>
<span data-ttu-id="214c3-107">A probléma mindig kapcsolódik, a hardver expressroute kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="214c3-107">This issue is always related to the hardware circuit connectivity.</span></span> <span data-ttu-id="214c3-108">Az alábbi lépések segítségével azonosíthatja a problémákat.</span><span class="sxs-lookup"><span data-stu-id="214c3-108">Use the following steps to identify problems.</span></span>

1. <span data-ttu-id="214c3-109">Ellenőrizze, hogy úgy döntött, hogy a helyes **GPIO** a táblán.</span><span class="sxs-lookup"><span data-stu-id="214c3-109">Check that you chose the correct **GPIO** on your board.</span></span> <span data-ttu-id="214c3-110">A két portokat kell **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**.</span><span class="sxs-lookup"><span data-stu-id="214c3-110">The two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="214c3-111">Ellenőrizze, hogy helyesen-e a LED polaritásának.</span><span class="sxs-lookup"><span data-stu-id="214c3-111">Check that the polarity of your LED is correct.</span></span> <span data-ttu-id="214c3-112">A hosszabb engedélyezi kell jeleznie a **pozitív**, anód PIN-kódot.</span><span class="sxs-lookup"><span data-stu-id="214c3-112">The longer leg should indicate the **positive**, anode pin.</span></span>
3. <span data-ttu-id="214c3-113">Használja a **3.3V PIN-kód** és **GND PIN-kód** málna a Pi a 3.</span><span class="sxs-lookup"><span data-stu-id="214c3-113">Use the **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="214c3-114">A tartományvezérlő power Pi kezelje.</span><span class="sxs-lookup"><span data-stu-id="214c3-114">Treat Pi as the DC power.</span></span> <span data-ttu-id="214c3-115">Ellenőrizze, hogy jól működik a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="214c3-115">Check that the LED works fine.</span></span>

![LED meghatározása](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="214c3-117">Más hardverekkel kapcsolatos problémák szerepelnek</span><span class="sxs-lookup"><span data-stu-id="214c3-117">Other hardware issues</span></span>
<span data-ttu-id="214c3-118">Málna Pi 3 gyakori problémáinak megoldásával kapcsolatban további információkért lásd: a [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="214c3-118">For information about solving common problems on Raspberry Pi 3, see the [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="214c3-119">NODE.js csomag problémák</span><span class="sxs-lookup"><span data-stu-id="214c3-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="214c3-120">Gulp műveletek során nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="214c3-120">No response during gulp tasks</span></span>
<span data-ttu-id="214c3-121">Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá a `--verbose` hibakeresési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="214c3-121">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="214c3-122">Állítsa le a jelenlegi gulp feladatok használatával próbál `Ctrl + C`, és futtassa a következő parancsot a konzolablakban tekintse meg a hibakeresési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="214c3-122">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="214c3-123">A részletes hibaüzenetek a konzol kimeneti jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="214c3-123">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="214c3-124">Eszköz-felderítési problémák</span><span class="sxs-lookup"><span data-stu-id="214c3-124">Device discovery issues</span></span>
<span data-ttu-id="214c3-125">A gyakori problémák elhárításában súgó a `devdisco` parancs, ellenőrizze a [információs](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="214c3-125">For help in troubleshooting common problems with the `devdisco` command, check the [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="214c3-126">NPM problémák</span><span class="sxs-lookup"><span data-stu-id="214c3-126">NPM issues</span></span>
<span data-ttu-id="214c3-127">Próbálja meg frissíteni az NPM-csomagot a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="214c3-127">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="214c3-128">Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span><span class="sxs-lookup"><span data-stu-id="214c3-128">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="214c3-129">Távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="214c3-129">Remote debugging</span></span>

<span data-ttu-id="214c3-130">Távoli hibaelhárítási támogatás a Visual Studio Code C/C++ bővítmény hamarosan elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="214c3-130">Remote debugging support will be available soon in Visual Studio Code C/C++ Extension.</span></span>

<span data-ttu-id="214c3-131">Egy meanwhile GDB használhat a kedvenc SSH terminál keresztül:</span><span class="sxs-lookup"><span data-stu-id="214c3-131">In a meanwhile you can use GDB via your favourite SSH terminal:</span></span>

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a><span data-ttu-id="214c3-132">Azure-CLI problémák</span><span class="sxs-lookup"><span data-stu-id="214c3-132">Azure-CLI issues</span></span>
<span data-ttu-id="214c3-133">Az Azure parancssori felület (CLI) preview build.</span><span class="sxs-lookup"><span data-stu-id="214c3-133">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="214c3-134">Keresse meg a megoldást a [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) megoldások keresése.</span><span class="sxs-lookup"><span data-stu-id="214c3-134">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="214c3-135">Próbálja meg az Azure-cli parancsok nem a várt módon működik legújabb verziójára történő frissítés.</span><span class="sxs-lookup"><span data-stu-id="214c3-135">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="214c3-136">Ha az eszköz hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a a **problémák** a GitHub-tárház szakasza.</span><span class="sxs-lookup"><span data-stu-id="214c3-136">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="214c3-137">A gyakori problémák elhárításához tekintse meg a [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="214c3-137">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="214c3-138">Ha megfelel a "Egy olyan verzióra, amely eleget tesz a követelmény nem található", futtassa a következő parancsot a pip frissítése a legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="214c3-138">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="214c3-139">Python telepítési problémák</span><span class="sxs-lookup"><span data-stu-id="214c3-139">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="214c3-140">A hagyományos telepítési problémák (macOS)</span><span class="sxs-lookup"><span data-stu-id="214c3-140">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="214c3-141">Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="214c3-141">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="214c3-142">Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="214c3-142">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="214c3-143">Néhány **pip** a korábbi telepítés csomagok létrehozásakor a gyökérszintű, amely azt eredményezi, az engedély hiba.</span><span class="sxs-lookup"><span data-stu-id="214c3-143">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="214c3-144">A megoldás, hogy távolítsa el azokat a legfelső szintű telepített csomagokat.</span><span class="sxs-lookup"><span data-stu-id="214c3-144">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="214c3-145">Ez a feladat befejezéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="214c3-145">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="214c3-146">Ugrás: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="214c3-146">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="214c3-147">Csomagok létrehozása a legfelső szintű:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="214c3-147">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="214c3-148">A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="214c3-148">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="214c3-149">Telepítse újra a Python.</span><span class="sxs-lookup"><span data-stu-id="214c3-149">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="214c3-150">Az Azure IoT Hub-problémák</span><span class="sxs-lookup"><span data-stu-id="214c3-150">Azure IoT Hub issues</span></span>
<span data-ttu-id="214c3-151">Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és meg kell-e olyan eszköz, amely az IoT hub csatlakozó eszközök kezelésére, és próbálkozzon az alábbi eszközöket:</span><span class="sxs-lookup"><span data-stu-id="214c3-151">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="214c3-152">Eszköz Explorer</span><span class="sxs-lookup"><span data-stu-id="214c3-152">Device Explorer</span></span>
<span data-ttu-id="214c3-153">[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT hub csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="214c3-153">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="214c3-154">A következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="214c3-154">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="214c3-155">*Eszköz Identitáskezelés* szeretnék telepíteni és kezelni az IoT hub regisztrált eszközökre.</span><span class="sxs-lookup"><span data-stu-id="214c3-155">*Device identity management* to provision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="214c3-156">*Eszköz-felhő kap* , figyelheti az IoT hub az eszközről küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="214c3-156">*Receive device-to-cloud* so you can monitor messages sent from your device to your IoT hub.</span></span>
* <span data-ttu-id="214c3-157">*Felhő eszközre küldése* úgy küldhet üzeneteket az eszközök az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="214c3-157">*Send cloud-to-device* so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="214c3-158">Konfigurálja a `IoT hub connection string` ebből az eszközből készülék képességeinek használatához.</span><span class="sxs-lookup"><span data-stu-id="214c3-158">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="214c3-159">Az IoT-központ Explorer</span><span class="sxs-lookup"><span data-stu-id="214c3-159">IoT hub Explorer</span></span>
<span data-ttu-id="214c3-160">[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközügyfeleken kezelésére.</span><span class="sxs-lookup"><span data-stu-id="214c3-160">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="214c3-161">Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő eszközparancsok küldése.</span><span class="sxs-lookup"><span data-stu-id="214c3-161">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="214c3-162">Telepítse a legújabb (előzetes) verzióját az IOT hubbal-explorer eszköz, a következő parancsot a parancssori környezetben:</span><span class="sxs-lookup"><span data-stu-id="214c3-162">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```
npm install -g iothub-explorer@latest
```

<span data-ttu-id="214c3-163">A következő paranccsal minden IOT hubbal-explorer parancsot és paramétereket kapcsolatos további segítség:</span><span class="sxs-lookup"><span data-stu-id="214c3-163">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="214c3-164">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="214c3-164">Azure portal</span></span>
<span data-ttu-id="214c3-165">Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="214c3-165">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="214c3-166">Érdemes lehet használni a [Azure-portálon](../azure-portal-overview.md) kiépítéséhez, kezeléséhez, és hibakeresése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="214c3-166">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="214c3-167">Az Azure storage-problémák</span><span class="sxs-lookup"><span data-stu-id="214c3-167">Azure storage issues</span></span>
<span data-ttu-id="214c3-168">[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft, amelyek segítségével a Windows, a macOS és a Linux Azure Storage-adatokkal dolgozni.</span><span class="sxs-lookup"><span data-stu-id="214c3-168">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="214c3-169">Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="214c3-169">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="214c3-170">Az eszköz segítségével az Azure Storage problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="214c3-170">You can use this tool to troubleshoot your Azure Storage issues.</span></span>
