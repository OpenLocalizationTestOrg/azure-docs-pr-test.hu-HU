---
title: "aaaAzure Active Directory B2B együttműködés API és a Testreszabás |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés a vállalatokon átívelő kapcsolatok üzleti partnerek tooselectively hozzáférés engedélyezése a vállalati alkalmazásokat támogatja."
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
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: 2609971ffa5d2ebc9466c61f4e4af11f5b045ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Az Azure Active Directory B2B együttműködés API és a Testreszabás

Adja meg, hogy meg kívánják-toocustomize hello meghívó folyamat úgy, hogy működik a legjobban, a szervezeteknek számos ügyfél volt. Az API-val most, hogy elvégezhető. [https://Developer.microsoft.com/Graph/docs/API-Reference/V1.0/Resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-hello-invitation-api"></a>Hello meghívó API képességei
hello API hello a következő lehetőségeket nyújtja:

1. A külső felhasználó meghívása *bármely* e-mail címét.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Testre szabhatja, ahová a felhasználók tooland azok elfogadja a meghívót.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Válassza ki a toosend hello szabványos meghívó e-mail velünk keresztül

    ```
    "sendInvitationMessage": true
    ```

  egy üzenet toohello címzettel testre szabható

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. Válassza a toocc: személyek tookeep hello a kapcsolatos a fióknevet a közreműködő hurok.

5. Vagy teljesen testre szabhatja a meghívó és a bevezetési munkafolyamat nem toosend értesítések az Azure AD keresztül kiválasztásával.

    ```
    "sendInvitationMessage": false
    ```

  Ebben az esetben vissza érvényesítési URL-címet a hello API, amely beágyazása e-mail sablont, IM vagy más elosztási módszer az Ön által választott.

6. Végül ha Ön rendszergazda, kiválaszthatja tooinvite hello felhasználó tagja.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Engedélyezési modellt
a következő hitelesítési módok hello hello API is futtatható:

### <a name="app--user-mode"></a>Alkalmazás + felhasználói mód
Ebben a módban személy, aki által használt hello API igények toohave hello engedélyek toobe létrehozása B2B meghívókat.

### <a name="app-only-mode"></a>Egyetlen módja
Alkalmazás csak a környezetben a hello meghívó toosucceed hello User.ReadWrite.All vagy Directory.ReadWrite.All hatókört kell hello alkalmazást.

További információkért lásd: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Most már lehetséges toouse PowerShell tooadd és a meghívás külső felhasználók tooan szervezet könnyen is. Hello parancsmaggal meghívót létrehozni:

```
New-AzureADMSInvitation
```

Használhatja az alábbi beállítások hello:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Is megtekintheti hello meghívó API-hivatkozás [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?](active-directory-b2b-admin-add-users.md)
* [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?](active-directory-b2b-iw-add-users.md)
* [hello B2B együttműködés meghívó e-mail hello elemei](active-directory-b2b-invitation-email.md)
* [B2B együttműködés meghívó érvényesítési](active-directory-b2b-redemption-experience.md)
* [Az Azure AD B2B együttműködés licencelés](active-directory-b2b-licensing.md)
* [Hibaelhárítás az Azure Active Directory B2B együttműködés](active-directory-b2b-troubleshooting.md)
* [Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)](active-directory-b2b-faq.md)
* [Többtényezős hitelesítés a B2B-együttműködés felhasználói számára](active-directory-b2b-mfa-instructions.md)
* [Adja hozzá a B2B együttműködés felhasználók nélkül](active-directory-b2b-add-user-without-invite.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
