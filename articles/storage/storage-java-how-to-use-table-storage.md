---
title: a Table storage a Java aaaHow toouse |} Microsoft Docs
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
services: storage
documentationcenter: java
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 45145189-e67f-4ca6-b15d-43af7bfd3f97
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: f72cac3fc10cf0aef74780b84c515d93d715d787
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-java"></a>Hogyan toouse Table storage-ának Java
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Table storage szolgáltatást. hello minták Java nyelven íródtak, és használja a hello [Azure Storage SDK for Java][Azure Storage SDK for Java]. hello tárgyalt forgatókönyvekben szerepel a **létrehozása**, **felsoroló**, és **törlése** táblák, valamint **beszúrása**,  **lekérdezése**, **módosítása**, és **törlése** entitások egy táblázatban. További tudnivalókat lásd: hello [további lépések](#Next-Steps) szakasz.

Megjegyzés: Az SDK nem elérhető a fejlesztők számára, akik Azure Storage Android-eszközökön. További információkért lásd: hello [Azure Storage SDK for Android][Azure Storage SDK for Android].

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java-alkalmazás létrehozása
Ez az útmutató a tárolási szolgáltatásokkal, amelyek helyben, vagy a webes szerepkör vagy a feldolgozói szerepkör az Azure-ban futó belül Java-alkalmazások futtatása fogja használni.

toodo tooinstall kell tehát hello Java fejlesztői készlet (JDK), és hozzon létre egy Azure storage-fiókot az Azure-előfizetéshez. Ha ezzel végzett, szüksége lesz a fejlesztői rendszerhez megfelelő hello minimális követelményeit és függőségeit, amelyek hello vannak felsorolva tooverify [Azure Storage SDK for Java] [ Azure Storage SDK for Java] GitHub tárházából. Ha a rendszer megfelel ezeknek a követelményeknek, letöltése és telepítése hello Azure Storage-könyvtárakban Java ebből a rendszeren hello utasításokat kövesse. E feladatok befejezése után fog tudni toocreate ebben a cikkben hello példák használó Java-alkalmazások.

## <a name="configure-your-application-tooaccess-table-storage"></a>Az alkalmazás tooaccess tábla tároló konfigurálása
Adja hozzá a következő importálási utasítások toohello felső részén hello Java fájl toouse Microsoft Azure storage API-k tooaccess táblák hello:

```java
// Include hello following imports toouse table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Egy Azure storage kapcsolati karakterlánc beállítása
Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatok felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat. Ha egy ügyfél-alkalmazás fut, adja meg a hello tárolási kapcsolati karakterlánc formátuma a következő, a tárfiók nevére hello segítségével hello majd hello felsorolt hello tárfiók elsődleges elérési kulcsát hello [Azure-portálon](https://portal.azure.com)a hello *AccountName* és *AccountKey* értékeket. Ez a példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot:

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Egy alkalmazás fut, a Microsoft Azure-ban a szerepkörön belüli, az ezt a karakterláncot tárolhatja hello szolgáltatás konfigurációs fájljában, *ServiceConfiguration.cscfg*, és egy hívás toohello az elérhető  **RoleEnvironment.getConfigurationSettings** metódust. Íme egy példa a hello kapcsolati karakterláncának beolvasása egy **beállítás** nevű elem *StorageConnectionString* hello szolgáltatás konfigurációs fájljában:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.

## <a name="how-to-create-a-table"></a>Útmutató: a tábla létrehozása
A **CloudTableClient** objektum lehetővé teszi, hogy a hivatkozás objektum lekérése a táblákat és entitásokat. hello alábbi kód létrehoz egy **CloudTableClient** objektumra, és használja ezt a toocreate új **CloudTable** objektum egy tábla nevű, "személyek". (Megjegyzés: vannak további lehetőségek toocreate **CloudStorageAccount** objektumok; további információkért lásd: **CloudStorageAccount** a hello [Azure Storage-ügyfél SDK-dokumentáció].)

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create hello table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-hello-tables"></a>Hogyan: hello táblák listázása
tooget listáját táblákat, hívás hello **CloudTableClient.listTables()** metódus tooretrieve egy táblanevek iterable listája.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through hello collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-an-entity-tooa-table"></a>Útmutató: az entitás tooa táblázat hozzáadása
Entitások leképezése egy egyéni osztály végrehajtási tooJava objektumok **TableEntity**. Az egyszerűség kedvéért hello **TableServiceEntity** osztály megvalósítja **TableEntity** és használ reflexiós toomap tulajdonságok toogetter és beállítására szolgáló módszerek nevű hello tulajdonságait. egy entitás tooa tábla tooadd először létre kell hoznia egy osztály, amely meghatározza az entitás hello tulajdonságait. hello alábbi kód meghatároz egy entitásosztályt, amely hello ügyfél keresztnevét használja, mint a hello sorkulcsot és Vezetéknév hello partíció kulcsként. Együtt egy entitás partíció- és sorkulcsa egyértelműen hello entitás hello táblában. Ugyanazzal a partíciókulccsal gyorsabb, mint a különböző partíciókulcsúak lekérdezhetők, hello rendelkező entitások.

```java
public class CustomerEntity extends TableServiceEntity {
    public CustomerEntity(String lastName, String firstName) {
        this.partitionKey = lastName;
        this.rowKey = firstName;
    }

    public CustomerEntity() { }

    String email;
    String phoneNumber;

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```

Entitások érintő tábla működéséhez szükségesek egy **TableOperation** objektum. Ez az objektum meghatározása hello művelet toobe entitás, amely kell végrehajtani, végre egy **CloudTable** objektum. hello alábbi kód létrehoz egy új példányát hello **CustomerEntity** néhány tárolt felhasználói adatok toobe osztályra. kód következő hívások hello **TableOperation.insertOrReplace** toocreate egy **TableOperation** tooinsert entitás objektum egy táblába, és új hello társult **CustomerEntity**vele. Végezetül hello kódja meghívja hello **hajtható végre** hello metódusa **CloudTable** objektum hello "személyek" táblázat és az új hello **TableOperation**, mely majd küld egy kérelem toohello tárolási szolgáltatás tooinsert hello új ügyfélentitást hello "személyek" táblába, vagy cserélje le a hello entitás, ha már létezik.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation tooadd hello new customer toohello people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit hello operation toohello table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-a-batch-of-entities"></a>Útmutató: egy teljes entitásköteget beszúrása
Egyetlen írási művelettel egy tranzakcióköteghez entitások toohello table szolgáltatás is beszúrhat. hello alábbi kód létrehoz egy **TableBatchOperation** objektumot, majd hozzáadja a három beszúrási műveletek tooit. Minden egyes végrehajtott beszúrási művelet hozzáadott új forrásentitás-objektum létrehozása, annak értékét, és majd hívja a hello **beszúrása** hello metódusa **TableBatchOperation** tooassociate hello entitás az új objektum Helyezze be a műveletet. Ezután a kód hívások hello **hajtható végre** hello a **CloudTable** objektum hello "személyek" tábla és egyéb hello **TableBatchOperation** objektum, amely küldi hello kötegelt tábla műveletek toohello storage szolgáltatást a egyetlen kérelem.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity tooadd toohello table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity tooadd toohello table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity tooadd toohello table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute hello batch of operations on hello "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

Néhány dolgot toonote a kötegműveletekkel kapcsolatban:

* Too100 insert, törlés, egyesítési, csere, Beszúrás vagy egyesítés, hajtsa végre, és beszúrása vagy csereműveletek bármilyen kombinációban egyetlen kötegben.
* A kötegelt művelet lehet a beolvasási műveletet hello hello köteg egyetlen művelete esetén.
* Hello rendelkeznie kell egy kötegművelet összes entitásának ugyanazzal a partíciókulccsal.
* A kötegelt művelet korlátozott tooa 4MB adattartalom.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Útmutató: egy partíció összes entitásának lekérése
a partíció entitástartományának tábla tooquery, használhatja a **TableQuery**. Hívás **TableQuery.from** toocreate egy adott táblán, amely visszaadja a megadott típusú lekérdezést. hello alábbira megad egy szűrőt entitások hello partíciós kulcs "Smith" esetén. **TableQuery.generateFilterCondition** van egy segédmetódust a lekérdezések toocreate szűrők. Hívás **ahol** hello által visszaadott hello hivatkozással **TableQuery.from** tooapply hello szűrő toohello lekérdezés. Ha hello lekérdezést végrehajtja a rendszer a következőt hívja túl**hajtható végre** a hello **CloudTable** objektumot ad vissza egy **iterátor** a hello **CustomerEntity**eredménye a megadott típust. Hello segítségével **iterátor** vissza egy minden hurok tooconsume hello eredmények. Ez a kód kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where hello partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as hello partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through hello results, displaying information about hello entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Útmutató: egy partíció entitástartományának lekérése
Ha tooquery egy partíció összes hello entitások nem szeretné, megadhat egy tartományt a szűrőben lévő összehasonlító operátorok használatával. a következő két szűrők tooget "Smith" partíció összes entitásának ahol hello sorkulcs (Keresztnév) mentése too'E betűvel kezdődik kód kombinálja hello "hello ábécé. Majd jelenít hello lekérdezés eredményeit. Ha használ hello entitások hozzáadott toohello tábla hello kötegben című szakaszában talál, csak két olyan entitásra visszaadott (Ben és Denise Smith); Jeff Smith nincs megadva.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where hello partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where hello row key is less than hello letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine hello two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as hello partition key,
    // with hello row key being up toohello letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through hello results, displaying information about hello entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-single-entity"></a>Hogyan: egyetlen entitás lekérdezése
A lekérdezés tooretrieve írhat egy adott entitás. hello következő kódot a hívások **TableOperation.retrieve** ügyféllel partíciós kulcs és a sor paramétereket toospecify hello "Jeff Smith" létrehozása helyett egy **TableQuery** és szűrőket toodo hello használata ugyanaz. Végrehajtásakor hello beolvasni egy gyűjtemény helyett művelet értéket ad vissza csak egyetlen entitást. Hello **getResultAsType** metódus árnyékot toohello eredménytípus hello hello hozzárendelés TARGET, egy **CustomerEntity** objektum. Ha ez a típus nem kompatibilis a hello a lekérdezéshez megadott hello típus, egy kivételt fog jelezni. Null értékű ad vissza, ha nem entitás egy pontos partíció- és sorkulcsa felel meg. Hello leggyorsabb módon tooretrieve hello Table szolgáltatásból egy entitás partíció-és sorkulcsok megadása a lekérdezésben.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit hello operation toohello table service and get hello specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output hello entity.
    if (specificEntity != null)
    {
        System.out.println(specificEntity.getPartitionKey() +
            " " + specificEntity.getRowKey() +
            "\t" + specificEntity.getEmail() +
            "\t" + specificEntity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-modify-an-entity"></a>Hogyan: entitás módosítása
egy entitás toomodify lekéréséhez hello table szolgáltatásból, ne módosítások toohello forrásentitás-objektum, és egy áthelyezési vagy egyesítési művelet hátsó toohello table szolgáltatás hello módosítások mentése. hello alábbira vált egy meglévő ügyfél telefonszámát. Telefonhívás helyett **TableOperation.insert** tooinsert megszokott meghívja-e ezt a kódot **TableOperation.replace**. Hello **CloudTable.execute** metódushívások hello table szolgáltatás, és hello entitás helyébe, kivéve, ha egy másik alkalmazás módosított hello időben mivel ez az alkalmazás azt lekérése. Ebben az esetben kivételt vált ki, és hello entitás kell beolvasni, módosíthatók, és újra menti. Egyidejű hozzáférések optimista újrapróbálkozási zárolás közös az olyan elosztott tárolórendszer.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit hello operation toohello table service and get hello specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation tooreplace hello entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit hello operation toohello table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-query-a-subset-of-entity-properties"></a>Útmutató: az Entitástulajdonságok egy részének lekérdezése
A lekérdezéstábla tooa pár tulajdonságok kérhetnek le egy entitás. Ez a leképezésnek hívott technika csökkenti a sávszélesség felhasználását, és javítja a lekérdezési teljesítményt, főleg a nagy entitások esetében. hello hello kód a következő lekérdezést használ hello **válasszon** metódus tooreturn csak hello e-mail címek entitások hello táblában. hello eredmények vannak képezik le gyűjteménye **karakterlánc** hello segítségével egy **EntityResolver**, amely does hello hello entitások hello kiszolgáló által visszaadott típus átalakítást. A leképezési kapcsolatos részletesebb [Azure táblák: Introducing Upsert és Query Projection][Azure Tables: Introducing Upsert and Query Projection]. Vegye figyelembe, hogy nem támogatja a hello helyi storage emulatort, így a kód csak akkor, ha a hello table szolgáltatás egy olyan fiók használatával.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only hello Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define a Entity resolver tooproject hello entity toohello Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through hello results, displaying hello Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-or-replace-an-entity"></a>Hogyan: beszúrása vagy entitás cseréje
Gyakran érdemes tooadd egy entitás tooa tábla anélkül, hogy tudnák, ha hello tábla már létezik. Egy beszúrása vagy lecserélése művelet lehetővé teszi egy egyetlen kérelmet, amely szúrnak hello entitás, ha nem létezik, és cserélje ki egy meglévő, ellenkező esetben hello toomake. Előzetes példák kialakításának, hello következő kód beszúrása vagy "Walter grönlandi" hello entitás helyébe lép. Új entitás létrehozása, után ez a kód meghívja a hello **TableOperation.insertOrReplace** metódust. Ez a kód majd meghívja a **hajtható végre** a hello **CloudTable** hello tábla és hello insert rendelkező objektum, vagy a tábla a csere hello paraméterekként. csak egy része az entitás tooupdate hello **TableOperation.insertOrMerge** módszer is lehet használni. Vegye figyelembe, hogy beszúrása vagy lecserélése nem támogatott a hello helyi storage emulatort, így a kód csak akkor, ha a hello table szolgáltatás egy olyan fiók használatával. További információ a beszúrása vagy lecserélése és beszúrási vagy-egyesítéses ezen tudhat [Azure táblák: Introducing Upsert és Query Projection][Azure Tables: Introducing Upsert and Query Projection].

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation tooadd hello new customer toohello people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit hello operation toohello table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-an-entity"></a>Hogyan: entitás törlése
Egy entitás azt beolvasása után egyszerűen törölheti. Miután hello entitás beolvasott, hívja **TableOperation.delete** hello entitás toodelete együtt. Majd meghívják a **hajtható végre** a hello **CloudTable** objektum. a következő kód hello kéri le, és töröl egy ügyfélentitást.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation toodelete hello entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit hello delete operation toohello table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-table"></a>Hogyan: tábla törlése
Végezetül hello alábbira törölhető egy tábla a tárfiókból. Egy tábla már törölt újra létrehozza a következő hello törlésre, általában kevesebb, mint körülbelül 40 másodpercig egy ideig nem érhető el toobe lesz.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete hello table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```
[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a>Következő lépések

* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.
* [Az Azure Storage Java SDK][Azure Storage SDK for Java]
* [Az Azure Storage ügyfél SDK-dokumentáció][Azure Storage-ügyfél SDK-dokumentáció]
* [Az Azure Storage REST API-n][Azure Storage REST API]
* [Az Azure Storage csapat blogja][Azure Storage Team Blog]

További információkért lásd még: hello [Java fejlesztői központ](/develop/java/).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage-ügyfél SDK-dokumentáció]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
