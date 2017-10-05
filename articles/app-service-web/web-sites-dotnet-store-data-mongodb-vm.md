---
title: "Webalkalmazás létrehozása az Azure-ban, amely virtuális gépen futó MongoDB-csatlakozással rendelkezik"
description: "Egy oktatóanyag, amely útmutatást ad meg egy ASP.NET alkalmazás telepítése az Azure App Service, a Git segítségével csatlakozik a MongoDB egy Azure virtuális gép."
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
ms.openlocfilehash: a3f289ed9c764d0859573de4f834e042d0f103c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-in-azure-that-connects-to-mongodb-running-on-a-virtual-machine"></a>Webalkalmazás létrehozása az Azure-ban, amely virtuális gépen futó MongoDB-csatlakozással rendelkezik
Git használatával telepítheti is az ASP.NET-alkalmazások az Azure App Service Web Apps. Ebben az oktatóanyagban egy egyszerű előtér-ASP.NET MVC feladat csatlakozik a MongoDB-adatbázist, az Azure virtuális gépen futó alkalmazást fog létrehozni.  [MongoDB] [ MongoDB] van egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis. Után fut, és az ASP.NET-alkalmazást a fejlesztési számítógépen tesztelése, fel kell töltenie az alkalmazás az App Service Web Apps Git használatával.

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="background-knowledge"></a>Háttér Tudásbázis
A következő ismerete akkor hasznos, ebben az oktatóanyagban azonban nem kötelező:

* A C# MongoDB illesztőprogramját. A fejleszt alkalmazásokat C# MongoDB további információkért lásd: a MongoDB [CSharp nyelvű Center][MongoC#LangCenter]. 
* Az ASP .NET webes keretrendszer. Megismerheti az összes a a [ASP.net-webhely][ASP.NET].
* Az ASP .NET MVC webes keretrendszer. Megismerheti az összes a a [ASP.NET MVC webhely][MVCWebSite].
* Azure-t. Ismerkedés a olvasása [Azure][WindowsAzure].

