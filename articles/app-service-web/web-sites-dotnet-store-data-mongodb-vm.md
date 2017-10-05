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
# <a name="create-a-web-app-in-azure-that-connects-to-mongodb-running-on-a-virtual-machine"></a><span data-ttu-id="e8c95-103">Webalkalmazás létrehozása az Azure-ban, amely virtuális gépen futó MongoDB-csatlakozással rendelkezik</span><span class="sxs-lookup"><span data-stu-id="e8c95-103">Create a web app in Azure that connects to MongoDB running on a virtual machine</span></span>
<span data-ttu-id="e8c95-104">Git használatával telepítheti is az ASP.NET-alkalmazások az Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="e8c95-104">Using Git, you can deploy an ASP.NET application to Azure App Service Web Apps.</span></span> <span data-ttu-id="e8c95-105">Ebben az oktatóanyagban egy egyszerű előtér-ASP.NET MVC feladat csatlakozik a MongoDB-adatbázist, az Azure virtuális gépen futó alkalmazást fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e8c95-105">In this tutorial, you will build a simple front-end ASP.NET MVC task list application that connects to a MongoDB database running on a virtual machine in Azure.</span></span>  <span data-ttu-id="e8c95-106">[MongoDB] [ MongoDB] van egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e8c95-106">[MongoDB][MongoDB] is a popular open source, high performance NoSQL database.</span></span> <span data-ttu-id="e8c95-107">Után fut, és az ASP.NET-alkalmazást a fejlesztési számítógépen tesztelése, fel kell töltenie az alkalmazás az App Service Web Apps Git használatával.</span><span class="sxs-lookup"><span data-stu-id="e8c95-107">After running and testing the ASP.NET application on your development computer, you will upload the application to App Service Web Apps using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="e8c95-108">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="e8c95-108">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="e8c95-109">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="e8c95-109">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="background-knowledge"></a><span data-ttu-id="e8c95-110">Háttér Tudásbázis</span><span class="sxs-lookup"><span data-stu-id="e8c95-110">Background knowledge</span></span>
<span data-ttu-id="e8c95-111">A következő ismerete akkor hasznos, ebben az oktatóanyagban azonban nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="e8c95-111">Knowledge of the following is useful for this tutorial, though not required:</span></span>

* <span data-ttu-id="e8c95-112">A C# MongoDB illesztőprogramját.</span><span class="sxs-lookup"><span data-stu-id="e8c95-112">The C# driver for MongoDB.</span></span> <span data-ttu-id="e8c95-113">A fejleszt alkalmazásokat C# MongoDB további információkért lásd: a MongoDB [CSharp nyelvű Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="e8c95-113">For more information on developing C# applications against MongoDB, see the MongoDB [CSharp Language Center][MongoC#LangCenter].</span></span> 
* <span data-ttu-id="e8c95-114">Az ASP .NET webes keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="e8c95-114">The ASP .NET web application framework.</span></span> <span data-ttu-id="e8c95-115">Megismerheti az összes a a [ASP.net-webhely][ASP.NET].</span><span class="sxs-lookup"><span data-stu-id="e8c95-115">You can learn all about it at the [ASP.net website][ASP.NET].</span></span>
* <span data-ttu-id="e8c95-116">Az ASP .NET MVC webes keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="e8c95-116">The ASP .NET MVC web application framework.</span></span> <span data-ttu-id="e8c95-117">Megismerheti az összes a a [ASP.NET MVC webhely][MVCWebSite].</span><span class="sxs-lookup"><span data-stu-id="e8c95-117">You can learn all about it at the [ASP.NET MVC website][MVCWebSite].</span></span>
* <span data-ttu-id="e8c95-118">Azure-t.</span><span class="sxs-lookup"><span data-stu-id="e8c95-118">Azure.</span></span> <span data-ttu-id="e8c95-119">Ismerkedés a olvasása [Azure][WindowsAzure].</span><span class="sxs-lookup"><span data-stu-id="e8c95-119">You can get started reading at [Azure][WindowsAzure].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8c95-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e8c95-120">Prerequisites</span></span>
* <span data-ttu-id="e8c95-121">[A Visual Studio 2013 Express Web] [ VSEWeb] vagy [Visual Studio 2013][VSUlt]</span><span class="sxs-lookup"><span data-stu-id="e8c95-121">[Visual Studio Express 2013 for Web][VSEWeb] or [Visual Studio 2013][VSUlt]</span></span>
* [<span data-ttu-id="e8c95-122">Azure SDK for .NET</span><span class="sxs-lookup"><span data-stu-id="e8c95-122">Azure SDK for .NET</span></span>](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)
* <span data-ttu-id="e8c95-123">Egy aktív Microsoft Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="e8c95-123">An active Microsoft Azure subscription</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<a id="virtualmachine"></a> 

