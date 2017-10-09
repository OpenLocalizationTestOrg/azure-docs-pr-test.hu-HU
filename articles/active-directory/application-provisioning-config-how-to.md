---
title: "aaaHow tooconfigure felhasználók átadásához tooan az Azure AD-gyűjtemény alkalmazás |} Microsoft Docs"
description: "Gazdag felhasználói fiók kiépítésének és megszüntetésének biztosítása már szerepel az Azure AD Application Gallery hello tooapplications gyors konfigurálásához"
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
ms.openlocfilehash: 2c28e59a3ac8f221ed93b2f6b0b1221f7604af23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-user-provisioning-tooan-azure-ad-gallery-application"></a>Hogyan tooconfigure felhasználói tooan az Azure AD-gyűjtemény alkalmazás kiépítése

*Felhasználói fiók kiépítése* hello act létrehozása, frissítése, illetve letiltása a felhasználói fiókot rögzíti az alkalmazás helyi felhasználói profil tárolójában van. A legtöbb felhő- és SaaS-alkalmazásokhoz tárolása hello felhasználók szerepköröket és engedélyeket a saját helyi felhasználói profil tároló, valamint ilyen felhasználói rekordban a helyi tárolóban levő jelenléte *szükséges* az egyszeri bejelentkezés és a hozzáférés toowork.

Hello Azure-portálon, a hello **kiépítési** egy vállalati alkalmazást jeleníti meg, milyen üzembe helyezési mód támogatja az alkalmazás hello bal oldali navigációs panelen lapján. Ez a két értékek egyike lehet:

## <a name="configuring-an-application-for-manual-provisioning"></a>Alkalmazások konfigurálása a manuális üzembe helyezéséhez

*Manuális* kiépítés azt jelenti, hogy felhasználói fiókokat kell létrehozni az alkalmazás által biztosított hello módszerek segítségével manuálisan. Ez azt jelentheti, hogy az alkalmazáshoz egy felügyeleti portálra való bejelentkezéskor, és a webes felhasználói felülete segítségével a felhasználók hozzáadásával. Vagy az sikerült feltölteni egy táblázatot a felhasználói fiók részletes, egy adott alkalmazás által biztosított mechanizmus használatával. Dokumentációjában talál hello megadott szerint hello alkalmazást, illetve kapcsolattartási hello alkalmazást fejlesztői toodetermine Nyugat-afrikai mechanizmusok érhetők el.

Ha manuális hello egyetlen mód jelenik meg egy adott alkalmazáshoz, az azt jelenti, hogy nincs automatikus az Azure AD összekötő kiépítése még létrehozva hello alkalmazás. Vagy az azt jelenti, hogy hello alkalmazás nem nem támogatási hello működéséhez szükséges felhasználói felügyeleti API mely toobuild követően az automatikus létesítési csatlakozó.

Ha szeretné, hogy egy adott alkalmazás automatikus kiépítés toorequest támogatása, a kérelem kitöltheti <http://aka.ms/aadapprequest>.

## <a name="configuring-an-application-for-automatic-provisioning"></a>Alkalmazások konfigurálása az Automatikus kiépítés

*Automatikus* azt jelenti, hogy egy összekötő kiépítése az Azure AD identitáskezelési ehhez az alkalmazáshoz. További információ a hello kiépítése szolgáltatáshoz, és hogyan működik, az Azure AD: [Felhasználókiépítés és -megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).

További információ a hogyan tooprovision bizonyos felhasználók és csoportok tooan alkalmazás: [kezelése a felhasználói fiók kiépítése vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).

hello szükséges tooenable tényleges lépéseket, és állítsa be automatikus kiépítés hello alkalmazástól függően változhat.

>[!NOTE]
>Először meg kell válaszolnia hello beállítása adott oktatóanyag toosetting be az alkalmazás telepítése, és ezeket a lépéseket tooconfigure következő hello alkalmazás, mind az Azure AD toocreate hello létesítési kapcsolat keresése. 
>
>

Alkalmazás oktatóprogramok találhatók [oktatóanyagok listáját hogyan tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

Ha kiépítés beállítása tooreview egy fontos tooconsider hello attribútum-leképezésekhez és munkafolyamatok, amelyek meghatározzák, milyen felhasználói (vagy a csoport) tulajdonságok folyamata az Azure AD toohello alkalmazásból és konfigurálása. Ez magában foglalja, hello "egyező tulajdonság" beállítása használt toouniquely kell azonosítani és felel meg a felhasználók/csoportok hello két rendszerek között. További információ a fontos folyamatban.

## <a name="next-steps"></a>Következő lépések
[Attribútum-leképezésekhez kiépítés az SaaS-alkalmazásokhoz az Azure Active Directory felhasználói testreszabása](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

