---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - lecke 1: eszközök konfigurálása |} Microsoft Docs"
description: "Intel Edison konfigurálása az első használatnál."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Állítsa be, arduino csatlakozni arduino pc, a telepítő arduino, arduino board"
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
ms.openlocfilehash: 87bf3a917af096e43a43a2143afa4bf43a72d7fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-intel-edison"></a><span data-ttu-id="92691-104">Az Intel Edison konfigurálása</span><span class="sxs-lookup"><span data-stu-id="92691-104">Configure your Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="92691-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="92691-105">What you will do</span></span>
<span data-ttu-id="92691-106">Intel Edison konfigurálása az első alkalommal a üzenőfalon, működtetéséhez azt fel összegyűjtésével és konfigurációs eszköz telepítése az asztali operációs Flash Edison a belső vezérlőprogram, hogy beállította a jelszót, és csatlakoztassa Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="92691-106">Configure Intel Edison for first-time use by assembling the board, powering it up and installing configuration tool to your desktop OS to flash Edison's firmware, set its password and connect it to Wi-Fi.</span></span> <span data-ttu-id="92691-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="92691-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="92691-108">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="92691-108">What you will learn</span></span>
<span data-ttu-id="92691-109">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="92691-109">In this article, you will learn:</span></span>

* <span data-ttu-id="92691-110">Hogyan Edison board össze, és kapcsolja be a mentést.</span><span class="sxs-lookup"><span data-stu-id="92691-110">How to assemble Edison board and power it up.</span></span>
* <span data-ttu-id="92691-111">Hogyan Edison a belső vezérlőprogram flash, jelszó beállítását, és csatlakozzon a Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="92691-111">How to flash Edison's firmware, set password and connect Wi-Fi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="92691-112">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="92691-112">What you need</span></span>
<span data-ttu-id="92691-113">A művelet végrehajtásához a következő részek a Intel Edison Starter Kit kell:</span><span class="sxs-lookup"><span data-stu-id="92691-113">To complete this operation, you need the following parts from your Intel Edison Starter Kit:</span></span>

