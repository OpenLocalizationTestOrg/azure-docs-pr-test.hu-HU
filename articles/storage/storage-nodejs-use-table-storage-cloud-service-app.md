---
title: "A table storage (Node.js) webalkalmazásnál |} Microsoft Docs"
description: "Ez az oktatóanyag Azure Storage szolgáltatás és az Azure-modul hozzáadásával a webalkalmazás az Express oktatóanyag épül."
services: cloud-services, storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: e90959a2-4cb2-4b19-9bfb-aede15b18b1c
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 5d7ee2f529b5127ee60ec8b4f5acaa49e75ddf39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="6fa54-103">Storage használata node.js-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="6fa54-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="6fa54-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6fa54-104">Overview</span></span>
<span data-ttu-id="6fa54-105">Ebben az oktatóanyagban az létrehozott alkalmazás ki kell terjesztenie a [Express használata Node.js-webalkalmazás] oktatóanyag az adatok szolgáltatások működéséhez a Microsoft Azure Klienskódtárak segítségével a Node.js használatával.</span><span class="sxs-lookup"><span data-stu-id="6fa54-105">In this tutorial, you will extend the application created in the [Node.js Web Application using Express] tutorial by using the Microsoft Azure Client Libraries for Node.js to work with data management services.</span></span> <span data-ttu-id="6fa54-106">Az alkalmazás, amely központilag telepíthető az Azure web-alapú feladatlista alkalmazás létrehozásának ki kell terjesztenie.</span><span class="sxs-lookup"><span data-stu-id="6fa54-106">You will extend your application to create a web-based task-list application that you can deploy to Azure.</span></span> <span data-ttu-id="6fa54-107">A feladatok lista lehetővé teszi a felhasználóknak feladatokat beolvasni, adja hozzá az új feladatok és feladatok megjelölése befejezettként.</span><span class="sxs-lookup"><span data-stu-id="6fa54-107">The task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="6fa54-108">A feladat tárolódnak az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6fa54-108">The task items are stored in Azure Storage.</span></span> <span data-ttu-id="6fa54-109">Az Azure Storage biztosítja a hibatűrő és magas rendelkezésre állású strukturálatlan adatok tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="6fa54-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="6fa54-110">Az Azure Storage több adatstruktúrákat, ahol tárolhatja és érheti el az adatait, és kihasználhatják a tárolási szolgáltatások az API-szerepel az Azure SDK for Node.js vagy a REST API-kon keresztül tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6fa54-110">Azure Storage includes several data structures where you can store and access data, and you can leverage the storage services from the APIs included in the Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="6fa54-111">További információkért lásd: [tárolása és az adatok elérése az Azure-ban].</span><span class="sxs-lookup"><span data-stu-id="6fa54-111">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="6fa54-112">Ez az oktatóanyag feltételezi, hogy végrehajtotta a [Node.js-webalkalmazás] és [expressz Node.js][Express használata Node.js-webalkalmazás] oktatóanyagok.</span><span class="sxs-lookup"><span data-stu-id="6fa54-112">This tutorial assumes that you have completed the [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="6fa54-113">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="6fa54-113">You will learn:</span></span>

* <span data-ttu-id="6fa54-114">A Jade sablon motor használata</span><span class="sxs-lookup"><span data-stu-id="6fa54-114">How to work with the Jade template engine</span></span>
* <span data-ttu-id="6fa54-115">Azure Data szolgáltatások használata</span><span class="sxs-lookup"><span data-stu-id="6fa54-115">How to work with Azure Data Management services</span></span>

<span data-ttu-id="6fa54-116">A kész alkalmazás képernyőfelvételének alatt van:</span><span class="sxs-lookup"><span data-stu-id="6fa54-116">A screenshot of the completed application is below:</span></span>

