---
title: "A felhőalapú m0: lágyított M0 Wi-Fi csatlakozni az Azure IoT Hub |} Microsoft Docs"
description: "Megtudhatja, hogyan állítson be és Adafruit lágyított M0 Wi-Fi csatlakozni az Azure IoT Hub adatokat küldeni az Azure felhőalapú platform ebben az oktatóanyagban."
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
ms.openlocfilehash: 0dcf6b46a4c6c743c713d24ce7844e801b278dcf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="3f5f3-103">Csatlakozás Adafruit lágyított M0 Wi-Fi Azure IoT Hub a felhőben</span><span class="sxs-lookup"><span data-stu-id="3f5f3-103">Connect Adafruit Feather M0 WiFi to Azure IoT Hub in the cloud</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Egy BME280 lágyított M0 Wi-Fi és az IoT-központ közötti kapcsolat](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

<span data-ttu-id="3f5f3-105">Ebben az oktatóanyagban akkor először tanulás alapjainak a Arduino board használatifeltétel.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-105">In this tutorial, you begin by learning the basics of working with your Arduino board.</span></span> <span data-ttu-id="3f5f3-106">Majd megtudhatja, hogyan kapcsolódhat zökkenőmentesen az eszközök a felhőbe [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="3f5f3-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="3f5f3-107">Mit</span><span class="sxs-lookup"><span data-stu-id="3f5f3-107">What you do</span></span>

<span data-ttu-id="3f5f3-108">Az IoT-központ az Ön által létrehozott Adafruit lágyított M0 Wi-Fi csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-108">Connect Adafruit Feather M0 WiFi to an IoT hub that you create.</span></span> <span data-ttu-id="3f5f3-109">Majd futtassa a mintaalkalmazást a hőmérséklet és a páratartalom adatokat gyűjteni a BME280 M0 Wi-Fi a.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-109">Then you run a sample application on M0 WiFi to collect the temperature and humidity data from a BME280.</span></span> <span data-ttu-id="3f5f3-110">Végezetül az érzékelő adatokat küldött az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-110">Finally, you send the sensor data to your IoT hub.</span></span>


## <a name="what-you-learn"></a><span data-ttu-id="3f5f3-111">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="3f5f3-111">What you learn</span></span>

* <span data-ttu-id="3f5f3-112">Létrehoz egy IoT-központot, és lágyított M0 Wi-Fi az eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="3f5f3-112">How to create an IoT hub and register a device for Feather M0 WiFi</span></span>
* <span data-ttu-id="3f5f3-113">Wi-Fi M0 lágyított az érzékelő és a számítógép összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="3f5f3-113">How to connect Feather M0 WiFi with the sensor and your computer</span></span>
* <span data-ttu-id="3f5f3-114">Futtatja a mintaalkalmazás lágyított M0 Wi-Fi érzékelői adatok gyűjtéséről</span><span class="sxs-lookup"><span data-stu-id="3f5f3-114">How to collect sensor data by running a sample application on Feather M0 WiFi</span></span>
* <span data-ttu-id="3f5f3-115">Útmutató az érzékelő adatokat küldeni az IoT hub</span><span class="sxs-lookup"><span data-stu-id="3f5f3-115">How to send the sensor data to your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="3f5f3-116">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="3f5f3-116">What you need</span></span>

![Az oktatóanyaghoz szükség részei](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="3f5f3-118">A művelet végrehajtásához a következő részek a lágyított M0 Wi-Fi Starter Kit kell:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-118">To complete this operation, you need the following parts from your Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="3f5f3-119">A Wi-Fi M0 lágyított tábla</span><span class="sxs-lookup"><span data-stu-id="3f5f3-119">The Feather M0 WiFi board</span></span>
* <span data-ttu-id="3f5f3-120">A típus egy USB-kábel Micro USB</span><span class="sxs-lookup"><span data-stu-id="3f5f3-120">A Micro USB to Type A USB cable</span></span>

<span data-ttu-id="3f5f3-121">A fejlesztési környezetet is kell a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-121">You also need the following things for your development environment:</span></span>

* <span data-ttu-id="3f5f3-122">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-122">An active Azure subscription.</span></span> <span data-ttu-id="3f5f3-123">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-123">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="3f5f3-124">A Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-124">A Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="3f5f3-125">Vezeték nélküli hálózat lágyított M0 Wi-Fi való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-125">A wireless network for Feather M0 WiFi to connect to.</span></span>
* <span data-ttu-id="3f5f3-126">A kiszolgálókonfigurációs eszköz letöltéséhez internetkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-126">An Internet connection to download the configuration tool.</span></span>
* <span data-ttu-id="3f5f3-127">[Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-127">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="3f5f3-128">Az Azure IoT Hub könyvtár korábbi verziói nem működik.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-128">Earlier versions don't work with the Azure IoT Hub library.</span></span>

<span data-ttu-id="3f5f3-129">Ha az érzékelő nincs, a következő elemek nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-129">If you don’t have a sensor, the following items are optional.</span></span> <span data-ttu-id="3f5f3-130">Akkor is szimulált érzékelőadatait használatát:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-130">You also have the option of using simulated sensor data:</span></span>

* <span data-ttu-id="3f5f3-131">BME280 hőmérséklet és a páratartalom érzékelő</span><span class="sxs-lookup"><span data-stu-id="3f5f3-131">A BME280 temperature and humidity sensor</span></span>
* <span data-ttu-id="3f5f3-132">Egy breadboard</span><span class="sxs-lookup"><span data-stu-id="3f5f3-132">A breadboard</span></span>
* <span data-ttu-id="3f5f3-133">M/M átkötés fenyegetéseknek</span><span class="sxs-lookup"><span data-stu-id="3f5f3-133">M/M jumper wires</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a><span data-ttu-id="3f5f3-134">Csatlakozás lágyított M0 Wi-Fi az érzékelő és a számítógép</span><span class="sxs-lookup"><span data-stu-id="3f5f3-134">Connect Feather M0 WiFi with the sensor and your computer</span></span>
<span data-ttu-id="3f5f3-135">Ebben a szakaszban csatlakozhat az érzékelők a tábla.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-135">In this section, you connect the sensors to your board.</span></span> <span data-ttu-id="3f5f3-136">Majd a eszközt csatlakoztat a számítógéphez további használatra.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-136">Then you plug in your device to your computer for further use.</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a><span data-ttu-id="3f5f3-137">Csatlakozás DHT22 hőmérséklet és a páratartalom érzékelő lágyított M0 WiFi</span><span class="sxs-lookup"><span data-stu-id="3f5f3-137">Connect a DHT22 temperature and humidity sensor to Feather M0 WiFi</span></span>

<span data-ttu-id="3f5f3-138">Használják a breadboard és átkötés a kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-138">Use the breadboard and jumper wires to make the connection.</span></span> <span data-ttu-id="3f5f3-139">Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-139">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Kapcsolatok referencia](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


<span data-ttu-id="3f5f3-141">PIN-kód érzékelő használja a következő vezetékezést:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-141">For sensor pins, use the following wiring:</span></span>


| <span data-ttu-id="3f5f3-142">Kezdő (érzékelő)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-142">Start (sensor)</span></span>           | <span data-ttu-id="3f5f3-143">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-143">End (board)</span></span>            | <span data-ttu-id="3f5f3-144">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="3f5f3-144">Cable color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="3f5f3-145">VDD (PIN-kód 27A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-145">VDD (Pin 27A)</span></span>            | <span data-ttu-id="3f5f3-146">3V (PIN-kód 3A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-146">3V (Pin 3A)</span></span>            | <span data-ttu-id="3f5f3-147">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="3f5f3-147">Red cable</span></span>     |
| <span data-ttu-id="3f5f3-148">GND (PIN-kód 29A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-148">GND (Pin 29A)</span></span>            | <span data-ttu-id="3f5f3-149">GND [PIN-kód 6]</span><span class="sxs-lookup"><span data-stu-id="3f5f3-149">GND (Pin 6A)</span></span>           | <span data-ttu-id="3f5f3-150">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="3f5f3-150">Black cable</span></span>   |
| <span data-ttu-id="3f5f3-151">SCK (PIN-kód 30A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-151">SCK (Pin 30A)</span></span>            | <span data-ttu-id="3f5f3-152">SCK (PIN-kód 12A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-152">SCK (Pin 12A)</span></span>          | <span data-ttu-id="3f5f3-153">Sárga kábel</span><span class="sxs-lookup"><span data-stu-id="3f5f3-153">Yellow cable</span></span>  |
| <span data-ttu-id="3f5f3-154">SDO (PIN-kód 31A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-154">SDO (Pin 31A)</span></span>            | <span data-ttu-id="3f5f3-155">MI (PIN-kód 14A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-155">MI (Pin 14A)</span></span>           | <span data-ttu-id="3f5f3-156">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="3f5f3-156">White cable</span></span>   |
| <span data-ttu-id="3f5f3-157">SDI (PIN-kód 32A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-157">SDI (Pin 32A)</span></span>            | <span data-ttu-id="3f5f3-158">M0 (PIN-kód 13A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-158">M0 (Pin 13A)</span></span>           | <span data-ttu-id="3f5f3-159">Kék kábel</span><span class="sxs-lookup"><span data-stu-id="3f5f3-159">Blue cable</span></span>    |
| <span data-ttu-id="3f5f3-160">CS (PIN-kód 33A)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-160">CS (Pin 33A)</span></span>             | <span data-ttu-id="3f5f3-161">GPIO 5 (PIN-kód 15J)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-161">GPIO 5 (Pin 15J)</span></span>       | <span data-ttu-id="3f5f3-162">Narancssárga kábel</span><span class="sxs-lookup"><span data-stu-id="3f5f3-162">Orange cable</span></span>  |

<span data-ttu-id="3f5f3-163">További információkért lásd: [Adafruit BME280 páratartalom + légnyomás + hőmérséklet-érzékelő kitörése](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) és [Adafruit lágyított M0 Wi-Fi érintkezőkiosztása szerepel](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span><span class="sxs-lookup"><span data-stu-id="3f5f3-163">For more information, see [Adafruit BME280 Humidity + Barometric Pressure + Temperature Sensor Breakout](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) and [Adafruit Feather M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span></span>



<span data-ttu-id="3f5f3-164">Most már a lágyított M0 Wi-Fi kell csatlakoztatni a működő érzékelő.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-164">Now your Feather M0 WiFi should be connected with a working sensor.</span></span>

![Csatlakozás DHT22 lágyított Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a><span data-ttu-id="3f5f3-166">Wi-Fi M0 lágyított kapcsolódni a számítógéphez</span><span class="sxs-lookup"><span data-stu-id="3f5f3-166">Connect Feather M0 WiFi to your computer</span></span>

<span data-ttu-id="3f5f3-167">A típus egy USB-kábel Micro USB való csatlakozáskor használandó lágyított M0 Wi-Fi a számítógép látható módon:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-167">Use the Micro USB to Type A USB cable to connect Feather M0 WiFi to your computer, as shown:</span></span>

![Lágyított Huzzah kapcsolódni a számítógéphez](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="3f5f3-169">Adja hozzá a soros port engedélyek (csak Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="3f5f3-169">Add serial port permissions (Ubuntu only)</span></span>

<span data-ttu-id="3f5f3-170">Ha Ubuntu használ, győződjön meg arról a engedélye ahhoz, hogy a lágyított M0 WiFi USB port működik.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-170">If you use Ubuntu, make sure you have the permissions to operate on the USB port of Feather M0 WiFi.</span></span> <span data-ttu-id="3f5f3-171">Soros port engedélyek hozzáadásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-171">To add serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="3f5f3-172">A terminálon futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-172">At a terminal, run the following commands:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="3f5f3-173">A következő kimenetek egyik beolvasása:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-173">You get one of the following outputs:</span></span>

   * <span data-ttu-id="3f5f3-174">crw-rw---1 legfelső szintű uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="3f5f3-174">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="3f5f3-175">crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="3f5f3-175">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="3f5f3-176">Figyelje meg, hogy a kimenet `uucp` vagy `dialout` a csoport tulajdonosának neve az USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-176">In the output, notice that `uucp` or `dialout` is the group owner name of the USB port.</span></span>

2. <span data-ttu-id="3f5f3-177">A felhasználó hozzáadása a csoporthoz, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-177">To add the user to the group, run the following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="3f5f3-178">Az előző lépésben beszerzett a csoport tulajdonosának neve `<group-owner-name>`.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-178">In the previous step, you obtained the group owner name `<group-owner-name>`.</span></span> <span data-ttu-id="3f5f3-179">Ubuntu felhasználónév `<username>`.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-179">Your Ubuntu user name is `<username>`.</span></span>

3. <span data-ttu-id="3f5f3-180">A módosítás jelenik meg jelentkezzen ki Ubuntu, és jelentkezzen be újból.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-180">For the change to appear, sign out of Ubuntu and then sign in again.</span></span>

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a><span data-ttu-id="3f5f3-181">Érzékelő adatokat gyűjteni, és küldje el az IoT hub</span><span class="sxs-lookup"><span data-stu-id="3f5f3-181">Collect sensor data and send it to your IoT hub</span></span>

<span data-ttu-id="3f5f3-182">Ebben a szakaszban telepítése, és futtassa a mintaalkalmazást a lágyított M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-182">In this section, you deploy and run a sample application on Feather M0 WiFi.</span></span> <span data-ttu-id="3f5f3-183">A mintaalkalmazás az LED villogási lágyított M0 Wi-Fi teszi.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-183">The sample application makes the LED blink on Feather M0 WiFi.</span></span> <span data-ttu-id="3f5f3-184">Ezután elküldi a hőmérséklet és a páratartalom gyűjtött adatokat a BME280 érzékelő az IoT hubhoz is.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-184">It then sends the temperature and humidity data collected from the BME280 sensor to your IoT hub.</span></span>

### <a name="get-the-sample-application-from-github-and-prepare-the-arduino-ide"></a><span data-ttu-id="3f5f3-185">A mintaalkalmazás beszerzése a Githubról, és készítse elő a Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="3f5f3-185">Get the sample application from GitHub and prepare the Arduino IDE</span></span>

<span data-ttu-id="3f5f3-186">A mintaalkalmazás tárolja a Githubon.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-186">The sample application is hosted on GitHub.</span></span> <span data-ttu-id="3f5f3-187">Klónozza a minta-tárház, amely tartalmazza a mintaalkalmazást a Githubról.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-187">Clone the sample repository that contains the sample application from GitHub.</span></span> <span data-ttu-id="3f5f3-188">A minta-tárház klónozása, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-188">To clone the sample repository, follow these steps:</span></span>

1. <span data-ttu-id="3f5f3-189">Nyisson meg egy parancssort vagy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-189">Open a command prompt or a terminal window.</span></span>

2. <span data-ttu-id="3f5f3-190">Nyissa meg a mappát, ahol a mintaalkalmazáshoz történő tárolását.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-190">Go to a folder where you want the sample application to be stored.</span></span>
3. <span data-ttu-id="3f5f3-191">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-191">Run the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-the-package-for-feather-m0-wifi-in-the-arduino-ide"></a><span data-ttu-id="3f5f3-192">A csomag telepítése lágyított M0 Wi-Fi a Arduino ide</span><span class="sxs-lookup"><span data-stu-id="3f5f3-192">Install the package for Feather M0 WiFi in the Arduino IDE</span></span>

1. <span data-ttu-id="3f5f3-193">Nyissa meg a mappát, ahol a mintaalkalmazás tárolja.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-193">Open the folder where the sample application is stored.</span></span>

2. <span data-ttu-id="3f5f3-194">Nyissa meg a app.ino fájlt a Arduino ide app mappában.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-194">Open the app.ino file in the app folder in the Arduino IDE.</span></span>

   ![Nyissa meg a mintaalkalmazás Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. <span data-ttu-id="3f5f3-196">Kattintson a **fájl** > **beállítások** (Windows/Linux) vagy **Arduino** > **beállítások** (Mac), másolja és illessze be az alábbi hivatkozásra a **további modulok Manager URL-címet** a Arduino IDE beállítások lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-196">Click **File** > **Preferences** (Windows/Linux) or **Arduino** > **Preferences** (Mac) and copy and paste the link below into the **Additional Boards Manager URLs** option in the Arduino IDE preferences.</span></span>
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. <span data-ttu-id="3f5f3-197">Kattintson a **eszközök** > **Board** > **modulok Manager**, majd telepítse a `Arduino SAMD Boards` verzió `1.6.2` vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-197">Click **Tools** > **Board** > **Boards Manager**, and then install the `Arduino SAMD Boards` version `1.6.2` or later.</span></span> 

1. <span data-ttu-id="3f5f3-198">Az azonos ablakban telepítse `Adafruit SAMD Boards` csomag hozzáadása a tábla fájl definíciókat.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-198">Then in the same window, install `Adafruit SAMD Boards` package to add the board file definitions.</span></span>

   ![A esp8266 csomag telepítve van](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. <span data-ttu-id="3f5f3-200">Kattintson a **eszközök** > **Board** > **Adafruit M0 Wi-Fi**.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-200">Click **Tools** > **Board** > **Adafruit M0 WiFi**.</span></span>

5. <span data-ttu-id="3f5f3-201">Illesztőprogramok telepítéséhez (csak Windows).</span><span class="sxs-lookup"><span data-stu-id="3f5f3-201">Install drivers (for Windows only).</span></span> <span data-ttu-id="3f5f3-202">Csatlakoztatásakor lágyított M0 Wi-Fi, szükség lehet illesztőprogramot telepíteni.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-202">When you plug in Feather M0 WiFi, you might need to install a driver.</span></span> <span data-ttu-id="3f5f3-203">Kattintson a [a letöltési hivatkozás a weblapon](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) az illesztőprogram-telepítő letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-203">Click [the download link on the webpage](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) to download the driver installer.</span></span> <span data-ttu-id="3f5f3-204">Kövesse a kívánt illesztőprogramok telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-204">Follow the steps to install the drivers you want.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="3f5f3-205">Szükséges kódtárak telepítése</span><span class="sxs-lookup"><span data-stu-id="3f5f3-205">Install necessary libraries</span></span>

1. <span data-ttu-id="3f5f3-206">Kattintson a Arduino ide **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-206">In the Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>

2. <span data-ttu-id="3f5f3-207">Keresse meg a következő könyvtár nevek egyenként.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-207">Search for the following library names one by one.</span></span> <span data-ttu-id="3f5f3-208">Az egyes tárak talált, kattintson a **telepítése**:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-208">For each library that you find, click **Install**:</span></span>

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. <span data-ttu-id="3f5f3-209">Telepítse manuálisan `Adafruit_WINC1500`.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-209">Manually install `Adafruit_WINC1500`.</span></span> <span data-ttu-id="3f5f3-210">Nyissa meg a [ezen a webhelyen](https://github.com/adafruit/Adafruit_WINC1500) kattintson **Klónozás vagy letöltési** > **töltse le a ZIP-**.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-210">Go to [this website](https://github.com/adafruit/Adafruit_WINC1500) and click **Clone or download** > **Download ZIP**.</span></span> <span data-ttu-id="3f5f3-211">A Arduino ide, folytassa a **vázlat** > **közé tartozik könyvtár** > **.zip könyvtár hozzáadása** , és adja hozzá a zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-211">Then in your Arduino IDE, go to **Sketch** > **Include Library** > **Add .zip Library** and add the zip file.</span></span>

### <a name="use-the-sample-application-if-you-dont-have-a-real-bme280-sensor"></a><span data-ttu-id="3f5f3-212">A mintaalkalmazás használja, ha nincs valós BME280 érzékelő</span><span class="sxs-lookup"><span data-stu-id="3f5f3-212">Use the sample application if you don’t have a real BME280 sensor</span></span>

<span data-ttu-id="3f5f3-213">Ha egy valódi BME280 érzékelő nincs, a mintaalkalmazást szimulálhatja hőmérséklet és a páratartalom adatok.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-213">If you don’t have a real BME280 sensor, the sample application can simulate temperature and humidity data.</span></span> <span data-ttu-id="3f5f3-214">Állítsa be a mintaalkalmazást szimulált adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-214">To set up the sample application to use simulated data, follow these steps:</span></span>

1. <span data-ttu-id="3f5f3-215">Nyissa meg a `config.h` fájlt a `app` mappát.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-215">Open the `config.h` file in the `app` folder.</span></span>

2. <span data-ttu-id="3f5f3-216">Keresse meg a következő kódsort, és módosítsa az értéket `false` való `true`:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-216">Locate the following line of code and change the value from `false` to `true`:</span></span>

   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurálja a mintaalkalmazás szimulált adatok használata](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. <span data-ttu-id="3f5f3-218">Mentse a fájlt a `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-218">Save the file with `Control-s`.</span></span>

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a><span data-ttu-id="3f5f3-219">A mintaalkalmazás az lágyított M0 Wi-Fi telepítése</span><span class="sxs-lookup"><span data-stu-id="3f5f3-219">Deploy the sample application to Feather M0 WiFi</span></span>

1. <span data-ttu-id="3f5f3-220">Kattintson a Arduino ide **eszköz** > **Port**, majd kattintson a soros port a lágyított M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-220">In the Arduino IDE, click **Tool** > **Port**, and then click the serial port for Feather M0 WiFi.</span></span>

2. <span data-ttu-id="3f5f3-221">Kattintson a **vázlat** > **feltöltése** felépítéséhez és az lágyított M0 Wi-Fi minta alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-221">Click **Sketch** > **Upload** to build and deploy the sample application to Feather M0 WiFi.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="3f5f3-222">Adja meg hitelesítő adatait</span><span class="sxs-lookup"><span data-stu-id="3f5f3-222">Enter your credentials</span></span>

<span data-ttu-id="3f5f3-223">Ha sikeresen befejeződött a feltöltés, kövesse az alábbi lépéseket a hitelesítő adatok megadását:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-223">After the upload completes successfully, follow these steps to enter your credentials:</span></span>

1. <span data-ttu-id="3f5f3-224">Kattintson a Arduino ide **eszközök** > **soros figyelő**.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-224">In the Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>

2. <span data-ttu-id="3f5f3-225">Válassza ki a soros figyelő ablak jobb alsó sarkában **nincs sor befejezési** a legördülő listában, a bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-225">In the lower-right corner of the serial monitor window, select **No line ending** in the drop-down list on the left.</span></span>
3. <span data-ttu-id="3f5f3-226">Válassza ki **115200 átviteli** jobb legördülő listáról.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-226">Select **115200 baud** in the drop-down list on the right.</span></span>
4. <span data-ttu-id="3f5f3-227">A beviteli mezőbe, a bal felső, adja meg az az alábbi adatokat, ha megkérdezi, adja meg, és kattintson **küldése**:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-227">In the input box at the top, enter the following information if you're asked to provide it, and click **Send**:</span></span>

   * <span data-ttu-id="3f5f3-228">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="3f5f3-228">Wi-Fi SSID</span></span>
   * <span data-ttu-id="3f5f3-229">Wi-Fi jelszó</span><span class="sxs-lookup"><span data-stu-id="3f5f3-229">Wi-Fi password</span></span>
   * <span data-ttu-id="3f5f3-230">Eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="3f5f3-230">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="3f5f3-231">A hitelesítő adatokat a lágyított EEPROM M0 WiFi tárolódik.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-231">The credential information is stored in the EEPROM of Feather M0 WiFi.</span></span> <span data-ttu-id="3f5f3-232">Ha a Wi-Fi M0 lágyított táblán a Visszaállítás gombra kattint, a mintaalkalmazást megkérdezi, hogy szeretné-e használni a.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-232">If you click the reset button on the Feather M0 WiFi board, the sample application asks if you want to erase the information.</span></span> <span data-ttu-id="3f5f3-233">Adja meg `Y` törlésére az adatokat.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-233">Enter `Y` to erase the information.</span></span> <span data-ttu-id="3f5f3-234">Megkérdezi, hogy az adatok még egyszer.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-234">You're asked to provide the information a second time.</span></span>

### <a name="verify-that-the-sample-application-is-running-successfully"></a><span data-ttu-id="3f5f3-235">Győződjön meg arról, hogy a mintaalkalmazás sikeresen fut.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-235">Verify that the sample application is running successfully</span></span>

<span data-ttu-id="3f5f3-236">Ha látható a következő kimeneti a soros figyelő ablakból és a villogó LED lágyított M0 Wi-Fi, a mintaalkalmazás sikeresen fut:</span><span class="sxs-lookup"><span data-stu-id="3f5f3-236">If you see the following output from the serial monitor window and the blinking LED on Feather M0 WiFi, the sample application is running successfully:</span></span>

![Végső kimenetet a Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="3f5f3-238">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f5f3-238">Next steps</span></span>

<span data-ttu-id="3f5f3-239">Sikeresen csatlakoztatva az IoT hub lágyított M0 Wi-Fi és a rögzített érzékelő adatokat küldött az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3f5f3-239">You have successfully connected Feather M0 WiFi to your IoT hub and sent the captured sensor data to your IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

