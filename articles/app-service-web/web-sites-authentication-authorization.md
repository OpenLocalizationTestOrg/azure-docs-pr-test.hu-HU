---
title: "A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez |} Microsoft Docs"
description: "További tudnivalók a különböző lehetőségek közül az üzletági alkalmazások az Azure App Service szolgáltatásban való hitelesítéshez szükséges a helyszíni Active Directory"
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
ms.openlocfilehash: a68bcd7040498515a6e35a87ee6e6940a84506d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez
Ez a cikk bemutatja, hogyan tudják hitelesíteni magukat a helyszíni Active Directory (AD) [Azure App Service](../app-service/app-service-value-prop-what-is.md). Egy Azure-alkalmazás a felhőben található, de hitelesítéséhez módjai a helyszíni Active Directory-felhasználók biztonságosan. 

## <a name="authenticate-through-azure-active-directory"></a>Hitelesítés az Azure Active Directoryn keresztül
Az Azure Active Directory-bérlő lehet könyvtár-szinkronizálása megtörtént-e egy helyszíni AD. Ez a megközelítés lehetővé teszi, hogy az AD felhasználók hozzáférni az alkalmazáshoz az internetről, és a helyszíni hitelesítő adataik használatával. Ezenkívül az Azure App Service biztosít egy [kulcsrakész megoldás ehhez a metódushoz](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Mindössze néhány kattintással gomb engedélyezheti a címtár-vel szinkronizált bérlők hitelesítés az Azure alkalmazás. Ezt a módszert használja a következő előnyökkel jár:

* Nincs szükség semmilyen hitelesítési kódot az alkalmazásban. A hitelesítést, és az időt a funkciók az alkalmazás az App Service segítségével.
* [Az Azure AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) lehetővé teszi a hozzáférést a címtáradatok emelheti Azure alkalmazását.
* Az egyszeri Bejelentkezést biztosít [Azure Active Directory által támogatott összes alkalmazás](/marketplace/active-directory/), beleértve az Office 365, a Dynamics CRM Online-hoz, a Microsoft Intune és a nem Microsoft felhőalapú alkalmazások ezer. 
* Az Azure Active Directory támogatja a szerepköralapú hozzáférés-vezérlést. Segítségével az [Authorize(Roles="X")] minta legfeljebb minimális változtatásokra a kódot.

Üzleti az Azure alkalmazás, amely hitelesíti magát az Azure Active Directoryval írásával kapcsolatos információk: [üzleti Azure-alkalmazás létrehozása az Azure Active Directory hitelesítési](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Egy helyszíni STS keresztül hitelesítéséhez
Ha egy helyszíni biztonságos biztonságijogkivonat-szolgáltatás (STS) például az Active Directory összevonási szolgáltatások (AD FS), összevonást végezni a hitelesítés az Azure alkalmazás használhatja, amely. Ezt a módszert akkor ajánlott, ha a vállalati házirend tiltja, hogy a hirdetés adatait az Azure-ban tárolja. Azonban, vegye figyelembe a következőket:

* STS-topológia kell központilag telepített a helyszíni, költségeket és kezelési terhelés mellett.
* Csak az AD FS-rendszergazdák konfigurálhatják [függő fél bizalmi kapcsolatok és jogcímszabályok](http://technet.microsoft.com/library/dd807108.aspx), ami korlátozhatja a fejlesztői beállításokat. Másrészről, kezelése és testreszabása lehetőség [jogcímek](http://technet.microsoft.com/library/ee913571.aspx) alkalmazásonkénti alapon.
* Hozzáférés a helyszíni AD-adatok előírja, hogy egy külön megoldásban a vállalati tűzfalon keresztül.

Egy helyszíni STS az egy üzleti Azure alkalmazást, amely hitelesíti írásával kapcsolatos információk: [üzleti Azure-alkalmazás létrehozása az AD FS-hitelesítéshez](web-sites-dotnet-lob-application-adfs.md).

