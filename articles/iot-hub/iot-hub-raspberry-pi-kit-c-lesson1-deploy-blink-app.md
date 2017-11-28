---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 1: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazás hello a Githubból, és toodeploy gulp az alkalmazás tooyour málna Pi 3 tábla. A mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként."
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
ms.openlocfilehash: e90c3360c4de1873313db19561c781eb21dbf1d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a><span data-ttu-id="32e83-105">Hello villogási alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="32e83-105">Create and deploy hello blink application</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="32e83-106">Mit fog</span><span class="sxs-lookup"><span data-stu-id="32e83-106">What you will do</span></span>
<span data-ttu-id="32e83-107">Klónozza a C mintaalkalmazás hello a Githubból, és hello gulp eszköz toodeploy hello minta alkalmazás tooRaspberry Pi 3 használja.</span><span class="sxs-lookup"><span data-stu-id="32e83-107">Clone hello sample C application from GitHub, and use hello gulp tool toodeploy hello sample application tooRaspberry Pi 3.</span></span> <span data-ttu-id="32e83-108">hello mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="32e83-108">hello sample application blinks hello LED connected toohello board every two seconds.</span></span> <span data-ttu-id="32e83-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="32e83-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="32e83-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="32e83-110">What you will learn</span></span>
<span data-ttu-id="32e83-111">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="32e83-111">In this article, you will learn:</span></span>

* <span data-ttu-id="32e83-112">Hogyan toouse hello `device-discover-cli` Pi információ a hálózati eszköz tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="32e83-112">How toouse hello `device-discover-cli` tool tooretrieve networking information about Pi.</span></span>
* <span data-ttu-id="32e83-113">Hogyan toodeploy és futtatási hello mintaalkalmazást a Pi.</span><span class="sxs-lookup"><span data-stu-id="32e83-113">How toodeploy and run hello sample application on Pi.</span></span>
* <span data-ttu-id="32e83-114">Hogyan távolról futó Pi toodeploy és hibakeresési alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="32e83-114">How toodeploy and debug applications running remotely on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="32e83-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="32e83-115">What you need</span></span>
<span data-ttu-id="32e83-116">Sikeresen végrehajtotta a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="32e83-116">You must have successfully completed hello following operations:</span></span>

