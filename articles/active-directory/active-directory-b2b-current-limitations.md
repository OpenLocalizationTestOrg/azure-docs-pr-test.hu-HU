---
title: "az Azure Active Directory B2B együttműködés aaaLimitations |} Microsoft Docs"
description: "Azure Active Directory B2B együttműködés aktuális korlátozásai"
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 322081f32fbacfe67ee1300993c7df1870e498bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Azure AD B2B együttműködés korlátozásai
Az Azure Active Directory (Azure AD) B2B együttműködés jelenleg ebben a cikkben ismertetett tulajdonos toohello korlátozások.

## <a name="possible-double-multi-factor-authentication"></a>Lehetséges dupla többtényezős hitelesítés
Az Azure AD B2B kényszerítheti a többtényezős hitelesítés hello erőforrás-szervezetben (szervezet meghívása hello). Ez a megközelítés hello okait részletezi [feltételes hozzáférés a B2B együttműködés felhasználók](active-directory-b2b-mfa-instructions.md). Ha egy partnert már többtényezős hitelesítés beállítása, és azt, a felhasználóknak problémájuk lehet tooperform hello hitelesítési egyszer a otthoni szervezetek, majd ismét az Öné.

## <a name="instant-on"></a>Azonnali
Hello B2B együttműködés adatfolyamok azt felhasználók toohello könyvtár hozzáadása, és dinamikusan frissítse azokat a meghívó érvényesítési és alkalmazás-hozzárendelés, stb. hello frissítések és az írás szokásos fordul elő egy címtárpéldány, és minden példányára kell replikálni. Replikáció befejezése után minden példány frissítése. Néha amikor hello objektum írt, vagy egy példány frissítése és a hello hívás tooretrieve Ez az objektum nem tooanother példány replikációs késések fordulnak elő akkor fordulhat elő. Ebben az esetben, ha frissítse, vagy próbálkozzon újra a toohelp. Ha az alkalmazást az API, majd az ismételt próbálkozás egy biztonsági ki van egy megfelelő, a védelem gyakorlat tooalleviate probléma.

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B együttműködés felhasználó tulajdonságai](active-directory-b2b-user-properties.md)
* [B2B együttműködés felhasználói tooa szerepkör hozzáadása](active-directory-b2b-add-guest-to-role.md)
* [B2bB együttműködés meghívókat delegálása](active-directory-b2b-delegate-invitations.md)
* [Dinamikus csoportok és a B2B együttműködés](active-directory-b2b-dynamic-groups.md)
* [B2B együttműködés kód és a PowerShell-példák](active-directory-b2b-code-samples.md)
* [B2B együttműködés SaaS-alkalmazások konfigurálása](active-directory-b2b-configure-saas-apps.md)
* [B2B együttműködés felhasználói jogkivonatokhoz](active-directory-b2b-user-token.md)
* [B2B együttműködés felhasználói jogcímek leképezése](active-directory-b2b-claims-mapping.md)
* [Külső Office 365-megosztás](active-directory-b2b-o365-external-user.md)
