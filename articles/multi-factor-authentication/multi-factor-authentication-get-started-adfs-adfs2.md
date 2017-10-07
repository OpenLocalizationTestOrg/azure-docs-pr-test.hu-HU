---
title: "Azure MFA kiszolgáló az AD FS 2.0 aaaUse |} Microsoft Docs"
description: "Ez a hello Azure többtényezős hitelesítés lap, amely leírja, hogyan tooget lépések az Azure MFA és az AD FS 2.0-s."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 96168849-241a-4499-a224-d829913caa7e
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 15011a94ca69dc6e51aa3edf74f5db6cd44d697a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-toowork-with-ad-fs-20"></a>Azure multi-factor Authentication kiszolgáló toowork konfigurálása az AD FS 2.0
Ez a cikk olyan szervezeteknek, amelyek az Azure Active Directoryval összevont vannak, és szeretné toosecure erőforrásokat a helyszínen vagy hello felhőben van. Az erőforrások védelméről hello Azure multi-factor Authentication kiszolgáló használatával, majd konfigurálni toowork AD FS-sel, hogy a kétlépéses ellenőrzést akkor váltódik ki, a nagy értékű végpontok.

Ez a dokumentáció tartalmazza hello Azure multi-factor Authentication kiszolgáló az AD FS 2.0 használatával. Az AD FS-sel kapcsolatos információkért lásd: [A felhő és a helyszíni erőforrások védelme az Azure Multi-Factor Authentication-kiszolgáló és az AD FS használatával a Windows Server 2012 R2 rendszeren](multi-factor-authentication-get-started-adfs-w2k12.md).

## <a name="secure-ad-fs-20-with-a-proxy"></a>Az AD FS 2.0 védelme proxyval
AD FS 2.0 azonosítónak egy proxyval toosecure hello Azure multi-factor Authentication kiszolgáló hello AD FS-proxykiszolgáló telepítse.

