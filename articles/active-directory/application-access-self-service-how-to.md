---
title: "aaaHow tooconfigure önkiszolgáló alkalmazás-hozzárendelés |} Microsoft Docs"
description: "Lehetővé teszi a önkiszolgáló alkalmazás hozzáférés tooallow felhasználók toofind saját alkalmazások"
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
ms.openlocfilehash: d25a0146c4c8cebf9c2ae8c516f094a8eccb4570
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-self-service-application-assignment"></a>Hogyan tooconfigure önkiszolgáló alkalmazás-hozzárendelés

Mielőtt a felhasználók saját maguk felderíthetők az alkalmazások a hozzáférési panelen, meg kell-e tooenable **önkiszolgáló alkalmazás-hozzáférés** tooany alkalmazások, hogy kívánja-e tooallow felhasználók tooself-felderítését és ő hozzáférést tud adni.

Ez a funkció az Ön kiváló módja toosave időt és pénzt, egy IT-részleg, és erősen ajánlott az Azure Active Directoryban a modern alkalmazások központi telepítésének részeként.

Ez a szolgáltatás használatával:

-   Önálló felderítése hello alkalmazások felhasználók [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) nélkül bothering hello informatikai csoport.

-   Adja hozzá ezeket felhasználók tooa előre konfigurált csoportot, tekintse meg, akik hozzáférést kér, megszünteti a hozzáférést, és hozzárendelt toothem hello szerepkörök kezelése.

-   Opcionálisan egy vállalati jóváhagyó tooapprove alkalmazás hozzáférési kérelmek engedélyezéséhez, IT-részleg hello nem kell.

-   Választható lehetőségként konfigurálhatja too10 használhatják, akik jóváhagyhatja access toothis alkalmazás fel.

-   Opcionálisan lehetővé teszi a vállalati jóváhagyó tooset hello jelszavak azoknak a felhasználóknak használható toosign toohello alkalmazásban, közvetlenül a hello vállalati jóváhagyó [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).

-   Nem kötelező automatikus hozzárendelése közvetlenül hozzárendelt önkiszolgáló felhasználók tooan alkalmazás-szerepkör.

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
[Azure Active Directory beállítása önkiszolgáló csoportkezelés](active-directory-accessmanagement-self-service-group-management.md)
