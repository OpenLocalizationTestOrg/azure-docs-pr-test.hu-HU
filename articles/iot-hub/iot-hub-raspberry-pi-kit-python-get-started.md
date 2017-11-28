---
title: "aaaRaspberry Pi toocloud (Python) - csatlakozás málna Pi tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban málna Pi tooAzure IoT-központ málna Pi toosend adatok toohello Azure cloud platform csatlakoznak."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot raspberry pi raspberry pi iot hub, raspberry pi küldési adatok toocloud raspberry pi toocloud"
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/31/2017
ms.author: xshi
ms.openlocfilehash: 86f5c91ab9dd4e23c563437827fb7d2d06916d2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-python"></a><span data-ttu-id="031d6-104">Csatlakozás málna Pi tooAzure IoT Hub (Python)</span><span class="sxs-lookup"><span data-stu-id="031d6-104">Connect Raspberry Pi tooAzure IoT Hub (Python)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="031d6-105">Ebben az oktatóanyagban akkor először tanulási hello használatának alapjait málna Pi Raspbian futtató.</span><span class="sxs-lookup"><span data-stu-id="031d6-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="031d6-106">Majd megismerheti, hogyan tooseamlessly összekapcsolni használatával eszközök toohello felhőalapú [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="031d6-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="031d6-107">Windows 10 IoT minta, nyissa meg a toohello [Windows fejlesztői központ](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="031d6-107">For Windows 10 IoT Core samples, go toohello [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="031d6-108">Még nem rendelkezik egy csomagot?</span><span class="sxs-lookup"><span data-stu-id="031d6-108">Don't have a kit yet?</span></span> <span data-ttu-id="031d6-109">Próbálja [málna Pi online szimulátor](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="031d6-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="031d6-110">Vagy egy új csomag vásárlása [Itt](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="031d6-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="031d6-111">Mit</span><span class="sxs-lookup"><span data-stu-id="031d6-111">What you do</span></span>

* <span data-ttu-id="031d6-112">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="031d6-112">Create an IoT hub.</span></span>
* <span data-ttu-id="031d6-113">Eszköz regisztrálása az IoT hub a pi.</span><span class="sxs-lookup"><span data-stu-id="031d6-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="031d6-114">A telepítő Raspberry Pi.</span><span class="sxs-lookup"><span data-stu-id="031d6-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="031d6-115">Futtassa a mintaalkalmazást Pi toosend érzékelő adatokat tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="031d6-115">Run a sample application on Pi toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="031d6-116">Csatlakozás málna Pi tooan IoT-központ az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="031d6-116">Connect Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="031d6-117">Akkor futtassa a mintaalkalmazást Pi toocollect hőmérséklet és a páratartalom adatok BME280 érzékelő.</span><span class="sxs-lookup"><span data-stu-id="031d6-117">Then you run a sample application on Pi toocollect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="031d6-118">Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="031d6-118">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="031d6-119">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="031d6-119">What you learn</span></span>

* <span data-ttu-id="031d6-120">Hogyan toocreate az Azure IoT-központ és az új eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="031d6-120">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="031d6-121">Hogyan tooconnect BME280 érzékelő a pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="031d6-121">How tooconnect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="031d6-122">Hogyan toocollect érzékelőadatait Pi mintaalkalmazás futtatásával.</span><span class="sxs-lookup"><span data-stu-id="031d6-122">How toocollect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="031d6-123">Hogyan toosend érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="031d6-123">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="031d6-124">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="031d6-124">What you need</span></span>

![Mi szükséges](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* <span data-ttu-id="031d6-126">hello málna Pi 2 vagy málna Pi 3.</span><span class="sxs-lookup"><span data-stu-id="031d6-126">hello Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="031d6-127">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="031d6-127">An active Azure subscription.</span></span> <span data-ttu-id="031d6-128">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="031d6-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="031d6-129">A figyelő egy USB-billentyűzet és egér tooPi-hez.</span><span class="sxs-lookup"><span data-stu-id="031d6-129">A monitor, a USB keyboard, and mouse that connect tooPi.</span></span>
* <span data-ttu-id="031d6-130">A Mac vagy a Windows vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="031d6-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="031d6-131">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="031d6-131">An Internet connection.</span></span>
* <span data-ttu-id="031d6-132">Egy 16 GB-os vagy újabb microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="031d6-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="031d6-133">Egy USB-SD adapter vagy microSD kártya tooburn hello operációsrendszer-képet hello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="031d6-133">A USB-SD adapter or microSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="031d6-134">Egy 5-volt 2-amp tápegység hello 6-mértékű kiszolgálóhasználat micro USB-kábellel.</span><span class="sxs-lookup"><span data-stu-id="031d6-134">A 5-volt 2-amp power supply with hello 6-foot micro USB cable.</span></span>

<span data-ttu-id="031d6-135">a következő elemek hello nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="031d6-135">hello following items are optional:</span></span>

* <span data-ttu-id="031d6-136">Az összeállított Adafruit BME280 hőmérséklet, a terhelés, és a páratartalom érzékelő.</span><span class="sxs-lookup"><span data-stu-id="031d6-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="031d6-137">Egy breadboard.</span><span class="sxs-lookup"><span data-stu-id="031d6-137">A breadboard.</span></span>
* <span data-ttu-id="031d6-138">6/aljzat átkötés fenyegetéseknek.</span><span class="sxs-lookup"><span data-stu-id="031d6-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="031d6-139">Egy szórt 10 mm LED-jét.</span><span class="sxs-lookup"><span data-stu-id="031d6-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="031d6-140">Ezek az elemek nem kötelező, mert hello kód a minta támogatási szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="031d6-140">These items are optional because hello code sample support simulated sensor data.</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a><span data-ttu-id="031d6-141">Málna Pi beállítása</span><span class="sxs-lookup"><span data-stu-id="031d6-141">Set up Raspberry Pi</span></span>

### <a name="install-hello-raspbian-operating-system-for-pi"></a><span data-ttu-id="031d6-142">A Pi hello Raspbian operációs rendszer telepítése</span><span class="sxs-lookup"><span data-stu-id="031d6-142">Install hello Raspbian operating system for Pi</span></span>

<span data-ttu-id="031d6-143">Készítse elő a hello microSD-kártyán hello Raspbian lemezkép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="031d6-143">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="031d6-144">Töltse le a Raspbian.</span><span class="sxs-lookup"><span data-stu-id="031d6-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="031d6-145">[Töltse le az asztallal Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip fájl).</span><span class="sxs-lookup"><span data-stu-id="031d6-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip file).</span></span>
   1. <span data-ttu-id="031d6-146">Bontsa ki a hello Raspbian kép tooa mappát a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="031d6-146">Extract hello Raspbian image tooa folder on your computer.</span></span>
1. <span data-ttu-id="031d6-147">Telepítse a Raspbian toohello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="031d6-147">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="031d6-148">[Töltse le és telepítse a hello Etcher SD-kártya író segédprogram](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="031d6-148">[Download and install hello Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="031d6-149">Futtassa a Etcher és hello Raspbian kép kibontott válassza az 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="031d6-149">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="031d6-150">Válassza ki a hello microSD-kártyát meghajtó.</span><span class="sxs-lookup"><span data-stu-id="031d6-150">Select hello microSD card drive.</span></span> <span data-ttu-id="031d6-151">Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott hello megfelelő meghajtót.</span><span class="sxs-lookup"><span data-stu-id="031d6-151">Note that Etcher may have already selected hello correct drive.</span></span>
   1. <span data-ttu-id="031d6-152">Kattintson a Flash tooinstall Raspbian toohello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="031d6-152">Click Flash tooinstall Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="031d6-153">Ha a telepítés hello microSD-kártyán eltávolítása a számítógépről.</span><span class="sxs-lookup"><span data-stu-id="031d6-153">Remove hello microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="031d6-154">Ennek az oka biztonságos tooremove hello microSD-kártyán közvetlenül Etcher automatikusan kiadása vagy leválasztja hello microSD-kártyán befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="031d6-154">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   1. <span data-ttu-id="031d6-155">A Pi hello microSD-kártyán beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="031d6-155">Insert hello microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="031d6-156">SSH- és I2C engedélyezése</span><span class="sxs-lookup"><span data-stu-id="031d6-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="031d6-157">Csatlakozás Pi toohello monitor, billentyűzet és egér. Indítsa el a Pi, majd jelentkezzen be Raspbian `pi` a hello felhasználónévvel és `raspberry` hello jelszóként.</span><span class="sxs-lookup"><span data-stu-id="031d6-157">Connect Pi toohello monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as hello user name and `raspberry` as hello password.</span></span>
1. <span data-ttu-id="031d6-158">Kattintson a hello Raspberry ikon > **beállítások** > **málna Pi konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="031d6-158">Click hello Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![hello Raspbian beállítások menü](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="031d6-160">A hello **felületek** lapon **I2C** és **SSH** túl**engedélyezése**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="031d6-160">On hello **Interfaces** tab, set **I2C** and **SSH** too**Enable**, and then click **OK**.</span></span> <span data-ttu-id="031d6-161">Ha nem rendelkezik a fizikai érzékelők, és szeretné, hogy szimulált toouse érzékelőadatait, ez a lépés nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="031d6-161">If you don't have physical sensors and want toouse simulated sensor data, this step is optional.</span></span>

   ![I2C és a Raspberry Pi SSH engedélyezése](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="031d6-163">tooenable SSH és I2C található további referencia dokumentumok [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) és [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span><span class="sxs-lookup"><span data-stu-id="031d6-163">tooenable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span></span>

### <a name="connect-hello-sensor-toopi"></a><span data-ttu-id="031d6-164">Csatlakozás hello érzékelő tooPi</span><span class="sxs-lookup"><span data-stu-id="031d6-164">Connect hello sensor tooPi</span></span>

<span data-ttu-id="031d6-165">Hello breadboard és átkötés fenyegetéseknek tooconnect LED és egy BME280 tooPi a következőképpen használhatja.</span><span class="sxs-lookup"><span data-stu-id="031d6-165">Use hello breadboard and jumper wires tooconnect an LED and a BME280 tooPi as follows.</span></span> <span data-ttu-id="031d6-166">Ha még nem rendelkezik hello érzékelő [hagyja ki ezt a szakaszt](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="031d6-166">If you don’t have hello sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![hello málna Pi és érzékelő kapcsolat](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="031d6-168">hello BME280 érzékelő gyűjthet a hőmérséklet és a páratartalom adatokat.</span><span class="sxs-lookup"><span data-stu-id="031d6-168">hello BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="031d6-169">És hello LED fog villogni, ha egy eszköz és a hello közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="031d6-169">And hello LED will blink if there is a communication between device and hello cloud.</span></span> 

<span data-ttu-id="031d6-170">Érzékelő PIN-kód használja a következő vezetékezést hello:</span><span class="sxs-lookup"><span data-stu-id="031d6-170">For sensor pins, use hello following wiring:</span></span>

| <span data-ttu-id="031d6-171">Indítsa el a (érzékelő & LED)</span><span class="sxs-lookup"><span data-stu-id="031d6-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="031d6-172">Záró (tábla)</span><span class="sxs-lookup"><span data-stu-id="031d6-172">End (Board)</span></span>            | <span data-ttu-id="031d6-173">Kábel szín</span><span class="sxs-lookup"><span data-stu-id="031d6-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="031d6-174">VDD (PIN-kód 5 g.)</span><span class="sxs-lookup"><span data-stu-id="031d6-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="031d6-175">3.3V PWR (1. Pin)</span><span class="sxs-lookup"><span data-stu-id="031d6-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="031d6-176">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="031d6-176">White cable</span></span>   |
| <span data-ttu-id="031d6-177">GND (7G PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="031d6-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="031d6-178">GND (PIN-kód 6)</span><span class="sxs-lookup"><span data-stu-id="031d6-178">GND (Pin 6)</span></span>            | <span data-ttu-id="031d6-179">Barna kábel</span><span class="sxs-lookup"><span data-stu-id="031d6-179">Brown cable</span></span>   |
| <span data-ttu-id="031d6-180">SDI (PIN-kód 10G)</span><span class="sxs-lookup"><span data-stu-id="031d6-180">SDI (Pin 10G)</span></span>            | <span data-ttu-id="031d6-181">I2C1 SDA (3 PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="031d6-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="031d6-182">Piros kábel</span><span class="sxs-lookup"><span data-stu-id="031d6-182">Red cable</span></span>     |
| <span data-ttu-id="031d6-183">SCK (8G PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="031d6-183">SCK (Pin 8G)</span></span>             | <span data-ttu-id="031d6-184">I2C1 SCL (PIN-kód 5)</span><span class="sxs-lookup"><span data-stu-id="031d6-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="031d6-185">Narancssárga kábel</span><span class="sxs-lookup"><span data-stu-id="031d6-185">Orange cable</span></span>  |
| <span data-ttu-id="031d6-186">LED VDD (PIN-kód 18F)</span><span class="sxs-lookup"><span data-stu-id="031d6-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="031d6-187">GPIO 24 (PIN-kód 18)</span><span class="sxs-lookup"><span data-stu-id="031d6-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="031d6-188">A fehér kábel</span><span class="sxs-lookup"><span data-stu-id="031d6-188">White cable</span></span>   |
| <span data-ttu-id="031d6-189">LED GND (PIN-kód 17F)</span><span class="sxs-lookup"><span data-stu-id="031d6-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="031d6-190">GND (PIN-kód 20)</span><span class="sxs-lookup"><span data-stu-id="031d6-190">GND (Pin 20)</span></span>           | <span data-ttu-id="031d6-191">Fekete kábel</span><span class="sxs-lookup"><span data-stu-id="031d6-191">Black cable</span></span>   |

<span data-ttu-id="031d6-192">Kattintson a tooview [málna Pi 2. és 3 PIN-kód hozzárendelések](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="031d6-192">Click tooview [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="031d6-193">Miután sikeresen csatlakozott BME280 tooyour málna Pi, meg kell például a kép alatt.</span><span class="sxs-lookup"><span data-stu-id="031d6-193">After you've successfully connected BME280 tooyour Raspberry Pi, it should be like below image.</span></span>

![Csatlakoztatott Pi és BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a><span data-ttu-id="031d6-195">A Pi toohello hálózat</span><span class="sxs-lookup"><span data-stu-id="031d6-195">Connect Pi toohello network</span></span>

<span data-ttu-id="031d6-196">Kapcsolja be a Pi hello micro USB-kábelen és hello tápegység.</span><span class="sxs-lookup"><span data-stu-id="031d6-196">Turn on Pi by using hello micro USB cable and hello power supply.</span></span> <span data-ttu-id="031d6-197">Használjon hello Ethernet kábel tooconnect Pi tooyour vezetékes hálózati, vagy hajtsa végre a hello [hello málna Pi Foundation utasításainak](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour vezeték nélküli hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="031d6-197">Use hello Ethernet cable tooconnect Pi tooyour wired network or follow hello [instructions from hello Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="031d6-198">A Pi sikeresen csatlakoztatott toohello hálózati után tootake hello jegyezze fel kell [IP-címet a pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="031d6-198">After your Pi has been successfully connected toohello network, you need tootake a note of hello [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Csatlakoztatott toowired hálózati](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="031d6-200">Győződjön meg arról, hogy Pi ugyanaz, mint a számítógép hálózati csatlakoztatott toohello.</span><span class="sxs-lookup"><span data-stu-id="031d6-200">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="031d6-201">Például, ha a számítógép vezeték nélküli hálózathoz csatlakoztatott tooa Pi pedig vezetékes hálózathoz csatlakoztatott tooa, akkor előfordulhat, hogy nem lásd: hello IP-cím hello devdisco kimenet.</span><span class="sxs-lookup"><span data-stu-id="031d6-201">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="031d6-202">A Pi mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="031d6-202">Run a sample application on Pi</span></span>

### <a name="install-hello-prerequisite-packages"></a><span data-ttu-id="031d6-203">Hello előfeltételként szükséges csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="031d6-203">Install hello prerequisite packages</span></span>

<span data-ttu-id="031d6-204">SSH-ügyfél követően a gazdagép számítógép tooconnect tooyour málna Pi hello egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="031d6-204">Use one of hello following SSH clients from your host computer tooconnect tooyour Raspberry Pi.</span></span>
   
   <span data-ttu-id="031d6-205">**Windows-felhasználók**</span><span class="sxs-lookup"><span data-stu-id="031d6-205">**Windows Users**</span></span>
   1. <span data-ttu-id="031d6-206">Töltse le és telepítse [PuTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="031d6-206">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="031d6-207">Másolja a Pi hello gazdagép nevét (vagy IP-cím) szakasz hello IP-címét, és válassza ki az SSH hello kapcsolattípus.</span><span class="sxs-lookup"><span data-stu-id="031d6-207">Copy hello IP address of your Pi into hello Host name (or IP address) section and select SSH as hello connection type.</span></span>
   
   
   <span data-ttu-id="031d6-208">**Mac és Ubuntu felhasználók**</span><span class="sxs-lookup"><span data-stu-id="031d6-208">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="031d6-209">Ubuntu vagy macOS hello beépített SSH-ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="031d6-209">Use hello built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="031d6-210">Előfordulhat, hogy toorun `ssh pi@<ip address of pi>` tooconnect Pi SSH-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="031d6-210">You might need toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="031d6-211">hello alapértelmezett felhasználónév az `pi` , és hello jelszó `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="031d6-211">hello default username is `pi` , and hello password is `raspberry`.</span></span>


### <a name="configure-hello-sample-application"></a><span data-ttu-id="031d6-212">Hello mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="031d6-212">Configure hello sample application</span></span>

1. <span data-ttu-id="031d6-213">Klónozza a mintaalkalmazást hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="031d6-213">Clone hello sample application by running hello following command:</span></span>

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. <span data-ttu-id="031d6-214">Nyissa meg a konfigurációs fájl hello hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="031d6-214">Open hello config file by running hello following commands:</span></span>

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   <span data-ttu-id="031d6-215">5 makrók van ebben a fájlban configurate is.</span><span class="sxs-lookup"><span data-stu-id="031d6-215">There are 5 macros in this file you can configurate.</span></span> <span data-ttu-id="031d6-216">hello először egy van `MESSAGE_TIMESPAN`, amely megadja, hogy hello időtartam (ezredmásodpercben) két toocloud küldött üzenetek között.</span><span class="sxs-lookup"><span data-stu-id="031d6-216">hello first one is `MESSAGE_TIMESPAN`, which defines hello time interval (in milliseconds) between two messages that send toocloud.</span></span> <span data-ttu-id="031d6-217">második hello `SIMULATED_DATA`, vagyis az, hogy toouse szimulált-e érzékelőadatait vagy nem logikai értéket.</span><span class="sxs-lookup"><span data-stu-id="031d6-217">hello second one `SIMULATED_DATA`, which is a Boolean value for whether toouse simulated sensor data or not.</span></span> <span data-ttu-id="031d6-218">`I2C_ADDRESS`hello I2C cím a BME280 érzékelő csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="031d6-218">`I2C_ADDRESS` is hello I2C address which your BME280 sensor is connected.</span></span> <span data-ttu-id="031d6-219">`GPIO_PIN_ADDRESS`hello GPIO cím szolgál a LED-jét.</span><span class="sxs-lookup"><span data-stu-id="031d6-219">`GPIO_PIN_ADDRESS` is hello GPIO address for your LED.</span></span> <span data-ttu-id="031d6-220">hello utolsó még `BLINK_TIMESPAN`, amely hello timespan megadva, amikor a LED ezredmásodpercben be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="031d6-220">hello last one is `BLINK_TIMESPAN`, which defined hello timespan when your LED is turned on in milliseconds.</span></span>

   <span data-ttu-id="031d6-221">Ha Ön **nincs hello érzékelő**, beállíthatja hello `SIMULATED_DATA` érték túl`True` toomake hello mintaalkalmazás létrehozása, és használjon szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="031d6-221">If you **don't have hello sensor**, set hello `SIMULATED_DATA` value too`True` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="031d6-222">Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="031d6-222">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="build-and-run-hello-sample-application"></a><span data-ttu-id="031d6-223">Hozza létre és hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="031d6-223">Build and run hello sample application</span></span>

1. <span data-ttu-id="031d6-224">Build hello mintaalkalmazás hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="031d6-224">Build hello sample application by running hello following command.</span></span> <span data-ttu-id="031d6-225">Mivel hello Azure IoT készült SDK-k Python burkolók hello Azure IoT eszköz C SDK fölött, ha azt szeretné, vagy toogenerate hello Python-könyvtárakat a forráskód kell kell toocompile hello C-függvénytárak.</span><span class="sxs-lookup"><span data-stu-id="031d6-225">Because hello Azure IoT SDKs for Python are wrappers on top of hello Azure IoT Device C SDK, you will need toocompile hello C libraries if you want or need toogenerate hello Python libraries from source code.</span></span>

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   <span data-ttu-id="031d6-226">Azt is megadhatja, futtassa a kívánt hello verzió `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`.</span><span class="sxs-lookup"><span data-stu-id="031d6-226">You can also specify hello version you want by running `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`.</span></span> <span data-ttu-id="031d6-227">Ha paraméter nélkül parancsfájlt futtatja, hello parancsfájl automatikusan észleli, python telepített verziójának hello (keresési feladatütemezési 2.7 -> 3.4 -> 3.5).</span><span class="sxs-lookup"><span data-stu-id="031d6-227">If you run script without parameter, hello script will automatically detect hello version of python installed (Search sequence 2.7->3.4->3.5).</span></span> <span data-ttu-id="031d6-228">Győződjön meg arról, hogy a Python verzió tartja konzisztens során kialakításához és futtatásához.</span><span class="sxs-lookup"><span data-stu-id="031d6-228">Make sure your Python version keeps consistent during building and running.</span></span> 
   
   > [!NOTE] 
   <span data-ttu-id="031d6-229">Épület hello Python ügyféloldali kódtár (iothub_client.so) kisebb, mint 1GB RAM-MAL rendelkező Linux rendszerű eszközökön, láthatja a build a lent látható módon iothub_client_python.cpp felépítésekor első Beragadt 98 % `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`.</span><span class="sxs-lookup"><span data-stu-id="031d6-229">On building hello Python client library (iothub_client.so) on Linux devices that have less than 1GB RAM, you may see build getting stuck at 98% while building iothub_client_python.cpp as shown below `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`.</span></span> <span data-ttu-id="031d6-230">Ha ezt a problémát tapasztal, ellenőrizze a hello memóriahasználatának hello eszköz használatával `free -m command` egy másik terminál ablakban ebben az időszakban.</span><span class="sxs-lookup"><span data-stu-id="031d6-230">If you run into this issue, check hello memory consumption of hello device using `free -m command` in another terminal window during that time.</span></span> <span data-ttu-id="031d6-231">Ha kevés a memória iothub_client_python.cpp fájl fordítása közben fut, előfordulhat, hogy növelje a lapozófájl-terület tooget hello tootemporarily több elérhető memória toosuccessfully hello Python ügyféloldali eszköz SDK-könyvtár létrehozása.</span><span class="sxs-lookup"><span data-stu-id="031d6-231">If you are running out of memory while compiling iothub_client_python.cpp file, you may have tootemporarily increase hello swap space tooget more available memory toosuccessfully build hello Python client-side device SDK library.</span></span>
   
1. <span data-ttu-id="031d6-232">Futtassa a mintaalkalmazást hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="031d6-232">Run hello sample application by running hello following command:</span></span>

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="031d6-233">Győződjön meg arról, hogy Ön-e beillesztési hello eszköz kapcsolati karakterláncot a hello szimpla idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="031d6-233">Make sure you copy-paste hello device connection string into hello single quotes.</span></span> <span data-ttu-id="031d6-234">Ha hello python 3, akkor is használhatja és hello parancs `python3 app.py '<your Azure IoT hub device connection string>'`.</span><span class="sxs-lookup"><span data-stu-id="031d6-234">And if you use hello python 3, then you can use hello command `python3 app.py '<your Azure IoT hub device connection string>'`.</span></span>


   <span data-ttu-id="031d6-235">Meg kell jelennie a hello parancskimenet, hogy látható hello érzékelő adatokat és hello küldött üzenetek tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="031d6-235">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

   ![Kimeneti - érzékelő adatokat küldött az málna Pi tooyour IoT-központ](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a><span data-ttu-id="031d6-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="031d6-237">Next steps</span></span>

<span data-ttu-id="031d6-238">Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="031d6-238">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span> <span data-ttu-id="031d6-239">a málna Pi egy parancssori felületen, a elküldte tooyour IoT hub vagy küldési üzenetek tooyour málna Pi toosee köszönőüzenetei lásd: hello [kezelése felhő eszközt az IOT hubbal-explorer oktatóanyag üzenetküldési](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="031d6-239">toosee hello messages that your Raspberry Pi has sent tooyour IoT hub or send messages tooyour Raspberry Pi in a command line interface, see hello [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