### <a name="configure-iis-authentication"></a>Az IIS-hitelesítés konfigurálása
1. A hello Azure multi-factor Authentication kiszolgáló, kattintson a hello **IIS-hitelesítés** ikonra a bal oldali menü hello.
2. Kattintson a hello **űrlapalapú** fülre.
3. Kattintson az **Add** (Hozzáadás) parancsra.

   <center>![Telepítés](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>

4. toodetect felhasználónév, jelszó és tartomány változók automatikusan, hello bejelentkezési URL-címe (például https://sso.contoso.com/adfs/ls) hello űrlapalapú webhely párbeszédpanelen adja meg, majd kattintson **OK**.
5. Ellenőrizze a hello **Azure multi-factor Authentication felhasználói egyeztetés megkövetelése** mezőben, ha az összes felhasználót törölték, vagy importálja a hello kiszolgáló és a tulajdonos tootwo lépés ellenőrzése. Ha a felhasználók jelentős számú nem még importálva lettek hello kiszolgálón és/vagy a kétlépéses ellenőrzés alól, hagyja bejelölve hello mezőt.
6. Ha hello oldalváltozók automatikusan nem észlelhető, kattintson a hello **adja meg manuálisan...** gombra a hello űrlapalapú webhely párbeszédpanelen.
7. A hello űrlapalapú webhely párbeszédpanelen adja meg a hello URL-cím toohello AD FS bejelentkezési oldalára hello Eseménybeküldési URL-cím mezőben (például https://sso.contoso.com/adfs/ls), és adja meg az alkalmazás nevét (nem kötelező). hello alkalmazásnév Azure multi-factor Authentication-jelentésekben jelenik meg, és előfordulhat, hogy SMS vagy mobilalkalmazás hitelesítési üzenet jelenjen meg.
8. Hello kérés formátuma túl beállítása**POST vagy GET**.
9. Adja meg a hello tartozó felhasználónév változójának (ctl00$ ContentPlaceHolder1$ UsernameTextBox) és a jelszó változót (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Ha az űrlapalapú hitelesítés bejelentkezési oldalát jeleníti meg a tartomány szövegmezőben, adja meg a hello tartomány-, valamint változót. toofind hello nevek hello bemeneti listamezők hello bejelentkezési lapján lépjen toohello bejelentkezési oldalt egy webböngészőben, kattintson a jobb gombbal a hello lapot, és válassza **forrás megtekintése**.
10. Ellenőrizze a hello **Azure multi-factor Authentication felhasználói egyeztetés megkövetelése** mezőben, ha az összes felhasználót törölték, vagy importálja a hello kiszolgáló és a tulajdonos tootwo lépés ellenőrzése. Ha a felhasználók jelentős számú nem még importálva lettek hello kiszolgálón és/vagy a kétlépéses ellenőrzés alól, hagyja bejelölve hello mezőt.
    <center>![Telepítés](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Kattintson a **Speciális...** elemre Speciális beállítások tooreview. A konfigurálható beállítások közé tartoznak az alábbiak:

    - Egyéni elutasítási oldalfájl kiválasztása
    - Gyorsítótár sikeres hitelesítések toohello webhely cookie-k használata
    - Válassza ki, hogyan tooauthenticate hello elsődleges hitelesítő adatok

12. Hello AD FS-proxykiszolgáló nem valószínű, mivel toobe toohello tartományhoz csatlakozott, LDAP tooconnect tooyour tartományvezérlő felhasználó importálása és előhitelesítési használható. A hello speciális, űrlapalapú webhely párbeszédpanelen kattintson a hello **elsődleges hitelesítési** lapot, és válasszon **LDAP-kötés** hello előhitelesítési hitelesítési típus esetében.
13. Amikor végzett, kattintson **OK** tooreturn toohello űrlapalapú webhely párbeszédpanelen.
14. Kattintson a **OK** tooclose hello párbeszédpanel megnyitásához.
15. Egyszer hello URL-cím és oldalváltozók észlelt vagy a megadott, hello űrlapalapú panel hello webhelyek adatainak megjelenítése.
16. Kattintson a hello **natív modul** lapra, és jelöljön ki hello, hello futtató webhelyen hello AD FS proxy alatt (például a "Default Web Site"), vagy az AD FS proxy (például "AD FS" a "ls") alkalmazás tooenable hello IIS beépülő modul következő hello hello kívánt szint.
17. Kattintson a hello **engedélyezése az IIS-hitelesítés** mezőt üdvözlő képernyőt hello tetején.

hello IIS-hitelesítés engedélyezve van.

### <a name="configure-directory-integration"></a>Címtárintegrálás konfigurálása
Az IIS-hitelesítés vannak engedélyezve, de tooperform hello előhitelesítési tooyour aktív a Directory (AD) a használatával konfigurálnia kell a LDAP hello LDAP kapcsolódási toohello tartományvezérlő.

1. Kattintson a hello **címtár-integráció** ikonra.
2. Hello beállítások lapon válassza ki a hello **adott LDAP-konfiguráció használata** választógombot.

   <center>![Telepítés](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>

3. Kattintson a **Szerkesztés** gombra.
4. A hello LDAP-konfiguráció szerkesztése párbeszédpanelen feltöltése hello mezőket a hello információ szükséges tooconnect toohello AD tartományvezérlőt. Hello mezők leírását hello Azure multi-factor Authentication kiszolgáló súgófájl szerepelnek.
5. Hello LDAP-kapcsolat tesztelése gombra kattintva hello **teszt** gombra.

   <center>![Telepítés](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>

6. Ha hello LDAP kapcsolat ellenőrzése sikeres volt, kattintson a **OK**.

### <a name="configure-company-settings"></a>Vállalati beállítások konfigurálása
1. Ezután kattintson a hello **vállalati beállítások** ikonra, és jelölje be hello **felhasználónév-feloldás** fülre.
2. Jelölje be hello **LDAP egyedi azonosító attribútum használata a felhasználónevek egyeztetéséhez** választógombot.
3. Ha a felhasználók "tartomány\felhasználónév" formátumban adja meg a felhasználónevét, hello Server kell toobe képes toostrip hello tartomány ki hello felhasználónév hello LDAP-lekérdezés létrehozásakor. Ez a beállításjegyzék beállításával végezhető el.
4. Nyissa meg a Beállításszerkesztőt hello, és nyissa meg tooHKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/pozitív hálózatok/PhoneFactor 64 bites kiszolgálóra. Ha egy 32-bites kiszolgálóról, végre hello "Wow6432Node" hello elérési kívül. Hozzon létre egy DWORD beállításkulcsot "UsernameCxz_stripPrefixDomain" nevezik, és állítsa be a hello érték too1. Az Azure multi-factor Authentication most adminisztrálásakor hello AD FS proxy.

Győződjön meg arról, hogy felhasználók importálva lettek az Active Directoryból hello kiszolgáló. Lásd: hello [megbízható IP-cím szakasz](#trusted-ips) belső IP-címek, hogy a kétlépéses ellenőrzés esetén nem szükséges jelentkezzen be azokon a helyeken toohello webhely toowhitelist szeretné.

<center>![Telepítés](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 Direct proxy nélkül
Az AD FS biztosíthassa hello AD FS proxy nem használata esetén. Hello Azure multi-factor Authentication kiszolgáló telepítése hello AD FS-kiszolgálón, és a következő hello kiszolgálónként lépések hello konfigurálása:

1. Belül hello Azure multi-factor Authentication kiszolgáló, kattintson a hello **IIS-hitelesítés** ikonra a bal oldali menü hello.
2. Kattintson a hello **HTTP** fülre.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. Hello alap URL-cím hozzáadása párbeszéd mezőben adja meg az AD FS webhely, ahol a HTTP-hitelesítést végeznek (például https://sso.domain.com/adfs/ls/auth/integrated) hello alap URL-cím mezőbe hello hello URL-cím. Ezután írja be az alkalmazás nevét (nem kötelező). hello alkalmazásnév Azure multi-factor Authentication-jelentésekben jelenik meg, és előfordulhat, hogy SMS vagy mobilalkalmazás hitelesítési üzenet jelenjen meg.
5. Szükség esetén módosítsa a hello időtúllépést és a maximális munkamenet időtúllépés.
6. Ellenőrizze a hello **Azure multi-factor Authentication felhasználói egyeztetés megkövetelése** mezőben, ha az összes felhasználót törölték, vagy importálja a hello kiszolgáló és a tulajdonos tootwo lépés ellenőrzése. Ha a felhasználók jelentős számú nem még importálva lettek hello kiszolgálón és/vagy a kétlépéses ellenőrzés alól, hagyja bejelölve hello mezőt.
7. Ha szükséges, jelölje be az hello cookie-k gyorsítótár jelölőnégyzetet.

   <center>![Telepítés](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>

8. Kattintson az **OK** gombra.
9. Kattintson a hello **natív modul** lapon és a select hello server, a hello webhelyet (például a "Default Web Site") vagy a hello Active Directory összevonási szolgáltatások (például "AD FS" a "ls") alkalmazás tooenable hello IIS beépülő modul következő hello szint szükséges.
10. Kattintson a hello **engedélyezése az IIS-hitelesítés** mezőt üdvözlő képernyőt hello tetején.

Az Azure Multi-Factor Authentication mostantól védi az AD FS-t.

Győződjön meg arról, hogy felhasználók importálva lettek az Active Directoryból hello kiszolgáló. Lásd: hello megbízható IP-címek szakaszban, ha azt szeretné, hogy toowhitelist belső IP-címek, hogy a kétlépéses ellenőrzés esetén nem szükséges jelentkezzen be azokon a helyeken toohello webhelyhez.

## <a name="trusted-ips"></a>Megbízható IP-címek
Megbízható IP-címek engedélyezése a felhasználók toobypass Azure multi-factor Authentication az adott IP-címek és alhálózatok címekről érkező webhelykérelmek esetén. Például érdemes lehet a kétlépéses ellenőrzést tooexempt felhasználók hello office való bejelentkezéskor. Ehhez a hello irodai alhálózatot megbízható IP-címek bejegyzésének kellene megadnia.

### <a name="tooconfigure-trusted-ips"></a>tooconfigure megbízható IP-címek
1. Az IIS-hitelesítés szakasz hello, kattintson a hello **megbízható IP-címek** fülre.
2. Kattintson a hello **hozzáadása...** gombra.
3. Hello megbízható IP hozzáadása párbeszédpanel megjelenésekor választanak hello **egyetlen IP-cím**, **IP-címtartomány**, vagy **alhálózati** választógombokkal.
4. Adja meg a hello IP-cím, az IP-címet vagy alhálózatot, amely szerepel az engedélyezési listán kell lennie. Ha megad egy alhálózatot, válassza ki hello megfelelő hálózati maszkot, majd kattintson hello **OK** gombra. hello megbízható IP most már hozzá lett adva.

<center>![Telepítés](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
