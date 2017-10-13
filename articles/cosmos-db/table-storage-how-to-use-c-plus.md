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
# <a name="how-toouse-table-storage-from-c"></a><span data-ttu-id="c5f71-103">Hogyan toouse Table storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="c5f71-103">How toouse Table storage from C++</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="c5f71-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c5f71-104">Overview</span></span>
<span data-ttu-id="c5f71-105">Ez az útmutató bemutatja, hogyan tooperform gyakori helyzetek használatával hello Azure Table storage szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c5f71-105">This guide will show you how tooperform common scenarios by using hello Azure Table storage service.</span></span> <span data-ttu-id="c5f71-106">hello minták C++ nyelven íródtak, és használja a hello [Azure Storage ügyféloldali kódtára a C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="c5f71-106">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="c5f71-107">hello tárgyalt forgatókönyvekben szerepel a **létrehozása és egy tábla törlése** és **táblaentitásokat használata**.</span><span class="sxs-lookup"><span data-stu-id="c5f71-107">hello scenarios covered include **creating and deleting a table** and **working with table entities**.</span></span>

> [!NOTE]
> <span data-ttu-id="c5f71-108">Ez az útmutató célok hello a Azure Storage ügyféloldali kódtára a C++ 1.0.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="c5f71-108">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="c5f71-109">hello ajánlott verzió a Storage ügyféloldali kódtára 2.2.0, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="c5f71-109">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="c5f71-110">A C++-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5f71-110">Create a C++ application</span></span>
<span data-ttu-id="c5f71-111">Ez az útmutató egy C++ alkalmazáson belül futó tárolási szolgáltatásokkal fogja használni.</span><span class="sxs-lookup"><span data-stu-id="c5f71-111">In this guide, you will use storage features that can be run within a C++ application.</span></span> <span data-ttu-id="c5f71-112">toodo tooinstall kell tehát hello Azure Storage ügyféloldali kódtára a C++ és az Azure storage-fiók létrehozása az Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="c5f71-112">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>  

