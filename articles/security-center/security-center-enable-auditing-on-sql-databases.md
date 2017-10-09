---
title: "aaaEnable naplózás és a fenyegetések észlelésére az SQL-adatbázisok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** engedélyezése az SQL adatbázisok ** naplózás és a fenyegetések észlelésére."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: c94140acf37cabaca3e681ba5db79d6827e7b9db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a>SQL-adatbázisok az Azure Security Centerben a naplózás és a fenyegetések észlelésére engedélyezése
Az Azure Security Center azt javasolja, hogy bekapcsolja a naplózást, és a fenyegetések észlelésére az összes SQL-adatbázisok ha naplózás és fenyegetésészlelés nincs engedélyezve. Naplózás és a fenyegetések észlelése segítségével törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és betekintést azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére.

Után a naplózás bekapcsolta a Fenyegetésészlelés beállítások és e-mailek tooreceive biztonsági riasztások állíthatja be. A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli. Ez lehetővé teszi toodetect, és válaszoljon toopotential fenyegetéseket, azok bekövetkezésekor.

Ez a javaslat alkalmazása toohello Azure SQL-szolgáltatás csak; a virtuális gépeken futó SQL nem tartoznak bele.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **naplózás engedélyezését & Threat detection SQL-adatbázisok**.  Ekkor megnyílik a hello **naplózás engedélyezését & Threat detection SQL-adatbázisok** panelen.

   ![SQL-adatbázis naplózásának engedélyezése][1]
2. Válassza ki, egy SQL adatbázis tooenable naplózást. Ekkor megnyílik a hello **naplózás és Fenyegetésészlelés** panelen.

3. A hello **naplózás és Fenyegetésészlelés** panelen válassza **ON** alatt **naplózási**.

   ![Kapcsolja be a naplózás és a fenyegetések észlelése][2]
4. Hello kövesse [SQL adatbázis Fenyegetésészlelés hello Azure-portálon](../sql-database/sql-database-threat-detection-portal.md) tooturn a fenyegetések észlelése és tooconfigure hello listáját, amelyek megkapják a biztonsági riasztásokat, a rendellenes tevékenységek észlelésekor e-mailek és konfigurálása.

## <a name="see-also"></a>Lásd még:
Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "Enable naplózási & Threat észlelési SQL-adatbázisok." További információ az SQL-adatbázis védelme toolearn hello következő lásd:

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
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
