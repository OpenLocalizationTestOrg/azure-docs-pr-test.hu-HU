---
title: "hello elastic database ügyféloldali kódtárának aaaManaging hitelesítő |} Microsoft Docs"
description: "Hogyan tooset hello megfelelő szintű hitelesítő adatai, rendszergazda tooread csak, rugalmas adatbázis-alkalmazások"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 72e0edaf-795e-4856-84a5-6594f735fb7e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 218783ca2a07e3c0a4b089aa92634f32c41386e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="credentials-used-tooaccess-hello-elastic-database-client-library"></a>A hitelesítő adatokat használni tooaccess hello Elastic Database ügyféloldali kódtár
Hello [Elastic Database ügyféloldali kódtárának](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) használja a hitelesítő adatok tooaccess hello három különböző típusú [shard térkép manager](sql-database-elastic-scale-shard-map-management.md). Attól függően, hogy hello kell hello hozzáférés a lehető legalacsonyabb szintű hello hitelesítő adatok használata.

* **Felügyeleti hitelesítő adatok**: létrehozásához vagy a shard térkép manager kezelésére. (Lásd: hello [szószedet](sql-database-elastic-scale-glossary.md).) 
* **Hozzáférési hitelesítő adatokat**: tooaccess egy meglévő shard manager tooobtain információ a szilánkok leképezése.
* **Kapcsolat hitelesítő adatait**: tooconnect tooshards. 

Lásd még: [adatbázisok és az Azure SQL Database bejelentkezések kezelése](sql-database-manage-logins.md). 

## <a name="about-management-credentials"></a>Tudnivalók a felügyeleti hitelesítő adatok
Felügyeleti hitelesítő adatok használt toocreate egy [ **ShardMapManager** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) alkalmazásokat, amelyek kezelhetők a shard maps-objektumot. (Lásd például: [hozzáadása egy rugalmas eszközökkel shard](sql-database-elastic-scale-add-a-shard.md) és [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md)) hello felhasználói hello rugalmas bővítést ügyféloldali kódtár hello SQL-felhasználók és az SQL-bejelentkezésekben hoz létre, és biztosítja, hogy mindegyik hello olvasási/írási engedéllyel a globális shard hello nyújtott adatbázis és az összes shard adatbázis, valamint hozzárendelését. Ezeket a hitelesítő adatokat használt toomaintain hello globális shard térkép és hello helyi shard maps is, ha a változások toohello shard térkép hajtja végre. Használja például a hello felügyeleti hitelesítő adatok toocreate hello shard térkép objektum (használatával [ **GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

hello változó **smmAdminConnectionString** hello felügyeleti hitelesítő adatokat tartalmazó kapcsolati karakterlánc. hello felhasználói Azonosítót és jelszót biztosít az olvasási/írási hozzáférést tooboth shard adatbázist és az egyes szilánkok. hello felügyeleti kapcsolati karakterláncot is hello kiszolgáló nevét és az adatbázis neve tooidentify hello globális shard adatbázist. Íme egy jellegzetes kapcsolati karakterlánc erre a célra:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Ne használjon értékek hello formájában "username@server" – helyette használjon hello "felhasználónév" értéket.  Ennek az az oka a hitelesítő adatokat kell hello shard manager adatbázist és az egyes szilánkok, lehet, hogy a különböző kiszolgálókon is dolgozhat.

## <a name="access-credentials"></a>Hozzáférési hitelesítő adatok
Létrehozásakor a shard térkép manager alkalmazásban nem felügyelheti a shard maps, használja a hello globális shard térképen csak olvasási engedéllyel rendelkező hitelesítő adatokat. hello hello globális shard térkép ezeket a hitelesítő adatokat a lekért információt használt [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md) és toopopulate hello szilánkok leképezése hello ügyfél gyorsítótárába. hello hitelesítő adatok megadott hello keresztül azonos hívás minta túl**GetSqlShardMapManager** a fentiek szerint: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Vegye figyelembe a hello hello használata **smmReadOnlyConnectionString** tooreflect hello különböző hitelesítő adatokat használjanak a a nevében hozzáférés **nem rendszergazdai** felhasználók: ezek a hitelesítő adatok nem biztosítania kell írási hello globális shard térkép engedélyeit. 

## <a name="connection-credentials"></a>Kapcsolat hitelesítő adatait
További hitelesítő adatok szükségesek a hello használatakor [ **OpenConnectionForKey** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) metódus tooaccess egy egy horizontális skálázási kulcshoz tartozó szilánkcímtárban. Ezek a hitelesítő adatok csak olvasási hozzáféréssel toohello helyi shard térkép táblák hello shard levő tooprovide engedélyekre van szükség. Ez az adatok függő útválasztás hello shard a szükséges tooperform kapcsolat ellenőrzése. A kódrészletet lehetővé teszi, hogy adat-hozzáférési adatokat függő útválasztási hello környezetében: 

    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

Ebben a példában **smmUserConnectionString** hello hello kapcsolati karakterlánca tartalmazza a felhasználói hitelesítő adatokat. Az Azure SQL Database Szolgáltatásnak Ez a felhasználói hitelesítő adatokat egy tipikus kapcsolati karakterlánc: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Hello rendszergazdai hitelesítő adataival, a nem érték hello formájában "username@server". Ehelyett használja "felhasználónév".  Ne feledje, hogy hello kapcsolati karakterlánca nem tartalmazza a kiszolgáló és adatbázis nevét. Mivel ez hello **OpenConnectionForKey** hívás automatikusan átirányítja a hello kapcsolat toohello megfelelő shard hello kulcs alapján. Emiatt hello adatbázis nevét és a kiszolgáló neve nincs megadva. 

## <a name="see-also"></a>Lásd még:
[Adatbázisok és bejelentkezések kezelése az Azure SQL Database-ben](sql-database-manage-logins.md)

[Az SQL Database-adatbázis védelme](sql-database-security-overview.md)

[Ismerkedés a rugalmas adatbázis-feladatok](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

