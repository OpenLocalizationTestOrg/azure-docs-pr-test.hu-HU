---
title: ".NET-alkalmazás létrehozása a Service Fabric |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy alkalmazást az ASP.NET Core előtér- és egy megbízható szolgáltatás állapot-nyilvántartó háttér-alkalmazás és központi telepítését a fürthöz."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: ef50adf3af19bce494c3256308b443c8eaccdcea
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Hozzon létre és telepítsen egy alkalmazást az ASP.NET Core Web API előtér- és egy állapotalapú háttér-szolgáltatás
Ez az oktatóanyag egy sorozat része.  Megtudhatja, hogyan egy Azure Service Fabric-alkalmazás létrehozása az ASP.NET Core Web API előtér és állapot-nyilvántartó háttér-szolgáltatás az adatok tárolásához. Amikor végzett, hogy a szavazóalkalmazást az ASP.NET Core webes előtér-állapot-nyilvántartó háttér-szolgáltatás a fürt szavazó eredmények takarít meg, amely a. Ha nem szeretné manuálisan létrehozni a szavazóalkalmazást, akkor [töltse le a forráskód](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) a kész alkalmazás, és ugorjon előre [végezze el a szavazó mintaalkalmazás](#walkthrough_anchor).

![Alkalmazásdiagram](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

A rész az adatsorozatok megismerheti, hogyan:

> [!div class="checklist"]
> * Állapot-nyilvántartó megbízható szolgáltatásként az ASP.NET Core webes API-szolgáltatás létrehozása
> * Állapot nélküli webszolgáltatásként ASP.NET Core webalkalmazás-szolgáltatás létrehozása
> * A fordított proxy segítségével kommunikál az állapotalapú szolgáltatással

Az oktatóanyag adatsorozat elsajátíthatja, hogyan:
> [!div class="checklist"]
> * A .NET Service Fabric-alkalmazás létrehozása
> * [Telepítse központilag az alkalmazást egy távoli fürthöz](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurálja a CI/CD Visual Studio Team Services használatával](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez:
- Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Telepítse a Visual Studio 2017](https://www.visualstudio.com/) és telepítse a **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.
- [A Service Fabric SDK telepítése](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Megbízható szolgáltatásként ASP.NET Web API-szolgáltatás létrehozása
Először hozza létre a webes előtér-, az ASP.NET Core segítségével szavazóalkalmazást. Az ASP.NET Core egy egyszerűsített, platformfüggetlen webes fejlesztési keretrendszer, amely a webes API-k és modern webes felhasználói felület létrehozása segítségével. Ahhoz, hogy a teljes ismertetése, hogy az ASP.NET Core hogyan integrálható a Service Fabric, mindenképpen olvassa a [ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) cikk. Most hajtsa végre ezt az oktatóanyagot gyorsan. Az ASP.NET Core kapcsolatos további tudnivalókért tekintse meg a [ASP.NET Core dokumentációja](https://docs.microsoft.com/aspnet/core/).

> [!NOTE]
> Ez az oktatóanyag alapul a [ASP.NET Core eszközei Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). A .NET Core eszközöket a Visual Studio 2015 már nem frissítés alatt álló.

1. Indítsa el a Visual Studiót **rendszergazdaként**.

2. A projekt létrehozása **fájl**->**új**->**projekt**

3. Az **Új projekt** párbeszédpanelen válassza a **Felhő > Service Fabric-alkalmazás** elemet.

4. Adjon nevet az alkalmazásnak **Voting** nyomja le az ENTER **OK**.

   ![A Visual Studio Új projekt párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. Az a **új Service Fabric-szolgáltatás** lapon, válassza ki **állapotmentes ASP.NET Core**, és a szolgáltatás **VotingWeb**.
   
   ![ASP.NET webszolgáltatás kiválasztása az új service párbeszédpanelen](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. A következő oldalon biztosít az ASP.NET Core projektsablonjai. A jelen oktatóanyag esetében válassza ki a **webalkalmazás**. 
   
   ![ASP.NET-projekt típusának kiválasztása](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio létrehoz egy alkalmazás és szolgáltatás projekt, és megjeleníti őket a Megoldáskezelőben.

   ![A megoldáskezelő ASP.NET alapvető szolgáltatás webes API-alkalmazás létrehozása]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a>A VotingWeb szolgáltatás AngularJS hozzáadása
Adja hozzá [AngularJS](http://angularjs.org/) a szolgáltatáshoz a beépített [Bower támogatási](/aspnet/core/client-side/bower). Nyissa meg *bower.json* szögben kifejezett és szögben kifejezett rendszerindítási bejegyzés hozzáadása, majd mentse a módosításokat.

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.5",
    "angular-bootstrap": "v1.1.0"
  }
}
```
Mentéskor a *bower.json* fájl, Angular telepítve van a projekt *wwwroot/lib* mappát. Ezenkívül szerepel a listán belül a *függőségek/Bower* mappa.

### <a name="update-the-sitejs-file"></a>A site.js fájl frissítése
Nyissa meg a *wwwroot/js/site.js* fájlt.  Cserélje ki annak tartalmát a JavaScript a kezdőlap nézetek által használt:

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-the-indexcshtml-file"></a>A Index.cshtml fájl frissítése
Nyissa meg a *Views/Home/Index.cshtml* fájl, a nézet a kezdőlap vezérlő jellemző.  Cserélje ki annak tartalmát a következőt, majd a változtatások mentéséhez.

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item" />
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr />

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click to vote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-the-layoutcshtml-file"></a>A _Layout.cshtml fájl frissítése
Nyissa meg a *Views/Shared/_Layout.cshtml* fájlt, az ASP.NET-alkalmazás alapértelmezett elrendezését.  Cserélje ki annak tartalmát a következőt, majd a változtatások mentéséhez.

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" />
    <link href="~/css/site.css" rel="stylesheet" />

</head>
<body>
    <div class="container body-content">
        @RenderBody()
    </div>

    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/angular/angular.js"></script>
    <script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="~/js/site.js"></script>

    @RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-the-votingwebcs-file"></a>A VotingWeb.cs fájl frissítése
Nyissa meg a *VotingWeb.cs* fájlt, amely létrehozza az ASP.NET Core WebHost az állapot nélküli-szolgáltatást a WebListener webkiszolgáló belül.  Adja hozzá a `using System.Net.Http;` irányelv a fájl elejéhez.  Cserélje le a `CreateServiceInstanceListeners()` és a következő működni, akkor a módosítások mentéséhez.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
            {
                ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting WebListener on {url}");

                return new WebHostBuilder().UseWebListener()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<StatelessServiceContext>(serviceContext)
                                    .AddSingleton<HttpClient>())
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseApplicationInsights()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
            }))
    };
}
```

### <a name="add-the-votescontrollercs-file"></a>Adja hozzá a VotesController.cs
Vegyen fel egy vezérlőt, amely meghatározza a szavazó műveletek. Kattintson a jobb gombbal a a **tartományvezérlők** mappát, majd válassza ki **Hozzáadás -> Új elem -> osztály**.  A fájl neve "VotesController.cs", és kattintson a **Hozzáadás**.  Cserélje ki a fájl tartalmát a következőt, majd a változtatások mentéséhez.  A későbbi [VotesController.cs fájl frissíthető](#updatevotecontroller_anchor), ezt a fájlt úgy módosítjuk, hogy olvasási és írási szavazó adatok a háttér-szolgáltatás.  A lépést a tartományvezérlő nézetbe statikus karakterlánc adatokat ad vissza.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

namespace VotingWeb.Controllers
{
    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```



### <a name="deploy-and-run-the-application-locally"></a>Telepítheti és futtathatja az alkalmazást helyileg
Most lépjen tovább, és futtassa az alkalmazást. Nyomja le az `F5` billentyűt a Visual Studióban, hogy üzembe helyezze az alkalmazást a hibakereséshez. `F5`sikertelen lesz, ha nem korábban nyissa meg a Visual Studióban **rendszergazda**.

> [!NOTE]
> Az alkalmazás első helyi történő üzembe helyezésekor a Visual Studio létrehoz egy helyi hibakeresési fürtöt.  A fürt létrehozása eltarthat egy ideig. A fürt létrehozási állapota a Visual Studio kimeneti ablakában jelenik meg.

Ezen a ponton a webalkalmazás kell kinéznie:

![Az ASP.NET Core előtér-](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

Az alkalmazás hibakeresését végzi leállításához vissza a Visual Studio, és nyomja le az **Shift + F5**.

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Állapot-nyilvántartó háttér-szolgáltatás hozzáadása az alkalmazáshoz
Most, hogy az ASP.NET Web API-szolgáltatás fut az alkalmazás, lépjen tovább, és adja hozzá egy állapotalapú megbízható szolgáltatást, hogy néhány adat tárolása az alkalmazás.

A Service Fabric következetesen és megbízhatóan tárolja az adatok jobb belül a szolgáltatás megbízható gyűjtemények segítségével teszi lehetővé. Megbízható gyűjtemények olyan magas rendelkezésre állású és megbízható gyűjteményosztály, amelyek bárki, aki használta a C# gyűjtemények számára.

Ebben az oktatóanyagban létrehoz egy szolgáltatás, amely egy megbízható gyűjtemény egy számláló értékét tárolja.

1. A Megoldáskezelőben kattintson a jobb gombbal **szolgáltatások** az alkalmazásban le, és válassza a **Hozzáadás > új Service Fabric-szolgáltatás**.
   
    ![Egy új szolgáltatás hozzáadása meglévő alkalmazáshoz](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. Az a **új Service Fabric-szolgáltatás** párbeszédpanelen válasszon **állapotalapú alkalmazások és szolgáltatások ASP.NET Core**, és a szolgáltatás **VotingData** nyomja le az ENTER **OK**.

    ![A Visual Studio Új szolgáltatás párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    A service-projekt létrehozása után a felhasználó két szolgáltatást az alkalmazásban. Továbbra is építenie az alkalmazást, mert a szolgáltatás ugyanúgy is hozzáadhat. Minden egyes lehet függetlenül rendszerverzióval ellátott és frissített.

3. A következő oldalon biztosít az ASP.NET Core projektsablonjai. A jelen oktatóanyag esetében válassza ki a **Web API**.

    ![ASP.NET-projekt típusának kiválasztása](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    A Visual Studio létrehoz egy szolgáltatási projektet, és megjeleníti őket a Megoldáskezelőben.

    ![Megoldáskezelő](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-the-votedatacontrollercs-file"></a>Adja hozzá a VoteDataController.cs

A a **VotingData** projekt kattintson a jobb gombbal a **tartományvezérlők** mappát, majd válassza ki **Hozzáadás -> Új elem -> osztály**. A fájl neve "VoteDataController.cs", és kattintson a **Hozzáadás**. Cserélje ki a fájl tartalmát a következőt, majd a változtatások mentéséhez.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.ServiceFabric.Data;
using System.Threading;
using Microsoft.ServiceFabric.Data.Collections;

namespace VotingData.Controllers
{
    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var ct = new CancellationToken();

            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                var list = await votesDictionary.CreateEnumerableAsync(tx);

                var enumerator = list.GetAsyncEnumerator();

                var result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-the-services"></a>Csatlakozás a szolgáltatások
A következő lépésben rendszer összekapcsolja a két szolgáltatásokat és ellenőrizze az előtér-webkiszolgálók alkalmazás beolvasása és beállítása a szavazás a háttér-szolgáltatás adatait.

A Service Fabric hogyan kommunikáljanak megbízható szolgáltatások teljes rugalmasságot biztosít. Egyetlen alkalmazásban lehetséges, hogy TCP-n keresztül elérhető szolgáltatások. Elképzelhető, hogy egy HTTP REST API-n keresztül érhető el, hogy más szolgáltatások és egyéb szolgáltatások továbbra is lehet webes szoftvercsatornák keresztül érhető el. A rendelkezésre álló lehetőségeket, és a mellékhatásokkal jár a háttérben, lásd: [szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md).

Ebben az oktatóanyagban használjuk [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a>A VotesController.cs fájl frissítése
Az a **VotingWeb** projektben nyissa meg a *Controllers/VotesController.cs* fájlt.  Cserélje le a `VotesController` definíciójának tartalma a következő osztályt, majd mentse a módosításokat.

```csharp
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;
        string serviceProxyUrl = "http://localhost:19081/Voting/VotingData/api/VoteData";
        string partitionKind = "Int64Range";
        string partitionKey = "0";

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            IEnumerable<KeyValuePair<string, int>> votes;

            HttpResponseMessage response = await this.httpClient.GetAsync($"{serviceProxyUrl}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            votes = JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync());

            return Json(votes);
        }

        // PUT: api/Votes/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            string payload = $"{{ 'name' : '{name}' }}";
            StringContent putContent = new StringContent(payload, Encoding.UTF8, "application/json");
            putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            string proxyUrl = $"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}";

            HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent);

            return new ContentResult()
            {
                StatusCode = (int)response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }

        // DELETE: api/Votes/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            HttpResponseMessage response = await this.httpClient.DeleteAsync($"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            return new OkResult();

        }
    }
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-the-voting-sample-application"></a>Végezze el a szavazó mintaalkalmazás
A szavazó alkalmazás két szolgáltatásból áll:
- Webes előtér-szolgáltatás (VotingWeb) – az ASP.NET Core webes előtér-szolgáltatás, a weblap szolgál, és tesz elérhetővé webes API-khoz a háttér-szolgáltatással való kommunikációra.
- Háttér-szolgáltatás (VotingData)-az ASP.NET Core webszolgáltatáshoz, amely közzétesz egy API-t, a megbízható szótárban az szavazattal eredmények tárolásához a lemezen maradnak.

![Alkalmazásdiagram](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Amikor az alkalmazás szavaz a következők történnek:
1. A JavaScript a szavazás kérést küld egy HTTP PUT-kérelmet, a a webes API-t a webes előtér-szolgáltatás.

2. A webes előtér-szolgáltatás egy proxy használatával keresse meg és a háttér-szolgáltatás egy HTTP PUT-kérelmet továbbítja.

3. A háttér-szolgáltatás időt vesz igénybe a bejövő kérelem, és tárolja a frissített eredmény megbízható szótár, amely lekérdezi a fürt több csomópontja replikálása, és a lemezen maradnak. Az alkalmazás adatokat tárolja a fürt így adatbázis szükséges.

## <a name="debug-in-visual-studio"></a>A Visual Studio hibakeresési
A Visual Studio alkalmazás nyomkövetésére használ egy helyi Service Fabric-fejlesztési fürtöt. Lehetősége van úgy, hogy adott esetben a hibakeresési felhasználói élmény. Ebben az alkalmazásban tároljuk adatok a háttér-szolgáltatásban egy megbízható szótár használatával. A Visual Studio eltávolítja az alkalmazás alapértelmezés szerint a hibakereső leállítása. Az alkalmazás eltávolítása azt eredményezi, az adatok a háttér-szolgáltatás is el kell távolítani. Az adatok között munkamenetek hibakeresés megőrizni, módosíthatja a **alkalmazás hibakeresési módban** meg tulajdonságként a **Voting** projektre a Visual Studio.

Nézze meg mi történik, a kódban, végezze el a következő lépéseket:
1. Nyissa meg a **VotesController.cs** fájlt, és állítson be egy töréspontot a webes API **Put** metódus (sor: 47) – a fájlt a Visual studióban a Solution Explorer kereshet.

2. Nyissa meg a **VoteDataController.cs** fájlt, és állítson be egy töréspontot ezen webes API **Put** metódus (sor 50).

3. Lépjen vissza a böngésző és szavazó lehetőségre, vagy adja hozzá egy új szavazó lehetőséget. A webalkalmazás első-a befejezési tartozó api-vezérlőben első töréspont kattint.
    
    1. Ez azért, ahol a JavaScript a böngészőben egy kérést küld a webes API-vezérlőben az előtér-szolgáltatás.
    
    ![Szavazás előtér-szolgáltatás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. Először a háttér-szolgáltatás URL-CÍMÉT a ReverseProxy azt összeállításához **(1)**.
    3. Ezt követően nem küldeni a HTTP PUT kérés a ReverseProxy **(2)**.
    4. Végül a rendszer visszaadja a már a válasz a háttér-szolgáltatásból az ügyfélnek **(3)**.

4. Nyomja le az **F5** folytatja
    1. Ekkor a számítógép a töréspontot a háttér-szolgáltatásban.
    
    ![Szavazás háttér-szolgáltatás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. A metódus első sorában **(1)** használjuk a `StateManager` nevű megbízható szótár felvétele, illetve `counts`.
    3. A megbízható szótárban értékek minden interakció tranzakció, a használatával szükséges utasítás **(2)** hoz létre, hogy a tranzakció.
    4. A tranzakció frissítjük majd a szavazó beállítás értékének a megfelelő kulcs értékét és véglegesíti a művelet **(3)**. Amikor a véglegesítési mód értéket ad vissza, az adatok frissítése a szótárban és replikálja a fürt többi tagján. Az adatok immár biztonságosan tárolja a fürt, és a háttér-szolgáltatás átveheti más csomópontokat, a rendelkezésre álló adatok továbbra is fennáll.
5. Nyomja le az **F5** folytatja

A hibakeresési munkamenetben leállításához nyomja le az **Shift + F5**.


## <a name="next-steps"></a>Következő lépések
Az oktatóanyag ezen része megtanulta, hogyan:

> [!div class="checklist"]
> * Állapot-nyilvántartó megbízható szolgáltatásként az ASP.NET Core webes API-szolgáltatás létrehozása
> * Állapot nélküli webszolgáltatásként ASP.NET Core webalkalmazás-szolgáltatás létrehozása
> * A fordított proxy segítségével kommunikál az állapotalapú szolgáltatással

Előzetes következő oktatóanyagot:
> [!div class="nextstepaction"]
> [Telepítse központilag az alkalmazást az Azure-bA](service-fabric-tutorial-deploy-app-to-party-cluster.md)