---
title: "aaaCORS támogatja az App Service szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan támogatják a toouse CORS Azure Azure App Service-ben."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 4f980a97-b9f5-4d1d-87ab-82b60bb96e1c
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/27/2016
ms.author: alkarche
ms.openlocfilehash: c229378b75840bc0f7b2eefc3df3031233f9b494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-api-app-from-javascript-using-cors"></a>API-alkalmazások felhasználása JavaScriptből a CORS használatával
App Service beépített támogatást nyújt a [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), lehetővé teszi a JavaScript ügyfelek toomake tartományközi meghívja az API-alkalmazások tárolt tooAPIs. App Service segítségével úgy konfigurálja a CORS hozzáférés tooyour API a API programozás nélkül.

Ez a cikk két részből áll:

* Hello [hogyan tooconfigure CORS](#corsconfig) a szakasz ismerteti a általános hogyan tooconfigure CORS API-alkalmazás, webalkalmazás, és mobilalkalmazás. Ez egyaránt érvényes tooall keretrendszerek, amelyet az App Service, beleértve a .NET, Node.js és Java támogatott. 
* Hello kezdve [hello .NET-bevezető oktatóanyagok folytatása](#tutorialstart) szakaszban hello cikk mutatja be a CORS-támogatás oktató építve [hello első API-alkalmazások alapszintű bemutató ](app-service-api-dotnet-get-started.md). 

## <a id="corsconfig"></a>Hogyan tooconfigure az Azure App Service CORS
Cors szolgáltatást konfigurálhatja a hello Azure-portálon vagy [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) eszközök.

#### <a name="configure-cors-in-hello-azure-portal"></a>A CORS konfigurálása hello Azure-portálon
1. Egy böngészőben nyissa meg toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **alkalmazásszolgáltatások**, majd kattintson az API-alkalmazás hello nevét.
   
    ![API-alkalmazás kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. A hello **beállítások** panelen megjelenő toohello jobb oldalán hello **API-alkalmazás** panelen, a keresés hello **API** szakaszt, és kattintson a **CORS**.
   
   ![Válassza a CORS lehetőséget a Beállítások panelen](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Hello mezőbe írjon be hello URL-címe vagy tooallow JavaScript-hívásokat toocome a használni kívánt URL-címeket.

    Például ha telepítette a JavaScript alkalmazás tooa webalkalmazás todolistangular nevű webalkalmazásra telepítette, akkor adja meg az "https://todolistangular.azurewebsites.net" értéket. Alternatív megoldásként megadhat egy csillag (*) toospecify, hogy minden eredettartományból elfogadja a hívásokat.


1. Kattintson a **Save** (Mentés) gombra.
   
   ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   Miután rákattintott **mentése**, hello API-alkalmazás fogadni fogja a JavaScript hello hívásait megadott URL-címeket.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>A CORS konfigurálása az Azure Resource Manager eszközeinek használatával
Beállíthatja úgy is a CORS API-alkalmazás használatával [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-authoring-templates.md) parancssori eszközökben, például a [Azure PowerShell](/powershell/azureps-cmdlets-docs) és hello [Azure CLI](../cli-install-nodejs.md). 

Például egy olyan Azure Resource Manager sablon, amely beállítja hello CORS tulajdonságot, nyissa meg a hello [azuredeploy.json fájlt az oktatóanyag példaalkalmazását hello tárház](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). A következő példa hello néz hello sablon hello szakasz keresése:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Hello .NET-bevezető oktatóanyag folytatása
Ha hello Node.js vagy Java-bevezető sorozat API-alkalmazások, első lépések adatsorozat befejezett hello rendelkezik. Hagyja ki a toohello [további lépések](#next-steps) szakasz toofind javaslatok API-alkalmazások bővebb megismeréséhez.

hello a cikk hátralévő része hello .NET-bevezető sorozat folytatása, és feltételezi, hogy Ön sikeresen elvégezte [hello első oktatóanyaga, amely](app-service-api-dotnet-get-started.md).

## <a name="deploy-hello-todolistangular-project-tooa-new-web-app"></a>Hello ToDoListAngular projekt tooa új webalkalmazás telepítése
A [hello első oktatóanyaga, amely](app-service-api-dotnet-get-started.md), létrehozott egy középső rétegbeli API-alkalmazást és egy adatrétegbeli API-alkalmazást. Ebben az oktatóanyagban létrehoz egy egyoldalas alkalmazások (SPA) webalkalmazás adott hívások hello középső réteg API-alkalmazást. Hello SPA toowork a hello középső rétegbeli API-alkalmazás CORS tooenable rendelkezik. 

A hello [ToDoList példaalkalmazásban](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), hello ToDoListAngular projekt egy egyszerű AngularJS ügyfél hello középső rétegbeli ToDoListAPI webes API projektet hívja. JavaScript-kód hello hello *app/scripts/todoListSvc.js* fájl hívja hello API-t hello AngularJS HTTP-szolgáltató használatával. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 

            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-hello-todolistangular-project"></a>Hozzon létre egy új webalkalmazást hello ToDoListAngular projekthez
hello eljárás toocreate egy új App Service web app és projekt telepítése tooit a látott hasonló toowhat [létrehozása és telepítése az API-alkalmazás az a sorozat első oktatóanyaga hello](app-service-api-dotnet-get-started.md#createapiapp). hello egyetlen különbség az, hogy hello alkalmazás típusa van **webalkalmazás** helyett **API-alkalmazás**.  Hello párbeszédpanelek képernyőképeit lásd: 

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a ToDoListAngular projekt hello, és kattintson a **közzététel**.
2. A hello **profil** hello lapján **webhely közzététele** varázsló, kattintson a **Microsoft Azure App Service**.
3. A hello **App Service** párbeszédpanel, kattintson a **új**.
4. A hello **üzemeltetési** hello lapján **App Service létrehozása** párbeszédpanelen adja meg egy **webalkalmazás neve** hello az egyedi *azurewebsites.net* tartomány. 
5. Válassza ki a hello Azure **előfizetés** azt szeretné, hogy a toowork.
6. A hello **erőforráscsoport** legördülő menüben válassza ki a hello ugyanazt az a korábban létrehozott erőforráscsoportot.
7. A hello **App Service-csomag** legördülő menüben válassza ki a hello korábban létrehozott csomagot. 
8. Kattintson a **Create** (Létrehozás) gombra.
   
    A Visual Studio létrehozza hello webalkalmazást, a közzétételi profilt hozza létre és hello megjeleníti **kapcsolat** hello lépését **webhely közzététele** varázsló.
   
    Még ne kattintson a **Publish** (Közzététel) elemre. A következő szakasz hello hello új alkalmazás toocall hello középső réteg API webalkalmazást az App Service-ben futtató konfigurálja. 

### <a name="set-hello-middle-tier-url-in-web-app-settings"></a>Hello középső réteg URL-Címének beállítása a webalkalmazás beállításaiban
1. Toohello lépjen [Azure-portálon](https://portal.azure.com/), és navigáljon a toohello **webalkalmazás** toohost hello TodoListAngular (kezelőfelület) projekt létrehozott hello webalkalmazás panelen.
2. Kattintson a **Settings > Application Settings** (Beállítások > Alkalmazásbeállítások) lehetőségre.
3. A hello **Alkalmazásbeállítások** területen írja be a következő hello kulcs-érték:
   
   | Kulcs | Érték | Példa |
   | --- | --- | --- |
   | toDoListAPIURL |https://{a középső réteg API-alkalmazásának neve}.azurewebsites.net |https://todolistapi0121.azurewebsites.NET |
4. Kattintson a **Save** (Mentés) gombra.
   
    Amikor hello kód lefut az Azure-ban, ez az érték felülbírálja hello localhost URL-címet a hello *Web.config* fájlt. 
   
    hello beállításérték hello kód van *index.cshtml*:
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    a kód hello *todoListSvc.js* hello beállítást használja:
   
        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-hello-todolistangular-web-project-toohello-new-web-app"></a>Hello ToDoListAngular webes projekt toohello új webalkalmazás telepítése
* A Visual Studio, a hello **kapcsolat** hello lépését **webhely közzététele** varázsló, kattintson a **közzététel**.
  
   A Visual Studio hello ToDoListAngular projekt toohello webalkalmazása telepíti, és megnyitja a böngésző toohello webalkalmazás URL-címe hello. 

### <a name="test-hello-application-without-cors-enabled"></a>Hello alkalmazás tesztelése a CORS engedélyezése nélkül
1. Nyissa meg a böngésző fejlesztői eszközök, hello Console ablakban.
2. Megjeleníti az AngularJS felhasználói felület hello hello böngészőablakban, kattintson a hello **tooDo lista** hivatkozásra.
   
    hello JavaScript-kód megpróbál toocall hello középső rétegbeli API-alkalmazás, de hello hívás sikertelen lesz, mivel hello előtér mint hello vissza egy másik tartományban fut célból. hello böngésző fejlesztői eszközök konzol ablakának egy eltérő eredetű hibaüzenetet jelenít meg.
   
    ![Hibaüzenet az eltérő eredetről](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-hello-middle-tier-api-app"></a>Hello középső rétegbeli API-alkalmazás CORS konfigurálása
Ebben a szakaszban konfigurálni hello CORS beállítása az Azure-ban hello középső rétegbeli ToDoListAPI API-alkalmazás. Ez a beállítás lehetővé teszi a hello középső rétegbeli API app tooreceive JavaScript-hívásokat webalkalmazásból hello hello ToDoListAngular projekthez létrehozott.

1. Egy böngészőben nyissa meg toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **alkalmazásszolgáltatások**, majd kattintson a hello ToDoListAPI (középső réteg) API-alkalmazást.
   
    ![API-alkalmazás kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. A hello **beállítások** panelen megjelenő toohello jobb oldalán hello **API-alkalmazás** panelen, a keresés hello **API** szakaszt, és kattintson a **CORS**.
   
   ![A CORS kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Hello szövegmezőben hello ToDoListAngular (kezelőfelület) webalkalmazás hello URL-cím megadása. Például ha hello ToDoListAngular projekt tooa webalkalmazás todolistangular0121 nevű webalkalmazáshoz telepítette, engedélyezze a hívásokat a hello URL-cím `https://todolistangular0121.azurewebsites.net`.
   
   Alternatív megoldásként megadhat egy csillag (*) toospecify, hogy minden eredettartományból elfogadja a hívásokat.
5. Kattintson a **Save** (Mentés) gombra.
   
   ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   Miután rákattintott **mentése**, hello API-alkalmazás fogadni fogja a JavaScript hello hívásait megadott URL-CÍMÉT. Ezen a képernyőfelvételen a hello ToDoListAPI0223 API-alkalmazás fogja fogadni a ToDoListAngular webalkalmazásból hello JavaScript-ügyfélhívásokat.

### <a name="test-hello-application-with-cors-enabled"></a>Hello alkalmazás tesztelése a CORS engedélyezése mellett
* Nyissa meg a böngésző toohello hello webalkalmazás HTTPS URL-CÍMÉT. 
  
    Az idő hello alkalmazás lehetővé teszi megtekintése, hozzáadása, szerkesztése és törlése a Tennivalólista elemein. 
  
    ![a mintaalkalmazás tooDo lista lap](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Az App Service CORS és a webes API CORS összehasonlítása
A Web API-projektet telepíthet hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet csomag toospecify a kódban, mely tartományok tartományokból fogadja az API a JavaScript-hívásokat.

A Web API CORS-támogatása rugalmasabb, mint az App Service CORS-támogatása. Például a kódban a különböző műveletekhez különböző elfogadott származási helyeket adhat meg, míg az App Service CORS esetében az API-alkalmazás összes függvényéhez csupán az elfogadott tartományok egyetlen halmazát állíthatja be.

> [!NOTE]
> Ne próbáljon toouse Web API CORS és az App Service CORS egy API-alkalmazás is. Az App Service CORS szolgáltatása elsőbbséget élvez, így a Web API CORS szolgáltatásának nem lesz hatása. Például ha engedélyezi az App Service egy forrástartományt, és minden eredettartományból engedélyezéséhez a Web API-kódban, az Azure API-alkalmazás fogja csak hívásokat fogadni hello Azure-ban megadott tartományból.
> 
> 

### <a name="how-tooenable-cors-in-web-api-code"></a>Hogyan tooenable CORS webes API-kódban
a lépéseket követve hello hello folyamat Web API CORS-támogatás engedélyezésének foglalják össze. További információ: [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api) (Az eltérő eredetű kérések engedélyezése az ASP.NET Web API 2-ben).

1. A Web API-projektet, telepítse a hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-csomagot.
2. Tartalmaznak egy `config.EnableCors()` hello kódsort **regisztrálása** hello metódusában **register** osztály, mint például a következő hello. 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // hello following line enables you toocontrol CORS by using Web API code
                config.EnableCors();
   
                // Web API routes
                config.MapHttpAttributeRoutes();
   
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }
3. Adja hozzá a Web API-vezérlőben egy `using` hello nyilatkozata `System.Web.Http.Cors` névteret, és adja hozzá a hello `EnableCors` toohello-vezérlő osztályhoz vagy tooindividual műveletmetódusokhoz attribútum. A következő példa hello, a CORS-támogatás toohello teljes vezérlőre vonatkozik.
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a>Az Azure API Management használata API-alkalmazásokkal
Ha Azure API Management használata API-alkalmazást, adja meg a CORS API felügyelete hello API-alkalmazás. További információkért tekintse meg a következő erőforrások hello:

* [Az Azure API Management áttekintése (videó: a CORS-ról szóló rész 12:10-nél kezdődik)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Az API Management tartományközi szabályzatai](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a>Hibaelhárítás
Ha az oktatóanyag lépéseinek elvégzése közben hibákba ütközne, olvassa el az alábbi hibaelhárítási tippeket:

* Győződjön meg arról, hogy hello hello legújabb verzióját használja [Azure SDK for .NET for Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).
* Győződjön meg arról, hogy a megadott `https` hello CORS beállítását, és győződjön meg arról, hogy használata `https` toorun hello előtér-webalkalmazást.
* Győződjön meg arról, hogy a megadott hello CORS beállításba hello középső rétegbeli API-alkalmazás, és nem hello előtér-webalkalmazást.
* Ha az alkalmazás kódjában és az Azure App Service CORS konfigurálja, vegye figyelembe, hogy hello App Service CORS-beállítása felülírja alkalmazáskód végzett függetlenül. 

További információ a Visual Studio funkcióit, hibaelhárítást egyszerűsítő toolearn lásd [hibaelhárítási Azure App Service apps szolgáltatásban a Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Következő lépések
Ebből a cikkből megtudhatta, hogyan támogatják a tooenable App Service CORS, hogy az ügyfélbeli JavaScript-kód meghívhatja az API-k egy másik tartományban. További API-alkalmazások, olvassa el a hello toolearn [bemutatása tooauthentication az App Service](../app-service/app-service-authentication-overview.md), és folytassa a toohello [API-alkalmazások felhasználói hitelesítésének](app-service-api-dotnet-user-principal-auth.md) oktatóanyag.

