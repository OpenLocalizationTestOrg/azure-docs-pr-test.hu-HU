---
title: "Php-ből a table storage használata |} Microsoft Docs"
description: "Használata php-ből a Table szolgáltatás létrehozásához, és törölni kívánja a táblázatot, és helyezze, törlése, és a tábla lekérdezése."
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 7a48446a11c5c6db0c9f4fdd8872b1e3c12e85c3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-table-storage-from-php"></a><span data-ttu-id="e0624-103">Php-ből a table storage használata</span><span class="sxs-lookup"><span data-stu-id="e0624-103">How to use table storage from PHP</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="e0624-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e0624-104">Overview</span></span>
<span data-ttu-id="e0624-105">Ez az útmutató bemutatja, hogyan hajthat végre a gyakori forgatókönyvek az Azure Table szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="e0624-105">This guide shows you how to perform common scenarios using the Azure Table service.</span></span> <span data-ttu-id="e0624-106">A mintákat a PHP és -felhasználási nyelven íródtak a [Azure SDK for PHP][download].</span><span class="sxs-lookup"><span data-stu-id="e0624-106">The samples are written in PHP and use the [Azure SDK for PHP][download].</span></span> <span data-ttu-id="e0624-107">Az ismertetett forgatókönyvek **létrehozása és egy tábla törlésével és beszúrását, törlését és entitások egy tábla lekérdezése**.</span><span class="sxs-lookup"><span data-stu-id="e0624-107">The scenarios covered include **creating and deleting a table, and inserting, deleting, and querying entities in a table**.</span></span> <span data-ttu-id="e0624-108">Az Azure Table szolgáltatás további információkért tekintse meg a [további lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="e0624-108">For more information on the Azure Table service, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="e0624-109">PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0624-109">Create a PHP application</span></span>
<span data-ttu-id="e0624-110">A csak az Azure Table szolgáltatás hozzáférő PHP-alkalmazások létrehozására vonatkozó mérete a hivatkozási osztályok az Azure SDK-ban a php a kód.</span><span class="sxs-lookup"><span data-stu-id="e0624-110">The only requirement for creating a PHP application that accesses the Azure Table service is the referencing of classes in the Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="e0624-111">A fejlesztői eszközök hozhat létre az alkalmazás, például a Jegyzettömbbel.</span><span class="sxs-lookup"><span data-stu-id="e0624-111">You can use any development tools to create your application, including Notepad.</span></span>

<span data-ttu-id="e0624-112">Ebben az útmutatóban hívható a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó tábla szolgáltatás funkciók használatára.</span><span class="sxs-lookup"><span data-stu-id="e0624-112">In this guide, you use Table service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="e0624-113">Az Azure Ügyfélkódtárai beolvasása</span><span class="sxs-lookup"><span data-stu-id="e0624-113">Get the Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a><span data-ttu-id="e0624-114">Állítsa be az alkalmazását, a Table szolgáltatás eléréséhez</span><span class="sxs-lookup"><span data-stu-id="e0624-114">Configure your application to access the Table service</span></span>
<span data-ttu-id="e0624-115">Az Azure Table szolgáltatás API-kat használ, meg kell:</span><span class="sxs-lookup"><span data-stu-id="e0624-115">To use the Azure Table service APIs, you need to:</span></span>

1. <span data-ttu-id="e0624-116">A robotot fájl használatával hivatkozik a [require_once] [ require_once] utasítást, és</span><span class="sxs-lookup"><span data-stu-id="e0624-116">Reference the autoloader file using the [require_once][require_once] statement, and</span></span>
2. <span data-ttu-id="e0624-117">Bármely osztályok segítségével lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="e0624-117">Reference any classes you might use.</span></span>

<span data-ttu-id="e0624-118">A következő példa bemutatja, hogyan a robotot fájl és a hivatkozás a **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="e0624-118">The following example shows how to include the autoloader file and reference the **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="e0624-119">Ebben a cikkben szereplő példák azt feltételezik, hogy telepítette az Azure-szerkesztő segítségével, a PHP Klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="e0624-119">The examples in this article assume you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="e0624-120">Ha a könyvtárak manuálisan telepítette, hivatkoznia kell a <code>WindowsAzure.php</code> robotjába fájlt.</span><span class="sxs-lookup"><span data-stu-id="e0624-120">If you installed the libraries manually, you need to reference the <code>WindowsAzure.php</code> autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="e0624-121">Az alábbi példákban a `require_once` utasítás mindig megjelenik, de csak a szükséges végrehajtandó például osztályok hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="e0624-121">In the examples below, the `require_once` statement is always shown, but only the classes necessary for the example to execute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="e0624-122">Az Azure storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="e0624-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="e0624-123">Az Azure Table szolgáltatásügyfél hozható létre, először egy érvényes kapcsolati karakterláncot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="e0624-123">To instantiate an Azure Table service client, you must first have a valid connection string.</span></span> <span data-ttu-id="e0624-124">A szolgáltatás kapcsolati karakterlánc formátuma:</span><span class="sxs-lookup"><span data-stu-id="e0624-124">The format for the Table service connection string is:</span></span>

<span data-ttu-id="e0624-125">Élő szolgáltatások elérésére:</span><span class="sxs-lookup"><span data-stu-id="e0624-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="e0624-126">Az emulátor tárolóinak eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="e0624-126">For accessing the emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="e0624-127">Bármely Azure szolgáltatás ügyfele létrehozásához kell használnia a **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="e0624-127">To create any Azure service client, you need to use the **ServicesBuilder** class.</span></span> <span data-ttu-id="e0624-128">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="e0624-128">You can:</span></span>

* <span data-ttu-id="e0624-129">a kapcsolati karakterláncot adja át a közvetlenül vagy</span><span class="sxs-lookup"><span data-stu-id="e0624-129">pass the connection string directly to it or</span></span>
* <span data-ttu-id="e0624-130">használja a **CloudConfigurationManager (CCM)** ellenőrizze a kapcsolati karakterlánc több külső forrás:</span><span class="sxs-lookup"><span data-stu-id="e0624-130">use the **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="e0624-131">Alapértelmezés szerint mellékelt egy külső adatforrás - környezeti változók támogatása</span><span class="sxs-lookup"><span data-stu-id="e0624-131">by default, it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="e0624-132">új forrásból történő kiterjesztésével adhat hozzá a **ConnectionStringSource** osztály</span><span class="sxs-lookup"><span data-stu-id="e0624-132">you can add new sources by extending the **ConnectionStringSource** class</span></span>

<span data-ttu-id="e0624-133">Az itt leírt példák a kapcsolati karakterlánc közvetlenül fog adható át.</span><span class="sxs-lookup"><span data-stu-id="e0624-133">For the examples outlined here, the connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a><span data-ttu-id="e0624-134">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0624-134">Create a table</span></span>
<span data-ttu-id="e0624-135">A **TableRestProxy** objektum lehetővé teszi, hogy a tábla létrehozása a **createTable** metódust.</span><span class="sxs-lookup"><span data-stu-id="e0624-135">A **TableRestProxy** object lets you create a table with the **createTable** method.</span></span> <span data-ttu-id="e0624-136">Ha a táblázatok létrehozásáról, beállíthatja a Table szolgáltatás időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="e0624-136">When creating a table, you can set the Table service timeout.</span></span> <span data-ttu-id="e0624-137">(A Table szolgáltatás időtúllépési kapcsolatos további információkért lásd: [időtúllépések beállítása a szolgáltatási műveletek tábla][table-service-timeouts].)</span><span class="sxs-lookup"><span data-stu-id="e0624-137">(For more information about the Table service timeout, see [Setting Timeouts for Table Service Operations][table-service-timeouts].)</span></span>

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

<span data-ttu-id="e0624-138">Táblanevek vonatkozó megkötésekkel kapcsolatos információkért lásd: [ismertetése a Table szolgáltatás adatmodell][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="e0624-138">For information about restrictions on table names, see [Understanding the Table Service Data Model][table-data-model].</span></span>

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="e0624-139">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="e0624-139">Add an entity to a table</span></span>
<span data-ttu-id="e0624-140">Egy entitás felvételére egy táblához, hozzon létre egy új **entitás** objektumot, és adja át a **TableRestProxy -> insertEntity**.</span><span class="sxs-lookup"><span data-stu-id="e0624-140">To add an entity to a table, create a new **Entity** object and pass it to **TableRestProxy->insertEntity**.</span></span> <span data-ttu-id="e0624-141">Vegye figyelembe, hogy egy entitás létrehozásakor meg kell adnia egy `PartitionKey` és `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="e0624-141">Note that when you create an entity, you must specify a `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="e0624-142">Egy entitás egyedi azonosítói azok és értékek, amelyek sokkal gyorsabb, mint más entitás tulajdonságai kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="e0624-142">These are the unique identifiers for an entity and are values that can be queried much faster than other entity properties.</span></span> <span data-ttu-id="e0624-143">A rendszer használja `PartitionKey` automatikusan terjesztené a táblaentitásokat számos tárolási csomópont feladatait.</span><span class="sxs-lookup"><span data-stu-id="e0624-143">The system uses `PartitionKey` to automatically distribute the table's entities over many storage nodes.</span></span> <span data-ttu-id="e0624-144">Entitások azonos `PartitionKey` ugyanazon a csomóponton találhatók.</span><span class="sxs-lookup"><span data-stu-id="e0624-144">Entities with the same `PartitionKey` are stored on the same node.</span></span> <span data-ttu-id="e0624-145">(Ugyanazon a csomóponton tárolt entitásokat műveletek végrehajtása jobb, mint a más csomópontjai között tárolt entitásokat.) A `RowKey` egy partíció egy entitás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e0624-145">(Operations on multiple entities stored on the same node perform better than on entities stored across different nodes.) The `RowKey` is the unique ID of an entity within a partition.</span></span>

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
$entity->addProperty("Description", null, "Take out the trash.");
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

<span data-ttu-id="e0624-146">További információ a táblázat tulajdonságai és típusok: [ismertetése a Table szolgáltatás adatmodell][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="e0624-146">For information about Table properties and types, see [Understanding the Table Service Data Model][table-data-model].</span></span>

<span data-ttu-id="e0624-147">A **TableRestProxy** osztály entitások beszúrására szolgáló két alternatív módszert kínál: **insertOrMergeEntity** és **insertOrReplaceEntity**.</span><span class="sxs-lookup"><span data-stu-id="e0624-147">The **TableRestProxy** class offers two alternative methods for inserting entities: **insertOrMergeEntity** and **insertOrReplaceEntity**.</span></span> <span data-ttu-id="e0624-148">Ezeket a módszereket használja, hozzon létre egy új **entitás** , és adja át paraméterként mindkét módszer.</span><span class="sxs-lookup"><span data-stu-id="e0624-148">To use these methods, create a new **Entity** and pass it as a parameter to either method.</span></span> <span data-ttu-id="e0624-149">Az egyes módszerek szúrnak az entitás, ha nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e0624-149">Each method will insert the entity if it does not exist.</span></span> <span data-ttu-id="e0624-150">Ha már létezik az entitás, **insertOrMergeEntity** tulajdonságértékek frissíti, ha a tulajdonságai már léteznek, és hozzáadja az új tulajdonságok ha azok nem léteznek, miközben **insertOrReplaceEntity** teljesen lecseréli a meglévő entitás.</span><span class="sxs-lookup"><span data-stu-id="e0624-150">If the entity already exists, **insertOrMergeEntity** updates property values if the properties already exist and adds new properties if they do not exist, while **insertOrReplaceEntity** completely replaces an existing entity.</span></span> <span data-ttu-id="e0624-151">A következő példa bemutatja, hogyan használható **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="e0624-151">The following example shows how to use **insertOrMergeEntity**.</span></span> <span data-ttu-id="e0624-152">Ha az entitás `PartitionKey` "tasksSeattle" és `RowKey` "1" már nem létezik, szúrja be.</span><span class="sxs-lookup"><span data-stu-id="e0624-152">If the entity with `PartitionKey` "tasksSeattle" and `RowKey` "1" does not already exist, it will be inserted.</span></span> <span data-ttu-id="e0624-153">Azonban ha azt korábban beszúrt (a fenti példa szerint), a `DueDate` tulajdonság frissülni fog, és a `Status` tulajdonság megkapja.</span><span class="sxs-lookup"><span data-stu-id="e0624-153">However, if it has previously been inserted (as shown in the example above), the `DueDate` property will be updated, and the `Status` property will be added.</span></span> <span data-ttu-id="e0624-154">A `Description` és `Location` tulajdonságok is frissülnek, de értékekkel, amely hatékonyan hagyja őket változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="e0624-154">The `Description` and `Location` properties are also updated, but with values that effectively leave them unchanged.</span></span> <span data-ttu-id="e0624-155">Ha ez utóbbi a tulajdonság a volt nem szerepel a példában látható módon, de a célentitás már létezett, meglévő értékeik változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="e0624-155">If these latter two properties were not added as shown in the example, but existed on the target entity, their existing values would remain unchanged.</span></span>

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
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
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

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="e0624-156">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="e0624-156">Retrieve a single entity</span></span>
<span data-ttu-id="e0624-157">A **TableRestProxy -> getEntity** módszer lehetővé teszi egyetlen entitás lekérdezése által végzett lekérdezés a `PartitionKey` és `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="e0624-157">The **TableRestProxy->getEntity** method allows you to retrieve a single entity by querying for its `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="e0624-158">A partíciós kulcs az alábbi példában a `tasksSeattle` és sorkulcsa `1` kerülnek átadásra a **getEntity** metódust.</span><span class="sxs-lookup"><span data-stu-id="e0624-158">In the example below, the partition key `tasksSeattle` and row key `1` are passed to the **getEntity** method.</span></span>

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

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="e0624-159">Egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="e0624-159">Retrieve all entities in a partition</span></span>
<span data-ttu-id="e0624-160">Entitás lekérdezések össze szűrők használata (további információkért lásd: [lekérdezése táblákat és entitásokat][filters]).</span><span class="sxs-lookup"><span data-stu-id="e0624-160">Entity queries are constructed using filters (for more information, see [Querying Tables and Entities][filters]).</span></span> <span data-ttu-id="e0624-161">A partíció összes entitásának lekérése, a szűrő használata "PartitionKey eq *partíció_neve*".</span><span class="sxs-lookup"><span data-stu-id="e0624-161">To retrieve all entities in partition, use the filter "PartitionKey eq *partition_name*".</span></span> <span data-ttu-id="e0624-162">A következő példa bemutatja, hogyan összes entitásának lekérése a `tasksSeattle` partíció úgy, hogy egy szűrőt, amely a **queryEntities** metódust.</span><span class="sxs-lookup"><span data-stu-id="e0624-162">The following example shows how to retrieve all entities in the `tasksSeattle` partition by passing a filter to the **queryEntities** method.</span></span>

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

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a><span data-ttu-id="e0624-163">Partíció entitástartományának részhalmazát</span><span class="sxs-lookup"><span data-stu-id="e0624-163">Retrieve a subset of entities in a partition</span></span>
<span data-ttu-id="e0624-164">Az előző példában használt ugyanilyen mintájú segítségével bármely részét partíció entitástartományának lekérése.</span><span class="sxs-lookup"><span data-stu-id="e0624-164">The same pattern used in the previous example can be used to retrieve any subset of entities in a partition.</span></span> <span data-ttu-id="e0624-165">Az entitások lekérése részhalmazát határozza meg a szűrő használata (további információkért lásd: [lekérdezése táblákat és entitásokat][filters]). A következő példa bemutatja, hogyan szűrő segítségével egy adott összes entitásának lekérése `Location` és egy `DueDate` kisebb, mint a megadott dátum.</span><span class="sxs-lookup"><span data-stu-id="e0624-165">The subset of entities you retrieve are determined by the filter you use (for more information, see [Querying Tables and Entities][filters]).The following example shows how to use a filter to retrieve all entities with a specific `Location` and a `DueDate` less than a specified date.</span></span>

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

## <a name="retrieve-a-subset-of-entity-properties"></a><span data-ttu-id="e0624-166">Entitás tulajdonságai részhalmazát</span><span class="sxs-lookup"><span data-stu-id="e0624-166">Retrieve a subset of entity properties</span></span>
<span data-ttu-id="e0624-167">A lekérdezés Entitástulajdonságok egy részének kérheti le.</span><span class="sxs-lookup"><span data-stu-id="e0624-167">A query can retrieve a subset of entity properties.</span></span> <span data-ttu-id="e0624-168">Ezzel a módszerrel nevű *leképezése*, csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat.</span><span class="sxs-lookup"><span data-stu-id="e0624-168">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="e0624-169">Kérhető tulajdonság megadásához adja át a tulajdonság nevét a **-> addSelectField** metódust.</span><span class="sxs-lookup"><span data-stu-id="e0624-169">To specify a property to be retrieved, pass the name of the property to the **Query->addSelectField** method.</span></span> <span data-ttu-id="e0624-170">Ön a metódus hívása többször fel további tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="e0624-170">You can call this method multiple times to add more properties.</span></span> <span data-ttu-id="e0624-171">Végrehajtása után **TableRestProxy -> queryEntities**, a visszaadott entitások csak van a megadott tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="e0624-171">After executing **TableRestProxy->queryEntities**, the returned entities will only have the selected properties.</span></span> <span data-ttu-id="e0624-172">(Ha táblaentitásokat részhalmazát adja vissza, használjon szűrőt a lekérdezések a fenti ábrán.)</span><span class="sxs-lookup"><span data-stu-id="e0624-172">(If you want to return a subset of Table entities, use a filter as shown in the queries above.)</span></span>

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

// All entities in the table are returned, regardless of whether
// they have the Description field.
// To limit the results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a><span data-ttu-id="e0624-173">Egy entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="e0624-173">Update an entity</span></span>
<span data-ttu-id="e0624-174">Meglévő entitás használatával frissíthető a **entitás -> setProperty** és **entitás -> addProperty** entitást, és ezután hívása módszerek **TableRestProxy updateEntity ->**.</span><span class="sxs-lookup"><span data-stu-id="e0624-174">An existing entity can be updated by using the **Entity->setProperty** and **Entity->addProperty** methods on the entity, and then calling **TableRestProxy->updateEntity**.</span></span> <span data-ttu-id="e0624-175">A következő példa egy entitás lekéri, módosítja egy tulajdonságot, egy másik tulajdonságának eltávolítja és új tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="e0624-175">The following example retrieves an entity, modifies one property, removes another property, and adds a new property.</span></span> <span data-ttu-id="e0624-176">Vegye figyelembe, hogy eltávolításához a tulajdonság érték beállítása **null**.</span><span class="sxs-lookup"><span data-stu-id="e0624-176">Note that you can remove a property by setting its value to **null**.</span></span>

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

## <a name="delete-an-entity"></a><span data-ttu-id="e0624-177">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="e0624-177">Delete an entity</span></span>
<span data-ttu-id="e0624-178">Entitás törlése, adja át a táblázat nevét, és az entitás `PartitionKey` és `RowKey` számára a **TableRestProxy -> deleteEntity** metódus.</span><span class="sxs-lookup"><span data-stu-id="e0624-178">To delete an entity, pass the table name, and the entity's `PartitionKey` and `RowKey` to the **TableRestProxy->deleteEntity** method.</span></span>

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

<span data-ttu-id="e0624-179">Vegye figyelembe, hogy párhuzamossági ellenőrzések állíthatók be a Etag, hogy egy entitás használatával a törlendő a **DeleteEntityOptions -> setEtag** metódus és a sikeres a **DeleteEntityOptions** objektum **deleteEntity** negyedik paraméterként.</span><span class="sxs-lookup"><span data-stu-id="e0624-179">Note that for concurrency checks, you can set the Etag for an entity to be deleted by using the **DeleteEntityOptions->setEtag** method and passing the **DeleteEntityOptions** object to **deleteEntity** as a fourth parameter.</span></span>

## <a name="batch-table-operations"></a><span data-ttu-id="e0624-180">Kötegelt tábla műveletek</span><span class="sxs-lookup"><span data-stu-id="e0624-180">Batch table operations</span></span>
<span data-ttu-id="e0624-181">A **TableRestProxy -> kötegelt** módszer lehetővé teszi egyetlen kérelem több műveletek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="e0624-181">The **TableRestProxy->batch** method allows you to execute multiple operations in a single request.</span></span> <span data-ttu-id="e0624-182">A mintát itt magában foglalja a műveletek hozzáadása **BatchRequest** objektumot, és ezután átadja a **BatchRequest** az objektum a **TableRestProxy -> kötegelt** metódust.</span><span class="sxs-lookup"><span data-stu-id="e0624-182">The pattern here involves adding operations to **BatchRequest** object and then passing the **BatchRequest** object to the **TableRestProxy->batch** method.</span></span> <span data-ttu-id="e0624-183">A művelet hozzáadása egy **BatchRequest** objektum többször hívása a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="e0624-183">To add an operation to a **BatchRequest** object, you can call any of the following methods multiple times:</span></span>

* <span data-ttu-id="e0624-184">**addInsertEntity** (hozzáad egy insertEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="e0624-184">**addInsertEntity** (adds an insertEntity operation)</span></span>
* <span data-ttu-id="e0624-185">**addUpdateEntity** (hozzáad egy updateEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="e0624-185">**addUpdateEntity** (adds an updateEntity operation)</span></span>
* <span data-ttu-id="e0624-186">**addMergeEntity** (hozzáad egy mergeEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="e0624-186">**addMergeEntity** (adds a mergeEntity operation)</span></span>
* <span data-ttu-id="e0624-187">**addInsertOrReplaceEntity** (hozzáad egy insertOrReplaceEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="e0624-187">**addInsertOrReplaceEntity** (adds an insertOrReplaceEntity operation)</span></span>
* <span data-ttu-id="e0624-188">**addInsertOrMergeEntity** (hozzáad egy insertOrMergeEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="e0624-188">**addInsertOrMergeEntity** (adds an insertOrMergeEntity operation)</span></span>
* <span data-ttu-id="e0624-189">**addDeleteEntity** (hozzáad egy deleteEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="e0624-189">**addDeleteEntity** (adds a deleteEntity operation)</span></span>

<span data-ttu-id="e0624-190">A következő példa bemutatja, hogyan hajthat végre **insertEntity** és **deleteEntity** egyetlen kérelem műveletek:</span><span class="sxs-lookup"><span data-stu-id="e0624-190">The following example shows how to execute **insertEntity** and **deleteEntity** operations in a single request:</span></span>

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

// Add operation to list of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation to list of batch operations.
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

<span data-ttu-id="e0624-191">Tábla műveletek kötegelése kapcsolatos további információkért lásd: [entitás csoport tranzakciók végrehajtása][entity-group-transactions].</span><span class="sxs-lookup"><span data-stu-id="e0624-191">For more information about batching Table operations, see [Performing Entity Group Transactions][entity-group-transactions].</span></span>

## <a name="delete-a-table"></a><span data-ttu-id="e0624-192">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="e0624-192">Delete a table</span></span>
<span data-ttu-id="e0624-193">Végezetül törölni kívánja a táblázatot, adja át a táblázat nevét, hogy a **TableRestProxy -> deleteTable** metódust.</span><span class="sxs-lookup"><span data-stu-id="e0624-193">Finally, to delete a table, pass the table name to the **TableRestProxy->deleteTable** method.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e0624-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0624-194">Next steps</span></span>
<span data-ttu-id="e0624-195">Most, hogy megismerte az Azure Table szolgáltatás alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről.</span><span class="sxs-lookup"><span data-stu-id="e0624-195">Now that you've learned the basics of the Azure Table service, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="e0624-196">A [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, önálló alkalmazás, amelynek segítségével vizuálisan dolgozhat Azure Storage-adatokkal Windows, macOS és Linux rendszereken.</span><span class="sxs-lookup"><span data-stu-id="e0624-196">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

* <span data-ttu-id="e0624-197">[A PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="e0624-197">[PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
