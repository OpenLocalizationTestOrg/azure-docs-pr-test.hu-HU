---
title: "Csatlakozás az Azure IoT - 1. lecke Raspberry Pi (C): alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazást a Githubból, és ezt az alkalmazást a málna Pi 3 board telepítendő gulp. A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Raspberry pi vezetett villogási, villogási vezetett a raspberry pi"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f601d1e1-2bc3-4cc5-a6b1-0467e5304dcf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 2ae409c6a39521711777ec329d2507a2801cc985
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="082ad-105">A villogóalkalmazás elkészítése és üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="082ad-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="082ad-106">Mit fog</span><span class="sxs-lookup"><span data-stu-id="082ad-106">What you will do</span></span>
<span data-ttu-id="082ad-107">Klónozza a C mintaalkalmazást a Githubból, és a gulp eszközzel a mintaalkalmazás málna Pi 3 telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="082ad-107">Clone the sample C application from GitHub, and use the gulp tool to deploy the sample application to Raspberry Pi 3.</span></span> <span data-ttu-id="082ad-108">A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét.</span><span class="sxs-lookup"><span data-stu-id="082ad-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="082ad-109">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="082ad-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="082ad-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="082ad-110">What you will learn</span></span>
<span data-ttu-id="082ad-111">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="082ad-111">In this article, you will learn:</span></span>

* <span data-ttu-id="082ad-112">Hogyan használható a `device-discover-cli` eszköz Pi hálózati adatainak lekérésére.</span><span class="sxs-lookup"><span data-stu-id="082ad-112">How to use the `device-discover-cli` tool to retrieve networking information about Pi.</span></span>
* <span data-ttu-id="082ad-113">Hogyan telepítheti, és futtassa a mintaalkalmazást a Pi.</span><span class="sxs-lookup"><span data-stu-id="082ad-113">How to deploy and run the sample application on Pi.</span></span>
* <span data-ttu-id="082ad-114">Hogyan telepítheti, és távolról futó Pi-alkalmazások hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="082ad-114">How to deploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="082ad-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="082ad-115">What you need</span></span>
<span data-ttu-id="082ad-116">Sikeresen végrehajtotta a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="082ad-116">You must have successfully completed the following operations:</span></span>

