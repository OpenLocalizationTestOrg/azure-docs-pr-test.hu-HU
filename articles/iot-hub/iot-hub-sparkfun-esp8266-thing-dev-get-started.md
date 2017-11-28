---
title: "Felhőbe - csatlakozás Sparkfun ESP8266 dolog fejlesztés az Azure IoT Hub ESP8266 |} Microsoft Docs"
description: "Megtudhatja, hogyan kell beállítania, és Azure IoT-központ az adatokat küldeni az Azure felhőalapú platform ebben az oktatóanyagban Sparkfun ESP8266 dolog fejlesztői csatlakozni."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 557f0cdf375b345e0dbe0526f5a5bd3c050dec38
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="20339-103">Csatlakozás Sparkfun ESP8266 dolog fejlesztői Azure IoT Hub a felhőben</span><span class="sxs-lookup"><span data-stu-id="20339-103">Connect Sparkfun ESP8266 Thing Dev to Azure IoT Hub in the cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 dolog fejlesztői és az IoT-központ közötti kapcsolat](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a><span data-ttu-id="20339-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="20339-105">What you will do</span></span>

<span data-ttu-id="20339-106">Csatlakozás Sparkfun ESP8266 dolog fejlesztői során létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="20339-106">Connect Sparkfun ESP8266 Thing Dev to an IoT hub you will create.</span></span> <span data-ttu-id="20339-107">Ezután futtassa a mintaalkalmazást ESP8266 DHT22 érzékelő hőmérséklet és a páratartalom adatokat gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="20339-107">Then run a sample application on ESP8266 to collect temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="20339-108">Végezetül az érzékelő adatokat továbbít az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="20339-108">Finally, send the sensor data to your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="20339-109">Ha használ egyéb ESP8266 modulok, ezeket a lépéseket az IoT hub csatlakozni továbbra is követheti.</span><span class="sxs-lookup"><span data-stu-id="20339-109">If you are using other ESP8266 boards, you can still follow these steps to connect it to your IoT hub.</span></span> <span data-ttu-id="20339-110">Attól függően, hogy a ESP8266 üzenőfalon használ, szükség lehet, hogy engedélyezzék a `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="20339-110">Depending on the ESP8266 board you are using, you may need to reconfigure the `LED_PIN`.</span></span> <span data-ttu-id="20339-111">Például ha az Eszközintelligencia-Thinker ESP8266 használ, módosíthatja az `0` való `2`.</span><span class="sxs-lookup"><span data-stu-id="20339-111">For example, if you are using ESP8266 from AI-Thinker, you may change it from `0` to `2`.</span></span> <span data-ttu-id="20339-112">Még nem rendelkezik a csomag?: kattintson a [Itt](http://azure.com/iotstarterkits)</span><span class="sxs-lookup"><span data-stu-id="20339-112">Don't have a kit yet?: Click [here](http://azure.com/iotstarterkits)</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="20339-113">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="20339-113">What you will learn</span></span>

* <span data-ttu-id="20339-114">Létrehoz egy IoT-központot, és az eszköz regisztrálása dolog helyőrzőkivétel fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="20339-114">How to create an IoT hub and register a device for Thing Dev.</span></span>
* <span data-ttu-id="20339-115">Hogyan lehet az érzékelő és a számítógép dolog fejlesztői csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="20339-115">How to connect Thing Dev with the sensor and your computer.</span></span>
* <span data-ttu-id="20339-116">Futtatja a mintaalkalmazás dolog helyőrzőkivétel fejlesztői érzékelői adatok gyűjtéséről</span><span class="sxs-lookup"><span data-stu-id="20339-116">How to collect sensor data by running a sample application on Thing Dev.</span></span>
* <span data-ttu-id="20339-117">Az érzékelő adatokat küldeni az IoT hub módjáról.</span><span class="sxs-lookup"><span data-stu-id="20339-117">How to send the sensor data to your IoT hub.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="20339-118">Mit kell</span><span class="sxs-lookup"><span data-stu-id="20339-118">What you will need</span></span>

![Az oktatóanyaghoz szükség részei](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="20339-120">A művelet végrehajtásához a következő részek a dolog fejlesztői Starter Kit kell:</span><span class="sxs-lookup"><span data-stu-id="20339-120">To complete this operation, you need the following parts from your Thing Dev Starter Kit:</span></span>

* <span data-ttu-id="20339-121">A Sparkfun ESP8266 dolog fejlesztői kártya.</span><span class="sxs-lookup"><span data-stu-id="20339-121">The Sparkfun ESP8266 Thing Dev board.</span></span>
* <span data-ttu-id="20339-122">Egy Micro USB-típus egy USB-kábellel.</span><span class="sxs-lookup"><span data-stu-id="20339-122">A Micro USB to Type A USB cable.</span></span>

<span data-ttu-id="20339-123">A fejlesztési környezetet is kell a következőket:</span><span class="sxs-lookup"><span data-stu-id="20339-123">You also need the following for your development environment:</span></span>

* <span data-ttu-id="20339-124">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="20339-124">An active Azure subscription.</span></span> <span data-ttu-id="20339-125">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="20339-125">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="20339-126">Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.</span><span class="sxs-lookup"><span data-stu-id="20339-126">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="20339-127">Vezeték nélküli hálózat Sparkfun ESP8266 dolog fejlesztői való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="20339-127">Wireless network for Sparkfun ESP8266 Thing Dev to connect to.</span></span>
* <span data-ttu-id="20339-128">A kiszolgálókonfigurációs eszköz letöltéséhez internetkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="20339-128">Internet connection to download the configuration tool.</span></span>
* <span data-ttu-id="20339-129">[Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verziót (vagy újabb), korábbi verziói nem fog működni a AzureIoT könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="20339-129">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 (or newer), earlier versions will not work with the AzureIoT library.</span></span>

<span data-ttu-id="20339-130">A következő elemek nem kötelező, abban az esetben, ha nincs érzékelő.</span><span class="sxs-lookup"><span data-stu-id="20339-130">The following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="20339-131">Akkor is a szimulált érzékelőadatait használatának lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="20339-131">You also have the option of using simulated sensor data.</span></span>

* <span data-ttu-id="20339-132">Egy Adafruit DHT22 hőmérséklet és a páratartalom érzékelő.</span><span class="sxs-lookup"><span data-stu-id="20339-132">An Adafruit DHT22 temperature and humidity sensor.</span></span>
* <span data-ttu-id="20339-133">Egy breadboard.</span><span class="sxs-lookup"><span data-stu-id="20339-133">A breadboard.</span></span>
* <span data-ttu-id="20339-134">M/M átkötés fenyegetéseknek.</span><span class="sxs-lookup"><span data-stu-id="20339-134">M/M jumper wires.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-the-sensor-and-your-computer"></a><span data-ttu-id="20339-135">Csatlakozás az érzékelő és a számítógép ESP8266 dolog fejlesztői</span><span class="sxs-lookup"><span data-stu-id="20339-135">Connect ESP8266 Thing Dev with the sensor and your computer</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-esp8266-thing-dev"></a><span data-ttu-id="20339-136">Csatlakozás ESP8266 dolog fejlesztői DHT22 hőmérséklet és a páratartalom érzékelő</span><span class="sxs-lookup"><span data-stu-id="20339-136">Connect a DHT22 temperature and humidity sensor to ESP8266 Thing Dev</span></span>

<span data-ttu-id="20339-137">Használják a breadboard és átkötés az alábbiak szerint a kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="20339-137">Use the breadboard and jumper wires to make the connection as follows.</span></span> <span data-ttu-id="20339-138">Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.</span><span class="sxs-lookup"><span data-stu-id="20339-138">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Kapcsolatok referencia](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

<span data-ttu-id="20339-140">Az érzékelő PIN-kód a következő vezetékezést használjuk:</span><span class="sxs-lookup"><span data-stu-id="20339-140">For sensor pins, we will use the following wiring:</span></span>

| <span data-ttu-id="20339-141">Indítsa el a (érzékelő)</span><span class="sxs-lookup"><span data-stu-id="20339-141">Start (Sensor)</span></span>           | <span data-ttu-id="20339-142">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="20339-142">End (Board)</span></span>           | <span data-ttu-id="20339-143">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="20339-143">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="20339-144">VDD (PIN-kód 27F)</span><span class="sxs-lookup"><span data-stu-id="20339-144">VDD (Pin 27F)</span></span>            | <span data-ttu-id="20339-145">3V (PIN-kód 8A)</span><span class="sxs-lookup"><span data-stu-id="20339-145">3V (Pin 8A)</span></span>           | <span data-ttu-id="20339-146">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="20339-146">Red cable</span></span>     |
| <span data-ttu-id="20339-147">ADATOK (PIN-kód 28F)</span><span class="sxs-lookup"><span data-stu-id="20339-147">DATA (Pin 28F)</span></span>           | <span data-ttu-id="20339-148">GPIO 2 (PIN-kód 9A)</span><span class="sxs-lookup"><span data-stu-id="20339-148">GPIO 2 (Pin 9A)</span></span>       | <span data-ttu-id="20339-149">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="20339-149">White cable</span></span>    |
| <span data-ttu-id="20339-150">GND (PIN-kód 30F)</span><span class="sxs-lookup"><span data-stu-id="20339-150">GND (Pin 30F)</span></span>            | <span data-ttu-id="20339-151">GND (PIN-kód 7J)</span><span class="sxs-lookup"><span data-stu-id="20339-151">GND (Pin 7J)</span></span>          | <span data-ttu-id="20339-152">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="20339-152">Black cable</span></span>   |


- <span data-ttu-id="20339-153">További információkért lásd: [DHT22 érzékelő telepítő](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) és [Sparkfun ESP8266 dolog fejlesztői specifikációja](https://www.sparkfun.com/products/13711)</span><span class="sxs-lookup"><span data-stu-id="20339-153">For more information, see: [DHT22 sensor setup](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) and [Sparkfun ESP8266 Thing Dev specification](https://www.sparkfun.com/products/13711)</span></span>

<span data-ttu-id="20339-154">Most már a Sparkfun ESP8266 dolog fejlesztői kell csatlakoztatni a működő érzékelő.</span><span class="sxs-lookup"><span data-stu-id="20339-154">Now your Sparkfun ESP8266 Thing Dev should be connected with a working sensor.</span></span>

![Csatlakozás dht22 ESP8266 dolog fejlesztői](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-to-your-computer"></a><span data-ttu-id="20339-156">Sparkfun ESP8266 dolog fejlesztői kapcsolódni a számítógéphez</span><span class="sxs-lookup"><span data-stu-id="20339-156">Connect Sparkfun ESP8266 Thing Dev to your computer</span></span>

<span data-ttu-id="20339-157">A Micro USB,-típus egy USB-kábel segítségével Sparkfun ESP8266 dolog fejlesztői csatlakozni a számítógép az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="20339-157">Use the Micro USB to Type A USB cable to connect Sparkfun ESP8266 Thing Dev to your computer as follows.</span></span>

![lágyított huzzah kapcsolódni a számítógéphez](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a><span data-ttu-id="20339-159">Soros port engedélyek – csak Ubuntu hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20339-159">Add serial port permissions – Ubuntu only</span></span>

<span data-ttu-id="20339-160">Ubuntu használatakor győződjön meg arról a normál felhasználói a jogosult működéséhez a a USB port a Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői</span><span class="sxs-lookup"><span data-stu-id="20339-160">If you use Ubuntu, make sure a normal user has the permissions to operate on the USB port of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="20339-161">A szokványos felhasználói engedélyeinek soros port hozzáadásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="20339-161">To add serial port permissions for a normal user, follow these steps:</span></span>

1. <span data-ttu-id="20339-162">A következő parancsokat a terminálon:</span><span class="sxs-lookup"><span data-stu-id="20339-162">Run the following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="20339-163">A következő kimenetek egyik beolvasása:</span><span class="sxs-lookup"><span data-stu-id="20339-163">You get one of the following outputs:</span></span>

   * <span data-ttu-id="20339-164">crw-rw---1 legfelső szintű uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="20339-164">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="20339-165">crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="20339-165">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="20339-166">Figyelje meg, a kimenetben `uucp` vagy `dialout` , amely a csoport tulajdonosának neve az USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="20339-166">In the output, notice `uucp` or `dialout` that is the group owner name of the USB port.</span></span>

1. <span data-ttu-id="20339-167">A felhasználó felvétele a csoportba a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="20339-167">Add the user to the group by running the following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="20339-168">`<group-owner-name>`az az előző lépésben beolvasott tulajdonos neve van.</span><span class="sxs-lookup"><span data-stu-id="20339-168">`<group-owner-name>` is the group owner name you obtained in the previous step.</span></span> <span data-ttu-id="20339-169">`<username>`Ubuntu felhasználónevének van.</span><span class="sxs-lookup"><span data-stu-id="20339-169">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="20339-170">Ubuntu kijelentkezés, és jelentkezzen be újra az a változtatás érvénybe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="20339-170">Log out Ubuntu and log in it again for the change to take effect.</span></span>

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a><span data-ttu-id="20339-171">Érzékelő adatokat gyűjteni, és küldje el az IoT hub</span><span class="sxs-lookup"><span data-stu-id="20339-171">Collect sensor data and send it to your IoT hub</span></span>

<span data-ttu-id="20339-172">Ebben a részben telepíti, és futtassa a mintaalkalmazást a Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői</span><span class="sxs-lookup"><span data-stu-id="20339-172">In this section, you deploy and run a sample application on Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="20339-173">A mintaalkalmazás a Sparkfun ESP8266 dolog fejlesztői LED villogjon, és elküldi a hőmérséklet és a páratartalom gyűjtött adatokat a DHT22 érzékelő az IoT hubhoz.</span><span class="sxs-lookup"><span data-stu-id="20339-173">The sample application blinks the LED on Sparkfun ESP8266 Thing Dev and sends the temperature and humidity data collected from the DHT22 sensor to your IoT hub.</span></span>

### <a name="get-the-sample-application-from-github"></a><span data-ttu-id="20339-174">A mintaalkalmazás beszerzése a Githubról</span><span class="sxs-lookup"><span data-stu-id="20339-174">Get the sample application from GitHub</span></span>

<span data-ttu-id="20339-175">A mintaalkalmazás tárolja a Githubon.</span><span class="sxs-lookup"><span data-stu-id="20339-175">The sample application is hosted on GitHub.</span></span> <span data-ttu-id="20339-176">Klónozza a minta-tárház, amely tartalmazza a mintaalkalmazást a Githubról.</span><span class="sxs-lookup"><span data-stu-id="20339-176">Clone the sample repository that contains the sample application from GitHub.</span></span> <span data-ttu-id="20339-177">A minta-tárház klónozása, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="20339-177">To clone the sample repository, follow these steps:</span></span>

1. <span data-ttu-id="20339-178">Nyisson meg egy parancssort vagy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="20339-178">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="20339-179">Nyissa meg a mappát, ahol a mintaalkalmazáshoz történő tárolását.</span><span class="sxs-lookup"><span data-stu-id="20339-179">Go to a folder where you want the sample application to be stored.</span></span>
1. <span data-ttu-id="20339-180">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="20339-180">Run the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

<span data-ttu-id="20339-181">A csomag telepítése Sparkfun ESP8266 dolog fejlesztői Arduino ide:</span><span class="sxs-lookup"><span data-stu-id="20339-181">Install the package for Sparkfun ESP8266 Thing Dev in Arduino IDE:</span></span>

1. <span data-ttu-id="20339-182">Nyissa meg a mappát, ahol a mintaalkalmazás tárolja.</span><span class="sxs-lookup"><span data-stu-id="20339-182">Open the folder where the sample application is stored.</span></span>
1. <span data-ttu-id="20339-183">Nyissa meg a app.ino fájlt Arduino IDE az alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="20339-183">Open the app.ino file in the app folder in Arduino IDE.</span></span>

   ![Nyissa meg a mintaalkalmazás arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="20339-185">Kattintson a Arduino ide **fájl** > **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="20339-185">In the Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="20339-186">Az a **beállítások** párbeszédpanel mellett kattintson a ikonra a **további modulok Manager URL-címet** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="20339-186">In the **Preferences** dialog box, click the icon next to the **Additional Boards Manager URLs** text box.</span></span>
1. <span data-ttu-id="20339-187">Az előugró ablakban írja be a következő URL-címet, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="20339-187">In the pop-up window, enter the following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Mutasson a arduino ide egy alkalmazáscsomag URL-címe](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="20339-189">Az a **preferencia** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="20339-189">In the **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="20339-190">Kattintson a **eszközök** > **Board** > **modulok Manager**, majd keresse meg a esp8266.</span><span class="sxs-lookup"><span data-stu-id="20339-190">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>
   <span data-ttu-id="20339-191">ESP8266 2.2.0 vagy újabb verziójával kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="20339-191">ESP8266 with a version of 2.2.0 or later should be installed.</span></span>

   ![A esp8266 csomag telepítve van](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="20339-193">Kattintson a **eszközök** > **Board** > **Sparkfun ESP8266 dolog fejlesztői**.</span><span class="sxs-lookup"><span data-stu-id="20339-193">Click **Tools** > **Board** > **Sparkfun ESP8266 Thing Dev**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="20339-194">Szükséges kódtárak telepítése</span><span class="sxs-lookup"><span data-stu-id="20339-194">Install necessary libraries</span></span>

1. <span data-ttu-id="20339-195">Kattintson a Arduino ide **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.</span><span class="sxs-lookup"><span data-stu-id="20339-195">In the Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="20339-196">Keresse meg a következő könyvtár nevek egyenként.</span><span class="sxs-lookup"><span data-stu-id="20339-196">Search for the following library names one by one.</span></span> <span data-ttu-id="20339-197">Az egyes megtalálta a könyvtárban kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="20339-197">For each of the library you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="20339-198">Nincs valós DHT22 érzékelő?</span><span class="sxs-lookup"><span data-stu-id="20339-198">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="20339-199">A mintaalkalmazás szimulálhatja a hőmérséklet és a páratartalom adatok, abban az esetben, ha nincs valós DHT22 érzékelő.</span><span class="sxs-lookup"><span data-stu-id="20339-199">The sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="20339-200">Ahhoz, hogy a mintaalkalmazás szimulált adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="20339-200">To enable the sample application to use simulated data, follow these steps:</span></span>

1. <span data-ttu-id="20339-201">Nyissa meg a `config.h` fájlt a `app` mappát.</span><span class="sxs-lookup"><span data-stu-id="20339-201">Open the `config.h` file in the `app` folder.</span></span>
1. <span data-ttu-id="20339-202">Keresse meg a következő kódsort, és módosítsa az értéket `false` való `true`:</span><span class="sxs-lookup"><span data-stu-id="20339-202">Locate the following line of code and change the value from `false` to `true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurálja a mintaalkalmazás szimulált adatok használata](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. <span data-ttu-id="20339-204">Mentse `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="20339-204">Save with `Control-s`.</span></span>

### <a name="deploy-the-sample-application-to-sparkfun-esp8266-thing-dev"></a><span data-ttu-id="20339-205">A minta alkalmazást Sparkfun ESP8266 dolog fejlesztői telepíti</span><span class="sxs-lookup"><span data-stu-id="20339-205">Deploy the sample application to Sparkfun ESP8266 Thing Dev</span></span>

1. <span data-ttu-id="20339-206">Kattintson a Arduino ide **eszköz** > **Port**, majd kattintson a soros port Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői</span><span class="sxs-lookup"><span data-stu-id="20339-206">In the Arduino IDE, click **Tool** > **Port**, and then click the serial port for Sparkfun ESP8266 Thing Dev.</span></span>
1. <span data-ttu-id="20339-207">Kattintson a **vázlat** > **feltöltése** létrehozásához és telepítéséhez a mintaalkalmazáshoz történő Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői</span><span class="sxs-lookup"><span data-stu-id="20339-207">Click **Sketch** > **Upload** to build and deploy the sample application to Sparkfun ESP8266 Thing Dev.</span></span>

> [!Note]
> <span data-ttu-id="20339-208">MacOS használata valószínűleg bemutatásához az alábbi üzenetek feltöltése során.</span><span class="sxs-lookup"><span data-stu-id="20339-208">If you are using macOS you could probably see the following messages during uploading.</span></span> <span data-ttu-id="20339-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span><span class="sxs-lookup"><span data-stu-id="20339-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span></span> <span data-ttu-id="20339-210">Nyissa meg a ternimal ablakot, és a probléma megoldásához műveletek alatt Befejezés.</span><span class="sxs-lookup"><span data-stu-id="20339-210">Please open your ternimal window and finish below actions to solve this issue.</span></span>
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a><span data-ttu-id="20339-211">Adja meg hitelesítő adatait</span><span class="sxs-lookup"><span data-stu-id="20339-211">Enter your credentials</span></span>

<span data-ttu-id="20339-212">Ha sikeresen befejeződött a feltöltés, kövesse a hitelesítő adatok megadását:</span><span class="sxs-lookup"><span data-stu-id="20339-212">After the upload completes successfully, follow the steps to enter your credentials:</span></span>

1. <span data-ttu-id="20339-213">Kattintson a Arduino ide **eszközök** > **soros figyelő**.</span><span class="sxs-lookup"><span data-stu-id="20339-213">In the Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="20339-214">A soros figyelő ablakban figyelje meg, a két legördülő lista a jobb alsó sarokban.</span><span class="sxs-lookup"><span data-stu-id="20339-214">In the serial monitor window, notice the two drop-down lists on the bottom right corner.</span></span>
1. <span data-ttu-id="20339-215">Válassza ki **nincs sor befejezési** a bal oldali legördülő listát.</span><span class="sxs-lookup"><span data-stu-id="20339-215">Select **No line ending** for the left drop-down list.</span></span>
1. <span data-ttu-id="20339-216">Válassza ki **115200 átviteli** jobb legördülő listája.</span><span class="sxs-lookup"><span data-stu-id="20339-216">Select **115200 baud** for the right drop-down list.</span></span>
1. <span data-ttu-id="20339-217">A beviteli mezőbe, a soros figyelő ablak tetején található, adja meg az az alábbi adatokat, ha a rendszer kéri azokat, és kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="20339-217">In the input box located at the top of the serial monitor window, enter the following information if you are asked to provide them, and then click **Send**.</span></span>
   * <span data-ttu-id="20339-218">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="20339-218">Wi-Fi SSID</span></span>
   * <span data-ttu-id="20339-219">Wi-Fi jelszó</span><span class="sxs-lookup"><span data-stu-id="20339-219">Wi-Fi password</span></span>
   * <span data-ttu-id="20339-220">Eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="20339-220">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="20339-221">A hitelesítő adatokat a EEPROM a Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői van tárolva.</span><span class="sxs-lookup"><span data-stu-id="20339-221">The credential information is stored in the EEPROM of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="20339-222">Ha a Sparkfun ESP8266 dolog fejlesztői táblán a Visszaállítás gombra kattint, a mintaalkalmazást megkérdezi, hogy szeretné-e használni a.</span><span class="sxs-lookup"><span data-stu-id="20339-222">If you click the reset button on the Sparkfun ESP8266 Thing Dev board, the sample application asks you if you want to erase the information.</span></span> <span data-ttu-id="20339-223">Adja meg `Y` kell rendelkeznie az adatokat, törlése és a rendszer kéri újra az adatok.</span><span class="sxs-lookup"><span data-stu-id="20339-223">Enter `Y` to have the information erased and you are asked to provide the information again.</span></span>

### <a name="verify-the-sample-application-is-running-successfully"></a><span data-ttu-id="20339-224">Ellenőrizze a mintaalkalmazás sikeresen fut-e</span><span class="sxs-lookup"><span data-stu-id="20339-224">Verify the sample application is running successfully</span></span>

<span data-ttu-id="20339-225">A következő kimeneti a soros figyelő ablakból és a villogó LED Sparkfun ESP8266 dolog fejlesztői látható, ha sikeresen fut a mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="20339-225">If you see the following output from the serial monitor window and the blinking LED on Sparkfun ESP8266 Thing Dev, the sample application is running successfully.</span></span>

![végső kimenetet a arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="20339-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20339-227">Next steps</span></span>

<span data-ttu-id="20339-228">Sikeresen egy Sparkfun ESP8266 dolog fejlesztői csatlakozik az IoT hub és a rögzített érzékelő adatokat küldött az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="20339-228">You have successfully connected a Sparkfun ESP8266 Thing Dev to your IoT hub and sent the captured sensor data to your IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
