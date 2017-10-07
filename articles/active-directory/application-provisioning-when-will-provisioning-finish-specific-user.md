---
title: "Amikor egy adott felhasználó lesz-e az alkalmazás képes tooaccess aaaFind |} Microsoft Docs"
description: "Ha különösen fontos felhasználói kell tudni tooaccess alkalmazás kimenő toofind konfigurációjától felhasználói történő üzembe helyezéséhez az Azure AD"
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
ms.openlocfilehash: bb9520499dcc8bbbe6fae05c5238c8852815ea0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-tooaccess-an-application"></a>Egy adott felhasználó mindig tudja tooaccess alkalmazás megállapítása
Ha automatikus felhasználólétesítés alkalmazással, az Azure AD létesítése és a frissítés felhasználói fiókokat az alkalmazáson belüli alapján automatikusan többek között a [felhasználók és csoportok hozzárendelése](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) időközönként rendszeresen ütemezett, általában 10 percenként.

## <a name="how-long-does-it-take"></a>Mennyi időt vesz igénybe?

az egy adott felhasználó toobe kiépített szükséges hello időtartamát elsősorban attól függ-e már megtörtént egy kezdeti "teljes" szinkronizálást.

első szinkronizálás az Azure AD között hello és az alkalmazások is igénybe vehet 20 perc tooseveral óra, hello Azure Active directory és a felhasználók kialakítási hatókörében hello száma hello méretétől függően. 

Ezt követő szinkronizálások után hello kezdeti szinkronizálás lehet gyorsabb (pl. 10 percen belül), mivel hello szolgáltatás kiépítését, amelyek megfelelnek a hello állapot mindkét rendszer hello a kezdeti szinkronizálás ezt követő szinkronizálások teljesítményének javítása után vízjelek tárolja.

## <a name="how-toocheck-hello-status-of-a-user"></a>Hogyan toocheck hello egy felhasználó állapota

toosee hello kiépítési állapot a kiválasztott felhasználó, tekintse át a hello naplók az Azure AD-ben.

kiépítés naplók hello hello hello Azure portálon érhetők el **Azure Active Directory &gt; vállalati alkalmazások &gt; \[alkalmazásnév\] &gt; vizsgálati naplókban**fülre. Szűrő hello bejelentkezik hello **fiók** kategória tooonly, tekintse meg az adott alkalmazáshoz események kiépítés hello. Felhasználók hello "egyező ID" attribútum-leképezésekhez hello számukra konfigurált alapján is kereshet. 

Például, ha hello "egyszerű felhasználónév" vagy "e-mail cím" hello Azure AD oldalon attribútum megfelelő hello, konfigurálta, és nem kiépítés hello felhasználó érték "audrey@contoso.com", majd keresési hello auditnaplókat a"audrey@contoso.com", és tekintse át a majd az eredményt adott vissza.

kiépítés naplózási hello naplózza az összes szolgáltatás, kiépítését hello által végrehajtott műveletek hello rekord többek között:

* Azure ad-val történő üzembe helyezéséhez hatókörébe hozzárendelt felhasználók lekérdezése
* Azoknak a felhasználóknak hello megléte hello cél alkalmazás lekérdezése
* Hello felhasználói objektum hello rendszer közötti összehasonlítása
* Hozzáadása, frissítése vagy hello összehasonlítás alapján hello célrendszerben hello felhasználói fiók letiltása

## <a name="next-steps"></a>Következő lépések
[Felhasználói kiépítés és megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval automatizálásához](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)".
