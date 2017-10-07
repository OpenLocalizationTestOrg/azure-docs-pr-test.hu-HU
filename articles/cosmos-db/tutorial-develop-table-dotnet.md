---
title: "Az Azure Cosmos DB: Hello tábla API a .NET fejlesztést |} Microsoft Docs"
description: "Megtudhatja, hogyan toodevelop Azure Cosmos DB tábla API-t a .NET használatával"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 70c6985a1dffdbcdb07e377f8ad10355bb97712a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-table-api-in-net"></a>Azure Cosmos DB: Tábla API a .NET hello fejlesztést

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.

Ez az oktatóanyag ismerteti a következő feladatok hello: 

> [!div class="checklist"] 
> * Azure Cosmos DB-fiók létrehozása 
> * A funkciónak az üdvözlő app.config fájlban 
> * Tábla létrehozása hello segítségével [tábla API](table-introduction.md) (előzetes verzió)
> * Egy entitás tooa táblázat hozzáadása 
> * Entitásköteg beszúrása 
> * Egyetlen entitás lekérdezése 
> * Lekérdezés entitások automatikus másodlagos indexek használata 
> * Entitás cseréje 
> * Entitás törlése 
> * Tábla törlése
 
## <a name="tables-in-azure-cosmos-db"></a>Az Azure Cosmos DB táblák 

Az Azure Cosmos DB biztosít hello [tábla API](table-introduction.md) (előzetes) séma nélküli kialakítás egy kulcs-érték tároló igénylő alkalmazásokhoz. [Az Azure Table storage](../storage/common/storage-introduction.md) SDK-k és a REST API-k lehet az Azure Cosmos DB használt toowork. Magas teljesítmény-követelményekkel rendelkező Azure Cosmos DB toocreate táblák is használhatja. Az Azure Cosmos DB jelenleg nyilvános előzetes verzióban támogatja az átviteli sebességre optimalizált táblákat (más néven „prémium szintű táblákat”). 

Azure Table storage magas tárolási és alacsonyabb átviteli követelmények táblák toouse tovább. Azure Cosmos DB bevezet egy jövőbeli frissítéssel tárolási optimalizált táblák támogatása, és a meglévő és új Azure Table storage-fiókok zökkenőmentesen lesz frissítve tooAzure Cosmos DB.

Ha Azure Table storage jelenleg használ, a következő előnyöket hello "prémium table" előnézettel hello ettől kezdve:

- Kulcsrakész [globális terjesztési](distribute-data-globally.md) a többszörös homing és [automatikus és manuális feladatátvétel](regional-failover.md)
- Automatikus séma-független elleni tulajdonságokat ("másodlagos indexek"), és gyors lekérdezéseket indexelő támogatása 
- Támogatja az [független méretezése tárolási és átviteli](partition-data.md), tetszőleges számú régiók között
- Támogatja az [táblánként dedikált átviteli](request-units.md) , amely az akár több százszor is méretezhető toomillions kérelmek / másodperc
- Támogatja az [öt konzisztenciaszinteket](consistency-levels.md) tootrade rendelkezésre állás, a késés és a konzisztencia ki az alkalmazások igényeihez alapján
- rendelkezésre állás 99,99 % belül egy régiót, és képes tooadd további magas rendelkezésre állás érdekében, régiók és [iparágvezető átfogó SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) az általánosan rendelkezésre álló
- Hello meglévő az Azure storage .NET SDK-val, és egyetlen kód módosítások tooyour alkalmazás

