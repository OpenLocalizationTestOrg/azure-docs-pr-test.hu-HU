---
title: "Diagnosztikai eszköz, a StorSimple 8000 eszköz hibáinak elhárítása |} Microsoft Docs"
description: "A StorSimple eszköz módok és használatához a Windows PowerShell-lel módosíthatja eszköz módját ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 8fae7bb357f8e5e8eff249edfe3a2aaafe04283c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-storsimple-diagnostics-tool-to-troubleshoot-8000-series-device-issues"></a><span data-ttu-id="b7b07-103">A StorSimple diagnosztikai eszköz segítségével 8000 sorozat eszközök kapcsolatos problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="b7b07-103">Use the StorSimple Diagnostics Tool to troubleshoot 8000 series device issues</span></span>

## <a name="overview"></a><span data-ttu-id="b7b07-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b7b07-104">Overview</span></span>

<span data-ttu-id="b7b07-105">A StorSimple diagnosztikai eszköz diagnosztizálja a rendszer, a teljesítmény, a hálózati és a hardver összetevő állapota a StorSimple eszköz kapcsolatos problémákat.</span><span class="sxs-lookup"><span data-stu-id="b7b07-105">The StorSimple Diagnostics tool diagnoses issues related to system, performance, network, and hardware component health for a StorSimple device.</span></span> <span data-ttu-id="b7b07-106">A diagnosztikai eszköz használható a különböző forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="b7b07-106">The diagnostics tool can be used in various scenarios.</span></span> <span data-ttu-id="b7b07-107">Ilyen például, alkalmazások és szolgáltatások tervezése, a StorSimple eszköz üzembe helyezése, a hálózati környezet értékeléséhez és az operatív eszközök teljesítményének meghatározására.</span><span class="sxs-lookup"><span data-stu-id="b7b07-107">These scenarios include workload planning, deploying a StorSimple device, assessing the network environment, and determining the performance of an operational device.</span></span> <span data-ttu-id="b7b07-108">Ez a cikk áttekintést a diagnosztikai eszköz, és ismerteti, hogyan használható az eszközt a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-108">This article provides an overview of the diagnostics tool and describes how the tool can be used with a StorSimple device.</span></span>

<span data-ttu-id="b7b07-109">A diagnosztikai eszköz elsődlegesen a StorSimple 8000 series a helyszíni eszközök (8100 és 8600).</span><span class="sxs-lookup"><span data-stu-id="b7b07-109">The diagnostics tool is primarily intended for StorSimple 8000 series on-premises devices (8100 and 8600).</span></span>

## <a name="run-diagnostics-tool"></a><span data-ttu-id="b7b07-110">Diagnosztikai eszköz futtatása</span><span class="sxs-lookup"><span data-stu-id="b7b07-110">Run diagnostics tool</span></span>

<span data-ttu-id="b7b07-111">Ezt az eszközt a StorSimple eszköz a Windows PowerShell felületén keresztül is futtatható.</span><span class="sxs-lookup"><span data-stu-id="b7b07-111">This tool can be run via the Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="b7b07-112">A helyi kapcsolat az eszköz eléréséhez két módja van:</span><span class="sxs-lookup"><span data-stu-id="b7b07-112">There are two ways to access the local interface of your device:</span></span>

