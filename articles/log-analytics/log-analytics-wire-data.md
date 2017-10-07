---
title: "Naplóelemzési adatok megoldás aaaWire |} Microsoft Docs"
description: "Átviteli adatokat az OMS-ügynököt, beleértve az Operations Manager és a Windows-csatlakoztatott ügynökök rendelkező számítógépekről összevont hálózati és a teljesítmény adatai. Hálózati adatok egyesítése a naplózási adatok toohelp az adatok összefüggéseket."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: banders
ms.openlocfilehash: adafdf98dfbda9d87759643a1a606a84eafd1348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a>A Naplóelemzési átviteli adatok 2.0 (előzetes verzió) megoldás

![Átviteli adatokat szimbólum](./media/log-analytics-wire-data/wire-data2-symbol.png)

Átviteli adatokat az OMS-ügynököt, beleértve az Operations Manager, a Windows-csatlakozóval csatlakoztatott, és a Linux-ügynökök rendelkező számítógépekről összevont hálózati és a teljesítmény adatai. Hálózati adatok egyesítése a többi napló adatok toohelp az adatok összefüggéseket.

Továbbá tooOMS ügynökök hello átviteli adatokat megoldást használ Microsoft Dependency ügynökök, a számítástechnikai infrastruktúra számítógépeken telepíteni. Függőségi ügynökök figyelő hálózati adatforgalom tooand a számítógépek hálózati szintek a 2-3 hello [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), beleértve a hello különböző protokollok és portok. Adatokat küldi el a rendszer tooLog Analytics ügynököt használ.

> [!NOTE]
> Nem adható hozzá hello hello átviteli adatokat megoldás toonew munkaterületek korábbi verzióját. Ha hello eredeti átviteli adatokat megoldás, toouse folytathatja azt. Azonban, hálózaton adatok 2.0 toouse, el kell távolítania hello eredeti verzióját.

Alapértelmezés szerint a Naplóelemzési naplózott adatokat gyűjti a Processzor, memória, lemez és hálózat teljesítményadatokat számlálók a Windows beépített. Hálózati és egyéb adatok gyűjtése történik valós idejű minden ügynökhöz, beleértve az alhálózatok és hello számítógép által használt alkalmazásszintű protokollok. Más teljesítményszámlálók a hello beállítások lapon a hello naplófájlok lapján is hozzáadhat.

