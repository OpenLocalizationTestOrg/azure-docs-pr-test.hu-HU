---
title: "Az ASP.NET MVC 5 mobil webalkalmazás az Azure App Service telepítése"
description: "Ez az oktatóanyag útmutatást ad meg a webes alkalmazás telepítése az Azure App Service ASP.NET MVC 5 webalkalmazás mobil szolgáltatásainak használata."
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
ms.openlocfilehash: c98e9b485c52a82e5be5c0f6b0b67912d1e890b9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a>Az ASP.NET MVC 5 mobil webalkalmazás az Azure App Service telepítése
Ez az oktatóanyag fog tartalmazza az ASP.NET MVC 5 webes alkalmazást mobilbarát, és központilag telepítenie kell az Azure App Service alapjait. Ebben az oktatóanyagban kell [Visual Studio Express 2013 for Web alkalmazásokat] [ Visual Studio Express 2013] , vagy ha már rendelkezik, amely a Visual Studio professional kiadása. Használhat [Visual Studio 2015] , de a képernyőképek eltérőek lesznek, és az ASP.NET 4.x sablonok kell használnia.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a>Mit kell összeállítása
Ebben az oktatóanyagban az egyszerű konferencia-listát-alkalmazáshoz megadott mobil szolgáltatásokat fogja hozzáadni a [alapszintű projekt][StarterProject]. A böngésző-emulátor az Internet Explorer 11 F12 fejlesztői eszközök látható módon, akkor az alábbi képernyőfelvételen látható az ASP.NET munkamenet a kész alkalmazás.

![][FixedSessionsByTag]

Az Internet Explorer 11 F12 fejlesztői eszközök és a [Fiddler eszköz] [ Fiddler] érdekében, hogy az alkalmazás hibakeresését. 

## <a name="skills-youll-learn"></a>Megtudhatja, képességek
Az itt ismertetett témák:

* Hogyan használhatja a Visual Studio 2013 a webes alkalmazás közvetlenül a webalkalmazás közzététele az Azure App Service-ben.
* Az ASP.NET MVC 5 sablonok használatát a CSS rendszerindítási keretrendszer mobileszközökön megjelenítési javítása érdekében
* A célként megadott mobilböngészők, például az iPhone- és Android mobilalkalmazás-specifikus nézetek létrehozása
* Rugalmas nézetek (nézetek, amelyek válaszolnak az eszközön a különböző böngészők) létrehozása

## <a name="set-up-the-development-environment"></a>A fejlesztési környezet kialakítása
Állítsa be a fejlesztési környezetet telepítésével, az Azure SDK for .NET 2.5.1-es vagy újabb. 

1. .NET-keretrendszerhez készült Azure SDK telepítéséhez kattintson az alábbi hivatkozásra. Ha nincs még telepítve a Visual Studio 2013, akkor a kapcsolat által telepíti. Ez az oktatóanyag a Visual Studio 2013 van szükség. [Az Azure SDK for Visual Studio 2013][AzureSDKVs2013]
2. Kattintson a Webplatform-telepítő ablakban **telepítése** és a telepítés folytatásához.

Egy mobil böngésző emulátor is szüksége lesz. Az alábbi működnek:

* A böngésző emulátor [Internet Explorer 11 F12 fejlesztői eszközök] [ EmulatorIE11] (az összes mobil böngésző képernyőképeket szerepel). A Windows Phone 8, Windows Phone 7 és Apple iPad felhasználói ügynök karakterlánca készletek rendelkezik.
* A böngésző emulátor [Google Chrome DevTools][EmulatorChrome]. Számos Android-eszközök, valamint Apple iPhone, Apple iPad és Amazon Kindle tűz készletet tartalmaz. Azt is emulálja touch események.
* [Opera Mobilemulátoron][EmulatorOpera]

A Visual Studio-projektek C\# forráskód érhetők el ebben a témakörben kísérő:

* [Alapszintű projekt letöltése][StarterProject]
* [Projekt letöltése befejeződött][CompletedProject]

