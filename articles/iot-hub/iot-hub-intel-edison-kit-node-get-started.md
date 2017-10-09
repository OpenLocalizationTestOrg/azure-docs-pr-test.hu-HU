---
title: "aaaIntel Edison toocloud (Node.js) - csatlakozás Intel Edison tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban Intel Edison tooAzure IoT-központ Intel Edison toosend adatok toohello Azure cloud platform csatlakoznak."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot intel edison, intel edison iot-központot, intel edison küldése adatok toocloud, intel edison toocloud"
ms.assetid: a7c9cf2d-c102-41b0-aa45-41285c6877eb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/15/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfc3387efc532b4b83f0626a9cf61d12c2952af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-intel-edison-tooazure-iot-hub-nodejs"></a><span data-ttu-id="6033d-104">Csatlakozás Intel Edison tooAzure IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="6033d-104">Connect Intel Edison tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="6033d-105">Ebben az oktatóanyagban akkor először hello használatának alapjait Intel Edison tanulási.</span><span class="sxs-lookup"><span data-stu-id="6033d-105">In this tutorial, you begin by learning hello basics of working with Intel Edison.</span></span> <span data-ttu-id="6033d-106">Majd megismerheti, hogyan tooseamlessly összekapcsolni használatával eszközök toohello felhőalapú [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="6033d-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

<span data-ttu-id="6033d-107">Még nem rendelkezik egy csomagot?</span><span class="sxs-lookup"><span data-stu-id="6033d-107">Don't have a kit yet?</span></span> <span data-ttu-id="6033d-108">Start [Itt](https://azure.microsoft.com/develop/iot/starter-kits)</span><span class="sxs-lookup"><span data-stu-id="6033d-108">Start [here](https://azure.microsoft.com/develop/iot/starter-kits)</span></span>

## <a name="what-you-do"></a><span data-ttu-id="6033d-109">Mit</span><span class="sxs-lookup"><span data-stu-id="6033d-109">What you do</span></span>

* <span data-ttu-id="6033d-110">A telepítő Intel Edison és és Groove-modulok.</span><span class="sxs-lookup"><span data-stu-id="6033d-110">Setup Intel Edison and and Grove modules.</span></span>
* <span data-ttu-id="6033d-111">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6033d-111">Create an IoT hub.</span></span>
* <span data-ttu-id="6033d-112">Eszköz regisztrálása az Edison az IoT hub a.</span><span class="sxs-lookup"><span data-stu-id="6033d-112">Register a device for Edison in your IoT hub.</span></span>
* <span data-ttu-id="6033d-113">Futtassa a mintaalkalmazást Edison toosend érzékelő adatokat tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="6033d-113">Run a sample application on Edison toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="6033d-114">Csatlakoztassa az Intel Edison tooan IoT-központ az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="6033d-114">Connect Intel Edison tooan IoT hub that you create.</span></span> <span data-ttu-id="6033d-115">Majd, futtassa a mintaalkalmazást Edison toocollect hőmérséklet és a páratartalom adatok Groove hőmérséklet-érzékelő.</span><span class="sxs-lookup"><span data-stu-id="6033d-115">Then you run a sample application on Edison toocollect temperature and humidity data from a Grove temperature sensor.</span></span> <span data-ttu-id="6033d-116">Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6033d-116">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="6033d-117">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="6033d-117">What you learn</span></span>

* <span data-ttu-id="6033d-118">Hogyan toocreate az Azure IoT-központ és az új eszköz kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="6033d-118">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="6033d-119">Hogyan tooconnect Edison rendelkező Groove hőmérséklet-érzékelő.</span><span class="sxs-lookup"><span data-stu-id="6033d-119">How tooconnect Edison with a Grove temperature sensor.</span></span>
* <span data-ttu-id="6033d-120">Hogyan toocollect érzékelőadatait Edison mintaalkalmazás futtatásával.</span><span class="sxs-lookup"><span data-stu-id="6033d-120">How toocollect sensor data by running a sample application on Edison.</span></span>
* <span data-ttu-id="6033d-121">Hogyan toosend érzékelő adatokat tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6033d-121">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6033d-122">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="6033d-122">What you need</span></span>

