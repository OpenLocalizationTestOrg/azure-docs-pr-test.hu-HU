---
title: "aaaCompare B2B együttműködés és az Azure Active Directory B2C |} Microsoft Docs"
description: "Mi az Azure Active Directory B2B együttműködés és az Azure AD B2C hello különbségének?"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 34d88b9a7d023e077568e8df3d5e1610ae05b361
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Hasonlítsa össze a B2B együttműködés és az Azure Active Directory B2C

Azure Active Directory (Azure AD) B2B együttműködés, mind az Azure AD B2C lehetővé teszik külső felhasználókkal toowork Azure AD-ben. De annak tegye összevetése?


B2B együttműködés képességek |     Az Azure AD B2C önálló ajánlat
-------- | --------
Szánt: a szervezeteknek, amelyek szeretné, hogy egy fiókpartner-szervezet, függetlenül attól, identitásszolgáltató toobe képes tooauthenticate felhasználóit. | Szánt: a mobil ügyfelek és a web apps, hogy meghívása személyeket, intézményi vagy a szervezeti felhasználók az Azure AD-be.
Támogatott identitások: az alkalmazottak munkahelyi vagy iskolai fiókok, a munkahelyi vagy iskolai fiókokkal partnerek vagy akármilyen e-mail címet. Hamarosan toosupport közvetlen összevonási.  | Támogatott identitások: Identitásrendszerében a felhasználók a helyi alkalmazások fiókok (bármilyen e-mail címet vagy a felhasználó neve), vagy bármely támogatott az közvetlen összevonási közösségi identitását.
Amely directory hello partner felhasználók szerepelnek: hello külső szervezeti felhasználók kezelése az erőforráspartner kifejezetten hello alkalmazottak, de a megjegyzésekkel ellátott könyvtárába. Ezek kezelhetők hello azonos alkalmazottak, mint ahogyan lehet hozzáadott toohello ugyanahhoz a csoporthoz, és így tovább  | Amelyek directory hello vevő felhasználó entitások: hello alkalmazás könyvtárában. Külön felügyelettel történik könyvtárból hello szervezetének dolgozó és a partner (ha van ilyen.
Az egyszeri bejelentkezés (SSO) tooall Azure AD-kompatibilis alkalmazások használata támogatott. Például megadhatja a hozzáférés tooOffice 365 vagy a helyszíni alkalmazások és tooother SaaS-alkalmazások például Salesforce vagy Workday.  |  Egyszeri bejelentkezés toocustomer tulajdonában lévő alkalmazások hello Azure AD B2C-bérlőkön belül támogatott. Egyszeri bejelentkezés tooOffice 365 vagy tooother Microsoft és nem Microsoft SaaS - alkalmazások nem támogatott.
Partner életciklus: hello gazdagép/meghívása szervezet felügyeli.  | Ügyfél életciklus: önkiszolgáló vagy hello alkalmazás által felügyelt.
Biztonsági házirend és megfelelőség: hello gazdagép/meghívása szervezet felügyeli.  | Biztonsági házirend és megfelelőség: hello alkalmazás kezeli.
Márkajelzési: Állomás/meghívása szervezet márka szolgál.  |    Branding: Alkalmazás kezeli. Általában általában a vállalati arculattal rendelkező hello szervezet áttűnés hello háttérben történő toobe termék.
További információ: [blogbejegyzés](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [dokumentáció](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  | További információ: [termékoldalára](https://azure.microsoft.com/en-us/services/active-directory-b2c/), [dokumentáció](https://docs.microsoft.com/en-us/azure/active-directory-b2c/)


### <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B együttműködés felhasználó tulajdonságai](active-directory-b2b-user-properties.md)
* [B2B együttműködés felhasználói tooa szerepkör hozzáadása](active-directory-b2b-add-guest-to-role.md)
* [B2B együttműködés meghívókat delegálása](active-directory-b2b-delegate-invitations.md)
* [Dinamikus csoportok és a B2B együttműködés](active-directory-b2b-dynamic-groups.md)
* [B2B együttműködés SaaS-alkalmazások konfigurálása](active-directory-b2b-configure-saas-apps.md)
* [B2B együttműködés felhasználói jogkivonatokhoz](active-directory-b2b-user-token.md)
* [B2B együttműködés felhasználói jogcímek leképezése](active-directory-b2b-claims-mapping.md)
* [Külső Office 365-megosztás](active-directory-b2b-o365-external-user.md)
* [B2B együttműködés aktuális korlátozásai](active-directory-b2b-current-limitations.md)
* [Támogatási B2B együttműködés beolvasása](active-directory-b2b-support.md)
