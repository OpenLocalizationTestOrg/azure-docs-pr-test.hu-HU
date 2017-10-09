---
title: "többtényezős hitelesítés aaaAzure bejelentkezhet a kétlépéses ellenőrzéshez használttal |} Microsoft Docs"
description: "Ezen az oldalon pedig akkor útmutatást nyújtanak ahol toogo toosee hello különböző bejelentkezési módszer áll rendelkezésre az Azure MFA Használatát."
keywords: "felhasználó hitelesítése, a bejelentkezés során tapasztal élmény, jelentkezzen be a mobiltelefon, bejelentkezés az irodai telefon"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: fcd5eb5e8426eda537db9e099bf247bde29c195b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-sign-in-experience-with-azure-multi-factor-authentication"></a>Azure multi-factor Authentication hello bejelentkezési élmény
> [!NOTE]
> hello Ez a cikk célja toowalk egy szokásos bejelentkezési élmény keresztül. Jelentkezzen be, vagy tootroubleshoot problémák kapcsolatban lásd: [problémák adódtak a Azure multi-factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Mi lesz a bejelentkezés során tapasztal élmény?
A bejelentkezés során tapasztal élmény eltér a kiválasztott beállításoktól függően toouse a második tényezőként: telefonhívást, hitelesítési alkalmazást vagy szövegét. Válassza ki, amely a leginkább illik az Ön tevékenységeit hello beállítást:

| Hogyan bejelentkezik az? | 
| --- |
| [Telefonhívás toomy mobileszköz vagy irodai telefon](#signing-in-with-a-phone-call) |
| [A szöveg toomy mobiltelefon](#signing-in-with-a-text-message)
| [Értesítések hello Microsoft Authenticator alkalmazásról](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [Az ellenőrző kódok kezelésére hello Microsoft Authenticator alkalmazásról](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [Az alternatív módszert mert jelenleg nem használható a kívánt módszer](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Telefonhívás bejelentkezni
a következő információk hello hello kétlépéses ellenőrzés kipróbálhatja egy hívás tooyour mobilalkalmazás vagy az irodai telefon ismerteti.

1. Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.  
2. Microsoft hívja meg.  
3. Hello phone választ, majd nyomja le az hello # gombját.  

## <a name="signing-in-with-a-text-message"></a>Bejelentkezés a szöveges üzenet
a következő információk hello a szöveges üzenet tooyour mobiltelefon hello kétlépéses ellenőrzés élményt ismerteti:

1. Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval. 
2. A Microsoft egy kódot tartalmazó szöveges üzenetet küld. 
3. Hello a Bejelentkezés lapon található hello mezőbe írja be a hello kódot. 

## <a name="signing-in-with-hello-microsoft-authenticator-app"></a>Bejelentkezés hello Microsoft Authenticator alkalmazásról 
hello következő ismerteti hello élménye hello Microsoft Authenticator alkalmazással a kétlépéses ellenőrzést. Nincsenek két különböző módon toouse hello alkalmazást. Leküldéses értesítéseket kaphat az eszközön, illetve megnyithat hello app tooget ellenőrző kódot.

### <a name="toosign-in-with-a-notification-from-hello-microsoft-authenticator-app"></a>hello Microsoft Authenticator alkalmazás értesítéseinek be toosign
1. Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.
2. Microsoft küld egy értesítési toohello Microsoft Authenticator alkalmazást az eszközön.

  ![Microsoft értesítést küld.](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Nyissa meg hello értesítést a telefon és select hello **ellenőrizze** kulcs. Ha a vállalat a PIN-kód szükséges, írja be ide.
4. Meg kell a bejelentkezés megtörtént.

### <a name="toosign-in-using-a-verification-code-with-hello-microsoft-authenticator-app"></a>az ellenőrző kód használata hello Microsoft Authenticator alkalmazás toosign

Hello Microsoft Authenticator alkalmazás tooget ellenőrző kódok kezelésére használatakor majd hello alkalmazás megnyitásakor megjelenik egy számot a fiók alatt. Ez a szám 30 másodpercenként úgy módosítja, nem adja meg kétszer ugyanazt a number hello. Ha az ellenőrző kódot megkérdezi, nyissa meg hello alkalmazást, és használja a szám szerint jelenleg jelenik meg. 

1. Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.
2. A Microsoft ellenőrző kódot kér.

  ![Adja meg](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Nyissa meg a hello Microsoft Authenticator alkalmazást a telefonra, és írja be a hello kódot bejelentkezik a hello mezőbe.

## <a name="signing-in-with-an-alternate-method"></a>Alternatív módszert bejelentkezni
Egyes esetekben nincs hello telefon vagy eszköz, amely akkor állíthatja be az előnyben részesített ellenőrzési módszert. Ebben az esetben is, ezért azt javasoljuk, hogy beállított biztonsági mentési módszerek a fiókjához. hello következő szakasz bemutatja, hogyan toosign be egy alternatív módszert, ha az elsődleges módszer nem érhető el.

1. Jelentkezzen be a tooan alkalmazás vagy szolgáltatás, például az Office 365 a felhasználónévvel és jelszóval.
2. Válassza ki **használja egy másik ellenőrzési módszerrel**. Megjelenik a telepítés hogy hány másik ellenőrzési lehetőséget.
3. Válasszon egy másik módszert, és jelentkezzen be.

  ![Alternatív módszert használja.](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Következő lépések

Ha bejelentkezik a kétlépéses ellenőrzést, akkor további információért [problémák adódtak a Azure multi-factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

Ismerje meg, hogyan túl[kezelése a kétlépéses ellenőrzés beállításait](multi-factor-authentication-end-user-manage-settings.md).

Megtudhatja, hogyan túl[Ismerkedés a Microsoft Authenticator alkalmazást hello](microsoft-authenticator-app-how-to.md) , hogy az értesítések toosign-szövegek és telefonhívásokat helyett használhat. 