## <a name="create-a-virtual-machine-and-install-mongodb"></a><span data-ttu-id="e8c95-124">Hozzon létre egy virtuális gépet, és a MongoDB telepítése</span><span class="sxs-lookup"><span data-stu-id="e8c95-124">Create a virtual machine and install MongoDB</span></span>
<span data-ttu-id="e8c95-125">Ez az oktatóanyag feltételezi, hogy létrehozott egy virtuális gépet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e8c95-125">This tutorial assumes you have created a virtual machine in Azure.</span></span> <span data-ttu-id="e8c95-126">A virtuális gép létrehozása után kell a MongoDB telepítése a virtuális gépen:</span><span class="sxs-lookup"><span data-stu-id="e8c95-126">After creating the virtual machine you need to install MongoDB on the virtual machine:</span></span>

* <span data-ttu-id="e8c95-127">Windows virtuális gép létrehozása, és telepítse a MongoDB, [MongoDB telepítése a Windows Server rendszert futtató Azure virtuális gépen][InstallMongoOnWindowsVM].</span><span class="sxs-lookup"><span data-stu-id="e8c95-127">To create a Windows virtual machine and install MongoDB, see [Install MongoDB on a virtual machine running Windows Server in Azure][InstallMongoOnWindowsVM].</span></span>

<span data-ttu-id="e8c95-128">Miután létrehozta a virtuális gépet az Azure-ban, és a MongoDB telepített, ne felejtse el a virtuális gép ("testlinuxvm.cloudapp.net", például) és a külső portra DNS-nevét, amelyet a végpont a mongodb-protokolltámogatással.</span><span class="sxs-lookup"><span data-stu-id="e8c95-128">After you have created the virtual machine in Azure and installed MongoDB, be sure to remember the DNS name of the virtual machine ("testlinuxvm.cloudapp.net", for example) and the external port for MongoDB that you specified in the endpoint.</span></span>  <span data-ttu-id="e8c95-129">Ezt az információt az oktatóanyag későbbi részében szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="e8c95-129">You will need this information later in the tutorial.</span></span>

<a id="createapp"></a>

## <a name="create-the-application"></a><span data-ttu-id="e8c95-130">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8c95-130">Create the application</span></span>
<span data-ttu-id="e8c95-131">Ebben a szakaszban létrehoz egy ASP.NET alkalmazás "Saját feladatlista" nevű Visual Studio használatával, és hajtsa végre egy kezdeti üzembe helyezése az Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="e8c95-131">In this section you will create an ASP.NET application called "My Task List" by using Visual Studio and perform an initial deployment to Azure App Service Web Apps.</span></span> <span data-ttu-id="e8c95-132">Futtatja az alkalmazást helyileg, de a virtuális gépet az Azure csatlakozni, és nem használja a MongoDB-példány létrehozott.</span><span class="sxs-lookup"><span data-stu-id="e8c95-132">You will run the application locally, but it will connect to your virtual machine on Azure and use the MongoDB instance that you created there.</span></span>

1. <span data-ttu-id="e8c95-133">A Visual Studióban kattintson **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-133">In Visual Studio, click **New Project**.</span></span>
   
    ![Lap új projekt indítása][StartPageNewProject]
2. <span data-ttu-id="e8c95-135">Az a **új projekt** ablakban a bal oldali panelen, jelölje be **Visual C#**, majd válassza ki **webes**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-135">In the **New Project** window, in the left pane, select **Visual C#**, and then select **Web**.</span></span> <span data-ttu-id="e8c95-136">A középső ablaktáblán válassza ki a **ASP.NET Web Application**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-136">In the middle pane, select **ASP.NET  Web Application**.</span></span> <span data-ttu-id="e8c95-137">A lap alján, a projekt "MyTaskListApp" nevet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-137">At the bottom, name your project "MyTaskListApp," and then click **OK**.</span></span>
   
    ![Új projekt párbeszédpanel][NewProjectMyTaskListApp]
