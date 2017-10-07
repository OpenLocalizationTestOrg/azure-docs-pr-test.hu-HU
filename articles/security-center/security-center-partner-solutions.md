---
title: "aaaManaging partneri megoldások az Azure Security Centerben |} Microsoft Docs"
description: "A dokumentumból megtudhatja, hogyan az Azure Security Center lehetővé teszi, hogy egy pillanat alatt hello állapotát az Azure-előfizetésében integrált partneri megoldások a figyelő."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: terrylan
ms.openlocfilehash: fc97aedf709b9044bfd3d4ecae0b58d5fa716bbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Partnermegoldások figyelése az Azure Security Centerrel
A dokumentumból megtudhatja, hogyan toomonitor hello az Azure Security Centerben a partnermegoldások biztonsági állapotát.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be. Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="monitoring-partner-solutions"></a>Partnermegoldások figyelése
Hello **partneri megoldások** hello csempét **Security Center** panel segítségével egy pillanat alatt hello állapotát az Azure-előfizetésében integrált partneri megoldások a figyelheti.

![Partnermegoldások csempe][1]

Hello **partneri megoldások** csempe az előfizetésében integrált partneri megoldások hello számát jeleníti meg. Ha nincsenek partnermegoldások integrálva, hello csempe hello száma 0 jeleníti meg.

Partneri megoldások tooview hello állapotát:

1. Jelölje be hello **partneri megoldások** csempére. Hello **partneri megoldások** partneri megoldások listáját megjelenítő panel megnyitása tooSecurity Center csatlakoztatva.

   ![Partneri megoldások][3]

   egy partneri megoldást hello állapota lehet:

   * Védett (zöld) – nincs az állapottal kapcsolatos probléma.
   * Nem megfelelő (piros) – azonnali figyelmet igénylő állapottal kapcsolatos probléma.
   * Leállított jelentéskészítési (narancs) – a hello megoldás leállt állapotára.
   * Ismeretlen védelmi állapot (narancs) – hello megoldás hello állapota jelenleg ismeretlen miatt nem sikerült tooa egy új erőforrás toohello meglévő megoldás-Hozzáadás folyamata.
   * Nem jelentett (szürke) – hello megoldás még nem jelentett semmit, még a megoldás állapota lehet nem jelentett, ha nemrég csatlakozott, és még telepítés.

2. Válasszon ki egy partneri megoldást. Ebben a példában válassza hello segítségével **Qualys** megoldás.  Ekkor megnyílik egy panel megjelenítő hello partneri megoldás és hello megoldás hello állapotának kapcsolódó erőforrások. Válassza ki **megoldáskonzol** tooopen hello partner felület ebben a megoldásban.

   ![Partneri megoldás részletei][4]
3. Lépjen vissza toohello **Qualys** panelhez, és válassza **hivatkozás VM**. Hello **összekapcsolás** panel nyílik meg. Itt csatlakoztathatja erőforrások toohello partneri megoldás.

   ![Hivatkozás erőforrások toopartner megoldás][5]

## <a name="next-steps"></a>Következő lépések
Ebben a dokumentumban bevezetett toohello volt **partneri megoldások** Security Center csempére. További információ a Security Center toolearn tekintse meg a következő cikkek hello:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – további hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
