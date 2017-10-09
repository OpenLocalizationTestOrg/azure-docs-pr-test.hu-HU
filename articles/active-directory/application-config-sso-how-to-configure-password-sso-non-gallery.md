---
title: "aaaHow tooconfigure jelszó egyszeri bejelentkezés egy nem galéria applicationn a(z) |} Microsoft Docs"
description: "Hogyan tooconfigure egyéni nem galéria alkalmazás biztonságos jelszó-alapú egyszeri bejelentkezést Ha nem szerepel az Azure AD Application Gallery hello"
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
ms.openlocfilehash: e3d0e658f792a0a63110a198811edc66acd6ccf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Hogyan tooconfigure jelszó egyszeri bejelentkezés egy nem galéria alkalmazáshoz

Ezenkívül toohello lehetőségek található belül hello Azure AD Application Gallery, akkor is hello beállítás tooadd egy **nem galéria alkalmazás** amikor hello kívánt alkalmazás nem szerepel ott. Használja ezt a képességet, hozzáadhat bármely alkalmazás, amely a szervezet már létezik, vagy bármely alkalmazás, amelyet esetleg olyan szállítótól, akik már nem része a hello [az Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

Egy nem galéria alkalmazás felvétele után is konfigurálhat hello egyszeri bejelentkezés az alkalmazás által használt módszer hello kiválasztásával **egyszeri bejelentkezés** navigációs elem a hello a vállalati alkalmazás [Azure portál ](https://portal.azure.com/).

Hello egyszeri bejelentkezés módszer elérhető tooyou egyik hello [jelszó-alapú egyszeri bejelentkezést](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) lehetőséget. A hello **nem galéria alkalmazás hozzáadása** élményt, egy HTML-alapú felhasználónév Renderelés bármely olyan alkalmazás integrálható, és a jelszó bemeneti mező, még akkor is, ha nincs-e az előre integrált alkalmazások csoportját.

Ez a módszer hello úgy, hogy a technológia, amely része hello lekaparást oldal hozzáférési Panel-bővítménnyel, amely lehetővé teszi tooauto-felhasználónév és jelszó bemeneti mező észleléséhez, tárolja őket biztonságos helyen az adott alkalmazáspéldány esetében. Biztonságosan visszajátszásos felhasználónevek és jelszavak toothose mezők a felhasználó navigál toothat alkalmazás hello alkalmazás-hozzáférési panelre.

Ez az tooget bármilyen alkalmazás integrálása az Azure AD gyorsan elindult, és lehetővé teszi kiváló módja:

-   Integrálni **hello world bármely alkalmazás** az Azure AD-bérlőn, akkor ez a beállítás egy HTML felhasználónév és jelszó beviteli mezőt, amennyiben

-   Engedélyezése **egyszeri bejelentkezéshez a felhasználók** biztonságosan tárolja, és felhasználónevek és jelszavak hello alkalmazás visszajátszását integrált már az Azure ad-vel

-   **Az automatikus észlelés bemeneti** mezők bármely alkalmazás, és lehetővé teszik toomanually észleli azokat a mezőket hello hozzáférési Panel bővítmény, abban az esetben, ha automatikus észlelés nem találja meg őket

-   **Több bejelentkezési mező igénylő alkalmazások támogatását** csupán felhasználónévvel és jelszóval mezők toosign a igénylő alkalmazások

-   **Hello címkék testreszabása** hello felhasználónév és jelszó bemeneti mezők jelennek meg a felhasználók a hello [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) Ha a hitelesítő adatok megadása

-   Engedélyezi a **felhasználók** tooprovide saját felhasználónevek és jelszavak adatbevitel az manuálisan hello a meglévő alkalmazás fiókok számára [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Lehetővé teszi egy **hello üzleti csoport** toospecify hello felhasználónevek és jelszavak hozzárendelt tooa felhasználó hello segítségével [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) szolgáltatás

-   Lehetővé teszi egy **rendszergazda** toospecify hello felhasználónevek és jelszavak hozzárendelt tooa felhasználó hello frissítés hitelesítő adatok használatával funkciót [hozzárendelése egy felhasználói tooan alkalmazást](#_How_to_configure_1)

-   Lehetővé teszi egy **rendszergazda** toospecify megosztott hello felhasználónév vagy jelszó hello frissítés hitelesítő adatok segítségével egy csoport tagjainak által használt funkciót [egy csoport tooan alkalmazás hozzárendelése](#assign-an-application-to-a-group-directly)

Az alábbiakban ismerteti, hogyan engedélyezheti [jelszó-alapú egyszeri bejelentkezést](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) hello segítségével hozzáadása tooany alkalmazás **nem galéria alkalmazás hozzáadása** tapasztalhat.

## <a name="overview-of-steps-required"></a>A szükséges lépések – áttekintés

egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Nem galéria alkalmazás hozzáadása](#add-a-non-gallery-application)

-   [Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.](#configure-the-application-for-password-single-sign-on)

-   [Hello alkalmazás tooa felhasználó vagy csoport hozzárendelése](#assign-the-application-to-a-user-or-a-group)

    -   [Rendelje hozzá egy felhasználói tooan alkalmazás közvetlenül](#assign-a-user-to-an-application-directly)

    -   [Alkalmazáscsoport tooa közvetlenül](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a>Nem galéria alkalmazás hozzáadása

az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel

6.  Kattintson a **nem-gyűjtemény alkalmazás.**

7.  Adja meg az alkalmazás nevére hello hello **neve** szövegmező. Válassza ki **hozzáadása.**

Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.

## <a name="configure-hello-application-for-password-single-sign-on"></a>Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.

tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Jelölje be hello mód **jelszóalapú bejelentkezés.**

9.  Adja meg a hello **bejelentkezési URL-cím**. Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára. Győződjön meg arról, mezők hello bejelentkezés láthatók hello URL-címen.

10. Felhasználók hozzárendelése toohello alkalmazás.

11. Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében. Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.

## <a name="assign-a-user-tooan-application-directly"></a>Rendelje hozzá egy felhasználói tooan alkalmazás közvetlenül

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

## <a name="assign-an-application-tooa-group-directly"></a>Alkalmazáscsoport tooa közvetlenül

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

Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)
