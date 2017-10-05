---
title: "Intel Edison felhőbe (Node.js) - Intel Edison csatlakozzon az Azure IoT Hub |} Microsoft Docs"
description: "Megtudhatja, hogyan kell beállítania, és Azure IoT-központ Intel Edison adatokat küldeni az Azure felhőalapú platform ebben az oktatóanyagban Intel Edison csatlakozni."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot intel edison, intel edison iot-központot, intel edison adatküldés intel felhőbe edison felhőbe"
ms.assetid: a7c9cf2d-c102-41b0-aa45-41285c6877eb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/15/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5a31efba704045196b5563f7bc467c773bea7805
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-intel-edison-to-azure-iot-hub-nodejs"></a><span data-ttu-id="ab215-104">Intel Edison csatlakozni az Azure IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="ab215-104">Connect Intel Edison to Azure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="ab215-105">Ebben az oktatóanyagban akkor először tanulás alapjainak Intel Edison használata.</span><span class="sxs-lookup"><span data-stu-id="ab215-105">In this tutorial, you begin by learning the basics of working with Intel Edison.</span></span> <span data-ttu-id="ab215-106">Majd megtudhatja, hogyan kapcsolódhat zökkenőmentesen az eszközök a felhőbe [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="ab215-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

<span data-ttu-id="ab215-107">Még nem rendelkezik egy csomagot?</span><span class="sxs-lookup"><span data-stu-id="ab215-107">Don't have a kit yet?</span></span> <span data-ttu-id="ab215-108">Start [Itt](https://azure.microsoft.com/develop/iot/starter-kits)</span><span class="sxs-lookup"><span data-stu-id="ab215-108">Start [here](https://azure.microsoft.com/develop/iot/starter-kits)</span></span>

## <a name="what-you-do"></a><span data-ttu-id="ab215-109">Mit</span><span class="sxs-lookup"><span data-stu-id="ab215-109">What you do</span></span>

* <span data-ttu-id="ab215-110">A telepítő Intel Edison és és Groove-modulok.</span><span class="sxs-lookup"><span data-stu-id="ab215-110">Setup Intel Edison and and Grove modules.</span></span>
* <span data-ttu-id="ab215-111">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="ab215-111">Create an IoT hub.</span></span>
* <span data-ttu-id="ab215-112">Eszköz regisztrálása az Edison az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="ab215-112">Register a device for Edison in your IoT hub.</span></span>
* <span data-ttu-id="ab215-113">Futtassa a mintaalkalmazást érzékelő adatokat küldeni az IoT hub Edison.</span><span class="sxs-lookup"><span data-stu-id="ab215-113">Run a sample application on Edison to send sensor data to your IoT hub.</span></span>

<span data-ttu-id="ab215-114">Az IoT-központ az Ön által létrehozott Intel Edison csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="ab215-114">Connect Intel Edison to an IoT hub that you create.</span></span> <span data-ttu-id="ab215-115">Majd futtassa a mintaalkalmazást a Edison hőmérséklet és a páratartalom adatokat gyűjteni a Groove hőmérséklet-érzékelő.</span><span class="sxs-lookup"><span data-stu-id="ab215-115">Then you run a sample application on Edison to collect temperature and humidity data from a Grove temperature sensor.</span></span> <span data-ttu-id="ab215-116">Végezetül az érzékelő adatokat küldött az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab215-116">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="ab215-117">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="ab215-117">What you learn</span></span>

* <span data-ttu-id="ab215-118">Megtudhatja, hogyan hozzon létre egy Azure IoT-központot, és az új eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="ab215-118">How to create an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="ab215-119">Hogyan Edison kapcsolódni egy Groove hőmérséklet-érzékelő.</span><span class="sxs-lookup"><span data-stu-id="ab215-119">How to connect Edison with a Grove temperature sensor.</span></span>
* <span data-ttu-id="ab215-120">Megtudhatja, hogyan futtatja a mintaalkalmazás Edison érzékelő adatok gyűjtéséért felelős ügyfélfeladatot.</span><span class="sxs-lookup"><span data-stu-id="ab215-120">How to collect sensor data by running a sample application on Edison.</span></span>
* <span data-ttu-id="ab215-121">Hogyan érzékelő adatokat küldeni az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab215-121">How to send sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="ab215-122">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="ab215-122">What you need</span></span>

