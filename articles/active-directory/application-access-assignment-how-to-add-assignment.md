---
title: "aaaHow tooassign felhasználók és csoportok tooan alkalmazás |} Microsoft Docs"
description: "Felhasználók toohello alkalmazás toogrant hozzáférés hozzárendelése"
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
ms.openlocfilehash: e039a26e4b8f88ad747354859f1071b8f74b6789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-and-groups-tooan-application"></a>Hogyan tooassign felhasználók és csoportok tooan alkalmazás

Mielőtt a felhasználók lehetőségek közül választhat hello alább egy adott alkalmazáshoz, meg kell-e toofirst **rendeljen hozzájuk toohello alkalmazás** toogrant őket elérni:

-   Az alkalmazás által elérése **toohello alkalmazás URL-címet közvetlenül a Navigálás** (más néven Szolgáltató által kezdeményezett bejelentkezés).

-   Az alkalmazás hozzáférhet a hello **felhasználói URL-CÍMEN** olyan alkalmazást **tulajdonságok** (más néven IDP által kezdeményezett bejelentkezés) lap.

-   Tekintse meg az alkalmazás jelennek meg azok [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) vagy mobilalkalmazás.

-   Tekintse meg az alkalmazás jelennek meg azok [Office 365 alkalmazásindító](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).

## <a name="methods-tooassign-applications-with-azure-active-directory"></a>Módszerek tooassign alkalmazásokat az Azure Active Directoryval 

Alkalmazások az Azure Active Directoryval rendelhet 3 módja van:

-   [Rendelje hozzá a felhasználó közvetlenül tooan alkalmazás-rendszergazdaként](#assign-a-user-directly-as-an-administrator)

-   [Rendelje hozzá egy csoporthoz közvetlenül tooan alkalmazás-rendszergazdaként](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [Lehetővé teszi a önkiszolgáló alkalmazás hozzáférés tooallow felhasználók toofind saját alkalmazások](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a>Rendelje hozzá a felhasználó közvetlenül rendszergazdaként

tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.

8.  Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.

9.  hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.

10. Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.

11. Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**. Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.

12. **Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.

13. Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.

14. **Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.

15. Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.

Rövid időn belül a kijelölt hello felhasználók kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch.

## <a name="assign-a-group-directly-tooan-application-as-an-administrator"></a>Rendelje hozzá egy csoporthoz közvetlenül tooan alkalmazás-rendszergazdaként

egy vagy több tooassign csoportok közvetlenül, tooan alkalmazás kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.

8.  Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.

9.  hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.

10. Hello típusa **teljes csoportnév** érdekli hello való hozzárendelése hello csoport **Keresés név vagy e-mail cím alapján** keresőmezőbe.

11. Hello rámutat **csoport** a hello lista tooreveal egy **jelölőnégyzet**. Kattintson a profil fénykép vagy embléma tooadd hello jelölőnégyzet következő toohello csoport a felhasználó toohello **kijelölt** listája.

12. **Nem kötelező:** Ha túl szeretné**egynél több csoport hozzáadása**, egy másik típus **teljes csoport neve** hello be **Keresés név vagy e-mail cím alapján** keresőmezőbe hello jelölőnégyzet tooadd kattintson a csoport toohello **kijelölt** listája.

13. Ha befejezte a csoportok kiválasztásával, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.

14. **Választható lehetőség:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect egy szerepkör tooassign toohello csoportokat kijelölt.

15. Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt csoportok.

Rövid időn belül hello felhasználók kijelölt hello csoportok belül kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch. Ha ezek a dinamikus csoportok, néhány további feldolgozás késés előfordulhat ezek a felhasználók számára csoportok magukba belül szereplő hozzárendelésekben.

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a>Lehetővé teszi a önkiszolgáló alkalmazás hozzáférés tooallow felhasználók toofind saját alkalmazások

Önkiszolgáló alkalmazás hozzáférése egy remek mód tooallow felhasználók tooself-alkalmazások észlelése, nem kötelezően engedélyezése hello üzleti csoport tooapprove toothose alkalmazások. Engedélyezheti, hogy hello üzleti csoport toomanage hello hitelesítő adatokat kap toothose jelszó egyszeri bejelentkezést az alkalmazások közvetlenül a hozzáférési panel a felhasználókat.

tooenable önkiszolgáló alkalmazás access tooan alkalmazást, kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooenable önkiszolgáló hozzáférés toofrom hello lista hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **önkiszolgáló** hello alkalmazás bal oldali navigációs menüjében.

8.  tooenable önkiszolgáló alkalmazás hozzáférése az alkalmazáshoz, kapcsolja be a hello **toorequest access toothis alkalmazás engedélyezése a felhasználók?** túl Váltás**igen.**

9.  A következő tooselect hello csoport toowhich felhasználókat, akik kérnek hozzáférést toothis alkalmazás hozzá kell adni, kattintson a hello választó következő toohello címke **toowhich csoport kell hozzárendelve felhasználókat kell felvenni?** válasszon ki egy csoportot.

10. **Választható lehetőség:** Ha toorequire egy üzleti jóváhagyása előtt a felhasználók hozzáférhetnek, állítsa be a hello **access toothis alkalmazás megadása előtt jóvá kell hagyni?** túl váltása**Igen**.

11. **Választható lehetőség: az alkalmazások csak a jelszó egyszeri bejelentkezés használatával** Ha tooallow adott üzleti jóváhagyóknak toospecify hello jelszavak jóváhagyott felhasználók toothis kérelmet küldött, állítsa be a hello **jóváhagyóknak tooset engedélyezése felhasználói jelszavak ehhez az alkalmazáshoz?**  túl váltása**Igen**.

12. **Választható lehetőség:** toospecify hello üzleti jóváhagyóknak tooapprove access toothis alkalmazást, akik kattintson hello választó következő toohello címke **tooapprove access toothis alkalmazás engedélyezett?** tooselect mentése too10 egyedi üzleti jóváhagyóknak.

  >[!NOTE]
  >Csoportok használata nem támogatott.
  >
  >

13. **Választható lehetőség:** **az alkalmazások, amelyeknél a szerepkörök**, ha tooassign jóváhagyott felhasználók önkiszolgáló tooa szerepkör hello választó következő toohello kattintson **toowhich szerepkör felhasználóhoz rendelni ezen alkalmazás?**  tooselect hello szerepkör toowhich ezeket a felhasználókat hozzá kell rendelni.

14. Kattintson a hello **mentése** hello panel toofinish hello tetején gombra.

Ha befejezte az önkiszolgáló Alkalmazáskonfiguráció, a felhasználók navigálhatnak tootheir [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) hello kattintson **+ Hozzáadás** gomb toofind hello alkalmazások toowhich engedélyezte Önkiszolgáló hozzáférést. Üzleti jóváhagyóknak is megjelenik egy értesítés a saját [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/). Engedélyezheti a felhasználói hozzáférés tooan a jóváhagyást igénylő alkalmazás kérelmezésekor értesítő e-mailt. 

Ezek a jóváhagyások támogatja egyetlen jóváhagyási munkafolyamatok csak, ami azt jelenti, hogy több jóváhagyó ad meg, ha bármely egyetlen jóváhagyó jóváhagyó access toohello alkalmazást is.

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)
