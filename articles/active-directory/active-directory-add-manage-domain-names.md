---
title: "aaaManaging egyéni tartománynevek az Azure Active Directoryban |} Microsoft Docs"
description: "Felügyeleti fogalmakat és használati útmutatók az Azure Active Directoryban egyéni tartományok kezelése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cf3523bd-9ee0-439e-963d-ccea038867b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 4b6d06fecf3be0621be51c38a1330eafdc1b4d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Az Azure Active Directoryban egyéni tartománynevek kezelése
A tartomány neve lehet sok címtárerőforrásokkal fontos azonosítója részeként:

* A felhasználói név vagy e-mail cím egy felhasználó számára
* a csoport hello cím
* hello app ID URI az alkalmazáshoz

Az Azure Active Directory (Azure AD) erőforrás tartalmazhatnak toobe tulajdonában hello directory hello erőforrást tartalmaz, amely már ellenőrizve tartománynevet. Csak egy globális rendszergazda Azure AD-ben végrehajthat tartomány feladatokat.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan toomanage a tartománynevek hello Azure AD felügyeleti központban, lásd: [az Azure Active Directoryban egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a>Állítsa be az Azure AD-címtár hello elsődleges tartományi neve
A könyvtár jön létre, hello kezdeti tartománynevet, például "contoso.onmicrosoft.com," esetén is hello elsődleges tartománynevet a címtáron. hello elsődleges tartománya hello alapértelmezett tartomány nevét az új felhasználó új felhasználó létrehozásakor a hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), vagy más portálokon például hello Office 365 felügyeleti portálon. Ez leegyszerűsíti a hello folyamat egy rendszergazda toocreate új felhasználók hello portálon.

toochange hello elsődleges tartományt a könyvtár nevét:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) , amely az Azure AD-címtár globális rendszergazdájának felhasználói fiókkal.
2. Válassza ki **Active Directory** hello bal oldali navigációs sávon.
3. Nyissa meg a címtárat.
4. Jelölje be hello **tartományok** fülre.
5. Jelölje be hello **módosítsa elsődleges** hello parancssáv gombjára.
6. Válassza ki a megjeleníteni kívánt toobe hello új elsődleges tartományt a címtáron hello tartományt.

Elsődleges tartománynév hello a directory toobe módosíthatja bármely ellenőrzött egyéni tartomány, amely nincs összevonva. Változó hello elsődleges tartományt a könyvtár nem változik meg a hello felhasználónevek bármely létező felhasználó számára.

## <a name="add-custom-domain-names-tooyour-azure-ad"></a>Az egyéni tartomány nevét tooyour az Azure AD hozzá
Másolatot too900 egyéni tartomány nevét tooeach Azure AD-címtár is hozzáadhat. folyamat túl hello[további egyéni tartománynév hozzáadása](active-directory-add-domain.md) van hello azonos hello első egyéni tartomány nevét.

## <a name="add-subdomains-of-a-custom-domain"></a>Altartományok az egyéni tartomány hozzáadása
Ha azt szeretné, hogy egy harmadik szintű tartomány nevét, például a "europe.contoso.com" tooyour directory tooadd, először adja hozzá, és ellenőrizze hello második szintű tartomány, például contoso.com. hello altartomány Azure AD által automatikusan fog ellenőrizhető. ellenőrizte, hogy a most felvett altartomány hello toosee, frissítési hello lap, amely tartalmazza a könyvtárban hello tartományok hello böngészőben.

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a>Milyen toodo, ha módosítja az egyéni tartománynevet hello DNS-regisztráló webhelyén
Ha módosítja az egyéni tartománynevet hello DNS-regisztráló webhelyén, folytathatja az egyéni tartománynevet, és az Azure AD maga megszakítás nélkül, és további konfigurációs feladatok nélkül toouse. Az egyéni tartománynevet és az Office 365 használatakor, Intune, illetve egyéb szolgáltatások egyéni tartománynevekkel az Azure ad-ben, tekintse meg a toohello dokumentációját szolgáltatások esetében.

## <a name="delete-a-custom-domain-name"></a>Egyéni tartománynév törlése
Egy egyéni tartománynevet törölheti az Azure AD-ből, ha a szervezet már nem használja ezt a nevet, vagy ha egy másik Azure AD-val a tartománynevet kell toouse.

egy egyéni tartománynevet toodelete, akkor előbb ellenőrizze, hogy a címtárban nincsenek erőforrások támaszkodnak hello tartomány nevét. A címtárban lévő tartománynév nem törölhető, ha:

* Minden olyan felhasználó, egy felhasználónevet, e-mail címét vagy hello tartománynevet tartalmazó proxycímmel rendelkezik.
* Bármely csoport rendelkezik egy e-mail címet, vagy a proxykiszolgáló címét, amely tartalmazza a hello tartománynevet.
* Az Azure AD bármely alkalmazásnak egy app ID URI, amely tartalmazza a hello tartománynevet.

Módosít, vagy minden ilyen erőforrás törlése az Azure AD-címtárát hello egyéni tartománynév törlése előtt.

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a>PowerShell vagy a Graph API toomanage tartománynevek
A legtöbb kezelési feladatot az Azure Active Directoryban tartománynevek is elvégezhető, Microsoft PowerShell használatával, vagy az Azure AD Graph API-val programozott módon.

* [Az Azure AD PowerShell toomanage tartománynevek használatával](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Az Azure AD Graph API toomanage tartománynevek használatával](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Következő lépések
* [Tartománynevek megismerése az Azure ad-ben](active-directory-add-domain-concepts.md)
* [Egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md)

