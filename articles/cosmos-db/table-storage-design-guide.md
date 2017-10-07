---
title: "aaaAzure tárolási tábla tervezési útmutatója |} Microsoft Docs"
description: "A Tervező méretezhető és Performant táblák Azure Table Storage-ban"
services: cosmos-db
documentationcenter: na
author: mimig1
manager: tadb
editor: tysonn
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 02/28/2017
ms.author: mimig
ms.openlocfilehash: 059f05a1d20e4d9537034b7ca133c5334bbefa04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a>Az Azure Storage táblázat kialakítási Útmutató: Méretezhető tervezésével és Performant táblák
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

méretezhető toodesign és performant táblák, például a teljesítmény, méretezhetőség és költség tényező figyelembe kell vennie. Korábban létrehozott sémák a relációs adatbázisok, ha ezeket a szempontokat kell-e a megszokott tooyou, de amíg vannak bizonyos az hello Azure Table storage modell és a relációs modellek közötti Hasonlóságok, is számos fontos eltéréseket. Ezek a különbségek általában toovery különböző kialakításokról, előfordulhat, hogy ismeri a relációs adatbázisok counter-intuitive vagy rossz toosomeone keresse meg, de amelyek tegye célszerű jó egy NoSQL kulcs-érték tároló hello Azure Table szolgáltatás például tervezésekor vezethet. A Tervező különbségek számos módon változik meg hello arra, hogy hello Table szolgáltatás, amely tartalmazhat egy entitások (relációs adatbázis-terminológia sorainak) az adatok, vagy olyan adatkészletekhez, amelyek támogatnia kell a nagyon nagy tervezett toosupport felhőméretű alkalmazások tranzakció kötetek: így kell toothink másképp kapcsolatos hogyan tárolja az adatait, és hello Table szolgáltatás működésének megismerése. Egy jól kidolgozott NoSQL-adattár engedélyezheti a megoldás tooscale sokkal tovább (és alacsonyabb költségekkel) mint olyan megoldás, amely egy relációs adatbázist használ. Az útmutató az alábbi témakörök segítségével.  

## <a name="about-hello-azure-table-service"></a>Kapcsolatos hello Azure Table szolgáltatás
Ez a szakasz néhány hello fő szolgáltatásainak hello Table szolgáltatás teljesítményének és méretezhetőségének különösen fontos toodesigning mutatja be. Ha új tooAzure tárolási és hello Table szolgáltatás, elolvashatja [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md) és [Ismerkedés az Azure Table Storage használatának .NET](table-storage-how-to-use-dotnet.md) hello többi elolvasása előtt a cikk. Hello Ez az útmutató elsősorban hello Table szolgáltatás, bár ez magában foglalja a néhány leírását az Azure Queue hello Blob, és szolgáltatások hogyan használhatja őket a megoldás a Table szolgáltatás hello együtt.  

Mi az az hello Table szolgáltatás? Hello neve alapján várt, módon hello Table szolgáltatás használ a toostore táblázatos formátumú adatok. Hello ismertetésében hello tábla minden egyes sorára entitás jelöli, és hello oszlopok tároló hello adott entitás tulajdonságait. Minden entitás rendelkezik kulcsok toouniquely két azonosítását, és egy Timestamp típusú oszlop, amely a Table szolgáltatás hello tootrack használja, amikor hello entitás utolsó frissítésének (Ez automatikusan megtörténik, és adjon meg egy tetszőleges értéket nem lehet felülírni hello időbélyeg manuálisan). Table szolgáltatás hello a last-modified időbélyeg (LMT) toomanage egyidejű hozzáférések optimista használja.  

> [!NOTE]
> hello tábla szolgáltatás REST API-műveleteket is vissza egy **ETag** osztályból származik hello last-modified időbélyeg (LMT) értéket. Ebben a dokumentumban használjuk hello kifejezések ETag és LMT azonos értelemben mert toohello vonatkoznak ugyanazt az alapul szolgáló adatokat.  
> 
> 

hello alábbi példában egy egyszerű táblázat kialakítási toostore alkalmazott és részleg entitásokat. Ez a kialakítás egyszerű hello példák az útmutató későbbi részében látható számos alapul.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>időbélyeg</th>
<th></th>
</tr>
<tr>
<td>Marketing</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td>Nincs</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td>Jún</td>
<td>CaO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>Szervezeti egység</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>departmentname nevű</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marketing</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Értékesítés</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Az eddigi ez keresése nagyon hasonló tooa tábla egy relációs adatbázisban hello kulcs eltérésekkel alatt hello kötelező oszlopok és hello képességét toostore több entitás típusok a hello ugyanabban a táblában. Emellett egyes hello felhasználói tulajdonságok például **Keresztnév** vagy **kora** adattípusú, például az egész szám vagy karakterlánc, csak, például egy relációs adatbázisban oszlop. Bár eltérően egy relációs adatbázisban, hello séma nélküli jellege hello tábla szolgáltatás azt jelenti, hogy a tulajdonság nem szükséges hello azonos adattípus minden entitáshoz. toostore összetett adattípusú egyetlen tulajdonsággal, például a JSON- vagy XML-szerializált formátum kell használnia. Hello tábla szolgáltatás például a támogatott adattípusok, támogatott dátumtartományok, elnevezési szabályok és mérete megkötések kapcsolatos további információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Mivel látni fogja, a választott **PartitionKey** és **RowKey** alapvető toogood Táblatervezés van. Minden entitás egy táblázatban tárolja rendelkeznie kell egy egyedi kombinációja **PartitionKey** és **RowKey**. Csakúgy, mint a kulcsokat egy relációs adatbázis tábláinak, hello **PartitionKey** és **RowKey** értékei indexelt toocreate egy fürtözött indexet, amely lehetővé teszi, hogy a gyors look-ups; azonban hello Table szolgáltatás nem hoz létre bármelyik másodlagos indexek, így ezek a hello csak két indexelt tulajdonságok (néhány későbbi hello minták megjelenítése hogyan oldható meg a nyilvánvaló korlátozás).  

Egy tábla egy vagy több partíció épül fel, és mivel látni fogja, hello számos tervezési döntések körül megfelelő választás lehet lesz **PartitionKey** és **RowKey** toooptimize a megoldás. A megoldás csak egyetlen tábla összes, a partíciók szervezve entitásokat tartalmazó sikerült alkotják, de általában egy megoldás több táblák esetében. Táblázatok segítséget toologically rendezheti az entitások, amelyekkel kezelheti a hozzáférést toohello használatával végzett hozzáférés-vezérlési listák és egyetlen tárolási művelettel teljes táblázat elvetné.  

