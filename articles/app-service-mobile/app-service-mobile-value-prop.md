---
title: az Azure App Service Mobile Apps aaaAbout
description: "További információk a hello előnye, hogy az App Service számos lehetőséget kínál tooyour vállalati mobilalkalmazásokhoz."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: 4e96cb9d-a632-4cf6-8219-0810d8ade3f9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: ab96af11c3df196acfb9ecec1339e7f6093c7066
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started"></a>A Mobile Apps az Azure App Service-ben
Az Azure App Service egy teljes körűen felügyelt [platformszolgáltatás](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) professzionális fejlesztők számára. hello szolgáltatást számos lehetőséget kínál széles választéka képességek tooweb, mobil és integrációs célokra. 

Azure App Service Mobile Apps szolgáltatása hello biztosít, vállalati fejlesztők és a rendszer egy mobileszköz-alkalmazás kiegészítők fejlesztő platform, amely magas szinten méretezhető, valamint világszerte elérhető.

![A Mobile Apps képességeinek vizuális áttekintése](./media/app-service-mobile-value-prop/overview.png)

## <a name="why-mobile-apps"></a>A Mobile Apps szolgáltatás előnyei
Hello a Mobile Apps szolgáltatással a következőket teheti:

* **Natív és platformfüggetlen alkalmazások fejlesztése** – Akár natív iOS-, Android- vagy Windows-alkalmazást, akár platformfüggetlen Xamarin- vagy Cordova- (Phonegap-) alkalmazást készít, az App Service jól használható natív SDK-kat kínál.
* **Tooyour vállalati rendszerek**: hello a Mobile Apps szolgáltatással adhat hozzá vállalati bejelentkezés (percben), és csatlakozzon tooyour vállalat helyszíni vagy felhőalapú erőforrásaihoz.
* **Az adatszinkronizálás offline használatra kész alkalmazások készítéséhez**: mobil munkaerejét hatékonyabbá ellenőrizze épület-alkalmazásokat, amelyek offline állapotban működjenek, és használja a Mobile Apps toosync adatok hello háttérben internetkapcsolat esetén a vállalati adatforrások egyik vagy a szolgáltatott szoftverként (SaaS) API-k szoftver.
* **Leküldéses értesítések toomillions másodpercben**: azonnali leküldéses értesítésekkel bármilyen eszközön, az ügyfelek testreszabott tootheir igényeinek, és küldhet, ha hello pillanatban kódolása.

## <a name="mobile-apps-features"></a>A Mobile Apps funkciói
a következő funkciók hello fontos toocloud mobilalkalmazások fejlesztéséhez:

* **Hitelesítés és engedélyezés** – Folyamatosan bővülő identitásszolgáltatók listájából (az Azure Active Directory vállalati hitelesítési megoldását is beleértve), illetve olyan közösségi szolgáltatók közül választhat, mint a Facebook, a Google, a Twitter és a Microsoft-fiókok. A Mobile Apps minden szolgáltató számára OAuth 2.0 protokoll szerinti engedélyezést biztosít. A hello identitásszolgáltató szolgáltatói funkciót az SDK hello is integrálhatja.

    Részletesebben is tájékozódhat [hitelesítési szolgáltatások] kapcsolatban.

* **Adat-hozzáférési**: Mobile Apps mobilbarát OData v3 adatforrás tooAzure SQL-adatbázis vagy egy helyi SQL server kapcsolt biztosít. Mivel a szolgáltatás alapjául entitás-keretrendszer is használható, könnyen integrálható más NoSQL- és SQL-adatszolgáltatókkal, köztük az [Azure Table Storage] rendszerrel, a MongoDB-vel, az [Azure Cosmos DB]-vel, illetve olyan SaaS API-szolgáltatókkal, mint az Office 365 és a Salesforce.com.

* **Kapcsolat nélküli szinkronizálás**: az ügyfél-SDK révén könnyen toobuild hatékony és rugalmas mobilalkalmazások, amely egy kapcsolat nélküli adatkészletben működik. Szinkronizálhatja az adatkészlet automatikusan hello háttér-adatok, beleértve a ütközésfeloldás támogatási szolgálathoz.

  Részletesebben is tájékozódhat [az adatokkal kapcsolatos funkciókkal] kapcsolatban.