![Mi szükséges](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* <span data-ttu-id="6033d-124">hello Intel Edison board</span><span class="sxs-lookup"><span data-stu-id="6033d-124">hello Intel Edison board</span></span>
* <span data-ttu-id="6033d-125">Arduino bővítése tábla</span><span class="sxs-lookup"><span data-stu-id="6033d-125">Arduino expansion board</span></span>
* <span data-ttu-id="6033d-126">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="6033d-126">An active Azure subscription.</span></span> <span data-ttu-id="6033d-127">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="6033d-127">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="6033d-128">A Mac vagy a Windows vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="6033d-128">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="6033d-129">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="6033d-129">An Internet connection.</span></span>
* <span data-ttu-id="6033d-130">Egy Micro B tooType egy USB-kábellel</span><span class="sxs-lookup"><span data-stu-id="6033d-130">A Micro B tooType A USB cable</span></span>
* <span data-ttu-id="6033d-131">A közvetlen aktuális (DC) tápegység.</span><span class="sxs-lookup"><span data-stu-id="6033d-131">A direct current (DC) power supply.</span></span> <span data-ttu-id="6033d-132">A tápegység kell tekinthető meg az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6033d-132">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="6033d-133">7-15V TARTOMÁNYVEZÉRLŐ</span><span class="sxs-lookup"><span data-stu-id="6033d-133">7-15V DC</span></span>
  - <span data-ttu-id="6033d-134">Legalább 1500mA</span><span class="sxs-lookup"><span data-stu-id="6033d-134">At least 1500mA</span></span>
  - <span data-ttu-id="6033d-135">hello center/belső PIN-kód hello pozitív sarkpontot hello energiaellátás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6033d-135">hello center/inner pin should be hello positive pole of hello power supply</span></span>

<span data-ttu-id="6033d-136">a következő elemek hello nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="6033d-136">hello following items are optional:</span></span>

* <span data-ttu-id="6033d-137">Groove alap pajzs V2</span><span class="sxs-lookup"><span data-stu-id="6033d-137">Grove Base Shield V2</span></span>
* <span data-ttu-id="6033d-138">Groove - hőmérséklet-érzékelő</span><span class="sxs-lookup"><span data-stu-id="6033d-138">Grove - Temperature Sensor</span></span>
* <span data-ttu-id="6033d-139">Groove kábel</span><span class="sxs-lookup"><span data-stu-id="6033d-139">Grove Cable</span></span>
* <span data-ttu-id="6033d-140">Bármely Oszlopelválasztó sávok vagy csavart hello csomagolása, beleértve a két csavart toofasten hello modul toohello bővítése board és négy csavart és műanyag térköztartók szerepel.</span><span class="sxs-lookup"><span data-stu-id="6033d-140">Any spacer bars or screws included in hello packaging, including two screws toofasten hello module toohello expansion board and four sets of screws and plastic spacers.</span></span>

> [!NOTE] 
<span data-ttu-id="6033d-141">Ezek az elemek nem kötelező, mert hello kód a minta támogatási szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="6033d-141">These items are optional because hello code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a><span data-ttu-id="6033d-142">A telepítő Intel Edison</span><span class="sxs-lookup"><span data-stu-id="6033d-142">Setup Intel Edison</span></span>

### <a name="assemble-your-board"></a><span data-ttu-id="6033d-143">A tábla összeállítása</span><span class="sxs-lookup"><span data-stu-id="6033d-143">Assemble your board</span></span>

<span data-ttu-id="6033d-144">Jelen szakaszban található lépéseket tooattach az Intel® Edison modul tooyour bővítése tábla.</span><span class="sxs-lookup"><span data-stu-id="6033d-144">This section contains steps tooattach your Intel® Edison module tooyour expansion board.</span></span>

1. <span data-ttu-id="6033d-145">A bővítés üzenőfalon, hello lyuk hello modul hello csavart hello bővítése táblán a sorba állítása hello Intel® Edison modul belül fehér hello vázlat helyezze el.</span><span class="sxs-lookup"><span data-stu-id="6033d-145">Place hello Intel® Edison module within hello white outline on your expansion board, lining up hello holes on hello module with hello screws on hello expansion board.</span></span>

2. <span data-ttu-id="6033d-146">Nyomja le a hello modul hello szavak alatt `What will you make?` csak úgy érzi, hogy a beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="6033d-146">Press down on hello module just below hello words `What will you make?` until you feel a snap.</span></span>

   ![indítópanel 2 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. <span data-ttu-id="6033d-148">A két hello (hello csomagban található) hexadecimális nuts toosecure hello modul toohello bővítése tábla használatával.</span><span class="sxs-lookup"><span data-stu-id="6033d-148">Use hello two hex nuts (included in hello package) toosecure hello module toohello expansion board.</span></span>

   ![indítópanel 3 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. <span data-ttu-id="6033d-150">Helyezze be a elment hello négy sarok lyukat hello bővítése táblán egyikében.</span><span class="sxs-lookup"><span data-stu-id="6033d-150">Insert a screw in one of hello four corner holes on hello expansion board.</span></span> <span data-ttu-id="6033d-151">Csavarás és szigoríthatja fehér hello műanyag térköztartók alakzatot hello csavart egyikét.</span><span class="sxs-lookup"><span data-stu-id="6033d-151">Twist and tighten one of hello white plastic spacers onto hello screw.</span></span>

   ![indítópanel 4 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. <span data-ttu-id="6033d-153">Ismételje meg a másik három sarok térköztartók hello.</span><span class="sxs-lookup"><span data-stu-id="6033d-153">Repeat for hello other three corner spacers.</span></span>

   ![indítópanel 5 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

<span data-ttu-id="6033d-155">Most már a üzenőfalon kész.</span><span class="sxs-lookup"><span data-stu-id="6033d-155">Now your board is assembled.</span></span>

   ![üzenőfalon kész](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-hello-grove-base-shield-and-hello-temperature-sensor"></a><span data-ttu-id="6033d-157">Hello Groove talál pajzs és hello hőmérséklet-érzékelő</span><span class="sxs-lookup"><span data-stu-id="6033d-157">Connect hello Grove Base Shield and hello temperature sensor</span></span>

1. <span data-ttu-id="6033d-158">Hely hello Groove talál pajzs tooyour táblán.</span><span class="sxs-lookup"><span data-stu-id="6033d-158">Place hello Grove Base Shield on tooyour board.</span></span> <span data-ttu-id="6033d-159">Győződjön meg arról, hogy a tábla szorosan csatlakoztatott összes PIN-kód.</span><span class="sxs-lookup"><span data-stu-id="6033d-159">Make sure all pins are tightly plugged into your board.</span></span>
   
   ![Groove alap pajzs ikon](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. <span data-ttu-id="6033d-161">Használjon Groove kábel tooconnect Groove hőmérséklet-érzékelő alakzatot hello Groove talál pajzs **A0** port.</span><span class="sxs-lookup"><span data-stu-id="6033d-161">Use Grove Cable tooconnect Grove temperature sensor onto hello Grove Base Shield **A0** port.</span></span>

   ![Csatlakozás tootemperature érzékelő](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Edison és érzékelő kapcsolat](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

<span data-ttu-id="6033d-164">Most már készen áll az érzékelő.</span><span class="sxs-lookup"><span data-stu-id="6033d-164">Now your sensor is ready.</span></span>

### <a name="power-up-edison"></a><span data-ttu-id="6033d-165">Energiagazdálkodási Edison mentése</span><span class="sxs-lookup"><span data-stu-id="6033d-165">Power up Edison</span></span>

1. <span data-ttu-id="6033d-166">Beépülő modul hello tápegység.</span><span class="sxs-lookup"><span data-stu-id="6033d-166">Plug in hello power supply.</span></span>

   ![Beépülő modul tápegység](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. <span data-ttu-id="6033d-168">A zöld LED-jét (DS1 feliratú a hello Arduino * bővítése board) kell bonyolít le, és megvilágítottnak maradnak.</span><span class="sxs-lookup"><span data-stu-id="6033d-168">A green LED(labeled DS1 on hello Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="6033d-169">Várjon egy percet a hello board toofinish másolatából végrehajtott indítása.</span><span class="sxs-lookup"><span data-stu-id="6033d-169">Wait one minute for hello board toofinish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6033d-170">Ha egy tartományvezérlő tápegység nem rendelkezik, akkor is power hello board USB-porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="6033d-170">If you do not have a DC power supply, you can still power hello board through a USB port.</span></span> <span data-ttu-id="6033d-171">Lásd: `Connect Edison tooyour computer` című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="6033d-171">See `Connect Edison tooyour computer` section for details.</span></span> <span data-ttu-id="6033d-172">A tábla, ilyen módon működtetéséhez azt eredményezheti, hogy a tábláról előre nem látható viselkedéshez különösen akkor, ha a Wi-Fi használatával, vagy befolyásoló tényezők motorok.</span><span class="sxs-lookup"><span data-stu-id="6033d-172">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

### <a name="connect-edison-tooyour-computer"></a><span data-ttu-id="6033d-173">Edison tooyour számítógép</span><span class="sxs-lookup"><span data-stu-id="6033d-173">Connect Edison tooyour computer</span></span>

1. <span data-ttu-id="6033d-174">Váltás le hello mikrokapcsoló felé hello két micro USB-porttal, így Edison eszköz módban van.</span><span class="sxs-lookup"><span data-stu-id="6033d-174">Toggle down hello microswitch towards hello two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="6033d-175">Az eszköz és a gazdagép üzemmód közötti különbségeket, tekintse át [Itt](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="6033d-175">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Váltás a hello mikrokapcsoló](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. <span data-ttu-id="6033d-177">Hello micro USB-kábellel csatlakoztassa hello felső micro USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="6033d-177">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Felső micro USB-port](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. <span data-ttu-id="6033d-179">Plug hello USB-kábel másik végén a számítógépbe.</span><span class="sxs-lookup"><span data-stu-id="6033d-179">Plug hello other end of USB cable into your computer.</span></span>

   ![Számítógép USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. <span data-ttu-id="6033d-181">Tudni fogják, hogy a tábla teljes inicializálását, amikor a számítógép csatlakoztat egy új meghajtót (hasonlóan egy SD-kártya beszúrása a számítógép).</span><span class="sxs-lookup"><span data-stu-id="6033d-181">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-hello-configuration-tool"></a><span data-ttu-id="6033d-182">Hello konfigurációs eszköz letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="6033d-182">Download and run hello configuration tool</span></span>
<span data-ttu-id="6033d-183">Hello legújabb konfiguráló eszköz az beszerzése [Ez a hivatkozás](https://software.intel.com/en-us/iot/hardware/edison/downloads) alatt hello feltüntetve `Installers` fejléc.</span><span class="sxs-lookup"><span data-stu-id="6033d-183">Get hello latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under hello `Installers` heading.</span></span> <span data-ttu-id="6033d-184">Hello eszköz hajtható végre, és kövesse a képernyőn megjelenő utasításokat, amennyiben szükséges Tovább gombra kattint</span><span class="sxs-lookup"><span data-stu-id="6033d-184">Execute hello tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="6033d-185">Belső vezérlőprogram Flash</span><span class="sxs-lookup"><span data-stu-id="6033d-185">Flash firmware</span></span>
1. <span data-ttu-id="6033d-186">A hello `Set up options` kattintson `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="6033d-186">On hello `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="6033d-187">Válassza ki a hello kép tooflash alakzatot a tábla hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="6033d-187">Select hello image tooflash onto your board by doing one of hello following:</span></span>
   - <span data-ttu-id="6033d-188">toodownload és a flash hello legújabb belső vezérlőprogram lemezképpel érhetők el az Intel, a tábla válasszon `Download hello latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="6033d-188">toodownload and flash your board with hello latest firmware image available from Intel, select `Download hello latest image version xxxx`.</span></span>
   - <span data-ttu-id="6033d-189">tooflash a tábla olyan képpel már mentette a számítógépen, válassza ki `Select hello local image`.</span><span class="sxs-lookup"><span data-stu-id="6033d-189">tooflash your board with an image you already have saved on your computer, select `Select hello local image`.</span></span> <span data-ttu-id="6033d-190">Keresse meg a tooand válassza hello kép tooflash tooyour board szeretné.</span><span class="sxs-lookup"><span data-stu-id="6033d-190">Browse tooand select hello image you want tooflash tooyour board.</span></span>
3. <span data-ttu-id="6033d-191">hello telepítő eszköz tooflash kísérli meg a tábla.</span><span class="sxs-lookup"><span data-stu-id="6033d-191">hello setup tool will attempt tooflash your board.</span></span> <span data-ttu-id="6033d-192">hello teljes villogó folyamat too10 percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="6033d-192">hello entire flashing process may take up too10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="6033d-193">Jelszó beállítása</span><span class="sxs-lookup"><span data-stu-id="6033d-193">Set password</span></span>
1. <span data-ttu-id="6033d-194">A hello `Set up options` kattintson `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="6033d-194">On hello `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="6033d-195">Az Intel® Edison board egyéni nevet állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="6033d-195">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="6033d-196">Ez nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="6033d-196">This is optional.</span></span>
3. <span data-ttu-id="6033d-197">Adja meg a tábla jelszavát, majd kattintson az `Set password`.</span><span class="sxs-lookup"><span data-stu-id="6033d-197">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="6033d-198">Üzemszünetének hello jelszót, amelyre később szolgál.</span><span class="sxs-lookup"><span data-stu-id="6033d-198">Mark down hello password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="6033d-199">Wi-Fi csatlakozás</span><span class="sxs-lookup"><span data-stu-id="6033d-199">Connect Wi-Fi</span></span>
1. <span data-ttu-id="6033d-200">A hello `Set up options` kattintson `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="6033d-200">On hello `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="6033d-201">Várjon, amíg fel tooone perc, a számítógép vizsgálatok elérhető Wi-Fi-hálózatok.</span><span class="sxs-lookup"><span data-stu-id="6033d-201">Wait up tooone minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="6033d-202">A hello `Detected Networks` legördülő listára, válassza ki a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="6033d-202">From hello `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="6033d-203">A hello `Security` legördülő listában, jelölje be hello hálózati biztonság típusa.</span><span class="sxs-lookup"><span data-stu-id="6033d-203">From hello `Security` drop-down list, select hello network's security type.</span></span>
4. <span data-ttu-id="6033d-204">Adja meg a felhasználónevet és jelszót információkat, majd kattintson az `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="6033d-204">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="6033d-205">Üzemszünetének hello IP-címet, amely később szolgál.</span><span class="sxs-lookup"><span data-stu-id="6033d-205">Mark down hello IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="6033d-206">Győződjön meg arról, hogy Edison ugyanaz, mint a számítógép hálózati csatlakoztatott toohello.</span><span class="sxs-lookup"><span data-stu-id="6033d-206">Make sure that Edison is connected toohello same network as your computer.</span></span> <span data-ttu-id="6033d-207">A számítógép csatlakozik a tooyour Edison hello IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="6033d-207">Your computer connects tooyour Edison by using hello IP address.</span></span>

   ![Csatlakozás tootemperature érzékelő](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

<span data-ttu-id="6033d-209">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="6033d-209">Congratulations!</span></span> <span data-ttu-id="6033d-210">Sikeresen konfigurálta az Edison.</span><span class="sxs-lookup"><span data-stu-id="6033d-210">You've successfully configured Edison.</span></span>

## <a name="run-a-sample-application-on-intel-edison"></a><span data-ttu-id="6033d-211">Futtassa a mintaalkalmazást az Intel Edison</span><span class="sxs-lookup"><span data-stu-id="6033d-211">Run a sample application on Intel Edison</span></span>

### <a name="prepare-hello-azure-iot-device-sdk"></a><span data-ttu-id="6033d-212">Hello Azure IoT eszköz SDK előkészítése</span><span class="sxs-lookup"><span data-stu-id="6033d-212">Prepare hello Azure IoT Device SDK</span></span>

1. <span data-ttu-id="6033d-213">A fogadó számítógép tooconnect tooyour Intel Edison az SSH-ügyfél következő hello egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="6033d-213">Use one of hello following SSH clients from your host computer tooconnect tooyour Intel Edison.</span></span> <span data-ttu-id="6033d-214">hello IP-cím hello konfigurációs eszköze, hello jelszó pedig egy adott eszköz a beállított hello.</span><span class="sxs-lookup"><span data-stu-id="6033d-214">hello IP address is from hello configuration tool and hello password is hello one you've set in that tool.</span></span>
    - <span data-ttu-id="6033d-215">[A puTTY](http://www.putty.org/) Windows.</span><span class="sxs-lookup"><span data-stu-id="6033d-215">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="6033d-216">hello beépített SSH-ügyfél Ubuntu vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="6033d-216">hello built-in SSH client on Ubuntu or macOS.</span></span>

2. <span data-ttu-id="6033d-217">Klónozás hello sample app tooyour ügyféleszközön.</span><span class="sxs-lookup"><span data-stu-id="6033d-217">Clone hello sample client app tooyour device.</span></span> 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. <span data-ttu-id="6033d-218">Az összes csomag toohello tárház mappa toorun hello a következő parancs tooinstall navigáljon, közegészségre veszélyesnek perc toocomplete is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="6033d-218">Then navigate toohello repo folder toorun hello following command tooinstall all packages, it may take serval minutes toocomplete.</span></span>
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-hello-sample-application"></a><span data-ttu-id="6033d-219">Konfigurálja és hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="6033d-219">Configure and run hello sample application</span></span>

1. <span data-ttu-id="6033d-220">Nyissa meg a konfigurációs fájl hello hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6033d-220">Open hello config file by running hello following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![A konfigurációs fájl](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   <span data-ttu-id="6033d-222">Két makrók van ebben a fájlban configurate is.</span><span class="sxs-lookup"><span data-stu-id="6033d-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="6033d-223">hello először egy van `INTERVAL`, amely megadja, hogy két toocloud küldött üzenetek hello időközét.</span><span class="sxs-lookup"><span data-stu-id="6033d-223">hello first one is `INTERVAL`, which defines hello time interval between two messages that send toocloud.</span></span> <span data-ttu-id="6033d-224">második hello `simulatedData`, vagyis az, hogy toouse szimulált-e érzékelőadatait vagy nem logikai értéket.</span><span class="sxs-lookup"><span data-stu-id="6033d-224">hello second one `simulatedData`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="6033d-225">Ha Ön **nincs hello érzékelő**, beállíthatja hello `simulatedData` érték túl`true` toomake hello mintaalkalmazás létrehozása, és használjon szimulált érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="6033d-225">If you **don't have hello sensor**, set hello `simulatedData` value too`true` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="6033d-226">Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="6033d-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>


1. <span data-ttu-id="6033d-227">Futtassa a mintaalkalmazást hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6033d-227">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="6033d-228">Győződjön meg arról, hogy Ön-e beillesztési hello eszköz kapcsolati karakterláncot a hello szimpla idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="6033d-228">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>

<span data-ttu-id="6033d-229">Meg kell jelennie a hello parancskimenet, hogy látható hello érzékelő adatokat és hello küldött üzenetek tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6033d-229">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Kimeneti - érzékelő adatokat küldött az Intel Edison tooyour IoT-központ](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a><span data-ttu-id="6033d-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6033d-231">Next steps</span></span>

<span data-ttu-id="6033d-232">Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="6033d-232">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
