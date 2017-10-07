---
title: "a helyszíni Active Directoryval, az Azure alkalmazásban aaaAuthenticate |} Microsoft Docs"
description: "További tudnivalók hello Azure App Service tooauthenticate a helyszíni Active Directory-üzleti alkalmazások különböző lehetőségek közül:"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: dde6b7e6-bf6a-4fa5-8390-3a18155d21bd
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin
ms.openlocfilehash: 65bf25aaa0447fbbea7c754db55842d57e70757e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez
Ez a cikk bemutatja, hogyan tooauthenticate a helyszíni Active Directory (AD-) t [Azure App Service](../app-service/app-service-value-prop-what-is.md). Egy Azure-alkalmazás hello felhőben található, de módon tooauthenticate a helyszíni Active Directory-felhasználók biztonságos helyen. 

## <a name="authenticate-through-azure-active-directory"></a>Hitelesítés az Azure Active Directoryn keresztül
Az Azure Active Directory-bérlő lehet könyvtár-szinkronizálása megtörtént-e egy helyszíni AD. Ez a megközelítés lehetővé teszi, hogy az AD felhasználók férhetnek hozzá az alkalmazását hello internet és a hitelesítés a helyszíni hitelesítő adatok alapján. Ezenkívül az Azure App Service biztosít egy [kulcsrakész megoldás ehhez a metódushoz](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Mindössze néhány kattintással gomb engedélyezheti a címtár-vel szinkronizált bérlők hitelesítés az Azure alkalmazás. Ez a megközelítés azzal a következő előnyöket hello:

* Nincs szükség semmilyen hitelesítési kódot az alkalmazásban. App Service tegye hello hitelesítésről és az időt a funkciók az alkalmazás segítségével.
* [Az Azure AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) lehetővé teszi, hogy elérje toodirectory az adatokat az Azure alkalmazás.
* Egyszeri Bejelentkezést biztosít túl[Azure Active Directory által támogatott összes alkalmazás](/marketplace/active-directory/), beleértve az Office 365, a Dynamics CRM Online-hoz, a Microsoft Intune és a nem Microsoft felhőalapú alkalmazások ezer. 
* Az Azure Active Directory támogatja a szerepköralapú hozzáférés-vezérlést. Hello [Authorize(Roles="X")] minta minimális módosításait tooyour kód használható.

Hogyan toowrite üzleti Azure alkalmazás, amely hitelesíti magát az Azure Active Directoryval: toosee [üzleti Azure-alkalmazás létrehozása az Azure Active Directory hitelesítési](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Egy helyszíni STS keresztül hitelesítéséhez
Ha egy helyszíni biztonságos biztonságijogkivonat-szolgáltatás (STS) például az Active Directory összevonási szolgáltatások (AD FS), használhatja a toofederate hitelesítést az Azure alkalmazás. Ezt a módszert akkor ajánlott, ha a vállalati házirend tiltja, hogy a hirdetés adatait az Azure-ban tárolja. Azonban, vegye figyelembe a következőket hello:

* STS-topológia kell központilag telepített a helyszíni, költségeket és kezelési terhelés mellett.
* Csak az AD FS-rendszergazdák konfigurálhatják [függő fél bizalmi kapcsolatok és jogcímszabályok](http://technet.microsoft.com/library/dd807108.aspx), ami korlátozhatja hello fejlesztői beállítások. A hello, ugyanakkor ez lehetséges toomanage és testre szabhatja [jogcímek](http://technet.microsoft.com/library/ee913571.aspx) alkalmazásonkénti alapon.
* Hozzáférési tooon helyszíni AD-adatok szükséges egy külön megoldásban hello vállalati tűzfalon keresztül.

Hogyan toowrite egy üzleti Azure alkalmazást, amely végzi a hitelesítést egy helyszíni STS: toosee [üzleti Azure-alkalmazás létrehozása az AD FS-hitelesítéshez](web-sites-dotnet-lob-application-adfs.md).

