---
title: "Az Azure Active Directory B2C: Token, munkamenet és egyszeri bejelentkezés konfigurálása |} Microsoft Docs"
description: "Token, munkamenet és egyszeri bejelentkezés konfigurálása az Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: e78e6344-0089-49bf-8c7b-5f634326f58c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: swkrish
ms.openlocfilehash: 63325ed97a7363723c97ee3a992046ebb5592662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Az Azure Active Directory B2C: Token, munkamenet és egyszeri bejelentkezés konfigurálása
Ez a funkció lehetővé teszi részletes vezérlés, a egy [házirend alapon](active-directory-b2c-reference-policies.md), a:

1. Azure Active Directory (Azure AD) B2C által kibocsátott biztonsági jogkivonatok élettartama.
2. A webes alkalmazás munkamenetek kezeli az Azure AD B2C élettartamának.
3. Az Azure AD B2C által kibocsátott hello biztonsági jogkivonatokba fontos jogcímek formátumát.
4. Egyszeri bejelentkezés (SSO) viselkedés több alkalmazások és házirendek a B2C-bérlőben.

A B2C-bérlő az alábbiak szerint használhatja a szolgáltatást:

1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. Kattintson a **bejelentkezési házirendek**. *Megjegyzés: Használható ez a szolgáltatás bármely házirend típus nem csupán a **bejelentkezési házirendek***.
3. Egy házirend megnyitásához kattintson rá. Kattintson például a **B2C_1_SiIn**.
4. Kattintson a **szerkesztése** hello panel hello tetején.
5. Kattintson a **Token, a munkamenet és az egyszeri bejelentkezés config**.
6. A kívánt módosításokat. További információk a rendelkezésre álló tulajdonságok az ezt követő szakaszok.
7. Kattintson az **OK** gombra.
8. Kattintson a **mentése** hello felül hello panelről.

## <a name="token-lifetimes-configuration"></a>Token élettartama konfiguráció
Az Azure AD B2C támogatja hello [OAuth 2.0 protokoll](active-directory-b2c-reference-protocols.md) engedélyezésének biztonságos hozzáférés tooprotected erőforrásokat. tooimplement Ez a támogatás az Azure AD B2C bocsát ki, különböző [biztonsági jogkivonatokat](active-directory-b2c-reference-tokens.md). Használhatja az Azure AD B2C által kibocsátott biztonsági jogkivonatainak toomanage élettartama hello tulajdonságai a következők:

* **& Azonosító elérése token élettartama (perc)**: hello OAuth 2.0 tulajdonosi jogkivonat használt toogain hozzáférés tooa hello élettartama védett erőforrás. Az Azure AD B2C jelenleg csak az azonosító-jogkivonatokat állít. Ezt az értéket csak akkor vonatkozik tooaccess jogkivonatokat is, ha azt fel őket támogatása.
  * Alapértelmezett = 60 perc.
  * (A határokat is beleértve) minimális = 5 perc.
  * (A határokat is beleértve) legfeljebb 1440 perc =.
* **Frissítse a jogkivonatok élettartama (nap)**: hello maximális időszak, ameddig egy frissítési jogkivonat használt tooacquire lehet egy új hozzáférés vagy az azonosító token (és szükség esetén egy új frissítési jogkivonat, ha az alkalmazás megadták hello `offline_access` hatókör).
  * Alapértelmezett = 14 nap.
  * (A határokat is beleértve) minimális = 1 nap.
  * (A határokat is beleértve) maximális = 90 nap.
* **Frissítse mozgó ablakban az élettartam (nap)**: után ez alkalommal időszak eltelte hello felhasználói kényszerített toore-függetlenül hello érvényességi időszaka hello hello alkalmazás által igényelt legutóbbi frissítési jogkivonat hitelesítésére. Ez csak biztosítható, hogy ha hello kapcsoló értéke túl**Bounded**. Toobe nagyobb vagy egyenlő toohello igényel **frissítési jogkivonat élettartamát (nap)** érték. Ha hello kapcsoló értéke túl**Unbounded**, nem adhat meg egy adott értéket.
  * Alapértelmezett = 90 nap.
  * (A határokat is beleértve) minimális = 1 nap.
  * (A határokat is beleértve) legfeljebb 365 nap =.

Az alábbiak néhány engedélyezheti az ezekkel a tulajdonságokkal használati esethez:

* Engedélyezése egy felhasználó toostay mobilalkalmazás határozatlan ideig bejelentkeztetheti mindaddig, amíg nem folyamatosan aktív hello alkalmazástól. Ehhez hello beállítása által **frissítési jogkivonat mozgó ablak élettartamát (nap)** túl kapcsoló**Unbounded** a bejelentkezési házirend.
* Az iparági biztonsági és megfelelőségi követelményeknek megfelelő hello megfelelő hozzáférési jogkivonat élettartamát beállításával.

    > [!NOTE]
    > Ezek a beállítások nem érhetők el, a jelszó-átállítási házirendek.
    > 
    > 

## <a name="token-compatibility-settings"></a>Token kompatibilitási beállítások
A biztonsági jogkivonatokat az Azure AD B2C által kibocsátott azt végrehajtott formázási módosítások tooimportant jogcímeket. Ez volt kész tooimprove a szabványos protokoll támogatása és jobb együttműködés külső identity szalagtárak. Azonban tooavoid legfrissebb meglévő alkalmazások esetében létrehozott hello tulajdonságok tooallow ügyfelek tooopt-igény szerint a következő:

* **A jogcím kiállítójának (iss)**: hello Azure AD B2C bérlő hello jogkivonatot kibocsátó azonosítja.
  * `https://login.microsoftonline.com/{B2C tenant GUID}/v2.0/`: Ez az alapértelmezett érték hello.
  * `https://login.microsoftonline.com/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Ez az érték azonosítók hello B2C-bérlő és a használt hello jogkivonatkérelem hello házirend tartalmazza. Ha az alkalmazás vagy a könyvtárban van szüksége az Azure AD B2C toobe hello megfelelő [OpenID Connect felderítési 1.0 spec](http://openid.net/specs/openid-connect-discovery-1_0.html), használja ezt az értéket.
* **Tulajdonos (rész-) jogcím**: hello entitás, azaz egy hello felhasználói, mely hello token az használjon esetleg imperatív állításokat információkat azonosítja.
  * **ObjectID**: hello alapértelmezett érték. A hello hello Objektumazonosító hello directory hello felhasználó feltölt `sub` jogcím hello jogkivonatban.
  * **Nem támogatott**: Ez csak a visszamenőleges kompatibilitás érdekében biztosítja, és azt javasoljuk, hogy vált túl**ObjectID** , amint lehet.
* **Jogcím-házirend-azonosító képviselő**: Ez azonosítja hello jogcímtípus mely hello a házirend-azonosító, amelyet hello token kérés fel van töltve.
  * **tfp**: hello alapértelmezett érték.
  * **ACR**: Ez csak a visszamenőleges kompatibilitás érdekében biztosítja, és azt javasoljuk, hogy vált túl`tfp` , amint lehet.

## <a name="session-behavior"></a>Munkamenet viselkedése
Az Azure AD B2C támogatja hello [OpenID Connect hitelesítési protokoll](active-directory-b2c-reference-oidc.md) a biztonságos bejelentkezés tooweb alkalmazások engedélyezéséhez. Használható toomanage a webes alkalmazás munkamenetek hello tulajdonságok a következők:

* **A webalkalmazás munkamenet élettartama (perc)**: hello élettartama tárolt hello böngésző sikeres hitelesítés után az Azure AD B2C munkamenetcookie-t.
  * Alapértelmezett = 1440 perc.
  * (A határokat is beleértve) minimális = 15 perc.
  * (A határokat is beleértve) legfeljebb 1440 perc =.
* **Webes alkalmazás munkamenet időkorlátja**: Ha a kapcsoló értéke túl**abszolút**, hello felhasználó kényszerített toore-hello által megadott időszak után hitelesítéséhez **webalkalmazás munkamenet élettartama (perc)** eltelt. Ha a kapcsoló értéke túl**működés közbeni** (hello az alapértelmezett beállítás), hello felhasználói mindaddig bejelentkezett felhasználó hello a webalkalmazásban folyamatosan aktív.

Az alábbiak néhány engedélyezheti az ezekkel a tulajdonságokkal használati esethez:

* Az iparági biztonsági és megfelelőségi követelményeknek megfelelő hello megfelelő webes alkalmazás munkamenet beállításával élettartama.
* Egy felhasználó és a magas biztonsági részét a webes alkalmazás közötti interakció során beállított idő elteltével ismételt hitelesítés kényszerítése. 

    > [!NOTE]
    > Ezek a beállítások nem érhetők el, a jelszó-átállítási házirendek.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Egyszeri bejelentkezés (SSO) konfigurálása
Ha a B2C-bérlő több alkalmazásokat és házirendeket is van, felhasználói interakció mindegyik kezelheti hello segítségével **egyszeri bejelentkezés konfigurációs** tulajdonság. A beállítások a következő hello hello tulajdonság tooone állíthatja be:

* **Bérlői**: Ez az hello alapértelmezett beállítása. Ez a beállítás lehetővé teszi, hogy több alkalmazásokat és házirendeket a B2C-bérlő tooshare hello azonos felhasználói munkamenet. Például után a felhasználó bejelentkezik egy alkalmazásba, Contoso vásárlás, általa is is zökkenőmentesen jelentkezzen be egy másik egy, a Contoso gyógyszerészet, esetén használja.
* **Alkalmazás**: Ez egy felhasználói munkamenet kizárólag az alkalmazáshoz, más alkalmazások független toomaintain lehetővé teszi. Például, ha azt szeretné, hogy hello felhasználói toosign tooContoso Gyógyszertári a (a hello ugyanazokat a hitelesítő adatokat), akkor is, ha az őt van már be van jelentkezve Contoso vásárlás, egy másik alkalmazás hello azonos B2C-bérlő. 
* **Házirend**: Ez kizárólag a házirend, független hello alkalmazások használja azt a felhasználói munkamenet toomaintain lehetővé teszi. Például ha hello felhasználó már bejelentkezett és a többszörös többtényezős hitelesítés (MFA) lépés befejezése, ő adható meg több alkalmazás részei hozzáférés toohigher biztonsági mindaddig, amíg hello kötött munkamenet toohello házirend nem jár le.
* **Letiltott**: hello felhasználói toorun keresztül hello teljes felhasználói út ekkor elemezni a hello házirend minden végrehajtásakor. Például ez lehetővé teszi több felhasználók toosign mentése tooyour application (a megosztott asztali forgatókönyvek), még akkor is, míg egy-egy felhasználóhoz marad az aláírt hello teljes idő alatt.

    > [!NOTE]
    > Ezek a beállítások nem érhetők el, a jelszó-átállítási házirendek.
    > 
    > 

