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
# <a name="create-a-web-app-in-azure-that-connects-toomongodb-running-on-a-virtual-machine"></a><span data-ttu-id="88b60-103">A webalkalmazás létrehozása az Azure virtuális gépen futó tooMongoDB csatlakozó</span><span class="sxs-lookup"><span data-stu-id="88b60-103">Create a web app in Azure that connects tooMongoDB running on a virtual machine</span></span>
<span data-ttu-id="88b60-104">A Git, telepíthet egy ASP.NET alkalmazás tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="88b60-104">Using Git, you can deploy an ASP.NET application tooAzure App Service Web Apps.</span></span> <span data-ttu-id="88b60-105">Ebben az oktatóanyagban egy egyszerű előtér-ASP.NET MVC feladat lista alkalmazás, amely összeköti az Azure virtuális gépen futó tooa MongoDB-adatbázist fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="88b60-105">In this tutorial, you will build a simple front-end ASP.NET MVC task list application that connects tooa MongoDB database running on a virtual machine in Azure.</span></span>  <span data-ttu-id="88b60-106">[MongoDB] [ MongoDB] van egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="88b60-106">[MongoDB][MongoDB] is a popular open source, high performance NoSQL database.</span></span> <span data-ttu-id="88b60-107">Után fut, és a fejlesztési számítógépen hello ASP.NET alkalmazás tesztelése, fel kell töltenie hello alkalmazás tooApp Service Web Apps Git használatával.</span><span class="sxs-lookup"><span data-stu-id="88b60-107">After running and testing hello ASP.NET application on your development computer, you will upload hello application tooApp Service Web Apps using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="88b60-108">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="88b60-108">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="88b60-109">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="88b60-109">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="background-knowledge"></a><span data-ttu-id="88b60-110">Háttér Tudásbázis</span><span class="sxs-lookup"><span data-stu-id="88b60-110">Background knowledge</span></span>
<span data-ttu-id="88b60-111">A Tudásbázis következő hello akkor hasznos, ebben az oktatóanyagban azonban nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="88b60-111">Knowledge of hello following is useful for this tutorial, though not required:</span></span>

