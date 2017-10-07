---
title: "egy webalkalmazást az Azure virtuális gépen futó tooMongoDB csatlakozó aaaCreate"
description: "Egy oktatóanyag, amely útmutatást ad, hogy hogyan toouse Git toodeploy egy ASP.NET alkalmazás tooAzure App Service-ben csatlakoztatva tooMongoDB egy Azure virtuális gépen."
tags: azure-portal
services: app-service\web, virtual-machines
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: adf7a472-ae00-45a8-aec4-06247e21318b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/29/2016
ms.author: cephalin
ms.openlocfilehash: 1f5f42c28c3c294d92c9ebf1499374931d47c010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-that-connects-toomongodb-running-on-a-virtual-machine"></a>A webalkalmazás létrehozása az Azure virtuális gépen futó tooMongoDB csatlakozó
A Git, telepíthet egy ASP.NET alkalmazás tooAzure App Service Web Apps. Ebben az oktatóanyagban egy egyszerű előtér-ASP.NET MVC feladat lista alkalmazás, amely összeköti az Azure virtuális gépen futó tooa MongoDB-adatbázist fog létrehozni.  [MongoDB] [ MongoDB] van egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis. Után fut, és a fejlesztési számítógépen hello ASP.NET alkalmazás tesztelése, fel kell töltenie hello alkalmazás tooApp Service Web Apps Git használatával.

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="background-knowledge"></a>Háttér Tudásbázis
A Tudásbázis következő hello akkor hasznos, ebben az oktatóanyagban azonban nem kötelező:

* mongodb-protokolltámogatással hello C#-illesztőprogram. A fejleszt alkalmazásokat C# MongoDB további információkért lásd: hello MongoDB [CSharp nyelvű Center][MongoC#LangCenter]. 
* hello ASP.NET webszolgáltatás alkalmazás-keretrendszer. Megismerheti az összes hello a [ASP.net-webhely][ASP.NET].
* hello ASP .NET MVC webes alkalmazás-keretrendszer. Megismerheti az összes hello a [ASP.NET MVC webhely][MVCWebSite].
* Azure-t. Ismerkedés a olvasása [Azure][WindowsAzure].

