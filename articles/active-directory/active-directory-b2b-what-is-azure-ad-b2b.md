---
title: "aaaWhat az Azure Active Directory B2B együttműködés? | Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés a vállalatokon átívelő kapcsolatok üzleti partnerek tooselectively hozzáférés engedélyezése a vállalati alkalmazásokat támogatja."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.custom: aaddev
ms.reviewer: sasubram
ms.openlocfilehash: 359989b66f3d012c306e8748a607662fffacb919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Mi az az Azure AD B2B együttműködés?

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

Az Azure AD-vállalatok (B2B) együttműködési képességek engedélyezése bármely szervezet használata az Azure AD toowork biztonságos felhasználók bármilyen más szervezettől származik, minimális és maximális mérete. Azon szervezetek lehet az Azure ad-vel vagy nélkül, vagy akár egy informatikai szervezet vagy anélkül. 

A szervezetek az Azure AD használatával hozzáférést biztosíthat toodocuments, erőforrások és alkalmazások tootheir partnerek, teljes felügyeletet gyakorolhat a saját vállalati adatok megőrzésével. A fejlesztők a hello Azure AD vállalatok API-k toowrite alkalmazásokat, amelyek a két szervezet a kapcsolják össze több biztonságos helyen. Azt is igen egyszerű, a végfelhasználók toonavigate.

ügyfeleink 97 % jelzik, Azure AD B2B együttműködés nagyon fontos toothem.

![a tortadiagram](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

Korai április 2017 frissítésétől volt körülbelül 3 millió felhasználó alapján már használja az Azure AD B2B együttműködés képességeit. És az Azure AD a szervezeteknek, amelyek több mint 10 olyan felhasználót 23 százaléka már élvező ezeket a képességeket.

## <a name="hello-key-benefits-of-azure-ad-b2b-collaboration-tooyour-organization"></a>Azure AD B2B együttműködés tooyour szervezet hello legfontosabb előnyei

### <a name="work-with-any-user-from-any-partner"></a>Bármely felhasználó bármely partnertől használata

* Partnerek a saját hitelesítő adatok használata

* Az Azure AD partnerek toouse nincsenek követelményei

* Nincsenek külső címtárak vagy bonyolult telepítési szükséges

### <a name="simple-and-secure-collaboration"></a>Egyszerű és biztonságos együttműködési

* Adja meg a hozzáférés tooany vállalati alkalmazás vagy adatokat, kifinomult, az Azure AD által biztosított engedélyezési házirendek alkalmazása közben

* A felhasználók számára könnyen

* Vállalati szintű védelmet biztosít az alkalmazások és adatok

### <a name="no-management-overhead"></a>Egyetlen kezelési terhelés mellett.

* Nincsenek külső fiókjának vagy jelszavának kezelése

* Egyetlen szinkronizálási vagy manuális fiók életciklusának kezelésére

* Nincsenek külső adminisztrációs terhelés

## <a name="you-can-easily-add-b2b-collaboration-users-tooyour-organization"></a>Egyszerűen hozzáadhatja a B2B együttműködés felhasználók tooyour szervezet

Rendszergazdák B2B együttműködés (vendég) felhasználókat adhat hozzá hello [Azure-portálon](https://portal.azure.com).

![vendég felhasználók hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-toobring-their-own-identity"></a>A közreműködők toobring saját identitás engedélyezése

B2B közreműködők is jelentkezzen be az általuk választott identitást. Ha hello felhasználó nem rendelkezik Microsoft-fiókkal vagy egy Azure AD-fiókot – egy során jön létre a számukra zökkenőmentesen hello ajánlat beváltásra.

![bejelentkezési identitás választás](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-tooapplication-and-group-owners"></a>Delegált tooapplication és csoport tulajdonosainak 
Alkalmazás és a csoportházirend-tulajdonosok B2B felhasználókat adhat hozzá közvetlenül az Ön számára legfontosabb, hogy a Microsoft-alkalmazások vagy a nem tooany alkalmazás. Rendszergazdák engedélyt tooadd B2B felhasználók toonon-rendszergazdák számára delegálhatják. A nem rendszergazda használható hello [az Azure AD alkalmazás-hozzáférési Panel](https://myapps.microsoft.com) tooadd B2B együttműködés felhasználók tooapplications vagy csoportokat.

![hozzáférési panel](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![tag hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Engedélyezési házirendek a vállalati tartalom védelme

Kényszerítheti a feltételes hozzáférési szabályzatok, például többtényezős hitelesítés:
- Hello bérlői szinten
- Hello alkalmazás szinten
- Az adott felhasználóknak tooprotect vállalati alkalmazások és adatok

![tag hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-tooeasily-build-applications-tooonboard"></a>Az API-k és minta kód tooeasily alkalmazások tooonboard összeállítása
Kapcsolja a külső partnerek a board módon testreszabott tooyour szervezete igényeinek.

Hello segítségével [B2B együttműködés meghívó API-k](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), testre szabhatja a bevezetési lép, beleértve az előfizetési önkiszolgáló portálokat létrehozása. Az önkiszolgáló portál nyújtunk mintakód [a Githubon](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![regisztrációs portál](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

Azure AD B2B együttműködés érhet hello teljes hatványra emelésének Azure AD tooprotect a partnerkapcsolatok oly módon, hogy a végfelhasználók számára egyszerű és intuitív található. Ezért habozzon, illesztési hello több ezer a külső együttműködés az Azure AD B2B már használó szervezetek!

## <a name="next-steps"></a>Következő lépések

* Rendszergazdai élmény találhatók hello [Azure-portálon](https://portal.azure.com).

* Információk munkavégző lép érhetők el hello [hozzáférési Panel](https://myapps.microsoft.com).

* És visszajelzés, beszélgetéseket, és javaslatokat keresztül hello termékért felelős csoport szerint mindig csatlakozzon a [a Microsoft technikai közösségi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?](active-directory-b2b-admin-add-users.md)
* [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?](active-directory-b2b-iw-add-users.md)
* [hello B2B együttműködés meghívó e-mail hello elemei](active-directory-b2b-invitation-email.md)
* [B2B együttműködés meghívó érvényesítési](active-directory-b2b-redemption-experience.md)
* [Az Azure AD B2B együttműködés licencelés](active-directory-b2b-licensing.md)
* [Hibaelhárítás az Azure Active Directory B2B együttműködés](active-directory-b2b-troubleshooting.md)
* [Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)](active-directory-b2b-faq.md)
* [Az Azure Active Directory B2B együttműködés API és a Testreszabás](active-directory-b2b-api.md)
* [Többtényezős hitelesítés a B2B-együttműködés felhasználói számára](active-directory-b2b-mfa-instructions.md)
* [Adja hozzá a B2B együttműködés felhasználók nélkül](active-directory-b2b-add-user-without-invite.md)
* [Naplózási és jelentéskészítési B2B együttműködés felhasználó](active-directory-b2b-auditing-and-reporting.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
