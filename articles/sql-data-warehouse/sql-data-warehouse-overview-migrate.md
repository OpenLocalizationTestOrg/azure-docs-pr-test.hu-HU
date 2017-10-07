---
title: "aaaMigrate a megoldás tooSQL adatraktár |} Microsoft Docs"
description: "Áttelepítési útmutató annak érdekében, hogy a megoldás tooAzure SQL Data Warehouse platform."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/27/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 27b51f15247603f054070f360ede7f24541c0288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-solution-tooazure-sql-data-warehouse"></a>A megoldás tooAzure SQL Data Warehouse áttelepítése
Tekintse meg, mi részt vesz egy meglévő adatbázis megoldás tooAzure SQL Data Warehouse áttelepítése. 

## <a name="profile-your-workload"></a>Profilhoz, a számítási feladatok
Az áttelepítést, mielőtt azt szeretné, toobe arról, hogy az SQL Data Warehouse hello ideális megoldás a terhelést. Az SQL Data Warehouse egy elosztott rendszert tooperform analytics nagy.  Áttelepítése tooSQL adatraktár néhány tervezési módosításaival, amelyek nem túl szigorú toounderstand, de eltarthat néhány alkalommal tooimplement igényel. Ha a vállalat által igényelt egy vállalati szintű data warehouse-ba, hello előnyöket hello tevékenységi-érdemes vannak. Azonban az SQL Data Warehouse hello power nincs szüksége, esetén további költséghatékony toouse SQL Server vagy az Azure SQL Database.

Fontolja meg az SQL Data Warehouse amikor Ön:
- Rendelkezik egy vagy több terabájtos adatkészleteket
- Terv toorun analytics a nagy mennyiségű adat
- Hello képességét tooscale számítási és tárolási kell 
- Szeretné, hogy toosave költségek felfüggesztésével számítási erőforrásokat, amikor már nincs szükség.

Ne használjon az SQL Data Warehouse működési (OLTP) munkaterhelések esetén, amelyek rendelkeznek:
- Nagy gyakoriságú olvasása és írása
- Egypéldányos nagy számú kiválasztása
- Egyetlen sor beszúrása jelentős mennyiségű
- A soronkénti igényeinek feldolgozása
- Inkompatibilis formátumban (JSON, XML)


## <a name="plan-hello-migration"></a>Hello áttelepítés tervezése

Miután eldöntötte, hogy egy meglévő megoldás tooSQL adatraktár toomigrate, akkor fontos tooplan hello áttelepítés elkezdése előtt. 

A tervezés egy célja tooensure az adatokat, a táblasémákat és a kódot az SQL Data Warehouse szolgáltatással kompatibilis. Bizonyos kompatibilitási különbségek toowork, körül a jelenlegi rendszer és az SQL Data Warehouse között van. Ráadásul nagy mennyiségű adatok tooAzure időt vesz áttelepítése. Gondosan meg kell tervezni a adatok tooAzure első gyorsítja. 

Egy másik tervezési célja, a megoldás kihasználja az SQL Data warehouse hello a lekérdezési teljesítmény módosításának tooensure tervezett tooprovide toomake Tervező. Bővített adatraktárak tervezése vezet be a különböző kialakítási minta, és így hagyományos megközelítés nem mindig hello legjobban. Bár bizonyos tervezési módosításokra áttelepítés után, későbbi módosítások hamarabb hello folyamat menti.

a sikeres áttelepítéshez a tooperform, kell toomigrate a táblasémákat, a kódot és az adatokat. Az alábbi áttelepítési témakörök útmutatást lásd:

-  [Telepítse át a sémák](sql-data-warehouse-migrate-schema.md)
-  [Telepítse át a kódot](sql-data-warehouse-migrate-code.md)
-  [Adatok áttelepítése](sql-data-warehouse-migrate-data.md). 

<!--
## Perform hello migration


## Deploy hello solution


## Validate hello migration

-->

## <a name="next-steps"></a>Következő lépések
hello CAT (Ügyféltanácsadói csapatának) is rendelkezik néhány nagy az SQL Data Warehouse útmutatást, amelyek ezek teszik közzé a blogok keresztül.  Tekintse meg a cikk [áttelepítése adatok tooAzure SQL Data Warehouse a gyakorlatban] [ Migrating data tooAzure SQL Data Warehouse in practice] további útmutatást nyújt az áttelepítési.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data tooAzure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