## <a name="bkmk_DeployStarterProject"></a>Az alapszintű projekt telepítése az Azure-webalkalmazás
1. Töltse le a konferencia-listát alkalmazás [alapszintű projekt][StarterProject].
2. Ezután a Windows Intézőt, kattintson a jobb gombbal a letöltött ZIP-fájl, és válassza a *tulajdonságok*.
3. Az a **tulajdonságok** párbeszédpanelen válassza ki a **Unblock** gombra. (Letiltásuk feloldására, amely akkor fordul elő, amikor próbálja használni a biztonsági figyelmeztetést megakadályozza, hogy egy *.zip* az internetről letöltött fájl.)
4. Kattintson a jobb gombbal a ZIP-fájl, és válassza ki **összes kibontása** és csomagolja ki a fájlt. 
5. A Visual Studióban nyissa meg a *C#\Mvc5Mobile.sln* fájlt.
6. A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson a **közzététel**.
   
   ![][DeployClickPublish]
7. A webhely közzététele kattintson **Microsoft Azure App Service**.
   
   ![][DeployClickWebSites]
8. Ha még nem jelentkezett be Azure, kattintson a **vegyen fel egy fiókot**.
   
   ![][DeploySignIn]
9. Kövesse a megjelenő utasításokat az Azure-fiók be tudjon jelentkezni.
10. Az App Service párbeszédpanelen most meg kell jelennie, aláírt. Kattintson az **Új** lehetőségre.
    
    ![][DeployNewWebsite]  
11. Az a **webalkalmazásnév** mezőben adja meg egy egyedi alkalmazás nevének előtagja. A webalkalmazás teljesen minősített neve lesz  *&lt;előtag >*. azurewebsites.net. Válassza ki vagy adjon meg egy új erőforráscsoport neve az is, **erőforráscsoport**. Kattintson a **új** egy új App Service-csomag létrehozásához.
    
    ![][DeploySiteSettings]
12. Az új App Service-csomag konfigurálása és **OK**. 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. Az App Service létrehozása párbeszédpanel, kattintson **létrehozása**.
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. Miután az Azure erőforrások jönnek létre, a webhely közzététele párbeszédpanelen beírja a beállításokat az új alkalmazás. Kattintson a **Publish** (Közzététel) gombra.
    
    ![][DeployPublishSite]
    
    Visual Studio befejezi a alapszintű projekt közzététele az Azure web app, ha az asztali böngésző megnyitja jelenítheti meg az élő webes alkalmazást.
15. Indítsa el a mobil böngésző emulátor, és másolja az URL-címet a konferencia alkalmazás (*<prefix>*. azurewebsites.net) azokat az emulátorban, majd kattintson a jobb felső gombra, és válassza ki **kódcímke Tallózás**. Ha az alapértelmezett böngésző használ az Internet Explorer 11, csupán be kell beírnia `F12`, majd `Ctrl+8`, majd állítsa át a böngésző-profilhoz, amellyel **Windows Phone**. Az alábbi kép a *AllTags* nézet álló módban (válasszon **kódcímke Tallózás**).
    
    ![][AllTags]

> [!TIP]
> Visual Studio az MVC 5 alkalmazás megoldhassuk, amíg az Azure-bA ismét ellenőrizze az élő webalkalmazását-ről a mobil böngésző vagy a böngésző emulátor közzéteheti a webalkalmazás.
> 
> 

A megjelenített akkor nagyon olvasható mobil eszközön. A vizuális hatások a rendszerindítási CSS-keretrendszer által alkalmazott némelyike már is látható.
Kattintson a **ASP.NET** hivatkozásra.

![][SessionsByTagASP.NET]

Az ASP.NET címke nézet Nagyítás felszerelt meg automatikusan elvégzi a rendszerindítás, a képernyőhöz. Azonban ez a nézet jobban megfeleljenek a mobil böngésző javíthatja. Például a **dátum** oszlop nehezen olvasható. Az oktatóanyag későbbi részében fogja módosítani a *AllTags* abba, hogy mobilbarát nézet.

## <a name="bkmk_bootstrap"></a>A rendszerindítási CSS-keretrendszer
Új az MVC 5 sablon rendszer beépített rendszer-indításkori támogatja. Hogyan azonnal javítja a különböző nézetek az alkalmazás már láthatta. Például a felső navigációs sáv esetén automatikusan összecsukható a böngésző szélesség kisebb. Az asztali böngészőn próbálkozzon a böngészőablak átméretezése, és hogyan a navigációs sáv változása a Megjelenés és működés. Ez a rendszerindítási beépített rugalmas Webtervezés.

