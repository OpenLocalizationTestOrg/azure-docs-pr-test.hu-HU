---
title: "Üzleti Azure-alkalmazás létrehozása az Azure Active Directory hitelesítési |} Microsoft Docs"
description: "Ismerje meg, az ASP.NET MVC-üzleti alkalmazások létrehozása az Azure App Service-ben, amely hitelesíti magát az Azure Active Directoryval"
services: app-service\web, active-directory
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: ad947bdb-4463-43ff-a5e3-91d9b2169b60
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 09/01/2016
ms.author: cephalin
ms.openlocfilehash: 6eadf0a521a32c5bc580908e4e4b7f4305e2bf7e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a>Üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés
Ez a cikk bemutatja, hogyan hozzon létre egy .NET-üzleti alkalmazást az [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) használatával a [hitelesítési / engedélyezési](../app-service/app-service-authentication-overview.md) szolgáltatás. Azt is bemutatja, hogyan használja a [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) lekérdezés címtáradatok az alkalmazásban.

Az Azure Active Directory-bérlőt, amelyekkel egy csak Azure-címtár is lehet. Vagy lehet [a helyszíni Active Directoryból szinkronizált](../active-directory/active-directory-aadconnect.md) egy egyszeri bejelentkezéses felhasználói élmény biztosítása, amely a helyi és távoli dolgozók létrehozásához. Ebben a cikkben az alapértelmezett címtár az Azure-fiókját.

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Mit fog létrehozni
Fog létrehozni egy egyszerű üzleti Create-olvasás-Módosítás-Törlés (CRUD) alkalmazást az App Service Web Apps, hogy nyomon követi a következő funkciókkal munkaelemek:

* Hitelesíti a felhasználókat az Azure Active Directoryban
* Lekérdezi a directory-felhasználók és csoportok használatával [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)
* Használja az ASP.NET MVC *nem hitelesítési* sablon

