---
title: "Az Azure Storage adatreplikáció |} Microsoft Docs"
description: "A Microsoft Azure Storage-fiók adatait a tartósság és magas rendelkezésre állású replikálja a rendszer. Replikációs beállítások közé tartozik a helyileg redundáns tárolás (LRS), a zónaredundáns tárolás (ZRS), a georedundáns tárolás (GRS) és az írásvédett georedundáns tárolás (RA-GRS)."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 86bdb6d4-da59-4337-8375-2527b6bdf73f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: b9354484ff5b81e2561d017d039bf2c07a21a423
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-storage-replication"></a>Azure Storage replication (Azure Storage replikáció)

A Microsoft Azure tárfiók tartalmát mindig replikáljuk, így biztosítva az adatok tartósságát és magas rendelkezésre állását. A replikáció akár egyazon adatközponton belül, akár egy második adatközpontban készít másolatot az adatokról, a replikáció beállítástól függően. A replikáció óvja az adatokat és biztosítja az alkalmazás üzemidejét egy átmeneti hardverhiba esetén. Ha az adatok egy második adatközpontba van replikálva, az védett végzetes hibák esetén az elsődleges helyen.

A replikáció biztosítja, hogy a tárfiók még hibák esetén is teljesíti a [Storage szolgáltatói szerződés (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) feltételeit. Tekintse át az Azure Storage tartóssági és rendelkezésre állási garanciáit a szolgáltatói szerződésben .

Tárfiók létrehozásakor választhat a következő replikációs lehetőségek közül:

* [Helyileg redundáns tárolás (LRS)](#locally-redundant-storage)
* [Zónaredundáns tárolás (ZRS)](#zone-redundant-storage)
* [Georedundáns tárolás (GRS)](#geo-redundant-storage)
* [Írásvédett georedundáns tárolás (RA-GRS)](#read-access-geo-redundant-storage)

Írásvédett georedundáns tárolás (RA-GRS) beállítás az alapértelmezett tárfiók létrehozásakor.

A következő táblázat LRS, a zrs-t, a GRS és az RA-GRS, közötti különbségek gyors áttekintést nyújt az ezt követő szakaszok részletesebben replikációs különböző típusú célja.

| Replikációs stratégia | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Több adatközpont között replikálva. |Nem |Igen |Igen |Igen |
| A másodlagos helyre, valamint az elsődleges helyre is lehet adatokat olvasni. |Nem |Nem |Nem |Igen |
| A külön csomópontokon fenntartott adatmásolatok száma. |3 |3 |6 |6 |

Lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/) a díjszabásról különböző redundancia beállításokkal.

> [!NOTE]
> Prémium szintű Storage támogatja a csak a helyileg redundáns tárolás (LRS). Prémium szintű Storage kapcsolatos információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](storage-premium-storage.md).
>

> [!NOTE]
> Az Azure File storage támogatja a csak a helyileg redundáns tárolás (LRS) és georedundant tárolás (GRS). További információ az Azure File Storage: [Azure File storage áttekintése](storage-files-introduction.md).
>

## <a name="locally-redundant-storage"></a>Helyileg redundáns tárolás
Helyileg redundáns tárolás (LRS) az adatok egy tárolási méretezési egység, amely a régióban, amelyben létrehozta a tárfiók adatközpontban található belül háromszor replikálja. Az írási kérelem sikeresen beolvasása csak egyszer minden három replikákra lett írva. Ezek a három replikák külön tartalék tartományok és egy tárolási méretezési egység frissítési tartományban találhatók.

A tárolási méretezési egység tárolócsomópontok rackszekrények gyűjteménye. A tartalék tartomány (FD) olyan csomóponton, amelyeket egy fizikai egységet kialakulását jelöl, és az ugyanazon fizikai állványra tartozó számítógépen tekinthető. Frissítési tartományok (UD) olyan csomóponton, amelyeket a szolgáltatás frissítése (Bevezetés) folyamata során egyszerre frissítik. A három replikák vannak elosztva UDs és FDs belül egy tárolási méretezési egység győződjön meg arról, hogy érhetők el adatok akkor is, ha hardverhiba egyetlen állvány hatással van, vagy ha a csomópont frissítése a bevezetés alatt.

LRS a legalacsonyabb költséget beállítás, és más beállítások képest legalább tartósságot biztosít. (Fire, elárasztás stb) datacenter szintű katasztrófa esetén minden három replikák elveszett vagy helyreállíthatatlan lehet. Ennek a kockázatnak a mérséklése érdekében a legtöbb alkalmazás földrajzi redundáns tárolás (GRS) ajánlott.

Helyileg redundáns tárolás kívánatos bizonyos esetekben továbbra is lehet:

* Itt a legnagyobb maximális sávszélesség az Azure Storage replikálási beállításaival.
* Ha az alkalmazás, amely könnyen rekonstruálható adatokat tárol, LRS is választhat.
* Adatreplikálás adatok cégirányítási követelmények miatt országon belül csak bizonyos alkalmazások korlátozódnak. Egy párosított régióban lehet egy másik országból. A régió párok további információkért lásd: [Azure-régiók](https://azure.microsoft.com/regions/).

## <a name="zone-redundant-storage"></a>Zónaredundáns tárolás
Zónaredundáns tárolás (ZRS) az adatok tárolása három hasonló LRS, így biztosítva az LRS-nél nagyobb tartósságot biztosít replikák mellett egy vagy két régiókban üzemeltetésében aszinkron módon replikálja. A ZRS tárolt adatok állandó akkor is, ha az elsődleges adatközpont nem érhető el vagy helyreállíthatatlan.
Az ügyfelek, akik tervezi használni a zrs-t érdemes figyelembe venni, hogy:

* A ZRS csak a blokkblobokhoz az általános célú tárfiókok esetében érhető el, és csak a tárolási szolgáltatásverziók 2014-02-14-es és újabb.
* Mivel a aszinkron replikációs késés, helyi katasztrófa esetén is lehetséges, hogy még nem replikálódott a másodlagos változtatások elvesznek az adatokat nem lehet helyreállítani az elsődleges.
* A replika nem érhetők el addig, amíg a Microsoft kezdeményezi a másodlagos.
* A ZRS fiókok nem konvertálható később LRS- vagy GRS. Hasonlóképpen meglévő LRS- vagy GRS fiók nem alakítható át a ZRS fiók.
* A ZRS fiókok nem rendelkeznek, metrikákat és naplózási képesség.

## <a name="geo-redundant-storage"></a>Georedundáns tárolás
Georedundáns tárolás (GRS) replikálja az adatokat egy másodlagos régióban, amelyek több száz miles elhagyja az elsődleges régióban van. Ha a tárfiók Georedundáns engedélyezve van, az adatok akkor tartós teljes regionális kimaradás vagy az elsődleges régióban nincs helyreállítható katasztrófa esetén is.

A GRS engedélyezve van a tárfiókon egy frissítés először elkötelezte magát az elsődleges régióban, ahol azt rendszer háromszor replikálja. Majd a frissítés a rendszer replikálja aszinkron módon a másodlagos régióban, ahol azt az háromszor replikálja.

A GRS az elsődleges és másodlagos régióban külön tartalék tartományok közötti replikák kezelése, és frissítse a tárolási méretezési egység LRS leírt tartományban.

Szempontok:

* Mivel a aszinkron replikációs késés, regionális katasztrófa esetén is lehetséges, hogy még nem replikálódott a másodlagos régióba módosítások elvesznek az adatokat nem lehet helyreállítani az elsődleges régióban.
* A replika nem érhető el, kivéve, ha a Microsoft kezdeményezi a másodlagos régióba. Ha a Microsoft kezdeményezzen feladatátvételt a másodlagos régióban, akkor lesz olvasási és írási engedéllyel a feladatátvételt követően befejeződött. További információkért lásd: [vész-helyreállítási útmutatást](storage-disaster-recovery-guidance.md). 
* Ha egy alkalmazás szeretné a másodlagos régióba olvasni, a felhasználó engedélyeznie kell a RA-GRS.

A storage-fiók létrehozásakor ki kell választania a fiók az elsődleges régióban. A másodlagos régióba az elsődleges régió alapján történik, és nem módosítható. A következő táblázat az elsődleges és másodlagos régióban párosítása.

| Elsődleges | Másodlagos |
| --- | --- |
| USA északi középső régiója |USA déli középső régiója |
| USA déli középső régiója |USA északi középső régiója |
| USA keleti régiója |USA nyugati régiója |
| USA nyugati régiója |USA keleti régiója |
| USA 2. keleti régiója |USA középső régiója |
| USA középső régiója |USA 2. keleti régiója |
| Észak-Európa |Nyugat-Európa |
| Nyugat-Európa |Észak-Európa |
| Délkelet-Ázsia |Kelet-Ázsia |
| Kelet-Ázsia |Délkelet-Ázsia |
| Kelet-Kína |Észak-Kína |
| Észak-Kína |Kelet-Kína |
| Kelet-Japán |Nyugat-Japán |
| Nyugat-Japán |Kelet-Japán |
| Dél-Brazília |USA déli középső régiója |
| Kelet-Ausztrália |Délkelet-Ausztrália |
| Délkelet-Ausztrália |Kelet-Ausztrália |
| Dél-India |Közép-India |
| Közép-India |Dél-India |
| Nyugat-India |Dél-India |
| USA-beli államigazgatás – Iowa |USA-beli államigazgatás – Virginia |
| USA-beli államigazgatás – Virginia |USA-beli államigazgatás – Texas |
| USA-beli államigazgatás – Texas |USA-beli államigazgatás – Arizona |
| USA-beli államigazgatás – Arizona |USA-beli államigazgatás – Texas |
| Közép-Kanada |Kelet-Kanada |
| Kelet-Kanada |Közép-Kanada |
| Az Egyesült Királyság nyugati régiója |Az Egyesült Királyság déli régiója |
| Az Egyesült Királyság déli régiója |Az Egyesült Királyság nyugati régiója |
| Közép-Németország |Északkelet-Németország |
| Északkelet-Németország |Közép-Németország |
| USA nyugati régiója, 2. |USA nyugati középső régiója |
| USA nyugati középső régiója |USA nyugati régiója, 2. |

Használható az Azure-régiók naprakész információkat, lásd: [Azure-régiók](https://azure.microsoft.com/regions/).

>[!NOTE]  
> USA – (kormányzati) Virginia másodlagos régióba Velünk – (kormányzati) Texas. Velünk – (kormányzati) Virginia korábban, mint egy másodlagos régióban be a US – (kormányzati) Iowa. Tárfiókok továbbra is használhatja a US – (kormányzati) Iowa egy másodlagos régióban, egy másodlagos régióban, a US – (kormányzati) Texas is áttelepíti. 
> 
> 

## <a name="read-access-geo-redundant-storage"></a>Írásvédett georedundáns tárolás
Írásvédett georedundáns tárolás (RA-GRS) rendelkezésre állási a tárfiók maximalizálja a csak olvasási hozzáféréssel a másodlagos helyen mellett két régióban is GRS biztosítja a replikációs adatok megadásával.

Csak olvasási hozzáféréssel a másodlagos régióba adatai engedélyezésekor a másodlagos végponti, a tárfiók elsődleges végpont mellett az adatok érhető el. A másodlagos végponti az elsődleges végpont hasonló, de a utótag `–secondary` fióknevet. Például, ha az elsődleges végpont a Blob szolgáltatás `myaccount.blob.core.windows.net`, akkor a másodlagos végponti `myaccount-secondary.blob.core.windows.net`. A tárfiók hozzáférési kulcsainak megegyeznek az elsődleges és másodlagos végpontok.

Szempontok:

* Az alkalmazás melyik végponthoz az RA-GRS használatakor dolgozik kezelésére van.
* Mivel a aszinkron replikációs késés, regionális katasztrófa esetén is lehetséges, hogy még nem replikálódott a másodlagos régióba módosítások elvesznek az adatokat nem lehet helyreállítani az elsődleges régióban.
* Ha a Microsoft a másodlagos régióba történő feladatátvételt kezdeményez, hogy fog rendelkezik olvasási és írási engedéllyel a feladatátvételt követően befejeződött. További információkért lásd: [vész-helyreállítási útmutatást](storage-disaster-recovery-guidance.md). 
* RA-GRS magas rendelkezésre állású célokra szolgál. Méretezhetőség útmutatásért tekintse át a [teljesítmény ellenőrzőlista](storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Gyakori kérdések

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Hogyan lehet módosítani a georedundáns replikáció típusa storage-fiókom?

   A földrajzi replikációs típusát a tárfiókja közötti LRS, GRS és RA-GRS használatával módosíthatja a [Azure-portálon](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md) vagy programozott módon, egyet a sok Storage Ügyfélkódtáraival. Vegye figyelembe, hogy a ZRS fiókok konvertált LRS- vagy GRS nem lehet. Hasonlóképpen meglévő LRS- vagy GRS fiók nem alakítható át a ZRS fiók.

<a id="changedowntime"></a>
#### <a name="2-will-there-be-any-down-time-if-i-change-the-replication-type-of-my-storage-account"></a>2. Lesz-e bármely állásidő storage-fiókom replikációs típusú módosításakor?

   Nem, akkor nem adható meg bármelyik állásidő.

<a id="changecost"></a>
#### <a name="3-will-there-be-any-additional-cost-if-i-change-the-replication-type-of-my-storage-account"></a>3. Lesz-e minden további költség nélkül Ha a replikáció típusát storage-fiókom?

   Igen. Ha módosítja az LRS tárfiók grs-re (vagy RA-GRS), azt volna fel Önnek egy, a kimenő forgalom meglévő adatok másolása az elsődleges helyről másodlagos helyre részt a kell külön fizetni. Miután a kezdeti adatok másolásakor használata az adatokat az elsődleges másodlagos helyre történő replikálása földrajzi további további kilépő díjmentes. A sávszélesség-költségek részletes található meg a [Azure Storage szolgáltatás díjszabása lap](https://azure.microsoft.com/pricing/details/storage/blobs/). Ha átvált a Georedundáns LRS, további költség nélkül van, de az adatok törli a másodlagos helyről.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Hogyan RA-GRS segítik számomra?
   
   Georedundáns tárolás biztosítja az adatok egy másodlagos régióban, amely több száz elhagyja az elsődleges régióban miles egy elsődleges replikáció. Ebben az esetben az adatok tartósságát teljes regionális kimaradás vagy az elsődleges régióban nincs helyreállítható katasztrófa esetén is. RA-GRS-tároló tartalmazza ezt, és hozzáadja az adatokat a másodlagos helyről olvasás. Hogyan használhatók ki ez a lehetőség ötleteket, olvassa el [tervezése magas rendelkezésre álló alkalmazások RA-GRS-tárolót](storage-designing-ha-apps-with-ragrs.md). 

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-for-me-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Van mód a annak eldöntésére, hogy mennyi ideig tart a másodlagos régióba replikálja a az adatokat az elsődleges?
   
   Ha RA-GRS tárhelyet használ, ellenőrizheti a legutóbbi szinkronizálásának időpontja a tárfiók. Utolsó szinkronizálás GMT dátum/idő értéket; minden elsődleges írás előtt a legutóbbi szinkronizálás ideje sikeresen elkészült a másodlagos helyre, mely közepét érhetők el a másodlagos helyről kell olvasni. Elsődleges írása után utolsó szinkronizálásának időpontja is, vagy nem érhető el az olvasási műveletek még. Az érték használatával kérdezheti a [Azure-portálon](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), vagy programozott módon, a REST API-t vagy a Storage Ügyfélkódtáraival egyikét. 

<a id="outage"></a>
#### <a name="6-how-can-i-switch-to-the-secondary-region-if-there-is-an-outage-in-the-primary-region"></a>6. Hogyan lehet átváltani a másodlagos régióba. Ha az elsődleges régióban kimaradás?
   
   Tekintse meg a cikk [Mi a teendő, ha egy Azure Storage esetleges leálláskor](storage-disaster-recovery-guidance.md) további részleteket.

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. Mi az a helyreállítási Időkorlát és a GRS RTO?
   
   Helyreállítás időkorlát (RPO): Georedundáns és RA-GRS, a tárolás szolgáltatás aszinkron módon földrajzi-replikálja az adatokat a másodlagos hely az elsődleges. Ha nincs a regionális jelentős katasztrófa következik és a feladatátvétel végrehajtását, majd legutóbbi a változási különbözeteket, amelyek még nincsenek georeplikált elveszhetnek. A lehetséges elveszett adatok percet a helyreállítási Időkorlát (azaz a pont, amelyre az adatok helyreállíthatók időben) hivatkozunk. Általában van egy Helyreállítási kevesebb mint 15 percet, noha jelenleg nincs SLA-t a georeplikáció mennyi ideig tart.

   A helyreállítási idő célkitűzése (RTO): Ez az méri, hogy mennyi ideig tart, hajtsa végre a feladatátvételt, és a tárfiók újra online Ha feladatátvétellel kell. Az idő a feladatátvétel elvégzéséhez a következőket tartalmazza:
    * A szükséges időt, hogy vizsgálja meg, és határozza meg, ha azt állíthatja helyre az adatait az elsődleges helyen, vagy ha igazolnia kell a feladatátvétellel.
    * A fiók az elsődleges DNS-bejegyzések módosításával feladatátvételt a másodlagos helyére mutasson.

   Az adatok nagyon súlyos megőrzi az, ha az adatok helyreállítását minden alkalommal, rendszer kis türelmet a feladatátvétel és az elsődleges helyen az adatok helyreállítását összpontosítani felelőssége vesszük. A jövőben tervezzük az API-t indítás, feladatátvétel, amely lehetővé teszi a majd vezérlését a RTO saját magának, fiók szintjén is biztosítanak, de ez még nem érhető el.
   
## <a name="next-steps"></a>Következő lépések
* [RA-GRS-tárolót magas rendelkezésre állású alkalmazások tervezése](storage-designing-ha-apps-with-ragrs.md)
* [Az Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/)
* [Az Azure storage-fiókokról](storage-create-storage-account.md)
* [Az Azure Storage méretezhetőségi és Teljesítménycélok](storage-scalability-targets.md)
* [A Microsoft Azure tárolás redundancia beállítások és olvasási hozzáférést földrajzi redundáns tárolás](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP Paper - Azure Storage: Egy magas rendelkezésre állású felhőalapú tárolási szolgáltatásba erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

