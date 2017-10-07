---
title: "M0 toocloud: csatlakozás lágyított M0 Wi-Fi tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset össze, és csatlakozzon a Adafruit lágyított M0 Wi-Fi tooAzure IoT-központ toosend toohello Azure felhőalapú adatplatform ebben az oktatóanyagban."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/16/2017
ms.author: xshi
ms.openlocfilehash: 6aabeb961a50ba5d3934f77eb1ccda4af1bf64c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-m0-wifi-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="15183-103">Csatlakozás Adafruit lágyított M0 Wi-Fi tooAzure hello felhőben az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="15183-103">Connect Adafruit Feather M0 WiFi tooAzure IoT Hub in hello cloud</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Egy BME280 lágyított M0 Wi-Fi és az IoT-központ közötti kapcsolat](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

<span data-ttu-id="15183-105">Ebben az oktatóanyagban meg először a Arduino board használatifeltétel hello alapjait tanulási.</span><span class="sxs-lookup"><span data-stu-id="15183-105">In this tutorial, you begin by learning hello basics of working with your Arduino board.</span></span> <span data-ttu-id="15183-106">Majd megismerheti, hogyan tooseamlessly összekapcsolni használatával eszközök toohello felhőalapú [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="15183-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="15183-107">Mit</span><span class="sxs-lookup"><span data-stu-id="15183-107">What you do</span></span>

<span data-ttu-id="15183-108">Csatlakozás Adafruit lágyított M0 Wi-Fi tooan IoT-központ az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="15183-108">Connect Adafruit Feather M0 WiFi tooan IoT hub that you create.</span></span> <span data-ttu-id="15183-109">Akkor futtassa a mintaalkalmazást M0 Wi-Fi toocollect hello hőmérséklet és a páratartalom adatok egy BME280.</span><span class="sxs-lookup"><span data-stu-id="15183-109">Then you run a sample application on M0 WiFi toocollect hello temperature and humidity data from a BME280.</span></span> <span data-ttu-id="15183-110">Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="15183-110">Finally, you send hello sensor data tooyour IoT hub.</span></span>


## <a name="what-you-learn"></a><span data-ttu-id="15183-111">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="15183-111">What you learn</span></span>

* <span data-ttu-id="15183-112">Hogyan toocreate az IoT-központ és az eszköz regisztrálása az lágyított M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="15183-112">How toocreate an IoT hub and register a device for Feather M0 WiFi</span></span>
* <span data-ttu-id="15183-113">Hogyan tooconnect lágyított M0 Wi-Fi hello érzékelő és a számítógép</span><span class="sxs-lookup"><span data-stu-id="15183-113">How tooconnect Feather M0 WiFi with hello sensor and your computer</span></span>
* <span data-ttu-id="15183-114">Hogyan toocollect érzékelőadatait lágyított M0 Wi-Fi mintaalkalmazás futtatásával</span><span class="sxs-lookup"><span data-stu-id="15183-114">How toocollect sensor data by running a sample application on Feather M0 WiFi</span></span>
* <span data-ttu-id="15183-115">Hogyan toosend hello érzékelő adatokat tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="15183-115">How toosend hello sensor data tooyour IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="15183-116">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="15183-116">What you need</span></span>

![Hello oktatóanyaghoz szükség részei](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="15183-118">toocomplete Ez a művelet következő részek a lágyított M0 Wi-Fi Starter Kit hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="15183-118">toocomplete this operation, you need hello following parts from your Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="15183-119">hello lágyított M0 Wi-Fi tábla</span><span class="sxs-lookup"><span data-stu-id="15183-119">hello Feather M0 WiFi board</span></span>
* <span data-ttu-id="15183-120">Egy Micro USB tooType egy USB-kábellel</span><span class="sxs-lookup"><span data-stu-id="15183-120">A Micro USB tooType A USB cable</span></span>

<span data-ttu-id="15183-121">A fejlesztési környezet dolgok következő hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="15183-121">You also need hello following things for your development environment:</span></span>

* <span data-ttu-id="15183-122">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="15183-122">An active Azure subscription.</span></span> <span data-ttu-id="15183-123">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="15183-123">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="15183-124">A Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.</span><span class="sxs-lookup"><span data-stu-id="15183-124">A Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="15183-125">A Wi-Fi M0 lágyított tooconnect vezeték nélküli hálózat.</span><span class="sxs-lookup"><span data-stu-id="15183-125">A wireless network for Feather M0 WiFi tooconnect to.</span></span>
* <span data-ttu-id="15183-126">Az internetes kapcsolat toodownload hello konfigurációs eszközt.</span><span class="sxs-lookup"><span data-stu-id="15183-126">An Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="15183-127">[Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="15183-127">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="15183-128">A korábbi hello Azure IoT Hub szalagtár nem működik.</span><span class="sxs-lookup"><span data-stu-id="15183-128">Earlier versions don't work with hello Azure IoT Hub library.</span></span>

<span data-ttu-id="15183-129">Ha nem rendelkezik érzékelő, a következő elemek hello opcionálisak.</span><span class="sxs-lookup"><span data-stu-id="15183-129">If you don’t have a sensor, hello following items are optional.</span></span> <span data-ttu-id="15183-130">Akkor is hello beállítással, szimulált érzékelő adatokat:</span><span class="sxs-lookup"><span data-stu-id="15183-130">You also have hello option of using simulated sensor data:</span></span>

* <span data-ttu-id="15183-131">BME280 hőmérséklet és a páratartalom érzékelő</span><span class="sxs-lookup"><span data-stu-id="15183-131">A BME280 temperature and humidity sensor</span></span>
* <span data-ttu-id="15183-132">Egy breadboard</span><span class="sxs-lookup"><span data-stu-id="15183-132">A breadboard</span></span>
* <span data-ttu-id="15183-133">M/M átkötés fenyegetéseknek</span><span class="sxs-lookup"><span data-stu-id="15183-133">M/M jumper wires</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-hello-sensor-and-your-computer"></a><span data-ttu-id="15183-134">Csatlakozás lágyított M0 Wi-Fi hello érzékelő és a számítógép</span><span class="sxs-lookup"><span data-stu-id="15183-134">Connect Feather M0 WiFi with hello sensor and your computer</span></span>
<span data-ttu-id="15183-135">Ebben a szakaszban hello érzékelők tooyour board csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="15183-135">In this section, you connect hello sensors tooyour board.</span></span> <span data-ttu-id="15183-136">Majd csatlakoztassa a eszköz tooyour számítógép további használatra.</span><span class="sxs-lookup"><span data-stu-id="15183-136">Then you plug in your device tooyour computer for further use.</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-m0-wifi"></a><span data-ttu-id="15183-137">Csatlakozás egy DHT22 hőmérséklet és a páratartalom érzékelő tooFeather M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="15183-137">Connect a DHT22 temperature and humidity sensor tooFeather M0 WiFi</span></span>

<span data-ttu-id="15183-138">Hello breadboard és átkötés fenyegetéseknek toomake hello kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="15183-138">Use hello breadboard and jumper wires toomake hello connection.</span></span> <span data-ttu-id="15183-139">Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.</span><span class="sxs-lookup"><span data-stu-id="15183-139">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Kapcsolatok referencia](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


<span data-ttu-id="15183-141">Érzékelő PIN-kód használja a következő vezetékezést hello:</span><span class="sxs-lookup"><span data-stu-id="15183-141">For sensor pins, use hello following wiring:</span></span>


| <span data-ttu-id="15183-142">Kezdő (érzékelő)</span><span class="sxs-lookup"><span data-stu-id="15183-142">Start (sensor)</span></span>           | <span data-ttu-id="15183-143">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="15183-143">End (board)</span></span>            | <span data-ttu-id="15183-144">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="15183-144">Cable color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="15183-145">VDD (PIN-kód 27A)</span><span class="sxs-lookup"><span data-stu-id="15183-145">VDD (Pin 27A)</span></span>            | <span data-ttu-id="15183-146">3V (PIN-kód 3A)</span><span class="sxs-lookup"><span data-stu-id="15183-146">3V (Pin 3A)</span></span>            | <span data-ttu-id="15183-147">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="15183-147">Red cable</span></span>     |
| <span data-ttu-id="15183-148">GND (PIN-kód 29A)</span><span class="sxs-lookup"><span data-stu-id="15183-148">GND (Pin 29A)</span></span>            | <span data-ttu-id="15183-149">GND [PIN-kód 6]</span><span class="sxs-lookup"><span data-stu-id="15183-149">GND (Pin 6A)</span></span>           | <span data-ttu-id="15183-150">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="15183-150">Black cable</span></span>   |
| <span data-ttu-id="15183-151">SCK (PIN-kód 30A)</span><span class="sxs-lookup"><span data-stu-id="15183-151">SCK (Pin 30A)</span></span>            | <span data-ttu-id="15183-152">SCK (PIN-kód 12A)</span><span class="sxs-lookup"><span data-stu-id="15183-152">SCK (Pin 12A)</span></span>          | <span data-ttu-id="15183-153">Sárga kábel</span><span class="sxs-lookup"><span data-stu-id="15183-153">Yellow cable</span></span>  |
| <span data-ttu-id="15183-154">SDO (PIN-kód 31A)</span><span class="sxs-lookup"><span data-stu-id="15183-154">SDO (Pin 31A)</span></span>            | <span data-ttu-id="15183-155">MI (PIN-kód 14A)</span><span class="sxs-lookup"><span data-stu-id="15183-155">MI (Pin 14A)</span></span>           | <span data-ttu-id="15183-156">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="15183-156">White cable</span></span>   |
| <span data-ttu-id="15183-157">SDI (PIN-kód 32A)</span><span class="sxs-lookup"><span data-stu-id="15183-157">SDI (Pin 32A)</span></span>            | <span data-ttu-id="15183-158">M0 (PIN-kód 13A)</span><span class="sxs-lookup"><span data-stu-id="15183-158">M0 (Pin 13A)</span></span>           | <span data-ttu-id="15183-159">Kék kábel</span><span class="sxs-lookup"><span data-stu-id="15183-159">Blue cable</span></span>    |
| <span data-ttu-id="15183-160">CS (PIN-kód 33A)</span><span class="sxs-lookup"><span data-stu-id="15183-160">CS (Pin 33A)</span></span>             | <span data-ttu-id="15183-161">GPIO 5 (PIN-kód 15J)</span><span class="sxs-lookup"><span data-stu-id="15183-161">GPIO 5 (Pin 15J)</span></span>       | <span data-ttu-id="15183-162">Narancssárga kábel</span><span class="sxs-lookup"><span data-stu-id="15183-162">Orange cable</span></span>  |

<span data-ttu-id="15183-163">További információkért lásd: [Adafruit BME280 páratartalom + légnyomás + hőmérséklet-érzékelő kitörése](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) és [Adafruit lágyított M0 Wi-Fi érintkezőkiosztása szerepel](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span><span class="sxs-lookup"><span data-stu-id="15183-163">For more information, see [Adafruit BME280 Humidity + Barometric Pressure + Temperature Sensor Breakout](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) and [Adafruit Feather M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span></span>



<span data-ttu-id="15183-164">Most már a lágyított M0 Wi-Fi kell csatlakoztatni a működő érzékelő.</span><span class="sxs-lookup"><span data-stu-id="15183-164">Now your Feather M0 WiFi should be connected with a working sensor.</span></span>

![Csatlakozás DHT22 lágyított Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-tooyour-computer"></a><span data-ttu-id="15183-166">Wi-Fi M0 lágyított tooyour számítógép</span><span class="sxs-lookup"><span data-stu-id="15183-166">Connect Feather M0 WiFi tooyour computer</span></span>

<span data-ttu-id="15183-167">Használjon hello Micro USB tooType A USB kábel tooconnect lágyított M0 Wi-Fi tooyour számítógépet, látható módon:</span><span class="sxs-lookup"><span data-stu-id="15183-167">Use hello Micro USB tooType A USB cable tooconnect Feather M0 WiFi tooyour computer, as shown:</span></span>

![Lágyított Huzzah tooyour számítógép](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="15183-169">Adja hozzá a soros port engedélyek (csak Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="15183-169">Add serial port permissions (Ubuntu only)</span></span>

<span data-ttu-id="15183-170">Ubuntu használatakor ellenőrizze, rendelkezik hello engedélyek toooperate a hello USB port a lágyított M0 Wi-Fi-e.</span><span class="sxs-lookup"><span data-stu-id="15183-170">If you use Ubuntu, make sure you have hello permissions toooperate on hello USB port of Feather M0 WiFi.</span></span> <span data-ttu-id="15183-171">tooadd soros port engedélyek, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15183-171">tooadd serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="15183-172">A terminálon futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="15183-172">At a terminal, run hello following commands:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="15183-173">A következő kimenetek hello egyik beolvasása:</span><span class="sxs-lookup"><span data-stu-id="15183-173">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="15183-174">crw-rw---1 legfelső szintű uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="15183-174">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="15183-175">crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="15183-175">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="15183-176">Figyelje meg, hogy hello kimenet `uucp` vagy `dialout` hello csoport tulajdonos neve hello USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="15183-176">In hello output, notice that `uucp` or `dialout` is hello group owner name of hello USB port.</span></span>

2. <span data-ttu-id="15183-177">tooadd hello toohello felhasználócsoportra, és futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="15183-177">tooadd hello user toohello group, run hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="15183-178">Hello előző lépésben beszerzett hello csoport tulajdonos neve `<group-owner-name>`.</span><span class="sxs-lookup"><span data-stu-id="15183-178">In hello previous step, you obtained hello group owner name `<group-owner-name>`.</span></span> <span data-ttu-id="15183-179">Ubuntu felhasználónév `<username>`.</span><span class="sxs-lookup"><span data-stu-id="15183-179">Your Ubuntu user name is `<username>`.</span></span>

3. <span data-ttu-id="15183-180">Hello módosítás tooappear, az Ubuntu kijelentkezni, és jelentkezzen be újra.</span><span class="sxs-lookup"><span data-stu-id="15183-180">For hello change tooappear, sign out of Ubuntu and then sign in again.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="15183-181">Érzékelő adatokat gyűjteni, és elküldi a tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="15183-181">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="15183-182">Ebben a szakaszban telepítése, és futtassa a mintaalkalmazást a lágyított M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="15183-182">In this section, you deploy and run a sample application on Feather M0 WiFi.</span></span> <span data-ttu-id="15183-183">hello mintaalkalmazás teszi hello LED pislogás lágyított M0 Wi-Fi be.</span><span class="sxs-lookup"><span data-stu-id="15183-183">hello sample application makes hello LED blink on Feather M0 WiFi.</span></span> <span data-ttu-id="15183-184">Majd küldi hello hőmérséklet és a páratartalom adatgyűjtés hello BME280 érzékelő tooyour az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="15183-184">It then sends hello temperature and humidity data collected from hello BME280 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github-and-prepare-hello-arduino-ide"></a><span data-ttu-id="15183-185">Hello mintaalkalmazás beszerzése a Githubról és hello Arduino IDE előkészítése</span><span class="sxs-lookup"><span data-stu-id="15183-185">Get hello sample application from GitHub and prepare hello Arduino IDE</span></span>

<span data-ttu-id="15183-186">hello mintaalkalmazást a Githubon található.</span><span class="sxs-lookup"><span data-stu-id="15183-186">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="15183-187">Klónozás hello minta tárház, amely tartalmazza a Githubról hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="15183-187">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="15183-188">tooclone hello minta tárház, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15183-188">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="15183-189">Nyisson meg egy parancssort vagy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="15183-189">Open a command prompt or a terminal window.</span></span>

2. <span data-ttu-id="15183-190">Nyissa meg a tooa mappára, ahol hello minta alkalmazás toobe tárolja.</span><span class="sxs-lookup"><span data-stu-id="15183-190">Go tooa folder where you want hello sample application toobe stored.</span></span>
3. <span data-ttu-id="15183-191">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="15183-191">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-hello-package-for-feather-m0-wifi-in-hello-arduino-ide"></a><span data-ttu-id="15183-192">Hello csomag telepítése a lágyított M0 Wi-Fi hello Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="15183-192">Install hello package for Feather M0 WiFi in hello Arduino IDE</span></span>

1. <span data-ttu-id="15183-193">Nyissa meg a mintaalkalmazás hello tároló hello mappát.</span><span class="sxs-lookup"><span data-stu-id="15183-193">Open hello folder where hello sample application is stored.</span></span>

2. <span data-ttu-id="15183-194">Nyissa meg a hello app.ino fájlt hello Arduino IDE hello alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="15183-194">Open hello app.ino file in hello app folder in hello Arduino IDE.</span></span>

   ![Nyissa meg a mintaalkalmazás hello Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. <span data-ttu-id="15183-196">Kattintson a **fájl** > **beállítások** (Windows/Linux) vagy **Arduino** > **beállítások** (Mac), másolja és hello csatolás alatt a hello **további modulok Manager URL-címet** hello Arduino IDE beállítások lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="15183-196">Click **File** > **Preferences** (Windows/Linux) or **Arduino** > **Preferences** (Mac) and copy and paste hello link below into hello **Additional Boards Manager URLs** option in hello Arduino IDE preferences.</span></span>
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. <span data-ttu-id="15183-197">Kattintson a **eszközök** > **Board** > **modulok Manager**, és telepítse a hello `Arduino SAMD Boards` verzió `1.6.2` vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="15183-197">Click **Tools** > **Board** > **Boards Manager**, and then install hello `Arduino SAMD Boards` version `1.6.2` or later.</span></span> 

1. <span data-ttu-id="15183-198">Ezt a hello ugyanazon ablakra, telepítse `Adafruit SAMD Boards` csomag tooadd hello board fájl definíciókat.</span><span class="sxs-lookup"><span data-stu-id="15183-198">Then in hello same window, install `Adafruit SAMD Boards` package tooadd hello board file definitions.</span></span>

   ![hello esp8266 csomag telepítve van](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. <span data-ttu-id="15183-200">Kattintson a **eszközök** > **Board** > **Adafruit M0 Wi-Fi**.</span><span class="sxs-lookup"><span data-stu-id="15183-200">Click **Tools** > **Board** > **Adafruit M0 WiFi**.</span></span>

5. <span data-ttu-id="15183-201">Illesztőprogramok telepítéséhez (csak Windows).</span><span class="sxs-lookup"><span data-stu-id="15183-201">Install drivers (for Windows only).</span></span> <span data-ttu-id="15183-202">Csatlakoztatásakor lágyított M0 Wi-Fi, szükség lehet a tooinstall illesztőprogramot.</span><span class="sxs-lookup"><span data-stu-id="15183-202">When you plug in Feather M0 WiFi, you might need tooinstall a driver.</span></span> <span data-ttu-id="15183-203">Kattintson a [hello letöltési hivatkozás hello weblapon](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello illesztőprogram telepítő.</span><span class="sxs-lookup"><span data-stu-id="15183-203">Click [hello download link on hello webpage](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello driver installer.</span></span> <span data-ttu-id="15183-204">Hajtsa végre a hello lépéseket tooinstall hello illesztőprogramokat.</span><span class="sxs-lookup"><span data-stu-id="15183-204">Follow hello steps tooinstall hello drivers you want.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="15183-205">Szükséges kódtárak telepítése</span><span class="sxs-lookup"><span data-stu-id="15183-205">Install necessary libraries</span></span>

1. <span data-ttu-id="15183-206">Hello Arduino IDE, kattintson **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.</span><span class="sxs-lookup"><span data-stu-id="15183-206">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>

2. <span data-ttu-id="15183-207">Keresse meg a következő könyvtár nevek egyenként hello.</span><span class="sxs-lookup"><span data-stu-id="15183-207">Search for hello following library names one by one.</span></span> <span data-ttu-id="15183-208">Az egyes tárak talált, kattintson a **telepítése**:</span><span class="sxs-lookup"><span data-stu-id="15183-208">For each library that you find, click **Install**:</span></span>

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. <span data-ttu-id="15183-209">Telepítse manuálisan `Adafruit_WINC1500`.</span><span class="sxs-lookup"><span data-stu-id="15183-209">Manually install `Adafruit_WINC1500`.</span></span> <span data-ttu-id="15183-210">Nyissa meg túl[ezen a webhelyen](https://github.com/adafruit/Adafruit_WINC1500) kattintson **Klónozás vagy letöltési** > **töltse le a ZIP-**.</span><span class="sxs-lookup"><span data-stu-id="15183-210">Go too[this website](https://github.com/adafruit/Adafruit_WINC1500) and click **Clone or download** > **Download ZIP**.</span></span> <span data-ttu-id="15183-211">A Arduino ide folytassa túl**vázlat** > **közé tartozik könyvtár** > **.zip könyvtár hozzáadása** , és adja hozzá a hello zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="15183-211">Then in your Arduino IDE, go too**Sketch** > **Include Library** > **Add .zip Library** and add hello zip file.</span></span>

### <a name="use-hello-sample-application-if-you-dont-have-a-real-bme280-sensor"></a><span data-ttu-id="15183-212">Hello mintaalkalmazás használja, ha nincs valós BME280 érzékelő</span><span class="sxs-lookup"><span data-stu-id="15183-212">Use hello sample application if you don’t have a real BME280 sensor</span></span>

<span data-ttu-id="15183-213">Ha nincs valós BME280 érzékelő, hello mintaalkalmazás szimulálhatja hőmérséklet és a páratartalom adatokat.</span><span class="sxs-lookup"><span data-stu-id="15183-213">If you don’t have a real BME280 sensor, hello sample application can simulate temperature and humidity data.</span></span> <span data-ttu-id="15183-214">tooset hello minta alkalmazás szimulált toouse adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15183-214">tooset up hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="15183-215">Nyissa meg hello `config.h` hello fájlban `app` mappát.</span><span class="sxs-lookup"><span data-stu-id="15183-215">Open hello `config.h` file in hello `app` folder.</span></span>

2. <span data-ttu-id="15183-216">Keresse meg a következő kódsort hello, és módosítsa hello értéket `false` túl`true`:</span><span class="sxs-lookup"><span data-stu-id="15183-216">Locate hello following line of code and change hello value from `false` too`true`:</span></span>

   ```c
   define SIMULATED_DATA true
   ```
   ![Hello mintaadatok alkalmazás szimulált toouse konfigurálása](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. <span data-ttu-id="15183-218">Mentse a fájlt hello `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="15183-218">Save hello file with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toofeather-m0-wifi"></a><span data-ttu-id="15183-219">Hello minta alkalmazás tooFeather M0 Wi-Fi telepítése</span><span class="sxs-lookup"><span data-stu-id="15183-219">Deploy hello sample application tooFeather M0 WiFi</span></span>

1. <span data-ttu-id="15183-220">Kattintson Arduino IDE hello, **eszköz** > **Port**, majd kattintson a lágyított M0 Wi-Fi hello soros port.</span><span class="sxs-lookup"><span data-stu-id="15183-220">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Feather M0 WiFi.</span></span>

2. <span data-ttu-id="15183-221">Kattintson a **vázlat** > **feltöltése** toobuild és hello minta alkalmazás tooFeather M0 Wi-Fi telepítése.</span><span class="sxs-lookup"><span data-stu-id="15183-221">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooFeather M0 WiFi.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="15183-222">Adja meg hitelesítő adatait</span><span class="sxs-lookup"><span data-stu-id="15183-222">Enter your credentials</span></span>

<span data-ttu-id="15183-223">Ha sikeresen befejeződött a feltöltés hello, kövesse ezeket a lépéseket tooenter a hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="15183-223">After hello upload completes successfully, follow these steps tooenter your credentials:</span></span>

1. <span data-ttu-id="15183-224">Kattintson Arduino IDE hello, **eszközök** > **soros figyelő**.</span><span class="sxs-lookup"><span data-stu-id="15183-224">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>

2. <span data-ttu-id="15183-225">Hello soros figyelő ablak hello jobb alsó sarkában, válassza ki a **nincs sor befejezési** hello bal oldali hello legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="15183-225">In hello lower-right corner of hello serial monitor window, select **No line ending** in hello drop-down list on hello left.</span></span>
3. <span data-ttu-id="15183-226">Válassza ki **115200 átviteli** hello legördülő listában a megfelelő hello.</span><span class="sxs-lookup"><span data-stu-id="15183-226">Select **115200 baud** in hello drop-down list on hello right.</span></span>
4. <span data-ttu-id="15183-227">Hello hello felső beviteli mezőbe, írja be a következő adatokat, ha Ön hello tooprovide kéri, majd kattintson **küldése**:</span><span class="sxs-lookup"><span data-stu-id="15183-227">In hello input box at hello top, enter hello following information if you're asked tooprovide it, and click **Send**:</span></span>

   * <span data-ttu-id="15183-228">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="15183-228">Wi-Fi SSID</span></span>
   * <span data-ttu-id="15183-229">Wi-Fi jelszó</span><span class="sxs-lookup"><span data-stu-id="15183-229">Wi-Fi password</span></span>
   * <span data-ttu-id="15183-230">Eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="15183-230">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="15183-231">hello hitelesítő adatok hello EEPROM a lágyított M0 Wi-Fi tárolódik.</span><span class="sxs-lookup"><span data-stu-id="15183-231">hello credential information is stored in hello EEPROM of Feather M0 WiFi.</span></span> <span data-ttu-id="15183-232">Ha hello lágyított M0 Wi-Fi board hello Visszaállítás gombra kattint, hello mintaalkalmazás megkérdezi tooerase hello információkat.</span><span class="sxs-lookup"><span data-stu-id="15183-232">If you click hello reset button on hello Feather M0 WiFi board, hello sample application asks if you want tooerase hello information.</span></span> <span data-ttu-id="15183-233">Adja meg `Y` tooerase hello információkat.</span><span class="sxs-lookup"><span data-stu-id="15183-233">Enter `Y` tooerase hello information.</span></span> <span data-ttu-id="15183-234">Megkérdezi, hogy tooprovide hello információ még egyszer.</span><span class="sxs-lookup"><span data-stu-id="15183-234">You're asked tooprovide hello information a second time.</span></span>

### <a name="verify-that-hello-sample-application-is-running-successfully"></a><span data-ttu-id="15183-235">Győződjön meg arról, hogy a hello mintaalkalmazás sikeresen fut.</span><span class="sxs-lookup"><span data-stu-id="15183-235">Verify that hello sample application is running successfully</span></span>

<span data-ttu-id="15183-236">Ha a hello soros figyelő ablakból parancskimenet hello és hello villogó LED a lágyított M0 Wi-Fi, hello mintaalkalmazás sikeresen fut:</span><span class="sxs-lookup"><span data-stu-id="15183-236">If you see hello following output from hello serial monitor window and hello blinking LED on Feather M0 WiFi, hello sample application is running successfully:</span></span>

![Végső kimenetet a Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="15183-238">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15183-238">Next steps</span></span>

<span data-ttu-id="15183-239">Sikeresen csatlakoztatva lágyított M0 Wi-Fi tooyour IoT hub és rögzített hello érzékelő adatokat tooyour IoT-központ küldött.</span><span class="sxs-lookup"><span data-stu-id="15183-239">You have successfully connected Feather M0 WiFi tooyour IoT hub and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

