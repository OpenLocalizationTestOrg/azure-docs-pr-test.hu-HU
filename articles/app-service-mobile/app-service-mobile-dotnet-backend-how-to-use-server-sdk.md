---
title: "aaaHow toowork hello .NET háttérrendszer kiszolgálóval a Mobile Apps SDK |} Microsoft Docs"
description: "Ismerje meg, hogyan toowork az Azure App Service Mobile Apps háttérkiszolgáló .NET SDK hello."
keywords: "az App service, a azure app service, a mobilalkalmazás, a mobilszolgáltatást, a méretezési, méretezhető, központi telepítését, az azure app alkalmazástelepítés"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2946c5ba4424565ac764e2ce5597bf42362fcedf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-hello-net-backend-server-sdk-for-azure-mobile-apps"></a>Az Azure Mobile Apps hello .NET háttérkiszolgáló SDK használata
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Ez a témakör bemutatja, hogyan toouse hello .NET háttérkiszolgáló SDK az Azure App Service Mobile Apps főbb forgatókönyvek. hello Azure Mobile Apps SDK segítséget nyújt az ASP.NET-alkalmazás a mobil ügyfelek dolgozni.

> [!TIP]
> Hello [.NET server az Azure Mobile Apps SDK] [ 2] nyílt forráskódú a Githubon. hello tárház tartalmazza az összes hello teljes server SDK egység tesztcsomag és néhány mintaprojektjeit beleértve.
>
>

## <a name="reference-documentation"></a>Segédanyagok
hello server SDK hello referenciadokumentációt tartalmaz a következő helyen található: [Azure Mobile Apps .NET hivatkozás][1].

## <a name="create-app"></a>Hogyan: .NET Mobile Apps-háttéralkalmazás létrehozása
Ha a számítógépet egy új projektet, létrehozhat egy App Service-alkalmazást vagy hello [Azure-portálon] vagy a Visual Studio. Hello App Service alkalmazás helyileg történő futtatása, vagy hello projekt tooyour felhőalapú App Service mobile alkalmazás közzététele.

