## <a name="what-is-table-storage"></a>A Table Storage ismertetése
Az Azure Table Storage nagy mennyiségű strukturált adat tárolására alkalmas. hello szolgáltatás áll, egy NoSQL-adattár, amely elfogadja érkező hitelesített hívásokat belüli és kívüli hello Azure felhőben. Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak. A Table Storage gyakori használati módjai:

* Több TB-nyi, webes méretű alkalmazások kiszolgálására alkalmas strukturált adat tárolása
* Olyan adatkészletek tárolása, amelyekhez nincs szükség bonyolult illesztésekre, külső kulcsokra vagy tárolt eljárásokra, és a gyors hozzáférés érdekében denormalizálhatók
* Adatok gyors lekérdezése fürtözött indexszel
* A WCF Data Service .NET-kódtárakra hello OData protokoll és a LINQ-lekérdezések használja adatok elérése

Használhatja a Table storage toostore és lekérdezés hatalmas strukturált, nem relációs adatokat, és a táblák a igény szerinti növekedése lehessen méretezni.

## <a name="table-storage-concepts"></a>A Table Storage szolgáltatással kapcsolatos alapfogalmak
A TABLE storage a következő összetevők hello tartalmazza:

![Table Storage-összetevők ábrája][Table1]

* **URL-címformátum:** A kód egy fiók tábláira hivatkozik az alábbi címformátummal:   
  http://`<storage account>`.table.core.windows.net/`<table>`  
  
  Meg lehet oldani az Azure-táblákban közvetlenül hello OData protokoll fenti címet használja. További információk az [OData.org][OData.org] webhelyen találhatók.
* **Tárfiók:** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot. A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md) (Az Azure Storage méretezhetőségi és teljesítménycéljai).
* **Tábla:** A tábla az entitások gyűjteményét tartalmazza. A táblák nem kényszerítenek sémát az entitásokra, ami azt jelenti, hogy egyetlen tábla különböző tulajdonságkészletekkel rendelkező entitásokat is tartalmazhat. a tárfiók tartalmazó táblák hello száma csak hello tárolási kapacitási határa korlátozza korlátozza.
* **Entitás**: entitás tulajdonságait, tooa adatbázissorhoz hasonló. Egy entitás mentése too1MB méretű lehet.
* **Tulajdonságok:** A tulajdonság egy név-érték pár. Minden entitás too252 tulajdonságok toostore adatokat tartalmazhatnak. Minden entitás három rendszertulajdonsággal rendelkezik, amelyek egy partíciókulcsot, egy sorkulcsot és egy időbélyegzőt adnak meg. Hello ugyanazzal a partíciókulccsal kell gyorsabban lekérdezhetők, és beállíthatja, szűrhatók be/frissíthetők atomi műveletek rendelkező entitások. Egy entitás sorkulcsa a partíción belüli azonosítója.

Táblák és tulajdonságok elnevezésével kapcsolatos részletekért lásd: [ismertetése hello tábla szolgáltatás adatmodell](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