* <span data-ttu-id="92691-114">Intel® Edison modul</span><span class="sxs-lookup"><span data-stu-id="92691-114">Intel® Edison module</span></span>
* <span data-ttu-id="92691-115">Arduino bővítése tábla</span><span class="sxs-lookup"><span data-stu-id="92691-115">Arduino expansion board</span></span>
* <span data-ttu-id="92691-116">Bármely Oszlopelválasztó sávok vagy csavart a csomagban, beleértve a két csavart vizsgálókocsihoz a bővítés board és négy csavart és műanyag térköztartók modul a része.</span><span class="sxs-lookup"><span data-stu-id="92691-116">Any spacer bars or screws included in the packaging, including two screws to fasten the module to the expansion board and four sets of screws and plastic spacers.</span></span>
* <span data-ttu-id="92691-117">A típus egy USB-kábel Micro B</span><span class="sxs-lookup"><span data-stu-id="92691-117">A Micro B to Type A USB cable</span></span>
* <span data-ttu-id="92691-118">A közvetlen aktuális (DC) tápegység.</span><span class="sxs-lookup"><span data-stu-id="92691-118">A direct current (DC) power supply.</span></span> <span data-ttu-id="92691-119">A tápegység kell tekinthető meg az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="92691-119">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="92691-120">7-15V TARTOMÁNYVEZÉRLŐ</span><span class="sxs-lookup"><span data-stu-id="92691-120">7-15V DC</span></span>
  - <span data-ttu-id="92691-121">Legalább 1500mA</span><span class="sxs-lookup"><span data-stu-id="92691-121">At least 1500mA</span></span>
  - <span data-ttu-id="92691-122">A Központ/belső PIN-kódot kell lennie a pozitív sarkpontot az energiaellátás</span><span class="sxs-lookup"><span data-stu-id="92691-122">The center/inner pin should be the positive pole of the power supply</span></span>

  ![Az Starter Kit dolgot](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

<span data-ttu-id="92691-124">Emellett a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="92691-124">You also need:</span></span>

* <span data-ttu-id="92691-125">Windows, Mac vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="92691-125">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="92691-126">Vezeték nélküli kapcsolat Edison való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="92691-126">A wireless connection for Edison to connect to.</span></span>
* <span data-ttu-id="92691-127">Az internethez konfigurációs eszköz letöltése.</span><span class="sxs-lookup"><span data-stu-id="92691-127">An Internet connection to download configuration tool.</span></span>

## <a name="assemble-your-board"></a><span data-ttu-id="92691-128">A tábla összeállítása</span><span class="sxs-lookup"><span data-stu-id="92691-128">Assemble your board</span></span>

<span data-ttu-id="92691-129">Ez a szakasz az Intel® Edison modul csatlakoztatni a bővítés board lépéseket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="92691-129">This section contains steps to attach your Intel® Edison module to your expansion board.</span></span>

1. <span data-ttu-id="92691-130">A bővítés üzenőfalon, a bővítés táblán csavart, a modul a lyuk sorba állítása az Intel® Edison modul, a fehér vázlatban helyezze el.</span><span class="sxs-lookup"><span data-stu-id="92691-130">Place the Intel® Edison module within the white outline on your expansion board, lining up the holes on the module with the screws on the expansion board.</span></span>

2. <span data-ttu-id="92691-131">Nyomja le a modul a szavakat alatt `What will you make?` csak úgy érzi, hogy a beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="92691-131">Press down on the module just below the words `What will you make?` until you feel a snap.</span></span>

   ![indítópanel 2 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. <span data-ttu-id="92691-133">A bővítés board modul védelmére használja a két hexadecimális nuts (a csomagban található).</span><span class="sxs-lookup"><span data-stu-id="92691-133">Use the two hex nuts (included in the package) to secure the module to the expansion board.</span></span>

   ![indítópanel 3 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. <span data-ttu-id="92691-135">Helyezze be elment egyet a négy sarok lyuk a bővítés táblán.</span><span class="sxs-lookup"><span data-stu-id="92691-135">Insert a screw in one of the four corner holes on the expansion board.</span></span> <span data-ttu-id="92691-136">Csavarás és szigoríthatja a csavart alakzatot fehér műanyag térköztartók egyikét.</span><span class="sxs-lookup"><span data-stu-id="92691-136">Twist and tighten one of the white plastic spacers onto the screw.</span></span>

   ![indítópanel 4 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. <span data-ttu-id="92691-138">Ismételje meg a másik három sarok térköztartók.</span><span class="sxs-lookup"><span data-stu-id="92691-138">Repeat for the other three corner spacers.</span></span>

   ![indítópanel 5 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

<span data-ttu-id="92691-140">Most már a üzenőfalon kész.</span><span class="sxs-lookup"><span data-stu-id="92691-140">Now your board is assembled.</span></span>

   ![üzenőfalon kész](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a><span data-ttu-id="92691-142">Energiagazdálkodási Edison mentése</span><span class="sxs-lookup"><span data-stu-id="92691-142">Power up Edison</span></span>

1. <span data-ttu-id="92691-143">Csatlakoztassa a tápegység.</span><span class="sxs-lookup"><span data-stu-id="92691-143">Plug in the power supply.</span></span>

   ![Beépülő modul tápegység](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. <span data-ttu-id="92691-145">A zöld LED-jét (DS1 feliratú a táblán Arduino * bővítése) kell bonyolít le, és megvilágítottnak maradnak.</span><span class="sxs-lookup"><span data-stu-id="92691-145">A green LED(labeled DS1 on the Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="92691-146">Várjon egy percet a kártya a rendszerindítás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="92691-146">Wait one minute for the board to finish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="92691-147">Ha nem rendelkezik a tartományvezérlő tápegység, továbbra is a USB-porton keresztül board power.</span><span class="sxs-lookup"><span data-stu-id="92691-147">If you do not have a DC power supply, you can still power the board through a USB port.</span></span> <span data-ttu-id="92691-148">Lásd: `Connect Edison to your computer` című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="92691-148">See `Connect Edison to your computer` section for details.</span></span> <span data-ttu-id="92691-149">A tábla, ilyen módon működtetéséhez azt eredményezheti, hogy a tábláról előre nem látható viselkedéshez különösen akkor, ha a Wi-Fi használatával, vagy befolyásoló tényezők motorok.</span><span class="sxs-lookup"><span data-stu-id="92691-149">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

## <a name="connect-edison-to-your-computer"></a><span data-ttu-id="92691-150">Edison kapcsolódni a számítógéphez</span><span class="sxs-lookup"><span data-stu-id="92691-150">Connect Edison to your computer</span></span>

1. <span data-ttu-id="92691-151">Váltás a két micro USB-porttal felé mikrokapcsoló le, hogy Edison eszköz módban van.</span><span class="sxs-lookup"><span data-stu-id="92691-151">Toggle down the microswitch towards the two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="92691-152">Az eszköz és a gazdagép üzemmód közötti különbségeket, tekintse át [Itt](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="92691-152">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Váltás a mikrokapcsoló le](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. <span data-ttu-id="92691-154">A micro USB-kábellel csatlakoztassa a felső micro USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="92691-154">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Felső micro USB-port](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. <span data-ttu-id="92691-156">Másik végén USB-kábellel csatlakoztassa a számítógépet.</span><span class="sxs-lookup"><span data-stu-id="92691-156">Plug the other end of USB cable into your computer.</span></span>

   ![Számítógép USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. <span data-ttu-id="92691-158">Tudni fogják, hogy a tábla teljes inicializálását, amikor a számítógép csatlakoztat egy új meghajtót (hasonlóan egy SD-kártya beszúrása a számítógép).</span><span class="sxs-lookup"><span data-stu-id="92691-158">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-the-configuration-tool"></a><span data-ttu-id="92691-159">A konfigurációs eszköz letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="92691-159">Download and run the configuration tool</span></span>
<span data-ttu-id="92691-160">A legfrissebb konfigurációs eszköz az beszerzése [erre a hivatkozásra](https://software.intel.com/en-us/iot/hardware/edison/downloads) tartozó a `Installers` fejléc.</span><span class="sxs-lookup"><span data-stu-id="92691-160">Get the latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under the `Installers` heading.</span></span> <span data-ttu-id="92691-161">Az eszköz hajtható végre, és kövesse a képernyőn megjelenő utasításokat, amennyiben szükséges Tovább gombra kattint</span><span class="sxs-lookup"><span data-stu-id="92691-161">Execute the tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="92691-162">Belső vezérlőprogram Flash</span><span class="sxs-lookup"><span data-stu-id="92691-162">Flash firmware</span></span>
1. <span data-ttu-id="92691-163">Az a `Set up options` kattintson `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="92691-163">On the `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="92691-164">Válassza ki a lemezképet flash alakzatot a tábla a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="92691-164">Select the image to flash onto your board by doing one of the following:</span></span>
   - <span data-ttu-id="92691-165">Töltse le, és a tábla a legújabb belső vezérlőprogram lemezképpel érhetők el az Intel flash, jelölje be `Download the latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="92691-165">To download and flash your board with the latest firmware image available from Intel, select `Download the latest image version xxxx`.</span></span>
   - <span data-ttu-id="92691-166">A tábla már mentette a számítógépen lemezképhez flash, jelölje be `Select the local image`.</span><span class="sxs-lookup"><span data-stu-id="92691-166">To flash your board with an image you already have saved on your computer, select `Select the local image`.</span></span> <span data-ttu-id="92691-167">Keresse meg és jelölje ki a flash a kártyához kívánt lemezképet.</span><span class="sxs-lookup"><span data-stu-id="92691-167">Browse to and select the image you want to flash to your board.</span></span>
3. <span data-ttu-id="92691-168">A telepítő eszköz megkísérli a tábla flash.</span><span class="sxs-lookup"><span data-stu-id="92691-168">The setup tool will attempt to flash your board.</span></span> <span data-ttu-id="92691-169">A teljes villogó folyamat akár 10 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="92691-169">The entire flashing process may take up to 10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="92691-170">Jelszó beállítása</span><span class="sxs-lookup"><span data-stu-id="92691-170">Set password</span></span>
1. <span data-ttu-id="92691-171">Az a `Set up options` kattintson `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="92691-171">On the `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="92691-172">Az Intel® Edison board egyéni nevet állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="92691-172">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="92691-173">Ez nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="92691-173">This is optional.</span></span>
3. <span data-ttu-id="92691-174">Adja meg a tábla jelszavát, majd kattintson az `Set password`.</span><span class="sxs-lookup"><span data-stu-id="92691-174">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="92691-175">A jelszót, amely később üzemszünetének.</span><span class="sxs-lookup"><span data-stu-id="92691-175">Mark down the password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="92691-176">Wi-Fi csatlakozás</span><span class="sxs-lookup"><span data-stu-id="92691-176">Connect Wi-Fi</span></span>
1. <span data-ttu-id="92691-177">Az a `Set up options` kattintson `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="92691-177">On the `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="92691-178">Várjon, amíg legfeljebb egy percig a számítógép elérhető Wi-Fi hálózatok keres.</span><span class="sxs-lookup"><span data-stu-id="92691-178">Wait up to one minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="92691-179">Az a `Detected Networks` legördülő listára, válassza ki a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="92691-179">From the `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="92691-180">Az a `Security` legördülő listára, válassza ki a hálózati biztonság típusa.</span><span class="sxs-lookup"><span data-stu-id="92691-180">From the `Security` drop-down list, select the network's security type.</span></span>
4. <span data-ttu-id="92691-181">Adja meg a felhasználónevet és jelszót információkat, majd kattintson az `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="92691-181">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="92691-182">Az IP-cím, amely később üzemszünetének.</span><span class="sxs-lookup"><span data-stu-id="92691-182">Mark down the IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="92691-183">Győződjön meg arról, hogy Edison és a számítógép ugyanahhoz a hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="92691-183">Make sure that Edison is connected to the same network as your computer.</span></span> <span data-ttu-id="92691-184">A számítógép csatlakozik a Edison az IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="92691-184">Your computer connects to your Edison by using the IP address.</span></span>

<span data-ttu-id="92691-185">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="92691-185">Congratulations!</span></span> <span data-ttu-id="92691-186">Sikeresen konfigurálta az Edison.</span><span class="sxs-lookup"><span data-stu-id="92691-186">You've successfully configured Edison.</span></span>

## <a name="summary"></a><span data-ttu-id="92691-187">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="92691-187">Summary</span></span>
<span data-ttu-id="92691-188">Ebben a cikkben korábban megtudta, hogyan állítsa össze a Edison üzenőfalon, a belső vezérlőprogram, a telepítő jelszó flash és konfigurációs eszköz használatával csatlakoztassa Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="92691-188">In this article, you’ve learned how to assemble the Edison board, flash its firmware, setup password and connect it to Wi-Fi by using configuration tool.</span></span> <span data-ttu-id="92691-189">Vegye figyelembe, hogy a LED nem még bonyolít le.</span><span class="sxs-lookup"><span data-stu-id="92691-189">Note that the LED doesn't yet light up.</span></span> <span data-ttu-id="92691-190">A következő feladathoz a rendszer telepíti a szükséges eszközök és szoftverek mintaalkalmazás futó Edison előkészítésekor.</span><span class="sxs-lookup"><span data-stu-id="92691-190">The next task is to install the necessary tools and software in preparation for running a sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92691-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92691-191">Next steps</span></span>
<span data-ttu-id="92691-192">[Eszközök][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="92691-192">[Get the tools][get-the-tools]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md