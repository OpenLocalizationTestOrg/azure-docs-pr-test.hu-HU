---
title: "aaaEnable naplózás és a fenyegetések észlelésére SQL-kiszolgálón az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** naplózás engedélyezését & Threat detection az SQL-kiszolgálók **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 042fca4d-7dab-4172-8614-e8c21ccb4960
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: b082c48cdbc386f14e677f4e13a7f306f37fd0e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a>SQL-kiszolgálón az Azure Security Centerben a naplózás és a fenyegetések észlelésére engedélyezése
Azure Security Center javasolni fogja, hogy bekapcsolja a naplózást, és fenyegetésészlelés az összes olyan adatbázis az Azure SQL-kiszolgálón, ha a naplózás nincs engedélyezve. Naplózás és a fenyegetések észlelése segítségével törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és betekintést azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére.

Után a naplózás bekapcsolta a Fenyegetésészlelés beállítások és e-mailek tooreceive biztonsági riasztások állíthatja be. A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli. Ez lehetővé teszi toodetect, és válaszoljon toopotential fenyegetéseket, azok bekövetkezésekor.

Ez a javaslat alkalmazása toohello Azure SQL-szolgáltatás csak; az Azure infrastruktúra-szolgáltatásokat (Azure IaaS) a virtuális gépeken futó SQL Server nem tartoznak bele.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **naplózás engedélyezését & Threat detection SQL-kiszolgálón**.  Ekkor megnyílik a hello **naplózás engedélyezését & Threat detection SQL-kiszolgálón** panelen.

   ![SQL Serverek naplózásának engedélyezése][1]
2. Jelölje be egy SQL server tooenable naplózás és a fenyegetések észlelésére. Ekkor megnyílik a hello **naplózás és Fenyegetésészlelés** panelen.

3. A hello **naplózás és Fenyegetésészlelés** panelen válassza **ON** alatt **naplózási**.

   ![Kapcsolja be a naplózási beállítások][2]
4. Hello kövesse [SQL adatbázis naplózásának hello Azure-portálon](../sql-database/sql-database-auditing-portal.md) tooconfigure tárolási a naplók tárolásához. hello storage-fiók előfizetés-gyűjtemény hello alapértelmezett tárfiók.
5. Hello kövesse [Ismerkedés az SQL-adatbázis Fenyegetésészlelés](../sql-database/sql-database-threat-detection.md) tooturn a fenyegetések észlelése és tooconfigure hello listáját, amelyek megkapják a biztonsági riasztásokat, a rendellenes tevékenységek észlelésekor e-mailek és konfigurálása.

## <a name="see-also"></a>Lásd még:
Ez a cikk bemutatta, hogyan tooimplement hello-e a Security Center ajánlás "A naplózás engedélyezését és veszélyforrások detektálása SQL-kiszolgálón." További információ az SQL-adatbázis védelme toolearn hello következő lásd:

* [Az SQL Database-adatbázis védelme](../sql-database/sql-database-security-overview.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
