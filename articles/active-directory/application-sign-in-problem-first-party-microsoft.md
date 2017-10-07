---
title: "Bejelentkezés Microsoft-alkalmazáshoz tooa aaaProblems |} Microsoft Docs"
description: "Bejelentkezés toofirst féltől Microsoft Applications (például Office 365) az Azure AD használatával tapasztalt kapcsolatos gyakori problémák elhárításában"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 35849ca8dbaa909d17b6d0da572f5c11041a8559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="problems-signing-in-tooa-microsoft-application"></a>Bejelentkezés Microsoft-alkalmazáshoz tooa problémák

Microsoft Applications (például Office 365 Exchange, SharePoint, Yammer, stb.) rendelve, és egy kicsit másképp, mint 3. fél SaaS-alkalmazások vagy más alkalmazásokat, az egyszeri bejelentkezéshez az Azure AD integrálása felügyelni.

Háromféleképpen fő, amely a felhasználó kérheti le a hozzáférési tooa Microsoft közzétett alkalmazást.

-   Office 365 hello vagy más fizetős csomagok-alkalmazások, a felhasználók keresztül hozzáférést kapnak **licenc hozzárendelése** vagy közvetlenül tootheir felhasználói fiókot, vagy egy csoport, a licenc csoport-alapú hozzárendelés funkcióval keresztül.

-   Az, hogy a Microsoft vagy valamely harmadik fél közzéteszi szabadon bárki toouse alkalmazások, felhasználók adható hozzáférés a **felhasználói hozzájárulás**. This0 azt jelenti, hogy toohello alkalmazásokban az Azure AD munkahelyi vagy iskolai fiókkal jelentkeznek, és engedélyezze a toohave hozzáférés korlátozott toosome adatkészletet a fiókjuk.

-   Az, hogy a Microsoft vagy a 3. fél közzéteszi szabadon bárki toouse alkalmazások, felhasználók is adható hozzáférés a **rendszergazda jóváhagyását**. Ez azt jelenti, hogy a rendszergazda azt észlelte, hello alkalmazás felhasználhatja hello szervezet minden tagja számára, hogy jelentkezzen be egy globális rendszergazdai fiókkal toohello alkalmazás, és adjon hozzáférést tooeveryone hello szervezetében.

