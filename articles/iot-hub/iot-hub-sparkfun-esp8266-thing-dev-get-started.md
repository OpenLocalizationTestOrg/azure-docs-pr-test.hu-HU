---
title: "aaaESP8266 toocloud - csatlakozás Sparkfun ESP8266 dolog fejlesztői tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban toosend adatok toohello Azure cloud platform Sparkfun ESP8266 dolog fejlesztői tooAzure IoT-központ az csatlakoznak."
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
ms.openlocfilehash: 19b249df23b6df516634853521c6d532f51014da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="4a215-103">Csatlakozás Sparkfun ESP8266 dolog fejlesztői tooAzure hello felhőben az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="4a215-103">Connect Sparkfun ESP8266 Thing Dev tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 dolog fejlesztői és az IoT-központ közötti kapcsolat](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a><span data-ttu-id="4a215-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="4a215-105">What you will do</span></span>

<span data-ttu-id="4a215-106">Csatlakoztassa a Sparkfun ESP8266 dolog fejlesztői tooan IoT hubot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4a215-106">Connect Sparkfun ESP8266 Thing Dev tooan IoT hub you will create.</span></span> <span data-ttu-id="4a215-107">Futtassa a mintaalkalmazást ESP8266 toocollect hőmérséklet és a páratartalom adatok egy DHT22 érzékelő.</span><span class="sxs-lookup"><span data-stu-id="4a215-107">Then run a sample application on ESP8266 toocollect temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="4a215-108">Végezetül küldése hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="4a215-108">Finally, send hello sensor data tooyour IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="4a215-109">Ha más ESP8266 modulok használ, továbbra is kövesse ezeket a lépéseket tooconnect azt tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="4a215-109">If you are using other ESP8266 boards, you can still follow these steps tooconnect it tooyour IoT hub.</span></span> <span data-ttu-id="4a215-110">Attól függően, hogy hello ESP8266 üzenőfalon használ, szükség lehet a tooreconfigure hello `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="4a215-110">Depending on hello ESP8266 board you are using, you may need tooreconfigure hello `LED_PIN`.</span></span> <span data-ttu-id="4a215-111">Például ha az Eszközintelligencia-Thinker ESP8266 használ, módosíthatja az `0` túl`2`.</span><span class="sxs-lookup"><span data-stu-id="4a215-111">For example, if you are using ESP8266 from AI-Thinker, you may change it from `0` too`2`.</span></span> <span data-ttu-id="4a215-112">Még nem rendelkezik a csomag?: kattintson a [Itt](http://azure.com/iotstarterkits)</span><span class="sxs-lookup"><span data-stu-id="4a215-112">Don't have a kit yet?: Click [here](http://azure.com/iotstarterkits)</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4a215-113">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="4a215-113">What you will learn</span></span>

* <span data-ttu-id="4a215-114">Hogyan toocreate az IoT-központ és az eszköz regisztrálása dolog helyőrzőkivétel fejlesztői</span><span class="sxs-lookup"><span data-stu-id="4a215-114">How toocreate an IoT hub and register a device for Thing Dev.</span></span>
* <span data-ttu-id="4a215-115">Hogyan tooconnect dolog fejlesztői hello érzékelő és a számítógép.</span><span class="sxs-lookup"><span data-stu-id="4a215-115">How tooconnect Thing Dev with hello sensor and your computer.</span></span>
* <span data-ttu-id="4a215-116">Hogyan toocollect érzékelőadatait dolog helyőrzőkivétel fejlesztői mintaalkalmazás futtatásával</span><span class="sxs-lookup"><span data-stu-id="4a215-116">How toocollect sensor data by running a sample application on Thing Dev.</span></span>
* <span data-ttu-id="4a215-117">Hogyan toosend hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="4a215-117">How toosend hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="4a215-118">Mit kell</span><span class="sxs-lookup"><span data-stu-id="4a215-118">What you will need</span></span>

![Hello oktatóanyaghoz szükség részei](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="4a215-120">toocomplete Ez a művelet következő részek a dolog fejlesztői Starter Kit hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4a215-120">toocomplete this operation, you need hello following parts from your Thing Dev Starter Kit:</span></span>

* <span data-ttu-id="4a215-121">hello Sparkfun ESP8266 dolog fejlesztői tábla.</span><span class="sxs-lookup"><span data-stu-id="4a215-121">hello Sparkfun ESP8266 Thing Dev board.</span></span>
* <span data-ttu-id="4a215-122">Micro USB tooType egy USB-kábellel.</span><span class="sxs-lookup"><span data-stu-id="4a215-122">A Micro USB tooType A USB cable.</span></span>

<span data-ttu-id="4a215-123">A fejlesztési környezetet is kell hello következő:</span><span class="sxs-lookup"><span data-stu-id="4a215-123">You also need hello following for your development environment:</span></span>

* <span data-ttu-id="4a215-124">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="4a215-124">An active Azure subscription.</span></span> <span data-ttu-id="4a215-125">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="4a215-125">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="4a215-126">Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.</span><span class="sxs-lookup"><span data-stu-id="4a215-126">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="4a215-127">Sparkfun ESP8266 dolog fejlesztői tooconnect a vezeték nélküli hálózatot.</span><span class="sxs-lookup"><span data-stu-id="4a215-127">Wireless network for Sparkfun ESP8266 Thing Dev tooconnect to.</span></span>
* <span data-ttu-id="4a215-128">Internet kapcsolat toodownload hello konfigurációs eszközt.</span><span class="sxs-lookup"><span data-stu-id="4a215-128">Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="4a215-129">[Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verziót (vagy újabb), korábbi verziói hello AzureIoT szalagtár nem működik.</span><span class="sxs-lookup"><span data-stu-id="4a215-129">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 (or newer), earlier versions will not work with hello AzureIoT library.</span></span>

<span data-ttu-id="4a215-130">hello következő elemeket nem kötelező abban az esetben, ha nincs érzékelő.</span><span class="sxs-lookup"><span data-stu-id="4a215-130">hello following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="4a215-131">Akkor is hello szimulált érzékelőadatait használatának lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="4a215-131">You also have hello option of using simulated sensor data.</span></span>

* <span data-ttu-id="4a215-132">Egy Adafruit DHT22 hőmérséklet és a páratartalom érzékelő.</span><span class="sxs-lookup"><span data-stu-id="4a215-132">An Adafruit DHT22 temperature and humidity sensor.</span></span>
* <span data-ttu-id="4a215-133">Egy breadboard.</span><span class="sxs-lookup"><span data-stu-id="4a215-133">A breadboard.</span></span>
* <span data-ttu-id="4a215-134">M/M átkötés fenyegetéseknek.</span><span class="sxs-lookup"><span data-stu-id="4a215-134">M/M jumper wires.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-hello-sensor-and-your-computer"></a><span data-ttu-id="4a215-135">Csatlakozás ESP8266 dolog fejlesztői hello érzékelő és a számítógép</span><span class="sxs-lookup"><span data-stu-id="4a215-135">Connect ESP8266 Thing Dev with hello sensor and your computer</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-tooesp8266-thing-dev"></a><span data-ttu-id="4a215-136">Csatlakozás DHT22 hőmérséklet és a páratartalom érzékelő tooESP8266 dolog fejlesztői</span><span class="sxs-lookup"><span data-stu-id="4a215-136">Connect a DHT22 temperature and humidity sensor tooESP8266 Thing Dev</span></span>

<span data-ttu-id="4a215-137">Hello breadboard és átkötés fenyegetéseknek toomake hello kapcsolat a következőképpen használhatja.</span><span class="sxs-lookup"><span data-stu-id="4a215-137">Use hello breadboard and jumper wires toomake hello connection as follows.</span></span> <span data-ttu-id="4a215-138">Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.</span><span class="sxs-lookup"><span data-stu-id="4a215-138">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Kapcsolatok referencia](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

<span data-ttu-id="4a215-140">Az érzékelő PIN-kód a következő vezetékezést hello használjuk:</span><span class="sxs-lookup"><span data-stu-id="4a215-140">For sensor pins, we will use hello following wiring:</span></span>

| <span data-ttu-id="4a215-141">Indítsa el a (érzékelő)</span><span class="sxs-lookup"><span data-stu-id="4a215-141">Start (Sensor)</span></span>           | <span data-ttu-id="4a215-142">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="4a215-142">End (Board)</span></span>           | <span data-ttu-id="4a215-143">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="4a215-143">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="4a215-144">VDD (PIN-kód 27F)</span><span class="sxs-lookup"><span data-stu-id="4a215-144">VDD (Pin 27F)</span></span>            | <span data-ttu-id="4a215-145">3V (PIN-kód 8A)</span><span class="sxs-lookup"><span data-stu-id="4a215-145">3V (Pin 8A)</span></span>           | <span data-ttu-id="4a215-146">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="4a215-146">Red cable</span></span>     |
| <span data-ttu-id="4a215-147">ADATOK (PIN-kód 28F)</span><span class="sxs-lookup"><span data-stu-id="4a215-147">DATA (Pin 28F)</span></span>           | <span data-ttu-id="4a215-148">GPIO 2 (PIN-kód 9A)</span><span class="sxs-lookup"><span data-stu-id="4a215-148">GPIO 2 (Pin 9A)</span></span>       | <span data-ttu-id="4a215-149">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="4a215-149">White cable</span></span>    |
| <span data-ttu-id="4a215-150">GND (PIN-kód 30F)</span><span class="sxs-lookup"><span data-stu-id="4a215-150">GND (Pin 30F)</span></span>            | <span data-ttu-id="4a215-151">GND (PIN-kód 7J)</span><span class="sxs-lookup"><span data-stu-id="4a215-151">GND (Pin 7J)</span></span>          | <span data-ttu-id="4a215-152">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="4a215-152">Black cable</span></span>   |


- <span data-ttu-id="4a215-153">További információkért lásd: [DHT22 érzékelő telepítő](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) és [Sparkfun ESP8266 dolog fejlesztői specifikációja](https://www.sparkfun.com/products/13711)</span><span class="sxs-lookup"><span data-stu-id="4a215-153">For more information, see: [DHT22 sensor setup](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) and [Sparkfun ESP8266 Thing Dev specification](https://www.sparkfun.com/products/13711)</span></span>

<span data-ttu-id="4a215-154">Most már a Sparkfun ESP8266 dolog fejlesztői kell csatlakoztatni a működő érzékelő.</span><span class="sxs-lookup"><span data-stu-id="4a215-154">Now your Sparkfun ESP8266 Thing Dev should be connected with a working sensor.</span></span>

![Csatlakozás dht22 ESP8266 dolog fejlesztői](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-tooyour-computer"></a><span data-ttu-id="4a215-156">Sparkfun ESP8266 dolog fejlesztői tooyour számítógép</span><span class="sxs-lookup"><span data-stu-id="4a215-156">Connect Sparkfun ESP8266 Thing Dev tooyour computer</span></span>

<span data-ttu-id="4a215-157">Hello Micro USB tooType A USB kábel tooconnect Sparkfun ESP8266 dolog fejlesztői tooyour számítógép a következőképpen használhatja.</span><span class="sxs-lookup"><span data-stu-id="4a215-157">Use hello Micro USB tooType A USB cable tooconnect Sparkfun ESP8266 Thing Dev tooyour computer as follows.</span></span>

![lágyított huzzah tooyour számítógép](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a><span data-ttu-id="4a215-159">Soros port engedélyek – csak Ubuntu hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4a215-159">Add serial port permissions – Ubuntu only</span></span>

<span data-ttu-id="4a215-160">Ubuntu használatakor ellenőrizze, hogy a normál felhasználói rendelkezik hello engedélyek toooperate a hello USB port a Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői</span><span class="sxs-lookup"><span data-stu-id="4a215-160">If you use Ubuntu, make sure a normal user has hello permissions toooperate on hello USB port of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="4a215-161">a szokványos felhasználói engedélyeinek tooadd soros port kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4a215-161">tooadd serial port permissions for a normal user, follow these steps:</span></span>

1. <span data-ttu-id="4a215-162">Futtassa a következő parancsokat a terminálon hello:</span><span class="sxs-lookup"><span data-stu-id="4a215-162">Run hello following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="4a215-163">A következő kimenetek hello egyik beolvasása:</span><span class="sxs-lookup"><span data-stu-id="4a215-163">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="4a215-164">crw-rw---1 legfelső szintű uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="4a215-164">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="4a215-165">crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="4a215-165">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="4a215-166">Hello kimenet, figyelje meg `uucp` vagy `dialout` , amely hello tulajdonos neve hello USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="4a215-166">In hello output, notice `uucp` or `dialout` that is hello group owner name of hello USB port.</span></span>

1. <span data-ttu-id="4a215-167">Hello felhasználói toohello csoport hozzáadása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4a215-167">Add hello user toohello group by running hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="4a215-168">`<group-owner-name>`hello kapott csoport tulajdonos neve van hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="4a215-168">`<group-owner-name>` is hello group owner name you obtained in hello previous step.</span></span> <span data-ttu-id="4a215-169">`<username>`Ubuntu felhasználónevének van.</span><span class="sxs-lookup"><span data-stu-id="4a215-169">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="4a215-170">Jelentkezzen ki Ubuntu, és jelentkezzen be azt újra az tootake hatás hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="4a215-170">Log out Ubuntu and log in it again for hello change tootake effect.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="4a215-171">Érzékelő adatokat gyűjteni, és elküldi a tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="4a215-171">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="4a215-172">Ebben a részben telepíti, és futtassa a mintaalkalmazást a Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői</span><span class="sxs-lookup"><span data-stu-id="4a215-172">In this section, you deploy and run a sample application on Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="4a215-173">hello mintaalkalmazás villogjon-hello LED Sparkfun ESP8266 dolog fejlesztői a, és küldi hello hőmérséklet és a páratartalom adatgyűjtés hello DHT22 érzékelő tooyour az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="4a215-173">hello sample application blinks hello LED on Sparkfun ESP8266 Thing Dev and sends hello temperature and humidity data collected from hello DHT22 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github"></a><span data-ttu-id="4a215-174">Hello mintaalkalmazás beszerzése a Githubról</span><span class="sxs-lookup"><span data-stu-id="4a215-174">Get hello sample application from GitHub</span></span>

<span data-ttu-id="4a215-175">hello mintaalkalmazást a Githubon található.</span><span class="sxs-lookup"><span data-stu-id="4a215-175">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="4a215-176">Klónozás hello minta tárház, amely tartalmazza a Githubról hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4a215-176">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="4a215-177">tooclone hello minta tárház, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4a215-177">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="4a215-178">Nyisson meg egy parancssort vagy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="4a215-178">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="4a215-179">Nyissa meg a tooa mappára, ahol hello minta alkalmazás toobe tárolja.</span><span class="sxs-lookup"><span data-stu-id="4a215-179">Go tooa folder where you want hello sample application toobe stored.</span></span>
1. <span data-ttu-id="4a215-180">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4a215-180">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

<span data-ttu-id="4a215-181">Sparkfun ESP8266 dolog fejlesztői Arduino ide hello telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="4a215-181">Install hello package for Sparkfun ESP8266 Thing Dev in Arduino IDE:</span></span>

1. <span data-ttu-id="4a215-182">Nyissa meg a mintaalkalmazás hello tároló hello mappát.</span><span class="sxs-lookup"><span data-stu-id="4a215-182">Open hello folder where hello sample application is stored.</span></span>
1. <span data-ttu-id="4a215-183">Nyissa meg a hello app.ino fájlt a Arduino IDE hello app mappában.</span><span class="sxs-lookup"><span data-stu-id="4a215-183">Open hello app.ino file in hello app folder in Arduino IDE.</span></span>

   ![Nyissa meg a mintaalkalmazás hello arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="4a215-185">Kattintson Arduino IDE hello, **fájl** > **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="4a215-185">In hello Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="4a215-186">A hello **beállítások** párbeszédpanel hello ikon következő toohello kattintson **további modulok Manager URL-címet** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="4a215-186">In hello **Preferences** dialog box, click hello icon next toohello **Additional Boards Manager URLs** text box.</span></span>
1. <span data-ttu-id="4a215-187">Hello előugró ablak, írja be a következő URL-cím hello, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a215-187">In hello pop-up window, enter hello following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![a arduino ide pont tooa alkalmazáscsomag URL-címe](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="4a215-189">A hello **preferencia** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a215-189">In hello **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="4a215-190">Kattintson a **eszközök** > **Board** > **modulok Manager**, majd keresse meg a esp8266.</span><span class="sxs-lookup"><span data-stu-id="4a215-190">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>
   <span data-ttu-id="4a215-191">ESP8266 2.2.0 vagy újabb verziójával kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="4a215-191">ESP8266 with a version of 2.2.0 or later should be installed.</span></span>

   ![hello esp8266 csomag telepítve van](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="4a215-193">Kattintson a **eszközök** > **Board** > **Sparkfun ESP8266 dolog fejlesztői**.</span><span class="sxs-lookup"><span data-stu-id="4a215-193">Click **Tools** > **Board** > **Sparkfun ESP8266 Thing Dev**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="4a215-194">Szükséges kódtárak telepítése</span><span class="sxs-lookup"><span data-stu-id="4a215-194">Install necessary libraries</span></span>

1. <span data-ttu-id="4a215-195">Hello Arduino IDE, kattintson **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.</span><span class="sxs-lookup"><span data-stu-id="4a215-195">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="4a215-196">Keresse meg a következő könyvtár nevek egyenként hello.</span><span class="sxs-lookup"><span data-stu-id="4a215-196">Search for hello following library names one by one.</span></span> <span data-ttu-id="4a215-197">Az egyes hello könyvtárban talál, kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="4a215-197">For each of hello library you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="4a215-198">Nincs valós DHT22 érzékelő?</span><span class="sxs-lookup"><span data-stu-id="4a215-198">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="4a215-199">hello mintaalkalmazás szimulálhatja a hőmérséklet és a páratartalom adatok, abban az esetben, ha nincs valós DHT22 érzékelő.</span><span class="sxs-lookup"><span data-stu-id="4a215-199">hello sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="4a215-200">tooenable hello alkalmazás szimulált toouse mintaadatok, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4a215-200">tooenable hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="4a215-201">Nyissa meg hello `config.h` hello fájlban `app` mappát.</span><span class="sxs-lookup"><span data-stu-id="4a215-201">Open hello `config.h` file in hello `app` folder.</span></span>
1. <span data-ttu-id="4a215-202">Keresse meg a következő kódsort hello, és módosítsa hello értéket `false` túl`true`:</span><span class="sxs-lookup"><span data-stu-id="4a215-202">Locate hello following line of code and change hello value from `false` too`true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Hello mintaadatok alkalmazás szimulált toouse konfigurálása](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. <span data-ttu-id="4a215-204">Mentse `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="4a215-204">Save with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toosparkfun-esp8266-thing-dev"></a><span data-ttu-id="4a215-205">Hello minta alkalmazás tooSparkfun ESP8266 dolog fejlesztői telepítése</span><span class="sxs-lookup"><span data-stu-id="4a215-205">Deploy hello sample application tooSparkfun ESP8266 Thing Dev</span></span>

1. <span data-ttu-id="4a215-206">Kattintson Arduino IDE hello, **eszköz** > **Port**, majd kattintson a soros port hello Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői</span><span class="sxs-lookup"><span data-stu-id="4a215-206">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Sparkfun ESP8266 Thing Dev.</span></span>
1. <span data-ttu-id="4a215-207">Kattintson a **vázlat** > **feltöltése** toobuild és hello minta alkalmazás tooSparkfun ESP8266 dolog helyőrzőkivétel fejlesztői telepítése</span><span class="sxs-lookup"><span data-stu-id="4a215-207">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooSparkfun ESP8266 Thing Dev.</span></span>

> [!Note]
> <span data-ttu-id="4a215-208">MacOS használata üzenetek feltöltése során a következő hello valószínűleg látni sikerült.</span><span class="sxs-lookup"><span data-stu-id="4a215-208">If you are using macOS you could probably see hello following messages during uploading.</span></span> <span data-ttu-id="4a215-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span><span class="sxs-lookup"><span data-stu-id="4a215-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span></span> <span data-ttu-id="4a215-210">Nyissa meg a ternimal ablakot, és a probléma alatt műveletek toosolve Befejezés.</span><span class="sxs-lookup"><span data-stu-id="4a215-210">Please open your ternimal window and finish below actions toosolve this issue.</span></span>
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a><span data-ttu-id="4a215-211">Adja meg hitelesítő adatait</span><span class="sxs-lookup"><span data-stu-id="4a215-211">Enter your credentials</span></span>

<span data-ttu-id="4a215-212">Ha sikeresen befejeződött a feltöltés hello, kövesse a hitelesítő adatok hello lépéseket tooenter:</span><span class="sxs-lookup"><span data-stu-id="4a215-212">After hello upload completes successfully, follow hello steps tooenter your credentials:</span></span>

1. <span data-ttu-id="4a215-213">Kattintson Arduino IDE hello, **eszközök** > **soros figyelő**.</span><span class="sxs-lookup"><span data-stu-id="4a215-213">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="4a215-214">Hello soros figyelő ablakban figyelje meg hello két legördülő listák hello jobb alsó sarkában.</span><span class="sxs-lookup"><span data-stu-id="4a215-214">In hello serial monitor window, notice hello two drop-down lists on hello bottom right corner.</span></span>
1. <span data-ttu-id="4a215-215">Válassza ki **nincs sor befejezési** hello bal oldali legördülő listát.</span><span class="sxs-lookup"><span data-stu-id="4a215-215">Select **No line ending** for hello left drop-down list.</span></span>
1. <span data-ttu-id="4a215-216">Válassza ki **115200 átviteli** hello jobb legördülő listát.</span><span class="sxs-lookup"><span data-stu-id="4a215-216">Select **115200 baud** for hello right drop-down list.</span></span>
1. <span data-ttu-id="4a215-217">Hello beviteli mezőbe hello soros figyelő ablak hello tetején található, írja be a következő információ, ha a rendszer felkéri tooprovide hello őket, és kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="4a215-217">In hello input box located at hello top of hello serial monitor window, enter hello following information if you are asked tooprovide them, and then click **Send**.</span></span>
   * <span data-ttu-id="4a215-218">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="4a215-218">Wi-Fi SSID</span></span>
   * <span data-ttu-id="4a215-219">Wi-Fi jelszó</span><span class="sxs-lookup"><span data-stu-id="4a215-219">Wi-Fi password</span></span>
   * <span data-ttu-id="4a215-220">Eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4a215-220">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="4a215-221">hello hitelesítő adatokat a Sparkfun ESP8266 dolog fejlesztési EEPROM hello van tárolva.</span><span class="sxs-lookup"><span data-stu-id="4a215-221">hello credential information is stored in hello EEPROM of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="4a215-222">Ha hello Sparkfun ESP8266 dolog fejlesztői board hello Visszaállítás gombra kattint, hello mintaalkalmazás kéri, ha azt szeretné, tooerase hello információkat.</span><span class="sxs-lookup"><span data-stu-id="4a215-222">If you click hello reset button on hello Sparkfun ESP8266 Thing Dev board, hello sample application asks you if you want tooerase hello information.</span></span> <span data-ttu-id="4a215-223">Adja meg `Y` toohave hello információk törlése és a rendszer kéri-e újra tooprovide hello információt.</span><span class="sxs-lookup"><span data-stu-id="4a215-223">Enter `Y` toohave hello information erased and you are asked tooprovide hello information again.</span></span>

### <a name="verify-hello-sample-application-is-running-successfully"></a><span data-ttu-id="4a215-224">Győződjön meg arról hello mintaalkalmazás sikeresen fut.</span><span class="sxs-lookup"><span data-stu-id="4a215-224">Verify hello sample application is running successfully</span></span>

<span data-ttu-id="4a215-225">Ha a hello hello soros figyelő ablakból parancskimenet, és hello villogó LED a Sparkfun ESP8266 dolog fejlesztői, hello mintaalkalmazás sikeresen fut-e.</span><span class="sxs-lookup"><span data-stu-id="4a215-225">If you see hello following output from hello serial monitor window and hello blinking LED on Sparkfun ESP8266 Thing Dev, hello sample application is running successfully.</span></span>

![végső kimenetet a arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="4a215-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a215-227">Next steps</span></span>

<span data-ttu-id="4a215-228">Sikeresen csatlakozott egy Sparkfun ESP8266 dolog fejlesztői tooyour IoT-központ és rögzített hello érzékelő adatokat tooyour IoT-központ küldött.</span><span class="sxs-lookup"><span data-stu-id="4a215-228">You have successfully connected a Sparkfun ESP8266 Thing Dev tooyour IoT hub and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
