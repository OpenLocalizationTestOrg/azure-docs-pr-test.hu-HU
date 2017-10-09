---
title: "az Azure Storage aaaData replikációs |} Microsoft Docs"
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
ms.openlocfilehash: ce48d1992fec7bd5ed32feb96b86bef88ca759df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-replication"></a>Azure Storage replication (Azure Storage replikáció)

hello adatokat a Microsoft Azure storage-fiók mindig van replikálva tooensure tartósság és magas rendelkezésre állású. Replikációs másolja az adatokat, vagy a belül hello ugyanabban az adatközpontban, illetve tooa második adatközpont, attól függően, hogy a replikációs beállítás választja. Replikációs védi az adatokat, és megőrzi az alkalmazás üzemidő hello eseményben az átmeneti hardverhibák esetén. Ha az adatok replikált tooa második adatközpont, az elsődleges helyen hello végzetes hibák esetén védett.

Replikációs biztosítja, hogy a tárfiók megfelel-e a hello [szolgáltatásiszint-szerződés (SLA) a tároláshoz](https://azure.microsoft.com/support/legal/sla/storage/) még akkor is, hello tapasztalt hibák. Tekintse meg az Azure Storage információt SLA hello biztosítja, hogy a tartósság és a rendelkezésre állási.

Amikor létrehoz egy tárfiókot, hello a következő replikációs lehetőségek közül választhat:

* [Helyileg redundáns tárolás (LRS)](#locally-redundant-storage)
* [Zónaredundáns tárolás (ZRS)](#zone-redundant-storage)
* [Georedundáns tárolás (GRS)](#geo-redundant-storage)
* [Írásvédett georedundáns tárolás (RA-GRS)](#read-access-geo-redundant-storage)

Írásvédett georedundáns tárolás (RA-GRS) hello alapértelmezett beállítás esetén hozzon létre egy tárfiókot.

a következő táblázat hello gyors áttekintést nyújt az LRS, a zrs-t, a Georedundáns és az RA-GRS, hello különbségei amíg ezt követő szakaszok címtípus minden replikációs részletesebben.

| Replikációs stratégia | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Több adatközpont között replikálva. |Nem |Igen |Igen |Igen |
| Egy másodlagos helyre, valamint a hello elsődleges helyet is lehet adatokat olvasni. |Nem |Nem |Nem |Igen |
| A külön csomópontokon fenntartott adatmásolatok száma. |3 |3 |6 |6 |

Lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/) a díjszabásról a hello különböző redundancia lehetőségeket.

> [!NOTE]
> Prémium szintű Storage támogatja a csak a helyileg redundáns tárolás (LRS). Prémium szintű Storage kapcsolatos információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../storage-premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Helyileg redundáns tárolás
Helyileg redundáns tárolás (LRS) az adatok egy tárolási méretezési egység, amely hello régióban, amelyben létrehozta a tárfiók adatközpontban található belül háromszor replikálja. Az írási kérelem sikeresen függvény csak akkor, ha le van írva tooall három replikák. Ezek a három replikák külön tartalék tartományok és egy tárolási méretezési egység frissítési tartományban találhatók.

A tárolási méretezési egység tárolócsomópontok rackszekrények gyűjteménye. A tartalék tartomány (FD) olyan csomóponton, amelyeket felel meg a sikertelenség fizikai egység és toohello tartozó számítógépen tekinthető ugyanazon fizikai állvány. Frissítési tartományok (UD) olyan csomópontok, amelyek együtt megtörténik a szolgáltatás frissítési (Bevezetés) hello folyamat során. hello három replikák vannak elosztva UDs és FDs belül egy tárolási méretezési egység tooensure, hogy érhetők el adatok akkor is, ha hardverhiba egyetlen állvány hatással van, vagy ha a csomópont frissítése a bevezetés alatt.

LRS hello legalacsonyabb költséget beállítás, és olyan legkevesebb tartóssága képest tooother lehetőségeket kínál. A datacenter szintű katasztrófa (fire, elárasztás stb) hello esemény összes három replika elveszett vagy helyreállíthatatlan lehet. Földrajzi redundáns tárolás (GRS) ajánlott toomitigate ez kockázatát, a legtöbb alkalmazás.

Helyileg redundáns tárolás kívánatos bizonyos esetekben továbbra is lehet:

* Itt a legnagyobb maximális sávszélesség az Azure Storage replikálási beállításaival.
* Ha az alkalmazás, amely könnyen rekonstruálható adatokat tárol, LRS is választhat.
* Egyes alkalmazások korlátozott tooreplicating adatok csak a toodata cégirányítási követelmények miatt egy országon belül. Egy párosított régióban lehet egy másik országból. A régió párok további információkért lásd: [Azure-régiók](https://azure.microsoft.com/regions/).

## <a name="zone-redundant-storage"></a>Zónaredundáns tárolás
Zónaredundáns tárolás (ZRS) az adatok hozzáadása toostoring három replikák hasonló tooLRS, így biztosítva az LRS-nél nagyobb tartósságot biztosít az egy vagy két régiókban üzemeltetésében aszinkron módon replikálja. A ZRS tárolt adatok állandó akkor is, ha hello elsődleges adatközpont nem érhető el vagy helyreállíthatatlan.
Az ügyfelek, akik megtervezése toouse zrs-t érdemes figyelembe venni, hogy:

* A ZRS csak a blokkblobokhoz az általános célú tárfiókok esetében érhető el, és csak a tárolási szolgáltatásverziók 2014-02-14-es és újabb.
* Mivel a aszinkron replikációs késés, hello esemény van helyi katasztrófa, akkor lehetséges, hogy a módosítások, amelyek még nincsenek replikálása másodlagos toohello elvesznek hello adatokat nem lehet helyreállítani az elsődleges hello.
* hello replika előfordulhat, hogy nem lesz elérhető a Microsoft feladatátvevő toohello másodlagos kezdeményezi.
* A ZRS fiókokat nem lehet konvertálni, később tooLRS vagy GRS. Hasonlóképpen, egy meglévő LRS- vagy GRS fiókot nem lehet konvertálni tooa ZRS fiók.
* A ZRS fiókok nem rendelkeznek, metrikákat és naplózási képesség.

## <a name="geo-redundant-storage"></a>Georedundáns tárolás
Georedundáns tárolás (GRS) replikálja a tooa másodlagos adatterületen, amely több száz miles hello elsődleges régió befejeződött. Ha a tárfiók Georedundáns engedélyezve van, az adatok tartós, még akkor is hello esetekben egy teljes regionális kimaradás vagy egy olyan vészhelyzet esetén milyen hello elsődleges régió nincs helyreállítható.

A tárfiók a GRS engedélyezve egy frissítésre első véglegesített toohello elsődleges régió, ahol azt rendszer háromszor replikálja. Hello frissítés replikálása aszinkron módon történik, majd toohello másodlagos régióban, ahol azt az háromszor replikálja.

A GRS mindkét hello elsődleges és másodlagos régióban külön tartalék tartományok közötti replikák kezelése, és frissítse a tárolási méretezési egység LRS leírt tartományban.

Szempontok:

* Mivel a aszinkron replikációs késés, lehetséges, hogy módosítja, amely még nem replikálódott regionális katasztrófa hello esemény toohello másodlagos régióba elvesznek hello adatok nem állíthatók hello elsődleges régióban.
* hello replika nem érhető el, kivéve, ha a Microsoft feladatátvevő toohello másodlagos régióba kezdeményezi. Ha Microsoft feladatátvevő toohello másodlagos régióba kezdeményezni, akkor olvasási és írási hozzáférés toothat adatok, hello feladatátvételi befejeződése után. További információkért lásd: [vész-helyreállítási útmutatást](../storage-disaster-recovery-guidance.md). 
* Ha egy alkalmazás tooread hello másodlagos régióban, hello felhasználói engedélyezze az RA-GRS.

A storage-fiók létrehozásakor ki kell választania hello elsődleges régió hello fiók. hello másodlagos régióba hello elsődleges régió alapján történik, és nem módosítható. a következő táblázat hello hello elsődleges és másodlagos régióban párosítása jeleníti meg.

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
> USA – (kormányzati) Virginia másodlagos régióba Velünk – (kormányzati) Texas. Velünk – (kormányzati) Virginia korábban, mint egy másodlagos régióban be a US – (kormányzati) Iowa. Storage-fiókok Velünk – (kormányzati) Iowa továbbra is használhatja, mert folyamatban van a másodlagos régióba tooUS – (kormányzati) Texas át lesznek-e egy másodlagos régióban. 
> 
> 

## <a name="read-access-geo-redundant-storage"></a>Írásvédett georedundáns tárolás
Írásvédett georedundáns tárolás (RA-GRS) rendelkezésre állási a tárfiók, adja meg a csak olvasási hozzáféréssel toohello adatok hello másodlagos helyen, továbbá toohello replikációs két régióban GRS biztosítja a lehető legnagyobbra növeli.

Ha engedélyezi a csak olvasási hozzáféréssel tooyour adatok hello másodlagos régióban, az adatok nem áll rendelkezésre a másodlagos végponti, továbbá toohello elsődleges végpont a tárfiók. hello másodlagos végponti hasonló toohello elsődleges végpontja, de hello utótag `–secondary` toohello fióknevet. Például, ha az elsődleges végpont a Blob szolgáltatás hello van `myaccount.blob.core.windows.net`, akkor a másodlagos végponti `myaccount-secondary.blob.core.windows.net`. a tárfiók elérési kulcsainak hello vannak hello azonos az elsődleges és másodlagos végpontok hello is.

Szempontok:

* Az alkalmazás rendelkezik toomanage melyik végponthoz az RA-GRS használatakor dolgozik.
* Mivel a aszinkron replikációs késés, lehetséges, hogy módosítja, amely még nem replikálódott regionális katasztrófa hello esemény toohello másodlagos régióba elvesznek hello adatok nem állíthatók hello elsődleges régióban.
* Ha a Microsoft feladatátvevő toohello másodlagos régióba kezdeményez, hogy olvasási és írási hozzáférés toothat adatok hello feladatátvételi befejeződése után. További információkért lásd: [vész-helyreállítási útmutatást](../storage-disaster-recovery-guidance.md). 
* RA-GRS magas rendelkezésre állású célokra szolgál. Méretezhetőség útmutatásért tekintse át az hello [teljesítmény ellenőrzőlista](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Gyakori kérdések

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-hello-geo-replication-type-of-my-storage-account"></a>1. Hogyan módosítható hello georedundáns replikáció típusú storage-fiókom?

   Hello georedundáns replikáció típusát a tárfiókja LRS, GRS között módosíthatja, és az RA-GRS használatával hello [Azure-portálon](https://portal.azure.com/), [Azure Powershell](storage-powershell-guide-full.md) vagy programozott módon az számos ügyfél-tároló Szalagtárak. Vegye figyelembe, hogy a ZRS fiókok konvertált LRS- vagy GRS nem lehet. Hasonlóképpen, egy meglévő LRS- vagy GRS fiókot nem lehet konvertálni tooa ZRS fiók.

<a id="changedowntime"></a>
#### <a name="2-will-there-be-any-down-time-if-i-change-hello-replication-type-of-my-storage-account"></a>2. Lesz-e bármely állásidő Ha hello replikációs típusú storage-fiókom?

   Nem, akkor nem adható meg bármelyik állásidő.

<a id="changecost"></a>
#### <a name="3-will-there-be-any-additional-cost-if-i-change-hello-replication-type-of-my-storage-account"></a>3. Lesz-e bármilyen további költség nélkül Ha hello replikációs típusú storage-fiókom?

   Igen. LRS tooGRS (vagy RA-GRS) a tárfiók vált, ha azt szeretné fel Önnek egy a meglévő adatok másolása az elsődleges hely toohello másodlagos helyről részt kilépő kell külön fizetni. Miután hello kezdeti adatok másolásakor nincs további további kilépő díjat földrajzi hello adatreplikálás hello toosecondary elsődleges helyről. sávszélesség-költségek hello részleteit itt talál hello [Azure Storage szolgáltatás díjszabása lap](https://azure.microsoft.com/pricing/details/storage/blobs/). Georedundáns tooLRS vált, ha további költség nélkül van, de az adatok törlődnek hello másodlagos helyről.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Hogyan RA-GRS segítik számomra?
   
   Georedundáns tárolás biztosít egy elsődleges tooa másodlagos régióban, amely több száz miles hello elsődleges régió befejeződött az adatok replikálását. Ilyen esetben az adatok tartós, még akkor is hello esetekben egy teljes regionális kimaradás vagy egy olyan vészhelyzet esetén milyen hello elsődleges régió nincs helyreállítható. RA-GRS-tároló tartalmazza ezt, valamint hozzáadja hello képességét tooread hello adatok hello másodlagos helyről. Az egyes ötleteit tooleverage ez módjában álljon, tekintse meg a túl[tervezése magas rendelkezésre álló alkalmazások RA-GRS-tárolót](../storage-designing-ha-apps-with-ragrs.md). 

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-for-me-toofigure-out-how-long-it-takes-tooreplicate-my-data-from-hello-primary-toohello-secondary-region"></a>5. Van mód a számomra ki, hogy mennyi ideig tart tooreplicate adataimat hello elsődleges toohello másodlagos régióban toofigure?
   
   Ha az RA-GRS tárhelyet használ, ellenőrizheti a hello utolsó szinkronizálásának időpontja a tárfiók. Utolsó szinkronizálás GMT dátum/idő értéket; minden elsődleges írás előtt hello utolsó szinkronizálás sikeresen készült toohello másodlagos helyre, amelyek a-e elérhető toobe olvasni hello másodlagos helyén. Elsődleges ír hello után utolsó szinkronizálás is, vagy nem érhető el az olvasási műveletek még. Ez az érték hello segítségével lekérheti [Azure-portálon](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), vagy a REST API vagy egy arról hello Storage Ügyfélkódtáraival programozott módon hello. 

<a id="outage"></a>
#### <a name="6-how-can-i-switch-toohello-secondary-region-if-there-is-an-outage-in-hello-primary-region"></a>6. Hogyan megváltoztathatom toohello másodlagos régióban, ha kimaradás hello elsődleges régióban?
   
   Tekintse meg a toohello cikk [egy Azure Storage tervezett kimaradás esetén milyen toodo](../storage-disaster-recovery-guidance.md) további részleteket.

<a id="rpo-rto"></a>
#### <a name="7-what-is-hello-rpo-and-rto-with-grs"></a>7. Mi van hello RPO és a GRS RTO?
   
   Helyreállítás időkorlát (RPO): A GRS és RA-GRS hello tárolási szolgáltatásadatok aszinkron módon földrajzi-eljut hello hello elsődleges toohello másodlagos helyről. Ha nincs a regionális jelentős katasztrófa következik és a feladatátvétel végre toobe, majd legutóbbi a változási különbözeteket, amelyek még nincsenek georeplikált elveszhetnek. hello hány perc lehetséges elveszett adatok hivatkozott tooas hello helyreállítási Időkorlát (azaz hello pont idő toowhich adatok állíthatók helyre). Általában van egy Helyreállítási kevesebb mint 15 percet, noha jelenleg nincs SLA-t a georeplikáció mennyi ideig tart.

   A helyreállítási idő célkitűzése (RTO): Ez az mennyi ideig azt toodo hello feladatátvételi vesz igénybe, és vissza hello tárfiók online toodo feladatátvevő akkor, ha egy mértéket. hello idő toodo hello feladatátvételi hello következőket tartalmazza:
    * hello időt vesz igénybe tooinvestigate, és határozza meg, ha azt állíthatja helyre hello adatait hello elsődleges helyen, vagy ha igazolnia kell, hogy a feladatátvétel toodo.
    * Feladatátvételi hello fiók hello elsődleges DNS bejegyzés toopoint toohello másodlagos helye módosításával.

   Az adatok nagyon súlyos megőrzi az, ha minden alkalommal hello adatok helyreállítását, rendszer kis türelmet hello feladatátvétel és hello elsődleges helyen hello adatok helyreállítása összpontosítani hello felelőssége vesszük. A jövőbeli hello, hogy terv tooprovide az API-k tooallow meg tootrigger fiók szintjén feladatátvevő, amelyek ezután lehetővé teszi toocontrol hello RTO magát, de ez még nem érhető el.
   
## <a name="next-steps"></a>Következő lépések
* [RA-GRS-tárolót magas rendelkezésre állású alkalmazások tervezése](../storage-designing-ha-apps-with-ragrs.md)
* [Az Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/)
* [Az Azure storage-fiókokról](../storage-create-storage-account.md)
* [Az Azure Storage méretezhetőségi és Teljesítménycélok](storage-scalability-targets.md)
* [A Microsoft Azure tárolás redundancia beállítások és olvasási hozzáférést földrajzi redundáns tárolás](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP Paper - Azure Storage: Egy magas rendelkezésre állású felhőalapú tárolási szolgáltatásba erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

