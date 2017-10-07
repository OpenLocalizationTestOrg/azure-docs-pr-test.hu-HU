---
title: "az Azure AD-alkalmazások aaaDevelop |} Microsoft Docs"
description: "Az informatikai szakembereknek hello verziójához, ez a cikk útmutatást nyújt a Azure-alkalmazások integrálása az Active Directoryval."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: dd69f2bc-37c5-457c-857d-27acb84267fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2924be752af0be2843b1d9b74d9ec446d3fe1ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Az Azure Active Directory-üzleti alkalmazások fejlesztéséhez
Ez az útmutató áttekintést az Azure Active Directory (AD) .hello-üzletági (LoB) alkalmazások fejlesztéséhez szánt célközönségét az Active Directory vagy Office 365 globális rendszergazdák.

## <a name="overview"></a>Áttekintés
Az Azure AD integrált alkalmazások lehetőséget ad a felhasználók a szervezet egyszeri bejelentkezéshez és az Office 365 a. Mivel hello alkalmazás hello hitelesítési házirend hello alkalmazás vezérlésére biztosít az Azure AD lehetőséget. További információk a feltételes hozzáférés, és hogyan tooprotect alkalmazások és a többtényezős hitelesítés (MFA): toolearn [konfigurálása hozzáférési szabályok](active-directory-conditional-access-azuread-connected-apps.md).

Az alkalmazás toouse Azure Active Directoryban regisztrálja. Hello alkalmazás regisztrálása azt jelenti, hogy a fejlesztők az Azure AD tooauthenticate felhasználóinak használja-e, és kérjen hozzáférési toouser erőforrásokat, például az e-mailek, naptár és dokumentumok.

Bármelyik tag a könyvtár (nem vendégek) regisztrálhatja az alkalmazás, más néven a *alkalmazásobjektum létrehozása*.

Az alkalmazásnak lehetővé teszi, hogy bármely felhasználó toodo hello következő:

* Az alkalmazás számára az Azure AD felismerhető identitás lekérése
* Egy vagy több titkok/kulcsok alkalmazás hello is használja a maga tooauthenticate tooAD
* Hello alkalmazás márka hello Azure portál, amely egy egyéni nevet, embléma, stb.
* Alkalmazza az Azure AD hitelesítési szolgáltatások tootheir alkalmazás, beleértve:

  * Szerepköralapú hozzáférés-vezérlés (RBAC)
  * Az Azure Active Directory oAuth-engedélyezési kiszolgálóként (biztonságos hello alkalmazás által az API-k)
* Szükséges engedélyekkel, beleértve a várt módon hello alkalmazás toofunction szükséges deklarálható:

      - Alkalmazásengedélyek (csak a globális rendszergazdák). Példa: egy másik Azure AD alkalmazás vagy szerepkör tagsági relatív tooan Azure Resource, erőforráscsoport, szerepkör tagjának kell lennie, vagy az előfizetéshez
      - Delegált engedélyek (felhasználói). Például: az Azure AD-bejelentkezés, és olvasási profil

> [!NOTE]
> Alapértelmezés szerint minden tag kérelmet tud regisztrálni. Hogyan toorestrict engedélyek alkalmazások toospecific tagok, a regisztrációhoz: toolearn [hogyan alkalmazások felvétele tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

Ez milyen Ön hello globális rendszergazda, kell toodo toohelp fejlesztők ellenőrizze az alkalmazás készen áll a termelési:

* Hozzáférési szabályok (hozzáférési házirend vagy MFA) konfigurálása
* Hello app toorequire felhasználói hozzárendelésének konfigurálása és a felhasználók hozzárendelése
* Ne jelenjen meg többé hello alapértelmezett felhasználói hozzájárulás élmény

## <a name="configure-access-rules"></a>Hozzáférési szabályok konfigurálása
Alkalmazásonkénti hozzáférési szabályok tooyour SaaS-alkalmazások konfigurálása. Például többtényezős hitelesítés megkövetelése, vagy a hozzáférés toousers engedélyezése csak a megbízható hálózatokon. Ez hello részletei érhetők el hello dokumentum [konfigurálása hozzáférési szabályok](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-hello-app-toorequire-user-assignment-and-assign-users"></a>Hello app toorequire felhasználói hozzárendelésének konfigurálása és a felhasználók hozzárendelése
Alapértelmezés szerint felhasználók folyik a hozzárendelése nem férhet hozzá az alkalmazásokhoz. Ha hello alkalmazás közzététele a szerepköröket, vagy ha azt szeretné, hogy a felhasználó hozzáférési panel hello alkalmazás tooappear, felhasználó-hozzárendelés elvégzéséhez szükséges.

[Felhasználó-hozzárendelés igénylése](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Ha az Azure AD Premium vagy a nagyvállalati mobilitási csomag (EMS) előfizető, erősen ajánlott csoportok használatával. Csoportok toohello alkalmazás hozzárendelése lehetővé teszi toodelegate folyamatban lévő access management toohello hello csoport tulajdonosa. Hello csoport létrehozása, vagy kérje meg a hello felelős fél a szervezet toocreate hello csoportjában a csoport felügyeleti funkció segítségével.

[Felhasználók hozzárendelése tooan alkalmazás](active-directory-applications-guiding-developers-assigning-users.md)  
[Csoportok tooan alkalmazás hozzárendelése](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Felhasználói hozzájárulás mellőzése
Alapértelmezés szerint minden felhasználó végighalad a hozzájárulási élmény toosign a. hello hozzájárulási, kérni a felhasználó toogrant engedélyeivel tooan alkalmazás lehet disconcerting felhasználók számára nem ismeri az ilyen döntések meghozatalában.

A megbízható alkalmazások egyszerűbbé teheti az hello felhasználói élmény küldőnek toohello alkalmazás által a szervezet nevében.

További információ a felhasználói hozzájárulás és hello hozzájárulási tapasztalhat az Azure-ban, lásd: [alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Biztonságos távoli hozzáférés tooon helyszíni alkalmazások az Azure AD alkalmazásproxy engedélyezése](active-directory-application-proxy-get-started.md)
* [Azure feltételes hozzáférés előzetes verziója SaaS-alkalmazásokhoz](active-directory-conditional-access-azuread-connected-apps.md)
* [Az Azure AD hozzáférési tooapps kezelése](active-directory-managing-access-to-apps.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