Hogyan a webes alkalmazás nélkül rendszerindítási lenne megtekintéséhez nyissa meg a *App\_Start\\BundleConfig.cs* és tartalmazó sorok megjegyzésbe *bootstrap.js* és *bootstrap.css*. A következő kód bemutatja az utolsó két állapotkimutatások a `RegisterBundles` metódus a módosítás után:

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

Nyomja le az `Ctrl+F5` az alkalmazás futtatásához.

Figyelje meg, hogy a összecsukható navigációs sáv jelenleg csak egy szokásos rendezetlen lista. Kattintson a **kódcímke Tallózás** újra, majd kattintson a **ASP.NET**.
A mobilemulátoron nézetben tekintheti meg most, hogy már nincs nagyítás felszerelt a képernyőre, és kell ahhoz, hogy a tábla jobb oldalán talál oldalirányba görgessen.

![][SessionsByTagASP.NETNoBootstrap]

Visszavonja a módosításokat, és frissítse a mobil böngészőablakot győződjön meg arról, hogy a mobilbarát megjelenítési helyre lett állítva.

Rendszerindítási nem csak az ASP.NET MVC 5, és veheti hasznát ezeket a szolgáltatásokat bármely webalkalmazásban. De most épített ASP.NET MVC 5 projektsablon, így az MVC 5-webalkalmazás kihasználhatja a rendszerindítás alapértelmezés szerint.

Rendszerindítási kapcsolatos további információkért látogasson el a [rendszerindítási] [ BootstrapSite] hely.

A következő szakaszban láthatja, hogyan mobile-böngésző meghatározott nézetet biztosít.

## <a name="bkmk_overrideviews"></a>A nézetek, elrendezés és részleges nézetek felülbírálása
Általában az egyes mobil böngésző vagy adott böngészők mobilböngészők (beleértve a elrendezések és a részleges nézetek) nézeten felülbírálható. Mobileszköz-specifikus megjelenítésére szolgáló, nézet fájl másolása és hozzáadása *. Mobile* fájlneve. Ahhoz például, hogy hozzon létre egy mobile *Index* másolhatja nézet, *nézetek\\Home\\Index.cshtml* való *nézetek\\Home\\Index.Mobile.cshtml*.

Ebben a szakaszban létre fog hozni egy mobileszköz-specifikus fájlt.

Elindításához másolása *nézetek\\megosztott\\\_Layout.cshtml* való *nézetek\\megosztott\\\_Layout.Mobile.cshtml*. Nyissa meg  *\_Layout.Mobile.cshtml* , és módosítsa a cím és **MVC5 alkalmazás** való **MVC5-alkalmazás (mobil)**.

Az egyes `Html.ActionLink` a navigációs sáv hívja, akkor távolítsa el a "Tallózás alapja" minden hivatkozás *ActionLink*. A következő kód bemutatja a befejezett `<ul class="nav navbar-nav">` címkéjének a mobil fájlt.

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

Másolás a *nézetek\\Home\\AllTags.cshtml* fájl *nézetek\\Home\\AllTags.Mobile.cshtml*. Nyissa meg az új fájlt, és módosítsa a `<h2>` elem "Címkék" a "címkék (M)":

    <h2>Tags (M)</h2>

Tallózással keresse meg a Címkék lap egy asztali böngészőt használ, és a mobil böngésző emulator használatával. A mobil böngésző emulátor mutatja a két végrehajtott (a cím és  *\_Layout.Mobile.cshtml* és a cím és *AllTags.Mobile.cshtml*).

![][AllTagsMobile_LayoutMobile]

Ezzel szemben az asztal nem változott (a címei  *\_Layout.cshtml* és *AllTags.cshtml*).

![][AllTagsMobile_LayoutMobileDesktop]

## <a name="bkmk_browserviews"></a>Böngésző-specifikus nézetek létrehozása
Mobileszköz- és asztali-specifikus nézetek mellett egy egyedi böngésző nézeteket is létrehozhat. Például kimondottan az iPhone vagy az Android böngészőhöz nézeteket is létrehozhat. Ebben a szakaszban létrehozunk egy elrendezést a iPhone-böngésző és egy iPhone verzióját a *AllTags* nézet.

