---
title: "a Service Fabric .NET-alkalmazás aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan a toocreate az alkalmazás az ASP.NET Core előtér- és egy megbízható állapot-nyilvántartó háttér-szolgáltatás, és hello alkalmazás tooa fürt központi telepítése."
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
ms.openlocfilehash: bab331b9f8616c50a2794b6c048aace15579c8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Hozzon létre és telepítsen egy alkalmazást az ASP.NET Core Web API előtér- és egy állapotalapú háttér-szolgáltatás
Ez az oktatóanyag egy sorozat része.  Megtudhatja, hogyan toocreate az ASP.NET Core Web API első Azure Service Fabric alkalmazás befejezését, majd egy állapotalapú háttérszolgáltatásnak toostore adatait. Amikor végzett, hogy a szavazóalkalmazást az ASP.NET Core webes előtér-egy állapotalapú háttérszolgáltatásnak hello fürt szavazó eredmények takarít meg, amely a. Ha nem szeretné, hogy toomanually hello szavazás alkalmazás létrehozása, érdemes [hello forráskód letöltése](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) hello befejeződött a alkalmazást, és hagyja ki azokat, amelyek túl[mintaalkalmazás szavazás hello bízná](#walkthrough_anchor).

![Alkalmazásdiagram](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Egyik hello sorozat része, a megismerheti, hogyan:

> [!div class="checklist"]
> * Állapot-nyilvántartó megbízható szolgáltatásként az ASP.NET Core webes API-szolgáltatás létrehozása
> * Állapot nélküli webszolgáltatásként ASP.NET Core webalkalmazás-szolgáltatás létrehozása
> * Fordított proxy toocommunicate hello használata hello állapotalapú szolgáltatással

Az oktatóanyag adatsorozat elsajátíthatja, hogyan:
> [!div class="checklist"]
> * A .NET Service Fabric-alkalmazás létrehozása
> * [Hello alkalmazás tooa távoli fürt központi telepítése](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurálja a CI/CD Visual Studio Team Services használatával](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez:
- Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Telepítse a Visual Studio 2017](https://www.visualstudio.com/) és hello telepítése **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.
- [Hello Service Fabric SDK telepítése](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Megbízható szolgáltatásként ASP.NET Web API-szolgáltatás létrehozása
Először hozzon létre hello webes előtér-, az ASP.NET Core segítségével alkalmazás szavazás hello. Az ASP.NET Core egy egyszerűsített, platformfüggetlen webes fejlesztési keretrendszer, hogy toocreate modern webes felhasználói Felületét használja, és webes API-khoz. a teljes ismertetése, hogy az ASP.NET Core hogyan integrálható a Service Fabric tooget, mindenképpen olvassa keresztül hello [ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) cikk. Most hajtsa végre az oktatóanyag tooget gyorsan. További információ az ASP.NET Core toolearn lásd: hello [ASP.NET Core dokumentációja](https://docs.microsoft.com/aspnet/core/).

> [!NOTE]
> Ez az oktatóanyag hello alapuló [ASP.NET Core eszközei Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). a Visual Studio 2015 hello .NET Core eszközök már nem frissítés alatt álló.

1. Indítsa el a Visual Studiót **rendszergazdaként**.

2. A projekt létrehozása **fájl**->**új**->**projekt**

3. A hello **új projekt** párbeszédpanelen válasszon **felhő > Service Fabric-alkalmazás**.

4. Hello alkalmazás neve **Voting** nyomja le az ENTER **OK**.

   ![A Visual Studio Új projekt párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. A hello **új Service Fabric-szolgáltatás** lapon, válassza ki **állapotmentes ASP.NET Core**, és a szolgáltatás **VotingWeb**.
   
   ![ASP.NET webszolgáltatás kiválasztásával hello új szolgáltatás párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. hello következő oldal biztosít az ASP.NET Core projektsablonjai. A jelen oktatóanyag esetében válassza ki a **webalkalmazás**. 
   
   ![ASP.NET-projekt típusának kiválasztása](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio létrehoz egy alkalmazás és szolgáltatás projekt, és megjeleníti őket a Megoldáskezelőben.

   ![A megoldáskezelő ASP.NET alapvető szolgáltatás webes API-alkalmazás létrehozása]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-toohello-votingweb-service"></a>AngularJS toohello VotingWeb szolgáltatás hozzáadása
Adja hozzá [AngularJS](http://angularjs.org/) tooyour szolgáltatást hello beépített [Bower támogatási](/aspnet/core/client-side/bower). Nyissa meg *bower.json* szögben kifejezett és szögben kifejezett rendszerindítási bejegyzés hozzáadása, majd mentse a módosításokat.

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
Hello mentése után *bower.json* fájl, Angular telepítve van a projekt *wwwroot/lib* mappát. Ezenkívül szerepel a listán belül hello *függőségek/Bower* mappa.

### <a name="update-hello-sitejs-file"></a>Hello site.js fájl frissítése
Nyissa meg hello *wwwroot/js/site.js* fájlt.  Cserélje ki annak tartalmát hello hello otthoni nézetek által használt JavaScript:

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

### <a name="update-hello-indexcshtml-file"></a>Hello Index.cshtml fájl frissítése
Nyissa meg hello *Views/Home/Index.cshtml* fájl, hello adott toohello otthoni nézetvezérlő.  Cserélje ki annak tartalmát hello következőre, majd mentse a módosításokat.

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
                        Click toovote
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

### <a name="update-hello-layoutcshtml-file"></a>Hello _Layout.cshtml fájl frissítése
Nyissa meg hello *Views/Shared/_Layout.cshtml* fájl, hello alapértelmezett elrendezési hello ASP.NET alkalmazás.  Cserélje ki annak tartalmát hello következőre, majd mentse a módosításokat.

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

### <a name="update-hello-votingwebcs-file"></a>Hello VotingWeb.cs fájl frissítése
Nyissa meg hello *VotingWeb.cs* fájlt, amely az ASP.NET Core WebHost hello belül hello állapotmentes szolgáltatások hello WebListener webkiszolgáló használatával hoz létre.  Adja hozzá a hello `using System.Net.Http;` irányelv toohello hello fájl elejéhez.  Cserélje le a hello `CreateServiceInstanceListeners()` hello következő működik, akkor a módosítások mentéséhez.

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

### <a name="add-hello-votescontrollercs-file"></a>Hello VotesController.cs fájl hozzáadása
Vegyen fel egy vezérlőt, amely meghatározza a szavazó műveletek. Kattintson a jobb gombbal a hello **tartományvezérlők** mappát, majd válassza ki **Hozzáadás -> Új elem -> osztály**.  Hello fájl "VotesController.cs" nevet, és kattintson a **Hozzáadás**.  Cserélje ki hello következő hello fájl tartalmát, majd mentse a módosításokat.  A későbbi [hello VotesController.cs frissítésfájl](#updatevotecontroller_anchor), ezt a fájlt a rendszer módosított tooread, majd szavazó adatírás hello háttér-szolgáltatás.  Most hello vezérlő statikus karakterlánc toohello adatnézet adja vissza.

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



### <a name="deploy-and-run-hello-application-locally"></a>Központi telepítése és hello alkalmazás helyileg történő futtatása
Ezután lépjen tovább és hello alkalmazás futtatásához. A Visual Studio, nyomja le a `F5` toodeploy hello alkalmazást a hibakereséshez. `F5`sikertelen lesz, ha nem korábban nyissa meg a Visual Studióban **rendszergazda**.

> [!NOTE]
> hello első alkalommal futtatja, és hello-alkalmazás központi telepítése helyileg, a Visual Studio hibakeresési egy helyi fürtöt hoz létre.  A fürt létrehozása eltarthat egy ideig. hello fürt létrehozási állapota hello Visual Studio kimeneti ablakában jelenik meg.

Ezen a ponton a webalkalmazás kell kinéznie:

![Az ASP.NET Core előtér-](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

toostop hello alkalmazásban, lépjen vissza a tooVisual Studio, és nyomja le az **Shift + F5**.

## <a name="add-a-stateful-back-end-service-tooyour-application"></a>Állapot-nyilvántartó háttérszolgáltatásnak tooyour alkalmazás felvétele
Most, hogy az ASP.NET Web API-szolgáltatás fut az alkalmazás, lépjen tovább, és adja hozzá egy állapotalapú szolgáltatás toostore egyes adatokat az alkalmazás.

A Service Fabric tooconsistently lehetővé teszi, és megbízhatóan tárolja az adatok jobb belül a szolgáltatás megbízható gyűjtemények használatával. Megbízható gyűjtemények olyan magas rendelkezésre állású és megbízható gyűjteményosztály, amelyek a megszokott tooanyone, akik C# gyűjtemények használatban van.

Ebben az oktatóanyagban létrehoz egy szolgáltatás, amely egy megbízható gyűjtemény egy számláló értékét tárolja.

1. A Megoldáskezelőben kattintson a jobb gombbal **szolgáltatások** belül hello projektet, és válassza a **Hozzáadás > új Service Fabric-szolgáltatás**.
   
    ![Új szolgáltatás tooan meglévő alkalmazás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. A hello **új Service Fabric-szolgáltatás** párbeszédpanelen válasszon **állapotalapú alkalmazások és szolgáltatások ASP.NET Core**, és hello szolgáltatást **VotingData** nyomja le az ENTER **OK**.

    ![A Visual Studio Új szolgáltatás párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    A service-projekt létrehozása után a felhasználó két szolgáltatást az alkalmazásban. Az alkalmazás továbbra toobuild, adhat hozzá további szolgáltatások hello azonos módon. Minden egyes lehet függetlenül rendszerverzióval ellátott és frissített.

3. hello következő oldal biztosít az ASP.NET Core projektsablonjai. A jelen oktatóanyag esetében válassza ki a **Web API**.

    ![ASP.NET-projekt típusának kiválasztása](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    A Visual Studio létrehoz egy szolgáltatási projektet, és megjeleníti őket a Megoldáskezelőben.

    ![Megoldáskezelő](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-hello-votedatacontrollercs-file"></a>Hello VoteDataController.cs fájl hozzáadása

A hello **VotingData** projektben kattintson jobb gombbal a hello **tartományvezérlők** mappát, majd válassza ki **Hozzáadás -> Új elem -> osztály**. Hello fájl "VoteDataController.cs" nevet, és kattintson a **Hozzáadás**. Cserélje ki hello következő hello fájl tartalmát, majd mentse a módosításokat.

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


## <a name="connect-hello-services"></a>Connect hello szolgáltatások
A következő lépésben rendszer összekapcsolja a hello két szolgáltatásokat és ellenőrizze a hello előtér-webkiszolgáló alkalmazás beolvasása és beállítása a szavazás hello háttér-szolgáltatás adatait.

A Service Fabric hogyan kommunikáljanak megbízható szolgáltatások teljes rugalmasságot biztosít. Egyetlen alkalmazásban lehetséges, hogy TCP-n keresztül elérhető szolgáltatások. Elképzelhető, hogy egy HTTP REST API-n keresztül érhető el, hogy más szolgáltatások és egyéb szolgáltatások továbbra is lehet webes szoftvercsatornák keresztül érhető el. Háttér hello lehetőségeit és hello mellékhatásokkal jár, lásd: [szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md).

Ebben az oktatóanyagban használjuk [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-hello-votescontrollercs-file"></a>Hello VotesController.cs fájl frissítése
A hello **VotingWeb** projektet, nyissa meg hello *Controllers/VotesController.cs* fájlt.  Cserélje le a hello `VotesController` osztály hello következőre definíciójának tartalma, akkor a módosítások mentéséhez.

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

## <a name="walk-through-hello-voting-sample-application"></a>Szavazás mintaalkalmazás hello bízná
hello szavazás alkalmazás két szolgáltatásból áll:
- Webes előtér-szolgáltatás (VotingWeb) – az ASP.NET Core webes előtér-szolgáltatás, hello weblap szolgál, és tesz elérhetővé webes API-k toocommunicate a hello háttérszolgáltatásban.
- Háttér-szolgáltatás (VotingData)-az ASP.NET Core a webszolgáltatást tesz elérhetővé, az API toostore hello szavazattal megbízható dictionary eredményezi a lemezen maradnak.

![Alkalmazásdiagram](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Amikor szavaz a hello alkalmazás hello a következő események következnek be:
1. JavaScript hello webes előtér-szolgáltatás hello szavazattal kérelem toohello webes API-t, egy HTTP PUT-kérelmet küld.

2. hello webes előtér-szolgáltatás egy proxy toolocate használ, és továbbítja a HTTP PUT kérés toohello háttér-szolgáltatásnak.

3. hello háttérszolgáltatásnak hello bejövő kérelem vesz igénybe, és a tárolók hello frissítve megbízható szótár, amely hello fürt csomópontja replikált toomultiple kap, és a lemezen tárolt eredményt. Minden hello alkalmazásadatok hello fürt tárolja, így nincs adatbázis szükséges.

## <a name="debug-in-visual-studio"></a>A Visual Studio hibakeresési
A Visual Studio alkalmazás nyomkövetésére használ egy helyi Service Fabric-fejlesztési fürtöt. A hibakeresési élmény tooyour környezettel rendelkezik hello beállítás tooadjust. Ebben az alkalmazásban tároljuk adatok a háttér-szolgáltatásban egy megbízható szótár használatával. A Visual Studio hello alkalmazás alapértelmezés szerint eltávolítja a hello hibakereső leállítása. Hello adatok hello alkalmazás eltávolítása hatására a hello háttér-szolgáltatás tooalso el kell távolítani. toopersist hello adatok hibakeresés a munkamenetek között, módosíthatja hello **alkalmazás hibakeresési módban** hello meg tulajdonságként **Voting** projektre a Visual Studióban.

toolook, mi történik, a lépéseket követve teljes hello hello kódban:
1. Nyissa meg hello **VotesController.cs** fájlt, és állítson be egy töréspontot hello webes API **Put** metódus (sor: 47) – hello fájlra a Visual Studio Solution Explorer hello kereshet.

2. Nyissa meg hello **VoteDataController.cs** fájlt, és állítson be egy töréspontot ezen webes API **Put** metódus (sor 50).

3. Lépjen vissza toohello böngésző és szavazó lehetőségre, vagy adja hozzá egy új szavazó beállítás. Kattintson az első töréspont hello hello webes első-end api-vezérlőben.
    
    1. Ez azért, ahol hello böngészőjében JavaScript hello küldi hello előtér-szolgáltatás egy kérelem toohello webes API-vezérlőben.
    
    ![Szavazás előtér-szolgáltatás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. Először a háttér-szolgáltatás hello URL-cím toohello ReverseProxy azt összeállításához **(1)**.
    3. Ezt követően kapni hello HTTP PUT kérés toohello ReverseProxy **(2)**.
    4. Végül hello a rendszer visszaadja a hello válasz hello háttérszolgáltatásnak toohello ügyfélről **(3)**.

4. Nyomja le az **F5** toocontinue
    1. Ekkor a számítógép hello töréspontot hello háttér-szolgáltatás.
    
    ![Szavazás háttér-szolgáltatás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. Hello metódus első sorában hello **(1)** hello használjuk `StateManager` tooget nevű megbízható szótár felvétele vagy `counts`.
    3. A megbízható szótárban értékek minden interakció tranzakció, a használatával szükséges utasítás **(2)** hoz létre, hogy a tranzakció.
    4. Hello tranzakcióban, majd frissítjük hello megfelelő kulcs a szavazás beállítás hello hello értékét, és véglegesíti hello művelet **(3)**. Hello véglegesítése után metódus ad vissza, hello adatok hello szótárban frissül, és a replikált tooother hello fürt csomópontja. hello adatok immár biztonságosan tárolja hello fürtben, és hello háttérszolgáltatásnak átveheti tooother csomópontok, adatok hello továbbra is fennáll.
5. Nyomja le az **F5** toocontinue

hibakeresési munkamenetben, nyomja meg az toostop hello **Shift + F5**.


## <a name="next-steps"></a>Következő lépések
Hello oktatóanyag ezen része megtanulta, hogyan:

> [!div class="checklist"]
> * Állapot-nyilvántartó megbízható szolgáltatásként az ASP.NET Core webes API-szolgáltatás létrehozása
> * Állapot nélküli webszolgáltatásként ASP.NET Core webalkalmazás-szolgáltatás létrehozása
> * Fordított proxy toocommunicate hello használata hello állapotalapú szolgáltatással

Előzetes toohello következő oktatóanyaga:
> [!div class="nextstepaction"]
> [Hello alkalmazás tooAzure telepítése](service-fabric-tutorial-deploy-app-to-party-cluster.md)