3. <span data-ttu-id="e8c95-139">Az a **új ASP.NET projekt** párbeszédpanelen jelölje ki **MVC**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-139">In the **New ASP.NET Project** dialog box, select **MVC**, and then click **OK**.</span></span>
   
    ![MVC-sablon kiválasztása][VS2013SelectMVCTemplate]
4. <span data-ttu-id="e8c95-141">Ha még nem jelentkezett be a Microsoft Azure, kérni fogja a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="e8c95-141">If you aren't already signed into Microsoft Azure, you will be prompted to sign in.</span></span> <span data-ttu-id="e8c95-142">Kövesse az utasításokat, Azure-ba való bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="e8c95-142">Follow the prompts to sign into Azure.</span></span>
5. <span data-ttu-id="e8c95-143">Ha be van jelentkezve, indítsa el az App Service webalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e8c95-143">Once you are signed in, you can start configuring your App Service web app.</span></span> <span data-ttu-id="e8c95-144">Adja meg a **Web App name**, **App Service-csomag**, **erőforráscsoport**, és **régió**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-144">Specify the **Web App name**, **App Service plan**, **Resource group**, and **Region**, then click **Create**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSConfigureWebAppSettings.png)
6. <span data-ttu-id="e8c95-145">A projekt létrehozása után várjon, amíg a webalkalmazás az Azure App Service-ben létrehozni a **Azure App Service-tevékenység** ablak.</span><span class="sxs-lookup"><span data-stu-id="e8c95-145">After the project creation completes, wait for the web app to be created in Azure App Service as indicated in the **Azure App Service Activity** window.</span></span> <span data-ttu-id="e8c95-146">Kattintson a **most ezt a webes alkalmazás közzététele MyTaskListApp**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-146">Then, click **Publish MyTaskListApp to this Web App now**.</span></span>
7. <span data-ttu-id="e8c95-147">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="e8c95-147">Click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-store-data-mongodb-vm/VSPublishWeb.png)
   
    <span data-ttu-id="e8c95-148">Miután az alapértelmezett ASP.NET-alkalmazás közzé van téve az Azure App Service Web Apps, akkor indul el a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="e8c95-148">Once your default ASP.NET application is published to Azure App Service Web Apps, it will be launched in the browser.</span></span>

## <a name="install-the-mongodb-c-driver"></a><span data-ttu-id="e8c95-149">Telepítse a MongoDB C# illesztőprogramját</span><span class="sxs-lookup"><span data-stu-id="e8c95-149">Install the MongoDB C# driver</span></span>
<span data-ttu-id="e8c95-150">MongoDB a C#-alkalmazások pedig egy illesztőprogram, amelyen telepítenie kell a helyi fejlesztési számítógépen ügyféloldali támogatást nyújt.</span><span class="sxs-lookup"><span data-stu-id="e8c95-150">MongoDB offers client-side support for C# applications through a driver, which you need to install on your local development computer.</span></span> <span data-ttu-id="e8c95-151">A C# illesztőprogram Nugeten keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="e8c95-151">The C# driver is available through NuGet.</span></span>

<span data-ttu-id="e8c95-152">A MongoDB C# illesztőprogram telepítése:</span><span class="sxs-lookup"><span data-stu-id="e8c95-152">To install the MongoDB C# driver:</span></span>

1. <span data-ttu-id="e8c95-153">A **Megoldáskezelőben**, kattintson a jobb gombbal a **MyTaskListApp** projektre, és válassza ki **kezelése NuGetPackages**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-153">In **Solution Explorer**, right-click the **MyTaskListApp** project and select **Manage NuGetPackages**.</span></span>
   
    ![NuGet-csomagok kezelése][VS2013ManageNuGetPackages]