Nyissa meg a *Global.asax* fájlt, és adja hozzá a következő kódot alján a `Application_Start` metódust.

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

Ez a kód határozza meg egy új, "iPhone", amely az egyes bejövő kérelmek elleni megfeleltetésének nevű megjelenítési mód. Ha a bejövő kérelem megfelel a következő feltételt (Ha ez azt jelenti, hogy a felhasználói ügynök tartalmazza a karakterláncot "iPhone") megadott, az ASP.NET MVC nézetek, amelynek a neve tartalmazza a "iPhone" utótag fogja keresni.

> [!NOTE]
> Mobil böngésző-specifikus megjelenítési módok való hozzáadásakor, például iPhone-és Android, ügyeljen arra, hogy az első argumentum beállítása `0` (szúrja be a lista elején) Győződjön meg arról, hogy a böngésző-specifikus mód elsőbbséget élvez a mobil sablon (*. Mobile.cshtml). Ha a mobil sablon a lista tetején helyette, akkor lesz kiválasztva a kívánt megjelenítési mód (az első egyező wins, és a mobil sablon megegyezik a hordozható böngészők). 
> 
> 

A kódban, kattintson a jobb gombbal `DefaultDisplayMode`, válassza a **megoldásához**, és válassza a `using System.Web.WebPages;`. Ez hozzáad egy hivatkozást a `System.Web.WebPages` névtér, amely akkor, ha a `DisplayModeProvider` és `DefaultDisplayMode` van meghatározva.

![][ResolveDefaultDisplayMode]

Másik lehetőségként csak manuálisan adhat hozzá a következő sort a `using` a fájl a szakaszban.

    using System.Web.WebPages;

Mentse a módosításokat. Másolás a *nézetek\\megosztott\\\_Layout.Mobile.cshtml* fájl *nézetek\\megosztott\\\_Layout.iPhone.cshtml*. Nyissa meg az új fájlt, és módosítsa a címét, majd `MVC5 Application (Mobile)` való `MVC5 Application (iPhone)`.

Másolás a *nézetek\\Home\\AllTags.Mobile.cshtml* fájl *nézetek\\Home\\AllTags.iPhone.cshtml*. Az új fájl, módosítsa a `<h2>` "Címkék (iPhone)" a "címkék (M)" elemet.

Futtassa az alkalmazást. Futtassa a mobil böngésző emulátor, győződjön meg arról, hogy a felhasználói ügynök "iPhone" értékre van állítva, és keresse meg a *AllTags* nézet. Az emulátor használatakor az Internet Explorer 11 F12 fejlesztői eszközök konfigurálása emuláció a következőhöz:

* Böngésző profil = **Windows Phone**
* Felhasználói ügynök karakterlánca = **egyéni**
* Egyéni karakterláncot = **Apple-iPhone5C1/1001.525**

Az alábbi képernyőfelvételen látható a *AllTags* nézet megjelenítése az emulátorban, az Internet Explorer 11 F12 fejlesztői eszközök, az egyéni felhasználói ügynök karakterlánca (Ez az egy iPhone 5 C felhasználói ügynök karakterlánca).

![][AllTagsIPhone_LayoutIPhone]

A mobil böngészőben, válassza ki a **hangszórók** hivatkozásra. Mivel nincs mobil nézet (*AllSpeakers.Mobile.cshtml*), az alapértelmezett hangszórók megtekintése (*AllSpeakers.cshtml*) használata a mobil nézet megjelenítése (*\_Layout.Mobile.cshtml*). Az alábbi módon a cím **MVC5-alkalmazás (mobil)** meghatározott  *\_Layout.Mobile.cshtml*.

![][AllSpeakers_LayoutMobile]

Úgy, hogy a megjelenítési belül mobil elrendezés alapértelmezett (nem mobil) nézet globálisan letilthatja `RequireConsistentDisplayMode` való `true` a a *nézetek\\\_ViewStart.cshtml* fájl ehhez hasonló:

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