## <a name="prerequisites"></a>Előfeltételek
* [A Visual Studio 2013 Express Web] [ VSEWeb] vagy [Visual Studio 2013][VSUlt]
* [Azure SDK for .NET](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* Egy aktív Microsoft Azure-előfizetés

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a>Hozzon létre egy virtuális gépet, és a MongoDB telepítése
Ez az oktatóanyag feltételezi, hogy létrehozott egy virtuális gépet az Azure-ban. A virtuális gép létrehozása után kell a MongoDB telepítése a virtuális gépen:

* Windows virtuális gép létrehozása, és telepítse a MongoDB, [MongoDB telepítése a Windows Server rendszert futtató Azure virtuális gépen][InstallMongoOnWindowsVM].

Miután létrehozta a virtuális gépet az Azure-ban, és a MongoDB telepített, ne felejtse el a virtuális gép ("testlinuxvm.cloudapp.net", például) és a külső portra DNS-nevét, amelyet a végpont a mongodb-protokolltámogatással.  Ezt az információt az oktatóanyag későbbi részében szüksége lesz.

<a id="createapp"></a>

## <a name="create-the-application"></a>Az alkalmazás létrehozása
Ebben a szakaszban létrehoz egy ASP.NET alkalmazás "Saját feladatlista" nevű Visual Studio használatával, és hajtsa végre egy kezdeti üzembe helyezése az Azure App Service Web Apps. Futtatja az alkalmazást helyileg, de a virtuális gépet az Azure csatlakozni, és nem használja a MongoDB-példány létrehozott.

1. A Visual Studióban kattintson **új projekt**.
   
    ![Lap új projekt indítása][StartPageNewProject]
2. Az a **új projekt** ablakban a bal oldali panelen, jelölje be **Visual C#**, majd válassza ki **webes**. A középső ablaktáblán válassza ki a **ASP.NET Web Application**. A lap alján, a projekt "MyTaskListApp" nevet, és kattintson a **OK**.
   
    ![Új projekt párbeszédpanel][NewProjectMyTaskListApp]
3. Az a **új ASP.NET projekt** párbeszédpanelen jelölje ki **MVC**, és kattintson a **OK**.
   
    ![MVC-sablon kiválasztása][VS2013SelectMVCTemplate]
4. Ha még nem jelentkezett be a Microsoft Azure, kérni fogja a bejelentkezéshez. Kövesse az utasításokat, Azure-ba való bejelentkezéshez.
5. Ha be van jelentkezve, indítsa el az App Service webalkalmazás konfigurálása. Adja meg a **Web App name**, **App Service-csomag**, **erőforráscsoport**, és **régió**, majd kattintson a **létrehozása**.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. A projekt létrehozása után várjon, amíg a webalkalmazás az Azure App Service-ben létrehozni a **Azure App Service-tevékenység** ablak. Kattintson a **most ezt a webes alkalmazás közzététele MyTaskListApp**.
7. Kattintson a **Publish** (Közzététel) gombra.
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    Miután az alapértelmezett ASP.NET-alkalmazás közzé van téve az Azure App Service Web Apps, akkor indul el a böngészőben.

## <a name="install-the-mongodb-c-driver"></a>Telepítse a MongoDB C# illesztőprogramját
MongoDB a C#-alkalmazások pedig egy illesztőprogram, amelyen telepítenie kell a helyi fejlesztési számítógépen ügyféloldali támogatást nyújt. A C# illesztőprogram Nugeten keresztül érhető el.

A MongoDB C# illesztőprogram telepítése:

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a **MyTaskListApp** projektre, és válassza ki **kezelése NuGetPackages**.
   
    ![NuGet-csomagok kezelése][VS2013ManageNuGetPackages]
2. Az a **NuGet-csomagok kezelése** ablakban a bal oldali ablaktáblán kattintson a **Online**. Az a **keresési Online** a jobb oldali mezőbe írja be a "mongodb.driver".  Kattintson a **telepítése** az illesztőprogram telepítéséhez.
   
    ![MongoDB C# illesztőprogram keresése][SearchforMongoDBCSharpDriver]
3. Kattintson a **elfogadom** 10gen, Inc. feltételeinek elfogadására.
4. Kattintson a **Bezárás** Ha az illesztőprogram telepítve van.
    ![MongoDB-C# illesztőprogram][MongoDBCsharpDriverInstalled]

A MongoDB C# illesztőprogram most már telepítve van.  Hivatkozása a **MongoDB.Bson**, **MongoDB.Driver**, és **MongoDB.Driver.Core** szalagtárak lettek hozzáadva a projekthez.

![MongoDB C# illesztőprogram hivatkozások][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a>Modell hozzáadása
A **Megoldáskezelőben**, kattintson a jobb gombbal a *modellek* mappa és **Hozzáadás** egy új **osztály** és adjon neki nevet *TaskModel.cs*.  A *TaskModel.cs*, cserélje le a meglévő kódot az alábbira:

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

## <a name="add-the-data-access-layer"></a>Adja hozzá az adatelérési réteg
A **Megoldáskezelőben**, kattintson a jobb gombbal a *MyTaskListApp* projekt és **Hozzáadás** egy **új mappa** nevű *DAL*.  Kattintson a jobb gombbal a *DAL* mappa és **Hozzáadás** egy új **osztály**. Nevezze el az osztály fájlt *Dal.cs*.  A *Dal.cs*, cserélje le a meglévő kódot az alábbira:

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

            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net"
            private string connectionString = "mongodb://mongodbsrv20151211.cloudapp.net";

            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";

            // Default constructor.        
            public Dal()
            {
            }

            // Gets all Task items from the MongoDB server.        
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

            // Creates a Task and inserts it into the collection in MongoDB.
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
Nyissa meg a *Controllers\HomeController.cs* fájlt **Megoldáskezelőben** , és cserélje le a meglévő kódot a következőre:

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

## <a name="set-up-the-styles"></a>A stílusok beállítása
Az oldal tetején a cím módosításához nyissa meg a *Views\Shared\\_Layout.cshtml* fájlt **Megoldáskezelőben** és cserélje le a "Alkalmazás neve" a navigációs sávja fejlécében a "saját feladatlista Alkalmazás"úgy tűnik, hogy az informatikai, ez például:

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

A feladatlista menü beállításához nyissa meg a *\Views\Home\Index.cshtml* fájlt, és cserélje le a meglévő kódot az alábbira:

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


Hozzáadása egy új feladatot létrehozni, kattintson a jobb gombbal a *Views\Home\\*  mappa és **Hozzáadás** egy **nézet**.  A nézet neve *létrehozása*. Cserélje le a kód a következő:

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

## <a name="set-the-mongodb-connection-string"></a>A MongoDB-kapcsolati karakterlánc beállítása
A **Megoldáskezelőben**, nyissa meg a *DAL/Dal.cs* fájlt. Keresse meg a következő kódsort:

    private string connectionString = "mongodb://<vm-dns-name>";

Cserélje le `<vm-dns-name>` a DNS-névvel, a MongoDB létrehozott futtató virtuális gép a [hozzon létre egy virtuális gépet, és telepítse a MongoDB] [ Create a virtual machine and install MongoDB] . lépését Ez az oktatóanyag.  A DNS-neve, a virtuális gép található, nyissa meg az Azure portálon, válassza ki a **virtuális gépek**, és keresse meg **DNS-név**.

Ha a DNS-neve, a virtuális gép "testlinuxvm.cloudapp.net" és az alapértelmezett porton 27017 MongoDB figyel, a kapcsolati karakterlánc kódsort hasonlóan fog kinézni:

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

Ha a virtuális gép végpontjának mongodb határoz meg egy másik külső portot, a következőket teheti meg a kapcsolati karakterláncban a port:

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

A MongoDB-kapcsolati karakterláncok további információkért lásd: [kapcsolatok][MongoConnectionStrings].

## <a name="test-the-local-deployment"></a>A helyi központi telepítés tesztelése
Futtassa az alkalmazást a fejlesztési számítógépen, válassza ki **Start Debugging** a a **Debug** menü vagy találat **F5**. Az IIS Express elindul, és a böngészőben megnyílik, és elindítja az alkalmazás kezdőlapját.  Hozzáadhat egy új feladatot, amelyek nem kerülnek be az Azure-ban, a virtuális gépen futó MongoDB-adatbázist.

![A feladat alkalmazásában][TaskListAppBlank]

## <a name="publish-to-azure-app-service-web-apps"></a>Az Azure App Service Web Apps alkalmazások közzététele
Ebben a szakaszban az Azure App Service Web Apps tesznek közzé a módosításokat.

1. A Megoldáskezelőben kattintson a jobb gombbal **MyTaskListApp** újra kattintson **közzététel**.
2. Kattintson a **Publish** (Közzététel) gombra.
   
    Most látnia kell a webalkalmazás fut az Azure App Service-ben, és a MongoDB adatbázis Azure virtuális gépek elérése.

## <a name="summary"></a>Összefoglalás
Most már sikeresen telepítette az Azure App Service Web Apps ASP.NET alkalmazást. A webes alkalmazás megtekintése:

1. Jelentkezzen be az Azure portálon.
2. Kattintson a **webalkalmazások**. 
3. Válassza ki a webalkalmazás a **webalkalmazások** listája.

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
[Create and run the My Task List ASP.NET application on your development computer]: #createapp
[Create an Azure web site]: #createwebsite
[Deploy the ASP.NET application to the web site using Git]: #deployapp
