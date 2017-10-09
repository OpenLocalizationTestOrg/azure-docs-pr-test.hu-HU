---
title: "aaaHow toouse a table storage php-ből |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello PHP toocreate a Table szolgáltatás, és egy tábla, és a Beszúrás, a delete és a lekérdezéstábla hello törlése."
services: storage
documentationcenter: php
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 1e1036118e208280b4c205da7d7eea61e79359c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-php"></a>Hogyan toouse table storage php-ből
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Table szolgáltatás. hello minták PHP nyelven íródtak, és használja a hello [Azure SDK for PHP][download]. hello tárgyalt forgatókönyvekben szerepel a **létrehozása és egy tábla törlésével és beszúrását, törlését és entitások egy tábla lekérdezése**. Hello Azure Table szolgáltatás további információkért lásd: hello [további lépések](#next-steps) szakasz.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása
egyetlen hello Azure Table szolgáltatás hozzáférő PHP-alkalmazások létrehozására vonatkozó követelmény az hello hello való hivatkozás hello Azure SDK-t az osztályokat a php a kód. A fejlesztői eszközök toocreate használhatja az alkalmazás, például a Jegyzettömbbel.

Ebben az útmutatóban hívható a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó tábla szolgáltatás funkciók használatára.

## <a name="get-hello-azure-client-libraries"></a>Hello Azure Ügyfélkódtárai beolvasása
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-table-service"></a>Az alkalmazás tooaccess hello Table szolgáltatás konfigurálása
toouse hello Azure Table szolgáltatás API-k, kell:

1. Hello robotjába referenciafájlban hello segítségével [require_once] [ require_once] utasítást, és
2. Bármely osztályok segítségével lehet hivatkozni.

hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello **ServicesBuilder** osztály.

> [!NOTE]
> Ebben a cikkben a hello példák feltételezik, hogy telepítette hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával. Ha manuálisan telepítette a hello szalagtárak, meg kell-e tooreference hello <code>WindowsAzure.php</code> robotjába fájlt.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Hello a következő példa a hello `require_once` utasítás mindig megjelenik, de csak hello osztályok hello példa tooexecute szükséges hivatkozott.

## <a name="set-up-an-azure-storage-connection"></a>Az Azure storage-kapcsolat beállítása
az Azure Table szolgáltatásügyfél tooinstantiate, először rendelkeznie kell érvényes kapcsolati karakterláncot. hello hello tábla szolgáltatás kapcsolati karakterláncának formátuma:

Élő szolgáltatások elérésére:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Hello emulátor tárolóinak eléréséhez:

```php
UseDevelopmentStorage=true
```

toocreate bármely Azure szolgáltatás ügyfele toouse hello kell **ServicesBuilder** osztály. A következőket teheti:

* átadni hello kapcsolati karakterlánc közvetlenül tooit vagy
* hello használata **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:
  * Alapértelmezés szerint mellékelt egy külső adatforrás - környezeti változók támogatása
  * hozzáadhat új források hello kiterjesztésével **ConnectionStringSource** osztály

Itt vázolt hello példákért hello kapcsolati karakterlánc közvetlenül fog adható át.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a>Tábla létrehozása
A **TableRestProxy** objektum lehetővé teszi, hogy hozzon létre egy táblát hello **createTable** metódust. Egy tábla létrehozásakor beállíthatja hello tábla szolgáltatás időtúllépés. (Hello tábla szolgáltatás időtúllépési kapcsolatos további információkért lásd: [időtúllépések beállítása a szolgáltatási műveletek tábla][table-service-timeouts].)

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Create table.
    $tableRestProxy->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
}
```

Táblanevek vonatkozó megkötésekkel kapcsolatos információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell][table-data-model].

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása
egy entitás tooa tábla tooadd hozzon létre egy új **entitás** objektumot, és adja át túl**TableRestProxy -> insertEntity**. Vegye figyelembe, hogy egy entitás létrehozásakor meg kell adnia egy `PartitionKey` és `RowKey`. Ezek hello egy entitás egyedi azonosítót, és olyan értékek, amelyek sokkal gyorsabb, mint más entitás tulajdonságai kérdezhetők le. hello rendszer `PartitionKey` tooautomatically hello tábla entitások terjesztése számos tárolási csomópont feladatait. Az entitások hello azonos `PartitionKey` hello tárolt ugyanahhoz a csomóponthoz. (Több entitás hello ugyanahhoz a csomóponthoz hajtsa végre a tárolt műveletek jobb, mint a más csomópontjai között tárolt entitásokat.) Hello `RowKey` hello egy partíció egy entitás egyedi azonosítója.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out hello trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableRestProxy->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

További információ a táblázat tulajdonságai és típusok: [ismertetése hello tábla szolgáltatás adatmodell][table-data-model].

Hello **TableRestProxy** osztály entitások beszúrására szolgáló két alternatív módszert kínál: **insertOrMergeEntity** és **insertOrReplaceEntity**. toouse módszerekhez, hozzon létre egy új **entitás** , és adja át paraméter tooeither módszerként. Az egyes módszerek szúrnak hello entitás, ha nem létezik. Ha már létezik hello entitás, **insertOrMergeEntity** tulajdonságértékek frissíti, ha hello tulajdonságai már léteznek, és hozzáadja az új tulajdonságok ha azok nem léteznek, miközben **insertOrReplaceEntity** teljesen lecseréli a meglévő entitás. a következő példa azt mutatja meg hogyan hello toouse **insertOrMergeEntity**. Ha az entitás hello `PartitionKey` "tasksSeattle" és `RowKey` "1" már nem létezik, szúrja be. Azonban ha azt korábban beszúrt (a fenti példa hello), hello `DueDate` tulajdonság frissülni fog, és hello `Status` tulajdonság megkapja. Hello `Description` és `Location` tulajdonságok is frissülnek, de értékekkel, amely hatékonyan hagyja őket változatlan marad. Ha ez utóbbi a tulajdonság nem hozzáadott hello példában látható módon, de hello célentitás létezett, meglévő értékeik változatlan marad.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

//Create new entity.
$entity = new Entity();

// PartitionKey and RowKey are required.
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");

// If entity exists, existing properties are updated with new values and
// new properties are added. Missing properties are unchanged.
$entity->addProperty("Description", null, "Take out hello trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified hello DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace hello entity with PartitionKey "tasksSeattle" and RowKey "1".
    $tableRestProxy->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a>Egyetlen entitás lekérdezése
Hello **TableRestProxy -> getEntity** módszer lehetővé teszi a tooretrieve által végzett lekérdezés egyetlen entitás az `PartitionKey` és `RowKey`. Hello az alábbi példában a partíciós kulcs hello `tasksSeattle` és sorkulcsa `1` toohello átadott **getEntity** metódust.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entity = $result->getEntity();

echo $entity->getPartitionKey().":".$entity->getRowKey();
```

