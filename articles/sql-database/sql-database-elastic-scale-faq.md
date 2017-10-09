---
title: "aaaAzure SQL rugalmas méretezési – gyakori kérdések |} Microsoft Docs"
description: "Gyakori kérdések az Azure SQL Database rugalmas méretezést."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: e60dde9c-bb7b-4f2f-b52c-bdb506d49fcb
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 8c77902e8ce9cbbc5e081cd9d2c911d4c8dc9e5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-faq"></a>Rugalmas adatbáziseszközöket – gyakori kérdések
#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-hello-sharding-key-for-hello-schema-info"></a>Ha egy egyetlen-bérlő minden shard, és nem horizontális kulcs, hogyan tegye I feltöltése hello horizontális kulcs hello séma információk?
hello séma objektum csak a felhasznált toosplit egyesítési forgatókönyvek. Ha egy alkalmazás eleve egyetlen bérlői, nem szükséges hello vegyes egyesítőeszköz, és így nincs szükség toopopulate hello séma info objektum.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Szeretnék már kiépített egy adatbázis és a Shard térkép kezelő már rendelkezik, hogyan regisztrálhatom ezt az új adatbázist, a shard?
Ellenőrizze a  **[shard tooan-alkalmazást hello elastic database ügyféloldali kódtár hozzáadása](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Milyen mértékű költségeket a rugalmas adatbáziseszközöket?
Hello elastic database ügyféloldali kódtár használata nem jár költségeket. Költségek csak a szilánkok és hello Shard térkép Manager használó hello Azure SQL-adatbázisok, valamint hello webes vagy feldolgozói szerepkörök hello vegyes egyesítése eszköz kiépítése keletkeznek.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Miért a hitelesítő adataimat nem működnek a shard egy másik kiszolgálóról való hozzáadásakor?
Ne használjon hitelesítő adatok használatát a tartománynevekben hello "felhasználói azonosító =username@servername", ehelyett egyszerűen használja "felhasználói azonosító felhasználónév =".  Arra is ügyeljen, hogy hello a "felhasználónév" bejelentkezés hello shard rendelkezik a szükséges engedélyekkel.

#### <a name="do-i-need-toocreate-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>I toocreate Shard térkép vezető kell és szilánkok feltöltéséhez, az alkalmazás minden indításakor?
Nem – hello hello Shard térkép Manager létrehozása (például  **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) műveletet egyszer kell elvégezni.  Az alkalmazás használandó hello hívás  **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)**  alkalmazás indítási időpontban.  Nem kell ilyen alkalmazás tartományonként csak egyszer hívható.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>A rugalmas adatbázis-eszközökkel kapcsolatos kérdéseket van, hogyan állíthatók válaszolni?
Lépjen kapcsolatba a hello toous [Azure SQL Database fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-hello-same-shard--is-this-by-design"></a>Amikor egy adatbázis-kapcsolatot egy horizontális skálázási kulccsal, Igen továbbra is adatait kérdezi le az azonos hello a kulcsok más horizontális szilánkcímtárban.  Ez szándékosan van?
hello rugalmas, méretezhető API-k adja meg a horizontális kulcs a kapcsolat toohello megfelelő adatbázis, de nem ad meg horizontális kulcs szűrést.  Adja hozzá **ahol** záradékok tooyour lekérdezés toorestrict hello hatókör toohello megadott horizontális skálázási kulcs, ha szükséges.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Használható egy másik Azure-adatbázis-kiadás az egyes shard a shard készlet?
Igen, a shard egyedi adatbázis, és így egy shard lehet egy Premium edition míg egy másik Standard kiadását. További a shard hello kiadása méretezheti felfelé vagy lefelé hello shard hello élettartama során több alkalommal.

#### <a name="does-hello-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Nem osztott egyesítési eszköz kiépítése hello (vagy törlése) adatbázis egy megosztott vagy egyesítési művelet során?
Nem. A **vágási** műveletek, hello céladatbázis léteznie kell a megfelelő sémáját hello és hello Shard térkép Manager regisztrálni kell.  A **egyesítési** műveletek, törölnie kell hello shard hello shard térkép kezelőjéből, majd törölje a hello adatbázis.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

