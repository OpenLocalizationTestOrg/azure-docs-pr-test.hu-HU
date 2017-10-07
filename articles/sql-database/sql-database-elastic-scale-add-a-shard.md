---
title: "a rugalmas adatbázis-eszközökkel shard aaaAdding |} Microsoft Docs"
description: "Rugalmas, méretezhető API-k tooadd új szilánkok tooa shard toouse beállításának módját."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f44b59578376d1238b3012a3cb52339978079f0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>A rugalmas adatbázis-eszközökkel szilánkcímtárban hozzáadása
## <a name="tooadd-a-shard-for-a-new-range-or-key"></a>a szilánkok egy új tartományt vagy a kulcs tooadd
Alkalmazások gyakran kell toosimply új szilánkok toohandle adatok hozzáadása új kulcsokat vagy kulcstartományokkal, várt shard térképre már létezik. Például a bérlő-azonosító szerint szilánkos alkalmazás egy új shard tooprovision szükséges egy új bérlőt, vagy adatok szilánkos havi esetleg egy új shard kiépítése előtt minden új hónap elején hello. 

Hello új kulcs értékek tartománya nem már tartozik egy meglévő leképezést, akkor nagyon egyszerű tooadd hello új shard és társítás hello új kulcs vagy a tartomány toothat szilánkcímtárban. 

### <a name="example--adding-a-shard-and-its-range-tooan-existing-shard-map"></a>Példa: egy shard és a tartomány tooan meglévő shard leképezés hozzáadása
Ezt a mintát használ hello [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) hello [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) módszerek, és létrehoz egy új hello [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) osztály. Az alábbi nevű adatbázis hello mintában **sample_shard_2** , minden szükséges sémaobjektumok saját létrejöttek toohold tartomány [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create hello mapping and associate it with hello new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Alternatív megoldásként használhatja a Powershell toocreate egy új Shard térkép Manager. Példa: elérhető [Itt](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="tooadd-a-shard-for-an-empty-part-of-an-existing-range"></a>egy meglévő tartomány üres részét képezik a shard tooadd
Bizonyos körülmények között előfordulhat, hogy már le van képezve egy tartomány tooa shard és részben megtelt adatokkal, de arra most jövőbeli adatok toobe irányított tooa különböző szilánkcímtárban. Például, hogy a shard napi között, és már hozzárendelt 50 nap tooa shard, de 24. napon, egy másik shard a jövőbeli adatok tooland kívánt. hello rugalmas adatbázis [vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md) hajthatják végre a műveletet, de ha az adatmozgatás nem szükséges (például napos [25, 50 hello tartomány adatokat), azaz, 25 nap között lehet too50 kizárólagos, még nem létezik) hajthat végre a Shard térkép felügyeleti API-k közvetlenül hello a teljes mértékben használatával.

### <a name="example-splitting-a-range-and-assigning-hello-empty-portion-tooa-newly-added-shard"></a>Példa: a felosztás egy tartományt, és rendelje hozzá a hello üres részét tooa újonnan hozzáadott shard
Létrehozott egy "sample_shard_2" és az összes szükséges séma objektum saját nevű adatbázis.  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split hello Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) toodifferent shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline tooa different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Fontos**: Ezzel a technikával használja, csak ha egyes tartományon hello hello frissítve lesz üres.  a fenti hello módszerek hello tartomány áthelyezett adatok ellenőrzése, a legjobb tooinclude ellenőrzi a kódban.  Ha a sorok léteznek hello közé kerülnek, hello tényleges adatokat terjesztési nem fog egyezni a hello frissített shard térkép. Használjon hello [vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md) tooperform hello művelet helyette ezekben az esetekben.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

