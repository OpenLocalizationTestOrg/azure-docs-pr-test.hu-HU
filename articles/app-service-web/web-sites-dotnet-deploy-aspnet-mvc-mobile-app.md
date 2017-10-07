---
title: "az ASP.NET MVC 5 aaaDeploy mobil webalkalmazás az Azure App Service-ben"
description: "Ez az oktatóanyag útmutatást ad, hogy hogyan toodeploy egy webes alkalmazás tooAzure használatával mobile App Service szolgáltatások ASP.NET mvc 5 webalkalmazás."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 0752c802-8609-4956-a755-686116913645
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 01119c07246c0252fd357562774a2e90b3ef77d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a>Az ASP.NET MVC 5 mobil webalkalmazás az Azure App Service telepítése
Ez az oktatóanyag fog mutatja meg, hogyan toobuild egy ASP.NET MVC 5 webalkalmazás, amely mobilbarát alapjait hello és telepítheti az App Service tooAzure. Ebben az oktatóanyagban kell [Visual Studio Express 2013 for Web alkalmazásokat] [ Visual Studio Express 2013] vagy hello Ha már rendelkezik, amely a Visual Studio professional Edition kiadását. Használhat [Visual Studio 2015] , de hello képernyőképek eltérőek lesznek, és hello az ASP.NET 4.x sablonok kell használnia.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a>Mit kell összeállítása
Ebben az oktatóanyagban a hello fogja hozzáadni mobil szolgáltatások toohello egyszerű konferencia-listát alkalmazás biztosított [alapszintű projekt][StarterProject]. hello alábbi képernyőfelvételen látható hello ASP.NET munkamenetek befejeződött hello alkalmazásban, az Internet Explorer 11 F12 fejlesztői eszközök hello böngésző emulátor látható módon.

![][FixedSessionsByTag]

Használhatja az Internet Explorer 11 F12 hello Fejlesztőeszközök és hello [Fiddler eszköz] [ Fiddler] toohelp az alkalmazás hibakeresését. 

## <a name="skills-youll-learn"></a>Megtudhatja, képességek
Az itt ismertetett témák:

* Hogyan toouse Visual Studio 2013 toopublish a webes alkalmazás közvetlenül tooa webalkalmazást az Azure App Service-ben.
* Hogyan hello ASP.NET MVC 5 sablonok használatával hello CSS rendszerindítási keretrendszer javíthatja a mobileszközök megjelenítési
* Hogyan toocreate mobile-specifikus megtekinti tootarget adott mobilböngészők, például a hello iPhone- és Android rendszerhez
* Hogyan toocreate rugalmas nézetek (nézetek toodifferent böngészők válaszolnak az eszközön)

## <a name="set-up-hello-development-environment"></a>Hello fejlesztési környezet beállítása
Állítsa be a fejlesztési környezetet telepítésével hello Azure SDK for .NET 2.5.1-es vagy újabb. 

1. tooinstall hello Azure SDK for .NET, hello hivatkozásra kattintva. Ha nincs még telepítve a Visual Studio 2013, az hello csatlakozásonkénti települ. Ez az oktatóanyag a Visual Studio 2013 van szükség. [Az Azure SDK for Visual Studio 2013][AzureSDKVs2013]
2. Hello Webplatform-telepítő ablakában kattintson **telepítése** és hello a telepítés folytatásához.

Egy mobil böngésző emulátor is szüksége lesz. Hello következő működnek:

* A böngésző emulátor [Internet Explorer 11 F12 fejlesztői eszközök] [ EmulatorIE11] (az összes mobil böngésző képernyőképeket szerepel). A Windows Phone 8, Windows Phone 7 és Apple iPad felhasználói ügynök karakterlánca készletek rendelkezik.
* A böngésző emulátor [Google Chrome DevTools][EmulatorChrome]. Számos Android-eszközök, valamint Apple iPhone, Apple iPad és Amazon Kindle tűz készletet tartalmaz. Azt is emulálja touch események.
* [Opera Mobilemulátoron][EmulatorOpera]

A Visual Studio-projektek C\# forráskód elérhető tooaccompany ebben a témakörben vannak:

* [Alapszintű projekt letöltése][StarterProject]
* [Projekt letöltése befejeződött][CompletedProject]

