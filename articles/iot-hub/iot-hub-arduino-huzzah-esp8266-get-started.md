---
title: "Felhőbe - ESP8266 lágyított HUZZAH ESP8266 csatlakozni az Azure IoT Hub |} Microsoft Docs"
description: "Megtudhatja, hogyan kell beállítania, és Azure IoT-központ az adatokat küldeni az Azure felhőalapú platform ebben az oktatóanyagban Adafruit lágyított HUZZAH ESP8266 csatlakozni."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.openlocfilehash: 6a450579c848fe6030a328ddf410f139baae2324
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="2a904-103">Csatlakozás Adafruit lágyított HUZZAH ESP8266 Azure IoT Hub a felhőben</span><span class="sxs-lookup"><span data-stu-id="2a904-103">Connect Adafruit Feather HUZZAH ESP8266 to Azure IoT Hub in the cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 lágyított HUZZAH ESP8266 és az IoT-központ közötti kapcsolat](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a><span data-ttu-id="2a904-105">Mit</span><span class="sxs-lookup"><span data-stu-id="2a904-105">What you do</span></span>


<span data-ttu-id="2a904-106">Az IoT-központ az Ön által létrehozott Adafruit lágyított HUZZAH ESP8266 csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="2a904-106">Connect Adafruit Feather HUZZAH ESP8266 to an IoT hub that you create.</span></span> <span data-ttu-id="2a904-107">Majd futtassa a mintaalkalmazást a ESP8266 DHT22 érzékelő a hőmérséklet és a páratartalom adatokat gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="2a904-107">Then you run a sample application on ESP8266 to collect the temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="2a904-108">Végezetül az érzékelő adatokat küldött az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2a904-108">Finally, you send the sensor data to your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="2a904-109">Más ESP8266 modulok használata, akkor is továbbra is kövesse az alábbi lépéseket az IoT hub csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="2a904-109">If you're using other ESP8266 boards, you can still follow these steps to connect it to your IoT hub.</span></span> <span data-ttu-id="2a904-110">Attól függően, hogy a ESP8266 üzenőfalon használ, akkor módosítania kell, hogy engedélyezzék a `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="2a904-110">Depending on the ESP8266 board you're using, you might need to reconfigure the `LED_PIN`.</span></span> <span data-ttu-id="2a904-111">Például az Eszközintelligencia-Thinker ESP8266 használata, érdemes lehet módosítani az `0` való `2`.</span><span class="sxs-lookup"><span data-stu-id="2a904-111">For example, if you're using ESP8266 from AI-Thinker, you might change it from `0` to `2`.</span></span> <span data-ttu-id="2a904-112">Még nem rendelkezik egy csomagot?</span><span class="sxs-lookup"><span data-stu-id="2a904-112">Don't have a kit yet?</span></span> <span data-ttu-id="2a904-113">Az beszerzése a [Azure-webhelyen](http://azure.com/iotstarterkits).</span><span class="sxs-lookup"><span data-stu-id="2a904-113">Get it from the [Azure website](http://azure.com/iotstarterkits).</span></span>




## <a name="what-you-learn"></a><span data-ttu-id="2a904-114">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="2a904-114">What you learn</span></span>

* <span data-ttu-id="2a904-115">Létrehoz egy IoT-központot, és lágyított HUZZAH ESP8266 az eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="2a904-115">How to create an IoT hub and register a device for Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="2a904-116">Lágyított HUZZAH ESP8266 az érzékelő és a számítógép összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="2a904-116">How to connect Feather HUZZAH ESP8266 with the sensor and your computer</span></span>
* <span data-ttu-id="2a904-117">Futtatja a mintaalkalmazás lágyított HUZZAH ESP8266 érzékelő adatainak gyűjtéséről</span><span class="sxs-lookup"><span data-stu-id="2a904-117">How to collect sensor data by running a sample application on Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="2a904-118">Útmutató az érzékelő adatokat küldeni az IoT hub</span><span class="sxs-lookup"><span data-stu-id="2a904-118">How to send the sensor data to your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2a904-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="2a904-119">What you need</span></span>

![Az oktatóanyaghoz szükség részei](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="2a904-121">A művelet végrehajtásához a következő részek a lágyított HUZZAH ESP8266 Starter Kit kell:</span><span class="sxs-lookup"><span data-stu-id="2a904-121">To complete this operation, you need the following parts from your Feather HUZZAH ESP8266 Starter Kit:</span></span>

* <span data-ttu-id="2a904-122">A lágyított HUZZAH ESP8266 board</span><span class="sxs-lookup"><span data-stu-id="2a904-122">The Feather HUZZAH ESP8266 board</span></span>
* <span data-ttu-id="2a904-123">A típus egy USB-kábel Micro USB</span><span class="sxs-lookup"><span data-stu-id="2a904-123">A Micro USB to Type A USB cable</span></span>

<span data-ttu-id="2a904-124">A fejlesztési környezetet is kell a következőket:</span><span class="sxs-lookup"><span data-stu-id="2a904-124">You also need the following things for your development environment:</span></span>

* <span data-ttu-id="2a904-125">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2a904-125">An active Azure subscription.</span></span> <span data-ttu-id="2a904-126">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="2a904-126">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="2a904-127">Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.</span><span class="sxs-lookup"><span data-stu-id="2a904-127">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="2a904-128">Vezeték nélküli hálózat lágyított HUZZAH ESP8266 való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="2a904-128">Wireless network for Feather HUZZAH ESP8266 to connect to.</span></span>
* <span data-ttu-id="2a904-129">A kiszolgálókonfigurációs eszköz letöltéséhez internetkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="2a904-129">Internet connection to download the configuration tool.</span></span>
* <span data-ttu-id="2a904-130">[Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2a904-130">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="2a904-131">Korábbi verzióiban a AzureIoT szalagtár nem működik.</span><span class="sxs-lookup"><span data-stu-id="2a904-131">Earlier versions don't work with the AzureIoT library.</span></span>

<span data-ttu-id="2a904-132">A következő elemek nem kötelező, abban az esetben, ha nincs érzékelő.</span><span class="sxs-lookup"><span data-stu-id="2a904-132">The following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="2a904-133">Akkor is a szimulált érzékelőadatait használatának lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="2a904-133">You also have the option of using simulated sensor data.</span></span>

* <span data-ttu-id="2a904-134">Egy Adafruit DHT22 hőmérséklet és a páratartalom érzékelő</span><span class="sxs-lookup"><span data-stu-id="2a904-134">An Adafruit DHT22 temperature and humidity sensor</span></span>
* <span data-ttu-id="2a904-135">Egy breadboard</span><span class="sxs-lookup"><span data-stu-id="2a904-135">A breadboard</span></span>
* <span data-ttu-id="2a904-136">M/M átkötés fenyegetéseknek</span><span class="sxs-lookup"><span data-stu-id="2a904-136">M/M jumper wires</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a><span data-ttu-id="2a904-137">Csatlakozás lágyított HUZZAH ESP8266 az érzékelő és a számítógép</span><span class="sxs-lookup"><span data-stu-id="2a904-137">Connect Feather HUZZAH ESP8266 with the sensor and your computer</span></span>
<span data-ttu-id="2a904-138">Ebben a szakaszban csatlakozhat az érzékelők a tábla.</span><span class="sxs-lookup"><span data-stu-id="2a904-138">In this section, you connect the sensors to your board.</span></span> <span data-ttu-id="2a904-139">Majd a eszközt csatlakoztat a számítógéphez további használatra.</span><span class="sxs-lookup"><span data-stu-id="2a904-139">Then you plug in your device to your computer for further use.</span></span>
### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a><span data-ttu-id="2a904-140">Csatlakozás lágyított HUZZAH ESP8266 DHT22 hőmérséklet és a páratartalom érzékelő</span><span class="sxs-lookup"><span data-stu-id="2a904-140">Connect a DHT22 temperature and humidity sensor to Feather HUZZAH ESP8266</span></span>

<span data-ttu-id="2a904-141">Használják a breadboard és átkötés az alábbiak szerint a kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="2a904-141">Use the breadboard and jumper wires to make the connection as follows.</span></span> <span data-ttu-id="2a904-142">Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.</span><span class="sxs-lookup"><span data-stu-id="2a904-142">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Kapcsolatok referencia](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


<span data-ttu-id="2a904-144">PIN-kód érzékelő használja a következő vezetékezést:</span><span class="sxs-lookup"><span data-stu-id="2a904-144">For sensor pins, use the following wiring:</span></span>


| <span data-ttu-id="2a904-145">Indítsa el a (érzékelő)</span><span class="sxs-lookup"><span data-stu-id="2a904-145">Start (Sensor)</span></span>           | <span data-ttu-id="2a904-146">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="2a904-146">End (Board)</span></span>           | <span data-ttu-id="2a904-147">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="2a904-147">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="2a904-148">VDD (PIN-kód 31F)</span><span class="sxs-lookup"><span data-stu-id="2a904-148">VDD (Pin 31F)</span></span>            | <span data-ttu-id="2a904-149">3V (PIN-kód 58H)</span><span class="sxs-lookup"><span data-stu-id="2a904-149">3V (Pin 58H)</span></span>           | <span data-ttu-id="2a904-150">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="2a904-150">Red cable</span></span>     |
| <span data-ttu-id="2a904-151">ADATOK (PIN-kód 32F)</span><span class="sxs-lookup"><span data-stu-id="2a904-151">DATA (Pin 32F)</span></span>           | <span data-ttu-id="2a904-152">GPIO 2 (PIN-kód 46A)</span><span class="sxs-lookup"><span data-stu-id="2a904-152">GPIO 2 (Pin 46A)</span></span>       | <span data-ttu-id="2a904-153">Kék kábel</span><span class="sxs-lookup"><span data-stu-id="2a904-153">Blue cable</span></span>    |
| <span data-ttu-id="2a904-154">GND (PIN-kód 34F)</span><span class="sxs-lookup"><span data-stu-id="2a904-154">GND (Pin 34F)</span></span>            | <span data-ttu-id="2a904-155">GND (PIN-kód 56I)</span><span class="sxs-lookup"><span data-stu-id="2a904-155">GND (PIn 56I)</span></span>          | <span data-ttu-id="2a904-156">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="2a904-156">Black cable</span></span>   |

<span data-ttu-id="2a904-157">További információkért lásd: [Adafruit DHT22 érzékelő telepítő](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) és [Adafruit lágyított HUZZAH Esp8266 érintkezőkiosztása szerepel](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span><span class="sxs-lookup"><span data-stu-id="2a904-157">For more information, see [Adafruit DHT22 sensor setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) and [Adafruit Feather HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span></span>



<span data-ttu-id="2a904-158">Most már a lágyított Huzzah ESP8266 kell csatlakoztatni a működő érzékelő.</span><span class="sxs-lookup"><span data-stu-id="2a904-158">Now your Feather Huzzah ESP8266 should be connected with a working sensor.</span></span>

![Csatlakozás DHT22 lágyított Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a><span data-ttu-id="2a904-160">Lágyított HUZZAH ESP8266 kapcsolódni a számítógéphez</span><span class="sxs-lookup"><span data-stu-id="2a904-160">Connect Feather HUZZAH ESP8266 to your computer</span></span>

<span data-ttu-id="2a904-161">Látható tovább, a Micro USB,-típus egy USB-kábel segítségével lágyított HUZZAH ESP8266 kapcsolódni a számítógéphez.</span><span class="sxs-lookup"><span data-stu-id="2a904-161">As shown next, use the Micro USB to Type A USB cable to connect Feather HUZZAH ESP8266 to your computer.</span></span>

![Lágyított Huzzah kapcsolódni a számítógéphez](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="2a904-163">Adja hozzá a soros port engedélyek (csak Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="2a904-163">Add serial port permissions (Ubuntu only)</span></span>


<span data-ttu-id="2a904-164">Ha Ubuntu használ, győződjön meg arról a engedélye ahhoz, hogy a lágyított HUZZAH ESP8266 USB port működik.</span><span class="sxs-lookup"><span data-stu-id="2a904-164">If you use Ubuntu, make sure you have the permissions to operate on the USB port of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="2a904-165">Soros port engedélyek hozzáadásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2a904-165">To add serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="2a904-166">A következő parancsokat a terminálon:</span><span class="sxs-lookup"><span data-stu-id="2a904-166">Run the following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="2a904-167">A következő kimenetek egyik beolvasása:</span><span class="sxs-lookup"><span data-stu-id="2a904-167">You get one of the following outputs:</span></span>

   * <span data-ttu-id="2a904-168">crw-rw---1 legfelső szintű uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="2a904-168">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="2a904-169">crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="2a904-169">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="2a904-170">Figyelje meg, hogy a kimenet `uucp` vagy `dialout` a csoport tulajdonosának neve az USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="2a904-170">In the output, notice that `uucp` or `dialout` is the group owner name of the USB port.</span></span>

1. <span data-ttu-id="2a904-171">A felhasználó felvétele a csoportba a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2a904-171">Add the user to the group by running the following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="2a904-172">`<group-owner-name>`az az előző lépésben beolvasott tulajdonos neve van.</span><span class="sxs-lookup"><span data-stu-id="2a904-172">`<group-owner-name>` is the group owner name you obtained in the previous step.</span></span> <span data-ttu-id="2a904-173">`<username>`Ubuntu felhasználónevének van.</span><span class="sxs-lookup"><span data-stu-id="2a904-173">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="2a904-174">Ubuntu kijelentkezni, és jelentkezzen be újból a módosítás jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2a904-174">Sign out of Ubuntu, and then sign in again for the change to appear.</span></span>

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a><span data-ttu-id="2a904-175">Érzékelő adatokat gyűjteni, és küldje el az IoT hub</span><span class="sxs-lookup"><span data-stu-id="2a904-175">Collect sensor data and send it to your IoT hub</span></span>

<span data-ttu-id="2a904-176">Ebben a szakaszban telepítése, és futtassa a mintaalkalmazást lágyított HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="2a904-176">In this section, you deploy and run a sample application on Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="2a904-177">A mintaalkalmazás az lágyított HUZZAH ESP8266 LED villogjon, és elküldi a hőmérséklet és a páratartalom gyűjtött adatokat a DHT22 érzékelő az IoT hubhoz.</span><span class="sxs-lookup"><span data-stu-id="2a904-177">The sample application blinks the LED on Feather HUZZAH ESP8266, and sends the temperature and humidity data collected from the DHT22 sensor to your IoT hub.</span></span>

### <a name="get-the-sample-application-from-github"></a><span data-ttu-id="2a904-178">A mintaalkalmazás beszerzése a Githubról</span><span class="sxs-lookup"><span data-stu-id="2a904-178">Get the sample application from GitHub</span></span>

<span data-ttu-id="2a904-179">A mintaalkalmazás tárolja a Githubon.</span><span class="sxs-lookup"><span data-stu-id="2a904-179">The sample application is hosted on GitHub.</span></span> <span data-ttu-id="2a904-180">Klónozza a minta-tárház, amely tartalmazza a mintaalkalmazást a Githubról.</span><span class="sxs-lookup"><span data-stu-id="2a904-180">Clone the sample repository that contains the sample application from GitHub.</span></span> <span data-ttu-id="2a904-181">A minta-tárház klónozása, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2a904-181">To clone the sample repository, follow these steps:</span></span>

1. <span data-ttu-id="2a904-182">Nyisson meg egy parancssort vagy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="2a904-182">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="2a904-183">Nyissa meg a mappát, ahol a mintaalkalmazáshoz történő tárolását.</span><span class="sxs-lookup"><span data-stu-id="2a904-183">Go to a folder where you want the sample application to be stored.</span></span>
1. <span data-ttu-id="2a904-184">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="2a904-184">Run the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

<span data-ttu-id="2a904-185">A csomag telepítése lágyított HUZZAH ESP8266 a Arduino ide:</span><span class="sxs-lookup"><span data-stu-id="2a904-185">Install the package for Feather HUZZAH ESP8266 in the Arduino IDE:</span></span>

1. <span data-ttu-id="2a904-186">Nyissa meg a mappát, ahol a mintaalkalmazás tárolja.</span><span class="sxs-lookup"><span data-stu-id="2a904-186">Open the folder where the sample application is stored.</span></span>
1. <span data-ttu-id="2a904-187">Nyissa meg a app.ino fájlt a Arduino ide app mappában.</span><span class="sxs-lookup"><span data-stu-id="2a904-187">Open the app.ino file in the app folder in the Arduino IDE.</span></span>

   ![Nyissa meg a mintaalkalmazás Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="2a904-189">Kattintson a Arduino ide **fájl** > **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2a904-189">In the Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="2a904-190">Az a **beállítások** párbeszédpanel mellett kattintson a ikonra a **további modulok Manager URL-címet** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2a904-190">In the **Preferences** dialog box, click the icon next to the **Additional Boards Manager URLs** box.</span></span>
1. <span data-ttu-id="2a904-191">Az előugró ablakban írja be a következő URL-címet, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a904-191">In the pop-up window, enter the following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Mutasson a Arduino IDE egy alkalmazáscsomag URL-címe](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="2a904-193">Az a **preferencia** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a904-193">In the **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="2a904-194">Kattintson a **eszközök** > **Board** > **modulok Manager**, majd keresse meg a esp8266.</span><span class="sxs-lookup"><span data-stu-id="2a904-194">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>

   <span data-ttu-id="2a904-195">Modulok Manager, az azt jelenti, hogy telepítve van-e a ESP8266 2.2.0 vagy újabb verziójával.</span><span class="sxs-lookup"><span data-stu-id="2a904-195">Boards Manager indicates that ESP8266 with a version of 2.2.0 or later is installed.</span></span>

   ![A esp8266 csomag telepítve van](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="2a904-197">Kattintson a **eszközök** > **Board** > **Adafruit HUZZAH ESP8266**.</span><span class="sxs-lookup"><span data-stu-id="2a904-197">Click **Tools** > **Board** > **Adafruit HUZZAH ESP8266**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="2a904-198">Szükséges kódtárak telepítése</span><span class="sxs-lookup"><span data-stu-id="2a904-198">Install necessary libraries</span></span>

1. <span data-ttu-id="2a904-199">Kattintson a Arduino ide **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.</span><span class="sxs-lookup"><span data-stu-id="2a904-199">In the Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="2a904-200">Keresse meg a következő könyvtár nevek egyenként.</span><span class="sxs-lookup"><span data-stu-id="2a904-200">Search for the following library names one by one.</span></span> <span data-ttu-id="2a904-201">Az egyes tárak talált, kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="2a904-201">For each  library that you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="2a904-202">Nincs valós DHT22 érzékelő?</span><span class="sxs-lookup"><span data-stu-id="2a904-202">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="2a904-203">A mintaalkalmazás szimulálhatja a hőmérséklet és a páratartalom adatok, abban az esetben, ha nincs valós DHT22 érzékelő.</span><span class="sxs-lookup"><span data-stu-id="2a904-203">The sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="2a904-204">Állítsa be a mintaalkalmazást szimulált adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2a904-204">To set up the sample application to use simulated data, follow these steps:</span></span>

1. <span data-ttu-id="2a904-205">Nyissa meg a `config.h` fájlt a `app` mappát.</span><span class="sxs-lookup"><span data-stu-id="2a904-205">Open the `config.h` file in the `app` folder.</span></span>
1. <span data-ttu-id="2a904-206">Keresse meg a következő kódsort, és módosítsa az értéket `false` való `true`:</span><span class="sxs-lookup"><span data-stu-id="2a904-206">Locate the following line of code and change the value from `false` to `true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurálja a mintaalkalmazás szimulált adatok használata](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. <span data-ttu-id="2a904-208">Mentse a fájlt a `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="2a904-208">Save the file with `Control-s`.</span></span>

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a><span data-ttu-id="2a904-209">A mintaalkalmazás lágyított HUZZAH ESP8266 történő telepítése</span><span class="sxs-lookup"><span data-stu-id="2a904-209">Deploy the sample application to Feather HUZZAH ESP8266</span></span>

1. <span data-ttu-id="2a904-210">Kattintson a Arduino ide **eszköz** > **Port**, majd kattintson a soros port a lágyított HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="2a904-210">In the Arduino IDE, click **Tool** > **Port**, and then click the serial port for Feather HUZZAH ESP8266.</span></span>
1. <span data-ttu-id="2a904-211">Kattintson a **vázlat** > **feltöltése** felépítéséhez és az lágyított HUZZAH ESP8266 minta alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="2a904-211">Click **Sketch** > **Upload** to build and deploy the sample application to Feather HUZZAH ESP8266.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="2a904-212">Adja meg hitelesítő adatait</span><span class="sxs-lookup"><span data-stu-id="2a904-212">Enter your credentials</span></span>

<span data-ttu-id="2a904-213">Ha sikeresen befejeződött a feltöltés, kövesse az alábbi lépéseket a hitelesítő adatok megadását:</span><span class="sxs-lookup"><span data-stu-id="2a904-213">After the upload completes successfully, follow these steps to enter your credentials:</span></span>

1. <span data-ttu-id="2a904-214">Kattintson a Arduino ide **eszközök** > **soros figyelő**.</span><span class="sxs-lookup"><span data-stu-id="2a904-214">In the Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="2a904-215">A soros figyelő ablakban figyelje meg, a két legördülő lista jobb alsó sarkában.</span><span class="sxs-lookup"><span data-stu-id="2a904-215">In the serial monitor window, notice the two drop-down lists in the lower-right corner.</span></span>
1. <span data-ttu-id="2a904-216">Válassza ki **nincs sor befejezési** a bal oldali legördülő listát.</span><span class="sxs-lookup"><span data-stu-id="2a904-216">Select **No line ending** for the left drop-down list.</span></span>
1. <span data-ttu-id="2a904-217">Válassza ki **115200 átviteli** jobb legördülő listája.</span><span class="sxs-lookup"><span data-stu-id="2a904-217">Select **115200 baud** for the right drop-down list.</span></span>
1. <span data-ttu-id="2a904-218">A beviteli mezőbe, a soros figyelő ablak tetején található, adja meg az az alábbi adatokat, ha a rendszer kéri azokat, és kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="2a904-218">In the input box located at the top of the serial monitor window, enter the following information if you are asked to provide them, and then click **Send**.</span></span>
   * <span data-ttu-id="2a904-219">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="2a904-219">Wi-Fi SSID</span></span>
   * <span data-ttu-id="2a904-220">Wi-Fi jelszó</span><span class="sxs-lookup"><span data-stu-id="2a904-220">Wi-Fi password</span></span>
   * <span data-ttu-id="2a904-221">Eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a904-221">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="2a904-222">A hitelesítő adatokat a lágyított EEPROM HUZZAH ESP8266 tárolódik.</span><span class="sxs-lookup"><span data-stu-id="2a904-222">The credential information is stored in the EEPROM of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="2a904-223">Ha a lágyított HUZZAH ESP8266 táblán a Visszaállítás gombra kattint, a mintaalkalmazást megkérdezi, hogy szeretné-e használni a.</span><span class="sxs-lookup"><span data-stu-id="2a904-223">If you click the reset button on the Feather HUZZAH ESP8266 board, the sample application asks if you want to erase the information.</span></span> <span data-ttu-id="2a904-224">Adja meg `Y` az adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="2a904-224">Enter `Y` to have the information erased.</span></span> <span data-ttu-id="2a904-225">Adja meg az adatokat a második alkalommal kéri.</span><span class="sxs-lookup"><span data-stu-id="2a904-225">You are asked to provide the information a second time.</span></span>

### <a name="verify-the-sample-application-is-running-successfully"></a><span data-ttu-id="2a904-226">Ellenőrizze a mintaalkalmazás sikeresen fut-e</span><span class="sxs-lookup"><span data-stu-id="2a904-226">Verify the sample application is running successfully</span></span>

<span data-ttu-id="2a904-227">A következő kimeneti a soros figyelő ablakból és a villogó LED lágyított HUZZAH ESP8266 látható, ha sikeresen fut a mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2a904-227">If you see the following output from the serial monitor window and the blinking LED on Feather HUZZAH ESP8266, the sample application is running successfully.</span></span>

![Végső kimenetet a Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="2a904-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a904-229">Next steps</span></span>

<span data-ttu-id="2a904-230">Sikeresen egy lágyított HUZZAH ESP8266 csatlakozik az IoT hub, és a rögzített érzékelő adatokat küldött az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2a904-230">You have successfully connected a Feather HUZZAH ESP8266 to your IoT hub, and sent the captured sensor data to your IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

