---
title: "Kétlépéses ellenőrzés hibaelhárítása |} Microsoft Docs"
description: "Ez a dokumentum felhasználók információt nyújt a Mi a teendő, ha az Azure multi-factor Authentication problémát futnak."
services: multi-factor-authentication
keywords: "többtényezős hitelesítés ügyfél, hitelesítési probléma korrelációs azonosító"
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: barlan
ms.reviewer: richagi
ms.custom: end-user
ms.openlocfilehash: 20a90aa36b727b18fb37aaf658da884b5997cd44
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="get-help-with-two-step-verification"></a>Segítség a kétlépéses ellenőrzés
Ebben a cikkben megválaszolunk személyek kérje meg a kétlépéses ellenőrzést kapcsolatos leggyakoribb kérdésekre. 

## <a name="why-do-i-have-to-perform-two-step-verification-can-i-turn-it-off"></a>Miért kell végrehajtani a kétlépéses ellenőrzést? Ki lehet kapcsolni azt?

Kétlépéses ellenőrzés egy biztonsági funkció, a szervezet döntött, hogy a fiók védelme érdekében. Biztonságosabb, mint az egyszerű jelszavak, mert két űrlapos hitelesítés támaszkodnak: valami ismeri, és valami Önnel rendelkezik. A hiba tudja található, a jelszót. A valami úgy, hogy meg, egy telefonszám vagy egy eszközt, amelyek azt általában akkor. Fiókja biztonságát a kétlépéses ellenőrzést, ha a támadók nem tud bejelentkezni, ha Ön akkor is, ha azok a jelszó lekérése. Ezek nem tud bejelentkezni, mert nem rendelkeznek hozzáféréssel a telefonjára. 

A Microsoft biztosít a kétlépéses ellenőrzést, de a szervezet úgy dönt, hogy a szolgáltatás használatához. Nem választhat, ha a vállalat támogatási igényel, azt is, ahogy meg nem tilthatják le a fiók védelme jelszó használatával. 

Ha személyes Microsoft-fiókja-e kapcsolva a kétlépéses ellenőrzést, és a beállítások módosításához olvassa [tudnivalók a kétlépéses ellenőrzést](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) helyette. 

## <a name="i-dont-have-my-phone-with-me-today"></a>Nincs telefonszám velem még ma

Néhány napja hagyja a telefon, otthoni, de továbbra is be kell jelentkeznie a munkahelyi hálózatban. Meg kell elsőként van bejelentkezni egy másik ellenőrzési módszerrel. A kétlépéses ellenőrzéshez regisztrálásakor volt beállítása egynél több telefonszámot? Próbálja más módszert bejelentkezni, kövesse az alábbi lépéseket:

1. Jelentkezzen be a szokásos módon.
2. Amikor megnyílik a kétlépéses ellenőrzés oldal, **használja egy másik ellenőrzési módszerrel**.

   ![Másik ellenőrzési](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. A használni kívánt hitelesítési lehetőségnek a választásához. 
  - Ha Ön nem rendelkezik hozzáféréssel az alternatív módszerek, vagy forduljon a vállalat fiókjába történő bejelentkezés segítség támogatja.
  - Ha hozzáfér a alternatív módszerek, folytassa a kétlépéses ellenőrzést.

Ha nem látja a **használja egy másik ellenőrzési módszerrel** hivatkozásra, majd, amely azt jelenti, hogy alternatív módszereket nem adott meg, amikor először regisztrálja a kétlépéses ellenőrzéshez. Segítség a fiókkal való bejelentkezéskor a vállalat támogatási szolgálattól. Ha be van jelentkezve, ügyeljen arra, hogy [a beállítások kezelését az](multi-factor-authentication-end-user-manage-settings.md) további hitelesítési módszerek hozzáadása a következő alkalommal. 

## <a name="i-lost-my-phone-or-got-a-new-number"></a>I a telefon elvesztése vagy készült új szám
Kétféleképpen kattintva visszatérhet fiókját. Az egyik jelentkezzen be másik hitelesítési telefonszámát, ha a beállított egyet. A második pedig kérje meg a vállalat támogatási a beállításokat.

Ha telefonja lett elveszett vagy ellopták, azt javasoljuk, hogy a vállalat támogatási biztosítják. Akkor vissza kell állítani az alkalmazásjelszókat, és törölje a megjegyzett eszközökön. 

### <a name="use-an-alternate-phone-number"></a>Használjon másik telefonszámra
Ha több ellenőrzési beállítások, például egy másodlagos telefonszámot vagy egy másik eszközön hitelesítőalkalmazás beállítása egyik használatával jelentkezzen be.

Jelentkezzen be a másik telefonszámra, kövesse az alábbi lépéseket:

1. Jelentkezzen be a szokásos módon.
2. Amikor a rendszer kéri, további ellenőrizzük a fiókját, válassza ki a **használja egy másik ellenőrzési módszerrel**.
   
   ![Másik ellenőrzési](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Válassza ki a telefonszám vagy az eszköz, amely rendelkezik hozzáféréssel.
4. A fiókjához, miután [a beállítások kezelését az](multi-factor-authentication-end-user-manage-settings.md) módosítása a hitelesítési telefonszámát.

### <a name="clear-your-settings"></a>Törölje a beállításokat
Ha nem konfigurálta a másodlagos hitelesítés telefonszám, hogy a vállalat támogatási kérjen segítséget. Egyértelműen kell azokat a beállításokat, amikor legközelebb bejelentkeznek, kérni fogja a [regisztrálja a kétlépéses ellenőrzéshez](multi-factor-authentication-end-user-first-time.md) újra.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>I nem érkeznek meg a szöveg, vagy hívja a telefonomra
Több oka miért megpróbálhatja jelentkezzen be, de nem kapják meg a szöveg vagy egy telefonhívás. Ha már sikeresen kapott szövegek vagy telefonhívások a telefonjára a múltban, majd a probléma valószínűleg probléma a telefon szolgáltató, a fiókja nem. Győződjön meg arról, hogy rendelkezik-e a megfelelő cella jel. És, ha szöveges üzenetet küld, győződjön meg arról, hogy a szöveges üzeneteket fogadhat kívánt. Kérje meg a "Friend", vagy szöveges hívása, mert egy tesztelési. 

A szöveges vagy telefonhíváson több percig is várta, ha a leggyorsabb feltörni fiókját módja próbálkozzon egy másik lehetőséget.

1. Válassza ki **használja egy másik ellenőrzési módszerrel** a arra vár, hogy az ellenőrzés lapon.
   
    ![Másik ellenőrzési](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Válassza ki a használni kívánt telefon száma vagy a kézbesítési módszert.
   
    Ha kapott több ellenőrző kódok kezelésére, használja a legújabb.

Ha nem rendelkezik konfigurált egy másik módszert, a vállalat támogatási szolgálattól, és kérje meg a beállításokat. A következő bejelentkezéskor, a rendszer kérni fogja [többtényezős hitelesítés beállítása](multi-factor-authentication-end-user-first-time.md) újra.

Ha gyakran rossz cella jel miatt késések, azt javasoljuk, használja a [Microsoft Authenticator alkalmazás](microsoft-authenticator-app-how-to.md) okostelefonos. Az alkalmazás hozhat létre, amelyekkel bejelentkezhet véletlenszerű biztonsági kódokat, és ezek nem feltétlenül szükséges egy cella jel vagy az interneten kapcsolathoz.

## <a name="app-passwords-are-not-working"></a>Alkalmazásjelszók nem működnek.
Először is győződjön meg arról, hogy megfelelően van megadva az alkalmazásjelszót. A generált jelszót a felváltja a normál jelszavát, de csak a régebbi asztali alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést. Ha az eszköz még nem működik, próbálja aláíró a és [hozzon létre egy új alkalmazásjelszót](multi-factor-authentication-end-user-app-passwords.md).  Még mindig nem működik, ha a vállalat támogatási szolgálattól, és azok [törölje a meglévő alkalmazásjelszavak](../multi-factor-authentication-manage-users-and-devices.md) és majd létrehozhat egy újat.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Nem található a problémámat választ.
Hibaelhárítási lépések próbált, de a problémák továbbra is fut, ha a vállalat támogatási szolgálattól. Segíthet kell lennie.

## <a name="related-topics"></a>Kapcsolódó témakörök
* [A kétlépéses ellenőrzést beállításainak kezelése](multi-factor-authentication-end-user-manage-settings.md)  
* [A Microsoft Authenticator alkalmazás – gyakori kérdések](microsoft-authenticator-app-faq.md)

