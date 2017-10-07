---
title: "aaaDiagnostics eszközt a StorSimple 8000 tootroubleshoot eszköz |} Microsoft Docs"
description: "Hello StorSimple eszköz módokat ismerteti és bemutatja hogyan toouse Windows PowerShell a StorSimple toochange hello eszköz mód."
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
ms.openlocfilehash: e8b7fdbc44d2533973b63da841335ba73ba0014b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-diagnostics-tool-tootroubleshoot-8000-series-device-issues"></a><span data-ttu-id="8e07c-103">Használja a hello StorSimple diagnosztikai eszköz tootroubleshoot 8000 sorozat eszközökkel kapcsolatos problémákat</span><span class="sxs-lookup"><span data-stu-id="8e07c-103">Use hello StorSimple Diagnostics Tool tootroubleshoot 8000 series device issues</span></span>

## <a name="overview"></a><span data-ttu-id="8e07c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8e07c-104">Overview</span></span>

<span data-ttu-id="8e07c-105">StorSimple diagnosztikai eszköz hello diagnosztizálja a problémák kapcsolódó toosystem, a teljesítmény, a hálózati és a StorSimple eszköz hardver összetevő állapotát.</span><span class="sxs-lookup"><span data-stu-id="8e07c-105">hello StorSimple Diagnostics tool diagnoses issues related toosystem, performance, network, and hardware component health for a StorSimple device.</span></span> <span data-ttu-id="8e07c-106">hello diagnosztikai eszköz használható a különböző forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="8e07c-106">hello diagnostics tool can be used in various scenarios.</span></span> <span data-ttu-id="8e07c-107">Ilyen például, alkalmazások és szolgáltatások tervezése, a StorSimple eszköz üzembe helyezése, hello hálózati környezet értékeléséhez és egy operatív eszköz hello teljesítményének meghatározására.</span><span class="sxs-lookup"><span data-stu-id="8e07c-107">These scenarios include workload planning, deploying a StorSimple device, assessing hello network environment, and determining hello performance of an operational device.</span></span> <span data-ttu-id="8e07c-108">Ez a cikk áttekintést hello diagnosztikai eszköz, és ismerteti, hogyan használható hello eszközt a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-108">This article provides an overview of hello diagnostics tool and describes how hello tool can be used with a StorSimple device.</span></span>

<span data-ttu-id="8e07c-109">hello diagnosztikai eszköz elsődlegesen a StorSimple 8000 series a helyszíni eszközök (8100 és 8600).</span><span class="sxs-lookup"><span data-stu-id="8e07c-109">hello diagnostics tool is primarily intended for StorSimple 8000 series on-premises devices (8100 and 8600).</span></span>

## <a name="run-diagnostics-tool"></a><span data-ttu-id="8e07c-110">Diagnosztikai eszköz futtatása</span><span class="sxs-lookup"><span data-stu-id="8e07c-110">Run diagnostics tool</span></span>

<span data-ttu-id="8e07c-111">Ezt az eszközt a StorSimple eszköz hello Windows PowerShell felületén keresztül is futtatható.</span><span class="sxs-lookup"><span data-stu-id="8e07c-111">This tool can be run via hello Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="8e07c-112">Tooaccess hello az eszköz helyi kapcsolat két módja van:</span><span class="sxs-lookup"><span data-stu-id="8e07c-112">There are two ways tooaccess hello local interface of your device:</span></span>

