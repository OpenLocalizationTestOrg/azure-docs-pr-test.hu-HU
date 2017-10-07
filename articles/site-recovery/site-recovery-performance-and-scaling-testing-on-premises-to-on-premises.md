---
title: "a Hyper-V replikáció az Azure Site Recovery a helyek közötti aaaTest eredmények |} Microsoft Docs"
description: "Ez a cikk tájékoztatást ad azokról a helyszíni tooon helyszíni Hyper-V virtuális gépek replikálását teljesítménytesztelési Azure Site Recovery segítségével."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 96ff404f-0d88-43fa-a00b-2dffde93d192
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: raynew
ms.openlocfilehash: 3b37542fc88e0af05e05cee78183983667618816
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-results-for-on-premises-tooon-premises-hyper-v-replication-with-site-recovery"></a>Teszteredmények tooon helyszíni Hyper-V replikáció Site Recovery szolgáltatással

Microsoft Azure Site Recovery tooorchestrate használja, és a virtuális gépek és fizikai kiszolgálók tooAzure vagy tooa másodlagos adatközpont-replikáció kezelésére. Ez a cikk ismerteti azt hasonló a Hyper-V virtuális gépek replikálása két között a helyszíni adatközpontját a Teljesítménytesztelés hello eredményeit.

## <a name="test-goals"></a>Tesztelési célokat

tesztelés hello célja volt tooexamine hogyan hajt végre Azure Site Recovery stabil állapotot replikáció során. Ha virtuális gépek kezdeti replikáció befejeződött, és szinkronizál a változási különbözeteket stabil állapotot replikáció. Fontos toomeasure teljesítmény stabil állapotot használatával, mert hello állapota, amelyben a legtöbb virtuális gépek továbbra is, kivéve, ha nem várt leállások esetén is.

a VMM-kiszolgáló minden hely két helyszíni hely hello próbatelepítés állt. Ez az üzembe helyezési teszt hello elsődleges hely és a fiókiroda hello hello másodlagos vagy tartalék helyként központi iroda a központi iroda/fiók office központi telepítés, a jellemző jelenti.

## <a name="what-we-did"></a>Azt adta

Mi a Microsoft hello teszt felelt meg az itt található:

1. A létrehozott virtuális gépek VMM-sablonokkal.
2. Virtuális gépek és a készítés referenciakonfiguráció teljesítménymutatók 12 óránál nagyobb elindult.
3. Létrehozott felhők elsődleges és a helyreállítási VMM-kiszolgálókon.
4. Az Azure Site Recovery, beleértve a forrás- és a helyreállítási felhő hozzárendelése konfigurált felhővédelmet.
5. Engedélyezve van a virtuális gépek védelmét, és lehetővé teszik a kezdeti replikálás toocomplete.
6. Néhány óra múlva a rendszer stabilizációs várt.
7. Rögzített teljesítménymutatók 12 óránál nagyobb, győződjön meg arról, hogy az összes virtuális gép ezen 12 óra ki maradt várt replikációs állapotban.
8. Mérték hello különbözeti hello alapterv metrikák és hello replikációs teljesítménymutatók között.


## <a name="primary-server-performance"></a>Elsődleges kiszolgáló teljesítménye

* A Hyper-V replika aszinkron módon módosítások tooa naplófájlt, minimális tárolási terhelés mellett hello elsődleges kiszolgáló nyomon követi.
* A Hyper-V replika használja az önálló karbantartott gyorsítótár toominimize IOPS Memóriaterhelést nyomon követésére. A memória és őket hello naplófájl előtt hello ideje, hogy hello napló kiürítéseinek VHDX küldött toohello helyreállítási hely írások toohello tárol. Kiürítési lemezt is hello írások találati előre meghatározott maximális történik.
* az alábbi hello graph hello stabil állapotot IOPS terhelést jelenthet replikációs mutatja. Láthatja, hogy hello IOPS terhelés miatt tooreplication van körülbelül 5 %, mert túl alacsony.

