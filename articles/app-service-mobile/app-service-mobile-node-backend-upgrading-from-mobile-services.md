---
title: a Mobile Services tooAzure App Service - Node.js aaaUpgrade
description: "Ismerje meg, hogyan tooeasily frissítése a Mobile Services alkalmazás tooan App Service Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 722cda244d4f633247827f58ea6f1397137ea600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-tooapp-service"></a>A meglévő Node.js Azure Mobile Services mobilszolgáltatás tooApp szolgáltatás frissítése
App Service Mobile egy új módon toobuild alkalmazásokat a Microsoft Azure használatával. több, lásd: toolearn [Mik azok a Mobile Apps?].

Ez a témakör ismerteti, hogyan tooupgrade egy meglévő Node.js háttérrendszer alkalmazást az Azure Mobile Services tooa új App Service Mobile Apps. Ez a frissítés végrehajtása során a meglévő Mobile Services-alkalmazás tovább toooperate.  Ha egy Node.js a háttéralkalmazás tooupgrade van szüksége, tekintse meg a túl[frissítése a .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).

Ha mobil-háttéralkalmazást frissített tooAzure App Service, rendelkezik-e hozzáférési tooall App Service-szolgáltatások és a számlázott szerint túl[App Service díjszabás], nem a Mobile Services díjszabása.

## <a name="migrate-vs-upgrade"></a>Frissítése és áttelepítése
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Javasoljuk, hogy [áttelepítés](app-service-mobile-migrating-from-mobile-services.md) áthaladás a frissítés előtt. Így adhat meg az alkalmazás mindkét verziója azonos App Service-csomag hello és nem jelent többletköltséget fel Önnek.
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>A Mobile Apps Node.js server SDK fejlesztései
Új toohello frissítése [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) számos javítást tartalmaz, például:

* Hello alapján [keretrendszer Express](http://expressjs.com/en/index.html), hello új csomópont SDK nem passzív és a tervezett tookeep mentése új csomópont verziókkal, a származnak. Testre szabhatja az Express közbenső szoftvert hello alkalmazás működését.
* Jelentős teljesítményjavulást eredményezhet toohello Mobile Services SDK képest.
* Üzemeltethető webhely és a mobil-háttéralkalmazást; Hasonlóképpen könnyen tooadd hello Azure Mobile SDK tooany meglévő express.v4 alkalmazás is.
* Platformok közötti és a helyi fejlesztési, hello Mobile Apps SDK-t is kell fejlesztett és helyileg Windows, Linux vagy OSX platformokon futtatható. Mostantól egyszerűen toouse közös csomópont fejlesztői módszerek például futó [galuskánál](https://mochajs.org/) előzetes toodeployment teszteli.

## <a name="overview"></a>Alapszintű frissítési áttekintése
a Node.js-háttéralkalmazáshoz, Azure App Service frissítése tooaid nyújtott kompatibilitási csomagot.  Frissítés után akkor egy niew helyet, amely a telepített tooa új App Service-helyen lehet.

hello Mobile Services-ügyfél SDK-k vannak **nem** hello új Mobile Apps server SDK kompatibilis. A sorrend tooprovide szolgáltatási folytonosság érdekében az alkalmazások nem módosítások tooa webhelyet jelenleg szolgál a közzétett ügyfelek kell közzétenni. Ehelyett kell hozzon létre egy új mobil-alkalmazást, amely duplikált funkcionál. Ez az alkalmazás helyezhet el hello a járulékos költségekkel nélül tooavoid ugyanaz az App Service tervezze meg.

Ezután meg kell hello alkalmazás két verziója: egy marad hello azonos szolgál hello helyettesítő a közzétett alkalmazást és az egyéb, amely ezután frissítheti és egy új ügyfél célként kiadási. Helyezze át, és tesztelheti a kódját a ütemben, de meg kell győződnie arról, hogy a bármely hibajavításokat tartalmaz, akkor alkalmazott tooboth beolvasása. Ha úgy véli, hogy a helyettesítő alkalmazásokat ügyfél kívánt számát toohello legújabb verzióra frissítette, ha felügyelni hello eredeti áttelepített alkalmazás törlése. Azt nem költségnövekedéssel bármely pénzügyi, ha ugyanaz az App Service megtervezése, mint a Mobile Apps hello tárolva.

hello teljes vázlat hello frissítési folyamat a következőképpen történik:

1. Töltse le a meglévő (áttelepített) Azure Mobile szolgáltatást.
2. Átalakítás hello projekt tooan Azure Mobile Apps hello kompatibilitási csomag használatával.
3. Javítsa ki az összes különbséget (például a hitelesítési beállításait).
4. A konvertált Azure Mobile Apps-projekt tooa telepítése új App Service.
5. Az ügyfél alkalmazás új verzióját, hogy használjon hello új Mobile Apps kiadási.
6. (Választható) Az eredeti áttelepített mobilszolgáltatás-alkalmazás törlése.

Törlés akkor fordulhat elő, ha nem lát minden forgalom az eredeti áttelepített mobil szolgáltatáson.

## <a name="install-npm-package"></a>Hello szükséges előfeltételek telepítése
[Csomópont] a helyi számítógépre kell telepíteni.  Hello kompatibilitási csomagot kell telepíteni.  Csomópont telepítése után futtathatja a következő parancsot egy új cmd vagy a PowerShell-parancssorba hello:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Az Azure Mobile Services parancsfájlok beszerzése
* Jelentkezzen be toohello [Azure Portal].
* Használatával **összes erőforrás** vagy **alkalmazásszolgáltatások**, a Mobile Services webhely található.
* Hello webhelyen, kattintson a **eszközök** -> **Kudu** -> **Ugrás** tooopen hello Kudu hely.
* Kattintson a **Debug konzol** -> **PowerShell** tooopen hello hibakereső konzolt.
* Keresse meg a túl`site/wwwroot/App_Data/config` kattintva pedig minden könyvtár
* Kattintson a hello letöltési ikon következő toohello `scripts` könyvtár.

A ZIP-formátum hello parancsfájlok letöltése.  Hozzon létre egy új könyvtárat a helyi számítógépen, és csomagolja ki a hello `scripts.ZIP` hello könyvtárban lévő fájlt.  Ezzel létrehoz egy `scripts` directory.

## <a name="scaffold-app"></a>Új Azure Mobile Apps-háttéralkalmazás hello strukturálása
Futtassa a parancsot követő hello parancsfájlok könyvtárat tartalmazó hello könyvtárból hello:

```scaffold-mobile-app scripts out```

Ezzel létrehoz egy generált Azure Mobile Apps-háttéralkalmazás a hello `out` könyvtár.  Bár nem kötelező megadni, akkor egy jó ötlet toocheck hello `out` könyvtárhoz, az Ön által választott forráskódraktárban be.

## <a name="deploy-ama-app"></a>Azure Mobile Apps-háttéralkalmazásának telepítése
A telepítés során a toodo hello következőkre lesz szüksége:

1. Hozzon létre egy új Mobile Apps hello [Azure Portal].
2. Futtassa a hello `createViews.sql` a csatlakoztatott adatbázis a parancsfájlt.
3. Hivatkozás hello adatbázist csatolt tooyour Mobile Service tooyour új App Service.
4. Egyéb erőforrások (például a Notification hubs használatával) toohello hivatkozás új App Service.
5. Hello generált kód tooyour új hely telepítése.

### <a name="create-a-new-mobile-app"></a>Új mobileszköz-alkalmazás létrehozása
1. Jelentkezzen be hello [Azure Portal].
2. Kattintson az **+ÚJ** > **Web + mobil** > **Mobile App** elemre, majd adjon meg egy nevet a mobilalkalmazás háttérrendszerének.
3. A hello **erőforráscsoport**, válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat (a neve megegyezik az alkalmazás használatával hello.)

    Kiválaszthat egy másik App Service-csomagot, vagy újat is létrehozhat. További információk az App Service szolgáltatások terveket, és hogyan toocreate egy új csomagot egy másik tarifacsomagban réteg, majd a kívánt helyre, a következő látható: [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. A hello **App Service-csomag**, hello alapértelmezett terv (a hello [Standard csomagra](https://azure.microsoft.com/pricing/details/app-service/)) van kiválasztva. Kiválaszthat egy másik App Service-csomagot is, vagy [létrehozhat egy újat](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). hello App Service-csomag beállításai határozzák meg hello [helyet, szolgáltatásokat, költségeket és számítási erőforrásokat](https://azure.microsoft.com/pricing/details/app-service/) az alkalmazáshoz társított.

    A hello terv kiválasztása után kattintson **létrehozása**. Ez a Mobile Apps-háttéralkalmazás hello hoz létre.

### <a name="run-createviewssql"></a>CreateViews.SQL futtatása
hello generált alkalmazást tartalmaz egy nevű fájlt `createViews.sql`.  Ezt a parancsfájlt a cél adatbázisban kell végrehajtani.  hello kapcsolati karakterlánc hello céladatbázis lehet lekérni a hello áttelepített mobilszolgáltatáshoz **beállítások** részen **kapcsolati karakterláncok**.  A neve `MS_TableConnectionString`.

Ez a parancsfájl az SQL Server Management Studio vagy Visual Studio belül is futtathatja.

### <a name="link-hello-database-tooyour-app-service"></a>Hivatkozás hello adatbázis tooyour App Service
Hivatkozás hello meglévő adatbázis tooyour App Service:

* A hello [Azure Portal], nyissa meg az App Service.
* Válassza ki **összes beállítás** -> **adatkapcsolatok**.
* Kattintson a **+ Hozzáadás**.
* Hello legördülő listán, válassza ki **SQL-adatbázis**
* A **SQL-adatbázis**, válassza ki a meglévő adatbázist, majd kattintson a **válasszon**.
* A **kapcsolati karakterlánc**, hello adatbázis hello felhasználónevet és jelszót adjon meg, majd kattintson a **OK**.
* A hello **adja hozzá az adatokhoz való kapcsolódást** panelen kattintson a **OK**.

hello felhasználónév és jelszó található kapcsolati karakterlánc hello hello céladatbázis az áttelepített Mobile Service megtekintésével.

### <a name="set-up-authentication"></a>Hitelesítés beállítása
Az Azure Mobile Apps tooconfigure Azure Active Directory, a Facebook, a Google, a Microsoft és a Twitterhez hitelesítési belül hello szolgáltatás lehetővé teszi.  Egyéni hitelesítési toobe fejlesztett külön-külön kell.  Tekintse meg a hello [hitelesítési fogalmakkal] dokumentáció és [hitelesítés gyorsindító] dokumentációjában talál további információt.  

## <a name="updating-clients"></a>Mobil ügyfelek frissítése
Miután egy operatív mobil-háttéralkalmazást, használhat az ügyfélalkalmazást, amely használ fel az új verziója. Mobile Apps is hello ügyfél SDK-k új verzióját tartalmazza, és a fenti a hasonló toohello kiszolgáló frissítése, szüksége lesz az összes hivatkozott toohello Mobile Services SDK előtt tooremove a Mobile Apps-verziók telepítése.

Egyik fő változás hello hello verziói között, hogy a hello konstruktorok már nem szükséges-e egy alkalmazás-kulcsot.
Most már egyszerűen adja meg a a Mobile Apps hello URL-CÍMÉT. Például hello .NET ügyfeleken hello `MobileServiceClient` konstruktor:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of hello Mobile App
        );

Áttekintheti az telepítése új SDK-k hello és hello új struktúra keresztül az alábbi hivatkozások hello használata:

* [2.2 vagy újabb Android verziója](app-service-mobile-android-how-to-use-client-library.md)
* [iOS-verzióval 3.0.0 vagy újabb verzió](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) 2.0.0 verzió vagy újabb verzió](app-service-mobile-dotnet-how-to-use-client-library.md)
* [Apache Cordova 2.0 vagy újabb verzió](app-service-mobile-cordova-how-to-use-client-library.md)

Ha az alkalmazás lehetővé teszi leküldéses értesítések használni, jegyezze fel a hello regisztráció utasításokat minden egyes platformhoz, néhány módosítás történt hiba is.

Ha hello új ügyfélverziót készen áll, próbálja ki a frissített kiszolgálón projekthez. Után ellenőrzése, hogy működik-e, az alkalmazás toocustomers egy új verziója is megjelenhetnek. Előfordulhat Ha az ügyfelek rendelkezésére állt-e egy alkalommal tooreceive ezeket a frissítéseket, törölheti hello Mobile Services, verziószám: az alkalmazás. Ezen a ponton frissített tooan App Service Mobile Apps hello legújabb Mobile Apps server SDK használatával.

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Mik azok a Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[App Service díjszabás]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[hitelesítési fogalmakkal]: ../app-service/app-service-authentication-overview.md
[hitelesítés gyorsindító]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
