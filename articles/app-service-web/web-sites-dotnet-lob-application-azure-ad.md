---
title: "aaaCreate üzleti az Azure alkalmazás Azure Active Directory-hitelesítéssel |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy ASP.NET MVC-üzleti alkalmazások az Azure App Service-ben, amely hitelesíti az Azure Active Directoryval"
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
ms.openlocfilehash: 3bcafad78ac0151889b3e336784cc561009f244f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a>Üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés
Ez a cikk bemutatja, hogyan toocreate egy .NET-üzleti alkalmazás [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) hello segítségével [hitelesítési / engedélyezési](../app-service/app-service-authentication-overview.md) szolgáltatás. Azt is bemutatja, hogyan toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery címtáradatok hello alkalmazásban.

hello Azure Active Directory-bérlőt, amelyekkel egy csak Azure-címtár is lehet. Vagy lehet [a helyszíni Active Directoryból szinkronizált](../active-directory/active-directory-aadconnect.md) toocreate egy egyszeri bejelentkezéses felhasználói élmény biztosítása, amely a helyi és távoli dolgozók. Ebben a cikkben hello alapértelmezett címtár az Azure-fiókját.

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Mit fog létrehozni
Fog létrehozni egy egyszerű üzleti Create-olvasás-Módosítás-Törlés (CRUD) alkalmazást az App Service Web Apps, hogy nyomon követi a következő funkciók hello elemek használható:

* Hitelesíti a felhasználókat az Azure Active Directoryban
* Lekérdezi a directory-felhasználók és csoportok használatával [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)
* Használja az ASP.NET MVC hello *nem hitelesítési* sablon

