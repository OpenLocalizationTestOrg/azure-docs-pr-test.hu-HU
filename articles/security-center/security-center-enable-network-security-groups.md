---
title: "aaaEnable hálózati biztonsági csoportok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** engedélyezése hálózati biztonsági csoportok **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2f70fe432aa452f833a5c322d13102ebbd6dbb69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Engedélyezze a hálózati biztonsági csoportok az Azure Security Centerben
Az Azure Security Center azt javasolja, hogy a hálózati biztonsági csoport (NSG) engedélyezése, ha egy már nincs engedélyezve. NSG tartalmaz, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooyour Virtuálisgép-példány egy virtuális hálózati hozzáférés-vezérlési lista (ACL) szabályok listáját. Az NSG-ket alhálózatokhoz vagy az alhálózaton belüli virtuálisgép-példányokhoz lehet hozzárendelni. Amikor egy NSG-t hozzárendelnek egy alhálózathoz, hello ACL szabályok érvényessé válnak tooall hello Virtuálisgép-példány alhálózaton. Ezenkívül forgalom tooan adott virtuális Gépre is lehet korlátozni további korlátozásokat NGS társítása közvetlenül a virtuális gép toothat. több lásd toolearn [Mi az a hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)

Ha nem rendelkezik az NSG-k engedélyezve van, a Security Center mutatja be két javaslatok tooyou: engedélyezése hálózati biztonsági csoportok alhálózatok és a hálózati biztonsági csoportok engedélyezése virtuális gépeken. Mely szint, az alhálózati vagy a virtuális gép, tooapply NSG-k választja.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **engedélyezze a hálózati biztonsági csoportok** alhálózatok vagy virtuális gépeken.
   ![Hálózati biztonsági csoportok engedélyezése][1]
2. Ekkor megnyílik hello panel **hiányzó hálózati biztonsági csoportok beállítása** alhálózatok vagy virtuális gépekhez, attól függően, hogy a kiválasztott hello javaslat. Válassza ki egy alhálózatot vagy egy virtuális gép tooconfigure egy NSG-t a.

   ![NSG alhálózat konfigurálása][2]

   ![NSG a virtuális gép konfigurálása][3]
3. A hello **válassza a hálózati biztonsági csoport** panelen, jelöljön ki egy meglévő NSG vagy **hozzon létre új** toocreate egy NSG.

   ![Hálózati biztonsági csoport választása][4]

Ha létrehoz egy NSG-t, kövesse hello [hogyan toomanage NSG-k használatával hello Azure-portálon](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate egy NSG-t és a biztonsági szabályokat állíthat be.

## <a name="see-also"></a>Lásd még:
Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "engedélyezése hálózati biztonsági csoportok" alhálózathoz vagy a virtuális gépek. További információ az NSG-ket, engedélyezése toolearn hello következő lásd:

* [Mi az a hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Hogyan toomanage NSG-k használatával hello Azure-portálon](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
