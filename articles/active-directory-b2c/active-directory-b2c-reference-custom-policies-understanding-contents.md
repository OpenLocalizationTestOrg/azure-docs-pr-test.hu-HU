---
title: "Az Azure Active Directory B2C: Hello alapszintű csomag egyéni házirendjeinek ismertetése |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C egyéni házirendek"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 3484e8cc6fa6a9d57c2aa14c0cc9616065892d10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-custom-policies-of-hello-azure-ad-b2c-custom-policy-starter-pack"></a>Egyéni házirendek hello hello Azure AD B2C egyéni házirend alapszintű csomag ismertetése

Ez a rész felsorolja a hello hello B2C_1A_base házirend minden hello core elemei **alapszintű csomag** és, hogy a rendszer elkészítéséhez használja megfelelő saját házirendeket a hello hello örökléssel tartalomkészítéshez *B2C_1A_base_ bővítmények házirend*.

Azt több különösen mutatja be már meg van adva hello jogcím-típusokat, a jogcímek átalakítása, tartalom definíciók, az a műszaki profil Jogcímszolgáltatók, és alapvető felhasználói utak hello.

> [!IMPORTANT]
> A Microsoft nem vállal semmilyen kifejezett, kifejezett vagy vélelmezett, továbbiakban megadott tekintetben toohello adatokkal. GA idő, illetve után a módosítások vihetők bármikor GA időpont előtt.

Saját házirendek és a hello B2C_1A_base_extensions házirend felülírja ezeket a definíciókat, és a szülő házirend kiterjesztése újak megadásával, igény szerint.

alapvető összetevőjét hello hello *B2C_1A_base házirend* jogcímtípusok, a jogcímek átalakítása és a tartalom definíciókat. Ezeket az elemeket is ki vannak téve toobe hello mellett a saját a házirendeket, valamint a hivatkozott *B2C_1A_base_extensions házirend*.

## <a name="claims-schemas"></a>Jogcímek sémák

A jogcím-sémák három részből áll:

1.  Első szakasz hello minimális jogcímeket, amelyek szükségesek a hello felhasználói utak toowork megfelelően felsoroló.
2.  Második szakasz listák hello lekérdezési karakterlánc paraméter szükséges jogcímeket és a más különleges paraméterek toobe átadott tooother Jogcímszolgáltatók, különösen a hitelesítéshez login.microsoftonline.com. **Ne módosítsa ezeket a jogcímeket**.
3.  És végül a hello felhasználótól összegyűjthetők további, opcionális jogcímeket felsoroló harmadik szakasz hello könyvtárban tárolja, és a bejelentkezés során küldött jogkivonatokat. Új jogcím típusa toobe hello felhasználói gyűjtött és/vagy küldi hello token ebben a szakaszban lehet hozzáadni.

> [!IMPORTANT]
> hello jogcímek séma egyes jogcímek, például a jelszavak és a felhasználónevek korlátozásokat tartalmaz. hello megbízható keretrendszer (TF) házirend más jogcím-szolgáltató kezeli az Azure AD és a korlátozások vannak modellezni hello prémium házirendben. Egy házirend módosított tooadd további korlátozásokat, vagy egy másik jogcímszolgáltató használandó hitelesítő adatok tárolása, amelynek a saját korlátozások.

hello elérhető jogcímtípusok alább láthatók.

### <a name="claims-that-are-required-for-hello-user-journeys"></a>A hello felhasználói utak által igényelt jogcímeket

a következő jogcímeket hello szükségesek a felhasználó utak toowork megfelelően:

