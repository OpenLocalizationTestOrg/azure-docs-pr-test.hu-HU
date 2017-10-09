---
title: "az Azure Security Centerben webalkalmazási tűzfal aaaAdd |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslatait ** hozzáadása a webes alkalmazás tűzfal ** és ** véglegesítése alkalmazás védelmi **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.openlocfilehash: bff0aa2a5c6e0dde23396f93de52abe295053581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Webalkalmazási tűzfal hozzáadása az Azure Security Centerben
Az Azure Security Center is javasoljuk, hogy ad hozzá egy webes alkalmazás tűzfalat (waf-ot) az egy Microsoft partnert toosecure a webes alkalmazások. Ez a dokumentum útmutatást nyújt a példa bemutatja, hogyan tooapply Ez a javaslat.

WAF ajánlás bármely nyilvános felé néző IP-cím (példány szint IP vagy terhelés eloszlik IP), amely rendelkezik egy társított hálózati biztonsági csoportot a nyissa meg a bejövő webes portokkal (80,443) jelenik meg.

A Security Center javasolja, hogy egy WAF toohelp kiépítése a webalkalmazások virtuális gépek és az App Service Environment-környezet célzó támadások elleni védelemre-e. Az App Service környezetben (ASE) van egy [prémium](https://azure.microsoft.com/pricing/details/app-service/) terv lehetőséget az Azure App Service egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására az Azure App Service apps biztosító szolgáltatás. toolearn ASE, kapcsolatos további információkért lásd: hello [App Service környezetben dokumentáció](../app-service/app-service-app-service-environments-readme.md).

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **biztonságossá webalkalmazását webalkalmazási tűzfal használatával**.
   ![Biztonságos webes alkalmazás][1]
2. A hello **a webes alkalmazások webalkalmazási tűzfal biztonságos** panelen válassza ki a webalkalmazás. Hello **webalkalmazási tűzfal hozzáadása** panel nyílik meg.
   ![Webalkalmazási tűzfal hozzáadása][2]
3. Választhat toouse egy meglévő webalkalmazási tűzfal ha rendelkezésre áll, vagy létrehozhat egy újat. Ebben a példában érhetők el nem létező WAFs, létrehozhatunk egy WAF.
4. egy WAF toocreate integrált partnereink hello listában jelöljön ki egy megoldást. Ez a példa azt válassza **Barracuda webalkalmazási tűzfal**.
5. Hello **Barracuda webalkalmazási tűzfal** panel nyílik meg, és, hogy hello partneri megoldás információt nyújt. Válassza ki **létrehozása** hello információk panelen.

   ![Tűzfal információk panel][3]

6. Hello **új webalkalmazási tűzfal** panel megnyitása, ahol elvégezhető **Virtuálisgép-konfiguráció** lépéseit, és adja meg **WAF információk**. Válassza ki **Virtuálisgép-konfiguráció**.
7. A hello **Virtuálisgép-konfiguráció** panelen adhatja hello virtuális gép futó szükséges toospin hello WAF adatokat.
   ![Virtuálisgép-konfiguráció][4]
8. Térjen vissza a toohello **új webalkalmazási tűzfal** panelhez, és válassza **WAF információk**. A hello **WAF információk** panelen konfigurálja magát hello WAF. 7. lépés lehetővé teszi tooconfigure hello virtuális gép mely hello WAF a fut és 8. lépés lehetővé teszi a tooprovision hello WAF magát.

## <a name="finalize-application-protection"></a>Alkalmazás védelem véglegesítése
1. Térjen vissza a toohello **javaslatok** panelen. Egy új bejegyzést készítésének hello WAF nevű létrehozott **alkalmazás védelem véglegesítése**. Ez a bejegyzés lehetővé teszi, hogy kell-e toocomplete hello folyamat ténylegesen ezzel elvégeztük hello WAF hello Azure virtuális hálózaton belül, hogy megvédheti a hello alkalmazás.

   ![Alkalmazás védelem véglegesítése][5]

2. Válassza ki **alkalmazás védelem véglegesítése**. Ekkor megnyílik egy új panel. Láthatja, hogy van-e egy webes alkalmazás, amelyet a toohave a forgalom átirányítása megtörténik.
3. Válassza ki a hello webes alkalmazást. Ekkor megnyílik egy panel, amely lehetővé teszi az lépéseket a véglegesítése hello webalkalmazás tűzfal beállítása. Hello lépéseket, majd válassza ki **korlátozzák a forgalmat**. A Security Center majd hello vezetékezést-fel Önnek.

   ![Korlátozzák a forgalmat.][6]

> [!NOTE]
> A Security Center több webalkalmazás védelmet biztosíthat a következő alkalmazások tooyour meglévő WAF központi telepítések hozzáadásával.
>
>

az adott WAF hello naplók most már teljes mértékben. A Security Center automatikusan összegyűjtése és elemzése hello naplókat, hogy azt is surface fontos biztonsági riasztások tooyou elindíthatja.

## <a name="next-steps"></a>Következő lépések
Ez a dokumentum bemutatta, hogyan tooimplement hello Security Center ajánlás "Add a web application." bővebben a webes alkalmazás tűzfal konfigurálásáról toolearn hello következő lásd:

* [Webalkalmazási tűzfal (WAF) App Service Environment-környezet konfigurálása](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
