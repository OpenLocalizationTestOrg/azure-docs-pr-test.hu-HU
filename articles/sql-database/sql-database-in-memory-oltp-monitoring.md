---
title: "aaaMonitor XTP memórián belüli tároló |} Microsoft Docs"
description: "Becsült és a figyelő XTP memórián belüli tároló használatához kapacitás; Hárítsa el a kapacitás hiba 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: jodebrui
ms.openlocfilehash: fcb17bd8e9ebef4862d4b55bf5a79b45b9419fca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-in-memory-oltp-storage"></a>Memóriabeli OLTP-tár figyelése
Használata esetén [memórián belüli online Tranzakciófeldolgozási](sql-database-in-memory.md), a memóriaoptimalizált táblák és Táblaváltozók adatainak memórián belüli online Tranzakciófeldolgozási tárolóban található. Minden prémium szolgáltatásszintet felső korlátja memórián belüli online Tranzakciófeldolgozási tároló, amely hello ismertetett [SQL Database Service Tiers cikk](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). Ha túllépi ezt a határt, beszúrási és a frissítési műveletek kezdheti el hiba miatt sikertelenül (41823). Ezen a ponton hoz kell tooeither delete adatok tooreclaim memória, vagy hello teljesítményszinttel az adatbázis frissítéséhez.

## <a name="determine-whether-data-will-fit-within-hello-in-memory-storage-cap"></a>Határozza meg, hogy adatokat hello memórián belüli tároló maximális illeszkedni fog
Hello tárolási cap határozza meg: tekintse át a hello [SQL Database Service Tiers cikk](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) hello tárolási biztosít, amelyet a különböző Premium szolgáltatásszintek hello a.

Követelmények a memóriaoptimalizált táblát hello azonos memória becslése, mert az SQL Server módon nem Azure SQL Database. Témakör néhány perc tooreview érvénybe [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Vegye figyelembe, hogy hello tábla és a tábla változó sorok, valamint az indexek, count felé hello maximális felhasználói adatok mérete. Emellett az ALTER TABLE kell elegendő hely toocreate hello teljes táblázat és az indexeket egy új verziója.

## <a name="monitoring-and-alerting"></a>Figyelés és riasztás
Figyelheti a memórián belüli tárolóhasználat hello százalékában [tárolási kap a teljesítmény szinthez](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) a hello Azure [portal](https://portal.azure.com/): 

* Hello adatbázis paneljén található hello Erőforrás kihasználtsága mezőbe, majd kattintson a Szerkesztés.
* Válassza ki a metrika hello `In-Memory OLTP Storage percentage`.
* tooadd riasztást, hello erőforrás-használat mezőben tooopen hello metrika panelen kattintson, majd kattintson a Hozzáadás riasztás.

Vagy a következő lekérdezés tooshow hello memórián belüli tárhely kihasználtságát hello használata:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Javítsa ki a kevés memória helyzetek - 41823 hiba
Kevés a memória eredmények futó INSERT, UPDATE és LÉTREHOZÁSI műveletek 41823 hiba miatt sikertelen.

Hibaüzenet 41823 azt jelzi, hogy hello memóriaoptimalizált táblák és Táblaváltozók túllépte hello maximális méretét.

tooresolve a hiba miatt, vagy:

* Adatok törléséhez a hello memóriaoptimalizált táblákkal, a potenciálisan szervez hello adatok tootraditional, a lemezalapú táblák; vagy,
* Hello szolgáltatás réteg tooone frissítse az elegendő memórián belüli tároló hello adatok a memóriaoptimalizált táblák tookeep kell.

## <a name="next-steps"></a>Következő lépések
Figyelés útmutatást, lásd: [figyelése Azure SQL Database dinamikus felügyeleti nézetekkel](sql-database-monitoring-with-dmvs.md).
