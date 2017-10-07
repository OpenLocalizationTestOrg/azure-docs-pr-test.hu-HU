---
title: "Intel Edison (csomópont) tooAzure IoT - lecke 1 Kapcsolódás: eszközök konfigurálása |} Microsoft Docs"
description: "Intel Edison konfigurálása az első használatnál."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "beállítása, arduino csatlakozás arduino toopc, a telepítő arduino, arduino board"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 372c9b6d-e701-4ff6-8151-d262aa76aa55
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c0609542a49924c979f71297d808c09acefca0bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-intel-edison"></a><span data-ttu-id="86f2b-104">Az Intel Edison konfigurálása</span><span class="sxs-lookup"><span data-stu-id="86f2b-104">Configure your Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="86f2b-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="86f2b-105">What you will do</span></span>
<span data-ttu-id="86f2b-106">Első alkalommal hello üzenőfalon, működtetéséhez azt fel összegyűjtésével Intel Edison konfigurálása és telepítése konfigurációs eszköz tooyour asztali operációs rendszer tooflash Edison a belső vezérlőprogram, állítsa be a jelszavát, és csatlakoztassa tooWi-Fi.</span><span class="sxs-lookup"><span data-stu-id="86f2b-106">Configure Intel Edison for first-time use by assembling hello board, powering it up and installing configuration tool tooyour desktop OS tooflash Edison's firmware, set its password and connect it tooWi-Fi.</span></span> <span data-ttu-id="86f2b-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="86f2b-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="86f2b-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="86f2b-108">What you will learn</span></span>
<span data-ttu-id="86f2b-109">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="86f2b-109">In this article, you will learn:</span></span>

* <span data-ttu-id="86f2b-110">Hogyan tooassemble Edison board, és kapcsolja be a mentést.</span><span class="sxs-lookup"><span data-stu-id="86f2b-110">How tooassemble Edison board and power it up.</span></span>
* <span data-ttu-id="86f2b-111">Hogyan tooflash Edison belső vezérlőprogram, állítsa be a jelszót, és csatlakozzon a Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="86f2b-111">How tooflash Edison's firmware, set password and connect Wi-Fi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="86f2b-112">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="86f2b-112">What you need</span></span>
<span data-ttu-id="86f2b-113">toocomplete Ez a művelet következő részek a Intel Edison Starter Kit hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="86f2b-113">toocomplete this operation, you need hello following parts from your Intel Edison Starter Kit:</span></span>

