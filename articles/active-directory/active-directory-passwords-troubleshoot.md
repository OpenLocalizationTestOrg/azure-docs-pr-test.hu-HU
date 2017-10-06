---
title: "Hibáinak elhárítása: Az Azure AD SSPR |} Microsoft Docs"
description: "Hibaelhárítás az Azure AD az önkiszolgáló jelszóváltoztatás"
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
ms.date: 08/08/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 361ef5f37e4e9de83d3f9d5a8e5177d186fe86d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-self-service-password-reset"></a>Hogyan tootroubleshoot önkiszolgáló jelszóváltoztatás

Önkiszolgáló jelszóváltoztatás kezelése problémát tapasztal, ha az alábbi elemek hello segítenek tooget dolgot gyorsan működik-e.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-may-see"></a>Jelenhet meg a felhasználó önkiszolgáló jelszó-visszaállítási hibák elhárítása

| Hiba | Részletek | Technikai részletek |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Sajnos <br> Jelenleg nem lehet új jelszót, mert a rendszergazda letiltotta a jelszó alaphelyzetbe állítása a szervezet számára. Nincs semmilyen további műveletet elvégezhető tooresolve ebben a helyzetben. Forduljon a rendszergazdához, és kérje meg őket tooenable ezt a szolgáltatást. több, olvassa el az toolearn [segítségre van szüksége, az Azure AD-jelszó elfelejtettem](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: a rendszer azt észlelte, hogy jelszó-átállítási nincs engedélyezve a rendszergazda által. Forduljon a rendszergazdához, és kérje meg, jelszó-átállítási tooenable a szervezet számára. |
| WritebackNotEnabled = 10 |Sajnos <br> Jelenleg nem lehet új jelszót, mert a rendszergazda nem engedélyezte a szervezet számára szükséges szolgáltatás. Nincs semmilyen további műveletet elvégezhető tooresolve ebben a helyzetben. Forduljon a rendszergazdához, és kérje meg őket toocheck a szervezete beállításai. További információ a szükséges szolgáltatás toolearn olvasási [jelszóvisszaírás konfigurálása](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-writeback#configuring-password-writeback). | SSPR_0010: a rendszer azt észlelte, hogy a Jelszóvisszaírás nincs engedélyezve. Forduljon a rendszergazdához, és kérje meg a Jelszóvisszaírás tooenable. |
| SsprNotEnabledInUserPolicy = 11 | Sajnos  <br> Jelenleg nem lehet új jelszót, mert a rendszergazda nem állított be a szervezet új jelszó. Nincs semmilyen további műveletet elvégezhető tooresolve ebben a helyzetben. Forduljon a rendszergazdához, és kérje meg tooconfigure jelszó alaphelyzetbe állítása. toolearn bővebben a jelszó alaphelyzetbe állítása konfigurációs olvasási hello cikk [gyors üzembe helyezés: az Azure AD az önkiszolgáló jelszó-átállítási](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: A szervezet nincs definiálva a jelszó-visszaállítási házirend. Forduljon a rendszergazdához, és kérje meg a jelszó-visszaállítási házirend toodefine. |
| UserNotLicensed = 12 | Sajnos <br> Mert a szükséges licencek hiányoznak a szervezet jelenleg nem lehet új jelszót. Nincs semmilyen további műveletet elvégezhető tooresolve ebben a helyzetben. Forduljon a rendszergazdához, és kérje meg toocheck licenc-hozzárendelést. licenceléssel kapcsolatos további toolearn hello a cikk elolvasása [licencelés követelményei az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: A szervezet nem rendelkezik a szükséges hello licencek szükséges tooperform jelszó alaphelyzetbe állítása. Forduljon a rendszergazdához, és kérje meg tooreview licenc-hozzárendeléseket. |
| UserNotMemberOfScopedAccessGroup = 13 | Sajnos <br> Jelenleg nem lehet új jelszót, mert a rendszergazda nem állított be a fiók toouse jelszó alaphelyzetbe állítása. Nincs semmilyen további műveletet elvégezhető tooresolve ebben a helyzetben. Forduljon a rendszergazdához, és kérje meg őket tooconfigure a fiók, jelszó-visszaállításhoz. a jelszó alaphelyzetbe állítása hello a cikk elolvasása fiók konfigurálásával kapcsolatban további toolearn [jelszó alaphelyzetbe állítása a felhasználók fokozatosan](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012: Nincsenek engedélyezve a jelszó alaphelyzetbe állítása csoport tagja. Kérje a rendszergazda és a kérelem toobe hozzáadott toohello csoport. |
| UserNotProperlyConfigured = 14 | Sajnos <br> Jelenleg nem lehet új jelszót, mert hiányzik a szükséges információkat a fiókhoz tartozó. Nincs semmilyen további műveletet elvégezhető tooresolve ebben a helyzetben. Kérje meg segítségét, és kérje meg őket tooreset az Ön jelszavát. Után újra hozzáférési tooyour fiók, megismerheti, hogyan register hello szükséges adatait az alábbi hello hello cikkben szereplő lépések [regisztrálni az önkiszolgáló jelszóváltoztatás](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-reset-register). | SSPR_0014: További biztonsági adatokat pedig szükséges tooreset a jelszót. tooproceed, forduljon a rendszergazdához, és kérje meg tooreset a jelszavát. Miután hozzáférési tooyour fiók regisztrálhatja https://aka.ms/ssprsetup a további biztonsági adatokat. A rendszergazda hello utasításait követve adhat hozzá további biztonsági adatokat tooyour fiók [beállítása és a jelszó-változtatási olvasási hitelesítési adatok](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired = 29 | Sajnos <br> A szervezet jelszó alaphelyzetbe állítása konfigurációjával kapcsolatos probléma miatt jelenleg nem lehet új jelszavát. Nincs semmilyen további műveletet elvégezhető tooresolve ebben a helyzetben. Forduljon a rendszergazdához, és kérje meg tooinvestigate. További információ az hello lehetséges probléma toolearn hello a cikk elolvasása [jelszóvisszaírás hibaelhárítása](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: A Microsoft nem tooreset a jelszót a helyi konfigurációs tooan hiba miatt van. Forduljon a rendszergazdához, és kérje meg tooinvestigate. |
| OnPremisesConnectivityError = 30 | Sajnos <br> Kapcsolódási problémák tooyour szervezet miatt most nem lehet új jelszavát. Nincs művelet tootake most van, de hello probléma feloldása lehet, ha kísérli meg újra később. Ha hello a probléma továbbra is fennáll, forduljon a rendszergazdához, és kérje meg tooinvestigate. több, kapcsolatos problémák toolearn [Jelszóvisszaírás hibaelhárítása kapcsolat](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Nem lehet új jelszavát a helyszíni környezetben tooa gyenge kapcsolat miatt. Forduljon a rendszergazdához, és kérje meg tooinvestigate.|


## <a name="troubleshoot-password-reset-configuration-in-hello-azure-portal"></a>Jelszó alaphelyzetbe állítása konfigurálása az Azure-portálon hello hibaelhárítása

| **Hiba történt** | Megoldás |
| --- | --- |
| Nem látom hello **jelszó-átállítási** szakasz alatt az Azure AD-t hello Azure-portálon | Ez akkor fordulhat elő, ha nem az Azure AD prémium vagy alapszintű licenccel toohello rendszergazda teljesítő hello művelet. <br> A licenc toohello rendszergazdai fiók az adott cikk hello segítségével megoldhatók [rendelhet, győződjön meg arról, és a licencek kapcsolatos problémák megoldásához](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Nem szerepel az adott konfigurációs beállítás | Sok hello felhasználói felületi elemei megjelenítéséhez szükséges. Próbálja a kívánt toosee összes hello-beállítások engedélyezése. |
| Nem szerepel az hello **helyszíni integráció** lap | Ez a beállítás csak akkor van látható, ha már letöltötte az Azure AD Connect és konfigurálni a jelszóvisszaírást. A témakörrel kapcsolatos további információkért tekintse meg a hello cikket [Ismerkedés az Azure AD Connect a gyorsbeállításokkal](./connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Jelszó alaphelyzetbe állítása reporting hibaelhárítása

| **Hiba történt** | Megoldás |
| --- | --- |
| Nem szerepel az bármely jelszó felügyeleti tevékenység típusa jelennek meg hello **önkiszolgáló jelszókezelés** naplózási esemény kategóriája | Ez akkor fordulhat elő, ha nem az Azure AD prémium vagy alapszintű licenccel toohello rendszergazda teljesítő hello művelet. <br> A licenc toohello rendszergazdai fiók az adott cikk hello segítségével megoldhatók [rendelhet, ellenőrizze és licencek kapcsolatos problémák megoldásához] |
| Felhasználói regisztrációk megjelenítése többször | Jelenleg amikor a felhasználó regisztrál, a Microsoft jelenleg jelentkezzen minden egyes adat, amely egy különálló esemény néven van regisztrálva. <br> Ha tooaggregate ezeket az adatokat, letöltheti a hello jelentés és a kimutatás hello nyissa meg az adatok az excel toohave nagyobb rugalmasságot.

## <a name="troubleshoot-password-reset-registration-portal"></a>Jelszó-visszaállítási portál hibaelhárítása

| **Hiba történt** | Megoldás |
| --- | --- |
| Directory nincs engedélyezve a jelszó alaphelyzetbe állítása **a rendszergazda nem engedélyezte, toouse ezt a szolgáltatást** | Kapcsoló hello **önkiszolgáló jelszóváltoztatás szolgáltatás engedélyezve** túl jelzőt**csoport** vagy **mindenki** kattintson **mentése** |
| Felhasználó nem rendelkezik egy Azure AD prémium vagy alapszintű licenccel **a rendszergazda nem engedélyezte, toouse ezt a szolgáltatást** | Ez akkor fordulhat elő, ha nem az Azure AD prémium vagy alapszintű licenccel toohello rendszergazda teljesítő hello művelet. <br> A licenc toohello rendszergazdai fiók az adott cikk hello segítségével megoldhatók [rendelhet, győződjön meg arról, és a licencek kapcsolatos problémák megoldásához](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Hiba a kérelem feldolgozása | Ez sok problémákat okozhatja, de általában ez a hiba vagy egy szolgáltatás kimaradás vagy konfigurációs probléma okozza. Ha ezt a hibaüzenetet látja, és azt az üzleti van hatással, további segítségért forduljon a Microsoft támogatási szolgálatához. |

## <a name="troubleshoot-password-reset-portal"></a>Jelszó-változtatási portál hibaelhárítása

| **Hiba történt** | Megoldás |
| --- | --- |
| Címtárban nincs engedélyezve a jelszó alaphelyzetbe állítása. | Kapcsoló hello **önkiszolgáló jelszóváltoztatás szolgáltatás engedélyezve** túl jelzőt**csoport** vagy **mindenki** kattintson **mentése** |
| Felhasználó nem rendelkezik egy Azure AD prémium vagy alapszintű licenccel | Ez akkor fordulhat elő, ha nem az Azure AD prémium vagy alapszintű licenccel toohello rendszergazda teljesítő hello művelet. <br> A licenc toohello rendszergazdai fiók az adott cikk hello segítségével megoldhatók [rendelhet, győződjön meg arról, és a licencek kapcsolatos problémák megoldásához](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Directory engedélyezve van a jelszó alaphelyzetbe állítása, de a felhasználó rendelkezik-e hiányzó vagy helytelen formátumú hitelesítési adatokat | Győződjön meg arról, hogy a felhasználó rendelkezik megfelelően formázott hello fájl a folytatás előtt a kapcsolattartási adatok. A témakörrel kapcsolatos további információkért tekintse meg a hello cikket [az Azure AD önkiszolgáló jelszó-változtatási által használt adatok](active-directory-passwords-data.md). |
| Directory engedélyezve van a jelszó alaphelyzetbe állítása, de a felhasználó csak rendelkezzen kapcsolattartási adatok egy részét fájl, ha a házirend úgy van beállítva toorequire két ellenőrzési lépések | Győződjön meg arról, hogy a felhasználó rendelkezik legalább két, megfelelően konfigurált kapcsolattartási módszerek (Példa: mobiltelefon **és** irodai telefon) a folytatás előtt. |
| Directory engedélyezve van a jelszó alaphelyzetbe állítása, és felhasználói helyesen van konfigurálva, de felhasználói nem toobe érhető el | Ez lehet átmeneti hiba vagy helytelen kapcsolattartási adatokat, amelyek nem sikerült megállapítani megfelelően hello eredménye. <br> <br> Ha hello felhasználói Várjon 10 másodpercet, próbálja meg újra, és megjelenik a "forduljon a rendszergazdához" hivatkozásra. Hello hívás gombra kattintva próbálja meg újra újrapróbálkozik, mivel egy jelszó-létrehozási elvégzi, a felhasználói fiók toobe kérő űrlap e-mail tooadministrators kattintson a "forduljon a rendszergazdához" küld. |
| Felhasználó soha nem kap hello jelszó alaphelyzetbe állítása SMS vagy a telefonhívás | Ennek oka lehet egy helytelen formátumú telefonszámot hello könyvtárban hello eredményét. Ügyeljen arra, hogy hello telefonszám hello formátum "+ kap xxxyyyzzzzXeeee". <br> <br> Jelszó alaphelyzetbe állítása nem támogatja a kiterjesztések, még akkor is, ha megad egy hello könyvtárban (azok előtt hello hívás megtörténik vannak nélkül). Próbálja meg egy számot, kiterjesztés nélkül használatával vagy hello bővítmény integrálása a PBX hello telefonszámot. |
| Felhasználó nem kap jelszó alaphelyzetbe állítása e-mail | hello leggyakoribb a probléma oka, hogy üdvözlőüzenetére levélszemétszűrőt elutasította. Ellenőrizze a levélszemét, a levélszemét, vagy a törölt elemek mappa hello e-mailek. <br> Bizonyosodjon meg arról, hogy meg vannak mailjeik hello jobb hello üzenet. |
| Rendelkezik egy jelszó-visszaállítási házirend állítható be, de amikor egy rendszergazdai fiókot használ jelszó-visszaállítás, a rendszer nem alkalmazza az adott házirendnek | A Microsoft felügyeli, és vezérlők hello rendszergazdai jelszó alaphelyzetbe állítása házirend tooensure hello legmagasabb szintű biztonság. |
| Nem lehetséges a jelszó alaphelyzetbe állítása egy napon belül túl sokszor próbált felhasználó | Sávszélesség-szabályozás tooreset próbálnak mechanizmus tooblock felhasználók a jelszavuk egy rövid időn belül túl sokszor automatikus megvalósítása. Ez akkor fordul elő, ha: <br> 1. Felhasználó egy óra alatt 5 alkalommal kísérli meg a toovalidate egy telefonszámot. <br> 2. Felhasználó toouse hello biztonsági kérdések kapu kísérli meg az 5-ször egy óra. <br> 3. Felhasználó kísérli meg a jelszót az hello tooreset egy órával az 5-ször ugyanazt a felhasználói fiókot. <br> toofix, utasítsa hello felhasználói toowait 24 óra után utolsó kísérlet hello, és hello felhasználói lesz majd képes tooreset kell a jelszavát. |
| Felhasználónál hiba telefonszámukat érvényesítésekor | Ez akkor fordul elő, ha a megadott telefonszám hello nem felel meg a fájl hello telefonszám. Ellenőrizze, hogy hello felhasználói hello teljes telefonszám terület és ország kód toouse egy módszert jelszó-visszaállításhoz megkísérelte írja be. |
| Hiba a kérelem feldolgozása | Ez sok problémákat okozhatja, de általában ez a hiba vagy egy szolgáltatás kimaradás vagy konfigurációs probléma okozza. Ha ezt a hibaüzenetet látja, és azt az üzleti van hatással, további segítségért forduljon a Microsoft támogatási szolgálatához. |

## <a name="troubleshoot-password-writeback"></a>A jelszóvisszaírás hibaelhárítása

| **Hiba történt** | Megoldás |
| --- | --- |
| Jelszó alaphelyzetbe állítása nem indul el a helyszíni hibával 6800 hello Azure AD Connect gép alkalmazásnaplóban. <br> <br> Bevezetési összevont vagy a Jelszókivonat szinkronizálása után a felhasználók nem visszaállíthassák a jelszavukat. | Ha engedélyezve van a Jelszóvisszaírás, hello szinkronizálási motor hello visszaírási könyvtár tooperform hello-konfiguráció (Bevezetés) hívások toohello bevezetési felhőszolgáltatás van szó. Bármely bevezetése során, vagy az eseménynaplóban hello Azure AD Connect gépének eseménynaplóban hibákat hello WCF végpont a Jelszóvisszaírás eredmények indításakor észlelt hibák. <br> <br> Során ADSync szolgáltatás újraindítását Ha visszaírási be lett állítva, hello WCF végpont már elindult. Azonban ha hello végpont hello indítása sikertelen, rendszer eseménynaplóba 6800 és lehetővé teszik a hello szinkronizáló szolgáltatás indítása. Ez az esemény jelenlétét azt jelenti, hogy adott hello Jelszóvisszaírás végpontra nem elindult. Ez az esemény (6800) együtt eseménynapló-bejegyzések Eseménynapló részletei összetevő azt jelzi, hogy miért hello végpontja nem indítható el PasswordResetService elő. Ezek a hibák Eseménynapló át, és próbálja meg toorestart hello Azure AD Connect, ha a Jelszóvisszaírás még nem működik. Ha hello a probléma továbbra is fennáll, próbálja toodisable, és újra a Jelszóvisszaírás engedélyezése.
| Amikor a felhasználó megpróbál tooreset jelszót, vagy a jelszóvisszaírás engedélyezése az fiók zárolását kívánja feloldani, hello művelet sikertelen lesz. <br> <br> Ezenkívül megjelenik egy eseményre hello Azure AD Connect Eseménynapló tartalmazó: "szinkronizálási motor adott vissza egy hibakód: hr = 800700CE, üzenet = hello fájlnév vagy bővítmény túl hosszú" hello a feloldási műveletet után következik be. | Hello Active Directory-fiókot az Azure AD Connect található, és alaphelyzetbe állítja a hello jelszó toocontain legfeljebb 127 karakter hosszú lehet. Ezután nyissa meg a Synchronization Service hello Start menüből. Keresse meg a tooConnectors, és megkeresi az Active Directory-összekötő hello. Válassza ki azt, és kattintson a Tulajdonságok elemre. Keresse meg a toohello lapon hitelesítő adatokat, és írja be a hello új jelszót. Válassza az OK tooclose hello lapot. |
| A hello utolsó lépése annak az Azure AD Connect telepítési folyamat hello megjelenik egy hiba, amely jelzi, hogy a Jelszóvisszaírás nem konfigurálható. <br> <br> hello Azure AD Connect alkalmazások eseménynaplójában keresse meg a "Hiba történt hitelesítési token" hibát 32009 szövegre tartalmaz. | Ez a hiba akkor fordul elő, a következő két esetben hello:<br> <br> a. Hello globális rendszergazdai fiók hello hello Azure AD Connect telepítési folyamat elején meg helytelen jelszót adott meg.<br> b. Egy összevont felhasználó hello globális rendszergazdai fiók a megadott hello Azure AD Connect telepítési folyamat elején hello toouse próbált meg.<br> <br> toofix hibára, győződjön meg arról, hogy nem használ pedig összevont fiók hello hello elején hello Azure AD Connect telepítési folyamat a megadott globális rendszergazda, és, hogy a megadott hello jelszó helyességéről. |
| hello Azure AD Connect gép Eseménynapló hello PasswordResetService által okozott 32002 hibát tartalmaz. <br> <br> hello hiba olvassa be: "Hiba történt a kapcsolódás tooServiceBus, hello jogkivonat-szolgáltató volt nem tooprovide egy biztonsági jogkivonatot..." | A helyszíni környezet nem tudja tooconnect toohello service bus-végpont hello felhőben. Ez a hiba általában egy tűzfalszabály blokkolja egy kimenő kapcsolat tooa adott portot vagy webcímet okozza. Lásd: [hálózati vonatkozó követelmények](active-directory-passwords-how-it-works.md#network-requirements) vonatkozó információ. Ha ezek a szabályok frissítése befejeződött, újraindításra hello Azure AD Connect gép és a Jelszóvisszaíró kell ismét dolgozni. |
| Működő egyes idő összevont vagy a Jelszókivonat szinkronizálása után a felhasználók nem visszaállíthassák a jelszavukat. | Néhány esetben hello Jelszóvisszaírás szolgáltatás nem tud toorestart az Azure AD Connect újraindult. Ebben az esetben először, ellenőrizze, hogy Jelszóvisszaírás megjelenjen-e a toobe-memóriakapacitásánál engedélyezve Ezt megteheti hello Azure AD Connect varázsló vagy a PowerShell használatával (lásd: HowTos szakasz fenti). Ha hello szolgáltatás engedélyezve toobe jelenik meg, próbálja engedélyezését vagy letiltását hello szolgáltatást újra vagy hello felhasználói felületén vagy a Powershellen keresztül. Ha ez sem működik, próbálja meg teljesen eltávolítása és újratelepítése az Azure AD Connect. |
| Összevont vagy a Jelszókivonat szinkronizálása a felhasználók, akik tooreset jelszavukat hibaüzenet jelenik meg elküldése hello jelszót, amely jelzi, hogy a szolgáltatás hiba történt kísérlet. <br ><br> Ezenkívül toothis, során a jelszó alaphelyzetbe állítása műveletek, kezelőügynök kapcsolatos hiba rendszer megtagadta a hozzáférést a helyszíni on eseménynaplókban is megjelenik. | Ha ezeket a hibákat az eseménynaplóban jelenik meg, győződjön meg arról, hogy hello AD MA (megadott fiók hello varázslóban a konfigurációs hello időpontjában) hello szükséges engedélyekkel rendelkezik a jelszóvisszaírás. <br> <br> **Ha ezt az engedélyt megadják, is eltarthat, hello engedélyek tootrickle too1 órában sdprop háttérfeladat a hello DC keresztül.** <br> <br> A jelszó-átállítási toowork, hello engedélyt kell toobe hello biztonsági leíró hello felhasználói objektum, amelynek jelszó visszaállítása folyamatban van nyomtatva. Ez az engedély hello user objektum jeleníti meg, amíg jelszó alaphelyzetbe állítása toofail hozzáférés megtagadva továbbra is. |
| Összevont vagy a Jelszókivonat szinkronizálása a felhasználók, akik tooreset jelszavukat hibaüzenet jelenik meg elküldése hello jelszót, amely jelzi, hogy a szolgáltatás hiba történt kísérlet. <br> <br> Ezenkívül toothis, során a jelszó alaphelyzetbe állítása műveletek, az eseménynaplók hello Azure AD Connect szolgáltatás "Objektum nem található" hibát jelző hiba látható. | Ez a hiba általában azt jelzi, hogy hello szinkronizálási motor nem toofind van, vagy hello felhasználói hello AAD connector területen vagy hello csatolt MV vagy AD összekötő terület objektum. <br> <br> tootroubleshoot, győződjön meg arról, hogy a felhasználó hello valóban szinkronizálása a helyszíni tooAAD hello jelenlegi példány az Azure AD Connect használatával, majd ellenőrizze a hello objektumok hello összekötőterek és MV hello állapotát. Győződjön meg arról, hogy hello AD CS objektum összekötő toohello MV-objektum hello "Microsoft.InfromADUserAccountEnabled.xxx" szabály keresztül.|
| Összevont vagy a Jelszókivonat szinkronizálása a felhasználók, akik tooreset jelszavukat hibaüzenet jelenik meg elküldése hello jelszót, amely jelzi, hogy a szolgáltatás hiba történt kísérlet. <br> <br> Továbbá toothis, során a jelszó alaphelyzetbe állítása műveletek, az eseménynaplók hello Azure AD Connect szolgáltatást egy "Több találat" hibát jelző hiba látható. | Ez azt jelzi, hogy adott hello szinkronizálási motor észlelte, hogy hello MV-objektum csatlakoztatott toomore, mint egy Active Directory Tanúsítványszolgáltatások objektumok hello "Microsoft.InfromADUserAccountEnabled.xxx" keresztül. Ez azt jelenti, hogy hello felhasználó engedélyezett fióknak egynél több erdő. **Ebben a forgatókönyvben a jelszóvisszaírás nem támogatott.** |
| Jelszó-műveletek egy konfigurációs hiba miatt sikertelen. hello alkalmazásnaplót tartalmazza <br> <br> Az Azure AD Connect hiba 6329 szöveggel: 0x8023061f (a program hello művelet sikertelen volt, mert a jelszó-szinkronizálás nincs engedélyezve a kezelőügynök.) | Ez akkor fordul elő, ha hello Azure AD Connect konfiguráció megváltozott tooadd egy új AD erdőben (vagy tooremove és hozzáad egy meglévő erdőben) után hello Jelszóvisszaírás szolgáltatás már engedélyezve van. A felhasználók számára, például a jelszó műveletek az újonnan telepített erdők sikertelen lesz. toofix hello probléma, tiltsa le, majd engedélyezze újra hello Jelszóvisszaírás szolgáltatás hello erdő konfigurációs módosítások befejezése után. |

## <a name="password-writeback-event-log-error-codes"></a>Jelszó visszaírási eseménynapló hibakódok

Amikor Jelszóvisszaírás kapcsolatos hibák elhárításának ajánlott, hogy az alkalmazás eseménynaplóban az Azure AD Connect gép tooinspect. Ehhez az eseménynaplóhoz érdeklő két forrásból származó események Jelszóvisszaírás tartalmazza. hello PasswordResetService forrás a műveletek és a Jelszóvisszaíró problémák kapcsolódó toohello működését ismerteti. hello ADSync forrás írja le, műveletek és a problémák kapcsolódó toosetting jelszavak az AD-környezetben.

### <a name="source-of-event-is-adsync"></a>Esemény forrása ADSync

| Kód | Név vagy üzenet | Leírás |
| --- | --- | --- |
| 6329 | FEDEZETE: MMS(4924) 0x80230619 – "korlátozás megakadályozza, hogy a hello módosítani a jelszót nem toohello aktuális megadottal." | Ez az esemény hello Jelszóvisszaírás szolgáltatás megpróbál tooset a helyi címtárban hello jelszó élettartama, előzmények, összetettségi vagy hello tartomány szűrési követelményeinek nem megfelelő jelszót következik be. <br> <br> Ha a jelszó minimális élettartama, és nemrég módosította hello jelszó az idő ezt az ablakot, még nem tud toochange hello jelszó újra megadott hello eléréséig kora abban a tartományban. Tesztelési célokra minimális korú too0 kell állítani. <br> <br> Ha jelszó előzményekre vonatkozó követelményeknek engedélyezve van, akkor ki kell választania egy jelszót, amely nem használták hello utolsó N idő, ahol N az hello megjegyzése beállítás. Ha ebben az esetben tekintse meg a hibát, majd adja meg egy jelszót, amely használatban van a hello utolsó N idő. Tesztelési célokra előzmények too0 kell állítani. <br> <br> Ha összetettségi követelményeknek, az összes kerül minden alkalom, amikor hello felhasználó próbál toochange, vagy a jelszó alaphelyzetbe állítása. <br> <br> Ha a jelszószűrők engedélyezve van, és a felhasználó kiválaszt egy jelszót, amely nem felel meg a szűrési feltételeket hello, majd hello alaphelyzetbe állítása, vagy módosítsa a művelet sikertelen lesz. |
| HR 8023042 | Szinkronizálási motor adott vissza egy hibakód: hr = 80230402, üzenet egy objektum nem sikerült, mert ne legyenek ismétlődő bejegyzések a hello kísérlet tooget = horgonyhoz | Ezt az eseményt akkor fordul elő, amikor hello ugyanazt a felhasználói azonosítót engedélyezve van több tartományban. Ha például fiókot és az erőforrások erdők szinkronizálását végzi, és rendelkezik a hello ugyanazt a felhasználói azonosítót jelen, és engedélyezve van minden, a hiba akkor fordulhat elő. <br> <br> Ez a hiba akkor is előfordulhat, ha egy nem egyedi horgonyattribútum (például az alias vagy a UPN) használ, és két felhasználó osztozik egy adott azonos horgonyattribútum. <br> <br> tooresolve probléma, győződjön meg arról, hogy nincs duplikált felhasználókat a tartományban, és hogy minden felhasználóhoz egyedi horgonyattribútum használ. |

### <a name="source-of-event-is-passwordresetservice"></a>Esemény forrása PasswordResetService

| Kód | Név vagy üzenet | Leírás |
| --- | --- | --- |
| 31001 | PasswordResetStart | Az esemény azt jelzi, hogy a hello a helyszíni szolgáltatás észleli a jelszó-létrehozási kérelem hello felhőből származó összevont vagy a jelszó kivonatoló szinkronizált felhasználó számára. Ez az esemény akkor hello első esemény a minden jelszó alaphelyzetbe állítása a visszaírási művelet. |
| 31002 | PasswordResetSuccess | Az esemény azt jelzi, hogy egy felhasználó egy új jelszót kiválasztott a jelszó alaphelyzetbe állítása közben, a rendszer megállapította, hogy a jelszó megfelel-e vállalati jelszó követelményeinek, és ezt a jelszót sikeresen írt vissza toohello helyi AD-környezet. |
| 31003 | PasswordResetFail | Az esemény azt jelzi, hogy a felhasználó választott-e a jelszó és a jelszó érkező sikeresen toohello helyszíni környezetben, de során tooset hello jelszó hello helyi AD-környezetben, hiba történt. Ez akkor fordulhat elő, több okból: <br> <br> hello jelszó nem hello kora, előzmények, bonyolultsági megfeleljen vagy hello tartomány követelményei szűréséhez. Próbálja meg egy teljesen új jelszó tooresolve ez. <br> <br> hello MA-szolgáltatásfiókja hello megfelelő engedélyek tooset hello új jelszót a szóban forgó hello felhasználói fiók rendelkezik. <br> <br> hello felhasználói fiók pedig egy védett csoport, például a tartomány vagy a vállalati rendszergazdák, amely nem engedélyezi a jelszó set műveletek. |
| 31004 | OnboardingEventStart | Ez az esemény akkor következik be, ha engedélyezte a Jelszóvisszaírást az Azure AD Connect, és jelzi, hogy a Microsoft bevezetési a szervezet toohello Jelszóvisszaírás webszolgáltatás. |
| 31005 | OnboardingEventSuccess | Az esemény azt jelzi, hello bevezetési folyamat sikeres volt, és hogy a Jelszóvisszaírás funkció készen toouse. |
| 31006 | ChangePasswordStart | Az esemény azt jelzi, hogy hello a helyszíni szolgáltatás észlelt hello felhőből származó összevont vagy a jelszó szinkronizálva kivonatoló felhasználói jelszó változáskérést. Ez az esemény az hello első esemény minden jelszó módosítása visszaírási műveletben. |
| 31007 | ChangePasswordSuccess | Ez az esemény azt jelzi, hogy egy felhasználó egy új jelszót kiválasztott egy jelszó-változtatási művelet során kiderült, hogy a jelszó megfelel-e vállalati jelszó követelményeinek, és ezt a jelszót sikeresen írt vissza toohello helyi AD-környezet. |
| 31008 | ChangePasswordFail | Az esemény azt jelzi, hogy a felhasználó választott-e a jelszó és a jelszó érkező sikeresen toohello helyszíni környezetben, de során tooset hello jelszó hello helyi AD-környezetben, hiba történt. Ez akkor fordulhat elő, több okból: <br> <br> hello jelszó nem hello kora, előzmények, bonyolultsági megfeleljen vagy hello tartomány követelményei szűréséhez. Próbálja meg egy teljesen új jelszó tooresolve ez. <br> <br> hello MA-szolgáltatásfiókja hello megfelelő engedélyek tooset hello új jelszót a szóban forgó hello felhasználói fiók rendelkezik. <br> <br> hello felhasználói fiók pedig egy védett csoport, például a tartomány vagy a vállalati rendszergazdák, amely nem engedélyezi a jelszó set műveletek. |
| 31009 | ResetUserPasswordByAdminStart | hello a helyszíni szolgáltatás talált egy jelszó-átállítási kérelem egy felhasználó nevében hello rendszergazda származó összevont vagy a jelszó kivonatoló szinkronizált felhasználó számára. Ez az esemény az hello első esemény minden rendszergazda által kezdeményezett jelszó alaphelyzetbe állítása visszaírási műveletben. |
| 31010 | ResetUserPasswordByAdminSuccess | Üdvözöljük a rendszergazdákat egy rendszergazda által kezdeményezett jelszó-visszaállítási művelet során kiválasztották őket az új jelszót, a rendszer megállapította, hogy a jelszó megfelel-e vállalati jelszó követelményeinek, és ezt a jelszót sikeresen írt vissza toohello helyi AD-környezet. |
| 31011 | ResetUserPasswordByAdminFail | Üdvözöljük a rendszergazdákat kijelölt jelszó egy felhasználó nevében, és ezt a jelszót érkező sikeresen toohello helyszíni környezetben, de során tooset hello jelszó hello helyi AD-környezet, hiba történt. Ez akkor fordulhat elő, több okból: <br> <br> hello jelszó nem hello kora, előzmények, bonyolultsági megfeleljen vagy hello tartomány követelményei szűréséhez. Próbálja meg egy teljesen új jelszó tooresolve ez. <br> <br> hello MA-szolgáltatásfiókja hello megfelelő engedélyek tooset hello új jelszót a szóban forgó hello felhasználói fiók rendelkezik. <br> <br> hello felhasználói fiók pedig egy védett csoport, például a tartomány vagy a vállalati rendszergazdák, amely nem engedélyezi a jelszó set műveletek. |
| 31012 | OffboardingEventStart | Ez az esemény akkor fordul elő, ha letiltja a Jelszóvisszaírást az Azure AD Connect, és jelzi, hogy azt kivezetési a szervezet toohello Jelszóvisszaírás webszolgáltatáshoz. |
| 31013| OffboardingEventSuccess| Az esemény azt jelzi, hello kivezetési folyamat sikeres volt, és hogy a Jelszóvisszaírás funkció sikeresen letiltva. |
| 31014| OffboardingEventFail| Az esemény azt jelzi, hogy hello kivezetési folyamat sikertelen volt. Ennek oka lehet a hello felhő vagy a helyi rendszergazdai fiók konfigurálása során meghatározott tooa engedélyekkel kapcsolatos hibát, vagy mert toouse összevont felhő globális rendszergazda próbált Jelszóvisszaírás letiltásakor. toofix a, ellenőrizze a felügyeleti engedélyeket használ, és hogy nincsenek-e összevont fiók hello Jelszóvisszaírás funkció konfigurálása közben.|
| 31015| WriteBackServiceStarted| Az esemény azt jelzi, hogy hello Jelszóvisszaírás szolgáltatás sikeresen elindult és készen áll a tooaccept jelszó felügyeleti kérelmek hello felhőből.|
| 31016| WriteBackServiceStopped| Az esemény azt jelzi, hogy hello Jelszóvisszaírás szolgáltatás leállt, és bármely jelszó felügyeleti kérések hello felhőből nem lesz sikeres.|
| 31017| AuthTokenSuccess| Az esemény azt jelzi, hogy a Microsoft sikeresen beolvasta hello a globális rendszergazda Azure AD Connect telepítési toostart hello kivezetési vagy a bevezetési folyamat során megadott egy engedélyezési jogkivonatot.|
| 31018| KeyPairCreationSuccess| Az esemény azt jelzi, hogy sikeresen létrehozott hello jelszó-titkosítási kulcs, amely hello felhőalapú küldött toobe tooyour a helyszíni környezetben használt tooencrypt jelszavakat.|
| 32000| UnknownError| Ez az esemény azt jelzi, ismeretlen hiba a jelszó felügyeleti művelet során. Hello kivétel szövege hello eseményben további részletekért tekintse meg. Ha problémába ütközik, próbálja meg letiltásával és újbóli a Jelszóvisszaírás engedélyezése. Ha ez nem segít, tartalmazza a követési azonosító megadott belső tooyour támogatási szakértőhöz hello együtt Eseménynapló másolatát.|
| 32001| ServiceError| Ez az esemény azt jelzi hogy toohello felhő jelszó kapcsolódás során fellépő hiba szolgáltatás alaphelyzetbe állítása, és általában akkor fordul elő, amikor hello helyszíni szolgáltatást, ez azonban nem tooconnect toohello jelszó alaphelyzetbe állítása webszolgáltatáshoz.|
| 32002| ServiceBusError| Az esemény azt jelzi, hogy hiba történt a kapcsolódás tooyour bérlői service bus-példány. Ez akkor fordulhat elő, mert a kimenő kapcsolatokat blokkol a helyszíni környezetben. Ellenőrizze a tűzfal tooensure kapcsolatok engedélyezése a TCP 443-as és toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ keresztül, és próbálkozzon újra. Ha továbbra is problémába ütközik, próbálja meg letiltásával és újbóli a Jelszóvisszaírás engedélyezése.|
| 32003| InPutValidationError| Az esemény azt jelzi, hogy tooour webszolgáltatási API átadott hello bemenet érvénytelen. Próbálja megismételni a műveletet hello.|
| 32004| DecryptionError| Az esemény azt jelzi, hogy hiba történt a hello felhőből érkező hello jelszó visszafejtéséhez. Ezt okozhatják a visszafejtési kulcs eltérést hello felhőalapú szolgáltatás és a helyszíni környezetben. tooresolve, tiltsa le, majd engedélyezze újra a jelszavak visszaírását a helyszíni környezetben.|
| 32005| ConfigurationError| Bevezetési, során a bérlő-specifikus adatait. a konfigurációs fájlban, a helyszíni környezetben elmentjük. Ez az esemény azt jelzi hogy hiba történt a fájl, illetve hogy mentésekor, amikor hello szolgáltatás nem indult el hello fájl olvasásakor hiba történt. toofix probléma megoldásához próbálja meg letiltásával és újbóli engedélyezése a Jelszóvisszaírás tooforce egy módosítsa úgy a konfigurációs fájl.|
| 32007| OnBoardingConfigUpdateError| Bevezetési, során küldünk hello felhő toohello adatokat a helyszíni szolgáltatás a jelszó alaphelyzetbe állítása. Majd írása előtt tooan memórián belüli fájl küldött toohello szinkronizálási szolgáltatás toostore ezt az információt biztonságosan a lemezen. Ez az esemény azt jelzi, hogy írása vagy a memória az adatok frissítése. toofix probléma megoldásához próbálja letiltásával és újbóli engedélyezése a Jelszóvisszaírás tooforce egy módosítsa úgy a konfiguráció.|
| 32008| ValidationError| Az esemény azt jelzi, érvénytelen választ kapott a Microsoft hello jelszó alaphelyzetbe állítása webszolgáltatás. toofix probléma megoldásához próbálja meg letiltásával és újbóli a Jelszóvisszaírás engedélyezése.|
| 32009| AuthTokenError| Az esemény azt jelzi, hogy azt nem sikerült egy engedélyezési jogkivonat hello globális rendszergazdai fiók az Azure AD Connect telepítés során megadott. Ezt a hibát okozhatja egy hibás felhasználónév vagy jelszó hello globális rendszergazdai fiók megadva, vagy mert hello globális rendszergazdai fiókkal a megadott össze van vonva. a probléma, futtassa újra hello-konfiguráció javítsa a felhasználónevet és jelszót, valamint biztosítani hello rendszergazda felügyelt (csak felhőalapú vagy a jelszó szinkronizálva) fiók toofix.|
| 32010| CryptoError| Az esemény azt jelzi, hogy hiba történt, amikor a jelszó-titkosítási kulcs, vagy egy jelszót, amely hello felhőalapú szolgáltatás nem érkezik visszafejtése generálása hello. Ez a hiba valószínűleg a környezet hibát jelez. Tekintse meg az Eseménynapló további toolearn hello részleteit, és a probléma megoldásához. Is megpróbálhatja újra engedélyezése és letiltása hello Jelszóvisszaírás szolgáltatás tooresolve ez.|
| 32011| OnBoardingServiceError| Az esemény azt jelzi, hogy hello a helyszíni szolgáltatás nem tudott megfelelően kommunikálni hello jelszó alaphelyzetbe állítása web service tooinitiate hello bevezetési folyamat. Ezt okozhatja egy tűzfalszabály vagy probléma egy hitelesítési jogkivonatot a bérlő beolvasása. toofix, győződjön meg arról, hogy a TCP 443-as és 9350 – 9354 TCP vagy toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ nem blokkolják kimenő kapcsolatokat, és adott hello tooonboard használ az AAD-rendszergazdai fiók nem össze van vonva.|
| 32013| OffBoardingError| Az esemény azt jelzi, hogy hello a helyszíni szolgáltatás nem tudott megfelelően kommunikálni hello jelszó alaphelyzetbe állítása webes szolgáltatás tooinitiate hello kivezetési folyamat. Ezt okozhatja egy tűzfalszabály vagy probléma a bérlő egy engedélyezési jogkivonat beolvasása. toofix, győződjön meg arról, hogy a 443-as vagy toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ nem blokkolják kimenő kapcsolatokat, és adott hello toooffboard használ az AAD-rendszergazdai fiók nem össze van vonva.|
| 32014| ServiceBusWarning| Az esemény azt jelzi, hogy tooretry tooconnect tooyour bérlői service bus-példány volt. Normál körülmények között ez kell nem lehet probléma, de ha ez az esemény számos esetben ellenőrizze a hálózati kapcsolat tooservice bus, különösen, ha a nagy késleltetésű vagy alacsony sávszélességű kapcsolat.|
| 32015| ReportServiceHealthError| A sorrend toomonitor hello a Jelszóvisszaírás szolgáltatásának állapotát küldünk szívverés adatok tooour jelszó-átállítási webszolgáltatás 5 percenként. Az esemény azt jelzi, hogy hiba történt a egészségügyi adatokat hátsó toohello felhő webszolgáltatás küldésekor. Ez állapottal kapcsolatos adatok nem tartalmaz egy OII vagy személyazonosításra alkalmas adatok, és kizárólag a szívverés és alapszintű service statisztika, azt több szolgáltatás állapotinformáció hello felhőben.|
| 33001| ADUnKnownError| Ez az esemény azt jelzi, hogy ismeretlen hibát AD vissza, hello Azure AD Connect kiszolgáló eseménynaplója hibára vonatkozó további információ a hello ADSync forrásból származó események.|
| 33002| ADUserNotFoundError| Az esemény azt jelzi, hogy hello tooreset vagy a jelszó nem található a helyszíni címtár hello módosítás kívánó felhasználó. Ez akkor fordulhat elő, amikor hello felhasználói lett törölve a helyszínen, de nem felhőben hello, vagy ha a szinkronizálási problémát. Ellenőrizze a szinkronizálási naplókat, és hello utolsó néhány szinkronizálás futtatása részleteiben talál további információt.|
| 33003| ADMutliMatchError| A jelszó alaphelyzetbe állítása, vagy a változáskérés hello felhő származik, használjuk-e az Azure AD Connect toodetermine hello telepítés során megadott hogyan hello felhő horgonyzási toolink igénylő hátsó tooa felhasználói a helyszíni környezetben. Az esemény azt jelzi, hogy a helyszíni könyvtár hello észleltünk a két felhasználó azonos felhő horgonyattribútum. Ellenőrizze a szinkronizálási naplókat, és hello utolsó néhány szinkronizálás futtatása részleteiben talál további információt.|
| 33004| ADPermissionsError| Az esemény azt jelzi, hogy hello Management Agent szolgáltatásfiók használatával nem jogosult hello megfelelő hello a fiókot a szóban forgó tooset új jelszót. Gondoskodjon arról, hogy hello MA fiók hello felhasználói erdőben jogosult alaphelyzetbe állítása és módosítása jelszó hello erdő minden objektumához. Honnan toothis bővebben lásd a 4. lépés: hello megfelelő Active Directory-engedélyek beállítása.|
| 33005| ADUserAccountDisabled| Az esemény azt jelzi, hogy a Microsoft tooreset történt kísérlet, vagy módosítsa a jelszavát, amely a helyszíni le lett tiltva. Hello fiók engedélyezése, és próbálja megismételni a műveletet hello.|
| 33006| ADUserAccountLockedOut| Esemény azt jelzi, hogy a Microsoft tooreset történt kísérlet, vagy módosítsa a jelszavát, amely a helyszíni kizárt. Zárolásokat akkor fordulhat elő, amikor egy felhasználó próbálta meg a módosítása vagy alaphelyzetbe állítja a jelszót művelet rövid időn belül túl sokszor. Hello fiók zárolásának feloldása, és próbálja megismételni a műveletet hello.|
| 33007| ADUserIncorrectPassword| Az esemény azt jelzi, hogy hello a felhasználó aktuális érvénytelen jelszót adott meg, amikor művelet végrehajtása a jelszó módosítása. Adja meg a hello megfelelő jelenlegi jelszavát, és próbálkozzon újra.|
| 33008| ADPasswordPolicyError| Ez az esemény hello Jelszóvisszaírás szolgáltatás megpróbál tooset a helyi címtárban hello jelszó élettartama, előzmények, összetettségi vagy hello tartomány szűrési követelményeinek nem megfelelő jelszót következik be. <br> <br> Ha a jelszó minimális élettartama, és nemrég módosította hello jelszó az idő ezt az ablakot, még nem tud toochange hello jelszó újra megadott hello eléréséig kora abban a tartományban. Tesztelési célokra minimális korú too0 kell állítani. <br> <br> Ha jelszó előzményekre vonatkozó követelményeknek engedélyezve van, akkor ki kell választania egy jelszót, amely nem használták hello utolsó N idő, ahol N az hello megjegyzése beállítás. Ha ebben az esetben tekintse meg a hibát, majd adja meg egy jelszót, amely használatban van a hello utolsó N idő. Tesztelési célokra előzmények too0 kell állítani. <br> <br> Ha összetettségi követelményeknek, az összes kerül minden alkalom, amikor hello felhasználó próbál toochange, vagy a jelszó alaphelyzetbe állítása. <br> <br> Ha a jelszószűrők engedélyezve van, és a felhasználó kiválaszt egy jelszót, amely nem felel meg a szűrési feltételeket hello, majd hello alaphelyzetbe állítása, vagy módosítsa a művelet sikertelen lesz.|
| 33009| ADConfigurationError| Ez az esemény azt jelzi hogy írása jelszó hátsó tooyour a helyszíni címtár tooa konfigurációs hiba miatt az Active Directoryval kapcsolatos problémát. Eseménynaplóban hello Azure AD Connect gép alkalmazás milyen hiba történt a további tájékoztatást a hello ADSync szolgáltatás üzeneteit.|


## <a name="troubleshoot-password-writeback-connectivity"></a>A Jelszóvisszaírás kapcsolatok

Ha az Azure AD Connect hello Jelszóvisszaírás összetevő szolgáltatáskiesés tapasztalja, az alábbiakban néhány első lépéseket, amelyek tooresolve ez:

* [Indítsa újra az Azure AD Connect szinkronizálási szolgáltatás hello](#restart-the-azure-ad-connect-sync-service)
* [Letiltását és újraengedélyezését hello Jelszóvisszaírás szolgáltatás](#disable-and-re-enable-the-password-writeback-feature)
* [Hello legújabb az Azure AD Connect kiadást telepíteni](#install-the-latest-azure-ad-connect-release)
* [A jelszóvisszaíró hibaelhárítása](#troubleshoot-password-writeback)

Általában javasoljuk, hogy lefuttatja ezeket a lépéseket hello sorrendben toorecover fent a szolgáltatás hello leggyorsabb módon.

### <a name="restart-hello-azure-ad-connect-sync-service"></a>Indítsa újra az Azure AD Connect szinkronizálási szolgáltatás hello

Hello újraindítása az Azure AD Connect szinkronizálási szolgáltatás segítségével tooresolve kapcsolat hibái vagy más átmeneti probléma hello szolgáltatásban.

1. Ha Ön rendszergazda, kattintson a **Start** hello szolgáltatást futtató kiszolgálón **az Azure AD Connect**.
2. Típus **"Services.msc parancs"** hello keresési mezőbe, majd nyomja le az **Enter**.
3. Keresse meg hello **Microsoft Azure AD Sync** bejegyzés.
4. Kattintson a jobb gombbal a hello szolgáltatás bejegyzést, kattintson a **indítsa újra a**, és várja meg, hello művelet toocomplete.

   ![Hello Azure AD Sync szolgáltatás újraindítása][Service Restart]

Ezeket a lépéseket helyre a kapcsolatot hello felhőalapú szolgáltatással, és oldja meg a megszakításokat tapasztalhat. Hello szinkronizálási szolgáltatás újraindítása nem oldja meg a problémát, ha az azt javasoljuk, próbálja toodisable, és engedélyezze újra a hello Jelszóvisszaírás szolgáltatás a következő lépésben.

### <a name="disable-and-re-enable-hello-password-writeback-feature"></a>Letiltását és újraengedélyezését hello Jelszóvisszaírás szolgáltatás

Letiltásával és újbóli engedélyezése a Jelszóvisszaírás hello funkció segítségével tooresolve kapcsolódási problémák.

1. Ha Ön rendszergazda, nyissa meg a hello **az Azure AD Connect konfigurációs varázsló**.
2. A hello **tooAzure AD Connect** párbeszédpanelen adja meg a **Azure AD globális rendszergazda hitelesítő adatait**
3. A hello **tooAD DS csatlakozás** párbeszédpanelen adja meg a **AD tartományi szolgáltatások rendszergazdai hitelesítő adataival**.
4. A hello **felhasználók egyedi azonosítása** párbeszédpanel, kattintson a hello **következő** gombra.
5. A hello **választható szolgáltatások** párbeszédpanelen törölje hello **jelszóvisszaírás** jelölőnégyzetet.
6. Kattintson a **következő** keresztül nélkül semmit nem változtat amíg elér toohello párbeszédpaneljeivel fennmaradó hello **készen tooconfigure** lap.
7. Győződjön meg arról, hogy hello konfigurálása lapon látható hello **jelszó visszaírási beállítás le van tiltva** és kattintson a zöld hello **konfigurálása** toocommit gombra a módosítások.
8. A hello **befejezett** párbeszédpanelen kijelölésének megszüntetése hello **szinkronizálás most** lehetőséget, majd kattintson a **Befejezés** tooclose hello varázsló.
9. Nyissa meg újra a hello **az Azure AD Connect konfigurációs varázsló**.
10. **Ismételje meg a 2-8**, azzal a különbséggel győződjön meg arról, hogy **hello jelszó visszaírási beállítást** a hello **választható szolgáltatások** toore-enable hello szolgáltatás képernyőn.

Ezeket a lépéseket helyre a kapcsolatot a felhőalapú szolgáltatással, és oldja meg a megszakításokat tapasztalhat.

Ha letiltásával és újbóli engedélyezésével hello Jelszóvisszaírás nem oldja meg a problémát, azt javasoljuk, hogy a következő lépésben az Azure AD Connect tooreinstall meg.

### <a name="install-hello-latest-azure-ad-connect-release"></a>Hello legújabb az Azure AD Connect kiadást telepíteni

Az Azure AD Connect újratelepítése oldhatja konfigurációs és a problémák a felhőszolgáltatások és a helyi AD-környezet között.

Azt javasoljuk, akkor hajtsa végre ezt a lépést csak a fent leírt két lépést hello kísérlet után.

> [!WARNING]
> Ha testreszabott mezőben szinkronizálási szabályaiból, hello **mentésére ezeket, mielőtt folytatná a frissítést, és manuálisan telepítse újra őket befejezése után**.

1. Az Azure AD Connect legújabb verziójának hello letöltését hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
2. Mivel már telepítette az Azure AD Connect, kell egy helyi frissítési tooupdate tooperform az Azure AD Connect telepítési toohello legújabb verziója.
3. Hello letöltött csomagot, és a képernyőn megjelenő utasításokat tooupdate hello kövesse az Azure AD Connect-számítógépre.

Ezeket a lépéseket helyre a kapcsolatot a felhőalapú szolgáltatással, és oldja meg a megszakításokat tapasztalhat.

Ha telepítése hello legújabb verziójának hello Azure AD Connect-kiszolgáló nem oldja meg a problémát, azt javasoljuk, hogy meg letiltása és újbóli engedélyezése a Jelszóvisszaírás utolsó lépésként hello legújabb kiadásának telepítése után.

## <a name="verify-whether-azure-ad-connect-has-hello-required-permission-for-password-writeback"></a>Győződjön meg arról, hogy az Azure AD Connect jelszóvisszaírás hello szükséges engedéllyel rendelkezik-e 
Az Azure AD Connect szükséges AD **jelszó alaphelyzetbe állítása** engedély tooperform jelszóvisszaírás. toofind, hogy az Azure AD Connect hello engedéllyel rendelkezik a megadott helyszíni AD-felhasználó fiók számára, szolgáltatással hello Windows hatékony engedély:

1. Jelentkezzen be tooAzure AD Connect-kiszolgáló, majd indítsa el a hello **Synchronization Service Managert** (kezdő → szinkronizálási szolgáltatás).
2. A hello **összekötők** lapra, válassza ki a helyszíni hello **címtárösszekötőben** kattintson **tulajdonságok**.  
![Hatékony engedély - 2. lépés](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
3. Hello előugró párbeszédpanelen válassza ki a hello **tooActive Directory-erdő csatlakozás** lapra, és jegyezze fel a hello **felhasználónév** tulajdonság. Ez az Azure AD Connect címtár-szinkronizálás tooperform által használt hello AD DS-fiókjához. Az Azure AD Connect tooperform jelszóvisszaírás hello AD DS-fiókjához új jelszó engedéllyel kell rendelkeznie.  
![Hatékony engedély - 3. lépés](./media/active-directory-passwords-troubleshoot/checkpermission02.png)  
4. Bejelentkezés tooan a helyi tartomány, tartományvezérlő és a start hello **Active Directory – felhasználók és számítógépek** alkalmazás.
5. Kattintson a **nézet** , és győződjön meg arról, hogy **speciális funkciók** engedélyezve van.  
![Hatékony engedély - 5. lépés](./media/active-directory-passwords-troubleshoot/checkpermission03.png)  
6. AD-felhasználó fióknevet, amelyet az tooverify hello keresi. Kattintson a jobb gombbal a hello fiókot, és válassza ki **tulajdonságok**.  
![Hatékony engedély - 6. lépés](./media/active-directory-passwords-troubleshoot/checkpermission04.png)  
7. Hello előugró párbeszédpanelen nyissa meg toohello **biztonsági** fülre, és kattintson **speciális**.  
![Hatékony engedély - 7. lépés](./media/active-directory-passwords-troubleshoot/checkpermission05.png)  
8. Hello speciális biztonsági beállítások előugró párbeszédpanelen nyissa meg toohello **hatályos hozzáférés** fülre.
9. Kattintson a **válasszon ki egy felhasználót** , és válassza ki azt az Azure AD Connect által használt Active Directory tartományi szolgáltatások hello fiókot (lásd a 3. lépését). Kattintson a **hatályos hozzáférés megtekintése**.  
![Hatékony engedély - 9. lépés](./media/active-directory-passwords-troubleshoot/checkpermission06.png)  
10. Görgessen lefelé, és keresse meg **jelszó-átállítási**. Hello bejegyzés be van jelölve, ha azt jelenti, hogy hello AD DS-fiókjához engedélyt a kijelölt AD-felhasználó fiók hello tooreset hello jelszavát.  
![Hatékony engedély - 10. lépés](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Azure AD-fórumok

Ha az Azure AD általános kérdése van, és az önkiszolgáló jelszó-visszaállítás, megkérheti hello közösségi Ha segítségre van szüksége a hello [az Azure AD-fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Hello Közösség tagjai közé tartozik a mérnökök, termék kezelők, MVP és ösztöndíjas informatikai szakemberek számára.

## <a name="contact-microsoft-support"></a>Forduljon a Microsoft támogatási szolgálatához.

Ha nem találja a hello válasz tooan hibát a támogatási csoportokkal mindig elérhető tooassist további találhatók.

tooproperly segít, kérjük, hogy megadja mértékű részletes lehető eset beleértve megnyitásakor:

* **Általános leírása hello hiba** -hello hiba újdonságai? Mi volt volt megfigyelhető hello viselkedést? Hogyan Reprodukálja azt hello hiba? Adja meg a legnagyobb részletességgel lehető.
* **Lap** -milyen lap volt meg, amikor első fellépése hello hiba? Ha tudja tooand képernyőfelvételének tüntesse fel hello URL-CÍMÉT.
* **Támogatja a kód** -hello támogatási kód jön létre, ha a felhasználó hello látott hello hiba volt? 
    * toofind ez Reprodukálja hello hiba, majd kattintson az hello támogatja a kód hivatkozásra hello hello képernyő aljára, és a küldési hello támogatási szakértőhöz hello eredmények GUID.
    ![Hello támogatási kód üdvözlő képernyőt hello alján található][Support Code]
    * Ha hello alsó támogatás nélkül oldalon, nyomja le az F12, majd keresse meg a biztonsági AZONOSÍTÓT és a CID-et, és küldjön e két eredmények toohello támogatási szakértőhöz.
* **Dátum, idő és időzóna** -tüntesse fel hello pontos dátum és idő **a hello időzóna** , hogy hello hiba történt.
* **Felhasználói azonosító** -ki lett hello hiba látott hello felhasználó? (user@contoso.com)
    * Az egy összevont felhasználó?
    * A jelszó szinkronizálva kivonatoló felhasználó van szó?
    * A felhő csak felhasználó van szó?
* **Licencelési** -hello felhasználó nem rendelkezik az Azure AD prémium vagy alapszintű Azure AD-licenccel?
* **Alkalmazások eseménynaplójában** – Ha jelszóvisszaírás használ, és hello hibaüzenet, a helyszíni infrastruktúrára, az alkalmazások eseménynaplójában keresse meg a hello Azure AD Connect-kiszolgáló tömörített másolatát tartalmazza, amikor kapcsolatba lép a támogatási szolgálathoz.

    

[Service Restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Hello Azure AD Sync szolgáltatás újraindítása"
[Support Code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Támogatási kód itt található: hello alsó hello ablak jobb oldali"

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
* [**Jelszóvisszaíró**](active-directory-passwords-writeback.md) – Megtudhatja, hogyan használhatja a jelszóvisszaírót a helyszíni címtárával.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