## <a name="prerequisites"></a>Előfeltételek
* [A Visual Studio 2013 Express Web] [ VSEWeb] vagy [Visual Studio 2013][VSUlt]
* [Azure SDK for .NET](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* Egy aktív Microsoft Azure-előfizetés

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a>Hozzon létre egy virtuális gépet, és a MongoDB telepítése
Ez az oktatóanyag feltételezi, hogy létrehozott egy virtuális gépet az Azure-ban. Hello virtuális gép létrehozása után kell tooinstall MongoDB hello virtuális gépen:

* toocreate Windows virtuális gépek és a telepítés MongoDB, lásd: [MongoDB telepítése a Windows Server rendszert futtató Azure virtuális gépen][InstallMongoOnWindowsVM].

Hello virtuális gép létrehozása az Azure-ban, és MongoDB telepítése után meg arról, hogy tooremember hello DNS-neve lesz hello virtuális gép ("testlinuxvm.cloudapp.net", például) és a külső portra hello hello végpont megadott mongodb-protokolltámogatással.  Erre az információra hello oktatóanyag későbbi részében.

<a id="createapp"></a>

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása
Ebben a szakaszban létrehoz egy ASP.NET alkalmazás "Saját feladatlista" nevű Visual Studio használatával, és hajtsa végre egy kezdeti telepítési tooAzure App Service Web Apps. Hello alkalmazás helyileg fogja futtatni, de csatlakoztassa tooyour virtuális gépet az Azure-on, és létrehozott hello MongoDB-példány nem használható.

1. A Visual Studióban kattintson **új projekt**.
   
    ![Lap új projekt indítása][StartPageNewProject]
2. A hello **új projekt** ablakban hello bal oldali panelen, jelölje be az **Visual C#**, majd válassza ki **webes**. Hello középső ablaktábláján válassza ki **ASP.NET Web Application**. Hello lap alján, a projekt "MyTaskListApp" nevet, és kattintson **OK**.
   
    ![Új projekt párbeszédpanel][NewProjectMyTaskListApp]
3. A hello **új ASP.NET projekt** párbeszédpanelen jelölje ki **MVC**, és kattintson a **OK**.
   
    ![MVC-sablon kiválasztása][VS2013SelectMVCTemplate]
4. Ha még nem jelentkezett be a Microsoft Azure, a kért toosign fogja. Hello kér toosign kövesse az Azure.
5. Ha be van jelentkezve, indítsa el az App Service webalkalmazás konfigurálása. Adja meg a hello **Web App name**, **App Service-csomag**, **erőforráscsoport**, és **régió**, majd kattintson **létrehozása**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. Hello projekt létrehozása után várja hello web app toobe hello az Azure App Service-ben létrehozott **Azure App Service-tevékenység** ablak. Kattintson a **közzététele MyTaskListApp toothis webalkalmazás most**.
7. Kattintson a **Publish** (Közzététel) gombra.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    Ha az alapértelmezett ASP.NET-alkalmazás közzétett tooAzure App Service Web Apps, hello böngészőben elindul.

## <a name="install-hello-mongodb-c-driver"></a>Hello MongoDB C# illesztőprogram telepítése
MongoDB C#-alkalmazások pedig egy illesztőprogram, amely szükséges az ügyféloldali támogatást nyújt a helyi fejlesztési számítógépen tooinstall. hello C# illesztőprogram a Nugeten keresztül érhető el.

tooinstall hello MongoDB C# illesztőprogram:

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **MyTaskListApp** projektre, és válassza ki **kezelése NuGetPackages**.
   
    ![NuGet-csomagok kezelése][VS2013ManageNuGetPackages]
2. A hello **NuGet-csomagok kezelése** ablakban hello bal oldali ablaktáblában kattintson a **Online**. A hello **keresési Online** a jobb oldali hello mezőbe írja be a "mongodb.driver".  Kattintson a **telepítése** tooinstall hello illesztőprogram.
   
    ![MongoDB C# illesztőprogram keresése][SearchforMongoDBCSharpDriver]
3. Kattintson a **elfogadom** tooaccept hello 10gen, Inc. a licencfeltételeket.
4. Kattintson a **Bezárás** Ha hello illesztőprogram telepítve van.
    ![MongoDB-C# illesztőprogram][MongoDBCsharpDriverInstalled]

hello MongoDB C# illesztőprogram telepítve van.  Hivatkozások toohello **MongoDB.Bson**, **MongoDB.Driver**, és **MongoDB.Driver.Core** szalagtárak toohello projekt lettek hozzáadva.

![MongoDB C# illesztőprogram hivatkozások][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a>Modell hozzáadása
A **Megoldáskezelőben**, kattintson a jobb gombbal hello *modellek* mappa és **Hozzáadás** egy új **osztály** és adjon neki nevet *TaskModel.cs* .  A *TaskModel.cs*, cserélje le a következő kód hello hello meglévő kódot:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MongoDB.Bson.Serialization.Attributes;
    using MongoDB.Bson.Serialization.IdGenerators;
    using MongoDB.Bson;

    namespace MyTaskListApp.Models
    {
        public class MyTask
        {
            [BsonId(IdGenerator = typeof(CombGuidGenerator))]
            public Guid Id { get; set; }

            [BsonElement("Name")]
            public string Name { get; set; }

            [BsonElement("Category")]
            public string Category { get; set; }

            [BsonElement("Date")]
            public DateTime Date { get; set; }

            [BsonElement("CreatedDate")]
            public DateTime CreatedDate { get; set; }

        }
    }

## <a name="add-hello-data-access-layer"></a>Hello az adatelérési réteg hozzáadása
A **Megoldáskezelőben**, kattintson a jobb gombbal hello *MyTaskListApp* projekt és **Hozzáadás** egy **új mappa** nevű *DAL*.  Kattintson a jobb gombbal hello *DAL* mappa és **Hozzáadás** egy új **osztály**. Nevű hello osztály fájl *Dal.cs*.  A *Dal.cs*, cserélje le a következő kód hello hello meglévő kódot:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;


    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            private MongoServer mongoServer = null;
            private bool disposed = false;

            // toodo: update hello connection string with hello DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net"
            private string connectionString = "mongodb://mongodbsrv20151211.cloudapp.net";

            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  hello database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";

            // Default constructor.        
            public Dal()
            {
            }

            // Gets all Task items from hello MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }

            // Creates a Task and inserts it into hello collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }

            private IMongoCollection<MyTask> GetTasksCollection()
            {
                MongoClient client = new MongoClient(connectionString);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }

            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClient client = new MongoClient(connectionString);
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }

            # region IDisposable

            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }

            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                        if (mongoServer != null)
                        {
                            this.mongoServer.Disconnect();
                        }
                    }
                }

                this.disposed = true;
            }

            # endregion
        }
    }

