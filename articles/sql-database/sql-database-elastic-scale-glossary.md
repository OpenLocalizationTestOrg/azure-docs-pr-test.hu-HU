---
title: "aaaElastic adatbázis eszközök szószedet |} Microsoft Docs"
description: "A rugalmas adatbáziseszközöket használt kifejezések magyarázatát"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: a23a4e81-6706-452d-afc1-a550e5e47af9
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: d6573aad9a097e07135b0a64d1dafec19bb8cc7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-glossary"></a>A rugalmas adatbázis eszközök szószedet
hello következő fogalmakat a hello [skálázáshoz rugalmas adatbáziseszközöket](sql-database-elastic-scale-introduction.md), Azure SQL-adatbázis szolgáltatása. hello eszközök használt toomanage [shard leképezhető](sql-database-elastic-scale-shard-map-management.md), és hello [ügyféloldali kódtár](sql-database-elastic-database-client-library.md), hello [vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md), [rugalmas készletek](sql-database-elastic-pool.md), és [lekérdezések](sql-database-elastic-query-overview.md). 

Ezeket a kifejezéseket használjuk [hozzáadása egy rugalmas eszközökkel shard](sql-database-elastic-scale-add-a-shard.md) és [hello RecoveryManager osztály toofix shard térkép problémák használatával](sql-database-elastic-database-recovery-manager.md).

![Rugalmas méretezési feltételek][1]

**Adatbázis**: az Azure SQL-adatbázist. 

**Adatok függő útválasztási**: hello funkció, amely lehetővé teszi egy alkalmazás tooconnect tooa shard kap egy adott horizontális kulcsot. Lásd: [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md). Hasonlítsa össze a túl**[több Shard lekérdezés](sql-database-elastic-scale-multishard-querying.md)**.

**Globális shard térkép**: hello térkép horizontális kulcsok és a megfelelő szilánkok belül között egy **shard set**. hello globális shard térkép tárolódik hello **shard térkép manager**. Hasonlítsa össze a túl**helyi shard térkép**.

**Lista shard térkép**: shard térképre mely horizontális a kulcsok külön-külön vannak leképezve. Hasonlítsa össze a túl**tartomány Shard térkép**.   

**Helyi shard térkép**: egy shard tárolják, hello helyi shard térkép hello shardlets lévő hello shard leképezéseit tartalmazza.

**Több shard lekérdezés**: hello képességét tooissue egy lekérdezést hajtanak több szilánkok; eredmények beállítása UNION ALL szemantikájú (más néven "szétterítési lekérdezés") adott vissza. Hasonlítsa össze a túl**adatok függő útválasztási**.

**Több-bérlős** és **Single-bérlő**: Ez egy egyetlen-bérlő adatbázis és a több-bérlős adatbázis jeleníti meg:

![Egyetlen és több-bérlős adatbázisok](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Íme egy ábrázolása **szilánkos** egyetlen és több-bérlős adatbázisok. 

![Egyetlen és több-bérlős adatbázisok](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Tartomány shard térkép**: shard térképre mely hello a shard terjesztési stratégia több tartomány folytonos értékek alapján. 

**Táblák hivatkozhat**: táblákat, amelyek nem szilánkos, de a szilánkok replikálódnak. Zip-kódok például egy összefoglaló táblázatot is tárolható. 

**A shard**: az Azure SQL-adatbázis, amely a szilánkos adatkészletet adatait tárolja. 

**A shard rugalmasság**: hello mindkét lehetőséget tooperform **horizontális skálázás** és **függőleges skálázás**.

**Horizontálisan skálázott táblák**: táblákat, amelyek szilánkos, azaz, amelynek adatait a horizontális értékek alapján szilánkok között van elosztva. 

**Horizontális kulcs**: határozza meg, hogy milyen adatok szilánkok pontjain oszlop értéke. hello érték típusa lehet hello a következők egyikét: **int**, **bigint**, **varbinary**, vagy **uniqueidentifier**. 

**A shard set**: hello gyűjteménye, amelyek kiadott toohello szilánkok hello shard térkép manager ugyanazt a shard térképre.  

**Shardlet**: hello adatok társított egy horizontális skálázási kulcsának egy shard egyetlen értéket. Egy shardlet adatátvitelt jelölik a lehető legkisebb egységére hello esetén szilánkos táblák terjesztése. 

**A shard térkép**: hello horizontális kulcsok és a megfelelő szilánkok közötti leképezések halmaza.

**A shard térkép manager**: egy hello shard azt, a shard helyek és azon egy vagy több shard készletet tartalmazó felügyeleti-objektum és az adattárhoz.

![Hozzárendelések][2]

## <a name="verbs"></a>Műveletek
**Horizontális skálázás**: hello folyamat ki (vagy a) méretezés hozzáadásával vagy eltávolításával szilánkok tooa shard térkép, a lent látható módon szilánkok gyűjteménye.

![Vízszintes és függőleges skálázás][3]

**Egyesítési**: hello act shardlets áthelyezése két szilánkok tooone shard, és ennek megfelelően frissítése hello shard leképezés.

**Shardlet áthelyezés**: hello act egy egyetlen shardlet tooa különböző shard áthelyezését. 

**A shard**: hello act vízszintesen particionálás azonos strukturált adatok egy horizontális skálázási kulcs alapján több adatbázis között.

**Vegyes**: hello a folyamat több shardlets áthelyezése egy shard tooanother (általában új) szilánkcímtárban. A horizontális kulcs hello felhasználó által megadott módon hello vágási pont.

**Függőleges méretezés**: hello folyamat felfelé skálázással (vagy le) egy egyedi shard teljesítményszintjének hello. Például módosítása (amelynek eredménye további számítási erőforrások) normál tooPremium a szilánkcímtárban. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