* <span data-ttu-id="86f2b-114">Intel® Edison modul</span><span class="sxs-lookup"><span data-stu-id="86f2b-114">Intel® Edison module</span></span>
* <span data-ttu-id="86f2b-115">Arduino bővítése tábla</span><span class="sxs-lookup"><span data-stu-id="86f2b-115">Arduino expansion board</span></span>
* <span data-ttu-id="86f2b-116">Bármely Oszlopelválasztó sávok vagy csavart hello csomagolása, beleértve a két csavart toofasten hello modul toohello bővítése board és négy csavart és műanyag térköztartók szerepel.</span><span class="sxs-lookup"><span data-stu-id="86f2b-116">Any spacer bars or screws included in hello packaging, including two screws toofasten hello module toohello expansion board and four sets of screws and plastic spacers.</span></span>
* <span data-ttu-id="86f2b-117">Egy Micro B tooType egy USB-kábellel</span><span class="sxs-lookup"><span data-stu-id="86f2b-117">A Micro B tooType A USB cable</span></span>
* <span data-ttu-id="86f2b-118">A közvetlen aktuális (DC) tápegység.</span><span class="sxs-lookup"><span data-stu-id="86f2b-118">A direct current (DC) power supply.</span></span> <span data-ttu-id="86f2b-119">A tápegység kell tekinthető meg az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="86f2b-119">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="86f2b-120">7-15V TARTOMÁNYVEZÉRLŐ</span><span class="sxs-lookup"><span data-stu-id="86f2b-120">7-15V DC</span></span>
  - <span data-ttu-id="86f2b-121">Legalább 1500mA</span><span class="sxs-lookup"><span data-stu-id="86f2b-121">At least 1500mA</span></span>
  - <span data-ttu-id="86f2b-122">hello center/belső PIN-kód hello pozitív sarkpontot hello energiaellátás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="86f2b-122">hello center/inner pin should be hello positive pole of hello power supply</span></span>

  ![Az Starter Kit dolgot](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

<span data-ttu-id="86f2b-124">Emellett a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="86f2b-124">You also need:</span></span>

* <span data-ttu-id="86f2b-125">Windows, Mac vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="86f2b-125">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="86f2b-126">Egy Edison tooconnect a vezeték nélküli kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="86f2b-126">A wireless connection for Edison tooconnect to.</span></span>
* <span data-ttu-id="86f2b-127">Az internetes kapcsolat toodownload konfigurációs eszközt.</span><span class="sxs-lookup"><span data-stu-id="86f2b-127">An Internet connection toodownload configuration tool.</span></span>

## <a name="assemble-your-board"></a><span data-ttu-id="86f2b-128">A tábla összeállítása</span><span class="sxs-lookup"><span data-stu-id="86f2b-128">Assemble your board</span></span>

<span data-ttu-id="86f2b-129">Jelen szakaszban található lépéseket tooattach az Intel® Edison modul tooyour bővítése tábla.</span><span class="sxs-lookup"><span data-stu-id="86f2b-129">This section contains steps tooattach your Intel® Edison module tooyour expansion board.</span></span>

1. <span data-ttu-id="86f2b-130">A bővítés üzenőfalon, hello lyuk hello modul hello csavart hello bővítése táblán a sorba állítása hello Intel® Edison modul belül fehér hello vázlat helyezze el.</span><span class="sxs-lookup"><span data-stu-id="86f2b-130">Place hello Intel® Edison module within hello white outline on your expansion board, lining up hello holes on hello module with hello screws on hello expansion board.</span></span>

2. <span data-ttu-id="86f2b-131">Nyomja le a hello modul hello szavak alatt `What will you make?` csak úgy érzi, hogy a beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="86f2b-131">Press down on hello module just below hello words `What will you make?` until you feel a snap.</span></span>

   ![indítópanel 2 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. <span data-ttu-id="86f2b-133">A két hello (hello csomagban található) hexadecimális nuts toosecure hello modul toohello bővítése tábla használatával.</span><span class="sxs-lookup"><span data-stu-id="86f2b-133">Use hello two hex nuts (included in hello package) toosecure hello module toohello expansion board.</span></span>

   ![indítópanel 3 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. <span data-ttu-id="86f2b-135">Helyezze be a elment hello négy sarok lyukat hello bővítése táblán egyikében.</span><span class="sxs-lookup"><span data-stu-id="86f2b-135">Insert a screw in one of hello four corner holes on hello expansion board.</span></span> <span data-ttu-id="86f2b-136">Csavarás és szigoríthatja fehér hello műanyag térköztartók alakzatot hello csavart egyikét.</span><span class="sxs-lookup"><span data-stu-id="86f2b-136">Twist and tighten one of hello white plastic spacers onto hello screw.</span></span>

   ![indítópanel 4 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. <span data-ttu-id="86f2b-138">Ismételje meg a másik három sarok térköztartók hello.</span><span class="sxs-lookup"><span data-stu-id="86f2b-138">Repeat for hello other three corner spacers.</span></span>

   ![indítópanel 5 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

<span data-ttu-id="86f2b-140">Most már a üzenőfalon kész.</span><span class="sxs-lookup"><span data-stu-id="86f2b-140">Now your board is assembled.</span></span>

   ![üzenőfalon kész](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a><span data-ttu-id="86f2b-142">Energiagazdálkodási Edison mentése</span><span class="sxs-lookup"><span data-stu-id="86f2b-142">Power up Edison</span></span>

1. <span data-ttu-id="86f2b-143">Beépülő modul hello tápegység.</span><span class="sxs-lookup"><span data-stu-id="86f2b-143">Plug in hello power supply.</span></span>

   ![Beépülő modul tápegység](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. <span data-ttu-id="86f2b-145">A zöld LED-jét (DS1 feliratú a hello Arduino * bővítése board) kell bonyolít le, és megvilágítottnak maradnak.</span><span class="sxs-lookup"><span data-stu-id="86f2b-145">A green LED(labeled DS1 on hello Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="86f2b-146">Várjon egy percet a hello board toofinish másolatából végrehajtott indítása.</span><span class="sxs-lookup"><span data-stu-id="86f2b-146">Wait one minute for hello board toofinish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="86f2b-147">Ha egy tartományvezérlő tápegység nem rendelkezik, akkor is power hello board USB-porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="86f2b-147">If you do not have a DC power supply, you can still power hello board through a USB port.</span></span> <span data-ttu-id="86f2b-148">Lásd: `Connect Edison tooyour computer` című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="86f2b-148">See `Connect Edison tooyour computer` section for details.</span></span> <span data-ttu-id="86f2b-149">A tábla, ilyen módon működtetéséhez azt eredményezheti, hogy a tábláról előre nem látható viselkedéshez különösen akkor, ha a Wi-Fi használatával, vagy befolyásoló tényezők motorok.</span><span class="sxs-lookup"><span data-stu-id="86f2b-149">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

## <a name="connect-edison-tooyour-computer"></a><span data-ttu-id="86f2b-150">Edison tooyour számítógép</span><span class="sxs-lookup"><span data-stu-id="86f2b-150">Connect Edison tooyour computer</span></span>

1. <span data-ttu-id="86f2b-151">Váltás le hello mikrokapcsoló felé hello két micro USB-porttal, így Edison eszköz módban van.</span><span class="sxs-lookup"><span data-stu-id="86f2b-151">Toggle down hello microswitch towards hello two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="86f2b-152">Az eszköz és a gazdagép üzemmód közötti különbségeket, tekintse át [Itt](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="86f2b-152">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Váltás a hello mikrokapcsoló](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. <span data-ttu-id="86f2b-154">Hello micro USB-kábellel csatlakoztassa hello felső micro USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="86f2b-154">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Felső micro USB-port](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. <span data-ttu-id="86f2b-156">Plug hello USB-kábel másik végén a számítógépbe.</span><span class="sxs-lookup"><span data-stu-id="86f2b-156">Plug hello other end of USB cable into your computer.</span></span>

   ![Számítógép USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. <span data-ttu-id="86f2b-158">Tudni fogják, hogy a tábla teljes inicializálását, amikor a számítógép csatlakoztat egy új meghajtót (hasonlóan egy SD-kártya beszúrása a számítógép).</span><span class="sxs-lookup"><span data-stu-id="86f2b-158">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-hello-configuration-tool"></a><span data-ttu-id="86f2b-159">Hello konfigurációs eszköz letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="86f2b-159">Download and run hello configuration tool</span></span>
<span data-ttu-id="86f2b-160">Hello legújabb konfiguráló eszköz az beszerzése [Ez a hivatkozás](https://software.intel.com/en-us/iot/hardware/edison/downloads) alatt hello feltüntetve `Installers` fejléc.</span><span class="sxs-lookup"><span data-stu-id="86f2b-160">Get hello latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under hello `Installers` heading.</span></span> <span data-ttu-id="86f2b-161">Hello eszköz hajtható végre, és kövesse a képernyőn megjelenő utasításokat, amennyiben szükséges Tovább gombra kattint</span><span class="sxs-lookup"><span data-stu-id="86f2b-161">Execute hello tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="86f2b-162">Belső vezérlőprogram Flash</span><span class="sxs-lookup"><span data-stu-id="86f2b-162">Flash firmware</span></span>
1. <span data-ttu-id="86f2b-163">A hello `Set up options` kattintson `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="86f2b-163">On hello `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="86f2b-164">Válassza ki a hello kép tooflash alakzatot a tábla hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="86f2b-164">Select hello image tooflash onto your board by doing one of hello following:</span></span>
   - <span data-ttu-id="86f2b-165">toodownload és a flash hello legújabb belső vezérlőprogram lemezképpel érhetők el az Intel, a tábla válasszon `Download hello latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="86f2b-165">toodownload and flash your board with hello latest firmware image available from Intel, select `Download hello latest image version xxxx`.</span></span>
   - <span data-ttu-id="86f2b-166">tooflash a tábla olyan képpel már mentette a számítógépen, válassza ki `Select hello local image`.</span><span class="sxs-lookup"><span data-stu-id="86f2b-166">tooflash your board with an image you already have saved on your computer, select `Select hello local image`.</span></span> <span data-ttu-id="86f2b-167">Keresse meg a tooand válassza hello kép tooflash tooyour board szeretné.</span><span class="sxs-lookup"><span data-stu-id="86f2b-167">Browse tooand select hello image you want tooflash tooyour board.</span></span>
3. <span data-ttu-id="86f2b-168">hello telepítő eszköz tooflash kísérli meg a tábla.</span><span class="sxs-lookup"><span data-stu-id="86f2b-168">hello setup tool will attempt tooflash your board.</span></span> <span data-ttu-id="86f2b-169">hello teljes villogó folyamat too10 percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="86f2b-169">hello entire flashing process may take up too10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="86f2b-170">Jelszó beállítása</span><span class="sxs-lookup"><span data-stu-id="86f2b-170">Set password</span></span>
1. <span data-ttu-id="86f2b-171">A hello `Set up options` kattintson `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="86f2b-171">On hello `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="86f2b-172">Az Intel® Edison board egyéni nevet állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="86f2b-172">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="86f2b-173">Ez nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="86f2b-173">This is optional.</span></span>
3. <span data-ttu-id="86f2b-174">Adja meg a tábla jelszavát, majd kattintson az `Set password`.</span><span class="sxs-lookup"><span data-stu-id="86f2b-174">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="86f2b-175">Üzemszünetének hello jelszót, amelyre később szolgál.</span><span class="sxs-lookup"><span data-stu-id="86f2b-175">Mark down hello password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="86f2b-176">Wi-Fi csatlakozás</span><span class="sxs-lookup"><span data-stu-id="86f2b-176">Connect Wi-Fi</span></span>
1. <span data-ttu-id="86f2b-177">A hello `Set up options` kattintson `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="86f2b-177">On hello `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="86f2b-178">Várjon, amíg fel tooone perc, a számítógép vizsgálatok elérhető Wi-Fi-hálózatok.</span><span class="sxs-lookup"><span data-stu-id="86f2b-178">Wait up tooone minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="86f2b-179">A hello `Detected Networks` legördülő listára, válassza ki a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="86f2b-179">From hello `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="86f2b-180">A hello `Security` legördülő listában, jelölje be hello hálózati biztonság típusa.</span><span class="sxs-lookup"><span data-stu-id="86f2b-180">From hello `Security` drop-down list, select hello network's security type.</span></span>
4. <span data-ttu-id="86f2b-181">Adja meg a felhasználónevet és jelszót információkat, majd kattintson az `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="86f2b-181">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="86f2b-182">Üzemszünetének hello IP-címet, amely később szolgál.</span><span class="sxs-lookup"><span data-stu-id="86f2b-182">Mark down hello IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="86f2b-183">Győződjön meg arról, hogy Edison ugyanaz, mint a számítógép hálózati csatlakoztatott toohello.</span><span class="sxs-lookup"><span data-stu-id="86f2b-183">Make sure that Edison is connected toohello same network as your computer.</span></span> <span data-ttu-id="86f2b-184">A számítógép csatlakozik a tooyour Edison hello IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="86f2b-184">Your computer connects tooyour Edison by using hello IP address.</span></span>

<span data-ttu-id="86f2b-185">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="86f2b-185">Congratulations!</span></span> <span data-ttu-id="86f2b-186">Sikeresen konfigurálta az Edison.</span><span class="sxs-lookup"><span data-stu-id="86f2b-186">You've successfully configured Edison.</span></span>

## <a name="summary"></a><span data-ttu-id="86f2b-187">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="86f2b-187">Summary</span></span>
<span data-ttu-id="86f2b-188">A cikkben hogy megismerte hogyan tooassemble hello Edison üzenőfalon, a belső vezérlőprogram, a telepítő jelszó flash, és csatlakoztassa tooWi-Fi configuration tool használatával.</span><span class="sxs-lookup"><span data-stu-id="86f2b-188">In this article, you’ve learned how tooassemble hello Edison board, flash its firmware, setup password and connect it tooWi-Fi by using configuration tool.</span></span> <span data-ttu-id="86f2b-189">Vegye figyelembe, hogy hello LED nem még bonyolít le.</span><span class="sxs-lookup"><span data-stu-id="86f2b-189">Note that hello LED doesn't yet light up.</span></span> <span data-ttu-id="86f2b-190">hello következő feladata tooinstall hello szükséges eszközök és szoftverek mintaalkalmazás futó Edison előkészítésekor.</span><span class="sxs-lookup"><span data-stu-id="86f2b-190">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86f2b-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="86f2b-191">Next steps</span></span>
<span data-ttu-id="86f2b-192">[Hello eszközök beszerzése][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="86f2b-192">[Get hello tools][get-the-tools]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md