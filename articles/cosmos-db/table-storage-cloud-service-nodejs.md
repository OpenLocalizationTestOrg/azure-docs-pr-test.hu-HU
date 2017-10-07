---
title: "a table storage (Node.js) aaaWeb alkalmazás |} Microsoft Docs"
description: "Ez az oktatóanyag épít hello webalkalmazás Express oktatóanyag az Azure Storage szolgáltatás és hello Azure modul hozzáadásával."
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
ms.openlocfilehash: 7eefc09baab61cf44c98183135abe572b11812e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="dc1b4-103">Storage használata node.js-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="dc1b4-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="dc1b4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="dc1b4-104">Overview</span></span>
<span data-ttu-id="dc1b4-105">Ebben az oktatóanyagban hello-ben létrehozott alkalmazás a [Express használata Node.js-webalkalmazás] oktatóanyag ki van bővítve hello Microsoft Azure Ügyfélkódtárai használata Node.js toowork management szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-105">In this tutorial, hello application you created in the [Node.js Web Application using Express] tutorial is extended using hello Microsoft Azure Client Libraries for Node.js toowork with data management services.</span></span> <span data-ttu-id="dc1b4-106">Hozzon létre egy webalapú feladatlista alkalmazás, hogy tooAzure telepíthet kiterjeszti az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-106">You extend your application by creating a web-based task-list application that you can deploy tooAzure.</span></span> <span data-ttu-id="dc1b4-107">hello feladatlista lehetővé teszi a felhasználóknak feladatokat beolvasni, adja hozzá az új feladatok és feladatok megjelölése befejezettként.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-107">hello task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="dc1b4-108">hello feladat tárolódnak az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-108">hello task items are stored in Azure Storage.</span></span> <span data-ttu-id="dc1b4-109">Az Azure Storage biztosítja a hibatűrő és magas rendelkezésre állású strukturálatlan adatok tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="dc1b4-110">Az Azure Storage számos adatszerkezetek, ahol tárolhatja és érheti adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-110">Azure Storage includes several data structures where you can store and access data.</span></span> <span data-ttu-id="dc1b4-111">Hello tárolószolgáltatások a hello API-k szereplő hello Azure SDK for Node.js vagy a REST API-kon keresztül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-111">You can use hello storage services from hello APIs included in hello Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="dc1b4-112">További információkért lásd: [tárolása és az adatok elérése az Azure-ban].</span><span class="sxs-lookup"><span data-stu-id="dc1b4-112">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="dc1b4-113">Ez az oktatóanyag feltételezi, hogy végrehajtotta-e az hello [Node.js-webalkalmazás] és [expressz Node.js][Express használata Node.js-webalkalmazás] oktatóanyagok.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-113">This tutorial assumes that you have completed hello [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="dc1b4-114">Hello a következő információkat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-114">It contains hello following information:</span></span>

* <span data-ttu-id="dc1b4-115">Hogyan toowork a hello Jade sablon motor</span><span class="sxs-lookup"><span data-stu-id="dc1b4-115">How toowork with hello Jade template engine</span></span>
* <span data-ttu-id="dc1b4-116">Hogyan toowork Azure adatok szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="dc1b4-116">How toowork with Azure Data Management services</span></span>

<span data-ttu-id="dc1b4-117">a következő képernyőkép hello befejeződött hello alkalmazás látható:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-117">hello following screenshot shows hello completed application:</span></span>