* [<span data-ttu-id="32e83-117">Az eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="32e83-117">Configure your device</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [<span data-ttu-id="32e83-118">Hello eszközök beszerzése</span><span class="sxs-lookup"><span data-stu-id="32e83-118">Get hello tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-hello-ip-address-and-host-name-of-pi"></a><span data-ttu-id="32e83-119">Hello IP-cím és a Pi név beszerzése</span><span class="sxs-lookup"><span data-stu-id="32e83-119">Obtain hello IP address and host name of Pi</span></span>
<span data-ttu-id="32e83-120">Nyisson meg egy parancssort a Windows vagy a Terminálszolgáltatások macOS vagy Ubuntu, és futtassa a parancsot a következő hello:</span><span class="sxs-lookup"><span data-stu-id="32e83-120">Open a command prompt in Windows or a terminal in macOS or Ubuntu, and then run hello following command:</span></span>

```bash
devdisco list --eth
```

<span data-ttu-id="32e83-121">Amely hasonló toohello következő kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="32e83-121">You should see an output that is similar toohello following:</span></span>

![Eszköz felderítése](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

<span data-ttu-id="32e83-123">Jegyezze fel a hello `IP address` és `hostname` pi.</span><span class="sxs-lookup"><span data-stu-id="32e83-123">Take note of hello `IP address` and `hostname` of Pi.</span></span> <span data-ttu-id="32e83-124">Ez a cikk későbbi részében tájékoztatásra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="32e83-124">You need this information later in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="32e83-125">Győződjön meg arról, hogy Pi ugyanaz, mint a számítógép hálózati csatlakoztatott toohello.</span><span class="sxs-lookup"><span data-stu-id="32e83-125">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="32e83-126">Például, ha a számítógép vezeték nélküli hálózathoz csatlakoztatott tooa Pi pedig vezetékes hálózathoz csatlakoztatott tooa, akkor előfordulhat, hogy nem lásd: hello IP-cím hello devdisco kimenet.</span><span class="sxs-lookup"><span data-stu-id="32e83-126">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="open-hello-sample-application"></a><span data-ttu-id="32e83-127">Nyissa meg hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="32e83-127">Open hello sample application</span></span>
<span data-ttu-id="32e83-128">tooopen hello mintaalkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="32e83-128">tooopen hello sample application, follow these steps:</span></span>

1. <span data-ttu-id="32e83-129">A Githubból hello minta tárház klónozása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="32e83-129">Clone hello sample repository from GitHub by running hello following command:</span></span>
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. <span data-ttu-id="32e83-130">Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="32e83-130">Open hello sample application in Visual Studio Code by running hello following commands:</span></span>
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

<span data-ttu-id="32e83-132">Hello `main.c` hello fájlban `app` almappa is hello kód toocontrol hello LED tartalmazó hello kulcs forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="32e83-132">hello `main.c` file in hello `app` subfolder is hello key source file that contains hello code toocontrol hello LED.</span></span>

### <a name="install-application-dependencies"></a><span data-ttu-id="32e83-133">Telepítse az alkalmazásfüggőségek</span><span class="sxs-lookup"><span data-stu-id="32e83-133">Install application dependencies</span></span>
<span data-ttu-id="32e83-134">Hello szalagtárak és más modulok hello a következő parancs futtatásával kell hello mintaalkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="32e83-134">Install hello libraries and other modules you need for hello sample application by running hello following command:</span></span>

```bash
npm install
```

## <a name="configure-hello-device-connection"></a><span data-ttu-id="32e83-135">Hello eszköz kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="32e83-135">Configure hello device connection</span></span>
<span data-ttu-id="32e83-136">tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="32e83-136">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="32e83-137">Hello eszköz konfigurációs fájl létrehozása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="32e83-137">Generate hello device configuration file by running hello following command:</span></span>
   
   ```bash
   gulp init
   ```
   
   <span data-ttu-id="32e83-138">hello konfigurációs fájl `config-raspberrypi.json` hello segítségével toolog tooPi felhasználói hitelesítő adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="32e83-138">hello configuration file `config-raspberrypi.json` contains hello user credentials you use toolog in tooPi.</span></span> <span data-ttu-id="32e83-139">tooavoid hello szivárgásával járnak a felhasználói hitelesítő adatok, hello konfigurációs fájl jön létre hello almappájában `.iot-hub-getting-started` hello otthoni mappa a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="32e83-139">tooavoid hello leak of user credentials, hello configuration file is generated in hello subfolder `.iot-hub-getting-started` of hello home folder on your computer.</span></span>

2. <span data-ttu-id="32e83-140">Nyissa meg a Visual Studio Code hello eszköz konfigurációs fájl hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="32e83-140">Open hello device configuration file in Visual Studio Code by running hello following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. <span data-ttu-id="32e83-141">Cserélje le a hello helyőrző `[device hostname or IP address]` hello IP-címmel vagy hello állomásnév korábban portáltól "Szerezze be a hello IP cím és a host name pi."</span><span class="sxs-lookup"><span data-stu-id="32e83-141">Replace hello placeholder `[device hostname or IP address]` with hello IP address or hello host name that you got previously in "Obtain hello IP address and host name of Pi."</span></span>
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> <span data-ttu-id="32e83-143">Használhat SSH-kulcs felhasználónév és jelszó helyett, tooRaspberry Pi kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="32e83-143">You can use SSH key instead of user name and password when connecting tooRaspberry Pi.</span></span> <span data-ttu-id="32e83-144">A rendezés toodo ez toogenerate hello kulcs használatával kell **ssh-keygen** és **ssh--azonosítót pi @\<eszköz címe\>**.</span><span class="sxs-lookup"><span data-stu-id="32e83-144">In order toodo this you will have toogenerate hello key using **ssh-keygen** and **ssh-copy-id pi@\<device address\>**.</span></span>
>
> <span data-ttu-id="32e83-145">A Windows ezen parancsok is elérhetők, a **Git bash eszközt**.</span><span class="sxs-lookup"><span data-stu-id="32e83-145">On Windows these commands are available in **Git Bash**.</span></span>
>
> <span data-ttu-id="32e83-146">A MacOS toorun kell **brew telepítése ssh--azonosítót**.</span><span class="sxs-lookup"><span data-stu-id="32e83-146">On MacOS you need toorun **brew install ssh-copy-id**.</span></span>
>
> <span data-ttu-id="32e83-147">Sikeresen feltöltése hello kulcs toohello málna Pi, után cserélje le a **device_password** rendelkező **device_key_path** tulajdonság **config-raspberrypi.json**.</span><span class="sxs-lookup"><span data-stu-id="32e83-147">After successfully uploading hello key toohello Raspberry Pi, replace **device_password** with **device_key_path** property in **config-raspberrypi.json**.</span></span>
>
> <span data-ttu-id="32e83-148">Frissített sorok az alábbiak szerint kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="32e83-148">Updated lines should look as below:</span></span>
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

<span data-ttu-id="32e83-149">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="32e83-149">Congratulations!</span></span> <span data-ttu-id="32e83-150">Sikeresen létrehozta a hello pi első mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="32e83-150">You've successfully created hello first sample application for Pi.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="32e83-151">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="32e83-151">Deploy and run hello sample application</span></span>
### <a name="install-hello-azure-iot-hub-sdk-on-pi"></a><span data-ttu-id="32e83-152">A Pi hello Azure IoT Hub SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="32e83-152">Install hello Azure IoT Hub SDK on Pi</span></span>
<span data-ttu-id="32e83-153">Hello Azure IoT Hub SDK telepítése Pi hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="32e83-153">Install hello Azure IoT Hub SDK on Pi by running hello following command:</span></span>

```bash
gulp install-tools
```

<span data-ttu-id="32e83-154">Ez a feladat eltarthat néhány percig toocomplete hello első futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="32e83-154">This task might take a few minutes toocomplete hello first time you run it.</span></span>

### <a name="deploy-and-run-hello-sample-app"></a><span data-ttu-id="32e83-155">Regisztrálhat és futtathat hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="32e83-155">Deploy and run hello sample app</span></span>
<span data-ttu-id="32e83-156">Telepíthet, és futtassa a hello mintaalkalmazást hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="32e83-156">Deploy and run hello sample application by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a><span data-ttu-id="32e83-157">Ellenőrizze a hello az alkalmazás akkor működik</span><span class="sxs-lookup"><span data-stu-id="32e83-157">Verify hello app works</span></span>
<span data-ttu-id="32e83-158">hello mintaalkalmazás automatikusan leáll, miután hello LED villogjon-20 alkalommal a.</span><span class="sxs-lookup"><span data-stu-id="32e83-158">hello sample application terminates automatically after hello LED blinks for 20 times.</span></span> <span data-ttu-id="32e83-159">Ha nem lát hello LED villogó, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="32e83-159">If you don’t see hello LED blinking, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>
<span data-ttu-id="32e83-160">![LED villogó](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span><span class="sxs-lookup"><span data-stu-id="32e83-160">![LED blinking](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)</span></span>

## <a name="summary"></a><span data-ttu-id="32e83-161">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="32e83-161">Summary</span></span>
<span data-ttu-id="32e83-162">A Pi hello szükséges eszközök toowork telepítése és telepített egy minta alkalmazás tooPi tooblink hello LED-jét.</span><span class="sxs-lookup"><span data-stu-id="32e83-162">You've installed hello required tools toowork with Pi and deployed a sample application tooPi tooblink hello LED.</span></span> <span data-ttu-id="32e83-163">Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely a Pi tooAzure IoT-központ toosend és üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="32e83-163">You can now create, deploy, and run another sample application that connects Pi tooAzure IoT Hub toosend and receive messages.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32e83-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32e83-164">Next steps</span></span>
[<span data-ttu-id="32e83-165">Azure-eszközök beszerzése</span><span class="sxs-lookup"><span data-stu-id="32e83-165">Get Azure tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

