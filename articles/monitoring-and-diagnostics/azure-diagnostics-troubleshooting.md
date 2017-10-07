---
title: Azure Diagnostics aaaTroubleshooting |} Microsoft Docs
description: "Kapcsolatos problémák elhárítása az Azure diagnostics Azure virtuális gépek, a Service Fabric vagy a Cloud Services használata esetén."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 66469bce-d457-4d1e-b550-a08d2be4d28c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: robb
ms.openlocfilehash: daaf9fa4c40982eb9ba04030d7e8ea1ad9fe085b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-troubleshooting"></a>Hibaelhárítás az Azure Diagnostics
Hibaelhárítási információk vonatkozó toousing Azure Diagnostics. Az Azure diagnostics további információkért lásd: [Azure Diagnostics áttekintése](azure-diagnostics.md).

## <a name="logical-components"></a>Logikai összetevők
**Diagnosztika beépülő modulja indítója (DiagnosticsPluginLauncher.exe)**: hello Azure Diagnostics bővítmény elindul. Hello bejegyzésének szolgál pont folyamatának.

**Diagnosztika beépülő modul (DiagnosticsPlugin.exe)**: fő folyamat, amely a fenti hello indítója indít és hello Figyelőügynököt konfigurálja, elindítja azt és élettartamuk kezeli. 

**Figyelési ügynök (MonAgent\*.exe folyamatok)**: ezek a folyamatok hello hello munka tömeges; azaz, figyelés, gyűjtő és hello diagnosztikai adatokat.  

## <a name="logartifact-paths"></a>Napló/összetevőinek elérési utak
Az alábbiakban hello elérési utak toosome fontos naplókat és az összetevők. A Microsoft tartsa utaló toothese teljes hello dokumentum többi részén hello:
### <a name="cloud-services"></a>Cloud Services
| Összetevő | Elérési út |
| --- | --- |
| **Az Azure Diagnostics konfigurációs fájl** | %SYSTEMDRIVE%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<verzió > \Config.txt |
| **Naplófájlok** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<verzió > \ |
| **Diagnosztikai adatok helyi tárolóból.** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Tables |
| **Figyelési ügynök konfigurációs fájl** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Az Azure diagnostics kiterjesztési csomag kiterjesztése** | %SYSTEMDRIVE%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<verzió > |
| **Naplófájl elérési útvonala segédprogram** | %SYSTEMDRIVE%\Packages\GuestAgent\ |
| **MonAgentHost naplófájl** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Configuration\MonAgentHost. < seq_num > .log |

### <a name="virtual-machines"></a>Virtuális gépek
| Összetevő | Elérési út |
| --- | --- |
| **Az Azure Diagnostics konfigurációs fájl** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<verzió > \RuntimeSettings |
| **Naplófájlok** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<verzió > \Logs\ |
| **Diagnosztikai adatok helyi tárolóból.** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Tables |
| **Figyelési ügynök konfigurációs fájl** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Configuration\MaConfig.xml |
| **Állapotfájl** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<verzió > \Status |
| **Az Azure diagnostics kiterjesztési csomag kiterjesztése** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion >|
| **Naplófájl elérési útvonala segédprogram** | C:\WindowsAzure\Packages |
| **MonAgentHost naplófájl** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Configuration\MonAgentHost. < seq_num > .log |

## <a name="metric-data-doesnt-show-in-azure-portal"></a>Metrika értékét nem jeleníti meg az Azure portálon
Az Azure Diagnostics álló, lemezcsoport típusú metrikaadatokat, Azure-portálon megjelenő biztosít. Ha problémába ütközik az jelent meg ezeket az adatokat a portálon, a jelölőnégyzet hello diagnosztikai tárfiók -> WADMetrics\* toosee táblából hello megfelelő metrika rekordok vannak-e. Itt hello hello tábla PartitionKey hello erőforrás-azonosítója, a virtuális gép vagy virtuálisgép-méretezési csoport, és hello RowKey hello metrika neve azaz teljesítményszámláló neve.

Hello erőforrás-azonosító nem megfelelő, ha a jelölőnégyzet diagnosztikai konfigurációja -> metrikák -> ResourceId toosee, ha az erőforrás-azonosító hello megfelelően van-e beállítva.

