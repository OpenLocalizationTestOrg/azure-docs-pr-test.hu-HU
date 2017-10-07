---
title: "a PaaS aaaSecuring webes és mobilalkalmazásokhoz az Azure App Services |} Microsoft Docs"
description: " Tudnivalók Azure App Service szolgáltatások ajánlott biztonsági eljárások az PaaS webes és mobilalkalmazások védelme. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: terrylan
ms.openlocfilehash: 68a741c668efe2157cff114b649e0e287ef196f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-services"></a>A PaaS webes és mobilalkalmazásokhoz, az Azure App Services biztonságossá tétele

Ez a cikk arról lesz szó gyűjteménye [Azure App Service szolgáltatások](https://azure.microsoft.com/services/app-service/) ajánlott biztonsági eljárások az PaaS webes és mobilalkalmazások védelme. Az alábbi gyakorlati tanácsok az Azure-ral tapasztalatunk származik, és ügyfelek hello lép, például saját maga.

## <a name="azure-app-services"></a>Azure App Services
[Az Azure App Services](../app-service/app-service-value-prop-what-is.md) a Platformszolgáltatási ajánlat, amely lehetővé teszi, hogy hozzon létre a web- és mobilalkalmazásokat bármilyen platformra vagy eszközre, és csatlakozzon a bárhol, toodata hello felhőalapú vagy helyszíni. App Service szolgáltatások hello webes és mobil funkciókkal, Azure Websites és Azure Mobile Services korábban kapott külön-külön is tartalmaz. Ezenkívül új lehetőségek is elérhetők az üzleti folyamatok automatizálásához és felhőalapú API-k üzemeltetéséhez. Egyetlen integrált szolgáltatásként alkalmazásszolgáltatások számos lehetőséget kínál széles választéka képességek tooweb, mobil és integrációs célokra.

toolearn több, lásd a áttekintését [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) és [webalkalmazások](../app-service-web/app-service-web-overview.md).

## <a name="best-practices"></a>Ajánlott eljárások

App Service szolgáltatások használata esetén kövesse az alábbi gyakorlati tanácsok:

- [Hitelesítés az Azure Active Directory (AD) révén](../app-service-web/web-sites-authentication-authorization.md#authenticate-through-azure-active-directory). Alkalmazásszolgáltatások az identitásszolgáltató az OAuth 2.0 szolgáltatást biztosít. Ügyfél fejlesztői egyszerűség ugyanakkor biztosítható a megadott engedélyezési flow webes alkalmazásokhoz, az asztali alkalmazások és a mobiltelefonok OAuth 2.0 összpontosít. Az Azure AD által használt OAuth 2.0 tooenable meg tooauthorize toomobile és a webes alkalmazásokat.
- Korlátozhatja a hozzáférést hello kell tooknow és a legalacsonyabb jogosultsági biztonsági alapelvek alapján. Hozzáférés korlátozása elengedhetetlen a szervezeteknek, amelyek szeretné, hogy az adatelérési tooenforce biztonsági házirendeket. Szerepköralapú hozzáférés-vezérlés (RBAC) lehet használt tooassign engedélyek toousers, csoportok és alkalmazások egy adott hatókörben. toolearn biztosítása a felhasználók férhetnek hozzá a tooapplications, kapcsolatos további információkért lásd: [Ismerkedés a kezelési](../active-directory/role-based-access-control-what-is.md).
- Kulcsok védelmét. Nem számít, mennyire jó a biztonság az Ha elveszíti a előfizetés kulcsokat. Az Azure Key Vault segít a felhőalapú alkalmazások és szolgáltatások által használt titkosítási kulcsok és titkos kulcsok védelmében. A Key Vault lehetővé teszi, hogy hardveres biztonsági modulokkal (HSM) védett kulcsokkal titkosítsa a kulcsokat és a titkos kulcsokat (például a hitelesítési kulcsokat, a tárfiókok kulcsait, az adattitkosítási kulcsokat, a PFX-fájlokat és a jelszavakat). A még nagyobb biztonság érdekében lehetőség van arra is, hogy kulcsokat importáljon és generáljon a hardveres biztonsági modulokban. Lásd: [Azure Key Vault](../key-vault/key-vault-whatis.md) további toolearn. Is használható Key Vault toomanage a TLS-tanúsítványok automatikus megújítási.
- Bejövő forrás IP-címek korlátozása. [App Services környezet](../app-service-web/app-service-app-service-environment-intro.md) egy virtuális hálózati integráció funkció, amely segít a bejövő forrás IP-címek a hálózati biztonsági csoportokkal (NSG-k) korlátozása. Ha ismeri az Azure virtuális hálózatokról (Vnetekről), ez az a képesség, amely lehetővé teszi az Azure-erőforrások, amelyek nem internetes, az irányítható hálózaton számos elérhető tooplace. több, lásd: toolearn [az alkalmazás integrálja az Azure virtuális hálózat](../app-service-web/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Következő lépések
Ez a cikk bevezetett alkalmazásszolgáltatások ajánlott biztonsági eljárások az PaaS webes és mobilalkalmazások védelme tooa gyűjteménye. További információ a PaaS üzemelő példányok védelme toolearn lásd:

- [PaaS-környezetek védelme](security-paas-deployments.md)
- [PaaS webes és mobilalkalmazások használata az Azure SQL Database és az SQL Data Warehouse védelme](security-paas-applications-using-sql.md)
