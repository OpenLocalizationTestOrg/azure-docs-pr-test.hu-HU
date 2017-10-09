---
title: "aaaUse mongoimport és a mongorestore hello Azure Cosmos DB API MongoDB |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse mongoimport és mongorestore tooimport adatok tooan API MongoDB-fiók"
keywords: mongoimport, mongorestore
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 921354bc7b09a076a73e0cbf5e4aabcc9e83d5a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-import-mongodb-data"></a><span data-ttu-id="2e655-104">Az Azure Cosmos DB: Import MongoDB adatok</span><span class="sxs-lookup"><span data-stu-id="2e655-104">Azure Cosmos DB: Import MongoDB data</span></span> 

<span data-ttu-id="2e655-105">adatok toomigrate MongoDB tooan Azure Cosmos DB fiók mongodb hello API való használatra kell:</span><span class="sxs-lookup"><span data-stu-id="2e655-105">toomigrate data from MongoDB tooan Azure Cosmos DB account for use with hello API for MongoDB, you must:</span></span>

* <span data-ttu-id="2e655-106">Töltse le vagy *mongoimport.exe* vagy *mongorestore.exe* a hello [MongoDB letöltőközpontból](https://www.mongodb.com/download-center).</span><span class="sxs-lookup"><span data-stu-id="2e655-106">Download either *mongoimport.exe* or *mongorestore.exe* from hello [MongoDB Download Center](https://www.mongodb.com/download-center).</span></span>
* <span data-ttu-id="2e655-107">Beolvasása a [API-t a MongoDB-kapcsolati karakterlánc](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="2e655-107">Get your [API for MongoDB connection string](connect-mongodb-account.md).</span></span>

<span data-ttu-id="2e655-108">Adatok importálása a MongoDB és a terv toouse azt hello Azure Cosmos DB, használjon hello [adatáttelepítési eszközét](import-data.md) tooimport adatokat.</span><span class="sxs-lookup"><span data-stu-id="2e655-108">If you are importing data from MongoDB and plan toouse it with hello Azure Cosmos DB, you should use hello [Data Migration tool](import-data.md) tooimport data.</span></span>

<span data-ttu-id="2e655-109">Ez az oktatóanyag ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="2e655-109">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2e655-110">A kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="2e655-110">Retrieving your connection string</span></span>
> * <span data-ttu-id="2e655-111">MongoDB-adatok importálása mongoimport használatával</span><span class="sxs-lookup"><span data-stu-id="2e655-111">Importing MongoDB data by using mongoimport</span></span>
> * <span data-ttu-id="2e655-112">MongoDB-adatok importálása mongorestore használatával</span><span class="sxs-lookup"><span data-stu-id="2e655-112">Importing MongoDB data by using mongorestore</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e655-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2e655-113">Prerequisites</span></span>

* <span data-ttu-id="2e655-114">Átviteli sebesség növelése: az adatáttelepítés hello időtartama állít be a gyűjteményekben átviteli hello mennyisége függ.</span><span class="sxs-lookup"><span data-stu-id="2e655-114">Increase throughput: hello duration of your data migration depends on hello amount of throughput you set up for your collections.</span></span> <span data-ttu-id="2e655-115">Lehet, hogy tooincrease hello átviteli nagyobb adatok áttelepítésre.</span><span class="sxs-lookup"><span data-stu-id="2e655-115">Be sure tooincrease hello throughput for larger data migrations.</span></span> <span data-ttu-id="2e655-116">Hello áttelepítés befejezése után csökkentheti a hello átviteli toosave költségeket.</span><span class="sxs-lookup"><span data-stu-id="2e655-116">After you've completed hello migration, decrease hello throughput toosave costs.</span></span> <span data-ttu-id="2e655-117">További információ a hello növelésében [Azure-portálon](https://portal.azure.com), lásd: [teljesítményszintet és az Azure Cosmos Adatbázisba tarifacsomagok](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="2e655-117">For more information about increasing throughput in hello [Azure portal](https://portal.azure.com), see [Performance levels and pricing tiers in Azure Cosmos DB](performance-levels.md).</span></span>

* <span data-ttu-id="2e655-118">SSL engedélyezése: Azure Cosmos-adatbázis a szigorú biztonsági követelmények és szabványok tartoznak.</span><span class="sxs-lookup"><span data-stu-id="2e655-118">Enable SSL: Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="2e655-119">Lehet, hogy tooenable SSL fiókja folytatott kommunikáció során.</span><span class="sxs-lookup"><span data-stu-id="2e655-119">Be sure tooenable SSL when you interact with your account.</span></span> <span data-ttu-id="2e655-120">hello eljárások hello cikk többi hello hogyan tooenable SSL mongoimport és mongorestore.</span><span class="sxs-lookup"><span data-stu-id="2e655-120">hello procedures in hello rest of hello article include how tooenable SSL for mongoimport and mongorestore.</span></span>

## <a name="find-your-connection-string-information-host-port-username-and-password"></a><span data-ttu-id="2e655-121">Található kapcsolati karakterlánc adatainak (host, port, felhasználónév és jelszó)</span><span class="sxs-lookup"><span data-stu-id="2e655-121">Find your connection string information (host, port, username, and password)</span></span>

1. <span data-ttu-id="2e655-122">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali panelen, kattintson a hello **Azure Cosmos DB** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="2e655-122">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Cosmos DB** entry.</span></span>
2. <span data-ttu-id="2e655-123">A hello **előfizetések** ablaktáblán válassza ki a fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="2e655-123">In hello **Subscriptions** pane, select your account name.</span></span>
3. <span data-ttu-id="2e655-124">A hello **kapcsolati karakterlánc** panelen kattintson a **kapcsolati karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="2e655-124">In hello **Connection String** blade, click **Connection String**.</span></span>  
<span data-ttu-id="2e655-125">jobb oldali ablaktábla tartalmaz minden hello információt, hogy kell-e toosuccessfully hello tooyour fiók csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="2e655-125">hello right pane contains all hello information that you need toosuccessfully connect tooyour account.</span></span>

    ![Kapcsolati karakterlánc panel](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-toohello-api-for-mongodb-by-using-mongoimport"></a><span data-ttu-id="2e655-127">Adatok toohello API importálása mongodb mongoimport használatával</span><span class="sxs-lookup"><span data-stu-id="2e655-127">Import data toohello API for MongoDB by using mongoimport</span></span>

<span data-ttu-id="2e655-128">tooimport adatok tooyour Azure Cosmos DB fiókot, használja a következő sablon hello.</span><span class="sxs-lookup"><span data-stu-id="2e655-128">tooimport data tooyour Azure Cosmos DB account, use hello following template.</span></span> <span data-ttu-id="2e655-129">Töltse ki *állomás*, *felhasználónév*, és *jelszó* , amelyek adott tooyour fiók hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="2e655-129">Fill in *host*, *username*, and *password* with hello values that are specific tooyour account.</span></span>  

<span data-ttu-id="2e655-130">Sablon:</span><span class="sxs-lookup"><span data-stu-id="2e655-130">Template:</span></span>

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

<span data-ttu-id="2e655-131">Példa:</span><span class="sxs-lookup"><span data-stu-id="2e655-131">Example:</span></span>  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-toohello-api-for-mongodb-by-using-mongorestore"></a><span data-ttu-id="2e655-132">Adatok toohello API importálása mongodb mongorestore használatával</span><span class="sxs-lookup"><span data-stu-id="2e655-132">Import data toohello API for MongoDB by using mongorestore</span></span>

<span data-ttu-id="2e655-133">toorestore adatok tooyour API MongoDB fiók Szolgáltatássablon tooexecute hello importálása a következő hello használja.</span><span class="sxs-lookup"><span data-stu-id="2e655-133">toorestore data tooyour API for MongoDB account, use hello following template tooexecute hello import.</span></span> <span data-ttu-id="2e655-134">Töltse ki *állomás*, *felhasználónév*, és *jelszó* , amelyek adott tooyour fiók hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="2e655-134">Fill in *host*, *username*, and *password* with hello values that are specific tooyour account.</span></span>

<span data-ttu-id="2e655-135">Sablon:</span><span class="sxs-lookup"><span data-stu-id="2e655-135">Template:</span></span>

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

<span data-ttu-id="2e655-136">Példa:</span><span class="sxs-lookup"><span data-stu-id="2e655-136">Example:</span></span>

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a><span data-ttu-id="2e655-137">A sikeres áttelepítéshez a útmutatója</span><span class="sxs-lookup"><span data-stu-id="2e655-137">Guide for a successful migration</span></span>

1. <span data-ttu-id="2e655-138">Előre létrehozni, illetve skálázni a gyűjtemények:</span><span class="sxs-lookup"><span data-stu-id="2e655-138">Pre-create and scale your collections:</span></span>
        
    * <span data-ttu-id="2e655-139">Alapértelmezés szerint Azure Cosmos DB látja el egy új MongoDB gyűjtemény 1000 kérelem egységek használatával (RUs).</span><span class="sxs-lookup"><span data-stu-id="2e655-139">By default, Azure Cosmos DB provisions a new MongoDB collection with 1,000 request units (RUs).</span></span> <span data-ttu-id="2e655-140">A kezdéshez hello áttelepítési mongoimport, mongorestore vagy mongomirror előzetes létrehozása hello a összes gyűjteményt [Azure-portálon](https://portal.azure.com) vagy a MongoDB-illesztőprogramok és eszközök.</span><span class="sxs-lookup"><span data-stu-id="2e655-140">Before you start hello migration by using mongoimport, mongorestore, or mongomirror, pre-create all your collections from hello [Azure portal](https://portal.azure.com) or from MongoDB drivers and tools.</span></span> <span data-ttu-id="2e655-141">Ha a gyűjtemény 10 GB-nál nagyobb, győződjön meg arról, hogy toocreate egy [szilánkos/particionált gyűjtemény](partition-data.md) megfelelő shard kulccsal.</span><span class="sxs-lookup"><span data-stu-id="2e655-141">If your collection is greater than 10 GB, make sure toocreate a [sharded/partitioned collection](partition-data.md) with an appropriate shard key.</span></span>

    * <span data-ttu-id="2e655-142">A hello [Azure-portálon](https://portal.azure.com), növelheti a gyűjtemények teljesítményt az egypartíciós gyűjtemény 1000 RUs és 2500 RUs szilánkos gyűjtemény csak hello áttelepítésre.</span><span class="sxs-lookup"><span data-stu-id="2e655-142">From hello [Azure portal](https://portal.azure.com), increase your collections' throughput from 1,000 RUs for a single partition collection and 2,500 RUs for a sharded collection just for hello migration.</span></span> <span data-ttu-id="2e655-143">Hello magasabb átviteli elkerülheti a sávszélesség-szabályozás, és kevesebb idő alatt át.</span><span class="sxs-lookup"><span data-stu-id="2e655-143">With hello higher throughput, you can avoid throttling and migrate in less time.</span></span> <span data-ttu-id="2e655-144">Óránkénti számlázási Azure Cosmos DB, hello átviteli hello áttelepítési toosave költségek után azonnal csökkentheti.</span><span class="sxs-lookup"><span data-stu-id="2e655-144">With hourly billing in Azure Cosmos DB, you can reduce hello throughput immediately after hello migration toosave costs.</span></span>

2. <span data-ttu-id="2e655-145">Egyetlen dokumentum írási hozzávetőleges hello RU kell fizetni számítása:</span><span class="sxs-lookup"><span data-stu-id="2e655-145">Calculate hello approximate RU charge for a single document write:</span></span>

    <span data-ttu-id="2e655-146">a.</span><span class="sxs-lookup"><span data-stu-id="2e655-146">a.</span></span> <span data-ttu-id="2e655-147">Csatlakoztassa hello MongoDB rendszerhéj tooyour Azure Cosmos DB MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="2e655-147">Connect tooyour Azure Cosmos DB MongoDB database from hello MongoDB Shell.</span></span> <span data-ttu-id="2e655-148">Utasításait található [csatlakozzon a MongoDB-alkalmazás tooAzure Cosmos DB](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="2e655-148">You can find instructions in [Connect a MongoDB application tooAzure Cosmos DB](connect-mongodb-account.md).</span></span>
    
    <span data-ttu-id="2e655-149">b.</span><span class="sxs-lookup"><span data-stu-id="2e655-149">b.</span></span> <span data-ttu-id="2e655-150">A minta insert parancs futtatása a minta dokumentumait hello MongoDB rendszerhéj egyikének használatával:</span><span class="sxs-lookup"><span data-stu-id="2e655-150">Run a sample insert command by using one of your sample documents from hello MongoDB Shell:</span></span>
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    <span data-ttu-id="2e655-151">c.</span><span class="sxs-lookup"><span data-stu-id="2e655-151">c.</span></span> <span data-ttu-id="2e655-152">Futtatás ```db.runCommand({getLastRequestStatistics: 1})``` és a jelen szoftverhez hasonló választ kap:</span><span class="sxs-lookup"><span data-stu-id="2e655-152">Run ```db.runCommand({getLastRequestStatistics: 1})``` and you will receive a response like this one:</span></span>
     
        ```
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
        ```
        
    <span data-ttu-id="2e655-153">d.</span><span class="sxs-lookup"><span data-stu-id="2e655-153">d.</span></span> <span data-ttu-id="2e655-154">Jegyezze fel a hello kérelem kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="2e655-154">Take note of hello request charge.</span></span>
    
3. <span data-ttu-id="2e655-155">Hello várakozási ideje a gép toohello Azure Cosmos DB cloud service az határozza meg:</span><span class="sxs-lookup"><span data-stu-id="2e655-155">Determine hello latency from your machine toohello Azure Cosmos DB cloud service:</span></span>
    
    <span data-ttu-id="2e655-156">a.</span><span class="sxs-lookup"><span data-stu-id="2e655-156">a.</span></span> <span data-ttu-id="2e655-157">A MongoDB rendszerhéj hello részletes naplózás engedélyezése a parancs segítségével:```setVerboseShell(true)```</span><span class="sxs-lookup"><span data-stu-id="2e655-157">Enable verbose logging from hello MongoDB Shell by using this command: ```setVerboseShell(true)```</span></span>
    
    <span data-ttu-id="2e655-158">b.</span><span class="sxs-lookup"><span data-stu-id="2e655-158">b.</span></span> <span data-ttu-id="2e655-159">Egy egyszerű lekérdezést futtassa hello adatbázison: ```db.coll.find().limit(1)```.</span><span class="sxs-lookup"><span data-stu-id="2e655-159">Run a simple query against hello database: ```db.coll.find().limit(1)```.</span></span> <span data-ttu-id="2e655-160">Az alábbihoz hasonló választ kap:</span><span class="sxs-lookup"><span data-stu-id="2e655-160">You will receive a response like this one:</span></span>

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. <span data-ttu-id="2e655-161">Szúrja be hello dokumentum előtt, hogy vannak-e ismétlődő dokumentumok hello áttelepítési tooensure eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="2e655-161">Remove hello inserted document before hello migration tooensure that there are no duplicate documents.</span></span> <span data-ttu-id="2e655-162">Dokumentumok eltávolításához használja a következő parancsot:```db.coll.remove({})```</span><span class="sxs-lookup"><span data-stu-id="2e655-162">You can remove documents by using this command: ```db.coll.remove({})```</span></span>

5. <span data-ttu-id="2e655-163">Hozzávetőleges hello kiszámításához *batchSize* és *numInsertionWorkers* értékeket:</span><span class="sxs-lookup"><span data-stu-id="2e655-163">Calculate hello approximate *batchSize* and *numInsertionWorkers* values:</span></span>

    * <span data-ttu-id="2e655-164">A *batchSize*, osztási hello összesen által felhasznált a 3. lépésben az egyetlen dokumentum írása a hello RUs RUs kiépítve.</span><span class="sxs-lookup"><span data-stu-id="2e655-164">For *batchSize*, divide hello total provisioned RUs by hello RUs consumed from your single document write in step 3.</span></span>
    
    * <span data-ttu-id="2e655-165">Ha hello számított *batchSize* < = 24, használja ezt a számot, mint a *batchSize* érték.</span><span class="sxs-lookup"><span data-stu-id="2e655-165">If hello calculated *batchSize* <= 24, use that number as your *batchSize* value.</span></span>
    
    * <span data-ttu-id="2e655-166">Ha hello számított *batchSize* > 24, a set hello *batchSize* érték too24.</span><span class="sxs-lookup"><span data-stu-id="2e655-166">If hello calculated *batchSize* > 24, set hello *batchSize* value too24.</span></span>
    
    * <span data-ttu-id="2e655-167">A *numInsertionWorkers*, használja a egyenlet: *numInsertionWorkers = (kiosztott átviteli sebesség * késleltetés másodpercben) / (kötegméret * RUs felhasznált egyetlen írási)*.</span><span class="sxs-lookup"><span data-stu-id="2e655-167">For *numInsertionWorkers*, use this equation:   *numInsertionWorkers =  (provisioned throughput * latency in seconds) / (batch size * consumed RUs for a single write)*.</span></span>
        
    |<span data-ttu-id="2e655-168">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2e655-168">Property</span></span>|<span data-ttu-id="2e655-169">Érték</span><span class="sxs-lookup"><span data-stu-id="2e655-169">Value</span></span>|
    |--------|-----|
    |<span data-ttu-id="2e655-170">batchSize</span><span class="sxs-lookup"><span data-stu-id="2e655-170">batchSize</span></span>| <span data-ttu-id="2e655-171">24</span><span class="sxs-lookup"><span data-stu-id="2e655-171">24</span></span> |
    |<span data-ttu-id="2e655-172">Kiépített RUs</span><span class="sxs-lookup"><span data-stu-id="2e655-172">RUs provisioned</span></span> | <span data-ttu-id="2e655-173">10000</span><span class="sxs-lookup"><span data-stu-id="2e655-173">10000</span></span> |
    |<span data-ttu-id="2e655-174">Késés</span><span class="sxs-lookup"><span data-stu-id="2e655-174">Latency</span></span> | <span data-ttu-id="2e655-175">0.100 s</span><span class="sxs-lookup"><span data-stu-id="2e655-175">0.100 s</span></span> |
    |<span data-ttu-id="2e655-176">RU felszámított 1 doc írásra</span><span class="sxs-lookup"><span data-stu-id="2e655-176">RU charged for 1 doc write</span></span> | <span data-ttu-id="2e655-177">10 RUs</span><span class="sxs-lookup"><span data-stu-id="2e655-177">10 RUs</span></span> |
    |<span data-ttu-id="2e655-178">numInsertionWorkers</span><span class="sxs-lookup"><span data-stu-id="2e655-178">numInsertionWorkers</span></span> | <span data-ttu-id="2e655-179">?</span><span class="sxs-lookup"><span data-stu-id="2e655-179">?</span></span> |
    
    <span data-ttu-id="2e655-180">*numInsertionWorkers = (10000 RUs x 0,1 s) / (24 darab 10 RUs) = 4.1666*</span><span class="sxs-lookup"><span data-stu-id="2e655-180">*numInsertionWorkers = (10000 RUs x 0.1 s) / (24 x 10 RUs) = 4.1666*</span></span>

6. <span data-ttu-id="2e655-181">Hello végső áttelepítési parancsot:</span><span class="sxs-lookup"><span data-stu-id="2e655-181">Run hello final migration command:</span></span>

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a><span data-ttu-id="2e655-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e655-182">Next steps</span></span>

<span data-ttu-id="2e655-183">Folytassa a következő oktatóanyag toohello, és megtudhatja, hogyan tooquery MongoDB adatok Azure Cosmos DB használatával.</span><span class="sxs-lookup"><span data-stu-id="2e655-183">You can proceed toohello next tutorial and learn how tooquery MongoDB data by using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="2e655-184">Hogyan tooquery MongoDB adatokat?</span><span class="sxs-lookup"><span data-stu-id="2e655-184">How tooquery MongoDB data?</span></span>](../cosmos-db/tutorial-query-mongodb.md)
