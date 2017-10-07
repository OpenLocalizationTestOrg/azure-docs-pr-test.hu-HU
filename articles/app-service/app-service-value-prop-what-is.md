---
title: "a webes, mobil és API-alkalmazások App Service aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan segíthet az Azure App Service web- és mobilalkalmazások fejlesztésében, telepítésében és kezelésében."
keywords: app service, azure app service, app service cost, scale, scalable, app deployment, azure app deployment, paas, platform-as-a-service, website, web site, web, azure mobile
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: 22f414c7d79092d87406a8d3538b946881fb4580
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-app-service"></a>Az Azure App Service-ről
Az *App Service* a Microsoft Azure [szolgáltatásként nyújtott platformja](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS). Web- és mobilalkalmazásokat hozhat létre bármely platformra és eszközre. Alkalmazásait integrálhatja SaaS megoldásokkal, csatlakozhat helyszíni alkalmazásokhoz és automatizálhatja üzleti folyamatait. Az alkalmazásokat az Azure teljes körűen felügyelt virtuális gépeken futtatja, az Ön választása szerint vagy közös virtuálisgép-erőforrásokkal, vagy dedikált virtuális gépekkel.

Az App Service hello webes és mobil funkciókkal, amelyeket korábban külön Azure Websites és Azure Mobile Services rendelkezik. Ezenkívül új lehetőségek is elérhetők az üzleti folyamatok automatizálásához és felhőalapú API-k üzemeltetéséhez. Egyetlen integrált szolgáltatásként az App Service lehetővé teszi egyetlen megoldás összeállítását különféle összetevőkből (webhelyekből, mobilalkalmazásokból, REST API-kból és üzleti folyamatokból).

hello alábbi 4 perces videó biztosít röviden bemutatja, hogyan kapcsolódik az App Service a tooearlier Azure ajánlatok és újdonságai azt.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>Miért előnyös az App Service használata?
Az App Service legfontosabb funkciói és képességei többek között az alábbiak:

* **Több nyelv és keretrendszer** – Az App Service kiváló támogatást nyújt az ASP.NET, Node.js, Java, PHP és Python nyelvekhez. Az App Service virtuális gépeken futtathat [Windows PowerShell és egyéb parancsfájlokat vagy végrehajtható fájlokat](../app-service-web/web-sites-create-web-jobs.md) is.
* **DevOps optimalizálás** – Beállíthat [folyamatos integrációt és üzembe helyezést](../app-service-web/app-service-continuous-deployment.md) a Visual Studio Team Services, GitHub vagy BitBucket szolgáltatásokhoz. [Teszt- és átmeneti környezetek](../app-service-web/web-sites-staged-publishing.md) segítségével küldheti ki a frissítéseket. [A/B tesztelést](../app-service-web/app-service-web-test-in-production-get-start.md) végezhet. Kezelheti az alkalmazásokat, az App Service segítségével [Azure PowerShell](/powershell/azureps-cmdlets-docs) vagy hello [platformfüggetlen parancssori felület (CLI)](../cli-install-nodejs.md).
* **Globális méret magas rendelkezésre állással** – Manuálisan vagy automatikusan is végezhet [vertikális skálázást](../app-service-web/web-sites-scale.md) és [horizontális skálázást](../monitoring-and-diagnostics/insights-how-to-scale.md). Az alkalmazások a Microsoft globális adatközpont infrastruktúrájában bárhol, meg, és az App Service hello [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) kihasználásának köszönhetően akár még a magas rendelkezésre állású.
* **Kapcsolatok tooSaaS platformokhoz és helyszíni adatok** -több mint 50 választhat [összekötők](../connectors/apis-list.md) nagyvállalati rendszerekhez (például SAP, Siebel vagy Oracle), SaaS szolgáltatásokhoz (többek között a Salesforce vagy Office 365), és internet szolgáltatások (például a Facebookhoz és a Twitterhez). Hozzáférhet helyszíni adatokhoz a [Hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és az [Azure virtuális hálózatok](../app-service-web/web-sites-integrate-with-vnet.md) segítségével.
* **Biztonság és megfelelőség** - Az App Service megfelel az [ISO, SOC és PCI szabványoknak](https://www.microsoft.com/TrustCenter/).
* **Alkalmazássablonok** -hello sablonjainak kiterjedt alkalmazássablon-listájáról válassza [Azure piactér](https://azure.microsoft.com/marketplace/) , amelyek lehetővé teszik, hogy a varázsló tooinstall népszerű nyílt forráskódú szoftverek például WordPress, a Joomla vagy a Drupal.
* **Visual Studio-integráció** -a Visual Studio dedikált eszközei egyszerűsítésére létrehozás, telepítés és hibakeresés hello munkái.

## <a name="app-types-in-app-service"></a>Alkalmazástípusok az App Service-ben
App Service számos kínál *alkalmazástípusok*, minden egyes, amelynek van tervezett toohost egy adott munkaterhelés:

* [**Web Apps**](../app-service-web/app-service-web-overview.md) - webhelyek és webalkalmazások üzemeltetéséhez
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md) Mobilalkalmazások háttérkomponenseinek üzemeltetéséhez
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) – RESTful API-k üzemeltetéséhez.
* [**Logic Apps**](../logic-apps/logic-apps-what-are-logic-apps.md) – Az üzleti folyamatok automatizálásához, rendszerek és adatok felhők közötti integrálásához – mindez kódírás nélkül.

hello word *app* itt az üzemeltető erőforrások dedikált toorunning a munkaterhelés toohello hivatkozik. Tart, "webalkalmazás" példaként, Ön valószínűleg egy webalkalmazás toothinking megszokott módon hello számítási erőforrásokat, mind az alkalmazás code tooa böngészőben együttesen funkciót. De az App Service egy *webalkalmazás* hello van a számítási erőforrásokat, amelyek az Azure biztosít az alkalmazás kódjában üzemeltetéséhez. 

Egy alkalmazás több, különféle típusú App Service-alkalmazásból is állhat. Ha például az alkalmazás webes kezelőfelületből és RESTful API-háttérrendszerből áll, akkor:

- Mind (előtér- és api) telepítése tooa egyetlen webalkalmazás  
- A kezelőfelület kódját tooa webalkalmazás és az háttérkódhoz tooan API-alkalmazás központi telepítése. 



## <a name="app-service-plans"></a>App Service-csomagok
[App Service-csomagok](azure-web-sites-web-hosting-plans-in-depth-overview.md) használt fizikai erőforrások toohost hello gyűjteményét képviseli az alkalmazások.

Az App Service-csomagok a következőket határozzák meg:

- **Régió** (USA nyugati régiója, USA keleti régiója stb.)
- **Méretezési szám** (egy, kettő, három példány, stb.)
- **Példányméret** (kicsi, közepes, nagy)
- **Termékváltozat** (Ingyenes, Közös, Alapszintű, Standard, Prémium)

Minden alkalmazás hozzárendelt tooan **App Service-csomag** határozzák meg, hogy lehetővé teszi a toosave költség esetén több alkalmazás hello erőforrások megosztása.

A **App Service-csomag** a méretezhető **szabad** és **megosztott** termékváltozatok túl**alapvető**, **szabványos**, és **Prémium** felkínálva termékváltozatok toomore erőforrások és szolgáltatások mentén hello eléréséhez módon. Az App Service-csomag túl beállítása után**alapvető** vagy újabb rendszerre is szabályozhatja hello **mérete** , a méretezés hello virtuális gépek száma.

Hello **SKU** és **méretezési** a hello App Service csomag határozza meg, hogy hello költségek és nincs hello azt az üzemeltetett alkalmazásokhoz. 

Ha  nagyobb méretezhetőségre van szüksége, az alkalmazásokat az [App Service-környezetben](../app-service-web/app-service-app-service-environment-intro.md) is futtathatja.

## <a name="pricing"></a>Díjszabás
Az App Service díjairól további információkat talál az [App Service díjszabásban](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Az App Service kipróbálása
[Hozzon létre egy mintául szolgáló web-, -mobil- vagy logikai alkalmazást](https://azure.microsoft.com/try/app-service/), és kísérletezzen vele egy órát. Nincs szükség hitelkártyára, nincs elkötelezettség, nincsenek felesleges bonyodalmak.

Vagy hozzon létre egy [ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/), és tekintse meg az első lépéseket bemutató oktatóanyagainkat:

* [Oktatóanyag: webalkalmazás létrehozása](../app-service-web/app-service-web-get-started.md)
* [Oktatóanyag: mobilalkalmazás létrehozása](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Oktatóanyag: API-alkalmazás létrehozása](../app-service-api/app-service-api-dotnet-get-started.md)
* [Oktatóanyag: logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)