Hello előzetes Azure Cosmos DB támogatja hello tábla API hello .NET SDK használatával. Letöltheti a hello [Azure Storage szolgáltatás előzetes SDK](https://aka.ms/premiumtablenuget) a Nugetből, amely rendelkezik hello azonos osztályok és metódus-aláírása hello, [Azure Storage szolgáltatás SDK](https://www.nuget.org/packages/WindowsAzure.Storage), de is kapcsolódhatnak tooAzure Cosmos DB fiókok hello használata Tábla API.

További információ az Azure Table storage összetett feladatok, toolearn lásd:

* [Bevezetés tooAzure Cosmos DB: tábla API](table-introduction.md)
* Table szolgáltatás referenciadokumentációt elérhető API-kat vonatkozó hello [Storage ügyféloldali kódtára a .NET-referencia](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

### <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Ez az oktatóanyag a fejlesztők számára, aki ismeri a hello Azure Table storage SDK, és szeretné toouse hello premium szolgáltatásainak használata Azure Cosmos DB. Alapul [Ismerkedés az Azure Table storage használatának .NET](table-storage-how-to-use-dotnet.md) és bemutatja, hogyan tootake előnye, hogy további funkciókat, például másodlagos indexek, a létesített átviteli sebesség és a többszörös homing. A Microsoft fedik le, hogyan toouse hello Azure portál toocreate Azure Cosmos DB adatait, majd létrehozása és egy tábla alkalmazás központi telepítése. A Microsoft .NET példákból létrehozása és egy tábla törlésével és beszúrni, frissítése, törlése és tábla adatok lekérdezése is ismerteti. 

Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

Először hozzon létre egy Azure Cosmos DB fiókot a hello Azure-portálon.  

> [!TIP]
> * Már van Azure Cosmos DB fiókja? Ha igen, hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).
> * Kellett az Azure DocumentDB-fiókot? Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).  
> * Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting tooany location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Most tegyük klónozza a githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt egy tábla alkalmazást.

1. Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.  

2. Futtassa a következő parancs tooclone hello minta tárház hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. Ezután nyissa meg a hello megoldásfájlt a Visual Studio.

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.

1. A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**. Hello képernyő toocopy hello kapcsolati karakterlánc jobb oldalán hello hello másolási gombok hello app.config fájlba hello következő lépésben fogja használni.

2. A Visual Studióban nyissa meg a hello app.config fájlt. 

3. Az URI értéket másol a portálról hello (hello Másolás gombra), és teszi hello hello fiók-kulcs az App.config fájlban értékét. Használja az App.config fájlban fióknevet a korábban létrehozott hello fióknevet.
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> toouse toochange hello kapcsolati karakterláncot a kell ezt az alkalmazást az szabványos Azure Table Storage, `app.config file`. Azure Storage elsődleges kulcsként használja tábla fióknevet és kulcsot hello fiók nevét. <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-hello-app"></a>Hozza létre és hello alkalmazás üzembe helyezése
1. A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**. 

2. A hello NuGet **Tallózás** mezőbe írja be ***windowsazure.Storage kifejezésre-PremiumTable***. Ellenőrizze **előzetes verzióját tartalmazzák**.

3. Hello eredmények közül telepítse a hello **windowsazure.Storage kifejezésre-PremiumTable** , és válassza a hello preview build `0.0.1-preview`. Ez a művelet hello Azure Table storage csomag és az összes függőséget telepíti.

4. Kattintson a CTRL + F5 toorun hello alkalmazás. 

Ezután lépjen vissza tooData Explorer és tekintse meg a lekérdezés, módosítása, és a tábla adatokkal dolgozni. 

> [!NOTE]
> toouse az alkalmazás emulátorral egy Azure Cosmos DB, csak akkor kell toochange hello kapcsolati karakterláncot a `app.config file`. Használja a hello Emulator érték alá. <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a>Azure Cosmos DB-képességek
Azure Cosmos DB képességeket kínál, amelyek nem érhetők el a hello Azure Table storage API számos támogat. hello új funkciók engedélyezéséhez hello következő `appSettings` konfigurációs értékeket. Bármely új aláírások vagy túlterhelés toohello preview Azure Storage szolgáltatás SDK nem bemutatása volt után. Ez lehetővé teszi, hogy tooconnect tooboth standard és premium táblák és az egyéb Azure Storage-szolgáltatásokat, mint a blobokat és üzenetsorokat használata. 


