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
# <a name="use-hello-storsimple-diagnostics-tool-tootroubleshoot-8000-series-device-issues"></a>Használja a hello StorSimple diagnosztikai eszköz tootroubleshoot 8000 sorozat eszközökkel kapcsolatos problémákat

## <a name="overview"></a>Áttekintés

StorSimple diagnosztikai eszköz hello diagnosztizálja a problémák kapcsolódó toosystem, a teljesítmény, a hálózati és a StorSimple eszköz hardver összetevő állapotát. hello diagnosztikai eszköz használható a különböző forgatókönyvekben. Ilyen például, alkalmazások és szolgáltatások tervezése, a StorSimple eszköz üzembe helyezése, hello hálózati környezet értékeléséhez és egy operatív eszköz hello teljesítményének meghatározására. Ez a cikk áttekintést hello diagnosztikai eszköz, és ismerteti, hogyan használható hello eszközt a StorSimple eszköz.

hello diagnosztikai eszköz elsődlegesen a StorSimple 8000 series a helyszíni eszközök (8100 és 8600).

## <a name="run-diagnostics-tool"></a>Diagnosztikai eszköz futtatása

Ezt az eszközt a StorSimple eszköz hello Windows PowerShell felületén keresztül is futtatható. Tooaccess hello az eszköz helyi kapcsolat két módja van:

