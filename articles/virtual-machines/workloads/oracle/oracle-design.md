---
title: "Azure-adatbázis aaaDesign és alkalmazzon egy Oracle |} Microsoft Docs"
description: "Tervezése és megvalósítása az Oracle-adatbázishoz az Azure környezetben."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/22/2017
ms.author: rclaus
ms.openlocfilehash: 8fa1207458695df1c7330ec626888b1b6b8d8939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Tervezése és megvalósítása az Oracle-adatbázishoz az Azure-ban

## <a name="assumptions"></a>Előfeltételek

- Arra készül, toomigrate helyszíni tooAzure az Oracle-adatbázishoz.
- Van hello ismeretét különböző metrikák Oracle AWR jelentésekben.
- Rendelkezik egy alapkonfiguráció ismeretekkel az alkalmazások teljesítményének és a platform felhasználását.

## <a name="goals"></a>Célok

- Megértéséhez hogyan toooptimize az Oracle-telepítés az Azure-ban.
- Fedezze fel teljesítményének hangolása beállításait az Azure környezetben Oracle-adatbázishoz.

## <a name="hello-differences-between-an-on-premises-and-azure-implementation"></a>egy helyszíni közötti különbségek hello és Azure végrehajtása 

Az alábbiakban olyan fontos dolgok tookeep szem előtt, amikor, amelybe migrálna a helyszíni alkalmazások tooAzure. 

Egy fontos különbség az, hogy az Azure-megvalósításban erőforrások, például a virtuális gépek, a lemezek és a virtuális hálózatok megosztott egymás között. Ezenkívül erőforrásokat is lehet halmozódni hello követelmények alapján. Helyett összpontosító elkerülése sikertelenek lesznek (MTBF), Azure van, ahol inkább a még működő hello hiba (MTTR).

hello alábbi táblázat néhány hello különbségei egy helyszíni megvalósítás és az Oracle-adatbázishoz Azure megvalósítását.