## <a name="bkmk_DeployStarterProject"></a>Hello alapszintű projekt tooan Azure webalkalmazás telepítése
1. Töltse le a hello konferencia-listát alkalmazás [alapszintű projekt][StarterProject].
2. Ezután a Windows Intézőt, kattintson a jobb gombbal a hello letöltött ZIP-fájl, és válassza a *tulajdonságok*.
3. A hello **tulajdonságok** párbeszédpanelen válassza ki a hello **Unblock** gombra. (Blokkolásának feloldása megakadályozza, hogy a biztonsági figyelmeztetést, amely akkor fordul elő, amikor megpróbál toouse egy *.zip* hello webről letöltött fájl.)
4. Kattintson a jobb gombbal a hello ZIP-fájl, és válassza ki **összes kibontása** hello fájl kibontásához. 
5. A Visual Studióban nyissa meg a hello *C#\Mvc5Mobile.sln* fájlt.
6. A Megoldáskezelőben kattintson a jobb gombbal a projekt hello, és kattintson a **közzététel**.
   
   ![][DeployClickPublish]
7. A webhely közzététele kattintson **Microsoft Azure App Service**.
   
   ![][DeployClickWebSites]
8. Ha még nem jelentkezett be Azure, kattintson a **vegyen fel egy fiókot**.
   
   ![][DeploySignIn]
9. Hajtsa végre a hello kér toolog be Azure-fiókjába.
10. App Service párbeszédpanelen hello most meg kell jelennie, aláírt. Kattintson az **Új** lehetőségre.
    
    ![][DeployNewWebsite]  
11. A hello **webalkalmazásnév** mezőben adja meg egy egyedi alkalmazás nevének előtagja. A webalkalmazás teljesen minősített neve lesz  *&lt;előtag >*. azurewebsites.net. Válassza ki vagy adjon meg egy új erőforráscsoport neve az is, **erőforráscsoport**. Kattintson a **új** toocreate egy új App Service-csomagot.
    
    ![][DeploySiteSettings]
12. Hello új App Service-csomag konfigurálása és **OK**. 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. Vissza a hello létrehozása az App Service párbeszédpanelen kattintson **létrehozása**.
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. Hello Azure-erőforrások létrehozása után hello webhely közzététele párbeszédpanelen beírja az új alkalmazás hello-beállítások. Kattintson a **Publish** (Közzététel) gombra.
    
    ![][DeployPublishSite]
    
    Miután a Visual Studio befejezi a közzétételi hello alapszintű projekt toohello Azure web app, hello asztali böngésző megnyitja toodisplay hello élő webalkalmazását.
15. Indítsa el a mobil böngésző emulátor, és hello konferencia alkalmazás hello URL-Címének másolása (*<prefix>*. azurewebsites.net) történő hello emulátor, majd kattintson a jobb felső gombra, és válassza ki **kódcímkeKeressemeg**. Ha az Internet Explorer 11 hello az alapértelmezett böngészőt használ, csupán be kell tootype `F12`, majd `Ctrl+8`, majd módosítsa túl hello browser-profilt**Windows Phone**. Az alábbi képen láthatók hello *AllTags* nézet álló módban (válasszon **kódcímke Tallózás**).
    
    ![][AllTags]

> [!TIP]
> Visual Studio az MVC 5 alkalmazás megoldhassuk, amíg a webes alkalmazás tooAzure közzéteheti újra tooverify hello élő webalkalmazását-ről a mobil böngésző vagy a böngésző emulátor.
> 
> 

hello megjelenítési akkor nagyon olvasható mobil eszközön. Néhány hello vizuális hatások hello rendszerindítási CSS keretrendszer által alkalmazott már is látható.
Kattintson a hello **ASP.NET** hivatkozásra.

![][SessionsByTagASP.NET]

ASP.NET címke nézet hello nagyítás felszerelt toohello képernyő, amely rendszerindítási automatikusan elvégzi az Ön. Azonban a nézet toobetter színből hello mobil böngésző javíthatja. Például hello **dátum** oszlop nehezen olvasható. Hello oktatóanyag későbbi részében módosítani fogjuk hello *AllTags* toomake megtekintése mobilbarát azt.

## <a name="bkmk_bootstrap"></a>A rendszerindítási CSS-keretrendszer
Új az MVC 5 hello sablon rendszer beépített rendszer-indításkori támogatja. Hogyan azonnal javítja hello különböző nézeteket az alkalmazás már láthatta. Például hello navigációs sáv felső hello esetén automatikusan összecsukható hello böngésző szélesség kisebb. Hello a böngésző asztali próbálja hello böngészőablak átméretezése, és hogyan hello navigációs sáv változása a Megjelenés és működés. Ez a rendszerindítási beépített hello rugalmas Webtervezés.

