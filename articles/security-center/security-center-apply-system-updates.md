---
title: "aaaApply rendszerfrissítések az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslatait ** alkalmazza a rendszer frissítések ** és ** rendszer frissítések ** után indítsa újra."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: 02024f1558b4758c09141fe1934c2e1a9845cc96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a>Alkalmazza a rendszer frissítéseket az Azure Security Centerben
Az Azure Security Center naponta figyeli a Windows és Linux virtuális gépek (VM) operációsrendszer-frissítések hiányoznak. A Security Center lekér egy listát az elérhető biztonsági és kritikus frissítések a Windows Update webhelyről vagy a Windows Server Update Services (WSUS), attól függően, amelyek a szolgáltatás úgy van konfigurálva a Windows virtuális gép.  A Security Center is ellenőrzi a legújabb frissítéseket hello Linux rendszereken. Ha a virtuális gép hiányzik a rendszer frissítését, a Security Center javasolni fogja-e, hogy a rendszer frissítéseinek alkalmazása

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **rendszer frissítéseinek alkalmazása**.

   ![Rendszerfrissítések alkalmazása][1]
2. Hello **rendszer frissítéseinek alkalmazása** panel nyílik meg, egy hiányzó rendszerfrissítések virtuális gépek listájának megjelenítése. Jelöljön ki egy virtuális Gépet.

   ![Jelöljön ki egy virtuális Gépet][2]
3. Ekkor megnyílik egy panel hiányzó frissítések listájának ezt a virtuális gépet. Válassza ki a rendszer frissítését. Ebben a példában válassza ki most KB3156016.

   ![Hiányzó biztonsági frissítések][3]

4. Hello kövesse hello **biztonsági frissítések** panel tooapply hello hiányzó frissítés.

   ![Biztonsági frissítés][4]

## <a name="reboot-after-system-updates"></a>Rendszerfrissítések utáni újraindítás
1. Térjen vissza a toohello **javaslatok** panelen. Egy új bejegyzést készítésének rendszerfrissítések hívása után **rendszerfrissítések után indítsa újra**. Ez a bejegyzés lehetővé teszi, hogy kell-e tooreboot hello VM toocomplete hello folyamat a rendszer frissítéseinek alkalmazása.

   ![Rendszerfrissítések utáni újraindítás][5]
2. Válassza ki **rendszerfrissítések után indítsa újra**. Ekkor megnyílik **újraindítás függőben lévő toocomplete rendszerfrissítések** , hogy kell-e toorestart toocomplete hello virtuális gépek listáját megjelenítő panelen alkalmazza a rendszer folyamat.

   ![Újraindítása függőben van.][6]

Indítsa újra a virtuális gép hello Azure toocomplete hello folyamat során.

## <a name="see-also"></a>Lásd még:
További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
