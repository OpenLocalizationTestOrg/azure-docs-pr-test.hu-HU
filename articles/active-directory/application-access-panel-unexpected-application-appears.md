---
title: "aaaHow alkalmazások jelennek meg a hozzáférési panel hello |} Microsoft Docs"
description: "Ezért az alkalmazás jelenik-e meg a hozzáférési Panel hello hibaelhárítása"
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
ms.reviewr: japere
ms.openlocfilehash: 14ee732c4ed5260cba878e949cf9d90877aee67e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-applications-appear-on-hello-access-panel"></a>Alkalmazások megjelenésének hello hozzáférési panel

hello hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) tooview és kezdő felhőalapú alkalmazások, hogy hello Azure AD-rendszergazda rendelkezik hozzáféréssel őket. Ezeket az alkalmazásokat úgy vannak konfigurálva, hello Azure AD portálon hello felhasználó nevében. Üdvözöljük a rendszergazdákat hello toohello Alkalmazásfelhasználó közvetlenül vagy egy felhasználó része hello felhasználói hozzáférési Panel szereplő hello alkalmazás eredményezve tooa csoportot hozhat létre.

## <a name="general-issues-toocheck-first"></a>Általános problémák első toocheck

-   Ha az alkalmazás csak el lett távolítva a felhasználó vagy csoport hello felhasználó tagja, próbálja toosign bejövő és kimenő adatforgalma újra hello felhasználói hozzáférési panelre után néhány perc toosee hello alkalmazás eltávolításakor.

-   A licenc csak el lett távolítva a felhasználó vagy csoport hello felhasználó esetén tagja ennek attól függően, hogy hello méretét és összetettségét hello csoportot a végzett módosítások toobe hosszú ideig is eltarthat. Lehetővé teszi további alkalommal jelentkezik be a hozzáférési Panel hello előtt.

## <a name="problems-related-tooassigning-applications-toousers"></a>Problémák kapcsolódó tooassigning alkalmazások toousers

Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert azok már korábban kiosztott tooit. Az alábbiakban néhány módon toocheck:

-   [Ellenőrizze, hogy ha a felhasználó hozzá van rendelve toohello alkalmazás](#check-if-a-user-is-assigned-to-the-application)

-   [Annak ellenőrzése, hogy egy felhasználó egy licenc kapcsolódó toohello alkalmazás](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-toohello-application"></a>Ellenőrizze, hogy ha a felhasználó hozzá van rendelve toohello alkalmazás

toocheck a felhasználó hozzá van rendelve a toohello alkalmazást, kövesse az hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

6.  **Keresési** a szóban forgó hello alkalmazás hello nevét.

7.  Kattintson a **felhasználók és csoportok**.

8.  Ellenőrizze a toosee, ha a felhasználó a toohello alkalmazás hozzá van rendelve.

  * Ha azt szeretné, hogy tooremove hello felhasználói hello alkalmazásból **hello sorban kattintson** hello felhasználó, és válassza a **törlése**.

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a>Annak ellenőrzése, hogy egy felhasználó egy licenc kapcsolódó toohello alkalmazás

toocheck a felhasználó hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.

   * Hello felhasználó van hozzárendelve, tooan Office-licencet az első fél Office alkalmazások tooappear ezzel engedélyezi a felhasználó hozzáférési Panel hello.

## <a name="problems-related-tooassigning-applications-toogroups"></a>Problémák kapcsolódó tooassigning alkalmazások toogroups

Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert hello alkalmazás rendelt csoportba. Az alábbiakban néhány módon toocheck:

-   [A felhasználói csoporttagság ellenőrzése](#check-a-users-group-memberships)

-   [Ellenőrizze, hogy a felhasználó tagja egy csoportnak tooa licenc hozzárendelése](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>A felhasználói csoporttagság ellenőrzése

toocheck egy csoport tagságát, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **csoportok.**

8.  Toosee ellenőrizze, hogy a felhasználók egy csoportjának toohello alkalmazás részét.

   * Ha azt szeretné, hogy tooremove hello felhasználói hello csoportból **hello sorban kattintson** hello csoport, és válassza a törlés.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-tooa-license"></a>Ellenőrizze, hogy a felhasználó tagja egy csoportnak tooa licenc hozzárendelése

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **csoportok.**

8.  Kattintson egy meghatározott csoport hello sorát.

9.  Kattintson a **licencek** melyik licencek hello csoporthoz rendelt tooit toosee.

  * Ha hello csoport hozzárendelt tooan Office-licencet ezzel előfordulhat, hogy engedélyezi bizonyos első fél Office alkalmazások tooappear hello felhasználói hozzáférési panelen.


## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Ha a lépések nem hello hello probléma megoldásához.

Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:

-   Megfelelési hiba azonosítója

-   Egyszerű felhasználónév (felhasználó e-mail címe)

-   Bérlőazonosító

-   Böngésző típusa

-   Időzóna és idő/időkeretre során hiba történik.

-   Fiddler nyomkövetések

## <a name="next-steps"></a>Következő lépések
[Alkalmazások kezelése az Azure Active Directoryban](active-directory-enable-sso-scenario.md)