## <a name="add-a-controller"></a>Vezérlő hozzáadása
Nyissa meg hello *Controllers\HomeController.cs* fájlt **Megoldáskezelőben** , és cserélje le a meglévő kódot hello hello alábbira:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using MyTaskListApp.Models;
    using System.Configuration;

    namespace MyTaskListApp.Controllers
    {
        public class HomeController : Controller, IDisposable
        {
            private Dal dal = new Dal();
            private bool disposed = false;
            //
            // GET: /MyTask/

            public ActionResult Index()
            {
                return View(dal.GetAllTasks());
            }

            //
            // GET: /MyTask/Create

            public ActionResult Create()
            {
                return View();
            }

            //
            // POST: /MyTask/Create

            [HttpPost]
            public ActionResult Create(MyTask task)
            {
                try
                {
                    dal.CreateTask(task);
                    return RedirectToAction("Index");
                }
                catch
                {
                    return View();
                }
            }

            public ActionResult About()
            {
                return View();
            }

            # region IDisposable

            new protected void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }

            new protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                        this.dal.Dispose();
                    }
                }

                this.disposed = true;
            }

            # endregion

        }
    }

## <a name="set-up-hello-styles"></a>Hello stílusok beállítása
toochange hello cím hello oldal, nyissa meg hello hello tetején *Views\Shared\\_Layout.cshtml* fájlt **Megoldáskezelőben** és cserélje le a "Alkalmazás neve" hello navigációs sávja fejlécében a "saját feladat Lista alkalmazás"így néz ki:

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

Az order tooset hello feladatlista menü, nyissa meg a hello *\Views\Home\Index.cshtml* fájlt, és cserélje le a következő kód hello hello meglévő kódot:

    @model IEnumerable<MyTaskListApp.Models.MyTask>

    @{
        ViewBag.Title = "My Task List";
    }

    <h2>My Task List</h2>

    <table border="1">
        <tr>
            <th>Task</th>
            <th>Category</th>
            <th>Date</th>

        </tr>

    @foreach (var item in Model) {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Name)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Category)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Date)
            </td>

        </tr>
    }

    </table>
    <div>  @Html.Partial("Create", new MyTaskListApp.Models.MyTask())</div>


tooadd hello képességét toocreate új tevékenység, kattintson a jobb gombbal a hello *Views\Home\\*  mappa és **Hozzáadás** egy **nézet**.  Hello nézet neve *létrehozása*. Cserélje le a hello kód hello alábbira:

    @model MyTaskListApp.Models.MyTask

    <script src="@Url.Content("~/Scripts/jquery-1.10.2.min.js")" type="text/javascript"></script>
    <script src="@Url.Content("~/Scripts/jquery.validate.min.js")" type="text/javascript"></script>
    <script src="@Url.Content("~/Scripts/jquery.validate.unobtrusive.min.js")" type="text/javascript"></script>

    @using (Html.BeginForm("Create", "Home")) {
        @Html.ValidationSummary(true)
        <fieldset>
            <legend>New Task</legend>

            <div class="editor-label">
                @Html.LabelFor(model => model.Name)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Name)
                @Html.ValidationMessageFor(model => model.Name)
            </div>

            <div class="editor-label">
                @Html.LabelFor(model => model.Category)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Category)
                @Html.ValidationMessageFor(model => model.Category)
            </div>

            <div class="editor-label">
                @Html.LabelFor(model => model.Date)
            </div>
            <div class="editor-field">
                @Html.EditorFor(model => model.Date)
                @Html.ValidationMessageFor(model => model.Date)
            </div>

            <p>
                <input type="submit" value="Create" />
            </p>
        </fieldset>
    }

**Megoldáskezelő** kell kinéznie:

![Megoldáskezelő][SolutionExplorerMyTaskListApp]

## <a name="set-hello-mongodb-connection-string"></a>Hello MongoDB kapcsolati karakterlánc beállítása
A **Megoldáskezelőben**, nyissa meg hello *DAL/Dal.cs* fájlt. Keresse meg a következő kódsort hello:

    private string connectionString = "mongodb://<vm-dns-name>";

Cserélje le `<vm-dns-name>` hello DNS-névvel fut hello létrehozott MongoDB hello virtuális gép [hozzon létre egy virtuális gépet, és telepítse a MongoDB] [ Create a virtual machine and install MongoDB] . lépését Ez az oktatóanyag.  toofind hello DNS-nevét a virtuális gépet, nyissa meg toohello Azure portált, válassza ki **virtuális gépek**, és keresse meg **DNS-név**.

Ha hello virtuális gép hello DNS-neve "testlinuxvm.cloudapp.net", és a MongoDB hello 27017 alapértelmezett porton figyel, hello kapcsolati karakterlánc kódsort hasonlóan fog kinézni:

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

