---
title: "Az Azure Active Directory B2C: Beépített házirendek |} Microsoft Docs"
description: "A témakör az Azure Active Directory B2C hello bővíthető szabályzat-keretrendszert, és hogyan toocreate különféle házirend"
services: active-directory-b2c
documentationcenter: 
author: sama
manager: mbaldwin
editor: PatAltimore
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: sama
ms.openlocfilehash: 24bb85eba30f888c6ea7d0401e05235e8eb6ea79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Az Azure Active Directory B2C: Beépített házirendek


az Azure Active Directory (Azure AD) B2C hello bővíthető szabályzat-keretrendszernek hello core erőssége hello szolgáltatást. Például csak a teljes leírása fogyasztói identitással kapcsolatos műveletet házirendek regisztráció, bejelentkezés és profil szerkesztése. Például a regisztrációs szabályzatban lehetővé teszi a toocontrol viselkedés úgy konfigurálja a következő beállítások hello:

* Fiók típusa (közösségi, például a Facebookhoz) és helyi fiókok például az e-mail címeket, hogy a fogyasztók hello alkalmazás feliratkozott toosign használhatja
* Attribútumok (például utónév, irányítószámát és cipőméreten) toobe gyűjtött hello felhasználói regisztráció során
* Az Azure többtényezős hitelesítés
* hello megjelenését és működését minden bejelentkezési lap
* Információ (amely akkor jelentkezik, mint a jogcím egy jogkivonatba), amely az alkalmazás hello kap hello házirend futtatása után

Hozzon létre több, különböző típusú házirendet az Ön bérlőjében, és felhasználja az alkalmazásokban, igény szerint. Házirendek a különböző alkalmazások felhasználhatók. Erre a rugalmasságra lehetővé teszi a fejlesztők toodefine, és módosítsa a fogyasztói identitással kapcsolatos műveletet, a minimális vagy nincs módosítások tootheir kód.

Házirendek állnak rendelkezésre egy egyszerű fejlesztői felületen keresztül. Az alkalmazás elindítja egy házirendet a szabványos HTTP-hitelesítési kérelmek (házirend paraméter benyújtása hello kérelem) használatával, és megkapja a testreszabott jogkivonatot válaszként. Hello egyetlen különbség a között, amelyek aktiválják az előfizetési házirend és invoke egy bejelentkezési házirend kérelmeket például hello "p" lekérdezésparaméter karakterlánca használt hello házirend neve:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Hello szabályzat-keretrendszernek kapcsolatos további információkért lásd: [ebben a blogbejegyzésben kapcsolatban az Azure AD B2C a hello nagyvállalati mobilitással és biztonsággal foglalkozó blogban](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-or-sign-in-policy"></a>Regisztráció vagy bejelentkezés házirend létrehozása

Ez a házirend mindkét fogyasztói előfizetési & bejelentkezési élmény egyetlen konfigurációval kezeli. A fogyasztók hello megfelelő elérési utat (regisztráció vagy bejelentkezés) hello környezettől függően le vannak vezetett. Azt is bemutatja hello tartalmát jogkivonatokat, amelyek hello alkalmazás sikeres regisztrációra vagy bejelentkezések fog kapni.  A kód a minta hello regisztráció vagy bejelentkezés házirend [érhető el itt](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Ajánlott maszkolandó, hogy a regisztrációs szabályzatban, és a bejelentkezési házirend keresztül használja ezt a házirendet is.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Előfizetési házirend létrehozása

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Bejelentkezési házirend létrehozása

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>A Profilszerkesztési házirend létrehozása

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>A jelszó-visszaállítási házirend létrehozása

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a>Gyakori kérdések

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Hogyan hivatkozásra a jelszó-visszaállítási házirend regisztráció vagy bejelentkezés házirenddel?
A regisztráció vagy bejelentkezés házirendje (helyi fiókoknál) hoz létre, ha megjelenik egy **elfelejtette jelszó?** hello hello tapasztalat első oldalán lévő hivatkozásra. Ez a hivatkozás nem automatikusan eseményindító jelszó alaphelyzetbe állítása házirend. 

Ehelyett hello hibakód  **`AADB2C90118`**  tooyour app adja vissza. Az alkalmazás a hibakóddal kell toohandle figyelőn egy adott jelszó-visszaállítási házirend. További információkért lásd: a [minta azt mutatja be a hello módszert is, amely a linking házirendek](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>A regisztráció vagy bejelentkezés házirend vagy a regisztrációs szabályzatban, és a bejelentkezési házirend érdemes használni?
Azt javasoljuk, hogy a regisztrációs szabályzatban, és a bejelentkezési házirend keresztül használja a regisztráció vagy bejelentkezés házirend.  

hello regisztráció vagy bejelentkezés házirend hello bejelentkezési házirend-nál több képességekkel rendelkezik. Lehetővé teszi a toouse lap felhasználói felületének testreszabása és a honosításhoz jobb támogatást. 

hello bejelentkezési házirend ajánlott toolocalize a házirendek nem szükséges, ha csak kisebb testreszabási lehetőségek szükséges branding, és szeretné a jelszó alaphelyzetbe állítása azt beépítve.

## <a name="next-steps"></a>Következő lépések
* [Token, munkamenet és egyszeri bejelentkezés konfigurálása](active-directory-b2c-token-session-sso.md)
* [Tiltsa le a felhasználói regisztráció során e-mail ellenőrzése](active-directory-b2c-reference-disable-ev.md)