<span data-ttu-id="c5f71-113">tooinstall hello Azure Storage ügyféloldali kódtára a C++, a következő módszerek hello használhatják:</span><span class="sxs-lookup"><span data-stu-id="c5f71-113">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="c5f71-114">**Linux:** hello adott hello utasítások [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.</span><span class="sxs-lookup"><span data-stu-id="c5f71-114">**Linux:** Follow hello instructions given on hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="c5f71-115">**Windows:** a Visual Studióban kattintson **eszközök > NuGet-Csomagkezelő > Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="c5f71-115">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="c5f71-116">Típus hello következő parancsot a hello történő [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="c5f71-116">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press Enter.</span></span>  
  
     <span data-ttu-id="c5f71-117">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="c5f71-117">Install-Package wastorage</span></span>

## <a name="configure-your-application-tooaccess-table-storage"></a><span data-ttu-id="c5f71-118">Az alkalmazás tooaccess a Table storage konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c5f71-118">Configure your application tooaccess Table storage</span></span>
<span data-ttu-id="c5f71-119">Adja hozzá, hello következő tartalmazó utasítások toohello felső részén hello C++ fájl toouse hello az Azure storage API-k tooaccess táblák:</span><span class="sxs-lookup"><span data-stu-id="c5f71-119">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess tables:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="c5f71-120">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="c5f71-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="c5f71-121">Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatok felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c5f71-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="c5f71-122">Ha egy ügyfél-alkalmazás fut, meg kell adnia hello tárolási kapcsolati karakterlánc formátuma a következő hello.</span><span class="sxs-lookup"><span data-stu-id="c5f71-122">When running a client application, you must provide hello storage connection string in hello following format.</span></span> <span data-ttu-id="c5f71-123">Használjon hello a tárolási fiók és hello tárelérési kulcs hello tárfiók neve a hello [Azure Portal](https://portal.azure.com) a hello *AccountName* és *AccountKey* értékek.</span><span class="sxs-lookup"><span data-stu-id="c5f71-123">Use hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="c5f71-124">A storage-fiókok és a hívóbetűk információkért lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="c5f71-124">For information on storage accounts and access keys, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="c5f71-125">Ez a példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="c5f71-125">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="c5f71-126">tootest az alkalmazás a helyi Windows-alapú számítógépeken, használhatja a hello Azure [storage emulator](../storage/common/storage-use-emulator.md) hello a telepített [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c5f71-126">tootest your application in your local Windows-based computer, you can use hello Azure [storage emulator](../storage/common/storage-use-emulator.md) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="c5f71-127">hello storage emulator egy segédprogram, amely szimulálja hello Azure Blob, Queue és Table szolgáltatások a helyi fejlesztési számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c5f71-127">hello storage emulator is a utility that simulates hello Azure Blob, Queue, and Table services available on your local development machine.</span></span> <span data-ttu-id="c5f71-128">hello következő példa bemutatja, hogyan deklarálhatnak egy statikus mező toohold hello kapcsolati karakterlánc tooyour helyi storage emulatort:</span><span class="sxs-lookup"><span data-stu-id="c5f71-128">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>  

```cpp
// Define hello connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="c5f71-129">toostart hello Azure storage emulatort, kattintson a hello **Start** gombra vagy nyomja le az hello Windows kulcsot.</span><span class="sxs-lookup"><span data-stu-id="c5f71-129">toostart hello Azure storage emulator, click hello **Start** button or press hello Windows key.</span></span> <span data-ttu-id="c5f71-130">Írja be a szöveget **Azure Storage Emulator**, majd válassza ki **Microsoft Azure Storage Emulator** hello alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c5f71-130">Begin typing **Azure Storage Emulator**, and then select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>  

<span data-ttu-id="c5f71-131">hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="c5f71-131">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="c5f71-132">A kapcsolat-karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="c5f71-132">Retrieve your connection string</span></span>
<span data-ttu-id="c5f71-133">Használhatja a hello **cloud_storage_account** osztály toorepresent a tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="c5f71-133">You can use hello **cloud_storage_account** class toorepresent your storage account information.</span></span> <span data-ttu-id="c5f71-134">tooretrieve a tárolási fiók hello tárolási kapcsolati karakterlánc adatait, hello parse metódus használatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="c5f71-134">tooretrieve your storage account information from hello storage connection string, you can use hello parse method.</span></span>

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="c5f71-135">A következő lekérni egy hivatkozás tooa **cloud_table_client** osztályt, lehetővé teszi, hogy a hivatkozás objektumok lekérni a táblák és entitásokat a Table storage szolgáltatás tárolja hello.</span><span class="sxs-lookup"><span data-stu-id="c5f71-135">Next, get a reference tooa **cloud_table_client** class, as it lets you get reference objects for tables and entities stored within hello Table storage service.</span></span> <span data-ttu-id="c5f71-136">hello alábbi kód létrehoz egy **cloud_table_client** objektum hello tárolási fiók objektum azt lekérése fent használatával:</span><span class="sxs-lookup"><span data-stu-id="c5f71-136">hello following code creates a **cloud_table_client** object by using hello storage account object we retrieved above:</span></span>  

```cpp
// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a><span data-ttu-id="c5f71-137">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5f71-137">Create a table</span></span>
<span data-ttu-id="c5f71-138">A **cloud_table_client** objektum lehetővé teszi, hogy a hivatkozás objektum lekérése a táblákat és entitásokat.</span><span class="sxs-lookup"><span data-stu-id="c5f71-138">A **cloud_table_client** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="c5f71-139">hello alábbi kód létrehoz egy **cloud_table_client** objektumra, és használja ezt a toocreate új tábla.</span><span class="sxs-lookup"><span data-stu-id="c5f71-139">hello following code creates a **cloud_table_client** object and uses it toocreate a new table.</span></span>

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

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="c5f71-140">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c5f71-140">Add an entity tooa table</span></span>
<span data-ttu-id="c5f71-141">egy entitás tooa tábla tooadd hozzon létre egy új **table_entity** objektumot, és adja át túl**table_operation::insert_entity**.</span><span class="sxs-lookup"><span data-stu-id="c5f71-141">tooadd an entity tooa table, create a new **table_entity** object and pass it too**table_operation::insert_entity**.</span></span> <span data-ttu-id="c5f71-142">hello alábbira használja hello ügyfél keresztnevét hello sorkulcsnak és a Vezetéknév hello partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="c5f71-142">hello following code uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="c5f71-143">Együtt egy entitás partíció- és sorkulcsa egyértelműen hello entitás hello táblában.</span><span class="sxs-lookup"><span data-stu-id="c5f71-143">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="c5f71-144">Ugyanazzal a partíciókulccsal gyorsabb, mint a különböző lekérdezhetők, hello rendelkező entitások partícióazonosító kulcsok, de az eltérő partíciókulcsok használata lehetővé teszi a párhuzamos művelet nagyobb méretezhetőségét.</span><span class="sxs-lookup"><span data-stu-id="c5f71-144">Entities with hello same partition key can be queried faster than those with different partition keys, but using diverse partition keys allows for greater parallel operation scalability.</span></span> <span data-ttu-id="c5f71-145">További információkért lásd: [Microsoft Azure storage teljesítményére és méretezhetőségére ellenőrzőlista](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="c5f71-145">For more information, see [Microsoft Azure storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>

<span data-ttu-id="c5f71-146">hello alábbi kód létrehoz egy új példányát **table_entity** az egyes tárolt felhasználói adatok toobe.</span><span class="sxs-lookup"><span data-stu-id="c5f71-146">hello following code creates a new instance of **table_entity** with some customer data toobe stored.</span></span> <span data-ttu-id="c5f71-147">kód következő hívások hello **table_operation::insert_entity** toocreate egy **table_operation** tooinsert entitás objektum egy táblába, és társult hello új táblaentitássá vele.</span><span class="sxs-lookup"><span data-stu-id="c5f71-147">hello code next calls **table_operation::insert_entity** toocreate a **table_operation** object tooinsert an entity into a table, and associates hello new table entity with it.</span></span> <span data-ttu-id="c5f71-148">Végezetül hello kód hívások hello metódus végrehajtása a hello **cloud_table** objektum.</span><span class="sxs-lookup"><span data-stu-id="c5f71-148">Finally, hello code calls hello execute method on hello **cloud_table** object.</span></span> <span data-ttu-id="c5f71-149">És új hello **table_operation** egy kérelem toohello Table szolgáltatás tooinsert hello új ügyfélentitást küldi hello "személyek" táblába.</span><span class="sxs-lookup"><span data-stu-id="c5f71-149">And hello new **table_operation** sends a request toohello Table service tooinsert hello new customer entity into hello "people" table.</span></span>  

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

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="c5f71-150">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="c5f71-150">Insert a batch of entities</span></span>
<span data-ttu-id="c5f71-151">Egyetlen írási művelettel egy tranzakcióköteghez entitások toohello Table szolgáltatás is beszúrhat.</span><span class="sxs-lookup"><span data-stu-id="c5f71-151">You can insert a batch of entities toohello Table service in one write operation.</span></span> <span data-ttu-id="c5f71-152">hello alábbi kód létrehoz egy **table_batch_operation** objektumot, és hozzáadja a három beszúrási műveletek tooit.</span><span class="sxs-lookup"><span data-stu-id="c5f71-152">hello following code creates a **table_batch_operation** object, and then adds three insert operations tooit.</span></span> <span data-ttu-id="c5f71-153">Minden egyes végrehajtott beszúrási művelet bekerül hozzon létre egy új entitásobjektumot, állítsa be az értékeket, és majd hívja a hello beszúrása hello metódusa **table_batch_operation** objektum tooassociate hello entitást egy új beszúrási művelet esetén.</span><span class="sxs-lookup"><span data-stu-id="c5f71-153">Each insert operation is added by creating a new entity object, setting its values, and then calling hello insert method on hello **table_batch_operation** object tooassociate hello entity with a new insert operation.</span></span> <span data-ttu-id="c5f71-154">Ezt követően **cloud_table.execute** tooexecute hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="c5f71-154">Then, **cloud_table.execute** is called tooexecute hello operation.</span></span>  

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

<span data-ttu-id="c5f71-155">Néhány dolgot toonote a kötegműveletekkel kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="c5f71-155">Some things toonote on batch operations:</span></span>  

* <span data-ttu-id="c5f71-156">Too100 insert, törlés, egyesítési, csere, bármilyen kombinációban egyetlen kötegben insert vagy egyesítés és beszúrása vagy lecserélése műveletek elvégzése.</span><span class="sxs-lookup"><span data-stu-id="c5f71-156">You can perform up too100 insert, delete, merge, replace, insert-or-merge, and insert-or-replace operations in any combination in a single batch.</span></span>  
* <span data-ttu-id="c5f71-157">A kötegelt művelet lehet a beolvasási műveletet hello hello köteg egyetlen művelete esetén.</span><span class="sxs-lookup"><span data-stu-id="c5f71-157">A batch operation can have a retrieve operation, if it is hello only operation in hello batch.</span></span>  
* <span data-ttu-id="c5f71-158">Hello rendelkeznie kell egy kötegművelet összes entitásának ugyanazzal a partíciókulccsal.</span><span class="sxs-lookup"><span data-stu-id="c5f71-158">All entities in a single batch operation must have hello same partition key.</span></span>  
* <span data-ttu-id="c5f71-159">A kötegelt művelet korlátozott tooa 4 MB-os adattartalom.</span><span class="sxs-lookup"><span data-stu-id="c5f71-159">A batch operation is limited tooa 4-MB data payload.</span></span>  

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="c5f71-160">Egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="c5f71-160">Retrieve all entities in a partition</span></span>
<span data-ttu-id="c5f71-161">egy tábla használja egy partíció összes entitását tooquery egy **table_query** objektum.</span><span class="sxs-lookup"><span data-stu-id="c5f71-161">tooquery a table for all entities in a partition, use a **table_query** object.</span></span> <span data-ttu-id="c5f71-162">hello alábbi példakód megad egy szűrőt, ahol a "Smith" hello partíciókulcs az entitások.</span><span class="sxs-lookup"><span data-stu-id="c5f71-162">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="c5f71-163">Ez a példa kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="c5f71-163">This example prints hello fields of each entity in hello query results toohello console.</span></span>  

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

<span data-ttu-id="c5f71-164">Ebben a példában hello lekérdezés során az hello szűrési feltételeknek megfelelő összes hello entitások.</span><span class="sxs-lookup"><span data-stu-id="c5f71-164">hello query in this example brings all hello entities that match hello filter criteria.</span></span> <span data-ttu-id="c5f71-165">Ha nagy táblák és toodownload hello táblaentitásokat gyakran kell, azt javasoljuk, hogy tárolja az adatokat az Azure storage blobs helyette.</span><span class="sxs-lookup"><span data-stu-id="c5f71-165">If you have large tables and need toodownload hello table entities often, we recommend that you store your data in Azure storage blobs instead.</span></span>

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="c5f71-166">Partíció entitástartományának lekérése</span><span class="sxs-lookup"><span data-stu-id="c5f71-166">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="c5f71-167">Ha tooquery egy partíció összes hello entitások nem szeretné, megadhat egy tartományt hello partíció kulcs szűrő egy sor szűrőjének kombinálásával.</span><span class="sxs-lookup"><span data-stu-id="c5f71-167">If you don't want tooquery all hello entities in a partition, you can specify a range by combining hello partition key filter with a row key filter.</span></span> <span data-ttu-id="c5f71-168">hello alábbi példakód két szűrő tooget összes entitásának a 'Smith', ahol hello sorkulcs (Keresztnév) rendszernél korábbi hello ábécé "E" betűvel kezdődik, és majd kinyomtatja hello lekérdezési eredmények partíció.</span><span class="sxs-lookup"><span data-stu-id="c5f71-168">hello following code example uses two filters tooget all entities in partition 'Smith' where hello row key (first name) starts with a letter earlier than 'E' in hello alphabet and then prints hello query results.</span></span>  

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

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="c5f71-169">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="c5f71-169">Retrieve a single entity</span></span>
<span data-ttu-id="c5f71-170">A lekérdezés tooretrieve írhat egy adott entitás.</span><span class="sxs-lookup"><span data-stu-id="c5f71-170">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="c5f71-171">hello következő kódban **table_operation::retrieve_entity** toospecify hello ügyfél "Jeff Smith".</span><span class="sxs-lookup"><span data-stu-id="c5f71-171">hello following code uses **table_operation::retrieve_entity** toospecify hello customer 'Jeff Smith'.</span></span> <span data-ttu-id="c5f71-172">Ez a módszer adja vissza egy gyűjtemény helyett csak egyetlen entitást, és hello visszaadott érték **table_result**.</span><span class="sxs-lookup"><span data-stu-id="c5f71-172">This method returns just one entity, rather than a collection, and hello returned value is in **table_result**.</span></span> <span data-ttu-id="c5f71-173">Hello leggyorsabb módon tooretrieve hello Table szolgáltatásból egy entitás partíció-és sorkulcsok megadása a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="c5f71-173">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>  

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

## <a name="replace-an-entity"></a><span data-ttu-id="c5f71-174">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="c5f71-174">Replace an entity</span></span>
<span data-ttu-id="c5f71-175">tooreplace egy entitás lekéréséhez hello Table szolgáltatásból, módosítsa a hello forrásentitás-objektum, és majd hello módosítások mentése toohello Table szolgáltatás biztonsági.</span><span class="sxs-lookup"><span data-stu-id="c5f71-175">tooreplace an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="c5f71-176">hello alábbira vált egy meglévő ügyfél telefonszámát és az e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="c5f71-176">hello following code changes an existing customer's phone number and email address.</span></span> <span data-ttu-id="c5f71-177">Telefonhívás helyett **table_operation::insert_entity**, a kódban **table_operation::replace_entity**.</span><span class="sxs-lookup"><span data-stu-id="c5f71-177">Instead of calling **table_operation::insert_entity**, this code uses **table_operation::replace_entity**.</span></span> <span data-ttu-id="c5f71-178">Ennek hatására hello entitás toobe hello kiszolgálón teljesen lecseréli, kivéve, ha a lekérdezés, amely esetben hello sikertelen lesz hello entitás hello kiszolgálón megváltozott.</span><span class="sxs-lookup"><span data-stu-id="c5f71-178">This causes hello entity toobe fully replaced on hello server, unless hello entity on hello server has changed since it was retrieved, in which case hello operation will fail.</span></span> <span data-ttu-id="c5f71-179">Ez a hiba az alkalmazás ne írhasson felül véletlenül olyan módosítás történt közötti hello lekérését, és frissítse az alkalmazás egy másik összetevő tooprevent.</span><span class="sxs-lookup"><span data-stu-id="c5f71-179">This failure is tooprevent your application from inadvertently overwriting a change made between hello retrieval and update by another component of your application.</span></span> <span data-ttu-id="c5f71-180">hello Ez a hiba megfelelő kezeléséhez tooretrieve hello entitás újra, hajtsa végre a módosításokat (Ha még érvényesek), és ezután hajtson végre egy újabb **table_operation::replace_entity** műveletet.</span><span class="sxs-lookup"><span data-stu-id="c5f71-180">hello proper handling of this failure is tooretrieve hello entity again, make your changes (if still valid), and then perform another **table_operation::replace_entity** operation.</span></span> <span data-ttu-id="c5f71-181">hello a következő szakasz bemutatja, hogyan toooverride ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="c5f71-181">hello next section will show you how toooverride this behavior.</span></span>  

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

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="c5f71-182">Entitás beszúrása vagy lecserélése</span><span class="sxs-lookup"><span data-stu-id="c5f71-182">Insert-or-replace an entity</span></span>
<span data-ttu-id="c5f71-183">**table_operation::replace_entity** műveletek sikertelenek lesznek, ha hello entitás hello kiszolgálóról lekérdezés óta módosult.</span><span class="sxs-lookup"><span data-stu-id="c5f71-183">**table_operation::replace_entity** operations will fail if hello entity has been changed since it was retrieved from hello server.</span></span> <span data-ttu-id="c5f71-184">Ezenkívül be kell olvasni hello entitás ahhoz, hogy először a hello kiszolgálóról **table_operation::replace_entity** toobe sikeres.</span><span class="sxs-lookup"><span data-stu-id="c5f71-184">Furthermore, you must retrieve hello entity from hello server first in order for **table_operation::replace_entity** toobe successful.</span></span> <span data-ttu-id="c5f71-185">Egyes esetekben azonban nem tudja Ha hello entitás létezik-e hello kiszolgálón, és hello a benne tárolt aktuális értékek irrelevánsak – a frissítés mindent felülír.</span><span class="sxs-lookup"><span data-stu-id="c5f71-185">Sometimes, however, you don't know if hello entity exists on hello server and hello current values stored in it are irrelevant—your update should overwrite them all.</span></span> <span data-ttu-id="c5f71-186">tooaccomplish, használhatja a **table_operation::insert_or_replace_entity** műveletet.</span><span class="sxs-lookup"><span data-stu-id="c5f71-186">tooaccomplish this, you would use a **table_operation::insert_or_replace_entity** operation.</span></span> <span data-ttu-id="c5f71-187">Ez a művelet beszúrja hello entitás, ha nem létezik, vagy ha igen, mikor hello utolsó frissítés történt függetlenül lecseréli.</span><span class="sxs-lookup"><span data-stu-id="c5f71-187">This operation inserts hello entity if it doesn't exist, or replaces it if it does, regardless of when hello last update was made.</span></span> <span data-ttu-id="c5f71-188">Az alábbi kódpéldát hello, hello ügyfélentitást Jeff Smith program továbbra is, de azok majd mentésekor hátsó toohello kiszolgálón keresztül **table_operation::insert_or_replace_entity**.</span><span class="sxs-lookup"><span data-stu-id="c5f71-188">In hello following code example, hello customer entity for Jeff Smith is still retrieved, but it is then saved back toohello server via **table_operation::insert_or_replace_entity**.</span></span> <span data-ttu-id="c5f71-189">Toohello entitás hello lekérési és a frissítési művelet között történt összes módosítást felül lesznek írva.</span><span class="sxs-lookup"><span data-stu-id="c5f71-189">Any updates made toohello entity between hello retrieval and update operation will be overwritten.</span></span>  

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

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="c5f71-190">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="c5f71-190">Query a subset of entity properties</span></span>
<span data-ttu-id="c5f71-191">A lekérdezéstábla tooa pár tulajdonságok kérhetnek le egy entitás.</span><span class="sxs-lookup"><span data-stu-id="c5f71-191">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="c5f71-192">hello hello kód a következő lekérdezést használ hello **table_query::set_select_columns** metódus tooreturn csak hello e-mail címek entitások hello táblában.</span><span class="sxs-lookup"><span data-stu-id="c5f71-192">hello query in hello following code uses hello **table_query::set_select_columns** method tooreturn only hello email addresses of entities in hello table.</span></span>  

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
> <span data-ttu-id="c5f71-193">Egy entitás bizonyos tulajdonságait lekérdezése hatékonyabb működését, mint az összes tulajdonság beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c5f71-193">Querying a few properties from an entity is a more efficient operation than retrieving all properties.</span></span>
> 
> 

## <a name="delete-an-entity"></a><span data-ttu-id="c5f71-194">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="c5f71-194">Delete an entity</span></span>
<span data-ttu-id="c5f71-195">Egy entitás azt beolvasása után egyszerűen törölheti.</span><span class="sxs-lookup"><span data-stu-id="c5f71-195">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="c5f71-196">Miután hello entitás beolvasott, hívja **table_operation::delete_entity** hello entitás toodelete együtt.</span><span class="sxs-lookup"><span data-stu-id="c5f71-196">Once hello entity is retrieved, call **table_operation::delete_entity** with hello entity toodelete.</span></span> <span data-ttu-id="c5f71-197">Majd meghívják a hello **cloud_table.execute** metódust.</span><span class="sxs-lookup"><span data-stu-id="c5f71-197">Then call hello **cloud_table.execute** method.</span></span> <span data-ttu-id="c5f71-198">hello alábbi kód lekérdez, majd törli az entitást a partíciós kulcs "Smith" és "Jeff" sor kulcs.</span><span class="sxs-lookup"><span data-stu-id="c5f71-198">hello following code retrieves and deletes an entity with a partition key of "Smith" and a row key of "Jeff".</span></span>  

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

## <a name="delete-a-table"></a><span data-ttu-id="c5f71-199">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="c5f71-199">Delete a table</span></span>
<span data-ttu-id="c5f71-200">Végezetül alábbi kódpéldát hello törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="c5f71-200">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="c5f71-201">A törölt tábla nem érhető el toobe egy adott időn belül hello törlését követően újra létre lesz.</span><span class="sxs-lookup"><span data-stu-id="c5f71-201">A table that has been deleted will be unavailable toobe re-created for a period of time following hello deletion.</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="c5f71-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5f71-202">Next steps</span></span>
<span data-ttu-id="c5f71-203">Most, hogy megismerte a table storage alapjait hello, kövesse az alábbi hivatkozások toolearn Azure Storage-ról további:</span><span class="sxs-lookup"><span data-stu-id="c5f71-203">Now that you've learned hello basics of table storage, follow these links toolearn more about Azure Storage:</span></span>  

* <span data-ttu-id="c5f71-204">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c5f71-204">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* [<span data-ttu-id="c5f71-205">Hogyan toouse Blob storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="c5f71-205">How toouse Blob storage from C++</span></span>](../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="c5f71-206">Hogyan toouse C++ várólista-tárhellyel</span><span class="sxs-lookup"><span data-stu-id="c5f71-206">How toouse Queue storage from C++</span></span>](../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="c5f71-207">A c++ Azure Storage-erőforrások felsorolása</span><span class="sxs-lookup"><span data-stu-id="c5f71-207">List Azure Storage resources in C++</span></span>](../storage/common/storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="c5f71-208">A Storage ügyféloldali kódtára a c++ nyelvhez – dokumentáció</span><span class="sxs-lookup"><span data-stu-id="c5f71-208">Storage Client Library for C++ reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="c5f71-209">Az Azure Storage-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="c5f71-209">Azure Storage documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)