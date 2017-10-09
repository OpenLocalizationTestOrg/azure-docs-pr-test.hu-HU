---
title: "A Stream Analytics kimenetek: tárolási, elemzés lehetőségei |} Microsoft Docs"
description: "További információk a Stream Analytics-adatok kimenetek beállításai, többek között a Power BI analysis eredmények célzó."
keywords: "adatok átalakítása, az elemzés eredményeinek, adatok tárolási lehetőségek"
services: stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ba6697ac-e90f-4be3-bafd-5cfcf4bd8f1f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 99f8113f0464960e898293397fbe3de90d669857
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>A Stream Analytics kimenetek: tárolási, elemzés lehetőségei
A Stream Analytics-feladat készítésekor vegye figyelembe, hogyan hello kapott adatokban fognak használni. Lesz megtekintésének hello hello Stream Analytics-feladat eredményének és fog tárolására azt?

Azure Stream Analytics rendelés tooenable alkalmazás minták számos, a kimeneti tárolására és az elemzés eredményeinek megtekintése különböző beállításokat tartalmaz. Ez teszi, hogy könnyen tooview feladatkiemenetét, és adatraktározási vagy más célból hello fogyasztás és a tárolási hello feladat kimeneti rugalmasságot biztosít. Hello feladat konfigurált kimenetet létezzen hello feladat elindult és események start továbbítására. Például ha egy kimeneti Blob-tároló használja, hello feladat nem hoz létre egy tárfiókot automatikusan. Toobe hello felhasználó által létrehozott hello ASA feladat végrehajtásának megkezdése előtt szükséges.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
A Stream Analytics támogatja [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Ez a tároló lehetővé teszi a műveleti és felderítési jellegű bármilyen méretű, típusú és feldolgozási sebességű toostore adatok. További a Stream Analytics igények toobe tooaccess hello Data Lake Store engedélyezett. Engedélyezési és hogyan feliratkozott hello Data Lake Store (ha szükséges) toosign hello esik szó [Data Lake kimeneti cikk](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Egy Azure Data Lake Store engedélyezése
Data Lake tárolási kiválasztásakor az Azure-portálon hello kimenetként felszólító tooauthorize egy kapcsolat tooan meglévő Data Lake Store fogja.  

![Data Lake Store engedélyezése](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Ezután adja meg a Data Lake Store az alább látható kimeneti hello hello tulajdonságai:

![Data Lake Store engedélyezése](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

hello az alábbi táblázat a hello tulajdonság nevét és azok leírását, egy Data Lake Store kimenet létrehozásához szükséges.

<table>
<tbody>
<tr>
<td><B>TULAJDONSÁG NEVE</B></td>
<td><B>LEÍRÁS</B></td>
</tr>
<tr>
<td>A kimeneti Alias</td>
<td>Ez az egy rövid nevet használt a lekérdezések toodirect hello lekérdezés kimeneti toothis Data Lake Store.</td>
</tr>
<tr>
<td>Fiók neve</td>
<td>Data Lake-tárfiókra ahol küldendő a kimeneti hello hello neve. Meg fog jelenni a legördülő listából a Data Lake Store-fiókok toohello portálra bejelentkezve toowhich hello felhasználónak hozzáférése van.</td>
</tr>
<tr>
<td>Elérési út előtag mintája [<I>választható</I>]</td>
<td>fájl elérési útja toowrite hello hello található a fájl megadott Data Lake Store-fiók. <BR>a {date}, {time}<BR>1. példa: mappa1/logs / {date} / {time}<BR>2. példa: mappa1/logs / {date}</td>
</tr>
<tr>
<td>Dátum formátumban [<I>választható</I>]</td>
<td>Hello dátumtokent hello előtag elérési út használata esetén, amelyben a fájlok vannak rendszerezve hello dátumformátum választhatja meg. . Példa: Éééé/hh/nn</td>
</tr>
<tr>
<td>Idő formátumot [<I>választható</I>]</td>
<td>Ha hello idő jogkivonat hello előtag elérési útja, adja meg a hello idő formátumban, amelyben a fájlok vannak rendezve. Jelenleg csak a támogatott hello értéke HH.</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>A kimeneti adatok szerializálási formátum. JSON, CSV és az avro-hoz támogatott.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Ha a fürt megosztott kötetei szolgáltatás- vagy JSON formátumú, kódolással meg kell adni. Az UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg.</td>
</tr>
<tr>
<td>Elválasztó</td>
<td>Csak a fürt megosztott kötetei szolgáltatás szerializálási alkalmazható. A Stream Analytics számos általánosan használt elválasztó karaktert támogatja a CSV-adatok szerializálása során. Támogatott értékei vesszővel, a pontosvesszővel válassza el, a terület, a lapon és a függőleges vonal.</td>
</tr>
<tr>
<td>Formátumban</td>
<td>Csak a JSON-szerializálás alkalmazható. Sorral elválasztott beállítás megadja, hogy hello kimeneti azzal, hogy minden JSON-objektum sortöréssel elválasztva kell formázni. A tömb határozza meg, hogy hello kimeneti lesznek formázva, egy JSON-objektumok tömbjét.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Újítsa meg a Data Lake Store engedélyezési
Szüksége lesz a toore-hitelesítéséhez a Data Lake Store-fiókot, ha a jelszó megváltozott a feladat meg lett létrehozva, vagy utolsó hitelesített.

![Data Lake Store engedélyezése](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Az Azure SQL Database](https://azure.microsoft.com/services/sql-database/) használható kimenetként a relációs jellegű adatokhoz, vagy olyan alkalmazásnál, amely egy relációs adatbázisban szolgáltatott tartalmaktól függnek. Stream Analytics-feladatok tooan meglévő tábla ír egy Azure SQL-adatbázis.  Vegye figyelembe, hogy hello táblaséma pontosan meg kell egyeznie hello mezők és azok típusát, a feladat kimenete alatt. Egy [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) hello SQL-adatbázis output paraméter is (Ez egy előzetes verziójú funkciók) keresztül kimenetként is megadható. hello az alábbi táblázat hello tulajdonságnevek és egy SQL-adatbázis kimenet létrehozása leírását.

| Tulajdonság neve | Leírás |
| --- | --- |
| A kimeneti Alias |Ez az adatbázisban használt lekérdezések toodirect hello lekérdezés kimeneti toothis rövid nevét. |
| Adatbázis |ahol a kimeneti küldi hello adatbázis hello neve |
| Kiszolgáló neve |hello SQL adatbázis-kiszolgáló neve |
| Felhasználónév |hello felhasználónév, amelynek hozzáférési toowrite toohello adatbázis |
| Jelszó |hello jelszó tooconnect toohello adatbázis |
| Tábla |hello táblanév ahová hello kimeneti kerülnek. hello táblanév: kis-és nagybetűket, és ezt a táblázatot hello séma meg kell felelnie a mezők és a rendszer a feladat kimenetére által létrehozott pontosan toohello számát. |

> [!NOTE]
> A feladat kimenete a Stream Analytics jelenleg hello Azure SQL Database ajánlat támogatott. Csatolt egy adatbázist az SQL Server rendszert futtató Azure virtuális gépeket azonban nem támogatott. Ez a téma toochange a későbbi kiadásokban.
> 
> 

## <a name="blob-storage"></a>Blob Storage
A BLOB storage egy nagy mennyiségű strukturálatlan adatok tárolása hello felhő költséghatékony, méretezhető megoldást kínál.  Hello dokumentációját a témakörben megismerkedhet az Azure Blob storage és a használatát, [hogyan toouse Blobok](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

hello az alábbi táblázat a hello tulajdonság nevét és azok leírását egy blob kimenet létrehozásához.

<table>
<tbody>
<tr>
<td>TULAJDONSÁG NEVE</td>
<td>LEÍRÁS</td>
</tr>
<tr>
<td>A kimeneti Alias</td>
<td>Ez az egy rövid nevet használt a lekérdezések toodirect hello lekérdezés kimeneti toothis blob Storage tárolóban.</td>
</tr>
<tr>
<td>Tárfiók</td>
<td>ahol a kimeneti küldi hello tárfiók hello neve.</td>
</tr>
<tr>
<td>Tárfiók kulcsa</td>
<td>hello tartozó titkos kulcsot hello tárfiók.</td>
</tr>
<tr>
<td>A tároló</td>
<td>Tárolók adja meg a Microsoft Azure Blob szolgáltatás hello tárolt blobok logikai csoportosítása. A blob toohello Blob szolgáltatás feltöltésekor meg kell adnia, hogy a blob tárolója.</td>
</tr>
<tr>
<td>Elérési út előtag mintája [opcionális]</td>
<td>hello fájl elérési útját használt toowrite hello megadott tárolóban található blobok.<BR>Hello elérési útban úgy is dönthet, toouse egy vagy több példánya a következő 2 változó toospecify hello gyakoriság blobok írt hello:<BR>a {date}, {time}<BR>1. példa: cluster1/logs / {date} / {time}<BR>2. példa: cluster1/logs / {date}</td>
</tr>
<tr>
<td>[Választható] dátumformátum</td>
<td>Hello dátumtokent hello előtag elérési út használata esetén, amelyben a fájlok vannak rendszerezve hello dátumformátum választhatja meg. . Példa: Éééé/hh/nn</td>
</tr>
<tr>
<td>[Választható] idő formátuma</td>
<td>Ha hello idő jogkivonat hello előtag elérési útja, adja meg a hello idő formátumban, amelyben a fájlok vannak rendezve. Jelenleg csak a támogatott hello értéke HH.</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>A kimeneti adatok szerializálási formátum.  JSON, CSV és az avro-hoz támogatott.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Ha a fürt megosztott kötetei szolgáltatás- vagy JSON formátumú, kódolással meg kell adni. Az UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg.</td>
</tr>
<tr>
<td>Elválasztó</td>
<td>Csak a fürt megosztott kötetei szolgáltatás szerializálási alkalmazható. A Stream Analytics számos általánosan használt elválasztó karaktert támogatja a CSV-adatok szerializálása során. Támogatott értékei vesszővel, a pontosvesszővel válassza el, a terület, a lapon és a függőleges vonal.</td>
</tr>
<tr>
<td>Formátumban</td>
<td>Csak a JSON-szerializálás alkalmazható. Sorral elválasztott beállítás megadja, hogy hello kimeneti azzal, hogy minden JSON-objektum sortöréssel elválasztva kell formázni. A tömb határozza meg, hogy hello kimeneti lesznek formázva, egy JSON-objektumok tömbjét. A tömb csak amikor hello feladat leáll vagy a Stream Analytics áthelyezte a következő időszak toohello megszűnik. Ez általában elkülönített JSON, mivel nem igényel semmilyen különleges kezelést közben hello kimeneti fájl továbbra is írása a előnyösebb toouse sor.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Eseményközpont
[Az Event Hubs](https://azure.microsoft.com/services/event-hubs/) egy kiválóan méretezhető közzétételi-feliratkozási eseménybetöltőnek. Gyűjthet, hogy a több millió esemény / másodperc.  Egy Eseményközpont kimenetként használata egy Stream Analytics-feladat eredményének hello mindig hello bemeneti egy másik adatfolyam-feladatot.

Nincsenek szükséges tooconfigure Eseményközpont adatfolyamokat kimenetként néhány paramétereket.

| Tulajdonság neve | Leírás |
| --- | --- |
| A kimeneti Alias |Ez az a lekérdezések toodirect hello lekérdezés kimeneti toothis Eseményközpont használt rövid nevét. |
| Service Bus Namespace |A Service Bus-névtér az üzenetküldési entitások készletének tárolója. Egy új Eseményközpont létrehozásakor egy Szolgáltatásbusz-névtér is létrejött |
| Eseményközpont |az Event Hubs kimenet hello neve |
| Eseményközpont házirend neve |hello megosztott elérési házirendet, amely a hello Event Hub konfigurálása lapon is létrehozható. Minden megosztott elérési házirend fog rendelkezni egy névvel, Ön által meghatározott engedélyekkel és hozzáférési kulcsokkal |
| Event Hub házirend kulcs |hello megosztott elérési kulcsot használt tooauthenticate hozzáférés toohello Service Bus-névtér |
| [Választható] partíciós kulcs típusú oszlop |Ez az oszlop hello partíciós kulcs Eseményközpont a kimenet tartalmazza. |
| Esemény szerializálási formátum |A kimeneti adatok szerializálási formátum.  JSON, CSV és az avro-hoz támogatott. |
| Encoding |A fürt megosztott kötetei szolgáltatás és a JSON UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg |
| Elválasztó |Csak a fürt megosztott kötetei szolgáltatás szerializálási alkalmazható. A Stream Analytics számos általánosan használt elválasztó karaktert támogat az adatok CSV formátumban történő szerializálásához. Támogatott értékei vesszővel, a pontosvesszővel válassza el, a terület, a lapon és a függőleges vonal. |
| Formátumban |Csak érvényes JSON-típus. Sorral elválasztott beállítás megadja, hogy hello kimeneti azzal, hogy minden JSON-objektum sortöréssel elválasztva kell formázni. A tömb határozza meg, hogy hello kimeneti lesznek formázva, egy JSON-objektumok tömbjét. |

## <a name="power-bi"></a>Power BI
[A Power BI](https://powerbi.microsoft.com/) egy Stream Analytics-feladat tooprovide elemzés eredményeinek gazdag képi megjelenítés élmény kimenetként használható. Ez a funkció a működési irányítópultok, a jelentés létrehozásához és a jelentéskészítési folyamat metrika használható.

### <a name="authorize-a-power-bi-account"></a>Engedélyezze a Power BI-fiókja
1. A Power BI kiválasztásakor az Azure-portálon hello kimenetként fogja felszólító tooauthorize lehet egy meglévő Power BI-felhasználó vagy toocreate új Power BI-fiókkal.  
   
   ![Engedélyezze a Power BI-felhasználó](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Hozzon létre egy új fiókot, ha nem még egy, majd engedélyezze most kattintson.  Egy hasonló hello képernyő áll rendelkezésre.  
   
   ![Azure-fiók Power bi-ban](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. Ebben a lépésben adja meg a hello munkahelyi vagy iskolai fiók, amelyek engedélyezik a hello Power BI-kimenet. Ha még vannak nem jelentkezett a Power BI, válasszon jelentkezzen. hello munkahelyi vagy iskolai fiókkal használhatja a Power BI különbözhetnek hello Azure-előfizetés fiók, amely is van jelenleg bejelentkezve.

### <a name="configure-hello-power-bi-output-properties"></a>Hello Power BI kimeneti tulajdonságainak konfigurálása
Miután hello Power BI-fiók hitelesítését, a Power BI-kimenet állíthatja be a hello tulajdonságait. az alábbi táblázat hello tulajdonságnevek hello listája és azok leírása tooconfigure a Power BI-kimenet.

| Tulajdonság neve | Leírás |
| --- | --- |
| A kimeneti Alias |Ez az egy rövid nevet használt lekérdezések toodirect hello lekérdezés kimeneti toothis Power bi-kimenet. |
| Csoport munkaterület |adatok megosztása más Power BI-felhasználókkal tooenable választhat a Power BI belül csoportok fiókjaként, válassza a "Saját munkaterület", ha nem szeretné, hogy toowrite tooa csoport.  Meglévő csoport frissítése megújítása hello Power BI-hitelesítés szükséges. |
| A DataSet neve |Adja meg a DataSet adatkészlet neve, hogy van-e szükség a Power BI hello kimeneti toouse |
| Tábla neve |Adjon meg egy tábla nevét, a Power BI kimeneti hello hello adatkészlet alapján. Jelenleg a Power BI kimenetét a Stream Analytics-feladatok csak rendelkezhet egy tábla egy adatkészlet |

A Power BI-kimenet és az irányítópult konfigurálásának segédlet, talál hello [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) cikk.

> [!NOTE]
> Explicit módon létrehozni hello adatkészlet és a tábla hello Power BI-irányítópultot. hello adatkészlet és a tábla adatok automatikusan kitöltődnek hello feladat elindult és hello feladat szivattyúzó kimeneti elindul a Power BI-bA. Vegye figyelembe, hogy a beállítást, ha hello feladat lekérdezés nem jönnek létre, bármely eredményeket, hello adatkészlet és a tábla nem hozható létre. Azt is vegye figyelembe, hogy ha a Power bi-ban már egy adatkészlet és a tábla hello hello azonos néven egy megadott a Stream Analytics-feladat, hello meglévő adatokat a rendszer felülírja.
> 
> 

### <a name="schema-creation"></a>Séma létrehozása
Az Azure Stream Analytics egy Power BI dataset és hello felhasználó nevében táblázatot hoz létre, ha egy nem létezik. Minden más esetben hello tábla frissül új értékekkel. Jelenleg nincs hello korlátozása, hogy csak egy tábla egy adatkészleten belül létezhet.

### <a name="data-type-conversion-from-asa-toopower-bi"></a>Adattípus-konverzió ASA tooPower BI
Az Azure Stream Analytics hello adatmodell futásidőben dinamikusan frissíti, ha a hello kimeneti sémaváltozások. Oszlop neve megváltozik, az oszlop típusa módosításokat, és a hello hozzáadása vagy a oszlopok eltávolítása minden nyomon követni.

A táblázat ismerteti az adatok típuskonverziók hello a [Stream Analytics adattípusok](https://msdn.microsoft.com/library/azure/dn835065.aspx) tooPower BIs [Entity Data Model (EDM) típusok](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) a POWER BI adatkészlet és a tábla nem léteznek.


A Stream Analytics | tooPower BI
-----|-----|------------
bigint | Int64
típus: nvarchar(max) | Karakterlánc
Dátum és idő | Dátum és idő
Lebegőpontos | Dupla
Rekord tömb | Karakterlánc típus, konstans érték "IRecord" vagy "IArray"

### <a name="schema-update"></a>Séma frissítése
A Stream Analytics kikövetkezteti hello adatok alapján hello első hello kimeneti események modellsémát. Később Ha szükséges, hello adatok modellsémát frissített tooaccommodate bejövő események, amelyek esetleg nem felelnek meg hello eredeti séma.

Hello `SELECT *` kerülni kell a lekérdezés több sort tooprevent dinamikus séma módosítása. Ezenkívül toopotential teljesítményre gyakorolt hatása is okozhat a hello eredmények hello idő indeterminacy. hello pontos mezők jelenik meg a Power BI-irányítópultot toobe ki kell jelölni. Emellett hello adatértékek megfelelő kell lennie a kiválasztott adattípust hello.


Előző/aktuális | Int64 | Karakterlánc | Dátum és idő | Dupla
-----------------|-------|--------|----------|-------
Int64 | Int64 | Karakterlánc | Karakterlánc | Dupla
Dupla | Dupla | Karakterlánc | Karakterlánc | Dupla
Karakterlánc | Karakterlánc | Karakterlánc | Karakterlánc |  | Karakterlánc | 
Dátum és idő | Karakterlánc | Karakterlánc |  Dátum és idő | Karakterlánc


### <a name="renew-power-bi-authorization"></a>Újítsa meg a Power BI engedélyezési
Szüksége lesz a toore-hitelesítés Power BI-fiókjába, ha a jelszó megváltozott a feladat meg lett létrehozva, vagy utolsó hitelesített. Ha a multi-factor Authentication (MFA) konfigurálva van az Azure Active Directory (AAD) bérlő toorenew Power BI engedélyezési minden második héten is szüksége lesz. A probléma tünete nincs feladat kimenet és a "hitelesítés felhasználói hibát jelez" hello műveletnaplók:

  ![A Power BI frissítési jogkivonat hiba](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

tooresolve probléma leállítja a futó feladatot, és nyissa meg a Power BI-kimenet tooyour.  Hello "Megújítási engedélyezési" hivatkozásra, és indítsa újra a feladatot a hello feladat utolsó befejezési időpontja tooavoid adatvesztés.

  ![A Power BI újra a portálon](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Az Azure Table storage](../storage/common/storage-introduction.md) kínál a magas rendelkezésre állású, nagymértékben méretezhető tárolót, így az alkalmazás automatikusan átméretezheti magát toomeet felhasználó igény szerint. A TABLE storage a Microsoft NoSQL kulcs-/ attribútumtár azt, amelyiket a strukturált adatok kihasználhatják kevesebb korlátozza a hello séma. Az Azure Table storage lehet használt toostore adatok megőrzése és hatékony lekérdezéséhez.

hello az alábbi táblázat hello tulajdonságnevek és azok leírását a tábla kimenet létrehozása.

| Tulajdonság neve | Leírás |
| --- | --- |
| A kimeneti Alias |Ez az egy rövid nevet használt lekérdezések toodirect hello lekérdezés kimeneti toothis table storage-ban. |
| Tárfiók |ahol a kimeneti küldi hello tárfiók hello neve. |
| Tárfiók kulcsa |hello hello tárfiók tartozó hozzáférési kulcsot. |
| Tábla neve |hello hello tábla neve. Ha még nem létezik a hello tábla rendszer létrehozza. |
| Partíciókulcs |hello hello kimeneti oszlop tartalmazó hello partíciós kulcs neve. hello partíciós kulcs hello partíció egy entitás elsődleges kulcsának első részét hello egy adott táblán belül egyedi azonosítója. Egy lehet, hogy másolatot too1 KB méretű karakterláncérték. |
| Sorkulcsa |hello hello kimeneti oszlop tartalmazó sor hello kulcs neve. hello sorkulcsa egy adott partíció egy entitás egyedi azonosítója. Hello egy entitás elsődleges kulcsának második részét képezi. hello sorkulcsa egy lehet, hogy másolatot too1 KB méretű karakterláncérték. |
| Köteg mérete |a kötegelt művelet rekordjainak hello száma. Hello alapértelmezett általában elegendő-e a legtöbb feladat, tekintse meg a toohello [tábla kötegelt művelet spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) Ez a beállítás módosításával kapcsolatos további részletekért. |

## <a name="service-bus-queues"></a>Service Bus által kezelt üzenetsorok
[Service Bus-üzenetsorok](https://msdn.microsoft.com/library/azure/hh367516.aspx) kínálnak a First In, First Out (, FIFO) üzenet kézbesítési tooone vagy több versengő fogyasztó számára. Üzenetek általában várt toobe fogad és dolgoz fel hello fogadók hello historikus ahhoz, amelyben toohello várólista addig adták hozzá, és minden üzenetet kapott, és csak egy üzenetfogyasztó által feldolgozott.

hello az alábbi táblázat hello tulajdonságnevek és azok leírását a várólista kimenet létrehozása.

| Tulajdonság neve | Leírás |
| --- | --- |
| A kimeneti Alias |Ez az egy rövid nevet használt a lekérdezések toodirect hello lekérdezés kimeneti toothis Service Bus-üzenetsorba. |
| Service Bus Namespace |A Service Bus-névtér az üzenetküldési entitások készletének tárolója. |
| Várólista neve |Service Bus-üzenetsorba hello hello neve. |
| Várólista házirend neve |A várólista létrehozásakor, a megosztott elérési házirendeket hello sor konfigurálása lapon is létrehozhat. Minden megosztott elérési házirend fog rendelkezni egy névvel, Ön által meghatározott engedélyekkel és hozzáférési kulcsokkal. |
| Várólista házirend kulcs |hello megosztott elérési kulcsot használt tooauthenticate hozzáférés toohello Service Bus-névtér |
| Esemény szerializálási formátum |A kimeneti adatok szerializálási formátum.  JSON, CSV és az avro-hoz támogatott. |
| Encoding |A fürt megosztott kötetei szolgáltatás és a JSON UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg |
| Elválasztó |Csak a fürt megosztott kötetei szolgáltatás szerializálási alkalmazható. A Stream Analytics számos általánosan használt elválasztó karaktert támogat az adatok CSV formátumban történő szerializálásához. Támogatott értékei vesszővel, a pontosvesszővel válassza el, a terület, a lapon és a függőleges vonal. |
| Formátumban |Csak érvényes JSON-típus. Sorral elválasztott beállítás megadja, hogy hello kimeneti azzal, hogy minden JSON-objektum sortöréssel elválasztva kell formázni. A tömb határozza meg, hogy hello kimeneti lesznek formázva, egy JSON-objektumok tömbjét. |

## <a name="service-bus-topics"></a>Service Bus-üzenettémák
Amíg a Service Bus-üzenetsorok, adja meg a küldő tooreceiver egy tooone kommunikációs módszer [Service Bus-üzenettémakörök](https://msdn.microsoft.com/library/azure/hh367516.aspx) egy egy-a-többhöz típusú kommunikációt biztosítanak.

hello az alábbi táblázat hello tulajdonságnevek és azok leírását a tábla kimenet létrehozása.

| Tulajdonság neve | Leírás |
| --- | --- |
| A kimeneti Alias |Ez az egy rövid nevet használt a lekérdezések toodirect hello lekérdezés kimeneti toothis Service Bus-témakörbe. |
| Service Bus Namespace |A Service Bus-névtér az üzenetküldési entitások készletének tárolója. Egy új Eseményközpont létrehozásakor egy Szolgáltatásbusz-névtér is létrejött |
| A témakör neve |Témák üzenetküldési entitások, hasonló tooevent hubs és a várólisták. Több különféle eszközről és a szolgáltatások a tervezett toocollect eseményfolyamokat fontosságúak. A témakör létrehozásakor is kap egy egyedi nevet. hello üzenetek küldése tooa témakör is csak akkor érhető el egy előfizetés létrehozása, ezért figyeljen oda arra egy vagy több előfizetés hello témakör alatt van |
| A témakör házirend neve |Amikor létrehoz egy témát, a megosztott elérési házirendeket hello témakör konfigurálása lapon is létrehozhat. Minden megosztott elérési házirend fog rendelkezni egy névvel, Ön által meghatározott engedélyekkel és hozzáférési kulcsokkal |
| A témakör házirend kulcs |hello megosztott elérési kulcsot használt tooauthenticate hozzáférés toohello Service Bus-névtér |
| Esemény szerializálási formátum |A kimeneti adatok szerializálási formátum.  JSON, CSV és az avro-hoz támogatott. |
| Encoding |Ha a fürt megosztott kötetei szolgáltatás- vagy JSON formátumú, kódolással meg kell adni. Az UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg |
| Elválasztó |Csak a fürt megosztott kötetei szolgáltatás szerializálási alkalmazható. A Stream Analytics számos általánosan használt elválasztó karaktert támogat az adatok CSV formátumban történő szerializálásához. Támogatott értékei vesszővel, a pontosvesszővel válassza el, a terület, a lapon és a függőleges vonal. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Az Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) van egy teljes körűen felügyelt nosql típusú dokumentumadatbázis-szolgáltatás, amely a lekérdezés és a tranzakciók nyújt, mint a sémamentes adatokra, kiszámítható és megbízható teljesítményt és gyors fejlesztést.

hello részletek hello tulajdonság nevét és a leírást az Azure Cosmos DB kimenet létrehozása alatt.

* **A kimeneti Alias** – egy alias toorefer ezt a kimenetet a ASA lekérdezés  
* **A fiók neve** – hello nevét vagy a végpont URI-jának hello Cosmos DB fiók.  
* **Kulcs fiók** – Cosmos DB fiók hello hello megosztott hozzáférési kulcsot.  
* **Adatbázis** – hello Cosmos DB adatbázis neve.  
* **Gyűjteménynévmintája** – hello gyűjtemény neve vagy a használt hello gyűjtemények toobe mintát. hello gyűjteménynév-formátum használatával hello opcionális {partition} token, ahol a partíciók 0-tól kezdődnek lehet létrehozni. Minta érvényes bemenetei a következők:  
  1\) MyCollection – egy gyűjteményt a következő "MyCollection" néven már léteznie kell.  
  2\) MyCollection {partition} – ilyen gyűjteményeknek létezniük kell – "MyCollection0", "MyCollection1", "MyCollection2" és így tovább.  
* **Kulcs partícióazonosító** – nem kötelező. Ez csak akkor van szükség, ha a gyűjteménynévmintája {particionáló} jogkivonatot használ. a kimeneti használt események toospecify hello kulcsban a kimenet gyűjtemények közötti particionálására hello mező hello nevét. Egyetlen gyűjtemény kimeneti bármilyen tetszőleges kimeneti oszlop lehet például PartitionId használt.  
* **Dokumentálja azonosító** – nem kötelező. a kimeneti eseményekben a hello mező hello nevét használja toospecify hello elsődleges kulcs mely Beszúrás vagy frissítés műveletek alapulnak.  


## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
Már bevezetett tooStream Analytics, a felügyelt szolgáltatás streaming analytics hello az eszközök internetes hálózatát származó adatokon. További információ a szolgáltatás toolearn lásd:

* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
