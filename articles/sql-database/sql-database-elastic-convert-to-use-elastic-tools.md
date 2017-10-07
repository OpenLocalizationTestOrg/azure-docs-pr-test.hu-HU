---
title: "aaaMigrate meglévő adatbázisok tooscale kibővített |} Microsoft Docs"
description: "Hozzon létre egy shard térkép manager szilánkos adatbázisok toouse rugalmas adatbáziseszközöket átalakítása"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: fa2c9e3699f30667cf547d1faadf4504609199be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-existing-databases-tooscale-out"></a>Telepítse át a meglévő adatbázisok tooscale kimenő
Könnyedén felügyelheti a meglévő szilánkos kiterjesztett adatbázisok Azure SQL Database-adatbázis eszközökkel (például hello [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md)). Először alakítsa át egy meglévő adatbázisok toouse hello-készletből [shard térkép manager](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Áttekintés
meglévő szilánkos adatbázis toomigrate: 

1. Készítse elő a hello [shard manager adatbázist](sql-database-elastic-scale-shard-map-management.md).
2. Hello shard térkép létrehozásához.
3. Készítse elő a hello egyedi szilánkok.  
4. Hozzárendelések toohello shard-társítás hozzáadása.

Ezek a technológiák vagy hello valósítható [.NET-keretrendszer ügyféloldali kódtár](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), vagy a PowerShell-parancsfájlok hello a következő címen található [Azure SQL Database - rugalmas adatbázis eszközök parancsfájlok](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). Itt hello példák hello PowerShell-parancsfájlok használata.

Hello ShardMapManager kapcsolatos további információkért lásd: [Shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md). Hello rugalmas adatbáziseszközöket áttekintését lásd: [rugalmas adatbázis-szolgáltatások áttekintése](sql-database-elastic-scale-introduction.md).

## <a name="prepare-hello-shard-map-manager-database"></a>Hello shard térkép manager-adatbázis előkészítése
hello shard térkép manager különleges adatbázis hello adatok toomanage kiterjesztett adatbázisokat tartalmazza. Használjon egy meglévő adatbázist, vagy hozzon létre egy új adatbázist. Vegye figyelembe, hogy egy adatbázis forráscsoportként shard térkép manager hello nem lehet azonos adatbázis, a szilánkcímtárban. Ne feledje, hogy hello PowerShell-parancsfájl nem hoz létre hello adatbázis meg. 

## <a name="step-1-create-a-shard-map-manager"></a>1. lépés: hozzon létre egy shard térkép manager
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are hello server name and database name 
    # for hello new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="tooretrieve-hello-shard-map-manager"></a>tooretrieve hello shard térkép manager
A létrehozás után hello shard térkép manager ezzel a parancsmaggal kérheti le. Ez a lépés akkor szükséges, minden alkalommal, amikor toouse hello ShardMapManager objektumot kell.

    # Try tooget a reference toohello Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-hello-shard-map"></a>2. lépés: hello shard térkép létrehozásához.
Választania kell a shard térkép toocreate hello típusú. hello választáshoz hello adatbázis-architektúra: 

1. Adatbázisonként egybérlős (feltételei, lásd: hello [szószedet](sql-database-elastic-scale-glossary.md).) 
2. Több bérlő adatbázisonként (két típus):
   1. Lista leképezése
   2. Tartomány leképezése

Single-bérlő modell, hozzon létre egy **lista leképezési** shard leképezés. hello single-bérlős modell bérlőnként egy adatbázis rendeli hozzá. Ez megegyezik egy hatékony modell Szolgáltatottszoftver-fejlesztőknek egyszerűbbé teszi a felügyeleti.

![Lista leképezése][1]

hello több-bérlős modell rendeli hozzá több bérlő tooa egyetlen adatbázis (és a bérlő csoportok szét több adatbázis). Ezt a modellt használja, ha várhatóan mindegyik bérlő toohave kis adatokat. Ez a modell azt rendelje hozzá a bérlők tooa adatbázis használatával széles **tartomány leképezése**. 

![Tartomány leképezése][2]

Megvalósíthat egy adatbázis több-bérlős modell használatával vagy egy *lista leképezési* tooassign egy adatbázis több bérlő tooa. Például D1 bérlőazonosító 1 és 5 használt toostore információt, és DB2 eltárolja 7 bérlői és bérlői 10. 

![Az egyetlen DB Muliple bérlők][3] 

**A kiválasztott alapján, válassza ki az alábbi lehetőségek egyikét:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>1. lehetőség: a lista hozzárendelés shard térkép létrehozásához.
A hello ShardMapManager objektummal shard térkép létrehozásához. 

    # $ShardMapManager is hello shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>2. lehetőség: a tartomány hozzárendelés shard térkép létrehozásához.
Vegye figyelembe, hogy tooutilize ebben a leképezési mintában, bérlői azonosító értékekkel kell toobe folyamatos tartományokat, és elfogadható toohave résnek hello tartományok átugrásával egyszerűen hello tartomány hello adatbázisok létrehozásakor.

    # $ShardMapManager is hello shard map manager object 
    # 'RangeShardMap' is hello unique identifier for hello range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>3. lehetőség: Lista leképezése egy önálló adatbázis
A lista leképezés létrehozása is ebben a mintában beállítása igényel, ahogy az 1. lehetőség 2. lépést.

## <a name="step-3-prepare-individual-shards"></a>3. lépés: Felkészülés az egyes szilánkok
Minden egyes shard (adatbázis) toohello shard térkép-kezelő hozzáadása. Ez felkészíti hello egyedi adatbázisok identitásleképezési információk tárolására. Ez a módszer minden shard hajtható végre.

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # hello $ShardMap is hello shard map created in step 2.


## <a name="step-4-add-mappings"></a>4. lépés: Hozzárendelések hozzáadása
hozzárendelések hello hozzáadása a shard térkép létrehozott hello típusú függ. Ha létrehozott egy lista térkép, hozzá kell adnia hozzárendelések listáját. Ha létrehozott egy tartomány-társítást, hozzá kell adnia tartomány leképezéseket.

### <a name="option-1-map-hello-data-for-a-list-mapping"></a>1. lehetőség: a lista hozzárendelés hello adatok leképezése
Hello adatok leképezése egy lista leképezése hozzáadásával az egyes bérlők számára.  

    # Create hello mappings and associate it with hello new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-hello-data-for-a-range-mapping"></a>2. lehetőség: a tartomány hozzárendelés hello adatok leképezése
Minden hello bérlői azonosító tartomány - adatbázis társítások leképezéseit hello tartomány hozzáadása:

    # Create hello mappings and associate it with hello new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-hello-data-for-multiple-tenants-on-a-single-database"></a>4. lépés: 3. lehetőség: egy adatbázist a több bérlő hello adatok leképezése
Az egyes bérlők, futtassa az Add-ListMapping hello (1, lehetőséget fent). 

## <a name="checking-hello-mappings"></a>Hello hozzárendelések ellenőrzése
Hello meglévő szilánkok és a hozzájuk társított runbookokat hello hozzárendelések kapcsolatos információk lekérdezhetők, következő parancsok használatával:  

    # List hello shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Összefoglalás
Hello telepítő befejezése után megkezdheti a toouse hello Elastic Database ügyféloldali kódtárára. Is [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md) és [több shard lekérdezés](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Következő lépések
PowerShell-parancsfájlok hello az beszerzése [Azure SQL DB-rugalmas adatbázis eszközök sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

hello eszközök megtalálhatók a Githubon: [Azure/rugalmas-db-eszközök](https://github.com/Azure/elastic-db-tools).

Hello vegyes egyesítéses eszköz toomove adatok tooor egy több-bérlős modell tooa egybérlős modellből használja. Lásd: [vegyes egyesítő eszköz](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>További források
A több bérlős szoftverszolgáltatás (SaaS) típusú adatbázis-alkalmazások általános adatarchitektúra-mintázataival kapcsolatos információk: [Tervminták több-bérlős SaaS-alkalmazásokhoz Azure SQL Database esetén](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Kérdések és funkciókra vonatkozó kérések
A kérdésekhez, lépjen kapcsolatba a hello toous [SQL-adatbázis fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a funkciókra vonatkozó kérések, adjon hozzá toohello [SQL adatbázis-visszajelzési fórumon](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

