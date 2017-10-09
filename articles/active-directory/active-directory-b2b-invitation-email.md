---
title: "hello Azure Active Directory B2B együttműködés meghívó e-mail aaaThe elemeinek |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés meghívó e-mail sablon"
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
ms.openlocfilehash: f4908014d71a63442bbdca2182f54c7a79675a82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-elements-of-hello-b2b-collaboration-invitation-email"></a>hello B2B együttműködés meghívó e-mail hello elemei

Meghívót e-mailek egy kritikus fontosságú összetevők toobring partnerek a board az Azure AD B2B együttműködés felhasználóként. Használhatja őket tooincrease hello címzett megbízhatósági. hozzáadhat érvényességét és közösségi igazoló toohello e-mailek, toomake meg arról, hogy hello címzett érzi hello kiválasztása a Feladatkezelő **Ismerkedés** tooaccept hello meghívó gombra. A bizalmi kapcsolat a kulcs azt jelenti, hogy a megosztás súrlódás tooreduce. És szeretné toomake hello e-mail megjelenését nagyszerű!

![Az Azure AD B2b meghívó e-mail](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-hello-email"></a>Ismertető az üdvözlő e-mail
Vizsgáljuk meg néhány elemet az üdvözlő e-mailek, hogy a legjobb módja toouse képességeit.

### <a name="subject"></a>Tárgy
hello hello e-mail tárgya a következő mintát a következő hello: meghívjuk toohello &lt;tenantname&gt; szervezet

### <a name="from-address"></a>Feladó címe
A LinkedIn-szerű minta a feladó címe hello használjuk.  Meg kell törölje ki hello meghívó, és amely vállalati, és elmagyarázza is, hogy az üdvözlő e-mail érkezik egy Microsoft e-mail címet. hello formátuma: &lt;megjelenített nevével meghívó&gt; a &lt;tenantname&gt; (Microsoft) keresztül <invites@microsoft.com&gt;

### <a name="reply-to"></a>Válasz
hello válasz-tooemail toohello meghívó e-mail érhető el, ha van beállítva, hogy a toohello e-mailt küld egy e-mailek hátsó toohello meghívó a műveletre.

### <a name="branding"></a>Védjegyek
a bérlő az e-mailek használata hello vállalati arculat megjelenítése, amely hello meghívó előfordulhat, hogy állította be a bérlő számára. Ha azt szeretné, hogy ez a funkció előnyeit tootake [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) hello megtudhatja, hogyan vannak tooconfigure azt. hello szalagcím emblémájának hello e-mail jelenik meg. Hajtsa végre a hello a kép mérete és minőségi utasítások [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) a legjobb eredmények elérése érdekében. Ezenkívül hello vállalatnév is megjelennek hello hívás tooaction.

### <a name="call-tooaction"></a>Hívás tooaction
hello hívás tooaction két részből áll: Miért hello címzett kapott üdvözlő levelek, és milyen hello címzett alatt kapcsolatba toodo kapcsolatos foglalja össze.
- hello a "Miért" szakasz is kezelhetők, a következő mintát hello használata: a meghívott tooaccess alkalmazások hello már &lt;tenantname&gt; szervezet

- És "Mi alatt megkérdezi toodo" szakasz hello hello jelenlétét jelzi hello **Ismerkedés** gombra. Amikor hello címzett nélkül hello meghívókat hozzá lett adva, erre a gombra kattintva nem jelenik meg.

### <a name="inviters-information"></a>A meghívó a információk
hello meghívó megjelenített név hello e-mail tartalmazza. És Ezenkívül ha beállította az Azure AD-fiókot a profilkép, e-mailek meghívása hello fogja tartalmazni, valamint, hogy a kép. Mindkét tervezett tooincrease hello e-mailben abban, hogy a címzett vannak.

Ha még nem állított profilkép, hello meghívó monogramja hello kép helyett egy ikon jelenik meg:

  ![Hello meghívó monogramja megjelenítése](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a>Törzs
hello törzsében üdvözlőüzenetére adott hello meghívó composes vagy hello meghívó API keresztül. Így azt nem dolgozza fel az HTML-címkék biztonsági okokból egy területre.

### <a name="footer-section"></a>Élőláb szakasz
hello élőláb hello Microsoft vállalatának arculatát tartalmaz, és lehetővé teszi, hogy a hello címzettje, ha a nem figyelt alias hello e-mail elküldése. Bizonyos esetekben:

- hello meghívó e-mail cím nem rendelkezik hello bérleti meghívása

  ![Meghívó képe nem rendelkezik egy e-mail címet hello bérleti meghívása](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- hello címzett tooredeem hello meghívó nem szükséges.

  ![Ha a címzett nem szükséges tooredeem meghívó](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?](active-directory-b2b-admin-add-users.md)
* [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?](active-directory-b2b-iw-add-users.md)
* [B2B együttműködés meghívó érvényesítési](active-directory-b2b-redemption-experience.md)
* [Az Azure AD B2B együttműködés licencelés](active-directory-b2b-licensing.md)
* [Hibaelhárítás az Azure Active Directory B2B együttműködés](active-directory-b2b-troubleshooting.md)
* [Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)](active-directory-b2b-faq.md)
* [Az Azure Active Directory B2B együttműködés API és a Testreszabás](active-directory-b2b-api.md)
* [Többtényezős hitelesítés a B2B-együttműködés felhasználói számára](active-directory-b2b-mfa-instructions.md)
* [Adja hozzá a B2B együttműködés felhasználók nélkül](active-directory-b2b-add-user-without-invite.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