Nincs az adott metrika hello adat, ha a diagnosztikai konfiguráció ellenőrzése PerformanceCounter toosee -> Ha hello metrika (teljesítményszámláló) is megtalálható. Alapértelmezés szerint a számlálók következő hello engedélyezzük.
- \Processor(_Total)\% Processor Time
- \Memory\Available Bytes
- \ASP.NET alkalmazások (__teljes__) \Requests/Sec
- \ASP.NET alkalmazások (__teljes__) \Errors száma/s
- A várólistára \ASP.NET\Requests
- Elutasított \ASP.NET\Requests
- \Processor(w3wp)\% processzoridő
- \Process (w3wp) \Private bájt
- \Process(WaIISHost)\% processzoridő
- \Process (WaIISHost) \Private bájt
- \Process(WaWorkerHost)\% processzoridő
- \Process (WaWorkerHost) \Private bájt
- \Memory\Page hiba/mp
- \.NET CLR-memória (_globális_)\% töltött idő
- \LogicalDisk (C:) \Disk írt bájt/mp
- \LogicalDisk (C:) \Disk olvasott bájt/mp
- (D) \LogicalDisk \Disk írt bájt/mp
- (D) \LogicalDisk \Disk olvasott bájt/mp

Hello konfiguráció megfelelően van beállítva, de továbbra sem láthatók a hello metrika értékét, ha kövesse az alábbi iránymutatásokat hello hello további vizsgálat.


## <a name="azure-diagnostics-is-not-starting"></a>Az Azure Diagnostics nem indul.
Nézze meg **DiagnosticsPluginLauncher.log** és **DiagnosticsPlugin.log** hello hello helyéről fájlok naplófájlok megadott fent információt arról, hogy miért nem sikerült diagnostics toostart. 

Ha ezek a naplók alapján `Monitoring Agent not reporting success after launch`, az azt jelenti, hogy hiba történt a MonAgentHost.exe indítása. Keresse meg hello talál, amely a jelzett hello helyen `MonAgentHost log file` hello szakaszban.

