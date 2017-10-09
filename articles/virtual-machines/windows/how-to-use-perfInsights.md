---
title: a Microsoft Azure-ban PerfInsights aaaHow toouse |} Microsoft Docs
description: "Tanulja meg hogyan toouse PerfInsights tootroubleshoot Windows virtuális gép teljesítménybeli problémákat."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: genli
ms.openlocfilehash: f23ff7708c0c63bd02674b1bdc07753e8a89d9be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-perfinsights"></a>Hogyan toouse PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) egy automatizált parancsfájlt, amely hasznos diagnosztikai adatokat gyűjt, i/o-magas terhelés terhelések, egy elemző jelentés toohelp biztosít Windows virtuális gép teljesítményét kapcsolatos problémák elhárítása a Microsoft Azure. 

Azt javasoljuk, hogy futtassa ezt a parancsfájlt egy támogatási jegy a Microsofttal a virtuális gépek teljesítményproblémái megnyitása előtt.

## <a name="supported-troubleshooting-scenarios"></a>Hibaelhárítási forgatókönyveket támogatja.

PerfInsights is összegyűjti és elemzi az adatokat több különböző, egyedi forgatókönyvek szerint vannak csoportosítva.

### <a name="collect-disk-configuration"></a>A lemezkonfiguráció gyűjtése 

Ebben a forgatókönyvben gyűjt hello lemezkonfiguráció és más fontos információkat, beleértve a következő elemek hello:

-   Eseménynaplók

-   Az összes bejövő és kimenő kapcsolatok hálózati állapota

-   Hálózati és tűzfalbeállításokat konfigurációs beállításai

-   Feladatlista hello rendszeren futó minden alkalmazás

-   Információs fájl hozta létre msinfo32 hello virtuális gép (VM)

-   Microsoft SQL Server adatbázis-konfigurációs beállításokat (Ha a virtuális gép hello azonosítottak egy SQL Servert futtató kiszolgáló)

-   Tárolási megbízhatóság számlálók

-   Fontos Windows-gyorsjavítások

-   Telepített fájlrendszerszűrő-illesztőprogramok

Ez az információ, amelyek nem befolyásolják a hello rendszer passzív gyűjteménye. 

>[!Note]
>Ebben a forgatókönyvben az egyes forgatókönyvek a következő hello automatikusan tartalmazza.

### <a name="benchmarkstorage-performance-test"></a>Teljesítményteszt tárolási teljesítményének tesztelése

