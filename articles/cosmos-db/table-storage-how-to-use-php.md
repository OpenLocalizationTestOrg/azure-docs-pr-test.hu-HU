---
title: "aaaHow toouse a table storage php-ből |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello PHP toocreate a Table szolgáltatás, és egy tábla, és a Beszúrás, a delete és a lekérdezéstábla hello törlése."
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
ms.openlocfilehash: 5b7c92221069d1c2a6ca951c06ae8eea8bb8478c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-php"></a><span data-ttu-id="89520-103">Hogyan toouse table storage php-ből</span><span class="sxs-lookup"><span data-stu-id="89520-103">How toouse table storage from PHP</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="89520-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="89520-104">Overview</span></span>
<span data-ttu-id="89520-105">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Table szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="89520-105">This guide shows you how tooperform common scenarios using hello Azure Table service.</span></span> <span data-ttu-id="89520-106">hello minták PHP nyelven íródtak, és használja a hello [Azure SDK for PHP][download].</span><span class="sxs-lookup"><span data-stu-id="89520-106">hello samples are written in PHP and use hello [Azure SDK for PHP][download].</span></span> <span data-ttu-id="89520-107">hello tárgyalt forgatókönyvekben szerepel a **létrehozása és egy tábla törlésével és beszúrását, törlését és entitások egy tábla lekérdezése**.</span><span class="sxs-lookup"><span data-stu-id="89520-107">hello scenarios covered include **creating and deleting a table, and inserting, deleting, and querying entities in a table**.</span></span> <span data-ttu-id="89520-108">Hello Azure Table szolgáltatás további információkért lásd: hello [további lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="89520-108">For more information on hello Azure Table service, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="89520-109">PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="89520-109">Create a PHP application</span></span>
<span data-ttu-id="89520-110">egyetlen hello Azure Table szolgáltatás hozzáférő PHP-alkalmazások létrehozására vonatkozó követelmény az hello hello való hivatkozás hello Azure SDK-t az osztályokat a php a kód.</span><span class="sxs-lookup"><span data-stu-id="89520-110">hello only requirement for creating a PHP application that accesses hello Azure Table service is hello referencing of classes in hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="89520-111">A fejlesztői eszközök toocreate használhatja az alkalmazás, például a Jegyzettömbbel.</span><span class="sxs-lookup"><span data-stu-id="89520-111">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="89520-112">Ebben az útmutatóban hívható a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó tábla szolgáltatás funkciók használatára.</span><span class="sxs-lookup"><span data-stu-id="89520-112">In this guide, you use Table service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="89520-113">Hello Azure Ügyfélkódtárai beolvasása</span><span class="sxs-lookup"><span data-stu-id="89520-113">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-table-service"></a><span data-ttu-id="89520-114">Az alkalmazás tooaccess hello Table szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="89520-114">Configure your application tooaccess hello Table service</span></span>
<span data-ttu-id="89520-115">toouse hello Azure Table szolgáltatás API-k, kell:</span><span class="sxs-lookup"><span data-stu-id="89520-115">toouse hello Azure Table service APIs, you need to:</span></span>

1. <span data-ttu-id="89520-116">Hello robotjába referenciafájlban hello segítségével [require_once] [ require_once] utasítást, és</span><span class="sxs-lookup"><span data-stu-id="89520-116">Reference hello autoloader file using hello [require_once][require_once] statement, and</span></span>
2. <span data-ttu-id="89520-117">Bármely osztályok segítségével lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="89520-117">Reference any classes you might use.</span></span>

<span data-ttu-id="89520-118">hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="89520-118">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="89520-119">Ebben a cikkben a hello példák feltételezik, hogy telepítette hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával.</span><span class="sxs-lookup"><span data-stu-id="89520-119">hello examples in this article assume you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="89520-120">Ha manuálisan telepítette a hello szalagtárak, meg kell-e tooreference hello <code>WindowsAzure.php</code> robotjába fájlt.</span><span class="sxs-lookup"><span data-stu-id="89520-120">If you installed hello libraries manually, you need tooreference hello <code>WindowsAzure.php</code> autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="89520-121">Hello a következő példa a hello `require_once` utasítás mindig megjelenik, de csak hello osztályok hello példa tooexecute szükséges hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="89520-121">In hello examples below, hello `require_once` statement is always shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="89520-122">Az Azure storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="89520-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="89520-123">az Azure Table szolgáltatásügyfél tooinstantiate, először rendelkeznie kell érvényes kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="89520-123">tooinstantiate an Azure Table service client, you must first have a valid connection string.</span></span> <span data-ttu-id="89520-124">hello hello tábla szolgáltatás kapcsolati karakterláncának formátuma:</span><span class="sxs-lookup"><span data-stu-id="89520-124">hello format for hello Table service connection string is:</span></span>

<span data-ttu-id="89520-125">Élő szolgáltatások elérésére:</span><span class="sxs-lookup"><span data-stu-id="89520-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="89520-126">Hello emulátor tárolóinak eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="89520-126">For accessing hello emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="89520-127">toocreate bármely Azure szolgáltatás ügyfele toouse hello kell **ServicesBuilder** osztály.</span><span class="sxs-lookup"><span data-stu-id="89520-127">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="89520-128">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="89520-128">You can:</span></span>

* <span data-ttu-id="89520-129">átadni hello kapcsolati karakterlánc közvetlenül tooit vagy</span><span class="sxs-lookup"><span data-stu-id="89520-129">pass hello connection string directly tooit or</span></span>
* <span data-ttu-id="89520-130">hello használata **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:</span><span class="sxs-lookup"><span data-stu-id="89520-130">use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="89520-131">Alapértelmezés szerint mellékelt egy külső adatforrás - környezeti változók támogatása</span><span class="sxs-lookup"><span data-stu-id="89520-131">by default, it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="89520-132">hozzáadhat új források hello kiterjesztésével **ConnectionStringSource** osztály</span><span class="sxs-lookup"><span data-stu-id="89520-132">you can add new sources by extending hello **ConnectionStringSource** class</span></span>

<span data-ttu-id="89520-133">Itt vázolt hello példákért hello kapcsolati karakterlánc közvetlenül fog adható át.</span><span class="sxs-lookup"><span data-stu-id="89520-133">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a><span data-ttu-id="89520-134">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="89520-134">Create a table</span></span>
<span data-ttu-id="89520-135">A **TableRestProxy** objektum lehetővé teszi, hogy hozzon létre egy táblát hello **createTable** metódust.</span><span class="sxs-lookup"><span data-stu-id="89520-135">A **TableRestProxy** object lets you create a table with hello **createTable** method.</span></span> <span data-ttu-id="89520-136">Egy tábla létrehozásakor beállíthatja hello tábla szolgáltatás időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="89520-136">When creating a table, you can set hello Table service timeout.</span></span> <span data-ttu-id="89520-137">(Hello tábla szolgáltatás időtúllépési kapcsolatos további információkért lásd: [időtúllépések beállítása a szolgáltatási műveletek tábla][table-service-timeouts].)</span><span class="sxs-lookup"><span data-stu-id="89520-137">(For more information about hello Table service timeout, see [Setting Timeouts for Table Service Operations][table-service-timeouts].)</span></span>

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

<span data-ttu-id="89520-138">Táblanevek vonatkozó megkötésekkel kapcsolatos információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="89520-138">For information about restrictions on table names, see [Understanding hello Table Service Data Model][table-data-model].</span></span>

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="89520-139">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="89520-139">Add an entity tooa table</span></span>
<span data-ttu-id="89520-140">egy entitás tooa tábla tooadd hozzon létre egy új **entitás** objektumot, és adja át túl**TableRestProxy -> insertEntity**.</span><span class="sxs-lookup"><span data-stu-id="89520-140">tooadd an entity tooa table, create a new **Entity** object and pass it too**TableRestProxy->insertEntity**.</span></span> <span data-ttu-id="89520-141">Vegye figyelembe, hogy egy entitás létrehozásakor meg kell adnia egy `PartitionKey` és `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="89520-141">Note that when you create an entity, you must specify a `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="89520-142">Ezek hello egy entitás egyedi azonosítót, és olyan értékek, amelyek sokkal gyorsabb, mint más entitás tulajdonságai kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="89520-142">These are hello unique identifiers for an entity and are values that can be queried much faster than other entity properties.</span></span> <span data-ttu-id="89520-143">hello rendszer `PartitionKey` tooautomatically hello tábla entitások terjesztése számos tárolási csomópont feladatait.</span><span class="sxs-lookup"><span data-stu-id="89520-143">hello system uses `PartitionKey` tooautomatically distribute hello table's entities over many storage nodes.</span></span> <span data-ttu-id="89520-144">Az entitások hello azonos `PartitionKey` hello tárolt ugyanahhoz a csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="89520-144">Entities with hello same `PartitionKey` are stored on hello same node.</span></span> <span data-ttu-id="89520-145">(Több entitás hello ugyanahhoz a csomóponthoz hajtsa végre a tárolt műveletek jobb, mint a más csomópontjai között tárolt entitásokat.) Hello `RowKey` hello egy partíció egy entitás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="89520-145">(Operations on multiple entities stored on hello same node perform better than on entities stored across different nodes.) hello `RowKey` is hello unique ID of an entity within a partition.</span></span>

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

<span data-ttu-id="89520-146">További információ a táblázat tulajdonságai és típusok: [ismertetése hello tábla szolgáltatás adatmodell][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="89520-146">For information about Table properties and types, see [Understanding hello Table Service Data Model][table-data-model].</span></span>

<span data-ttu-id="89520-147">Hello **TableRestProxy** osztály entitások beszúrására szolgáló két alternatív módszert kínál: **insertOrMergeEntity** és **insertOrReplaceEntity**.</span><span class="sxs-lookup"><span data-stu-id="89520-147">hello **TableRestProxy** class offers two alternative methods for inserting entities: **insertOrMergeEntity** and **insertOrReplaceEntity**.</span></span> <span data-ttu-id="89520-148">toouse módszerekhez, hozzon létre egy új **entitás** , és adja át paraméter tooeither módszerként.</span><span class="sxs-lookup"><span data-stu-id="89520-148">toouse these methods, create a new **Entity** and pass it as a parameter tooeither method.</span></span> <span data-ttu-id="89520-149">Az egyes módszerek szúrnak hello entitás, ha nem létezik.</span><span class="sxs-lookup"><span data-stu-id="89520-149">Each method will insert hello entity if it does not exist.</span></span> <span data-ttu-id="89520-150">Ha már létezik hello entitás, **insertOrMergeEntity** tulajdonságértékek frissíti, ha hello tulajdonságai már léteznek, és hozzáadja az új tulajdonságok ha azok nem léteznek, miközben **insertOrReplaceEntity** teljesen lecseréli a meglévő entitás.</span><span class="sxs-lookup"><span data-stu-id="89520-150">If hello entity already exists, **insertOrMergeEntity** updates property values if hello properties already exist and adds new properties if they do not exist, while **insertOrReplaceEntity** completely replaces an existing entity.</span></span> <span data-ttu-id="89520-151">a következő példa azt mutatja meg hogyan hello toouse **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="89520-151">hello following example shows how toouse **insertOrMergeEntity**.</span></span> <span data-ttu-id="89520-152">Ha az entitás hello `PartitionKey` "tasksSeattle" és `RowKey` "1" már nem létezik, szúrja be.</span><span class="sxs-lookup"><span data-stu-id="89520-152">If hello entity with `PartitionKey` "tasksSeattle" and `RowKey` "1" does not already exist, it will be inserted.</span></span> <span data-ttu-id="89520-153">Azonban ha azt korábban beszúrt (a fenti példa hello), hello `DueDate` tulajdonság frissülni fog, és hello `Status` tulajdonság megkapja.</span><span class="sxs-lookup"><span data-stu-id="89520-153">However, if it has previously been inserted (as shown in hello example above), hello `DueDate` property will be updated, and hello `Status` property will be added.</span></span> <span data-ttu-id="89520-154">Hello `Description` és `Location` tulajdonságok is frissülnek, de értékekkel, amely hatékonyan hagyja őket változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="89520-154">hello `Description` and `Location` properties are also updated, but with values that effectively leave them unchanged.</span></span> <span data-ttu-id="89520-155">Ha ez utóbbi a tulajdonság nem hozzáadott hello példában látható módon, de hello célentitás létezett, meglévő értékeik változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="89520-155">If these latter two properties were not added as shown in hello example, but existed on hello target entity, their existing values would remain unchanged.</span></span>

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

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="89520-156">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="89520-156">Retrieve a single entity</span></span>
<span data-ttu-id="89520-157">Hello **TableRestProxy -> getEntity** módszer lehetővé teszi a tooretrieve által végzett lekérdezés egyetlen entitás az `PartitionKey` és `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="89520-157">hello **TableRestProxy->getEntity** method allows you tooretrieve a single entity by querying for its `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="89520-158">Hello az alábbi példában a partíciós kulcs hello `tasksSeattle` és sorkulcsa `1` toohello átadott **getEntity** metódust.</span><span class="sxs-lookup"><span data-stu-id="89520-158">In hello example below, hello partition key `tasksSeattle` and row key `1` are passed toohello **getEntity** method.</span></span>

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

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="89520-159">Egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="89520-159">Retrieve all entities in a partition</span></span>
<span data-ttu-id="89520-160">Entitás lekérdezések össze szűrők használata (további információkért lásd: [lekérdezése táblákat és entitásokat][filters]).</span><span class="sxs-lookup"><span data-stu-id="89520-160">Entity queries are constructed using filters (for more information, see [Querying Tables and Entities][filters]).</span></span> <span data-ttu-id="89520-161">tooretrieve partíció összes entitásának hello szűrő használata "PartitionKey eq *partíció_neve*".</span><span class="sxs-lookup"><span data-stu-id="89520-161">tooretrieve all entities in partition, use hello filter "PartitionKey eq *partition_name*".</span></span> <span data-ttu-id="89520-162">a következő példa azt mutatja meg hogyan hello tooretrieve hello összes entitásának `tasksSeattle` úgy, hogy a szűrő toohello partíció **queryEntities** metódust.</span><span class="sxs-lookup"><span data-stu-id="89520-162">hello following example shows how tooretrieve all entities in hello `tasksSeattle` partition by passing a filter toohello **queryEntities** method.</span></span>

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

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a><span data-ttu-id="89520-163">Partíció entitástartományának részhalmazát</span><span class="sxs-lookup"><span data-stu-id="89520-163">Retrieve a subset of entities in a partition</span></span>
<span data-ttu-id="89520-164">hello előző példában használt ugyanilyen mintájú hello lehet használt tooretrieve partíció entitástartományának csoportját.</span><span class="sxs-lookup"><span data-stu-id="89520-164">hello same pattern used in hello previous example can be used tooretrieve any subset of entities in a partition.</span></span> <span data-ttu-id="89520-165">visszaállíthatja az entitások hello részhalmazát hello szűrő használata határozza meg (további információkért lásd: [lekérdezése táblákat és entitásokat][filters]) a következő példa azt mutatja meg hogyan .hello toouse egy szűrő tooretrieve egy adott rendelkező összes entitás `Location` és egy `DueDate` kisebb, mint a megadott dátum.</span><span class="sxs-lookup"><span data-stu-id="89520-165">hello subset of entities you retrieve are determined by hello filter you use (for more information, see [Querying Tables and Entities][filters]).hello following example shows how toouse a filter tooretrieve all entities with a specific `Location` and a `DueDate` less than a specified date.</span></span>

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

## <a name="retrieve-a-subset-of-entity-properties"></a><span data-ttu-id="89520-166">Entitás tulajdonságai részhalmazát</span><span class="sxs-lookup"><span data-stu-id="89520-166">Retrieve a subset of entity properties</span></span>
<span data-ttu-id="89520-167">A lekérdezés Entitástulajdonságok egy részének kérheti le.</span><span class="sxs-lookup"><span data-stu-id="89520-167">A query can retrieve a subset of entity properties.</span></span> <span data-ttu-id="89520-168">Ezzel a módszerrel nevű *leképezése*, csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat.</span><span class="sxs-lookup"><span data-stu-id="89520-168">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="89520-169">toospecify egy tulajdonság toobe beolvasni, hello tulajdonság toohello hello nevét át **-> addSelectField** metódust.</span><span class="sxs-lookup"><span data-stu-id="89520-169">toospecify a property toobe retrieved, pass hello name of hello property toohello **Query->addSelectField** method.</span></span> <span data-ttu-id="89520-170">Ez a metódus többször tooadd további tulajdonságok is meghívható.</span><span class="sxs-lookup"><span data-stu-id="89520-170">You can call this method multiple times tooadd more properties.</span></span> <span data-ttu-id="89520-171">Végrehajtása után **TableRestProxy -> queryEntities**, hello visszaadott entitások csak van kiválasztva hello tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="89520-171">After executing **TableRestProxy->queryEntities**, hello returned entities will only have hello selected properties.</span></span> <span data-ttu-id="89520-172">(Ha azt szeretné, hogy tooreturn táblaentitásokat részhalmazát, használjon szűrőt hello lekérdezések a fenti ábrán.)</span><span class="sxs-lookup"><span data-stu-id="89520-172">(If you want tooreturn a subset of Table entities, use a filter as shown in hello queries above.)</span></span>

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

## <a name="update-an-entity"></a><span data-ttu-id="89520-173">Egy entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="89520-173">Update an entity</span></span>
<span data-ttu-id="89520-174">Meglévő entitás hello használatával frissíthető **entitás -> setProperty** és **entitás -> addProperty** hello entitáshoz, és ezután hívása metódusai **TableRestProxy updateEntity ->** .</span><span class="sxs-lookup"><span data-stu-id="89520-174">An existing entity can be updated by using hello **Entity->setProperty** and **Entity->addProperty** methods on hello entity, and then calling **TableRestProxy->updateEntity**.</span></span> <span data-ttu-id="89520-175">hello alábbi példa egy entitás lekéri, módosítja egy tulajdonságot, egy másik tulajdonságának eltávolítja és új tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="89520-175">hello following example retrieves an entity, modifies one property, removes another property, and adds a new property.</span></span> <span data-ttu-id="89520-176">Vegye figyelembe, hogy egy tulajdonság értéke túl beállításával eltávolíthatja**null**.</span><span class="sxs-lookup"><span data-stu-id="89520-176">Note that you can remove a property by setting its value too**null**.</span></span>

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

## <a name="delete-an-entity"></a><span data-ttu-id="89520-177">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="89520-177">Delete an entity</span></span>
<span data-ttu-id="89520-178">egy entitás toodelete átadni hello táblanév és hello entitás `PartitionKey` és `RowKey` toohello **TableRestProxy -> deleteEntity** metódust.</span><span class="sxs-lookup"><span data-stu-id="89520-178">toodelete an entity, pass hello table name, and hello entity's `PartitionKey` and `RowKey` toohello **TableRestProxy->deleteEntity** method.</span></span>

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

<span data-ttu-id="89520-179">Vegye figyelembe, hogy párhuzamossági ellenőrzések állíthatók be hello Etag egy entitás toobe hello használatával törli a **DeleteEntityOptions -> setEtag** metódus és sikeres hello **DeleteEntityOptions** objektum túl**deleteEntity** negyedik paraméterként.</span><span class="sxs-lookup"><span data-stu-id="89520-179">Note that for concurrency checks, you can set hello Etag for an entity toobe deleted by using hello **DeleteEntityOptions->setEtag** method and passing hello **DeleteEntityOptions** object too**deleteEntity** as a fourth parameter.</span></span>

## <a name="batch-table-operations"></a><span data-ttu-id="89520-180">Kötegelt tábla műveletek</span><span class="sxs-lookup"><span data-stu-id="89520-180">Batch table operations</span></span>
<span data-ttu-id="89520-181">Hello **TableRestProxy -> kötegelt** metódus tooexecute lehetővé teszi több művelet egyetlen kérelemben.</span><span class="sxs-lookup"><span data-stu-id="89520-181">hello **TableRestProxy->batch** method allows you tooexecute multiple operations in a single request.</span></span> <span data-ttu-id="89520-182">hello mintát itt magában foglalja a műveletek túl hozzáadása**BatchRequest** objektumot, és ezután átadja az hello **BatchRequest** toohello objektum **TableRestProxy -> kötegelt** metódust.</span><span class="sxs-lookup"><span data-stu-id="89520-182">hello pattern here involves adding operations too**BatchRequest** object and then passing hello **BatchRequest** object toohello **TableRestProxy->batch** method.</span></span> <span data-ttu-id="89520-183">egy művelet tooa tooadd **BatchRequest** objektum hívása a következő módszerek többször hello bármelyikét:</span><span class="sxs-lookup"><span data-stu-id="89520-183">tooadd an operation tooa **BatchRequest** object, you can call any of hello following methods multiple times:</span></span>

* <span data-ttu-id="89520-184">**addInsertEntity** (hozzáad egy insertEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="89520-184">**addInsertEntity** (adds an insertEntity operation)</span></span>
* <span data-ttu-id="89520-185">**addUpdateEntity** (hozzáad egy updateEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="89520-185">**addUpdateEntity** (adds an updateEntity operation)</span></span>
* <span data-ttu-id="89520-186">**addMergeEntity** (hozzáad egy mergeEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="89520-186">**addMergeEntity** (adds a mergeEntity operation)</span></span>
* <span data-ttu-id="89520-187">**addInsertOrReplaceEntity** (hozzáad egy insertOrReplaceEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="89520-187">**addInsertOrReplaceEntity** (adds an insertOrReplaceEntity operation)</span></span>
* <span data-ttu-id="89520-188">**addInsertOrMergeEntity** (hozzáad egy insertOrMergeEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="89520-188">**addInsertOrMergeEntity** (adds an insertOrMergeEntity operation)</span></span>
* <span data-ttu-id="89520-189">**addDeleteEntity** (hozzáad egy deleteEntity művelet)</span><span class="sxs-lookup"><span data-stu-id="89520-189">**addDeleteEntity** (adds a deleteEntity operation)</span></span>

<span data-ttu-id="89520-190">a következő példa azt mutatja meg hogyan hello tooexecute **insertEntity** és **deleteEntity** egyetlen kérelem műveletek:</span><span class="sxs-lookup"><span data-stu-id="89520-190">hello following example shows how tooexecute **insertEntity** and **deleteEntity** operations in a single request:</span></span>

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

<span data-ttu-id="89520-191">Tábla műveletek kötegelése kapcsolatos további információkért lásd: [entitás csoport tranzakciók végrehajtása][entity-group-transactions].</span><span class="sxs-lookup"><span data-stu-id="89520-191">For more information about batching Table operations, see [Performing Entity Group Transactions][entity-group-transactions].</span></span>

## <a name="delete-a-table"></a><span data-ttu-id="89520-192">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="89520-192">Delete a table</span></span>
<span data-ttu-id="89520-193">Végül adja át a toodelete egy tábla hello tábla neve toohello **TableRestProxy -> deleteTable** metódust.</span><span class="sxs-lookup"><span data-stu-id="89520-193">Finally, toodelete a table, pass hello table name toohello **TableRestProxy->deleteTable** method.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="89520-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89520-194">Next steps</span></span>
<span data-ttu-id="89520-195">Most, hogy megismerte hello Azure Table szolgáltatás hello alapjait, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="89520-195">Now that you've learned hello basics of hello Azure Table service, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="89520-196">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89520-196">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

* <span data-ttu-id="89520-197">[A PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="89520-197">[PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