toosee hogyan hello webalkalmazás lenne nélkül rendszerindítási, nyissa meg a *App\_Start\\BundleConfig.cs* és hello sorokat tartalmazó megjegyzésbe *bootstrap.js* és *bootstrap.css*. hello következő kód bemutatja, hello hello utolsó két állapotkimutatások `RegisterBundles` metódus hello módosítás után:

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

Nyomja le az `Ctrl+F5` toorun hello alkalmazás.

Megfigyelheti, hogy hello összecsukható navigációs sáv most csak egy szokásos rendezetlen lista. Kattintson a **kódcímke Tallózás** újra, majd kattintson a **ASP.NET**.
Hello mobilemulátoron nézetben tekintheti meg most, hogy már nem nagyítás felszerelt toohello képernyő, és kell rendelés toosee hello jobb oldalán hello tábla oldalirányba görgessen.

![][SessionsByTagASP.NETNoBootstrap]

Visszavonja a módosításokat, és frissítse a hello mobil böngésző tooverify hello mobilbarát megjelenítési helyreállt.

Rendszerindítási nincs meghatározott tooASP.NET MVC 5, és veheti hasznát ezeket a szolgáltatásokat bármely webalkalmazásban. De most épített ASP.NET MVC 5 projektsablon, így az MVC 5-webalkalmazás kihasználhatja a rendszerindítás alapértelmezés szerint.

A rendszerindítási kapcsolatos további információkért lásd a toothe [rendszerindítási] [ BootstrapSite] hely.

A következő szakaszban hello látni fogja, hogyan tooprovide mobile-böngésző bizonyos nézeteket.

## <a name="bkmk_overrideviews"></a>Bírálja felül a hello nézetek, elrendezés és részleges nézetek
Általában az egyes mobil böngésző vagy adott böngészők mobilböngészők (beleértve a elrendezések és a részleges nézetek) nézeten felülbírálható. tooprovide egy mobileszköz-specifikus megtekintéséhez másolja át a nézet fájlt, és adja hozzá *. Mobile* toohello fájl nevét. Például egy mobileszköz toocreate *Index* másolhatja nézet, *nézetek\\Home\\Index.cshtml* való *nézetek\\kezdőlap\\ Index.Mobile.cshtml*.

Ebben a szakaszban létre fog hozni egy mobileszköz-specifikus fájlt.

toostart, másolása *nézetek\\megosztott\\\_Layout.cshtml* való *nézetek\\megosztott\\\_Layout.Mobile.cshtml* . Nyissa meg  *\_Layout.Mobile.cshtml* , és módosítsa a hello cím **MVC5 alkalmazás** túl**MVC5-alkalmazás (mobil)**.

Az egyes `Html.ActionLink` hello navigációs sáv hívja, akkor távolítsa el a "Tallózás alapja" minden hivatkozás *ActionLink*. hello következő kód bemutatja befejeződött hello `<ul class="nav navbar-nav">` hello mobil elrendezés fájl címke.

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

Másolás hello *nézetek\\Home\\AllTags.cshtml* fájl *nézetek\\Home\\AllTags.Mobile.cshtml*. Nyissa meg hello új fájlt, és módosítsa a `<h2>` "Címkék" elem túl "címkék (M)":

    <h2>Tags (M)</h2>

Keresse meg a toohello Címkék lap egy asztali böngészőt használ, és a mobil böngésző emulator használatával. hello mobil böngésző emulátor hello két végrehajtott változtatásokat mutatja (a cím hello  *\_Layout.Mobile.cshtml* és hello cím és *AllTags.Mobile.cshtml*).

![][AllTagsMobile_LayoutMobile]

Ezzel ellentétben nem változott hello asztali megjelenítési (a címei  *\_Layout.cshtml* és *AllTags.cshtml*).

![][AllTagsMobile_LayoutMobileDesktop]

## <a name="bkmk_browserviews"></a>Böngésző-specifikus nézetek létrehozása
Továbbá toomobile- és asztali-specifikus nézetek, létrehozhat egy egyedi böngésző nézetek. Például létrehozhat nézeteket, amelyek a hello iPhone vagy hello Android böngészőhöz. Ebben a szakaszban létrehozunk egy elrendezés hello iPhone-böngésző és egy iPhone verzióját hello *AllTags* nézet.