Ha hello virtuális gép végpontjának mongodb egy másik külső portot határozza meg, akkor meg hello port hello kapcsolati karakterlánc:

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

A MongoDB-kapcsolati karakterláncok további információkért lásd: [kapcsolatok][MongoConnectionStrings].

## <a name="test-hello-local-deployment"></a>Hello helyi központi telepítés tesztelése
toorun az alkalmazást a fejlesztési számítógépen, válassza ki **Start Debugging** a hello **Debug** menü vagy nyomja le **F5**. Az IIS Express elindul, és a böngészőben megnyílik, és betölti az hello alkalmazás kezdőlapját.  Hozzáadhat egy új feladatot, amely megjelenik az Azure-ban, a virtuális gépen futó toohello MongoDB-adatbázist.

![A feladat alkalmazásában][TaskListAppBlank]

## <a name="publish-tooazure-app-service-web-apps"></a>TooAzure App Service Web Apps közzététele
Ebben a szakaszban a módosítások tooAzure App Service Web Apps tesznek közzé.

1. A Megoldáskezelőben kattintson a jobb gombbal **MyTaskListApp** újra kattintson **közzététel**.
2. Kattintson a **Publish** (Közzététel) gombra.
   
    Most látnia kell a webalkalmazás fut az Azure App Service-ben, és hello MongoDB adatbázis Azure virtuális gépek elérése.

## <a name="summary"></a>Összefoglalás
Most már sikeresen telepítette az ASP.NET alkalmazás tooAzure App Service Web Apps. tooview hello webalkalmazáshoz:

1. Jelentkezzen be hello Azure portálon.
2. Kattintson a **webalkalmazások**. 
3. Válassza ki a webalkalmazás hello **webalkalmazások** listája.

A fejleszt alkalmazásokat C# MongoDB további információkért lásd: [CSharp nyelvű Center][MongoC#LangCenter]. 

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- HYPERLINKS -->

[AzurePortal]: http://manage.windowsazure.com
[WindowsAzure]: http://www.windowsazure.com
[MongoC#LangCenter]: http://docs.mongodb.org/ecosystem/drivers/csharp/
[MVCWebSite]: http://www.asp.net/mvc
[ASP.NET]: http://www.asp.net/
[MongoConnectionStrings]: http://www.mongodb.org/display/DOCS/Connections
[MongoDB]: http://www.mongodb.org
[InstallMongoOnWindowsVM]:../virtual-machines/windows/classic/install-mongodb.md
[VSEWeb]: http://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express
[VSUlt]: http://www.microsoft.com/visualstudio/eng/2013-downloads

<!-- IMAGES -->


[StartPageNewProject]: ./media/web-sites-dotnet-store-data-mongodb-vm/NewProject.png
[NewProjectMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/NewProjectMyTaskListApp.png
[VS2013SelectMVCTemplate]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013SelectMVCTemplate.png
[VS2013DefaultMVCApplication]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013DefaultMVCApplication.png
[VS2013ManageNuGetPackages]: ./media/web-sites-dotnet-store-data-mongodb-vm/VS2013ManageNuGetPackages.png
[SearchforMongoDBCSharpDriver]: ./media/web-sites-dotnet-store-data-mongodb-vm/SearchforMongoDBCSharpDriver.png
[MongoDBCsharpDriverInstalled]: ./media/web-sites-dotnet-store-data-mongodb-vm/MongoDBCsharpDriverInstalled.png
[MongoDBCSharpDriverReferences]: ./media/web-sites-dotnet-store-data-mongodb-vm/MongoDBCSharpDriverReferences.png
[SolutionExplorerMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/SolutionExplorerMyTaskListApp.png
[TaskListAppBlank]: ./media/web-sites-dotnet-store-data-mongodb-vm/TaskListAppBlank.png
[WAWSCreateWebSite]: ./media/web-sites-dotnet-store-data-mongodb-vm/WAWSCreateWebSite.png
[WAWSDashboardMyTaskListApp]: ./media/web-sites-dotnet-store-data-mongodb-vm/WAWSDashboardMyTaskListApp.png
[Image9]: ./media/web-sites-dotnet-store-data-mongodb-vm/RepoReady.png
[Image10]: ./media/web-sites-dotnet-store-data-mongodb-vm/GitInstructions.png
[Image11]: ./media/web-sites-dotnet-store-data-mongodb-vm/GitDeploymentComplete.png

<!-- TOC BOOKMARKS -->
[Create a virtual machine and install MongoDB]: #virtualmachine
[Create and run hello My Task List ASP.NET application on your development computer]: #createapp
[Create an Azure web site]: #createwebsite
[Deploy hello ASP.NET application toohello web site using Git]: #deployapp