> 
> |  | **Helyszíni megvalósítás** | **Az Azure végrehajtása** |
> | --- | --- | --- |
> | **Hálózat** |LAN VAGY WAN  |SDN-alapú (szoftveresen meghatározott Hálózatkezelés)|
> | **Biztonsági csoport** |IP-/ port korlátozást eszközök |[Hálózati biztonsági csoport (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Rugalmasság** |MTBF (középidő hibák között) |MTTR (középidő toorecovery)|
> | **Tervezett karbantartás** |Javítás vagy frissítés|[Rendelkezésre állási készletek](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (javítás/frissítések Azure által kezelt) |
> | **Erőforrás** |Dedikált  |A más ügyfelekkel megosztott|
> | **Régiók** |Adatközpontok |[A régióban párok](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | **Tárolás** |SAN vagy fizikai lemezek |[Azure által kezelt tárolási](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Méretezés** |Függőleges méretezés |Horizontális skálázhatóság|


### <a name="requirements"></a>Követelmények

- Hello adatbázis méretére és növekedésére sebesség határozza meg.
- Oracle AWR jelentések vagy más hálózati eszközök figyelése alapján megbecsülheti hello IOPS követelmények meghatározása.

## <a name="configuration-options"></a>Konfigurációs beállítások

Számos négy lehetséges területet, hogy észlelheti a tooimprove teljesítmény az Azure környezetben.

- Virtuális gép mérete
- Hálózati teljesítmény
- Lemez legyen kijelölve, és konfigurációk
- Gyorsítótár-beállítások

### <a name="generate-an-awr-report"></a>Egy AWR jelentés készítése

Ha egy meglévő Oracle-adatbázishoz és toomigrate tooAzure tervez, több lehetőség közül választhat. Hello Oracle AWR jelentés tooget hello metrikák (IOPS, MB/s, GiBs és így tovább) is futtathatja. Válassza ki a virtuális gép összegyűjtött hello mérőszámok alapján hello. Vagy felveheti a kapcsolatot az infrastruktúra csoport tooget hasonló információkat.

Érdemes lehet a AWR jelentés futtatása során rendszeres és a csúcsidőre munkaterhelések esetén, összehasonlítás. Ezek a jelentések alapján, a munkaterhelés átlagos hello vagy hello maximális munkaterhelést alapuló virtuális gépek hello méretét.

Az alábbiakban látható egy példa bemutatja, hogyan toogenerate egy AWR jelentést:

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>Alapvető metrikákat

Az alábbiakban szerezhet be a hello AWR jelentés hello metrikák:

- Magok száma összesen
- Órajelű Processzor
- Teljes memória GB-ban
- CPU-felhasználás
- Maximális adatátviteli sebesség
- I/o-módosításokat (olvasás/írás) aránya
- Végezze el újra napló gyakorisága (MB/s)
- Hálózati teljesítmény
- Hálózati késés gyakorisága (alsó vagy felső)
- Adatbázis méretét GB-ban
- SQL keresztül fogadott bájtok * a nettó / tooclient

### <a name="virtual-machine-size"></a>Virtuális gép mérete

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-hello-awr-report"></a>1. Processzor, memória és I/O használata a hello AWR jelentés alapján becsült Virtuálisgép-méret

Egy dolog, előfordulhat, hogy megnézi hello felső öt időzített előtér jelző események hol vannak a hello rendszer szűk keresztmetszetek.

Például a következő diagram hello, hello napló fájlszinkronizálás van hello tetején. Azt jelzi, hogy hello száma, amelyek szükségesek, mielőtt hello LGWR ír hello napló puffer toohello ismét: naplófájl vár. Ezekkel az eredményekkel jelzi, hogy jobban teljesítő tároló vagy lemezek szükségesek. Emellett hello ábrán is látható hello memóriamennyiség és a Processzor (magok) hello száma.

![Hello AWR jelentés oldalát bemutató képernyőkép](./media/oracle-design/cpu_memory_info.png)

hello alábbi ábrán látható hello teljes i/o-olvasási és írási. Nem voltak 59 olvassa el és 247.3 GB-os írt hello jelentés hello idő alatt.

![Hello AWR jelentés oldalát bemutató képernyőkép](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Válassza ki a virtuális gépek

Begyűjti a hello AWR jelentés hello adatok alapján, hello tovább toochoose igényeknek megfelelő hasonló méretű virtuális gép. Rendelkezésre álló virtuális gépek listájának hello cikkben található [Memóriaoptimalizált](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).

#### <a name="3-fine-tune-hello-vm-sizing-with-a-similar-vm-series-based-on-hello-acu"></a>3. Hello Virtuálisgép-méretezési finomhangolásához hasonló virtuális gép több hello ACU alapján

Hello VM kiválasztása után kell fizetnie figyelmet toohello ACU hello virtuális gép számára. Akkor célszerű használni a különböző virtuális gépek hello igényeinek jobban megfelelő ACU érték alapján. További információkért lásd: [Azure számítási egység](https://docs.microsoft.com/azure/virtual-machines/windows/acu).

![Hello ACU egységek oldalát bemutató képernyőkép](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Hálózati teljesítmény

a következő ábra azt mutatja be hello kapcsolata az átviteli sebesség és IOPS hello:

![Képernyőkép a átviteli sebesség](./media/oracle-design/throughput.png)

hello teljes hálózati teljesítmény becsült hello a következő információk alapján:
- Az SQL * forgalom Net
- MB/s x (például Oracle Data Guard kimenő adatfolyam) kiszolgálók száma
- Más tényezőktől, például az alkalmazás-replikáció

![Képernyőfelvétel a hello SQL * nettó átviteli sebesség](./media/oracle-design/sqlnet_info.png)

A hálózati sávszélesség követelmények alapján, az toochoose a különböző átjáró típusa van. Ezek közé tartozik a basic, VpnGw, és az Azure ExpressRoute. További információkért lásd: hello [árképzést ismertető oldalra VPN-átjáró](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).

**Javaslatok**

- Hálózati késés nagyobb összehasonlított tooan helyszíni telepítés. Hálózati round való adatváltások számát is nagy mértékben csökkenti a teljesítmény javításához.
- tooreduce üzenetváltások utak számát, az alkalmazásokat, amelyek magas tranzakciók, illetve "chatty" alkalmazásokat a hello egyesíteni ugyanahhoz a virtuális géphez.

### <a name="disk-types-and-configurations"></a>Lemez legyen kijelölve, és konfigurációk

- *Az operációs rendszer lemezek alapértelmezett*: ezek lemeztípusokkal ajánlat állandó adatok és a gyorsítótár. Az operációs rendszer hozzáférés indításkor vannak optimalizálva, és nem terveztek vagy tranzakciós vagy az adatraktár (analitikai) munkaterhelések.

- *Nem felügyelt lemezek*: a lemez legyen kijelölve, a tároló hello virtuális merevlemez (VHD) fájlok, amelyek megfelelnek a virtuális gépek lemezei tooyour hello storage-fiókok kezelése. VHD-fájlokat, az Azure storage-fiókok lapblobokat tárolódnak.

- *Által kezelt lemezeken*: Azure kezeli hello storage-fiókok, amelyek a virtuális gép lemezeit használja. Megadhatja, hogy hello lemeztípus (prémium és standard) és a szükséges hello lemez hello mérete. Azure hoz létre, és kezeli a hello lemez meg.

- *Prémium szintű storage lemezek*: A lemez típusok a következők termelési számítási feladatokhoz ideális. Prémium szintű storage támogatja virtuális gépek lemezei használható csatolt toospecific mérete sorozatú virtuális gépeket, például a DS, DSv2, GS és F adatsorozat virtuális gépeket. prémium szintű lemez hello a különböző méretű, és 32 GB-os too4, 096 GB közötti lemezek közül választhat. Minden lemez mérete a saját teljesítmény specifikációi. Az alkalmazás követelményeitől függően egy vagy több lemezt tooyour VM csatolhat.

Egy új felügyelt lemezes hello portálról létrehozásakor választhat hello **fiók típusa** hello típusú lemez toouse keresi. Ne feledje, hogy nem minden rendelkezésre álló lemezek hello legördülő menüben jelennek meg. Egy adott VM-méret kiválasztása után hello menü csak hello érhető el prémium szintű storage, hogy a Virtuálisgép-méretet alapuló termékváltozatok jeleníti meg.

![Hello felügyelt lemezes oldalát bemutató képernyőkép](./media/oracle-design/premium_disk01.png)

További információkért lásd: [prémium szintű Storage nagy teljesítményű és a virtuális gépek felügyelt lemezek](https://docs.microsoft.com/azure/storage/storage-premium-storage).

Miután a virtuális gép tárhelyét konfigurált, érdemes lehet tooload teszt hello lemezek adatbázis létrehozása előtt. A késés célkitűzések átviteli tudatában hello i/o-sebessége késés és a teljesítmény tekintetében segít meghatározni, ha hello virtuális gépek támogatják hello várt.

Számos alkalmazás terhelés tesztelési, például Oracle Orion Sysbench vagy Fio eszközök.

Hello terheléstesztet futtassa újból az Oracle-adatbázis telepítése után. Indítsa el a szabályos és csúcsidőre munkaterhelések, és hello, a környezet eredeti hello eredmények megjelenítése.

Előfordulhat, hogy a fontosabb toosize hello tárolási hello tárméret helyett hello IOPS arány alapján. Például hello 5000 IOPS-érték, de csak akkor 200 GB-os csak szükség esetén továbbra is kaphat hello P30 osztály prémium szintű lemez annak ellenére, hogy a több mint 200 GB tárhelyet származik.

hello IOPS arány hello AWR jelentés lehet lekérni. Azt határozza meg hello visszaállítási napló, fizikai olvasási és írási műveletek sebessége.

![Hello AWR jelentés oldalát bemutató képernyőkép](./media/oracle-design/awr_report.png)

Például hello ismét: mérete 12,200,000 bájt / s, amely egyenlő too11.63 MB/s.
hello IOPS értéke 12,200,000 / 2,358 = 5,174.

Miután világossá hello i/o-követelményeinek, dönthet úgy, amelyek a legjobb olyan környezethez a legalkalmasabb toomeet meghajtók kombinációja ezt a követelményt.

**Javaslatok**

- Az adatok táblaterületen elosztva hello i/o-munkaterhelés lemezek számát felügyelt tároló- és Oracle ASM használatával.
- Hello i/o-blokkméret olvasásigényű és írási-igényes műveletek növekszik, adja hozzá a további adatlemezt.
- Növeli a nagy szekvenciális folyamatok hello blokkméretével.
- Adatok tömörítése tooreduce i/o használja (az adatok és indexek).
- Ismét: naplókat, a rendszer, és a temps külön, és külön adatlemezek a TS visszavonása.
- Ne helyezze el az alapértelmezett operációsrendszer-lemezek (/ dev/sda) alkalmazás fájljai. Ezek a lemezek nem optimalizált gyors virtuális gép rendszerindítási ideje, és előfordulhat, hogy nem adja meg az alkalmazás a megfelelő teljesítmény.

### <a name="disk-cache-settings"></a>Gyorsítótár-beállítások

Az állomás gyorsítótárazását három lehetőség áll rendelkezésre:

- *Csak olvasható*: összes kérelem későbbi olvasása gyorsítótárba kerüljenek-e. Összes írt megmaradnak közvetlenül tooAzure Blob Storage tárolóban.

- *Olvasási és írási*: Ez a "előreolvasási" algoritmus. hello olvas, és írási műveletek jövőbeli olvasása gyorsítótárba kerüljenek-e. Nem-író bejegyzést ír toohello helyi gyorsítótár először maradnak. Az SQL Server írási műveletek megőrzött tooAzure tárolási, mert író használ. Legkisebb mértékű késleltetést hello a lemez könnyű munkaterhelések is tartalmazza.

- *Nincs* (letiltva): Ez a beállítás használatával mellőzhetik hello gyorsítótár. Minden hello adat átvitt toodisk és tooAzure tárolási megőrzött. A metódus által biztosított, akkor hello i/o-igényes munkaterhelések a legmagasabb i/o-sebességet. Figyelembe kell tootake "tranzakciós költségek" is.

**Javaslatok**

toomaximize hello átviteli, azt javasoljuk, hogy a kiindulási pont **nincs** állomás gyorsítótárazását. A prémium szintű Storage, vegye figyelembe, hogy le kell tiltania hello "korlátok", ha fájlrendszer hello hello csatlakoztatja **ReadOnly** vagy **nincs** beállítások. Hello /etc/fstab frissítésfájl hello UUID toohello lemezzel.

További információkért lásd: [prémium szintű Storage Linux virtuális gépek](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).

![Hello felügyelt lemezes oldalát bemutató képernyőkép](./media/oracle-design/premium_disk02.png)

- Az operációs rendszer lemezek, használja az alapértelmezett **olvasási/írási** gyorsítótárazását.
- A rendszer, a TEMP és a visszavonás használja **nincs** a gyorsítótárazáshoz.
- Az adatokat, használja **nincs** a gyorsítótárazáshoz. De ha az adatbázis csak olvasható vagy olvasásigényű, **írásvédett** gyorsítótárazását.

Az adatok lemezre beállításainak mentése után a hello állomás gyorsítótár-beállítást csak leválasztása az operációs rendszer szintű hello hello meghajtót, és majd a módosítása hello elvégzése után csatlakoztassa újra módosítható.


## <a name="security"></a>Biztonság

Után, hogy állítson be és konfigurálta az Azure környezetben, hello tovább toosecure-e a hálózathoz. Az alábbiakban néhány javaslatot:

- *NSG házirend*: NSG definiálni egy alhálózatot vagy egy hálózati adaptert. Hello alhálózat-szintű biztonsági és a kényszerített útválasztást többek között a tűzfalak egyaránt egyszerűbb toocontrol hozzáférést is.

- *Jumpbox*: A biztonságosabb hozzáférés érdekében a rendszergazdák nem közvetlenül csatlakoznia toohello alkalmazásszolgáltatás vagy az adatbázis. Egy media hello rendszergazda számítógép és az Azure-erőforrások egy jumpbox lesz.
![Hello Jumpbox topológia oldalát bemutató képernyőkép](./media/oracle-design/jumpbox.png)

    hello rendszergazda gép IP-korlátozott hozzáférés toohello jumpbox kínálja. hello jumpbox access toohello alkalmazás és az adatbázis kell rendelkeznie.

- *Magánhálózati* (alhálózatok): javasoljuk, hogy külön hello alkalmazásszolgáltatás és az adatbázis külön alhálózatokon, hogy jobb vezérlő NSG házirend állítható be.


## <a name="additional-reading"></a>További olvasnivaló

- [Oracle ASM konfigurálása](configure-oracle-asm.md)
- [Oracle Data Guard konfigurálása](configure-oracle-dataguard.md)
- [Oracle Golden kapu beállítása](configure-oracle-golden-gate.md)
- [Oracle biztonsági mentés és helyreállítás](oracle-backup-recovery.md)

## <a name="next-steps"></a>Következő lépések

- [Oktatóanyag: Hozzon létre magas rendelkezésre állású virtuális gépek](../../linux/create-cli-complete.md)
- [Virtuális gép telepítése az Azure parancssori felület minták felfedezése](../../linux/cli-samples.md)
