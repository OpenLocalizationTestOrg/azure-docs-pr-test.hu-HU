---
title: "aaaTroubleshoot kétlépéses ellenőrzés |} Microsoft Docs"
description: "Ez a dokumentum felhasználók információt nyújt a milyen toodo Ha az Azure multi-factor Authentication problémát futnak."
services: multi-factor-authentication
keywords: "többtényezős hitelesítés ügyfél, hitelesítési probléma korrelációs azonosító"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: end-user
ms.openlocfilehash: 65c25fb70bebeb5eff15ffe63ce11a65f5cdb1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-help-with-two-step-verification"></a>Segítség a kétlépéses ellenőrzés
Ebben a cikkben megválaszolunk hello leggyakoribb kérdések személyek kérdezze a kétlépéses ellenőrzést. 

## <a name="why-do-i-have-tooperform-two-step-verification-can-i-turn-it-off"></a>Miért kell tooperform kétlépéses ellenőrzést? Ki lehet kapcsolni azt?

Kétlépéses ellenőrzés egy biztonsági funkció, hogy a szervezet választott toouse tooprotect a fiókjait. Biztonságosabb, mint az egyszerű jelszavak, mert két űrlapos hitelesítés támaszkodnak: valami ismeri, és valami Önnel rendelkezik. hello valami tudja, hogy a jelszót. hello valamit úgy, hogy Ön egy telefonszám vagy egy eszközt, amelyek azt általában akkor. Fiókja biztonságát a kétlépéses ellenőrzést, ha a támadók nem tud bejelentkezni, ha Ön akkor is, ha azok a jelszó lekérése. Ezek nem tud bejelentkezni, mert nem rendelkeznek hozzáféréssel tooyour phone. 

A Microsoft biztosít a kétlépéses ellenőrzést, de a szervezet úgy dönt, toouse hello szolgáltatást. Nem választhat, ha az IT-részleg azt is, ahogy meg nem tilthatják le a jelszó tooprotect használatával a fiókot igényel. 

Ha személyes Microsoft-fiókja-e kapcsolva a kétlépéses ellenőrzést, és toochange szeretné a beállításokat, olvassa el [tudnivalók a kétlépéses ellenőrzést](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) helyette. 

## <a name="i-dont-have-my-phone-with-me-today"></a>Nincs telefonszám velem még ma

Néhány napja, otthoni, de továbbra is a telefon hagyja a munkahelyi toosign kell. t érdemes kipróbálni! hello elsőként van bejelentkezni egy másik ellenőrzési módszerrel. A kétlépéses ellenőrzéshez regisztrálásakor volt beállítása egynél több telefonszámot? Bejelentkezés más módszert, tootry kövesse az alábbi lépéseket:

1. Jelentkezzen be a szokásos módon.
2. Amikor megnyílik a hello kétlépéses ellenőrzés oldal, **használja egy másik ellenőrzési módszerrel**.

   ![Másik ellenőrzési](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Válassza ki a kívánt toouse hello ellenőrzési módszerrel. 
  - Ha nem rendelkezik hozzáférési tooyour alternatív módszerek, vagy forduljon az informatikai részleg tooget súgó tooyour fiókkal bejelentkezni.
  - Ha rendelkezik hozzáféréssel tooyour alternatív módszerek, folytassa a kétlépéses ellenőrzést.

Ha nem lát hello **használja egy másik ellenőrzési módszerrel** hivatkozásra, majd, amely azt jelenti, hogy alternatív módszereket nem adott meg, amikor először regisztrálja a kétlépéses ellenőrzéshez. Lépjen kapcsolatba az informatikai részleg tooget súgó tooyour fiókkal bejelentkezni. Ha be van jelentkezve, ellenőrizze, hogy túl[a beállítások kezelését az](multi-factor-authentication-end-user-manage-settings.md) tooadd további hitelesítési módszerek a következő alkalommal. 

## <a name="i-lost-my-phone-or-got-a-new-number"></a>I a telefon elvesztése vagy készült új szám
Nincsenek két módon tooget vissza tooyour fiók. hello először esetén toosign be a másik hitelesítési telefonszámát beállított egyet. hello második van az informatikai részleg tooclear az tooask a beállításokat.

Ha telefonja lett elveszett vagy ellopták, azt javasoljuk, hogy az informatikai részleg közli. Meg kell tooreset az alkalmazásjelszókat, és törölje a megjegyzett eszközökön. 

### <a name="use-an-alternate-phone-number"></a>Használjon másik telefonszámra
Ha több ellenőrzési beállítások, például egy másodlagos telefonszámot vagy egy másik eszközön hitelesítőalkalmazás beállítása valamelyikével azokat a toosign.

toosign használatával hello másik telefonszámra, kövesse az alábbi lépéseket:

1. Jelentkezzen be a szokásos módon.
2. Amikor felszólító toofurther ellenőrizzük a fiókját, **használja egy másik ellenőrzési módszerrel**.
   
   ![Másik ellenőrzési](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Válassza ki a hello telefonszám vagy eszköz, amely rendelkezik hozzáféréssel.
4. A fiókjához, miután [a beállítások kezelését az](multi-factor-authentication-end-user-manage-settings.md) toochange a hitelesítési telefonszám.

### <a name="clear-your-settings"></a>Törölje a beállításokat
Ha nem konfigurálta a másodlagos hitelesítés telefonszám, hogy toocontact az IT-részleg segítségét. Rendelkezik őket törölje a jelet a beállításokat úgy hello legközelebbi időpont jelentkezzen be, túl bekéri[regisztrálja a kétlépéses ellenőrzéshez](multi-factor-authentication-end-user-first-time.md) újra.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>I nem érkeznek meg a szöveg, vagy hívja a telefonomra
Több oka miért, előfordulhat, hogy a toosign próbálja, de nem kapott hello szöveg vagy a telefonhívás. Ha az elmúlt hello szövegek vagy telefonhívások tooyour phone sikeresen beérkezett, majd a probléma a valószínűleg probléma hello phone szolgáltatóhoz, a fiók nem. Győződjön meg arról, hogy rendelkezik-e a megfelelő cella jel. És tooreceive szöveges üzenetet próbált meg, ha győződjön meg arról, hogy-e képes tooreceive szöveges üzeneteket. Kérje meg a "Friend" toocall, vagy a szöveg, mert egy tesztelési. 

Ha Ön már várta a szöveges vagy telefonhíváson több percig, hello leggyorsabb módon tooget a fiókja visszaszerzéséhez tootry egy másik lehetőséget.

1. Válassza ki **használja egy másik ellenőrzési módszerrel** arra vár, hogy az ellenőrzés hello oldalon.
   
    ![Másik ellenőrzési](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Válassza ki a hello telefon száma vagy a kézbesítési módszert toouse.
   
    Ha kapott több ellenőrző kódok kezelésére, használja a legújabb egy hello.

Ha nem rendelkezik konfigurált egy másik módszert, forduljon az informatikai részleghez, és kérje meg tooclear a beállításokat. hello amikor legközelebb bejelentkezik, a rendszer kérni fogja a túl[többtényezős hitelesítés beállítása](multi-factor-authentication-end-user-first-time.md) újra.

Ha gyakran toobad cella jel miatt késések, ajánlott hello [Microsoft Authenticator alkalmazás](microsoft-authenticator-app-how-to.md) okostelefonos. hello alkalmazást hozhat létre, amelyekkel a toosign véletlenszerű biztonsági kódokat, és ezek nem feltétlenül szükséges egy cella jel vagy az interneten kapcsolathoz.

## <a name="app-passwords-are-not-working"></a>Alkalmazásjelszók nem működnek.
Először is győződjön meg arról, hogy megfelelően van megadva hello alkalmazásjelszót. hello generált jelszót a felváltja a normál jelszavát, de csak a régebbi asztali alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést. Ha az eszköz még nem működik, próbálja aláíró a és [hozzon létre egy új alkalmazásjelszót](multi-factor-authentication-end-user-app-passwords.md).  Ha még mindig nem működik, forduljon az IT-részleg, és azokat [törölje a meglévő alkalmazásjelszavak](../multi-factor-authentication-manage-users-and-devices.md) és majd létrehozhat egy újat.

## <a name="i-didnt-find-an-answer-toomy-problem"></a>Nem található egy válasz toomy probléma.
Ha lépések próbált, de a problémák továbbra is fut, akkor forduljon az informatikai részleghez. Képes tooassist kell azt.

## <a name="related-topics"></a>Kapcsolódó témakörök
* [A kétlépéses ellenőrzést beállításainak kezelése](multi-factor-authentication-end-user-manage-settings.md)  
* [A Microsoft Authenticator alkalmazás – gyakori kérdések](microsoft-authenticator-app-faq.md)