* <span data-ttu-id="88b60-112">mongodb-protokolltámogatással hello C#-illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="88b60-112">hello C# driver for MongoDB.</span></span> <span data-ttu-id="88b60-113">A fejleszt alkalmazásokat C# MongoDB további információkért lásd: hello MongoDB [CSharp nyelvű Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="88b60-113">For more information on developing C# applications against MongoDB, see hello MongoDB [CSharp Language Center][MongoC#LangCenter].</span></span> 
* <span data-ttu-id="88b60-114">hello ASP.NET webszolgáltatás alkalmazás-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="88b60-114">hello ASP .NET web application framework.</span></span> <span data-ttu-id="88b60-115">Megismerheti az összes hello a [ASP.net-webhely][ASP.NET].</span><span class="sxs-lookup"><span data-stu-id="88b60-115">You can learn all about it at hello [ASP.net website][ASP.NET].</span></span>
* <span data-ttu-id="88b60-116">hello ASP .NET MVC webes alkalmazás-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="88b60-116">hello ASP .NET MVC web application framework.</span></span> <span data-ttu-id="88b60-117">Megismerheti az összes hello a [ASP.NET MVC webhely][MVCWebSite].</span><span class="sxs-lookup"><span data-stu-id="88b60-117">You can learn all about it at hello [ASP.NET MVC website][MVCWebSite].</span></span>
* <span data-ttu-id="88b60-118">Azure-t.</span><span class="sxs-lookup"><span data-stu-id="88b60-118">Azure.</span></span> <span data-ttu-id="88b60-119">Ismerkedés a olvasása [Azure][WindowsAzure].</span><span class="sxs-lookup"><span data-stu-id="88b60-119">You can get started reading at [Azure][WindowsAzure].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88b60-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="88b60-120">Prerequisites</span></span>
* <span data-ttu-id="88b60-121">[A Visual Studio 2013 Express Web] [ VSEWeb] vagy [Visual Studio 2013][VSUlt]</span><span class="sxs-lookup"><span data-stu-id="88b60-121">[Visual Studio Express 2013 for Web][VSEWeb] or [Visual Studio 2013][VSUlt]</span></span>
* [<span data-ttu-id="88b60-122">Azure SDK for .NET</span><span class="sxs-lookup"><span data-stu-id="88b60-122">Azure SDK for .NET</span></span>](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* <span data-ttu-id="88b60-123">Egy aktív Microsoft Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="88b60-123">An active Microsoft Azure subscription</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a><span data-ttu-id="88b60-124">Hozzon létre egy virtuális gépet, és a MongoDB telepítése</span><span class="sxs-lookup"><span data-stu-id="88b60-124">Create a virtual machine and install MongoDB</span></span>
<span data-ttu-id="88b60-125">Ez az oktatóanyag feltételezi, hogy létrehozott egy virtuális gépet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="88b60-125">This tutorial assumes you have created a virtual machine in Azure.</span></span> <span data-ttu-id="88b60-126">Hello virtuális gép létrehozása után kell tooinstall MongoDB hello virtuális gépen:</span><span class="sxs-lookup"><span data-stu-id="88b60-126">After creating hello virtual machine you need tooinstall MongoDB on hello virtual machine:</span></span>

* <span data-ttu-id="88b60-127">toocreate Windows virtuális gépek és a telepítés MongoDB, lásd: [MongoDB telepítése a Windows Server rendszert futtató Azure virtuális gépen][InstallMongoOnWindowsVM].</span><span class="sxs-lookup"><span data-stu-id="88b60-127">toocreate a Windows virtual machine and install MongoDB, see [Install MongoDB on a virtual machine running Windows Server in Azure][InstallMongoOnWindowsVM].</span></span>

<span data-ttu-id="88b60-128">Hello virtuális gép létrehozása az Azure-ban, és MongoDB telepítése után meg arról, hogy tooremember hello DNS-neve lesz hello virtuális gép ("testlinuxvm.cloudapp.net", például) és a külső portra hello hello végpont megadott mongodb-protokolltámogatással.</span><span class="sxs-lookup"><span data-stu-id="88b60-128">After you have created hello virtual machine in Azure and installed MongoDB, be sure tooremember hello DNS name of hello virtual machine ("testlinuxvm.cloudapp.net", for example) and hello external port for MongoDB that you specified in hello endpoint.</span></span>  <span data-ttu-id="88b60-129">Erre az információra hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="88b60-129">You will need this information later in hello tutorial.</span></span>

<a id="createapp"></a>

## <a name="create-hello-application"></a><span data-ttu-id="88b60-130">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="88b60-130">Create hello application</span></span>
<span data-ttu-id="88b60-131">Ebben a szakaszban létrehoz egy ASP.NET alkalmazás "Saját feladatlista" nevű Visual Studio használatával, és hajtsa végre egy kezdeti telepítési tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="88b60-131">In this section you will create an ASP.NET application called "My Task List" by using Visual Studio and perform an initial deployment tooAzure App Service Web Apps.</span></span> <span data-ttu-id="88b60-132">Hello alkalmazás helyileg fogja futtatni, de csatlakoztassa tooyour virtuális gépet az Azure-on, és létrehozott hello MongoDB-példány nem használható.</span><span class="sxs-lookup"><span data-stu-id="88b60-132">You will run hello application locally, but it will connect tooyour virtual machine on Azure and use hello MongoDB instance that you created there.</span></span>

1. <span data-ttu-id="88b60-133">A Visual Studióban kattintson **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="88b60-133">In Visual Studio, click **New Project**.</span></span>
   
    ![Lap új projekt indítása][StartPageNewProject]
2. <span data-ttu-id="88b60-135">A hello **új projekt** ablakban hello bal oldali panelen, jelölje be az **Visual C#**, majd válassza ki **webes**.</span><span class="sxs-lookup"><span data-stu-id="88b60-135">In hello **New Project** window, in hello left pane, select **Visual C#**, and then select **Web**.</span></span> <span data-ttu-id="88b60-136">Hello középső ablaktábláján válassza ki **ASP.NET Web Application**.</span><span class="sxs-lookup"><span data-stu-id="88b60-136">In hello middle pane, select **ASP.NET  Web Application**.</span></span> <span data-ttu-id="88b60-137">Hello lap alján, a projekt "MyTaskListApp" nevet, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="88b60-137">At hello bottom, name your project "MyTaskListApp," and then click **OK**.</span></span>
   
    ![Új projekt párbeszédpanel][NewProjectMyTaskListApp]
3. <span data-ttu-id="88b60-139">A hello **új ASP.NET projekt** párbeszédpanelen jelölje ki **MVC**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="88b60-139">In hello **New ASP.NET Project** dialog box, select **MVC**, and then click **OK**.</span></span>
   
    ![MVC-sablon kiválasztása][VS2013SelectMVCTemplate]
4. <span data-ttu-id="88b60-141">Ha még nem jelentkezett be a Microsoft Azure, a kért toosign fogja.</span><span class="sxs-lookup"><span data-stu-id="88b60-141">If you aren't already signed into Microsoft Azure, you will be prompted toosign in.</span></span> <span data-ttu-id="88b60-142">Hello kér toosign kövesse az Azure.</span><span class="sxs-lookup"><span data-stu-id="88b60-142">Follow hello prompts toosign into Azure.</span></span>
5. <span data-ttu-id="88b60-143">Ha be van jelentkezve, indítsa el az App Service webalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="88b60-143">Once you are signed in, you can start configuring your App Service web app.</span></span> <span data-ttu-id="88b60-144">Adja meg a hello **Web App name**, **App Service-csomag**, **erőforráscsoport**, és **régió**, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="88b60-144">Specify hello **Web App name**, **App Service plan**, **Resource group**, and **Region**, then click **Create**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. <span data-ttu-id="88b60-145">Hello projekt létrehozása után várja hello web app toobe hello az Azure App Service-ben létrehozott **Azure App Service-tevékenység** ablak.</span><span class="sxs-lookup"><span data-stu-id="88b60-145">After hello project creation completes, wait for hello web app toobe created in Azure App Service as indicated in hello **Azure App Service Activity** window.</span></span> <span data-ttu-id="88b60-146">Kattintson a **közzététele MyTaskListApp toothis webalkalmazás most**.</span><span class="sxs-lookup"><span data-stu-id="88b60-146">Then, click **Publish MyTaskListApp toothis Web App now**.</span></span>
7. <span data-ttu-id="88b60-147">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="88b60-147">Click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    <span data-ttu-id="88b60-148">Ha az alapértelmezett ASP.NET-alkalmazás közzétett tooAzure App Service Web Apps, hello böngészőben elindul.</span><span class="sxs-lookup"><span data-stu-id="88b60-148">Once your default ASP.NET application is published tooAzure App Service Web Apps, it will be launched in hello browser.</span></span>

## <a name="install-hello-mongodb-c-driver"></a><span data-ttu-id="88b60-149">Hello MongoDB C# illesztőprogram telepítése</span><span class="sxs-lookup"><span data-stu-id="88b60-149">Install hello MongoDB C# driver</span></span>
<span data-ttu-id="88b60-150">MongoDB C#-alkalmazások pedig egy illesztőprogram, amely szükséges az ügyféloldali támogatást nyújt a helyi fejlesztési számítógépen tooinstall.</span><span class="sxs-lookup"><span data-stu-id="88b60-150">MongoDB offers client-side support for C# applications through a driver, which you need tooinstall on your local development computer.</span></span> <span data-ttu-id="88b60-151">hello C# illesztőprogram a Nugeten keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="88b60-151">hello C# driver is available through NuGet.</span></span>

<span data-ttu-id="88b60-152">tooinstall hello MongoDB C# illesztőprogram:</span><span class="sxs-lookup"><span data-stu-id="88b60-152">tooinstall hello MongoDB C# driver:</span></span>

1. <span data-ttu-id="88b60-153">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **MyTaskListApp** projektre, és válassza ki **kezelése NuGetPackages**.</span><span class="sxs-lookup"><span data-stu-id="88b60-153">In **Solution Explorer**, right-click hello **MyTaskListApp** project and select **Manage NuGetPackages**.</span></span>
   
    ![NuGet-csomagok kezelése][VS2013ManageNuGetPackages]
2. <span data-ttu-id="88b60-155">A hello **NuGet-csomagok kezelése** ablakban hello bal oldali ablaktáblában kattintson a **Online**.</span><span class="sxs-lookup"><span data-stu-id="88b60-155">In hello **Manage NuGet Packages** window, in hello left pane, click **Online**.</span></span> <span data-ttu-id="88b60-156">A hello **keresési Online** a jobb oldali hello mezőbe írja be a "mongodb.driver".</span><span class="sxs-lookup"><span data-stu-id="88b60-156">In hello **Search Online** box on hello right, type "mongodb.driver".</span></span>  <span data-ttu-id="88b60-157">Kattintson a **telepítése** tooinstall hello illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="88b60-157">Click **Install** tooinstall hello driver.</span></span>
   
    ![MongoDB C# illesztőprogram keresése][SearchforMongoDBCSharpDriver]
3. <span data-ttu-id="88b60-159">Kattintson a **elfogadom** tooaccept hello 10gen, Inc. a licencfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="88b60-159">Click **I Accept** tooaccept hello 10gen, Inc. license terms.</span></span>
4. <span data-ttu-id="88b60-160">Kattintson a **Bezárás** Ha hello illesztőprogram telepítve van.</span><span class="sxs-lookup"><span data-stu-id="88b60-160">Click **Close** after hello driver has installed.</span></span>
    <span data-ttu-id="88b60-161">![MongoDB-C# illesztőprogram][MongoDBCsharpDriverInstalled]</span><span class="sxs-lookup"><span data-stu-id="88b60-161">![MongoDB C# Driver Installed][MongoDBCsharpDriverInstalled]</span></span>

<span data-ttu-id="88b60-162">hello MongoDB C# illesztőprogram telepítve van.</span><span class="sxs-lookup"><span data-stu-id="88b60-162">hello MongoDB C# driver is now installed.</span></span>  <span data-ttu-id="88b60-163">Hivatkozások toohello **MongoDB.Bson**, **MongoDB.Driver**, és **MongoDB.Driver.Core** szalagtárak toohello projekt lettek hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="88b60-163">References toohello **MongoDB.Bson**, **MongoDB.Driver**, and **MongoDB.Driver.Core**  libraries have been added toohello project.</span></span>

![MongoDB C# illesztőprogram hivatkozások][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a><span data-ttu-id="88b60-165">Modell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="88b60-165">Add a model</span></span>
<span data-ttu-id="88b60-166">A **Megoldáskezelőben**, kattintson a jobb gombbal hello *modellek* mappa és **Hozzáadás** egy új **osztály** és adjon neki nevet *TaskModel.cs* .</span><span class="sxs-lookup"><span data-stu-id="88b60-166">In **Solution Explorer**, right-click hello *Models* folder and **Add** a new **Class** and name it *TaskModel.cs*.</span></span>  <span data-ttu-id="88b60-167">A *TaskModel.cs*, cserélje le a következő kód hello hello meglévő kódot:</span><span class="sxs-lookup"><span data-stu-id="88b60-167">In *TaskModel.cs*, replace hello existing code with hello following code:</span></span>

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

## <a name="add-hello-data-access-layer"></a><span data-ttu-id="88b60-168">Hello az adatelérési réteg hozzáadása</span><span class="sxs-lookup"><span data-stu-id="88b60-168">Add hello data access layer</span></span>
<span data-ttu-id="88b60-169">A **Megoldáskezelőben**, kattintson a jobb gombbal hello *MyTaskListApp* projekt és **Hozzáadás** egy **új mappa** nevű *DAL*.</span><span class="sxs-lookup"><span data-stu-id="88b60-169">In **Solution Explorer**, right-click hello *MyTaskListApp* project and **Add** a **New Folder** named *DAL*.</span></span>  <span data-ttu-id="88b60-170">Kattintson a jobb gombbal hello *DAL* mappa és **Hozzáadás** egy új **osztály**.</span><span class="sxs-lookup"><span data-stu-id="88b60-170">Right-click hello *DAL* folder and **Add** a new **Class**.</span></span> <span data-ttu-id="88b60-171">Nevű hello osztály fájl *Dal.cs*.</span><span class="sxs-lookup"><span data-stu-id="88b60-171">Name hello class file *Dal.cs*.</span></span>  <span data-ttu-id="88b60-172">A *Dal.cs*, cserélje le a következő kód hello hello meglévő kódot:</span><span class="sxs-lookup"><span data-stu-id="88b60-172">In *Dal.cs*, replace hello existing code with hello following code:</span></span>

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

## <a name="add-a-controller"></a><span data-ttu-id="88b60-173">Vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="88b60-173">Add a controller</span></span>
<span data-ttu-id="88b60-174">Nyissa meg hello *Controllers\HomeController.cs* fájlt **Megoldáskezelőben** , és cserélje le a meglévő kódot hello hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="88b60-174">Open hello *Controllers\HomeController.cs* file in **Solution Explorer** and replace hello existing code with hello following:</span></span>

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

## <a name="set-up-hello-styles"></a><span data-ttu-id="88b60-175">Hello stílusok beállítása</span><span class="sxs-lookup"><span data-stu-id="88b60-175">Set up hello styles</span></span>
<span data-ttu-id="88b60-176">toochange hello cím hello oldal, nyissa meg hello hello tetején *Views\Shared\\_Layout.cshtml* fájlt **Megoldáskezelőben** és cserélje le a "Alkalmazás neve" hello navigációs sávja fejlécében a "saját feladat Lista alkalmazás"így néz ki:</span><span class="sxs-lookup"><span data-stu-id="88b60-176">toochange hello title at hello top of hello page, open hello *Views\Shared\\_Layout.cshtml* file in **Solution Explorer** and replace "Application name" in hello navbar header with "My Task List Application" so that it looks like this:</span></span>

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

<span data-ttu-id="88b60-177">Az order tooset hello feladatlista menü, nyissa meg a hello *\Views\Home\Index.cshtml* fájlt, és cserélje le a következő kód hello hello meglévő kódot:</span><span class="sxs-lookup"><span data-stu-id="88b60-177">In order tooset up hello Task List menu, open hello *\Views\Home\Index.cshtml* file and replace hello existing code with hello following code:</span></span>

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


<span data-ttu-id="88b60-178">tooadd hello képességét toocreate új tevékenység, kattintson a jobb gombbal a hello *Views\Home\\*  mappa és **Hozzáadás** egy **nézet**.</span><span class="sxs-lookup"><span data-stu-id="88b60-178">tooadd hello ability toocreate a new task, right-click hello *Views\Home\\* folder and **Add** a **View**.</span></span>  <span data-ttu-id="88b60-179">Hello nézet neve *létrehozása*.</span><span class="sxs-lookup"><span data-stu-id="88b60-179">Name hello view *Create*.</span></span> <span data-ttu-id="88b60-180">Cserélje le a hello kód hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="88b60-180">Replace hello code with hello following:</span></span>

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

<span data-ttu-id="88b60-181">**Megoldáskezelő** kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="88b60-181">**Solution Explorer** should look like this:</span></span>

![Megoldáskezelő][SolutionExplorerMyTaskListApp]

## <a name="set-hello-mongodb-connection-string"></a><span data-ttu-id="88b60-183">Hello MongoDB kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="88b60-183">Set hello MongoDB connection string</span></span>
<span data-ttu-id="88b60-184">A **Megoldáskezelőben**, nyissa meg hello *DAL/Dal.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="88b60-184">In **Solution Explorer**, open hello *DAL/Dal.cs* file.</span></span> <span data-ttu-id="88b60-185">Keresse meg a következő kódsort hello:</span><span class="sxs-lookup"><span data-stu-id="88b60-185">Find hello following line of code:</span></span>

    private string connectionString = "mongodb://<vm-dns-name>";

<span data-ttu-id="88b60-186">Cserélje le `<vm-dns-name>` hello DNS-névvel fut hello létrehozott MongoDB hello virtuális gép [hozzon létre egy virtuális gépet, és telepítse a MongoDB] [ Create a virtual machine and install MongoDB] . lépését Ez az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="88b60-186">Replace `<vm-dns-name>` with hello DNS name of hello virtual machine running MongoDB you created in hello [Create a virtual machine and install MongoDB][Create a virtual machine and install MongoDB] step of this tutorial.</span></span>  <span data-ttu-id="88b60-187">toofind hello DNS-nevét a virtuális gépet, nyissa meg toohello Azure portált, válassza ki **virtuális gépek**, és keresse meg **DNS-név**.</span><span class="sxs-lookup"><span data-stu-id="88b60-187">toofind hello DNS name of your virtual machine, go toohello Azure Portal, select **Virtual Machines**, and find **DNS Name**.</span></span>

<span data-ttu-id="88b60-188">Ha hello virtuális gép hello DNS-neve "testlinuxvm.cloudapp.net", és a MongoDB hello 27017 alapértelmezett porton figyel, hello kapcsolati karakterlánc kódsort hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="88b60-188">If hello DNS name of hello virtual machine is "testlinuxvm.cloudapp.net" and MongoDB is listening on hello default port 27017, hello connection string line of code will look like:</span></span>

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

<span data-ttu-id="88b60-189">Ha hello virtuális gép végpontjának mongodb egy másik külső portot határozza meg, akkor meg hello port hello kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="88b60-189">If hello virtual machine endpoint specifies a different external port for MongoDB, you can specifiy hello port in hello connection string:</span></span>

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

<span data-ttu-id="88b60-190">A MongoDB-kapcsolati karakterláncok további információkért lásd: [kapcsolatok][MongoConnectionStrings].</span><span class="sxs-lookup"><span data-stu-id="88b60-190">For more information on MongoDB connection strings, see [Connections][MongoConnectionStrings].</span></span>

## <a name="test-hello-local-deployment"></a><span data-ttu-id="88b60-191">Hello helyi központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="88b60-191">Test hello local deployment</span></span>
<span data-ttu-id="88b60-192">toorun az alkalmazást a fejlesztési számítógépen, válassza ki **Start Debugging** a hello **Debug** menü vagy nyomja le **F5**.</span><span class="sxs-lookup"><span data-stu-id="88b60-192">toorun your application on your development computer, select **Start Debugging** from hello **Debug** menu or hit **F5**.</span></span> <span data-ttu-id="88b60-193">Az IIS Express elindul, és a böngészőben megnyílik, és betölti az hello alkalmazás kezdőlapját.</span><span class="sxs-lookup"><span data-stu-id="88b60-193">IIS Express starts and a browser opens and launches hello application's home page.</span></span>  <span data-ttu-id="88b60-194">Hozzáadhat egy új feladatot, amely megjelenik az Azure-ban, a virtuális gépen futó toohello MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="88b60-194">You can add a new task, which will be added toohello MongoDB database running on your virtual machine in Azure.</span></span>

![A feladat alkalmazásában][TaskListAppBlank]

## <a name="publish-tooazure-app-service-web-apps"></a><span data-ttu-id="88b60-196">TooAzure App Service Web Apps közzététele</span><span class="sxs-lookup"><span data-stu-id="88b60-196">Publish tooAzure App Service Web Apps</span></span>
<span data-ttu-id="88b60-197">Ebben a szakaszban a módosítások tooAzure App Service Web Apps tesznek közzé.</span><span class="sxs-lookup"><span data-stu-id="88b60-197">In this section you will publish your changes tooAzure App Service Web Apps.</span></span>

1. <span data-ttu-id="88b60-198">A Megoldáskezelőben kattintson a jobb gombbal **MyTaskListApp** újra kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="88b60-198">In Solution Explorer, right-click **MyTaskListApp** again and click **Publish**.</span></span>
2. <span data-ttu-id="88b60-199">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="88b60-199">Click **Publish**.</span></span>
   
    <span data-ttu-id="88b60-200">Most látnia kell a webalkalmazás fut az Azure App Service-ben, és hello MongoDB adatbázis Azure virtuális gépek elérése.</span><span class="sxs-lookup"><span data-stu-id="88b60-200">You should now see your web app running in Azure App Service and accessing hello MongoDB database in Azure Virtual Machines.</span></span>

## <a name="summary"></a><span data-ttu-id="88b60-201">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="88b60-201">Summary</span></span>
<span data-ttu-id="88b60-202">Most már sikeresen telepítette az ASP.NET alkalmazás tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="88b60-202">You have now successfully deployed your ASP.NET application tooAzure App Service Web Apps.</span></span> <span data-ttu-id="88b60-203">tooview hello webalkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="88b60-203">tooview hello web app:</span></span>

1. <span data-ttu-id="88b60-204">Jelentkezzen be hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="88b60-204">Log into hello Azure Portal.</span></span>
2. <span data-ttu-id="88b60-205">Kattintson a **webalkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="88b60-205">Click **Web apps**.</span></span> 
3. <span data-ttu-id="88b60-206">Válassza ki a webalkalmazás hello **webalkalmazások** listája.</span><span class="sxs-lookup"><span data-stu-id="88b60-206">Select your web app in hello **Web Apps** list.</span></span>

<span data-ttu-id="88b60-207">A fejleszt alkalmazásokat C# MongoDB további információkért lásd: [CSharp nyelvű Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="88b60-207">For more information on developing C# applications against MongoDB, see [CSharp Language Center][MongoC#LangCenter].</span></span> 

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
