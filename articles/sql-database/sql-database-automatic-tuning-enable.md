---
title: "automatikus hangolása az Azure SQL Database aaaEnable |} Microsoft Docs"
description: "Automatikus hangolása az Azure SQL adatbázis könnyen engedélyezheti."
services: sql-database
documentationcenter: 
author: vvasic
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 06/05/2016
ms.author: vvasic
ms.openlocfilehash: af9da161eabc0f8c4cb100c050288f234efb8093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-automatic-tuning"></a>Automatikus hangolás engedélyezése

Az Azure SQL Database az automatikusan kezelt adatok szolgáltatása, amely folyamatosan figyeli a lekérdezéseket, és azonosítja, hogy elvégezhető-e a számítási feladatok teljesítményére tooimprove hello művelet. Áttekintheti javaslatok és manuálisan alkalmazhatja azokat, vagy lehetővé teszik az Azure SQL Database automatikusan alkalmazza a javítási műveleteket – ez az úgynevezett **automatikus hangolási módot**. Automatikus hangolása hello kiszolgáló vagy hello adatbázis szintjén engedélyezhető.

## <a name="enable-automatic-tuning-on-server"></a>Engedélyezze a kiszolgálón automatikus hangolása

tooenable automatikus hangolása Azure SQL adatbázis-kiszolgálón nyissa meg a toohello server az Azure portál, és jelölje ki **automatikus hangolása** hello menüben. Válassza ki a kívánt tooenable, és válassza ki az automatikus hangolási lehetőségeket hello **alkalmaz**:

![Kiszolgáló](./media/sql-database-automatic-tuning-enable/server.png)

Automatikus hangolása beállítások a kiszolgálón alkalmazott tooall adatbázisok hello kiszolgálón vannak. Alapértelmezés szerint minden adatbázisok hello konfigurációs öröklése a fölérendelt kiszolgáló, de ez felül, és az egyes adatbázisok külön-külön megadva.

## <a name="configure-automatic-tuning-on-database"></a>Konfigurálja az adatbázis automatikus hangolása

hello Azure portál lehetővé teszi, hogy tooindividually adja meg a hello automatikus hangolási beállítás, az összes adatbázisra.

> [!NOTE]
> általános javaslat hello toomanage hello automatikus hangolási beállítás kiszolgálói szinten azonos konfigurációs beállítások alkalmazhatók az összes adatbázis automatikusan Igen hello. Konfigurálja az automatikus hangolással a egyedi adatbázis Ha hello adatbázis különböző, hogy a többi hello ugyanarra a kiszolgálóra.
>

automatikus hangolása egy adatbázist, a tooenable toohello adatbázis hello Azure-portálon a keresse meg és majd, és válassza ki **automatikus hangolása**. Egy önálló adatbázis tooinherit hello beállításokat hello adatbázisból hello jelölőnégyzet kiválasztásával konfigurálhatja, vagy külön-külön is hello konfigurációs adatbázis megadása.

![Adatbázis](./media/sql-database-automatic-tuning-enable/database.png)

Miután kiválasztotta a megfelelő konfigurációs, kattintson a **alkalmaz**.

## <a name="next-steps"></a>Következő lépések
* Olvasási hello [automatikus hangolási cikk](sql-database-automatic-tuning.md) toolearn további automatikus hangolása és szerepéről javítják a teljesítményt.
* Lásd: [teljesítmény javaslatok](sql-database-advisor.md) teljesítmény javaslatok az Azure SQL Database áttekintését.
* Lásd: [lekérdezési teljesítmény Insights](sql-database-query-performance.md) toolearn hello teljesítményre gyakorolt hatását a leggyakoribb lekérdezések megtekintése.