![Mi szükséges](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* <span data-ttu-id="ab215-124">Az Intel Edison Tanács</span><span class="sxs-lookup"><span data-stu-id="ab215-124">The Intel Edison board</span></span>
* <span data-ttu-id="ab215-125">Arduino bővítése tábla</span><span class="sxs-lookup"><span data-stu-id="ab215-125">Arduino expansion board</span></span>
* <span data-ttu-id="ab215-126">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ab215-126">An active Azure subscription.</span></span> <span data-ttu-id="ab215-127">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="ab215-127">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="ab215-128">A Mac vagy a Windows vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="ab215-128">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="ab215-129">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="ab215-129">An Internet connection.</span></span>
* <span data-ttu-id="ab215-130">A típus egy USB-kábel Micro B</span><span class="sxs-lookup"><span data-stu-id="ab215-130">A Micro B to Type A USB cable</span></span>
* <span data-ttu-id="ab215-131">A közvetlen aktuális (DC) tápegység.</span><span class="sxs-lookup"><span data-stu-id="ab215-131">A direct current (DC) power supply.</span></span> <span data-ttu-id="ab215-132">A tápegység kell tekinthető meg az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ab215-132">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="ab215-133">7-15V TARTOMÁNYVEZÉRLŐ</span><span class="sxs-lookup"><span data-stu-id="ab215-133">7-15V DC</span></span>
  - <span data-ttu-id="ab215-134">Legalább 1500mA</span><span class="sxs-lookup"><span data-stu-id="ab215-134">At least 1500mA</span></span>
  - <span data-ttu-id="ab215-135">A Központ/belső PIN-kódot kell lennie a pozitív sarkpontot az energiaellátás</span><span class="sxs-lookup"><span data-stu-id="ab215-135">The center/inner pin should be the positive pole of the power supply</span></span>

<span data-ttu-id="ab215-136">A következő elemek nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="ab215-136">The following items are optional:</span></span>

* <span data-ttu-id="ab215-137">Groove alap pajzs V2</span><span class="sxs-lookup"><span data-stu-id="ab215-137">Grove Base Shield V2</span></span>
* <span data-ttu-id="ab215-138">Groove - hőmérséklet-érzékelő</span><span class="sxs-lookup"><span data-stu-id="ab215-138">Grove - Temperature Sensor</span></span>
* <span data-ttu-id="ab215-139">Groove kábel</span><span class="sxs-lookup"><span data-stu-id="ab215-139">Grove Cable</span></span>
* <span data-ttu-id="ab215-140">Bármely Oszlopelválasztó sávok vagy csavart a csomagban, beleértve a két csavart vizsgálókocsihoz a bővítés board és négy csavart és műanyag térköztartók modul a része.</span><span class="sxs-lookup"><span data-stu-id="ab215-140">Any spacer bars or screws included in the packaging, including two screws to fasten the module to the expansion board and four sets of screws and plastic spacers.</span></span>

> [!NOTE] 
<span data-ttu-id="ab215-141">Ezek az elemek nem kötelező, mert a kód a minta támogatási szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="ab215-141">These items are optional because the code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a><span data-ttu-id="ab215-142">A telepítő Intel Edison</span><span class="sxs-lookup"><span data-stu-id="ab215-142">Setup Intel Edison</span></span>

### <a name="assemble-your-board"></a><span data-ttu-id="ab215-143">A tábla összeállítása</span><span class="sxs-lookup"><span data-stu-id="ab215-143">Assemble your board</span></span>

<span data-ttu-id="ab215-144">Ez a szakasz az Intel® Edison modul csatlakoztatni a bővítés board lépéseket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ab215-144">This section contains steps to attach your Intel® Edison module to your expansion board.</span></span>

1. <span data-ttu-id="ab215-145">A bővítés üzenőfalon, a bővítés táblán csavart, a modul a lyuk sorba állítása az Intel® Edison modul, a fehér vázlatban helyezze el.</span><span class="sxs-lookup"><span data-stu-id="ab215-145">Place the Intel® Edison module within the white outline on your expansion board, lining up the holes on the module with the screws on the expansion board.</span></span>

2. <span data-ttu-id="ab215-146">Nyomja le a modul a szavakat alatt `What will you make?` csak úgy érzi, hogy a beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="ab215-146">Press down on the module just below the words `What will you make?` until you feel a snap.</span></span>

   ![indítópanel 2 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. <span data-ttu-id="ab215-148">A bővítés board modul védelmére használja a két hexadecimális nuts (a csomagban található).</span><span class="sxs-lookup"><span data-stu-id="ab215-148">Use the two hex nuts (included in the package) to secure the module to the expansion board.</span></span>

   ![indítópanel 3 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. <span data-ttu-id="ab215-150">Helyezze be elment egyet a négy sarok lyuk a bővítés táblán.</span><span class="sxs-lookup"><span data-stu-id="ab215-150">Insert a screw in one of the four corner holes on the expansion board.</span></span> <span data-ttu-id="ab215-151">Csavarás és szigoríthatja a csavart alakzatot fehér műanyag térköztartók egyikét.</span><span class="sxs-lookup"><span data-stu-id="ab215-151">Twist and tighten one of the white plastic spacers onto the screw.</span></span>

   ![indítópanel 4 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. <span data-ttu-id="ab215-153">Ismételje meg a másik három sarok térköztartók.</span><span class="sxs-lookup"><span data-stu-id="ab215-153">Repeat for the other three corner spacers.</span></span>

   ![indítópanel 5 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

<span data-ttu-id="ab215-155">Most már a üzenőfalon kész.</span><span class="sxs-lookup"><span data-stu-id="ab215-155">Now your board is assembled.</span></span>

   ![üzenőfalon kész](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a><span data-ttu-id="ab215-157">Csatlakoztassa a Groove talál pajzs és a hőmérséklet-érzékelő</span><span class="sxs-lookup"><span data-stu-id="ab215-157">Connect the Grove Base Shield and the temperature sensor</span></span>

1. <span data-ttu-id="ab215-158">Jelölje be a tábla a Groove talál pajzs.</span><span class="sxs-lookup"><span data-stu-id="ab215-158">Place the Grove Base Shield on to your board.</span></span> <span data-ttu-id="ab215-159">Győződjön meg arról, hogy a tábla szorosan csatlakoztatott összes PIN-kód.</span><span class="sxs-lookup"><span data-stu-id="ab215-159">Make sure all pins are tightly plugged into your board.</span></span>
   
   ![Groove alap pajzs ikon](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. <span data-ttu-id="ab215-161">Groove kábellel csatlakoztassa a Groove talál pajzs alakzatot Groove hőmérséklet-érzékelő **A0** port.</span><span class="sxs-lookup"><span data-stu-id="ab215-161">Use Grove Cable to connect Grove temperature sensor onto the Grove Base Shield **A0** port.</span></span>

   ![Csatlakozás hőmérséklet-érzékelő](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Edison és érzékelő kapcsolat](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

<span data-ttu-id="ab215-164">Most már készen áll az érzékelő.</span><span class="sxs-lookup"><span data-stu-id="ab215-164">Now your sensor is ready.</span></span>

### <a name="power-up-edison"></a><span data-ttu-id="ab215-165">Energiagazdálkodási Edison mentése</span><span class="sxs-lookup"><span data-stu-id="ab215-165">Power up Edison</span></span>

1. <span data-ttu-id="ab215-166">Csatlakoztassa a tápegység.</span><span class="sxs-lookup"><span data-stu-id="ab215-166">Plug in the power supply.</span></span>

   ![Beépülő modul tápegység](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. <span data-ttu-id="ab215-168">A zöld LED-jét (DS1 feliratú a táblán Arduino * bővítése) kell bonyolít le, és megvilágítottnak maradnak.</span><span class="sxs-lookup"><span data-stu-id="ab215-168">A green LED(labeled DS1 on the Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="ab215-169">Várjon egy percet a kártya a rendszerindítás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="ab215-169">Wait one minute for the board to finish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ab215-170">Ha nem rendelkezik a tartományvezérlő tápegység, továbbra is a USB-porton keresztül board power.</span><span class="sxs-lookup"><span data-stu-id="ab215-170">If you do not have a DC power supply, you can still power the board through a USB port.</span></span> <span data-ttu-id="ab215-171">Lásd: `Connect Edison to your computer` című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="ab215-171">See `Connect Edison to your computer` section for details.</span></span> <span data-ttu-id="ab215-172">A tábla, ilyen módon működtetéséhez azt eredményezheti, hogy a tábláról előre nem látható viselkedéshez különösen akkor, ha a Wi-Fi használatával, vagy befolyásoló tényezők motorok.</span><span class="sxs-lookup"><span data-stu-id="ab215-172">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

### <a name="connect-edison-to-your-computer"></a><span data-ttu-id="ab215-173">Edison kapcsolódni a számítógéphez</span><span class="sxs-lookup"><span data-stu-id="ab215-173">Connect Edison to your computer</span></span>

1. <span data-ttu-id="ab215-174">Váltás a két micro USB-porttal felé mikrokapcsoló le, hogy Edison eszköz módban van.</span><span class="sxs-lookup"><span data-stu-id="ab215-174">Toggle down the microswitch towards the two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="ab215-175">Az eszköz és a gazdagép üzemmód közötti különbségeket, tekintse át [Itt](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="ab215-175">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Váltás a mikrokapcsoló le](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. <span data-ttu-id="ab215-177">A micro USB-kábellel csatlakoztassa a felső micro USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="ab215-177">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Felső micro USB-port](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. <span data-ttu-id="ab215-179">Másik végén USB-kábellel csatlakoztassa a számítógépet.</span><span class="sxs-lookup"><span data-stu-id="ab215-179">Plug the other end of USB cable into your computer.</span></span>

   ![Számítógép USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. <span data-ttu-id="ab215-181">Tudni fogják, hogy a tábla teljes inicializálását, amikor a számítógép csatlakoztat egy új meghajtót (hasonlóan egy SD-kártya beszúrása a számítógép).</span><span class="sxs-lookup"><span data-stu-id="ab215-181">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-the-configuration-tool"></a><span data-ttu-id="ab215-182">A konfigurációs eszköz letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="ab215-182">Download and run the configuration tool</span></span>
<span data-ttu-id="ab215-183">A legfrissebb konfigurációs eszköz az beszerzése [erre a hivatkozásra](https://software.intel.com/en-us/iot/hardware/edison/downloads) tartozó a `Installers` fejléc.</span><span class="sxs-lookup"><span data-stu-id="ab215-183">Get the latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under the `Installers` heading.</span></span> <span data-ttu-id="ab215-184">Az eszköz hajtható végre, és kövesse a képernyőn megjelenő utasításokat, amennyiben szükséges Tovább gombra kattint</span><span class="sxs-lookup"><span data-stu-id="ab215-184">Execute the tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="ab215-185">Belső vezérlőprogram Flash</span><span class="sxs-lookup"><span data-stu-id="ab215-185">Flash firmware</span></span>
1. <span data-ttu-id="ab215-186">Az a `Set up options` kattintson `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="ab215-186">On the `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="ab215-187">Válassza ki a lemezképet flash alakzatot a tábla a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="ab215-187">Select the image to flash onto your board by doing one of the following:</span></span>
   - <span data-ttu-id="ab215-188">Töltse le, és a tábla a legújabb belső vezérlőprogram lemezképpel érhetők el az Intel flash, jelölje be `Download the latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="ab215-188">To download and flash your board with the latest firmware image available from Intel, select `Download the latest image version xxxx`.</span></span>
   - <span data-ttu-id="ab215-189">A tábla már mentette a számítógépen lemezképhez flash, jelölje be `Select the local image`.</span><span class="sxs-lookup"><span data-stu-id="ab215-189">To flash your board with an image you already have saved on your computer, select `Select the local image`.</span></span> <span data-ttu-id="ab215-190">Keresse meg és jelölje ki a flash a kártyához kívánt lemezképet.</span><span class="sxs-lookup"><span data-stu-id="ab215-190">Browse to and select the image you want to flash to your board.</span></span>
3. <span data-ttu-id="ab215-191">A telepítő eszköz megkísérli a tábla flash.</span><span class="sxs-lookup"><span data-stu-id="ab215-191">The setup tool will attempt to flash your board.</span></span> <span data-ttu-id="ab215-192">A teljes villogó folyamat akár 10 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="ab215-192">The entire flashing process may take up to 10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="ab215-193">Jelszó beállítása</span><span class="sxs-lookup"><span data-stu-id="ab215-193">Set password</span></span>
1. <span data-ttu-id="ab215-194">Az a `Set up options` kattintson `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="ab215-194">On the `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="ab215-195">Az Intel® Edison board egyéni nevet állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="ab215-195">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="ab215-196">Ez nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="ab215-196">This is optional.</span></span>
3. <span data-ttu-id="ab215-197">Adja meg a tábla jelszavát, majd kattintson az `Set password`.</span><span class="sxs-lookup"><span data-stu-id="ab215-197">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="ab215-198">A jelszót, amely később üzemszünetének.</span><span class="sxs-lookup"><span data-stu-id="ab215-198">Mark down the password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="ab215-199">Wi-Fi csatlakozás</span><span class="sxs-lookup"><span data-stu-id="ab215-199">Connect Wi-Fi</span></span>
1. <span data-ttu-id="ab215-200">Az a `Set up options` kattintson `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="ab215-200">On the `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="ab215-201">Várjon, amíg legfeljebb egy percig a számítógép elérhető Wi-Fi hálózatok keres.</span><span class="sxs-lookup"><span data-stu-id="ab215-201">Wait up to one minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="ab215-202">Az a `Detected Networks` legördülő listára, válassza ki a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="ab215-202">From the `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="ab215-203">Az a `Security` legördülő listára, válassza ki a hálózati biztonság típusa.</span><span class="sxs-lookup"><span data-stu-id="ab215-203">From the `Security` drop-down list, select the network's security type.</span></span>
4. <span data-ttu-id="ab215-204">Adja meg a felhasználónevet és jelszót információkat, majd kattintson az `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="ab215-204">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="ab215-205">Az IP-cím, amely később üzemszünetének.</span><span class="sxs-lookup"><span data-stu-id="ab215-205">Mark down the IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="ab215-206">Győződjön meg arról, hogy Edison és a számítógép ugyanahhoz a hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="ab215-206">Make sure that Edison is connected to the same network as your computer.</span></span> <span data-ttu-id="ab215-207">A számítógép csatlakozik a Edison az IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="ab215-207">Your computer connects to your Edison by using the IP address.</span></span>

   ![Csatlakozás hőmérséklet-érzékelő](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

<span data-ttu-id="ab215-209">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="ab215-209">Congratulations!</span></span> <span data-ttu-id="ab215-210">Sikeresen konfigurálta az Edison.</span><span class="sxs-lookup"><span data-stu-id="ab215-210">You've successfully configured Edison.</span></span>

## <a name="run-a-sample-application-on-intel-edison"></a><span data-ttu-id="ab215-211">Futtassa a mintaalkalmazást az Intel Edison</span><span class="sxs-lookup"><span data-stu-id="ab215-211">Run a sample application on Intel Edison</span></span>

### <a name="prepare-the-azure-iot-device-sdk"></a><span data-ttu-id="ab215-212">Az Azure IoT eszköz SDK előkészítése</span><span class="sxs-lookup"><span data-stu-id="ab215-212">Prepare the Azure IoT Device SDK</span></span>

1. <span data-ttu-id="ab215-213">A számítógép a következő SSH-ügyfél használatával az Intel Edison csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="ab215-213">Use one of the following SSH clients from your host computer to connect to your Intel Edison.</span></span> <span data-ttu-id="ab215-214">Az IP-cím származik-e a konfigurációs eszközt, és a jelszót, az egy adott eszköz a beállított.</span><span class="sxs-lookup"><span data-stu-id="ab215-214">The IP address is from the configuration tool and the password is the one you've set in that tool.</span></span>
    - <span data-ttu-id="ab215-215">[A puTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="ab215-215">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="ab215-216">A beépített SSH-ügyfél Ubuntu vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="ab215-216">The built-in SSH client on Ubuntu or macOS.</span></span>

2. <span data-ttu-id="ab215-217">Klónozza a mintaalkalmazást ügyfél az eszközre.</span><span class="sxs-lookup"><span data-stu-id="ab215-217">Clone the sample client app to your device.</span></span> 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. <span data-ttu-id="ab215-218">Keresse meg a tárház mappa összes csomag telepítése a következő parancsot, közegészségre veszélyesnek percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="ab215-218">Then navigate to the repo folder to run the following command to install all packages, it may take serval minutes to complete.</span></span>
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-the-sample-application"></a><span data-ttu-id="ab215-219">Konfigurálja és futtassa a mintaalkalmazást</span><span class="sxs-lookup"><span data-stu-id="ab215-219">Configure and run the sample application</span></span>

1. <span data-ttu-id="ab215-220">Nyissa meg a konfigurációs fájlban a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ab215-220">Open the config file by running the following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![A konfigurációs fájl](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   <span data-ttu-id="ab215-222">Két makrók van ebben a fájlban configurate is.</span><span class="sxs-lookup"><span data-stu-id="ab215-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="ab215-223">Az első egy `INTERVAL`, amely megadja, hogy két felhőbe küldött üzeneteket közötti időközt.</span><span class="sxs-lookup"><span data-stu-id="ab215-223">The first one is `INTERVAL`, which defines the time interval between two messages that send to cloud.</span></span> <span data-ttu-id="ab215-224">A második érték `simulatedData`, vagyis az, hogy szimulált érzékelőadatait vagy nem logikai értéket.</span><span class="sxs-lookup"><span data-stu-id="ab215-224">The second one `simulatedData`,which is a Boolean value for whether to use simulated sensor data or not.</span></span>

   <span data-ttu-id="ab215-225">Ha Ön **nem rendelkezik az érzékelő**, beállíthatja a `simulatedData` egy érték `true` a minta kérelem létrehozása és használata a szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="ab215-225">If you **don't have the sensor**, set the `simulatedData` value to `true` to make the sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="ab215-226">Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="ab215-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>


1. <span data-ttu-id="ab215-227">Futtassa a mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ab215-227">Run the sample application by running the following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="ab215-228">Győződjön meg arról, hogy Ön-e beillesztési az eszköz kapcsolati karakterláncát azokat a szimpla idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="ab215-228">Make sure you copy-paste the device connection string into the single quotes.</span></span>

<span data-ttu-id="ab215-229">A következő kimeneti bemutatja az érzékelő adatokat és az IoT hub küldött üzenetek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="ab215-229">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

![Kimeneti - érzékelő adatokat küldött az Intel Edison az IoT hubhoz](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a><span data-ttu-id="ab215-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab215-231">Next steps</span></span>

<span data-ttu-id="ab215-232">Egy mintaalkalmazás érzékelő adatokat gyűjteni, és küldje el az IoT hub futtatását.</span><span class="sxs-lookup"><span data-stu-id="ab215-232">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