* [Használjon PuTTY tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
* [Hello eszköz hello Windows PowerShell használatával távolról el a StorSimple](storsimple-remote-connect.md).

Ebben a cikkben azt feltételezzük, hogy csatlakozott-e toohello eszköz soros konzolon keresztül PuTTY.

#### <a name="toorun-hello-diagnostics-tool"></a>toorun hello diagnosztikai eszköz

Ha a Windows PowerShell-felületet toohello hello eszköz csatlakozott, hajtsa végre a következő lépéseket toorun hello parancsmag hello.
1. Jelentkezzen be toohello eszköz soros konzoljához hello utasításait követve [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).

2. Írja be a következő parancs hello:

    `Invoke-HcsDiagnostics`

    Ha hello hatókör-paraméter nincs megadva, a hello parancsmag minden hello diagnosztikai tesztek hajtja végre. Ezek a tesztek közé tartozik a rendszer, a hardver összetevő állapotát, a hálózati és a teljesítményt. 
    
    csak egy adott teszt toorun hello hatókör-paramétert adja meg. Például toorun csak hello hálózati vizsgálat típusa

    `Invoke-HcsDiagnostics -Scope Network`

3. Válassza ki és másolja hello kimeneti hello PuTTY ablak egy szövegfájlba további elemzés céljából.

## <a name="scenarios-toouse-hello-diagnostics-tool"></a>Forgatókönyvek toouse hello diagnosztikai eszköz

Hello diagnosztikai eszköz tootroubleshoot hello hálózati, a teljesítmény, a rendszer és a hardver állapotát hello rendszer használja. Az alábbiakban néhány lehetséges forgatókönyv szerint:

* **Az eszköz offline** -a StorSimple 8000 series eszköz offline állapotban. Azonban hello Windows PowerShell felületén, úgy tűnik, hogy mindkét hello tartományvezérlők megfelelően működik.
    * Ezzel az eszközzel használható toothen hello hálózati állapot meghatározásához.
         
         > [!NOTE]
         > Használja az eszköz tooassess teljesítmény és a hálózati beállítások egy eszköz előtt hello regisztrációs (vagy a telepítő varázsló segítségével konfigurálása). Egy érvényes IP-cím hozzá van rendelve a toohello eszköz telepítővarázslóját, és a regisztráció során. Olyan eszköz, amely nincs regisztrálva, a hardver és a rendszer ezt a parancsmagot futtathatja. Használjon a hello hatókör-paraméter, például:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Állandó eszközökkel kapcsolatos problémákat** -eszközökkel kapcsolatos problémákat, amelyek az adatok toopersist tapasztalja. Például regisztrációja sikertelen. Sikerült is lehet tapasztal eszközökkel kapcsolatos problémákat hello eszköz után sikeresen regisztrált és működési egy ideig.
    * Ebben az esetben ezzel az eszközzel előzetes hibaelhárítási, mielőtt bejelentkezik egy szolgáltatási kérelem Microsoft Support. Azt javasoljuk, hogy az eszköz és a rögzítési hello kimeneti az eszköz. A kimeneti tooSupport tooexpedite hibaelhárítási majd meg.
    * Minden összetevő vagy a fürt hardverhibák esetén be kell jelentkezni egy támogatási kérést.

* **Kevés a Teljesítmény eszköz** -a StorSimple eszköz lassú.
    * Ebben az esetben futtassa ezt a parancsmagot a hatókör-paraméter beállítása tooperformance. Hello kimeneti elemzése. Hello felhő írható-olvasható késések beolvasása. Használjon hello jelentett késések maximális elérhető célként, figyelembe a hello belső adatkezelési némi többletterhelést okoz, és hello munkaterhelések hello rendszeren telepíteni. További információ: túl[hello hálózati teszt tootroubleshoot eszköz teljesítményű](#network-test).


## <a name="diagnostics-test-and-sample-outputs"></a>Diagnosztika teszt- és minta kimenet

### <a name="hardware-test"></a>Hardver ellenőrzése

Ez a vizsgálat hello hardverösszetevők, hello legpontosabb Beállításhoz belső vezérlőprogram és hello lemez belső vezérlőprogramját a rendszeren futó hello állapota határozza meg.

* jelentett hello hardverösszetevők ezek az összetevők adott sikertelen hello teszt vagy hello rendszerben.
* hello legpontosabb Beállításhoz belső vezérlőprogram és lemez a belső vezérlőprogram verzióinak hello vezérlő 0, a vezérlő 1 jelzett, és a rendszer a megosztott összetevőit. A teljes listáját, és hardverösszetevők Ugrás:

    * [Elsődleges szolgáltatással összetevők](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [Összetevők EBOD szolgáltatással](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Ha az hello hardveren végzett ellenőrzéshez összetevő, [jelentkezzen be Microsoft Support szolgáltatáskérés](storsimple-contact-microsoft-support.md).

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>Minta kimenet hardveren végzett ellenőrzéshez egy 8100-eszközön fut

Íme egy minta kimenet egy StorSimple 8100 eszközről. A hello 8100-as modell eszközhöz hello EBOD ház nincs jelen. Emiatt hello EBOD vezérlő összetevők nem jelennek meg.

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

### <a name="system-test"></a>Rendszer-teszt

Ez a vizsgálat hello Rendszerinformáció, hello a rendelkezésre álló frissítések, hello fürtinformációkat és hello szolgáltatás adatait az eszköz jelentéseket.

* hello Rendszerinformáció hello modell, sorozatszámát, időzóna, tartományvezérlő állapotát és hello részletes szoftververzió hello rendszeren futó tartalmaz. különböző rendszer paraméterek jelentett hello kimeneti, nyissa meg túl toounderstand hello[Rendszerinformáció értelmezése](#appendix-interpreting-system-information).

* hello frissítés rendelkezésre állási jelenti, hogy elérhetők-e a hello rendszeres és karbantartási mód és a csomaghoz kapcsolódó nevek. Ha `RegularUpdates` és `MaintenanceModeUpdates` vannak `false`, ez azt jelzi, hogy hello frissítések nem érhetők el. Az eszköz naprakész állapotban.
* hello fürtinformációkat összes hello HCS fürtcsoport és hozzájuk megfelelő állapotok hello logikai összetevők információkat tartalmaz. Ha megjelenik egy kapcsolat nélküli fürtcsoport ebben a szakaszban hello jelentés [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md).
* hello szolgáltatás adatait hello nevét és az összes hello HCS állapotok és az eszközön futó CiS szolgáltatásokat tartalmazza. Ez az információ a Microsoft Support hello hasznosak hello eszköz hiba elhárításához.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>Minta kimenet egy 8100-eszközön fut, a rendszer vizsgálat

Íme egy minta kimenet hello rendszer vizsgálat futtatása egy 8100-eszközön.

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

### <a name="network-test"></a>Hálózati tesztek

Hello hello hálózati adapterek, portok, DNS és NTP kapcsolat a kiszolgálóval, SSL tanúsítvány, tárfiók hitelesítő adatainak, kapcsolat toohello Update-kiszolgálókról, és állapotának webes proxy kapcsolatot a StorSimple eszköz ellenőrzi.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Minta kimenet hálózat csak DATA0 engedélyezésekor a rendszer tesztelése

Íme egy minta kimenet hello 8100-as eszköz. Hello kimenetében láthatja, hogy:
* Csak DATA 0 DATA 1 hálózati adapter engedélyezve és konfigurálva.
* 2 – 5 adatok hello portálon nem engedélyezettek.
* hello DNS-kiszolgáló konfigurációja érvényes és hello eszköz keresztül hello DNS-kiszolgáló képes kapcsolódni.
* hello NTP-kiszolgáló kapcsolatát is rendben.
* 80-as és 443-as port nyitva. Azonban 9354-es port blokkolva van. Hello alapján [hálózati rendszerkövetelmények](storsimple-system-requirements.md), portra van szüksége tooopen a hello service bus-kommunikációhoz.
* hello SSL-tanúsítvány érvénytelen.
* hello eszközök csatlakozhatnak toohello tárfiók: _myss8000storageacct_.
* hello kapcsolat tooUpdate kiszolgálók esetén érvényes.
* hello webes proxy nincs beállítva ezen az eszközön.

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a>Minta kimenet hálózati vizsgálat DATA0 és adat1 engedélyezésekor.

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

### <a name="performance-test"></a>Teljesítményteszt

Ez a vizsgálat jelentések hello felhő teljesítmény keresztül hello felhő írható-olvasható késések fordulnak elő az eszközhöz. Ez az eszköz lehet használt tooestablish hello felhő alapteljesítményének, amelyek a StorSimple érhet el. hello eszköz jelentések hello maximális teljesítmény (ajánlott eset az olvasási és írási késések), amely a kapcsolat kaphat.

Hello eszköz hello elérhető Teljesítménycentrikus jelzi, hogy használatával tárolók telepítése során a munkaterhelések hello jelentett hello írható-olvasható késések fordulnak elő.

hello teszt hello blob mérete hello másik kötetre típusok hello eszközön társított szimulálja. Rendszeres rétegzett, és a helyileg rögzített kötetek biztonsági mentések használja a 64 KB-os blob mérete. Rétegzett kötetek archív beállítás be van jelölve a 512 KB blob adatok mérete. Ha az eszköznek rétegzett és helyileg rögzített kötetek konfigurált, csak hello teszt megfelelő too64 KB blob adatméret futtatása.

toouse ez eszközzel, hajtsa végre az alábbi lépésekkel hello:

1.  Először hozzon létre a rétegzett kötetek és a rétegzett kötetek archivált beállítás be van jelölve. Ez a művelet biztosítja, hogy hello eszköz hello teszteket 64 KB-os és 512 KB blob mérete.

2. Miután létrehozott és beállított hello kötetek hello parancsmag futtatása Típus:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Jegyezze fel a hello írható-olvasható késések hello eszköz által jelentett. Ez a vizsgálat előtt a rendszer jelzi, hogy hello eredmények is igénybe vehet néhány percet toorun.

4. Ha hello kapcsolat vannak összes hello a várt tartományon, akkor hello késések hello eszköz által használható maximális elérhető célként hello munkaterhelések telepítésekor. Figyelembe a belső adatkezelési némi többletterhelést okoz.

    Ha hello írható-olvasható késések jelentett hello diagnosztikai eszköz magas:

    1. Tárolási analitika konfigurálása a blob-szolgáltatásokhoz, és elemezze hello kimeneti toounderstand hello késések hello Azure storage-fiók. Részletes információkra van szüksége, lépjen túl[engedélyezheti és konfigurálhatja a tárolási analitika](../storage/common/storage-enable-and-view-metrics.md). Ha ezek is magas és összehasonlítható toohello számok kapott hello StorSimple diagnosztikai eszköz, akkor szüksége toolog egy szolgáltatási kérelmet az Azure storage.

    2. Ha hello tárolási fiók késések alacsony, forduljon a hálózati rendszergazda tooinvestigate bármely késési problémák a hálózaton.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>Minta kimenet teljesítmény vizsgálat futtatása egy 8100-eszközön

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

## <a name="appendix-interpreting-system-information"></a>A függelék: rendszeradatok értelmezése

Ez a táblázat mely hello rendszer-információkat a különböző Windows PowerShell paraméterek leképezése hello:. 

| PowerShell-paraméter    | Leírás  |
|-------------------------|------------------|
| Instance ID (Példányazonosító)             | Minden tartományvezérlő egyedi azonosítóval rendelkezik, vagy a vele társított egy GUID Azonosítót.|
| Név                    | hello hello Azure-portálon keresztül eszköz telepítése során konfigurált hello eszköz rövid neve. hello alapértelmezett valódi neve: hello eszköz sorozatszámát. |
| Modell                   | a StorSimple 8000 series eszköz hello modellje. hello modell 8100 vagy 8600 lehet.|
| Sorozatszám            | hello eszköz sorozatszámát hello gyárban hozzá van rendelve, és 15 karakter hosszú. Például 8600-SHX0991003G44HT jelzi:<br> 8600 – hello eszközmodell van.<br>SHX – van hello gyártási hely.<br> 0991003 - termékekkel is. <br> Utolsó 5 számjegy G44HT hello toocreate egyedi sorozatszámokat növekszik. Ez nem lehet egy soros készlet.|
| Időzóna                | hello eszköz időzóna szerint elavultnak hello Azure-portálon eszköz üzembe helyezése során.|
| CurrentController       | hello vezérlő, hogy-e csatlakoztatott toothrough hello Windows PowerShell felületet a StorSimple eszköz.|
| ActiveController        | hello tartományvezérlőre, amely az eszköz aktív, és minden hello hálózati és lemez műveletet vezérli. Ez lehet vezérlő 0 vagy 1 vezérlő.  |
| Controller0Status       | hello állapota vezérlő 0 az eszközön. lehet, hogy hello vezérlő állapotát normál, helyreállítási módban, vagy nem érhető el.|
| Controller1Status       | hello állapota a vezérlő 1 az eszközön.  lehet, hogy hello vezérlő állapotát normál, helyreállítási módban, vagy nem érhető el.|
| SystemMode              | a StorSimple eszköz az általános állapotának hello. lehet, hogy hello Eszközállapot normál, karbantartási, vagy leszerelt (az Azure-portálon hello toodeactivated felel meg).|
| FriendlySoftwareVersion | hello rövid karakterlánc, amely megfelel a toohello eszköz szoftverének verziójával. Az operációs rendszert futtató Update 4 hello rövid szoftverének verziójával lenne a StorSimple 8000 Series Update 4.0-s verzióját.|
| HcsSoftwareVersion      | hello HCS szoftver verziója fut az eszközön. Például hello HCS szoftver verziója megfelelő tooStorSimple 8000 Series Update 4.0 6.3.9600.17820. |
| ApiVersion              | a Windows PowerShell API hello HCS eszköz hello hello szoftverének verziójával.|
| VhdVersion              | a teljesített hello gyári lemezképről, amely az eszköz hello hello szoftverének verziójával. Ha alaphelyzetbe állítja az eszköz toofactory alapértelmezett beállításokat, majd azt a szoftver verzióját futtatja.|
| OSVersion               | hello hello eszközön futó Windows Server operációs rendszer hello szoftverének verziójával. hello StorSimple eszközt a Windows Server 2012 R2 too6.3.9600 megfelelő hello épül.|
| CisAgentVersion         | a Konfigurációelemek ügynök, a StorSimple eszközön futó hello verziója. Ez az ügynök segít kommunikálni hello StorSimple Manager szolgáltatás fut az Azure-ban.|
| MdsAgentVersion         | hello verziója megfelelő toohello Mds ügynök fut a StorSimple eszköz. Ez az ügynök helyezi át az adatokat toohello megfigyelési és diagnosztikai szolgáltatás (MDS).|
| Lsisas2Version          | hello verziója megfelelő toohello LSI illesztőprogramokat a StorSimple eszköz.|
| Kapacitás                | hello bájtban hello eszközök teljes kapacitását.|
| RemoteManagementMode    | Azt jelzi, hogy hello eszköz távolról felügyelhetik a Windows PowerShell felületén keresztül. |
| FipsMode                | Azt jelzi, hogy hello az Amerikai Egyesült Államok Federal Information Processing Standard (FIPS) mód engedélyezve van-e az eszközön. hello FIPS 140 szabvány határozza meg a bizalmas adatok védelmének hello amerikai szövetségi kormányzati számítógépes rendszerek által jóváhagyott titkosítási algoritmusokat. 4 vagy újabb frissítés rendszerű eszközöket FIPS-módban alapértelmezés szerint engedélyezve van. |

## <a name="next-steps"></a>Következő lépések

* Ismerje meg, hello [hello Invoke-HcsDiagnostics parancsmag szintaxisa](https://technet.microsoft.com/library/mt795371.aspx).

* További tudnivalók túl[telepítési problémák elhárításához](storsimple-troubleshoot-deployment.md) a StorSimple eszköz.