Nyissa meg hello *Global.asax* fájlt, és adja hozzá a következő kód toohello alsó részén hello a `Application_Start` metódust.

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

Ez a kód határozza meg egy új, "iPhone", amely az egyes bejövő kérelmek elleni megfeleltetésének nevű megjelenítési mód. Ha hello bejövő kérelem megfelel a következő feltételt (ha hello felhasználói ügynök tartalmaz hello karakterlánc "iPhone") megadott, az ASP.NET MVC nézetek, amelynek a neve tartalmazza a "iPhone" utótag fogja keresni.

> [!NOTE]
> Ha mobil böngésző-specifikus hozzáadása megjelenítési módok, például iPhone és Android rendszerhez készült, túl lehet, hogy tooset hello első argumentum`0` (Beszúrás hello lista hello tetején) toomake meg arról, hogy hello böngésző-specifikus üzemmód elsőbbséget élvez hello mobil sablon (*. Mobile.cshtml). Ha hello mobil sablon hello lista hello tetején helyette, ki lesz jelölve a kívánt megjelenítési mód (hello első egyezés a wins, és megfelelő összes mobilböngészők hello mobil sablon). 
> 
> 

Hello kódban, kattintson a jobb gombbal `DefaultDisplayMode`, válassza a **megoldásához**, és válassza a `using System.Web.WebPages;`. Ezzel hozzáad egy hivatkozást toothe `System.Web.WebPages` névtér, amely akkor, ha a `DisplayModeProvider` és `DefaultDisplayMode` van meghatározva.

![][ResolveDefaultDisplayMode]

Azt is megteheti, csak kézzel is felveheti a következő sor toothe hello `using` hello fájl szakaszában.

    using System.Web.WebPages;

Hello módosítások mentéséhez. Másolás a *nézetek\\megosztott\\\_Layout.Mobile.cshtml* fájl *nézetek\\megosztott\\\_Layout.iPhone.cshtml*. Nyissa meg hello új fájlt, és módosítsa a hello cím `MVC5 Application (Mobile)` való `MVC5 Application (iPhone)`.

Másolás hello *nézetek\\Home\\AllTags.Mobile.cshtml* fájl *nézetek\\Home\\AllTags.iPhone.cshtml*. Hello új fájlba, módosítsa a hello `<h2>` elem a "címkék (M)" túl "címkét (iPhone)".

Hello alkalmazás futtatásához. Futtassa a mobil böngésző emulátor, győződjön meg arról, hogy a felhasználói ügynök értéke túl "iPhone", és keresse meg a toohello *AllTags* nézet. Ha hello emulátor használ az Internet Explorer 11 F12 fejlesztői eszközök, állítsa be az emuláció toohello következőket:

* Böngésző profil = **Windows Phone**
* Felhasználói ügynök karakterlánca = **egyéni**
* Egyéni karakterláncot = **Apple-iPhone5C1/1001.525**

hello alábbi képernyőfelvételen látható hello *AllTags* nézet megjelenítése az emulátorban, az Internet Explorer 11 F12 fejlesztői eszközök hello egyéni felhasználói ügynök karakterlánca (Ez az egy iPhone 5 C felhasználói ügynök karakterlánca).

![][AllTagsIPhone_LayoutIPhone]

Hello mobil böngészőben, válassza ki a hello **hangszórók** hivatkozásra. Mivel nincs mobil nézet (*AllSpeakers.Mobile.cshtml*), hello alapértelmezett hangszórók megtekintése (*AllSpeakers.cshtml*) használatával hello mobil elrendezés nézet megjelenítése ( *\_ Layout.Mobile.cshtml*). Az alábbi módon hello cím **MVC5-alkalmazás (mobil)** meghatározott  *\_Layout.Mobile.cshtml*.

![][AllSpeakers_LayoutMobile]

Úgy, hogy a megjelenítési belül mobil elrendezés alapértelmezett (nem mobil) nézet globálisan letilthatja `RequireConsistentDisplayMode` való `true` a hello *nézetek\\\_ViewStart.cshtml* fájl ehhez hasonló:

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

Ha `RequireConsistentDisplayMode` értéke túl`true`, hello mobil elrendezés (*\_Layout.Mobile.cshtml*) csak mobil nézetek használatos (azaz hello űrlap a nézet fájl esetén  ***Nézet_neve** . Mobile.cshtml*). Érdemes lehet tooset `RequireConsistentDisplayMode` túl`true` Ha a mobil elrendezés és a nem mobil nézetek nem működik. hello az alábbi képernyőfelvételen hogyan hello *hangszórók* lap Renderelés mikor `RequireConsistentDisplayMode` értéke túl`true` (nélkül hello "(mobil)" hello felső navigációs sáv hello karakterlánc).