* <span data-ttu-id="b7b07-113">[A PuTTY használata az eszköz soros konzoljához való csatlakozáshoz](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="b7b07-113">[Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
* <span data-ttu-id="b7b07-114">[Távoli eléréséhez a Windows PowerShell eszközt a StorSimple](storsimple-remote-connect.md).</span><span class="sxs-lookup"><span data-stu-id="b7b07-114">[Remotely access the tool via the Windows PowerShell for StorSimple](storsimple-remote-connect.md).</span></span>

<span data-ttu-id="b7b07-115">Ebben a cikkben azt feltételezzük, hogy az eszköz soros konzoljához PuTTY keresztül csatlakoztatott.</span><span class="sxs-lookup"><span data-stu-id="b7b07-115">In this article, we assume that you have connected to the device serial console via PuTTY.</span></span>

#### <a name="to-run-the-diagnostics-tool"></a><span data-ttu-id="b7b07-116">A diagnosztikai eszköz futtatása</span><span class="sxs-lookup"><span data-stu-id="b7b07-116">To run the diagnostics tool</span></span>

<span data-ttu-id="b7b07-117">Miután csatlakozott az eszköz a Windows PowerShell felületén, hajtsa végre az alábbi lépéseket a parancsmag futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b7b07-117">Once you have connected to the Windows PowerShell interface of the device, perform the following steps to run the cmdlet.</span></span>
1. <span data-ttu-id="b7b07-118">Jelentkezzen be az eszköz soros konzoljához lépéseit követve [a PuTTY használata az eszköz soros konzoljához való csatlakozáshoz](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="b7b07-118">Log on to the device serial console by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>

2. <span data-ttu-id="b7b07-119">Írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b7b07-119">Type the following command:</span></span>

    `Invoke-HcsDiagnostics`

    <span data-ttu-id="b7b07-120">Ha a hatókör-paraméter nincs megadva, a parancsmag a diagnosztikai tesztek hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="b7b07-120">If the scope parameter is not specified, the cmdlet executes all the diagnostic tests.</span></span> <span data-ttu-id="b7b07-121">Ezek a tesztek közé tartozik a rendszer, a hardver összetevő állapotát, a hálózati és a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="b7b07-121">These tests include system, hardware component health, network, and performance.</span></span> 
    
    <span data-ttu-id="b7b07-122">Csak egy adott teszt futtatásához, adja meg a hatókör-paramétert.</span><span class="sxs-lookup"><span data-stu-id="b7b07-122">To run only a specific test, specify the scope parameter.</span></span> <span data-ttu-id="b7b07-123">Például csak a hálózati tesztek futtatásához írja be</span><span class="sxs-lookup"><span data-stu-id="b7b07-123">For instance, to run only the network test, type</span></span>

    `Invoke-HcsDiagnostics -Scope Network`

3. <span data-ttu-id="b7b07-124">Jelölje ki, és a kimeneti másolja a PuTTY ablakból egy szövegfájlba további elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="b7b07-124">Select and copy the output from the PuTTY window into a text file for further analysis.</span></span>

## <a name="scenarios-to-use-the-diagnostics-tool"></a><span data-ttu-id="b7b07-125">A diagnosztikai eszköz forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="b7b07-125">Scenarios to use the diagnostics tool</span></span>

<span data-ttu-id="b7b07-126">A diagnosztikai eszköz segítségével hibaelhárítása a rendszer a hálózaton, a teljesítmény, a rendszer és a hardver állapotát.</span><span class="sxs-lookup"><span data-stu-id="b7b07-126">Use the diagnostics tool to troubleshoot the network, performance, system, and hardware health of the system.</span></span> <span data-ttu-id="b7b07-127">Az alábbiakban néhány lehetséges forgatókönyv szerint:</span><span class="sxs-lookup"><span data-stu-id="b7b07-127">Here are some possible scenarios:</span></span>

* <span data-ttu-id="b7b07-128">**Az eszköz offline** -a StorSimple 8000 series eszköz offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="b7b07-128">**Device offline** - Your StorSimple 8000 series device is offline.</span></span> <span data-ttu-id="b7b07-129">Azonban a Windows PowerShell felületén, úgy tűnik, hogy mindkét vezérlőhöz megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="b7b07-129">However, from the Windows PowerShell interface, it seems that both the controllers are up and running.</span></span>
    * <span data-ttu-id="b7b07-130">Az eszköz segítségével ellenőrizze a hálózati állapotot.</span><span class="sxs-lookup"><span data-stu-id="b7b07-130">You can use this tool to then determine the network state.</span></span>
         
         > [!NOTE]
         > <span data-ttu-id="b7b07-131">Az eszköz nem használható fel annak ellenőrzéséhez, teljesítmény- és hálózati beállításokat a regisztrációs (vagy keresztül telepítővarázsló konfigurálása) előtt egy eszközön.</span><span class="sxs-lookup"><span data-stu-id="b7b07-131">Do not use this tool to assess performance and network settings on a device before the registration (or configuring via setup wizard).</span></span> <span data-ttu-id="b7b07-132">Egy érvényes IP-cím hozzá van rendelve az eszköz telepítővarázslóját, és a regisztráció során.</span><span class="sxs-lookup"><span data-stu-id="b7b07-132">A valid IP is assigned to the device during setup wizard and registration.</span></span> <span data-ttu-id="b7b07-133">Olyan eszköz, amely nincs regisztrálva, a hardver és a rendszer ezt a parancsmagot futtathatja.</span><span class="sxs-lookup"><span data-stu-id="b7b07-133">You can run this cmdlet, on a device that is not registered, for hardware health and system.</span></span> <span data-ttu-id="b7b07-134">Használja például a hatókör-paramétert:</span><span class="sxs-lookup"><span data-stu-id="b7b07-134">Use the scope parameter, for example:</span></span>
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* <span data-ttu-id="b7b07-135">**Állandó eszközökkel kapcsolatos problémákat** -eszközökkel kapcsolatos problémákat, amelyek az adatok megőrzéséhez tapasztalja.</span><span class="sxs-lookup"><span data-stu-id="b7b07-135">**Persistent device issues** - You are experiencing device issues that seem to persist.</span></span> <span data-ttu-id="b7b07-136">Például regisztrációja sikertelen.</span><span class="sxs-lookup"><span data-stu-id="b7b07-136">For instance, registration is failing.</span></span> <span data-ttu-id="b7b07-137">Sikerült is lehet tapasztal eszközökkel kapcsolatos problémákat Miután az eszköz sikeresen regisztrált és működési egy ideig.</span><span class="sxs-lookup"><span data-stu-id="b7b07-137">You could also be experiencing device issues after the device is successfully registered and operational for a while.</span></span>
    * <span data-ttu-id="b7b07-138">Ebben az esetben ezzel az eszközzel előzetes hibaelhárítási, mielőtt bejelentkezik egy szolgáltatási kérelem Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="b7b07-138">In this case, use this tool for preliminary troubleshooting before you log a service request with Microsoft Support.</span></span> <span data-ttu-id="b7b07-139">Azt javasoljuk, hogy az eszköz futtatásához, és ez az eszköz kimenetét rögzíti.</span><span class="sxs-lookup"><span data-stu-id="b7b07-139">We recommend that you run this tool and capture the output of this tool.</span></span> <span data-ttu-id="b7b07-140">Ezután az hibaelhárítás elősegítésére támogatáshoz adja meg a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="b7b07-140">You can then provide this output to Support to expedite troubleshooting.</span></span>
    * <span data-ttu-id="b7b07-141">Minden összetevő vagy a fürt hardverhibák esetén be kell jelentkezni egy támogatási kérést.</span><span class="sxs-lookup"><span data-stu-id="b7b07-141">If there are any hardware component or cluster failures, you should log in a Support request.</span></span>

* <span data-ttu-id="b7b07-142">**Kevés a Teljesítmény eszköz** -a StorSimple eszköz lassú.</span><span class="sxs-lookup"><span data-stu-id="b7b07-142">**Low device performance** - Your StorSimple device is slow.</span></span>
    * <span data-ttu-id="b7b07-143">Ebben az esetben futtassa ezt a parancsmagot a teljesítmény beállítása hatókör-paramétert.</span><span class="sxs-lookup"><span data-stu-id="b7b07-143">In this case, run this cmdlet with scope parameter set to performance.</span></span> <span data-ttu-id="b7b07-144">Vizsgálja meg a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="b7b07-144">Analyze the output.</span></span> <span data-ttu-id="b7b07-145">A felhő írható-olvasható késések beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b7b07-145">You get the cloud read-write latencies.</span></span> <span data-ttu-id="b7b07-146">A jelentésben szereplő késleltetések maximális elérhető célként használni, számításba a némi többletterhelést okoz, a belső az adatok feldolgozásához, és telepíteni a feladatait, a rendszer.</span><span class="sxs-lookup"><span data-stu-id="b7b07-146">Use the reported latencies as maximum achievable target, factor in some overhead for the internal data processing, and then deploy the workloads on the system.</span></span> <span data-ttu-id="b7b07-147">További információkért látogasson el [a hálózati tesztek segítségével eszköz elhárítása](#network-test).</span><span class="sxs-lookup"><span data-stu-id="b7b07-147">For more information, go to [Use the network test to troubleshoot device performance](#network-test).</span></span>


## <a name="diagnostics-test-and-sample-outputs"></a><span data-ttu-id="b7b07-148">Diagnosztika teszt- és minta kimenet</span><span class="sxs-lookup"><span data-stu-id="b7b07-148">Diagnostics test and sample outputs</span></span>

### <a name="hardware-test"></a><span data-ttu-id="b7b07-149">Hardver ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b7b07-149">Hardware test</span></span>

<span data-ttu-id="b7b07-150">Ez a vizsgálat meghatározza a hardverösszetevők, a legpontosabb Beállításhoz belső vezérlőprogram és a lemez belső vezérlőprogramját a rendszeren futó állapotát.</span><span class="sxs-lookup"><span data-stu-id="b7b07-150">This test determines the status of the hardware components, the USM firmware, and the disk firmware running on your system.</span></span>

* <span data-ttu-id="b7b07-151">A jelentett hardverösszetevőket összetevőket, a teszt sikertelen, vagy nincsenek jelen a rendszerben.</span><span class="sxs-lookup"><span data-stu-id="b7b07-151">The hardware components reported are those components that failed the test or are not present in the system.</span></span>
* <span data-ttu-id="b7b07-152">A legpontosabb Beállításhoz belső vezérlőprogram és lemez belsővezérlőprogram-verziók a vezérlő 0, a vezérlő 1 jelzett, és a rendszer a megosztott összetevőit.</span><span class="sxs-lookup"><span data-stu-id="b7b07-152">The USM firmware and disk firmware versions are reported for the Controller 0, Controller 1, and shared components in your system.</span></span> <span data-ttu-id="b7b07-153">A teljes listáját, és hardverösszetevők Ugrás:</span><span class="sxs-lookup"><span data-stu-id="b7b07-153">For a complete list of hardware components, go to:</span></span>

    * [<span data-ttu-id="b7b07-154">Elsődleges szolgáltatással összetevők</span><span class="sxs-lookup"><span data-stu-id="b7b07-154">Components in primary enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [<span data-ttu-id="b7b07-155">Összetevők EBOD szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b7b07-155">Components in EBOD enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> <span data-ttu-id="b7b07-156">Ha a hardver teszt eredményeként összetevő, [jelentkezzen be Microsoft Support szolgáltatáskérés](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="b7b07-156">If the hardware test reports failed components, [log in a service request with Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a><span data-ttu-id="b7b07-157">Minta kimenet hardveren végzett ellenőrzéshez egy 8100-eszközön fut</span><span class="sxs-lookup"><span data-stu-id="b7b07-157">Sample output of hardware test run on an 8100 device</span></span>

<span data-ttu-id="b7b07-158">Íme egy minta kimenet egy StorSimple 8100 eszközről.</span><span class="sxs-lookup"><span data-stu-id="b7b07-158">Here is a sample output from a StorSimple 8100 device.</span></span> <span data-ttu-id="b7b07-159">A EBOD ház nincs jelen a 8100-as modell eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-159">In the 8100 model device, the EBOD enclosure is not present.</span></span> <span data-ttu-id="b7b07-160">Emiatt a EBOD vezérlő összetevők nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b7b07-160">Hence, the EBOD controller components are not reported.</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Hardware
Running hardware diagnostics ...
--------------------------------------------------
Hardware components failed or not present
----------------------

           Type           State      Controller           Index     EnclosureId
           ----           -----      ----------           -----     -----------
...rVipResource      NotPresent            None               1            None
...rVipResource      NotPresent            None               2            None
...rVipResource      NotPresent            None               3            None
...rVipResource      NotPresent            None               4            None
...rVipResource      NotPresent            None               5            None
...rVipResource      NotPresent            None               6            None
...rVipResource      NotPresent            None               7            None
...rVipResource      NotPresent            None               8            None
...rVipResource      NotPresent            None               9            None
...rVipResource      NotPresent            None              10            None
...rVipResource      NotPresent            None              11            None

Firmware information
----------------------
TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

--------------------------------------------------
```

### <a name="system-test"></a><span data-ttu-id="b7b07-161">Rendszer-teszt</span><span class="sxs-lookup"><span data-stu-id="b7b07-161">System test</span></span>

<span data-ttu-id="b7b07-162">Ez a vizsgálat a rendszer-információkat, a rendelkezésre álló frissítések, a fürt információkat és a szolgáltatás adatait az eszköz jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="b7b07-162">This test reports the system information, the updates available, the cluster information, and the service information for your device.</span></span>

* <span data-ttu-id="b7b07-163">A rendszer-információkat tartalmazza a modell, sorozatszámát, időzóna, tartományvezérlő állapotát, és a rendszeren futó részletes szoftververzió.</span><span class="sxs-lookup"><span data-stu-id="b7b07-163">The system information includes the model, device serial number, time zone, controller status, and the detailed software version running on the system.</span></span> <span data-ttu-id="b7b07-164">A kimeneti jelentett különböző rendszer paraméterek megértéséhez, keresse fel [Rendszerinformáció értelmezése](#appendix-interpreting-system-information).</span><span class="sxs-lookup"><span data-stu-id="b7b07-164">To understand the various system parameters reported as the output, go to [Interpreting system information](#appendix-interpreting-system-information).</span></span>

* <span data-ttu-id="b7b07-165">A frissítés rendelkezésre állási jelenti, hogy elérhetők-e a normál és a karbantartási mód és a csomaghoz kapcsolódó nevek.</span><span class="sxs-lookup"><span data-stu-id="b7b07-165">The update availability reports whether the regular and maintenance modes are available and their associated package names.</span></span> <span data-ttu-id="b7b07-166">Ha `RegularUpdates` és `MaintenanceModeUpdates` vannak `false`, ez azt jelzi, hogy a frissítések nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b7b07-166">If `RegularUpdates` and `MaintenanceModeUpdates` are `false`, this indicates that the updates are not available.</span></span> <span data-ttu-id="b7b07-167">Az eszköz naprakész állapotban.</span><span class="sxs-lookup"><span data-stu-id="b7b07-167">Your device is up-to-date.</span></span>
* <span data-ttu-id="b7b07-168">A fürt adatokat a a HCS fürt összes csoport és a hozzájuk megfelelő állapotok logikai összetevők információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-168">The cluster information contains the information on various logical components of all the HCS cluster groups and their respective statuses.</span></span> <span data-ttu-id="b7b07-169">Ha megjelenik egy kapcsolat nélküli fürtcsoport ebben a szakaszban a jelentés [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="b7b07-169">If you see an offline cluster group in this section of the report, [contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="b7b07-170">A szolgáltatás információk közé tartozik, a neveket és az eszközön futó összes HCS és a CIS szükségességét Services állapotok.</span><span class="sxs-lookup"><span data-stu-id="b7b07-170">The service information includes the names and statuses of all the HCS and CiS services running on your device.</span></span> <span data-ttu-id="b7b07-171">Ez az információ akkor hasznos, for a Microsoft Support az eszköz a probléma elhárításához.</span><span class="sxs-lookup"><span data-stu-id="b7b07-171">This information is helpful for the Microsoft Support in troubleshooting the device issue.</span></span>

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a><span data-ttu-id="b7b07-172">Minta kimenet egy 8100-eszközön fut, a rendszer vizsgálat</span><span class="sxs-lookup"><span data-stu-id="b7b07-172">Sample output of system test run on an 8100 device</span></span>

<span data-ttu-id="b7b07-173">Íme egy minta kimenet, a rendszer vizsgálat futtatása egy 8100-as eszközön.</span><span class="sxs-lookup"><span data-stu-id="b7b07-173">Here is a sample output of the system test run on an 8100 device.</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope System
Running system diagnostics ...
--------------------------------------------------

System information
----------------------
Controller0:

InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                : (UTC-08:00) Pacific Time (US & Canada)
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : Disabled
FipsMode                : Enabled

Controller1:
InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                :
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : HttpsAndHttpEnabled
FipsMode                : Enabled

Update availability
----------------------
RegularUpdates              : False
MaintenanceModeUpdates      : False
RegularUpdatesTitle         : {}
MaintenanceModeUpdatesTitle : {}

Cluster information
----------------------
Name                          State OwnerGroup
----                          ----- ----------
ApplicationHostRLUA           Online HCS Cluster Group
Data0v4                       Online HCS Cluster Group
HCS Vnic Resource             Online HCS Cluster Group
hcs_cloud_connectivity_...    Online HCS Cluster Group
hcs_controller_replacement    Online HCS Cluster Group
hcs_datapath_service          Online HCS Cluster Group
hcs_management_servic         Online HCS Cluster Group
hcs_nvram_service             Online HCS Cluster Group
hcs_passive_datapath          Online HCS Passive Cluster Group
hcs_platform_service          Online HCS Cluster Group
hcs_saas_agent_service        Online HCS Cluster Group
HddDataClusterDisk            Online HCS Cluster Group
HddMgmtClusterDisk            Online HCS Cluster Group
HddReplClusterDisk            Online HCS Cluster Group
iSCSI Target Server           Online HCS Cluster Group
NvramClusterDisk              Online HCS Cluster Group
SSAdminRLUA                   Online HCS Cluster Group
SsdDataClusterDisk            Online HCS Cluster Group
SsdNvramClusterDisk           Online HCS Cluster Group

Service information
----------------------
Name                                          Status DisplayName
----                                          ------ -----------
CiSAgentSvc                                   Stopped CiS Service Agent
hcs_cloud_connectivity_...                    Running hcs_cloud_connectivity...
hcs_controller_replacement                    Running HCS Controller Replace...
hcs_datapath_service                          Running HCS Datapath Service
hcs_management_service                        Running HCS Management Service
hcs_minishell                                 Running hcs_minishell
HCS_NVRAM_Service                             Running HCS NVRAM Service
hcs_passive_datapath                          Stopped HCS Passive Datapath S...
hcs_platform_service                          Running HCS Platform Monitor S...
hcs_saas_agent_service                        Running hcs_saas_agent_service
hcs_startup                                   Stopped hcs_startup
--------------------------------------------------
```

### <a name="network-test"></a><span data-ttu-id="b7b07-174">Hálózati tesztek</span><span class="sxs-lookup"><span data-stu-id="b7b07-174">Network test</span></span>

<span data-ttu-id="b7b07-175">A hálózati adapterek, portok, a DNS és NTP kapcsolat a kiszolgálóval, SSL tanúsítvány, tárfiók hitelesítő adatait, a frissítési kiszolgálók és webes proxy kapcsolatot a StorSimple eszköz állapotát ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="b7b07-175">This test validates the status of the network interfaces, ports, DNS and NTP server connectivity, SSL certificate, storage account credentials, connectivity to the Update servers, and web proxy connectivity on your StorSimple device.</span></span>

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a><span data-ttu-id="b7b07-176">Minta kimenet hálózat csak DATA0 engedélyezésekor a rendszer tesztelése</span><span class="sxs-lookup"><span data-stu-id="b7b07-176">Sample output of network test when only DATA0 is enabled</span></span>

<span data-ttu-id="b7b07-177">Íme egy minta kimenet a 8100-eszköz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-177">Here is a sample output of the 8100 device.</span></span> <span data-ttu-id="b7b07-178">A kimenet látható, amely:</span><span class="sxs-lookup"><span data-stu-id="b7b07-178">You can see in the output that:</span></span>
* <span data-ttu-id="b7b07-179">Csak DATA 0 DATA 1 hálózati adapter engedélyezve és konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b7b07-179">Only DATA 0 and DATA 1 network interfaces are enabled and configured.</span></span>
* <span data-ttu-id="b7b07-180">2 – 5 adatok nincsenek engedélyezve a portálon.</span><span class="sxs-lookup"><span data-stu-id="b7b07-180">DATA 2 - 5 are not enabled in the portal.</span></span>
* <span data-ttu-id="b7b07-181">Érvénytelen, a DNS-kiszolgáló konfigurációját, és az eszközök csatlakozhatnak a DNS-kiszolgálón keresztül.</span><span class="sxs-lookup"><span data-stu-id="b7b07-181">The DNS server configuration is valid and the device can connect via the DNS server.</span></span>
* <span data-ttu-id="b7b07-182">Az NTP-kiszolgáló kapcsolatát is rendben.</span><span class="sxs-lookup"><span data-stu-id="b7b07-182">The NTP server connectivity is also fine.</span></span>
* <span data-ttu-id="b7b07-183">80-as és 443-as port nyitva.</span><span class="sxs-lookup"><span data-stu-id="b7b07-183">Ports 80 and 443 are open.</span></span> <span data-ttu-id="b7b07-184">Azonban 9354-es port blokkolva van.</span><span class="sxs-lookup"><span data-stu-id="b7b07-184">However, port 9354 is blocked.</span></span> <span data-ttu-id="b7b07-185">Alapján a [hálózati rendszerkövetelmények](storsimple-system-requirements.md), meg kell nyitni ezt a portot, a service bus való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-185">Based on the [system network requirements](storsimple-system-requirements.md), you need to open this port for the service bus communication.</span></span>
* <span data-ttu-id="b7b07-186">Az SSL-tanúsítvány érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="b7b07-186">The SSL certification is valid.</span></span>
* <span data-ttu-id="b7b07-187">Az eszközök csatlakozhatnak a tárfiók: _myss8000storageacct_.</span><span class="sxs-lookup"><span data-stu-id="b7b07-187">The device can connect to the storage account: _myss8000storageacct_.</span></span>
* <span data-ttu-id="b7b07-188">Az Update-kiszolgálókon kapcsolat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="b7b07-188">The connectivity to Update servers is valid.</span></span>
* <span data-ttu-id="b7b07-189">A webalkalmazás-proxy nincs beállítva ezen az eszközön.</span><span class="sxs-lookup"><span data-stu-id="b7b07-189">The web proxy is not configured on this device.</span></span>

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a><span data-ttu-id="b7b07-190">Minta kimenet hálózati vizsgálat DATA0 és adat1 engedélyezésekor.</span><span class="sxs-lookup"><span data-stu-id="b7b07-190">Sample output of network test when DATA0 and DATA1 are enabled</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Network
Running network diagnostics ....
--------------------------------------------------
Validating networks .....
Name                Entity              Result              Details
----                ------              ------              -------
Network interface   Data0               Valid
Network interface   Data1               Valid
Network interface   Data2               Not enabled
Network interface   Data3               Not enabled
Network interface   Data4               Not enabled
Network interface   Data5               Not enabled
DNS                 10.222.118.154      Valid
NTP                 time.windows.com    Valid
Port                80                  Open
Port                443                 Open
Port                9354                Blocked
SSL certificate     https://myss8000... Valid
Storage account ... myss8000storageacct Valid
URL                 http://download.... Valid
URL                 http://download.... Valid
Web proxy                               Not enabled         Web proxy is not...
--------------------------------------------------
```

### <a name="performance-test"></a><span data-ttu-id="b7b07-191">Teljesítményteszt</span><span class="sxs-lookup"><span data-stu-id="b7b07-191">Performance test</span></span>

<span data-ttu-id="b7b07-192">Ez a vizsgálat a jelentés a felhő teljesítmény keresztül a felhő írható-olvasható késések fordulnak elő az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-192">This test reports the cloud performance via the cloud read-write latencies for your device.</span></span> <span data-ttu-id="b7b07-193">Ez az eszköz segítségével érhető el a StorSimple felhő teljesítménybeli meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="b7b07-193">This tool can be used to establish a baseline of the cloud performance that you can achieve with StorSimple.</span></span> <span data-ttu-id="b7b07-194">Az eszköz a maximális teljesítmény (ajánlott eset az olvasási és írási késések), amely a kapcsolat kaphat jelenti.</span><span class="sxs-lookup"><span data-stu-id="b7b07-194">The tool reports the maximum performance (best case scenario for read-write latencies) that you can get for your connection.</span></span>

<span data-ttu-id="b7b07-195">Az eszköz jelenti a legnagyobb elérhető teljesítmény, azt használatával a jelentésben szereplő késleltetések írható-olvasható célként a munkaterhelések telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="b7b07-195">As the tool reports the maximum achievable performance, we can use the reported read-write latencies as targets when deploying the workloads.</span></span>

<span data-ttu-id="b7b07-196">A vizsgálat a blob mérete, a másik kötetre típusok, az eszközön társított szimulálja.</span><span class="sxs-lookup"><span data-stu-id="b7b07-196">The test simulates the blob sizes associated with the different volume types on the device.</span></span> <span data-ttu-id="b7b07-197">Rendszeres rétegzett, és a helyileg rögzített kötetek biztonsági mentések használja a 64 KB-os blob mérete.</span><span class="sxs-lookup"><span data-stu-id="b7b07-197">Regular tiered and backups of locally pinned volumes use a 64 KB blob size.</span></span> <span data-ttu-id="b7b07-198">Rétegzett kötetek archív beállítás be van jelölve a 512 KB blob adatok mérete.</span><span class="sxs-lookup"><span data-stu-id="b7b07-198">Tiered volumes with archive option checked use 512 KB blob data size.</span></span> <span data-ttu-id="b7b07-199">Ha az eszköznek rétegzett és helyileg rögzített kötetek konfigurált, csak a megfelelő adatok mérete futtatása 64 KB-os blob vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="b7b07-199">If your device has tiered and locally pinned volumes configured, only the test corresponding to 64 KB blob data size is run.</span></span>

<span data-ttu-id="b7b07-200">Az eszköz használatához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b7b07-200">To use this tool, perform the following steps:</span></span>

1.  <span data-ttu-id="b7b07-201">Először hozzon létre a rétegzett kötetek és a rétegzett kötetek archivált beállítás be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="b7b07-201">First, create a mix of tiered volumes and tiered volumes with archived option checked.</span></span> <span data-ttu-id="b7b07-202">Ez a művelet biztosítja, hogy az eszköz a teszteket 64 KB-os és 512 KB blob mérete.</span><span class="sxs-lookup"><span data-stu-id="b7b07-202">This action ensures that the tool runs the tests for both 64 KB and 512 KB blob sizes.</span></span>

2. <span data-ttu-id="b7b07-203">Miután létrehozott és a kötetek konfigurált, futtassa a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="b7b07-203">Run the cmdlet after you have created and configured the volumes.</span></span> <span data-ttu-id="b7b07-204">Típus:</span><span class="sxs-lookup"><span data-stu-id="b7b07-204">Type:</span></span>

    `Invoke-HcsDiagnostics -Scope Performance`

3. <span data-ttu-id="b7b07-205">Jegyezze fel az eszköz által jelentett írható-olvasható késések fordulnak elő.</span><span class="sxs-lookup"><span data-stu-id="b7b07-205">Make a note of the read-write latencies reported by the tool.</span></span> <span data-ttu-id="b7b07-206">Ez a vizsgálat futtatása előtt a rendszer jelzi, hogy az eredmények több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="b7b07-206">This test can take several minutes to run before it reports the results.</span></span>

4. <span data-ttu-id="b7b07-207">Ha a kapcsolat a várt tartományon a vannak, majd a késések fordulnak elő a eszköz által használható maximális elérhető célként a munkaterhelések telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="b7b07-207">If the connection latencies are all under the expected range, then the latencies reported by the tool can be used as maximum achievable target when deploying the workloads.</span></span> <span data-ttu-id="b7b07-208">Figyelembe a belső adatkezelési némi többletterhelést okoz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-208">Factor in some overhead for internal data processing.</span></span>

    <span data-ttu-id="b7b07-209">Ha az írható-olvasható késések fordulnak elő a diagnosztikai eszköz által jelentett magas:</span><span class="sxs-lookup"><span data-stu-id="b7b07-209">If the read-write latencies reported by the diagnostics tool are high:</span></span>

    1. <span data-ttu-id="b7b07-210">Tárolási analitika konfigurálása a blob-szolgáltatásokhoz, és elemezheti az az Azure storage-fiók késések megértése.</span><span class="sxs-lookup"><span data-stu-id="b7b07-210">Configure Storage Analytics for blob services and analyze the output to understand the latencies for the Azure storage account.</span></span> <span data-ttu-id="b7b07-211">A részletes utasításokat lásd [engedélyezheti és konfigurálhatja a tárolási analitika](../storage/common/storage-enable-and-view-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="b7b07-211">For detailed instructions, go to [enable and configure Storage Analytics](../storage/common/storage-enable-and-view-metrics.md).</span></span> <span data-ttu-id="b7b07-212">Ha ezek késések is magas és a StorSimple diagnosztikai eszköz kapott számok hasonló, majd kell bejelentkeznie egy szolgáltatási kérelmet az Azure storage szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="b7b07-212">If those latencies are also high and comparable to the numbers you received from the StorSimple Diagnostics tool, then you need to log a service request with Azure storage.</span></span>

    2. <span data-ttu-id="b7b07-213">Ha a tárolási fiók késések alacsony, a hálózati rendszergazdától vizsgálja meg a hálózati késés hibát.</span><span class="sxs-lookup"><span data-stu-id="b7b07-213">If the storage account latencies are low, contact your network administrator to investigate any latency issues in your network.</span></span>

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a><span data-ttu-id="b7b07-214">Minta kimenet teljesítmény vizsgálat futtatása egy 8100-eszközön</span><span class="sxs-lookup"><span data-stu-id="b7b07-214">Sample output of performance test run on an 8100 device</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Performance
Running performance diagnostics...
--------------------------------------------------
Cloud performance: writing blobs
Cloud write latency: 194 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: reading blobs..
Cloud read latency: 544 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: writing blobs.
Cloud write latency: 369 ms using credential 'myss8000storageacct', blob size '512KB'
Cloud performance: reading blobs...
Cloud read latency: 4924 ms using credential 'myss8000storageacct', blob size '512KB'
--------------------------------------------------
Controller0>
```

## <a name="appendix-interpreting-system-information"></a><span data-ttu-id="b7b07-215">A függelék: rendszeradatok értelmezése</span><span class="sxs-lookup"><span data-stu-id="b7b07-215">Appendix: interpreting system information</span></span>

<span data-ttu-id="b7b07-216">Ez a táblázat a különböző Windows PowerShell paraméterek a rendszer adatokat leképezés:.</span><span class="sxs-lookup"><span data-stu-id="b7b07-216">Here is a table describing what the various Windows PowerShell parameters in the system information map to.</span></span> 

| <span data-ttu-id="b7b07-217">PowerShell-paraméter</span><span class="sxs-lookup"><span data-stu-id="b7b07-217">PowerShell Parameter</span></span>    | <span data-ttu-id="b7b07-218">Leírás</span><span class="sxs-lookup"><span data-stu-id="b7b07-218">Description</span></span>  |
|-------------------------|------------------|
| <span data-ttu-id="b7b07-219">Instance ID (Példányazonosító)</span><span class="sxs-lookup"><span data-stu-id="b7b07-219">Instance ID</span></span>             | <span data-ttu-id="b7b07-220">Minden tartományvezérlő egyedi azonosítóval rendelkezik, vagy a vele társított egy GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="b7b07-220">Every controller has a unique identifier or a GUID associated with it.</span></span>|
| <span data-ttu-id="b7b07-221">Név</span><span class="sxs-lookup"><span data-stu-id="b7b07-221">Name</span></span>                    | <span data-ttu-id="b7b07-222">Az eszköz üzembe helyezése során az Azure portálon keresztül konfigurált eszköz rövid neve.</span><span class="sxs-lookup"><span data-stu-id="b7b07-222">The friendly name of the device as configured through the Azure portal during device deployment.</span></span> <span data-ttu-id="b7b07-223">Az alapértelmezett felhasználóbarát név az eszköz sorozatszámát.</span><span class="sxs-lookup"><span data-stu-id="b7b07-223">The default friendly name is the device serial number.</span></span> |
| <span data-ttu-id="b7b07-224">Modell</span><span class="sxs-lookup"><span data-stu-id="b7b07-224">Model</span></span>                   | <span data-ttu-id="b7b07-225">A StorSimple 8000 series eszköz modelljét.</span><span class="sxs-lookup"><span data-stu-id="b7b07-225">The model of your StorSimple 8000 series device.</span></span> <span data-ttu-id="b7b07-226">A modell 8100 vagy 8600 lehet.</span><span class="sxs-lookup"><span data-stu-id="b7b07-226">The model can be 8100 or 8600.</span></span>|
| <span data-ttu-id="b7b07-227">Sorozatszám</span><span class="sxs-lookup"><span data-stu-id="b7b07-227">SerialNumber</span></span>            | <span data-ttu-id="b7b07-228">Az eszköz sorozatszámát gyári hozzá van rendelve, és 15 karakter hosszú.</span><span class="sxs-lookup"><span data-stu-id="b7b07-228">The device serial number is assigned at the factory and is 15 characters long.</span></span> <span data-ttu-id="b7b07-229">Például 8600-SHX0991003G44HT jelzi:</span><span class="sxs-lookup"><span data-stu-id="b7b07-229">For instance, 8600-SHX0991003G44HT indicates:</span></span><br> <span data-ttu-id="b7b07-230">8600 – az eszköz típusa van.</span><span class="sxs-lookup"><span data-stu-id="b7b07-230">8600 – Is the device model.</span></span><br><span data-ttu-id="b7b07-231">SHX – a gyártási hely.</span><span class="sxs-lookup"><span data-stu-id="b7b07-231">SHX – Is the manufacturing site.</span></span><br> <span data-ttu-id="b7b07-232">0991003 - termékekkel is.</span><span class="sxs-lookup"><span data-stu-id="b7b07-232">0991003 - Is a specific product.</span></span> <br> <span data-ttu-id="b7b07-233">G44HT – az utolsó 5 számjegy eggyel növekszik, egyedi sorozatszámokat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b7b07-233">G44HT- the last 5 digits are incremented to create unique serial numbers.</span></span> <span data-ttu-id="b7b07-234">Ez nem lehet egy soros készlet.</span><span class="sxs-lookup"><span data-stu-id="b7b07-234">This may not be a sequential set.</span></span>|
| <span data-ttu-id="b7b07-235">Időzóna</span><span class="sxs-lookup"><span data-stu-id="b7b07-235">TimeZone</span></span>                | <span data-ttu-id="b7b07-236">Az eszköz időzóna eszköz üzembe helyezése során az Azure portálon konfigurált módon.</span><span class="sxs-lookup"><span data-stu-id="b7b07-236">The device time zone as configured in the Azure portal during device deployment.</span></span>|
| <span data-ttu-id="b7b07-237">CurrentController</span><span class="sxs-lookup"><span data-stu-id="b7b07-237">CurrentController</span></span>       | <span data-ttu-id="b7b07-238">A tartományvezérlő a StorSimple eszközt a Windows PowerShell felületén keresztül csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="b7b07-238">The controller that you are connected to through the Windows PowerShell interface of your StorSimple device.</span></span>|
| <span data-ttu-id="b7b07-239">ActiveController</span><span class="sxs-lookup"><span data-stu-id="b7b07-239">ActiveController</span></span>        | <span data-ttu-id="b7b07-240">A vezérlő aktív, az eszközön, és a hálózati és lemez műveletek vezérli.</span><span class="sxs-lookup"><span data-stu-id="b7b07-240">The controller that is active on your device and is controlling all the network and disk operations.</span></span> <span data-ttu-id="b7b07-241">Ez lehet vezérlő 0 vagy 1 vezérlő.</span><span class="sxs-lookup"><span data-stu-id="b7b07-241">This can be Controller 0 or Controller 1.</span></span>  |
| <span data-ttu-id="b7b07-242">Controller0Status</span><span class="sxs-lookup"><span data-stu-id="b7b07-242">Controller0Status</span></span>       | <span data-ttu-id="b7b07-243">A vezérlő 0, az eszköz állapota.</span><span class="sxs-lookup"><span data-stu-id="b7b07-243">The status of Controller 0 on your device.</span></span> <span data-ttu-id="b7b07-244">Lehet, hogy a vezérlő állapotát normál, helyreállítási módban, vagy nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="b7b07-244">The controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="b7b07-245">Controller1Status</span><span class="sxs-lookup"><span data-stu-id="b7b07-245">Controller1Status</span></span>       | <span data-ttu-id="b7b07-246">A vezérlő 1 állapotát az eszköz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-246">The status of Controller 1 on your device.</span></span>  <span data-ttu-id="b7b07-247">Lehet, hogy a vezérlő állapotát normál, helyreállítási módban, vagy nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="b7b07-247">The controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="b7b07-248">SystemMode</span><span class="sxs-lookup"><span data-stu-id="b7b07-248">SystemMode</span></span>              | <span data-ttu-id="b7b07-249">A StorSimple eszköz általános állapota.</span><span class="sxs-lookup"><span data-stu-id="b7b07-249">The overall status of your StorSimple device.</span></span> <span data-ttu-id="b7b07-250">Lehet, hogy az eszköz állapotát normál, karbantartási, vagy leszerelt (felel meg az Azure portálon inaktiválása).</span><span class="sxs-lookup"><span data-stu-id="b7b07-250">The device status can be normal, maintenance, or decommissioned (corresponds to deactivated in the Azure portal).</span></span>|
| <span data-ttu-id="b7b07-251">FriendlySoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="b7b07-251">FriendlySoftwareVersion</span></span> | <span data-ttu-id="b7b07-252">A rövid karakterlánc, amely megfelel az eszköz szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="b7b07-252">The friendly string that corresponds to the device software version.</span></span> <span data-ttu-id="b7b07-253">Az operációs rendszert futtató Update 4 a rövid szoftververzió lenne a StorSimple 8000 Series Update 4.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="b7b07-253">For a system running Update 4, the friendly software version would be StorSimple 8000 Series Update 4.0.</span></span>|
| <span data-ttu-id="b7b07-254">HcsSoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="b7b07-254">HcsSoftwareVersion</span></span>      | <span data-ttu-id="b7b07-255">Az eszközön futó HCS szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="b7b07-255">The HCS software version running on your device.</span></span> <span data-ttu-id="b7b07-256">A StorSimple 8000 Series Update 4.0 megfelelő HCS szoftververzió például 6.3.9600.17820.</span><span class="sxs-lookup"><span data-stu-id="b7b07-256">For instance, the HCS software version corresponding to StorSimple 8000 Series Update 4.0 is 6.3.9600.17820.</span></span> |
| <span data-ttu-id="b7b07-257">ApiVersion</span><span class="sxs-lookup"><span data-stu-id="b7b07-257">ApiVersion</span></span>              | <span data-ttu-id="b7b07-258">A Windows PowerShell API a HCS eszköz szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="b7b07-258">The software version of the Windows PowerShell API of the HCS device.</span></span>|
| <span data-ttu-id="b7b07-259">VhdVersion</span><span class="sxs-lookup"><span data-stu-id="b7b07-259">VhdVersion</span></span>              | <span data-ttu-id="b7b07-260">A gyári lemezképről, az eszköz a teljesített szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="b7b07-260">The software version of the factory image that the device was shipped with.</span></span> <span data-ttu-id="b7b07-261">Ha az eszköz visszaállítása a gyári beállítások, majd azt a szoftver verzióját futtatja.</span><span class="sxs-lookup"><span data-stu-id="b7b07-261">If you reset your device to factory defaults, then it runs this software version.</span></span>|
| <span data-ttu-id="b7b07-262">OSVersion</span><span class="sxs-lookup"><span data-stu-id="b7b07-262">OSVersion</span></span>               | <span data-ttu-id="b7b07-263">A szoftver az eszközön futó Windows Server operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="b7b07-263">The software version of the Windows Server operating system running on the device.</span></span> <span data-ttu-id="b7b07-264">A StorSimple eszközt a Windows Server 2012 R2 6.3.9600 megfelelő épül.</span><span class="sxs-lookup"><span data-stu-id="b7b07-264">The StorSimple device is based off the Windows Server 2012 R2 that corresponds to 6.3.9600.</span></span>|
| <span data-ttu-id="b7b07-265">CisAgentVersion</span><span class="sxs-lookup"><span data-stu-id="b7b07-265">CisAgentVersion</span></span>         | <span data-ttu-id="b7b07-266">A StorSimple eszköz futó a Cis ügynök verzióját.</span><span class="sxs-lookup"><span data-stu-id="b7b07-266">The version for your Cis agent running on your StorSimple device.</span></span> <span data-ttu-id="b7b07-267">Ez az ügynök segít kommunikálni az Azure-beli StorSimple Manager szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="b7b07-267">This agent helps communicate with the StorSimple Manager service running in Azure.</span></span>|
| <span data-ttu-id="b7b07-268">MdsAgentVersion</span><span class="sxs-lookup"><span data-stu-id="b7b07-268">MdsAgentVersion</span></span>         | <span data-ttu-id="b7b07-269">A megfelelő az Mds-ügynököt a StorSimple eszközön futó verziója.</span><span class="sxs-lookup"><span data-stu-id="b7b07-269">The version corresponding to the Mds agent running on your StorSimple device.</span></span> <span data-ttu-id="b7b07-270">Ez az ügynök mozgatja az adatokat a megfigyelési és diagnosztikai szolgáltatás (MDS).</span><span class="sxs-lookup"><span data-stu-id="b7b07-270">This agent moves data to the Monitoring and Diagnostics Service (MDS).</span></span>|
| <span data-ttu-id="b7b07-271">Lsisas2Version</span><span class="sxs-lookup"><span data-stu-id="b7b07-271">Lsisas2Version</span></span>          | <span data-ttu-id="b7b07-272">A verzió a LSI illesztőprogramokat, a StorSimple eszköz megfelelő.</span><span class="sxs-lookup"><span data-stu-id="b7b07-272">The version corresponding to the LSI drivers on your StorSimple device.</span></span>|
| <span data-ttu-id="b7b07-273">Kapacitás</span><span class="sxs-lookup"><span data-stu-id="b7b07-273">Capacity</span></span>                | <span data-ttu-id="b7b07-274">Az eszköz bájtban teljes kapacitását.</span><span class="sxs-lookup"><span data-stu-id="b7b07-274">The total capacity of the device in bytes.</span></span>|
| <span data-ttu-id="b7b07-275">RemoteManagementMode</span><span class="sxs-lookup"><span data-stu-id="b7b07-275">RemoteManagementMode</span></span>    | <span data-ttu-id="b7b07-276">Azt jelzi, hogy az eszköz távolról felügyelhetik a Windows PowerShell felületén keresztül.</span><span class="sxs-lookup"><span data-stu-id="b7b07-276">Indicates whether the device can be remotely managed via its Windows PowerShell interface.</span></span> |
| <span data-ttu-id="b7b07-277">FipsMode</span><span class="sxs-lookup"><span data-stu-id="b7b07-277">FipsMode</span></span>                | <span data-ttu-id="b7b07-278">Azt jelzi, hogy az Amerikai Egyesült Államokban Federal Information Processing Standard (FIPS) mód engedélyezve van-e az eszközön.</span><span class="sxs-lookup"><span data-stu-id="b7b07-278">Indicates whether the United States Federal Information Processing Standard (FIPS) mode is enabled on your device.</span></span> <span data-ttu-id="b7b07-279">A FIPS 140 szabvány határozza meg a bizalmas adatok védelmének amerikai szövetségi kormányzati számítógépes rendszerek által jóváhagyott titkosítási algoritmusokat.</span><span class="sxs-lookup"><span data-stu-id="b7b07-279">The FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for the protection of sensitive data.</span></span> <span data-ttu-id="b7b07-280">4 vagy újabb frissítés rendszerű eszközöket FIPS-módban alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b7b07-280">For devices running Update 4 or later, FIPS mode is enabled by default.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b7b07-281">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7b07-281">Next steps</span></span>

* <span data-ttu-id="b7b07-282">Ismerje meg a [az Invoke-HcsDiagnostics parancsmag szintaxisa](https://technet.microsoft.com/library/mt795371.aspx).</span><span class="sxs-lookup"><span data-stu-id="b7b07-282">Learn the [syntax of the Invoke-HcsDiagnostics cmdlet](https://technet.microsoft.com/library/mt795371.aspx).</span></span>

* <span data-ttu-id="b7b07-283">További tudnivalók a [telepítési problémák elhárításához](storsimple-troubleshoot-deployment.md) a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="b7b07-283">Learn more about how to [troubleshoot deployment issues](storsimple-troubleshoot-deployment.md) on your StorSimple device.</span></span>