### <a name="table-partitions"></a>Táblapartíciók
hello fiók nevét, a tábla neve és **PartitionKey** hello partíción belül hol tárolja az hello table szolgáltatás a hello entitás hello társzolgáltatás együtt azonosításához. Amellett, hogy a címzési séma entitások hello részét, partíciók tranzakciók hatókör meghatározása (lásd: [entitás csoport tranzakciók](#entity-group-transactions) alább), és hogyan hello table szolgáltatás méretezi űrlap hello alapját. További információk a partíciókon: [Azure Storage méretezhetőségi és teljesítménycéloknak](../storage/common/storage-scalability-targets.md).  

A Table szolgáltatás hello, az egyes csomópontok szolgáltatások egyik, vagy több partíció befejeződését, és méretezik service dinamikus terheléselosztás hello partíciók csomópontjai között. Ha egy csomópont terhelésnek van kitéve, hello table szolgáltatás is *vágási* partíciók hello számos különböző csomópontokon, a csomópont által kiszolgált; forgalom enyhül, amikor hello szolgáltatást is *egyesítési* hello partíció találhatók csendes csomópontok biztonsági alakzatot egyetlen csomópont.  

További információ a hello hello belső részleteit Table szolgáltatás, és különösen hello szolgáltatás kezeli a partíciókat, hogy olvassa hello papír [Microsoft Azure Storage: A magas rendelkezésre álló felhőalapú tárolási szolgáltatásba az erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Entitás csoport tranzakciók
A Table szolgáltatás hello entitás csoport tranzakciók (EGTs) csak beépített mechanizmus hello atomi frissítések végrehajtásához a több egység közötti. EGTs megtalálhatók hivatkozott tooas *kötegelt tranzakciókat* bizonyos dokumentációkban. EGTs csak működhessenek hello tárolt entitásokat tartalmazó partícióra (megosztás hello egy adott táblában azonos partíciós kulcs), így bármikor atomi tranzakciós viselkedés szüksége több egység közötti tartozó entitásokból hello tooensure kell egyazon partícióra kerüljenek. Ez gyakran az több entitástípusok megőrzi hello azonos tábla (és particionálhatja) található, és több tábla nem használ másik entitástípusok okát. Egyetlen EGT legfeljebb 100 entitást is működik.  Ha több egyidejű EGTs fontos tooensure feldolgozása van e EGTs nem működik, amelyek közösek a EGTs között, mivel ellenkező esetben feldolgozása késleltethető entitások.

EGTs is vezethet a potenciális kompromisszum a tooevaluate kialakításában: több partíciót használata megnöveli a hello méretezhetőség, az alkalmazás Azure-csomópontokon keresztüli kérelmek terheléselosztási további lehetőségekkel rendelkezik, de ez korlátozhatja a hello az alkalmazás tooperform atomi tranzakciók azon képessége, és erős konzisztenciát biztosít az adatok karbantartása. Emellett nincsenek egyedi méretezhetőségi célok javasoljuk, hogy előfordulhat, hogy korlátozza az egyetlen csomópont számíthat tranzakciók hello átviteli szintű hello: További információ az Azure storage-fiókok és hello tábla hello méretezhetőségi célok szolgáltatás című [Azure Storage méretezhetőségi és teljesítménycéloknak](../storage/common/storage-scalability-targets.md). A jelen útmutató későbbi szakaszok tárgyalják különböző kialakítási stratégiák, amelyek segítenek kezelni például ez kompromisszumot és ismertetik, hogyan lehet a legjobban toochoose a partíciós kulcs hello meghatározott követelmények alapján az ügyfélalkalmazás.  

### <a name="capacity-considerations"></a>A kapacitás kapcsolatos szempontok
hello alábbi táblázat tartalmaz néhány hello kulcsértékei toobe tudomást amikor tervezésekor a Table szolgáltatás megoldás:  

| Az Azure storage-fiók teljes kapacitás | 500 TB |
| --- | --- |
| Egy Azure storage-fiókot a táblák száma |Csak hello tárfiók hello kapacitásának korlátozva |
| Egy tábla partíciók száma |Csak hello tárfiók hello kapacitásának korlátozva |
| Partíció entitástartományának száma |Csak hello tárfiók hello kapacitásának korlátozva |
| Az egyes entitás mérete |Legfeljebb 255 tulajdonságok legfeljebb too1 MB (beleértve a hello **PartitionKey**, **RowKey**, és **időbélyeg**) |
| Hello mérete **PartitionKey** |Egy karakterlánc too1 KB méretű mentése |
| Hello mérete **RowKey** |Egy karakterlánc too1 KB méretű mentése |
| Egy entitás csoport tranzakció mérete |Egy tranzakció legfeljebb 100 entitást tartalmazhat, és hello hasznos lehet kisebb, mint 4 MB-nál. Egy EGT csak frissíthető entitás egyszer. |

További információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Költség kapcsolatos szempontok
A TABLE storage költségei viszonylag alacsonyak, de költség becslése mindkét kapacitás használati és hello tranzakciók mennyisége a hello Table szolgáltatás használó megoldások próbaidőszakában részeként tartalmaznia kell. Azonban számos forgatókönyvben denormalizált vagy ismétlődő adatok tárolását rendelés tooimprove hello teljesítmény vagy a méretezhetőség miatt a megoldás nem egy érvényes megközelítés tootake. Az árazással kapcsolatos további információkért lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Táblatervezés vonatkozó irányelvek
A listák összesítésének néhány hello kulcs irányelveket kell figyelembe venni a táblák tervezése során, és ez az útmutató foglalkozni fog velük az összes későbbi részében részletesebben. Ezeket az irányelveket nagyon eltérnek általában végrehajtania a relációs adatbázis-tervező hello irányelveket.  

A Table szolgáltatás megoldás toobe tervezése *olvasási* hatékony:

* ***Tervezze meg az olvasási műveleteket alkalmazásokban lekérdezése.*** A táblák tervezésekor gondolniuk hello lekérdezéseket (különösen hello késés bizalmas is), amely, végrehajtja a véleménye hogyan frissíti az entitások előtt. Ez általában annak az eredménye a hatékony és performant megoldás.  
* ***Adja meg a lekérdezések PartitionKey és RowKey is.*** *Mutasson a lekérdezések* például ezek a hello leghatékonyabb tábla szolgáltatás lekérdezéseket.  
* ***Érdemes tárolni a duplikált entitásokat.*** A TABLE storage olcsó ezért fontolja meg több alkalommal (különböző kulccsal rendelkező) ugyanaz az entitás tárolása hello tooenable hatékonyabb lekérdezések.  
* ***Vegye figyelembe az adatok denormalizing.*** A TABLE storage olcsó ezért fontolja meg az adatok denormalizing. Összegző entitások például tárolja, hogy a lekérdezések összesített adatok csak egyetlen entitáshoz tooaccess kell.  
* ***Használja az összetett kulcs értékeket.*** hello csak akkor kulcsokban **PartitionKey** és **RowKey**. Például használja az összetett kulcs értékeket tooenable másodlagos kulccsal elérési útvonalak tooentities.  
* ***Lekérdezés vetítéshez használni.*** Válassza ki a csak a szükséges mezők hello lekérdezésekkel hello hálózati átvitel adatok mennyisége hello csökkentése érdekében.  

A Table szolgáltatás megoldás toobe tervezése *írási* hatékony:  

* ***Ne hozzon létre a gyakran használt adatok partíciókat.*** Válassza ki, amelyek lehetővé teszik toospread kulcsok a kérelmek közötti idő minden helyen több partíciót.  
* ***Kerülje a forgalmat a teljesítményt.*** Hello forgalom sima ésszerű meghatározott időtartam során rendelkezésre, és elkerülheti a forgalmat a teljesítményt.
* ***Feltétlenül ne hozzon létre egy külön táblázat az egyes entitás.*** Ha atomi tranzakciók entitástípusok között, tárolhat több entitás típusaival azonos hello partíciójához hello ugyanabban a táblában.
* ***Vegye figyelembe a hello maximális átviteli sebesség kell elérni.*** Kell figyelembe vennie a Table szolgáltatás hello hello méretezhetőségi célok, és győződjön meg arról, hogy a tervező nem miatt a tooexceed őket.  

Ez az útmutató olvasás példák, amelyek a gyakorlatban az összes alapelvek jelenik meg.  

## <a name="design-for-querying"></a>Tervezési lekérdezése
TABLE szolgáltatási megoldások intenzív, írási intenzív vagy hello két vegyesen olvashatók. Ez a szakasz összpontosít hello dolgot toobear szem előtt, a Table szolgáltatás toosupport hatékonyan olvasási műveletek tervezése során. A Tervező, támogatja a hatékony olvassa el az operations általában is hatékony az írási műveletek. Van azonban további szempontok toobear figyelembe amikor designing toosupport írási műveleteket, hello a következő szakaszban tárgyalt [adatmódosítás kialakítása](#design-for-data-modification).

A Table szolgáltatás megoldás tooenable tervezéséhez jó kiindulási pont tooread adatok hatékonyan tooask "milyen lekérdezések fogja a szükséges tooexecute tooretrieve hello alkalmazásadatok a Table szolgáltatás hello kell?"  

> [!NOTE]
> Table szolgáltatás hello, fontos tooget hello megfelelő kialakítás előre mert bonyolult és költséges toochange később. Például egy relációs adatbázisban, gyakran egyszerűen hozzáadásával lehetséges tooaddress teljesítményproblémák indexeli a meglévő adatbázis tooan: Ez a lehetőség nem érhető a Table szolgáltatás hello.  
> 
> 

Ez a szakasz hello kapcsolatos problémák meg kell oldania a táblák lekérdezése tervezésekor összpontosít. Ebben a szakaszban ismertetett hello témaköröket tartalmazza:

* [Hogy a választott PartitionKey és RowKey milyen hatással van a teljesítmény-küszöbérték](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Egy megfelelő PartitionKey kiválasztása](#choosing-an-appropriate-partitionkey)
* [A Table szolgáltatás hello lekérdezések optimalizálása](#optimizing-queries-for-the-table-service)
* [A Table szolgáltatás hello adatok rendezése](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Hogy a választott PartitionKey és RowKey milyen hatással van a teljesítmény-küszöbérték
hello alábbi példák azt feltételezik hello table szolgáltatás alkalmazott entitások tárolja a struktúra a következő hello (hello példák a legtöbb hagyja el a hello **időbélyeg** jobb érthetőség kedvéért bizonyos tulajdonság):  

| *Oszlop neve* | *Adattípus* |
| --- | --- |
| **PartitionKey** (részleg neve) |Karakterlánc |
| **RowKey** (alkalmazott azonosítója) |Karakterlánc |
| **Utónév** |Karakterlánc |
| **Vezetéknév** |Karakterlánc |
| **Kora** |Egész szám |
| **E-mail cím** |Karakterlánc |

a szakasz korábbi hello [Azure Table szolgáltatás áttekintése](#overview) néhány hello funkciói a hello Azure Table szolgáltatás, amely közvetlenül befolyásolják a lekérdezéshez tervezéséről foglalja össze. Ezek a következő általános irányelveket a Table szolgáltatás Lekérdezéstervezés hello eredményez. Vegye figyelembe, hogy az alábbi hello a példákban szereplő hello szűrőszintaxisának hello Table szolgáltatás REST API-t a további tudnivalókat lásd a [lekérdezés entitások](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

* A ***pont lekérdezés*** hello leghatékonyabb keresési toouse, és nagy mennyiségű keresések vagy a legkisebb mértékű késleltetést igénylő keresések használt toobe ajánlott. Ilyen lekérdezés nagyon hatékonyan az hello indexek toolocate egyedi entitás használhatja mindkettő hello megadásával **PartitionKey** és **RowKey** értékeket. Például: $filter = (PartitionKey eq 'Értékesítés') és (RowKey eq '2')  
* Második legjobb van egy ***értéktartomány lekérdezésének*** hello használó **PartitionKey** és a szűrők számos **RowKey** értékek tooreturn egynél több entitás. Hello **PartitionKey** érték azonosítja az egy adott partícióra, és a hello **RowKey** értékek azonosítása hello entitások az adott partíció egy részét. Például: $filter = "Értékesítési és RowKey ge" PartitionKey eq és RowKey lt jelzést "  
* Harmadik legjobb van egy ***partíció vizsgálata*** hello használó **PartitionKey** és olyan nem kulcs tulajdonsága, és hogy a szűrők térhetnek vissza egy entitást. Hello **PartitionKey** érték egy adott partícióra azonosítja, és hello tulajdonság értékei válassza ki az adott partíció hello entitásának egy részéhez. Például: $filter PartitionKey eq "Értékesítési" és a Vezetéknév eq 'Smith' =  
* A ***tábla vizsgálata*** nem tartalmazza a hello **PartitionKey** és nem nagyon hatékony, mivel az összes pedig a táblázatban a megfelelő entitások hello partíciókat keresi. A táblázatbeolvasás, függetlenül attól, hogy a szűrő-e használja a hello hajtja végre **RowKey**. Például: $filter Vezetéknév eq "János" =  
* Több entitás visszaadó lekérdezések Alapértelmezések rendezve **PartitionKey** és **RowKey** sorrendje. tooavoid keresésére átrendezésével hello entitásának hello ügyfél, válassza ki a **RowKey** hello leggyakrabban használt rendezési sorrend meghatározó.  

Vegye figyelembe, hogy használja az "**vagy**" szűrő alapján toospecify **RowKey** értéket egy partíció vizsgálati eredményeket, majd egy értéktartomány lekérdezésének nem számít. Ezért kerülje el a lekérdezéseket, szűrők, mint amelyekkel: $filter = PartitionKey eq 'Értékesítés' és (RowKey eq "121" vagy "322" RowKey eq)  

Az ügyféloldali kódot hello a Storage ügyféloldali kódtára tooexecute hatékony lekérdezések használó című részben talál példákat:  

* [A Storage ügyféloldali kódtára hello segítségével pont lekérdezése](#executing-a-point-query-using-the-storage-client-library)
* [LINQ használatával több entitás beolvasásakor](#retrieving-multiple-entities-using-linq)
* [Kiszolgálóoldali leképezése](#server-side-projection)  

Példák ügyféloldali kódot, amelyet több entitás kezelni tud a típusok tárolt hello azonos táblázatban, lásd:  

* [Heterogén entitástípusok használata](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a>Egy megfelelő PartitionKey kiválasztása
A választott **PartitionKey** kell egyensúlyba hello kell tooenables hello használata EGTs (tooensure konzisztencia) hello követelmény toodistribute szemben az entitások több partíciót (tooensure méretezhető megoldás).  

Egy rendkívüli az entitások sikerült egyetlen partícióra vannak tárolva, de ez korlátozhatja a megoldás hello méretezhetőséget, és szeretné, hogy hello table szolgáltatás nem tudja tooload-egyenleg kérelmek. A hello más extreme tárolhatja partíciónként, amely magas szinten méretezhető lenne, és lehetővé teszi a hello tábla szolgáltatáskérések tooload elosztani a kéréseket, de amelyek akadályozzák meg entitás csoport tranzakciók használatával egy entitást.  

Az ideális **PartitionKey** , amely lehetővé teszi toouse hatékony lekérdezések és a megoldás megfelelő partíciók tooensure rendelkező méretezhető. Általában találja, hogy az entitások lesz-e az entitások elosztja elegendő partíciók megfelelő tulajdonság.

> [!NOTE]
> A rendszer, hogy a felhasználók vagy az alkalmazottak kapcsolatos információkat tárolja, például UserID lehet egy jó PartitionKey. Előfordulhat, hogy több entitás, amely egy adott felhasználói azonosítóját használják hello partíciós kulcs. Minden entitás, amely a felhasználó adatait tárolja egyetlen partícióra vannak csoportosítva, és ezért ezeket az entitásokat elérhetők entitás csoport tranzakciók, miközben továbbra is magas szinten méretezhető.
> 
> 

Nincsenek további szempontok a választott **PartitionKey** toohow fog beszúrási, frissítési és törlési entitások kapcsolódó: hello című [adatmódosítás kialakítása](#design-for-data-modification) alatt.  

### <a name="optimizing-queries-for-hello-table-service"></a>A Table szolgáltatás hello lekérdezések optimalizálása
Table szolgáltatás hello automatikusan elvégzi az entitások hello segítségével **PartitionKey** és **RowKey** értékeket egy fürtözött index, ezért hello oka, hogy a pont lekérdezések vannak hello leghatékonyabb toouse . Van azonban nem fürtözött index hello hello eltérő indexei **PartitionKey** és **RowKey**.

Sok meg kell felelnie a követelmények tooenable keresési entitások több feltétel alapján. Például az e-mailek alapján alkalmazott entitások keresése Alkalmazottazonosító vagy vezetéknevét. következő hello szakasz minták hello [táblázat kialakítási minta](#table-design-patterns) követelmény az ilyen típusú cím és megkerülő megoldásának végrehajtásához hello tényt, hogy hello Table szolgáltatás nem nyújt másodlagos indexek módjait ismerteti:  

* [Intra-partíció másodlagos index mintát](#intra-partition-secondary-index-pattern) -tárolja a több másolatot minden entitás használatával különböző **RowKey** értékek (hello a partícióra) tooenable gyors és hatékony keresést és a másodlagos rendezési sorrend rendelések használatával különböző **RowKey** értékeket.  
* [Másodlagos helyek közötti partíció index mintát](#inter-partition-secondary-index-pattern) – minden értékekkel különböző RowKey külön partíciók vagy külön táblázatban tooenable gyors entitás több példányát tárolja, és a különböző rendelésekhatékonykereséstésamásodlagosrendezésisorrend**RowKey** értékeket.  
* [Index entitások mintát](#index-entities-pattern) -index entitások tooenable hatékony keresések entitások listájának visszaadó karbantartása.  

### <a name="sorting-data-in-hello-table-service"></a>A Table szolgáltatás hello adatok rendezése
Table szolgáltatás hello növekvő sorrendben rendezve entitásokat ad vissza **PartitionKey** és a majd **RowKey**. Ezek a kulcsok karakterlánc-értékek és tooensure, amely numerikus érték rendezése megfelelően kell tooa rögzített hosszúságú alakíthatja át őket, és nulla kitölti őket. Például ha hello alkalmazott azonosító értékét használja hello **RowKey** megadott egész szám, alkalmazottazonosító átalakíthatja **123** túl**00000123**.  

Számos alkalmazás toouse adatok más-más sorrendben rendezve követelményekkel rendelkezik: az alkalmazottak például rendezést a neve vagy való csatlakozás dátumát. következő hello szakasz minták hello [táblázat kialakítási minta](#table-design-patterns) hogyan tooalternate sorrendjét az entitások cím:  

* [Intra-partíció másodlagos index mintát](#intra-partition-secondary-index-pattern) -minden különböző RowKey értékekkel entitás több példányát tárolja (hello a partícióra) tooenable gyors és hatékony keresést és a másodlagos rendezési sorrend rendelések különböző RowKey értékek használatával.  
* [Másodlagos helyek közötti partíció index mintát](#inter-partition-secondary-index-pattern) – minden különböző RowKey érték használatát külön táblázatban tooenable külön partíciók gyors entitás több példányát tárolja, és hatékony keresést és a másodlagos rendezési sorrend rendelések különböző RowKey használatával értékek.
* [Napló végéről mintát](#log-tail-pattern) -lekérése hello  *n*  entitások legutóbb hozzáadott tooa partíció használatával egy **RowKey** érték, amely fordított dátum és idő sorrendben rendezi.  

## <a name="design-for-data-modification"></a>Adatmódosítás kialakítása
Ez a szakasz hello kialakítási szempontjai a beszúrások, a frissítések optimalizálása összpontosít, és törli. Néhány esetben szüksége lesz a tooevaluate hello kompromisszum közötti terveket, terveket, hasonlóan a relációs adatbázisok terveket (bár hello technikák kezelésére szolgáló hello tervezési optimalizálás adatok módosítása ellen lekérdezése optimalizálása kompromisszumot eltérő módon jelennek meg a relációs). a szakasz hello [táblázat kialakítási minta](#table-design-patterns) néhány részletes tervminták hello Table szolgáltatás ismerteti, és kiemeli a néhányat ezek kompromisszumot. A gyakorlatban találja, hogy az entitás lekérdezése optimalizált sok tervek is alkalmas entitások módosítása.  

### <a name="optimizing-hello-performance-of-insert-update-and-delete-operations"></a>Hello teljesítmény optimalizálása a beszúrási, frissítési és törlési műveletek
tooupdate vagy törölni egy entitást, tooidentify képesnek kell lennie az hello segítségével **PartitionKey** és **RowKey** értékeket. Ebben a tekintetben a választott **PartitionKey** és **RowKey** entitások módosítását érdemes követnie a hasonló feltételek tooyour választott toosupport lekérdezések mutasson, mivel tooidentify szervezetek, mint a lehető leghatékonyabb módon. Nem szeretné, hogy egy hatékony partíció vagy tábla vizsgálat toolocate rendelés toodiscover hello egy entitás toouse **PartitionKey** és **RowKey** értékek tooupdate kell, vagy törölje azt.  

következő hello szakasz minták hello [táblázat kialakítási minta](#table-design-patterns) cím optimalizálás hello teljesítmény vagy a beszúrási, frissítési és törlési műveletek:  

* [Nagy mennyiségű törlése mintát](#high-volume-delete-pattern) -Enable hello törlésének nagyszámú egyidejű törlésre entitásokhoz hello tárolása saját külön táblázatban entitások; hello tábla törlésével hello entitások törlésére.  
* [Adatsorozat adatmintát](#data-series-pattern) -tároló teljes adatsorozat elvégezte kérelmek egyetlen entitás toominimize hello számos.  
* [Széles entitások mintát](#wide-entities-pattern) -több fizikai entitások toostore logikai entitás legfeljebb 252 tulajdonságot használja.  
* [Nagy entitások mintát](#large-entities-pattern) -blob storage toostore nagy tulajdonság értékek használata.  

### <a name="ensuring-consistency-in-your-stored-entities"></a>A tárolt entitásokat következetes biztosítása
más kulcsfontosságú tényező, amely befolyásolja a kulcsokat a választott érték módosítása optimalizálása hello hogyan tooensure konzisztencia atomi tranzakciók használatával. Csak egy EGT toooperate használhatja hello tárolt entitásokat tartalmazó partícióra.  

következő hello szakasz minták hello [táblázat kialakítási minta](#table-design-patterns) cím konzisztencia kezelése:  

* [Intra-partíció másodlagos index mintát](#intra-partition-secondary-index-pattern) -tárolja a több másolatot minden entitás használatával különböző **RowKey** értékek (hello a partícióra) tooenable gyors és hatékony keresést és a másodlagos rendezési sorrend rendelések használatával különböző **RowKey** értékeket.  
* [Másodlagos helyek közötti partíció index mintát](#inter-partition-secondary-index-pattern) – minden értékekkel különböző RowKey külön partíciók vagy külön táblázatban tooenable gyors entitás több példányát tárolja, és a különböző rendelésekhatékonykereséstésamásodlagosrendezésisorrend**RowKey** értékeket.  
* [Idővel konzisztenssé tranzakciók mintát](#eventually-consistent-transactions-pattern) -engedélyezni a idővel konzisztenssé partícióhatárok vagy tárolási rendszer határok Azure üzenetsorok használatával.
* [Index entitások mintát](#index-entities-pattern) -index entitások tooenable hatékony keresések entitások listájának visszaadó karbantartása.  
* [Denormalization mintát](#denormalization-pattern) -egyesítése kapcsolódó adatok együtt egy egyetlen entitás tooenable a tooretrieve, az összes hello egyetlen pont lekérdezéssel szükséges adatokat.  
* [Adatsorozat adatmintát](#data-series-pattern) -tároló teljes adatsorozat elvégezte kérelmek egyetlen entitás toominimize hello számos.  

Entitás csoport tranzakciókkal kapcsolatos információkért lásd: hello szakasz [entitás csoport tranzakciók](#entity-group-transactions).  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>A módosítások hatékony kialakítása biztosítása elősegíti a hatékony lekérdezések
Sok esetben egy tervezési módosítások hatékony, de hatékony lekérdező eredményez kell mindig mérlegelje, hogy az adott forgatókönyv szerint hello eset. Néhány hello minták hello szakaszban [táblázat kialakítási minta](#table-design-patterns) kérdez le, és módosítja az entitások közötti kompromisszumot explicit módon kiértékeléséhez és hello számát az egyes művelet mindig kell figyelembe.  

következő hello szakasz minták hello [táblázat kialakítási minta](#table-design-patterns) hatékony lekérdezések tervezése és kialakítása hatékony adatok módosítása a közötti kompromisszumot cím:  

* [Összetett kulcs mintát](#compound-key-pattern) – használható összetett **RowKey** értékek tooenable egy ügyfél toolookup kapcsolódó adatok egyetlen pont lekérdezéssel.  
* [Napló végéről mintát](#log-tail-pattern) -lekérése hello  *n*  entitások legutóbb hozzáadott tooa partíció használatával egy **RowKey** érték, amely fordított dátum és idő sorrendben rendezi.  

## <a name="encrypting-table-data"></a>Tábla adatok titkosítása
hello .NET Azure Storage ügyféloldali kódtár által támogatott titkosítási insert karakterlánc entitás tulajdonságait, és cserélje le a műveleteket. Titkosított hello karakterláncok bináris tulajdonságként hello szolgáltatásban tárolja, és a visszafejtés után hátsó toostrings telepítésekké lesznek átalakítva.    

A táblák, továbbá toohello titkosítási házirendnek, felhasználók meg kell adnia hello tulajdonságok toobe titkosítva. Ezt megteheti megadásával vagy (az POCO entitások, amelyek a TableEntity) [EncryptProperty] attribútum vagy egy titkosítási feloldó lehetőségek. Egy titkosítási feloldó egy delegált veszi a partíciós kulcs, sorkulcsot és tulajdonság nevét, és logikai érték beolvasása, amely jelzi, hogy a tulajdonság titkosítani kell-e. Titkosítás során hello ügyféloldali kódtár fogja használni a információk toodecide e tulajdonság titkosítani toohello vezetékes írása közben. hello delegált is körül hogyan tulajdonságok vannak titkosítva logika hello lehetőségét biztosítja. (Például, ha X, majd titkosítása A tulajdonság; ellenkező esetben a Tulajdonságok A és b titkosítása) Vegye figyelembe, hogy az nem szükséges tooprovide ezen információ olvasása vagy entitás lekérdezése közben.

Vegye figyelembe, hogy a merge jelenleg nem támogatott. Mivel a Tulajdonságok részhalmazát, előfordulhat, hogy korábban használatával titkosított egy másik kulcsot, egyszerűen hello új tulajdonságok egyesítése és hello metaadatainak frissítése adatok elvesztését eredményezi. Így további service hívásai tooread hello már meglévő entitás hello szolgáltatást, vagy tulajdonságonként egy új kulcsot használ, amelyek mindegyikét nem alkalmasak a teljesítményre vonatkozó megfontolásból vagy egyesítés van szükség.     

További információ a táblabeli adatok titkosítása: [ügyféloldali titkosítás és a Microsoft Azure tárolás az Azure Key Vault](../storage/common/storage-client-side-encryption.md).  

## <a name="modelling-relationships"></a>Kapcsolatok modellezésére
Összetett rendszerek hello tervezési kulcsfontosságú lépés létrehozási modelleket tartomány. Általában folyamat tooidentify entitások modellezés hello használata, hello kapcsolatai őket egy módon toounderstand hello üzleti tartományhoz, majd értesíteni kell a hello tervezési a rendszer. Ez a szakasz összpontosít hogyan tudja lefordítani néhány hello közös kapcsolattípusok tartomány modellek toodesigns a Table szolgáltatás hello található. egy logikai adatmodell tooa fizikai alapú NoSQL-adatmodell-leképezés hello folyamat nagyon eltér a relációs tervezése során használt. Relációs adatbázisok terv általában azt feltételezi, hogy a normalizálási adatfeldolgozás minimalizálja a redundancia – és egy deklaratív lekérdező képesség, amely a kivonatok hogyan hello hello adatbázis működésének végrehajtásának optimalizálva.  

### <a name="one-to-many-relationships"></a>Egy-a-többhöz kapcsolatok
Egy-a-többhöz kapcsolatok üzleti tartomány objektumok közötti nagyon gyakran előfordulnak: például egy részleg rendelkezik sok alkalmazott. Hello tábla minden egyes rendelkező szolgáltatás előnyei és hátrányai, lehet, hogy megfelelő toohello adott forgatókönyv számos módon tooimplement egy-a-többhöz kapcsolatok találhatók.  

Vegye figyelembe a hello példa egy nagy több nemzeti vállalat osztályai és ahol minden részlege rendelkezik sok alkalmazott, és minden alkalmazott, egy adott részleg társított alkalmazott entitások tízezreit tartalmazó. Egy megoldás, toostore külön részleg és az alkalmazott entitások, például ezek:  

![][1]

Ez a példa bemutatja egy implicit egy-a-többhöz kapcsolat hello alapján hello típus között **PartitionKey** érték. Minden részleghez rendelkezhet sok alkalmazott.  

Ebben a példában is látható egy szervezeti egység és a kapcsolódó alkalmazott entitásának hello egyazon partícióra kerüljenek. Megadhatja, hogy toouse különböző partíciók, táblák vagy hello különböző entitástípusok még tárfiókját.  

Egy másik módszert is van, az adatok és áruházbeli csak alkalmazott entitások denormalizált részleg adatokkal toodenormalize vagy hello a következő példában látható. Adott esetben ez a megközelítés denormalizált nem lehet ajánlott hello Ha egy követelmény toobe képes toochange hello részleteit a részleg vezetője mivel toodo ez tooupdate minden alkalmazott hello részleg van szüksége.  

![][2]

További információkért lásd: hello [Denormalization mintát](#denormalization-pattern) című útmutatóban.  

hello következő táblázat összefoglalja hello előnyei és hátrányai minden alkalmazott, és a részleg entitások, amelyek egy-a-többhöz tárolását a fent vázolt hello módszerekkel. Érdemes azt is, hogy várhatóan milyen gyakran tooperform különféle műveletek: lehet, hogy elfogadható toohave, olcsóbbá műveletet tartalmaz, ha ez a művelet csak akkor fordul elő, a ritkán kialakítást.  

<table>
<tr>
<th>Módszer</th>
<th>Informatikai szakemberek</th>
<th>Hátrányok</th>
</tr>
<tr>
<td>Külön entitástípusok, egyazon partícióra kerüljenek, ugyanahhoz a táblához</td>
<td>
<ul>
<li>A részleg entitás egyetlen művelettel frissítheti.</li>
<li>Egy EGT toomaintain konzisztencia használhatja, ha egy követelmény toomodify egy részleg entitás amikor Ön frissítés/Beszúrás/törlés alkalmazott entitás. Például ha a részlegszintű alkalmazott száma minden részleg számára.</li>
</ul>
</td>
<td>
<ul>
<li>Szükség lehet tooretrieve mind az alkalmazott, és a részleg entitás bizonyos ügyfél tevékenységek.</li>
<li>Tárolási műveletekre kerül sor a hello azonos partíció. A magas tranzakció kötetek Ez azt eredményezheti, interaktív terület.</li>
<li>Egy alkalmazott tooa új osztály használatával egy EGT nem helyezhető át.</li>
</ul>
</td>
</tr>
<tr>
<td>Külön entitástípusok, a különböző partíciók vagy a táblák vagy a storage-fiókok</td>
<td>
<ul>
<li>A részleg entitás vagy az alkalmazott entitás egyetlen művelettel frissítheti.</li>
<li>A magas tranzakció kötetek, ez segíthet terjedésének hello betöltése között több partíciót.</li>
</ul>
</td>
<td>
<ul>
<li>Szükség lehet tooretrieve mind az alkalmazott, és a részleg entitás bizonyos ügyfél tevékenységek.</li>
<li>Nem használhat EGTs toomaintain konzisztencia amikor Ön frissítés/Beszúrás/Törlés az alkalmazottak és a frissítési részleget. Például frissítése folyamatban van egy szervezeti egységben egy alkalmazott száma.</li>
<li>Egy alkalmazott tooa új osztály használatával egy EGT nem helyezhető át.</li>
</ul>
</td>
</tr>
<tr>
<td>Az egyetlen entitás típusa denormalize</td>
<td>
<ul>
<li>Az egy kérelemhez szükséges összes hello információkat kérheti le.</li>
</ul>
</td>
<td>
<ul>
<li>Költséges toomaintain konzisztencia lehet, ha (Ez igényelnének, tooupdate egy osztály összes hello alkalmazott) tooupdate részleg információra van szüksége.</li>
</ul>
</td>
</tr>
</table>

Hogyan választja, ezeket a beállításokat, és amely hello szakemberek között, és hátrányának legjelentősebb, amelyek az adott alkalmazás forgatókönyvek függ. Például milyen gyakran tegye módosítása részleg entitások; szükség van hello kiegészítő részlegre vonatkozó információkat; az alkalmazott lekérdezés milyen közel vannak, a partíciók vagy a tárfiók toohello méretezhetőségének korlátai?  

### <a name="one-to-one-relationships"></a>-Az-egyhez kapcsolat
Tartomány modellek egy az egyhez típusú entitások közötti kapcsolatok is tartalmazhat. Ha egy az egyhez kapcsolat a Table szolgáltatás hello tooimplement van szüksége, is választania kell hogyan toolink hello-e két kapcsolódó entitás újból tooretrieve is. Ez a hivatkozás lehet implicit, egy egyezmény hello értékek alapján, vagy explicit hivatkozás tárolása hello formája **PartitionKey** és **RowKey** minden entitás tooits szereplő értékek kapcsolódó entitás. E hello tárolja az információt a kapcsolódó entitás a hello partícióra, hello című [egy-a-többhöz kapcsolatok](#one-to-many-relationships).  

Vegye figyelembe, hogy nincsenek-e is vezethet, tooimplement-az-egyhez kapcsolat hello tábla szolgáltatás megvalósítási szempontot:  

* Nagy entitások kezelése (további információkért lásd: [nagy entitások mintát](#large-entities-pattern)).  
* A végrehajtási hozzáférés-vezérlést (további információkért lásd: [megosztott hozzáférési aláírásokkal-hozzáférés szabályozásáról](#controlling-access-with-shared-access-signatures)).  

### <a name="join-in-hello-client"></a>Hello ügyfél csatlakozott
Bár a Table szolgáltatás hello módon toomodel kapcsolatokat, kell nem elfelejti, hogy hello két elsődleges hello Table szolgáltatás használatára vonatkozó okai méretezhetőséget és teljesítményt nyújt. Ha meg vannak modellezés több kapcsolatot, amelyek veszélyeztetik hello teljesítményét és méretezhetőségét, a megoldás, kérdezze meg saját kezűleg szükséges toobuild esetén az összes hello adatok kapcsolatok tábla rendszerbe. Előfordulhat, hogy képes toosimplify hello tervezési kell és hello méretezhetőség és a megoldás teljesítményének javítása, ha az ügyfélalkalmazást, hajtsa végre a szükséges illesztések.  

Például ha adatokat tartalmaznak, amelyek nem változik gyakran nagyon kis táblákhoz, majd után lekérdezhetik ezeket az adatokat többször is hello ügyfélen gyorsítótárazása azt. Ennek elkerülése érdekében ismételt használatával tooretrieve hello ugyanazokat az adatokat. Azt kell venni, a jelen útmutató hello példákban hello kis szervezet szervezeti egységek lesz valószínűleg toobe kis és ritkán így egy jó jelölt az ügyfélalkalmazás tölthetik le egyszer adatok és a gyorsítótár, keresse meg a adatok módosítása.  

### <a name="inheritance-relationships"></a>Öröklődés kapcsolatok
Ha az ügyfél alkalmazás osztályok az öröklési kapcsolat toorepresent üzleti entitásokat részét alkotó készlete, egyszerűen a ezeket a Table szolgáltatás hello entitások maradnak meg. Például lehetséges, hogy az ügyfél alkalmazásban megadott készlete a következő hello ahol **személy** absztrakt osztály.

![][3]

A Table szolgáltatás segítségével, hogy a hely egyetlen személy tábla használatával hello tényleges osztály két hello példányai is továbbra is fennáll:  

![][4]

Ugyanaz a tábla az Ügyfélkód hello több entitástípusok kezelésével kapcsolatos további információkért lásd: hello szakasz [használata heterogén entitástípusok](#working-with-heterogeneous-entity-types) című útmutatóban. Ez példákat hogyan toorecognize hello az Ügyfélkód entity Type típusként.  

## <a name="table-design-patterns"></a>Táblázat kialakítási minták
A korábbi szakaszokban néhány hogyan toooptimize a táblázat kialakítási mindkét lekérése entitás adatok lekérdezésekkel és beszúrni, frissítése és törlése Entitásadatok kapcsolatos részletes beszélgetéseket láthatta. Ez a szakasz ismerteti az egyes mintázatok megfelelő Table szolgáltatási megoldások való használatra. Ezenkívül láthatja, hogyan meg is gyakorlatilag cím egyes hello problémák és kompromisszumot alakítson ki az útmutatóban korábban következik be. hello következő diagram foglalja hello kapcsolatai hello különböző minták:  

![][5]

hello mintát térkép fent néhány minták (kék) és a jelen útmutatóban leírt elleni minták (narancs) közötti kapcsolatokat mutatja be. Természetesen nincsenek sok más mintákat, amelyek érdemes figyelembe véve. Például hello főbb forgatókönyvek a Table szolgáltatás egyik toouse hello [materializált nézet mintát](https://msdn.microsoft.com/library/azure/dn589782.aspx) a hello [parancs lekérdezés felelősségi elkülönítése (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) mintát.  

### <a name="intra-partition-secondary-index-pattern"></a>Helyen belüli-partíció másodlagos index minta
Több másolatot minden entitás használatával különböző tárolására **RowKey** értékek (hello a partícióra) segítségével különböző rendelések tooenable gyors és hatékony keresést és a másodlagos rendezési sorrend **RowKey** értékek. Példányok közötti frissítéseket is konzisztens EGT tartozó használatával.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Table szolgáltatás hello automatikusan elvégzi a hello segítségével **PartitionKey** és **RowKey** értékeket. Ez lehetővé teszi, hogy egy ügyfél alkalmazás tooretrieve egy entitás hatékonyan végez ezeket az értékeket. Például a lent látható módon hello táblaszerkezet, ügyfélalkalmazás használatával egy pont lekérdezés tooretrieve alkalmazott entitás hello részleg neve és hello alkalmazott azonosítója (hello **PartitionKey** és  **RowKey** értékek). Egy ügyfél is kérje le az entitásokat alkalmazottazonosító belül minden részleg szerint rendezve.

![][6]

Ha szeretné, hogy toobe képes toofind hello egy másik tulajdonság értéke, például az e-mail cím alapján alkalmazott entitás egy kevésbé hatékony partíció vizsgálat toofind egyezés kell használnia. Ennek az az oka hello table szolgáltatás nem biztosít a másodlagos kulcsot. Emellett nincs nincs lehetőség toorequest az alkalmazottakat, mint egy másik sorrendbe rendezve **RowKey** sorrendje.  

#### <a name="solution"></a>Megoldás
másodlagos indexek hello hiánya körül toowork, minden egyes másolataihoz, a különböző entitás több példányát tárolhatja **RowKey** érték. Ha egy entitás tárolja az alább látható hello struktúrák, e-mail cím vagy az alkalmazott azonosítója alapján alkalmazott entitások hatékonyan kérheti le. hello előtag értékeinek hello **RowKey**, "empid_" és "email_" lehetővé teszik a tooquery egyetlen alkalmazott vagy alkalmazottak számos e-mail címek vagy alkalmazott azonosítók használatával.  

![][7]

a következő két szűrési feltételeket (egy keresésekor által alkalmazott azonosítója, a másik e-mail cím alapján keresése) hello is adja meg a pont lekérdezéseket:  

* $filter = (PartitionKey eq 'Értékesítés') és (RowKey eq "empid_000223")  
* $filter = (PartitionKey eq 'Értékesítés') és (RowKey eq 'email_jonesj@contoso.com")  

Ha alkalmazott entitástartományának kérdezze le, megadhatja a alkalmazott azonosítója sorrendbe rendezve tartománya vagy egy tartományt a hello hello a megfelelő előtaggal rendelkező entitások lekérdezésével e-mail cím sorrendbe rendezve **RowKey**.  

* toofind összes hello hello értékesítési részleg a hello tartomány 000100 too000199 alkalmazott azonosítójú munkavállalók: $filter = (PartitionKey eq 'Értékesítés') és (RowKey ge "empid_000100") és (RowKey le "empid_000199")  
* összes hello értékesítési osztályon dolgozók hello hello kezdve e-mail címmel rendelkező toofind levél "a" használja: $filter = (PartitionKey eq 'Értékesítés') és (RowKey ge "email_a") és (RowKey lt "email_b")  
  
  Vegye figyelembe, hogy a fenti példákban hello használt hello szűrőszintaxisának hello Table szolgáltatás REST API-t a további tudnivalókat lásd a [lekérdezés entitások](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* A TABLE storage viszonylag olcsó toouse, így a terhelést növelni az ismétlődő adatok tárolására költség hello nem lehet fő szempont. Azonban kell kiértékelésének eredménye mindig a várható tárolási szükségletek előkészítését hello költség, és csak hozzá kettős bejegyzés toosupport hello lekérdezések végrehajtja az ügyfélalkalmazást.  
* Mivel hello másodlagos index entitások hello azonos hello eredeti entitásként partíció található, ellenőrizze, hogy nem haladhatja meg a hello méretezhetőségi célok az egyes partíciók.  
* Beállíthatja, hogy az ismétlődő entitások egymással konzisztenssé EGTs tooupdate hello két példányban hello entitás i használatával. Ez azt jelenti, hogy egy entitás összes másolatát tárolja hello egyazon partícióra kerüljenek. További információkért lásd: hello szakasz [entitás csoport tranzakciók használatával](#entity-group-transactions).  
* hello használt érték hello **RowKey** minden egyes entitásnál egyedinek kell lennie. Érdemes lehet összetett kulcs értékeket.  
* Kitöltési hello szerepel benne numerikus érték **RowKey** (például hello alkalmazottazonosító 000223), lehetővé teszi, hogy javítsa ki, rendezési és szűrési alapján felső és alsó határait.  
* Nem feltétlenül kell tooduplicate az entitás összes hello tulajdonságait. Például ha hello lekérdezéseiben, amelyeket keresési hello entitásnak hello segítségével e-mail-címre a hello **RowKey** soha nem kell hello alkalmazott életkora, ezeket az entitásokat hello struktúra a következő lehet:

![][8]

* Általában jobb toostore ismétlődő adatokat, és győződjön meg arról, hogy egyetlen lekérdezést a szükséges összes hello adatok kérheti le, mint toouse egy lekérdezést toolocate entitás és egy másik toolookup hello szükséges adatokat.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Ezt a mintát használja, ha az ügyfélalkalmazást számos különböző kulcsok használatát, ha az ügyfélnek kell eltérő rendezési sorrend tooretrieve entitásának tooretrieve entitásokat, és ahol minden entitás egyedi értékeket számos segítségével azonosíthatja. Azonban ügyelnie kell, hogy Ön nem lépik túl hello partíció méretezhetőségi korlátok különböző hello segítségével entitás keresések végrehajtásakor **RowKey** értékeket.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Másodlagos helyek közötti partíció index minta](#inter-partition-secondary-index-pattern)
* [Összetett kulcs minta](#compound-key-pattern)
* [Entitás csoport tranzakciók](#entity-group-transactions)
* [Heterogén entitástípusok használata](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>Másodlagos helyek közötti partíció index minta
Több másolatot minden entitás használatával különböző tárolására **RowKey** külön partíciók vagy külön táblázatban tooenable gyors és hatékony keresést és a másodlagos rendezési sorrend használatával különböző értékek **RowKey**értékeket.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Table szolgáltatás hello automatikusan elvégzi a hello segítségével **PartitionKey** és **RowKey** értékeket. Ez lehetővé teszi, hogy egy ügyfél alkalmazás tooretrieve egy entitás hatékonyan végez ezeket az értékeket. Például a lent látható módon hello táblaszerkezet, ügyfélalkalmazás használatával egy pont lekérdezés tooretrieve alkalmazott entitás hello részleg neve és hello alkalmazott azonosítója (hello **PartitionKey** és  **RowKey** értékek). Egy ügyfél is kérje le az entitásokat alkalmazottazonosító belül minden részleg szerint rendezve.  

![][9]

Ha szeretné, hogy toobe képes toofind hello egy másik tulajdonság értéke, például az e-mail cím alapján alkalmazott entitás egy kevésbé hatékony partíció vizsgálat toofind egyezés kell használnia. Ennek az az oka hello table szolgáltatás nem biztosít a másodlagos kulcsot. Emellett nincs nincs lehetőség toorequest az alkalmazottakat, mint egy másik sorrendbe rendezve **RowKey** sorrendje.  

Ezeket az entitásokat tranzakciókról nagyon nagy mennyiségű rendszer előrejelző, és szeretné, hogy hello Table szolgáltatás szabályozása az ügyfél toominimize hello kockázatát.  

#### <a name="solution"></a>Megoldás
másodlagos indexek hello hiánya körül toowork, több másolatot minden entitás, minden példány használatával különböző tárolhatja **PartitionKey** és **RowKey** értékeket. Ha egy entitás tárolja az alább látható hello struktúrák, e-mail cím vagy az alkalmazott azonosítója alapján alkalmazott entitások hatékonyan kérheti le. hello előtag értékeinek hello **PartitionKey**, "empid_" és "email_" lehetővé teszik a index, amely tooidentify toouse szeretné, hogy egy lekérdezéshez.  

![][10]

a következő két szűrési feltételeket (egy keresésekor által alkalmazott azonosítója, a másik e-mail cím alapján keresése) hello is adja meg a pont lekérdezéseket:  

* $filter = (PartitionKey eq ' empid_Sales") és (RowKey eq"000223")
* $filter = (PartitionKey eq ' email_Sales") és (RowKey eq 'jonesj@contoso.com")  

Ha alkalmazott entitástartományának kérdezze le, megadhatja a alkalmazott azonosítója sorrendbe rendezve tartománya vagy egy tartományt a hello hello a megfelelő előtaggal rendelkező entitások lekérdezésével e-mail cím sorrendbe rendezve **RowKey**.  

* összes alkalmazottja hello toofind hello értékesítési részleg hello közé alkalmazott azonosítójú **000100** túl**000199** alkalmazott azonosítója sorrendben használja rendezve: $filter = (PartitionKey eq ' empid_Sales") és (RowKey ge" 000100') és (RowKey le "000199")  
* toofind hello értékesítési részleg a rendezett "a" karakterlánccal kezdődik e-mail címmel rendelkező összes hello alkalmazott e-mail cím sorrendben használja: $filter = (PartitionKey eq ' email_Sales") és (RowKey ge"a") és (RowKey lt"b")  

Vegye figyelembe, hogy a fenti példákban hello használt hello szűrőszintaxisának hello Table szolgáltatás REST API-t a további tudnivalókat lásd a [lekérdezés entitások](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Hálózati adaptere esetében megtarthatja az ismétlődő entitások idővel konzisztenssé egymással hello segítségével [idővel konzisztenssé tranzakciók mintát](#eventually-consistent-transactions-pattern) toomaintain hello elsődleges és másodlagos index entitásokat.  
* A TABLE storage viszonylag olcsó toouse, így a terhelést növelni az ismétlődő adatok tárolására költség hello nem lehet fő szempont. Azonban kell kiértékelésének eredménye mindig a várható tárolási szükségletek előkészítését hello költség, és csak hozzá kettős bejegyzés toosupport hello lekérdezések végrehajtja az ügyfélalkalmazást.  
* hello használt érték hello **RowKey** minden egyes entitásnál egyedinek kell lennie. Érdemes lehet összetett kulcs értékeket.  
* Kitöltési hello szerepel benne numerikus érték **RowKey** (például hello alkalmazottazonosító 000223), lehetővé teszi, hogy javítsa ki, rendezési és szűrési alapján felső és alsó határait.  
* Nem feltétlenül kell tooduplicate az entitás összes hello tulajdonságait. Például ha hello lekérdezéseiben, amelyeket keresési hello entitásnak hello segítségével e-mail-címre a hello **RowKey** soha nem kell hello alkalmazott életkora, ezeket az entitásokat hello struktúra a következő lehet:
  
  ![][11]
* Általában jobb toostore ismétlődő adatokat, és győződjön meg arról, hogy minden hello a szükséges adatok egyetlen lekérdezést, mint egy entitás használatával toouse egy lekérdezést toolocate hello másodlagos index kérheti le, és egy másik toolookup hello hello elsődleges index szükséges adatok.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Ezt a mintát használja, ha az ügyfélalkalmazást számos különböző kulcsok használatát, ha az ügyfélnek kell eltérő rendezési sorrend tooretrieve entitásának tooretrieve entitásokat, és ahol minden entitás egyedi értékeket számos segítségével azonosíthatja. Ebben a mintában használja, ha azt szeretné, hogy hello partíció méretezhetőségi korlátok meghaladja a különböző hello segítségével entitás keresések végrehajtásakor tooavoid **RowKey** értékeket.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Idővel konzisztenssé tranzakciók minta](#eventually-consistent-transactions-pattern)  
* [Helyen belüli-partíció másodlagos index minta](#intra-partition-secondary-index-pattern)  
* [Összetett kulcs minta](#compound-key-pattern)  
* [Entitás csoport tranzakciók](#entity-group-transactions)  
* [Heterogén entitástípusok használata](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Idővel konzisztenssé tranzakciók minta
Engedélyezze a idővel konzisztenssé viselkedés partícióhatárok vagy tárolási rendszer határok az Azure-üzenetsorok használatával.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
EGTs atomi tranzakciók engedélyezése között, amelyek közös hello több entitás ugyanazzal a partíciókulccsal. A teljesítmény és méretezhetőség érdekében dönthet úgy, hogy külön partíciók vagy egy különálló tárhelyet rendszer konzisztencia-követelményekkel rendelkező entitások toostore: ilyen esetben EGTs toomaintain konzisztencia nem használható. Például lehetséges, hogy egy követelmény toomaintain végleges konzisztencia között:  

* Két különböző partíciók hello tárolt entitásokat azonos tábla, a különböző táblák, a különböző tárfiókokban.  
* Egy entitás hello Table szolgáltatás tárolja, és a blob hello Blob szolgáltatásban tárolja.  
* Egy entitás tárolt hello Table szolgáltatás és a fájl a fájlrendszerben.  
* Egy entitás hello Table szolgáltatás tárolása, még indexelt hello Azure Search szolgáltatás használatával.  

#### <a name="solution"></a>Megoldás
Azure üzenetsorok használata esetén alkalmazhat olyan megoldás, amely továbbítja a végleges konzisztencia két vagy több partíciót, vagy tárolási rendszerek között.
tooillustrate ez közelítse, során feltételezzük, hogy egy követelmény toobe képes tooarchive régi alkalmazott entitásokat. Régi alkalmazott entitások ritkán a rendszer megkérdezi, és olyan tevékenységet, amely az aktuális alkalmazottak kezelésére ki kell zárni. tooimplement hello aktív tárolja ezt a követelményt **aktuális** tábla és a régi dolgozók hello **archív** tábla. Egy alkalmazott archiválás szükséges toodelete hello entitásának hello **aktuális** tábla, és adja hozzá a hello entitás toohello **archív** táblában, de nem használható egy EGT tooperform két művelet. tooavoid hello kockázata, hogy hibát okoz, egy entitás tooappear mindkét vagy egyiket se táblák, hello archiválási művelet idővel konzisztenssé kell lennie. hello következő Szekvenciadiagram lépéseit sorolja fel hello ebben a műveletben. Kivétel elérési utak, a következő hello szöveg biztosított további információkhoz juthat.  

![][12]

Egy Azure-üzenetsorba, ez például tooarchive alkalmazotti #456 helyez el egy üzenetet az ügyfél hello archiválási művelet kezdeményez. A feldolgozói szerepkör kérdezze le az új üzenetek; hello várólista Ha talál egyet, üdvözlőüzenetére olvas, és hagyja rejtett másolatot hello a várakozási. hello feldolgozói szerepkör ezután lekéri hello entitás másolatát hello **aktuális** table, szúrja be a másolási hello **archív** tábla, és törli hello hello az eredeti **aktuális**tábla. Végül ha nincs hiba hello előző lépéseiből, hello feldolgozói szerepkör törli rejtett üdvözlőüzenetére hello üzenetsorból.  

Ebben a példában a 4. lépés hello alkalmazott illeszt hello **archív** tábla. Hello alkalmazott tooa blob a Blob szolgáltatás hello vagy annak egy fájljához hozzáadhatja, a fájlrendszer.  

#### <a name="recovering-from-failures"></a>Végezze el a hibák
Fontos, hogy hello lépéseket az operations **4** és **5** kell lennie *idempotent* abban az esetben hello feldolgozói szerepkör kell toorestart hello archiválási művelet. Ha hello Table szolgáltatás lépés az **4** kell használnia az "insert vagy cserélje le a" művelet; lépés **5** kell használnia egy "törlése, ha létezik-e" hello ügyféloldali kódtár használata a műveletet. Ha egy másik tárolási rendszert használ, egy megfelelő idempotent műveletet kell használnia.  

Ha hello feldolgozói szerepkör soha nem fejeződik be a lépés **6**, majd hello időtúllépési üzenet ismét megjelenik, hello várólista után készen áll a hello feldolgozói szerepkör tootry tooreprocess azt. hello feldolgozói szerepkör ellenőrizheti egy hello várólista olvasták futtatásainak számát, és szükség esetén egy "poison" üzenetet, hogy meg lehessen vizsgálni tooa elküldésével jelző külön várólista. További információ az üzenetek olvasásakor és hello ellenőrzése created száma, lásd: [üzeneteket beolvasni](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Bizonyos hibák hello tábla és a Queue szolgáltatások átmeneti hibák, és az ügyfélalkalmazás tartalmaznia kell a megfelelő újrapróbálkozási logika toohandle őket.  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Ez a megoldás elkülönítési tranzakció nem ad meg. Például egy ügyfél olvasható hello **aktuális** és **archív** állapotában hello feldolgozói szerepkör lépések között táblák **4** és **5**, és tekintse meg egy hello adatok inkonzisztens nézetét. Vegye figyelembe, hogy hello adatok konzisztens idővel.  
* Meg arról, hogy 4. és 5 rendelés tooensure végleges konzisztencia az idempotent kell lennie.  
* Több várólisták és feldolgozópéldányok szerepkör segítségével méretezhető hello megoldás.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Ezt a mintát használja, ha azt szeretné, hogy a végleges konzisztencia tooguarantee entitás, amely szerepel a különböző partíciók, illetve olyan táblázatok között. A minta tooensure végleges konzisztencia műveletekhez kiterjesztése hello tábla és hello Blob szolgáltatás és más Azure Storage adatforrások, például az adatbázis vagy hello fájlrendszer.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Entitás csoport tranzakciók](#entity-group-transactions)  
* [Egyesítés vagy cseréje](#merge-or-replace)  

> [!NOTE]
> Ha a tranzakció elkülönítési fontos tooyour megoldás, érdemes lehet a táblák tooenable újratervezése toouse EGTs meg.  
> 
> 

### <a name="index-entities-pattern"></a>Index entitások minta
Index entitások tooenable hatékony keresések entitások listájának visszaadó karbantartása.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Table szolgáltatás hello automatikusan elvégzi a hello segítségével **PartitionKey** és **RowKey** értékeket. Ez lehetővé teszi, hogy egy ügyfél alkalmazás tooretrieve hatékonyan pont lekérdezéssel entitás. Például alább látható hello táblaszerkezet használ, egy ügyfélalkalmazás hatékonyan alkalmazott entitás használatával lekérhető hello részleg neve és hello alkalmazott azonosítója (hello **PartitionKey** és **RowKey** ).  

![][13]

Ha toobe képes tooretrieve alkalmazott entitások listájának hello egy másik nem egyedi tulajdonság értéke, például a vezetéknevét, alapján is érdemes használnia kell a kevésbé hatékony partíció toofind egyező vizsgálata helyett egy index toolook azokat fel közvetlenül. Ennek az az oka hello table szolgáltatás nem biztosít a másodlagos kulcsot.  

#### <a name="solution"></a>Megoldás
tooenable keresési alkalmazott azonosítók listájának kezelnie kell a fent látható hello entitás struktúrájú utolsó név szerint. Ha azt szeretné, hogy tooretrieve hello alkalmazott entitások egy adott nevű utolsó, például a János, először keresse meg az alkalmazott azonosítók hello listáját János a Vezetéknév, az alkalmazottak számára, és az majd lekérheti az alkalmazott entitásokból. Hello alkalmazott azonosítók listájának tárolására három fő lehetőség áll rendelkezésre:  

* A blob storage használata.  
* Hozzon létre indexet entitások azonos partícióazonosító hello alkalmazott entitásként hello.  
* Hozzon létre indexet entitások külön partíció vagy tábla.  

<u>#1. lehetőség: Használja a blob storage</u>  

Hello első lehetőség, létrehoz egy blob minden egyedi Vezetéknév, és minden egyes blob tárolóban hello listája **PartitionKey** (osztály) és **RowKey** (alkalmazott azonosítója) értékeit, amely utoljára rendelkező alkalmazottak név. Adja hozzá, vagy egy alkalmazott törlésekor biztosítania kell, hogy hello hello vonatkozó blob tartalma idővel konzisztenssé váljanak hello alkalmazott entitásokat.  

<u>#2. lehetőség:</u> a Create index entitásokat tartalmazó partícióra hello  

Hello második lehetőség használja a következő adatok hello tárolására szolgáló index entitás:  

![][14]

Hello **EmployeeIDs** hello tárolt hello utolsó nevű tulajdonsága tartalmazza az alkalmazottak számára alkalmazott azonosítók listájának **RowKey**.  

hello lépései hello folyamat felvételekor új alkalmazott hello második lehetőség használata ajánlott. Ebben a példában azt ad hozzá egy alkalmazott azonosítója 000152 és a Vezetéknév János hello értékesítési részleg:  

1. Hello index entitás beolvasni egy **PartitionKey** érték az "Értékesítési" és a hello **RowKey** érték "János." Ez entity toouse az ETag hello mentése a 2.  
2. Hozzon létre egy entitás csoport tranzakció (Ez azt jelenti, hogy a kötegelt művelet) hello új alkalmazott entitás beszúrása (**PartitionKey** "Értékesítési" érték és **RowKey** "000152" értéket), és a frissítések hello index entitás ( **PartitionKey** "Értékesítési" érték és **RowKey** "János" értékre) hello EmployeeIDs mezőben hello új alkalmazott azonosítója toohello lista hozzáadásával. Entitás csoport tranzakciókkal kapcsolatos további információkért lásd: [entitás csoport tranzakciók](#entity-group-transactions).  
3. Ha hello entitás csoport tranzakció (valaki más csak módosította hello index entitás) egyidejű hozzáférések optimista hiba miatt nem sikerül, akkor szüksége toostart keresztül 1. lépés: újra.  

Használhat egy hasonló megközelítés toodeleting egy alkalmazott hello második lehetőség használata. Egy alkalmazott Vezetéknév módosítása nem kicsit bonyolultabb, mert szüksége lesz egy entitás csoport tranzakció, amely három entitások frissíti tooexecute: hello alkalmazott entitás hello index entitás hello régi Vezetéknév és hello index entitás hello új utolsó neve. Minden entitás módosítása előtt sorrendben tooretrieve hello ETag érték majd használható tooperform hello frissítések optimista konkurenciát alkalmazott be kell olvasni.  

hello lépései hello folyamat, ha egy osztály adott utolsó nevű alkalmazottait hello toolook hello második lehetőség használata ajánlott. Ebben a példában is szívesen látjuk hello értékesítési részleg utolsó nevű János alkalmazottait hello:  

1. Hello index entitás beolvasni egy **PartitionKey** érték az "Értékesítési" és a hello **RowKey** érték "János."  
2. Elemezni hello alkalmazott hello EmployeeIDs mezőben azonosítók listáját.  
3. Ha további információ az egyes ezeknek a dolgozóknak (például az e-mail címmel) van szüksége, lekéri az egyes hello alkalmazott entitások használatával **PartitionKey** "Értékesítési" értéket, és **RowKey** közötti értéket 2. lépésben beolvasott alkalmazottak hello listája.  

<u>#3. lehetőség:</u> index entitások külön partíció vagy tábla létrehozása  

Hello harmadik beállítás használja a következő adatok hello tárolására szolgáló index entitás:  

![][15]

Hello **EmployeeIDs** hello tárolt hello utolsó nevű tulajdonsága tartalmazza az alkalmazottak számára alkalmazott azonosítók listájának **RowKey**.  

Hello harmadik beállítás EGTs toomaintain konzisztencia nem használható, mert a hello alkalmazott entitásokból külön partícióra vannak hello index entitásokat. Ellenőrizze, hogy idővel konzisztenssé váljanak hello alkalmazott entitások-e hello index entitásokat.  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Ehhez a megoldáshoz szükségesek entitások megfelelő legalább két lekérdezést tooretrieve: egy tooquery hello index entitások tooobtain hello listája **RowKey** értéket, majd ezután lekérdezi tooretrieve hello listán szereplő minden egyes entitásnál.  
* Fényében, hogy egyes entitás a maximális méret 1 MB, #2. lehetőség van, és a #3. lehetőség hello megoldásban azt feltételezik, hogy hello listája bármely adott Vezetéknév alkalmazott azonosítóinak mindig 1 MB-nál nagyobb. Ha alkalmazott azonosítók listáját hello 1 MB-nál nagyobb valószínűséggel toobe, #1. lehetőség és hello index adatokat a blob Storage tárolóban tárolja.  
* #2. beállítás (EGTs toohandle hozzáadása és az alkalmazottak törlése és módosítása az alkalmazott Vezetéknév használatával) ki kell értékelnie, ha tranzakciók mennyisége hello megközelítést alkalmaz majd hello méretezhetőségi korlátok egy adott partíció. Ha ez helyzet hello, fontolja meg egy idővel konzisztenssé (#1. lehetőség vagy #3. lehetőség) által használt megoldás várólisták toohandle hello frissítés kér, és lehetővé teszi, hogy a hogy toostore az index entitások a hello alkalmazott entitásokból külön partícióra.  
* #2. lehetőség a megoldás feltételezi, hogy toolook fel egy részleg utolsó nevű: például azt szeretné, hogy tooretrieve az alkalmazottak a Vezetéknév János hello értékesítési részleg listáját. Ha azt szeretné, hogy képes toolook toobe hello alkalmazottait nevű utolsó János hello teljes szervezeten belül, #1. lehetőség vagy a #3. lehetőség használatával.
* Megvalósíthat egy várólista-alapú megoldás, amely a végleges konzisztencia biztosítja (lásd: hello [idővel konzisztenssé tranzakciók mintát](#eventually-consistent-transactions-pattern) további részletekért).  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Ezt a mintát használja, ha azt szeretné, hogy az, hogy minden közös közös tulajdonság értéke, például minden alkalmazott Vezetéknév hello János a entitások készletének toolookup.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Összetett kulcs minta](#compound-key-pattern)  
* [Idővel konzisztenssé tranzakciók minta](#eventually-consistent-transactions-pattern)  
* [Entitás csoport tranzakciók](#entity-group-transactions)  
* [Heterogén entitástípusok használata](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Denormalization minta
Egyesíthet egy egyetlen entitás tooenable együtt a kapcsolódó adatokat tooretrieve, az összes hello egyetlen pont lekérdezéssel szükséges adatokat.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Egy relációs adatbázisban akkor általában szabványosíthatja a lekérdezéseiben, amelyeket adatainak lekérése több táblából származó adatok tooremove ismétlődést. Ha normalizálása az adatok Azure-táblákban, meg kell nyitnia a hello ügyfél toohello server tooretrieve több kiszolgálókkal való adatváltások számát a kapcsolódó adatokat. Például az alábbi értékekre, akkor hello táblaszerkezet kell két kerekíteni utazgatással tooretrieve hello részleteit egy részleg: egy toofetch hello részleg entitás, amely tartalmazza egy alkalmazott hello kezelő azonosítója, és a majd egy másik kérelem toofetch hello kezelő részletek az entitás.  

![][16]

#### <a name="solution"></a>Megoldás
Helyett két külön entitás hello adatok tárolását, denormalize hello adatokat, és megőrzi hello manager részletei hello részleg entitásban. Példa:  

![][17]

Ezekkel a tulajdonságokkal tárolt részleg entitásokkal kapcsolatos pont lekérdezéssel részleget szükséges összes hello információt most le.  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Van néhány költsége terhet kétszer néhány adatainak tárolásához. hello teljesítménynövekedést (kevesebb kérelmek toohello társzolgáltatás eredő) általában ez fontosabb, mint hello marginális növekedése tárolási költségek (és az ehhez kapcsolódó költséget részben az eltolás tranzakciók száma hello csökkenése toofetch hello részletek megkövetelése a részleg).  
* Meg kell egységesíthetők hello hello két entitások kezelők adatainak tárolására. Kezelheti hello konzisztencia probléma EGTs tooupdate használatával egy atomi tranzakción belül több entitás: Ebben az esetben hello részleg entitás és hello alkalmazott entitás hello részleg Manager tárolja hello egyazon partícióra kerüljenek.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Ebben a mintában akkor használja, ha gyakran kell toolook kapcsolódó adatokat. Ebben a mintában csökkenti az ügyfél biztosítsa tooretrieve hello adatokat igényel lekérdezések hello száma.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Összetett kulcs minta](#compound-key-pattern)  
* [Entitás csoport tranzakciók](#entity-group-transactions)  
* [Heterogén entitástípusok használata](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Összetett kulcs minta
Használjon összetett **RowKey** értékek tooenable egy ügyfél toolookup kapcsolódó adatok egyetlen pont lekérdezéssel.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Egy relációs adatbázisban elég természetes toouse illesztések lekérdezésekben tooreturn kapcsolatos adatok toohello ügyfél egyetlen lekérdezést adatot. Például használhatja a hello alkalmazott azonosítója toolook kapcsolódó entitásokból, melyek teljesítményét, és tekintse át az adatokat azt listája.  

Tegyük fel, alkalmazott entitások hello Table szolgáltatás a következő struktúra hello segítségével tárolja:  

![][18]

Szükség toostore előzményadatokat tooreviews vonatkozó és teljesítmény minden év hello alkalmazott működött a szervezet számára, és toobe képes tooaccess évente ezekre az információkra szüksége. Egy beállítás toocreate van egy másik tábla entitások tárolja a struktúra a következő hello:  

![][19]

Figyelje meg, hogy ennek közelítse meg dönthet tooduplicate egyes információk (például a Keresztnév és Vezetéknév) hello új entitás tooenable meg tooretrieve az adatok egy kérelemhez. Azonban az erős konzisztencia nem fenntartása, mivel egy EGT tooupdate hello két entitás i nem használható.  

#### <a name="solution"></a>Megoldás
Egy új entitástípus tárolása az eredeti entitások használata hello struktúra a következő táblában:  

![][20]

Figyelje meg, hogyan hello **RowKey** most egy összetett kulcs áll hello alkalmazott- és hello év, amely lehetővé teszi hello felülvizsgálati adatok tooretrieve hello alkalmazott teljesítményét, és ellenőrizze az adatokat egyetlen entitás egyetlen kérelmet tartalmazó.  

hello a következő példa bemutatja, hogyan kérheti le minden hello felülvizsgálati adatokat (például alkalmazott 000123 hello kereskedelmi osztály) egy alkalmazott:  

$filter = (PartitionKey eq 'Értékesítés') és (RowKey ge "empid_000123") és (RowKey lt "empid_000124") & $select = RowKey, Manager értékelése, társ értékelése, megjegyzések  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Így könnyen tooparse hello megfelelő elválasztó karaktert kell használnia **RowKey** érték: például **000123_2012**.  
* Ehhez az entitáshoz is tárolja hello megegyezik más személyként hello kapcsolódó adatok tartalmazó partíció ugyanaz az alkalmazott, ami azt jelenti, EGTs toomaintain az erős konzisztencia is használhatja.
* Akkor érdemes megfontolni, hogy milyen gyakran fogja kérdezni hello adatok toodetermine, hogy megfelelő-e ezt a mintát.  Például ha éri el ritkán hello tekintse át adatokat, és hello fő alkalmazott adatok gyakran külön entitásokként kell tárolni.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Ebben a mintában használatára kell toostore egy vagy több kapcsolódó entitások lekérdezett gyakran.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Entitás csoport tranzakciók](#entity-group-transactions)  
* [Heterogén entitástípusok használata](#working-with-heterogeneous-entity-types)  
* [Idővel konzisztenssé tranzakciók minta](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Napló végéről minta
Hello beolvasása  *n*  entitások legutóbb hozzáadott tooa partíció használatával egy **RowKey** érték, amely fordított dátum és idő sorrendben rendezi.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
A közös vonatkozó követelmény akkor kell tudni tooretrieve utoljára létrehozott hello entitások, például hello tíz legújabb kiadás jogcímek az alkalmazottak által küldött. Táblázat a támogatási lekérdezi egy **$top** először lekérdezési művelet tooreturn hello  *n*  egy entitás: nincs egyenértékű lekérdezési művelet tooreturn hello utolsó n entitások van egy.  

#### <a name="solution"></a>Megoldás
Tároló hello entitások használatával egy **RowKey** , hogy természetes névkeresési rendezések dátum/idő rendelés használatával, ezért a legutóbbi bejegyzés hello mindig hello hello táblázat első szerkezetével.  

Például toobe képes tooretrieve hello által alkalmazott tíz legújabb kiadás jogcímeket, egy fordított osztásjelek származó értékkel hello aktuális dátum és idő is használhatja. hello alábbi C# kódminta látható egyirányú toocreate megfelelő "fordított ticks" értéket egy **RowKey** , hogy a legrégebbi hello legutóbbi toohello rendezi:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Is vissza toohello dátum idő érték a következő kód hello használata:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

hello lekérdezés így néz ki:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Kell írni, hello fordított osztásjelek érték nullából vezető tooensure hello karakterláncértéket rendezi a várt módon.  
* Hello méretezhetőségi célok partíció hello szinten tisztában kell lennie. Legyen óvatos nem interaktív terület partíciókat.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Használja ezt a mintát Ha tooaccess entitások fordított dátum/idő sorrendben kell, vagy ha tooaccess hello legutóbb kell entitások hozzá.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Illesztenie elleni mintát hozzáfűzése](#prepend-append-anti-pattern)  
* [Entitások beolvasása](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a>Nagy mennyiségű delete minta
A saját külön táblázatban szereplő összes hello entitás egyidejű törlésre elhelyezésével nagyszámú entitások hello törlésének engedélyezése hello entitások hello tábla törlésével törli.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Számos alkalmazás törölje a régi adatokat, amelyek már nincs szüksége a toobe elérhető tooa ügyfélalkalmazást, vagy hello alkalmazás rendelkezik archivált tooanother adathordozóra. Általában azonosíthatja az ilyen adatokat a dátum: például, hogy egy követelmény toodelete rögzíti, amelyek több mint 60 napos bejelentkezési kérelmek.  

Egy lehetséges tervezési toouse hello dátuma és időpontja hello bejelentkezési kérelem hello **RowKey**:  

![][21]

Ez a megközelítés partíció elérési pontokhoz való elkerülhető, mert hello alkalmazás beszúrása, és minden felhasználóhoz külön partícióra bejelentkezési entitások törlésére. Azonban előfordulhat, hogy ez a megközelítés költséges és sok időt vesz igénybe Ha sok entitást mivel tooperform először egy tábla vizsgálata a rendelés tooidentify összes hello entitások toodelete, és törölnie kell minden egyes régi entitás. Vegye figyelembe, hogy csökkentheti hello adatváltások toohello szükséges kiszolgáló toodelete hello régi entitások száma több törlési kérelmek kötegelés EGTs be.  

#### <a name="solution"></a>Megoldás
Egy külön táblázattal minden nap a bejelentkezési kísérletek. Használhatja a hello entitás tervezési fent tooavoid elérési pontokhoz való beszúrt entitásokat, és most már egyszerűen törlése egy tábla minden nap adott esetben régi entitások törlése (egyetlen tárhelyművelettel) keresése és több száz és több ezer törlése helyett Egyéni bejelentkezési entitások minden nap.  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Támogatja a Tervező más módszereket, az alkalmazás fogja hello adatok, például az adott entitások kapcsolódik-e más adatok, vagy előállítása összesített adatát keresésekor?  
* Nem a Tervező elkerülése interaktív területek új entitások beszúráskor?  
* A késleltetés, ha azt szeretné, hogy tooreuse hello azonos várt azt törlését követően a tábla neve. Jobb tooalways használjon egyedi tábla neve is.  
* Várt néhány szabályozás első használata alkalmával érdemes egy új tábla hello Table szolgáltatás hello hozzáférési minták megtanulja, és hello partíciók elosztja a csomópontok között. Meg kell figyelembe vennie, milyen gyakran toocreate új táblákat.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Ebben a mintában használni, ha nagyszámú törölnie kell a hello entitások ugyanannyi időt vesz igénybe.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Entitás csoport tranzakciók](#entity-group-transactions)
* [Entitások módosítása](#modifying-entities)  

### <a name="data-series-pattern"></a>Adatsorozat adatmintát
Tároló teljes adatsorok egy egyetlen entitás toominimize hello kérelemszámot elvégezte.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Egy általános forgatókönyv egy alkalmazás toostore adatokat, hogy általában tooretrieve egyszerre több. Például az alkalmazás előfordulhat, hogy minden alkalmazott óránként küld hány Csevegési üzeneteket rögzíti, és aztán a információk tooplot minden felhasználó keresztül küldött üzenetek számának hello előző 24 óra. Lehet, hogy egy Tervező toostore 24 entitások minden alkalmazott számára:  

![][22]

Ezzel a kialakítással egyszerűen keresse meg és hello entitás tooupdate hello alkalmazásnak kell tooupdate hello üzenet számérték minden alkalmazott frissítésére. Azonban tooretrieve hello információk tooplot előző 24 óra hello hello tevékenységét diagramot, 24 entitások be kell olvasni.  

#### <a name="solution"></a>Megoldás
Tervezési egy külön tulajdonság toostore hello üzenetek száma a következő minden hello használata:  

![][23]

Ezzel a kialakítással használható egy merge művelet tooupdate hello üzenetek száma egy alkalmazott egy adott egy óra. Most tooplot hello diagram kérelmet használatával egyetlen entitás szükséges összes hello információkat kérheti le.  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Ha a teljes adatsor nem fér el egyetlen entitás (a egy entitás legfeljebb too252 tulajdonságok tartalmazhat), például blob alternatív adattár használata.  
* Ha egy entitás frissítése egyszerre több ügyfélnek, szüksége lesz a toouse hello **ETag** tooimplement egyidejű hozzáférések optimista. Ha sok ügyfél, magas versengés tapasztalhatja.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Használja ezt a mintát, ha a tooupdate kell, és az egyes entitáshoz kapcsolódó adatsor beolvasása.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Nagy entitások minta](#large-entities-pattern)  
* [Egyesítés vagy cseréje](#merge-or-replace)  
* [Idővel konzisztenssé tranzakciók mintát](#eventually-consistent-transactions-pattern) (ha tárolja hello adatsorozat blob)  

### <a name="wide-entities-pattern"></a>Széles entitások minta
Több fizikai entitások toostore logikai entitás legfeljebb 252 tulajdonságot használja.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Egyes entitás legfeljebb 252 tulajdonságot (kivéve a hello kötelező Rendszertulajdonságok) állhat, és nem 1 MB-nál több adat tárolása összesen. Egy relációs adatbázisban általában számíthat round bármely sor mérete hello vonatkozó korlátozások hozzáadása egy új tábla és a közöttük 1-1 kapcsolatot.  

#### <a name="solution"></a>Megoldás
Hello Table szolgáltatás használ, több entitások toorepresent legfeljebb 252 tulajdonságot az egyetlen nagy üzleti objektumot is tárolhatók. Például ha azt szeretné, hogy toostore hello IM üzenetek száma küld minden alkalmazott hello az elmúlt 365 napban számát, a következő kialakítás ezért más sémák használ, két olyan entitásra hello használhatja:  

![][24]

Ha van szüksége, amely mindkét entitások tookeep őket szinkronizálja egymással frissíteni kell az toomake egy EGT is használhatja. Ellenkező esetben egy adott napon egy egyetlen egyesítési művelet tooupdate hello üzenetek száma is használhatja. be kell olvasni mindkét entitások, amely egyaránt két hatékony kérelmek teheti adott alkalmazott összes hello adatok tooretrieve egy **PartitionKey** és egy **RowKey** érték.  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* A teljes logikai entitás beolvasásakor magában foglalja a legalább két storage-tranzakció: egy tooretrieve fizikai entitás.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Használja ezt a mintát, ha a szükséges toostore entitások, amelynek méretét vagy a tulajdonságok száma meghaladja a hello korlátok hello található egyes entitás Table szolgáltatás.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Entitás csoport tranzakciók](#entity-group-transactions)
* [Egyesítés vagy cseréje](#merge-or-replace)

### <a name="large-entities-pattern"></a>Nagy entitások minta
A blob storage toostore nagy tulajdonságértékek használja.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Egyes entitás nem 1 MB-nál több adat tárolása összesen. Ha egy vagy több, a tulajdonság tárolja az értékeket, amelyeket az entitás tooexceed hello teljes mérete okozza ezt az értéket, a Table szolgáltatás hello hello teljes entitás nem tárolhat.  

#### <a name="solution"></a>Megoldás
Az entitás meghaladja mérete 1 MB, mert egy vagy több tulajdonságának nagy mennyiségű adatot tartalmaz, ha a Blob szolgáltatás hello tárolják az adatokat és tárolására is használható majd hello blob hello címe hello entitásban tulajdonsággal. Például egy alkalmazott hello fénykép tárolni a blob Storage tárolóban, és hivatkozás toohello fénykép tárolása hello **fénykép** tulajdonság az alkalmazott entitás:  

![][25]

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* hello adatok hello Blob szolgáltatás, és a Table szolgáltatás hello hello entitás közötti toomaintain a végleges konzisztencia hello használata [idővel konzisztenssé tranzakciók mintát](#eventually-consistent-transactions-pattern) toomaintain az entitások.
* Legalább két storage-tranzakció teljes entitásnak beolvasása foglalja magában: egy tooretrieve hello entitás és egy tooretrieve hello blob-adatokat.  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
Használja ezt a mintát, ha a toostore entitások, amelyek mérete meghaladja a hello korlátok hello Table szolgáltatás található egyes entitás van szüksége.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Idővel konzisztenssé tranzakciók minta](#eventually-consistent-transactions-pattern)  
* [Széles entitások minta](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a>Víruskereső mintát illesztenie hozzáfűzése
Méretezhetőség javítása, ha nagyszámú Beszúrások által hello Beszúrások több partíciót keresztül terjednek.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Fertőző vagy entitások tooyour tárolt entitásokat általában fűznek eredményezi hello alkalmazás új entitások toohello először hozzáadásával vagy a partíciók több partíció utolsó. Ebben az esetben hello beszúrása egy adott időpontban mind a hello létrehozása, amely megakadályozza, hogy a table szolgáltatás hello terheléselosztás interaktív terület partícióra beszúrása több csomópont között zajló, és ami miatt a kérelem toohit hello méretezhetőség célok partíció. Például ha egy alkalmazásnál naplók hálózati és az erőforrás elérhető alkalmazottai, majd egy entitás struktúra alább látható módon eredményezhet hello jelenlegi órán partíció egy interaktív terület váljon, ha hello tranzakciók mennyisége eléri hello méretezhetőség célja az egyes partíció:  

![][26]

#### <a name="solution"></a>Megoldás
hello következő alternatív entitás struktúra elkerülhető bármely adott partíció interaktív terület hello alkalmazás naplók eseményként is:  

![][27]

Az ebben a példában láthatja, hogyan mindkét hello **PartitionKey** és **RowKey** összetett kulcs. Hello **PartitionKey** használ hello részleg és az alkalmazott azonosítója toodistribute hello naplózási egyszerre több partíciót.  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Vegye figyelembe a következő pontok meghatározásakor hello hogyan tooimplement ebben a mintában:  

* Biztosítja hello alternatív struktúra, amellyel elkerülhető a gyakran használt adatok partíciók beszúrása a hatékony támogatási hello lekérdezések létrehozásáról az ügyfélalkalmazást lesz?  
* A tranzakciók várható mennyisége azt jelenti, hogy valószínűleg tooreach hello méretezhetőségi célok az egyes partíciók és hello tároló szolgáltatás által szabályozott kell?  

#### <a name="when-toouse-this-pattern"></a>Ha toouse ezt a mintát
A tranzakciók mennyisége esetén a sávszélesség-szabályozás hello tároló szolgáltatás által a gyakran használt adatok partíció elérésekor valószínűleg tooresult, ne hello illesztenie hozzáfűzése elleni mintát.  

#### <a name="related-patterns-and-guidance"></a>Útmutató és a kapcsolódó minták
hello következő mintákat és útmutatókat is megfelelő ebben a mintában végrehajtása során:  

* [Összetett kulcs minta](#compound-key-pattern)  
* [Napló végéről minta](#log-tail-pattern)  
* [Entitások módosítása](#modifying-entities)  

### <a name="log-data-anti-pattern"></a>Napló elleni adatmintát
Hello Blob szolgáltatás általában helyett hello tábla toostore Szolgáltatásnapló-adatait kell használni.  

#### <a name="context-and-problem"></a>A környezetben, és probléma
Egy közös használati eset, a naplóadatok tooretrieve a kijelölt naplóbejegyzések adott dátum/idő köre: például azt szeretné, hogy az összes hiba és a kritikus üzeneteihez, az alkalmazás a rendszer 15:04 és 15:06 egy adott dátumon közötti hello toofind. Nem szeretné, hogy toouse hello dátum és idő hello napló üzenet toodetermine hello partíció napló entitásokat mentése:, hogy a működés közbeni partíció eredményez, mert az adott időpontban hello napló entitásokhoz aliashoz hello azonos **PartitionKey** érték (hello című [elleni mintát Prepend hozzáfűzése](#prepend-append-anti-pattern)). Például a következő entitás sémája egy naplófájlüzenetre hello okozza, a működés közbeni partíció hello alkalmazás írja az összes naplóüzenetek toohello partíció az aktuális dátumot és az óra hello:  

![][28]

Ebben a példában hello **RowKey** tartalmazza hello dátuma és időpontja hello napló üzenet tooensure, hogy naplóüzenetek kell tárolni, dátum/idő sorrendbe rendezve, és egy üzenetazonosítója abban az esetben, ha több naplóüzenetek megosztása hello azonos dátum és idő.  

Egy másik megoldás, toouse egy **PartitionKey** , amely biztosítja, hogy hello alkalmazás írja az üzenetek között számos különböző partíciókat. Például hello forrás hello naplóüzenet lehetővé teszi az olyan módon toodistribute üzenetek sok partíciót, használhatja a következő entitás séma hello:  

![][29]

Ebben a sémában hello probléma azonban, hogy az összes naplózott üzeneteket az adott időtartama kell megkeresni hello tooretrieve minden partíció hello táblában.

#### <a name="solution"></a>Megoldás
hello előző szakasz kiemelt hello probléma toouse közben a Table szolgáltatás toostore naplóbejegyzések hello és javasolt két, nem megfelelő, tervez. Egy megoldás vezetett tooa működés közbeni partíció hello kockázata, gyenge teljesítményt naplóüzenetek; írása hello más megoldás, így gyenge lekérdezési teljesítmény miatt hello követelmény tooscan minden partíció hello tábla tooretrieve naplózott üzeneteket az adott időtartama. A BLOB storage ilyen esetben jobb megoldást kínál, és az Azure Storage Analytics tárolók hello naplóadatokat összegyűjti.  

Ez a szakasz ismerteti, hogyan tárolási analitika napló adatot tárol a blob storage általában tartomány lekérdező megközelítés toostoring adatot szemléltetésére.  

Tárolási analitika naplóüzenetek több blobok tagolt formátumban tárolja. hello tagolt formátumú megkönnyíti, hogy az ügyfél tooparse hello alkalmazásadatok hello napló üzenetben.  

Tárolási analitika blobot, amely lehetővé teszi toolocate hello blob (vagy bináris objektumok) keres, amelynek hello üzeneteket tartalmazó elnevezési konvenciót használ. Például a "queue/2014/07/31/1800/000001.log" nevű blob naplóüzenetek toohello queue szolgáltatás indítása 31 2014. július 18:00 órakor hello óráig is tartalmazza. hello "000001" jelzi, hogy a hello első naplófájl ebben az időszakban. Tárolási analitika hello időbélyegeket hello az első rögzíti, és a legutóbbi a hello blob metaadatai részeként hello fájlban tárolt üzenetek naplózása. a blob storage lehetővé teszi, hogy blobok keresse meg a tartománynév előtagján alapuló tároló API hello: toolocate valamennyi hello BLOB várólista tartalmazó naplózni az adatokat hello óráig 18:00-tól kezdve, használhatja a hello előtag "várólista/2014/07/31/1800."  

Tárolási analitika belső napló üzeneteket pufferel, és rendszeres időközönként frissíti a megfelelő blob hello, vagy létrehoz egy új-es hello legújabb naplóbejegyzéseket a. Ez hello szám csökkenti az írások toohello blob szolgáltatás kell végrehajtania.  

Egy hasonló megoldás webkiszolgálókból a saját alkalmazásban, meg kell fontolnia hogyan toomanage hello (írása minden naplók bejegyzés tooblob tárolásához, akkor történik) megbízhatóság és a költségek és a méretezhetőség között (frissítések pufferelés az alkalmazás és rögzíti őket tooblob tárolási kötegekben).  

#### <a name="issues-and-considerations"></a>Problémákat és szempontok
Hello követő pontokat, hogyan toostore naplózni meghatározásakor vegye figyelembe:  

* Ha létrehoz egy Táblatervezés, amellyel elkerülhető a potenciális kiemelt partíciók, előfordulhat, hogy nem férhet hozzá a naplóadatok hatékonyan.  
* tooprocess adatok naplózása, az ügyfél gyakran kell tooload sok rekord.  
* Bár gyakran felépítése naplóadatokat, a blob storage egy jobb megoldás lehet.  

### <a name="implementation-considerations"></a>Megvalósítási kapcsolatos szempontok
Ez a szakasz ismerteti néhány hello szempontok toobear figyelembe hello fentebbi szakaszokban leírt hello minták bevezetésekor. Ez a szakasz a legtöbb a Storage ügyféloldali kódtára (verzió: 4.3.0 írásának hello időpontjában) hello használó C# nyelven írt példák használja.  

### <a name="retrieving-entities"></a>Entitások beolvasása
Hello szakaszban leírtaknak megfelelően [lekérdezése tervezési](#design-for-querying), hello leghatékonyabb Ez egy pont lekérdezés. Azonban bizonyos esetekben szükség lehet tooretrieve több entitás. Ez a témakör néhány gyakori megközelítések tooretrieving entitás hello Storage ügyféloldali kódtár segítségével.  

#### <a name="executing-a-point-query-using-hello-storage-client-library"></a>A Storage ügyféloldali kódtára hello segítségével pont lekérdezése
hello legegyszerűbb módja tooexecute pont lekérdezés toouse hello **beolvasása** művelet táblázatban látható módon hello következő C# kódrészletet, amely lekéri az entitás egy **PartitionKey** érték "értékesítési" és a  **RowKey** "212" érték:  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

Figyelje meg, hogyan vár az ebben a példában a hello entitás lekérdezi a toobe típusú **EmployeeEntity**.  

#### <a name="retrieving-multiple-entities-using-linq"></a>LINQ használatával több entitás beolvasásakor
A Storage ügyféloldali kódtára a LINQ használatával, és a lekérdezés megadásával több entitás le egy **ahol** záradékban. tooavoid egy táblázatbeolvasás, meg kell adnia hello **PartitionKey** hello értéket ahol záradék, és ha lehetséges hello **RowKey** tooavoid tábla és a partíció vizsgálatok érték. hello table szolgáltatás támogatja a korlátozott számú összehasonlító operátorok (nagyobb, mint, nagyobb vagy egyenlő, kevesebb mint, kisebb vagy egyenlő, egyenlő, és nem egyenlő) toouse hello ahol záradékban. hello következő C# kódrészletet talál minden hello alkalmazott amelynek utolsó neve kezdődik, "B" (feltéve, hogy hello **RowKey** tárolók hello Vezetéknév) hello értékesítési osztályon (feltéve, hogy hello **PartitionKey** tárolja a hello részleg neve):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

Figyelje meg, hogyan hello lekérdezés határoz meg, mindkét egy **RowKey** és egy **PartitionKey** tooensure jobb teljesítmény érdekében.  

hello alábbi példakód mutatja hello Folyékonyan beszél API-jával funkciókat (További információ a Folyékonyan beszél API-k általában: [Folyékonyan beszél API tervezéséhez gyakorlati tanácsok](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> hello minta ágyazza több **CombineFilters** módszerek tooinclude hello három szűrési feltételeket.  
> 
> 

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Egy lekérdezés által nagyszámú entitások beolvasása
Az optimális lekérdezési alapuló egyéni entitást adja vissza egy **PartitionKey** érték és egy **RowKey** érték. Azonban bizonyos esetekben előfordulhat, hogy egy követelmény tooreturn hello azonos particionálása, vagy akár több partícióról származó sok entitásokat.  

Ilyen esetekben mindig teljes mértékben az alkalmazás teljesítményének hello kell tesztelni.  

Egy lekérdezést hajtanak hello table szolgáltatás térhetnek vissza egy adott időpontban legfeljebb 1000 entitásokat, és előfordulhat, hogy öt másodpercenként legfeljebb hajtható végre. Ha hello eredménykészlet 1000-nél több entitásokat tartalmaz, ha hello lekérdezés nem fejeződött be 5 másodpercen belül, vagy ha hello lekérdezés áthalad hello partíció határán, Table szolgáltatás hello folytatási token tooenable hello ügyfél alkalmazás toorequest köszönésére az entitások készletének tovább. Hogyan folytatási jogkivonatok munkahelyi kapcsolatos további információkért lásd: [lekérdezés időkorlátja és tördelési](http://msdn.microsoft.com/library/azure/dd135718.aspx).  

A Storage ügyféloldali kódtára hello használ, ha azt kezelni tud a automatikusan folytatási jogkivonatok meg, akkor ad vissza entitásokat hello Table szolgáltatás. hello alábbi C# kódminta hello Storage ügyféloldali kódtár segítségével automatikusan kezeli a folytatási jogkivonatok Ha hello table szolgáltatás visszaadja azokat választ:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

a következő C#-kódban hello folytatási jogkivonatok explicit módon kezeli:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

Jogkivonatok segítségével képes folytatási explicit módon, megadhatja, ha az alkalmazás lekéri a következő szegmens hello adatok. Például ha az ügyfélalkalmazást lehetővé teszi, hogy a felhasználók toopage keresztül hello entitások egy táblázatban tárolja, a felhasználó dönthet nem keresztül, az alkalmazás csak használna a folytatási token tooretrieve hello mellett hello lekérdezés lekéri az összes hello entitások toopage szegmens befejezésekor hello felhasználói kellett hello aktuális szegmensben lévő összes hello entitás lapozást. Ez a megközelítés azzal számos előnnyel jár:  

* Lehetővé teszi a Table szolgáltatás hello adatok tooretrieve toolimit hello mennyiségét és hello hálózati átvitele.  
* Ez lehetővé teszi, hogy Ön tooperform aszinkron IO a .NET.  
* Lehetővé teszi a tooserialize hello folytatási token toopersistent tárolási így hello egy alkalmazás összeomlása esetén is.  

> [!NOTE]
> A folytatási kód általában 1000 entitásokat tartalmazó szegmens adja vissza, bár kevesebb lehet. Ez helyzet is hello Ha csak a lekérdezés visszaadja a bejegyzések száma hello **érvénybe** tooreturn hello első n entitások a keresési feltételeknek eleget tevő: hello table szolgáltatás térhetnek vissza egy szegmenst mentén n entitások nem lépi-e a folytatási token tooenable meg tooretrieve hello fennmaradó entitásokat.  
> 
> 

hello következő C# kód bemutatja, hogyan entitások száma toomodify hello visszaadott szegmens belül:  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a>Kiszolgálóoldali leképezése
Egyetlen entitás be too255 tulajdonságait és a too1 MB méretű lehet. Hello tábla lekérdezése, és kérje le az entitásokat, előfordulhat, hogy nem kell minden hello tulajdonság, és elkerülheti a feleslegesen adatátvitel (toohelp késés és csökkentése költség). Kiszolgálóoldali leképezése tootransfer csak hello tulajdonságok kell használhatók. hello következő példája lekéri csak hello **E-mail** tulajdonság (valamint **PartitionKey**, **RowKey**, **időbélyeg**, és  **ETag**) a kijelölt hello lekérdezésével hello entitásokból.  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

Figyelje meg, hogyan hello **RowKey** érték érhető el annak ellenére, hogy az nem tartalmazott tulajdonságokat tooretrieve hello listája.  

### <a name="modifying-entities"></a>Entitások módosítása
a Storage ügyféloldali kódtára hello lehetővé teszi, hogy a toomodify, az entitások hello table szolgáltatás tárolva, hogy beszúrása törlés és frissítési entitások. Több insert, update és delete művelet adatváltások száma együtt tooreduce hello szükséges, és a megoldás hello teljesítményének növelése a EGTs toobatch használható.  

Fontos megjegyezni, hogy a kivételek végrehajtásakor a Storage ügyféloldali kódtár hello egy EGT általában hello index hello entitás, ami miatt a hello kötegelt toofail tartalmazza. Ez akkor hasznos, ha EGTs kód hibakeresés alatt.  

Azt is figyelembe kell venni, hogyan a kialakítás befolyásolja, miként kezeli az ügyfélalkalmazást a feldolgozási mód és a frissítési művelet.  

#### <a name="managing-concurrency"></a>Párhuzamossági kezelése
Alapértelmezés szerint hello table szolgáltatás megvalósítja az optimista feldolgozási ellenőrzi az egyes entitások hello szinten **beszúrása**, **egyesítése**, és **törlése** műveletek Bár lehetséges a table szolgáltatás toobypass ellenőrzést egy ügyfél tooforce hello. Hogyan kezeli a hello table szolgáltatás az egyidejű kapcsolatos további információkért lásd: [egyidejűségi kezelése a Microsoft Azure Storage](../storage/common/storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Egyesítés vagy cseréje
Hello **cserélje le** hello metódusában **TableOperation** mindig a felváltja hello teljes entitásnak a Table szolgáltatás hello osztály. Ha nem adja meg a tulajdonság hello kérelem során ez a tulajdonság tárolja hello entitásban található, a hello kérelem hello tulajdonságot tárolt entitás eltávolítja. Kivéve, ha azt szeretné, hogy egy tulajdonság explicit módon tárolt entitásból tooremove, meg kell adnia minden tulajdonsághoz hello kérelemben.  

Használhatja a hello **egyesítése** hello metódusában **TableOperation** osztály tooreduce hello adatmennyiséget, hogy küldjön toohello Table szolgáltatás Ha azt szeretné, hogy tooupdate entitás. Hello **egyesítése** metódus bármelyik tulajdonságot tárolt hello entitás cseréli a tulajdonságértékek hello entitásból hello kérelemben szereplő, de ép leaves hello bármelyik tulajdonságot tárolja hello kérelemben szereplő nem entitás. Ez akkor hasznos, ha nagy entitásokat, és csak a kérelemben tulajdonságok kis számú tooupdate kell.  

> [!NOTE]
> Hello **cserélje le** és **egyesítése** módszer sem jár sikerrel, ha hello entitás nem létezik. Alternatív megoldásként használhatja hello **InsertOrReplace** és **InsertOrMerge** módszereket, amelyek új entitás létrehozása, ha még nem létezik.  
> 
> 

### <a name="working-with-heterogeneous-entity-types"></a>Heterogén entitástípusok használata
Table szolgáltatás hello van egy *séma nélküli* tábla tároló, amely azt jelenti, hogy egyetlen tábla tud tárolni a Tervező nagyfokú rugalmasságot biztosít több típusú entitásokat. hello alábbi példában látható módon egy tábla dolgozó és a részleg entitások tárolására:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>időbélyeg</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>departmentname nevű</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Vegye figyelembe, hogy minden egyes szervezet továbbra is rendelkeznie kell **PartitionKey**, **RowKey**, és **időbélyeg** értékeket, de rendelkezhetnek bármely tulajdonságkészletbe. Ezenkívül nincs tooindicate hello típusú entitás, kivéve, ha úgy dönt, toostore valahol ezt az információt. Azonosítsa a hello entitástípus két lehetőség áll rendelkezésre:  

* Hello entitás típusa toohello illesztenie **RowKey** (vagy valószínűleg hello **PartitionKey**). Például **EMPLOYEE_000123** vagy **DEPARTMENT_SALES** , **RowKey** értékeket.  
* Használjon egy külön tulajdonság toorecord hello entitástípus hello az alábbi táblázatban ismertetett módon.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>időbélyeg</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td>Alkalmazott</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td>Alkalmazott</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>departmentname nevű</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Szervezeti egység</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Utónév</th>
<th>Vezetéknév</th>
<th>Kor</th>
<th>E-mail cím</th>
</tr>
<tr>
<td>Alkalmazott</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

hello első lehetőség, fertőző hello entitás típusa toohello **RowKey**, akkor hasznos, ha lehetséges, hogy két különböző típusú lehet, hogy entitásnak hello ugyanaz a kulcs értékét. Azt is csoportok hello hello partíció együtt azonos adja meg az entitásokat.  

hello ebben a szakaszban bemutatott eljárások különösen fontos toohello vitafórum [öröklési kapcsolatok](#inheritance-relationships) hello szakaszában az útmutató korábbi [kapcsolatok modellezésére](#modelling-relationships).  

> [!NOTE]
> Érdemes egy verziószámot hello entitás típusú érték tooenable ügyfél alkalmazások tooevolve POCO objektumok kell, és különböző verzióival működnek.  
> 
> 

hello hátralévő részét ez a szakasz ismerteti, néhány hello szolgáltatást, amelyek egyszerűbbé teszik a több entitástípusok hello használata azonos hello a Storage ügyféloldali kódtára a táblában.  

#### <a name="retrieving-heterogeneous-entity-types"></a>Heterogén entitástípusok beolvasása
A Storage ügyféloldali kódtára hello használ, az entitástípusok több három beállításai lesz.  

Ha tudja, hogy egy adott tárolt hello entitások hello típusú **RowKey** és **PartitionKey** értékeket, majd megadhat hello entitástípus, amikor hello entitás visszaállíthatja az előző két hello példákban típusú entitásokat beolvasása, amely **EmployeeEntity**: [hello Storage ügyféloldali kódtár segítségével pont lekérdezése](#executing-a-point-query-using-the-storage-client-library) és [LINQhasználatávaltöbbentitásbeolvasásakor](#retrieving-multiple-entities-using-linq).  

hello második lehetőség toouse hello **DynamicTableEntity** típusú (egy tulajdonságcsomagot) helyett egy konkrét POCO entitás típusa (ezt a lehetőséget is is teljesítményének javítása, mert nincs szükség tooserialize és hello entitás túl deszerializálni. NETTÓ esetében). a következő C#-kódban potenciálisan hello különböző típusú entitásokat hello táblából, de mint minden entitásokat ad vissza **DynamicTableEntity** példányok. Ezután hello **EntityType** toodetermine hello tulajdonságtípus minden entitás:  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

Vegye figyelembe, hogy tooretrieve egyéb tulajdonságok hello kell használnia **TryGetValue** hello metódusa **tulajdonságok** hello tulajdonságának **DynamicTableEntity** osztály.  

A harmadik lehetőség hello segítségével toocombine **DynamicTableEntity** típust és egy **EntityResolver** példány. Ez lehetővé teszi tooresolve toomultiple POCO típusainak hello ugyanabban a lekérdezésben. Ebben a példában hello **EntityResolver** delegált használ hello **EntityType** tulajdonság toodistinguish közötti hello két típusú entitás, amely hello lekérdezés értéket ad vissza. Hello **megoldásához** metódusnak hello **feloldó** tooresolve delegálása **DynamicTableEntity** túl példányok**TableEntity** példányok.  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

#### <a name="modifying-heterogeneous-entity-types"></a>Heterogén entitástípusok módosítása
Egy entitás toodelete tooknow hello típusú és hello típusú entitás mindig jelzi, ha azt nem kell. Használhat azonban **DynamicTableEntity** írja be a tooupdate entitást, anélkül, hogy tudnák típusára és egy POCO entitásosztályt használata nélkül. hello alábbi kódminta egyetlen entitás kéri le, és ellenőrzi hello **EmployeeCount** tulajdonság létezik-e frissítése előtt.  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

### <a name="controlling-access-with-shared-access-signatures"></a>Megosztott hozzáférési aláírásokkal-hozzáférés szabályozásáról
Használhat közös hozzáférésű Jogosultságkód (SAS) tokenek tooenable ügyfél alkalmazások toomodify (és lekérdezése) táblaentitásokat közvetlenül hello kell tooauthenticate közvetlenül az hello table szolgáltatás nélkül. Nincsenek általában három fő előnnyel jár toousing SAS az alkalmazásban:  

* Nem kell toodistribute tárhelyét rendelés tooallow kulcs tooan nem biztonságos platform (például egy mobileszköz) fiók adott eszköz tooaccess, és módosítsa a Table szolgáltatás hello szerepelnek.  
* A web is kiszervezése egy része hello és feldolgozói szerepkörök hajtsa végre az entitások tooclient eszközök, például a végfelhasználói számítógépek és mobileszközök kezeléséhez.  
* Hozzárendelhet egy korlátozott, és időt korlátozott engedélyek tooa ügyfél (például így csak olvasási hozzáféréssel toospecific erőforrások) készletét.  

SAS-tokenje hello Table szolgáltatás használatával kapcsolatban további információkért lásd: [használatával megosztott hozzáférési aláírásokkal (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Azonban továbbra is kell létrehoznia, amely adja meg egy ügyfél toohello alkalmazásentitásokat a table szolgáltatás hello hello SAS-tokenje: akkor tegye ezt, amely rendelkezik biztonságos hozzáférés tooyour tárfiókkulcsok környezetben. Általában egy webes vagy feldolgozói szerepkör toogenerate hello SAS jogkivonatokat, és letöltheti toohello ügyfélalkalmazások elérő tooyour entitások kell használni. Mivel nincs még egy terhelés létrehozása és SAS-jogkivonatok tooclients kézbesítéséhez részt, vegye figyelembe a legjobb módja tooreduce Ez a terhelés, különösen a nagy mennyiségű forgatókönyvek.  

Már lehetséges toogenerate egy SAS-jogkivonatot, hogy a hozzáférési tooa részhalmazát hello entitások egy táblázatban. Alapértelmezés szerint a teljes táblázat egy SAS-jogkivonat létrehozása, de azt is lehetséges toospecify adott hello SAS token grant hozzáférés tooeither a számos **PartitionKey** értékeket, vagy számos **PartitionKey** és  **RowKey** értékeket. Akkor érdemes választania toogenerate SAS-tokenje egyedi számára a rendszer úgy, hogy minden felhasználó SAS-jogkivonat csak engedélyezi tootheir saját entitások hello a table szolgáltatás.  

### <a name="asynchronous-and-parallel-operations"></a>Aszinkron és párhuzamos műveletek
A kérelmek több partíciót között vannak oszlik, feltéve aszinkron vagy párhuzamos lekérdezések használatával is javítható az átviteli sebesség és az ügyfél válaszképességét.
Például lehetséges, hogy két vagy több szerepkör feldolgozópéldányok párhuzamosan a táblák elérése. Sikerült partíciók adott készleteinek felelős az egyes feldolgozói szerepköröket használ, vagy egyszerűen a feldolgozói szerepkör több példánya, minden egyes képes tooaccess rendelkezik az összes hello partíciókat egy táblázat.  

Belül egy ügyfél példány aszinkron módon tárolási műveletek végrehajtásával javíthatja a teljesítményt. a Storage ügyféloldali kódtára hello segítségével könnyen toowrite aszinkron lekérdezések és módosításokat. Például előfordulhat, hogy a kiindulási pont hello szinkron módszer, amely lekéri a egy partíció összes hello entitások, ahogy az alábbi C#-kódban hello:  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

Ez a kód hello lekérdezés aszinkron módon fut, könnyen módosíthatja:  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

A aszinkron példában látható hello következő hello szinkron verziójáról módosításai:  

* hello metódus-aláírás mostantól tartalmazza a hello **aszinkron** módosító és értéket ad vissza egy **feladat** példány.  
* Helyett hívó hello **ExecuteSegmented** metódus tooretrieve eredményeket, hello metódus most hívások hello **ExecuteSegmentedAsync** metódus és felhasználási hello **await** módosító tooretrieve aszinkron eredmény.  

hello ügyfélalkalmazás a metódus hívása többször (hello különböző értékekkel **részleg** paraméter), és minden egyes lekérdezés külön szálban futtatandó.  

Vegye figyelembe, hogy van-e nem hello aszinkron verziója **Execute** metódus a hello **TableQuery** osztálynál, mert hello **IEnumerable** illesztőfelület nem támogatja a aszinkron enumerálása.  

Helyezze, frissítése, és aszinkron módon entitások törlésére. a következő példa C# hello egy egyszerű, a szinkron módszer tooinsert jeleníti meg, vagy alkalmazott entitás cseréje:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Ez a kód könnyen módosíthatja, így hello frissítés aszinkron módon fut:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

A aszinkron példában látható hello következő hello szinkron verziójáról módosításai:  

* hello metódus-aláírás mostantól tartalmazza a hello **aszinkron** módosító és értéket ad vissza egy **feladat** példány.  
* Helyett hívó hello **Execute** metódus tooupdate hello entitás, hello metódus most hívások hello **ExecuteAsync** metódus és felhasználási hello **await** módosító tooretrieve aszinkron módon annak az eredménye.  

hello ügyfélalkalmazás ehhez hasonló több aszinkron metódus meghívása, és minden metódushívás külön szálban futtatandó.  

### <a name="credits"></a>Kreditek
A következő hello hozzájárulásuk Azure-csapat tagjai toothank hello tapasztalatairól: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah Serdar Ozler, valamint a Microsoft DX Tom Hollander. 

Azt is szeretné, Microsoft MVP tekintse meg az értékes visszajelzést való felülvizsgálati ciklus során toothank hello: Igor Papirov és Edward Bakker.

[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png