Ha az üzleti alkalmazás az Azure szerepköralapú hozzáférés-vezérlést (RBAC) van szüksége, tekintse meg [tovább](#next).

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Mi szükséges
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Meg kell hello toocomplete követően ez az oktatóanyag:

* Az Azure Active Directory-bérlő különböző csoportok felhasználóival
* Engedélyek toocreate alkalmazások hello Azure Active Directory-bérlő
* A Visual Studio 2013 Update 4 vagy újabb verzió
* [Az Azure SDK 2.8.1-es verziójának vagy újabb verzió](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-tooazure"></a>Létrehozhat és telepíthet egy webes alkalmazás tooAzure
1. A Visual Studio eszközből a kattintson **fájl** > **új** > **projekt**.
2. Válassza ki **ASP.NET Web Application**, nevezze el a projektet, és kattintson a **OK**.
3. SELECT hello **MVC** sablont, majd módosítsa túl hello hitelesítési**nem hitelesítési**. Győződjön meg arról, hogy **gazdagépet a felhő hello** van kiválasztva, és kattintson a **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. A hello **App Service létrehozása** párbeszédpanel, kattintson a **vegyen fel egy fiókot** (, majd **vegyen fel egy fiókot** hello a legördülő) az Azure-fiók tooyour toolog.
5. Miután bejelentkezett a webalkalmazás konfigurálása. Hozzon létre egy erőforráscsoportot és egy új App Service-csomag megfelelő hello kattintva **új** gombra. Kattintson a **további Azure-szolgáltatások felfedezés** toocontinue.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. A hello **szolgáltatások** lapra, majd  **+**  tooadd az alkalmazás SQL-adatbázis. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. A **SQL-adatbázis beállítása**, kattintson a **új** toocreate egy SQL Server-példányt.
8. A **SQL-kiszolgáló konfigurálása**, konfigurálja az SQL Server-példányt. Kattintson a **OK**, **OK**, és **létrehozása** tookick ki hello létrehozása az Azure-ban.
9. A **Azure App Service-tevékenység**, láthatja, mikor hello létrehozása befejeződött. Kattintson a  **közzététel &lt;* appname*> toothis webalkalmazás most **, majd kattintson **közzététel**. 
   
    Miután befejezte a Visual Studio, hello nyílik alkalmazás közzététele hello böngészőben. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a>Hitelesítés és a directory hozzáférés konfigurálása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello bal oldali menüben kattintson **alkalmazásszolgáltatások** > **&lt;*appname*> ** > **hitelesítési / engedélyezési**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. Azure Active Directory-hitelesítés bekapcsolása kattintva **a** > **Azure Active Directory** > **Express** > **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. Kattintson a **mentése** hello parancs sávon.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    Miután hello hitelesítési beállításainak mentése sikeresen befejeződött, próbálja a Navigálás tooyour app újra hello böngészőben. Az alapértelmezett beállítások a teljes alkalmazás hello hitelesítés kikényszerítéséhez. Ha még nem jelentkezett, átirányított tooa bejelentkezési képernyő áll. Miután bejelentkezett, megjelenik az HTTPS által védett alkalmazáshoz. A következő lépésben tooenable access toodirectory adatokat. 
5. Keresse meg a toohello [klasszikus portál](https://manage.windowsazure.com).
6. Hello bal oldali menüben kattintson **Active Directory** > **alapértelmezett címtárat** > **alkalmazások**  >   **&lt;* appname*> **.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    Ez az App Service létrehozott meg tooenable hello engedélyezési hello Azure Active Directory-alkalmazás / hitelesítési funkció.
7. Kattintson a **felhasználók** és **csoportok** toomake, hogy rendelkezik-egyes felhasználók és csoportok hello könyvtárban. Ha nem, néhány teszt felhasználók és csoportok létrehozása.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. Kattintson a **konfigurálása** tooconfigure ezt az alkalmazást.
9. Görgessen lefelé toohello **kulcsok** szakaszt, és a kulcs hozzáadása egy duration kiválasztásával. Kattintson a **delegált engedélyek** válassza **címtáradatok olvasása**. 
   Kattintson a **Save** (Mentés) gombra.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. Miután a beállítások lesznek mentve, görgessen toohello készítsen biztonsági másolatot **kulcsok** szakaszt, és kattintson a hello **másolása** gomb toocopy hello ügyfélkulcsot. 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > Ha most elhagyni a lapot, akkor ez az ügyfél kulcs soha újra tudja tooaccess.
    > 
    > 
11. A következő lépésben tooconfigure a webalkalmazás a kulccsal. Jelentkezzen be toohello [Azure erőforrás-kezelő](https://resources.azure.com) az Azure-fiókjába.
12. Hello hello oldal tetején, kattintson a **olvasási/írási** hello Azure erőforrás-kezelő toomake változásairól.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. Hello hitelesítési beállításai az alkalmazások, előfizetések helyen található >  **&lt;* subscriptionname*> ** > **resourceGroups**  >   **&lt;* resourcegroupname*> ** > **szolgáltatók** > **Microsoft.Web**  >  **helyek** > **&lt;*appname*> ** > **config**  >  **authsettings**.
14. Kattintson a **Szerkesztés** gombra.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. Hello ablaktábla szerkesztése, állítson be hello `clientSecret` és `additionalLoginParams` tulajdonságok az alábbiak szerint.
    
        ...
        "clientSecret": "<client key from hello Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. Kattintson a **Put** : hello felső toosubmit a módosításokat.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. Mostantól, ha hello engedélyezési jogkivonatot tooaccess hello Azure Active Directory Graph API, egyszerűen lépjen tootest  **https://&lt;*appname*> a.azurewebsites.net/.auth/me** a böngésző. Ha Ön mindent megfelelően konfigurálva, megjelenik hello `access_token` hello JSON-választ a tulajdonság.
    
    Hello `~/.auth/me` URL-címet felügyeli App Service hitelesítés / engedélyezés toogive meg minden hello vonatkozó információkat tooyour hitelesített munkamenet. További információkért lásd: [hitelesítési és engedélyezési az Azure App Service](../app-service/app-service-authentication-overview.md).
    
    > [!NOTE]
    > Hello `access_token` rendelkezik egy olyan lejárati időt. Azonban App Service hitelesítés / engedélyezés a token frissítési funkciókat kínál `~/.auth/refresh`. További információt a toouse, lásd: [szolgáltatás Token Alkalmazásáruházból](https://cgillum.tech/2016/03/07/app-service-token-store/).
    > 
    > 

A következő fogja végrehajtani valamit a címtáradatok hasznos.

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-tooyour-app"></a>Üzleti funkció tooyour alkalmazás hozzáadása
Ezután hozzon létre egy egyszerű CRUD munkahelyi elemek Rendszerleállási események követése.  

1. Hello ~\Models mappában hozzon létre egy WorkItem.cs nevű osztályt, és cserélje le `public class WorkItem {...}` a hello a következő kódot:
   
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
2. Hello projekt toomake az új modell elérhető toohello állványok programot a Visual Studio build.
3. Egy új generált elem hozzáadása a `WorkItemsController` toohello ~\Controllers mappa (kattintson a jobb gombbal **tartományvezérlők**, pont túl**Hozzáadás**, és válassza ki **új generált elem**). 
4. Válassza ki **MVC 5 Controller nézetek, entitás-keretrendszer használatával** kattintson **Hozzáadás**.
5. SELECT hello modell létrehozott, majd kattintson a  **+**  , majd **Hozzáadás** tooadd egy adatkörnyezetet, és kattintson a **Hozzáadás**.
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. A ~\Views\WorkItems\Create.cshtml (az automatikusan generált elem), megkeresi a hello `Html.BeginForm` segédmetódus, és győződjön meg a kijelölt változtatások hello:  
   
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
    @Html.ActionLink(&quot;Back tooList&quot;, &quot;Index&quot;)
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
   
        // Submit hello selected user/group toobe asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   Vegye figyelembe, hogy `token` és `tenant` hello által használt `AadPicker` objektum toomake Azure Active Directory Graph API-hívásokat. Fogja hozzáadni `AadPicker` később.     
   
   > [!NOTE]
   > Is kaphat `token` és `tenant` hello ügyfél oldalán a `~/.auth/me`, de ez egy további kiszolgálóra hívás lenne. Példa:
   > 
   > $.ajax ({adattípus: "json" URL-címe: "/.auth/me", a sikeres: függvény (adatok) {var token adatok [0] = .access_token; var bérlői adatok [0] .user_claims .find = (c = > c.typ === "http://schemas.microsoft.com/identity/claims/tenantid") .val;}});
   > 
   > 
7. Hello végez azonos módosításokat ~ \Views\WorkItems\Edit.cshtml.
8. Hello `AadPicker` objektum van definiálva egy parancsprogram, hogy kell-e tooadd tooyour projekt. Jobb gombbal az hello ~\Scripts mappa túl**Hozzáadás**, és kattintson a **JavaScript-fájl**. Típus `AadPickerLibrary` hello fájlnevet, majd kattintson **OK**.
9. Másolja a tartalmat hello [Itt](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) történő ~ \Scripts\AadPickerLibrary.js.
   
   Hello parancsfájlban hello `AadPicker` hívások objektum [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) a felhasználókat és csoportokat, amelyek megfelelnek a hello bemeneti toosearch.  
10. ~\Scripts\AadPickerLibrary.js is használ az hello [jQuery felhasználói felület Autocomplete widget](https://jqueryui.com/autocomplete/). Így kell tooadd jQuery felhasználói felület tooyour projekt. Kattintson a jobb gombbal a projektre a, és kattintson a **NuGet-csomagok kezelése**.
11. A NuGet-Csomagkezelő hello, kattintson a Tallózás gombra, típus **jquery-ui** hello keresősávban, és kattintson a **jQuery.UI.Combined**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. Hello jobb oldali ablaktáblában kattintson **telepítése**, majd kattintson a **OK** tooproceed.
13. Nyissa meg a ~\App_Start\BundleConfig.cs, és hajtsa végre a kijelölt változtatások hello:  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
        // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
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
    
    Nincsenek további performant módon toomanage JavaScript és CSS fájlok az alkalmazásban. Azonban az egyszerűség kedvéért csak fog toopiggyback a hello csomagok, amelyek minden nézet vannak betöltve.
14. Végezetül az ~ \Global.asax, adja hozzá a következő kódsort hello hello `Application_Start()` metódust. `Ctrl`+`.`minden elnevezési névfeloldási lévő hiba túl javítást.
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > Mivel hello alapértelmezett MVC-sablont használ a kódsort szüksége <code>[ValidateAntiForgeryToken]</code> decoration egyes hello műveletek. Lejáró leírt toohello viselkedéstől [Brock Allen](https://twitter.com/BrockLAllen) : [MVC 4, AntiForgeryToken és jogcímek](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) a HTTP POST sikertelenek lehetnek hamisításgátló jogkivonat érvényesítésére, mert:
    > 
    > * Az Azure Active Directory nem küldi hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, amelyre azonban szükség van alapértelmezés szerint hello hamisításgátló jogkivonat.
    > * Ha Azure Active Directory szinkronizálása megtörtént-e az AD FS directory, AD FS hello megbízhatósági alapértelmezés szerint nem jogcímet küld ki hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider vagy, bár manuálisan konfigurálhatja az AD FS toosend Ezt a kérelmet.
    > 
    > `ClaimTypes.NameIdentifies`Adja meg a hello jogcím `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, amely Azure Active Directory fogja tartalmazni.  
    > 
    > 
15. Most tegye közzé a módosításokat. Kattintson jobb gombbal a projektre, és kattintson a **közzététel**.
16. Kattintson a **beállítások**, győződjön meg arról, hogy van egy kapcsolati karakterlánc tooyour SQL-adatbázis, válassza ki **frissítés adatbázis** toomake hello változás történt a modell, és kattintson a **közzététel** .
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. Hello böngészőben nyissa meg a toohttps: / /&lt;*appname*>.azurewebsites.net/workitems, és kattintson **hozzon létre új**.
18. Kattintson a hello **AssignedToName** mezőbe. A legördülő listában az Azure Active Directory-bérlő felhasználók és csoportok ekkor megjelenik. Írja be a toofilter, vagy használja a hello `Up` vagy `Down` kulcs, vagy kattintson a tooselect hello felhasználó vagy csoport. 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. Kattintson a **létrehozása** toosave hello módosításokat. Kattintson a **szerkesztése** létrehozott hello munka elem tooobserve ugyanez a viselkedés hello.

Congrats most már fut egy sor üzleti alkalmazás Azure directory elérésével! Nincs sokkal több hello Graph API teheti. Lásd: [Azure AD Graph API-referencia](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).

<a name="next"></a>

## <a name="next-step"></a>Következő lépés
Ha az üzleti alkalmazás az azure szerepköralapú hozzáférés-vezérlést (RBAC) van szüksége, tekintse meg [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) a hello Azure Active Directory csapat egy mintát. Megmutatja, hogyan tooenable szerepkörök az Azure Active Directory-alkalmazás, és majd engedélyezése a felhasználók hello `[Authorize]` decoration.

Ha a sor üzleti alkalmazás tooon helyszíni adatokhoz kell használni, lásd: [hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service](web-sites-hybrid-connection-get-started.md).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>További erőforrások
* [Hitelesítési és engedélyezési az Azure App Service-ben](../app-service/app-service-authentication-overview.md)
* [A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez](web-sites-authentication-authorization.md)
* [Üzleti alkalmazás létrehozása az Azure-ban az AD FS-hitelesítéshez](web-sites-dotnet-lob-application-adfs.md)
* [App Service hitelesítés és hello Azure AD Graph API](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [A Microsoft Azure Active Directory-példák és dokumentáció](https://github.com/AzureADSamples)
* [Az Azure Active Directory támogatott jogkivonat és jogcímtípusok](http://msdn.microsoft.com/library/azure/dn195587.aspx)