Ez a forgatókönyv futtatása hello [diskspd](https://github.com/Microsoft/diskspd) olyan, amelyek minden meghajtó csatlakoztatva toohello VM elvégez egy teljesítménytesztet a teszt (iops-érték és MB/s). 

> [!Note]
> Ez a forgatókönyv hatással lehet a hello rendszer, és egy éles rendszeren nem futtatható. Ha szükséges, futtassa ebben a forgatókönyvben egy dedikált karbantartási ablak tooavoid problémákat. A nyomkövetési vagy teljesítményteszt teszt által okozott nagyobb munkaterhelést ronthatja hello teljesítményt, a virtuális gép.
>

### <a name="general-vm-slow-analysis"></a>Általános virtuális gép lassú elemzés 

Ez a forgatókönyv futtatása egy [teljesítményszámláló](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) nyomkövetési hello Generalcounters.txt fájlban megadott hello számlálók használatával. Ha hello VM SQL Servert futtató kiszolgálónak, teljesítmény számláló nyomkövetés futása hello Sqlcounters.txt fájlban található hello számlálók használatával. Teljesítmény diagnosztikai adatokat is tartalmaz.

### <a name="vm-slow-analysis-and-benchmark"></a>Virtuális gép lassú elemzés és teljesítményteszt

Ez a forgatókönyv futtatása egy [teljesítményszámláló](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) nyomkövetési elé egy [diskspd](https://github.com/Microsoft/diskspd) teljesítményteszt. 

> [!Note]
> Ez a forgatókönyv hatással lehet a hello rendszer, és egy éles rendszeren nem futtatható. Ha szükséges, futtassa ebben a forgatókönyvben egy dedikált karbantartási ablak tooavoid problémákat. A nyomkövetési vagy teljesítményteszt teszt által okozott nagyobb munkaterhelést ronthatja hello teljesítményt, a virtuális gép.
>

### <a name="azure-files-analysis"></a>Az Azure fájlok elemzés 

Ebben a forgatókönyvben egy különleges teljesítmény számláló rögzítési együtt a hálózati nyomkövetés fut. A rögzítés összes hello "SMB-ügyfél megosztások" számlálókat tartalmaz. hello az alábbiakban néhány kulcsfontosságú SMB ügyfél megosztás teljesítményszámlálók hello rögzítési részét képező:

| **Típus**     | **SMB-ügyfél megosztások számláló** |
|--------------|-------------------------------|
| IO         | Kérelmek/másodperc             |
|              | Olvassa el a kérelmek/másodperc             |
|              | Írási kérelmek/másodperc            |
| Késés      | Kérelemfeldolgozás mp/adatok         |
|              | Átlagos olvasási idő                 |
|              | Átlagos mp/írás                |
| IO-méret      | Átlagos Bájtok/kérelem       |
|              | Átlagos Bájtos, Olvasás               |
|              | Átlagos Bájt írása              |
| Teljesítmény   | Adatok bájtok/s                |
|              | Olvasott bájt/mp                |
|              | Írási bájtok/s               |
| Várólista hossza | Átlagos Olvasási várólistájának hossza        |
|              | Átlagos Írni a várólista hossza       |
|              | Átlagos Várólista hossza        |

### <a name="custom-configuration"></a>Egyéni konfiguráció 

Egyéni konfiguráció futtatásakor meg (teljesítmény diagnosztika, teljesítményszámláló, következő helyen, hálózati, storport) összes adat a párhuzamosan futó, attól függően, hány különböző nyomkövetések van kiválasztva. Nyomkövetés befejezése után hello eszköz hello diskspd teljesítményteszt, fut, ha be van jelölve. 

> [!Note]
> Ez a forgatókönyv hatással lehet a hello rendszer, és egy éles rendszeren nem futtatható. Ha szükséges, futtassa ebben a forgatókönyvben egy dedikált karbantartási ablak tooavoid problémákat. A nyomkövetési vagy teljesítményteszt teszt által okozott nagyobb munkaterhelést ronthatja hello teljesítményt, a virtuális gép.
>

## <a name="what-kind-of-information-is-collected-by-hello-script"></a>Milyen típusú adatokat gyűjtött hello parancsfájlt?

Windows virtuális gép, lemezt vagy tárolási adatokat a konfigurációs készletbe, teljesítményszámlálók, a naplók és a különböző nyomkövetési adatokat gyűjti használt hello teljesítmény forgatókönyvtől függően:

|Összegyűjtött adatok                              |  |  | Teljesítmény-forgatókönyvek |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Lemez konfigurációs gyűjtése | Teljesítményteszt tárolási teljesítményének tesztelése | Általános virtuális gép lassú elemzés | Virtuális gép lassú elemzés és teljesítményteszt | Az Azure fájlok elemzés | Egyéni konfiguráció |
| Eseménynapló információk      | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Rendszerinformáció               | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Kötet térkép                       | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Lemez térkép                         | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Az éppen futó feladatok                    | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Tárolási megbízhatóság számlálók     | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Szolgáltatás adatai              | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Fsutil kimeneti                    | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Szűrő-illesztőprogram adatai               | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Netstat kimeneti                   | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Hálózati konfiguráció            | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Tűzfal-konfiguráció           | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| SQL Server-konfigurációs         | Igen                        | Igen                                | Igen                      | Igen                            | Igen                  | Igen                  |
| Teljesítmény diagnosztikai nyomkövetési * |                            |                                    | Igen                      |                                |                      | Igen                  |
| Teljesítményszámláló nyomkövetési **     |                            |                                    |                          |                                |                      | Igen                  |
| SMB-számláló nyomkövetési **             |                            |                                    |                          |                                | Igen                  |                      |
| SQL Server számláló nyomkövetési **      |                            |                                    |                          |                                |                      | Igen                  |
| Nyomkövetési a következő helyen                      |                            |                                    |                          |                                |                      | Igen                  |
| StorPort-nyomkövetési                   |                            |                                    |                          |                                |                      | Igen                  |
| Hálózati nyomkövetés futtatása                    |                            |                                    |                          |                                | Igen                  | Igen                  |
| A Diskspd teljesítményteszt nyomkövetési x      |                            | Igen                                |                          | Igen                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Teljesítmény-diagnosztikai nyomkövetési (*)

Egy szabályalapú motor hello toocollect háttéradatok fut, és folyamatos teljesítményt eseményadatokat. a következő szabályok hello jelenleg támogatottak:

- HighCpuUsage szabály: magas CPU-használat időszakok észleli, és ezen időszakokban hello felső CPU-használat fogyasztók mutatja.
- HighDiskUsage szabály: a fizikai lemezek magas lemez időszakai észleli, és ezen időszakokban használati fogyasztók hello felső lemez mutatja.
- HighResolutionDiskMetric szabály: látható IOPS, az átviteli sebesség és I/O várakozási ideje metrikák / minden fizikai lemez 50 ideje ezredmásodpercben. Ennek segítségével tooquickly időszakok szabályozás lemez azonosításához használatos.
- HighMemoryUsage szabály: felső memóriaterület időszakai észleli, és ezen időszakokban használati fogyasztók hello felső memória mutatja.

> [!NOTE] 
> Jelenleg a Windows hello .NET-keretrendszer 3.5 tartalmazó vagy későbbi verziói támogatottak.

### <a name="performance-counter-trace-"></a>Teljesítmény-számláló nyomkövetési (*)

Gyűjti a következő teljesítményszámlálók hello:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk, \LogicalDisk
- \Cache\Dirty lapok, \Cache\Lazy írási ürítések másodpercenkénti \Server\Pool lapozható, hibák, a lapozható \Server\Pool hibák
- Kijelölt számlálók \Network felületet, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network Adapter, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \Network QoS Policy\Packets, a hálózati illesztő kártya processzortevékenység, \Microsoft Winsock BSP \Per

#### <a name="for-sql-server-instances"></a>SQL Server-példányok
- \SQL server: pufferkezelőnek, \SQLServer:Resource készlet statisztikák, \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, statisztikák
- \SQLServer:Access módszerek

#### <a name="for-azure-files"></a>Az Azure-fájlok
\SMB Ügyfélmegosztások

### <a name="diskspd-benchmark-trace-"></a>A Diskspd teljesítményteszt nyomkövetési (*)
A Diskspd IO munkaterhelés tesztek [operációsrendszer-lemez (írás) és készlet meghajtók (olvasás/írás)]

## <a name="run-hello-perfinsights-on-your-vm"></a>Hello PerfInsights futtassa a virtuális gépen

### <a name="what-do-i-have-tooknow-before-i-run-hello-script"></a>Mit kell tooknow I hello parancsfájl futtatása előtt? 

**Parancsfájl-követelmények**

1.  Ezt a parancsfájlt a virtuális Gépet, amely rendelkezik hello teljesítményprobléma hello kell futtatni. 

2.  hello használata támogatott a következő operációs rendszer: Windows Server 2008 R2, 2012, 2012 R2-ben 2016; Windows 8.1 és Windows 10.

**Lehetséges problémák, a virtuális gépek éles hello parancsfájl futtatásakor:**

1.  hello parancsfájl kedvezőtlen hatással lehet hello VM hello teljesítményének hello "Referenciaalap" vagy "Egyéni" forgatókönyvekben, amelyek a következő helyen vagy a DiskSpd segítségével konfigurálhatók és együttes használata esetén. Legyen óvatos hello parancsfájl éles környezetben való futtatásakor.

2.  Amikor parancsfájllal hello együtt hello "Referenciaalap" vagy "Egyéni" forgatókönyv, amely a DiskSpd használatával van konfigurálva, győződjön meg arról, hogy nincs más háttértevékenység ütköző hello i/o-munkaterhelés tesztelt hello lemezeken.

3.  Alapértelmezés szerint a hello parancsfájl hello ideiglenes tárolási meghajtó toocollect adatokat. Ha nyomkövetése engedélyezve van a hosszabb ideig marad, előfordulhat, hogy gyűjtött adatok mennyisége hello megfelelő. Ezzel csökkenthető a lemezen hello ideiglenes, ezért érintő bármilyen alkalmazás, amely a meghajtó támaszkodik hello rendelkezésre állását.

### <a name="how-do-i-run-perfinsights"></a>Hogyan futtathatok PerfInsights? 

toorun hello parancsfájl, kövesse az alábbi lépéseket:

1. Töltse le [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Hello PerfInsights.zip fájl feloldása. toodo ez, kattintson a jobb gombbal hello PerfInsights.zip fájl kiválasztása **tulajdonságok**. A hello **általános** lapon jelölje be **Unblock** , és válassza **OK**. Ezzel biztosítható, hogy hello parancsfájl nélkül más biztonsági kérdések fut-e.  

    ![Hello zip-fájl zárolásának feloldása](media/how-to-use-perfInsights/unlock-file.png)

3.  Bontsa ki a tömörített hello PerfInsights.zip fájl az ideiglenes meghajtóba (alapértelmezés szerint ez általában a hello D meghajtó). hello tömörített fájl tartalmaznia kell hello következő fájlok és mappák:

    ![hello zip mappában lévő fájlok](media/how-to-use-perfInsights/file-folder.png)

4.  Nyissa meg a Windows Powershellt rendszergazdaként, és futtassa a hello PerfInsights.ps1 parancsfájl.

    ```
    cd <hello path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Lehetséges, hogy tooenter "y" Ön tooif ismételt tooconfirm, amelyet az toochange hello végrehajtási házirendet.

    Hello jogi nyilatkozat párbeszédpanelen lehetősége van a hello beállítás tooshare diagnosztikai adatokat a Microsoft Support. Licenc-megállapodás toocontinue toohello is bele kell egyeznie. Adja meg a beállításokat, és kattintson a **-parancsfájl futtatása**.

    ![Jogi nyilatkozat mezőbe](media/how-to-use-perfInsights/disclaimer.png)

5.  Küldje el a hello esetek számát, amennyiben az rendelkezésre áll, (Ez a a statisztika) hello parancsprogram futtatásakor. Kattintson a **OK**.
    
    ![Adja meg a támogatási azonosító](media/how-to-use-perfInsights/enter-support-number.png)

6.  Válassza ki az átmeneti tárolási meghajtót. hello parancsfájlt is automatikus észlelésű hello hello meghajtó betűjele. Ha bármilyen probléma ebben a szakaszban lép fel, valószínűleg tooselect hello meghajtó kéri (hello alapértelmezett meghajtó: D). Létrehozott naplók vannak tárolva itt hello napló\_gyűjtemény mappát. Adja meg, vagy fogadja el hello meghajtóbetűjelet után kattintson **OK**.

    ![Adja meg a meghajtó](media/how-to-use-perfInsights/enter-drive.png)

7.  A hibaelhárítási választása a listában megadott hello.

       ![Válassza ki a támogatási példák](media/how-to-use-perfInsights/select-scenarios.png)

8.  PerfInsights felhasználói felület nélkül is futtathatja.

    hello következő parancs futtatása hello "Általános virtuális gép lassú analysis" hibaelhárítási forgatókönyv felhasználói felület figyelmeztetés nélkül, vagy adatok rögzítéséhez 30 másodpercig. A rendszer kérni fogja azt tooconsent toohello azonos jogi nyilatkozat és a 4. lépésben említett EULA.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

    Ha azt szeretné, PerfInsights toorun csendes módban, a **- AcceptDisclaimerAndShareDiagnostics** paraméter. Például használja a következő parancs hello:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-hello-script"></a>Hogyan hibáinak elhárítása problémák hello parancsprogram futtatása során?

Ha hello parancsfájl rendellenesen áll, törölheti is inkonzisztens állapotba együtt hello parancsfájl futtatásával hello - karbantartási kapcsoló, az alábbiak szerint:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Ha bármilyen probléma hello automatikus észlelési hello ideiglenes meghajtó fordulhat elő, valószínűleg tooselect hello meghajtó kéri (hello alapértelmezett meghajtó: D).

![Adja meg meghajtó](media/how-to-use-perfInsights/enter-drive.png)

hello parancsfájl hello segédeszközök eltávolítja, és eltávolítja az ideiglenes mappát.

### <a name="troubleshooting-other-script-issues"></a>Más parancsfájl problémák elhárítása 

Ha bármilyen probléma hello parancsprogram futtatásakor fordulhat elő, nyomja meg a Ctrl + C toointerrupt hello parancsfájl végrehajtása. tooremove ideiglenes objektumok, hello, "Nem várt leállás után törlése" című szakaszában talál.

Ha tooexperience parancsfájl hiba még több próbálkozást követően továbbra is, azt javasoljuk, hogy hello parancsfájl "hibakeresési módban" hello segítségével "-Debug" paraméter a következő indításakor.

Miután hello hiba történik, a Másolás hello teljes kimenete hello PowerShell-konzolban, és elküldi a toohello Microsoft Support ügynök segít a toohelp hello probléma elhárításához.

### <a name="how-do-i-run-hello-script-in-custom-configuration-mode"></a>Hogyan futtathatok hello parancsfájl egyéni beállításokkal?

Hello kiválasztásával **egyéni** konfigurációs, engedélyezheti a több nyomkövetések párhuzamos (használja a Shift toomulti-kiválasztása):

![Válassza ki a forgatókönyvek](media/how-to-use-perfInsights/select-scenario.png)

Hello teljesítmény diagnosztika, teljesítmény-számláló nyomkövetési, következő helyen nyomkövetési, hálózati nyomkövetés vagy Storport nyomkövetési forgatókönyvek kiválasztásakor hello utasításait hello kövesse, és próbálja tooreproduce hello teljesítményromlást hello nyomkövetések elindítása után.

a következő párbeszédpanel hello lehetővé teszi a nyomkövetés indításához:

![Start-nyomkövetési](media/how-to-use-perfInsights/start-trace-message.png)

toostop hello nyomkövetési adatokat, egy második párbeszédpanel akkor tooconfirm hello parancs.

![nyomkövetés leállítása](media/how-to-use-perfInsights/stop-trace-message.png)
![nyomkövetés leállítása](media/how-to-use-perfInsights/ok-trace-message.png)

Ha a nyomkövetési adatokat hello vagy műveletek befejeződtek, egy új fájl jön létre a D:\\napló\_gyűjtemény (vagy hello ideiglenes meghajtó) nevű **CollectedData\_éééé-hh-nn\_hh\_mm \_ss.zip.** A fájl toohello támogatási ügynök elemzéshez küldhet.

## <a name="review-hello-diagnostics-report-created-by-perfinsights"></a>Tekintse át a PerfInsights által létrehozott hello diagnosztikai jelentés

Hello belül **CollectedData\_éééé-hh-nn\_hh\_mm\_ss.zip fájl** , amely PerfInsights által generált, található olyan HTML-jelentést, amely leírja, hello eredményei PerfInsights. tooreview hello jelentés, bontsa ki a hello **CollectedData\_éééé-hh-nn\_hh\_mm\_ss.zip** fájlt, és nyissa meg a hello **PerfInsights diagram jelentés.HTML**fájlt.

Jelölje be hello **megállapítások** fülre.

![Keresés lap](media/how-to-use-perfInsights/findingtab.png)

**Megjegyzések**

-   Piros üzenetek ismert konfigurációs problémákat, amelyek teljesítményproblémákat okozhatnak.

-   Sárga üzeneteket, amelyben egy nem szükségszerűen teljesítményproblémákat okozhatnak nem optimális konfigurációkra figyelmeztetések.

-   A kék üzenetek csak tájékoztató jellegű utasítások.

Felülvizsgálati hello piros tooget az összes hibaüzenet HTTP hivatkozások részletesebb hello eredményeit, és hogyan ezek hatással lehetnek hello teljesítményt és ajánlott eljárások a teljesítményre optimalizált konfigurációk kapcsolatos információkat.

### <a name="disk-configuration-tab"></a>Lemez konfiguráció lap

Hello **áttekintése** szakasz hello tárolási konfigurációt, beleértve a Diskpart és a tárolóhelyek különböző nézeteit jelenít meg.

Hello **DiskMap** és **VolumeMap** szakaszok ismertetik a kettős perspektíva hogyan logikai kötetek és a fizikai lemezek más kapcsolódó tooeach.

A hello fizikai lemez perspektíva (DiskMap) a hello táblázat hello lemez futó összes logikai kötetek. A következő példa hello PhysicalDrive2 több partícióra (J és H) létrehozott 2 logikai kötetek fut:

![az adatok lapon](media/how-to-use-perfInsights/disktab.png)

A kötet perspektíva hello (*VolumeMap*), hello táblázatban megtekintheti az összes hello fizikai lemez minden logikai mennyiségi. Figyelje meg, hogy a RAID/dinamikus lemez, futtassa az logikai kötet több fizikai lemezek esetén. A következő minta hello *C:\\csatlakoztatási* konfigurálva, a csatlakozási pont *SpannedDisk* a PhysicalDisks \#2 és \#3:

![kötet lap](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-server-tab"></a>SQL Server lap

Ha hello cél virtuális gép üzemelteti az SQL Server-példányokat, látni fogja a hello jelentésben nevű egy lap **SQL Server**:

![SQL lap](media/how-to-use-perfInsights/sqltab.png)

Ez a szakasz egy "Overview" és a további sub lapon az egyes hello hello virtuális gép futó SQL Server-példányokat.

hello "Overview" szakasz összegzi az összes hello fizikai lemez (rendszer- és adatlemezekkel) futtató és az adatfájlok és a tranzakciós naplófájlok vegyesen tartalmazhatnak, amely hasznos táblát tartalmaz.

A példában a következő hello *PhysicalDrive0* (fut hello C meghajtó) jelenik meg, mivel mindkét hello *modeldev* és *modellog* hello C meghajtón található fájlok és különböző típusú vannak (például adatfájl és a tranzakciós napló, illetve):

![LogInfo](media/how-to-use-perfInsights/loginfo.png)

hello SQL Server példány lapok tartalmaz egy általános szakaszt, amely a kijelölt példányhoz hello alapvető információkat jelenít meg, és további szakaszaiban speciális tudnivalókat – többek között a beállításait, a konfiguráció és a felhasználói beállítások.

## <a name="references-toohello-external-tools-used"></a>Toohello használt külső eszközök hivatkozik

### <a name="diskspd"></a>A Diskspd

A DISKSPD egy olyan tárolási terhelés generator és teljesítmény vizsgálati eszköz a hello Windows és Windows Server és Cloud kiszolgálói infrastruktúra mérnöki csapat. További információkért lásd: [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>Következő helyen

Következő helyen, a Windows Performance eszközök Kit hello parancssori eszköz toocapture nyomkövetéseit.

További információkért lásd: [Windows Performance Toolkit – a következő helyen](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Következő lépések

### <a name="upload-diagnostics-logs-and-reports-toomicrosoft-support-for-further-review"></a>Töltse fel diagnosztikai naplózza, és további ellenőrzésre támogatási tooMicrosoft jelentések

Microsoft Support csapattal hello használatakor esetleg kért tootransmit hello kimeneti PerfInsights tooassist hello hibaelhárítási folyamat által generált.

hello támogatási ügynök DTM munkaterület hozza létre, és küld e-mailt, amely tartalmaz egy hivatkozást toohello [DTM portal (https://filetransfer.support.microsoft.com/EFTClient/Account/Login.htm) és egy egyedi felhasználói Azonosítót és jelszót.

Az üzenet fog érkezni a **CTS automatikus diagnosztika szolgáltatás** (ctsadiag@microsoft.com).

![Minta hello üzenet](media/how-to-use-perfInsights/supportemail.png)

A fokozott biztonság érdekében fogja szükséges toochange jelszó első alkalommal.

Miután tooDTM bejelentkezik, a párbeszédpanel bezárásához tooupload hello található **CollectedData\_éééé-hh-nn\_hh\_mm\_ss.zip** PerfInsights által összegyűjtött fájl.