2. <span data-ttu-id="e8c95-155">Az a **NuGet-csomagok kezelése** ablakban a bal oldali ablaktáblán kattintson a **Online**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-155">In the **Manage NuGet Packages** window, in the left pane, click **Online**.</span></span> <span data-ttu-id="e8c95-156">Az a **keresési Online** a jobb oldali mezőbe írja be a "mongodb.driver".</span><span class="sxs-lookup"><span data-stu-id="e8c95-156">In the **Search Online** box on the right, type "mongodb.driver".</span></span>  <span data-ttu-id="e8c95-157">Kattintson a **telepítése** az illesztőprogram telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e8c95-157">Click **Install** to install the driver.</span></span>
   
    ![MongoDB C# illesztőprogram keresése][SearchforMongoDBCSharpDriver]
3. <span data-ttu-id="e8c95-159">Kattintson a **elfogadom** 10gen, Inc. feltételeinek elfogadására.</span><span class="sxs-lookup"><span data-stu-id="e8c95-159">Click **I Accept** to accept the 10gen, Inc. license terms.</span></span>
4. <span data-ttu-id="e8c95-160">Kattintson a **Bezárás** Ha az illesztőprogram telepítve van.</span><span class="sxs-lookup"><span data-stu-id="e8c95-160">Click **Close** after the driver has installed.</span></span>
    <span data-ttu-id="e8c95-161">![MongoDB-C# illesztőprogram][MongoDBCsharpDriverInstalled]</span><span class="sxs-lookup"><span data-stu-id="e8c95-161">![MongoDB C# Driver Installed][MongoDBCsharpDriverInstalled]</span></span>

<span data-ttu-id="e8c95-162">A MongoDB C# illesztőprogram most már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="e8c95-162">The MongoDB C# driver is now installed.</span></span>  <span data-ttu-id="e8c95-163">Hivatkozása a **MongoDB.Bson**, **MongoDB.Driver**, és **MongoDB.Driver.Core** szalagtárak lettek hozzáadva a projekthez.</span><span class="sxs-lookup"><span data-stu-id="e8c95-163">References to the **MongoDB.Bson**, **MongoDB.Driver**, and **MongoDB.Driver.Core**  libraries have been added to the project.</span></span>

![MongoDB C# illesztőprogram hivatkozások][MongoDBCSharpDriverReferences]

## <a name="add-a-model"></a><span data-ttu-id="e8c95-165">Modell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e8c95-165">Add a model</span></span>
<span data-ttu-id="e8c95-166">A **Megoldáskezelőben**, kattintson a jobb gombbal a *modellek* mappa és **Hozzáadás** egy új **osztály** és adjon neki nevet *TaskModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="e8c95-166">In **Solution Explorer**, right-click the *Models* folder and **Add** a new **Class** and name it *TaskModel.cs*.</span></span>  <span data-ttu-id="e8c95-167">A *TaskModel.cs*, cserélje le a meglévő kódot az alábbira:</span><span class="sxs-lookup"><span data-stu-id="e8c95-167">In *TaskModel.cs*, replace the existing code with the following code:</span></span>

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

## <a name="add-the-data-access-layer"></a><span data-ttu-id="e8c95-168">Adja hozzá az adatelérési réteg</span><span class="sxs-lookup"><span data-stu-id="e8c95-168">Add the data access layer</span></span>
<span data-ttu-id="e8c95-169">A **Megoldáskezelőben**, kattintson a jobb gombbal a *MyTaskListApp* projekt és **Hozzáadás** egy **új mappa** nevű *DAL*.</span><span class="sxs-lookup"><span data-stu-id="e8c95-169">In **Solution Explorer**, right-click the *MyTaskListApp* project and **Add** a **New Folder** named *DAL*.</span></span>  <span data-ttu-id="e8c95-170">Kattintson a jobb gombbal a *DAL* mappa és **Hozzáadás** egy új **osztály**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-170">Right-click the *DAL* folder and **Add** a new **Class**.</span></span> <span data-ttu-id="e8c95-171">Nevezze el az osztály fájlt *Dal.cs*.</span><span class="sxs-lookup"><span data-stu-id="e8c95-171">Name the class file *Dal.cs*.</span></span>  <span data-ttu-id="e8c95-172">A *Dal.cs*, cserélje le a meglévő kódot az alábbira:</span><span class="sxs-lookup"><span data-stu-id="e8c95-172">In *Dal.cs*, replace the existing code with the following code:</span></span>

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

## <a name="add-a-controller"></a><span data-ttu-id="e8c95-173">Vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e8c95-173">Add a controller</span></span>
<span data-ttu-id="e8c95-174">Nyissa meg a *Controllers\HomeController.cs* fájlt **Megoldáskezelőben** , és cserélje le a meglévő kódot a következőre:</span><span class="sxs-lookup"><span data-stu-id="e8c95-174">Open the *Controllers\HomeController.cs* file in **Solution Explorer** and replace the existing code with the following:</span></span>

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

## <a name="set-up-the-styles"></a><span data-ttu-id="e8c95-175">A stílusok beállítása</span><span class="sxs-lookup"><span data-stu-id="e8c95-175">Set up the styles</span></span>
<span data-ttu-id="e8c95-176">Az oldal tetején a cím módosításához nyissa meg a *Views\Shared\\_Layout.cshtml* fájlt **Megoldáskezelőben** és cserélje le a "Alkalmazás neve" a navigációs sávja fejlécében a "saját feladatlista Alkalmazás"úgy tűnik, hogy az informatikai, ez például:</span><span class="sxs-lookup"><span data-stu-id="e8c95-176">To change the title at the top of the page, open the *Views\Shared\\_Layout.cshtml* file in **Solution Explorer** and replace "Application name" in the navbar header with "My Task List Application" so that it looks like this:</span></span>

     @Html.ActionLink("My Task List Application", "Index", "Home", null, new { @class = "navbar-brand" })

<span data-ttu-id="e8c95-177">A feladatlista menü beállításához nyissa meg a *\Views\Home\Index.cshtml* fájlt, és cserélje le a meglévő kódot az alábbira:</span><span class="sxs-lookup"><span data-stu-id="e8c95-177">In order to set up the Task List menu, open the *\Views\Home\Index.cshtml* file and replace the existing code with the following code:</span></span>

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


<span data-ttu-id="e8c95-178">Hozzáadása egy új feladatot létrehozni, kattintson a jobb gombbal a *Views\Home\\*  mappa és **Hozzáadás** egy **nézet**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-178">To add the ability to create a new task, right-click the *Views\Home\\* folder and **Add** a **View**.</span></span>  <span data-ttu-id="e8c95-179">A nézet neve *létrehozása*.</span><span class="sxs-lookup"><span data-stu-id="e8c95-179">Name the view *Create*.</span></span> <span data-ttu-id="e8c95-180">Cserélje le a kód a következő:</span><span class="sxs-lookup"><span data-stu-id="e8c95-180">Replace the code with the following:</span></span>

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

<span data-ttu-id="e8c95-181">**Megoldáskezelő** kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="e8c95-181">**Solution Explorer** should look like this:</span></span>

![Megoldáskezelő][SolutionExplorerMyTaskListApp]

## <a name="set-the-mongodb-connection-string"></a><span data-ttu-id="e8c95-183">A MongoDB-kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="e8c95-183">Set the MongoDB connection string</span></span>
<span data-ttu-id="e8c95-184">A **Megoldáskezelőben**, nyissa meg a *DAL/Dal.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e8c95-184">In **Solution Explorer**, open the *DAL/Dal.cs* file.</span></span> <span data-ttu-id="e8c95-185">Keresse meg a következő kódsort:</span><span class="sxs-lookup"><span data-stu-id="e8c95-185">Find the following line of code:</span></span>

    private string connectionString = "mongodb://<vm-dns-name>";

<span data-ttu-id="e8c95-186">Cserélje le `<vm-dns-name>` a DNS-névvel, a MongoDB létrehozott futtató virtuális gép a [hozzon létre egy virtuális gépet, és telepítse a MongoDB] [ Create a virtual machine and install MongoDB] . lépését Ez az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="e8c95-186">Replace `<vm-dns-name>` with the DNS name of the virtual machine running MongoDB you created in the [Create a virtual machine and install MongoDB][Create a virtual machine and install MongoDB] step of this tutorial.</span></span>  <span data-ttu-id="e8c95-187">A DNS-neve, a virtuális gép található, nyissa meg az Azure portálon, válassza ki a **virtuális gépek**, és keresse meg **DNS-név**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-187">To find the DNS name of your virtual machine, go to the Azure Portal, select **Virtual Machines**, and find **DNS Name**.</span></span>

<span data-ttu-id="e8c95-188">Ha a DNS-neve, a virtuális gép "testlinuxvm.cloudapp.net" és az alapértelmezett porton 27017 MongoDB figyel, a kapcsolati karakterlánc kódsort hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="e8c95-188">If the DNS name of the virtual machine is "testlinuxvm.cloudapp.net" and MongoDB is listening on the default port 27017, the connection string line of code will look like:</span></span>

    private string connectionString = "mongodb://testlinuxvm.cloudapp.net";

<span data-ttu-id="e8c95-189">Ha a virtuális gép végpontjának mongodb határoz meg egy másik külső portot, a következőket teheti meg a kapcsolati karakterláncban a port:</span><span class="sxs-lookup"><span data-stu-id="e8c95-189">If the virtual machine endpoint specifies a different external port for MongoDB, you can specifiy the port in the connection string:</span></span>

     private string connectionString = "mongodb://testlinuxvm.cloudapp.net:12345";

<span data-ttu-id="e8c95-190">A MongoDB-kapcsolati karakterláncok további információkért lásd: [kapcsolatok][MongoConnectionStrings].</span><span class="sxs-lookup"><span data-stu-id="e8c95-190">For more information on MongoDB connection strings, see [Connections][MongoConnectionStrings].</span></span>

## <a name="test-the-local-deployment"></a><span data-ttu-id="e8c95-191">A helyi központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e8c95-191">Test the local deployment</span></span>
<span data-ttu-id="e8c95-192">Futtassa az alkalmazást a fejlesztési számítógépen, válassza ki **Start Debugging** a a **Debug** menü vagy találat **F5**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-192">To run your application on your development computer, select **Start Debugging** from the **Debug** menu or hit **F5**.</span></span> <span data-ttu-id="e8c95-193">Az IIS Express elindul, és a böngészőben megnyílik, és elindítja az alkalmazás kezdőlapját.</span><span class="sxs-lookup"><span data-stu-id="e8c95-193">IIS Express starts and a browser opens and launches the application's home page.</span></span>  <span data-ttu-id="e8c95-194">Hozzáadhat egy új feladatot, amelyek nem kerülnek be az Azure-ban, a virtuális gépen futó MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="e8c95-194">You can add a new task, which will be added to the MongoDB database running on your virtual machine in Azure.</span></span>

![A feladat alkalmazásában][TaskListAppBlank]

## <a name="publish-to-azure-app-service-web-apps"></a><span data-ttu-id="e8c95-196">Az Azure App Service Web Apps alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="e8c95-196">Publish to Azure App Service Web Apps</span></span>
<span data-ttu-id="e8c95-197">Ebben a szakaszban az Azure App Service Web Apps tesznek közzé a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e8c95-197">In this section you will publish your changes to Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="e8c95-198">A Megoldáskezelőben kattintson a jobb gombbal **MyTaskListApp** újra kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-198">In Solution Explorer, right-click **MyTaskListApp** again and click **Publish**.</span></span>
2. <span data-ttu-id="e8c95-199">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="e8c95-199">Click **Publish**.</span></span>
   
    <span data-ttu-id="e8c95-200">Most látnia kell a webalkalmazás fut az Azure App Service-ben, és a MongoDB adatbázis Azure virtuális gépek elérése.</span><span class="sxs-lookup"><span data-stu-id="e8c95-200">You should now see your web app running in Azure App Service and accessing the MongoDB database in Azure Virtual Machines.</span></span>

## <a name="summary"></a><span data-ttu-id="e8c95-201">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e8c95-201">Summary</span></span>
<span data-ttu-id="e8c95-202">Most már sikeresen telepítette az Azure App Service Web Apps ASP.NET alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e8c95-202">You have now successfully deployed your ASP.NET application to Azure App Service Web Apps.</span></span> <span data-ttu-id="e8c95-203">A webes alkalmazás megtekintése:</span><span class="sxs-lookup"><span data-stu-id="e8c95-203">To view the web app:</span></span>

1. <span data-ttu-id="e8c95-204">Jelentkezzen be az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e8c95-204">Log into the Azure Portal.</span></span>
2. <span data-ttu-id="e8c95-205">Kattintson a **webalkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e8c95-205">Click **Web apps**.</span></span> 
3. <span data-ttu-id="e8c95-206">Válassza ki a webalkalmazás a **webalkalmazások** listája.</span><span class="sxs-lookup"><span data-stu-id="e8c95-206">Select your web app in the **Web Apps** list.</span></span>

<span data-ttu-id="e8c95-207">A fejleszt alkalmazásokat C# MongoDB további információkért lásd: [CSharp nyelvű Center][MongoC#LangCenter].</span><span class="sxs-lookup"><span data-stu-id="e8c95-207">For more information on developing C# applications against MongoDB, see [CSharp Language Center][MongoC#LangCenter].</span></span> 

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
