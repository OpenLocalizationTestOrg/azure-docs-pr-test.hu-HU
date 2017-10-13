---
title: "Önkiszolgáló alkalmazás-hozzárendelés konfigurálása |} Microsoft Docs"
description: "A felhasználók a saját alkalmazások keresése az önkiszolgáló alkalmazás hozzáférésének engedélyezése"
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
ms.openlocfilehash: 7991dc19d41c5eb8e149c3ee08069e1a162929cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-self-service-application-assignment"></a>Önkiszolgáló alkalmazás-hozzárendelés konfigurálása

Mielőtt a felhasználók saját maguk felderíthetők az alkalmazások a hozzáférési panelen, engedélyeznie kell **önkiszolgáló alkalmazás-hozzáférés** olyan alkalmazásokat, amelyek önálló felderítését, és kérjen engedélyezni szeretné a hozzáférést.

Ez a szolgáltatás ahhoz, hogy időt és pénzt elmentse az IT-részleg remek mód, és erősen ajánlott az Azure Active Directoryban a modern alkalmazások központi telepítésének részeként.

Ez a szolgáltatás használatával:

-   Önálló felderítése az alkalmazások a felhasználók a [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) az IT-részleg bothering nélkül.

-   Ezek a felhasználók hozzáadása az előre konfigurált csoporthoz, tekintse meg, akik hozzáférést kér, távolítsa el a hozzáférést, és a hozzájuk rendelt szerepkörök kezelése.

-   Alkalmazás-hozzáférési kérelmek jóváhagyása, így nem kell az IT-részleg a vállalati jóváhagyó opcionálisan engedélyezése.

-   Választható lehetőségként konfigurálhatja legfeljebb 10 egyéni felhasználók számára, akik jóváhagyhatja az alkalmazáshoz való hozzáférés.

-   Opcionálisan engedélyezése a vállalati jóváhagyó beállítása a jelszavak azoknak a felhasználóknak használatával jelentkezzen be az alkalmazás közvetlenül a vállalati jóváhagyó a [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/).

-   Nem kötelező automatikus hozzárendelése közvetlenül egy alkalmazás szerepkörhöz hozzárendelt felhasználók önkiszolgáló.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>A felhasználók a saját alkalmazások keresése az önkiszolgáló alkalmazás hozzáférésének engedélyezése

Önkiszolgáló alkalmazás-hozzáférés kiváló módja annak, hogy önálló felderítéséhez az alkalmazások, felhasználók igény szerint engedélyezett az üzleti csoport ezeket az alkalmazásokat a hozzáférést. Engedélyezze az üzleti csoport társítva a hozzáférési panel azoknak a felhasználóknak jelszót egyszeri bejelentkezést az alkalmazások jobb a hitelesítő adatok kezeléséhez.

Ahhoz, hogy az alkalmazás önkiszolgáló hozzáférést egy alkalmazáshoz, kövesse az alábbi lépéseket:

1.  Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.

3.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.

  * Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**

6.  Válassza ki az önkiszolgáló engedélyezni szeretné a hozzáférést a listából.

7.  Ha az alkalmazás betölt, kattintson **önkiszolgáló** az alkalmazás bal oldali navigációs menüjében.

8.  Ahhoz, hogy az önkiszolgáló alkalmazás hozzáférése az alkalmazáshoz, kapcsolja be a **az alkalmazáshoz való hozzáférés kérését?** kapcsolót **igen.**

9.  A következő, amelyekhez a felhasználók, akik kérnek az alkalmazáshoz való hozzáférés hozzá kell adni a csoport kijelöléséhez kattintson a felirat melletti választó **mely csoporthoz rendelt felhasználók vehet fel?** válasszon ki egy csoportot.

10. **Választható lehetőség:** előtt egy üzleti jóváhagyás megkövetelése, ha a felhasználók jogosultak-e a hozzáférést, és állítsa a **az alkalmazáshoz való hozzáférés megadása előtt jóvá kell hagyni?** kapcsolót **Igen**.

11. **Választható lehetőség: az alkalmazások csak a jelszó egyszeri bejelentkezés használatával** adott üzleti jóváhagyóknak adhatja meg a jelszavakat, a jóváhagyott felhasználók számára az alkalmazás küldött engedélyezni szeretné, ha a **engedélyezése az alkalmazás felhasználói jelszavak beállítása jóváhagyóknak?** kapcsolót **Igen**.

12. **Választható lehetőség:** az üzleti jóváhagyóknak, akik jogosultak a hagyja jóvá az alkalmazáshoz való hozzáférés megadásához kattintson a felirat melletti választó **ki jogosult az alkalmazáshoz való hozzáférés jóváhagyásához?** legfeljebb 10 egyéni üzleti jóváhagyóknak kiválasztásához.

   >[!NOTE]
   >Csoportok használata nem támogatott.
   >
   >

13. **Választható lehetőség:** **az alkalmazások, amelyeknél a szerepkörök**, ha önkiszolgáló jóváhagyott felhasználók hozzárendelése egy szerepkörhöz, kattintson a Tovább gombra a választó a **mely szerepkörhöz felhasználók hozzárendeli az alkalmazásban?** válassza ki a szerepkört, amelyhez hozzá kell rendelni a felhasználók számára.

14. Kattintson a **mentése** gombra a befejezéshez panel tetején.

Ha befejezte az önkiszolgáló Alkalmazáskonfiguráció, felhasználók lépjen a [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) , és kattintson a **+ Hozzáadás** gombra kattintva keresse meg az alkalmazások, amelyhez engedélyezte a hozzáférést az önkiszolgáló. Üzleti jóváhagyóknak is megjelenik egy értesítés a saját [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/). Egy e-mailt, amely értesíti őket, amikor a felhasználó által kért a jóváhagyást igénylő alkalmazáshoz való hozzáférés engedélyezéséhez. 

Ezek a jóváhagyások támogatja egyetlen jóváhagyási munkafolyamatok csak, ami azt jelenti, hogy több jóváhagyó ad meg, ha bármely egyetlen jóváhagyó előfordulhat, hogy jóváhagyó hozzáférni az alkalmazáshoz.

## <a name="next-steps"></a>Következő lépések
[Azure Active Directory beállítása önkiszolgáló csoportkezelés](active-directory-accessmanagement-self-service-group-management.md)
