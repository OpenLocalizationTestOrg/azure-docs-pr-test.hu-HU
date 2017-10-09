---
title: "horizontálisan skálázott Azure SQL-adatbázisok aaaQuery |} Microsoft Docs"
description: "Lekérdezések futtatása szilánkok hello elastic database ügyféloldali kódtár segítségével."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2016
ms.author: torsteng
ms.openlocfilehash: a1f0763935a6807b74aa9dec477714e8d117417d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multi-shard-querying"></a>Több shard lekérdezése
## <a name="overview"></a>Áttekintés
A hello [skálázáshoz rugalmas adatbáziseszközöket](sql-database-elastic-scale-introduction.md), szilánkos adatbázis megoldásokat hozhat létre. **Több shard lekérdezése** feladatokhoz, mint például több szilánkok gyűjtemény/jelentő lekérdezés futtatása igénylő átnyúljon használja. (A túl ellentétben[adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md), egyetlen shard összes munkahelyi végzi, amely.) 

1. Első egy [ **RangeShardMap** ](https://msdn.microsoft.com/library/azure/dn807318.aspx) vagy [ **ListShardMap** ](https://msdn.microsoft.com/library/azure/dn807370.aspx) hello segítségével [ **TryGetRangeShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ **TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), vagy hello [ **GetShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metódust. Lásd: [ **hozhat létre egy ShardMapManager** ](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) és [ **egy RangeShardMap vagy ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Hozzon létre egy  **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)**  objektum.
3. Hozzon létre egy  **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
4. Set hello  **[a CommandText tulajdonság](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)**  tooa T-SQL-parancsot.
5. Hello parancs hajtható végre a hívó hello által  **[ExecuteReader metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
6. Hello segítségével hello eredmények megtekintése  **[MultiShardDataReader osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Példa
hello alábbi kód bemutatja lekérdezése használatával több shard hello használatát egy adott **ShardMap** nevű *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                 } 
           } 
    } 


A legfontosabb különbség több shard kapcsolatok hello építése. Ha **SqlConnection** működik egy önálló adatbázis hello **MultiShardConnection** tart egy ***szilánkok gyűjteménye*** bemenetként. A szilánkok leképezés szilánkok hello gyűjteménye feltöltéséhez. hello lekérdezés majd végrehajtása használatával szilánkok hello gyűjteménye **UNION ALL** szemantikáját tooassemble egyetlen teljes eredmény. Nem kötelező, ahol hello sor származik hello shard hello neve felveheti toohello kimeneti hello segítségével **ExecutionOptions** parancs tulajdonságát. 

Vegye figyelembe a hello hívás túl**myShardMap.GetShards()**. Ez a módszer minden szilánkok kikeresi hello shard térkép, és lehetővé teszi egy egyszerűen toorun lekérdezés az összes vonatkozó adatbázis. hello szilánkok gyűjteménye több shard lekérdezés kifinomultabb lehet további LINQ elvégzésével lekérdezése hívás által visszaadott hello túl hello gyűjtemény**myShardMap.GetShards()**. Hello részleges eredmények házirend együtt hello aktuális képességét több shard lekérdezése lett tervezett toowork több másolatot a szilánkok toohundreds esetén.

A többszörös shard lekérdezése korlátozása jelenleg érvényesíteni a következő szilánkok és, hogy a rendszer megkérdezi a shardlets hello hiánya. Amíg az adatok függő útválasztási ellenőrzi, hogy egy adott shard hello shard térkép részét hello lekérdezése során, több shard lekérdezések ne hajtsa végre ezt az ellenőrzést. Ennek eredményeképpen előfordulhat hello shard leképezés eltávolított adatbázisok futó toomulti-shard lekérdezések.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Több shard lekérdezések és a felosztott-merge műveletek
Több shard lekérdezések nem ellenőrizhető, hogy shardlets hello lekérdezett adatbázis részt vegyes-egyesítés folyamatban lévő műveletek. (Lásd: [méretezési hello rugalmas adatbázis vegyes egyesítéses eszközzel](sql-database-elastic-scale-overview-split-and-merge.md).) Ennek eredményeképpen előfordulhat tooinconsistencies ahol sorát hello a hello több adatbázis ugyanazon shardlet megjelenítése ugyanazt a shard több a lekérdezést. Ezek a korlátozások figyelembe vennie, és fontolja meg a folyamatban lévő megosztott egyesítéses és módosításait toohello shard térkép kifuttatja több shard lekérdezések végrehajtása közben.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Lásd még:
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)**  osztályok és metódusok.

Hello segítségével szilánkok kezelése [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md). A névtér neve tartalmazza [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) hello képességét tooquery egyetlen lekérdezést és eredménye több szilánkok biztosít. A lekérdező absztrakciós biztosítanak szilánkok gyűjteménye. Emellett biztosítja az alternatív végrehajtási házirendek, különösen részleges eredményt, toodeal hibákkal keresztül sok szilánkok lekérdezésekor.  

