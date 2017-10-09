---
title: "az alkalmazások listáját a aaaUnexpected alkalmazás |} Microsoft Docs"
description: "Hogyan toosee a bérlő lévő összes alkalmazásnál és megérteni, hogyan alkalmazások jelennek meg az összes alkalmazások listáját a vállalati alkalmazások"
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
ms.openlocfilehash: 2f974046b655bc36a05f002c56511a8a988cd01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-application-in-my-applications-list"></a>Az alkalmazások listáját a váratlan alkalmazás

Ez a cikk segítséget toounderstand hogyan az alkalmazások megjelennek a **összes alkalmazás** listában az **vállalati alkalmazások**. 

## <a name="how-toosee-all-applications-in-your-tenant"></a>Hogyan toosee összes alkalmazás-bérlőben

toosee összes hello alkalmazások az Ön bérlőjében, toouse hello kell **szűrő** tooshow szabályozása **összes alkalmazás** alatt hello **összes alkalmazás** listája. toodo ezt, hajtsa végre hello az alábbi lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

6.  Kattintson a hello használata hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját**.

7.  A hello **szűrő** panelen, a set hello **megjelenítése** beállítás túl**összes alkalmazást.**

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Miért nem egy adott alkalmazás jelenik meg az összes alkalmazások listáját?

Túl szűrve**összes alkalmazás**, hello **összes alkalmazás** **lista** minden egyszerű szolgáltatásnév objektum szerepel a bérlő. Egyszerű objektumok ebben a listában, a különböző módon jelenhetnek meg:

1.  Amikor bármely alkalmazás hello alkalmazás gyűjteményből, beleértve:

   1. **Azure AD-gyűjtemény alkalmazások** –, amelyet az egyszeri bejelentkezés az Azure ad-vel előre integrált alkalmazás.

   2. **Alkalmazás Proxy alkalmazások** – a helyszíni környezetben futó tooprovide biztonságos egyszeri bejelentkezést tooexternally kívánt alkalmazáshoz.

   3. **Egyéni által fejlesztett alkalmazások** – olyan alkalmazás, amely a szervezet által toodevelop a hello Azure AD alkalmazás-fejlesztő Platform, de, amely nem még létezik.

   4. **Nem-gyűjtemény alkalmazások** – kapcsolja a saját alkalmazásai! Bármely kívánt webes hivatkozást, vagy egy felhasználónevet és jelszót a mezőben, amely alkalmazás SAML vagy az OpenID Connect protokollt támogatja, vagy az egyszeri bejelentkezés az Azure ad-val toointegrate verzióváltáshoz SCIM támogatja.

2.  Ha regisztrálni szeretne, vagy egy 3 történő bejelentkezés<sup>távoli asztali</sup> gyártótól származó alkalmazás Azure Active Directory integrált része. Egy példa erre, [Smartsheet](https://app.smartsheet.com/b/home) vagy [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).

3.  Ha regisztrálni szeretne, vagy a licenc tooa felhasználó vagy csoport tooa első gyártótól származó alkalmazás hozzáadása, például [Microsoft Office 365](http://products.office.com/).

4.  Amikor felvesz egy új alkalmazás regisztrációja hozzon létre egy egyéni által fejlesztett alkalmazás használatával hello [alkalmazás beállításjegyzék](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).

5.  Amikor felvesz egy új alkalmazás regisztrációja hozzon létre egy egyéni által fejlesztett alkalmazás használatával hello [V2.0 alkalmazásregisztrációs portálra](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).

6.  Ha egy alkalmazás kidolgozása Visual Studio használatával [ASP.net hitelesítési módszerek](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) vagy [kapcsolódó szolgáltatások](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).

7.  Amikor egy szolgáltatás egyszerű objektumot hello segítségével hoz létre [Azure AD PowerShell modult](/powershell/azure/install-adv2?view=azureadps-2.0).

8.  Ha Ön [tooan alkalmazás hozzájárulás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) egy a bérlői rendszergazda toouse adatként.

9.  Ha egy [felhasználói beleegyezik tooan alkalmazás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse adatok az Ön bérelt szolgáltatásának.

10. Ha engedélyezi az egyes szolgáltatások tárolt adatok az Ön bérelt szolgáltatásának. Egy példa erre, a jelszó alaphelyzetbe állítása, amely van modellezve a szolgáltatás egyszerű toostore, a jelszó-visszaállítási házirend biztonságos helyen.

tooget hogyan az alkalmazások kerülnek tooyour directory, a további részletekért olvassa el [útmutató és érvek alkalmazások felvétele tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a>Egy adott felhasználó vagy csoport hozzárendelése tooan alkalmazás kívánt tooremove

egy felhasználó vagy csoport hozzárendelése tooan alkalmazást, tooremove lépésekkel hello hello felsorolt [egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) cikk.

## <a name="i-want-toodisable-all-access-tooan-application-for-every-user"></a>Minden felhasználó az összes access tooan alkalmazás kívánt toodisable

toodisable minden felhasználói bejelentkezések tooan alkalmazást, kövesse hello lépéseket hello [tiltsa le a felhasználói bejelentkezések Azure Active Directory vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) cikk.

## <a name="i-want-toodelete-an-application-entirely"></a>Egy alkalmazás toodelete teljesen kívánt

túl**alkalmazás törlése**, kövesse az alábbi hello utasításokat:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt toodelete hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **törlése** hello felső alkalmazás ikonja **áttekintése** panelen.

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a>Az összes jövőbeni felhasználói hozzájárulás műveletek tooany alkalmazás kívánt toodisable

Felhasználói hozzájárulás letiltása, az a teljes címtár megakadályozhatja a végfelhasználók számára a küldőnek tooany alkalmazásból. A rendszergazdák továbbra is a felhasználó behalves is hozzájárulás. További információ az alkalmazás toolearn hozzájárulás, és miért lehet, vagy előfordulhat, hogy nem kívánja toodo, olvassa el a következőt [ismertetése felhasználói és rendszergazdai hozzájárulási](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

túl**tiltsa le a teljes címtár minden jövőbeni felhasználói hozzájárulás műveletei**, kövesse az alábbi hello utasításokat:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **felhasználói beállítások**.

6.  Tiltsa le az összes jövőbeni felhasználói hozzájárulás műveletek hello beállítása az **felhasználók engedélyezhetik alkalmazások tooaccess adataikat** túl váltása**nem** hello kattintson **mentése** gombra.

## <a name="next-steps"></a>Következő lépések
[Alkalmazások kezelése az Azure Active Directoryban](active-directory-enable-sso-scenario.md)