utolsó sora hello hello naplófájlok hello kilépési kódot tartalmaz.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Ha talál egy **negatív** kilépési kód, tekintse meg a toohello [kilépési kód tábla](#azure-diagnostics-plugin-exit-codes) a hello [hivatkozások](#references).

## <a name="diagnostics-data-is-not-logged-tooazure-storage"></a>Diagnosztikai adatok bejelentkezve tooAzure tároló
Határozza meg, ha nincsenek adatok akkor jelenik meg, vagy csak hello adatok némelyike nem jelenik meg.

### <a name="diagnostics-infrastructure-logs"></a>Diagnosztika infrastruktúra naplók
Diagnosztika infrastruktúra naplók, amelyben az azure diagnostics naplózza az általa futtatott hibáit. Győződjön meg arról, hogy engedélyezte ([hogyan?](#how-to-check-diagnostics-extension-configuration)) konfigurációs infrastruktúra diagnosztikai naplók rögzítése és gyorsan keresse meg a megfelelő hibákat láthatók a hello `DiagnosticInfrastructureLogsTable` tábla konfigurált tárfiókba.

### <a name="no-data-is-showing-up"></a>Nincs adat akkor jelenik meg
hello leggyakoribb teljesen hiányzó esemény adatok oka nem megfelelően megadott tárfiók adatait.

Megoldás: A diagnosztika-konfiguráció kijavítása, majd telepítse újra a diagnosztika.

Ha hello tárfiók megfelelően konfigurált, a távoli asztali hello gép, és győződjön meg arról, hogy DiagnosticsPlugin.exe és MonAgentCore.exe futnak. Ha nem futnak, hajtsa végre az [nem indítja az Azure Diagnostics](#azure-diagnostics-is-not-starting). Hello folyamat fut, ha túl jump[első helyileg rögzített adatok](#is-data-getting-captured-locally) , és kövesse a jelen útmutató ott.

### <a name="part-of-hello-data-is-missing"></a>Hello adatok egy részét hiányzik.
Ha néhány adat kap, de nem más. Ez azt jelenti, hogy a hello adatgyűjtés / átviteli folyamat megfelelően van-e beállítva. Hajtsa végre a hello alszakaszokat itt toonarrow mely hello probléma le van:
#### <a name="is-collection-configured"></a>Gyűjtemény konfigurálható: 
Diagnosztikai konfiguráció hello része, amely arra utasítja az adott típusú összegyűjtött adatok toobe tartalmazza. [Tekintse át a konfigurációs](#how-to-check-diagnostics-extension-configuration) toomake meg arról, hogy nem keres adatok meg nem konfigurálta a gyűjteményhez.
#### <a name="is-hello-host-generating-data"></a>Hello állomás adatok létrehozása:
- **Teljesítményszámlálók**: Nyissa meg a perfmon, és ellenőrizze hello számlálót.
- **Nyomkövetési naplók**: távoli asztali kapcsolatot hello a virtuális gép, és adja hozzá a értékének a TextWriterTraceListener figyelőre toohello alkalmazás konfigurációs fájljában.  Lásd: http://msdn.microsoft.com/library/sk36c28t.aspx tooset hello szöveg figyelő be.  Győződjön meg arról, hogy hello `<trace>` elem `<trace autoflush="true">`.<br />
Ha létrehozott első nyomkövetési naplók nem látja, hajtsa végre a [több kapcsolatos nyomkövetési naplók hiányzó](#more-about-trace-logs-missing).
- **ETW-nyomkövetési**: távoli asztali kapcsolatot hello VM és telepítése a PerfView.  A PerfView futtatási fájl -> felhasználói parancs -> figyelés etwprovder1, etwprovider2, stb.  Vegye figyelembe, hogy hello figyelési parancs kis-és nagybetűket, és nem lehetnek szóközök hello vesszővel elválasztott listája ETW-szolgáltató között.  Ha hello parancs toorun, kattintson a hello hello jobb alsó részén hello Perfview eszköz toosee megkísérelt toorun és milyen hello eredmény volt "Log" gombra.  Feltéve, hogy hello adatok helyesek majd egy új ablakban jelenik-e fel, és pár másodpercen belül ettől kezdve látni fogja ETW-nyomkövetési jelent.
- **Eseménynaplók**: távoli asztali kapcsolatot hello virtuális gép. Nyissa meg `Event Viewer` és hello események biztosítson.
#### <a name="is-data-getting-captured-locally"></a>Adatok helyben első rögzített:
Ezután ellenőrizze, hogy hello adatok helyileg rögzített beolvasásakor.
hello adatok helyi tárolása `*.tsf` -fájlok [hello helyi tárolóból. diagnosztikai adatok](#log-artifacts-path). Különböző típusú naplók beolvasása gyűjtött különböző `.tsf` fájlokat. hello nevek szerepelnek az azure storage hasonló toohello tábla neve. Például `Performance Counters` kigyűjtése az összegyűjtött `PerformanceCountersTable.tsf`, eseménynaplók gyűjtött beolvasása `WindowsEventLogsTable.tsf`. Hello utasításokat a [helyi napló kinyerés](#local-log-extraction) tooopen hello helyi gyűjtés fájlok szakaszt, és ellenőrizze, hogy azokat első gyűjtött a lemezen.

Ha nem a helyileg gyűjtött első naplófájljaiban, és már rendelkezik ellenőrizni, hogy hello állomás adatokat előállító, valószínűleg konfigurációs probléma van. Tekintse át a gondosan hello megfelelő szakaszra vonatkozó konfigurációt. Emellett nézze át az előállított MonitoringAgent hello konfigurációs [MaConfig.xml](#log-artifacts-path) , és győződjön meg arról, hogy néhány rész: hello megfelelő naplófájlok forrás- és ne vesszenek el, hogy azt az azure diagnostics konfigurációs közötti címfordítás és figyelési ügynök konfigurációját.
#### <a name="is-data-getting-transferred"></a>Az első átvitt adatok:
Ha ellenőrizte hello adatok helyileg rögzített első, de még nem látja a tárfiókban lévő: 
- Mindenekelőtt győződjön meg arról, hogy megadta-e a megfelelő storage-fiókok és, hogy Ön rendelkezik nem tanúsítványváltást kulcsok etc.for hello megadott tárfiók. A felhőszolgáltatások néha látható, hogy a felhasználók nem frissíti a `useDevelopmentStorage=true`.
- Ha a megadott tárfiók helyes-e. Győződjön meg arról, hogy nem rendelkezik néhány korlátozásait a hálózati, amely engedélyezi a hello összetevők tooreach nyilvános tárolási végpontok. Egyirányú toodo, amely a hello tooremote asztali számítógéphez, és próbálja meg toowrite valami toohello ugyanazt a tárhelyet fiókot saját maga.
- Végül próbálja meg, és milyen hiba hogy jelez-figyelő ügynök által. Figyelési ügynök írási naplók `maeventtable.tsf` található [hello helyi tárolóból. diagnosztikai adatok](#log-artifacts-path). Kövesse az utasításokat [helyi napló kinyerés](#local-log-extraction) szakasz tooopen ezt a fájlt, és próbálja meg és mérje fel, hogy van-e `errors` jelző hibák tooread helyi fájlok írási vagy olvasási toostorage.

### <a name="capturing--archiving-logs"></a>A rögzítés / naplók archiválása
A végrehajtás során hello a fenti lépéseket, de sikerült nem mérje fel, mi nem volt megfelelő, és lépjen kapcsolatba az ügyfélszolgálattal továbbléphetnek. hello elsőként azokat fel, a gép toocollect naplók. Időt takaríthat módon, hogy saját maga. Futtatás hello `CollectGuestLogs.exe` segédprogram, [napló adatgyűjtési segédprogramot elérési](#log-artifacts-path) hello hoz létre egy zip-fájlt, és megfelelő azure naplófájlokat, és ugyanabban a mappában.

## <a name="diagnostics-data-tables-not-found"></a>Diagnosztikai adatok nem találhatók táblák
az Azure storage ETW-események okozó hello táblák megnevezett hello kód a következő használatával:

```C#
        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;
```

Például:

```XML
        <EtwEventSourceProviderConfiguration provider="prov1">
          <Event id="1" />
          <Event id="2" eventDestination="dest1" />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider="prov2">
          <DefaultEvents eventDestination="dest2" />
        </EtwEventSourceProviderConfiguration>
```
```JSON
"EtwEventSourceProviderConfiguration": [
    {
        "provider": "prov1",
        "Event": [
            {
                "id": 1
            },
            {
                "id": 2,
                "eventDestination": "dest1"
            }
        ],
        "DefaultEvents": {
            "eventDestination": "DefaultEventDestination",
            "sinks": ""
        }
    },
    {
        "provider": "prov2",
        "DefaultEvents": {
            "eventDestination": "dest2"
        }
    }
]
```
4 táblák létrehozó:

| Esemény | Tábla neve |
| --- | --- |
| szolgáltató = "prov1" &lt;eseményazonosító = "1" /&gt; |WADEvent + MD5("prov1") + "1" |
| szolgáltató = "prov1" &lt;eseményazonosító = "2" eventDestination = "dest1" /&gt; |WADdest1 |
| szolgáltató = "prov1" &lt;DefaultEvents /&gt; |WADDefault+MD5("prov1") |
| szolgáltató = "prov2" &lt;DefaultEvents eventDestination = "dest2" /&gt; |WADdest2 |

## <a name="references"></a>Referencia

### <a name="how-toocheck-diagnostics-extension-configuration"></a>Hogyan toocheck diagnosztikai bővítmény konfigurációja
Ennek legegyszerűbb módja toocheck hello a bővítmény konfigurációja toonavigate toohttp://resources.azure.com, keresse meg a toohello virtuális gép vagy a felhőbeli szolgáltatás mely hello Azure Diagnostics bővítmény (IaaSDiagnostics / PaaDiagnostics) az adott.

Azt is megteheti, a távoli asztali kapcsolatot hello gép, és nézze meg hello Azure Diagnostics konfigurációs fájl hello megfelelő szakaszban leírt [Itt](#log-artifacts-path).

Mindkét esetben keresse meg a **Microsoft.Azure.Diagnostics** a hello **xmlCfg** vagy **WadCfg** mező. 

Virtuális gépek esetén ha hello WadCfg mező szerepel, ez azt jelenti hello config a JSON-ban. Ha hello xmlCfg mező található, akkor az XML-ben hello config és base64 kódolása. Túl kell[dekódolni a](http://www.bing.com/search?q=base64+decoder) toosee hello XML-t a diagnosztika töltött be.

A felhőalapú szolgáltatás-szerepkör a lemezről, hello konfigurációs választott hello adatok is, ha base64-kódolású, ezért túl kell[dekódolni a](http://www.bing.com/search?q=base64+decoder) toosee hello XML-t a diagnosztika töltött be.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Az Azure Diagnostics beépülő modul olyan kilépési kódot
hello beépülő modul hello a következő kilépési kódot adja vissza:

| Kilépési kód | Leírás |
| --- | --- |
| 0 |Siker. |
| -1 |Általános hiba. |
| -2 |Nem lehet tooload hello rcf fájlt.<p>Ez a belső hiba csak megtörténik hello Vendég ügynök beépülő modulja indítója van kézzel is elindítható, nem megfelelő, a virtuális gép hello. |
| -3 |Hello diagnosztika konfigurációs fájl nem tölthető be.<p><p>Megoldás: A konfigurációs fájl nem továbbítja a séma érvényesítését a okozza. hello megoldás tooprovide egy konfigurációs fájl, amely megfelel az hello séma. |
| -4 |A figyelőügynök diagnosztika hello egy másik példánya már használja hello helyi erőforrás-könyvtár.<p><p>Megoldás: Adjon meg más értéket a **LocalResourceDirectory**. |
| -6 |hello Vendég ügynök beépülő modulja indítója toolaunch diagnosztika érvénytelen parancssorral történt kísérlet.<p><p>Ez a belső hiba csak megtörténik hello Vendég ügynök beépülő modulja indítója van kézzel is elindítható, nem megfelelő, a virtuális gép hello. |
| -10 |hello diagnosztika beépülő modul kilépett a nem kezelt kivétel. |
| -11 |hello Vendég ügynöke (agent) nem toocreate hello folyamat elindítása, és figyelőügynök hello felügyeletéért felelős.<p><p>Megoldás: Ellenőrizze, hogy elegendő rendszererőforrás elérhető toolaunch új folyamatok.<p> |
| -101 |Érvénytelen argumentumok hello diagnosztika beépülő modul hívásakor.<p><p>Ez a belső hiba csak megtörténik hello Vendég ügynök beépülő modulja indítója van kézzel is elindítható, nem megfelelő, a virtuális gép hello. |
| -102 |a rendszer nem tooinitialize magát hello beépülő modul folyamatot.<p><p>Megoldás: Ellenőrizze, hogy elegendő rendszererőforrás elérhető toolaunch új folyamatok. |
| -103 |a rendszer nem tooinitialize magát hello beépülő modul folyamatot. Kifejezetten nem toocreate hello naplózó objektum.<p><p>Megoldás: Ellenőrizze, hogy elegendő rendszererőforrás elérhető toolaunch új folyamatok. |
| -104 |Nem lehet tooload hello rcf fájl hello vendégügynök által biztosított.<p><p>Ez a belső hiba csak megtörténik hello Vendég ügynök beépülő modulja indítója van kézzel is elindítható, nem megfelelő, a virtuális gép hello. |
| -105 |hello diagnosztika beépülő modul hello diagnosztika konfigurációs fájl nem nyitható meg.<p><p>Ez a belső hiba csak megtörténik hello diagnosztika beépülő modul az kézzel is elindítható, helytelenül, a virtuális gép hello. |
| -106 |Hello diagnosztika konfigurációs fájl nem olvasható.<p><p>Megoldás: A konfigurációs fájl nem továbbítja a séma érvényesítését a okozza. Ezért hello megoldás nem tooprovide, amely megfelel az hello séma konfigurációs fájlt. Lásd: [hogyan toocheck diagnosztika Bővítménykonfiguráció](#how-to-check-diagnostics-extension-configuration). |
| -107 |hello erőforrás directory fázis toohello figyelőügynök érvénytelen.<p><p>Ez a belső hiba csak megtörténik figyelőügynök hello van kézzel is elindítható, nem megfelelő, a virtuális gép hello.</p> |
| -108 |Nem tooconvert hello diagnosztika konfigurációs fájlból hello figyelési ügynök konfigurációs fájlt.<p><p>Ez a belső hiba csak akkor kell történik, ha a hello diagnosztika beépülő modul manuális hívása érvénytelen konfigurációs fájlt. |
| -110 |Általános diagnosztika konfigurációs hiba.<p><p>Ez a belső hiba csak akkor kell történik, ha a hello diagnosztika beépülő modul manuális hívása érvénytelen konfigurációs fájlt. |
| -111 |Nem lehet toostart hello figyelési ügynök.<p><p>Megoldás: Ellenőrizze, hogy rendelkezésre áll-e elegendő rendszer erőforrás. |
| -112 |Általános hiba |

### <a name="local-log-extraction"></a>Helyi napló kinyerés
figyelési ügynök hello gyűjti a naplókat, és mint `.tsf` fájlokat. `.tsf`fájl nem olvasható, de a átalakíthatja egy `.csv` az alábbiak szerint: 

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Új fájl neve `<relevantLogFile>.csv` létrejön hello azonos elérési útjára hello megfelelő `.tsf` fájlt.

**Megjegyzés:**: csak akkor kell toorun ezt a segédprogramot hello fő tsf fájl (pl. PerformanceCountersTable.tsf) ellen. fájlok kísérő hello (pl. PerformanceCountersTables_\*\*001.tsf, PerformanceCountersTables_\*\*002. tsf stb.) a rendszer automatikusan fogja feldolgozni.

### <a name="more-about-trace-logs-missing"></a>További információ a nyomkövetési naplók hiányzik

**Megjegyzés:** Ez többnyire érvényes toocloud szolgáltatások csak akkor, ha az infrastruktúra-szolgáltatási virtuális gép futó alkalmazást a hello DiagnosticsMonitorTraceListener van konfigurálva. 

- Győződjön meg arról, hogy hello DiagnosticMonitorTraceListener van konfigurálva a hello web.config vagy az App.config fájlt.  Ez alapértelmezés szerint a cloud service projektek úgy van konfigurálva, de az egyes ügyfelek megjegyzéseket fűzni out, aminek következtében hello nyomkövetési utasítások toonot diagnosztika lehet összegyűjteni. 
- Ha hello OnStart vagy futtassa a metódus nem első írja a naplókat győződjön meg arról, hogy hello DiagnosticMonitorTraceListener hello app.config van.  Alapértelmezés szerint a hello Web.config fájlban, de csak érvényes w3wp.exe; belül futó toocode így az app.config toocapture nyomkövetések WaIISHost.exe futó van szükség.
- Ellenőrizze, hogy Diagnostics.Trace.TraceXXX Diagnostics.Debug.WriteXXX helyett használ.  hello hibakeresési utasításokat törlődni fog a kiadott buildjét.
- Ellenőrizze, hogy hello lefordított kódot ténylegesen hello Diagnostics.Trace sorok (megfelelési, ildasm vagy ILSpy tooverify használja).  Diagnostics.Trace parancsok el lesznek távolítva összeállított hello bináris hello NYOMKÖVETÉSI feltételes fordítási szimbólum használata.  Ha msbuild toobuild hello projekt segítségével majd ez az egy gyakori probléma toorun be.

## <a name="known-issues-and-mitigations"></a>Ismert problémák és azok mérséklési
Ismert azok mérséklési szolgáltatással kapcsolatos ismert problémák listája itt található:

**1. a .NET 4.5 függőséget:**

ÜVEGVATTA rendelkezik egy runtime függőségdefiniáló, .NET 4.5 keretrendszerre vagy újabb. Ezt a hello időpontjában felhőszolgáltatások, valamint az összes hivatalos azure virtuális gép alap-rendszerképek kiosztott összes gép .NET 4.5-ös vagy újabb szükséges. Ha megpróbál toorun ÜVEGVATTA egy számítógépen, amely nem rendelkezik a .NET 4.5-ös vagy újabb helyzetben azonban továbbra is lehetséges tooland. Ez akkor fordul elő, amikor hoz létre, hogy a számítógép egy régi lemezképet, vagy a pillanatkép; vagy a saját egyéni lemez állapotba.

Ez általában akkor jelentkezik, mint a kilépési kódot **255** DiagnosticsPluginLauncher.exe jelentés futtatásakor. Hiba történik, toohello nem kezelt kivétel miatt: 
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Megoldás:** telepítse a .NET 4.5-ös vagy újabb verzióját a számítógépen.

**2. Számlálók teljesítményadatokat érhető el a tárolási, de nem jelennek meg a portálon**

Virtuális gépek portál tapasztalat egyes teljesítményszámlálók alapértelmezés szerint. Ha nem látja, és biztos, hogy hello adatok első keletkezik, mert a tároló érhető el. Részletek:
- Ha hello adatok angolul számláló nevet tartalmaz. Ha hello számláló neve nem angol nyelvű, a portál metrika diagram nem lesz képes toorecognize azt.
- Helyettesítő karakterek használata (\*) a teljesítményszámláló-neveket, hello portál nem lesz képes toocorrelate hello konfigurálva és teljesítményszámláló gyűjtése.

**Megoldás**: hello gép nyelvi tooEnglish rendszer fiókok módosítása. Vezérlőpult -> terület -> felügyeleti -> Beállítások ->, hogy hello egyéni nyelvi ne törölje a jelet "Üdvözli és rendszerfiókok" toosystem fiók alkalmazza. Emellett győződjön meg arról, ha azt szeretné, portál toobe elsődleges felhasználási élményét, ne használjon helyettesítő karakterek.
