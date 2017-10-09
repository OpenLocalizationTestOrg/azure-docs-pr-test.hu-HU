---
title: "aaaMicrosoft hitelesítőalkalmazása Súgó és támogatás |} Microsoft Docs"
description: "Gyakori kérdések a listáját tartalmazza, és a kapcsolódó toohello Microsoft Authentication alkalmazást és az Azure multi-factor Authentication válaszok."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: afba9b59ccaac60d022e8516fcf573dcfbf68af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-authenticator-app-faq"></a>A Microsoft hitelesítő alkalmazást – gyakori kérdések

Ebben a cikkben megválaszolunk érkező hello Microsoft Authenticator alkalmazással kapcsolatos gyakori kérdésekre. Ha egy válasz tooyour kérdés nem látja, nyissa meg toohello [Microsoft Authenticator alkalmazás fórum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Azt is rendelkezik egy adott szolgáltatás egy másik gyakori kérdések hello app [jelentkezzen be a telefon – gyakori kérdések](microsoft-authenticator-app-phone-signin-faq.md).

hello Microsoft Authenticator alkalmazás helyett hello Azure Authenticator alkalmazást, és hello ajánlott alkalmazás Azure multi-factor Authentication hitelesítés használatakor. hello Microsoft Authenticator alkalmazás érhető el [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="frequently-asked-questions"></a>Gyakori kérdések

### <a name="what-are-hello-codes-in-hello-app-for-why-does-hello-number-keep-counting-down"></a>Mik azok a hello kódok hello alkalmazást? Miért nem hello számát hagyja számbavételi?

Hello Microsoft Authenticator alkalmazás megnyitásakor látni felvett hello fiókokat és egy hat vagy nyolc-jegyű szám szerint. Egy 30 másodperces számlálóval jelenhet meg.

Ezek a kódok használt tooyour fiókkal való bejelentkezést követően. Miután megadta a felhasználónevét és jelszavát, valószínűleg ismételt tooenter ellenőrző kódot. Nyissa meg a hello Microsoft Authenticator alkalmazást, és másolja hello kódot, amely jelenleg jelenik. Adja meg, hogy a kód hello bejelentkezési oldal toofinish.

hello oka, hogy módosítása 30 másodpercenként van, hogy nem használja a hello kódok kétszer hello-e ugyanazt a kódot. Nincs például a jelszót, hogy van-e kellene tooremember. hello lényege, hogy csak a személy hozzáférés tooyour phone tudja-e az ellenőrző kódot.

hello kódok nem feltétlenül szükséges, interneten vagy adatokat, így nem kell Telefon szolgáltatás toosign kapcsolatos tooworry. Hello app bezárásakor hello háttérben nem tovább futnak, és azt nem üríti ki a telep. Hello-alkalmazások bezárása, és csak a következő alkalommal, amikor bejelentkezik a hello figyelmen kívül hagyja.  

### <a name="i-only-get-notifications-when-i-have-hello-app-open-if-hello-app-isnt-open-i-dont-get-any-notifications"></a>Csak jelenik értesítések, ha van hello alkalmazás megnyitása. Ha hello alkalmazás nincs megnyitva, nem jelenik meg értesítéseket.

Ha értesítést kap, de nem valamilyen hangot vagy mozog, annak ellenére, hogy az a csengető, először ellenőrizze a hello alkalmazás beállításait. Hello app toouse hang engedélyezéséhez, vagy az értesítések mozog.

Ha egyáltalán nem kérek értesítést, ellenőrizze a következő esetekben hello:

- A telefon távvezérlési vagy csendes módban van? Módban alkalmazások megtartása értesítések küldése.
- Értesítést kaphat a többi alkalmazás? Ha nem, előfordulhat, hogy hello hálózati kapcsolatok telefonon, vagy az Android- vagy Apple hello értesítések csatorna kapcsolatos problémát. Meg lehet oldani hello első lehetőség a telefon beállításait a, de tootalk tooyour szolgáltató hello második lehetőség segítségre szükség lehet.
- Akkor kap értesítést, az egyes fiókok hello alkalmazásában, de nem mások? Ha igen, hello problematikus fiók eltávolítása az alkalmazásból, és adja hozzá újra tooenable leküldéses értesítéseket. 

Ha ezek a hibaelhárítási javaslataival megoldhatja próbált, de továbbra is problémát tapasztal, küldjön a diagnosztikai naplókat. Toohello beállításait, majd jelölje ki az **Súgó és visszajelzés** és **elküldeni a naplókat**. Folytassa a toohello [Microsoft Authenticator alkalmazás fórum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) és tudassa velünk, milyen probléma most azt láthatja, és hogyan próbált eddig. 

### <a name="im-already-using-hello-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-tooone-click-push-notifications"></a>Már használom hello Microsoft Authenticator alkalmazás az ellenőrző kódok kezelésére. Leküldéses értesítések tooone kattintással átváltása?
A bejelentkezés révén leküldéses értesítés jóváhagyása csak érhető el személyes Microsoft-fiókok vagy munkahelyi és iskolai fiókok Microsoft, nem a harmadik fél fiókok például a Google vagy Facebook. Ha a munkahelyi vagy iskolai Microsoft-fiókkal rendelkezik, a szervezet toodisable Ez a beállítás választható.

Ha Ön egy Microsoft-fiókot használni a személyes fiókot és tooswitch keresztül toopush értesítések, meg kell tooadd fiókja újra. Regisztrálja újra a hello eszköz-fiókjával, és leküldéses értesítések beállítása.  

Ha a Microsoft Authenticator a munkahelyi vagy iskolai fiók, akkor a szervezet úgy dönt, hogy tooallow egy kattintással értesítések.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Egy kattintással leküldéses értesítések a-Microsoft fiókok működnek?
Leküldéses értesítések nem, csak működik a Microsoft-fiókok és az Azure Active Directory-fiókokat. Ha a munkahelyi vagy iskolai használ az Azure AD-fiókok, akkor előfordulhat, hogy letiltani ezt a funkciót.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>Szeretnék saját eszközt biztonsági másolatból történt visszaállítása, és a fiók kódok hiányzik vagy nem működik. mi történt?
Biztonsági okokból azt nem fiókok helyreállítása alkalmazás biztonsági másolatból.  Hello alkalmazás visszaállítása után törli a fiókokat, és vegye fel újra.

### <a name="i-got-a-new-device-how-do-i-remove-hello-microsoft-authenticator-app-from-my-old-device-and-move-toohello-new-one"></a>Egy új eszközt jelent meg. Hogyan távolíthatja el az hello Microsoft Authenticator alkalmazása a régi és helyezze át a toohello újat?
Hozzáadását hello Microsoft Authenticator alkalmazás tooa új eszköz nem automatikusan eltávolítja azokat az eszközöket. toomanage, mely eszközök vannak konfigurálva a fiókjához, keresse fel a hello ugyanazt a webhelyet, hogy használjon toomanage kétlépéses ellenőrzést, és tooremove régi alkalmazások lehetőséget.

Személyes Microsoft-fiók, az ezen a webhelyen van a [fiókvédelme](https://account.microsoft.com/security) lap. Munkahelyi vagy iskolai Microsoft-fiók, az ezen a webhelyen lehet akár [MyApps](https://myapps.microsoft.com) vagy egy egyéni portál, amely a szervezet van beállítva.

### <a name="how-do-i-remove-an-account-from-hello-app"></a>Hogyan távolíthatom fiók hello alkalmazásból?
* iOS: hello fő képernyőjén lehúzása balra, az a fiók csempéjére. Válassza a **Törlés** elemet.
* Windows Phone: Hello fő képernyőn válassza ki hello menü gombjára kattintva, majd **fiókok szerkesztése**. Koppintson a hello **X** következő toohello fióknevet.
* Android: Hello fő képernyőn válassza ki hello menü gombjára kattintva, majd **fiókok szerkesztése**. Koppintson a hello **X** következő toohello fióknevet.

Ha a szervezet által regisztrált eszközzel rendelkezik, szükség lehet egy további lépést tooremove toocomplete fiókját. Ezeken az eszközökön a hello Microsoft Authenticator alkalmazást a rendszer automatikusan regisztrálja mint eszközrendszergazdát. Ha azt szeretné, hogy toocompletely eltávolítás hello app, meg kell-e toofirst hello Alkalmazásbeállítások hello alkalmazás regisztrációjának törlése.

### <a name="why-does-hello-app-request-so-many-permissions"></a>Miért hello app sok engedélyek kéréséhez?
Engedélyek a Microsoft kérheti, hogy a teljes listáját itt, és megismerkedhet használatukkal hello alkalmazásban. hello konkrét engedélyeket látja attól függ, hogy rendelkezik phone hello típusú.

* **Kamera**: a munkahelyi, iskolai vagy -Microsoft fiók hozzáadásakor a kamera tooscan QR-kódokat használjuk.
* **Ügyfelek és a telefon**: Jelentkezzen be személyes Microsoft-fiókjával, ha azt próbálja toosimplify hello folyamat található meglévő fiókokat, amelyekkel a telefonján.
* **SMS**: jelentkezik be személyes Microsoft-fiókjával hello az első alkalommal, amikor meg arról, hogy a telefon száma megegyezik egy a rekordot kell hello toomake van. A szöveges üzenet toohello telefon kapni, amelybe letöltötte a hello alkalmazást. üdvözlőüzenetére 6 – 8 számjegyű ellenőrző kódot tartalmaz. Helyett toofind kéri a kód, és írja be a hello alkalmazásban találtunk azt hello szöveges üzenetben meg.
* **Más alkalmazások keresztül megrajzolásához**: Amikor megjelenik egy értesítés tooverify személyazonosságát, azt, hogy az értesítési terület felett megjelenített bármely más potenciálisan futó alkalmazás.
* **Adatok fogadása hello internet**: Ez az engedély szükség, az értesítések küldésével.
* **Megakadályozza, hogy a telefon alvó**: Ha az eszköz regisztrálása a szervezet, módosíthatják ezt a házirendet a telefonján.
* **Vezérlő vibráció**: dönthet úgy, hogy milyen egy vibráció személyazonosságát egy értesítési tooverify érkezésekor.
* **Ujjlenyomat hardver**: néhány munkahelyi és iskolai fiókok egy további PIN-kód szükséges, amikor a személyazonosságát. toomake hello folyamat egyszerűbb, hogy lehetővé teszik toouse megadása helyett ujjlenyomatot hello PIN-kódot.
* **Hálózati kapcsolatok megtekintéséhez**: Microsoft-fiók hozzáadásakor a hello app hálózati/internetkapcsolatot igényel.
* **Olvassa el a tároló tartalmának hello**: Ezzel az engedéllyel csak akkor használható, ha hello alkalmazás beállításaiban műszaki probléma jelentése. A tárolási tárolt adatok gyűjtése toodiagnose hello probléma.
* **Teljes hálózati hozzáférés**: Ez az engedély szükség, az értesítések tooverify személyazonosságát.
* **Indítási**: Ha újraindítja a telefon, ezzel az engedéllyel biztosítja, hogy folytatja személyazonosságát tooverify értesítéseket kapni.

### <a name="why-does-hello-microsoft-authenticator-app-allow-you-tooapprove-a-request-without-unlocking-hello-device"></a>Miért hello Microsoft Hitelesítőalkalmazása lehetővé teszi a tooapprove feloldásának hello eszköz nélküli kérelmet?

Az eszköz tooapprove ellenőrzés kér, mert tooprove szüksége, hogy rendelkezik-e a telefon Önnel toounlock nincs. Kétlépéses ellenőrzést igényel, két dolgot – egy tudja dolog, és egy annyi teendője van igazolására. tudja hello dolog a jelszavát. hello annyi teendője van, akkor a telefon (állítsa be a hello Microsoft Authenticator alkalmazást, és egy igazoló MFA néven van regisztrálva.) Ezért hello phone rendelkezik, és megfelel-e hello hello második tényezős hitelesítés feltételeinek hello kérés jóváhagyása. 

### <a name="what-does-hello-lock-icon-in-hello-account-list-mean"></a>Mit jelent a hello fióklista hello zárolási ikonjával jelenik?

hello lakat ikon jelzi a hello eszköz regisztrálva van az Azure ad-ben, és regisztrálva toohello fiók. IOS-eszközök regisztrációja a Microsoft Intune-regisztráció során történik.

## <a name="next-steps"></a>Következő lépések

### <a name="contact-us"></a>Kapcsolat
A kérdését itt nem választ, ha az Ön toohear szeretnénk. Nyissa meg toohello [Microsoft Authenticator alkalmazás fórum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) toopost a kérdés és a get hello Közösség segítségével, vagy szóljon hozzá ezen a lapon.


### <a name="related-topics"></a>Kapcsolódó témakörök
* [Tudnivalók a kétlépéses ellenőrzést](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) Microsoft-fiók
* [Hiba történt a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md) munkahelyi vagy iskolai fiókja?
* [Hello Microsoft Authenticator toosign a Phone használata](microsoft-authenticator-app-phone-signin-faq.md)