| Jogcím típusa | Leírás |
|-------------|-------------|
| *Felhasználói azonosítóját* | Felhasználónév |
| *signInName* | Jelentkezzen be neve |
| *a tenantId* | Bérlő azonosítóját az Azure AD B2C prémium hello felhasználói objektum |
| *Objektumazonosító* | Objektumazonosító (ID) az Azure AD B2C prémium hello felhasználói objektum |
| *jelszó* | Jelszó |
| *ÚjJelszó* | |
| *reenterPassword* | |
| *passwordPolicies* | A jelszóházirendek használják az Azure AD B2C prémium toodetermine jelszó erőssége, a lejárat, stb. |
| *Sub* | |
| *alternativeSecurityId* | |
| *identityProvider* | |
| *displayName* | |
| *strongAuthenticationPhoneNumber* | A felhasználó telefonszáma |
| *Verified.strongAuthenticationPhoneNumber* | |
| *e-mailek* | E-mail cím használható használt toocontact hello felhasználó |
| *signInNamesInfo.emailAddress* | E-mail címet, amely a felhasználó hello használhatja a toosign |
| *otherMails* | E-mail címek, amelyek lehetnek használt toocontact hello felhasználó |
| *userPrincipalName* | Az Azure AD B2C prémium hello tárolt felhasználónév |
| *upnUserName* | Egyszerű felhasználónév létrehozásához felhasználónév |
| *mailNickName* | Mail nick felhasználónév tárolt hello Azure AD B2C-támogatás |
| *Új_felhasználó* | |
| *végre SelfAsserted-bemenet* | Jogcímet, amely megadja, hogy attribútumok gyűjtötte a program a hello felhasználó |
| *végre PhoneFactor-bemenet* | Jogcímet, amely megadja, hogy egy új telefonszámot gyűjtötte a program a hello felhasználó |
| *authenticationSource* | Megadja, hogy hello felhasználó közösségi identitásszolgáltató, login.microsoftonline.com vagy helyi fiók hitelesítési |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>A lekérdezési karakterlánc és más különleges paraméterei szükséges jogcímeket

hello következő jogcímek olyan speciális paramétert (beleértve az egyes lekérdezési karakterlánc paraméterek) tooother Jogcímszolgáltatók a szükséges toopass:

| Jogcím típusa | Leírás |
|-------------|-------------|
| *nux* | Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com |
| *a hálózati csatlakozási Segéd* | Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com |
| *parancssor* | Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com |
| *mkt* | Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com |
| *LC* | Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com |
| *grant_type* | Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com |
| *hatókör* | Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com |
| *client_id* | Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com |
| *objectIdFromSession* | Hello alapértelmezett munkamenet felügyeleti szolgáltató tooindicate, objektumazonosító: hello által biztosított paraméter egy egyszeri bejelentkezési munkamenet beolvasása |
| *isActiveMFASession* | Hello MFA munkamenet felügyeleti tooindicate által biztosított paraméter hello rendelkezik aktív MFA munkamenet |

### <a name="additional-optional-claims-that-can-be-collected"></a>További (nem kötelező) jogcímeket is

hello következő hello felhasználóktól összegyűjthetők további jogcímeket hello könyvtárban tárolja, és hello tokent küldött jogcímek. Mielőtt leírtak további jogcímek toothis lista lehet hozzáadni.

| Jogcím típusa | Leírás |
|-------------|-------------|
| *givenName* | A megadott felhasználónév (más néven Keresztnév) |
| *Vezetéknév* | Felhasználó vezetékneve (más néven Családnév vagy vezetéknevet) |
| *Extension_picture* | Felhasználó társadalombiztosítási kép |

## <a name="claim-transformations"></a>A jogcímek átalakításához

hello érhető el a jogcímek átalakításához alább láthatók.

| Jogcím-átalakítást | Leírás |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>Tartalom definíciók

Ez a szakasz ismerteti a definíciók tartalom hello hello már deklarálva *B2C_1A_base* házirend. A tartalom definíciók téve toobe hivatkozott, felül, illetve szükség esetén hello mellett a saját a házirendeket, valamint a kiterjesztett *B2C_1A_base_extensions* házirend.

| Jogcím-szolgáltató | Leírás |
|-----------------|-------------|
| *Facebook* | |
| *Helyi fiók SignIn* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Önkiszolgáló magas* | |
| *Helyi fiók* | |
| *Munkamenet-kezelés* | |
| *A házirendmotor Trustframework* | |
| *TechnicalProfiles* | |
| *Jogkivonatot kibocsátó* | |

## <a name="technical-profiles"></a>Műszaki profilok