## <a name="retrieve-all-entities-in-a-partition"></a>Egy partíció összes entitásának lekérése
Entitás lekérdezések össze szűrők használata (további információkért lásd: [lekérdezése táblákat és entitásokat][filters]). tooretrieve partíció összes entitásának hello szűrő használata "PartitionKey eq *partíció_neve*". a következő példa azt mutatja meg hogyan hello tooretrieve hello összes entitásának `tasksSeattle` úgy, hogy a szűrő toohello partíció **queryEntities** metódust.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Partíció entitástartományának részhalmazát
hello előző példában használt ugyanilyen mintájú hello lehet használt tooretrieve partíció entitástartományának csoportját. visszaállíthatja az entitások hello részhalmazát hello szűrő használata határozza meg (további információkért lásd: [lekérdezése táblákat és entitásokat][filters]) a következő példa azt mutatja meg hogyan .hello toouse egy szűrő tooretrieve egy adott rendelkező összes entitás `Location` és egy `DueDate` kisebb, mint a megadott dátum.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entity-properties"></a>Entitás tulajdonságai részhalmazát
A lekérdezés Entitástulajdonságok egy részének kérheti le. Ezzel a módszerrel nevű *leképezése*, csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat. toospecify egy tulajdonság toobe beolvasni, hello tulajdonság toohello hello nevét át **-> addSelectField** metódust. Ez a metódus többször tooadd további tulajdonságok is meghívható. Végrehajtása után **TableRestProxy -> queryEntities**, hello visszaadott entitások csak van kiválasztva hello tulajdonságok. (Ha azt szeretné, hogy tooreturn táblaentitásokat részhalmazát, használjon szűrőt hello lekérdezések a fenti ábrán.)

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableRestProxy->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

