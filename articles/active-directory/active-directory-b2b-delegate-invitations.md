---
title: "az Azure Active Directory B2B együttműködés aaaDelegate meghívókat |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés felhasználó tulajdonságainak konfigurálható"
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
ms.openlocfilehash: c0122d6f60d494c6e251c41d947dc254ea887620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Az Azure Active Directory B2B együttműködés meghívókat delegálása

Az Azure Active Directory (Azure AD) üzleti vállalatközi (B2B) együttműködés nincs toobe egy globális rendszergazdai toosend meghívókat. Ehelyett házirendekkel, és amelynek szerepkörök lehetővé teszik toosend meghívókat meghívókat toousers delegálása. Egy fontos új módon toodelegate Vendég felhasználó meghívókat hello Vendég meghívó szerepkör keresztül történik.

## <a name="guest-inviter-role"></a>Vendég meghívó szerepkör
Azt is hozzárendelheti hello felhasználói tooGuest meghívó szerepkör toosend meghívókat. Nincs hello globális rendszergazdai szerepkör toosend meghívókat toobe tagja. Alapértelmezés szerint rendszeres felhasználók is hívhat meg a meghívás API hello egy globális rendszergazdai tiltja le a rendszeres felhasználók meghívókat. A felhasználó is hívhat meg hello API hello Azure-portálon vagy a PowerShell használatával.

Íme egy példa bemutatja, hogyan toouse PowerShell tooadd toohello Vendég meghívó felhasználói szerepkört:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Személyek kérhetnek

![Vezérlő hogyan tooinvite](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

Az Azure AD B2B együttműködés egy Bérlői rendszergazda következő meghívó házirendek hello állíthatja be:

- Kapcsolja ki a meghívókat
- Csak a rendszergazdák és a hello Vendég meghívó szerepet betöltő felhasználók kérhetnek
- Rendszergazdák, a hello Vendég meghívó szerepkör és a tagjainak meghívása
- Minden felhasználó, beleértve a Vendégek, hívhat

Alapértelmezés szerint bérlők túl van beállítva 4. (Az összes olyan felhasználót, beleértve a Vendégek, B2B felhasználók kérhetnek.)

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B együttműködés felhasználó tulajdonságai](active-directory-b2b-user-properties.md)
* [B2B együttműködés felhasználói tooa szerepkör hozzáadása](active-directory-b2b-add-guest-to-role.md)
* [Dinamikus csoportok és a B2B együttműködés](active-directory-b2b-dynamic-groups.md)
* [B2B együttműködés kód és a PowerShell-példák](active-directory-b2b-code-samples.md)
* [B2B együttműködés SaaS-alkalmazások konfigurálása](active-directory-b2b-configure-saas-apps.md)
* [B2B együttműködés felhasználói jogkivonatokhoz](active-directory-b2b-user-token.md)
* [B2B együttműködés felhasználói jogcímek leképezése](active-directory-b2b-claims-mapping.md)
* [Külső Office 365-megosztás](active-directory-b2b-o365-external-user.md)
* [B2B együttműködés aktuális korlátozásai](active-directory-b2b-current-limitations.md)
