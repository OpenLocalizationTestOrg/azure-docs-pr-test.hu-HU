---
title: "aaaWeb áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogy az Azure App Service segítségével miként fejleszthet és üzemeltethet webalkalmazásokat."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ce27519bddd62a7fca6ba1fb23c763d0fc378c2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="web-apps-overview"></a>A webalkalmazások áttekintése
Az *App Service Web Apps* egy teljes körűen felügyelt számítógépes platform, amely webhelyek és webalkalmazások üzemeltetéséhez van optimalizálva. Ez [platform,--szolgáltatás](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) elérhető Microsoft Azure lehetővé teszi, hogy az üzleti logikát összpontosítson, amíg az Azure intézkedik hello infrastruktúra toorun és az alkalmazások skálázása.

a következő 5 perces videót hello Azure App Service Web Apps vezet be.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

## <a name="what-is-a-web-app-in-app-service"></a>Mi az az App Service-webalkalmazás?
Az App Service-ben egy *webalkalmazás* hello van a számítási erőforrásokat, amelyek az Azure biztosít egy webhelyre vagy webalkalmazásra alkalmazást.  

megosztott vagy dedikált virtuális gépek (VM), attól függően, hogy a választott tarifacsomag hello hello számítási erőforrások lehetnek. Az alkalmazás kódja egy felügyelt virtuális gépen fut, amely el van különítve az egyéb ügyfelektől.

A kód bármilyen nyelven vagy keretrendszerben lehet, amelyet támogat az [Azure App Service](../app-service/app-service-value-prop-what-is.md). Ilyen például az ASP.NET, a Node.js, a Java, a PHP vagy a Python. Olyan parancsfájlokat is futtathat, amelyek a [PowerShellt és más parancsfájl-készítő nyelveket](web-sites-create-web-jobs.md#acceptablefiles) használnak egy webalkalmazásban.

Példák általános alkalmazás-forgatókönyvekre, hogy használható Web Apps, lásd: [webalkalmazások forgatókönyvei](https://azure.microsoft.com/documentation/scenarios/web-app/) és hello **forgatókönyvek és javaslatok** szakasza [Azure App Service, a virtuális Gépek, a Service Fabric és a Cloud Services összehasonlítása](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Miért érdemes használni a webalkalmazásokat?
Az alábbiakban néhány kulcsszolgáltatása App Service vonatkozó tooWeb alkalmazások:

* **Több nyelv és keretrendszer** – Az App Service kiváló támogatást nyújt az ASP.NET, Node.js, Java, PHP és Python nyelvekhez. Futtathat [PowerShell és egyéb parancsfájlokat vagy futtatható fájlokat](web-sites-create-web-jobs.md) is az App Service virtuális gépeken.
* **DevOps optimalizálás** – Beállíthat [folyamatos integrációt és üzembe helyezést](app-service-continuous-deployment.md) a Visual Studio Team Services, GitHub vagy BitBucket szolgáltatásokhoz. [Teszt- és átmeneti környezetek](web-sites-staged-publishing.md) segítségével küldheti ki a frissítéseket. [A/B tesztelést](app-service-web-test-in-production-get-start.md) végezhet. Kezelheti az alkalmazásokat, az App Service segítségével [Azure PowerShell](/powershell/azureps-cmdlets-docs) vagy hello [platformfüggetlen parancssori felület (CLI)](../cli-install-nodejs.md).
* **Globális méret magas rendelkezésre állással** – Manuálisan vagy automatikusan is végezhet [vertikális skálázást](web-sites-scale.md) és [horizontális skálázást](../monitoring-and-diagnostics/insights-how-to-scale.md). Az alkalmazások a Microsoft globális adatközpont infrastruktúrájában bárhol, meg, és az App Service hello [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) kihasználásának köszönhetően akár még a magas rendelkezésre állású.
* **Kapcsolatok tooSaaS platformokhoz és helyszíni adatok** -több mint 50 választhat [összekötők](../connectors/apis-list.md) nagyvállalati rendszerekhez (például SAP, Siebel vagy Oracle), SaaS szolgáltatásokhoz (többek között a Salesforce vagy Office 365), és internet szolgáltatások (például a Facebookhoz és a Twitterhez). Hozzáférhet helyszíni adatokhoz a [Hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és az [Azure virtuális hálózatok](web-sites-integrate-with-vnet.md) segítségével.
* **Biztonság és megfelelőség** - Az App Service megfelel az [ISO, SOC és PCI szabványoknak](https://www.microsoft.com/TrustCenter/).
* **Alkalmazássablonok** -alkalmazás sablonjainak hello kiterjedt alkalmazássablon-listájáról válassza [Azure piactér](https://azure.microsoft.com/marketplace/) , amelyek lehetővé teszik, hogy a varázsló tooinstall népszerű nyílt forráskódú szoftverek például WordPress, a Joomla vagy a Drupal.
* **Visual Studio-integráció** -a Visual Studio dedikált eszközei egyszerűsítésére létrehozás, telepítés és hibakeresés hello munkái.

Ráadásul egy webalkalmazás kihasználhatja az [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) által kínált szolgáltatásokat (mint a CORS-támogatás), valamint a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) által kínált szolgáltatásokat (mint a leküldéses értesítések). További információk az App Service alkalmazástípusairól: [Az Azure App Service áttekintése](../app-service/app-service-value-prop-what-is.md).

Az App Service-webalkalmazásokon kívül az Azure más szolgáltatásokat is kínál, amelyek használhatók webhelyek és webalkalmazások üzemeltetésére. A legtöbb esetben a Web Apps hello legjobb választás.  Mikroszolgáltatási architektúra esetében érdemes lehet [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric), és ha a kódot futtató virtuális gépek hello teljesebb körű vezérlése van szüksége, fontolja meg a [Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/). További információ az Azure-szolgáltatások közötti toochoose lásd [Azure App Service, a virtuális gépek, a Service Fabric és a Cloud Services összehasonlítása](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Bevezetés
minta kód tooa új webalkalmazást az App Service-ben telepítése által elindított tooget kövesse a legördülő listában a következő hello hello oktatóprogramok valamelyikét. Szüksége lesz egy ingyenes Azure-fiókra.

> [!div class="op_single_selector"]
> * [Telepíti az első ASP.NET web app tooAzure 5 perc](app-service-web-get-started-dotnet.md)
> * [Telepíti az első PHP webes alkalmazások tooAzure 5 perc](app-service-web-get-started-php.md)
> * [Telepíti az első Node.js web app tooAzure 5 perc](app-service-web-get-started-nodejs.md)
> * [Telepíti az első Java web app tooAzure 5 perc](app-service-web-get-started-java.md)
> * [Telepíti az első Python webes alkalmazás tooAzure 5 perc](app-service-web-get-started-python.md)
> * [Telepíti az első HTML hely tooAzure 5 perc](app-service-web-get-started-html.md)
> 
> 

> [!NOTE]
> Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges. Hozzon létre egy alapszintű alkalmazást, és nincs szükség bankkártyára, nem jár kötelezettségekkel tooan óra--be azt a lejátszásához.
> 
> 
