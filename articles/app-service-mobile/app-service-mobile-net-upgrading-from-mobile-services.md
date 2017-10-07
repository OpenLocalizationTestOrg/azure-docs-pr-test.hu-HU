---
title: az App Service Mobile Services tooAzure aaaUpgrade
description: "Ismerje meg, hogyan tooeasily frissítése a Mobile Services alkalmazás tooan App Service Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 9c0ac353-afb6-462b-ab94-d91b8247322f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2b75a1b824e8ef0d36c9053f2f19af051479f928
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-net-azure-mobile-service-tooapp-service"></a>A meglévő .NET Azure Mobile Service tooApp szolgáltatás frissítése
App Service Mobile egy új módon toobuild alkalmazásokat a Microsoft Azure használatával. több, lásd: toolearn [Mik azok a Mobile Apps?].

Ez a témakör ismerteti, hogyan tooupgrade egy meglévő .NET háttérrendszer alkalmazást az Azure Mobile Services tooa új App Service Mobile Apps. Ez a frissítés végrehajtása során a meglévő Mobile Services-alkalmazás tovább toooperate.   Ha egy Node.js a háttéralkalmazás tooupgrade van szüksége, tekintse meg a túl[a Node.js Mobile Services frissítése](app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Ha mobil-háttéralkalmazást frissített tooAzure App Service, rendelkezik-e hozzáférési tooall App Service-szolgáltatások és a számlázott szerint túl[App Service díjszabás], nem a Mobile Services díjszabása.

## <a name="migrate-vs-upgrade"></a>Frissítése és áttelepítése
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Javasoljuk, hogy [áttelepítés](app-service-mobile-migrating-from-mobile-services.md) áthaladás a frissítés előtt. Így adhat meg az alkalmazás mindkét verziója azonos App Service-csomag hello és nem jelent többletköltséget fel Önnek.
>
>

### <a name="improvements-in-mobile-apps-net-server-sdk"></a>A Mobile Apps .NET server SDK fejlesztései
Új toohello frissítése [Mobile Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) hello a következő előnyöket biztosítja:

* Nagyobb rugalmasság a NuGet-függőségek. hello üzemeltetési környezet már nem nyújt a saját NuGet-csomagok verzióit, így alternatív kompatibilis verziója is használhatja. Azonban ha vannak új kritikus bugfixes vagy biztonsági frissítések toohello Mobile Server SDK vagy függőségek, kézzel kell frissítenie a szolgáltatáshoz.
* Nagyobb rugalmasságot biztosítanak a mobil SDK hello. Explicit módon szabályozhatja a fejlesztendő funkciók és útvonalak vannak beállítva, például a hitelesítés, tábla API-kat, és leküldéses regisztrációs végpont hello. több, lásd: toolearn [toouse hogyan .NET server SDK hello Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).
* Más típusú ASP.NET-projekt és az útvonalak támogatása. A mobil-háttéralkalmazást projektként üzemeltethető ugyanaz a projekt hello MVC és a Web API tartományvezérlőinek.
* Támogatás az új App Service hitelesítési szolgáltatások, amelyek lehetővé teszik a webes és mobilalkalmazások toouse egy közös hitelesítési konfigurációt.

## <a name="overview"></a>Alapszintű frissítési áttekintése
Sok esetben frissítése lesz egyszerűen váltás toohello új Mobile Apps server SDK és a közzététel a kódot egy új Mobile Apps-példányt. Vannak, azonban néhány olyan forgatókönyvet, amely további konfigurálást, például a speciális hitelesítési forgatókönyvek és a szükséges ütemezett feladatokat. Ezek mindegyikének hello tárgyalja későbbi szakaszokban.

> [!TIP]
> Javasoljuk, hogy olvassa el, és teljesen tisztában hello a témakör hátralévő része a frissítés megkezdése előtt. Jegyezze fel minden funkcióját használja alatt elnevezése.
>
>

hello Mobile Services-ügyfél SDK-k vannak **nem** hello új Mobile Apps server SDK kompatibilis. A sorrend tooprovide szolgáltatási folytonosság érdekében az alkalmazások nem módosítások tooa webhelyet jelenleg szolgál a közzétett ügyfelek kell közzétenni. Ehelyett kell hozzon létre egy új mobil-alkalmazást, amely duplikált funkcionál. Ez az alkalmazás helyezhet el hello a járulékos költségekkel nélül tooavoid ugyanaz az App Service tervezze meg.

Ezután meg kell hello alkalmazás két verziója: egy mely nem hello azonos, és más, amely ezután frissítheti és a cél új ügyfél kiadását helyettesítő hello és hello közzétett alkalmazások szolgálja ki. Helyezze át, és tesztelheti a kódját a ütemben, de meg kell győződnie arról, hogy a bármely hibajavításokat tartalmaz, akkor alkalmazott tooboth beolvasása. Ha úgy véli, hogy helyettesítő hello ügyfél alkalmazások kívánt számát toohello legújabb verzióra frissítette, ha felügyelni hello eredeti áttelepített alkalmazás törlése.

hello teljes vázlat hello frissítési folyamat a következőképpen történik:

1. Új mobileszköz-alkalmazás létrehozása
2. Frissítés hello projekt toouse hello új Server SDK-k
3. Az ügyfél alkalmazás új verzióját
4. (Választható) Az eredeti áttelepített példány törlése

## <a name="mobile-app-version"></a>Egy második alkalmazáspéldány létrehozása
hello első lépéseként frissítése toocreate hello mobilalkalmazás erőforrás az alkalmazás új verziójának hello futtatni. Ha egy meglévő mobilszolgáltatás már áttelepített, célszerű toocreate hello ezen verzióját tartalmazó csomagot. Nyissa meg hello [Azure-portálon] , és keresse meg a tooyour alkalmazás át. Jegyezze fel az App Service-csomag futtatása hello.

Ezután hozzon létre hello második alkalmazáspéldány által következő hello [.NET háttérrendszer létrehozásának utasításokat](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Ha Ön App Service-csomagot, vagy "üzemeltetési terv" Válassza ki a kért tooselect hello az áttelepített alkalmazás tervét.

Érdemes toouse hello ugyanazt az adatbázist, és az értesítési központot, ha Ön volt a Mobile Services szolgáltatásban. Ezek az értékek másolásához megnyitása [Azure-portálon] és a Navigálás toohello eredeti alkalmazást, majd kattintson a **beállítások** > **Alkalmazásbeállítások**. A **kapcsolati karakterláncok**, másolása `MS_NotificationHubConnectionString` és `MS_TableConnectionString`. Keresse meg az új frissítési hely tooyour, és illessze be őket, felülírja a meglévő értékkel. Ismételje meg ezt a folyamatot minden más alkalmazás beállításait az alkalmazás igényeinek. Ha nem használ az áttelepített szolgáltatás, áttekintheti a kapcsolati karakterláncokat és Alkalmazásbeállítások a hello **konfigurálása** hello hello Mobile Services című lapján [a klasszikus Azure portálon].

Másolatot készít az ASP.NET-projekt hello az alkalmazáshoz, és tegye közzé tooyour új helyet. Az ügyfélalkalmazás hello új URL-címet a frissített példányával, ellenőrizze, hogy minden megfelelően működik-e.

## <a name="updating-hello-server-project"></a>Hello server projekt frissítése
Mobile Apps biztosít egy új [Mobile App SDK-t Server] nagy részét hello biztosító funkcióját is hello Mobile Services futásidejű. Először el kell távolítania minden hivatkozások toohello Mobile Services csomagok. A hello NuGet-Csomagkezelő, keresse meg a `WindowsAzure.MobileServices.Backend`. Legtöbb alkalmazást megjelenik itt, több csomagot, beleértve a `WindowsAzure.MobileServices.Backend.Tables` és `WindowsAzure.MobileServices.Backend.Entity`. Ilyen esetben a kiindulási pont hello legalacsonyabb csomag hello függőségi fa, például `Entity`, és távolítsa el. Amikor a rendszer kéri, ne távolítsa el az összes függő csomagot. Ismételje meg a folyamatot, amíg nem távolította el `WindowsAzure.MobileServices.Backend` magát.

Ezen a ponton a projekt, amely már nem hivatkozik a Mobile Services SDK-k fog.

Ezután adhat hivatkozások hello Mobile Apps SDK-t. Ehhez a frissítéshez legtöbb fejlesztő szeretné, hogy toodownload lesz, és telepítse a hello `Microsoft.Azure.Mobile.Server.Quickstart` csomagot, mivel ez fogja lekérni a hello teljes szükséges készlet.

Számos fordítóprogram-hibákkal eredő hello SDK-k közötti különbségek lesznek, de ezek könnyen tooaddress, és ez a szakasz többi hello tartoznak.

### <a name="base-configuration"></a>Alapkonfiguráció
Ezt követően WebApiConfig.cs, módosíthatja:

        // Use this class tooset configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class tooset WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

a

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

> [!NOTE]
> Ha a toolearn többet hello új a .NET server SDK, és hogyan tooadd és törlése funkció az alkalmazásból, tekintse meg a hello [hogyan toouse hello .NET server SDK] témakör.
>
>

Ha az alkalmazás lehetővé teszi hello hitelesítési funkcióját használja, konfigurálnia kell egy OWIN köztes tooregister. Ebben az esetben kell áthelyez hello konfigurációs kód fent egy új OWIN indítási osztályt.

1. Adja hozzá a NuGet-csomag hello `Microsoft.Owin.Host.SystemWeb` Ha már nincs belefoglalva a projektben.
2. A Visual Studióban, kattintson a jobb gombbal a projekt, és válassza ki **Hozzáadás** -> **új elem**. Válassza ki **webes** -> **általános** -> **OWIN indítási osztály**.
3. Kód fent hello MobileAppConfiguration az áthelyezés `WebApiConfig.Register()` toohello `Configuration()` az új indítási osztály.

Győződjön meg arról, hogy hello `Configuration()` metódus végződik:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Nincsenek további módosításokat kapcsolódó tooauthentication, amelyek hello teljes hitelesítést az alábbi részben szerepelnek.

### <a name="working-with-data"></a>Adatok használata
A Mobile Services szolgáltatásban hello mobilalkalmazás neve hello alapértelmezett séma nevének a hello Entity Framework telepítő szolgált.

tooensure, amelyekhez azonos hello séma hivatkozott azt korábban használata hello tooset hello séma hello DbContext az alkalmazás a következő:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Győződjön meg arról, hogy be, ha az Ön hello fent MS_MobileServiceName. Ha az alkalmazás korábban testreszabott ez is megadhatja egy másik sémanév.

### <a name="system-properties"></a>Rendszer tulajdonságai
#### <a name="naming"></a>Elnevezés
Hello Azure Mobile Services-kiszolgáló SDK, a rendszer tulajdonságai mindig tartalmaznak dupla aláhúzás (`__`) hello tulajdonságok előtagja:

* __createdAt
* __updatedAt
* __deleted
* __version

hello Mobile Services-ügyfél SDK-k különleges logikát elemzésekor a rendszer tulajdonságai ebben a formátumban van.

Az Azure Mobile Apps a rendszer tulajdonságai már nem kell egy speciális formátumú, és rendelkezik a következő neveket hello:

* createdAt
* updatedAt
* törölve
* Verzió

hello Mobile Apps ügyfél SDK-k hello új rendszer tulajdonságok nevek használata, így nincs szükség tooclient kódot. Ha közvetlenül REST így meghívja a tooyour szolgáltatást, majd ennek megfelelően módosítsa a lekérdezéseket.

#### <a name="local-store"></a>Helyi tárolóból.
hello módosítások toohello nevek tulajdonságainak jelenti azt, hogy egy kapcsolat nélküli szinkronizálás a Mobile Services helyi adatbázis nem kompatibilis a Mobile Apps. Ha lehetséges ne a Mobile Services tooMobile alkalmazások amíg után függőben lévő módosítások elküldése toohello server ügyfélalkalmazások frissítése. Ezt követően hello frissített alkalmazás egy új adatbázis fájlneve kell használni.

Ha egy ügyfél-alkalmazást a Mobile Services tooMobile alkalmazások frissítve van, amíg vannak függőben lévő módosítások offline hello művelet sorban, majd hello rendszeradatbázis frissített toouse hello új oszlopnevek kell lennie. IOS rendszerű eszközökön ez elérhető egyszerűsített áttelepítések toochange hello oszlopnevek használatával. Android és hello .NET felügyelt ügyfél kell írt egyéni SQL toorename hello oszlopok a adattáblák objektum.

IOS az adatok entitások toomatch hello következő kell módosítani a Core adatsémája nem támogatott. Vegye figyelembe, hogy a Tulajdonságok hello `createdAt`, `updatedAt` és `version` már nem rendelkezik egy `ms_` előtagja:

| Attribútum | Típus | Megjegyzés |
| --- | --- | --- |
| id |Karakterlánc, kötelezőként megjelölt |elsődleges távoli tárolóban levő kulccsal. |
| createdAt |Dátum |(választható) maps toocreatedAt rendszer tulajdonság |
| updatedAt |Dátum |(választható) maps tooupdatedAt rendszer tulajdonság |
| Verzió |Karakterlánc |(választható) használt toodetect ütközések, maps tooversion |

#### <a name="querying-system-properties"></a>A Rendszertulajdonságok lekérdezése
Azure Mobile Services, a rendszer tulajdonságai nem kap alapértelmezés szerint, de csak akkor, amikor erre felkérést kapnak hello lekérdezési karakterláncot `__systemProperties`. Ezzel szemben az Azure Mobile Apps rendszer tulajdonságainak vannak **mindig ki** óta hello server SDK objektummodell részét képezik.

Ez a változás főként hatással van a tartomány-kezelő bővítményének egyéni implementációja `MappedEntityDomainManager`. A Mobile Services szolgáltatásban egy ügyfél soha nem kér a rendszer tulajdonságai esetén lehetséges toouse egy `MappedEntityDomainManager` , amelyek nem tényleges képezze le az összes tulajdonságait. Azonban az Azure Mobile Apps, a nem leképezett tulajdonságok hibát okoz a GET-lekérdezésekben.

hello legegyszerűbb módja tooresolve hello probléma van a a DTOs toomodify, hogy azok örökölhet `ITableData` helyett `EntityData`. Adja hozzá a hello `[NotMapped]` megadni toohello mezők attribútum.

Például hello következő meghatározása `TodoItem` rendszer tulajdonságok nélkül:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Megjegyzés: Ha hibaüzenet jelenik meg `NotMapped`, vegye fel a referencia-toohello szerelvény `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS
Mobile Services használatával hello ASP.NET CORS megoldás néhány CORS támogatás tartalmazza. Az alkalmazásburkoló réteg lett eltávolított toogive hello fejlesztői nagyobb felügyeletet, így közvetlenül hasznosíthatja [ASP.NET CORS-támogatás](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

hello fő a problémás területeket. Ha a CORS használatával is, hogy hello `eTag` és `Location` fejlécek engedélyezni kell ahhoz, hogy hello ügyfél SDK-k toowork megfelelően.

### <a name="push-notifications"></a>Leküldéses értesítések
A leküldéses hello fő előfordulhat, hogy hiányzik a kiszolgáló SDK hello tétel hello PushRegistrationHandler osztály. Regisztráció a Mobile Apps kicsit másképp kezeli, és tagless regisztrációk alapértelmezés szerint engedélyezve vannak. Címkék kezelése végezhető egyéni API-k használatával. Tekintse meg a hello [címkék regisztrálása](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) olvashat útmutatást.

### <a name="scheduled-jobs"></a>Ütemezett feladatok
Ütemezett feladatok nem beépített Mobile Apps, így a meglévő feladatokat, a .NET-háttérrendszer rendelkező toobe külön-külön frissíteni kell. Egy elem egy ütemezett toocreate [webes projekt] hello mobilalkalmazás kód helyen. Is képes állítson be egy tartományvezérlőre, amely tárolja a feladat kódot, és hello konfigurálása [Azure Scheduler] toohit adott végpontra várt hello ütemezés szerint.

### <a name="miscellaneous-changes"></a>Egyéb változások
Minden ApiControllers, amely fognak használni a mobil ügyfél most már rendelkeznie kell hello `[MobileAppController]` attribútum. Ez az már nem veszi fel alapértelmezés szerint úgy, hogy más érinti ApiControllers toogo hello mobil is.

Hello `ApiServices` objektum már nincs hello SDK része. mobilalkalmazás beállításai tooaccess, hello következő használható:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Hasonlóképpen naplózási most teszi lehetővé hello szabványos az ASP.NET nyomkövetési írásakor:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

## <a name="authentication"></a>Hitelesítési kapcsolatos szempontok
a Mobile Services hello hitelesítés összetevői most helyezte hello App Service hitelesítési/engedélyezési funkciójához. Megismerheti a hely ez általi olvasása hello [Add authentication tooyour mobilalkalmazás](app-service-mobile-ios-get-started-users.md) témakör.

Az egyes szolgáltatók – például az aad-ben, a Facebook-on és a Google a Másolás-alkalmazás képes tooleverage hello meglévő regisztrációs kell lennie. Egyszerűen toonavigate toohello identitásszolgáltató portálon kell, és adja hozzá egy új átirányítási URL-cím toohello regisztrációt. Ezután konfigurálja az App Service hitelesítési/engedélyezési hello ügyfél-azonosító és a titkos kulcs.

### <a name="controller-action-authorization"></a>A tartományvezérlő műveleti engedélyezési
Minden példányát hello `[AuthorizeLevel(AuthorizationLevel.User)]` attribútum most kell lennie a módosított toouse szabványos ASP.NET hello `[Authorize]` attribútum. Emellett tartományvezérlők, névtelen alapértelmezett, mint más ASP.NET-alkalmazások.
Ha használta egy hello más AuthorizeLevel beállítások, például a rendszergazda vagy az alkalmazás, vegye figyelembe, hogy ezek elvesznek. Ehelyett a közös titkokat AuthorizationFilters beállítása, vagy biztonságosan konfigurálása az AAD-szolgáltatásnév tooenable szolgáltatások közötti hívások.

### <a name="getting-additional-user-information"></a>További felhasználói adatainak lekérése
Kaphat további felhasználói információkat, beleértve a hozzáférési jogkivonatok hello keresztül `GetAppServiceIdentityAsync()` módszert:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Továbbá ha az alkalmazás függőségek tart a felhasználói azonosítót, például az adatbázisban tárolja őket, fontos, hogy a Mobile Services és az App Service Mobile Apps közötti felhasználói azonosítók hello toonote eltérőek. Hello Mobile Services felhasználói Azonosítóját, azonban továbbra is megjelenik. Mindegyik hello ProviderCredentials alosztályok rendelkezik a UserId tulajdonság. Így a folytatás előtt hello példa:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Ha az alkalmazás végrehajtása függőségek, a felhasználói azonosítót, akkor fontos, hogy kihasználja lehetőleg hello regisztrálás egy identitásszolgáltatóval. Felhasználói azonosítói általában hatókörön belüli toohello regisztrációja használt, így vezet be, egy új regisztrációs problémák megfelelő felhasználók tootheir adatok sikerült létrehozni.

### <a name="custom-authentication"></a>Egyéni hitelesítési
Ha az alkalmazás egy egyéni hitelesítési megoldás használja, érdemes arról, hogy a frissített hello hely hozzáférési toohello toomake. Hello új egyéni hitelesítési hello az utasításokat követve [.NET SDK – áttekintés] toointegrate a megoldás. Vegye figyelembe, hogy hello egyéni összetevők továbbra is a képen vannak-e.

## <a name="updating-clients"></a>Ügyfelek frissítése
Miután egy operatív mobil-háttéralkalmazást, használhat az ügyfélalkalmazást, amely használ fel az új verziója. Mobile Apps is hello ügyfél SDK-k új verzióját tartalmazza, és a fenti a hasonló toohello kiszolgáló frissítése, szüksége lesz az összes hivatkozott toohello Mobile Services SDK előtt tooremove hello Mobile Apps verziók telepítése.

Egyik fő változás hello hello verziói között, hogy a hello konstruktorok már nem szükséges-e egy alkalmazás-kulcsot. Most már egyszerűen adja meg a a Mobile Apps hello URL-CÍMÉT. Például hello .NET ügyfeleken hello `MobileServiceClient` konstruktor:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of hello Mobile App
        );

Áttekintheti az telepítése új SDK-k hello és hello új struktúra keresztül az alábbi hivatkozások hello használata:

* [iOS-verzióval 3.0.0 vagy újabb verzió](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) 2.0.0 verzió vagy újabb verzió](app-service-mobile-dotnet-how-to-use-client-library.md)

Ha az alkalmazás lehetővé teszi leküldéses értesítések használni, jegyezze fel a hello regisztráció utasításokat minden egyes platformhoz, néhány módosítás történt hiba is.

Ha hello új ügyfélverziót készen áll, próbálja ki a frissített kiszolgálón projekthez. Után ellenőrzése, hogy működik-e, az alkalmazás toocustomers egy új verziója is megjelenhetnek. Előfordulhat Ha az ügyfelek rendelkezésére állt-e egy alkalommal tooreceive ezeket a frissítéseket, törölheti hello Mobile Services, verziószám: az alkalmazás. Ezen a ponton frissített tooan App Service Mobile Apps hello legújabb Mobile Apps server SDK használatával.

<!-- URLs. -->

[Azure-portálon]: https://portal.azure.com/
[a klasszikus Azure portálon]: https://manage.windowsazure.com/
[Mik azok a Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App SDK-t Server]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[webes projekt]: ../app-service-web/websites-webjobs-resources.md
[hogyan toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[App Service díjszabás]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET SDK – áttekintés]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