Ha `RequireConsistentDisplayMode` értéke `true`, a mobil elrendezés (*\_Layout.Mobile.cshtml*) csak mobil nézetek használatos (azaz az űrlap a nézet fájl esetén  ***Nézet_neve**. Mobile.cshtml*). Előfordulhat, hogy szeretné állítani `RequireConsistentDisplayMode` való `true` Ha a mobil elrendezés és a nem mobil nézetek nem működik. Az alábbi képernyőfelvételen az *hangszórók* lap Renderelés mikor `RequireConsistentDisplayMode` értékre van állítva `true` (nélkül a karakterlánc "(mobil)" a felső navigációs sáv).

![][AllSpeakers_LayoutMobileOverridden]

Adott nézetben konzisztens megjelenítési mód beállításával letilthatja `RequireConsistentDisplayMode` való `false` a nézet fájlban. A következő kódban a *nézetek\\Home\\AllSpeakers.cshtml* beállítása fájl `RequireConsistentDisplayMode` való `false`:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

Ebben a szakaszban is láttuk mobil elrendezések és nézetek létrehozása és elrendezések és a meghatározott eszközök, például az iPhone nézetek létrehozása.
Azonban a fő a rendszerindítási CSS-keretrendszer előnye a megfelelően rugalmas elrendezést, ami azt jelenti, hogy egyetlen stíluslap alkalmazható asztali, telefon és táblagép böngészők egy egységes megjelenést és működést létrehozásához. A következő szakaszban láthatja, hogyan használhatók ki a rendszerindítási mobilbarát nézetek létrehozásához.

## <a name="bkmk_Improvespeakerslist"></a>A hangszórók lista javítása
Ekkor csak a böngészőben, a *hangszórók* nézet olvasható, de a hivatkozások kicsi, és nehéz a mobileszköz koppintson. Ebben a szakaszban meg kell győződnie a *AllSpeakers* mobilbarát, amely nagy, könnyen koppintson mutató hivatkozások megjelennek, és tartalmazza a keresőmezőbe gyorsan hangszórók megtekintése.

Használhatja a rendszerindítási [csatolt csoportja] [ linked list group] stílusbeállításokat javítása érdekében a *hangszórók* nézet. A *nézetek\\Home\\AllSpeakers.cshtml*, cserélje le a Razor fájl tartalmát az alábbi kódra.

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

A `class="list-group"` attribútumnak a `<div>` címke alkalmazza a rendszer-indításkori lista stíluselemekkel és a `class="input-group-item"` attribútum Bootstrap lista elem stílusbeállításokat vonatkozik minden hivatkozás.

Frissítse a mobil böngészőablakot. A frissített nézet így néz ki:

![][AllSpeakersFixed]

A rendszerindítási [csatolt csoportja] [ linked list group] stílusbeállításokat lehetővé teszi a teljes csoportban kattintható, minden hivatkozás, amely sok jobb felhasználói élményt. Az asztali nézetet, és tekintse meg az egységes megjelenést és működést.

![][AllSpeakersFixedDesktop]

Bár a mobil böngésző nézet javult, akkor nehezen hangszórók listája túl hosszú. Rendszerindítási nem biztosít egy keresési szűrő funkciót az a-kész, de néhány sornyi kódot is hozzáadhat. Akkor lesz először a keresőmező a nézethez történő hozzáadáshoz, majd a szűrő függvény JavaScript kóddal a számítógéphez. A *nézetek\\Home\\AllSpeakers.cshtml*, vegye fel a \<űrlap\> címke után csak a \<h2\> címke, a lent látható módon:

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

Figyelje meg, hogy a `<form>` és `<input>` mindkét címkék rendelkezik vonatkoznak annak biztosítása érdekében a rendszer-indításkori stílusok. A `<span>` elem hozzáadása a rendszerindítási [glyphicon] [ glyphicon] a keresési mezőbe.