Ha mobil funkciókkal tooan létező projekt hozzáadni, lásd: hello [töltse le és hello SDK inicializálása](#install-sdk) szakasz.

### <a name="create-a-net-backend-using-hello-azure-portal"></a>Hello Azure-portál használatával a .NET-háttéralkalmazás létrehozása
az App Service mobil-háttéralkalmazást toocreate, vagy hajtsa végre a hello [gyors üzembe helyezési útmutató] [ 3] vagy kövesse az alábbi lépéseket:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Vissza a hello *Ismerkedés* panelen, a **tábla API létrehozása**, válassza a **C#** , a **háttéralkalmazás-nyelv**. Kattintson a **letöltése**, bontsa ki a tömörített project fájlok tooyour helyi számítógépen, és nyissa meg a hello megoldást a Visual Studióban.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 és a Visual Studio 2015-öt használó .NET-háttéralkalmazás létrehozása
Telepítse a hello [Azure SDK for .NET] [ 4] (2.9.0 verzió vagy újabb) toocreate egy Azure Mobile Apps-projektet a Visual Studio. Hello SDK telepítése után hozzon létre egy ASP.NET alkalmazást hello a következő lépéseket:

1. Nyissa meg hello **új projekt** párbeszédpanelen (a *fájl* > **új** > **projekt …** ).
2. Bontsa ki a **sablonok** > **Visual C#**, és válassza ki **webes**.
3. Válassza ki **ASP.NET webalkalmazás**.
4. Töltse ki hello projekt nevét. Ezután kattintson az **OK** gombra.
5. A *ASP.NET 4.5.2 sablont*, jelölje be **Azure Mobile Apps**. Ellenőrizze **hello felhőben lévő gazdagéphez** toocreate hello a mobil-háttéralkalmazást cloud toowhich közzéteheti ebben a projektben.
6. Kattintson az **OK** gombra.

## <a name="install-sdk"></a>Hogyan: Töltse le és hello SDK inicializálása
hello SDK nem érhető el a [NuGet.org]. Ez a csomag hello szükséges alapvető funkciók tooget hello SDK használatának tartalmazza. tooinitialize hello SDK, hello tooperform műveleteket kell **HttpConfiguration** objektum.

### <a name="install-hello-sdk"></a>Hello SDK telepítése
tooinstall hello SDK-t, kattintson a jobb gombbal a hello server projektre a Visual Studio kiválasztása **NuGet-csomagok kezelése**, keressen a hello [Microsoft.Azure.Mobile.Server] csomagot, majd kattintson az  **Telepítés**.

### <a name="server-project-setup"></a>Hello kiszolgálóprojektet inicializálása
Egy .NET-háttérrendszer kiszolgálóprojektet inicializált hasonló tooother ASP.NET projektek, az OWIN indítási osztály-ot. Győződjön meg arról, hogy rendelkezik-e hivatkozott hello NuGet-csomag `Microsoft.Owin.Host.SystemWeb`. tooadd Ez az osztály a Visual Studióban, kattintson a jobb gombbal a kiszolgáló-projektet, majd válassza ki a **Hozzáadás** >
**új elem**, majd **webes**  >  ** Általános** > **OWIN indítási osztály**.  Egy osztály hoz létre a következő attribútum hello:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

A hello `Configuration()` az OWIN indítási osztályt, használja a metódus egy **HttpConfiguration** objektum tooconfigure hello Azure Mobile Apps-környezetben.
a következő példa hello inicializálja hello kiszolgálóprojektet semmilyen hozzáadott szolgáltatásokkal:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

tooenable egyes funkciókat, meg kell hívnia kiterjesztésmetódusok a hello **MobileAppConfiguration** hívása előtt objektum **ApplyTo**. Például létrehozza a következő kód hello hello alapértelmezett útvonalak tooall API tartományvezérlők hello attribútummal rendelkező `[MobileAppController]` inicializálása közben:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

hello server gyors üzembe helyezés az Azure portál hívások hello **UseDefaultConfiguration()**. A telepítő a következő egyenértékű toohello:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from hello Home package
            .MapApiControllers()
            .AddTables(                               // from hello Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from hello Entity package
                )
            .AddPushNotifications()                   // from hello Notifications package
            .MapLegacyCrossDomainController()         // from hello CrossDomain package
            .ApplyTo(config);

használt hello kiegészítő módszerek a következők:

* `AddMobileAppHomeController()`hello alapértelmezett Azure Mobile Apps kezdőlap biztosít.
* `MapApiControllers()`egyéni API képességeket biztosít a hello attribútummal WebAPI tartományvezérlők `[MobileAppController]` attribútum.
* `AddTables()`biztosítja a leképezést, hello `/tables` végpontok tootable tartományvezérlők.
* `AddTablesWithEntityFramework()`leképezési hello egy rövid aktuális van `/tables` használó Entity Framework végpontok alapú tartományvezérlők.
* `AddPushNotifications()`eszközök regisztrálása a Notification Hubs egy egyszerű módszert kínál.
* `MapLegacyCrossDomainController()`standard CORS fejlécek biztosít helyi fejlesztési.

### <a name="sdk-extensions"></a>SDK-bővítmények
a következő NuGet-alapú bővítménycsomagok hello adja meg az alkalmazás által használható különböző mobil funkciókat. Bővítmények inicializálásakor használatával engedélyezheti hello **MobileAppConfiguration** objektum.

* [Microsoft.Azure.Mobile.Server.Quickstart] támogatja hello alapvető Mobile Apps beállítási. Hívó hello által hozzáadott toohello konfigurációs **UseDefaultConfiguration** kiterjesztésmetódus inicializálása során. Ezt a bővítményt tartalmazza a következő kiterjesztések: értesítések, a hitelesítés, az entitás, a táblák, a tartományok közötti és otthoni csomagok. Ez a csomag hello Mobile Apps gyorsindítási hello Azure-portálon elérhető használják.
* [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) hello alapértelmezett megvalósítja *a mobilalkalmazás megfelelően működik, és lap* hello webhely gyökér. Meghívásával toohello konfiguráció hozzáadása a **AddMobileAppHomeController** kiterjesztésmetódus.
* [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) adatok és a készletek felfelé hello adatok csővezeték való munkához osztályokat tartalmazza. Toohello konfigurációs hozzá hívó hello **AddTables** kiterjesztésmetódus.
* [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) lehetővé teszi, hogy hello Entity Framework tooaccess hello SQL-adatbázis található. Toohello konfigurációs hozzá hívó hello **AddTablesWithEntityFramework** kiterjesztésmetódus.
* [Microsoft.Azure.Mobile.Server.Authentication] lehetővé teszi, hogy hitelesítést és a készletek felfelé hello OWIN köztes használt toovalidate jogkivonatokat. Toohello konfigurációs hozzá hívó hello **AddAppServiceAuthentication** és **IAppBuilder**. **UseAppServiceAuthentication** kiterjesztésmetódusok.
* [Microsoft.Azure.Mobile.Server.Notifications] lehetővé teszi a leküldéses értesítések és egy leküldéses regisztrációs végpontot határozza meg. Toohello konfigurációs hozzá hívó hello **AddPushNotifications** kiterjesztésmetódus.
* [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) hoz létre, amely adatok toolegacy szolgál a Mobile Apps a webböngésző vezérlő. Meghívásával toohello konfiguráció hozzáadása a **MapLegacyCrossDomainController** kiterjesztésmetódus.
* [Microsoft.Azure.Mobile.Server.Login] egyéni hitelesítési forgatókönyvek során használt statikus metódus hello AppServiceLoginHandler.CreateToken() módszert biztosít.

## <a name="publish-server-project"></a>Hogyan: hello server projekt közzététele
Ez a szakasz bemutatja, hogyan toopublish a .NET-háttérrendszer-projektet a Visual Studio. A háttérprojekt Git használatával is telepítheti, vagy bármely más hello ismertetett módszerek hello [Azure App Service üzembe helyezési dokumentációja](../app-service-web/web-sites-deploy.md).

1. A Visual Studio újraépítése hello projekt toorestore NuGet-csomagok.
2. A Megoldáskezelőben kattintson a jobb gombbal hello projektben kattintson **közzététel**. hello közzéteszi, amikor első alkalommal kell toodefine egy közzétételi profilt. Ha már rendelkezik egy definiált profil, válassza ki azt, és kattintson a **közzététel**.
3. Tooselect egy közzétételi célként, kattintson az **Microsoft Azure App Service** > **következő**, majd (ha szükséges) jelentkezzen be Azure hitelesítő adatait.
   A Visual Studio letölti és tárolja biztonságos helyen a közzétételi beállítások közvetlenül az Azure-ból.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. Válassza ki a **előfizetés**, jelölje be **erőforrástípus** a **nézet**, bontsa ki a **mobilalkalmazás**, és kattintson a mobil-háttéralkalmazást, majd **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. Ellenőrizze a hello profiladatok közzététele, és kattintson a **közzététel**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Ha a mobilalkalmazás háttérrendszerének közzététele sikeresen befejeződött, megjelenik a sikert jelző kezdőlapja.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <a name="define-table-controller"></a>Útmutató: egy tábla vezérlő megadása
Adja meg egy tábla vezérlő tooexpose egy SQL-tábla toomobile ügyfelek.  Egy tábla Controller konfigurálása három lépésből áll:

1. Hozzon létre egy adatok átvitele objektum (DTO) osztályt.
2. Adja meg egy táblahivatkozás hello Mobile DbContext osztályt.
3. Hozzon létre egy tábla tartományvezérlőre.

Egy adatok átvitele objektum (DTO) mező egy egyszerű C#-objektum, amely örökli `EntityData`.  Példa:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

hello DTO használt toodefine hello tábla hello SQL-adatbázis belül.  toocreate hello bejegyzés adatbázisra, és adja hozzá a `DbSet<>` hello használata DbContext tulajdonságot.  A hello alapértelmezett projekt sablon az Azure Mobile Apps, hello DbContext nevezik `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Ha hello Azure SDK telepítve van, most létrehozhat egy sablont table vezérlő az alábbiak szerint:

1. Kattintson a jobb gombbal a hello, tartományvezérlői mappa, és válassza ki **Hozzáadás** > **vezérlő...** .
2. Jelölje be hello **Azure Mobile Apps Table vezérlő** lehetőséget, majd kattintson az **Hozzáadás**.
3. A hello **adja hozzá a tartományvezérlő** párbeszédpanel:
   * A hello **Model class** legördülő menüben válassza ki az új DTO.
   * A hello **DbContext** legördülő menüben válassza hello Mobile Service DbContext osztályt.
   * hello vezérlőnév akkor jön létre.
4. Kattintson az **Add** (Hozzáadás) parancsra.

hello gyors üzembe helyezés kiszolgálóprojektet egy egyszerű példa tartalmaz **TodoItemController**.

### <a name="adjust-pagesize"></a>Hogyan: hello tábla a lapozófájl méretének módosítása
Alapértelmezés szerint az Azure Mobile Apps kérelmenként 50 bejegyzést ad vissza.  Lapozófájl biztosítja, hogy hello az ügyfél nem köti a felhasználói felület és a szál nem hello kiszolgáló túl sokáig a jó felhasználói élményt biztosítva. toochange hello táblázat a lapozófájl méretét, növelje hello kiszolgálóoldali "engedélyezett lekérdezés mérete", és hello ügyféloldali oldal méretét hello kiszolgálóoldali "engedélyezett a lekérdezés mérete" módosul hello segítségével `EnableQuery` attribútum:

    [EnableQuery(PageSize = 500)]

Helyezze a pageSize értékének hello hello azonos vagy nagyobb hello hello ügyfél által kért.  Hello ügyfél Oldalméret módosítása a részletekért tekintse meg a toohello adott ügyfél útmutató dokumentációját.

## <a name="how-to-define-a-custom-api-controller"></a>Hogyan: Adja meg egy egyéni API-vezérlő
hello egyéni API-vezérlőben hello legalapvetőbb funkció tooyour Mobile Apps-háttéralkalmazás jelentkezik, mintha a végpont biztosítja. A mobileszköz-specifikus API-vezérlőben hello [MobileAppController] attribútum használatával regisztrálhatja. Hello `MobileAppController` attribútum hello útvonal regisztrálja, hello Mobile Apps JSON szerializáló beállítja és bekapcsolja a [ügyfél verzióellenőrzés](app-service-mobile-client-and-server-versioning.md).

1. A Visual Studióban, kattintson a jobb gombbal hello tartományvezérlők mappát, majd kattintson a **Hozzáadás** > **vezérlő**, jelölje be **Web API 2-es vezérlőhöz&mdash;üres** és Kattintson a **Hozzáadás**.
2. Adjon meg egy **tartományvezérlő neve**, például `CustomController`, és kattintson a **Hozzáadás**.
3. Hello új tartományvezérlő osztály fájlban adja hozzá a hello következő using utasítást:

        using Microsoft.Azure.Mobile.Server.Config;
4. Hello alkalmazása **[MobileAppController]** toohello API vezérlő osztálydefiníció, mint például a következő hello attribútum:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. App_Start/Startup.MobileApp.cs fájlban adja hozzá a hívás toohello **MapApiControllers** kiterjesztésmetódus, mint például a következő hello:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Is használhatja a hello `UseDefaultConfiguration()` kiterjesztésmetódus helyett `MapApiControllers()`. Minden futtassa, amelynek nincs **MobileAppControllerAttribute** alkalmazza az ügyfelek által továbbra is elérhető, de előfordulhat, hogy nem megfelelően védelmekor ügyfelek bármely Mobile Apps-ügyfél SDK használatával.

## <a name="how-to-work-with-authentication"></a>Útmutató: hitelesítés
Az Azure Mobile Apps használja az App Service hitelesítés / engedélyezés toosecure a mobil-háttéralkalmazást.  Ez a szakasz bemutatja, hogyan tooperform hello hitelesítési modulhoz kapcsolódó feladatokat a .NET háttérrendszer kiszolgálóprojektet a következő:

* [Útmutató: hitelesítés tooa projekt hozzáadása](#add-auth)
* [Útmutató: az alkalmazás egyéni hitelesítés használata](#custom-auth)
* [Hogyan: lekérése hitelesített felhasználói adatok](#user-info)
* [Útmutató: a jogosult felhasználók adatokhoz való hozzáférést](#authorize)

### <a name="add-auth"></a>Útmutató: hitelesítés tooa projekt hozzáadása
Hello kiterjesztésével is hozzáadhat a hitelesítési tooyour kiszolgálóprojektet **MobileAppConfiguration** objektum és OWIN köztes konfigurálása. Hello telepítésekor [Microsoft.Azure.Mobile.Server.Quickstart] csomag- és hívás hello **UseDefaultConfiguration** kiterjesztésmetódus, kihagyhatja a toostep 3.

1. A Visual Studio telepítése hello [Microsoft.Azure.Mobile.Server.Authentication] csomag.
2. Hello Startup.cs projekt fájlban adja hozzá a következő kódsort hello hello elején hello **konfigurációs** módszert:

        app.UseAppServiceAuthentication(config);

    Az OWIN köztes összetevő hello tartozó App Service-átjáró által kiállított jogkivonatokat ellenőrzi.
3. Adja hozzá a hello `[Authorize]` attribútum tooany vezérlő vagy metódust, amelyhez hitelesítés szükséges.

arról, hogyan toolearn tooauthenticate ügyfelek tooyour Mobile Apps-háttéralkalmazás, lásd: [Add authentication tooyour alkalmazás](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Útmutató: az alkalmazás egyéni hitelesítés használata
Ha nem kíván toouse hello App Service hitelesítési/engedélyezési szolgáltatók egyikét, a saját bejelentkezési rendszer valósíthat meg. Telepítse a hello [Microsoft.Azure.Mobile.Server.Login] csomag tooassist a hitelesítési jogkivonatok létrehozásához.  Adja meg a saját kódot a felhasználó hitelesítő adatainak ellenőrzése. Például ellenőrizze sózott és kivonatolt jelszavakat adatbázisban ellen. Hello az alábbi példában a hello `isValidAssertion()` (határozni) metódus felelős az ellenőrzést.

egyéni hitelesítési hello tesz elérhetővé, és adjon meg egy ApiController létrehozása `register` és `login` műveletek. hello ügyfélnek használnia kell, egy egyéni felhasználói felületi toocollect hello információ hello felhasználótól.  hello információt nem egy normál HTTP POST az elküldött toohello API hívása. Miután hello kiszolgáló hello helyességi feltétel érvényesíti, jogkivonat használatával hello kiadott `AppServiceLoginHandler.CreateToken()` metódust.  hello ApiController **nem kell** hello használata `[MobileAppController]` attribútum.

Példa `login` művelet:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

A fenti példa hello LoginResult és LoginResultUser a szükséges tulajdonságok feltáró szerializálható objektumok. hello ügyfél bejelentkezési válaszok toobe adja vissza a JSON-objektumok hello űrlap vár:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

Hello `AppServiceLoginHandler.CreateToken()` metódust tartalmaz egy *célközönség* és egy *kibocsátó* paraméter. Mindkét paraméter beállított toohello URL-CÍMÉT az alkalmazás gyökérkönyvtára hello HTTPS protokollt használ. Hasonló módon állítsa be *secretKey* toobe hello értéket az alkalmazás csomagazonosítóját aláírási kulcs. Ne ossza el a hello aláírókulcsának ügyfélprogram használt toomint kulcsok kell és felhasználók megszemélyesítéséhez. Ezt úgy szerezheti be aláírókulcsának közben hello Vezérlőpultjának az App Service-ben üzemeltetett hello *webhely\_AUTH\_aláírás\_kulcs* környezeti változó. Szükség esetén egy helyi hibakeresési környezetben – kövesse a hello hello utasításait [hitelesítéssel helyi hibakeresés](#local-debug) tooretrieve hello kulcs szakaszt, és tárolja, az alkalmazás-beállítás.

hello kiállított jogkivonat más jogcímeket és lejárati dátummal is.  A minimális követelmény, hello kiállított jogkivonat tartalmaznia kell a tulajdonos (**sub**) jogcímek.

Hello szabványos ügyfél támogathatja `loginAsync()` metódus által túl van terhelve hello hitelesítési útvonal.  Ha a hello ügyfél `client.loginAsync('custom');` a toolog, az útvonal lehet `/.auth/login/custom`.  Hello útvonal a hello egyéni vezérlő használatával állíthatja be `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> Hello segítségével `loginAsync()` megközelítés biztosítja, hogy hello hitelesítésére szolgáló jogkivonat csatolt tooevery későbbi hívás toohello szolgáltatás.
>
>

### <a name="user-info"></a>Hogyan: lekérése hitelesített felhasználói adatok
Amikor a felhasználó hitelesítése az App Service, hozzárendelt felhasználói Azonosítót és egyéb információkat a .NET-háttéralkalmazás kódjának hello végezheti el. hello felhasználói adatok a hitelesítési döntések meghozatala során a hello háttérbeli használhatók. hello alábbira szerzi be a kérelemhez társított hello Felhasználóazonosító:

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

hello SID hello szolgáltatóhoz tartozó felhasználói azonosító származik, és egy adott felhasználó és a bejelentkezés-szolgáltató statikus.  hello SID értéke érvénytelen a hitelesítési tokenek null.

App Service lehetővé teszi a bejelentkezési szolgáltató által adott jogcímek igénylésére. Minden egyes identitásszolgáltató az identitásszolgáltató SDK használatával további információk megadására.  Például ismerősök információt hello Facebook Graph API-t is használhatja.  Hello szolgáltató panel az Azure-portálon hello igényelt jogcímeket adhat meg. Egyes jogcímek hello identitásszolgáltatóval további beállításokra van szükség.

hello következő hívások hello kódot **GetAppServiceIdentityAsync** bővítmény metódus tooget hello bejelentkezési hitelesítő adatok, többek között hello hozzáférési jogkivonat szükséges toomake kérelmek hello Facebook Graph API szemben:

    // Get hello credentials for hello logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with hello Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request hello current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with hello Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Adja hozzá egy utasítást `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** kiterjesztésmetódus.

### <a name="authorize"></a>Útmutató: a jogosult felhasználók adatokhoz való hozzáférést
Hello előző szakaszban azt lehet bemutatta, hogyan tooretrieve hello egy hitelesített felhasználó felhasználói azonosító. Korlátozhatja a hozzáférést toodata és más erőforrások, ez az érték alapján. Például egy userId oszlop tootables hozzáadása, és a szűrés hello lekérdezési eredmények hello felhasználói azonosító nem egy adott vissza az adatokat csak tooauthorized felhasználók egyszerűen toolimit. hello alábbira sorát adja vissza adatok csak akkor, ha hello biztonsági azonosítója megegyezik-e a hello UserId oszlopban hello TodoItem tábla:

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong toohello current user.
    return Query().Where(t => t.UserId == sid);

Hello `Query()` metódus értéket ad vissza egy `IQueryable` , amely a LINQ-toohandle szűréssel kezelhetők.

## <a name="how-to-add-push-notifications-tooa-server-project"></a>Hogyan: leküldéses értesítések tooa server projekt hozzáadása
Adja hozzá a leküldéses értesítések tooyour kiszolgálóprojektet hello kiterjesztésével **MobileAppConfiguration** objektum és a Notification Hubs ügyfelet hoz létre.

1. A Visual Studióban, kattintson a jobb gombbal a projekt hello, és kattintson **NuGet-csomagok kezelése**, keressen `Microsoft.Azure.Mobile.Server.Notifications`, majd kattintson a **telepítése**.
2. Ismételje meg ezt a lépést tooinstall hello `Microsoft.Azure.NotificationHubs` csomagot, amely tartalmazza a Notification Hubs ügyfélkódtár hello.
3. A App_Start/Startup.MobileApp.cs és adja hozzá a hívás toohello **AddPushNotifications()** kiterjesztésmetódus inicializálása közben:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. Adja hozzá a következő kódot, amely létrehozza a Notification Hubs-ügyfél hello:

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Most már használhatja a hello Notification Hubs toosend leküldéses értesítések tooregistered ügyféleszközökön. További információkért lásd: [Hozzáadás leküldéses értesítések tooyour app](app-service-mobile-ios-get-started-push.md). toolearn Notification hubs használatával kapcsolatos további információkért lásd: [Notification Hubs – áttekintés](../notification-hubs/notification-hubs-push-notification-overview.md).

## <a name="tags"></a>Hogyan: engedélyezése megcélzott ügyfélleküldéses címkék használatával
A Notification Hubs lehetővé teszi a címkék segítségével toospecific regisztrációk célzott értesítéseket küldeni. Több címkék jönnek létre automatikusan:

* Telepítési Azonosítót hello azonosítja az egy adott eszközhöz.
* hello felhasználói azonosító alapján hitelesített hello SID azonosítja egy megadott felhasználó.

telepítési azonosító hello elérhető hello **végrehajtott** hello tulajdonságának **MobileServiceClient**.  hello következő példa bemutatja, hogyan egy telepítési azonosító tooadd használatára egy címke tooa adott telepítési a Notification hubs használatával:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Hello ügyfél leküldéses értesítési regisztráció során megadott címkéket figyelmen kívül hagyják hello háttér hello telepítési létrehozásakor. egy ügyfél tooadd tooenable toohello telepítési címkéket, létre kell hoznia egy egyéni API-t, amely mintát megelőző hello segítségével címkék.

Lásd: [ügyfél által hozzáadott leküldéses értesítések címkék] [ 5] hello App Service Mobile Apps befejezett gyors üzembe helyezési minta példát.

## <a name="push-user"></a>Hogyan: küldés leküldéses értesítések tooan hitelesített felhasználó
Amikor a hitelesített felhasználó regisztrálja a leküldéses értesítések, a felhasználói azonosító címke automatikusan kerül toohello regisztrációs. Ezt a címkét használatával küldhet leküldéses értesítések tooall eszközök adott személy által regisztrált. hello alábbira lekérdezi a kérelmező felhasználó biztonsági azonosítója hello és elküldi a sablon leküldéses értesítési tooevery eszközregisztráció személyre:

    // Get hello current user SID and create a tag for hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for hello template with hello item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification toohello user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Amikor regisztrál egy hitelesített ügyfél leküldéses értesítésekhez, győződjön meg arról, hogy a hitelesítési teljes regisztrációs megkísérlése előtt. További információkért lásd: [toousers leküldéses] [ 6] hello App Service Mobile Apps befejezett gyors üzembe helyezési minta .NET-háttérrendszer.

## <a name="how-to-debug-and-troubleshoot-hello-net-server-sdk"></a>Hogyan: Hibakeresés és hibaelhárítás hello .NET SDK-kiszolgáló
Az Azure App Service számos Hibakeresés és hibaelhárítási módszerekről az ASP.NET-alkalmazások biztosítja:

* [Az Azure App Service figyelése](../app-service-web/web-sites-monitor.md)
* [Az Azure App Service diagnosztikai naplózás engedélyezése](../app-service-web/web-sites-enable-diagnostic-log.md)
* [A Visual Studio egy Azure App Service hibaelhárítása](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Naplózás
Írhat tooApp szolgáltatás diagnosztikai naplók hello szabványos az ASP.NET nyomkövetési írás használatával. Ahhoz, hogy toohello naplókat, engedélyeznie kell a Mobile Apps-háttéralkalmazás diagnosztika.

tooenable diagnostics és az írási toohello naplói:

1. Hello kövesse [hogyan tooenable diagnosztika](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).
2. Adja hozzá hello következő using utasítást a kód fájlban:

        using System.Web.Http.Tracing;
3. Hozzon létre egy nyomkövetési író toowrite hello .NET háttérrendszer toohello diagnosztikai naplók, a az alábbiak szerint:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. A projekt ismételt közzététele, és elérési hello mobilalkalmazás háttér tooexecute hello kód hello naplózással.
5. Töltse le és hello naplókat, kiértékelheti a [hogyan: naplók letöltéséhez](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Helyi hitelesítéssel hibakeresés
Az alkalmazás futtatása helyben tootest toohello felhő közzététel előtt változik. Nyomja le az Azure Mobile Apps-háttérkiszolgálókon legtöbb, *F5* a Visual Studio során. Van azonban néhány további szempontok hitelesítése során.

Rendelkeznie kell egy felhőalapú mobilalkalmazást az App Service hitelesítési/engedélyezési konfigurálva, és az ügyfél hello másodlagos bejelentkezési gazdagépként megadott hello felhő végpontot kell rendelkeznie. Hello lépéseit szükséges ügyfélplatform hello dokumentációjában talál.

Győződjön meg arról, hogy rendelkezik-e a mobil-háttéralkalmazást [Microsoft.Azure.Mobile.Server.Authentication] telepítve. Ezt követően az alkalmazás OWIN indítási osztályt, adja hozzá hello következő után `MobileAppConfiguration` lett alkalmazott tooyour `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

A fenti példa hello, konfigurálnia kell a hello *authAudience* és *authIssuer* Alkalmazásbeállítások belül a Web.config fájl tooeach kell az URL-CÍMÉT az alkalmazás gyökérkönyvtára hello HTTPS használatával a rendszer. Hasonló módon állítsa be *authSigningKey* toobe hello értéket az alkalmazás csomagazonosítóját aláírási kulcs.
tooobtain hello aláírási kulcs:

1. Keresse meg a tooyour app belül hello [Azure-portálon]
2. Kattintson a **eszközök**, **Kudu**, **Ugrás**.
3. Hello Kudu felügyeleti hely, kattintson a **környezet**.
4. Keresse meg a hello értéket *webhely\_AUTH\_aláírás\_kulcs*.

Aláírási kulcs hello használata hello *authSigningKey* paraméter a helyi alkalmazások Config.  A mobil-háttéralkalmazást már felszerelt toovalidate jogkivonatokat, a helyi futtatás során, mely hello ügyfél kapja hello token hello felhőalapú végpont.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure-portálon]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