![][AllSpeakers_LayoutMobileOverridden]

Adott nézetben konzisztens megjelenítési mód beállításával letilthatja `RequireConsistentDisplayMode` túl`false` hello nézet fájlban. A következő kódban a hello *nézetek\\Home\\AllSpeakers.cshtml* beállítása fájl `RequireConsistentDisplayMode` túl`false`:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

Ebben a szakaszban is láttuk hogyan toocreate mobil elrendezések és a nézetek, és hogyan toocreate elrendezések és konkrét eszközöket, többek között nézeteket hello iPhone.
Azonban hello fő hello rendszerindítási CSS keretrendszer előnye a megfelelően rugalmas elrendezést, ami azt jelenti, hogy egyetlen stíluslap asztali, telefon és táblagép böngészők toocreate egy egységes megjelenést és működést alkalmazható. Hello a következő szakaszban láthatja, hogyan tooleverage Bootstrap toocreate mobilbarát nézetek.

## <a name="bkmk_Improvespeakerslist"></a>Hello hangszórók lista javítása
Ekkor csak a böngészőben, hello *hangszórók* nézet olvasható, de hello hivatkozások kicsi, és nehéz tootap mobil eszközön. Ebben a szakaszban meg kell győződnie hello *AllSpeakers* nézet mobilbarát, amely nagy, könnyen koppintson mutató hivatkozások megjelennek, és tartalmazza a keresési mezőbe tooquickly hangszórók található.

Használhatja a rendszerindítási hello [csatolt csoportja] [ linked list group] stílusbeállításokat hello javítására *hangszórók* nézet. A *nézetek\\Home\\AllSpeakers.cshtml*, hello hello Razor fájl tartalmának lecserélése hello kódot.

     @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker }, new { @class = "list-group-item" })
        }
    </div>

Hello `class="list-group"` hello attribútum `<div>` címke alkalmazza a rendszer-indításkori lista stíluselemekkel és hello `class="input-group-item"` attribútum Bootstrap lista stílusbeállításokat tooeach hivatkozás vonatkozik.

Frissítse a hello mobil böngészőt. hello nézet néz frissítése:

![][AllSpeakersFixed]

hello rendszerindítási [csatolt csoportja] [ linked list group] stílusbeállításokat teszi hello teljes csoportban minden hivatkozás kattintható, amely sok jobb felhasználói élményt. Váltás toothe asztali nézetben, és tekintse meg az hello egységes megjelenést és működést.

![][AllSpeakersFixedDesktop]

Hello mobil böngésző nézet javult, bár nehezen hello hangszórók hosszú listáját. Rendszerindítási nem biztosít egy keresési szűrő funkciót az a-kész, de néhány sornyi kódot is hozzáadhat. Akkor lesz először a keresési mezőbe toohello nézet hozzáadása, majd hello hello szűrőfüggvény JavaScript-kódot a fájlkiszolgálófürtöt. A *nézetek\\Home\\AllSpeakers.cshtml*, vegye fel a \<űrlap\> címke után hello \<h2\> címke, a lent látható módon:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <form class="input-group">
        <span class="input-group-addon"><span class="glyphicon glyphicon-search"></span></span>
        <input type="text" class="form-control" placeholder="Search speaker">
    </form>
    <br />
    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class = "list-group-item" })
        }
    </div>

Figyelje meg, hogy hello `<form>` és `<input>` mindkét címkék hello alkalmazott Bootstrap stílusok toothem rendelkezik. Hello `<span>` elem hozzáadása a rendszerindítási [glyphicon] [ glyphicon] toothe keresőmezőbe.

A hello *parancsfájlok* mappát, adja hozzá a JavaScript-fájl neve *filter.js*. Nyissa meg hello fájlt, és illessze be a kódot bele a következő hello:

    $(function () {

        // reset hello search form when hello page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up hello events toohello <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with hello typed text and hide others
            for (var i = 0; i < items.length; i++) {
                var val = items[i].text.toLowerCase();
                val = val.substring(0, searchtxt.length);
                if (val == searchtxt) {
                    $(items[i]).show();
                }
                else {
                    $(items[i]).hide();
                }
            }
        });
    });