![A befejezett weblap az internet Explorerben](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="6fa54-118">Tárolási hitelesítő adatok beállítása a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="6fa54-118">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="6fa54-119">Azure Storage eléréséhez szüksége felelt meg a tároló hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="6fa54-119">To access Azure Storage, you need to pass in storage credentials.</span></span> <span data-ttu-id="6fa54-120">Ehhez használhatja a web.config alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="6fa54-120">To do this, you utilize web.config application settings.</span></span>
<span data-ttu-id="6fa54-121">Ezek a beállítások átkerülnek környezeti változóként csomópont, amely majd beolvassa vannak az Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="6fa54-121">Those settings will be passed as environment variables to Node, which are then read by the Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="6fa54-122">Tárolási csak szolgálnak, amikor az alkalmazás központi telepítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="6fa54-122">Storage credentials are only used when the application is deployed to Azure.</span></span> <span data-ttu-id="6fa54-123">Ha az emulátorban futtatja, az alkalmazás a storage emulator fogja használni.</span><span class="sxs-lookup"><span data-stu-id="6fa54-123">When running in the emulator, the application will use the storage emulator.</span></span>
>
>

<span data-ttu-id="6fa54-124">Hajtsa végre a tárfiók hitelesítő adatainak lekérésére, és adja hozzá a web.config beállítások az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6fa54-124">Perform the following steps to retrieve the storage account credentials and add them to the web.config settings:</span></span>

1. <span data-ttu-id="6fa54-125">Ha még nincs nyitva, indítsa el az Azure PowerShell, a a **Start** kibontásával menü **minden program és az Azure-**, kattintson a jobb gombbal **Azure PowerShell**, majd válassza ki a  **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="6fa54-125">If it is not already open, start the Azure PowerShell from the **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="6fa54-126">Váltson át a mappát, amely tartalmazza az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6fa54-126">Change directories to the folder containing your application.</span></span> <span data-ttu-id="6fa54-127">Például a C:\\csomópont\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="6fa54-127">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="6fa54-128">Az Azure Powershell ablakban adja meg a következő parancsmagot a tárfiókadatok beolvasása:</span><span class="sxs-lookup"><span data-stu-id="6fa54-128">From the Azure Powershell window enter the following cmdlet to retrieve the storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="6fa54-129">Ez lekéri a listában tárfiókok és a kulcsok az üzemeltetett szolgáltatás társított fiókot.</span><span class="sxs-lookup"><span data-stu-id="6fa54-129">This retrieves the list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6fa54-130">Mivel az Azure SDK-t hoz tárfiókot, a szolgáltatás telepítésekor, a tárfiók már léteznie kell a az előző útmutatók az alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6fa54-130">Since the Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in the previous guides.</span></span>
   >
   >
4. <span data-ttu-id="6fa54-131">Nyissa meg a **ServiceDefinition.csdef** telepítik az alkalmazást az Azure-bA használt környezet beállításokat tartalmazó fájlt:</span><span class="sxs-lookup"><span data-stu-id="6fa54-131">Open the **ServiceDefinition.csdef** file containing the environment settings that are used when the application is deployed to Azure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="6fa54-132">Helyezze be a következő kódblokk a **környezet** elemet, és a {TÁRFIÓK} és {TÁRELÉRÉSI kulcs} a fióknevet és a központi telepítéshez használni kívánt tárfiók elsődleges kulcs:</span><span class="sxs-lookup"><span data-stu-id="6fa54-132">Insert the following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with the account name and the primary key for the storage account you want to use for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![A web.cloud.config tartalmát](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6. <span data-ttu-id="6fa54-134">Mentse a fájlt, és zárja be a Jegyzettömböt.</span><span class="sxs-lookup"><span data-stu-id="6fa54-134">Save the file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="6fa54-135">A kiegészítő modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="6fa54-135">Install additional modules</span></span>
1. <span data-ttu-id="6fa54-136">A következő paranccsal telepítse az [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok helyileg, valamint hogy menteni egy bejegyzést, hogy a **package.json** fájlt:</span><span class="sxs-lookup"><span data-stu-id="6fa54-136">Use the following command to install the [azure], [node-uuid], [nconf] and [async] modules locally as well as to save an entry for them to the **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="6fa54-137">Ez a parancs a következőhöz hasonlóan kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="6fa54-137">The output of this command should appear similar to the following:</span></span>

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

## <a name="using-the-table-service-in-a-node-application"></a><span data-ttu-id="6fa54-138">A Table szolgáltatás használata node.js-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="6fa54-138">Using the Table service in a node application</span></span>
<span data-ttu-id="6fa54-139">Ebben a szakaszban az az alapszintű alkalmazás által létrehozott ki kell terjesztenie a **expressz** parancs hozzáadásával a **task.js** fájl, amely tartalmazza a modell a feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="6fa54-139">In this section you will extend the basic application created by the **express** command by adding a **task.js** file which contains the model for your tasks.</span></span> <span data-ttu-id="6fa54-140">Akkor is módosítani fogja a meglévő **app.js** és hozzon létre egy új **tasklist.js** fájlt, amely a modellt használ.</span><span class="sxs-lookup"><span data-stu-id="6fa54-140">You will also modify the existing **app.js** and create a new **tasklist.js** file that uses the model.</span></span>

### <a name="create-the-model"></a><span data-ttu-id="6fa54-141">A modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fa54-141">Create the model</span></span>
1. <span data-ttu-id="6fa54-142">Az a **WebRole1** könyvtár, hozzon létre egy új könyvtárat nevű **modellek**.</span><span class="sxs-lookup"><span data-stu-id="6fa54-142">In the **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="6fa54-143">Az a **modellek** könyvtár, hozzon létre egy új fájlt **task.js**.</span><span class="sxs-lookup"><span data-stu-id="6fa54-143">In the **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="6fa54-144">Ez a fájl tartalmazza majd a modellt az alkalmazás által létrehozott feladatok számára.</span><span class="sxs-lookup"><span data-stu-id="6fa54-144">This file will contain the model for the tasks created by your application.</span></span>
3. <span data-ttu-id="6fa54-145">Elején a **task.js** fájlt, adja hozzá a következő kódot a szükséges kódtárak hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="6fa54-145">At the beginning of the **task.js** file, add the following code to reference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="6fa54-146">Ezután adja hozzá a feladatobjektum meghatározására és exportálására használt kódot.</span><span class="sxs-lookup"><span data-stu-id="6fa54-146">Next, you will add code to define and export the Task object.</span></span> <span data-ttu-id="6fa54-147">Ez az objektum felelős a tábla kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="6fa54-147">This object is responsible for connecting to the table.</span></span>

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

5. <span data-ttu-id="6fa54-148">Ezután adja hozzá a következő kódot a feladatobjektumokhoz további metódusok meghatározásához a feladatobjektumot, amelyek lehetővé teszik a táblában tárolt adatok interakció:</span><span class="sxs-lookup"><span data-stu-id="6fa54-148">Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in the table:</span></span>

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

6. <span data-ttu-id="6fa54-149">Mentse és zárja be a **task.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6fa54-149">Save and close the **task.js** file.</span></span>

### <a name="create-the-controller"></a><span data-ttu-id="6fa54-150">A vezérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fa54-150">Create the controller</span></span>
1. <span data-ttu-id="6fa54-151">Az a **WebRole1/útvonalak** könyvtár, hozzon létre egy új fájlt **tasklist.js** , majd nyissa meg szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="6fa54-151">In the **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="6fa54-152">Adja hozzá a következő kódot a **tasklist.js** fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="6fa54-152">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="6fa54-153">Ez betölti az azure és async modult által használt **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="6fa54-153">This loads the azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="6fa54-154">Ez határozza meg a **TaskList** függvénynek, amely egy példánya átadása a **feladat** objektum korábban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="6fa54-154">This also defines the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="6fa54-155">Folytassa a **tasklist.js** fájl által használt metódusok hozzáadásával **showTasks**, **Addtasks**, és **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="6fa54-155">Continue adding to the **tasklist.js** file by adding the methods used to **showTasks**, **addTask**, and **completeTasks**:</span></span>

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

4. <span data-ttu-id="6fa54-156">Mentse a **tasklist.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6fa54-156">Save the **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="6fa54-157">Az app.js fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="6fa54-157">Modify app.js</span></span>
1. <span data-ttu-id="6fa54-158">Az a **WebRole1** könyvtár, nyissa meg a **app.js** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="6fa54-158">In the **WebRole1** directory, open the **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="6fa54-159">A fájl elején adja hozzá a következő, azure-moduljának betöltése, és állítsa be a táblázat nevét, és a partíció kulcsot:</span><span class="sxs-lookup"><span data-stu-id="6fa54-159">At the beginning of the file, add the following to load the azure module and set the table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="6fa54-160">Az app.js fájlban görgessen le, ahol megjelenik a következő sort:</span><span class="sxs-lookup"><span data-stu-id="6fa54-160">In the app.js file, scroll down to where you see the following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="6fa54-161">A fenti sorok cserélje le az alább látható kódot.</span><span class="sxs-lookup"><span data-stu-id="6fa54-161">Replace the above lines with the code shown below.</span></span> <span data-ttu-id="6fa54-162">Ez egy példányát inicializálja <strong>feladat</strong> a tárfiók kapcsolattal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="6fa54-162">This will initialize an instance of <strong>Task</strong> with a connection to your storage account.</span></span> <span data-ttu-id="6fa54-163">Ez átadott a <strong>TaskList</strong>, amelyek segítségével kommunikálnak a Table szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="6fa54-163">This is passed to the <strong>TaskList</strong>, which will use it to communicate with the Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="6fa54-164">Mentse a **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6fa54-164">Save the **app.js** file.</span></span>

### <a name="modify-the-index-view"></a><span data-ttu-id="6fa54-165">Az index nézetről módosítása</span><span class="sxs-lookup"><span data-stu-id="6fa54-165">Modify the index view</span></span>
1. <span data-ttu-id="6fa54-166">Lépjen a **nézetek** könyvtárhoz, és nyissa meg a **index.jade** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="6fa54-166">Change directories to the **views** directory and open the **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="6fa54-167">Cserélje le a tartalmát a **index.jade** fájlt az alábbi kóddal.</span><span class="sxs-lookup"><span data-stu-id="6fa54-167">Replace the contents of the **index.jade** file with the code below.</span></span> <span data-ttu-id="6fa54-168">Ez határozza meg a nézet a meglévő feladatokat, valamint az új feladatok hozzáadása és meglévőket megjelölés befejezettként űrlap megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="6fa54-168">This defines the view for displaying existing tasks, as well as a form for adding new tasks and marking existing ones as completed.</span></span>

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

3. <span data-ttu-id="6fa54-169">Mentse és zárja be **index.jade** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6fa54-169">Save and close **index.jade** file.</span></span>

### <a name="modify-the-global-layout"></a><span data-ttu-id="6fa54-170">A globális elrendezés módosítása</span><span class="sxs-lookup"><span data-stu-id="6fa54-170">Modify the global layout</span></span>
<span data-ttu-id="6fa54-171">A rendszer a **views** (nézetek) könyvtárban található **layout.jade** fájlt használja a többi **.jade** fájl globális sablonjaként.</span><span class="sxs-lookup"><span data-stu-id="6fa54-171">The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="6fa54-172">Ebben a lépésben ezt a sablont a [Twitter Bootstrap](https://github.com/twbs/bootstrap) eszközkészletre módosítja majd, amellyel könnyen tervezhet tetszetős webhelyeket.</span><span class="sxs-lookup"><span data-stu-id="6fa54-172">In this step you will modify it to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking website.</span></span>

1. <span data-ttu-id="6fa54-173">Töltse le és csomagolja ki a fájlokat a [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="6fa54-173">Download and extract the files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="6fa54-174">Másolás a **bootstrap.min.css** fájlt a **bootstrap\\eloszlás\\css** mappát a **nyilvános\\stíluslapok** az tasklist alkalmazás könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6fa54-174">Copy the **bootstrap.min.css** file from the **bootstrap\\dist\\css** folder to the **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="6fa54-175">Az a **nézetek** mappa, nyissa meg a **Views** a szövegszerkesztőben, és cserélje ki a tartalmát a következőre:</span><span class="sxs-lookup"><span data-stu-id="6fa54-175">From the **views** folder, open the **layout.jade** in your text editor and replace the contents with the following:</span></span>

    <span data-ttu-id="6fa54-176">DOCTYPE html html központi cím cím hivatkozás = (rel = "xsl", href='/stylesheets/bootstrap.min.css) hivatkozás (rel = "xsl", href='/stylesheets/style.css) body.app nav.navbar.navbar alapértelmezett div.navbar-fejléc a.navbar-brand(href='/') a  Feladatok blokkolja a tartalmat</span><span class="sxs-lookup"><span data-stu-id="6fa54-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="6fa54-177">Mentse a **Views** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6fa54-177">Save the **layout.jade** file.</span></span>

### <a name="running-the-application-in-the-emulator"></a><span data-ttu-id="6fa54-178">Az alkalmazás futtatása az emulátorban</span><span class="sxs-lookup"><span data-stu-id="6fa54-178">Running the Application in the Emulator</span></span>
<span data-ttu-id="6fa54-179">A következő paranccsal indítsa el az alkalmazást az emulátorban.</span><span class="sxs-lookup"><span data-stu-id="6fa54-179">Use the following command to start the application in the emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="6fa54-180">A böngésző csak akkor nyílik meg, és megjeleníti a következő lapot:</span><span class="sxs-lookup"><span data-stu-id="6fa54-180">The browser will open and displays the following page:</span></span>

![A webes lapozható saját feladatlista című feladatok és adjon hozzá egy új feladatot mezőket tartalmazó tábla.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

<span data-ttu-id="6fa54-182">Ezen a képernyőn elemek hozzáadását, vagy távolítsa el a meglévő elemeket befejezettként jelöli meg őket.</span><span class="sxs-lookup"><span data-stu-id="6fa54-182">Use the form to add items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-the-application-to-azure"></a><span data-ttu-id="6fa54-183">Az Azure-bA az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="6fa54-183">Publishing the Application to Azure</span></span>
<span data-ttu-id="6fa54-184">A Windows PowerShell-ablakban hívja meg a következő parancsmagot újratelepíteni az üzemeltetett szolgáltatás az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="6fa54-184">In the Windows PowerShell window, call the following cmdlet to redeploy your hosted service to Azure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="6fa54-185">Cserélje le **myuniquename** ehhez az alkalmazáshoz, egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="6fa54-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="6fa54-186">Cserélje le **datacentername** nevű, egy Azure-adatközponthoz, például a **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="6fa54-186">Replace **datacentername** with the name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="6fa54-187">A telepítés befejezése után, a következőhöz hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="6fa54-187">After the deployment is complete, you should see a response similar to the following:</span></span>

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

<span data-ttu-id="6fa54-188">Mint korábban mert a megadott a **-elindítása** beállítást, a böngészőben megnyílik, és megjeleníti az alkalmazás Azure-beli közzététel befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="6fa54-188">As before, because you specified the **-launch** option, the browser opens and displays your application running in Azure when publishing is completed.</span></span>

![A saját feladatlista lapot megjelenítő böngészőablak.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="6fa54-191">Leállítása és az alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="6fa54-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="6fa54-192">Után az alkalmazás telepítéséhez, érdemes lehet le kell tiltani, költségek elkerülése vagy létrehozhatja és más alkalmazások telepítése a ingyenes próba időn belül.</span><span class="sxs-lookup"><span data-stu-id="6fa54-192">After deploying your application, you may want to disable it so you can avoid costs or build and deploy other applications within the free trial time period.</span></span>

<span data-ttu-id="6fa54-193">Az Azure a webesszerepkör-példányok esetében óránként számol fel díjat a felhasznált kiszolgálóidő után.</span><span class="sxs-lookup"><span data-stu-id="6fa54-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="6fa54-194">A kiszolgálóidő felhasználása az alkalmazás üzembe helyezésétől kezdődik, még akkor is, ha a példányok nem futnak, és leállított állapotban vannak.</span><span class="sxs-lookup"><span data-stu-id="6fa54-194">Server time is consumed once your application is deployed, even if the instances are not running and are in the stopped state.</span></span>

<span data-ttu-id="6fa54-195">A következő lépések bemutatják a állítsa le és törölje az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6fa54-195">The following steps show you how to stop and delete your application.</span></span>

1. <span data-ttu-id="6fa54-196">Állítsa le az előző szakaszban létrehozott szolgáltatástelepítést a Windows PowerShell-ablakban az alábbi parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="6fa54-196">In the Windows PowerShell window, stop the service deployment created in the previous section with the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="6fa54-197">A szolgáltatás leállítása eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="6fa54-197">Stopping the service may take several minutes.</span></span> <span data-ttu-id="6fa54-198">Miután a szolgáltatás leállt, kap egy üzenetet, amely tájékoztatja a leállásról.</span><span class="sxs-lookup"><span data-stu-id="6fa54-198">When the service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="6fa54-199">A szolgáltatás törléséhez hívja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6fa54-199">To delete the service, call the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="6fa54-200">Ha a rendszer rákérdez, írja be az **Y** karaktert a szolgáltatás törléséhez.</span><span class="sxs-lookup"><span data-stu-id="6fa54-200">When prompted, enter **Y** to delete the service.</span></span>

   <span data-ttu-id="6fa54-201">A szolgáltatás törlése eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="6fa54-201">Deleting the service may take several minutes.</span></span> <span data-ttu-id="6fa54-202">Miután megtörtént a szolgáltatás törlése, kap egy üzenetet, amely tájékoztatást ad erről.</span><span class="sxs-lookup"><span data-stu-id="6fa54-202">After the service has been deleted you receive a message indicating that the service was deleted.</span></span>

<span data-ttu-id="6fa54-203">[Express használata Node.js-webalkalmazás]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/</span><span class="sxs-lookup"><span data-stu-id="6fa54-203">[Node.js Web Application using Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/</span></span>
<span data-ttu-id="6fa54-204">[tárolása és az adatok elérése az Azure-ban]: http://msdn.microsoft.com/library/azure/gg433040.aspx</span><span class="sxs-lookup"><span data-stu-id="6fa54-204">[Storing and Accessing Data in Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx</span></span>
<span data-ttu-id="6fa54-205">[Node.js-webalkalmazás]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/</span><span class="sxs-lookup"><span data-stu-id="6fa54-205">[Node.js Web Application]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/</span></span>


