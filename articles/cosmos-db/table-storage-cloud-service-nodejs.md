---
title: "A table storage (Node.js) webalkalmazásnál |} Microsoft Docs"
description: "Ez az oktatóanyag Azure Storage szolgáltatás és az Azure-modul hozzáadásával a webalkalmazás az Express oktatóanyag épül."
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: e90959a2-4cb2-4b19-9bfb-aede15b18b1c
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: b802f880c1131abb7eb9ba00dd8f2e65017bc802
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="25188-103">Storage használata node.js-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="25188-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="25188-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="25188-104">Overview</span></span>
<span data-ttu-id="25188-105">Ebben az oktatóanyagban az alkalmazás létrehozott a [Express használata Node.js-webalkalmazás] oktatóanyag ki van bővítve használata a Microsoft Azure Klienskódtárak segítségével a Node.js adatok szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="25188-105">In this tutorial, the application you created in the [Node.js Web Application using Express] tutorial is extended using the Microsoft Azure Client Libraries for Node.js to work with data management services.</span></span> <span data-ttu-id="25188-106">Hozzon létre egy webalapú feladatlista alkalmazás, amely központilag telepíthető a Azure terjesztheti ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="25188-106">You extend your application by creating a web-based task-list application that you can deploy to Azure.</span></span> <span data-ttu-id="25188-107">A feladatok lista lehetővé teszi a felhasználóknak feladatokat beolvasni, adja hozzá az új feladatok és feladatok megjelölése befejezettként.</span><span class="sxs-lookup"><span data-stu-id="25188-107">The task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="25188-108">A feladat tárolódnak az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="25188-108">The task items are stored in Azure Storage.</span></span> <span data-ttu-id="25188-109">Az Azure Storage biztosítja a hibatűrő és magas rendelkezésre állású strukturálatlan adatok tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="25188-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="25188-110">Az Azure Storage számos adatszerkezetek, ahol tárolhatja és érheti adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="25188-110">Azure Storage includes several data structures where you can store and access data.</span></span> <span data-ttu-id="25188-111">A tárolási szolgáltatások az API-szerepel az Azure SDK for Node.js vagy a REST API-kon keresztül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="25188-111">You can use the storage services from the APIs included in the Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="25188-112">További információkért lásd: [tárolása és az adatok elérése az Azure-ban].</span><span class="sxs-lookup"><span data-stu-id="25188-112">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="25188-113">Ez az oktatóanyag feltételezi, hogy végrehajtotta a [Node.js-webalkalmazás] és [expressz Node.js][Express használata Node.js-webalkalmazás] oktatóanyagok.</span><span class="sxs-lookup"><span data-stu-id="25188-113">This tutorial assumes that you have completed the [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="25188-114">A következő adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="25188-114">It contains the following information:</span></span>

* <span data-ttu-id="25188-115">A Jade sablon motor használata</span><span class="sxs-lookup"><span data-stu-id="25188-115">How to work with the Jade template engine</span></span>
* <span data-ttu-id="25188-116">Azure Data szolgáltatások használata</span><span class="sxs-lookup"><span data-stu-id="25188-116">How to work with Azure Data Management services</span></span>

<span data-ttu-id="25188-117">Az alábbi képernyőfelvételen a kész alkalmazás látható:</span><span class="sxs-lookup"><span data-stu-id="25188-117">The following screenshot shows the completed application:</span></span>