A regisztrált kötegek tooinclude filter.js kell. Nyissa meg *App\_Start\\BundleConfig.cs* , és módosítsa az első hello csomagok. Módosítsa az első `bundles.Add` utasítás (a hello **jquery** csomagot) tooinclude *parancsfájlok\\filter.js*, az alábbiak szerint:

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

Hello **jquery** hello alapértelmezés szerint már megjelenítve a nyaláb  *\_elrendezés* nézet. Később, hello használhatja ugyanazt a JavaScript-kód tooapply a szűrő funkció tooother nézetek.

Frissítse a hello mobil böngésző, és nyissa meg toohello *AllSpeakers* nézet. A keresési mezőbe írja be az "sc". hello hangszórók lista most szűri az tooyour keresési karakterlánc alapján történik.

![][AllSpeakersFixedSearchBySC]

## <a name="bkmk_improvetags"></a>Címkelista hello javítása
Például a hello *hangszórók* megtekintéséhez hello *címkék* nézet olvasható, de hello hivatkozások kis- és nehéz a mobileszköz tootap. Ezt úgy javíthatja ki hello *címkék* nézet hello azonos módon hello megoldása *hangszórók* megtekintése, ha korábban, de hello következőre leírt hello kódmódosításokat `Html.ActionLink` metódus szintaxist  *Nézetek\\Home\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

hello frissítésének asztali böngészőn megkeresi az alábbiak szerint:

![][AllTagsFixedDesktop]

És hello frissülnek mobil böngésző megkeresi az alábbiak szerint: 

![][AllTagsFixed]

> [!NOTE]
> Észlel bennük hello eredeti lista formázást van továbbra is a mobil böngésző hello és milyen történt tooyour töltött Bootstrap stílusbeállításokat wonder, ez az összetevő a korábbi művelet toocreate mobil adott nézetek. Most, hogy hello rendszerindítási CSS keretrendszer toocreate rugalmas Webtervezés használ, lépjen a head, és távolítsa el a mobileszköz-specifikus és nézeteket hello mobile-specifikus elrendezés. Ezzel végzett, hello frissíteni mobil böngésző hello Bootstrap stílusbeállításokat fognak megjelenni.
> 
> 

## <a name="bkmk_improvedates"></a>Hello dátumok lista javítása
Javíthatja a hello *dátumok* megtekintése, például hogy továbbfejlesztett hello *hangszórók* és *címkék* megtekinti hello kódmódosításokat korábban, de a következő hello leírt használatakor`Html.ActionLink` metódus szintaxist *nézetek\\Home\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

A frissített mobil böngésző nézet jelenik meg:

![][AllDatesFixed]

Tovább növelhető a hello *dátumok* nézet hello dátum-idő értékek dátum szerint rendezésével. Ezt megteheti a rendszerindítási hello [panelek] [ panels] frizurakészítő. Cserélje le a hello hello tartalmát *nézetek\\Home\\AllDates.cshtml* fájl a következő kóddal:

    @model IEnumerable<DateTime>

    @{
        ViewBag.Title = "All Dates";
    }

    <h2>Dates</h2>

    @foreach (var dategroup in Model.GroupBy(x=>x.Date))
    {
        <div class="panel panel-primary">
            <div class="panel-heading">
                @dategroup.Key.ToString("ddd, MMM dd")
            </div>
            <div class="panel-body list-group">
                @foreach (var date in dategroup)
                {
                    @Html.ActionLink(date.ToString("h:mm tt"), 
                                     "SessionsByDate", 
                                     new { date }, 
                                     new { @class = "list-group-item" })
                }
            </div>
        </div>
    }

Ez a kód létrehoz egy különálló `<div class="panel panel-primary">` címke az egyes különálló dátumának hello listában, és használja hello [csatolt csoportja] [ linked list group] számára a megfelelő csatolja azt korábban. Nyilvántartásunk szerint, amikor a kód lefut hello mobil böngésző itt található:

![][AllDatesFixed2]

Kapcsoló toohello asztali böngészőn. Ebben az esetben Megjegyzés hello egységes megjelenést.

![][AllDatesFixed2Desktop]

## <a name="bkmk_improvesessionstable"></a>Hello SessionsTable nézet javítása
Ebben a szakaszban meg kell győződnie hello *SessionsTable* mobilbarát további megtekintése. Ez a változás szélesebb körű hello korábbi módosítások.

Hello mobil böngészőben, koppintson a hello **címke** gombra, majd adja meg `asp` be a keresőmezőbe.

