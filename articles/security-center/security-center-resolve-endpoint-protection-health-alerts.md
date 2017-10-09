---
title: "aaaResolve endpoint protection állapotát riasztások az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** hárítsa el az Endpoint Protection állapotát riasztások **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 9631d15aa1dfa9003d56332363ae7911061ed0b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Hárítsa el az endpoint protection állapotát riasztások az Azure Security Centerben
Azure Security Center javasolni fogja megoldani az észlelt endpoint protection állapotát riasztások.  Biztonsági központ lehetővé teszi virtuális gépek (VM) rendelkezik, az endpoint protection hibáinak és hány sikertelen toosee.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be. A dokumentum nem tartalmaz lépésenkénti útmutatót.
> 
> 

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok panel**, jelölje be **hárítsa el az Endpoint Protection állapotát riasztások**.
   ![Endpoint Protection-állapotriasztások feloldása][1]
2. Ekkor megnyílik hello panel **Endpoint Protection hiba** amely felsorolja azokat a virtuális gépek hibák és hello hibák száma az egyes virtuális gépek. Válassza ki a virtuális gépek hello listából.
   ![Az Endpoint protection hiba][2]
3. A **hibák lista** megnyílik a hello kiválasztott VM hibák listáját megjelenítő panel. Válassza ki a hiba hello lista toolearn további. Ekkor megnyílik egy panel kijelölt hello a hibával kapcsolatos információkat.
   ![Hibák lista][3]
   ![esemény][4]

## <a name="see-also"></a>Lásd még:
További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md)– megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md)– megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md)– megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