![hello befejeződött az internet explorer weblapot](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="dc1b4-119">Tárolási hitelesítő adatok beállítása a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="dc1b4-119">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="dc1b4-120">Át kell adnia a tároló hitelesítő adatait tooaccess Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-120">You must pass in storage credentials tooaccess Azure Storage.</span></span> <span data-ttu-id="dc1b4-121">Ez való hello web.config Alkalmazásbeállítások használatával történik.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-121">This is done by utilizing hello web.config application settings.</span></span>
<span data-ttu-id="dc1b4-122">hello web.config beállítások átadása pedig környezeti változók tooNode, amely majd olvassák hello Azure SDK-t.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-122">hello web.config settings are passed as environment variables tooNode, which are then read by hello Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="dc1b4-123">Tárolási csak szolgálnak, ha hello alkalmazás telepített tooAzure.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-123">Storage credentials are only used when hello application is deployed tooAzure.</span></span> <span data-ttu-id="dc1b4-124">Hello emulátorban futtatásakor hello alkalmazás a hello storage emulator használja.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-124">When running in hello emulator, hello application uses hello storage emulator.</span></span>
>
>

<span data-ttu-id="dc1b4-125">Hajtsa végre a következő lépéseket tooretrieve hello tárfiók hitelesítő adatainak hello, és vegye fel őket toohello web.config beállítást:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-125">Perform hello following steps tooretrieve hello storage account credentials and add them toohello web.config settings:</span></span>

1. <span data-ttu-id="dc1b4-126">Még nincs nyitva, hogy kezdődnie hello Azure PowerShell hello **Start** kibontásával menü **minden program és az Azure-**, kattintson a jobb gombbal **Azure PowerShell**, majd válassza ki a  **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-126">If it is not already open, start hello Azure PowerShell from hello **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="dc1b4-127">Módosítsa a könyvtárakat toohello mappát, amely tartalmazza az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-127">Change directories toohello folder containing your application.</span></span> <span data-ttu-id="dc1b4-128">Például a C:\\csomópont\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-128">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="dc1b4-129">Adja meg a következő parancsmag tooretrieve hello tárfiókadatok hello hello Azure Powershell-ablakban:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-129">From hello Azure Powershell window, enter hello following cmdlet tooretrieve hello storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="dc1b4-130">hello előző parancsmag beolvassa hello listában tárfiókok és az üzemeltetett szolgáltatás társított kulcsait.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-130">hello preceding cmdlet retrieves hello list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dc1b4-131">Hello Azure SDK tárfiók létrehozása, ha a szolgáltatás telepítése, mert a tárfiók már léteznie kell a hello előző útmutatók az alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-131">Since hello Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in hello previous guides.</span></span>
   >
   >
4. <span data-ttu-id="dc1b4-132">Nyissa meg hello **ServiceDefinition.csdef** Ha hello alkalmazás telepített tooAzure használt hello környezet beállításokat tartalmazó fájlt:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-132">Open hello **ServiceDefinition.csdef** file containing hello environment settings that are used when hello application is deployed tooAzure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="dc1b4-133">INSERT hello következő letiltása a **környezet** elemet, és a {TÁRFIÓK} és {TÁRELÉRÉSI kulcs} hello fióknévvel és hello elsődleges kulcsának hello toouse szánt központi telepítés:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-133">Insert hello following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with hello account name and hello primary key for hello storage account you want toouse for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![hello web.cloud.config fájl tartalma](./media/table-storage-cloud-service-nodejs/node37.png)

6. <span data-ttu-id="dc1b4-135">Hello fájlt mentse és zárja be a Jegyzettömböt.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-135">Save hello file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="dc1b4-136">A kiegészítő modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="dc1b4-136">Install additional modules</span></span>
1. <span data-ttu-id="dc1b4-137">Használjon hello a következő parancs tooinstall hello [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok helyileg, valamint toosave bejegyzése számukra toohello **package.json** fájlt:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-137">Use hello following command tooinstall hello [azure], [node-uuid], [nconf] and [async] modules locally as well as toosave an entry for them toohello **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="dc1b4-138">a parancs kimenetének hello hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-138">hello output of this command should appear similar toohello following:</span></span>

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

## <a name="using-hello-table-service-in-a-node-application"></a><span data-ttu-id="dc1b4-139">Hello Table szolgáltatás használata node.js-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="dc1b4-139">Using hello Table service in a node application</span></span>
<span data-ttu-id="dc1b4-140">Ebben a szakaszban hello alapszintű hello által létrehozott alkalmazás **expressz** parancs hozzáadásával ki van bővítve a **task.js** a tevékenységek hello modellt tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-140">In this section, hello basic application created by hello **express** command is extended by adding a **task.js** file containing hello model for your tasks.</span></span> <span data-ttu-id="dc1b4-141">Módosítsa a meglévő hello **app.js** fájlt, és hozzon létre egy új **tasklist.js** fájlt, amely hello modellt használ.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-141">Modify hello existing **app.js** file and create a new **tasklist.js** file that uses hello model.</span></span>

### <a name="create-hello-model"></a><span data-ttu-id="dc1b4-142">Hello modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc1b4-142">Create hello model</span></span>
1. <span data-ttu-id="dc1b4-143">A hello **WebRole1** könyvtár, hozzon létre egy új könyvtárat nevű **modellek**.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-143">In hello **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="dc1b4-144">A hello **modellek** könyvtár, hozzon létre egy új fájlt **task.js**.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-144">In hello **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="dc1b4-145">Ez a fájl tartalmazza az alkalmazás által létrehozott hello feladatok hello modelljét.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-145">This file contains hello model for hello tasks created by your application.</span></span>
3. <span data-ttu-id="dc1b4-146">Hello hello elején **task.js** fájlt, adja hozzá a következő kód szükséges tooreference szalagtárak hello:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-146">At hello beginning of hello **task.js** file, add hello following code tooreference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="dc1b4-147">Ezután adja hozzá a kódot toodefine és hello feladat objektum exportálása.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-147">Next, add code toodefine and export hello Task object.</span></span> <span data-ttu-id="dc1b4-148">hello feladatobjektum toohello tábla csatlakozás felelős.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-148">hello Task object is responsible for connecting toohello table.</span></span>

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

5. <span data-ttu-id="dc1b4-149">Ezután adja hozzá hello kód toodefine további módszereket követően hello feladatobjektum, amelyek lehetővé teszik az hello táblában tárolt adatok interakció:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-149">Next, add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in hello table:</span></span>

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
        // use entityGenerator tooset types
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

6. <span data-ttu-id="dc1b4-150">Mentse és zárja be a hello **task.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-150">Save and close hello **task.js** file.</span></span>

### <a name="create-hello-controller"></a><span data-ttu-id="dc1b4-151">Hello tartományvezérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc1b4-151">Create hello controller</span></span>
1. <span data-ttu-id="dc1b4-152">A hello **WebRole1/útvonalak** könyvtár, hozzon létre egy új fájlt **tasklist.js** , majd nyissa meg szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-152">In hello **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="dc1b4-153">Adja hozzá a következő kód túl hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-153">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="dc1b4-154">Ez a kód betölti hello azure és a modulok aszinkron által használt **tasklist.js** és hello meghatározása **TaskList** függvény, amelyet hello példányának **feladat** azt objektum korábban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-154">This code loads hello azure and async modules, which are used by **tasklist.js** and defines hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="dc1b4-155">Hozzáadja a toohello **tasklist.js** fájl túl használt hello metódusok hozzáadásával**showTasks**, **Addtasks**, és **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-155">Continue adding toohello **tasklist.js** file by adding hello methods used too**showTasks**, **addTask**, and **completeTasks**:</span></span>

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

4. <span data-ttu-id="dc1b4-156">Mentse a hello **tasklist.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-156">Save hello **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="dc1b4-157">Az app.js fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="dc1b4-157">Modify app.js</span></span>
1. <span data-ttu-id="dc1b4-158">A hello **WebRole1** könyvtárába, nyissa meg hello **app.js** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-158">In hello **WebRole1** directory, open hello **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="dc1b4-159">Elején hello hello fájlt, adja hozzá a következő tooload hello azure modul hello és hello tábla nevét és a partíciós kulcs beállítása:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-159">At hello beginning of hello file, add hello following tooload hello azure module and set hello table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="dc1b4-160">Hello app.js fájlban, görgessen lefelé látja toowhere hello a következő sort:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-160">In hello app.js file, scroll down toowhere you see hello following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="dc1b4-161">Cserélje le a következő kód hello sorok megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-161">Replace hello preceding lines with hello following code.</span></span> <span data-ttu-id="dc1b4-162">Ez a kód egy példányát inicializálja <strong>feladat</strong> kapcsolat tooyour storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-162">This code initializes an instance of <strong>Task</strong> with a connection tooyour storage account.</span></span> <span data-ttu-id="dc1b4-163">Hello <strong>feladat</strong> toohello átadása <strong>TaskList</strong>, melyik használ az toocommunicate hello tábla szolgáltatással:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-163">hello <strong>Task</strong> is passed toohello <strong>TaskList</strong>, which uses it toocommunicate with hello Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="dc1b4-164">Mentse a hello **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-164">Save hello **app.js** file.</span></span>

### <a name="modify-hello-index-view"></a><span data-ttu-id="dc1b4-165">Hello index nézet módosítása</span><span class="sxs-lookup"><span data-stu-id="dc1b4-165">Modify hello index view</span></span>
1. <span data-ttu-id="dc1b4-166">Módosítsa a könyvtárakat toohello **nézetek** könyvtárhoz, és nyissa meg hello **index.jade** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-166">Change directories toohello **views** directory and open hello **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="dc1b4-167">Cserélje le a hello hello tartalmát **index.jade** hello kód a következő fájl.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-167">Replace hello contents of hello **index.jade** file with hello following code.</span></span> <span data-ttu-id="dc1b4-168">Ezzel a kóddal hello nézetet meglévő feladatok megjelenítéséhez határozza meg, és határozza meg az új feladatok hozzáadása és meglévőket megjelölés befejezettként űrlap.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-168">This code defines hello view for displaying existing tasks, and defines a form for adding new tasks and marking existing ones as completed.</span></span>

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

3. <span data-ttu-id="dc1b4-169">Mentse és zárja be **index.jade** fájlt.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-169">Save and close **index.jade** file.</span></span>

### <a name="modify-hello-global-layout"></a><span data-ttu-id="dc1b4-170">Hello globális elrendezés módosítása</span><span class="sxs-lookup"><span data-stu-id="dc1b4-170">Modify hello global layout</span></span>
<span data-ttu-id="dc1b4-171">Hello **Views** hello fájlban **nézetek** directory globális sablonként szolgál az egyéb **.jade** fájlokat.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-171">hello **layout.jade** file in hello **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="dc1b4-172">Ebben a lépésben módosítsa a hello **Views** toouse fájl [Twitter Bootstrap](https://github.com/twbs/bootstrap), amelyen egy eszközkészlet, így könnyen toodesign egy töltött tetszetős webhelyeket.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-172">In this step, modify hello **layout.jade** file toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking website.</span></span>

1. <span data-ttu-id="dc1b4-173">A hello fájlok letöltéséhez és kibontásához [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="dc1b4-173">Download and extract hello files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="dc1b4-174">Másolás hello **bootstrap.min.css** hello fájlt **bootstrap\\eloszlás\\css** mappa toohello **nyilvános\\stíluslapok** az tasklist alkalmazás könyvtár.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-174">Copy hello **bootstrap.min.css** file from hello **bootstrap\\dist\\css** folder toohello **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="dc1b4-175">A hello **nézetek** mappa, nyissa meg hello **Views** fájlt a szövegszerkesztőben, és cserélje ki hello tartalmát hello következőre:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-175">From hello **views** folder, open hello **layout.jade** file in your text editor and replace hello contents with hello following:</span></span>

    <span data-ttu-id="dc1b4-176">DOCTYPE html html központi cím cím hivatkozás = (rel = "xsl", href='/stylesheets/bootstrap.min.css) hivatkozás (rel = "xsl", href='/stylesheets/style.css) body.app nav.navbar.navbar alapértelmezett div.navbar-fejléc a.navbar-brand(href='/') a  Feladatok blokkolja a tartalmat</span><span class="sxs-lookup"><span data-stu-id="dc1b4-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="dc1b4-177">Mentse a hello **Views** fájlt.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-177">Save hello **layout.jade** file.</span></span>

### <a name="running-hello-application-in-hello-emulator"></a><span data-ttu-id="dc1b4-178">Hello emulátor hello alkalmazás fut</span><span class="sxs-lookup"><span data-stu-id="dc1b4-178">Running hello Application in hello Emulator</span></span>
<span data-ttu-id="dc1b4-179">A következő parancs toostart hello alkalmazás hello emulátorban hello használata.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-179">Use hello following command toostart hello application in hello emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="dc1b4-180">hello böngészőben nyílik meg, és megjeleníti a következő lap hello:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-180">hello browser opens and displays hello following page:</span></span>

![A webes lapozható című saját feladatlista feladatok és a mezők tooadd tartalmazó tábla egy új feladatot.](./media/table-storage-cloud-service-nodejs/node44.png)

<span data-ttu-id="dc1b4-182">Hello űrlap tooadd elemek használja, vagy távolítsa el a meglévő elemeket befejezettként jelöli meg őket.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-182">Use hello form tooadd items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-hello-application-tooazure"></a><span data-ttu-id="dc1b4-183">Közzétételi hello alkalmazás tooAzure</span><span class="sxs-lookup"><span data-stu-id="dc1b4-183">Publishing hello Application tooAzure</span></span>
<span data-ttu-id="dc1b4-184">Windows PowerShell-ablakban hello hívja a következő parancsmag tooredeploy hello az üzemeltetett szolgáltatás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-184">In hello Windows PowerShell window, call hello following cmdlet tooredeploy your hosted service tooAzure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="dc1b4-185">Cserélje le **myuniquename** ehhez az alkalmazáshoz, egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="dc1b4-186">Cserélje le **datacentername** hello nevet, egy Azure-adatközponthoz, például a **USA nyugati régiója**.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-186">Replace **datacentername** with hello name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="dc1b4-187">Hello telepítés befejezése után, a válasz hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-187">After hello deployment is complete, you should see a response similar toohello following:</span></span>

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist tooMicrosoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package toostorage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

<span data-ttu-id="dc1b4-188">Hello megadásával **-elindítása** hello előző parancsmag, hello böngésző beállítás megnyílik, és megjeleníti az alkalmazás Azure-beli közzététel befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-188">By specifying hello **-launch** option in hello previous cmdlet, hello browser opens and displays your application running in Azure when publishing is completed.</span></span>

![Hello saját feladatlista lapot megjelenítő böngészőablak.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="dc1b4-191">Leállítása és az alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="dc1b4-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="dc1b4-192">Után az alkalmazás telepítéséhez, érdemes lehet toodisable, így költségek elkerülése vagy létrehozhatja és más alkalmazások belül hello központi telepítése ingyenes próbaverziós időszak.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-192">After deploying your application, you may want toodisable it so you can avoid costs or build and deploy other applications within hello free trial time period.</span></span>

<span data-ttu-id="dc1b4-193">Az Azure a webesszerepkör-példányok esetében óránként számol fel díjat a felhasznált kiszolgálóidő után.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="dc1b4-194">Kiszolgálói felhasznált után az alkalmazás van telepítve, akkor is, ha a példányok nem futnak, és hello leállt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-194">Server time is consumed once your application is deployed, even if the instances are not running and are in hello stopped state.</span></span>

<span data-ttu-id="dc1b4-195">hello következő lépések bemutatják, hogyan toostop és törli az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-195">hello following steps show you how toostop and delete your application.</span></span>

1. <span data-ttu-id="dc1b4-196">Hello Windows PowerShell-ablakban állítsa le a hello szolgáltatás központi telepítése a következő parancsmag hello hello előző szakaszban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-196">In hello Windows PowerShell window, stop hello service deployment created in hello previous section with hello following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="dc1b4-197">Hello szolgáltatás leállítása eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-197">Stopping hello service may take several minutes.</span></span> <span data-ttu-id="dc1b4-198">Hello szolgáltatás leáll, amikor megjelenik egy üzenet, amely azt jelzi, hogy leállt.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-198">When hello service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="dc1b4-199">toodelete hello szolgáltatást, a következő parancsmag hívás hello:</span><span class="sxs-lookup"><span data-stu-id="dc1b4-199">toodelete hello service, call hello following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="dc1b4-200">Amikor a rendszer kéri, adja meg a **Y** toodelete hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-200">When prompted, enter **Y** toodelete hello service.</span></span>

   <span data-ttu-id="dc1b4-201">Hello szolgáltatás törlése eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-201">Deleting hello service may take several minutes.</span></span> <span data-ttu-id="dc1b4-202">Hello szolgáltatás törlését követően kapni fog egy üzenet, amely azt jelzi, hogy törölve lett-e a hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="dc1b4-202">After hello service is deleted, you will receive a message indicating that hello service was deleted.</span></span>

[Express használata Node.js-webalkalmazás]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[tárolása és az adatok elérése az Azure-ban]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-webalkalmazás]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


