---
title: aaaAzure cool storage BLOB objektumokhoz |} Microsoft Docs
description: "Az Azure Blob Storage tárolási rétegei objektumadatok költséghatékony tárolását teszik lehetővé hozzáférési minták alapján. hello ritkán használt adatok tárolási réteg a ritkábban elért adatok van optimalizálva."
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/05/2017
ms.author: mihauss
ms.openlocfilehash: 56fae3fdae31a6958ebae01fd96a0366e70ae938
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob Storage: a gyakran és a ritkán használt adatok tárolási rétegei
## <a name="overview"></a>Áttekintés
Az Azure Storage két tárolási réteget kínál a Blob-objektumtároláshoz, hogy adatait a legköltséghatékonyabb módon tárolhassa a használat függvényében. hello Azure **gyakran használt adatok tárolási rétege** gyakran használt adatok tárolására van optimalizálva. hello Azure **ritkán használt adatok tárolási** , amely ritkán használt, hosszú élettartamú adatok tárolására van optimalizálva. Adatok hello ritkán használt adatok tárolási rétegében tűri ugyan alacsonyabb rendelkezésre állás, de továbbra is szükséges a magas tartósság és hasonló időben való hozzáférésre és átviteli gyakran használt adatokkal egyező. A ritkán használt adatok esetében a valamelyest alacsonyabb rendelkezésre állási szolgáltatási szintek és magasabb hozzáférési költségek elfogadható kompromisszumot jelentenek az alacsonyabb tárolási költségek ellenében.

Napjainkban hello felhőben tárolt adatok mennyisége exponenciálisan nő nő. a növekvő tárolási szükségletek toomanage költségei, az adatok tulajdonságai, például a gyakoriság elérés alapján, és a tervezett megőrzési időtartam hasznos tooorganize. Hello felhőben tárolt adatok hogyan azt létrehozása, feldolgozása és az élettartamuk során elért tekintetében eltérő lehet. Egyes adatokat aktívan használnak és módosítanak teljes élettartamuk során. Bizonyos adatokat élettartamuk, gyakran korai elérik a hozzáférések mennyisége drasztikusan hello adatok életkorának. Néhány adat hello felhőben tétlen és ritkán, ha valaha is, azonos egyszer tárolja.

A fent vázolt adathozzáférési forgatókönyvek esetében számos előnyt biztosít az olyan rétegelt tárolási megoldás, amely egy adott hozzáférési mintára van optimalizálva. A gyakran és ritkán használt adatok tárolási rétegek hello bevezetése az Azure Blob storage most a rétegelt tárolási igényt szükségességét különböző árképzési modellekkel.

## <a name="blob-storage-accounts"></a>Blob Storage-fiókok
A **Blob Storage-fiókok** speciális tárfiókok a strukturálatlan adatok blobként (objektumokként) való tárolására az Azure Storage-ban. A Blob storage-fiókok most kiválaszthatja gyakran és ritkán használt adatok tárolási rétegek toostore között a ritkábban használt adatok egy alacsonyabb tárolási költségek, és több gyakran használt tároló működés közbeni adatainak pedig alacsonyabb hozzáférési költség. BLOB storage-fiókok hasonló tooyour meglévő általános célú tárfiókok és megosztása hello szintű tartósságot, rendelkezésre állását, méretezhetőségét és teljesítményt nyújtanak, amelyekkel még ma, beleértve a 100 %-os API-konzisztenciát a blokkblobokhoz és hozzáfűző blobokat.

> [!NOTE]
> A Blob Storage-fiókok csak a blokkblobokat és a hozzáfűző blobokat támogatják, a lapblobokat nem.
> 
> 

A BLOB storage-fiókokban elérhető hello **hozzáférési szint** attribútum, amely lehetővé teszi toospecify hello tárolási réteg **gyakran használt adatok** vagy **lassú** attól függően, hogy a hello hello adataihoz fiók. Az adatok használati módja hello változása esetén is válthat a tárolási rétegek között.

