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
# <a name="azure-cosmos-db-import-mongodb-data"></a>Az Azure Cosmos DB: Import MongoDB adatok 

adatok toomigrate MongoDB tooan Azure Cosmos DB fiók mongodb hello API való használatra kell:

* Töltse le vagy *mongoimport.exe* vagy *mongorestore.exe* a hello [MongoDB letöltőközpontból](https://www.mongodb.com/download-center).
* Beolvasása a [API-t a MongoDB-kapcsolati karakterlánc](connect-mongodb-account.md).

Adatok importálása a MongoDB és a terv toouse azt hello Azure Cosmos DB, használjon hello [adatáttelepítési eszközét](import-data.md) tooimport adatokat.

Ez az oktatóanyag ismerteti a következő feladatok hello:

> [!div class="checklist"]
> * A kapcsolati karakterlánc beolvasása
> * MongoDB-adatok importálása mongoimport használatával
> * MongoDB-adatok importálása mongorestore használatával

## <a name="prerequisites"></a>Előfeltételek

* Átviteli sebesség növelése: az adatáttelepítés hello időtartama állít be a gyűjteményekben átviteli hello mennyisége függ. Lehet, hogy tooincrease hello átviteli nagyobb adatok áttelepítésre. Hello áttelepítés befejezése után csökkentheti a hello átviteli toosave költségeket. További információ a hello növelésében [Azure-portálon](https://portal.azure.com), lásd: [teljesítményszintet és az Azure Cosmos Adatbázisba tarifacsomagok](performance-levels.md).

* SSL engedélyezése: Azure Cosmos-adatbázis a szigorú biztonsági követelmények és szabványok tartoznak. Lehet, hogy tooenable SSL fiókja folytatott kommunikáció során. hello eljárások hello cikk többi hello hogyan tooenable SSL mongoimport és mongorestore.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Található kapcsolati karakterlánc adatainak (host, port, felhasználónév és jelszó)

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali panelen, kattintson a hello **Azure Cosmos DB** bejegyzés.
2. A hello **előfizetések** ablaktáblán válassza ki a fiók nevét.
3. A hello **kapcsolati karakterlánc** panelen kattintson a **kapcsolati karakterlánc**.  
jobb oldali ablaktábla tartalmaz minden hello információt, hogy kell-e toosuccessfully hello tooyour fiók csatlakozzon.

    ![Kapcsolati karakterlánc panel](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-toohello-api-for-mongodb-by-using-mongoimport"></a>Adatok toohello API importálása mongodb mongoimport használatával

tooimport adatok tooyour Azure Cosmos DB fiókot, használja a következő sablon hello. Töltse ki *állomás*, *felhasználónév*, és *jelszó* , amelyek adott tooyour fiók hello értékekkel.  

Sablon:

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Példa:  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-toohello-api-for-mongodb-by-using-mongorestore"></a>Adatok toohello API importálása mongodb mongorestore használatával

toorestore adatok tooyour API MongoDB fiók Szolgáltatássablon tooexecute hello importálása a következő hello használja. Töltse ki *állomás*, *felhasználónév*, és *jelszó* , amelyek adott tooyour fiók hello értékekkel.

Sablon:

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Példa:

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a>A sikeres áttelepítéshez a útmutatója

1. Előre létrehozni, illetve skálázni a gyűjtemények:
        
    * Alapértelmezés szerint Azure Cosmos DB látja el egy új MongoDB gyűjtemény 1000 kérelem egységek használatával (RUs). A kezdéshez hello áttelepítési mongoimport, mongorestore vagy mongomirror előzetes létrehozása hello a összes gyűjteményt [Azure-portálon](https://portal.azure.com) vagy a MongoDB-illesztőprogramok és eszközök. Ha a gyűjtemény 10 GB-nál nagyobb, győződjön meg arról, hogy toocreate egy [szilánkos/particionált gyűjtemény](partition-data.md) megfelelő shard kulccsal.

    * A hello [Azure-portálon](https://portal.azure.com), növelheti a gyűjtemények teljesítményt az egypartíciós gyűjtemény 1000 RUs és 2500 RUs szilánkos gyűjtemény csak hello áttelepítésre. Hello magasabb átviteli elkerülheti a sávszélesség-szabályozás, és kevesebb idő alatt át. Óránkénti számlázási Azure Cosmos DB, hello átviteli hello áttelepítési toosave költségek után azonnal csökkentheti.

2. Egyetlen dokumentum írási hozzávetőleges hello RU kell fizetni számítása:

    a. Csatlakoztassa hello MongoDB rendszerhéj tooyour Azure Cosmos DB MongoDB-adatbázist. Utasításait található [csatlakozzon a MongoDB-alkalmazás tooAzure Cosmos DB](connect-mongodb-account.md).
    
    b. A minta insert parancs futtatása a minta dokumentumait hello MongoDB rendszerhéj egyikének használatával:
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    c. Futtatás ```db.runCommand({getLastRequestStatistics: 1})``` és a jelen szoftverhez hasonló választ kap:
     
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
        
    d. Jegyezze fel a hello kérelem kell fizetni.
    
3. Hello várakozási ideje a gép toohello Azure Cosmos DB cloud service az határozza meg:
    
    a. A MongoDB rendszerhéj hello részletes naplózás engedélyezése a parancs segítségével:```setVerboseShell(true)```
    
    b. Egy egyszerű lekérdezést futtassa hello adatbázison: ```db.coll.find().limit(1)```. Az alábbihoz hasonló választ kap:

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. Szúrja be hello dokumentum előtt, hogy vannak-e ismétlődő dokumentumok hello áttelepítési tooensure eltávolítása. Dokumentumok eltávolításához használja a következő parancsot:```db.coll.remove({})```

5. Hozzávetőleges hello kiszámításához *batchSize* és *numInsertionWorkers* értékeket:

    * A *batchSize*, osztási hello összesen által felhasznált a 3. lépésben az egyetlen dokumentum írása a hello RUs RUs kiépítve.
    
    * Ha hello számított *batchSize* < = 24, használja ezt a számot, mint a *batchSize* érték.
    
    * Ha hello számított *batchSize* > 24, a set hello *batchSize* érték too24.
    
    * A *numInsertionWorkers*, használja a egyenlet: *numInsertionWorkers = (kiosztott átviteli sebesség * késleltetés másodpercben) / (kötegméret * RUs felhasznált egyetlen írási)*.
        
    |Tulajdonság|Érték|
    |--------|-----|
    |batchSize| 24 |
    |Kiépített RUs | 10000 |
    |Késés | 0.100 s |
    |RU felszámított 1 doc írásra | 10 RUs |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10000 RUs x 0,1 s) / (24 darab 10 RUs) = 4.1666*

6. Hello végső áttelepítési parancsot:

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a>Következő lépések

Folytassa a következő oktatóanyag toohello, és megtudhatja, hogyan tooquery MongoDB adatok Azure Cosmos DB használatával. 

> [!div class="nextstepaction"]
>[Hogyan tooquery MongoDB adatokat?](../cosmos-db/tutorial-query-mongodb.md)
