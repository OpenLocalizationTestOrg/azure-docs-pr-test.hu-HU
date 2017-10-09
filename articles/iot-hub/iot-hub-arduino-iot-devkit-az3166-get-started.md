---
title: "aaaIoT DevKit toocloud - csatlakozás IoT DevKit AZ3166 tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és csatlakozzon az IoT DevKit AZ3166 tooAzure IoT-központ az Azure-felhőbe toohello toosend adatplatform ebben az oktatóanyagban."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: xshi
ms.openlocfilehash: fd36abaed1fd9f73028b84312bca9e900fb9c004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-iot-devkit-az3166-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="2d7e0-103">Csatlakozás IoT DevKit AZ3166 tooAzure hello felhőben az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="2d7e0-103">Connect IoT DevKit AZ3166 tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="2d7e0-104">Hello [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) lehet használt toodevelop és prototípus kihasználva a Microsoft Azure-szolgáltatások az eszközök internetes hálózatát (IoT) megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-104">hello [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) can be used toodevelop and prototype Internet of Things (IoT) solutions leveraging Microsoft Azure services.</span></span> <span data-ttu-id="2d7e0-105">Ez magában foglalja egy Arduino kompatibilis board gazdag perifériák és érzékelők, egy nyílt forráskódú board csomagot, és egy növekvő [projektek katalógus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span><span class="sxs-lookup"><span data-stu-id="2d7e0-105">It includes an Arduino compatible board with rich peripherals and sensors, an open-source board package and a growing [projects catalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="2d7e0-106">Mit</span><span class="sxs-lookup"><span data-stu-id="2d7e0-106">What you do</span></span>
<span data-ttu-id="2d7e0-107">Csatlakozás [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT hub létrehozása, az érzékelők hőmérséklet és a páratartalom adatok gyűjtése hello, és hello adatok tooIoT hub küldése.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-107">Connect [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT hub that you create, collect hello temperature and humidity data from sensors and send hello data tooIoT hub.</span></span>

<span data-ttu-id="2d7e0-108">Még nem rendelkezik egy DevKit?</span><span class="sxs-lookup"><span data-stu-id="2d7e0-108">Don't have a DevKit yet?</span></span> <span data-ttu-id="2d7e0-109">Egy új beolvasása [Itt](https://aka.ms/iot-devkit-purchase).</span><span class="sxs-lookup"><span data-stu-id="2d7e0-109">Get a new one [here](https://aka.ms/iot-devkit-purchase).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="2d7e0-110">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="2d7e0-110">What you learn</span></span>

* <span data-ttu-id="2d7e0-111">Hogyan tooconnect IoT DevKit tooWireless hozzáférési pont, és a fejlesztőkörnyezet előkészítése.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-111">How tooconnect IoT DevKit tooWireless access point and prepare your development environment.</span></span>
* <span data-ttu-id="2d7e0-112">Hogyan toocreate az IoT-központ és az eszköz regisztrálása az MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-112">How toocreate an IoT hub and register a device for MXChip IoT DevKit.</span></span>
* <span data-ttu-id="2d7e0-113">Hogyan toocollect érzékelőadatait MXChip IoT DevKit mintaalkalmazás futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-113">How toocollect sensor data by running a sample application on MXChip IoT DevKit.</span></span>
* <span data-ttu-id="2d7e0-114">Hogyan toosend hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-114">How toosend hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2d7e0-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="2d7e0-115">What you need</span></span>

* <span data-ttu-id="2d7e0-116">Egy MXChip IoT DevKit kártya micro USB-kábellel.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-116">An MXChip IoT DevKit board with a micro USB cable.</span></span> [<span data-ttu-id="2d7e0-117">Most töltse le innen</span><span class="sxs-lookup"><span data-stu-id="2d7e0-117">Get it now</span></span>](https://aka.ms/iot-devkit-purchase)
* <span data-ttu-id="2d7e0-118">A Windows 10 vagy macOS 10.10 + rendszert futtató számítógépen</span><span class="sxs-lookup"><span data-stu-id="2d7e0-118">A computer running Windows 10 or macOS 10.10+</span></span>
* <span data-ttu-id="2d7e0-119">Aktív Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="2d7e0-119">An active Azure subscription</span></span>
  * <span data-ttu-id="2d7e0-120">Aktiválja a [ingyenes 30 napos próbafiókot Microsoft Azure](https://azureinfo.microsoft.com/us-freetrial.html)</span><span class="sxs-lookup"><span data-stu-id="2d7e0-120">Activate a [free 30-day trial Microsoft Azure account](https://azureinfo.microsoft.com/us-freetrial.html)</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="2d7e0-121">Készítse elő a hardvert</span><span class="sxs-lookup"><span data-stu-id="2d7e0-121">Prepare your hardware</span></span>

<span data-ttu-id="2d7e0-122">Hello hardver tooyour számítógép bekötése.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-122">Hook up hello hardware tooyour computer.</span></span>

### <a name="hardware-you-need"></a><span data-ttu-id="2d7e0-123">Szükséges hardver</span><span class="sxs-lookup"><span data-stu-id="2d7e0-123">Hardware you need</span></span>

* <span data-ttu-id="2d7e0-124">DevKit tábla</span><span class="sxs-lookup"><span data-stu-id="2d7e0-124">DevKit board</span></span>
* <span data-ttu-id="2d7e0-125">Micro USB-kábellel</span><span class="sxs-lookup"><span data-stu-id="2d7e0-125">Micro USB cable</span></span>

![első-lépések – hardver](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-tooyour-computer"></a><span data-ttu-id="2d7e0-127">DevKit tooyour számítógép</span><span class="sxs-lookup"><span data-stu-id="2d7e0-127">Connect DevKit tooyour computer</span></span>

1. <span data-ttu-id="2d7e0-128">Csatlakoztassa az USB end tooyour PC</span><span class="sxs-lookup"><span data-stu-id="2d7e0-128">Connect USB end tooyour PC</span></span>
2. <span data-ttu-id="2d7e0-129">Csatlakozás Micro USB end toohello DevKit</span><span class="sxs-lookup"><span data-stu-id="2d7e0-129">Connect Micro USB end toohello DevKit</span></span>
3. <span data-ttu-id="2d7e0-130">zöld hello LED következő toopower megerősíti, hogy kapcsolat</span><span class="sxs-lookup"><span data-stu-id="2d7e0-130">hello green LED next toopower confirms connection</span></span>

![első lépések-csatlakozású](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a><span data-ttu-id="2d7e0-132">Wi-Fi konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2d7e0-132">Configure WiFi</span></span>

<span data-ttu-id="2d7e0-133">IoT-projektek internetkapcsolat támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-133">IoT projects rely on Internet connectivity.</span></span> <span data-ttu-id="2d7e0-134">A következő utasításokat tooconfigure hello DevKit tooconnect tooWiFi hello használata.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-134">Use hello following instructions tooconfigure hello DevKit tooconnect tooWiFi.</span></span>

### <a name="enter-ap-mode"></a><span data-ttu-id="2d7e0-135">Ázsia és a Csendes üzemmód megadása</span><span class="sxs-lookup"><span data-stu-id="2d7e0-135">Enter AP Mode</span></span>

<span data-ttu-id="2d7e0-136">Tartsa lenyomva a B gomb, majd állítsa vissza a leküldéses és kiadás hello a gombra, majd a kiadás gomb a b kiszolgálóra. A DevKit konfigurálásához a Wi-Fi hozzáférési mód adja meg.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-136">Hold down button B, then push and release hello reset button, then release button B. Your DevKit will enter AP Mode for configuring WiFi.</span></span> <span data-ttu-id="2d7e0-137">üdvözlő képernyőt hello szolgáltatás, állítsa be a hello DevKit Identifier(SSID), valamint a hello konfigurációs portál IP-cím jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-137">hello screen will display hello Service Set Identifier(SSID) of hello DevKit as well as hello configuration portal IP address:</span></span>

![első-elindult-wifi-hozzáférési pont](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-toodevkit-ap"></a><span data-ttu-id="2d7e0-139">Csatlakozás tooDevKit hozzáférési pont</span><span class="sxs-lookup"><span data-stu-id="2d7e0-139">Connect tooDevKit AP</span></span>

<span data-ttu-id="2d7e0-140">Most már használja egy másik Wi-Fi engedélyezett eszköz (PC vagy telefonon keresztül) tooconnect toohello DevKit SSID (hello fenti képernyőfelvételen kiemelt), hello jelszót hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-140">Now, use another WiFi enabled device (PC or mobile phone) tooconnect toohello DevKit SSID (highlighted in hello screenshot above), leave hello password empty.</span></span>

![első-elindult-ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a><span data-ttu-id="2d7e0-142">Wi-Fi DevKit konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2d7e0-142">Configure WiFi for DevKit</span></span>

<span data-ttu-id="2d7e0-143">Megnyitás hello IP-címet a hello DevKit képernyőn a számítógépen vagy a mobiltelefon böngésző, válassza ki azt szeretné, hogy hello DevKit tooconnect hello Wi-Fi hálózatot, majd hello jelszót írja be.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-143">Open hello IP address shown on hello DevKit screen on your PC or mobile phone browser, select hello WiFi network you want hello DevKit tooconnect to, then type hello password.</span></span> <span data-ttu-id="2d7e0-144">Kattintson a **Connect** toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-144">Click **Connect** toocomplete:</span></span>

![első-elindult-Wi-Fi-portál](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

<span data-ttu-id="2d7e0-146">Miután hello csatlakozás sikeres, hello DevKit néhány másodperc múlva újraindul.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-146">Once hello connection succeeds, hello DevKit will reboot in a few seconds.</span></span> <span data-ttu-id="2d7e0-147">Ha sikeres volt, az üdvözlő képernyőt hello Wi-Fi nevét és IP-címet fog látni:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-147">If succeeded, you will see hello WiFi name and IP address on hello screen:</span></span>

![első lépések – Wi-Fi-ip-](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
<span data-ttu-id="2d7e0-149">hello IP-cím hello fénykép megjelenő előfordulhat, hogy nem egyezik a tényleges IP hello rendelve, és a hello DevKit képernyőn jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-149">hello IP address displayed in hello photo may not match hello actual IP assigned and displayed on hello DevKit screen.</span></span> <span data-ttu-id="2d7e0-150">Ez nem rendellenes, DHCP toodynamically IP-címek hozzárendelése Wi-Fi használ.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-150">This is normal as WiFi uses DHCP toodynamically assign IPs.</span></span>

<span data-ttu-id="2d7e0-151">Wi-Fi beállítások konfigurálása után a hitelesítő adatok maradnak az adott kapcsolathoz hello eszköz akkor is, ha a választható le.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-151">After WiFi is configured, your credentials will be persisted on hello device for that connection, even if unplugged.</span></span> <span data-ttu-id="2d7e0-152">Például, ha az otthoni Wi-Fi hello DevKit konfigurálva, és ezután tartott hello DevKit toohello office, akkor tooreconfigure Ázsia és a Csendes módban (lépéstől kezdve **Ázsia és a Csendes módot adja meg**) tooconnect hello DevKit tooyour office Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-152">For example, if you configured hello DevKit for WiFi in your home and then took hello DevKit toohello office, you will need tooreconfigure AP mode (starting at step **Enter AP Mode**) tooconnect hello DevKit tooyour office WiFi.</span></span> 

## <a name="start-using-devkit"></a><span data-ttu-id="2d7e0-153">Ismerkedés a DevKit</span><span class="sxs-lookup"><span data-stu-id="2d7e0-153">Start using DevKit</span></span>

<span data-ttu-id="2d7e0-154">hello alapértelmezett app DevKit futó ellenőrizze hello hello belső vezérlőprogramjának legújabb verzióját, és a meg néhány érzékelő diagnosztikai adatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-154">hello default app running on DevKit will check hello latest version of hello firmware and display some sensor diagnosis data for you.</span></span>

### <a name="upgrade-toohello-latest-firmware"></a><span data-ttu-id="2d7e0-155">Toohello legújabb belső vezérlőprogram frissítése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-155">Upgrade toohello latest firmware</span></span>

<span data-ttu-id="2d7e0-156">Kérni fogja az üdvözlő képernyőt mindkét hello aktuális és a legújabb belső vezérlőprogram-verziója, ha egy frissítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-156">You will be prompted on hello screen both hello current and latest firmware version if there is an upgrade needed.</span></span> <span data-ttu-id="2d7e0-157">Hajtsa végre a [belső vezérlőprogram frissítése](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) tooupgrade útmutató azt.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-157">Follow [Upgrade firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide tooupgrade it.</span></span>

![első-elindult-belső vezérlőprogram](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
<span data-ttu-id="2d7e0-159">Ez egy egyszeri annak érdekében, a hello DevKit fejlesztése elindítását követően, és töltse fel az alkalmazás, hogy hello legújabb belső vezérlőprogrammal rendelkeznek az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-159">This is a one-time effort, once you start developing on hello DevKit and upload your app, you will have hello latest firmware come with your app.</span></span>

### <a name="test-various-sensors"></a><span data-ttu-id="2d7e0-160">Különböző érzékelők tesztelése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-160">Test various sensors</span></span>

<span data-ttu-id="2d7e0-161">Nyomja le az ENTER gombot B tootest érzékelők, akkor folytassa a lenyomásával, és tehessen közzé hello B gomb toocycle keresztül minden érzékelő.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-161">Press button B tootest sensors, continue pressing and releasing hello B button toocycle through each sensor.</span></span>

![első-elindult-érzékelő](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a><span data-ttu-id="2d7e0-163">Fejlesztői környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-163">Prepare development environment</span></span>

<span data-ttu-id="2d7e0-164">Most azt is, hogy idő tooset hello fejlesztési környezet létrehozása: eszközök és toobuild, IoT-alkalmazásokhoz kábítás csomagjai.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-164">Now it's time tooset up hello development environment: tools and packages for you toobuild stunning IoT applications.</span></span> <span data-ttu-id="2d7e0-165">Lehetősége van a Windows vagy macOS verzió tooyour operációs rendszer szerint.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-165">You can choose Windows or macOS version according tooyour operating system.</span></span>


### <a name="windows"></a><span data-ttu-id="2d7e0-166">Windows</span><span class="sxs-lookup"><span data-stu-id="2d7e0-166">Windows</span></span>

<span data-ttu-id="2d7e0-167">Javasoljuk, hogy toouse hello telepítési csomag tooprepare hello fejlesztési környezet.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-167">We encourage you toouse hello installation package tooprepare hello development environment.</span></span> <span data-ttu-id="2d7e0-168">Ha problémába ütközik, kövesse hello [manuális](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget azt meg.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-168">If you encounter any issues, you can follow hello [manual steps](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget it done.</span></span>

#### <a name="download-latest-package"></a><span data-ttu-id="2d7e0-169">Töltse le a legfrissebb csomagot</span><span class="sxs-lookup"><span data-stu-id="2d7e0-169">Download latest package</span></span>

<span data-ttu-id="2d7e0-170">Hello `.zip` letöltött fájl tartalmazza az összes szükséges eszközök és DevKit fejlesztéshez szükséges csomagok.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-170">hello `.zip` file you download contains all necessary tools and packages required for DevKit development.</span></span>

> [!div class="button"]
[<span data-ttu-id="2d7e0-171">Letöltés</span><span class="sxs-lookup"><span data-stu-id="2d7e0-171">Download</span></span>](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> <span data-ttu-id="2d7e0-172">Hello `.zip` fájl tartalmazza-e hello alábbi eszközök és a csomagok.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-172">hello `.zip` file contains hello following tools and packages.</span></span> <span data-ttu-id="2d7e0-173">Ha már rendelkezik néhány összetevőt telepíteni, hello parancsfájl azonosítja, és hagyja ki őket.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-173">If you already have some components installed, hello script will detect and skip them.</span></span>
> * <span data-ttu-id="2d7e0-174">NODE.js és a Yarn: futásidejű hello telepítési parancsfájlt és automatizált feladatok</span><span class="sxs-lookup"><span data-stu-id="2d7e0-174">Node.js and Yarn: Runtime for hello setup script and automated tasks</span></span>
> * <span data-ttu-id="2d7e0-175">[Az Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) -platformfüggetlen parancssori élmény kezeléséhez az Azure-erőforrások, hello MSI függő Python és a pip tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) - Cross-platform command-line experience for managing Azure resources, hello MSI contains dependent Python and pip.</span></span>
> * <span data-ttu-id="2d7e0-176">[A Visual Studio Code](https://code.visualstudio.com/): DevKit fejlesztési egyszerűsített kód szerkesztése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-176">[Visual Studio Code](https://code.visualstudio.com/): Lightweight code editor for DevKit development</span></span>
> * <span data-ttu-id="2d7e0-177">[A Visual Studio Code kiterjesztése Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): lehetővé teszi, hogy Arduino fejlesztési Visual STUDIO Code</span><span class="sxs-lookup"><span data-stu-id="2d7e0-177">[Visual Studio Code extension for Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Enables Arduino development in VS Code</span></span>
> * <span data-ttu-id="2d7e0-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): hello kiterjesztése Arduino támaszkodik ezt az eszközt</span><span class="sxs-lookup"><span data-stu-id="2d7e0-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): hello extension for Arduino relies on this tool</span></span>
> * <span data-ttu-id="2d7e0-179">DevKit Bizottság csomag: Eszköz láncok, könyvtárak és hello DevKit projektek</span><span class="sxs-lookup"><span data-stu-id="2d7e0-179">DevKit Board Package: Tool chains, libraries and projects for hello DevKit</span></span>
> * <span data-ttu-id="2d7e0-180">St. csatolású segédprogram: Alapvető segédprogramok és illesztőprogramok</span><span class="sxs-lookup"><span data-stu-id="2d7e0-180">ST-Link Utility: Essential utilities and drivers</span></span>

#### <a name="run-installation-script"></a><span data-ttu-id="2d7e0-181">Telepítési parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="2d7e0-181">Run installation script</span></span>

<span data-ttu-id="2d7e0-182">A Windows Fájlkezelőben keresse meg a hello `.zip` és kiszolgálóprojektjét, keresse meg `install.cmd`, kattintson a jobb gombbal, és válassza ki **"Futtatás rendszergazdaként"** toostart.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-182">In Windows File Explorer, locate hello `.zip` and extract it, find `install.cmd`, right-click and select **"Run as administrator"** toostart.</span></span>

![első lépések-Futtatás-rendszergazda](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

<span data-ttu-id="2d7e0-184">A telepítés során jelenik meg minden egyes eszköz vagy a csomag hello előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-184">During installation, you will see hello progress of each tool or package.</span></span>

![első lépések – telepítés](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-tooinstall-drivers"></a><span data-ttu-id="2d7e0-186">Erősítse meg a tooinstall illesztőprogramok</span><span class="sxs-lookup"><span data-stu-id="2d7e0-186">Confirm tooinstall drivers</span></span>

<span data-ttu-id="2d7e0-187">Visual STUDIO Code hello Arduino bővítmény hello Arduino IDE támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-187">hello VS Code for Arduino extension relies on hello Arduino IDE.</span></span> <span data-ttu-id="2d7e0-188">Ha hello első alkalommal hello Arduino IDE telepíti, rákérdezéses tooinstall megfelelő illesztőprogramok fogja:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-188">If this is hello first time you are installing hello Arduino IDE, you will be prompted tooinstall relevant drivers:</span></span>

![első lépések-illesztőprogram](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

<span data-ttu-id="2d7e0-190">Attól függően, hogy az internetkapcsolat sebessége körülbelül 10 percig toofinish telepítési kell vennie.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-190">It should take around 10 minutes toofinish installation depending on your Internet speed.</span></span> <span data-ttu-id="2d7e0-191">Hello telepítés befejeződése után megtekintheti a Visual Studio Code és Arduino IDE parancsikonok az asztalon.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-191">Once hello installation is complete, you should see Visual Studio Code and Arduino IDE shortcuts on your desktop.</span></span>

> [!NOTE] 
<span data-ttu-id="2d7e0-192">Alkalmanként Visual STUDIO Code elindításakor kérni fogja, amely nem található Arduino IDE vagy a kapcsolódó tábla csomag hiba történt.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-192">Occasionally, when you launch VS Code, you will be prompted with an error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="2d7e0-193">azt, zárja be és a kód Arduino IDE indítsa el egyszer toosolve és a Visual STUDIO Code kell megkeresheti Arduino IDE elérési utat helyesen.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-193">toosolve it, close VS Code, launch Arduino IDE once and VS Code should locate Arduino IDE path correctly.</span></span>


### <a name="macos-preview"></a><span data-ttu-id="2d7e0-194">macOS (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="2d7e0-194">macOS (Preview)</span></span>

<span data-ttu-id="2d7e0-195">Kövesse ezeket a lépéseket tooprepare fejlesztőkörnyezet macOS.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-195">Follow these steps tooprepare development environment on macOS.</span></span>

#### <a name="install-azure-cli-20"></a><span data-ttu-id="2d7e0-196">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-196">Install Azure CLI 2.0</span></span>

<span data-ttu-id="2d7e0-197">Hajtsa végre a hello [hivatalos útmutató](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-197">Follow hello [official guide](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:</span></span>

<span data-ttu-id="2d7e0-198">Azure CLI 2.0 telepítése egy `curl` parancs:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-198">Install Azure CLI 2.0 with one `curl` command:</span></span>

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="2d7e0-199">Majd indítsa újra a parancs-rendszerhéj módosítások tootake hatás:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-199">And restart your command shell for changes tootake effect:</span></span>

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a><span data-ttu-id="2d7e0-200">Arduino IDE telepítése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-200">Install Arduino IDE</span></span>

<span data-ttu-id="2d7e0-201">a Visual Studio Code Arduino bővítmény hello hello Arduino IDE támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-201">hello Visual Studio Code Arduino extension relies on hello Arduino IDE.</span></span> <span data-ttu-id="2d7e0-202">Töltse le és telepítse a hello [Arduino IDE tartozó macOS](https://www.arduino.cc/en/Main/Software).</span><span class="sxs-lookup"><span data-stu-id="2d7e0-202">Download and install hello [Arduino IDE for macOS](https://www.arduino.cc/en/Main/Software).</span></span>

#### <a name="install-visual-studio-code"></a><span data-ttu-id="2d7e0-203">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-203">Install Visual Studio Code</span></span>

<span data-ttu-id="2d7e0-204">Töltse le és telepítse [macOS a Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="2d7e0-204">Download and install [Visual Studio Code for macOS](https://code.visualstudio.com/).</span></span> <span data-ttu-id="2d7e0-205">Ez lesz hello elsődleges fejlesztőeszköz DevKit IoT-alkalmazások készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-205">This will be hello primary development tool for building DevKit IoT applications.</span></span>

####  <a name="download-latest-package"></a><span data-ttu-id="2d7e0-206">Töltse le a legfrissebb csomagot</span><span class="sxs-lookup"><span data-stu-id="2d7e0-206">Download latest package</span></span>

1. <span data-ttu-id="2d7e0-207">Telepítse a Node.js.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-207">Install Node.js.</span></span> <span data-ttu-id="2d7e0-208">Használhatja a legnépszerűbb macOS Csomagkezelő [Homebrew](https://brew.sh/) vagy [előzetesen elkészített telepítő](https://nodejs.org/en/download/) tooinstall azt.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-208">You can use popular macOS package manager [Homebrew](https://brew.sh/) or [pre-built installer](https://nodejs.org/en/download/) tooinstall it.</span></span>

2. <span data-ttu-id="2d7e0-209">Töltse le `.zip` DevKit fejlesztési Visual STUDIO Code szükséges feladat parancsfájlokat tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-209">Download `.zip` file containing task scripts required for DevKit development in VS Code.</span></span>

   > [!div class="button"]
   [<span data-ttu-id="2d7e0-210">Letöltés</span><span class="sxs-lookup"><span data-stu-id="2d7e0-210">Download</span></span>](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   <span data-ttu-id="2d7e0-211">Keresse meg a hello `.zip` és bontsa ki.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-211">Locate hello `.zip` and extract it.</span></span> <span data-ttu-id="2d7e0-212">Majd indítsa el **Terminálszolgáltatások** alkalmazást, és futtassa a következő parancsok tooconfigure hello:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-212">Then launch **Terminal** app and run hello following commands tooconfigure:</span></span>

   <span data-ttu-id="2d7e0-213">Kibontott mappát tooyour macOS felhasználói mappa áthelyezése:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-213">Move extracted folder tooyour macOS user folder:</span></span>
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   <span data-ttu-id="2d7e0-214">Telepítés `npm` csomagok:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-214">Install `npm` packages:</span></span>
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a><span data-ttu-id="2d7e0-215">Arduino a Visual STUDIO Code-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-215">Install VS Code extension for Arduino</span></span>

<span data-ttu-id="2d7e0-216">A Visual Studio Code lehetővé teszi tooinstall piactér bővítmények közvetlenül a hello eszköz, hello bővítmények ikon hello menü bal oldali ablaktáblán kattintson és kereshet `Arduino` tooinstall:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-216">Visual Studio Code allows you tooinstall Marketplace extensions directly in hello tool, simply click hello extensions icon in hello left menu pane and then search `Arduino` tooinstall:</span></span>

![telepítés-bővítmények](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a><span data-ttu-id="2d7e0-218">DevKit Bizottság csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-218">Install DevKit board package</span></span>

<span data-ttu-id="2d7e0-219">Szüksége lesz tooadd hello DevKit board a Visual Studio Code hello Board Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-219">You will need tooadd hello DevKit board using hello Board Manager in Visual Studio Code.</span></span>

1. <span data-ttu-id="2d7e0-220">Használja `Cmd+Shift+P` tooinvoke parancs paletta és típus **Arduino** majd keresse meg és jelölje **Arduino: Board Manager**.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-220">Use `Cmd+Shift+P` tooinvoke command palette and type **Arduino** then find and select **Arduino: Board Manager**.</span></span>

2. <span data-ttu-id="2d7e0-221">Kattintson a **"További URL-címet"** jobb hello alján.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-221">Click **'Additional URLs'** at hello bottom right.</span></span>
   <span data-ttu-id="2d7e0-222">![telepítés-további-URL-címek](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span><span class="sxs-lookup"><span data-stu-id="2d7e0-222">![installation-additional-urls](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span></span>

3. <span data-ttu-id="2d7e0-223">A hello `settings.json` fájlt, adja hozzá a sort hello aljához `USER SETTINGS` ablaktáblán, és mentse.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-223">In hello `settings.json` file, add a line at hello bottom of `USER SETTINGS` pane and save.</span></span>
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![telepítés-beállítások-json](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. <span data-ttu-id="2d7e0-225">Most a hello Board Manager keresse meg a "az3166", és telepítse hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-225">Now in hello Board Manager search for 'az3166' and install hello latest version.</span></span>
   <span data-ttu-id="2d7e0-226">![telepítés-az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span><span class="sxs-lookup"><span data-stu-id="2d7e0-226">![installation-az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span></span>

<span data-ttu-id="2d7e0-227">Most már rendelkezik az összes hello szükséges eszközöket és a macOS telepített csomagok.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-227">You now have all hello necessary tools and packages installed for macOS.</span></span>


## <a name="open-project-folder"></a><span data-ttu-id="2d7e0-228">Projekt megnyitása mappa</span><span class="sxs-lookup"><span data-stu-id="2d7e0-228">Open project folder</span></span>

### <a name="launch-vs-code"></a><span data-ttu-id="2d7e0-229">Indítsa el a VS Code szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-229">Launch VS Code</span></span>

<span data-ttu-id="2d7e0-230">Győződjön meg arról, hogy a DevKit nincs csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-230">Make sure your DevKit is not connected.</span></span> <span data-ttu-id="2d7e0-231">Indítsa el a Visual STUDIO Code először, és csatlakoztassa hello DevKit tooyour számítógépet.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-231">Launch VS Code first and connect hello DevKit tooyour computer.</span></span> <span data-ttu-id="2d7e0-232">Visual STUDIO Code automatikusan megtalálja, és egy bemutató lapja felugró:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-232">VS Code will automatically find it and pop up an introduction page:</span></span>

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
<span data-ttu-id="2d7e0-234">Alkalmanként Visual STUDIO Code elindításakor kérni fogja hibával, ami Arduino IDE vagy a kapcsolódó tábla csomag nem található.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-234">Occasionally, when you launch VS Code, you will be prompted with error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="2d7e0-235">azt, zárja be és a kód Arduino IDE indítsa el ismét toosolve és a Visual STUDIO Code kell megkeresheti Arduino IDE elérési utat helyesen.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-235">toosolve it, close VS Code, launch Arduino IDE once again and VS Code should locate Arduino IDE path correctly.</span></span>

### <a name="open-arduino-examples-folder"></a><span data-ttu-id="2d7e0-236">Arduino példák mappa megnyitása</span><span class="sxs-lookup"><span data-stu-id="2d7e0-236">Open Arduino Examples folder</span></span>

<span data-ttu-id="2d7e0-237">Váltás túl**"Arduino példák"** lapján keresse meg a túl`Examples for MXCHIP AZ3166 > AzureIoT` , majd kattintson a `GetStarted`.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-237">Switch too**'Arduino Examples'** tab, navigate too`Examples for MXCHIP AZ3166 > AzureIoT` and click on `GetStarted`.</span></span>

![Mini solution példák](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

<span data-ttu-id="2d7e0-239">Ha véletlenül tooclose hello ablaktáblában tooreload használja azt `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke parancs paletta és típus **Arduino** toofind válassza ki **Arduino: Példák**.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-239">If you happen tooclose hello pane, tooreload it, use `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke command palette and type **Arduino** toofind and select **Arduino: Examples**.</span></span>

## <a name="provision-azure-services"></a><span data-ttu-id="2d7e0-240">Azure-szolgáltatások kiépítése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-240">Provision Azure services</span></span>

<span data-ttu-id="2d7e0-241">Hello megoldás ablakban, a feladat futtatása `Ctrl+P` (macOS: `Cmd+P`) írja be a "felhő-provision feladat":</span><span class="sxs-lookup"><span data-stu-id="2d7e0-241">In hello solution window, run your task through `Ctrl+P` (macOS: `Cmd+P`) by typing 'task cloud-provision':</span></span>

<span data-ttu-id="2d7e0-242">Visual STUDIO Code terminál, egy interaktív parancssori végigvezeti Önt a kiépítési hello hello igényelt az Azure-szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-242">In hello VS Code terminal, an interactive command line will guide you through provisioning hello required Azure services:</span></span>

![Mini-solution-felhő-kiépítés](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a><span data-ttu-id="2d7e0-244">Build és Arduino vázlat feltöltése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-244">Build and upload Arduino sketch</span></span>

### <a name="install-required-library"></a><span data-ttu-id="2d7e0-245">Telepítse a szükséges kódtári</span><span class="sxs-lookup"><span data-stu-id="2d7e0-245">Install required library</span></span>

1. <span data-ttu-id="2d7e0-246">Nyomja le az `F1` vagy `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke parancs paletta és típus **Arduino** majd keresse meg és jelölje **Arduino: könyvtárkezelő**.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-246">Press `F1` or `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke command palette and type **Arduino** then find and select **Arduino: Library Manager**.</span></span>

2. <span data-ttu-id="2d7e0-247">Keresse meg `ArduinoJson` könyvtárban, és kattintson **telepítése**</span><span class="sxs-lookup"><span data-stu-id="2d7e0-247">Search for `ArduinoJson` library and click **Install**</span></span>

### <a name="build-and-upload-hello-device-code"></a><span data-ttu-id="2d7e0-248">Build és hello kód feltöltése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-248">Build and upload hello device code</span></span>

<span data-ttu-id="2d7e0-249">Használjon `Ctrl+P` (macOS: `Cmd+P`) toorun "feladat eszköz – feltöltés".</span><span class="sxs-lookup"><span data-stu-id="2d7e0-249">Use `Ctrl+P` (macOS: `Cmd+P`) toorun 'task device-upload'.</span></span> <span data-ttu-id="2d7e0-250">a Terminálszolgáltatások hello arra fogja kérni, tooenter konfigurációs módja.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-250">hello terminal will prompt you tooenter configuration mode.</span></span> <span data-ttu-id="2d7e0-251">toodo Igen, tartsa lenyomva a gombot A, majd leküldéses és kiadás hello alaphelyzetbe állítása gomb.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-251">toodo so, hold down button A, then push and release hello reset button.</span></span> <span data-ttu-id="2d7e0-252">"Konfiguráció" üdvözlő képernyőt jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-252">hello screen will display 'Configuration'.</span></span> <span data-ttu-id="2d7e0-253">Ez a tooset hello kapcsolati karakterláncot, amely lekéri a "feladat felhő-provision" lépéssel.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-253">This is tooset hello connection string that retrieves from 'task cloud-provision' step.</span></span>

<span data-ttu-id="2d7e0-254">Majd ellenőrzése, majd ismét feltölteni a hello Arduino vázlat fog elindulni:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-254">Then it will start verifying and uploading hello Arduino sketch:</span></span>

![Mini-solution-eszköz-feltöltése](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

<span data-ttu-id="2d7e0-256">hello DevKit újraindul, és a elinduló hello kódot.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-256">hello DevKit will reboot and start running hello code.</span></span>

## <a name="test-hello-project"></a><span data-ttu-id="2d7e0-257">Teszt hello projekt</span><span class="sxs-lookup"><span data-stu-id="2d7e0-257">Test hello project</span></span>

<span data-ttu-id="2d7e0-258">VS-kódban kattintson a hello power plug ikonjára hello állapotsorban tooopen hello soros figyelő.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-258">In VS Code, click hello power plug icon on hello status bar tooopen hello Serial Monitor.</span></span>

<span data-ttu-id="2d7e0-259">Amikor megjelenik a következő eredmények hello hello mintaalkalmazás sikeresen fut:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-259">hello sample application is running successfully when you see hello following results:</span></span>

* <span data-ttu-id="2d7e0-260">hello soros képernyők hello alatt hello képernyőfelvétel a hello tartalomként ugyanazokat az információkat.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-260">hello Serial Monitor displays hello same information as hello content in hello screenshot below.</span></span>
* <span data-ttu-id="2d7e0-261">a MXChip IoT DevKit LED hello villogó van.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-261">hello LED on MXChip IoT DevKit is blinking.</span></span>

![Végső kimenetet a Visual STUDIO Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a><span data-ttu-id="2d7e0-263">Problémák és visszajelzés</span><span class="sxs-lookup"><span data-stu-id="2d7e0-263">Problems and feedback</span></span>

<span data-ttu-id="2d7e0-264">Található [– gyakori kérdések](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) Ha problémákat tapasztal, vagy érheti el az alábbi hello csatornák toous.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-264">You can find [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) if you encounter problems or reach out toous from hello channels below.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d7e0-265">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d7e0-265">Next steps</span></span>

<span data-ttu-id="2d7e0-266">Sikeresen csatlakozott egy MXChip IoT DevKit tooyour IoT-központot, és elküldött hello rögzített érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2d7e0-266">You have successfully connected an MXChip IoT DevKit tooyour IoT Hub, and sent hello captured sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="2d7e0-267">első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:</span><span class="sxs-lookup"><span data-stu-id="2d7e0-267">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

- [<span data-ttu-id="2d7e0-268">Eszközök felhőalapú üzenetkezelése az iothub-explorerrel</span><span class="sxs-lookup"><span data-stu-id="2d7e0-268">Manage cloud device messaging with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [<span data-ttu-id="2d7e0-269">Az IoT-központ üzenetek tooAzure adattárolás mentése</span><span class="sxs-lookup"><span data-stu-id="2d7e0-269">Save IoT Hub messages tooAzure data storage</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [<span data-ttu-id="2d7e0-270">Használja a Power BI toovisualize valós idejű érzékelőadatok Azure IoT hubról</span><span class="sxs-lookup"><span data-stu-id="2d7e0-270">Use Power BI toovisualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [<span data-ttu-id="2d7e0-271">Használja az Azure Web Apps toovisualize valós idejű érzékelőadatok Azure IoT hubról</span><span class="sxs-lookup"><span data-stu-id="2d7e0-271">Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [<span data-ttu-id="2d7e0-272">Időjárás: hello érzékelő adatokat az IoT hub használata az Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2d7e0-272">Weather forecast using hello sensor data from your IoT hub in Azure Machine Learning</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [<span data-ttu-id="2d7e0-273">Eszközkezelés az iothub-explorerrel</span><span class="sxs-lookup"><span data-stu-id="2d7e0-273">Device management with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [<span data-ttu-id="2d7e0-274">Távoli figyelés és értesítések a Logic Apps használatával</span><span class="sxs-lookup"><span data-stu-id="2d7e0-274">Remote monitoring and notifications with Logic Apps</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)