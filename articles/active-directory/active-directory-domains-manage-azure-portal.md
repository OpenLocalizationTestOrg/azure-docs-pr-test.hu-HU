---
title: "Az Azure Active Directoryban egyéni tartománynevek kezelése |} Microsoft Docs"
description: "Felügyeleti fogalmakat és a tartomány nevét az Azure Active Directory felügyeletéhez használati útmutatók"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5063cd0a-dba2-4ba9-aa65-b8117490d73a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: curtand;jeffsta
ms.openlocfilehash: 402c1be07b8ee885ee5341128fb3f419611b924d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Az Azure Active Directoryban egyéni tartománynevek kezelése
A tartománynév sok címtárerőforrásokkal azonosítója fontos része: felhasználói név vagy e-mail cím egy felhasználó, a cím egy csoport része, és hozzájárulhat az alkalmazás az alkalmazás URI azonosítója. Az Azure Active Directory (Azure AD) erőforrás lehetnek egy tartomány nevét, amely már ellenőrizve, a könyvtárban, amely tartalmazza az erőforrás tulajdonosa. Csak egy globális rendszergazda Azure AD-ben végrehajthat tartomány feladatokat.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Állítsa be az elsődleges tartomány nevét az Azure AD-címtár
A könyvtár jön létre, a kezdeti tartománynevet, például "contoso.onmicrosoft.com," esetén is az elsődleges tartomány nevét. Az elsődleges tartomány az alapértelmezett tartomány nevét az új felhasználó esetén létrehoz egy új felhasználót. Elsődleges tartománynév beállítása leegyszerűsíti és felgyorsítja a rendszergazda számára hozzon létre új felhasználók a portálon. Az elsődleges tartomány nevének módosítása:

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** a szövegmezőbe, majd válassza ki azt a **Enter**.
   
   ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Az a ***könyvtárnév*** panelen válassza **tartománynevek**.
4. Az a  ***könyvtárnév* -tartománynevek** panelen válassza ki a kívánt tartomány nevét ellenőrizze az elsődleges tartomány nevét.
5. A a ***tartománynév*** (Ez azt jelenti, hogy a panelen megjelenő, amely rendelkezik az új tartomány nevét a címben) panelen válassza ki a **ellenőrizze elsődleges** parancsot. Erősítse meg választását, amikor a rendszer kéri.
   
   ![Ellenőrizze a tartománynév elsődleges](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Módosíthatja a címtáron bármely, a ellenőrzött egyéni tartományt is, hogy az nem összevont elsődleges tartományneve. Az elsődleges tartomány a címtáron módosítása nem módosítja a felhasználónevek, a meglévő felhasználókat.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Egyéni tartománynevek hozzáadása az Azure AD
Minden Azure AD-címtár legfeljebb 900 egyéni tartományneveket adhat hozzá. A folyamat [további egyéni tartománynév hozzáadása](add-custom-domain.md) megegyezik az első egyéni tartománynevet.

## <a name="add-subdomains-of-a-custom-domain"></a>Altartományok az egyéni tartomány hozzáadása
Ha szeretne egy harmadik szintű tartomány neve, pl. "europe.contoso.com" adni a könyvtárhoz, először adja hozzá, és ellenőrizze a második szintű tartomány, például contoso.com. Az altartomány Azure AD által automatikusan fog ellenőrizhető. Jelenik meg, hogy a most felvett altartomány ellenőrzése megtörtént, frissítse a lapot a böngészőben, amely felsorolja azokat a tartományokat.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Mi a teendő, ha a DNS-regisztráló webhelyén módosítja az egyéni tartománynevet
Ha módosítja a DNS-regisztráló webhelyén az egyéni tartománynevet, továbbra is az egyéni tartománynevet használja az Azure AD maga megszakítás nélkül, és további konfigurációs feladatok nélkül. Az egyéni tartománynevet és az Office 365 használatakor, Intune, illetve egyéb szolgáltatások egyéni tartománynevekkel az Azure ad-ben, tekintse meg a szolgáltatások dokumentációjában.

## <a name="delete-a-custom-domain-name"></a>Egyéni tartománynév törlése
Egy egyéni tartománynevet törölheti az Azure AD-ből, ha a szervezet már nem használja ezt a nevet, vagy ha ezt a nevet használja egy másik Azure AD-val kell.

Törölni egy egyéni tartománynevet, akkor előbb ellenőrizze, hogy a címtárban nincsenek erőforrások támaszkodjon a tartomány nevét. A címtárban lévő tartománynév nem törölhető, ha:

* Bármely felhasználó rendelkezik egy felhasználónevet, a e-mail cím vagy a proxykiszolgáló címét, amely tartalmazza a tartománynevet.
* Bármely csoport rendelkezik egy e-mail címet, vagy a proxykiszolgáló címét, amely a tartomány nevét tartalmazza.
* Az Azure AD bármely alkalmazásnak egy app ID URI, amely a tartomány nevét tartalmazza.

Módosít, vagy minden ilyen erőforrás törlése az Azure AD-címtárát, az egyéni tartománynév törlése előtt.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Használja a Powershellt vagy a Graph API tartománynevek kezelése
A legtöbb kezelési feladatot az Azure Active Directoryban tartománynevek is elvégezhető, Microsoft PowerShell használatával, vagy az Azure AD Graph API-val programozott módon.

* [Tartománynevek kezelése az Azure AD PowerShell segítségével](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Tartománynevek kezelése az Azure AD Graph API segítségével](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Következő lépések
* [Egyéni tartománynevek hozzáadása](add-custom-domain.md)