Ha már használta [sFlow](http://www.sflow.org/) vagy más szoftvereket [Cisco tartozó NetFlow protokoll](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), majd hello statisztika, és adatokat átviteli adatokat a megszokott tooyou.

Beépített napló keresési lekérdezések hello típusai a következők:

- Átviteli adatokat szolgáltató ügynökök
- Átviteli adatokat szolgáltató ügynökök IP-címe
- Kimenő kommunikáció IP-cím
- Alkalmazás-protokollokra által küldött bájtok száma
- Egy alkalmazás szolgáltatás által küldött bájtok száma
- A különböző protokollok által fogadott bájtok
- IP-verziója által küldött és fogadott bájtok teljes száma
- Volt megbízhatóan mérhető kapcsolatok átlagos késése
- A számítógép folyamatainak kezdeményezett, illetve a hálózati forgalom érkezett
- A folyamatot a hálózati forgalom mennyisége

Átviteli adatokat használó keresésénél szűrheti is, és csoport adatok tooview információ hello felső ügynökök és a felső protokollok. Szeretné megtekinteni, ha egyes számítógépek (IP-címet vagy MAC-címek) közölt egymással, hogyan hosszú, és mennyi adatot küldött – alapvetően, megtekintheti a hálózati forgalmat, amely keresési-alapú metaadatait.

Azonban mivel megtekintett metaadatok, célszerű nem feltétlenül részletes hibaelhárítási. Átvitel közbeni Naplóelemzési adatai nem a teljes rögzítési hálózati adatok. Tehát nem készült részletes csomagszintű hibaelhárítás. hello hello ügynök használatának előnye, tooother adatgyűjtési módszerek képest, hogy nem tooinstall készülékek, konfigurálja újra a hálózati kapcsolókat, vagy összetett konfigurációk végrehajtásához. Átviteli adatokat egyszerűen ügynök-alapú – hello ügynököt telepít egy számítógépre, és azt a saját hálózati forgalom figyeli. Egy másik előnye, ha azt szeretné, hogy a szolgáltatók vagy az üzemeltetési szolgáltató vagy a Microsoft Azure, ahol a hello felhasználói saját hello háló réteg nem toomonitor munkaterhelésekre.

## <a name="connected-sources"></a>Összekapcsolt források

Átviteli adatokat veszi hello Microsoft függőségi ügynök az adatokat. hello függőségi ügynök hello OMS-ügynököt a kapcsolatok tooLog Analytics a függ. Ez azt jelenti, hogy egy kiszolgáló hello OMS-ügynököt telepítette, és konfigurálta az első kell rendelkeznie, és hello függőségi ügynök telepítését követően. hello következő táblázat ismerteti, amely támogatja a hello átviteli adatokat megoldás hello csatlakoztatott adatforrások.

| **Csatlakoztatott adatforrás** | **Támogatott** | **Leírás** |
| --- | --- | --- |
| Windows-ügynökök | Igen | Átviteli adatokat elemzi, és a Windows-ügynök számítógépekről gyűjt adatokat. <br><br> A hozzáadása toohello [OMS-ügynököt](log-analytics-windows-agents.md), Windows-ügynökök hello Microsoft függőségi ügynök szükséges. Lásd: hello [támogatott operációs rendszerek](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) operációs rendszerek teljes listáját. |
| Linux-ügynökök | Igen | Átviteli adatokat elemzi, és a Linux-ügynök számítógépekről gyűjt adatokat.<br><br> A hozzáadása toohello [OMS-ügynököt](log-analytics-linux-agents.md), Linux-ügynökök hello Microsoft függőségi ügynök szükséges. Lásd: hello [támogatott operációs rendszerek](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) operációs rendszerek teljes listáját. |
| System Center Operations Manage felügyeleti csoport | Igen | Átviteli adatokat elemzi, és összegyűjti az adatokat a Windows és Linux-ügynökök a csatlakoztatott [System Center Operations Manager felügyeleti csoport](log-analytics-om-agents.md). <br><br> Közvetlen kapcsolat az hello System Center Operations Manager ügynök számítógép tooLog Analytics megadása kötelező. A hello felügyeleti csoport tooLog Analytics adat továbbítódik. |
| Azure Storage-fiók | Nem | Átviteli adatokat adatokat gyűjt ügynök számítógépekről, így nincsenek adatok, toocollect Azure Storage-ból. |

Windows, a Microsoft Monitoring Agent (MMA) hello System Center Operations Manager és a Naplóelemzési toogather használja, és adatokat küldeni. Attól függően, hogy hello környezetben hello ügynök hello System Center Operations Manager ügynök, OMS-ügynököt, Log Analytics Agent, MMA vagy közvetlen ügynök neve. A System Center Operations Manager és a Naplóelemzési biztosít hello MMA némileg különböző verziói. Ezen verziói egyes jelenthetik tooSystem Center Operations Manager, tooLog Analytics vagy tooboth.

Linux hello Linux OMS-ügynököt gyűjt, és elküldi az adatokat tooLog elemzés. Átviteli adatokat közvetlen OMS-ügynököt a kiszolgálón vagy kiszolgálókon, amelyek a System Center Operations Manager felügyeleti csoportok keresztül csatlakoztatott tooLog Analytics használható.

Ez a cikk hivatkozik tooall ügynökök, hogy Linux vagy a Windows, hogy csatlakoztatott tooa System Center Operations Manager felügyeleti csoportot, vagy közvetlenül tooLog Analytics hello hívják _OMS-ügynököt_. Hello ügynök hello adott központi telepítés neve csak akkor, ha szükség van a környezetben fogjuk használni.

hello függőségi ügynök nem továbbítja az adatokat saját magát, és nem igényel módosításokat toofirewalls vagy portok. hello az átviteli adatokat mindig továbbított adatok által hello OMS ügynök tooLog elemzés, vagy közvetlenül, vagy az OMS átjáró használatával hello.

![ügynök diagramja](./media/log-analytics-wire-data/agents.png)

Ha egy felügyeleti csoporthoz csatlakoztatott tooLog Analytics rendelkező System Center Operations Manager felhasználó:

- Hello Internet tooconnect tooLog Analytics férhetnek hozzá a System Center Operations Manager-ügynökök nincs szükség további konfigurációra.
- Ha a System Center Operations Manager-ügynökök nem tud hozzáférni a Naplóelemzési hello interneten keresztül kell tooconfigure hello OMS átjáró toowork a System Center Operations Manager.

Ha közvetlen ügynök hello használ, kell tooconfigure hello OMS-ügynököt maga tooconnect tooLog Analytics vagy tooyour OMS-átjáró. Hello OMS átjáró letölthető hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

## <a name="prerequisites"></a>Előfeltételek

- Hello szükséges [betekintést és az elemzések](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) megoldás ajánlat.
- Átviteli adatokat megoldás hello korábbi verziójának hello használata, távolítsa el azt. Azonban az összes rögzített hello eredeti átviteli adatokat megoldáson keresztül érhetők el az adatok továbbra is átviteli adatok 2.0 és a keresési napló.
- Rendszergazdai jogosultságokkal szükséges tooinstall, vagy távolítsa el a függőségi ügynök hello.
- hello függőségi ügynök telepítenie kell a 64 bites operációs rendszerrel rendelkező számítógépen.

### <a name="operating-systems"></a>Operációs rendszerek

hello alábbi szakaszok tartalmazzák hello támogatott operációs rendszerek hello függőségi ügynök. Átviteli adatokat nem támogatja 32 bites operációs rendszert.

#### <a name="windows-server"></a>Windows Server

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Asztali Windows

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, a CentOS Linux és az Oracle Linux (az RHEL Kernel)

- Csak az alapértelmezett és az SMP Linux kernel verziókban támogatott.
- Nem szabványos kernel kiadását, például a Xen, és a fizikai nem támogatottak a Linux-disztribúció. Például egy hello kiadás karakterláncot a rendszer _2.6.16.21-0.8-xen_ nem támogatott.
- Egyéni mag, beleértve a szabványos kernelek újrafordításainak maximális nem támogatottak.
- CentOSPlus kernel nem támogatott.
- Ez a cikk későbbi részében Oracle szoros vállalati Kernel (UEK) vonatkozik.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| **Operációs rendszer verziója** | **Kernel-verzió** |
| --- | --- |
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| **Operációs rendszer verziója** | **Kernel-verzió** |
| --- | --- |
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5

| **Operációs rendszer verziója** | **Kernel-verzió** |
| --- | --- |
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398 <br> 2.6.18-400 <br>2.6.18-402 <br>2.6.18-404 <br>2.6.18-406 <br> 2.6.18-407 <br> 2.6.18-408 <br> 2.6.18-409 <br> 2.6.18-410 <br> 2.6.18-411 <br> 2.6.18-412 <br> 2.6.18-416 <br> 2.6.18-417 <br> 2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Szoros vállalati Kernel az Oracle Enterprise Linux

#### <a name="oracle-linux-6"></a>Oracle Linux 6

| **Operációs rendszer verziója** | **Kernel-verzió** |
| --- | --- |
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| **Operációs rendszer verziója** | **Kernel-verzió** |
| --- | --- |
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11

| **Operációs rendszer verziója** | **Kernel-verzió** |
| --- | --- |
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10

| **Operációs rendszer verziója** | **Kernel-verzió** |
| --- | --- |
| 10 SP4 | 2.6.16.60 |

#### <a name="dependency-agent-downloads"></a>A függőségi ügynök letöltése

| **Fájl** | **AZ OPERÁCIÓS RENDSZER** | **Verzió** | **AZ SHA-256-RA** |
| --- | --- | --- | --- |
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |



## <a name="configuration"></a>Konfiguráció

Hajtsa végre a következő lépéseket tooconfigure hello átviteli adatokat megoldás a munkaterületek hello.

1. Hello tevékenység Naplóelemzési megoldást hello engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
2. Hello függőségi ügynök telepítése minden olyan számítógépen, ahová tooget adatokat. hello függőségi ügynök kapcsolatok tooimmediate szomszédok, figyelhet, így előfordulhat, hogy nem kell minden olyan számítógépen az ügynök.

### <a name="install-hello-dependency-agent-on-windows"></a>A Windows hello függőségi ügynök telepítése

Rendszergazdai jogosultság szükséges tooinstall vagy hello ügynök eltávolítása.

hello függőségi ügynök InstallDependencyAgent-Windows.exe Windows rendszert futtató számítógépekre telepíthető. Ha a végrehajtható fájl kapcsolók nélkül futtatja, az elindít egy varázslót, hogy követésével tooinstall interaktív módon.

Hello lépéseket tooinstall hello függőségi ügynök minden Windows rendszerű számítógépen a következő használja:

1. Telepítés hello OMS-ügynököt a hello található utasítások segítségével: [csatlakozás Windows számítógépek toohello az Azure Naplóelemzés szolgáltatás](log-analytics-windows-agents.md).
2. Az előző szakaszban hello hello hivatkozással hello Windows-ügynök letöltése, és futtassa a következő parancs hello segítségével: InstallDependencyAgent-Windows.exe
3. Hajtsa végre a hello varázsló tooinstall hello ügynök.
4. Ha hello függőségi ügynök toostart sikertelen, tekintse meg a hibával kapcsolatos részletes információk hello naplókat. A Windows-ügynökök hello naplókönyvtár %Programfiles%\Microsoft függőségi Agent\logs.

#### <a name="windows-command-line"></a>A Windows parancssor

A hello tábla tooinstall a parancssorból a következő beállításokkal. hello telepítési jelző, hello telepítő futtatásához hello segítségével listája toosee /? Ez a jelző az alábbiak szerint.

InstallDependencyAgent-Windows.exe /?

| **Jelzője** | **Leírás** |
| --- | --- |
| <code>/?</code> | Hello parancssori kapcsolók listájának lekérése. |
| <code>/S</code> | Felhasználói beavatkozás nélküli telepítés végrehajtásához. |

Hello Windows függőségi ügynök fájljait alapértelmezésben a C:\Program Files\Microsoft függőségi ügynök kerül.

### <a name="install-hello-dependency-agent-on-linux"></a>Hello függőségi ügynök telepítése Linux rendszerre

Legfelső szintű hozzáférés szükséges tooinstall vagy hello ügynök konfigurálása.

Linux rendszerű számítógépek InstallDependencyAgent Linux64.bin, egy önkicsomagoló bináris héjparancsfájlt keresztül hello függőségi ügynök van telepítve. Hello fájl segítségével is futtathatja _megosztása_ , vagy adja hozzá engedélyek toohello fájl saját magát.

Hello lépéseket tooinstall hello függőségi ügynök a Linux rendszerű számítógépekre a következő használja:

1. Telepítés hello OMS-ügynököt a hello található utasítások segítségével: [adatok gyűjtésére és kezelésére a Linux rendszerű számítógépek](log-analytics-agent-linux.md).
2. Hello Linux függőségi ügynök az előző szakaszban hello hello hivatkozással töltse le és telepítse legfelső szintű hello a következő parancs használatával: sh InstallDependencyAgent-Linux64.bin
3. Ha hello függőségi ügynök toostart sikertelen, tekintse meg a hibával kapcsolatos részletes információk hello naplókat. A Linux-ügynökök hello naplókönyvtár van: /var/opt/microsoft/dependency-agent/log.

hello telepítési jelző, hello programjának futtatása a hello listája toosee `-help` jelzőt az alábbiak szerint.

```
InstallDependencyAgent-Linux64.bin -help
```

| **Jelzője** | **Leírás** |
| --- | --- |
| <code>-help</code> | Hello parancssori kapcsolók listájának lekérése. |
| <code>-s</code> | Felhasználói beavatkozás nélküli telepítés végrehajtásához. |
| <code>--check</code> | Ellenőrizze az engedélyeit és hello operációs rendszer, de ne telepítse hello ügynök. |

Hello függőségi ügynök fájlok kerülnek, a következő könyvtárak hello:

| **Fájlok** | **Hely** |
| --- | --- |
| Alapvető fájljait | /OPT/Microsoft/Dependency-Agent |
| Naplófájlok | /var/OPT/Microsoft/Dependency-Agent/log |
| Olyan konfigurációs fájlt | /ETC/OPT/Microsoft/Dependency-Agent/config |
| Végrehajtható fájlok | /OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent<br><br>/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent-Manager |
| A tároló bináris fájljai | /var/OPT/Microsoft/Dependency-Agent/Storage |

### <a name="installation-script-examples"></a>Telepítési parancsfájl példák

tooeasily telepítése több kiszolgálón függőségi ügynök hello egyszerre, hanem toouse parancsfájlt. A következő parancsfájl példák toodownload hello használja, és hello függőségi ügynök telepíthető Windows vagy Linux.

#### <a name="powershell-script-for-windows"></a>A Windows PowerShell-parancsfájl

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>A Linux rendszerhez parancsfájl

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>Célállapot-konfiguráló

toodeploy hello függőségi ügynök keresztül célállapot-konfiguráció, hello xPSDesiredStateConfiguration modul és a kód hasonló hello használhatja:

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install hello Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-hello-dependency-agent"></a>Hello függőségi ügynök eltávolítása

A következő szakaszok toohelp hello függőségi ügynök eltávolítása hello használata.

#### <a name="uninstall-hello-dependency-agent-on-windows"></a>Távolítsa el a függőségi ügynök a Windows hello

A rendszergazda eltávolíthat hello függőségi ügynök a Windows Vezérlőpult segítségével.

A rendszergazda %Programfiles%\Microsoft függőségi Agent\Uninstall.exe toouninstall hello függőségi ügynök is futtathatja.

#### <a name="uninstall-hello-dependency-agent-on-linux"></a>Hello Linux függőségi ügynök eltávolítása

toocompletely eltávolítás hello függőségi Linux-ügynököt, el kell távolítania a hello ügynök magát, és hello összekötőt, amely automatikusan telepítve van a hello ügynökkel. Eltávolíthatja, mindkettő hello egyetlen parancs a következő használatával:

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>Felügyeleti csomagok

A Naplóelemzési munkaterület átviteli adatokat aktiválásakor 300 KB-os felügyeleti csomag munkaterületről tooall hello Windows kiszolgálók küldése. Ha a System Center Operations Manager ügynököt használ egy [csatlakoztatott felügyeleti csoport](log-analytics-om-agents.md), hello alkalmazásfüggőség-figyelő a felügyeleti csomag a System Center Operations Manager telepítésének módja. Ha hello ügynökök közvetlenül csatlakoztatott, Naplóelemzési nyújt hello felügyeleti csomag.

Microsoft.IntelligencePacks.ApplicationDependencyMonitor hello felügyeleti csomag neve. Az írás: %Programfiles%\Microsoft figyelési Agent\Agent\Health State\Management szervizcsomagok. hello felügyeleti csomagot használó hello adatforrás: % Program files%\Microsoft figyelés Agent\Agent\Health szolgáltatás State\Resources&lt;AutoGeneratedID&gt;\ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="using-hello-solution"></a>Hello megoldással

**Telepítése és konfigurálása hello megoldás**

A következő információk tooinstall hello használja, és hello megoldás konfigurálása.

- Átviteli adatokat megoldás hello szerez be a Windows Server 2012 R2, Windows 8.1 és újabb operációs rendszereket futtató számítógépek adatait.
- A számítógépeken, ahol azt szeretné, hogy tooacquire átviteli adatokat a Microsoft .NET-keretrendszer 4.0-s vagy újabb szükséges.
- Adja hozzá a hello átviteli adatokat megoldás tooyour Naplóelemzési munkaterület ismertetett hello eljárással [hozzáadni a Naplóelemzési megoldások hello megoldások gyűjteménye a](log-analytics-add-solutions.md). Nincs szükség további konfigurációra.
- Ha egy bizonyos megoldást szeretne tooview átviteli adatokat, megoldásra van szüksége toohave hello már hozzá van adva tooyour munkaterületen.

Miután telepítette az ügynököt telepíteni, és hello megoldás telepítése, a munkaterület hello átviteli adatok 2.0 csempe jelenik meg.

> [!NOTE]
> Jelenleg hello OMS portál tooview átviteli adatokat kell használnia. Hello Azure portál tooview átviteli adatokat nem használható.

![Átviteli adatokat csempe](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-hello-wire-data-20-solution"></a>Átviteli adatokat 2.0 hello megoldással

Az hello OMS-portálon, kattintson a hello **átviteli adatok 2.0** csempe tooopen hello átviteli adatokat irányítópult. hello irányítópult hello paneleken hello a következő táblázat tartalmazza. Minden egyes panel be, hogy a panel a feltételek hello megadva hatókör és a kívánt időtartományt megfelelő too10 elemeket sorolja fel. A napló keresési, amely visszaadja az összes rekord kattintva futtathatja **láthatja az összes** hello panel vagy hello panel fejléc kattintva hello alján.

| **Panel** | **Leírás** |
| --- | --- |
| Hálózati forgalmat rögzítő ügynökök | Ügynökök, a hálózati forgalom rögzítése hello számát jeleníti meg, és hello felső 10 olyan számítógépet megjeleníti, hogy a forgalom rögzítése. Kattintson a szám toorun hello napló keresése <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>. Hello lista toorun számítógépen a napló kattintson a Keresés gombra visszaadó hello rögzített bájtok teljes száma. |
| Helyi alhálózatok | Helyi alhálózatok ügynökök észlelt hello számát jeleníti meg.  Kattintson a szám toorun hello napló keresése <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> összes alhálózatot, amely felsorolja a hello minden egyes küldött bájtok száma. Kattintson a hello lista toorun lévő alhálózatot a napló keresés visszaadó hello hello alhálózattal küldött bájtok teljes száma. |
| Alkalmazásszintű protokollok | Alkalmazásszintű protokollok hello számát jeleníti meg használja, mint az ügynököket. Kattintson a szám toorun hello napló keresése <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>. A protokoll toorun a napló kattintson a Keresés gombra visszaadó hello hello protokoll használatával küldött bájtok teljes száma. |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Átviteli adatokat irányítópult](./media/log-analytics-wire-data/wire-data-dash.png)

Használhatja a hello **hálózati forgalmat rögzítő ügynökök** számítógépek fogyasztja, a hálózati sávszélesség hány panel toodetermine. Ez a panel segítségével könnyen keresés hello _chattiest_ számítógép a környezetben. Ezek a számítógépek sikerült túlterheltté vált, rendellenesen működő, vagy további hálózati erőforrások, mint a normál használatával.

![naplófájl-keresési példa](./media/log-analytics-wire-data/log-search-example01.png)

Hasonlóképpen, használhatja a hello **helyi alhálózatok** panel toodetermine mekkora hálózati forgalom mozgatása a alhálózatokon keresztül. Felhasználók gyakran határozza meg azt az alkalmazáshoz kritikus területeket körül alhálózatokat. Ezen a panelen a fenti területekre megtekintésében kínál.

![naplófájl-keresési példa](./media/log-analytics-wire-data/log-search-example02.png)

Hello **alkalmazásszintű protokollok** panel akkor hasznos, előnyös, mert tudják, mit protokollt használja. Például várt SSH toonot használatban a hálózati környezetben. Hello panelen elérhető adatok megtekintéséhez használatos időkategóriát gyorsan erősítse meg, és a várt eredmény disprove.

![naplófájl-keresési példa](./media/log-analytics-wire-data/log-search-example03.png)

Ebben a példában lehetett-feltárás SSH részletek toosee mely számítógépeket használ SSH és sok más kommunikáció részletei.

![SH keresési eredmények](./media/log-analytics-wire-data/ssh-details.png)

Akkor is hasznos tooknow Ha növekvő vagy csökkenő időbeli protokoll forgalmat. Például az alkalmazások által továbbított adatok hello mennyisége növekszik, ha, előfordulhat, hogy valami kell ügyelnie, vagy hogy előfordulhat, hogy a fontos.

## <a name="input-data"></a>A bemeneti adatok

Átviteli adatokat gyűjt, amelyeken engedélyezve hello ügynököt használ, a hálózati forgalommal kapcsolatos metaadatokat. Minden ügynök 15 másodpercenként adatokat küld.

## <a name="output-data"></a>kimeneti adatok

A típusú rekord _WireData_ jön létre az egyes bemeneti adatokat. WireData rekordok hello a következő táblázatban látható jellemzőkkel rendelkezik:

| Tulajdonság | Leírás |
|---|---|
| Computer | Számítógép neve, hol történt adatgyűjtés |
| TimeGenerated | Hello rekord idő |
| LocalIP | Hello helyi számítógép IP-címe |
| SessionState | Csatlakoztatva, vagy nincs csatlakoztatva |
| ReceivedBytes | Fogadott bájtok mennyiségét |
| ProtocolName | Hello használt hálózati protokoll neve |
| IPVersion | IP-verziója |
| Irány | Bejövő vagy kimenő |
| MaliciousIP | Egy ismert rosszindulatú forrás IP-címe |
| Súlyosság | Gyanús kártevőt súlyossága |
| RemoteIPCountry | Ország hello távoli IP-cím |
| ManagementGroupName | Hello Operations Manager felügyeleti csoport neve |
| SourceSystem | Ha adatokat gyűjtött a program forrás |
| SessionStartTime | A munkamenet kezdési idejét |
| SessionEndTime | Munkamenet befejezési időpontja |
| LocalSubnet | Alhálózati hol történt adatgyűjtés |
| LocalPortNumber | Helyi port száma |
| RemoteIP | Hello távoli számítógép által használt távoli IP-cím |
| RemotePortNumber | Hello távoli IP-cím által használt portszám |
| Munkamenet-azonosító | Két IP-címek közötti kommunikáció munkamenetet azonosító egyedi érték |
| SentBytes | Küldött bájtok száma |
| TotalBytes | A munkamenetben küldött bájtok teljes száma |
| ApplicationProtocol | A használt hálózati protokoll típusa   |
| Folyamatazonosító | Windows-folyamat azonosítója |
| Folyamatnév | Hello folyamat elérési útját és nevét |
| RemoteIPLongitude | IP-hosszúság érték |
| RemoteIPLatitude | IP-szélesség értéke |


## <a name="next-steps"></a>Következő lépések

- [Naplók keresése](log-analytics-log-searches.md) tooview részletes vezetékes keresési rekordok.