// All entities in hello table are returned, regardless of whether
// they have hello Description field.
// toolimit hello results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a>Egy entitás frissítése
Meglévő entitás hello használatával frissíthető **entitás -> setProperty** és **entitás -> addProperty** hello entitáshoz, és ezután hívása metódusai **TableRestProxy updateEntity ->** . hello alábbi példa egy entitás lekéri, módosítja egy tulajdonságot, egy másik tulajdonságának eltávolítja és új tulajdonsággal. Vegye figyelembe, hogy egy tulajdonság értéke túl beállításával eltávolíthatja**null**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();

$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

$entity->setPropertyValue("Location", null); //Removed Location.

$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableRestProxy->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a>Entitás törlése
egy entitás toodelete átadni hello táblanév és hello entitás `PartitionKey` és `RowKey` toohello **TableRestProxy -> deleteEntity** metódust.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete entity.
    $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Vegye figyelembe, hogy párhuzamossági ellenőrzések állíthatók be hello Etag egy entitás toobe hello használatával törli a **DeleteEntityOptions -> setEtag** metódus és sikeres hello **DeleteEntityOptions** objektum túl**deleteEntity** negyedik paraméterként.

## <a name="batch-table-operations"></a>Kötegelt tábla műveletek
Hello **TableRestProxy -> kötegelt** metódus tooexecute lehetővé teszi több művelet egyetlen kérelemben. hello mintát itt magában foglalja a műveletek túl hozzáadása**BatchRequest** objektumot, és ezután átadja az hello **BatchRequest** toohello objektum **TableRestProxy -> kötegelt** metódust. egy művelet tooa tooadd **BatchRequest** objektum hívása a következő módszerek többször hello bármelyikét:

* **addInsertEntity** (hozzáad egy insertEntity művelet)
* **addUpdateEntity** (hozzáad egy updateEntity művelet)
* **addMergeEntity** (hozzáad egy mergeEntity művelet)
* **addInsertOrReplaceEntity** (hozzáad egy insertOrReplaceEntity művelet)
* **addInsertOrMergeEntity** (hozzáad egy insertOrMergeEntity művelet)
* **addDeleteEntity** (hozzáad egy deleteEntity művelet)

a következő példa azt mutatja meg hogyan hello tooexecute **insertEntity** és **deleteEntity** egyetlen kérelem műveletek:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

// Create list of batch operation.
$operations = new BatchOperations();

$entity1 = new Entity();
$entity1->setPartitionKey("tasksSeattle");
$entity1->setRowKey("2");
$entity1->addProperty("Description", null, "Clean roof gutters.");
$entity1->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity1->addProperty("Location", EdmType::STRING, "Home");

// Add operation toolist of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation toolist of batch operations.
$operations->addDeleteEntity("mytable", "tasksSeattle", "1");

try    {
    $tableRestProxy->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Tábla műveletek kötegelése kapcsolatos további információkért lásd: [entitás csoport tranzakciók végrehajtása][entity-group-transactions].

## <a name="delete-a-table"></a>Tábla törlése
Végül adja át a toodelete egy tábla hello tábla neve toohello **TableRestProxy -> deleteTable** metódust.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete table.
    $tableRestProxy->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte hello Azure Table szolgáltatás hello alapjait, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.

* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.

* [A PHP fejlesztői központ](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
