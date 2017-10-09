---
title: "kétlépéses ellenőrzést, a munkahelyi vagy iskolai fiók mentése aaaSet |} Microsoft Docs"
description: "Amikor a vállalat konfigurálja az Azure multi-factor Authentication, fogja felszólító toosign feliratkozott a kétlépéses ellenőrzést. Megtudhatja, hogyan tooset azt be. "
services: multi-factor-authentication
keywords: "Hogyan toouse az azure directory, az active directory hello felhőben, az active directory-oktatóanyag"
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 69b04f74f6b28d0bcd94ca649b51092d9d139581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-my-account-for-two-step-verification"></a>A kétlépéses ellenőrzéshez a fiók beállítása
Kétlépéses ellenőrzés egy további biztonsági lépés, amely segít megvédeni a fiókját azáltal, hogy a más személyek toobreak nehezebben. Ha a cikk elolvasása valószínűleg kapott e-mailt a munkahelyi vagy iskolai rendszergazda többtényezős hitelesítéssel kapcsolatos. Vagy lehet, hogy a toosign próbált és további biztonsági ellenőrzés be tooset kérő üzenetet kapott. Hello esetben **nem tud bejelentkezni a hello automatikus igénylés folyamat befejezése után**.

Ez a cikk segít beállítani a **munkahelyi vagy iskolai fiók**. Ha a saját, személyes Microsoft-fiók tooenable kétlépéses ellenőrzést, lásd: [tudnivalók a kétlépéses ellenőrzést](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="set-up-your-account"></a>A fiók beállítása

Az IT-részleg segítségével kétlépéses hitelesítéssel toostart igényel, akkor megjelenik egy képernyő, amely szerint **a rendszergazda előírta, állítsa be ezt a fiókot, a további védelem ellenőrzési**:

![Beállítás](./media/multi-factor-authentication-end-user-first-time/first.png)

tooget lépései, válassza ki **, állítsa be most.**

Ha egy ehhez hasonló képernyőt bejelentkezéskor nem látja, hajtsa végre a hello utasításait [kezelheti a kétlépéses ellenőrzés beállításait](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) toofind hello beállítások lapon lehet kezelni a ellenőrzési beállítások. 

## <a name="decide-how-you-want-tooverify-your-sign-ins"></a>Eldöntheti, hogyan tooverify a bejelentkezések

hello hello regisztrációs folyamat első kérdést módjának velünk toocontact meg. Tekintsen meg hello beállítások hello táblában, és használja a hello hivatkozások toogo toohello beállítási lépéseket az egyes módszerek.

| Kapcsolatfelvétel módja | Leírás |
| --- | --- |
| [Mobilalkalmazás](#use-a-mobile-app-as-the-contact-method) |- **Az ellenőrzési értesítések.** Ezt a beállítást a okostelefonján vagy táblagépén lévő leküldéses értesítések a notification toohello hitelesítő alkalmazást. Hello értesítési és, ha az megbízható, választhatja ki, **hitelesítés** hello alkalmazásban. A munkahelyi vagy iskolai szükség lehet PIN-kód megadása előtt a hitelesítést.<br>- **Ellenőrzőkód használata.** Ebben a módban hello állítanak elő egy megerősítési kódot, amely 30 másodpercenként frissíti. Adja meg hello legfrissebb hello bejelentkezési felületen.<br>hello Microsoft Authenticator alkalmazás érhető el [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), és [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| [Mobiltelefon hívása vagy szöveg](#use-your-mobile-phone-as-the-contact-method) |- **Telefonhívás** helyezi el egy automatizált hang hívás toohello telefonszámot megadnia. Hello hívás választ, és nyomja meg a # hello telefon billentyűzetén tooauthenticate.<br>- **Szöveges üzenet** karakterlánccal végződik-e egy megerősítési kódot tartalmazó szöveges üzenetet. Következő hello a megjelenő szöveg hello vagy válasz toohello szöveges üzenetet, vagy hello bejelentkezési felületén megadott hello ellenőrzőkódot. |
| [Irodai telefon hívása](#use-your-office-phone-as-the-contact-method) |Az automatizált hang hívás toohello telefonszámot megadnia helyezi. Válasz hello hívja, és a hello telefon billentyűzetén tooauthenticate kell nyomnia a #. |

## <a name="use-a-mobile-app-as-hello-contact-method"></a>A mobilalkalmazások használata hello kapcsolattartási módszerként
Ezzel a módszerrel kell a telefonját vagy táblagépét egy hitelesítő alkalmazást telepíteni. hello a cikkben ismertetett alapuló hello Microsoft Authenticator alkalmazást, amely [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), és [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Válassza ki **mobilalkalmazás** hello legördülő listából.
2. Válassza **értesítéseket az ellenőrzéshez** vagy **Ellenőrzőkód használata**, majd jelölje be **beállítása**.

   ![További biztonsági ellenőrzési képernyő](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. A telefonját vagy táblagépét, nyissa meg hello alkalmazást, és válassza  **+**  tooadd fiókkal. (Az Android-eszközök válasszon hello három pontra, majd **fiók hozzáadása**.)
4. Adja meg, hogy tooadd munkahelyi vagy iskolai fiókkal. Megnyílik a hello QR-kód képolvasó a telefonján. Ha a kamera nem működik megfelelően, akkor választhat tooenter a vállalati adatok manuálisan. További információkért lásd: [fiók manuális hozzáadása](#add-an-account-manually).  
5. Hello QR kód kép történt az üdvözlő képernyőt hello mobilalkalmazás konfigurálásához beolvasásához.  Válassza ki **végzett** tooclose hello QR-kód képernyő.  

   ![A QR-kód képernyő](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. Hello telefonon aktiválási befejezésekor válassza **forduljon me**.  Ez a lépés vagy egy értesítés, vagy egy ellenőrző kód tooyour phone küld. Válassza ki **ellenőrizze**.  
7. Ha a vállalat a PIN-kód bejelentkezés-ellenőrző jóváhagyására van szüksége, adja meg.

   ![Be a PIN-kód megadása](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. PIN-kód bevitelének befejezése után válassza ki a **Bezárás**. Ezen a ponton az ellenőrzés sikeres legyen.
9. Azt javasoljuk, hogy a mobiltelefon számát adja meg, abban az esetben, ha hozzáférést tooyour mobilalkalmazás elvesznek. Válassza ki a hello legördülő listából válassza ki az országot, és írja be mobiltelefonszámát hello mezőben következő toohello ország neve. Válassza ki **következő**.
10. Ezen a ponton alkalmazásjelszókat a böngészőn kívüli alkalmazások, például az Outlook 2010 vagy korábbi vagy Apple-eszközök hello natív e-mail alkalmazás másolatot kért tooset áll. Ennek oka az, bizonyos alkalmazások nem támogatják a kétlépéses ellenőrzést. Ha nem használja ezeket az alkalmazásokat, kattintson a **végzett** és kihagyja a hello további hello lépéseit.
11. Ha használja ezeket az alkalmazásokat, a Másolás hello alkalmazásjelszót megadott, majd illessze be a rendszeres jelszó helyett az alkalmazásra. Hello használhatja ugyanazt a jelszót több alkalmazásokhoz. További információk [Súgó az alkalmazásjelszavak].
12. Kattintson a **Done** (Kész) gombra.

### <a name="add-an-account-manually"></a>Egy fiók manuális hozzáadása
Ha azt szeretné, egy fiók toohello mobilalkalmazás tooadd manuálisan, helyett hello QR-olvasó, kövesse az alábbi lépéseket.

1. Jelölje be hello **manuálisan adja meg a fiók** gombra.  
2. Adja meg, amelyek a megadott hello az ugyanazon az oldalon bemutatja, hogy hello vonalkód hello kód és hello URL-cím. Ezeket az adatokat a hello kerül **kód** és **URL-cím** hello mobilalkalmazás mezőket.

    ![Beállítás](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Hello aktiválási befejezését, válassza ki a **forduljon me**. Ez a lépés vagy egy értesítés, vagy egy ellenőrző kód tooyour phone küld. Válassza ki **ellenőrizze**.

## <a name="use-your-mobile-phone-as-hello-contact-method"></a>Hello kapcsolattartási módszerként a mobiltelefonjára használata
1. Válassza ki **hitelesítéshez megadott telefonját** hello legördülő listából.  

    ![Beállítás](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Válassza ki az országot hello legördülő listából, és adja meg a mobiltelefonszámát.
3. Válassza ki a mobiltelefonjával - szöveges vagy telefonhíváson inkább toouse hello módszert.
4. Válassza ki **forduljon me** tooverify a telefonszámát. A kiválasztott hello módtól függően Microsoft szöveget küld vagy hívja meg. Az üdvözlő képernyőt megadott hello utasításokat, majd válasszon **ellenőrizze**.
5. Ezen a ponton alkalmazásjelszókat a böngészőn kívüli alkalmazások, például az Outlook 2010 vagy korábbi vagy Apple-eszközök hello natív e-mail alkalmazás másolatot kért tooset áll. Ennek oka az, bizonyos alkalmazások nem támogatják a kétlépéses ellenőrzést. Ha nem használja ezeket az alkalmazásokat, kattintson a **végzett** és kihagyja a hello további hello lépéseit.
6. Ha használja ezeket az alkalmazásokat, a Másolás hello alkalmazásjelszót megadott, majd illessze be a rendszeres jelszó helyett az alkalmazásra. Hello használhatja ugyanazt a jelszót több alkalmazásokhoz. További információk [Súgó az alkalmazásjelszavak].
7. Kattintson a **Done** (Kész) gombra.

## <a name="use-your-office-phone-as-hello-contact-method"></a>Hello kapcsolattartási módszerként az irodai telefon használata
1. Válassza ki **irodai telefon** a hello legördülő  

    ![Beállítás](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. hello telefonszám mezőben automatikusan kitölti a vállalat kapcsolattartási adatait. Ha hello száma hibás vagy hiányzik, kérje meg a rendszergazdát toomake módosításokat.
3. Válassza ki **forduljon me** tooverify a telefonszámát, és azt fel fogja hívni a számot. Az üdvözlő képernyőt megadott hello utasításokat, majd válasszon **ellenőrizze**.
4. Ezen a ponton alkalmazásjelszókat a böngészőn kívüli alkalmazások, például az Outlook 2010 vagy korábbi vagy Apple-eszközök hello natív e-mail alkalmazás másolatot kért tooset áll. Ennek oka az, bizonyos alkalmazások nem támogatják a kétlépéses ellenőrzést. Ha nem használja ezeket az alkalmazásokat, kattintson a **végzett** és kihagyja a hello további hello lépéseit.
5. Ha használja ezeket az alkalmazásokat, a Másolás hello alkalmazásjelszót megadott, majd illessze be a rendszeres jelszó helyett az alkalmazásra. Hello használhatja ugyanazt a jelszót több alkalmazásokhoz. További információk: [Mik az Alkalmazásjelszók](multi-factor-authentication-end-user-app-passwords.md).
6. Kattintson a **Done** (Kész) gombra.

## <a name="next-steps"></a>Következő lépések
* Az előnyben részesített beállításainak módosítására és [a kétlépéses ellenőrzést beállításainak kezelése](multi-factor-authentication-end-user-manage-settings.md)
* Állítson be [alkalmazásjelszók](multi-factor-authentication-end-user-app-passwords.md) natív eszköz alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést.
* Tekintse meg a hello [Microsoft Authenticator alkalmazás](microsoft-authenticator-app-how-to.md) gyors, biztonságos hitelesítés akkor is, ha a cella szolgáltatás nem rendelkezik.
