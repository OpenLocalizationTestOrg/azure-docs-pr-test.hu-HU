---
title: "aaaHow Azure App Service szolgáltatásban működik"
description: "Ismerje meg az App Service működését"
keywords: "app service, azure app service, méret, méretezhető, app service-csomag, app service ára"
services: app-service
documentationcenter: 
author: yochay
manager: erikre
editor: 
ms.assetid: ae74fc32-969e-4580-8d61-02c922f1f184
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/23/2017
ms.author: yochayk
ms.openlocfilehash: b20733ec8844773d063e2b6918605c4a48db1f5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-app-service-works"></a>Az App Service működése
Az Azure App Service egy felhőalapú szolgáltatás, amely tervezett toosolve hello gyakorlati problémákat, mérnökök szembesülhetnek ma.
App Service fókusz szolgáltatásában hatékony fejlesztést tesz lehetővé a hello veszélyeztetése nélkül toodeliver alkalmazások felhőhöz méretezett kell. 

App Service a hello szolgáltatásokat és vállalati üzleti alkalmazások létrehozásához szükséges keretrendszerek is biztosít. App Service lehetővé teszi a legnépszerűbb fejlesztési nyelveket, többek között a Java, PHP, Node.js, Python és a Microsoft .NET-nyelveket hello alkalmazások fejlesztéséhez. Az App Service-szel a következőket teheti:

* Kiválóan méretezhető webalkalmazások létrehozása.
* A Mobile Apps könnyen kezelhető mobileszközös lehetőségek (adatkezelési háttéralkalmazások, felhasználói hitelesítés és leküldéses értesítések) egész tárházát biztosítja.
* API-k végrehajtása, üzembe helyezése és közzététele az API Apps szolgáltatással.
* Üzleti alkalmazások összekapcsolása munkafolyamatokban és az adatok átalakítása a Logic Apps segítségével.

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

Mindegyik alkalmazástípus alapja hello méretezhető és rugalmas webalkalmazások platformon, amely lehetővé teszi a fejlesztők toohave alkalmazás tervezési tooapp karbantartási tapasztalata az optimalizált teljes életciklusát. hello életciklus-kezelési lehetőségek hello következő engedélyezése:

* **Alkalmazások gyors létrehozása**. Új, vagy válasszon egy hello Azure piactér működési támogatási rendszer (OSS) csomagot.
* **Folyamatos üzembe helyezés**. Népszerű verziókövetési megoldásokból (például a TFS-ből, a GitHubról és a BitBucketből) származó új kód automatikus üzembe helyezése és online társzolgáltatásokban (például a OneDrive-ban vagy a DropBoxban) tárolt tartalom szinkronizálása.
* **Tesztelés élesben**. Éles üzem előtti környezetek létrehozása és problémamentesen hello mennyisége toothem forgalom kezelése. Szükség esetén hello felhőben hibakeresési, és állítsa vissza, ha problémák észlelése.
* **Aszinkron és kötegelt feladatok futtatása**. Kód futtatása háttérfolyamatként, vagy a kód aktiválása meghatározott események bekövetkezésekor (például az Azure Storage-üzenetsorokra érkező üzenetek esetén) és ütemezett időpontokban (CRON).
* **Méretezési hello app**. A vízszintes és függőleges adatforgalom és erőforrás-használat alapján szolgáltatás használata sok beállítások tooautomatically méretezési egyikét. Igazított privát környezet, dedikált tooyour alkalmazások közül.   
* **Karbantartása hello app**. Számos hello hibakeresés használja, és diagnosztikai funkciók toostay problémák és tooefficiently előre feloldása vagy a valós időben (például az automatikus javítás és éles hibakeresési), vagy után hello tény elemzésével naplók és memóriaképek.

Egy teljes App Service tehát a kódra a fejlesztők toofocus engedélyezése, és gyorsan állapotot elérni a stabil és kiválóan méretezhető éles. Hello API-alkalmazások és a Logic Apps funkcióinak a fejlesztők létrehozhatják a valós vállalati alkalmazások, amelyek az üzleti megoldások és a helyszíni toocloud integrációs közötti akadályok hídjaként. 

## <a name="videos"></a>Videók
* [Azure App Service-architektúra](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a>Következő lépések

További tudnivalók az App Service a következő témakörök hello egyikében:

* [Az Azure App Service-ről](app-service-value-prop-what-is.md)
  * [Webalkalmazás](../app-service-web/app-service-web-overview.md)
  * [Mobilalkalmazás](../app-service-mobile/app-service-mobile-value-prop.md)
  * [API-alkalmazás](../app-service-api/app-service-api-apps-why-best-platform.md)
* [Azure App Service-architektúra (bemutató)](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [Az Azure App Service, a Cloud Services és a Virtual Machines összehasonlítása](../app-service-web/choose-web-site-cloud-service-vm.md)
* [Az App Service-csomagok ismertetése](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [Bevezetés tooApp Service-környezet](../app-service-web/app-service-app-service-environment-intro.md)
  * [Gyakorlat: App Service Environment létrehozása](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Azure App Service fejlesztői vermek támogatása](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)