![Elsődleges eredmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

A Hyper-V replika hello elsődleges kiszolgáló toooptimize lemez teljesítménye memóriáját használja. Ahogy az a következő diagram hello, memóriáját terhet hello elsődleges fürtben lévő mindegyik kiszolgálónak marginális. protokollterhelés látható hello memória hello képest replikációs toohello teljes telepített memória hello Hyper-V kiszolgálón által használt memória aránya.

![Elsődleges eredmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V replika rendelkezik olyan minimális CPU-terhelés. Amint hello graph, replikációs terhelés érték 2-3 % hello esik.

![Elsődleges eredmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

## <a name="secondary-recovery-server-performance"></a>Másodlagos (helyreállítási) kiszolgáló teljesítménye

A Hyper-V replika használja a kis méretű memória hello helyreállítási server toooptimize hello tárolási műveletek száma. hello graph hello memóriahasználat hello helyreállítási kiszolgálón foglalja össze. protokollterhelés látható hello memória hello képest replikációs toohello teljes telepített memória hello Hyper-V kiszolgálón által használt memória aránya.

![Másodlagos eredmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

hello i/o-műveletek hello helyreállítási helyen mérete hello hello elsődleges hely az írási műveletek számának függvényében. Nézzük meg hello teljes írási és olvasási műveletei hello helyreállítási helyen összehasonlítva hello összes i/o-művelet és az írási műveletek hello elsődleges helyen. hello diagramjait megjelenítése, hogy a helyreállítási helyen hello IOPS-érték hello összesen

* Körülbelül 1,5-szerese hello IOPS elsődleges hello írása.
* Körülbelül hello 37 %-os teljes IOPS hello elsődleges helyen.

![Másodlagos eredmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Másodlagos eredmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

## <a name="effect-on-network-utilization"></a>Hálózathasználat hatása

Az átlagos 275 MB / s a hálózati sávszélesség volt használva hello elsődleges és a helyreállítási csomópontok között (tömörítés engedélyezése) 5 Gb / s meglévő sávszélesség ellen.

![Hálózathasználat eredmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

## <a name="effect-on-vm-performance"></a>VM teljesítményre gyakorolt hatás

Egy fontos szempont, hogy a termelési számítási feladatokhoz hello virtuális gépeken futó replikációs hello hatását. Ha hello elsődleges hely képes megfelelően lett beállítva a replikáció, akkor nem adható meg gyakorolt hatás a hello munkaterhelésére. Követési mechanizmus a Hyper-V replika lightweight biztosítja, hogy hello virtuális gépeken futó számítási feladatok nem érintettek stabil állapot replikáció során. Ezt mutatja be a következő diagramjait hello.

Ez a grafikon azt ábrázolja, IOPS végre a különböző terhelésekhez előtt futó virtuális gépek és a replikáció engedélyezése után. Figyelheti, hogy nincs-e két hello közötti különbség.

![A replika hatás eredmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

hello következő grafikon azt ábrázolja, hello átviteli különböző terhelésekhez előtt futó virtuális gépek és a replikáció engedélyezése után. Azt is láthatja, hogy a replikáció nincs jelentős hatással van.

![Eredmények replika hatások](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

## <a name="conclusion"></a>Összegzés

hello eredmények szétválasztásához, hogy Azure Site Recovery szolgáltatásban alapján kialakulhat Hyper-V replika, méretezze át is, legalább egy nagy méretű fürt esetén.  Az Azure Site Recovery egyszerű üzembe helyezés, a replikáció, a kezelési és figyelési biztosít. A Hyper-V replika hello szükséges infrastruktúrát biztosít a sikeres replikáció méretezést. Az optimális központi telepítésének tervezéséhez javasoljuk, hogy hello letöltése [Hyper-V replika Capacity Planner](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Környezet részletei

### <a name="primary-site"></a>Elsődleges hely

* hello elsődleges hely öt Hyper-V rendszert futtató 470 virtuális gépeket tartalmazó fürt rendelkezik.
* hello virtuális gépek különböző alkalmazásokat és szolgáltatásokat futtathatnak, és mindegyik rendelkezik engedélyezett Azure Site Recovery általi védelmet.
* Egy iSCSI SAN által biztosított tárolási hello fürt összes csomópontjára vonatkozóan. Modell – Hitachi HUS130.
* Minden egyes fürtkiszolgálón rendelkezik négy hálózati kártyák (NIC) a egy kapcsolóport.
* Hello hálózati adapterek közül kettő csatlakoztatott tooan iSCSI magánhálózati és két csatlakoztatott tooan külső vállalati hálózatra. Egy hello külső hálózatok csak a fürtkommunikáció számára van fenntartva.

![Elsődleges hardverkövetelmények](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

| Kiszolgáló | RAM | Modell | Processzor | Processzorok száma | A HÁLÓZATI ADAPTER | Szoftver |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V fürtben lévő kiszolgálókat: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128ESTLAB-HOST25 rendelkezik 256 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz |4 |I x 4 kapcsolóport |Windows Server Datacenter 2012 R2 (x64) + Hyper-V szerepkör |
| VMM-kiszolgáló |2 | | |2 |1 Gbps |Windows Server 2012 adatbázis R2 (x 64) + VMM 2012 R2 |

### <a name="secondary-recovery-site"></a>Másodlagos (helyreállítási) helyet

* másodlagos hely hello hat csomópontos feladatátvevő fürt rendelkezik.
* Egy iSCSI SAN által biztosított tárolási hello fürt összes csomópontjára vonatkozóan. Modell – Hitachi HUS130.

![Elsődleges hardver meghatározása](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

| Kiszolgáló | RAM | Modell | Processzor | Processzorok száma | A HÁLÓZATI ADAPTER | Szoftver |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V fürtben lévő kiszolgálókat: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell™ PowerEdge™ R720 |Intel(R) Xeon(R) CPU E5-2630 0 @ 2.30-as GHz |2 |I x 4 kapcsolóport |Windows Server Datacenter 2012 R2 (x64) + Hyper-V szerepkör |
| ESTLAB-HOST17 |128 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V szerepkör |
| ESTLAB-HOST24 |256 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V szerepkör |
| VMM-kiszolgáló |2 | | |2 |1 Gbps |Windows Server 2012 adatbázis R2 (x 64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Kiszolgáló-munkaterhelések

* Tesztelési célokra azt kivételezett gyakori vállalati forgatókönyvet a munkaterhelések.
* Használjuk [IOMeter](http://www.iometer.org) hello munkaterhelés jellemző szimuláció hello táblázat foglalja össze.
* Minden IOMeter profilok toowrite véletlenszerű bájt toosimulate legrosszabb írási minták munkaterhelések vannak beállítva.

| Számítási feladat | I/o-méret (KB) | % Hozzáférés | % Olvasása | Függőben lévő i/o műveletek | I/o-minta |
| --- | --- | --- | --- | --- | --- |
| Fájlkiszolgáló |48163264 |60%20%5%5%10% |80%80%80%80%80% |88888 |Véletlenszerű 100 % |
| SQL Server (1. kötet) SQL Server (kötet 2) |864 |100%100% |70%0% |88 |szekvenciális 100 %-os random100 % |
| Exchange |32 |100% |67% |8 |100 %-os véletlenszerű |
| Munkaállomás/VDI |464 |66%34% |70%95% |11 |Mindkét véletlenszerű 100 %. |
| Webkiszolgáló-fájl |4864 |33%34%33% |95%95%95% |888 |Véletlenszerű 75 % |

### <a name="vm-configuration"></a>Virtuálisgép-konfiguráció

* 470 hello elsődleges fürtön lévő virtuális gépek.
* Minden virtuális gépek VHDX-lemezzel.
* Hello táblázatban összefoglalt szolgáltatásokat futtató virtuális gépekhez. Az összes VMM-sablonok-ekkel hozta létre.

| Számítási feladat | Száma virtuális gépek | Minimális memória (GB) | Maximális memória (GB) | Logikai lemez mérete (GB) virtuális gépenként | Maximális IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Fájlkiszolgáló |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Webkiszolgáló |149 |.5 |1 |80 |6 |
| ÖSSZESEN |470 | | |96.83 TB |4108 |

### <a name="site-recovery-settings"></a>Webhely-helyreállítási beállításai

* Az Azure Site Recovery a helyszíni tooon helyszíni védelem konfigurálása
* hello VMM-kiszolgáló hello Hyper-V fürt kiszolgálók és virtuális gépeik tartalmazó konfigurált, négy felhők rendelkezik.

| Elsődleges VMM-felhő | Védett virtuális gépet hello felhőben | Replikációs gyakoriság | A további helyreállítási pontok |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 perc |None |
| PrimaryCloudRpo30s |47 |30 másodperc |None |
| PrimaryCloudRpo30sArp1 |47 |30 másodperc |1 |
| PrimaryCloudRpo5m |235 |5 perc |None |

### <a name="performance-metrics"></a>Teljesítménymértékeket

hello tábla hello metrikák és hello környezetben volt mért számlálókat foglalja össze.

| Metrika | A számláló |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Rendelkezésre álló memória |\Memory\Available memória (MB) |
| IO |\PhysicalDisk (_Total) \Disk átvitel/mp |
| Virtuális gép olvasási művelet/mp (IOPS) |\Hyper-V virtuális tárolóeszköz (<VHD>) \Read művelet/mp |
| Virtuális gép (IOPS) írási művelet/mp |\Hyper-V virtuális tárolóeszköz (<VHD>) \Write műveletek/mp |
| Olvassa el az átviteli sebesség VM |\Hyper-V virtuális tárolóeszköz (<VHD>) \Read bájtok/s |
| Virtuális gép teljesítménye |\Hyper-V virtuális tárolóeszköz (<VHD>) \Write bájtok/s |

## <a name="next-steps"></a>Következő lépések

[A két helyszíni VMM-helyek közötti replikáció beállítása](site-recovery-vmm-to-vmm.md)