![][AllTagsFixedSearchByASP]

Koppintson a hello **ASP.NET** hivatkozásra.

![][SessionsTableTagASP.NET]

Ahogy látja, hello megjelenítési táblaként, amely jelenleg tervezett toobe hello asztali böngészőben megtekintett van formázva. Azonban egy kicsit nehéz tooread mobil böngésző. toofix a, nyissa meg *nézetek\\Home\\SessionsTable.cshtml* és majd a fájl tartalmát hello cserélje le a következő kód hello:

    @model IEnumerable<Mvc5Mobile.Models.Session>

    <h2>@ViewBag.Title</h2>

    <div class="container">
        <div class="row">
            @foreach (var session in Model)
            {
                <div class="col-md-4">
                    <div class="list-group">
                        @Html.ActionLink(session.Title, 
                                         "SessionByCode", 
                                         new { session.Code }, 
                                         new { @class="list-group-item active" })
                        <div class="list-group-item">
                            <div class="list-group-item-text">
                                @Html.Partial("_SpeakersLinks", session)
                            </div>
                            <div class="list-group-item-info">
                                @session.DateText
                            </div>
                            <div class="list-group-item-info small hidden-xs">
                                @Html.Partial("_TagsLinks", session)
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    </div>

hello kód 3 dolgot hajtja végre:

* használja a rendszerindítási hello [egyéni csatolt csoportja] [ custom linked list group] tooformat hello munkamenetadatai függőleges, így ezt az információt nem olvasható a mobil böngészőt (osztályok, mint a lista-csoport-elem-szövege)
* hello vonatkozik [rács rendszer] [ grid system] toothe elrendezés, így hello munkamenetben elemek vízszintes a hello asztali böngészőn kívül függőleges hello mobil böngészőben (hello oszlop-md-4 osztály használatával)
* felhasználási hello [rugalmas segédprogramok] [ responsive utilities] hello munkamenet címkék (hello rejtett xs osztály használatával) hello mobil böngészőben megtekintve elrejtése

A cím hivatkozás toogo toohello megfelelő munkamenet is megtekintheti. az alábbi képen hello hello kódmódosításokat tükrözi.

![][FixedSessionsByTag]

hello Bootstrap rács rendszer, amely automatikusan alkalmazza a munkamenetek függőleges böngészőben hello mobil rendezi. Figyelje meg is, hogy hello címkék nem láthatók. Kapcsoló toohello asztali böngészőn.

![][SessionsTableFixedTagASP.NETDesktop]

Hello asztali böngészőben figyelje meg, hogy hello címkék jelennek meg. Ezenkívül látható, hogy a hello Bootstrap rács rendszer alkalmazott hello munkamenet elemek két oszlop rendezése. Ha nagyítása a böngészőben, látni fogja, hogy a hello elrendezéssel toothree oszlopok megváltozik.

## <a name="bkmk_improvesessionbycode"></a>Hello SessionByCode nézet javítása
Végezetül hello fogjuk kijavítani *SessionByCode* toomake megtekintése mobilbarát azt.

Hello mobil böngészőben, koppintson a hello **címke** gombra, majd adja meg `asp` be a keresőmezőbe.

![][AllTagsFixedSearchByASP]

Koppintson a hello **ASP.NET** hivatkozásra. Hello ASP.NET címke munkamenetek jelennek meg.

![][FixedSessionsByTag]

Válassza ki a hello **létrehozása egyetlen lap alkalmazást az ASP.NET és az AngularJS** hivatkozásra.

![][SessionByCode3-644]

hello alapértelmezett asztali nézetben megfelelően működik, de könnyen javíthatja a hello tekintse meg a bizonyos rendszerindítási GUI-összetevők.

Nyissa meg *nézetek\\Home\\SessionByCode.cshtml* és hello tartalma cserélje le a következő markup hello:

    @model Mvc5Mobile.Models.Session

    @{
        ViewBag.Title = "Session details";
    }
    <h3>@Model.Title (@Model.Code)</h3>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Speakers
        </div>
        @foreach (var speaker in Model.Speakers)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class="panel-body" })
        }
    </div>

    <p>@Model.Abstract</p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Tags
        </div>
        @foreach (var tag in Model.Tags)
        {
            @Html.ActionLink(tag, 
                             "SessionsByTag", 
                             new { tag }, 
                             new { @class = "panel-body" })
        }
    </div>

