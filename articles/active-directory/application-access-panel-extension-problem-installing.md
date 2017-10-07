---
title: "hello alkalmazás hozzáférési panel bővítmény telepítése aaaProblem |} Microsoft Docs"
description: "Hogyan toofix előforduló hibákat észlelt, amikor hello hozzáférési panel bővítmény telepítése"
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
ms.reviewer: japere
ms.openlocfilehash: 5f750d12c5f9b405ec4f81596d5cc5e0a48f9a62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-access-panel-browser-extension"></a>A probléma hello alkalmazás hozzáférési panel bővítmény telepítése

Hozzáférési Panel hello egy webes portál, mely lehetővé teszi, hogy egy felhasználó, aki rendelkezik a munkahelyi vagy iskolai fiókot az Azure Active Directory (Azure AD) tooview, és indítsa el felhőalapú alkalmazások, hogy az Azure AD hello rendszergazda engedélyezte őket a hozzáférést. Egy felhasználó, aki rendelkezik az Azure AD-verziók is használhatja, önkiszolgáló csoportkezelési és a hozzáférési Panel hello keresztül kezelési képességeit. Hozzáférési Panel hello elkülönül hello Azure-portálon, és nem igényli a felhasználók toohave Azure-előfizetéssel.

toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében. A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>A hozzáférési Panel hello értekezlet böngészőre vonatkozó követelményei

hello hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte. toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében. A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.

Az egyszeri bejelentkezés jelszóalapú hello végfelhasználó böngészőkkel lehet:

-   Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb

-   Peremhálózati Windows 10 évforduló Edition vagy újabb 

-   Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb

-   Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hogyan tooinstall hello hozzáférési Panel bővítmény

tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.

2.  Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.

3.  Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.

4.  A böngésző alapján kell irányított toohello letöltési hivatkozását. **Adja hozzá** hello bővítmény tooyour böngésző.

5.  Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.

6.  Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.

7.  Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz

Az alábbi hello közvetlen hivatkozások Chrome és a peremhálózati is letöltheti hello bővítmény:

-   [Chrome hozzáférési Panel bővítmény](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Peremhálózati hozzáférési Panel bővítmény](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>A csoportházirend beállítása az Internet Explorerben

Beállíthatja a csoportházirend, amelyek lehetővé teszik tooremotely telepítés hello hozzáférési Panel bővítményt az Internet Explorer a felhasználók gépeken.

hello Előfeltételek a következők:

-   Ezzel beállította [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), és a felhasználók gépek tooyour tartományhoz csatlakozott.

-   Hello "Beállítások módosítása" engedéllyel tooedit hello csoportházirend-objektumot (GPO) kell rendelkeznie. Alapértelmezés szerint a következő biztonsági csoportokat hello tagjai rendelkezik ilyen engedéllyel: tartományi rendszergazdák, a vállalati rendszergazdák és a Csoportházirend-létrehozó tulajdonosok. [További információk](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Hello az oktatóanyagot követve [hogyan tooDeploy hello csoportházirend használatával az Internet Explorer hozzáférési Panel bővítmény](active-directory-saas-ie-group-policy.md) hogyan tooconfigure hello csoportházirend, majd központilag telepítenie toousers részletes információkra van szüksége.

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a>Az Internet Explorerben a hozzáférési Panel hello hibaelhárítása

Hajtsa végre a hello [kapcsolatos problémák elhárítása hello hozzáférési Panel bővítményét az Internet Explorer](active-directory-saas-ie-troubleshooting.md) útmutatót a hozzáféréshez olyan diagnosztikai eszköz és részletes utasításokat az hello bővítmény konfigurálása az Internet Explorer.

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a>Ha ezek a hibaelhárítási lépéseket nem oldható meg hello probléma

Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:

-   Megfelelési hiba azonosítója

-   Egyszerű felhasználónév (felhasználó e-mail címe)

-   A TenantID

-   Böngésző típusa

-   Időzóna és idő/időkeretre során hiba történik.

-   Fiddler nyomkövetések

## <a name="next-steps"></a>Következő lépések
[Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
