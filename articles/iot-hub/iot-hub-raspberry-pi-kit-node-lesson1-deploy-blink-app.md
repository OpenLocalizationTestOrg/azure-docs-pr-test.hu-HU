---
featureFlags: usabilla
title: "Csatlakozás az Azure IoT - 1. lecke Raspberry Pi (csomópont): alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a mintaalkalmazást Node.js a Githubból, és ezt az alkalmazást a málna Pi 3 board telepítendő gulp. A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Raspberry pi vezetett villogási, villogási vezetett a raspberry pi"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: a5a03a57-fe86-416f-90ff-6eca17775842
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8b73000c166950172c07b8e188025dc9da5bc011
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a><span data-ttu-id="1b181-105">A villogóalkalmazás elkészítése és üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="1b181-105">Create and deploy the blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="1b181-106">Mit fog</span><span class="sxs-lookup"><span data-stu-id="1b181-106">What you will do</span></span>
<span data-ttu-id="1b181-107">Klónozza a mintaalkalmazást Node.js a Githubból, és a gulp eszközzel a mintaalkalmazást a málna Pi 3 telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1b181-107">Clone the sample Node.js application from GitHub and use the gulp tool to deploy the sample application to your Raspberry Pi 3.</span></span> <span data-ttu-id="1b181-108">A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét.</span><span class="sxs-lookup"><span data-stu-id="1b181-108">The sample application blinks the LED connected to the board every two seconds.</span></span> <span data-ttu-id="1b181-109">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="1b181-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="1b181-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="1b181-110">What you will learn</span></span>
<span data-ttu-id="1b181-111">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="1b181-111">In this article, you will learn:</span></span>

* <span data-ttu-id="1b181-112">Hogyan használható a `device-discover-cli` eszköz Pi hálózati adatainak lekérésére.</span><span class="sxs-lookup"><span data-stu-id="1b181-112">How to use the `device-discover-cli` tool to retrieve networking information about Pi.</span></span>
* <span data-ttu-id="1b181-113">Hogyan telepítheti, és futtassa a mintaalkalmazást a Pi.</span><span class="sxs-lookup"><span data-stu-id="1b181-113">How to deploy and run the sample application on Pi.</span></span>
* <span data-ttu-id="1b181-114">Hogyan telepítheti, és távolról futó Pi-alkalmazások hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="1b181-114">How to deploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1b181-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="1b181-115">What you need</span></span>
<span data-ttu-id="1b181-116">Sikeresen végrehajtotta a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="1b181-116">You must have successfully completed the following operations:</span></span>

