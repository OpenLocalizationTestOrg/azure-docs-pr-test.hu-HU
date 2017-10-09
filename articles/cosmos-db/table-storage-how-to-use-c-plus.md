---
title: a Table storage (C++) aaaHow toouse |} Microsoft Docs
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jahogg
editor: tysonn
ms.assetid: f191f308-e4b2-4de9-85cb-551b82b1ea7c
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: mimig
ms.openlocfilehash: 8096fe531427ba4858f7f4cb7f664f941892d1c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-c"></a>Hogyan toouse Table storage-ának C++
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooperform gyakori helyzetek használatával hello Azure Table storage szolgáltatást. hello minták C++ nyelven íródtak, és használja a hello [Azure Storage ügyféloldali kódtára a C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). hello tárgyalt forgatókönyvekben szerepel a **létrehozása és egy tábla törlése** és **táblaentitásokat használata**.

> [!NOTE]
> Ez az útmutató célok hello a Azure Storage ügyféloldali kódtára a C++ 1.0.0 verzió vagy újabb. hello ajánlott verzió a Storage ügyféloldali kódtára 2.2.0, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>A C++-alkalmazás létrehozása
Ez az útmutató egy C++ alkalmazáson belül futó tárolási szolgáltatásokkal fogja használni. toodo tooinstall kell tehát hello Azure Storage ügyféloldali kódtára a C++ és az Azure storage-fiók létrehozása az Azure-előfizetése.  

tooinstall hello Azure Storage ügyféloldali kódtára a C++, a következő módszerek hello használhatják:

* **Linux:** hello adott hello utasítások [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.  
* **Windows:** a Visual Studióban kattintson **eszközök > NuGet-Csomagkezelő > Csomagkezelő konzol**. Típus hello következő parancsot a hello történő [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER billentyűt.  
  
     Install-Package wastorage

## <a name="configure-your-application-tooaccess-table-storage"></a>Az alkalmazás tooaccess a Table storage konfigurálása
Adja hozzá, hello következő tartalmazó utasítások toohello felső részén hello C++ fájl toouse hello az Azure storage API-k tooaccess táblák:  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Egy Azure storage kapcsolati karakterlánc beállítása
Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatok felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat. Ha egy ügyfél-alkalmazás fut, meg kell adnia hello tárolási kapcsolati karakterlánc formátuma a következő hello. Használjon hello a tárolási fiók és hello tárelérési kulcs hello tárfiók neve a hello [Azure Portal](https://portal.azure.com) a hello *AccountName* és *AccountKey* értékek. A storage-fiókok és a hívóbetűk információkért lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md). Ez a példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot:  

```cpp
// Define hello connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

tootest az alkalmazás a helyi Windows-alapú számítógépeken, használhatja a hello Azure [storage emulator](../storage/common/storage-use-emulator.md) hello a telepített [Azure SDK](https://azure.microsoft.com/downloads/). hello storage emulator egy segédprogram, amely szimulálja hello Azure Blob, Queue és Table szolgáltatások a helyi fejlesztési számítógépen. hello következő példa bemutatja, hogyan deklarálhatnak egy statikus mező toohold hello kapcsolati karakterlánc tooyour helyi storage emulatort:  

```cpp
// Define hello connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

toostart hello Azure storage emulatort, kattintson a hello **Start** gombra vagy nyomja le az hello Windows kulcsot. Írja be a szöveget **Azure Storage Emulator**, majd válassza ki **Microsoft Azure Storage Emulator** hello alkalmazások listája.  

hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.  

## <a name="retrieve-your-connection-string"></a>A kapcsolat-karakterlánc beolvasása
Használhatja a hello **cloud_storage_account** osztály toorepresent a tárfiók adatait. tooretrieve a tárolási fiók hello tárolási kapcsolati karakterlánc adatait, hello parse metódus használatával kérheti le.

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

A következő lekérni egy hivatkozás tooa **cloud_table_client** osztályt, lehetővé teszi, hogy a hivatkozás objektumok lekérni a táblák és entitásokat a Table storage szolgáltatás tárolja hello. hello alábbi kód létrehoz egy **cloud_table_client** objektum hello tárolási fiók objektum azt lekérése fent használatával:  

```cpp
// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a>Tábla létrehozása
A **cloud_table_client** objektum lehetővé teszi, hogy a hivatkozás objektum lekérése a táblákat és entitásokat. hello alábbi kód létrehoz egy **cloud_table_client** objektumra, és használja ezt a toocreate új tábla.

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference tooa table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table if it doesn't exist.
table.create_if_not_exists();  
```

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása
egy entitás tooa tábla tooadd hozzon létre egy új **table_entity** objektumot, és adja át túl**table_operation::insert_entity**. hello alábbira használja hello ügyfél keresztnevét hello sorkulcsnak és a Vezetéknév hello partíció kulcsként. Együtt egy entitás partíció- és sorkulcsa egyértelműen hello entitás hello táblában. Ugyanazzal a partíciókulccsal gyorsabb, mint a különböző lekérdezhetők, hello rendelkező entitások partícióazonosító kulcsok, de az eltérő partíciókulcsok használata lehetővé teszi a párhuzamos művelet nagyobb méretezhetőségét. További információkért lásd: [Microsoft Azure storage teljesítményére és méretezhetőségére ellenőrzőlista](../storage/common/storage-performance-checklist.md).

hello alábbi kód létrehoz egy új példányát **table_entity** az egyes tárolt felhasználói adatok toobe. kód következő hívások hello **table_operation::insert_entity** toocreate egy **table_operation** tooinsert entitás objektum egy táblába, és társult hello új táblaentitássá vele. Végezetül hello kód hívások hello metódus végrehajtása a hello **cloud_table** objektum. És új hello **table_operation** egy kérelem toohello Table szolgáltatás tooinsert hello új ügyfélentitást küldi hello "személyek" táblába.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference tooa table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table if it doesn't exist.
table.create_if_not_exists();

// Create a new customer entity.
azure::storage::table_entity customer1(U("Harp"), U("Walter"));

azure::storage::table_entity::properties_type& properties = customer1.properties();
properties.reserve(2);
properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

// Create hello table operation that inserts hello customer entity.
azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

// Execute hello insert operation.
azure::storage::table_result insert_result = table.execute(insert_operation);
```

## <a name="insert-a-batch-of-entities"></a>Entitásköteg beszúrása
Egyetlen írási művelettel egy tranzakcióköteghez entitások toohello Table szolgáltatás is beszúrhat. hello alábbi kód létrehoz egy **table_batch_operation** objektumot, és hozzáadja a három beszúrási műveletek tooit. Minden egyes végrehajtott beszúrási művelet bekerül hozzon létre egy új entitásobjektumot, állítsa be az értékeket, és majd hívja a hello beszúrása hello metódusa **table_batch_operation** objektum tooassociate hello entitást egy új beszúrási művelet esetén. Ezt követően **cloud_table.execute** tooexecute hello művelet neve.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define a batch operation.
azure::storage::table_batch_operation batch_operation;

// Create a customer entity and add it toohello table.
azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

azure::storage::table_entity::properties_type& properties1 = customer1.properties();
properties1.reserve(2);
properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

// Create another customer entity and add it toohello table.
azure::storage::table_entity customer2(U("Smith"), U("Ben"));

azure::storage::table_entity::properties_type& properties2 = customer2.properties();
properties2.reserve(2);
properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

// Create a third customer entity tooadd toohello table.
azure::storage::table_entity customer3(U("Smith"), U("Denise"));

azure::storage::table_entity::properties_type& properties3 = customer3.properties();
properties3.reserve(2);
properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

// Add customer entities toohello batch insert operation.
batch_operation.insert_or_replace_entity(customer1);
batch_operation.insert_or_replace_entity(customer2);
batch_operation.insert_or_replace_entity(customer3);

// Execute hello batch operation.
std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);
```

Néhány dolgot toonote a kötegműveletekkel kapcsolatban:  

* Too100 insert, törlés, egyesítési, csere, bármilyen kombinációban egyetlen kötegben insert vagy egyesítés és beszúrása vagy lecserélése műveletek elvégzése.  
* A kötegelt művelet lehet a beolvasási műveletet hello hello köteg egyetlen művelete esetén.  
* Hello rendelkeznie kell egy kötegművelet összes entitásának ugyanazzal a partíciókulccsal.  
* A kötegelt művelet korlátozott tooa 4 MB-os adattartalom.  

## <a name="retrieve-all-entities-in-a-partition"></a>Egy partíció összes entitásának lekérése
egy tábla használja egy partíció összes entitását tooquery egy **table_query** objektum. hello alábbi példakód megad egy szűrőt, ahol a "Smith" hello partíciókulcs az entitások. Ez a példa kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Construct hello query operation for all customer entities where PartitionKey="Smith".
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Print hello fields for each customer.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

Ebben a példában hello lekérdezés során az hello szűrési feltételeknek megfelelő összes hello entitások. Ha nagy táblák és toodownload hello táblaentitásokat gyakran kell, azt javasoljuk, hogy tárolja az adatokat az Azure storage blobs helyette.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Partíció entitástartományának lekérése
Ha tooquery egy partíció összes hello entitások nem szeretné, megadhat egy tartományt hello partíció kulcs szűrő egy sor szűrőjének kombinálásával. hello alábbi példakód két szűrő tooget összes entitásának a 'Smith', ahol hello sorkulcs (Keresztnév) rendszernél korábbi hello ábécé "E" betűvel kezdődik, és majd kinyomtatja hello lekérdezési eredmények partíció.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table query.
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
    azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
    azure::storage::query_comparison_operator::equal, U("Smith")),
    azure::storage::query_logical_operator::op_and,
    azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Loop through hello results, displaying information about hello entity.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

## <a name="retrieve-a-single-entity"></a>Egyetlen entitás lekérdezése
A lekérdezés tooretrieve írhat egy adott entitás. hello következő kódban **table_operation::retrieve_entity** toospecify hello ügyfél "Jeff Smith". Ez a módszer adja vissza egy gyűjtemény helyett csak egyetlen entitást, és hello visszaadott érték **table_result**. Hello leggyorsabb módon tooretrieve hello Table szolgáltatásból egy entitás partíció-és sorkulcsok megadása a lekérdezésben.  

```cpp
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Output hello entity.
azure::storage::table_entity entity = retrieve_result.entity();
const azure::storage::table_entity::properties_type& properties = entity.properties();

std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
    << U(", Property1: ") << properties.at(U("Email")).string_value()
    << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
```

## <a name="replace-an-entity"></a>Entitás cseréje
tooreplace egy entitás lekéréséhez hello Table szolgáltatásból, módosítsa a hello forrásentitás-objektum, és majd hello módosítások mentése toohello Table szolgáltatás biztonsági. hello alábbira vált egy meglévő ügyfél telefonszámát és az e-mail címét. Telefonhívás helyett **table_operation::insert_entity**, a kódban **table_operation::replace_entity**. Ennek hatására hello entitás toobe hello kiszolgálón teljesen lecseréli, kivéve, ha a lekérdezés, amely esetben hello sikertelen lesz hello entitás hello kiszolgálón megváltozott. Ez a hiba az alkalmazás ne írhasson felül véletlenül olyan módosítás történt közötti hello lekérését, és frissítse az alkalmazás egy másik összetevő tooprevent. hello Ez a hiba megfelelő kezeléséhez tooretrieve hello entitás újra, hajtsa végre a módosításokat (Ha még érvényesek), és ezután hajtson végre egy újabb **table_operation::replace_entity** műveletet. hello a következő szakasz bemutatja, hogyan toooverride ezt a viselkedést.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Replace an entity.
azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
properties_to_replace.reserve(2);

// Specify a new phone number.
properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

// Specify a new email address.
properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

// Create an operation tooreplace hello entity.
azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

// Submit hello operation toohello Table service.
azure::storage::table_result replace_result = table.execute(replace_operation);
```

## <a name="insert-or-replace-an-entity"></a>Entitás beszúrása vagy lecserélése
**table_operation::replace_entity** műveletek sikertelenek lesznek, ha hello entitás hello kiszolgálóról lekérdezés óta módosult. Ezenkívül be kell olvasni hello entitás ahhoz, hogy először a hello kiszolgálóról **table_operation::replace_entity** toobe sikeres. Egyes esetekben azonban nem tudja Ha hello entitás létezik-e hello kiszolgálón, és hello a benne tárolt aktuális értékek irrelevánsak – a frissítés mindent felülír. tooaccomplish, használhatja a **table_operation::insert_or_replace_entity** műveletet. Ez a művelet beszúrja hello entitás, ha nem létezik, vagy ha igen, mikor hello utolsó frissítés történt függetlenül lecseréli. Az alábbi kódpéldát hello, hello ügyfélentitást Jeff Smith program továbbra is, de azok majd mentésekor hátsó toohello kiszolgálón keresztül **table_operation::insert_or_replace_entity**. Toohello entitás hello lekérési és a frissítési művelet között történt összes módosítást felül lesznek írva.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Insert-or-replace an entity.
azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

properties_to_insert_or_replace.reserve(2);

// Specify a phone number.
properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

// Specify an email address.
properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

// Create an operation tooinsert-or-replace hello entity.
azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

// Submit hello operation toohello Table service.
azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);
```

## <a name="query-a-subset-of-entity-properties"></a>Az entitástulajdonságok egy részének lekérdezése
A lekérdezéstábla tooa pár tulajdonságok kérhetnek le egy entitás. hello hello kód a következő lekérdezést használ hello **table_query::set_select_columns** metódus tooreturn csak hello e-mail címek entitások hello táblában.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define hello query, and select only hello Email property.
azure::storage::table_query query;
std::vector<utility::string_t> columns;

columns.push_back(U("Email"));
query.set_select_columns(columns);

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Display hello results.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

    const azure::storage::table_entity::properties_type& properties = it->properties();
    for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
    {
        std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
    }

    std::wcout << std::endl;
}
```

> [!NOTE]
> Egy entitás bizonyos tulajdonságait lekérdezése hatékonyabb működését, mint az összes tulajdonság beolvasása.
> 
> 

## <a name="delete-an-entity"></a>Entitás törlése
Egy entitás azt beolvasása után egyszerűen törölheti. Miután hello entitás beolvasott, hívja **table_operation::delete_entity** hello entitás toodelete együtt. Majd meghívják a hello **cloud_table.execute** metódust. hello alábbi kód lekérdez, majd törli az entitást a partíciós kulcs "Smith" és "Jeff" sor kulcs.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation toodelete hello entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit hello delete operation toohello Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);  
```

## <a name="delete-a-table"></a>Tábla törlése
Végezetül alábbi kódpéldát hello törölhető egy tábla a tárfiókból. A törölt tábla nem érhető el toobe egy adott időn belül hello törlését követően újra létre lesz.  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation toodelete hello entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit hello delete operation toohello Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a table storage alapjait hello, kövesse az alábbi hivatkozások toolearn Azure Storage-ról további:  

* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.
* [Hogyan toouse Blob storage-ának C++](../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Hogyan toouse C++ várólista-tárhellyel](../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [A c++ Azure Storage-erőforrások felsorolása](../storage/common/storage-c-plus-plus-enumeration.md)
* [A Storage ügyféloldali kódtára a c++ nyelvhez – dokumentáció](http://azure.github.io/azure-storage-cpp)
* [Az Azure Storage-dokumentáció](https://azure.microsoft.com/documentation/services/storage/)