* [<span data-ttu-id="082ad-117">Az eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="082ad-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [<span data-ttu-id="082ad-118">Eszközök</span><span class="sxs-lookup"><span data-stu-id="082ad-118">Get the tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a><span data-ttu-id="082ad-119">A IP-cím és a név a pi beszerzése</span><span class="sxs-lookup"><span data-stu-id="082ad-119">Obtain the IP address and host name of Pi</span></span>
<span data-ttu-id="082ad-120">Nyisson meg egy parancssort a Windows vagy a Terminálszolgáltatások macOS vagy Ubuntu, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="082ad-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run the following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="082ad-121">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="082ad-121">You should see an output that is similar to the following:</span></span>

![Eszköz felderítése](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="082ad-123">Vegye figyelembe a `IP address` és `hostname` pi.</span><span class="sxs-lookup"><span data-stu-id="082ad-123">Take note of the `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="082ad-124">Ez a cikk későbbi részében tájékoztatásra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="082ad-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="082ad-125">Győződjön meg arról, hogy a Pi és a számítógép ugyanahhoz a hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="082ad-125">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="082ad-126">Például ha a számítógép vezeték nélküli hálózathoz Pi egy vezetékes hálózathoz van csatlakoztatva, előfordulhat, hogy nem látja az IP-cím devdisco kimenet.</span><span class="sxs-lookup"><span data-stu-id="082ad-126">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="open-the-sample-application"></a><span data-ttu-id="082ad-127">Nyissa meg a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="082ad-127">Open the sample application</span></span>
<span data-ttu-id="082ad-128">A mintaalkalmazás megnyitásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="082ad-128">To open the sample application, follow these steps:</span></span>

1. <span data-ttu-id="082ad-129">A Githubból a minta-tárház klónozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="082ad-129">Clone the sample repository from GitHub by running the following command:</span></span>
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. <span data-ttu-id="082ad-130">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="082ad-130">Open the sample application in Visual Studio Code by running the following commands:</span></span>
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

<span data-ttu-id="082ad-132">A `main.c` fájlt a `app` almappa is szabályozhatja a LED kódot tartalmazó kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="082ad-132">The `main.c` file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="082ad-133">Telepítse az alkalmazásfüggőségek</span><span class="sxs-lookup"><span data-stu-id="082ad-133">Install application dependencies</span></span>
<span data-ttu-id="082ad-134">A szalagtárak és egyéb modulok a következő parancs futtatásával kell a mintaalkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="082ad-134">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="082ad-135">Az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="082ad-135">Configure the device connection</span></span>
<span data-ttu-id="082ad-136">Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="082ad-136">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="082ad-137">Az eszköz konfigurációs fájl létrehozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="082ad-137">Generate the device configuration file by running the following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="082ad-138">A konfigurációs fájl `config-raspberrypi.json` felhasználói hitelesítő adatokkal kell bejelentkezni Pi tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="082ad-138">The configuration file `config-raspberrypi.json` contains the user credentials you use to log in to Pi.</span></span> <span data-ttu-id="082ad-139">A felhasználói hitelesítő adatok memóriavesztés elkerülése érdekében a konfigurációs fájl jön létre, almappájában `.iot-hub-getting-started` az otthoni mappa a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="082ad-139">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="082ad-140">Nyissa meg a Visual Studio Code eszköz konfigurációs fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="082ad-140">Open the device configuration file in Visual Studio Code by running the following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. <span data-ttu-id="082ad-141">Cserélje le a helyőrző `[device hostname or IP address]` az IP-cím vagy a korábban kapott állomásnév "szerezze be a IP-cím és a név a pi."</span><span class="sxs-lookup"><span data-stu-id="082ad-141">Replace the placeholder `[device hostname or IP address]` with the IP address or the host name that you got previously in "Obtain the IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="082ad-143">Használhat SSH-kulcs felhasználónév és jelszó helyett, málna Pi történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="082ad-143">You can use SSH key instead of user name and password when connecting to Raspberry Pi.</span></span> <span data-ttu-id="082ad-144">Ehhez meg kell létrehozni, a kulcs használatával **ssh-keygen** és **ssh--azonosítót pi @\<eszköz címe\>**.</span><span class="sxs-lookup"><span data-stu-id="082ad-144">In order to do this you will have to generate the key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="082ad-145">A Windows ezen parancsok is elérhetők, a **Git bash eszközt**.</span><span class="sxs-lookup"><span data-stu-id="082ad-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="082ad-146">MacOS szüksége futtatásához **brew telepítése ssh--azonosítót**.</span><span class="sxs-lookup"><span data-stu-id="082ad-146">On MacOS you need to run **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="082ad-147">Után a kulcs sikeres feltöltését a málna Pi, cserélje le a **device_password** rendelkező **device_key_path** tulajdonság **config-raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="082ad-147">After successfully uploading the key to the Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="082ad-148">Frissített sorok az alábbiak szerint kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="082ad-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="082ad-149">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="082ad-149">Congratulations!</span></span> <span data-ttu-id="082ad-150">A pi első mintaalkalmazás sikeresen létrehozta.</span><span class="sxs-lookup"><span data-stu-id="082ad-150">You've successfully created the first sample application for Pi.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="082ad-151">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="082ad-151">Deploy and run the sample application</span></span>
### <a name="install-the-azure-iot-hub-sdk-on-pi"></a><span data-ttu-id="082ad-152">Az Azure IoT Hub SDK Pi telepítése</span><span class="sxs-lookup"><span data-stu-id="082ad-152">Install the Azure IoT Hub SDK on Pi</span></span>
<span data-ttu-id="082ad-153">Az Azure IoT Hub SDK Pi telepítése a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="082ad-153">Install the Azure IoT Hub SDK on Pi by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="082ad-154">Ez a feladat befejeződik, az első futtatásakor néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="082ad-154">This task might take a few minutes to complete the first time you run it.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="082ad-155">Központi telepítése és a mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="082ad-155">Deploy and run the sample app</span></span>
<span data-ttu-id="082ad-156">Telepíthet, és futtassa a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="082ad-156">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="082ad-157">Ellenőrizze az alkalmazás akkor működik</span><span class="sxs-lookup"><span data-stu-id="082ad-157">Verify the app works</span></span>
<span data-ttu-id="082ad-158">A mintaalkalmazás automatikusan leáll, miután a LED villogjon-20 alkalommal a.</span><span class="sxs-lookup"><span data-stu-id="082ad-158">The sample application terminates automatically after the LED blinks for 20 times.</span></span> <span data-ttu-id="082ad-159">Ha nem látja a LED villogó, tekintse meg a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="082ad-159">If you don’t see the LED blinking, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>
<span data-ttu-id="082ad-160">![LED villogó](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="082ad-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="082ad-161">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="082ad-161">Summary</span></span>
<span data-ttu-id="082ad-162">Pi használható szükséges eszközök telepítése és telepített egy mintaalkalmazást a LED villogni a-pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="082ad-162">You've installed the required tools to work with Pi and deployed a sample application to Pi to blink the LED.</span></span> <span data-ttu-id="082ad-163">Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti Pi Azure IoT Hub az üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="082ad-163">You can now create, deploy, and run another sample application that connects Pi to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="082ad-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="082ad-164">Next steps</span></span>
[<span data-ttu-id="082ad-165">Azure-eszközök beszerzése</span><span class="sxs-lookup"><span data-stu-id="082ad-165">Get Azure tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

