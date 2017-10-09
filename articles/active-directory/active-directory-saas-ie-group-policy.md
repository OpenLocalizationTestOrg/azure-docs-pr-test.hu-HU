---
title: "Azure hozzáférési Panel bővítményét az Internet Explorer egy csoportházirend-objektummal aaaDeploy |} Microsoft Docs"
description: "Hogyan toouse csoport házirend toodeploy hello Internet Explorer bővítmény hello saját alkalmazások portálhoz."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 7c2d49c8-5be0-4e7e-abac-332f9dfda736
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 926f15950bbe81d2fbfe1b0b856470da40880d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-hello-access-panel-extension-for-internet-explorer-using-group-policy"></a>Hogyan tooDeploy hello hozzáférési Panel bővítményét az Internet Explorer csoportházirend használatával
Ez az oktatóanyag bemutatja, hogyan toouse csoport házirend tooremotely hello hozzáférési Panel bővítmény Internet Explorer telepítése a felhasználók gépeken. A bővítmény szükség, az Internet Explorer felhasználók, akik toosign szolgáltatással konfigurált alkalmazásokba [jelszó-alapú egyszeri bejelentkezést](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

Javasoljuk, hogy a rendszergazdák hello telepítési bővítmény automatizálásához. Ellenkező esetben a felhasználók toodownload rendelkezik, és hello-kiterjesztés telepítése, amelyek nagyon eséllyel fordulnak elő toouser hiba, és rendszergazdai engedélyekkel kell rendelkeznie. Ez az oktatóanyag ismerteti egyik lehetséges módja automatizálása a szoftverek központi telepítése csoportházirend használatával. [További információ a csoportházirend.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

hello hozzáférési Panel bővítmény érhető el is [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) és [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), egyike sem szükséges rendszergazdai engedélyek tooinstall.

## <a name="prerequisites"></a>Előfeltételek
* Ezzel beállította [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), és a felhasználók gépek tooyour tartományhoz csatlakozott.
* Hello "Beállítások módosítása" engedéllyel tooedit hello csoportházirend-objektumot (GPO) kell rendelkeznie. Alapértelmezés szerint a következő biztonsági csoportokat hello tagjai rendelkezik ilyen engedéllyel: tartományi rendszergazdák, a vállalati rendszergazdák és a Csoportházirend-létrehozó tulajdonosok. [Részletek](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-hello-distribution-point"></a>1. lépés: Hello terjesztési pont létrehozása
Először egy hálózati helyre hello gépek által elérhető, amely a tooremotely telepítés hello bővítmény kívánja hello telepítőcsomag kell elhelyezni. toodo, kövesse az alábbi lépéseket:

1. Jelentkezzen be rendszergazdaként toohello kiszolgáló
2. A hello **Kiszolgálókezelő** ablakban nyissa meg túl**fájl- és tárolási szolgáltatások**.
   
    ![Nyissa meg a fájl- és tárolási szolgáltatások](./media/active-directory-saas-ie-group-policy/files-services.png)
3. Nyissa meg toohello **megosztások** fülre. Kattintson a **feladatok** > **új megosztás...**
   
    ![Nyissa meg a fájl- és tárolási szolgáltatások](./media/active-directory-saas-ie-group-policy/shares.png)
4. Teljes hello **új megosztás varázsló** és set engedélyek tooensure, hogy az elérhető a felhasználók gépek. [További információ a megosztásokat.](https://technet.microsoft.com/library/cc753175.aspx)
5. Töltse le a Microsoft Windows Installer-csomag (.msi fájlt) a következő hello: [hozzáférési Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)
6. Másolja a hello telepítő szükséges tooa csomaghely hello megosztáson.
   
    ![Hello .msi toohello fájlmegosztásról.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. Ellenőrizze, hogy az ügyfél gépek képesek tooaccess hello telepítőcsomag hello megosztásból. 

## <a name="step-2-create-hello-group-policy-object"></a>2. lépés: Hello csoportházirend-objektum létrehozása
1. Jelentkezzen be az Active Directory tartományi szolgáltatások (AD DS) telepítése üzemeltető toohello kiszolgálón.
2. A Kiszolgálókezelő hello, váltson túl**eszközök** > **csoportházirend-kezelő**.
   
    ![Nyissa meg tooTools > csoport házirend kezelése](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. A bal oldali ablaktáblájában hello hello **csoportházirend-kezelő** ablakban, a szervezeti egység (OU) hierarchia megtekintése, és milyen hatókörben tooapply hello csoportházirend szeretné meghatározni. Dönthet egy kis OU toodeploy tooa toopick néhány felhasználó tesztelési, vagy például előfordulhat, hogy válassza ki a legfelső szintű szervezeti egység toodeploy tooyour teljes szervezet számára.
   
   > [!NOTE]
   > Volna, például toocreate vagy szerkesztésekor a szervezeti egységekhez (OU-k), hátsó toohello Kiszolgálókezelő váltson, és nyissa meg túl**eszközök** > **Active Directory – felhasználók és számítógépek**.
   > 
   > 
4. Miután kiválasztotta a szervezeti egység, a jobb gombbal kattintson rá, és válassza ki **egy csoportházirend-objektum létrehozása ebben a tartományban, és hivatkozás létrehozása itt...**
   
    ![Hozzon létre egy új csoportházirend-objektum](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. A hello **új csoportházirend-objektum** parancssorból, írja be egy nevet a hello új csoportházirend-objektum.
   
    ![Név hello új csoportházirend-objektum](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. Kattintson a jobb gombbal hello csoportházirend-objektumot létrehozta, és válassza ki **szerkesztése**.
   
    ![Hello új csoportházirend-objektum szerkesztéséhez](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-hello-installation-package"></a>3. lépés: Hello telepítőcsomag hozzárendelése
1. Határozza meg, hogy szeretné-e toodeploy hello bővítmény alapján **számítógép konfigurációja** vagy **felhasználói konfiguráció**. Használata esetén [számítógép konfigurációja](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), függetlenül attól, amely a felhasználók jelentkezhetnek be tooit hello számítógépen telepítve van a hello bővítmény. A [felhasználói konfiguráció](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), felhasználóknál hello bővítmények vannak telepítve, függetlenül azok a számítógépek jelentkeznek be rájuk vonatkozóan.
2. A bal oldali ablaktáblájában hello hello **Csoportházirendkezelés-szerkesztő** ablakban, a következő mappák elérési útjaiban, attól függően, hogy milyen típusú konfigurációs úgy döntött, hogy hello lépjen tooeither:
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. Kattintson a jobb gombbal **Szoftvertelepítés**, majd jelölje be **új** > **csomag...**
   
    ![Hozzon létre egy új telepítési csomag](./media/active-directory-saas-ie-group-policy/new-package.png)
4. Lépjen toohello megosztott mappa, amely tartalmazza a telepítőcsomag hello a [1. lépés: hello terjesztési pont létrehozása](#step-1-create-the-distribution-point), válasszon hello .msi fájlt, és kattintson a **nyitott**.
   
   > [!IMPORTANT]
   > Ha hello megosztás ugyanazon a kiszolgálón található, győződjön meg arról, hogy Ön keresztül érik el hello .msi hello hálózati fájl elérési útját, nem pedig hello helyi fájl elérési útját.
   > 
   > 
   
    ![Válassza ki a telepítőcsomag hello hello megosztott mappából.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. A hello **szoftver központi telepítése** rákérdezés, jelölje be **hozzárendelt** a központi telepítési módszer. Ezután kattintson az **OK** gombra.
   
    ![Válassza ki a hozzárendelt, majd kattintson az OK gombra.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

hello bővítmény már telepített toohello szervezeti Egységet, verziószámával. [További információ a Csoportházirend szoftver telepítése.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-hello-extension-for-internet-explorer"></a>4. lépés: Automatikus engedélyezése hello az Internet Explorer bővítmény
Továbbá toorunning hello installer, az Internet Explorer minden bővítmény explicit módon engedélyezni kell ahhoz, hogy kell használni. Hajtsa végre hello lépéseket alatti tooenable hello hozzáférési Panel bővítmény csoportházirend használatával:

1. A hello **Csoportházirendkezelés-szerkesztő** ablakot, az elérési utak, attól függően, hogy milyen típusú konfigurációs beállítást választja a következő hello lépjen tooeither [3. lépés: hozzárendelése hello telepítőcsomag](#step-3-assign-the-installation-package):
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. Kattintson a jobb gombbal **Bővítménylista**, és válassza ki **szerkesztése**.
    ![Bővítménylista szerkesztése.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)
3. A hello **Bővítménylista** ablakban válassza ki **engedélyezve**. Ekkor a hello **beállítások** kattintson **megjelenítése...** .
   
    ![Kattintson az Engedélyezés parancsra, majd kattintson a megjelenítése...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. A hello **tartalom megjelenítése** ablak, hajtsa végre az alábbi lépésekkel hello:
   
   1. Az első oszlop hello (hello **Azonosítónév** mező), másolja és illessze be a következő Osztályazonosító hello:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   2. Hello második oszlop (hello **érték** mező), írja be a következő érték hello:`1`
   3. Kattintson a **OK** tooclose hello **tartalom megjelenítése** ablak.
      
      ![Töltse ki a fenti hello értékeket.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. Kattintson a **OK** tooapply a módosításokat és Bezárás hello **Bővítménylista** ablak.

hello bővítményét most engedélyezni kell a kiválasztott hello hello gépek szervezeti Egységet. [Csoport házirend tooenable használatával kapcsolatos további, vagy tiltsa le a gyakori.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>5. lépés (nem kötelező): Prompt "Jelszó megjegyzése" letiltása
Amikor a felhasználók bejelentkezési toowebsites hello hozzáférési Panel bővítmény használatával, az Internet Explorer előfordulhat, hogy megjelenítése hello következő kérni, azzal a kérdéssel "szeretné toostore jelszavát?"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Ha tooprevent kívánja a felhasználók megtekinthessék a kérdéshez, majd lépésekkel hello automatikus kitöltés tooprevent alatt a jelszavak megjegyzése:

1. A hello **Csoportházirendkezelés-szerkesztő** ablakban, a lenti lépjen toohello elérési út. A konfigurációs beállítás csak érhető el a **felhasználói konfiguráció**.
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. Nevű hello beállítás található **hello automatikus kitöltés szolgáltatás a felhasználónevek és jelszavak űrlapokon**.
   
   > [!NOTE]
   > Active Directory korábbi verzióiban ez a beállítás hello nevű sorolhatja **nem teszik lehetővé az automatikus kitöltés toosave jelszavak**. hello konfigurációs beállítás különbözik a hello beállítása ebben az oktatóanyagban leírt.
   > 
   > 
   
    ![Ne felejtse el toolook ehhez a felhasználói beállítások szakasz.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. Hello beállítás fölött kattintson jobb gombbal, és válassza ki **szerkesztése**.
4. Című hello ablakban **hello automatikus kitöltés szolgáltatás a felhasználónevek és jelszavak űrlapokon**, jelölje be **letiltott**.
   
    ![Válassza ki a letiltása](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. Kattintson a **OK** tooapply ezeket a módosításokat és Bezárás hello ablak.

Felhasználók többé nem kell tudni toostore hitelesítő adataikat, és automatikusan hajthat végre tooaccess korábban tárolt hitelesítő adatok használata. Azonban ez a házirend lehetővé teszi felhasználók toocontinue toouse más típusú űrlapmezők, például a keresési mezők automatikusan hajthat végre.

> [!WARNING]
> Ha a házirend engedélyezve van, miután a felhasználók kiválasztott toostore egyes hitelesítő adatokat, ez a házirend lesz *nem* hello már tárolt hitelesítő adatok törlése.
> 
> 

## <a name="step-6-testing-hello-deployment"></a>6. lépés: Hello központi telepítés tesztelése
Lépésekkel hello tooverify alatt Ha hello bővítmény telepítése sikeres volt:

1. Ha használatával telepített **számítógép konfigurációja**, jelentkezzen be egy ügyfélszámítógépre, amelyhez tartozik a kiválasztott OU toohello [2. lépés: hello csoportházirend-objektum létrehozása](#step-2-create-the-group-policy-object). Ha használatával telepített **felhasználói konfiguráció**, győződjön meg arról, hogy toosign a toothat szervezeti Egységhez tartozik felhasználóként.
2. Ez eltarthat néhány bejelentkezési hello csoportházirend modulok toofully frissítés változik-e. tooforce hello frissítés, nyissa meg a **parancssor** ablakot, és futtassa a következő parancs hello:`gpupdate /force`
3. Hello telepítési tootake hely hello gépet újra kell indítani. Rendszerindítási szokásos hello bővítmény során telepíti jelentősen több időt is igénybe vehet.
4. Az újraindítás után nyissa meg a **Internet Explorer**. A hello ablak hello jobb felső sarkában kattintson **eszközök** (hello fogaskerék ikonra), majd válassza ki **bővítmények kezelése**.
   
    ![Nyissa meg tooTools > Bővítmények kezelése](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. A hello **bővítmények kezelése** ablakban győződjön meg arról, hogy hello **hozzáférési Panel bővítmény** telepítve van, és hogy a **állapot** túl lett beállítva**engedélyezve**.
   
    ![Győződjön meg arról, hogy hello hozzáférési Panel bővítmény van telepítve és engedélyezve van.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md)
* [Az Internet Explorer hello hozzáférési Panel bővítmény hibaelhárítása](active-directory-saas-ie-troubleshooting.md)

