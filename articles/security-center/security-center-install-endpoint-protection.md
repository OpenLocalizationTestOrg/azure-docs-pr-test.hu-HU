---
title: aaaInstall Endpoint Protection az Azure Security Centerben |} Microsoft Docs
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** telepíteni az Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: fea891810e042e4d4f6e55094c0cd4de013ba68a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-endpoint-protection-in-azure-security-center"></a>Az Endpoint Protection telepítése az Azure Security Centerben
Az Azure Security Center azt javasolja, hogy az endpoint protection az Azure virtuális gépek (VM) Ha az endpoint protection nincs engedélyezve. Ez a javaslat csak virtuális gépek tooWindows vonatkozik.

> [!NOTE]
> Telepítési példában a Microsoft Antimalware használja. Lásd: [Partner integrálása az Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) partnerek listáját a Security Center integrálva.  
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

1. A hello **javaslatok** panelen válassza **Endpoint Protection telepítése**.
   ![Válassza ki az Endpoint Protection telepítése][1]
2. Hello **Endpoint Protection telepítése** panel nyílik meg, az endpoint protection nélkül a virtuális gépek listáját. Kiválaszthatja, hello lista hello a kívánt tooinstall az endpoint protection, és kattintson a virtuális gépek **telepítse a virtuális gépek**.
   ![Válassza ki a virtuális gépek tooinstall Endpoint Protection a][2]
3. Hello **válassza ki az Endpoint Protection** panel megnyitása tooallow meg tooselect hello végpontvédelmi megoldás toouse szeretné. Ebben a példában most válasszon **Microsoft Antimalware**.
   ![Válassza ki az Endpoint Protection][3]
4. Hello végpontvédelmi megoldás kapcsolatos további információk jelennek meg. Kattintson a **Létrehozás** gombra.
   ![Kártevőirtó megoldás létrehozása][4]
5. Adja meg a szükséges hello konfigurációs beállításokat a hello **hozzáadása bővítmény** panelt, és válassza **OK**. toolearn hello konfigurációs beállításokkal, kapcsolatos további információkért lásd: [alapértelmezett és egyéni kártevőirtó-konfiguráció](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[A Microsoft Antimalware](../security/azure-security-antimalware.md) mostantól aktív hello a kijelölt virtuális gépek.

## <a name="see-also"></a>Lásd még:
Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "Az Endpoint Protection telepítése." További információ az Azure, a Microsoft Antimalware engedélyezése toolearn lásd: a következő dokumentum hello:

* [A Microsoft Antimalware Felhőszolgáltatásokhoz és virtuális gépek](../security/azure-security-antimalware.md) – megtudhatja, hogyan toodeploy Microsoft Antimalware.

További információ a Security Center toolearn tekintse meg a következő dokumentumok hello:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendeket.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
