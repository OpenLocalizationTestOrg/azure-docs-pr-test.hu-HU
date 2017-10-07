---
title: "aaaESP8266 toocloud - csatlakozás lágyított HUZZAH ESP8266 tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban toosend adatok toohello Azure cloud platform Adafruit lágyított HUZZAH ESP8266 tooAzure IoT-központ az csatlakoznak."
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
ms.openlocfilehash: 44fd47232488948d21c7aa71bdd865397e41e63e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="1a62f-103">Csatlakozás Adafruit lágyított HUZZAH ESP8266 tooAzure hello felhőben az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="1a62f-103">Connect Adafruit Feather HUZZAH ESP8266 tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 lágyított HUZZAH ESP8266 és az IoT-központ közötti kapcsolat](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a><span data-ttu-id="1a62f-105">Mit</span><span class="sxs-lookup"><span data-stu-id="1a62f-105">What you do</span></span>


<span data-ttu-id="1a62f-106">Csatlakozás Adafruit lágyított HUZZAH ESP8266 tooan IoT-központ az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1a62f-106">Connect Adafruit Feather HUZZAH ESP8266 tooan IoT hub that you create.</span></span> <span data-ttu-id="1a62f-107">Ezután egy DHT22 érzékelő adatokon ESP8266 toocollect hello hőmérséklet és a páratartalom futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1a62f-107">Then you run a sample application on ESP8266 toocollect hello temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="1a62f-108">Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="1a62f-108">Finally, you send hello sensor data tooyour IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="1a62f-109">Ha más ESP8266 modulok használata, továbbra is kövesse ezeket a lépéseket tooconnect azt tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="1a62f-109">If you're using other ESP8266 boards, you can still follow these steps tooconnect it tooyour IoT hub.</span></span> <span data-ttu-id="1a62f-110">Attól függően, hogy hello ESP8266 üzenőfalon használ, szükség lehet a tooreconfigure hello `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="1a62f-110">Depending on hello ESP8266 board you're using, you might need tooreconfigure hello `LED_PIN`.</span></span> <span data-ttu-id="1a62f-111">Például az Eszközintelligencia-Thinker ESP8266 használata, érdemes lehet módosítani az `0` túl`2`.</span><span class="sxs-lookup"><span data-stu-id="1a62f-111">For example, if you're using ESP8266 from AI-Thinker, you might change it from `0` too`2`.</span></span> <span data-ttu-id="1a62f-112">Még nem rendelkezik egy csomagot?</span><span class="sxs-lookup"><span data-stu-id="1a62f-112">Don't have a kit yet?</span></span> <span data-ttu-id="1a62f-113">Az beszerzése hello [Azure-webhelyen](http://azure.com/iotstarterkits).</span><span class="sxs-lookup"><span data-stu-id="1a62f-113">Get it from hello [Azure website](http://azure.com/iotstarterkits).</span></span>




## <a name="what-you-learn"></a><span data-ttu-id="1a62f-114">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="1a62f-114">What you learn</span></span>

* <span data-ttu-id="1a62f-115">Hogyan toocreate az IoT-központ és az eszköz regisztrálása az lágyított HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="1a62f-115">How toocreate an IoT hub and register a device for Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="1a62f-116">Hogyan tooconnect lágyított HUZZAH ESP8266 hello érzékelő és a számítógép</span><span class="sxs-lookup"><span data-stu-id="1a62f-116">How tooconnect Feather HUZZAH ESP8266 with hello sensor and your computer</span></span>
* <span data-ttu-id="1a62f-117">Hogyan toocollect érzékelőadatait lágyított HUZZAH ESP8266 mintaalkalmazás futtatásával</span><span class="sxs-lookup"><span data-stu-id="1a62f-117">How toocollect sensor data by running a sample application on Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="1a62f-118">Hogyan toosend hello érzékelő adatokat tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="1a62f-118">How toosend hello sensor data tooyour IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1a62f-119">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="1a62f-119">What you need</span></span>

![Hello oktatóanyaghoz szükség részei](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="1a62f-121">toocomplete Ez a művelet következő részek a lágyított HUZZAH ESP8266 Starter Kit hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="1a62f-121">toocomplete this operation, you need hello following parts from your Feather HUZZAH ESP8266 Starter Kit:</span></span>

* <span data-ttu-id="1a62f-122">hello lágyított HUZZAH ESP8266 board</span><span class="sxs-lookup"><span data-stu-id="1a62f-122">hello Feather HUZZAH ESP8266 board</span></span>
* <span data-ttu-id="1a62f-123">Egy Micro USB tooType egy USB-kábellel</span><span class="sxs-lookup"><span data-stu-id="1a62f-123">A Micro USB tooType A USB cable</span></span>

<span data-ttu-id="1a62f-124">A fejlesztési környezet dolgok következő hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="1a62f-124">You also need hello following things for your development environment:</span></span>

* <span data-ttu-id="1a62f-125">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="1a62f-125">An active Azure subscription.</span></span> <span data-ttu-id="1a62f-126">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="1a62f-126">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="1a62f-127">Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.</span><span class="sxs-lookup"><span data-stu-id="1a62f-127">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="1a62f-128">Lágyított HUZZAH ESP8266 tooconnect a vezeték nélküli hálózatot.</span><span class="sxs-lookup"><span data-stu-id="1a62f-128">Wireless network for Feather HUZZAH ESP8266 tooconnect to.</span></span>
* <span data-ttu-id="1a62f-129">Internet kapcsolat toodownload hello konfigurációs eszközt.</span><span class="sxs-lookup"><span data-stu-id="1a62f-129">Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="1a62f-130">[Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="1a62f-130">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="1a62f-131">A korábbi hello AzureIoT szalagtár nem működik.</span><span class="sxs-lookup"><span data-stu-id="1a62f-131">Earlier versions don't work with hello AzureIoT library.</span></span>

<span data-ttu-id="1a62f-132">hello következő elemeket nem kötelező abban az esetben, ha nincs érzékelő.</span><span class="sxs-lookup"><span data-stu-id="1a62f-132">hello following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="1a62f-133">Akkor is hello szimulált érzékelőadatait használatának lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="1a62f-133">You also have hello option of using simulated sensor data.</span></span>

* <span data-ttu-id="1a62f-134">Egy Adafruit DHT22 hőmérséklet és a páratartalom érzékelő</span><span class="sxs-lookup"><span data-stu-id="1a62f-134">An Adafruit DHT22 temperature and humidity sensor</span></span>
* <span data-ttu-id="1a62f-135">Egy breadboard</span><span class="sxs-lookup"><span data-stu-id="1a62f-135">A breadboard</span></span>
* <span data-ttu-id="1a62f-136">M/M átkötés fenyegetéseknek</span><span class="sxs-lookup"><span data-stu-id="1a62f-136">M/M jumper wires</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-hello-sensor-and-your-computer"></a><span data-ttu-id="1a62f-137">Csatlakozás lágyított HUZZAH ESP8266 hello érzékelő és a számítógép</span><span class="sxs-lookup"><span data-stu-id="1a62f-137">Connect Feather HUZZAH ESP8266 with hello sensor and your computer</span></span>
<span data-ttu-id="1a62f-138">Ebben a szakaszban hello érzékelők tooyour board csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="1a62f-138">In this section, you connect hello sensors tooyour board.</span></span> <span data-ttu-id="1a62f-139">Majd csatlakoztassa a eszköz tooyour számítógép további használatra.</span><span class="sxs-lookup"><span data-stu-id="1a62f-139">Then you plug in your device tooyour computer for further use.</span></span>
### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-huzzah-esp8266"></a><span data-ttu-id="1a62f-140">Csatlakozás egy DHT22 hőmérséklet és a páratartalom érzékelő tooFeather HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="1a62f-140">Connect a DHT22 temperature and humidity sensor tooFeather HUZZAH ESP8266</span></span>

<span data-ttu-id="1a62f-141">Hello breadboard és átkötés fenyegetéseknek toomake hello kapcsolat a következőképpen használhatja.</span><span class="sxs-lookup"><span data-stu-id="1a62f-141">Use hello breadboard and jumper wires toomake hello connection as follows.</span></span> <span data-ttu-id="1a62f-142">Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.</span><span class="sxs-lookup"><span data-stu-id="1a62f-142">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![Kapcsolatok referencia](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


<span data-ttu-id="1a62f-144">Érzékelő PIN-kód használja a következő vezetékezést hello:</span><span class="sxs-lookup"><span data-stu-id="1a62f-144">For sensor pins, use hello following wiring:</span></span>


| <span data-ttu-id="1a62f-145">Indítsa el a (érzékelő)</span><span class="sxs-lookup"><span data-stu-id="1a62f-145">Start (Sensor)</span></span>           | <span data-ttu-id="1a62f-146">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="1a62f-146">End (Board)</span></span>           | <span data-ttu-id="1a62f-147">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="1a62f-147">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="1a62f-148">VDD (PIN-kód 31F)</span><span class="sxs-lookup"><span data-stu-id="1a62f-148">VDD (Pin 31F)</span></span>            | <span data-ttu-id="1a62f-149">3V (PIN-kód 58H)</span><span class="sxs-lookup"><span data-stu-id="1a62f-149">3V (Pin 58H)</span></span>           | <span data-ttu-id="1a62f-150">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="1a62f-150">Red cable</span></span>     |
| <span data-ttu-id="1a62f-151">ADATOK (PIN-kód 32F)</span><span class="sxs-lookup"><span data-stu-id="1a62f-151">DATA (Pin 32F)</span></span>           | <span data-ttu-id="1a62f-152">GPIO 2 (PIN-kód 46A)</span><span class="sxs-lookup"><span data-stu-id="1a62f-152">GPIO 2 (Pin 46A)</span></span>       | <span data-ttu-id="1a62f-153">Kék kábel</span><span class="sxs-lookup"><span data-stu-id="1a62f-153">Blue cable</span></span>    |
| <span data-ttu-id="1a62f-154">GND (PIN-kód 34F)</span><span class="sxs-lookup"><span data-stu-id="1a62f-154">GND (Pin 34F)</span></span>            | <span data-ttu-id="1a62f-155">GND (PIN-kód 56I)</span><span class="sxs-lookup"><span data-stu-id="1a62f-155">GND (PIn 56I)</span></span>          | <span data-ttu-id="1a62f-156">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="1a62f-156">Black cable</span></span>   |

<span data-ttu-id="1a62f-157">További információkért lásd: [Adafruit DHT22 érzékelő telepítő](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) és [Adafruit lágyított HUZZAH Esp8266 érintkezőkiosztása szerepel](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span><span class="sxs-lookup"><span data-stu-id="1a62f-157">For more information, see [Adafruit DHT22 sensor setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) and [Adafruit Feather HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span></span>



<span data-ttu-id="1a62f-158">Most már a lágyított Huzzah ESP8266 kell csatlakoztatni a működő érzékelő.</span><span class="sxs-lookup"><span data-stu-id="1a62f-158">Now your Feather Huzzah ESP8266 should be connected with a working sensor.</span></span>

![Csatlakozás DHT22 lágyított Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-tooyour-computer"></a><span data-ttu-id="1a62f-160">Lágyított HUZZAH ESP8266 tooyour számítógép</span><span class="sxs-lookup"><span data-stu-id="1a62f-160">Connect Feather HUZZAH ESP8266 tooyour computer</span></span>

<span data-ttu-id="1a62f-161">Amint tovább, hello Micro USB tooType A USB kábel tooconnect lágyított HUZZAH ESP8266 tooyour számítógépet használni.</span><span class="sxs-lookup"><span data-stu-id="1a62f-161">As shown next, use hello Micro USB tooType A USB cable tooconnect Feather HUZZAH ESP8266 tooyour computer.</span></span>

![Lágyított Huzzah tooyour számítógép](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="1a62f-163">Adja hozzá a soros port engedélyek (csak Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="1a62f-163">Add serial port permissions (Ubuntu only)</span></span>


<span data-ttu-id="1a62f-164">Ha Ubuntu használ, győződjön meg arról a hello USB port a lágyított HUZZAH ESP8266 rendelkezik hello engedélyek toooperate.</span><span class="sxs-lookup"><span data-stu-id="1a62f-164">If you use Ubuntu, make sure you have hello permissions toooperate on hello USB port of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="1a62f-165">tooadd soros port engedélyek, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1a62f-165">tooadd serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="1a62f-166">Futtassa a következő parancsokat a terminálon hello:</span><span class="sxs-lookup"><span data-stu-id="1a62f-166">Run hello following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="1a62f-167">A következő kimenetek hello egyik beolvasása:</span><span class="sxs-lookup"><span data-stu-id="1a62f-167">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="1a62f-168">crw-rw---1 legfelső szintű uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="1a62f-168">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="1a62f-169">crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="1a62f-169">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="1a62f-170">Figyelje meg, hogy hello kimenet `uucp` vagy `dialout` hello csoport tulajdonos neve hello USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="1a62f-170">In hello output, notice that `uucp` or `dialout` is hello group owner name of hello USB port.</span></span>

1. <span data-ttu-id="1a62f-171">Hello felhasználói toohello csoport hozzáadása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1a62f-171">Add hello user toohello group by running hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="1a62f-172">`<group-owner-name>`hello kapott csoport tulajdonos neve van hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="1a62f-172">`<group-owner-name>` is hello group owner name you obtained in hello previous step.</span></span> <span data-ttu-id="1a62f-173">`<username>`Ubuntu felhasználónevének van.</span><span class="sxs-lookup"><span data-stu-id="1a62f-173">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="1a62f-174">Ubuntu kijelentkezni, és jelentkezzen be újból a hello módosítása tooappear.</span><span class="sxs-lookup"><span data-stu-id="1a62f-174">Sign out of Ubuntu, and then sign in again for hello change tooappear.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="1a62f-175">Érzékelő adatokat gyűjteni, és elküldi a tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="1a62f-175">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="1a62f-176">Ebben a szakaszban telepítése, és futtassa a mintaalkalmazást lágyított HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="1a62f-176">In this section, you deploy and run a sample application on Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="1a62f-177">hello mintaalkalmazás villogjon-hello LED lágyított HUZZAH ESP8266 a és küldi hello hőmérséklet és a páratartalom adatgyűjtés hello DHT22 érzékelő tooyour az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="1a62f-177">hello sample application blinks hello LED on Feather HUZZAH ESP8266, and sends hello temperature and humidity data collected from hello DHT22 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github"></a><span data-ttu-id="1a62f-178">Hello mintaalkalmazás beszerzése a Githubról</span><span class="sxs-lookup"><span data-stu-id="1a62f-178">Get hello sample application from GitHub</span></span>

<span data-ttu-id="1a62f-179">hello mintaalkalmazást a Githubon található.</span><span class="sxs-lookup"><span data-stu-id="1a62f-179">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="1a62f-180">Klónozás hello minta tárház, amely tartalmazza a Githubról hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1a62f-180">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="1a62f-181">tooclone hello minta tárház, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1a62f-181">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="1a62f-182">Nyisson meg egy parancssort vagy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="1a62f-182">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="1a62f-183">Nyissa meg a tooa mappára, ahol hello minta alkalmazás toobe tárolja.</span><span class="sxs-lookup"><span data-stu-id="1a62f-183">Go tooa folder where you want hello sample application toobe stored.</span></span>
1. <span data-ttu-id="1a62f-184">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1a62f-184">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

<span data-ttu-id="1a62f-185">Hello csomag telepítése a lágyított HUZZAH ESP8266 hello Arduino IDE:</span><span class="sxs-lookup"><span data-stu-id="1a62f-185">Install hello package for Feather HUZZAH ESP8266 in hello Arduino IDE:</span></span>

1. <span data-ttu-id="1a62f-186">Nyissa meg a mintaalkalmazás hello tároló hello mappát.</span><span class="sxs-lookup"><span data-stu-id="1a62f-186">Open hello folder where hello sample application is stored.</span></span>
1. <span data-ttu-id="1a62f-187">Nyissa meg a hello app.ino fájlt hello Arduino IDE hello alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="1a62f-187">Open hello app.ino file in hello app folder in hello Arduino IDE.</span></span>

   ![Nyissa meg a mintaalkalmazás hello Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="1a62f-189">Kattintson Arduino IDE hello, **fájl** > **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1a62f-189">In hello Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="1a62f-190">Hello a **beállítások** párbeszédpanelen kattintson az ikonra következő toohello hello **további modulok Manager URL-címet** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="1a62f-190">In hello **Preferences** dialog box, click hello icon next toohello **Additional Boards Manager URLs** box.</span></span>
1. <span data-ttu-id="1a62f-191">Hello előugró ablak, írja be a következő URL-cím hello, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a62f-191">In hello pop-up window, enter hello following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![A Arduino IDE pont tooa alkalmazáscsomag URL-címe](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="1a62f-193">A hello **preferencia** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a62f-193">In hello **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="1a62f-194">Kattintson a **eszközök** > **Board** > **modulok Manager**, majd keresse meg a esp8266.</span><span class="sxs-lookup"><span data-stu-id="1a62f-194">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>

   <span data-ttu-id="1a62f-195">Modulok Manager, az azt jelenti, hogy telepítve van-e a ESP8266 2.2.0 vagy újabb verziójával.</span><span class="sxs-lookup"><span data-stu-id="1a62f-195">Boards Manager indicates that ESP8266 with a version of 2.2.0 or later is installed.</span></span>

   ![hello esp8266 csomag telepítve van](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="1a62f-197">Kattintson a **eszközök** > **Board** > **Adafruit HUZZAH ESP8266**.</span><span class="sxs-lookup"><span data-stu-id="1a62f-197">Click **Tools** > **Board** > **Adafruit HUZZAH ESP8266**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="1a62f-198">Szükséges kódtárak telepítése</span><span class="sxs-lookup"><span data-stu-id="1a62f-198">Install necessary libraries</span></span>

1. <span data-ttu-id="1a62f-199">Hello Arduino IDE, kattintson **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.</span><span class="sxs-lookup"><span data-stu-id="1a62f-199">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="1a62f-200">Keresse meg a következő könyvtár nevek egyenként hello.</span><span class="sxs-lookup"><span data-stu-id="1a62f-200">Search for hello following library names one by one.</span></span> <span data-ttu-id="1a62f-201">Az egyes tárak talált, kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="1a62f-201">For each  library that you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="1a62f-202">Nincs valós DHT22 érzékelő?</span><span class="sxs-lookup"><span data-stu-id="1a62f-202">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="1a62f-203">hello mintaalkalmazás szimulálhatja a hőmérséklet és a páratartalom adatok, abban az esetben, ha nincs valós DHT22 érzékelő.</span><span class="sxs-lookup"><span data-stu-id="1a62f-203">hello sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="1a62f-204">tooset hello minta alkalmazás szimulált toouse adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1a62f-204">tooset up hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="1a62f-205">Nyissa meg hello `config.h` hello fájlban `app` mappát.</span><span class="sxs-lookup"><span data-stu-id="1a62f-205">Open hello `config.h` file in hello `app` folder.</span></span>
1. <span data-ttu-id="1a62f-206">Keresse meg a következő kódsort hello, és módosítsa hello értéket `false` túl`true`:</span><span class="sxs-lookup"><span data-stu-id="1a62f-206">Locate hello following line of code and change hello value from `false` too`true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Hello mintaadatok alkalmazás szimulált toouse konfigurálása](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. <span data-ttu-id="1a62f-208">Mentse a fájlt hello `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="1a62f-208">Save hello file with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toofeather-huzzah-esp8266"></a><span data-ttu-id="1a62f-209">Hello minta alkalmazás tooFeather HUZZAH ESP8266 telepítése</span><span class="sxs-lookup"><span data-stu-id="1a62f-209">Deploy hello sample application tooFeather HUZZAH ESP8266</span></span>

1. <span data-ttu-id="1a62f-210">Kattintson Arduino IDE hello, **eszköz** > **Port**, majd kattintson a lágyított HUZZAH ESP8266 hello soros port.</span><span class="sxs-lookup"><span data-stu-id="1a62f-210">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Feather HUZZAH ESP8266.</span></span>
1. <span data-ttu-id="1a62f-211">Kattintson a **vázlat** > **feltöltése** toobuild és hello minta alkalmazás tooFeather HUZZAH ESP8266 telepítése.</span><span class="sxs-lookup"><span data-stu-id="1a62f-211">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooFeather HUZZAH ESP8266.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="1a62f-212">Adja meg hitelesítő adatait</span><span class="sxs-lookup"><span data-stu-id="1a62f-212">Enter your credentials</span></span>

<span data-ttu-id="1a62f-213">Ha sikeresen befejeződött a feltöltés hello, kövesse ezeket a lépéseket tooenter a hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="1a62f-213">After hello upload completes successfully, follow these steps tooenter your credentials:</span></span>

1. <span data-ttu-id="1a62f-214">Kattintson Arduino IDE hello, **eszközök** > **soros figyelő**.</span><span class="sxs-lookup"><span data-stu-id="1a62f-214">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="1a62f-215">Hello soros figyelő ablakban figyelje meg, a két hello legördülő listákból hello jobb alsó sarkában található.</span><span class="sxs-lookup"><span data-stu-id="1a62f-215">In hello serial monitor window, notice hello two drop-down lists in hello lower-right corner.</span></span>
1. <span data-ttu-id="1a62f-216">Válassza ki **nincs sor befejezési** hello bal oldali legördülő listát.</span><span class="sxs-lookup"><span data-stu-id="1a62f-216">Select **No line ending** for hello left drop-down list.</span></span>
1. <span data-ttu-id="1a62f-217">Válassza ki **115200 átviteli** hello jobb legördülő listát.</span><span class="sxs-lookup"><span data-stu-id="1a62f-217">Select **115200 baud** for hello right drop-down list.</span></span>
1. <span data-ttu-id="1a62f-218">Hello beviteli mezőbe hello soros figyelő ablak hello tetején található, írja be a következő információ, ha a rendszer felkéri tooprovide hello őket, és kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="1a62f-218">In hello input box located at hello top of hello serial monitor window, enter hello following information if you are asked tooprovide them, and then click **Send**.</span></span>
   * <span data-ttu-id="1a62f-219">Wi-Fi SSID</span><span class="sxs-lookup"><span data-stu-id="1a62f-219">Wi-Fi SSID</span></span>
   * <span data-ttu-id="1a62f-220">Wi-Fi jelszó</span><span class="sxs-lookup"><span data-stu-id="1a62f-220">Wi-Fi password</span></span>
   * <span data-ttu-id="1a62f-221">Eszköz kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1a62f-221">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="1a62f-222">hello hitelesítő adatai hello EEPROM a lágyított HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="1a62f-222">hello credential information is stored in hello EEPROM of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="1a62f-223">Ha hello lágyított HUZZAH ESP8266 board hello Visszaállítás gombra kattint, hello mintaalkalmazás megkérdezi tooerase hello információkat.</span><span class="sxs-lookup"><span data-stu-id="1a62f-223">If you click hello reset button on hello Feather HUZZAH ESP8266 board, hello sample application asks if you want tooerase hello information.</span></span> <span data-ttu-id="1a62f-224">Adja meg `Y` toohave hello adatokat törölni.</span><span class="sxs-lookup"><span data-stu-id="1a62f-224">Enter `Y` toohave hello information erased.</span></span> <span data-ttu-id="1a62f-225">Tooprovide hello információ még egyszer kell adnia.</span><span class="sxs-lookup"><span data-stu-id="1a62f-225">You are asked tooprovide hello information a second time.</span></span>

### <a name="verify-hello-sample-application-is-running-successfully"></a><span data-ttu-id="1a62f-226">Győződjön meg arról hello mintaalkalmazás sikeresen fut.</span><span class="sxs-lookup"><span data-stu-id="1a62f-226">Verify hello sample application is running successfully</span></span>

<span data-ttu-id="1a62f-227">Ha a hello hello soros figyelő ablakból parancskimenet, és hello villogó LED a lágyított HUZZAH ESP8266 hello mintaalkalmazás sikeresen fut-e.</span><span class="sxs-lookup"><span data-stu-id="1a62f-227">If you see hello following output from hello serial monitor window and hello blinking LED on Feather HUZZAH ESP8266, hello sample application is running successfully.</span></span>

![Végső kimenetet a Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="1a62f-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1a62f-229">Next steps</span></span>

<span data-ttu-id="1a62f-230">Sikeresen csatlakozott egy lágyított HUZZAH ESP8266 tooyour IoT-központ, és rögzített hello érzékelő adatokat tooyour IoT-központ küldött.</span><span class="sxs-lookup"><span data-stu-id="1a62f-230">You have successfully connected a Feather HUZZAH ESP8266 tooyour IoT hub, and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