tootroubleshoot a problémát, hello kezdődik [általános probléma területet, és alkalmazás-hozzáférés tooconsider](#general-problem-areas-with-application-access-to-consider) majd elolvashatják a hello [forgatókönyv: lépéseket tootroubleshoot Microsoft Application hozzáférés](#walkthrough-steps-to-troubleshoot-microsoft-application-access) hello részleteinek tooget.

## <a name="general-problem-areas-with-application-access-tooconsider"></a>Az alkalmazás-hozzáférés tooconsider általános problémás területek

Az alábbiakban felsoroljuk a hello részletezhető Ha felmérheti, ahol toostart, de javasoljuk, olvassa el a hello forgatókönyv tooget is gyorsan általános problématerületeket: [forgatókönyv: lépéseket tootroubleshoot Microsoft Application hozzáférés](#walkthrough-steps-to-troubleshoot-microsoft-application-access).

-   [Hello felhasználói fiókkal kapcsolatos problémák](#problems-with-the-users-account)

-   [A csoportokkal kapcsolatos problémák](#problems-with-groups)

-   [Feltételes hozzáférési szabályzatokkal kapcsolatos problémák](#problems-with-conditional-access-policies)

-   [Az alkalmazás hozzájárulásával problémák](#problems-with-application-consent)

## <a name="steps-tootroubleshoot-microsoft-application-access"></a>Microsoft Application hozzáférés lépéseket tootroubleshoot

Alább néhány gyakori problémák segítsen futnak be amikor a felhasználók nem jelentkezhet be Microsoft-alkalmazáshoz tooa.

-   Általános problémák első toocheck

  * Ellenőrizze, hogy hello felhasználói bejelentkezéskor van toohello **javítsa ki az URL-cím** és nem egy helyi alkalmazás URL-címet.

  * Ellenőrizze, hogy a hello felhasználói fiók **nincs zárolva.**

  * Győződjön meg arról, hogy hello **felhasználói fiók létezik** az Azure Active Directoryban. [Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban](#problems-with-the-users-account)

  * Ellenőrizze, hogy a hello felhasználói fiók **engedélyezett** a indított bejelentkezések. [A felhasználói fiók állapotának ellenőrzése](#problems-with-the-users-account)

  * Győződjön meg arról, hogy hello felhasználói **nem lejárt vagy elfelejtett jelszó.** [A jelszó alaphelyzetbe állítása](#reset-a-users-password) vagy [önkiszolgáló jelszó-visszaállítás engedélyezése](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

   * Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést. [A felhasználó a multi-factor authentication állapotának](#check-a-users-multi-factor-authentication-status) vagy [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info)

   * Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést. [Ellenőrizze a megadott feltételes hozzáférési házirend](#problems-with-conditional-access-policies) vagy [egy adott alkalmazás feltételes hozzáférési házirendben](#check-a-specific-applications-conditional-access-policy) vagy [letiltása adott feltételes hozzáférési házirend](#disable-a-specific-conditional-access-policy)

   * Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** működik-e toodate tooallow többtényezős hitelesítést vagy feltételes hozzáférési házirendek toobe lépnek érvénybe. [A felhasználó a multi-factor authentication állapotának](#check-a-users-multi-factor-authentication-status) vagy [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info)

-   A **Microsoft** **licenc igénylő alkalmazások** (például az Office365), az alábbiakban néhány konkrét problémák toocheck után kiderül, hogy a fenti általános problémák hello:

   * Győződjön meg arról hello felhasználó vagy van egy **licenccel.** [Ellenőrizze a felhasználó licenc-hozzárendeléseket](#check-a-users-assigned-licenses) vagy [egy csoporthoz hozzárendelt licencekkel ellenőrzése](#check-a-groups-assigned-licenses)

   * Ha hello licenc **tooa hozzárendelt** **statikus csoport**, győződjön meg arról, hogy hello **felhasználó tagja** csoport. [A felhasználói csoporttagság ellenőrzése](#check-a-users-group-memberships)

   * Ha hello licenc **tooa hozzárendelt** **dinamikus csoport**, győződjön meg arról, hogy hello **dinamikus csoport szabály megfelelően van-e beállítva**. [A dinamikus csoport tagsági feltételek ellenőrzése](#check-a-dynamic-groups-membership-criteria)

   * Ha hello licenc **tooa hozzárendelt** **dinamikus csoport**, győződjön meg arról, hogy hello dinamikus csoport rendelkezik **fejezett** tagságát és, hogy hello **felhasználó egy tag** (Ez eltarthat egy ideig). [A felhasználói csoporttagság ellenőrzése](#check-a-users-group-memberships)

   *  Ha mindenképpen rendel hozzá licencet hello, ellenőrizze, hogy hello licenc **nem járt le**.

   *  Ellenőrizze, hogy hello licenc **hello alkalmazás** érik el.

-   A **Microsoft** **alkalmazások, amelyek nem igényelnek licencet**, az alábbiakban néhány más dolgokat toocheck:

   * Ha hello alkalmazás **felhasználói szintű engedélyek** (például "érhetik el a felhasználó postaládájához"), győződjön meg arról, hogy hello a felhasználó bejelentkezett-e toohello alkalmazás, és hajtott végre egy **felhasználói szintű hozzájárulási művelet**  toolet hello alkalmazás fér hozzá az adatokhoz.

   * Ha hello alkalmazás **rendszergazdai engedélyek** (például "érhetik el az összes felhasználó postaládák"), győződjön meg arról, hogy elvégezte-e globális rendszergazda egy **rendszergazdai hozzájárulási művelet az összes olyan felhasználó nevében** hello szervezetében.

## <a name="problems-with-hello-users-account"></a>Hello felhasználói fiókkal kapcsolatos problémák

Alkalmazás-hozzáférés blokkolható toohello alkalmazáshoz rendelt felhasználó tooa kapcsolatos probléma miatt. Az alábbiakban néhány módszert hibákat, és a felhasználó és a fiók beállításainak problémáinak megoldásával:

-   [Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban](#check-if-a-user-account-exists-in-azure-active-directory)

-   [A felhasználói fiók állapotának ellenőrzése](#check-a-users-account-status)

-   [A jelszó alaphelyzetbe állítása](#reset-a-users-password)

-   [Önkiszolgáló jelszóátállítás engedélyezése](#enable-self-service-password-reset)

-   [A felhasználó a multi-factor authentication állapotának ellenőrzése](#check-a-users-multi-factor-authentication-status)

-   [Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai](#check-a-users-authentication-contact-info)

-   [A felhasználói csoporttagság ellenőrzése](#check-a-users-group-memberships)

-   [A felhasználó licenc-hozzárendeléseket ellenőrzése](#check-a-users-assigned-licenses)

-   [A felhasználó a licenc hozzárendelése](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban

toocheck, ha egy felhasználói fiók található, kövesse a hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Ellenőrizze a hello tulajdonságainak hello felhasználói objektum toobe meg arról, hogy azok meg, mint a várt, és nincs adat hiányzik.

### <a name="check-a-users-account-status"></a>A felhasználói fiók állapotának ellenőrzése

toocheck egy felhasználói fiók állapota, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **profil**.

8.  A **beállítások** ügyeljen arra, hogy **blokk bejelentkezés** értéke túl**nem**.

### <a name="reset-a-users-password"></a>A jelszó alaphelyzetbe állítása

tooreset egy felhasználó jelszavát, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a hello **jelszó-átállítási** hello felhasználói panel felső hello gombra.

8.  Kattintson a hello **jelszó-átállítási** hello gombjára **jelszó-átállítási** megjelenő panelen.

9.  Másolás hello **ideiglenes jelszó** vagy **adjon meg egy új jelszót** hello felhasználó számára.

10. Az új jelszó toohello felhasználói kommunikációhoz, ezt a jelszót, a következő során jelentkezzen be az Active Directory tooAzure szükséges toochange legyenek.

### <a name="enable-self-service-password-reset"></a>Az önkiszolgáló jelszó-visszaállítás engedélyezése

tooenable önkiszolgáló jelszó alaphelyzetbe állítása, kövesse az alábbi hello telepítési lépéseket:

-   [Engedélyezze a felhasználók tooreset a Azure Active Directory-jelszavaikat](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [Felhasználók tooreset engedélyezése vagy az Active Directory helyszíni jelszavak módosítása](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a>A felhasználó a multi-factor authentication állapotának ellenőrzése

a felhasználó toocheck a multi-factor Authentication hitelesítési állapot, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  Kattintson a hello **multi-factor Authentication** hello panel felső hello gombra.

7.  Egyszer hello **multi-factor Authentication felügyeleti portál** terhelés esetén gondoskodjon arról, hogy hello **felhasználók** fülre.

8.  Keresés, szűréshez vagy rendezés hello felhasználói keresése hello azoknak a felhasználóknak.

9.  Azon felhasználók hello listájáról válassza hello felhasználói és **engedélyezése**, **tiltsa le a**, vagy **érvényesítése** többtényezős hitelesítést a kívánt módon működjenek.

  * **Megjegyzés:**: Ha egy felhasználó egy **kényszerített** állapotba kerül, előfordulhat, hogy túl beállítása**letiltott** ideiglenesen toolet újra be a fiókba. Miután bekerültek vissza, majd módosíthatja állapotukra túl**engedélyezve** újra toorequire őket toore regisztrálása során a következő kapcsolattartási adatait jelentkezzen be. Másik lehetőségként a lépésekkel hello a hello [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info) tooverify, vagy állítsa be ezeket az adatokat a számukra.

### <a name="check-a-users-authentication-contact-info"></a>Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai

a felhasználó hitelesítési kapcsolattartási adatok többtényezős hitelesítést, a feltételes hozzáférés, a Identity Protection és a jelszó alaphelyzetbe állításához használt toocheck hello lépéseket kövesse:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **profil**.

8.  Görgessen lefelé, túl**hitelesítési kapcsolattartási adatai**.

9.  **Felülvizsgálati** hello adatok regisztrált hello felhasználói és a frissítési igény szerint.

### <a name="check-a-users-group-memberships"></a>A felhasználói csoporttagság ellenőrzése

toocheck egy felhasználó csoporttagságok hajtsa végre az alábbi hello lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **csoportok** toosee, amely hello felhasználói csoportok tagja.

### <a name="check-a-users-assigned-licenses"></a>A felhasználó licenc-hozzárendeléseket ellenőrzése

toocheck a felhasználó hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.

### <a name="assign-a-user-a-license"></a>A felhasználó a licenc hozzárendelése 

a licenc tooa felhasználó tooassign kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.

8.  Kattintson a hello **hozzárendelése** gombra.

9.  Válassza ki **egy vagy több termék** választható termékek hello listája.

10. **Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek. Kattintson a **Ok** amikor ez befejeződik.

11. Kattintson a hello **hozzárendelése** tooassign ezen licencek toothis felhasználói gombra.

## <a name="problems-with-groups"></a>A csoportokkal kapcsolatos problémák

Alkalmazás-hozzáférés blokkolható egy csoportot, amely hozzá van rendelve toohello alkalmazás tooa kapcsolatos probléma miatt. Az alábbiakban néhány módszert, hibaelhárítási és csoportokkal vagy csoporttagsággal kapcsolatos problémák megoldásához:

-   [Ellenőrizze a csoport tagságát](#check-a-groups-membership)

-   [A dinamikus csoport tagsági feltételek ellenőrzése](#check-a-dynamic-groups-membership-criteria)

-   [Ellenőrizze a csoporthoz hozzárendelt licencekkel](#check-a-groups-assigned-licenses)

-   [Újból feldolgozza a csoport licencek](#reprocess-a-groups-licenses)

-   [Egy csoport a licenc hozzárendelése](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Ellenőrizze a csoport tagságát

toocheck egy csoport tagságát, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **összes csoport**.

6.  **Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **tagok** tooreview hello azoknak a felhasználóknak hozzárendelt toothis csoport.

### <a name="check-a-dynamic-groups-membership-criteria"></a>A dinamikus csoport tagsági feltételek ellenőrzése 

toocheck egy dinamikus csoport tagsági feltételek, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **összes csoport**.

6.  **Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **dinamikus tagsági szabályok szempontjából.**

8.  Felülvizsgálati hello **egyszerű** vagy **speciális** szabály definiálva, és azt szeretné, hogy a csoport tagjai toobe hello felhasználó megfelel-e ezek a feltételek.

### <a name="check-a-groups-assigned-licenses"></a>Ellenőrizze a csoporthoz hozzárendelt licencekkel

toocheck egy csoporthoz hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **összes csoport**.

6.  **Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee mely licencek hello csoport jelenleg hozzá van rendelve.

### <a name="reprocess-a-groups-licenses"></a>Újból feldolgozza a csoport licencek

tooreprocess egy csoporthoz hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **összes csoport**.

6.  **Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee mely licencek hello csoport jelenleg hozzá van rendelve.

8.  Kattintson a hello **újból feldolgozza** gomb tooensure, amely a hozzárendelt licencek toothis csoportnak a tagjai hello naprakészek legyenek. Ez eltarthat egy ideig, attól függően, hogy hello méretét és összetettségét hello csoport.

   >[!NOTE]
   >toodo Ez gyorsabb, érdemes lehet ideiglenesen hozzárendelés közvetlenül a licenc toohello felhasználó. [A felhasználó rendel egy licencet](#problems-with-application-consent).
   >
   >

### <a name="assign-a-group-a-license"></a>Egy csoport a licenc hozzárendelése

tooassign tooa licenccsoport, kövesse a hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **összes csoport**.

6.  **Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee mely licencek hello csoport jelenleg hozzá van rendelve.

8.  Kattintson a hello **hozzárendelése** gombra.

9.  Válassza ki **egy vagy több termék** választható termékek hello listája.

10. **Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek. Kattintson a **Ok** amikor ez befejeződik.

11. Kattintson a hello **hozzárendelése** tooassign licencek toothis csoportbiztonsági gombra. Ez eltarthat egy ideig, attól függően, hogy hello méretét és összetettségét hello csoport.

   >[!NOTE]
   >toodo Ez gyorsabb, érdemes lehet ideiglenesen hozzárendelés közvetlenül a licenc toohello felhasználó. [A felhasználó rendel egy licencet](#problems-with-application-consent).
   > 
   >

## <a name="problems-with-conditional-access-policies"></a>Feltételes hozzáférési szabályzatokkal kapcsolatos problémák

### <a name="check-a-specific-conditional-access-policy"></a>Egy adott feltételes hozzáférési házirend ellenőrzése

toocheck, illetve ellenőrizhető egy feltételes hozzáférési szabályzatot:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello navigációs menüjében.

5.  Kattintson a hello **feltételes hozzáférés** navigációs elem.

6.  Kattintson a hello házirend érdekli ellenőrzése.

7.  Ellenőrizze, hogy nincsenek-e nincs meghatározott feltételek, hozzárendelések vagy egyéb beállításokat, amelyek letilthatja a felhasználói hozzáférés.

   >[!NOTE]
   >Kezdésként érdemes lehet tootemporarily tiltsa le a házirend tooensure, ez nem érinti bejelentkezési biztosítási toodo, a set hello **házirend engedélyezése** túl váltása**nem** hello kattintson **mentése** gomb .
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Egy adott alkalmazás feltételes hozzáférési házirend ellenőrzése

toocheck vagy ellenőrizni az egyetlen alkalmazást jelenleg beállított feltételes hozzáférési szabályzatot:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello navigációs menüjében.

5.  Kattintson a **összes alkalmazás**.

6.  Keresés a hello alkalmazás kapcsolatban, vagy felhasználói hello kísérel meg toosign tooby alkalmazásban megjelenítése vagy egy alkalmazás azonosítója.

     >[!NOTE]
     >Ha nem látja a keresett hello alkalmazás, kattintson a hello **szűrő** gombra, majd bontsa ki a hello hatókör hello lista túl**összes alkalmazás**. Toosee több oszlopot, kattintson a hello **oszlopok** tooadd további részletek az alkalmazások gombra.
     >
     >

7.  Kattintson a hello **feltételes hozzáférés** navigációs elem.

8.  Kattintson a hello házirend érdekli ellenőrzése.

9.  Ellenőrizze, hogy nincsenek-e nincs meghatározott feltételek, hozzárendelések vagy egyéb beállításokat, amelyek letilthatja a felhasználói hozzáférés.

     >[!NOTE]
     >Kezdésként érdemes lehet tootemporarily tiltsa le a házirend tooensure, ez nem érinti bejelentkezési biztosítási toodo, a set hello **házirend engedélyezése** túl váltása**nem** hello kattintson **mentése** gomb .
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Egy adott feltételes hozzáférési házirend letiltása

toocheck, illetve ellenőrizhető egy feltételes hozzáférési szabályzatot:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello navigációs menüjében.

5.  Kattintson a hello **feltételes hozzáférés** navigációs elem.

6.  Kattintson a hello házirend érdekli ellenőrzése.

7.  Tiltsa le az hello házirend hello beállítása az **házirend engedélyezése** túl váltása**nem** hello kattintson **mentése** gombra.

## <a name="problems-with-application-consent"></a>Az alkalmazás hozzájárulásával problémák

Alkalmazás-hozzáférés is lehet tiltva, mert a megfelelő engedélyekkel a hozzájárulási művelet hello nem történt. Az alábbiakban néhány alkalmazás hozzájárulási problémák megoldásához és hibaelhárítást végezhessen módját:

-   [A felhasználói szintű hozzájárulási művelet végrehajtása](#perform-a-user-level-consent-operation)

-   [Bármely alkalmazás rendszergazdai hozzájárulási művelethez](#perform-administrator-level-consent-operation-for-any-application)

-   [Hajtsa végre a rendszergazdai hozzájárulási egyetlen bérlői alkalmazások](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Hajtsa végre a rendszergazdai hozzájárulási egy több-bérlős alkalmazáshoz](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>A felhasználói szintű hozzájárulási művelet végrehajtása

-   Bármely Open ID Connect-kompatibilis alkalmazás, amelyet engedélyeket igényel toohello alkalmazás bejelentkezési képernyő, lépjen a felhasználói szintű hozzájárulási toohello alkalmazás hello bejelentkezett felhasználó hajt végre.

-   Ha a toodo Ez programozott módon, lásd: [egyes felhasználói hozzájárulás kérése](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>Bármely alkalmazás rendszergazdai hozzájárulási művelethez

-   A **csak a hello V1 alkalmazásmodell használatával fejlesztett alkalmazások**, adja hozzá a rendszergazda szintű hozzájárulási toooccur kényszerítheti "**? rendszergazdai parancssorból =\_hozzájárulás**" toohello végét egy az alkalmazás bejelentkezési URL-címben.

-   A **bármely hello V2 alkalmazásmodell segítségével létrehozott alkalmazást**, a rendszergazdai hozzájárulási toooccur kényszerítheti a hello hello utasításokat követve **hello engedélyeket kérhet egy könyvtárat felügyeleti** szakasza [hello rendszergazda jóváhagyását végpont használatával](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Hajtsa végre a rendszergazdai hozzájárulási egyetlen bérlői alkalmazások

-   A **egyetlen bérlői alkalmazások** (mint fejleszt, vagy a szervezet saját), engedélyek kéréséhez, amely hajthat végre egy **rendszergazdai szintű hozzájárulási** nevében összes művelet Jelentkezzen be globális rendszergazdaként, majd kattintson a hello felhasználók **engedélyeket** hello hello tetején gomb **alkalmazás beállításjegyzék -&gt; összes alkalmazás -&gt; válasszon ki egy alkalmazást – &gt; Szükséges engedélyek** panelen.

-   A **bármely hello V1 vagy V2 alkalmazásmodell segítségével létrehozott alkalmazást**, a rendszergazdai hozzájárulási toooccur kényszerítheti a hello hello utasításokat követve **a hello engedélyek kéréséhez egy Directory-rendszergazda** szakasza [hello rendszergazda jóváhagyását végpont használatával](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Hajtsa végre a rendszergazdai hozzájárulási egy több-bérlős alkalmazáshoz

-   A **több-bérlős alkalmazásokhoz** adott kérelem engedélyek (például egy alkalmazás egy harmadik féltől származó, vagy a Microsoft, házon belül fejlesztett alkalmazásokra) hajthat végre egy **rendszergazdai szintű hozzájárulási** műveletet. Jelentkezzen be globális rendszergazdaként, és kattintson a hello **engedélyeket** alatt hello gomb **vállalati alkalmazások –&gt; összes alkalmazás -&gt; válasszon ki egy alkalmazást -&gt; Engedélyek** panel (rendelkezésre álló hamarosan).

-   A hello hello utasításokat követve is kényszerítheti a rendszergazdai hozzájárulási toooccur **hello engedélyeket kérhet a directory-rendszergazda** szakasza [hello rendszergazda jóváhagyását végponthasználatával](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Következő lépések
[Hello rendszergazda jóváhagyását végpont használatával](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

