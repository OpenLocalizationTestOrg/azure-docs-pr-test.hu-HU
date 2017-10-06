---
title: "aaaManaging egyéni tartománynevek az Azure Active Directoryban |} Microsoft Docs"
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
ms.openlocfilehash: 4719524c4e972f6c981db39f016729da13b37670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Az Azure Active Directoryban egyéni tartománynevek kezelése
Tartománynév sok címtárerőforrásokkal hello azonosítója fontos szerepet: a felhasználói név vagy e-mail cím egy felhasználó hello cím egy csoport része, és hozzájárulhat az alkalmazás hello app ID URI. Az Azure Active Directory (Azure AD) erőforrás egy tartomány nevét, amely már ellenőrizve, tulajdonában hello directory hello erőforrást tartalmaz, amelyek tartalmazhatnak. Csak egy globális rendszergazda Azure AD-ben végrehajthat tartomány feladatokat.

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a>Állítsa be az Azure AD-címtár hello elsődleges tartományi neve
A könyvtár jön létre, hello kezdeti tartománynevet, például "contoso.onmicrosoft.com," esetén is hello elsődleges tartomány nevét. hello elsődleges tartománya hello alapértelmezett tartomány nevét az új felhasználó új felhasználó létrehozásakor. Elsődleges tartománynév beállítása leegyszerűsíti a hello folyamat egy rendszergazda toocreate új felhasználók hello portálon. toochange hello elsődleges tartománynév:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.
   
   ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)
3. A hello ***könyvtárnév*** panelen válassza **tartománynevek**.
4. A hello  ***könyvtárnév* -tartománynevek** panelen, a select hello tartománynév szeretné toomake hello elsődleges tartomány nevét.
5. A hello ***tartománynév*** (Ez azt jelenti, hogy hello panelen megjelenő, amely rendelkezik az új tartomány nevét hello címben) panelen válassza ki a hello **ellenőrizze elsődleges** parancsot. Erősítse meg választását, amikor a rendszer kéri.
   
   ![Ellenőrizze a tartománynév elsődleges](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Elsődleges tartománynév hello a directory toobe módosíthatja bármely ellenőrzött egyéni tartomány, amely nincs összevonva. Változó hello elsődleges tartományt a könyvtár nem változik meg a hello felhasználónevek bármely létező felhasználó számára.

## <a name="add-custom-domain-names-tooyour-azure-ad"></a>Az egyéni tartomány nevét tooyour az Azure AD hozzá
Másolatot too900 egyéni tartomány nevét tooeach Azure AD-címtár is hozzáadhat. folyamat túl hello[további egyéni tartománynév hozzáadása](add-custom-domain.md) van hello azonos hello első egyéni tartomány nevét.

## <a name="add-subdomains-of-a-custom-domain"></a>Altartományok az egyéni tartomány hozzáadása
Ha azt szeretné, hogy egy harmadik szintű tartomány nevét, például a "europe.contoso.com" tooyour directory tooadd, először adja hozzá, és ellenőrizze hello második szintű tartomány, például contoso.com. hello altartomány Azure AD által automatikusan fog ellenőrizhető. ellenőrizte, hogy a most felvett altartomány hello toosee, frissítési hello böngészőben hello hello tartományok felsoroló.

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a>Milyen toodo, ha módosítja az egyéni tartománynevet hello DNS-regisztráló webhelyén
Ha módosítja az egyéni tartománynevet hello DNS-regisztráló webhelyén, folytathatja az egyéni tartománynevet, és az Azure AD maga megszakítás nélkül, és további konfigurációs feladatok nélkül toouse. Az egyéni tartománynevet és az Office 365 használatakor, Intune, illetve egyéb szolgáltatások egyéni tartománynevekkel az Azure ad-ben, tekintse meg a toohello dokumentációját szolgáltatások esetében.

## <a name="delete-a-custom-domain-name"></a>Egyéni tartománynév törlése
Egy egyéni tartománynevet törölheti az Azure AD-ből, ha a szervezet már nem használja ezt a nevet, vagy ha egy másik Azure AD-val a tartománynevet kell toouse.

egy egyéni tartománynevet toodelete, akkor előbb ellenőrizze, hogy a címtárban nincsenek erőforrások támaszkodnak hello tartomány nevét. A címtárban lévő tartománynév nem törölhető, ha:

* Bármely felhasználó rendelkezik egy felhasználónevet, a e-mail cím vagy a proxykiszolgáló címét, amely tartalmazza a hello tartománynevet.
* Bármely csoport rendelkezik egy e-mail címet, vagy a proxykiszolgáló címét, amely tartalmazza a hello tartománynevet.
* Az Azure AD bármely alkalmazásnak egy app ID URI, amely tartalmazza a hello tartománynevet.

Módosít, vagy minden ilyen erőforrás törlése az Azure AD-címtárát hello egyéni tartománynév törlése előtt.

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a>PowerShell vagy a Graph API toomanage tartománynevek
A legtöbb kezelési feladatot az Azure Active Directoryban tartománynevek is elvégezhető, Microsoft PowerShell használatával, vagy az Azure AD Graph API-val programozott módon.

* [Az Azure AD PowerShell toomanage tartománynevek használatával](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Az Azure AD Graph API toomanage tartománynevek használatával](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Következő lépések
* [Egyéni tartománynevek hozzáadása](add-custom-domain.md)

