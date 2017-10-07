---
title: "Azure Active Directory B2B együttműködés aaaTroubleshooting |} Microsoft Docs"
description: "Jogorvoslatok Azure Active Directory B2B együttműködés kapcsolatos általános problémák"
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
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 6fcfd7e543cd7bb833225f8aa56e332e7a989faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Hibaelhárítás az Azure Active Directory B2B együttműködés

Az alábbiakban néhány jogorvoslatok kapcsolatos Azure Active Directory (Azure AD) B2B együttműködés gyakori problémákat.


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-hello-people-picker"></a>Tudom felvételét a külső felhasználó, de ne legyenek láthatóak a globális címlista vagy hello személyek kiválasztása

Azokban az esetekben, ahol a külső felhasználók nincsenek feltöltve adattal hello listában hello objektum eltarthat néhány percig tooreplicate.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>A B2B Vendég felhasználó nem látható a SharePoint Online/OneDrive személyek kiválasztása 
 
hello képességét toosearch a hello SharePoint Online (SPO) személyek kiválasztása a meglévő vendégfelhasználók alapértelmezett toomatch örökölt működése értéke OFF.

Ez a szolgáltatás "ShowPeoplePickerSuggestionsForGuestUsers" szintű hello bérlő és a webhelycsoport beállítását hello használatával engedélyezheti. Beállíthatja a hello Set-SPOTenant és a Set-SPOSite-parancsmagok használatával, amelyek lehetővé teszik a tagok hello szolgáltatás toosearch hello címtárban szereplő összes meglévő Vendég felhasználó. Hello bérlői hatókörhöz változásai nem befolyásolják a már kiépített SPO helyek.

## <a name="invitations-have-been-disabled-for-directory"></a>Könyvtár meghívókat le van tiltva

Ha értesítést kap, hogy nem rendelkezik engedélyekkel tooinvite felhasználók, győződjön meg arról, hogy a felhasználói fiók jogosult tooinvite külső felhasználók a felhasználói beállítások:

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

Ha nemrég módosította ezeket a beállításokat, vagy hozzárendelt hello Vendég meghívó szerepkör tooa felhasználó, előfordulhat, a 15-60 perc késleltetéssel a hello módosítások érvénybe lépéséhez.

## <a name="hello-user-that-i-invited-is-receiving-an-error-during-redemption"></a>I meghívott hello felhasználó hibaüzenetet kap érvényesítési során

Gyakori hibák a következők:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Meghívott rendszergazda nem engedélyezte a bérlőben jöjjön létre EmailVerified felhasználók

Amikor felhasználók, akiknek a szervezet által használt Azure Active Directory, de ha hello az adott felhasználói fiók nem létezik (például hello felhasználó nem létezik az Azure AD contoso.com). a contoso.com hello rendszergazda is tartalmaznak egy házirend meggátolja, hogy a felhasználók létrehozása folyamatban. hello felhasználói kell egyeztetni a rendszergazda toodetermine, ha a külső felhasználók számára engedélyezett. hello külső felhasználó rendszergazda esetleg tooallow, e-mailek ellenőrzése a tartományukba tartozó felhasználók (Ez [cikk](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) az e-mailek ellenőrzése felhasználók).

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Külső felhasználó nem létezik már egy összevont tartományban

Ha hello felhasználó már nem létezik az Azure Active Directory összevonási hitelesítést használ, hello felhasználót nem hívhat meg.

a probléma hello tooresolve külső felhasználó rendszergazda hello felhasználói fiók tooAzure Active Directory kell szinkronizálnia.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Hogyan biztosítja a\#", amely nincs általában érvénytelen karakter, és az Azure AD sync?

"\#" nem egy foglalt karakter UPN-EK az Azure AD B2B együttműködés vagy külső felhasználók számára, mert hello meghívott fiók user@contoso.com user_contoso.com# válikEXT@fabrikam.onmicrosoft.com. Ezért \# az UPN-EK a helyszíni érkező nem engedélyezett toosign toohello Azure-portálon. 

## <a name="i-receive-an-error-when-adding-external-users-tooa-synchronized-group"></a>A megjelenő hibaüzenet szinkronizáláskor a külső felhasználók tooa hozzáadása csoporthoz

Külső felhasználók felveheti túl csak "hozzárendelt" vagy "Biztonság" csoportok és a nem a rendszer toogroups értékűre a helyszínen.

## <a name="my-external-user-did-not-receive-an-email-tooredeem"></a>A külső felhasználó nem kapta meg az e-mailek tooredeem

hello meghívott érdemes egyeztetni az internetszolgáltató által biztosított, vagy a levélszemét szűrő tooensure, amely a következő cím hello engedélyezve:Invites@microsoft.com

## <a name="i-notice-that-hello-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>I figyelje meg, hogy egyéni üdvözlőüzenetére nem kérdezhető le a meghívó üzeneteket mellékelt néha

az adatvédelmi törvényekkel toocomply, az API-k nem tartalmaznak egyéni üzenetek hello e-mail ajánlati során:

- hello meghívó hello meghívása a bérlő nem rendelkezik egy e-mail címet
- Amikor egy App Service principal küldi hello meghívó

Ha ebben a forgatókönyvben fontos tooyou, ne jelenjen meg többé az API-t meghívó e-mail, és küldje el az Ön által választott hello e-mail mechanizmus. Tekintse meg a szervezet védőt toomake meg arról, hogy minden e-mailek küldésekor, így is adatvédelmi törvényekkel összhangban.

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?](active-directory-b2b-admin-add-users.md)
* [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?](active-directory-b2b-iw-add-users.md)
* [hello B2B együttműködés meghívó e-mail hello elemei](active-directory-b2b-invitation-email.md)
* [B2B együttműködés meghívó érvényesítési](active-directory-b2b-redemption-experience.md)
* [Az Azure AD B2B együttműködés licencelés](active-directory-b2b-licensing.md)
* [Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)](active-directory-b2b-faq.md)
* [Az Azure Active Directory B2B együttműködés API és a Testreszabás](active-directory-b2b-api.md)
* [Többtényezős hitelesítés a B2B-együttműködés felhasználói számára](active-directory-b2b-mfa-instructions.md)
* [Adja hozzá a B2B együttműködés felhasználók nélkül](active-directory-b2b-add-user-without-invite.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