Ez a szakasz ábrázol hello műszaki profilok / hello a jogcímszolgáltató már deklarálva *B2C_1A_base* házirend. A műszaki profilok téve toobe további hivatkozott, felül, illetve szükség esetén hello mellett a saját a házirendeket, valamint a kiterjesztett *B2C_1A_base_extensions* házirend.

### <a name="technical-profiles-for-facebook"></a>Műszaki profilokat a Facebook-on

| Műszaki profil | Leírás |
|-------------------|-------------|
| *Facebook-OAUTH* | |

### <a name="technical-profiles-for-local-account-signin"></a>A helyi fiókkal bejelentkezik műszaki profilok

| Műszaki profil | Leírás |
|-------------------|-------------|
| *Bejelentkezési nem interaktív* | |

### <a name="technical-profiles-for-phone-factor"></a>A Phone Factor műszaki profilok

| Műszaki profil | Leírás |
|-------------------|-------------|
| *PhoneFactor-bemenet* | |
| *PhoneFactor-InputOrVerify* | |
| *PhoneFactor-ellenőrzése* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Az Azure Active Directory műszaki profilok

| Műszaki profil | Leírás |
|-------------------|-------------|
| *Az AAD-közös* | Műszaki profil benne hello más AAD-xxx műszaki profilok |
| *Az AAD-UserWriteUsingAlternativeSecurityId* | A közösségi bejelentkezések során műszaki profil |
| *Az AAD-UserReadUsingAlternativeSecurityId* | A közösségi bejelentkezések során műszaki profil |
| *Az AAD-UserReadUsingAlternativeSecurityId-NoError* | A közösségi bejelentkezések során műszaki profil |
| *Az AAD-UserWritePasswordUsingLogonEmail* | A helyi fiókok műszaki profil |
| *Az AAD-UserReadUsingEmailAddress* | A helyi fiókok műszaki profil |
| *Az AAD-UserWriteProfileUsingObjectId* | Műszaki profil használatával objectId felhasználói rekord frissítéséhez |
| *Az AAD-UserWritePhoneNumberUsingObjectId* | Műszaki profil használatával objectId felhasználói rekord frissítéséhez |
| *Az AAD-UserWritePasswordUsingObjectId* | Műszaki profil használatával objectId felhasználói rekord frissítéséhez |
| *Az AAD-UserReadUsingObjectId* | Műszaki profil az használt tooread adatai után a felhasználók hitelesítése |

### <a name="technical-profiles-for-self-asserted"></a>Az önkiszolgáló magas műszaki profilok

| Műszaki profil | Leírás |
|-------------------|-------------|
| *SelfAsserted-társadalombiztosítási* | |
| *SelfAsserted-ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Helyi fiók műszaki profilok

| Műszaki profil | Leírás |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Műszaki profilokat a munkamenet-kezelés

| Műszaki profil | Leírás |
|-------------------|-------------|
| *SM-Noop* | |
| *SM-AAD-BEN* | |
| *SM-SocialSignup* | Profil neve alatt használt toodisambiguate AAD munkamenet között bejelentkezési fel, és jelentkezzen be |
| *SM-SocialLogin* | |
| *SM-MFA* | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a>Trustframework házirend motor TechnicalProfiles műszaki profilok

Jelenleg nincsenek technikai profilok hello definiált **Trustframework házirend motor TechnicalProfiles** jogcím-szolgáltató.

### <a name="technical-profiles-for-token-issuer"></a>Jogkivonatot kibocsátó műszaki profilok

| Műszaki profil | Leírás |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>Felhasználói utak

Ez a szakasz ábrázol hello felhasználói utak hello már deklarálva *B2C_1A_base* házirend. Ezek az felhasználói utak téve toobe további hivatkozott, felül, illetve szükség esetén hello mellett a saját a házirendeket, valamint a kiterjesztett *B2C_1A_base_extensions* házirend.

| Felhasználói út | Leírás |
|--------------|-------------|
| *Regisztráció* | |
| *Bejelentkezés* | |
| *SignUpOrSignIn* | |
| *EditProfile* | |
| *PasswordReset* | |