* **Leküldéses értesítések**: az ügyfél-SDK integrálása zökkenőmentesen hello regisztrációs képességek az Azure Notification Hubs, így egyszerre el lehet küldeni a leküldéses értesítések toomillions felhasználók.

  Részletesebben is tájékozódhat [leküldéses értesítési szolgáltatások] kapcsolatban.

* **Ügyfél SDK-k** – Ügyfél SDK-ink választékából mind a natív fejlesztésekhez ([iOS], [Android] és [Windows]), mind a platformfüggetlen fejlesztésekhez ([Xamarin.iOS és Xamarin.Android], [Xamarin.Forms]) és hibrid alkalmazásfejlesztésekhez ([Apache Cordova]) talál megfelelőt. Minden ügyfél SDK MIT licenccel érhető el, és nyílt forráskódú.

## <a name="azure-app-service-features"></a>Azure App Service-szolgáltatások.
a következő platform funkciói hello mobil éles üzemben működő helyeken hasznosak lehetnek:

* **Automatikus skálázás**: az App Service, gyorsan növelheti, és terjessze ki toohandle beérkező terhelés. Manuálisan hello száma és mérete található virtuális gépek kiválasztása, vagy állítsa be automatikus skálázás tooscale a mobilalkalmazás háttér terhelés alapján vagy ütemezés.

  További tudnivalók az [automatikus skálázásról].

* **Átmeneti környezet** – Az App Service egy webhely több verziójának futtatására képes, így lehetővé teszi A/B tesztek végrehajtását, éles üzemi környezetben, egy nagyobb fejlesztési és üzemeltetési terv részeként elvégzett tesztelést, illetve egy új háttéralkalmazásnak a saját helyén történő előkészítését.

  További tudnivalók az [átmeneti környezet] kapcsolatban.

* **Folyamatos üzembe helyezés**: az App Service integrálható közös ellátási lánc (SCM) rendszert, hogy automatikusan új verziójának a háttér tooa fiókirodai az SCM-rendszer küldésével.

  Részletesebben is tájékozódhat a [telepítési lehetőségek] kapcsolatban.

* **Virtuális hálózat**: App Service virtuális hálózaton, Azure ExpressRoute vagy hibrid kapcsolatokon keresztül kapcsolódhatnak tooon helyszíni erőforrások.

  Részletesebben is tájékozódhat a [hibrid kapcsolatok], a [virtuális hálózatokkal] és az [ExpressRoute]-tal kapcsolatban.

* **Elkülönített és dedikált környezetek** – Az App Service egy teljesen elkülönített és dedikált környezetben futtatható, az Azure App Service-alkalmazások biztonságos, nagy méretekben történő futtatása érdekében. Ez a környezet ideális a nagy skálázást, elkülönített vagy biztonságos hálózati hozzáférést igénylő alkalmazások és szolgáltatások számára.

  Részletesebben is tájékozódhat az [App Service-környezetek] kapcsolatban.

## <a name="next-steps"></a>Következő lépések

tooget Mobile Apps szolgáltatás használatával az Azure App Service-ben teljes hello [bevezetés] oktatóanyag. hello oktatóanyag ismerteti, és a mobileszköz vissza end hello alapjait és az ügyfél az Ön által választott. Emellett ismerteti a hitelesítés beépítésének, a kapcsolat nélküli szinkronizálásnak és a leküldéses értesítések küldésének részleteit is. Hajthatja végre hello oktatóanyag többször, egyszer minden ügyfélalkalmazásnál.

A Mobile Apps szolgáltatással kapcsolatos további információkért lásd a [tanulási térképet].
Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service].

<!-- URLs. -->
[Migrate your mobile service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[bevezetés]: app-service-mobile-ios-get-started.md
[Azure Table Storage]:../cosmos-db/table-storage-how-to-use-dotnet.md
[Azure Cosmos DB]: ../cosmos-db/documentdb-get-started.md
[hitelesítési szolgáltatások]: ./app-service-mobile-auth.md
[az adatokkal kapcsolatos funkciókkal]: ./app-service-mobile-offline-data-sync.md
[leküldéses értesítési szolgáltatások]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.iOS és Xamarin.Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[automatikus skálázásról]: ../app-service-web/web-sites-scale.md
[átmeneti környezet]: ../app-service-web/web-sites-staged-publishing.md
[telepítési lehetőségek]: ../app-service-web/web-sites-deploy.md
[hibrid kapcsolatok]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[virtuális hálózatokkal]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[App Service-környezetek]: ../app-service-web/app-service-app-service-environment-intro.md
[tanulási térképet]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/