* <span data-ttu-id="8e07c-113">[Használjon PuTTY tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="8e07c-113">[Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
* <span data-ttu-id="8e07c-114">[Hello eszköz hello Windows PowerShell használatával távolról el a StorSimple](storsimple-remote-connect.md).</span><span class="sxs-lookup"><span data-stu-id="8e07c-114">[Remotely access hello tool via hello Windows PowerShell for StorSimple](storsimple-remote-connect.md).</span></span>

<span data-ttu-id="8e07c-115">Ebben a cikkben azt feltételezzük, hogy csatlakozott-e toohello eszköz soros konzolon keresztül PuTTY.</span><span class="sxs-lookup"><span data-stu-id="8e07c-115">In this article, we assume that you have connected toohello device serial console via PuTTY.</span></span>

#### <a name="toorun-hello-diagnostics-tool"></a><span data-ttu-id="8e07c-116">toorun hello diagnosztikai eszköz</span><span class="sxs-lookup"><span data-stu-id="8e07c-116">toorun hello diagnostics tool</span></span>

<span data-ttu-id="8e07c-117">Ha a Windows PowerShell-felületet toohello hello eszköz csatlakozott, hajtsa végre a következő lépéseket toorun hello parancsmag hello.</span><span class="sxs-lookup"><span data-stu-id="8e07c-117">Once you have connected toohello Windows PowerShell interface of hello device, perform hello following steps toorun hello cmdlet.</span></span>
1. <span data-ttu-id="8e07c-118">Jelentkezzen be toohello eszköz soros konzoljához hello utasításait követve [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="8e07c-118">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>

2. <span data-ttu-id="8e07c-119">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="8e07c-119">Type hello following command:</span></span>

    `Invoke-HcsDiagnostics`

    <span data-ttu-id="8e07c-120">Ha hello hatókör-paraméter nincs megadva, a hello parancsmag minden hello diagnosztikai tesztek hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="8e07c-120">If hello scope parameter is not specified, hello cmdlet executes all hello diagnostic tests.</span></span> <span data-ttu-id="8e07c-121">Ezek a tesztek közé tartozik a rendszer, a hardver összetevő állapotát, a hálózati és a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="8e07c-121">These tests include system, hardware component health, network, and performance.</span></span> 
    
    <span data-ttu-id="8e07c-122">csak egy adott teszt toorun hello hatókör-paramétert adja meg.</span><span class="sxs-lookup"><span data-stu-id="8e07c-122">toorun only a specific test, specify hello scope parameter.</span></span> <span data-ttu-id="8e07c-123">Például toorun csak hello hálózati vizsgálat típusa</span><span class="sxs-lookup"><span data-stu-id="8e07c-123">For instance, toorun only hello network test, type</span></span>

    `Invoke-HcsDiagnostics -Scope Network`

3. <span data-ttu-id="8e07c-124">Válassza ki és másolja hello kimeneti hello PuTTY ablak egy szövegfájlba további elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="8e07c-124">Select and copy hello output from hello PuTTY window into a text file for further analysis.</span></span>

## <a name="scenarios-toouse-hello-diagnostics-tool"></a><span data-ttu-id="8e07c-125">Forgatókönyvek toouse hello diagnosztikai eszköz</span><span class="sxs-lookup"><span data-stu-id="8e07c-125">Scenarios toouse hello diagnostics tool</span></span>

<span data-ttu-id="8e07c-126">Hello diagnosztikai eszköz tootroubleshoot hello hálózati, a teljesítmény, a rendszer és a hardver állapotát hello rendszer használja.</span><span class="sxs-lookup"><span data-stu-id="8e07c-126">Use hello diagnostics tool tootroubleshoot hello network, performance, system, and hardware health of hello system.</span></span> <span data-ttu-id="8e07c-127">Az alábbiakban néhány lehetséges forgatókönyv szerint:</span><span class="sxs-lookup"><span data-stu-id="8e07c-127">Here are some possible scenarios:</span></span>

* <span data-ttu-id="8e07c-128">**Az eszköz offline** -a StorSimple 8000 series eszköz offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="8e07c-128">**Device offline** - Your StorSimple 8000 series device is offline.</span></span> <span data-ttu-id="8e07c-129">Azonban hello Windows PowerShell felületén, úgy tűnik, hogy mindkét hello tartományvezérlők megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="8e07c-129">However, from hello Windows PowerShell interface, it seems that both hello controllers are up and running.</span></span>
    * <span data-ttu-id="8e07c-130">Ezzel az eszközzel használható toothen hello hálózati állapot meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="8e07c-130">You can use this tool toothen determine hello network state.</span></span>
         
         > [!NOTE]
         > <span data-ttu-id="8e07c-131">Használja az eszköz tooassess teljesítmény és a hálózati beállítások egy eszköz előtt hello regisztrációs (vagy a telepítő varázsló segítségével konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="8e07c-131">Do not use this tool tooassess performance and network settings on a device before hello registration (or configuring via setup wizard).</span></span> <span data-ttu-id="8e07c-132">Egy érvényes IP-cím hozzá van rendelve a toohello eszköz telepítővarázslóját, és a regisztráció során.</span><span class="sxs-lookup"><span data-stu-id="8e07c-132">A valid IP is assigned toohello device during setup wizard and registration.</span></span> <span data-ttu-id="8e07c-133">Olyan eszköz, amely nincs regisztrálva, a hardver és a rendszer ezt a parancsmagot futtathatja.</span><span class="sxs-lookup"><span data-stu-id="8e07c-133">You can run this cmdlet, on a device that is not registered, for hardware health and system.</span></span> <span data-ttu-id="8e07c-134">Használjon a hello hatókör-paraméter, például:</span><span class="sxs-lookup"><span data-stu-id="8e07c-134">Use hello scope parameter, for example:</span></span>
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* <span data-ttu-id="8e07c-135">**Állandó eszközökkel kapcsolatos problémákat** -eszközökkel kapcsolatos problémákat, amelyek az adatok toopersist tapasztalja.</span><span class="sxs-lookup"><span data-stu-id="8e07c-135">**Persistent device issues** - You are experiencing device issues that seem toopersist.</span></span> <span data-ttu-id="8e07c-136">Például regisztrációja sikertelen.</span><span class="sxs-lookup"><span data-stu-id="8e07c-136">For instance, registration is failing.</span></span> <span data-ttu-id="8e07c-137">Sikerült is lehet tapasztal eszközökkel kapcsolatos problémákat hello eszköz után sikeresen regisztrált és működési egy ideig.</span><span class="sxs-lookup"><span data-stu-id="8e07c-137">You could also be experiencing device issues after hello device is successfully registered and operational for a while.</span></span>
    * <span data-ttu-id="8e07c-138">Ebben az esetben ezzel az eszközzel előzetes hibaelhárítási, mielőtt bejelentkezik egy szolgáltatási kérelem Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="8e07c-138">In this case, use this tool for preliminary troubleshooting before you log a service request with Microsoft Support.</span></span> <span data-ttu-id="8e07c-139">Azt javasoljuk, hogy az eszköz és a rögzítési hello kimeneti az eszköz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-139">We recommend that you run this tool and capture hello output of this tool.</span></span> <span data-ttu-id="8e07c-140">A kimeneti tooSupport tooexpedite hibaelhárítási majd meg.</span><span class="sxs-lookup"><span data-stu-id="8e07c-140">You can then provide this output tooSupport tooexpedite troubleshooting.</span></span>
    * <span data-ttu-id="8e07c-141">Minden összetevő vagy a fürt hardverhibák esetén be kell jelentkezni egy támogatási kérést.</span><span class="sxs-lookup"><span data-stu-id="8e07c-141">If there are any hardware component or cluster failures, you should log in a Support request.</span></span>

* <span data-ttu-id="8e07c-142">**Kevés a Teljesítmény eszköz** -a StorSimple eszköz lassú.</span><span class="sxs-lookup"><span data-stu-id="8e07c-142">**Low device performance** - Your StorSimple device is slow.</span></span>
    * <span data-ttu-id="8e07c-143">Ebben az esetben futtassa ezt a parancsmagot a hatókör-paraméter beállítása tooperformance.</span><span class="sxs-lookup"><span data-stu-id="8e07c-143">In this case, run this cmdlet with scope parameter set tooperformance.</span></span> <span data-ttu-id="8e07c-144">Hello kimeneti elemzése.</span><span class="sxs-lookup"><span data-stu-id="8e07c-144">Analyze hello output.</span></span> <span data-ttu-id="8e07c-145">Hello felhő írható-olvasható késések beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8e07c-145">You get hello cloud read-write latencies.</span></span> <span data-ttu-id="8e07c-146">Használjon hello jelentett késések maximális elérhető célként, figyelembe a hello belső adatkezelési némi többletterhelést okoz, és hello munkaterhelések hello rendszeren telepíteni.</span><span class="sxs-lookup"><span data-stu-id="8e07c-146">Use hello reported latencies as maximum achievable target, factor in some overhead for hello internal data processing, and then deploy hello workloads on hello system.</span></span> <span data-ttu-id="8e07c-147">További információ: túl[hello hálózati teszt tootroubleshoot eszköz teljesítményű](#network-test).</span><span class="sxs-lookup"><span data-stu-id="8e07c-147">For more information, go too[Use hello network test tootroubleshoot device performance](#network-test).</span></span>


## <a name="diagnostics-test-and-sample-outputs"></a><span data-ttu-id="8e07c-148">Diagnosztika teszt- és minta kimenet</span><span class="sxs-lookup"><span data-stu-id="8e07c-148">Diagnostics test and sample outputs</span></span>

### <a name="hardware-test"></a><span data-ttu-id="8e07c-149">Hardver ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8e07c-149">Hardware test</span></span>

<span data-ttu-id="8e07c-150">Ez a vizsgálat hello hardverösszetevők, hello legpontosabb Beállításhoz belső vezérlőprogram és hello lemez belső vezérlőprogramját a rendszeren futó hello állapota határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8e07c-150">This test determines hello status of hello hardware components, hello USM firmware, and hello disk firmware running on your system.</span></span>

* <span data-ttu-id="8e07c-151">jelentett hello hardverösszetevők ezek az összetevők adott sikertelen hello teszt vagy hello rendszerben.</span><span class="sxs-lookup"><span data-stu-id="8e07c-151">hello hardware components reported are those components that failed hello test or are not present in hello system.</span></span>
* <span data-ttu-id="8e07c-152">hello legpontosabb Beállításhoz belső vezérlőprogram és lemez a belső vezérlőprogram verzióinak hello vezérlő 0, a vezérlő 1 jelzett, és a rendszer a megosztott összetevőit.</span><span class="sxs-lookup"><span data-stu-id="8e07c-152">hello USM firmware and disk firmware versions are reported for hello Controller 0, Controller 1, and shared components in your system.</span></span> <span data-ttu-id="8e07c-153">A teljes listáját, és hardverösszetevők Ugrás:</span><span class="sxs-lookup"><span data-stu-id="8e07c-153">For a complete list of hardware components, go to:</span></span>

    * [<span data-ttu-id="8e07c-154">Elsődleges szolgáltatással összetevők</span><span class="sxs-lookup"><span data-stu-id="8e07c-154">Components in primary enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [<span data-ttu-id="8e07c-155">Összetevők EBOD szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="8e07c-155">Components in EBOD enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> <span data-ttu-id="8e07c-156">Ha az hello hardveren végzett ellenőrzéshez összetevő, [jelentkezzen be Microsoft Support szolgáltatáskérés](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="8e07c-156">If hello hardware test reports failed components, [log in a service request with Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a><span data-ttu-id="8e07c-157">Minta kimenet hardveren végzett ellenőrzéshez egy 8100-eszközön fut</span><span class="sxs-lookup"><span data-stu-id="8e07c-157">Sample output of hardware test run on an 8100 device</span></span>

<span data-ttu-id="8e07c-158">Íme egy minta kimenet egy StorSimple 8100 eszközről.</span><span class="sxs-lookup"><span data-stu-id="8e07c-158">Here is a sample output from a StorSimple 8100 device.</span></span> <span data-ttu-id="8e07c-159">A hello 8100-as modell eszközhöz hello EBOD ház nincs jelen.</span><span class="sxs-lookup"><span data-stu-id="8e07c-159">In hello 8100 model device, hello EBOD enclosure is not present.</span></span> <span data-ttu-id="8e07c-160">Emiatt hello EBOD vezérlő összetevők nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="8e07c-160">Hence, hello EBOD controller components are not reported.</span></span>

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

### <a name="system-test"></a><span data-ttu-id="8e07c-161">Rendszer-teszt</span><span class="sxs-lookup"><span data-stu-id="8e07c-161">System test</span></span>

<span data-ttu-id="8e07c-162">Ez a vizsgálat hello Rendszerinformáció, hello a rendelkezésre álló frissítések, hello fürtinformációkat és hello szolgáltatás adatait az eszköz jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="8e07c-162">This test reports hello system information, hello updates available, hello cluster information, and hello service information for your device.</span></span>

* <span data-ttu-id="8e07c-163">hello Rendszerinformáció hello modell, sorozatszámát, időzóna, tartományvezérlő állapotát és hello részletes szoftververzió hello rendszeren futó tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-163">hello system information includes hello model, device serial number, time zone, controller status, and hello detailed software version running on hello system.</span></span> <span data-ttu-id="8e07c-164">különböző rendszer paraméterek jelentett hello kimeneti, nyissa meg túl toounderstand hello[Rendszerinformáció értelmezése](#appendix-interpreting-system-information).</span><span class="sxs-lookup"><span data-stu-id="8e07c-164">toounderstand hello various system parameters reported as hello output, go too[Interpreting system information](#appendix-interpreting-system-information).</span></span>

* <span data-ttu-id="8e07c-165">hello frissítés rendelkezésre állási jelenti, hogy elérhetők-e a hello rendszeres és karbantartási mód és a csomaghoz kapcsolódó nevek.</span><span class="sxs-lookup"><span data-stu-id="8e07c-165">hello update availability reports whether hello regular and maintenance modes are available and their associated package names.</span></span> <span data-ttu-id="8e07c-166">Ha `RegularUpdates` és `MaintenanceModeUpdates` vannak `false`, ez azt jelzi, hogy hello frissítések nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="8e07c-166">If `RegularUpdates` and `MaintenanceModeUpdates` are `false`, this indicates that hello updates are not available.</span></span> <span data-ttu-id="8e07c-167">Az eszköz naprakész állapotban.</span><span class="sxs-lookup"><span data-stu-id="8e07c-167">Your device is up-to-date.</span></span>
* <span data-ttu-id="8e07c-168">hello fürtinformációkat összes hello HCS fürtcsoport és hozzájuk megfelelő állapotok hello logikai összetevők információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-168">hello cluster information contains hello information on various logical components of all hello HCS cluster groups and their respective statuses.</span></span> <span data-ttu-id="8e07c-169">Ha megjelenik egy kapcsolat nélküli fürtcsoport ebben a szakaszban hello jelentés [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="8e07c-169">If you see an offline cluster group in this section of hello report, [contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="8e07c-170">hello szolgáltatás adatait hello nevét és az összes hello HCS állapotok és az eszközön futó CiS szolgáltatásokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8e07c-170">hello service information includes hello names and statuses of all hello HCS and CiS services running on your device.</span></span> <span data-ttu-id="8e07c-171">Ez az információ a Microsoft Support hello hasznosak hello eszköz hiba elhárításához.</span><span class="sxs-lookup"><span data-stu-id="8e07c-171">This information is helpful for hello Microsoft Support in troubleshooting hello device issue.</span></span>

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a><span data-ttu-id="8e07c-172">Minta kimenet egy 8100-eszközön fut, a rendszer vizsgálat</span><span class="sxs-lookup"><span data-stu-id="8e07c-172">Sample output of system test run on an 8100 device</span></span>

<span data-ttu-id="8e07c-173">Íme egy minta kimenet hello rendszer vizsgálat futtatása egy 8100-eszközön.</span><span class="sxs-lookup"><span data-stu-id="8e07c-173">Here is a sample output of hello system test run on an 8100 device.</span></span>

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

### <a name="network-test"></a><span data-ttu-id="8e07c-174">Hálózati tesztek</span><span class="sxs-lookup"><span data-stu-id="8e07c-174">Network test</span></span>

<span data-ttu-id="8e07c-175">Hello hello hálózati adapterek, portok, DNS és NTP kapcsolat a kiszolgálóval, SSL tanúsítvány, tárfiók hitelesítő adatainak, kapcsolat toohello Update-kiszolgálókról, és állapotának webes proxy kapcsolatot a StorSimple eszköz ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="8e07c-175">This test validates hello status of hello network interfaces, ports, DNS and NTP server connectivity, SSL certificate, storage account credentials, connectivity toohello Update servers, and web proxy connectivity on your StorSimple device.</span></span>

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a><span data-ttu-id="8e07c-176">Minta kimenet hálózat csak DATA0 engedélyezésekor a rendszer tesztelése</span><span class="sxs-lookup"><span data-stu-id="8e07c-176">Sample output of network test when only DATA0 is enabled</span></span>

<span data-ttu-id="8e07c-177">Íme egy minta kimenet hello 8100-as eszköz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-177">Here is a sample output of hello 8100 device.</span></span> <span data-ttu-id="8e07c-178">Hello kimenetében láthatja, hogy:</span><span class="sxs-lookup"><span data-stu-id="8e07c-178">You can see in hello output that:</span></span>
* <span data-ttu-id="8e07c-179">Csak DATA 0 DATA 1 hálózati adapter engedélyezve és konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="8e07c-179">Only DATA 0 and DATA 1 network interfaces are enabled and configured.</span></span>
* <span data-ttu-id="8e07c-180">2 – 5 adatok hello portálon nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="8e07c-180">DATA 2 - 5 are not enabled in hello portal.</span></span>
* <span data-ttu-id="8e07c-181">hello DNS-kiszolgáló konfigurációja érvényes és hello eszköz keresztül hello DNS-kiszolgáló képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="8e07c-181">hello DNS server configuration is valid and hello device can connect via hello DNS server.</span></span>
* <span data-ttu-id="8e07c-182">hello NTP-kiszolgáló kapcsolatát is rendben.</span><span class="sxs-lookup"><span data-stu-id="8e07c-182">hello NTP server connectivity is also fine.</span></span>
* <span data-ttu-id="8e07c-183">80-as és 443-as port nyitva.</span><span class="sxs-lookup"><span data-stu-id="8e07c-183">Ports 80 and 443 are open.</span></span> <span data-ttu-id="8e07c-184">Azonban 9354-es port blokkolva van.</span><span class="sxs-lookup"><span data-stu-id="8e07c-184">However, port 9354 is blocked.</span></span> <span data-ttu-id="8e07c-185">Hello alapján [hálózati rendszerkövetelmények](storsimple-system-requirements.md), portra van szüksége tooopen a hello service bus-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-185">Based on hello [system network requirements](storsimple-system-requirements.md), you need tooopen this port for hello service bus communication.</span></span>
* <span data-ttu-id="8e07c-186">hello SSL-tanúsítvány érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="8e07c-186">hello SSL certification is valid.</span></span>
* <span data-ttu-id="8e07c-187">hello eszközök csatlakozhatnak toohello tárfiók: _myss8000storageacct_.</span><span class="sxs-lookup"><span data-stu-id="8e07c-187">hello device can connect toohello storage account: _myss8000storageacct_.</span></span>
* <span data-ttu-id="8e07c-188">hello kapcsolat tooUpdate kiszolgálók esetén érvényes.</span><span class="sxs-lookup"><span data-stu-id="8e07c-188">hello connectivity tooUpdate servers is valid.</span></span>
* <span data-ttu-id="8e07c-189">hello webes proxy nincs beállítva ezen az eszközön.</span><span class="sxs-lookup"><span data-stu-id="8e07c-189">hello web proxy is not configured on this device.</span></span>

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a><span data-ttu-id="8e07c-190">Minta kimenet hálózati vizsgálat DATA0 és adat1 engedélyezésekor.</span><span class="sxs-lookup"><span data-stu-id="8e07c-190">Sample output of network test when DATA0 and DATA1 are enabled</span></span>

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

### <a name="performance-test"></a><span data-ttu-id="8e07c-191">Teljesítményteszt</span><span class="sxs-lookup"><span data-stu-id="8e07c-191">Performance test</span></span>

<span data-ttu-id="8e07c-192">Ez a vizsgálat jelentések hello felhő teljesítmény keresztül hello felhő írható-olvasható késések fordulnak elő az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-192">This test reports hello cloud performance via hello cloud read-write latencies for your device.</span></span> <span data-ttu-id="8e07c-193">Ez az eszköz lehet használt tooestablish hello felhő alapteljesítményének, amelyek a StorSimple érhet el.</span><span class="sxs-lookup"><span data-stu-id="8e07c-193">This tool can be used tooestablish a baseline of hello cloud performance that you can achieve with StorSimple.</span></span> <span data-ttu-id="8e07c-194">hello eszköz jelentések hello maximális teljesítmény (ajánlott eset az olvasási és írási késések), amely a kapcsolat kaphat.</span><span class="sxs-lookup"><span data-stu-id="8e07c-194">hello tool reports hello maximum performance (best case scenario for read-write latencies) that you can get for your connection.</span></span>

<span data-ttu-id="8e07c-195">Hello eszköz hello elérhető Teljesítménycentrikus jelzi, hogy használatával tárolók telepítése során a munkaterhelések hello jelentett hello írható-olvasható késések fordulnak elő.</span><span class="sxs-lookup"><span data-stu-id="8e07c-195">As hello tool reports hello maximum achievable performance, we can use hello reported read-write latencies as targets when deploying hello workloads.</span></span>

<span data-ttu-id="8e07c-196">hello teszt hello blob mérete hello másik kötetre típusok hello eszközön társított szimulálja.</span><span class="sxs-lookup"><span data-stu-id="8e07c-196">hello test simulates hello blob sizes associated with hello different volume types on hello device.</span></span> <span data-ttu-id="8e07c-197">Rendszeres rétegzett, és a helyileg rögzített kötetek biztonsági mentések használja a 64 KB-os blob mérete.</span><span class="sxs-lookup"><span data-stu-id="8e07c-197">Regular tiered and backups of locally pinned volumes use a 64 KB blob size.</span></span> <span data-ttu-id="8e07c-198">Rétegzett kötetek archív beállítás be van jelölve a 512 KB blob adatok mérete.</span><span class="sxs-lookup"><span data-stu-id="8e07c-198">Tiered volumes with archive option checked use 512 KB blob data size.</span></span> <span data-ttu-id="8e07c-199">Ha az eszköznek rétegzett és helyileg rögzített kötetek konfigurált, csak hello teszt megfelelő too64 KB blob adatméret futtatása.</span><span class="sxs-lookup"><span data-stu-id="8e07c-199">If your device has tiered and locally pinned volumes configured, only hello test corresponding too64 KB blob data size is run.</span></span>

<span data-ttu-id="8e07c-200">toouse ez eszközzel, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8e07c-200">toouse this tool, perform hello following steps:</span></span>

1.  <span data-ttu-id="8e07c-201">Először hozzon létre a rétegzett kötetek és a rétegzett kötetek archivált beállítás be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="8e07c-201">First, create a mix of tiered volumes and tiered volumes with archived option checked.</span></span> <span data-ttu-id="8e07c-202">Ez a művelet biztosítja, hogy hello eszköz hello teszteket 64 KB-os és 512 KB blob mérete.</span><span class="sxs-lookup"><span data-stu-id="8e07c-202">This action ensures that hello tool runs hello tests for both 64 KB and 512 KB blob sizes.</span></span>

2. <span data-ttu-id="8e07c-203">Miután létrehozott és beállított hello kötetek hello parancsmag futtatása</span><span class="sxs-lookup"><span data-stu-id="8e07c-203">Run hello cmdlet after you have created and configured hello volumes.</span></span> <span data-ttu-id="8e07c-204">Típus:</span><span class="sxs-lookup"><span data-stu-id="8e07c-204">Type:</span></span>

    `Invoke-HcsDiagnostics -Scope Performance`

3. <span data-ttu-id="8e07c-205">Jegyezze fel a hello írható-olvasható késések hello eszköz által jelentett.</span><span class="sxs-lookup"><span data-stu-id="8e07c-205">Make a note of hello read-write latencies reported by hello tool.</span></span> <span data-ttu-id="8e07c-206">Ez a vizsgálat előtt a rendszer jelzi, hogy hello eredmények is igénybe vehet néhány percet toorun.</span><span class="sxs-lookup"><span data-stu-id="8e07c-206">This test can take several minutes toorun before it reports hello results.</span></span>

4. <span data-ttu-id="8e07c-207">Ha hello kapcsolat vannak összes hello a várt tartományon, akkor hello késések hello eszköz által használható maximális elérhető célként hello munkaterhelések telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="8e07c-207">If hello connection latencies are all under hello expected range, then hello latencies reported by hello tool can be used as maximum achievable target when deploying hello workloads.</span></span> <span data-ttu-id="8e07c-208">Figyelembe a belső adatkezelési némi többletterhelést okoz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-208">Factor in some overhead for internal data processing.</span></span>

    <span data-ttu-id="8e07c-209">Ha hello írható-olvasható késések jelentett hello diagnosztikai eszköz magas:</span><span class="sxs-lookup"><span data-stu-id="8e07c-209">If hello read-write latencies reported by hello diagnostics tool are high:</span></span>

    1. <span data-ttu-id="8e07c-210">Tárolási analitika konfigurálása a blob-szolgáltatásokhoz, és elemezze hello kimeneti toounderstand hello késések hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="8e07c-210">Configure Storage Analytics for blob services and analyze hello output toounderstand hello latencies for hello Azure storage account.</span></span> <span data-ttu-id="8e07c-211">Részletes információkra van szüksége, lépjen túl[engedélyezheti és konfigurálhatja a tárolási analitika](../storage/common/storage-enable-and-view-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="8e07c-211">For detailed instructions, go too[enable and configure Storage Analytics](../storage/common/storage-enable-and-view-metrics.md).</span></span> <span data-ttu-id="8e07c-212">Ha ezek is magas és összehasonlítható toohello számok kapott hello StorSimple diagnosztikai eszköz, akkor szüksége toolog egy szolgáltatási kérelmet az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="8e07c-212">If those latencies are also high and comparable toohello numbers you received from hello StorSimple Diagnostics tool, then you need toolog a service request with Azure storage.</span></span>

    2. <span data-ttu-id="8e07c-213">Ha hello tárolási fiók késések alacsony, forduljon a hálózati rendszergazda tooinvestigate bármely késési problémák a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="8e07c-213">If hello storage account latencies are low, contact your network administrator tooinvestigate any latency issues in your network.</span></span>

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a><span data-ttu-id="8e07c-214">Minta kimenet teljesítmény vizsgálat futtatása egy 8100-eszközön</span><span class="sxs-lookup"><span data-stu-id="8e07c-214">Sample output of performance test run on an 8100 device</span></span>

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

## <a name="appendix-interpreting-system-information"></a><span data-ttu-id="8e07c-215">A függelék: rendszeradatok értelmezése</span><span class="sxs-lookup"><span data-stu-id="8e07c-215">Appendix: interpreting system information</span></span>

<span data-ttu-id="8e07c-216">Ez a táblázat mely hello rendszer-információkat a különböző Windows PowerShell paraméterek leképezése hello:.</span><span class="sxs-lookup"><span data-stu-id="8e07c-216">Here is a table describing what hello various Windows PowerShell parameters in hello system information map to.</span></span> 

| <span data-ttu-id="8e07c-217">PowerShell-paraméter</span><span class="sxs-lookup"><span data-stu-id="8e07c-217">PowerShell Parameter</span></span>    | <span data-ttu-id="8e07c-218">Leírás</span><span class="sxs-lookup"><span data-stu-id="8e07c-218">Description</span></span>  |
|-------------------------|------------------|
| <span data-ttu-id="8e07c-219">Instance ID (Példányazonosító)</span><span class="sxs-lookup"><span data-stu-id="8e07c-219">Instance ID</span></span>             | <span data-ttu-id="8e07c-220">Minden tartományvezérlő egyedi azonosítóval rendelkezik, vagy a vele társított egy GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="8e07c-220">Every controller has a unique identifier or a GUID associated with it.</span></span>|
| <span data-ttu-id="8e07c-221">Név</span><span class="sxs-lookup"><span data-stu-id="8e07c-221">Name</span></span>                    | <span data-ttu-id="8e07c-222">hello hello Azure-portálon keresztül eszköz telepítése során konfigurált hello eszköz rövid neve.</span><span class="sxs-lookup"><span data-stu-id="8e07c-222">hello friendly name of hello device as configured through hello Azure portal during device deployment.</span></span> <span data-ttu-id="8e07c-223">hello alapértelmezett valódi neve: hello eszköz sorozatszámát.</span><span class="sxs-lookup"><span data-stu-id="8e07c-223">hello default friendly name is hello device serial number.</span></span> |
| <span data-ttu-id="8e07c-224">Modell</span><span class="sxs-lookup"><span data-stu-id="8e07c-224">Model</span></span>                   | <span data-ttu-id="8e07c-225">a StorSimple 8000 series eszköz hello modellje.</span><span class="sxs-lookup"><span data-stu-id="8e07c-225">hello model of your StorSimple 8000 series device.</span></span> <span data-ttu-id="8e07c-226">hello modell 8100 vagy 8600 lehet.</span><span class="sxs-lookup"><span data-stu-id="8e07c-226">hello model can be 8100 or 8600.</span></span>|
| <span data-ttu-id="8e07c-227">Sorozatszám</span><span class="sxs-lookup"><span data-stu-id="8e07c-227">SerialNumber</span></span>            | <span data-ttu-id="8e07c-228">hello eszköz sorozatszámát hello gyárban hozzá van rendelve, és 15 karakter hosszú.</span><span class="sxs-lookup"><span data-stu-id="8e07c-228">hello device serial number is assigned at hello factory and is 15 characters long.</span></span> <span data-ttu-id="8e07c-229">Például 8600-SHX0991003G44HT jelzi:</span><span class="sxs-lookup"><span data-stu-id="8e07c-229">For instance, 8600-SHX0991003G44HT indicates:</span></span><br> <span data-ttu-id="8e07c-230">8600 – hello eszközmodell van.</span><span class="sxs-lookup"><span data-stu-id="8e07c-230">8600 – Is hello device model.</span></span><br><span data-ttu-id="8e07c-231">SHX – van hello gyártási hely.</span><span class="sxs-lookup"><span data-stu-id="8e07c-231">SHX – Is hello manufacturing site.</span></span><br> <span data-ttu-id="8e07c-232">0991003 - termékekkel is.</span><span class="sxs-lookup"><span data-stu-id="8e07c-232">0991003 - Is a specific product.</span></span> <br> <span data-ttu-id="8e07c-233">Utolsó 5 számjegy G44HT hello toocreate egyedi sorozatszámokat növekszik.</span><span class="sxs-lookup"><span data-stu-id="8e07c-233">G44HT- hello last 5 digits are incremented toocreate unique serial numbers.</span></span> <span data-ttu-id="8e07c-234">Ez nem lehet egy soros készlet.</span><span class="sxs-lookup"><span data-stu-id="8e07c-234">This may not be a sequential set.</span></span>|
| <span data-ttu-id="8e07c-235">Időzóna</span><span class="sxs-lookup"><span data-stu-id="8e07c-235">TimeZone</span></span>                | <span data-ttu-id="8e07c-236">hello eszköz időzóna szerint elavultnak hello Azure-portálon eszköz üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="8e07c-236">hello device time zone as configured in hello Azure portal during device deployment.</span></span>|
| <span data-ttu-id="8e07c-237">CurrentController</span><span class="sxs-lookup"><span data-stu-id="8e07c-237">CurrentController</span></span>       | <span data-ttu-id="8e07c-238">hello vezérlő, hogy-e csatlakoztatott toothrough hello Windows PowerShell felületet a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-238">hello controller that you are connected toothrough hello Windows PowerShell interface of your StorSimple device.</span></span>|
| <span data-ttu-id="8e07c-239">ActiveController</span><span class="sxs-lookup"><span data-stu-id="8e07c-239">ActiveController</span></span>        | <span data-ttu-id="8e07c-240">hello tartományvezérlőre, amely az eszköz aktív, és minden hello hálózati és lemez műveletet vezérli.</span><span class="sxs-lookup"><span data-stu-id="8e07c-240">hello controller that is active on your device and is controlling all hello network and disk operations.</span></span> <span data-ttu-id="8e07c-241">Ez lehet vezérlő 0 vagy 1 vezérlő.</span><span class="sxs-lookup"><span data-stu-id="8e07c-241">This can be Controller 0 or Controller 1.</span></span>  |
| <span data-ttu-id="8e07c-242">Controller0Status</span><span class="sxs-lookup"><span data-stu-id="8e07c-242">Controller0Status</span></span>       | <span data-ttu-id="8e07c-243">hello állapota vezérlő 0 az eszközön.</span><span class="sxs-lookup"><span data-stu-id="8e07c-243">hello status of Controller 0 on your device.</span></span> <span data-ttu-id="8e07c-244">lehet, hogy hello vezérlő állapotát normál, helyreállítási módban, vagy nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="8e07c-244">hello controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="8e07c-245">Controller1Status</span><span class="sxs-lookup"><span data-stu-id="8e07c-245">Controller1Status</span></span>       | <span data-ttu-id="8e07c-246">hello állapota a vezérlő 1 az eszközön.</span><span class="sxs-lookup"><span data-stu-id="8e07c-246">hello status of Controller 1 on your device.</span></span>  <span data-ttu-id="8e07c-247">lehet, hogy hello vezérlő állapotát normál, helyreállítási módban, vagy nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="8e07c-247">hello controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="8e07c-248">SystemMode</span><span class="sxs-lookup"><span data-stu-id="8e07c-248">SystemMode</span></span>              | <span data-ttu-id="8e07c-249">a StorSimple eszköz az általános állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="8e07c-249">hello overall status of your StorSimple device.</span></span> <span data-ttu-id="8e07c-250">lehet, hogy hello Eszközállapot normál, karbantartási, vagy leszerelt (az Azure-portálon hello toodeactivated felel meg).</span><span class="sxs-lookup"><span data-stu-id="8e07c-250">hello device status can be normal, maintenance, or decommissioned (corresponds toodeactivated in hello Azure portal).</span></span>|
| <span data-ttu-id="8e07c-251">FriendlySoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="8e07c-251">FriendlySoftwareVersion</span></span> | <span data-ttu-id="8e07c-252">hello rövid karakterlánc, amely megfelel a toohello eszköz szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="8e07c-252">hello friendly string that corresponds toohello device software version.</span></span> <span data-ttu-id="8e07c-253">Az operációs rendszert futtató Update 4 hello rövid szoftverének verziójával lenne a StorSimple 8000 Series Update 4.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="8e07c-253">For a system running Update 4, hello friendly software version would be StorSimple 8000 Series Update 4.0.</span></span>|
| <span data-ttu-id="8e07c-254">HcsSoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="8e07c-254">HcsSoftwareVersion</span></span>      | <span data-ttu-id="8e07c-255">hello HCS szoftver verziója fut az eszközön.</span><span class="sxs-lookup"><span data-stu-id="8e07c-255">hello HCS software version running on your device.</span></span> <span data-ttu-id="8e07c-256">Például hello HCS szoftver verziója megfelelő tooStorSimple 8000 Series Update 4.0 6.3.9600.17820.</span><span class="sxs-lookup"><span data-stu-id="8e07c-256">For instance, hello HCS software version corresponding tooStorSimple 8000 Series Update 4.0 is 6.3.9600.17820.</span></span> |
| <span data-ttu-id="8e07c-257">ApiVersion</span><span class="sxs-lookup"><span data-stu-id="8e07c-257">ApiVersion</span></span>              | <span data-ttu-id="8e07c-258">a Windows PowerShell API hello HCS eszköz hello hello szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="8e07c-258">hello software version of hello Windows PowerShell API of hello HCS device.</span></span>|
| <span data-ttu-id="8e07c-259">VhdVersion</span><span class="sxs-lookup"><span data-stu-id="8e07c-259">VhdVersion</span></span>              | <span data-ttu-id="8e07c-260">a teljesített hello gyári lemezképről, amely az eszköz hello hello szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="8e07c-260">hello software version of hello factory image that hello device was shipped with.</span></span> <span data-ttu-id="8e07c-261">Ha alaphelyzetbe állítja az eszköz toofactory alapértelmezett beállításokat, majd azt a szoftver verzióját futtatja.</span><span class="sxs-lookup"><span data-stu-id="8e07c-261">If you reset your device toofactory defaults, then it runs this software version.</span></span>|
| <span data-ttu-id="8e07c-262">OSVersion</span><span class="sxs-lookup"><span data-stu-id="8e07c-262">OSVersion</span></span>               | <span data-ttu-id="8e07c-263">hello hello eszközön futó Windows Server operációs rendszer hello szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="8e07c-263">hello software version of hello Windows Server operating system running on hello device.</span></span> <span data-ttu-id="8e07c-264">hello StorSimple eszközt a Windows Server 2012 R2 too6.3.9600 megfelelő hello épül.</span><span class="sxs-lookup"><span data-stu-id="8e07c-264">hello StorSimple device is based off hello Windows Server 2012 R2 that corresponds too6.3.9600.</span></span>|
| <span data-ttu-id="8e07c-265">CisAgentVersion</span><span class="sxs-lookup"><span data-stu-id="8e07c-265">CisAgentVersion</span></span>         | <span data-ttu-id="8e07c-266">a Konfigurációelemek ügynök, a StorSimple eszközön futó hello verziója.</span><span class="sxs-lookup"><span data-stu-id="8e07c-266">hello version for your Cis agent running on your StorSimple device.</span></span> <span data-ttu-id="8e07c-267">Ez az ügynök segít kommunikálni hello StorSimple Manager szolgáltatás fut az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8e07c-267">This agent helps communicate with hello StorSimple Manager service running in Azure.</span></span>|
| <span data-ttu-id="8e07c-268">MdsAgentVersion</span><span class="sxs-lookup"><span data-stu-id="8e07c-268">MdsAgentVersion</span></span>         | <span data-ttu-id="8e07c-269">hello verziója megfelelő toohello Mds ügynök fut a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-269">hello version corresponding toohello Mds agent running on your StorSimple device.</span></span> <span data-ttu-id="8e07c-270">Ez az ügynök helyezi át az adatokat toohello megfigyelési és diagnosztikai szolgáltatás (MDS).</span><span class="sxs-lookup"><span data-stu-id="8e07c-270">This agent moves data toohello Monitoring and Diagnostics Service (MDS).</span></span>|
| <span data-ttu-id="8e07c-271">Lsisas2Version</span><span class="sxs-lookup"><span data-stu-id="8e07c-271">Lsisas2Version</span></span>          | <span data-ttu-id="8e07c-272">hello verziója megfelelő toohello LSI illesztőprogramokat a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-272">hello version corresponding toohello LSI drivers on your StorSimple device.</span></span>|
| <span data-ttu-id="8e07c-273">Kapacitás</span><span class="sxs-lookup"><span data-stu-id="8e07c-273">Capacity</span></span>                | <span data-ttu-id="8e07c-274">hello bájtban hello eszközök teljes kapacitását.</span><span class="sxs-lookup"><span data-stu-id="8e07c-274">hello total capacity of hello device in bytes.</span></span>|
| <span data-ttu-id="8e07c-275">RemoteManagementMode</span><span class="sxs-lookup"><span data-stu-id="8e07c-275">RemoteManagementMode</span></span>    | <span data-ttu-id="8e07c-276">Azt jelzi, hogy hello eszköz távolról felügyelhetik a Windows PowerShell felületén keresztül.</span><span class="sxs-lookup"><span data-stu-id="8e07c-276">Indicates whether hello device can be remotely managed via its Windows PowerShell interface.</span></span> |
| <span data-ttu-id="8e07c-277">FipsMode</span><span class="sxs-lookup"><span data-stu-id="8e07c-277">FipsMode</span></span>                | <span data-ttu-id="8e07c-278">Azt jelzi, hogy hello az Amerikai Egyesült Államok Federal Information Processing Standard (FIPS) mód engedélyezve van-e az eszközön.</span><span class="sxs-lookup"><span data-stu-id="8e07c-278">Indicates whether hello United States Federal Information Processing Standard (FIPS) mode is enabled on your device.</span></span> <span data-ttu-id="8e07c-279">hello FIPS 140 szabvány határozza meg a bizalmas adatok védelmének hello amerikai szövetségi kormányzati számítógépes rendszerek által jóváhagyott titkosítási algoritmusokat.</span><span class="sxs-lookup"><span data-stu-id="8e07c-279">hello FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for hello protection of sensitive data.</span></span> <span data-ttu-id="8e07c-280">4 vagy újabb frissítés rendszerű eszközöket FIPS-módban alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="8e07c-280">For devices running Update 4 or later, FIPS mode is enabled by default.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8e07c-281">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8e07c-281">Next steps</span></span>

* <span data-ttu-id="8e07c-282">Ismerje meg, hello [hello Invoke-HcsDiagnostics parancsmag szintaxisa](https://technet.microsoft.com/library/mt795371.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e07c-282">Learn hello [syntax of hello Invoke-HcsDiagnostics cmdlet](https://technet.microsoft.com/library/mt795371.aspx).</span></span>

* <span data-ttu-id="8e07c-283">További tudnivalók túl[telepítési problémák elhárításához](storsimple-troubleshoot-deployment.md) a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="8e07c-283">Learn more about how too[troubleshoot deployment issues](storsimple-troubleshoot-deployment.md) on your StorSimple device.</span></span>