* [<span data-ttu-id="1b181-117">Az eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1b181-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
* [<span data-ttu-id="1b181-118">Eszközök</span><span class="sxs-lookup"><span data-stu-id="1b181-118">Get the tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a><span data-ttu-id="1b181-119">A IP-cím és a név a pi beszerzése</span><span class="sxs-lookup"><span data-stu-id="1b181-119">Obtain the IP address and host name of Pi</span></span>
<span data-ttu-id="1b181-120">Nyisson meg egy parancssort a Windows vagy a Terminálszolgáltatások macOS vagy Ubuntu, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1b181-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run the following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="1b181-121">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="1b181-121">You should see an output that is similar to the following:</span></span>

![Eszköz felderítése](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="1b181-123">Vegye figyelembe a `IP address` és `hostname` pi.</span><span class="sxs-lookup"><span data-stu-id="1b181-123">Take note of the `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="1b181-124">Ez a cikk későbbi részében tájékoztatásra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1b181-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="1b181-125">Győződjön meg arról, hogy a Pi és a számítógép ugyanahhoz a hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="1b181-125">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="1b181-126">Például ha a számítógép vezeték nélküli hálózathoz Pi egy vezetékes hálózathoz van csatlakoztatva, előfordulhat, hogy nem látja az IP-cím devdisco kimenet.</span><span class="sxs-lookup"><span data-stu-id="1b181-126">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="clone-the-sample-application"></a><span data-ttu-id="1b181-127">A mintaalkalmazás klónozása</span><span class="sxs-lookup"><span data-stu-id="1b181-127">Clone the sample application</span></span>
<span data-ttu-id="1b181-128">A mintakód megnyitásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1b181-128">To open the sample code, follow these steps:</span></span>

1. <span data-ttu-id="1b181-129">A Githubból a minta-tárház klónozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1b181-129">Clone the sample repository from GitHub by running the following command:</span></span>
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
   ```
2. <span data-ttu-id="1b181-130">Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1b181-130">Open the sample application in Visual Studio Code by running the following commands:</span></span>
   
   ```bash
   cd iot-hub-node-raspberrypi-getting-started
   cd Lesson1
   code .
   ```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

<span data-ttu-id="1b181-132">A `app.js` fájlt a `app` almappa is szabályozhatja a LED kódot tartalmazó kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="1b181-132">The `app.js` file in the `app` subfolder is the key source file that contains the code to control the LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="1b181-133">Telepítse az alkalmazásfüggőségek</span><span class="sxs-lookup"><span data-stu-id="1b181-133">Install application dependencies</span></span>
<span data-ttu-id="1b181-134">A szalagtárak és egyéb modulok a következő parancs futtatásával kell a mintaalkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="1b181-134">Install the libraries and other modules you need for the sample application by running the following command:</span></span>

```bash
npm install
```

## <a name="configure-the-device-connection"></a><span data-ttu-id="1b181-135">Az eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1b181-135">Configure the device connection</span></span>
<span data-ttu-id="1b181-136">Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1b181-136">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="1b181-137">Az eszköz konfigurációs fájl létrehozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1b181-137">Generate the device configuration file by running the following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="1b181-138">A konfigurációs fájl `config-raspberrypi.json` felhasználói hitelesítő adatokkal kell bejelentkezni Pi tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1b181-138">The configuration file `config-raspberrypi.json` contains the user credentials you use to log in to Pi.</span></span> <span data-ttu-id="1b181-139">A felhasználói hitelesítő adatok memóriavesztés elkerülése érdekében a konfigurációs fájl jön létre, almappájában `.iot-hub-getting-started` az otthoni mappa a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1b181-139">To avoid the leak of user credentials, the configuration file is generated in the subfolder `.iot-hub-getting-started` of the home folder on your computer.</span></span>

2. <span data-ttu-id="1b181-140">Nyissa meg a Visual Studio Code eszköz konfigurációs fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1b181-140">Open the device configuration file in Visual Studio Code by running the following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
3. <span data-ttu-id="1b181-141">Cserélje le a helyőrző `[device hostname or IP address]` az IP-cím vagy a korábban kapott állomásnév "szerezze be a IP-cím és a név a pi."</span><span class="sxs-lookup"><span data-stu-id="1b181-141">Replace the placeholder `[device hostname or IP address]` with the IP address or the host name that you got previously in "Obtain the IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="1b181-143">Használhat SSH-kulcs felhasználónév és jelszó helyett, málna Pi történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="1b181-143">You can use SSH key instead of user name and password when connecting to Raspberry Pi.</span></span> <span data-ttu-id="1b181-144">Ehhez meg kell létrehozni, a kulcs használatával **ssh-keygen** és **ssh--azonosítót pi @\<eszköz címe\>**.</span><span class="sxs-lookup"><span data-stu-id="1b181-144">In order to do this you will have to generate the key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="1b181-145">A Windows ezen parancsok is elérhetők, a **Git bash eszközt**.</span><span class="sxs-lookup"><span data-stu-id="1b181-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="1b181-146">MacOS szüksége futtatásához **brew telepítése ssh--azonosítót**.</span><span class="sxs-lookup"><span data-stu-id="1b181-146">On MacOS you need to run **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="1b181-147">Után a kulcs sikeres feltöltését a málna Pi, cserélje le a **device_password** rendelkező **device_key_path** tulajdonság **config-raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="1b181-147">After successfully uploading the key to the Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="1b181-148">Frissített sorok az alábbiak szerint kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="1b181-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="1b181-149">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1b181-149">Congratulations!</span></span> <span data-ttu-id="1b181-150">A pi első mintaalkalmazás sikeresen létrehozta.</span><span class="sxs-lookup"><span data-stu-id="1b181-150">You've successfully created the first sample application for Pi.</span></span>

## <a name="deploy-and-run-the-sample-application"></a><span data-ttu-id="1b181-151">Regisztrálhat és futtathat a mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="1b181-151">Deploy and run the sample application</span></span>
### <a name="install-nodejs-and-npm-on-pi"></a><span data-ttu-id="1b181-152">Node.js és NPM Pi telepítése</span><span class="sxs-lookup"><span data-stu-id="1b181-152">Install Node.js and NPM on Pi</span></span>
<span data-ttu-id="1b181-153">Node.js és telepítését NPM Pi a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1b181-153">Install Node.js and NPM on Pi by running the following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="1b181-154">Ez a feladat befejeződik, az első futtatásakor 10 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="1b181-154">This task might take 10 minutes to complete the first time you run it.</span></span>

### <a name="deploy-and-run-the-sample-app"></a><span data-ttu-id="1b181-155">Központi telepítése és a mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="1b181-155">Deploy and run the sample app</span></span>
<span data-ttu-id="1b181-156">Telepíthet, és futtassa a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1b181-156">Deploy and run the sample application by running the following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a><span data-ttu-id="1b181-157">Ellenőrizze az alkalmazás akkor működik</span><span class="sxs-lookup"><span data-stu-id="1b181-157">Verify the app works</span></span>
<span data-ttu-id="1b181-158">A Pi villogó két másodpercenként ekkor megjelenik a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="1b181-158">You should now see the LED on Pi blinking every two seconds.</span></span>  <span data-ttu-id="1b181-159">Ha nem látja a LED villogó, tekintse meg a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-node-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="1b181-159">If you don’t see the LED blinking, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>
<span data-ttu-id="1b181-160">![LED villogó](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="1b181-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="1b181-161">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="1b181-161">Summary</span></span>
<span data-ttu-id="1b181-162">Pi használható szükséges eszközök telepítése és telepített egy mintaalkalmazást a LED villogni a-pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="1b181-162">You've installed the required tools to work with Pi and deployed a sample application to Pi to blink the LED.</span></span> <span data-ttu-id="1b181-163">Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti Pi Azure IoT Hub az üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="1b181-163">You can now create, deploy, and run another sample application that connects Pi to Azure IoT Hub to send and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b181-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b181-164">Next steps</span></span>
[<span data-ttu-id="1b181-165">Az Azure eszközök</span><span class="sxs-lookup"><span data-stu-id="1b181-165">Get the Azure tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