> [!NOTE]
> Változó hello rétege további díjakat vonhat. Tekintse meg a hello [árak és számlázás](storage-blob-storage-tiers.md#pricing-and-billing) című szakaszban talál további információt.
> 
> 

Példa használati forgatókönyvek hello gyakran használt adatok a következők:

* Az aktív használatból vagy várható toobe rendszeren elérhető (az olvasási és írva) gyakran adatokat.
* A feldolgozásra és esetleges áttelepítési toohello tárolási rétege ritkán előkészített adatok.

Példa használati forgatókönyvek hello ritkán használt adatok a következők:

* Biztonsági mentési, archiválási és vészhelyreállítási adatkészletek.
* Régebbi nem gyakran többé megtekinthetők, de esetén várható toobe érhető el azonnal érhető el.
* Nagy méretű adatkészletekhez toobe igénylő közben további adatok későbbi feldolgozásra alatt gyűjtött hatékonyan tárolja költség. (*Például* tudományos adatok vagy gyártási létesítményből származó nyers telemetriaadatok hosszú távú tárolása)
* Eredeti (nyers) adatok, amelyeket a végső használható formába való feldolgozásukat követően is meg kell őrizni. (*Például* nyers médiafájlok a más formátumba való átkódolásukat követően)
* Megfelelőségi és archiválási adatok toobe igénylő hosszú ideig tárolja, és ritkán érhető el. (*Például* biztonsági kamerák felvételei, egészségügyi intézmények régi röntgen-/MRI-felvételei, pénzügyi szolgáltatók ügyfélszolgálati hívásainak hangfelvételei és átiratai)

A tárfiókokkal kapcsolatos további információk: [Tudnivalók az Azure Storage-fiókokról](storage-create-storage-account.md).

Blokkolása és hozzáfűző blobok tárolását igénylő csak alkalmazásokhoz, javasoljuk a Blob storage-fiókok használatával, hello tootake előnyeit differenciált árképzési modelljének előnyei rétegzett tárolás. Tisztában vagyunk azonban azzal az nem lehet bizonyos körülmények között lehetséges ahol fiókok lenne általános célú tárfiókok használata hello módon toogo, például:

* Toouse táblák, üzenetsorok, van szüksége, vagy a fájlok és tárolja a blobok kívánt hello ugyanazt a tárfiókot. Vegye figyelembe, hogy egyetlen műszaki előnye toostoring ezeket a hello fiókot használja eltérő rendelkező hello ugyanazt a megosztott kulcsok szükségesek.
* Szüksége van az toouse hello klasszikus telepítési modellt. BLOB storage-fiókok csak az hello Azure Resource Manager üzembe helyezési modellben keresztül érhetők el.
* Toouse lapblobokat van szüksége. A Blob Storage-fiókok nem támogatják a lapblobokat. Általában a blokkblobok használatát javasoljuk, kivéve, ha valamiért kifejezetten lapblobokra van szüksége.
* Hello verziót [Storage szolgáltatások REST API felülete](https://msdn.microsoft.com/library/azure/dd894041.aspx) 2014-02-14-nál korábbi, vagy a verziójával egy ügyféloldali kódtár 4.x-nél alacsonyabb, és nem tudja frissíteni az alkalmazást.

> [!NOTE]
> A Blob Storage-fiókok jelenleg az Azure-régiók mindegyikében támogatottak.
> 
> 

## <a name="comparison-between-hello-storage-tiers"></a>Hello tárolási Rétegek összehasonlítása
hello alábbi táblázat mutatja be hello két tárolási Rétegek összehasonlítása hello:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Gyakran használt adatok tárolási rétege</center></strong></td>
    <td><strong><center>Ritkán használt adatok tárolási rétege</center></strong></td
</tr>
<tr>
    <td><strong><center>Rendelkezésre állás</center></strong></td>
    <td><center>99.9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>Rendelkezésre állás<br>(RA-GRS olvasások)</center></strong></td>
    <td><center>99.99%</center></td>
    <td><center>99.9%</center></td>
</tr>
<tr>
    <td><strong><center>Használati díjak</center></strong></td>
    <td><center>Magasabb tárolási költségek<br>Alacsonyabb hozzáférési és tranzakciós költségek</center></td>
    <td><center>Alacsonyabb tárolási költségek<br>Magasabb hozzáférési és tranzakciós költségek</center></td>
</tr>
<tr>
    <td><strong><center>Minimális objektumméret<center></strong></td>
    <td colspan="2"><center>N/A</center></td>
</tr>
<tr>
    <td><strong><center>Minimális tárolási időtartam<center></strong></td>
    <td colspan="2"><center>N/A</center></td>
</tr>
<tr>
    <td><strong><center>Késés<br>(Idő toofirst bájt)<center></strong></td>
    <td colspan="2"><center>ezredmásodperc</center></td>
</tr>
<tr>
    <td><strong><center>Méretezhetőségi és teljesítménycélok<center></strong></td>
    <td colspan="2"><center>Ugyanazok, mint az általános célú tárfiókok esetében</center></td>
</tr>
</tbody>
</table>

> [!NOTE]
> BLOB-tároló fiókok támogatási hello azonos teljesítményének és méretezhetőségének célozza, általános célú tárfiókok esetében. További információkért lásd: [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) (Az Azure Storage méretezhetőségi és teljesítménycéljai).
> 
> 

## <a name="pricing-and-billing"></a>Árak és számlázás
BLOB storage-fiókok új árképzési modellt használ a blob storage hello tárolási réteg alapján. A Blob storage-fiók használata esetén alkalmazza a következő számlázási szempontok hello:

* **Tárolási költségek**: továbbá tárolt adatok mennyisége toohello, az adattárolás díja hello függ hello rétege. hello gigabájtonkénti költsége alacsonyabb hello ritkán használt adatok tárolási rétegben, mint az hello gyakran használt adatok tárolási rétegek.
* **Adathozzáférési költségek**: az adatok hello ritkán használt adatok tárolási rétegében, fizetnie kell gigabájtonkénti data access járnak olvasási és írási műveletek.
* **Tranzakciós költségek**: Mindkét réteg esetében tranzakciónkénti díjat kell fizetni. Hello tranzakciónkénti költség hello ritkán használt adatok tárolási réteg azonban nagyobb, mint az hello gyakran használt adatok tárolási rétegek.
* **Georeplikációs adatátviteli költségek**: Ez a georeplikációt konfigurálva, beleértve a GRS és az RA-GRS csak érvényes tooaccounts. A georeplikációs adatátvitel gigabájtonkénti díj ellenében érhető el.
* **Kimenő adatátviteli költségek**: A kimenő adatátvitel (azaz az adott Azure-régióból kivitt adatok) esetében gigabájtalapú sávszélesség-használati díjak lépnek fel, csakúgy, mint az általános célú tárfiókok esetében.
* **Változó hello rétege**: hello rétege rétegéről a ritkán használt adatok toohot tájékozódnia kell fizetni egyenlő tooreading minden váltás esetében hello tárfiókban lévő összes hello adatokon. Hello hello rétege rétegéről a gyakran használt adatok toocool, ugyanakkor lesz díjmentes.

> [!NOTE]
> A rendezés tooallow felhasználók tootry hello ki új tárolási rétegeket és megismerhessék a funkciókat, csak 30 2016. június hello díjat hello rétege rétegéről a ritkán használt adatok toohot fogja tekinteni ki. Július 1-jétől 2016 kezdődően hello kell fizetni ritkán használt adatok toohot alkalmazott tooall váltás lesz. További információ a modell Blob storage-fiókok árképzési hello:, [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/) lap. További részleteket a hello kimenő adatok adatátviteli díjakkal megtekintéséhez [adatforgalmi díjszabás](https://azure.microsoft.com/pricing/details/data-transfers/) lap.
> 
> 

## <a name="quick-start"></a>Első lépések
Ebben a szakaszban a bemutatjuk a következő forgatókönyveket hello Azure-portál használatával hello:

* Hogyan toocreate Blob storage-fiók.
* Hogyan toomanage Blob storage-fiók.

### <a name="using-hello-azure-portal"></a>Hello Azure-portál használatával
#### <a name="create-a-blob-storage-account-using-hello-azure-portal"></a>Hello Azure-portál használatával Blob storage-fiók létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben válassza ki a **új** > **adatok + tárolás** > **tárfiók**.
3. Adja meg a tárfiók nevét.
   
    Ez a név nem globálisan egyedi; hello URL-cím része használt tooaccess hello objektumok hello tárfiók használható.  
4. Válassza ki **erőforrás-kezelő** hello telepítési modell.
   
    Rétegzett tárolás csak használható erőforrás-kezelő tárfiókok; Ez az ajánlott telepítési modell az új erőforrások hello. További információkért tekintse meg a hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).  
5. Hello fiók típusa legördülő listában válassza ki a **Blob Storage**.
   
    Ez azért, ahol ki kell választania hello típusú tárfiókot. Rétegzett tárolás nem érhető el az általános célú tárfiókok; csak akkor érhető el a hello Blob storage-fiók.     
   
    Vegye figyelembe, hogy ha ezt választja, hello teljesítményszinttel tooStandard beállításai. Rétegzett tárolás és hello prémium szintű teljesítmény réteg nem érhető el.
6. A beállításnak a hello replikációs hello tárfiók: **LRS**, **Georedundáns**, vagy **RA-GRS**. hello alapértelmezett érték a **RA-GRS**.
   
    LRS = helyileg redundáns tárolás; Georedundáns = georedundáns tárolás (2 régiókban); RA-GRS az írásvédett georedundáns tárolás (2 régiók olvasási hozzáférés toohello második).
   
    Az Azure Storage replikálási beállításaival kapcsolatos további részleteket lásd: [Azure Storage replication](storage-redundancy.md) (Az Azure Storage replikációja).
7. Jelölje be hello jobb rétege az igényeinek: Set hello **hozzáférési szint** tooeither **lassú** vagy **gyakran használt adatok**. hello alapértelmezett érték a **gyakran használt adatok**.
8. Válassza ki a kívánt toocreate hello új tárfiók hello előfizetést.
9. Adjon meg egy új erőforráscsoportot, vagy válasszon ki egy meglévőt. További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
10. Válassza ki a tárfiók hello régióban.
11. Kattintson a **létrehozása** toocreate hello tárfiók.

#### <a name="change-hello-storage-tier-of-a-blob-storage-account-using-hello-azure-portal"></a>A Blob storage-fiók hello Azure-portál használatával hello tárolási szintjének módosítása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. toonavigate tooyour storage-fiók esetén válassza ki az összes erőforrást, majd válassza ki a tárfiók.
3. Hello-beállítások panelen kattintson **konfigurációs** tooview és/vagy módosítsa a hello konfigurációs.
4. Jelölje be hello jobb rétege az igényeinek: Set hello **hozzáférési szint** tooeither **lassú** vagy **gyakran használt adatok**.
5. Kattintson a Mentés hello panel hello tetején.

> [!NOTE]
> Változó hello rétege további díjakat vonhat. Tekintse meg a hello [árak és számlázás](storage-blob-storage-tiers.md#pricing-and-billing) című szakaszban talál további információt.
> 
> 

## <a name="evaluating-and-migrating-tooblob-storage-accounts"></a>Értékelése és tooBlob storage-fiókok áttelepítése
hello ebben a szakaszban célja toohelp felhasználók toomake zavartalan áttérés toousing Blob storage-fiókok. Két felhasználói forgatókönyv közül választhat:

* Meglévő általános célú tárfiók, és a módosítás tooa Blob storage-fiók hello jobb rétege tooevaluate szeretné.
* A Blob storage-fiók toouse mellett döntött vagy már rendelkezik egy és tooevaluate szeretné, hogy hello gyors vagy lassú tárolási réteg kell használnia.

Mindkét esetben hello első rendelés az üzleti tooestimate hello költségét tárolja, és a Blob storage-fiók tárolt adatok elérése és összehasonlításhoz, amely az aktuális költségeit.

### <a name="evaluating-blob-storage-account-tiers"></a>A Blob Storage-fiókok rétegeinek kiértékelése
Rendelés tooestimate hello költségét tárolja, és a Blob storage-fiók adataihoz fér hozzá tooevaluate kell az aktuális használati mintáját, vagy a várható használati szokások hozzávetőleges. Általában érdemes tooknow:

* Tárhelyhasználat – Mennyi adatot tárol, és ez milyen mértékben változik havi szinten?
* A tárolási minták – mennyi adatot folyamatban van. számú és írásbeli toohello fiók (beleértve az új adatok)? Hány tranzakciót használ az adatok eléréséhez, és ezek milyen típusú tranzakciók?

#### <a name="monitoring-existing-storage-accounts"></a>A meglévő tárfiókok figyelése
toomonitor a meglévő tárhely fiókok, és ezek az adatok összegyűjtése, hogy használja az Azure Storage Analytics hajtja végre a naplózás és a metrikák adatokat biztosít a tárfiókon.
Tárolási analitika tárolhatja, amely tartalmazza az összevont tranzakció statisztikák és a kapacitás adatait kérelmek toohello Blob storage szolgáltatást is általános célú tárfiókok, valamint a Blob storage-fiókok metrikákat.
Ezeket az adatokat a hello jól ismert táblában tárolja ugyanabban a tárfiókban.

A további részleteket lásd: [About Storage Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343258.aspx) (A Storage Analytics mérőszámainak áttekintése) és [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx) (A Storage Analytics mérőszámainak táblasémája).

> [!NOTE]
> A BLOB storage-fiókokban elérhető hello table szolgáltatási végpont csak történő tárolásához és hello metrikai adatok fiók eléréséhez.
> 
> 

toomonitor hello tároló fogyasztása a Blob storage szolgáltatás hello, szüksége lesz a tooenable hello kapacitási mérőszámokat.
Engedélyezve van a kapacitás adatok naponta rögzítik egy tárfiók a Blob szolgáltatás, és toohello írt tábla bejegyzésének rögzített *$MetricsCapacityBlob* tábla hello belül ugyanaz a tárfiók.

toomonitor hello adatok hozzáférési mintázatát hello Blob storage szolgáltatásban, szüksége lesz a tooenable hello óránkénti tranzakció metrikákat, az API-szintet.
Ennek engedélyezve van, egy API tranzakciók óránként összesítve, és toohello írt tábla bejegyzésének rögzített *$MetricsHourPrimaryTransactionsBlob* tábla hello belül ugyanaz a tárfiók. Hello *$MetricsHourSecondaryTransactionsBlob* tábla rekordok hello tranzakciók toohello másodlagos végponti RA-GRS-tárfiókok esetén.

> [!NOTE]
> Abban az esetben, ha rendelkezik egy általános célú tárfiókkal, amelyben lapblobokat és virtuálisgép-lemezek tárol a blokkblobok és a hozzáfűző blobok adatai mellett, akkor ez a becslési folyamat nem alkalmazható. Ennek az az oka, már nem fogja tudni megkülönböztető kapacitás és a tranzakció mérőszámok alapján hello blob csak letiltása, és a hozzáfűző blobok lesz áttelepítve tooa Blob storage-fiók.
> 
> 

Válassza ki a megőrzési idő, amely jellemző a normál használati hello metrikákat, és extrapolálják javasoljuk az adatok felhasználása és minták egy jó közelítéséről tooget.
Egy elem tooretain hello metrikai adatok 7 nap és a gyűjtés hello adatok minden héten hello hónap végén hello elemzés céljából.
Lehetősége tooretain hello metrikai adatok hello az elmúlt 30 napban és adatgyűjtés és -elemzés hello hello végén lévő hello 30 napos időtartamon belül.

További tudnivalók a mérőszámadatok engedélyezésével, gyűjtésével és megtekintésével kapcsolatban: [Enabling Azure Storage metrics and viewing metrics data](storage-enable-and-view-metrics.md) (Az Azure Storage mérőszámainak engedélyezése és a mérőszámadatok megtekintése).

> [!NOTE]
> Az elemzési adatok tárolása, elérése és letöltése ugyanúgy díjhoz kötött, mint a normál felhasználói adatok használata.
> 
> 

#### <a name="utilizing-usage-metrics-tooestimate-costs"></a>Használati metrikák tooestimate költségek használata
##### <a name="storage-costs"></a>Tárolási költségek
legújabb bejegyzés hello kapacitás metrikák táblázatban hello *$MetricsCapacityBlob* hello sor kulccsal *"data"* mutat be a felhasználói adatok által használt tárolókapacitás hello.
legújabb bejegyzés hello kapacitás metrikák táblázatban hello *$MetricsCapacityBlob* hello sor kulccsal *"analytics"* látható hello hello analytics naplók által használt tárolókapacitás.

A teljes kapacitás használni mindkét felhasználói adatok és analitikák naplók (Ha engedélyezve van), majd lehet tooestimate hello költségét hello tárfiókban lévő adatok tárolására használt.
hello ugyanezt a módszert a tárolási költségek letiltása a becsléséhez is használható, és a hozzáfűző blobok az általános célú tárfiókok esetében.

##### <a name="transaction-costs"></a>Tranzakciós költségek
hello összege *"TotalBillableRequests"*, az API-nak hello tranzakció minden bejegyzésben metrikák táblázat hello tranzakciók száma összesen, hogy az adott API-hoz. *pl.*, száma hello *"GetBlob"* egy adott időszakban tranzakciók kerülhet sor számlázható kérelmek teljes száma az összes bejegyzés hello összege hello sor kulccsal *"felhasználói; GetBlob "*.

A sorrend tooestimate tranzakciós költségek Blob storage-fiókok szüksége lesz toobreak hello tranzakciók három csoportokba le másképp áron óta.

* Írási tranzakciók, például *„PutBlob”*, *„PutBlock”*, *„PutBlockList”*, *„AppendBlock”*, *„ListBlobs”*, *„ListContainers”*, *„CreateContainer”*, *„SnapshotBlob”* és *„CopyBlob”*.
* Törlési tranzakciók, például *„DeleteBlob”* és *„DeleteContainer”*.
* Minden egyéb tranzakció.

A sorrend tooestimate tranzakciós költségek általános célú tárfiókok kell tooaggregate hello művelet/API függetlenül minden tranzakciót.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Az adatok hozzáférésének és a georeplikációs adatok átvitelének költségei
Míg tárolási analitika nyújt hello adatmennyiség olvassa be, illetve tooa tárfiók írt, nagyjából becslése hello tranzakció metrikák tábla megtekintésével.
hello összege *"TotalIngress"* minden bejegyzésnél egy API-nak hello tranzakció metrikák tábla bájtban érkező adatok teljes mennyiségének hello jelzi, hogy az adott API-hoz.
Hasonlóképpen hello összege *"TotalEgress"* hello teljes adatmennyiség kilépő, bájtban jelzi.

A sorrend tooestimate hello adatok hozzáférési költségek Blob storage-fiókok szüksége lesz a toobreak hello tranzakciók két csoportra bontják le.

* hello adatmennyiség hello tárfiókból hello összege alapján becsülhető *"TotalEgress"* a elsősorban hello *"GetBlob"* és *"CopyBlob"* műveletek.
* hello toohello tárfiók írt adatok mennyiségét hello összege alapján becsülhető *"TotalIngress"* a elsősorban hello *"PutBlob"*, *"PutBlock"*, *"CopyBlob"* és *"AppendBlock"* műveletek.

a BLOB storage-fiókok is hello adatmennyiség esetén a grs-re vagy RA-GRS tárfiók írt hello becsült használatával kell kiszámítani georeplikációs adatátviteli költségek hello.

> [!NOTE]
> Kiszámítása hello költségeit hello gyors vagy lassú tárolási rétegek használatával kapcsolatos további részletes például kérjük, szánjon egy pillantást hello – gyakori kérdések című *"Mik azok a gyakran használt adatok és ritkán hozzáférési rétegek, és hogyan kell eldönteni, mely egy toouse?"* a hello [Azure Storage árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage/).
> 
> 

### <a name="migrating-existing-data"></a>Meglévő adatok áttelepítése
A Blob Storage-fiókok kifejezetten blokkblobok és hozzáfűző blobok tárolására készültek. Meglévő általános célú tárfiókok esetében, amelyek lehetővé teszik a toostore táblák, üzenetsorok, fájlok és lemezek, valamint blobokat, átalakított tooBlob tárfiókok nem lehet. toouse hello tárolási rétegeket, a rendszer kell toocreate új Blob storage-fiókok és a meglévő adatokat áttelepíteni hello az újonnan létrehozott fiókok.

Használhatja a következő módszerek toomigrate meglévő adatok a Blob storage-fiókok, a helyszíni tárolási eszközökről, külső felhőtárolási szolgáltatóktól vagy az Azure-ban a meglévő általános célú tárfiókok hello:

#### <a name="azcopy"></a>AzCopy
AzCopy egy Windows parancssori segédprogram készült Azure Storage-ból adatokat tooand másolását nagy teljesítményű. Is használhatja a Blob storage-fiók a meglévő általános célú tárfiókok esetében az AzCopy toocopy adatok, vagy tooupload adatokat a helyszíni tárolási eszközökről a Blob storage-fiók.

További részletekért lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Adatátviteli könyvtár
Az Azure Storage adatátviteli könyvtár a .NET-hez hello alapvető adatátviteli keretrendszeren azcopyt működtető alapul. hello szalagtár végzi a nagy teljesítményű, megbízható és könnyű adatátviteli műveletek hasonló tooAzCopy. Ez lehetővé teszi minden előnyét tootake hello szolgáltatások azcopy által az alkalmazás natív módon fut, és az AzCopy külső példányait monitoring toodeal nélkül.

További részletek: [Azure Storage adatátviteli könyvtár a .Net-keretrendszerhez](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST API vagy ügyfélkódtár
Az adatok egy egyéni alkalmazást toomigrate létrehozása a Blob storage-fiók hello Azure ügyfélkódtárai egyikének használatával, vagy hello Azure storage szolgáltatások REST API-t. Az Azure Storage gazdag ügyfélkódtárakat biztosít több nyelvhez és platformhoz is, beleértve a következőket: .NET, Java, C++, Node.JS, PHP, Ruby és Python. hello ügyfélkódtárak olyan fejlett képességeket biztosítanak, például újrapróbálkozási logika, a naplózás vagy a párhuzamos feltöltések. Is fejleszthet közvetlenül a REST API, amely bármely olyan nyelvvel, amely lehetővé teszi a HTTP/HTTPS-kéréseket hívható hello szemben.

További részletekért lásd: [Ismerkedés az Azure Blob Storage szolgáltatással](storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Az ügyféloldali titkosítással titkosított blobok a titkosítással kapcsolatos metaadatok hello blobbal együtt tárolni tárolja. Elengedhetetlen fontosságú, hogy a másolási műveletek győződjön meg arról, hogy hello blob metaadatai, és különösen hello titkosítással kapcsolatos metaadatok, megőrzi a. Ha hello blobokat a metaadatok nélkül másolja, hello blobtartalom lekérhető nem lesz újra. A titkosítással kapcsolatos metaadatok további részletei: [Az Azure Storage ügyféloldali titkosítása](storage-client-side-encryption.md).
> 
> 

## <a name="faqs"></a>Gyakori kérdések
1. **A meglévő tárfiókok továbbra is elérhetőek?**
   
    Igen, a meglévő tárfiókok továbbra is elérhetőek változatlan árképzéssel és funkcionalitással.  Azok a tárolási réteg nem kell hello képességét toochoose, és nem lesz rétegezési képességek a jövőbeli hello.
2. **Miért és mikor érdemes elkezdenem Blob Storage-fiókot használni?**
   
    BLOB storage-fiókok kifejezetten blobok tárolására, és lehetővé teszik a számunkra toointroduce új blobközpontú szolgáltatások. Következő lépésként Blob storage-fiókok, az ilyen típusú alapján ajánlott módszer a blobok tárolására, mivel a későbbi képességek, például a hierarchikus tárolás és rétegezéséhez hello lesz bevezetve. Célszerű tooyou fel, ha azt szeretné, hogy az üzleti követelmények alapján toomigrate.
3. **A meglévő tárolási fiók tooa Blob storage-fiók tud konvertálni?**
   
    Nem. BLOB storage-fiók egy eltérő jellegű tárfiók, és toocreate szüksége lesz az új és az adatok áttelepítését a fentiekben leírtak szerint.
4. **Tárolhatok objektumokat mindkét hello a tárolási rétegek a ugyanazt a fiókot?**
   
    Hello *"Hozzáférési szint"* attribútum, amely megadja, hogy hello rétege fiók szintjén van beállítva, és alkalmazza a fiókhoz tartozó tooall objektumok. Egy objektum szinten hello hozzáférési réteg attribútum nem állítható.
5. **Módosíthatja a hello rétege a Blob storage-fiókom?**
   
    Igen. Fogja tudni toochange hello rétege által hello beállítása *"Hozzáférési szint"* hello tárfiók attribútum. Változó hello rétege hello fiókban tárolt tooall objektumok fog vonatkozni. Változás hello tárolási réteg a gyakran használt adatok toocool lesz mindaddig nem számítunk fel díjakat, amíg rétegéről a ritkán használt adatok toohot tájékozódnia a Gigabájtonkénti beolvasási költséggel hello fiók összes hello adatok olvasásához.
6. **Milyen gyakran módosíthatom hello rétege a Blob storage-fiókom?**
   
    Amíg nem határoztunk meg korlátozást hello tárolási réteg módosításának milyen gyakran, vegye figyelembe, hogy a hello rétege rétegéről a ritkán használt adatok toohot tájékozódnia jelentős költségekkel jár. Hello tárolási rétege gyakran módosítása nem ajánlott.
7. **Lesz hello blobok hello ritkán használt adatok tárolási rétegében eltérően viselkednek, mint a hello hello tároláselérési rétegében azokat?**
   
    Hello tároláselérési rétegében a blobok rendelkezik hello ugyanolyan késéssel, mint az általános célú tárfiókok esetében. Hello ritkán használt adatok tárolási rétegében a blobok (ezredmásodpercben), blobok hasonló késéssel rendelkeznek, az általános célú tárfiókok esetében.
   
    Hello ritkán használt adatok tárolási rétegében a blobok a valamelyest alacsonyabb rendelkezésre állási szolgáltatási szintek (SLA) mint hello tároláselérési rétegében tárolt hello blobok fog rendelkezni. További információkért lásd: [SLA a következőhöz: Storage](https://azure.microsoft.com/support/legal/sla/storage).
8. **Tárolhatok lapblobokat és virtuális gépek lemezeit a Blob Storage-fiókban?**
   
    A Blob Storage-fiókok csak a blokkblobokat és a hozzáfűző blobokat támogatják, a lapblobokat nem. Azure virtuális gépek lemezeit a lapblobokat üzemelnek, és ennek eredményeképpen a Blob storage-fiókok nem lehet használt toostore virtuálisgép-lemezeket. Azonban már lehetséges toostore hello virtuális gépek lemezeinek biztonsági másolatait blokkblobként Blob storage-fiók.
9. **Módosítanom kell toochange a meglévő alkalmazások toouse Blob storage-fiókok?**
   
    A Blob Storage-fiókok 100%-ban API-konzisztensek az általános célú tárfiókokkal a blokkblobok és a hozzáfűző blobok esetében. Mindaddig, amíg az alkalmazás blokkblobok használatát, vagy hozzáfűző blobokat és hello hello 2014-02-14-es verzióját használja [Storage szolgáltatások REST API felülete](https://msdn.microsoft.com/library/azure/dd894041.aspx) vagy annál nagyobb az alkalmazás kell működnie. Hello protokoll egy régebbi verzióját használja, ha akkor szüksége lesz tooupdate az alkalmazás toouse hello új verziója, mindkét típusú tárfiókkal zökkenőmentesen rendelkező toowork. Általánosságban elmondható mindig ajánlott hello legújabb verzió, függetlenül attól, milyen típusú tárfiókok használata.
10. **Változik a felhasználói élmény?**
    
    BLOB storage-fiókok nagyon hasonló tooa általános célú tárfiókok blokk tárolásához és hozzáfűző blobok, és támogatja az Azure-tároló, beleértve a magas tartósság és a rendelkezésre állási, a méretezhetőséget, a teljesítmény és a biztonsági összes hello funkciói. Hello szolgáltatásait és korlátozások adott tooBlob storage-fiókok és a tárolási rétegek, amelyek a fenti mindent feltüntettük kívül más marad hello azonos.

## <a name="next-steps"></a>Következő lépések
### <a name="evaluate-blob-storage-accounts"></a>A Blob Storage-fiókok értékelése
[A Blob Storage-fiókok elérhetőségének ellenőrzése régió alapján](https://azure.microsoft.com/regions/#services)

[Aktuális tárfiókjai használatának értékelése az Azure Storage mérőszámainak engedélyezésével](storage-enable-and-view-metrics.md)

[A Blob Storage régió szerinti árképzésének megtekintése](https://azure.microsoft.com/pricing/details/storage/)

[Az adatátviteli díjszabás megtekintése](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>A Blob Storage-fiókok használatának megkezdése
[Ismerkedés az Azure Blob Storage szolgáltatással](storage-dotnet-how-to-use-blobs.md)

[Adatok tooand áthelyezése az Azure Storage-ból](storage-moving-data.md)

[Adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md)

[A tárfiókok tallózása és felfedezése](http://storageexplorer.com/)

