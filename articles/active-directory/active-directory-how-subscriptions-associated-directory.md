---
title: "Meglévő Azure-előfizetés hozzáadása az Azure AD-címtárhoz | Microsoft Docs"
description: "Meglévő előfizetés hozzáadása az Azure AD-címtárhoz"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/12/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: e063e6a46b6b99c4bbe749347e6887a930adb882
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/06/2018
---
# <a name="how-to-associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Azure-előfizetés társítása vagy hozzáadása az Azure Active Directoryhoz

Ez a cikk az Azure-előfizetések és az Azure Active Directory (Azure AD) közötti kapcsolatról tartalmaz információt, illetve arról, hogy miként adható hozzá egy meglévő előfizetés az Azure AD-címtárhoz. Az Azure-előfizetés megbízhatósági kapcsolatban áll az Azure AD-vel, ami azt jelenti, hogy ezt a címtárat megbízhatónak tekinti a felhasználók, szolgáltatások és eszközök hitelesítéséhez. Több előfizetés is megbízhat ugyanabban a címtárban, de egy előfizetés csak egy megbízható címtárral rendelkezhet. 

Az előfizetés és a címtár közötti megbízhatósági kapcsolat nem olyan, mint ami az előfizetés és az Azure összes többi erőforrása (webhelyek, adatbázisok stb.) között áll fenn. Ha egy előfizetés lejár, akkor az előfizetéssel társított egyéb erőforrások hozzáférése is lejár. Az Azure AD-címtár azonban elérhető marad az Azure-ban, így társíthat hozzá egy másik előfizetést, és az új előfizetéssel kezelheti a címtár felhasználóit.

Minden felhasználó egyetlen saját címtárral rendelkezik, amely hitelesíti őt, de más címtárakban lehet vendég is. Az Azure AD-ben a felhasználó azokat a címtárakat látja, amelyeknek a felhasználói fiókja a tagja vagy vendége.

## <a name="before-you-begin"></a>Előkészületek

* Olyan fiókkal kell bejelentkeznie, amely RBAC tulajdonosi hozzáféréssel rendelkezik az előfizetéshez.
* Egy olyan fiókkal kell bejelentkeznie, amely az előfizetéshez jelenleg társított címtárban és abban a címtárban is létezik, amelyhez az előfizetést hozzá szeretné rendelni. További információ a más címtárakhoz való hozzáférésről: [Hogyan adhat hozzá az Azure Active Directory-rendszergazda vállalatközi együttműködési felhasználókat?](active-directory-b2b-admin-add-users.md)

## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Meglévő előfizetés társítása az Azure AD-címtárral

1. Jelentkezzen be, és válasszon ki egy előfizetést az [Azure Portal Előfizetés oldalán](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Kattintson a **Címtár módosítása** elemre.

    ![A Címtár módosítása gomb képernyőképe](./media/active-directory-how-subscriptions-associated-directory/edit-directory-button.PNG)
3. Tekintse át a figyelmeztetéseket. A címtár módosításakor minden [Szerepköralapú hozzáférés-vezérlés (RBAC-)](role-based-access-control-configure.md) felhasználó és minden előfizetés-rendszergazda elveszíti a hozzárendelt hozzáférését.
4. Válasszon egy címtárat.

    ![A Címtár módosítása felhasználói felület képernyőképe](./media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.PNG)
5. Kattintson a **Módosítás** elemre.
6. Sikerült! A címtárváltóval lépjen az új címtárra. Akár 10 percig is eltarthat, amíg minden megfelelően megjelenik.

    ![A sikeres címtármódosítást jelző értesítés képernyőképe](./media/active-directory-how-subscriptions-associated-directory/edit-directory-success.PNG)

    ![A váltó képernyőképe](./media/active-directory-how-subscriptions-associated-directory/directory-switcher.PNG)


Az előfizetés címtárának módosítása szolgáltatásszintű művelet. Ez nincs hatással az előfizetés számlázási tulajdonjogára, és a fiók rendszergazdája továbbra is módosíthatja a szolgáltatás rendszergazdáját a [Fiókközpontban](https://account.azure.com/subscriptions). Ha törölni kívánja az eredeti címtárat, át kell adnia az előfizetés számlázási tulajdonjogát egy új fiókrendszergazdának. További információ a számlázási tulajdonjog átadásáról: [Azure-előfizetés tulajdonjogának átruházása másik fiókra](../billing/billing-subscription-transfer.md). 

## <a name="next-steps"></a>További lépések

* További információ új Azure AD-címtár ingyenes létrehozásáról: [Azure Active Directory-bérlő beszerzése](develop/active-directory-howto-tenant.md)
* További információ az Azure-előfizetések számlázási tulajdonjogának átadásáról: [Azure-előfizetés tulajdonjogának átruházása másik fiókra](../billing/billing-subscription-transfer.md)
* Az erőforrások hozzáférésének Microsoft Azure-ban történő kezeléséről további információért lásd: [Az erőforrások hozzáférésének megismerése az Azure-ban](active-directory-understanding-resource-access.md)
* A szerepkörök Azure AD-ben történő hozzárendeléséről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles-azure-portal.md).

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
