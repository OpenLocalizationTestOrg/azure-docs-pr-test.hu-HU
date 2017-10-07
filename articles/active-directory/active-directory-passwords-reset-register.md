---
title: "Az Azure AD: SSPR regisztrációs |} Microsoft Docs"
description: "Regisztrálja az Azure AD az önkiszolgáló jelszó-hitelesítési adatok alaphelyzetbe állítása"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: joflore
ms.custom: end-user
ms.openlocfilehash: dfcd0106616218c84d23920b124bed5b202cdd6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="register-for-self-service-password-reset"></a>Regisztráció önkiszolgáló jelszó-visszaállításra

> [!IMPORTANT]
> **Azért van itt, mert problémák merültek fel a bejelentkezéssel kapcsolatban?** Ha igen, [így módosíthatja vagy állíthatja alaphelyzetbe a jelszavát](active-directory-passwords-update-your-own-password.md).

Végfelhasználói új jelszót, vagy a fiók zárolását kívánja feloldani anélkül, hogy az önkiszolgáló jelszó-változtatási (SSPR) használó toospeak tooa személy. Ez a funkció használata előtt tooregister hitelesítési módszerekkel van, vagy erősítse meg az előre megadott hello hitelesítési módszerek a rendszergazda fel van töltve.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>SSPR hitelesítés adatok regisztrálása vagy megerősítése

1. Az eszközt, majd a toohello nyitott hello webböngészőt [jelszó-változtatási regisztrációs lapjához](http://aka.ms/ssprsetup)
2. Adja meg a rendszergazdától kapott felhasználónevet és jelszót.
3. Attól függően, hogy az informatikai munkatársak konfigurált dolog, egy vagy több hello következő beállítások érhetők el, tooconfigure, és ellenőrizze. A rendszergazda előfordulhat, hogy feltöltése Ez néhány, ha rendelkeznek engedéllyel toouse hello adatait.
    * Irodai telefon csak a rendszergazda által beállított képes toobe
    * Hitelesítéshez megadott telefonját kellene lennie hozzáférés toolike fogadhassanak egy szöveges vagy telefonhíváson mobiltelefon tooanother telefonszámot kell megadni.
    * Hitelesítési E-mail kell beállítani, hogy van-e hozzáférési tooan másodlagos e-mail címére hello jelszó nélkül tooreset kell.
    * Biztonsági kérdések lehetővé teszi a rendszergazda jóváhagyta az Ön tooanswer kérdések listáját. Hello azonos kérdés vagy válaszolja meg egynél többször nem használhatók.
4. Adja meg, és ellenőrizze a rendszergazda által kötelezően előírt hello adatait. Ha egynél több lehetőség áll rendelkezésre, javasoljuk, hogy regisztrálja több módszerek tooprovide rugalmasságot biztosít, ha valami más módon nem érhető el (például: utazik, tooaccess az irodai telefonjára)

    ![Regisztrálja a hitelesítési módszereket, majd kattintson a Befejezés gombra][Register]

5. Ha befejezte a 4. lépés válasszon **Befejezés** , és most már tudja toouse önkiszolgáló jelszó alaphelyzetbe állítása esetén tooin hello jövőbeli van szüksége.

Ha hello hitelesítéshez megadott telefonját vagy e-mailek hitelesítési adatokat ad meg, nincs látható hello globális könyvtárban. hello csak személyek láthatják ezeket az adatokat, és a rendszergazdák. Csak akkor látható hello tooyour biztonsági kérdésekre.

A rendszergazdák szükség lehet, tooconfirm a hitelesítési módszerek idő toomake meg arról, hogy továbbra is regisztrálva megfelelő módszereket hello időszak után.

## <a name="common-problems-and-their-solutions"></a>Gyakori problémák és megoldásuk

 Az alábbiakban néhány gyakori hibák és a megoldások:

| Hiba eset| Milyen hiba látható?| Megoldás |
| --- | --- | --- |
| "Forduljon a rendszergazdához" oldal jelenik meg a felhasználói azonosító megadása után | Lépjen kapcsolatba a rendszergazdával <br> <br> A rendszer azt észlelte, hogy a felhasználói fiók jelszava nem Microsoft által felügyelt. Ennek eredményeképpen jelenleg nem tooautomatically ő alaphelyzetbe állítsa jelszavát. <br> <br> Meg kell toocontact az informatikai munkatársak további segítségért. | Azért jelent meg ezt az üzenetet, mert az informatikai munkatársak kezeli a jelszót a helyszíni környezetben, és nem teszi lehetővé a tooreset jelszót a nem tudja elérni a fiókot hivatkozásra. <br> <br> a jelszót, forduljon az informatikai személyzetet közvetlenül a segítségével, és hogy azok tooreset tudja azt szeretné, hogy tooreset a jelszót, ez a funkció az Ön is engedélyezhető.|
| A felhasználói azonosító megadása után jelenik meg a "a nem engedélyezett a jelszó alaphelyzetbe állítása" hiba | A fiókjára vonatkozóan nincs engedélyezve a jelszó alaphelyzetbe állítása <br> <br> Sajnáljuk, de az informatikai munkatársak nem állította be a fiók a szolgáltatással való használatra. <br> <br> Ha azt szeretné, is megkereshetjük a szervezet tooreset a rendszergazda a jelszót meg. | Ez az üzenet azért jelent, mert az informatikai munkatársak nincs engedélyezve a jelszó alaphelyzetbe állítása a szervezete számára a nem tudja elérni a fiókot hivatkozás, vagy nincs licence toouse hello szolgáltatást. <br> <br> tooreset a jelszavát, kattintson az ügyfél egy rendszergazda hivatkozás toosend egy e-mailek tooyour vállalati informatikai személyzetet tart fenn, és tájékoztassa, szeretné tooreset a jelszavát, ez a funkció az Ön is engedélyezhető. |
| A felhasználói azonosító megadása után jelenik meg a "nem tudtuk ellenőrizni fiókját" hiba | Nem tudtuk ellenőrizni a fiókot. <br> <br> Ha azt szeretné, is megkereshetjük a szervezet tooreset a rendszergazda a jelszót meg. | Ez az üzenet azért jelent meg, mert a jelszó-visszaállításhoz engedélyezve vannak, de nem regisztrált toouse hello szolgáltatást. tooregister a jelszó alaphelyzetbe állítása, után kell ismét megadott hozzáférési tooyour fiók, lépjen a toohttp://aka.ms/ssprsetup. <br> <br> tooreset a jelszavát, kattintson az ügyfél egy rendszergazda hivatkozás toosend egy e-mailek tooyour vállalathoz tartozó informatikai személyzetet tart fenn. |

## <a name="next-steps"></a>Következő lépések

* [Hogyan toochange önkiszolgáló jelszó használata a jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md)
* [Jelszó-visszaállítási regisztrációs oldal](http://aka.ms/ssprsetup)
* [Jelszó-visszaállítási portál](https://passwordreset.microsoftonline.com/)
* [Nem jelentkezhet be Microsoft-fiók tooyour](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Jelszó-visszaállítási regisztrációs oldal a regisztrált módszerekkel és a Befejezés gombbal"

