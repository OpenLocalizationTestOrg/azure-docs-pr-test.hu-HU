---
title: "az Azure Security Centerben új generációs tűzfal aaaAdd |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslatait ** adja hozzá a következő generációs tűzfal ** és ** útvonal traffice keresztül NGFW csak **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 9a80f12571ba08eadf3361728c6321388c863235
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Új generációs tűzfal hozzáadása az Azure Security Centerben
Az Azure Security Center is javasoljuk, hogy ad hozzá egy új generációs tűzfal (NGFW) az egy Microsoft partnert tooincrease a biztonsági védelmet. Ez a dokumentum útmutatást nyújt a példa bemutatja, hogyan toodo ez.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **adja hozzá az új generációs tűzfal**.
   ![Újgenerációs tűzfal hozzáadása][1]
2. A hello **adja hozzá az új generációs tűzfal** panelen, jelöljön ki egy végpontot.
   ![Válasszon ki egy végpontot][2]
3. Egy második **adja hozzá az új generációs tűzfal** panel nyílik meg. Dönthet úgy toouse egy meglévő megoldás ha rendelkezésre áll, vagy létrehozhat egy újat. Ebben a példában nincsenek nincs meglévő megoldások, létrehozhatunk egy NGFW.
   ![Hozzon létre új generációs tűzfal][3]
4. toocreate egy NGFW megoldás integrált partnereink hello listájából válassza ki. Ez a példa azt válassza **Check Point**.
   ![Új generációs tűzfal-megoldás kiválasztása][4]
5. Hello **Check Point** panel nyílik meg, és, hogy hello partneri megoldás információt nyújt. Válassza ki **létrehozása** hello információk panelen.
   ![Tűzfal információk panel][5]
6. Hello **hozzon létre virtuális gépet** panel nyílik meg. Itt adhatja meg a szükséges toospin futó virtuális gépek (VM) hello NGFW információkat. Hello lépésekkel, és adja meg a szükséges hello NGFW adatokat. Válassza ki a OK tooapply.
   ![Virtuális gép toorun NGFW létrehozása][6]

## <a name="route-traffic-through-ngfw-only"></a>Csak az újgenerációs tűzfalon keresztül haladjon a forgalom
Térjen vissza a toohello **javaslatok** panelen. Egy új bejegyzést jött létre, miután hozzáadta a Security Center nevű keresztül egy NGFW **keresztül NGFW forgalmat csak**. Ez a javaslat jön létre, csak akkor, ha telepítette a NGFW biztonsági központon keresztül történik. Ha Internet felé néző végpontok, a Security Center azt javasolja, hogy a bejövő forgalom tooyour keresztül az NGFW VM kényszerítése a hálózati biztonsági csoport szabályok konfigurálása.

1. A hello **javaslatok panel**, jelölje be **keresztül NGFW forgalmat csak**.
   ![Csak az újgenerációs tűzfalon keresztül haladjon a forgalom][7]
2. Hello paneljének megnyitása, ez **keresztül NGFW forgalmat csak**, amely felsorolja azokat a virtuális gépek, amelyek irányíthatja a forgalmat. Válassza ki a virtuális gépek hello listából.
   ![Jelöljön ki egy virtuális Gépet][8]
3. Egy hello panelen kiválasztott VM megnyílik, kapcsolódó bejövő szabályok megjelenítése. Olyan leírást nyújt további információt a következő lépést. Válassza ki **bejövő szabályok szerkesztése** tooproceed rendelkező egy bejövő forgalomra vonatkozó szabály szerkesztése. hello általános gyakorlat, hogy **forrás** értéke túl**bármely** a kapcsolt hello Internet felé néző végpontok hello NGFW. toolearn hello tulajdonságainak hello bejövő forgalomra vonatkozó szabály, bővebben lásd: [NSG-szabályok](../virtual-network/virtual-networks-nsg.md#nsg-rules).
   ![Konfigurálja a szabályok toolimit hozzáférést][9]
   ![bejövő forgalomra vonatkozó szabály szerkesztése][10]

## <a name="see-also"></a>Lásd még:
Ez a dokumentum bemutatta, hogyan tooimplement hello Security Center ajánlás "Vegyen fel új generációs tűzfal." További információ az NGFWs és hello Check Point partneri megoldás, toolearn hello következő lásd:

* [Új generációs tűzfal](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Check Point vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendeket.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