Az a *parancsfájlok* mappát, adja hozzá a JavaScript-fájl neve *filter.js*. Nyissa meg a fájlt, és illessze be az alábbi:

    $(function () {

        // reset the search form when the page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up the events to the <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with the typed text and hide others
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

Szükség filter.js szerepeljenek a regisztrált csomagokat. Nyissa meg *App\_Start\\BundleConfig.cs* , és módosítsa az első csomagokat. Módosítsa az első `bundles.Add` utasítás (az a **jquery** csomagot) felvenni *parancsfájlok\\filter.js*, az alábbiak szerint:

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

A **jquery** köteg már megjelenítése a alapértelmezés szerint  *\_elrendezés* nézet. Később használhatja a szűrés funkciót alkalmazandó más nézetek azonos JavaScript-kódot.

Frissítse a mobil böngészőt, és navigáljon a *AllSpeakers* nézet. A keresési mezőbe írja be az "sc". A hangszórók lista most szűri megfelelően a keresési karakterláncot.

![][AllSpeakersFixedSearchBySC]

## <a name="bkmk_improvetags"></a>A címkék lista javítása
Például a *hangszórók* nézet, a *címkék* nézet olvasható, de a hivatkozások a kis- és nehéz a mobileszköz koppintson. Ezt úgy javíthatja ki a *címkék* meg ugyanúgy megtekintheti a *hangszórók* megtekintése, ha korábban, de a következő leírt kódmódosításokat `Html.ActionLink` metódus szintaxist *nézetek\\Home\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

A frissített asztali böngészőn a következőképpen néz ki:

![][AllTagsFixedDesktop]

És a frissített mobil böngészőben a következőképpen néz ki: 

![][AllTagsFixed]

> [!NOTE]
> Ha azt észleli, hogy az eredeti lista formázás továbbra is van a mobil böngészőben, és mi történt a töltött Bootstrap stílusbeállításokat wonder, ez az összetevő a korábbi művelet mobil adott nézetek létrehozásához. Most, hogy a rendszerindítási CSS-keretrendszer használatával hozzon létre egy rugalmas Webtervezés, nyissa meg a head, és távolítsa el a mobileszköz-specifikus elrendezés és a mobileszköz-specifikus nézeteket. Ezzel végzett, a frissített mobil böngészőt a rendszer-indításkori stílusbeállításokat fognak megjelenni.
> 
> 

## <a name="bkmk_improvedates"></a>A dátumok lista javítása
Javíthatja a *dátumok* megtekintése, nagyobb, mint a *hangszórók* és *címkék* megtekinti a korábban, de a következő leírt kódmódosításokat használatakor `Html.ActionLink` metódus szintaxist *nézetek\\Home\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

A frissített mobil böngésző nézet jelenik meg:

![][AllDatesFixed]

Tovább javíthatja a *dátumok* nézet dátum-idő értékek dátum szerint rendezésével. Ezt megteheti a rendszerindítási rendelkező [panelek] [ panels] frizurakészítő. Cserélje le a tartalmát a *nézetek\\Home\\AllDates.cshtml* fájl a következő kóddal:

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

Ez a kód létrehoz egy különálló `<div class="panel panel-primary">` címkét az egyes különálló dátumának elemet a listában, és használja a [csatolt csoportja] [ linked list group] számára a megfelelő csatolja azt korábban. Ez a mobil böngésző néz ezt a kódot futtatásakor:

![][AllDatesFixed2]

Az asztali böngészőn váltani. Újra vegye figyelembe összhangban megjelenését.

![][AllDatesFixed2Desktop]

## <a name="bkmk_improvesessionstable"></a>A SessionsTable nézet javítása
Ebben a szakaszban meg kell győződnie a *SessionsTable* mobilbarát további megtekintése. Ez a változás szélesebb körű a korábbi módosítások.

A mobil böngészőben, koppintson a **címke** gombra, majd adja meg `asp` be a keresőmezőbe.

![][AllTagsFixedSearchByASP]

Koppintson a **ASP.NET** hivatkozásra.

![][SessionsTableTagASP.NET]

Ahogy látja, a megjelenítési táblaként, amely jelenleg tekinthető meg az asztali böngészőn van formázva. Azonban érdemes valamivel nehezen olvasható mobil böngésző. Nyissa meg a javításhoz *nézetek\\Home\\SessionsTable.cshtml* majd cserélje le a fájl tartalmát az alábbira:

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

A kód 3 dolgot hajtja végre:

* használja a rendszerindítási [egyéni csatolt csoportja] [ custom linked list group] formázandó a munkamenet-információk függőleges, úgy, hogy ezt az információt olvasható egy mobil böngésző (például a lista-csoport-elem szövege osztályok használatával)
* alkalmazza a [rács rendszer] [ grid system] elrendezését, így a munkamenet-elemek vízszintes az asztali böngészőn és függőlegesen a mobil böngészőben (az oszlop-md-4 osztály használatával)
* használja a [rugalmas segédprogramok] [ responsive utilities] elrejtheti a munkamenet címkék, ha a mobil böngészőben (a rejtett xs osztály használatával)

A cím mutató hivatkozásra a megfelelő munkamenet is megtekintheti. Az alábbi képen tükrözi a kód módosítására.

![][FixedSessionsByTag]

A rendszer-indításkori rács rendszer, amely automatikusan alkalmazza a munkamenetek függőlegesen a mobil böngészőben rendezi. Továbbá figyelje meg, hogy a címkék nem láthatók. Az asztali böngészőn váltani.

![][SessionsTableFixedTagASP.NETDesktop]

Az asztali böngészőn figyelje meg, hogy a címkék jelennek meg. Ezenkívül látható, hogy a rendszer-indításkori rács rendszer alkalmazott a munkamenet elemek két oszlop rendezése a. Ha nagyítása a böngészőben, látni fogja, hogy a megállapodás vált három oszlopot.

## <a name="bkmk_improvesessionbycode"></a>A SessionByCode nézet javítása
Végezetül fogjuk kijavítani a *SessionByCode* abba, hogy mobilbarát nézet.

A mobil böngészőben, koppintson a **címke** gombra, majd adja meg `asp` be a keresőmezőbe.

![][AllTagsFixedSearchByASP]

Koppintson a **ASP.NET** hivatkozásra. Az ASP.NET címke munkamenetek jelennek meg.

![][FixedSessionsByTag]

Válassza ki a **létrehozása egyetlen lap alkalmazást az ASP.NET és az AngularJS** hivatkozásra.

![][SessionByCode3-644]

Az alapértelmezett nézet megfelelően működik, de javíthatja a hely könnyen néhány rendszerindítási GUI-összetevők.

Nyissa meg *nézetek\\Home\\SessionByCode.cshtml* , és cserélje ki annak tartalmát a következő kódban:

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

Az új markup Bootstrap panelek frizurakészítő a mobil nézet javítására használja. 

Frissítse a mobil böngészőablakot. Az alábbi képen a kód végrehajtott módosítások el csak tükrözi:

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a>Burkolja és felülvizsgálata
Ez az oktatóanyag azt mutatja, hogyan használható az ASP.NET MVC 5 mobilbarát webes alkalmazások fejlesztéséhez. Ezek a következők:

* Egy App Service webalkalmazásba ASP.NET MVC 5 alkalmazás központi telepítése
* Az MVC 5-alkalmazás rugalmas webes elrendezés létrehozásához használja a rendszerindítási
* Bírálja felül a elrendezését, a nézeteket és a részleges nézetek globálisan és az egyes nézet
* Vezérlő elrendezése és a részleges felülbírálása kényszerítési használatával a `RequireConsistentDisplayMode` tulajdonság
* Bizonyos böngészők, például a iPhone böngésző célzó nézetek létrehozása
* Alkalmazza a rendszer-indításkori stílusbeállításokat Razor-kódban

## <a name="see-also"></a>Lásd még:
* [9-es alapelvek webes rugalmas kialakítás](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* [Rendszerindítási][BootstrapSite]
* [A rendszerindítási hivatalos blogja][Official Bootstrap Blog]
* [Twitter Bootstrap oktatóprogram oktatóanyag Köztársaság][Twitter Bootstrap Tutorial from Tutorial Republic]
* [A rendszer-indításkori Playground][The Bootstrap Playground]
* [W3C javaslat mobil webes alkalmazás gyakorlati tanácsok][W3C Recommendation Mobile Web Application Best Practices]
* [W3C jelölt javaslat media lekérdezések][W3C Candidate Recommendation for media queries]

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Internal Links -->
[Deploy the starter project to an Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve the Speakers List]: #bkmk_Improvespeakerslist
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode

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
[The Bootstrap Playground]: http://www.bootply.com/
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