Ha az üzleti alkalmazás az Azure szerepköralapú hozzáférés-vezérlést (RBAC) van szüksége, tekintse meg [tovább](#next).

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Mi szükséges
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:

* Az Azure Active Directory-bérlő különböző csoportok felhasználóival
* Az Azure Active Directory-bérlőt az alkalmazások létrehozásához szükséges engedélyek
* A Visual Studio 2013 Update 4 vagy újabb verzió
* [Az Azure SDK 2.8.1-es verziójának vagy újabb verzió](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-to-azure"></a>Hozzon létre, és a webalkalmazás üzembe helyezése az Azure-bA
1. A Visual Studio eszközből a kattintson **fájl** > **új** > **projekt**.
2. Válassza ki **ASP.NET Web Application**, nevezze el a projektet, és kattintson a **OK**.
3. Válassza ki a **MVC** sablont, majd módosítsa a hitelesítési **nem hitelesítési**. Győződjön meg arról, hogy **a felhőben lévő gazdagéphez** van kiválasztva, és kattintson a **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. Az a **App Service létrehozása** párbeszédpanel, kattintson a **vegyen fel egy fiókot** (, majd **vegyen fel egy fiókot** a legördülő) próbál bejelentkezni az Azure-fiókjával.
5. Miután bejelentkezett a webalkalmazás konfigurálása. Az erőforráscsoportot és egy új App Service-csomag létrehozásához kattintson a megfelelő **új** gombra. Kattintson a **további Azure-szolgáltatások felfedezés** folytatja.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. Az a **szolgáltatások** lapra, majd  **+**  hozzáadása az alkalmazás SQL-adatbázis. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. A **SQL-adatbázis beállítása**, kattintson a **új** SQL Server-példány létrehozása.
8. A **SQL-kiszolgáló konfigurálása**, konfigurálja az SQL Server-példányt. Kattintson a **OK**, **OK**, és **létrehozása** való indítsa a alkalmazás létrehozása az Azure-ban.
9. A **Azure App Service-tevékenység**, láthatja, amikor az alkalmazás létrehozása befejeződött. Kattintson a  **közzététel &lt;* appname*> Ez a webalkalmazás most **, majd kattintson **közzététel**. 
   
    Miután befejezte a Visual Studio, a böngészőben megnyitja a közzétételi app. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a>Hitelesítés és a directory hozzáférés konfigurálása
1. Jelentkezzen be az [Azure portálra](https://portal.azure.com).
2. Kattintson a bal oldali menü **alkalmazásszolgáltatások** > **&lt;*appname*> ** > **hitelesítési / engedélyezési**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. Azure Active Directory-hitelesítés bekapcsolása kattintva **a** > **Azure Active Directory** > **Express** > **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. Kattintson a **mentése** parancsra a parancssávon.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    Ha a hitelesítési beállításainak mentése sikeresen befejeződött, próbálja navigáljon a böngészőben újra az alkalmazást. Az alapértelmezett beállítások a teljes alkalmazás-hitelesítés kényszerítéséhez. Ha még nem jelentkezett, ekkor megnyílik egy bejelentkezési képernyőt. Miután bejelentkezett, megjelenik az HTTPS által védett alkalmazáshoz. A következő lehetővé kell tenni a címtár adataihoz való hozzáférés. 
5. Keresse meg a [klasszikus portál](https://manage.windowsazure.com).
6. Kattintson a bal oldali menü **Active Directory** > **alapértelmezett címtárat** > **alkalmazások** > **&lt;*appname*> **.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    Ez az App Service hozott létre az Ön Azure Active Directory-alkalmazás engedélyezése az engedély / hitelesítési funkció.
7. Kattintson a **felhasználók** és **csoportok** győződjön meg arról, hogy egyes felhasználók és csoportok a címtárban. Ha nem, néhány teszt felhasználók és csoportok létrehozása.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. Kattintson a **konfigurálása** konfigurálnia ezt az alkalmazást.
9. Görgessen le a **kulcsok** szakaszt, és a kulcs hozzáadása egy duration kiválasztásával. Kattintson a **delegált engedélyek** válassza **címtáradatok olvasása**. 
   Kattintson a **Save** (Mentés) gombra.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. Után a beállítások lesznek mentve, görgessen a biztonsági másolatot a **kulcsok** szakaszt, és kattintson a **másolási** ügyfél kulcsának az átmásolása gombra. 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > Ha most elhagyni a lapot, nem fogja tudni a jövőben esetleg újra az ügyfél hívóbetű.
    > 
    > 
11. A következő kell konfigurálni a webes alkalmazás ezt a kulcsot. Jelentkezzen be a [Azure erőforrás-kezelő](https://resources.azure.com) az Azure-fiókjába.
12. Kattintson a lap tetején **olvasási/írási** módosításokat az Azure erőforrás-kezelőben.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. A hitelesítési beállítások, az alkalmazás előfizetések helyen található >  **&lt;* subscriptionname*> ** > **resourceGroups** > **&lt;*resourcegroupname*> ** > **szolgáltatók** > **Microsoft.Web** > **helyek** > **&lt;*appname*> ** > **config** > **authsettings**.
14. Kattintson a **Szerkesztés** gombra.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. A szerkesztő ablakban állítsa a `clientSecret` és `additionalLoginParams` tulajdonságok az alábbiak szerint.
    
        ...
        "clientSecret": "<client key from the Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. Kattintson a **Put** a módosítások legfelül.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. Mostantól, ha az engedélyezési jogkivonatot az Azure Active Directory Graph API eléréséhez teszteléséhez egyszerűen lépjen  **https://&lt;*appname*>.azurewebsites.net/.auth/me** a böngészőben. Ha Ön mindent megfelelően konfigurálva, megjelenik a `access_token` a JSON-NÁ válaszul tulajdonság.
    
    A `~/.auth/me` URL-címet felügyeli App Service hitelesítés / engedélyezés a információkkal kapcsolatos a hitelesített munkamenet. További információkért lásd: [hitelesítési és engedélyezési az Azure App Service](../app-service/app-service-authentication-overview.md).
    
    > [!NOTE]
    > A `access_token` rendelkezik egy olyan lejárati időt. Azonban App Service hitelesítés / engedélyezés a token frissítési funkciókat kínál `~/.auth/refresh`. Azt használatáról további információk: [szolgáltatás Token Alkalmazásáruházból](https://cgillum.tech/2016/03/07/app-service-token-store/).
    > 
    > 

A következő fogja végrehajtani valamit a címtáradatok hasznos.

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-to-your-app"></a>Üzleti funkciók hozzáadása az alkalmazáshoz
Ezután hozzon létre egy egyszerű CRUD munkahelyi elemek Rendszerleállási események követése.  

1. A ~\Models mappában hozzon létre egy WorkItem.cs nevű osztály fájlt, és cserélje le `public class WorkItem {...}` az alábbi kódra:
   
     használatával System.ComponentModel.DataAnnotations;
   
     WorkItem {nyilvános osztály
   
         [Key]
         public int ItemID { get; set; }
         public string AssignedToID { get; set; }
         public string AssignedToName { get; set; }
         public string Description { get; set; }
         public WorkItemStatus Status { get; set; }
     }
   
     nyilvános enum WorkItemStatus {
   
         Open,
         Investigating,
         Resolved,
         Closed
     }
2. A elérhetővé az új modell a állványok logikát a Visual Studio projekt felépítéséhez.
3. Egy új generált elem hozzáadása a `WorkItemsController` a ~\Controllers mappába (kattintson a jobb gombbal **tartományvezérlők**, mutasson a **Hozzáadás**, és válassza ki **új generált elem**). 
4. Válassza ki **MVC 5 Controller nézetek, entitás-keretrendszer használatával** kattintson **Hozzáadás**.
5. Válassza ki a létrehozott, majd kattintson a modell  **+**  , majd **Hozzáadás** hozzáadása egy adatkörnyezetet, és kattintson **Hozzáadás**.
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. (Az automatikusan generált elem) ~\Views\WorkItems\Create.cshtml, keresse meg a `Html.BeginForm` segédmetódus és a következő kijelölt módosításokat:  
   
   <pre class="prettyprint">
   @model WebApplication1.Models.WorkItem
   
   @{
    ViewBag.Title = &quot;Create&quot;;
   }
   
   &lt;h2&gt;Create&lt;/h2&gt;
   
   @using (Html.BeginForm(<mark>&quot;Create&quot;, &quot;WorkItems&quot;, FormMethod.Post, new { id = &quot;main-form&quot; }</mark>)) 
   {
    @Html.AntiForgeryToken()
   
    &lt;div class=&quot;form-horizontal&quot;&gt;
        &lt;h4&gt;WorkItem&lt;/h4&gt;
        &lt;hr /&gt;
        @Html.ValidationSummary(true, &quot;&quot;, new { @class = &quot;text-danger&quot; })
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToID, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToID, new { htmlAttributes = new { @class = &quot;form-control&quot;<mark>, @type = &quot;hidden&quot;</mark> } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToID, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToName, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToName, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToName, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Description, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.Description, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.Description, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EnumDropDownListFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;form-control&quot; })
                @Html.ValidationMessageFor(model =&gt; model.Status, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            &lt;div class=&quot;col-md-offset-2 col-md-10&quot;&gt;
                &lt;input type=&quot;submit&quot; value=&quot;Create&quot; class=&quot;btn btn-default&quot;<mark> id=&quot;submit-button&quot;</mark> /&gt;
            &lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;
   }
   
   &lt;div&gt;
    @Html.ActionLink(&quot;Back to List&quot;, &quot;Index&quot;)
   &lt;/div&gt;
   
   @section Scripts {
    @Scripts.Render(&quot;~/bundles/jqueryval&quot;)
    <mark>&lt;script&gt;
        // People/Group Picker Code
        var maxResultsPerPage = 14;
        var input = document.getElementById(&quot;AssignedToName&quot;);
   
        // Access token from request header, and tenantID from claims identity
        var token = &quot;@Request.Headers[&quot;X-MS-TOKEN-AAD-ACCESS-TOKEN&quot;]&quot;;
        var tenant =&quot;@(System.Security.Claims.ClaimsPrincipal.Current.Claims
                        .Where(c => c.Type == &quot;http://schemas.microsoft.com/identity/claims/tenantid&quot;)
                        .Select(c => c.Value).SingleOrDefault())&quot;;
   
        var picker = new AadPicker(maxResultsPerPage, input, token, tenant);
   
        // Submit the selected user/group to be asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   Vegye figyelembe, hogy `token` és `tenant` használják a `AadPicker` objektum az Azure Active Directory Graph API-hívások indítása. Fogja hozzáadni `AadPicker` később.     
   
   > [!NOTE]
   > Is kaphat `token` és `tenant` az ügyféloldali `~/.auth/me`, de ez egy további kiszolgálóra hívás lenne. Példa:
   > 
   > $.ajax ({adattípus: "json" URL-címe: "/.auth/me", a sikeres: függvény (adatok) {var token adatok [0] = .access_token; var bérlői adatok [0] .user_claims .find = (c = > c.typ === "http://schemas.microsoft.com/identity/claims/tenantid") .val;}});
   > 
   > 
7. A módosítást ~ \Views\WorkItems\Edit.cshtml.
8. A `AadPicker` objektum hozzáadása a projekthez szükséges parancsfájl van megadva. Kattintson jobb gombbal a ~\Scripts mappa **Hozzáadás**, és kattintson a **JavaScript-fájl**. Típus `AadPickerLibrary` a fájlnevet, majd kattintson a **OK**.
9. Másolja a tartalmat a [Itt](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) történő ~ \Scripts\AadPickerLibrary.js.
   
   A parancsfájl a `AadPicker` hívások objektum [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) felhasználókat és csoportokat, amelyek megfelelnek a bemeneti adatok kereséséhez.  
10. ~\Scripts\AadPickerLibrary.js is használja a [jQuery felhasználói felület Autocomplete widget](https://jqueryui.com/autocomplete/). Így kell jQuery felhasználói felület hozzáadása a projekthez. Kattintson a jobb gombbal a projektre a, és kattintson a **NuGet-csomagok kezelése**.
11. A NuGet Package Manager, kattintson a Tallózás gombra, típus **jquery-ui** a keresősávban, és kattintson a **jQuery.UI.Combined**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. Kattintson a jobb oldali ablaktáblában **telepítése**, majd kattintson a **OK** folytatja.
13. Nyissa meg a ~\App_Start\BundleConfig.cs, és hajtsa végre a következő kijelölt módosításokat:  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
        // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
        bundles.Add(new ScriptBundle(&quot;~/bundles/modernizr&quot;).Include(
                    &quot;~/Scripts/modernizr-*&quot;));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/bootstrap&quot;).Include(
                    &quot;~/Scripts/bootstrap.js&quot;,
                    &quot;~/Scripts/respond.js&quot;));
    
        bundles.Add(new StyleBundle(&quot;~/Content/css&quot;).Include(
                    &quot;~/Content/bootstrap.css&quot;,
                    &quot;~/Content/site.css&quot;<mark>,
                    &quot;~/Content/themes/base/jquery-ui.css&quot;</mark>));
    }
    </pre>
    
    Az alkalmazás JavaScript és CSS fájlok kezeléséhez további performant módja van. Azonban az egyszerűség kedvéért most fog a csomagokat, amelyek minden nézet vannak betöltve a célszámítógépre.
14. Végezetül az ~ \Global.asax, adja hozzá a következő kódsort a `Application_Start()` metódust. `Ctrl`+`.`az egyes elnevezési névfeloldási hiba oldhatja meg a problémát.
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > Mivel az alapértelmezett MVC-sablont használ a kódsort szüksége <code>[ValidateAntiForgeryToken]</code> decoration egyes műveleteket. A leírt viselkedéstől miatt [Brock Allen](https://twitter.com/BrockLAllen) : [MVC 4, AntiForgeryToken és jogcímek](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) a HTTP POST sikertelenek lehetnek hamisításgátló jogkivonat érvényesítésére, mert:
    > 
    > * Az Azure Active Directory nem küldi el a http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, amelyre azonban szükség van alapértelmezés szerint a hamisításgátló jogkivonat.
    > * Ha Azure Active Directory szinkronizálása megtörtént-e az AD FS könyvtár, az AD FS megbízható alapértelmezés szerint nem jogcímet küld ki a http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider vagy, bár manuálisan konfigurálhatja az AD FS és a jogcímet küld ki.
    > 
    > `ClaimTypes.NameIdentifies`Adja meg a jogcímek `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, amely Azure Active Directory fogja tartalmazni.  
    > 
    > 
15. Most tegye közzé a módosításokat. Kattintson jobb gombbal a projektre, és kattintson a **közzététel**.
16. Kattintson a **beállítások**, győződjön meg arról, hogy az SQL-adatbázis kapcsolati karakterláncát, válassza ki **frissítés adatbázis** a séma módosításához a modell, és kattintson a **közzététel**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. A böngészőben navigáljon a https://&lt;*appname*>.azurewebsites.net/workitems, és kattintson **hozzon létre új**.
18. Kattintson a **AssignedToName** mezőbe. A legördülő listában az Azure Active Directory-bérlő felhasználók és csoportok ekkor megjelenik. Írja be a szűrésére, vagy használja a `Up` vagy `Down` kulcs, vagy jelölje be a felhasználó vagy csoport. 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. Kattintson a **létrehozása** menti a módosításokat. Kattintson a **szerkesztése** függ a létrehozott és figyelje meg a kívánt viselkedést eredményező beállítást.

Congrats most már fut egy sor üzleti alkalmazás Azure directory elérésével! Nincs sok más, a Graph API teheti. Lásd: [Azure AD Graph API-referencia](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).

<a name="next"></a>

## <a name="next-step"></a>Következő lépés
Ha az üzleti alkalmazás az azure szerepköralapú hozzáférés-vezérlést (RBAC) van szüksége, tekintse meg [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) az az Azure Active Directory ügyfélszolgálata egy mintát. Azt mutatja be az Azure Active Directory-alkalmazás szerepkörök engedélyezése, és majd a felhasználók engedélyezik a `[Authorize]` decoration.

Ha a sor üzleti kell a helyszíni adatokhoz való hozzáférés, lásd: [hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service](web-sites-hybrid-connection-get-started.md).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>További erőforrások
* [Hitelesítési és engedélyezési az Azure App Service-ben](../app-service/app-service-authentication-overview.md)
* [A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez](web-sites-authentication-authorization.md)
* [Üzleti alkalmazás létrehozása az Azure-ban az AD FS-hitelesítéshez](web-sites-dotnet-lob-application-adfs.md)
* [Az App Service hitelesítés és az Azure AD Graph API](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [A Microsoft Azure Active Directory-példák és dokumentáció](https://github.com/AzureADSamples)
* [Az Azure Active Directory támogatott jogkivonat és jogcímtípusok](http://msdn.microsoft.com/library/azure/dn195587.aspx)