hello új markup tooimprove hello mobil nézet frizurakészítő Bootstrap panelek használja. 

Frissítse a hello mobil böngészőt. hello példánycsoportokat módosításoknak hello kód csak végrehajtott:

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a>Burkolja és felülvizsgálata
Ez az oktatóanyag azt mutatja, hogy hogyan toouse ASP.NET MVC 5 toodevelop mobilbarát webes alkalmazásokhoz. Ezek a következők:

* Az ASP.NET MVC 5 alkalmazás tooan App Service-webalkalmazások telepítése
* A rendszerindítási toocreate az MVC 5 alkalmazás rugalmas webes formátum használata
* Bírálja felül a elrendezését, a nézeteket és a részleges nézetek globálisan és az egyes nézet
* Vezérlő elrendezése és a részleges felülbírálása kényszerítési használatával a `RequireConsistentDisplayMode` tulajdonság
* Bizonyos böngészők, például hello iPhone böngésző célzó nézetek létrehozása
* Alkalmazza a rendszer-indításkori stílusbeállításokat Razor-kódban

## <a name="see-also"></a>Lásd még:
* [9-es alapelvek webes rugalmas kialakítás](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* [Rendszerindítási][BootstrapSite]
* [A rendszerindítási hivatalos blogja][Official Bootstrap Blog]
* [Twitter Bootstrap oktatóprogram oktatóanyag Köztársaság][Twitter Bootstrap Tutorial from Tutorial Republic]
* [hello rendszerindítási Playground][hello Bootstrap Playground]
* [W3C javaslat mobil webes alkalmazás gyakorlati tanácsok][W3C Recommendation Mobile Web Application Best Practices]
* [W3C jelölt javaslat media lekérdezések][W3C Candidate Recommendation for media queries]

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Internal Links -->
[Deploy hello starter project tooan Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override hello Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve hello Speakers List]: #bkmk_Improvespeakerslist
[Improve hello Tags List]: #bkmk_improvetags
[Improve hello Dates List]: #bkmk_improvedates
[Improve hello SessionsTable View]: #bkmk_improvesessionstable
[Improve hello SessionByCode View]: #bkmk_improvesessionbycode

<!-- External Links -->
[Visual Studio Express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web
[Visual Studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs
[AzureSDKVs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409
[Fiddler]: http://www.fiddler2.com/fiddler2/
[EmulatorIE11]: http://msdn.microsoft.com/library/ie/dn255001.aspx
[EmulatorChrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation
[EmulatorOpera]: http://www.opera.com/developer/tools/mobile/
[StarterProject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409
[CompletedProject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409
[BootstrapSite]: http://getbootstrap.com/
[WebPIAzureSdk23NetVS13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png
[linked list group]: http://getbootstrap.com/components/#list-group-linked
[glyphicon]: http://getbootstrap.com/components/#glyphicons
[panels]: http://getbootstrap.com/components/#panels
[custom linked list group]: http://getbootstrap.com/components/#list-group-custom-content
[grid system]: http://getbootstrap.com/css/#grid
[responsive utilities]: http://getbootstrap.com/css/#responsive-utilities
[Official Bootstrap Blog]: http://blog.getbootstrap.com/
[Twitter Bootstrap Tutorial from Tutorial Republic]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/
[hello Bootstrap Playground]: http://www.bootply.com/
[W3C Recommendation Mobile Web Application Best Practices]: http://www.w3.org/TR/mwabp/
[W3C Candidate Recommendation for media queries]: http://www.w3.org/TR/css3-mediaqueries/

<!-- Images -->
[DeployClickPublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png
[MobileHomePage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png
[FixedSessionsByTag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png
[AllTags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png
[SessionsByTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png
[SessionsByTagASP.NETNoBootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png
[AllTagsMobile_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png
[AllTagsMobile_LayoutMobileDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png
[ResolveDefaultDisplayMode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png
[AllTagsIPhone_LayoutIPhone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png
[AllSpeakers_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png
[AllSpeakers_LayoutMobileOverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png
[AllSpeakersFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png
[AllSpeakersFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png
[AllSpeakersFixedSearchBySC]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png
[AllTagsFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[AllTagsFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png
[AllDatesFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png
[AllDatesFixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png
[AllDatesFixed2Desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png
[AllTagsFixedSearchByASP]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png
[SessionsTableTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png
[SessionsTableFixedTagASP.NETDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png
[SessionByCode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png
[SessionByCodeFixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png