| Kulcs | Leírás |
| --- | --- |
| TableConnectionMode  | Azure Cosmos-adatbázis két csatlakozási módot támogat. A `Gateway` mód, mindig kérések toohello Azure Cosmos DB átjárón, amely továbbítja azt toohello megfelelő adatok partíciókat. A `Direct` csatlakozási mód hello ügyfél lekéri a táblák toopartitions hello leképezése, és a kérések közvetlenül az adatok partíciók szemben. Ajánlott `Direct`, alapértelmezett hello.  |
| TableConnectionProtocol | Az Azure Cosmos DB támogatja két kapcsolat protokoll - `Https` és `Tcp`. `Tcp`hello alapértelmezett, és javasolt, mert több egyszerűsített. |
| TablePreferredLocations | Előnyben részesített (többhelyű) helyek az olvasási műveletek vesszővel elválasztott listája. Minden Azure Cosmos DB fiókhoz társítható 1-30 + régiók. Minden ügyfél példány előnyben részesített hello ahhoz, hogy kis késleltetésű olvasási adhatja meg e régiók egy részét. hello régiók névvel kell ellátni használatával a [megjelenített neveket](https://msdn.microsoft.com/library/azure/gg441293.aspx), például `West US`. Lásd még: [több homing API-k](tutorial-global-distribution-table.md).
| TableConsistencyLevel | Akkor is kompromisszumot közötti késleltetés, konzisztencia- és rendelkezésre állás öt jól meghatározott konzisztenciaszintek közötti választással: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, és `Eventual`. Alapértelmezett érték a `Session`. hello választott konzisztenciaszint lehetővé teszi egy jelentős teljesítménybeli különbség több területi beállításokat. Lásd: [konzisztenciaszintek](consistency-levels.md) részleteiről. |
| TableThroughput | Fenntartott átviteli sebességet a kérelemegység (RU) másodpercenként kifejezett hello tábla. Egyetlen tábla RU/mp 100-as egység millióit képes támogatni. Lásd: [egységek kérelem](request-units.md). Alapértelmezett érték`400` |
| TableIndexingPolicy | Egységes és automatikus másodlagos indexelése lévő táblák oszlopok | JSON string indexelő házirend meghatározása megfelelő toohello. Lásd: [indexelő házirend](indexing-policies.md) toosee hogyan módosíthatja az egyes oszlopok indexelési házirend tooinclude/kizárási. | Automatikus indexeléshez levő összes tulajdonság (kivonatoló karakterláncok), és a számok tartománya |
| TableQueryMaxItemCount | Konfigurálja a hello tábla lekérdezésenként egyetlen oda-vissza a visszaadott elemek maximális számát. Alapértelmezett érték a `-1`, ami lehetővé teszi, hogy Azure Cosmos DB hello értéket futásidőben dinamikus meghatározásához. |
| TableQueryEnableScan | Hello lekérdezés hello index bármely szűrő nem használható, ha majd, mégis futtatni a vizsgálat keresztül. Alapértelmezett érték a `false`.|
| TableQueryMaxDegreeOfParallelism | a kereszt-partíció lekérdezés végrehajtási párhuzamossági hello fok. `0`a nem lehívását, soros `1` lehívását a soros, és nagyobb értékek hello növelési párhuzamossági. Alapértelmezett érték a `-1`, ami lehetővé teszi, hogy Azure Cosmos DB hello értéket futásidőben dinamikus meghatározásához. |

toochange hello alapértelmezett érték, nyissa meg hello `app.config` fájlt a Visual Studio megoldáskezelőjében. Adja hozzá a hello hello tartalmát `<appSettings>` elem alább látható. Cserélje le `account-name` hello nevet a tárfiók, és `account-key` a fiók hozzáférési kulccsal. 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Most Meggyőződünk arról, mi történik a hello app gyors áttekintése. Nyissa meg hello `Program.cs` fájlt, és találja, hogy ezek a sorok, a kód hello tábla erőforrások létrehozása. 

## <a name="create-hello-table-client"></a>Hello tábla ügyfél létrehozása
Ön inicializálni egy `CloudTableClient` tooconnect toohello tábla fiók.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Ez az ügyfél inicializálása hello segítségével `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, és `TablePreferredLocations` konfigurációs értékei, ha meg van adva a hello Alkalmazásbeállítások.
    
## <a name="create-a-table"></a>Tábla létrehozása
Ezután létrehozhat egy tábla használatával `CloudTable`. Azure Cosmos DB táblák egymástól függetlenül is méretezhető tárolási és átviteli sebesség szempontjából, és particionálás kezeli automatikusan hello szolgáltatást. Azure Cosmos-adatbázis a rögzített méretű és korlátlan táblák is támogatja. Lásd: [Azure Cosmos DB a particionálás](partition-data.md) részleteiről. 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

Nincs a táblák létrehozását egy fontos különbséggel. Cosmos. Azure-adatbázis fenntartja a teljesítményt, eltérően az Azure storage fogyasztás alapján modell az egyes tranzakciókra vonatkozóan. hello foglalás modellnek két nagy előnye van:

* Az átviteli sebesség dedikált/foglalt, így Ön soha nem get szabályozva a kérelmek aránya vagy az alatt a létesített átviteli sebesség esetén
* hello foglalás modell több [költséghatékony a teljesítmény-gyakori feladatok](key-value-store-cost.md)

Hello alapértelmezett átviteli konfigurálhatja a hello beállítás konfigurálásával `TableThroughput` RU (kérelemegység) másodpercenként tekintetében. 

Egy 1 KB-os entitás olvasási van normalizált 1 RU és egyéb műveletekre RU érték a Processzor, memória és IOPS fogyasztás alapján rögzített normalizált tooa. További információ [Azure Cosmos DB egység kérelem](request-units.md).

> [!NOTE]
> A Table storage SDK jelenleg nem támogatja a módosítása átviteli, amíg hello átviteli azonnal bármikor hello Azure-portálon vagy az Azure parancssori felület használatával módosíthatja.

Ezután azt hello egyszerű olvasható útmutatót, és írási (CRUD) műveletet hello Azure Table storage SDK használatával. Ez az oktatóanyag azt mutatja be, előre jelezhető alacsony egyjegyű ezredmásodperces késések és Azure Cosmos DB által nyújtott gyors lekérdezéseket.

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása
Az Azure Table storage entitások terjessze ki a hello `TableEntity` osztályhoz, és rendelkeznie kell `PartitionKey` és `RowKey` tulajdonságok. Íme egy minta definíciója egy ügyfélentitást.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

a következő kódrészletet hello bemutatja, hogyan tooinsert egy entitás a hello az Azure storage SDK. Azure Cosmos DB végzi a kis késleltetésű bármilyen léptékben garantált hello world között.

Írási műveletek befejezése < 15 ms p99 és a futó alkalmazások p50 ~ 6 ms hello és hello Azure Cosmos DB fiók ugyanabban a régióban. És az ezen időtartam hello tényt, hogy beírja a fiókok ismernek hátsó toohello ügyfél csak azokat a rendszer szinkron módon replikálja, tartósan véglegesítve lett, és minden tartalom indexelt után.

Tábla API for Azure Cosmos DB hello jelenleg előzetes verzióban érhető. Általánosan rendelkezésre álló hello p99 késés garanciák SLA-k, például a más Azure Cosmos DB API-k által támogatott. 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute hello insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Entitásköteg beszúrása
Az Azure Table storage támogatja egy kötegelt művelet API-t, amely lehetővé teszi frissítés, törlés, kombinálhatja, és a Beszúrás hello egyetlen kötegművelettel. Azure Cosmos-adatbázis nem lehet hello korlátozások némelyike hello kötegelt API-t az Azure Table storage. Például egy kötegelt belül többszöri beolvasás végezheti el, több írások toohello hajthat végre egy kötegelt belül ugyanaz az entitás és kötegenként 100 műveletek korlátozva van. 

```csharp
// Create hello batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it toohello table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it toohello table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities toohello batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute hello batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a>Egyetlen entitás lekérdezése
(Lekérdezi) lekérdezi a teljes Azure Cosmos DB < 10 ms p99 és ~ 1, a p50 ms hello azonos Azure-régiót. Tetszőleges számú régiók tooyour fiók hozzáadása a kis késleltetésű olvasása, és központi telepítése a helyi régió ("többhelyű") az alkalmazások tooread beállításával `TablePreferredLocations`. 

A következő kódrészletet hello segítségével egyetlen entitás le:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> További tudnivalók a többhelyű API-k [fejlesztés több régióba az](tutorial-global-distribution-table.md)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Lekérdezés entitások automatikus másodlagos indexek használata
Táblák hello lekérdezhetők `TableQuery` osztály. Azure Cosmos-adatbázis egy adatbázis írási optimalizált motor, amely automatikusan elvégzi a belül a táblázat összes oszlopa van. Az Azure Cosmos Adatbázisba indexelő független tooschema. Ezért még akkor is, ha a séma sorok különböznek, vagy hello séma fejlődésének adott idő alatt, akkor automatikusan indexelt. Mivel Azure Cosmos DB támogatja az automatikus másodlagos indexek, a lekérdezések egyik tulajdonságnak sem használható hello index, és hatékonyan szolgáltatható.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

A képen Azure Cosmos DB támogatja hello azonos Azure Table storage a tábla API hello funkciókat lekérdezése. Azure Cosmos-adatbázis is támogatja a rendezést, összesítések, a földrajzi lekérdezést, a hierarchia és a számos különféle beépített funkciók. Tábla API hello jövőbeli szolgáltatásfrissítés hello további funkciók lesznek közzétéve. Lásd: [Azure Cosmos adatbázis-lekérdezés](documentdb-sql-query.md) ezeket a képességeket áttekintését. 

## <a name="replace-an-entity"></a>Entitás cseréje
tooupdate egy entitás lekéréséhez hello Table szolgáltatásból, módosítsa a hello forrásentitás-objektum, és majd hello módosítások mentése toohello Table szolgáltatás biztonsági. hello alábbira vált egy meglévő ügyfél telefonszámát. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Hasonló módon végezheti el `InsertOrMerge` vagy `Merge` műveletek.  

## <a name="delete-an-entity"></a>Entitás törlése
Egyszerűen törölheti entitás hello segítségével beolvasása után egy entitás frissítésénél bemutatott minta. a következő kód hello kéri le, és töröl egy ügyfélentitást.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Tábla törlése
Végezetül alábbi kódpéldát hello törölhető egy tábla a tárfiókból. Töröl, és hozza létre újból az azonnali Azure Cosmos DB tartalmazó tábla.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása 

Toocontinue toouse az alkalmazás nem fog, ha használja a következő lépéseket toodelete hello Azure-portálon az oktatóprogram során létrehozott összes erőforrás hello.   

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.  
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**. 

## <a name="next-steps"></a>Következő lépések

Az oktatóanyag azt kezelt indításának a tábla API hello Azure Cosmos DB használatával tooget, és hello következő ezt: 

> [!div class="checklist"] 
> * Egy Azure Cosmos DB-fiók létrehozása 
> * Engedélyezett funkciókat hello app.config fájlban 
> * Táblázat létrehozása 
> * Egy entitás tooa tábla hozzáadva 
> * Szúrja be egy teljes entitásköteget 
> * Egyetlen entitás lekérése 
> * Automatikus másodlagos indexek használatával lekérdezett entitásokat 
> * Entitás cseréje 
> * Egy entitás törlése 
> * Tábla törlése  

Ezután folytassa a következő oktatóanyag toohello és tudhat meg többet a tábla adatainak lekérdezésekor. 

> [!div class="nextstepaction"]
> [Tábla API hello lekérdezése](tutorial-query-table.md)