![A befejezett weblap az internet Explorerben](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="25188-119">Tárolási hitelesítő adatok beállítása a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="25188-119">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="25188-120">Át kell adnia a tároló hitelesítő adatok Azure Storage eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="25188-120">You must pass in storage credentials to access Azure Storage.</span></span> <span data-ttu-id="25188-121">Ez a web.config Alkalmazásbeállítások felügyelniük történik.</span><span class="sxs-lookup"><span data-stu-id="25188-121">This is done by utilizing the web.config application settings.</span></span>
<span data-ttu-id="25188-122">A web.config beállítások átadása pedig a környezeti változók csomópontra, amely majd beolvassa vannak az Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="25188-122">The web.config settings are passed as environment variables to Node, which are then read by the Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="25188-123">Tárolási csak szolgálnak, amikor az alkalmazás központi telepítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="25188-123">Storage credentials are only used when the application is deployed to Azure.</span></span> <span data-ttu-id="25188-124">Ha az emulátorban futtatja, az alkalmazás használja a storage emulator.</span><span class="sxs-lookup"><span data-stu-id="25188-124">When running in the emulator, the application uses the storage emulator.</span></span>
>
>

<span data-ttu-id="25188-125">Hajtsa végre a tárfiók hitelesítő adatainak lekérésére, és adja hozzá a web.config beállítások az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="25188-125">Perform the following steps to retrieve the storage account credentials and add them to the web.config settings:</span></span>

1. <span data-ttu-id="25188-126">Ha még nincs nyitva, indítsa el az Azure PowerShell, a a **Start** kibontásával menü **minden program és az Azure-**, kattintson a jobb gombbal **Azure PowerShell**, majd válassza ki a  **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="25188-126">If it is not already open, start the Azure PowerShell from the **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="25188-127">Váltson át a mappát, amely tartalmazza az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="25188-127">Change directories to the folder containing your application.</span></span> <span data-ttu-id="25188-128">Például a C:\\csomópont\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="25188-128">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="25188-129">Az Azure Powershell ablakban adja meg a következő parancsmagot a tárfiókadatok beolvasása:</span><span class="sxs-lookup"><span data-stu-id="25188-129">From the Azure Powershell window, enter the following cmdlet to retrieve the storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="25188-130">A fenti parancsmag lekéri a listában tárfiókok és a kulcsok az üzemeltetett szolgáltatás társított fiókot.</span><span class="sxs-lookup"><span data-stu-id="25188-130">The preceding cmdlet retrieves the list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="25188-131">Mivel az Azure SDK-t hoz tárfiókot, a szolgáltatás telepítésekor, a tárfiók már léteznie kell a az előző útmutatók az alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="25188-131">Since the Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in the previous guides.</span></span>
   >
   >
4. <span data-ttu-id="25188-132">Nyissa meg a **ServiceDefinition.csdef** telepítik az alkalmazást az Azure-bA használt környezet beállításokat tartalmazó fájlt:</span><span class="sxs-lookup"><span data-stu-id="25188-132">Open the **ServiceDefinition.csdef** file containing the environment settings that are used when the application is deployed to Azure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="25188-133">Helyezze be a következő kódblokk a **környezet** elemet, és a {TÁRFIÓK} és {TÁRELÉRÉSI kulcs} a fióknevet és a központi telepítéshez használni kívánt tárfiók elsődleges kulcs:</span><span class="sxs-lookup"><span data-stu-id="25188-133">Insert the following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with the account name and the primary key for the storage account you want to use for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![A web.cloud.config tartalmát](./media/table-storage-cloud-service-nodejs/node37.png)

6. <span data-ttu-id="25188-135">Mentse a fájlt, és zárja be a Jegyzettömböt.</span><span class="sxs-lookup"><span data-stu-id="25188-135">Save the file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="25188-136">A kiegészítő modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="25188-136">Install additional modules</span></span>
1. <span data-ttu-id="25188-137">A következő paranccsal telepítse az [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok helyileg, valamint hogy menteni egy bejegyzést, hogy a **package.json** fájlt:</span><span class="sxs-lookup"><span data-stu-id="25188-137">Use the following command to install the [azure], [node-uuid], [nconf] and [async] modules locally as well as to save an entry for them to the **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="25188-138">Ez a parancs a következőhöz hasonlóan kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="25188-138">The output of this command should appear similar to the following:</span></span>

  ```
  node-uuid@1.4.1 node_modules\node-uuid

  nconf@0.6.9 node_modules\nconf
  ├── ini@1.1.0
  ├── async@0.2.9
  └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

  azure-storage@0.1.0 node_modules\azure-storage
  ├── extend@1.2.1
  ├── xmlbuilder@0.4.3
  ├── mime@1.2.11
  ├── underscore@1.4.4
  ├── validator@3.1.0
  ├── node-uuid@1.4.1
  ├── xml2js@0.2.7 (sax@0.5.2)
  └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)
  ```

## <a name="using-the-table-service-in-a-node-application"></a><span data-ttu-id="25188-139">A Table szolgáltatás használata node.js-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="25188-139">Using the Table service in a node application</span></span>
<span data-ttu-id="25188-140">Ebben a szakaszban az alapvető alkalmazás hozta létre a **expressz** parancs hozzáadásával ki van bővítve a **task.js** a tevékenységek a modellt tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="25188-140">In this section, the basic application created by the **express** command is extended by adding a **task.js** file containing the model for your tasks.</span></span> <span data-ttu-id="25188-141">Módosítsa a meglévő **app.js** fájlt, és hozzon létre egy új **tasklist.js** fájlt, amely a modellt használ.</span><span class="sxs-lookup"><span data-stu-id="25188-141">Modify the existing **app.js** file and create a new **tasklist.js** file that uses the model.</span></span>

### <a name="create-the-model"></a><span data-ttu-id="25188-142">A modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="25188-142">Create the model</span></span>
1. <span data-ttu-id="25188-143">Az a **WebRole1** könyvtár, hozzon létre egy új könyvtárat nevű **modellek**.</span><span class="sxs-lookup"><span data-stu-id="25188-143">In the **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="25188-144">Az a **modellek** könyvtár, hozzon létre egy új fájlt **task.js**.</span><span class="sxs-lookup"><span data-stu-id="25188-144">In the **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="25188-145">Ez a fájl tartalmazza a modell az alkalmazás által létrehozott feladatok számára.</span><span class="sxs-lookup"><span data-stu-id="25188-145">This file contains the model for the tasks created by your application.</span></span>
3. <span data-ttu-id="25188-146">Elején a **task.js** fájlt, adja hozzá a következő kódot a szükséges kódtárak hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="25188-146">At the beginning of the **task.js** file, add the following code to reference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="25188-147">Ezután adja hozzá a kód határozza meg, és a feladat objektum exportálása.</span><span class="sxs-lookup"><span data-stu-id="25188-147">Next, add code to define and export the Task object.</span></span> <span data-ttu-id="25188-148">A feladatobjektum felelős a tábla kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="25188-148">The Task object is responsible for connecting to the table.</span></span>

    ```nodejs
    module.exports = Task;

    function Task(storageClient, tableName, partitionKey) {
      this.storageClient = storageClient;
      this.tableName = tableName;
      this.partitionKey = partitionKey;
      this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
        if(error) {
          throw error;
        }
      });
    };
    ```

5. <span data-ttu-id="25188-149">Ezután adja hozzá a következő kódot a feladatobjektumokhoz további metódusok meghatározásához a feladatobjektumot, amelyek lehetővé teszik a táblában tárolt adatok interakció:</span><span class="sxs-lookup"><span data-stu-id="25188-149">Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in the table:</span></span>

    ```nodejs
    Task.prototype = {
      find: function(query, callback) {
        self = this;
        self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
          if(error) {
            callback(error);
          } else {
            callback(null, result.entries);
          }
        });
      },

      addItem: function(item, callback) {
        self = this;
        // use entityGenerator to set types
        // NOTE: RowKey must be a string type, even though
        // it contains a GUID in this example.
        var itemDescriptor = {
          PartitionKey: entityGen.String(self.partitionKey),
          RowKey: entityGen.String(uuid()),
          name: entityGen.String(item.name),
          category: entityGen.String(item.category),
          completed: entityGen.Boolean(false)
        };

        self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
          if(error){
            callback(error);
          }
          callback(null);
        });
      },

      updateItem: function(rKey, callback) {
        self = this;
        self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
          if(error) {
            callback(error);
          }
          entity.completed._ = true;
          self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
            if(error) {
              callback(error);
            }
            callback(null);
          });
        });
      }
    }
    ```

6. <span data-ttu-id="25188-150">Mentse és zárja be a **task.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="25188-150">Save and close the **task.js** file.</span></span>

### <a name="create-the-controller"></a><span data-ttu-id="25188-151">A vezérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="25188-151">Create the controller</span></span>
1. <span data-ttu-id="25188-152">Az a **WebRole1/útvonalak** könyvtár, hozzon létre egy új fájlt **tasklist.js** , majd nyissa meg szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="25188-152">In the **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="25188-153">Adja hozzá a következő kódot a **tasklist.js** fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="25188-153">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="25188-154">Ez a kód betölti az azure és async modult által használt **tasklist.js** , és meghatározzák a **TaskList** függvény, amelyet példánya a **feladat** objektum azt korábban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="25188-154">This code loads the azure and async modules, which are used by **tasklist.js** and defines the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="25188-155">Folytassa a **tasklist.js** fájl által használt metódusok hozzáadásával **showTasks**, **Addtasks**, és **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="25188-155">Continue adding to the **tasklist.js** file by adding the methods used to **showTasks**, **addTask**, and **completeTasks**:</span></span>

    ```nodejs
    TaskList.prototype = {
      showTasks: function(req, res) {
        self = this;
        var query = azure.TableQuery()
          .where('completed eq ?', false);
        self.task.find(query, function itemsFound(error, items) {
          res.render('index',{title: 'My ToDo List ', tasks: items});
        });
      },

      addTask: function(req,res) {
        var self = this
        var item = req.body.item;
        self.task.addItem(item, function itemAdded(error) {
          if(error) {
            throw error;
          }
          res.redirect('/');
        });
      },

      completeTask: function(req,res) {
        var self = this;
        var completedTasks = Object.keys(req.body);
        async.forEach(completedTasks, function taskIterator(completedTask, callback) {
          self.task.updateItem(completedTask, function itemsUpdated(error) {
            if(error){
              callback(error);
            } else {
              callback(null);
            }
          });
        }, function goHome(error){
          if(error) {
            throw error;
          } else {
            res.redirect('/');
          }
        });
      }
    }
    ```

4. <span data-ttu-id="25188-156">Mentse a **tasklist.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="25188-156">Save the **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="25188-157">Az app.js fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="25188-157">Modify app.js</span></span>
1. <span data-ttu-id="25188-158">Az a **WebRole1** könyvtár, nyissa meg a **app.js** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="25188-158">In the **WebRole1** directory, open the **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="25188-159">A fájl elején adja hozzá a következő, azure-moduljának betöltése, és állítsa be a táblázat nevét, és a partíció kulcsot:</span><span class="sxs-lookup"><span data-stu-id="25188-159">At the beginning of the file, add the following to load the azure module and set the table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="25188-160">Az app.js fájlban görgessen le, ahol megjelenik a következő sort:</span><span class="sxs-lookup"><span data-stu-id="25188-160">In the app.js file, scroll down to where you see the following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="25188-161">Cserélje le a következő kódot az előző sorokat.</span><span class="sxs-lookup"><span data-stu-id="25188-161">Replace the preceding lines with the following code.</span></span> <span data-ttu-id="25188-162">Ez a kód egy példányát inicializálja <strong>feladat</strong> a tárfiók kapcsolattal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="25188-162">This code initializes an instance of <strong>Task</strong> with a connection to your storage account.</span></span> <span data-ttu-id="25188-163">A <strong>feladat</strong> számára a <strong>TaskList</strong>, amely használja azt a Table szolgáltatással való kommunikációra:</span><span class="sxs-lookup"><span data-stu-id="25188-163">The <strong>Task</strong> is passed to the <strong>TaskList</strong>, which uses it to communicate with the Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="25188-164">Mentse a **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="25188-164">Save the **app.js** file.</span></span>

### <a name="modify-the-index-view"></a><span data-ttu-id="25188-165">Az index nézetről módosítása</span><span class="sxs-lookup"><span data-stu-id="25188-165">Modify the index view</span></span>
1. <span data-ttu-id="25188-166">Lépjen a **nézetek** könyvtárhoz, és nyissa meg a **index.jade** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="25188-166">Change directories to the **views** directory and open the **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="25188-167">Cserélje le a tartalmát a **index.jade** fájl a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="25188-167">Replace the contents of the **index.jade** file with the following code.</span></span> <span data-ttu-id="25188-168">Ez a kód határozza meg a nézet a meglévő feladatokat megjelenő, és határozza meg az új feladatok hozzáadása és meglévőket megjelölés befejezettként űrlap.</span><span class="sxs-lookup"><span data-stu-id="25188-168">This code defines the view for displaying existing tasks, and defines a form for adding new tasks and marking existing ones as completed.</span></span>

    ```
    extends layout

    block content
      h1= title
      br

      form(action="/completetask", method="post")
        table.table.table-striped.table-bordered
          tr
            td Name
            td Category
            td Date
            td Complete
          if tasks != []
            tr
              td
          else
            each task in tasks
              tr
                td #{task.name._}
                td #{task.category._}
                - var day   = task.Timestamp._.getDate();
                - var month = task.Timestamp._.getMonth() + 1;
                - var year  = task.Timestamp._.getFullYear();
                td #{month + "/" + day + "/" + year}
                td
                  input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
        button.btn(type="submit") Update tasks
      hr
      form.well(action="/addtask", method="post")
        label Item Name:
        input(name="item[name]", type="textbox")
        label Item Category:
        input(name="item[category]", type="textbox")
        br
        button.btn(type="submit") Add item
    ```

3. <span data-ttu-id="25188-169">Mentse és zárja be **index.jade** fájlt.</span><span class="sxs-lookup"><span data-stu-id="25188-169">Save and close **index.jade** file.</span></span>

### <a name="modify-the-global-layout"></a><span data-ttu-id="25188-170">A globális elrendezés módosítása</span><span class="sxs-lookup"><span data-stu-id="25188-170">Modify the global layout</span></span>
<span data-ttu-id="25188-171">A rendszer a **views** (nézetek) könyvtárban található **layout.jade** fájlt használja a többi **.jade** fájl globális sablonjaként.</span><span class="sxs-lookup"><span data-stu-id="25188-171">The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="25188-172">Ebben a lépésben módosítsa a **Views** használandó [Twitter Bootstrap](https://github.com/twbs/bootstrap), ez az egy eszközkészlet, amely megkönnyíti a töltött tetszetős webhelyeket tervezéséhez.</span><span class="sxs-lookup"><span data-stu-id="25188-172">In this step, modify the **layout.jade** file to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking website.</span></span>

1. <span data-ttu-id="25188-173">Töltse le és csomagolja ki a fájlokat a [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="25188-173">Download and extract the files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="25188-174">Másolás a **bootstrap.min.css** fájlt a **bootstrap\\eloszlás\\css** mappát a **nyilvános\\stíluslapok** az tasklist alkalmazás könyvtár.</span><span class="sxs-lookup"><span data-stu-id="25188-174">Copy the **bootstrap.min.css** file from the **bootstrap\\dist\\css** folder to the **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="25188-175">Az a **nézetek** mappa, nyissa meg a **Views** fájlt a szövegszerkesztőben, és cserélje ki a tartalmát a következőre:</span><span class="sxs-lookup"><span data-stu-id="25188-175">From the **views** folder, open the **layout.jade** file in your text editor and replace the contents with the following:</span></span>

    <span data-ttu-id="25188-176">DOCTYPE html html központi cím cím hivatkozás = (rel = "xsl", href='/stylesheets/bootstrap.min.css) hivatkozás (rel = "xsl", href='/stylesheets/style.css) body.app nav.navbar.navbar alapértelmezett div.navbar-fejléc a.navbar-brand(href='/') a  Feladatok blokkolja a tartalmat</span><span class="sxs-lookup"><span data-stu-id="25188-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="25188-177">Mentse a **Views** fájlt.</span><span class="sxs-lookup"><span data-stu-id="25188-177">Save the **layout.jade** file.</span></span>

### <a name="running-the-application-in-the-emulator"></a><span data-ttu-id="25188-178">Az alkalmazás futtatása az emulátorban</span><span class="sxs-lookup"><span data-stu-id="25188-178">Running the Application in the Emulator</span></span>
<span data-ttu-id="25188-179">A következő paranccsal indítsa el az alkalmazást az emulátorban.</span><span class="sxs-lookup"><span data-stu-id="25188-179">Use the following command to start the application in the emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="25188-180">A böngészőben megnyílik, és megjeleníti a következő lapot:</span><span class="sxs-lookup"><span data-stu-id="25188-180">The browser opens and displays the following page:</span></span>

![A webes lapozható saját feladatlista című feladatok és adjon hozzá egy új feladatot mezőket tartalmazó tábla.](./media/table-storage-cloud-service-nodejs/node44.png)

<span data-ttu-id="25188-182">Ezen a képernyőn elemek hozzáadását, vagy távolítsa el a meglévő elemeket befejezettként jelöli meg őket.</span><span class="sxs-lookup"><span data-stu-id="25188-182">Use the form to add items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-the-application-to-azure"></a><span data-ttu-id="25188-183">Az Azure-bA az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="25188-183">Publishing the Application to Azure</span></span>
<span data-ttu-id="25188-184">A Windows PowerShell-ablakban hívja meg a következő parancsmagot újratelepíteni az üzemeltetett szolgáltatás az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="25188-184">In the Windows PowerShell window, call the following cmdlet to redeploy your hosted service to Azure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="25188-185">Cserélje le **myuniquename** ehhez az alkalmazáshoz, egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="25188-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="25188-186">Cserélje le **datacentername** nevű, egy Azure-adatközponthoz, például a **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="25188-186">Replace **datacentername** with the name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="25188-187">A telepítés befejezése után, a következőhöz hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="25188-187">After the deployment is complete, you should see a response similar to the following:</span></span>

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

<span data-ttu-id="25188-188">Megadja a **-indítása** az előző parancsmag, a böngésző beállítás megnyílik, és megjeleníti az alkalmazás Azure-beli közzététel befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="25188-188">By specifying the **-launch** option in the previous cmdlet, the browser opens and displays your application running in Azure when publishing is completed.</span></span>

![A saját feladatlista lapot megjelenítő böngészőablak.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="25188-191">Leállítása és az alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="25188-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="25188-192">Után az alkalmazás telepítéséhez, érdemes lehet le kell tiltani, költségek elkerülése vagy létrehozhatja és más alkalmazások telepítése a ingyenes próba időn belül.</span><span class="sxs-lookup"><span data-stu-id="25188-192">After deploying your application, you may want to disable it so you can avoid costs or build and deploy other applications within the free trial time period.</span></span>

<span data-ttu-id="25188-193">Az Azure a webesszerepkör-példányok esetében óránként számol fel díjat a felhasznált kiszolgálóidő után.</span><span class="sxs-lookup"><span data-stu-id="25188-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="25188-194">A kiszolgálóidő felhasználása az alkalmazás üzembe helyezésétől kezdődik, még akkor is, ha a példányok nem futnak, és leállított állapotban vannak.</span><span class="sxs-lookup"><span data-stu-id="25188-194">Server time is consumed once your application is deployed, even if the instances are not running and are in the stopped state.</span></span>

<span data-ttu-id="25188-195">A következő lépések bemutatják a állítsa le és törölje az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="25188-195">The following steps show you how to stop and delete your application.</span></span>

1. <span data-ttu-id="25188-196">Állítsa le az előző szakaszban létrehozott szolgáltatástelepítést a Windows PowerShell-ablakban az alábbi parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="25188-196">In the Windows PowerShell window, stop the service deployment created in the previous section with the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="25188-197">A szolgáltatás leállítása eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="25188-197">Stopping the service may take several minutes.</span></span> <span data-ttu-id="25188-198">Miután a szolgáltatás leállt, kap egy üzenetet, amely tájékoztatja a leállásról.</span><span class="sxs-lookup"><span data-stu-id="25188-198">When the service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="25188-199">A szolgáltatás törléséhez hívja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="25188-199">To delete the service, call the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="25188-200">Ha a rendszer rákérdez, írja be az **Y** karaktert a szolgáltatás törléséhez.</span><span class="sxs-lookup"><span data-stu-id="25188-200">When prompted, enter **Y** to delete the service.</span></span>

   <span data-ttu-id="25188-201">A szolgáltatás törlése eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="25188-201">Deleting the service may take several minutes.</span></span> <span data-ttu-id="25188-202">A szolgáltatás törlését követően kapni fog egy üzenet, amely azt jelzi, hogy törölve lett-e a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="25188-202">After the service is deleted, you will receive a message indicating that the service was deleted.</span></span>

[Express használata Node.js-webalkalmazás]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[tárolása és az adatok elérése az Azure-ban]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-webalkalmazás]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


