---
title: "Az Azure Table Service-t használó Node.js-alapú webalkalmazás"
description: "Ez az oktatóanyag útmutatást ad az Azure Table szolgáltatás használata az Azure App Service Web Apps egy Node.js-alkalmazás, amely üzemelteti tárolására."
tags: azure-portal
services: app-service\web, storage
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 029e6f46-f586-4309-adbf-71c7b8d537d4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 3252914934c1084a165fa39ee983d3039e04d567
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="nodejs-web-app-using-the-azure-table-service"></a><span data-ttu-id="97b32-103">Az Azure Table Service-t használó Node.js-alapú webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="97b32-103">Node.js web app using the Azure Table Service</span></span>
## <a name="overview"></a><span data-ttu-id="97b32-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="97b32-104">Overview</span></span>
<span data-ttu-id="97b32-105">Az oktatóanyag bemutatja, hogyan tárolhatja és érheti el az adatok adatkezelés Azure által biztosított Table szolgáltatás segítségével egy [csomópont] üzemeltetett alkalmazás [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="97b32-105">This tutorial shows you how to use Table service provided by Azure Data Management to store and access data from a [node] application hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="97b32-106">Ez az oktatóanyag feltételezi, hogy rendelkezik némi tapasztalattal a csomópont használatát és [Git].</span><span class="sxs-lookup"><span data-stu-id="97b32-106">This tutorial assumes that you have some prior experience using node and [Git].</span></span>

<span data-ttu-id="97b32-107">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="97b32-107">You will learn:</span></span>

* <span data-ttu-id="97b32-108">A csomópont-modulok telepítése (csomópont Csomagkezelő) npm segítségével</span><span class="sxs-lookup"><span data-stu-id="97b32-108">How to use npm (node package manager) to install the node modules</span></span>
* <span data-ttu-id="97b32-109">Az Azure Table szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="97b32-109">How to work with the Azure Table service</span></span>
* <span data-ttu-id="97b32-110">Hogyan webalkalmazás létrehozása az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="97b32-110">How to use the Azure CLI to create a web app.</span></span>

<span data-ttu-id="97b32-111">Az oktatóanyag utasításait követve egy egyszerű, webalapú fog létrehozni, amely lehetővé teszi a létrehozása, beolvasása és feladatok végrehajtása "Feladatlista" alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="97b32-111">By following this tutorial, you will build a simple web-based "to-do list" application that allows creating, retrieving and completing tasks.</span></span> <span data-ttu-id="97b32-112">A Table szolgáltatás tárolja a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="97b32-112">The tasks are stored in the Table service.</span></span>

<span data-ttu-id="97b32-113">A kész alkalmazás a következő:</span><span class="sxs-lookup"><span data-stu-id="97b32-113">Here is the completed application:</span></span>

![Egy üres tasklist szöveget megjelenítő weblap][node-table-finished]

> [!NOTE]
> <span data-ttu-id="97b32-115">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="97b32-115">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="97b32-116">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="97b32-116">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="97b32-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="97b32-117">Prerequisites</span></span>
<span data-ttu-id="97b32-118">Mielőtt Ez a cikk utasításait követve ellenőrizze, hogy a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="97b32-118">Before following the instructions in this article, ensure that you have the following installed:</span></span>

* <span data-ttu-id="97b32-119">[csomópont] 0.10.24 verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="97b32-119">[node] version 0.10.24 or higher</span></span>
* <span data-ttu-id="97b32-120">[Git]</span><span class="sxs-lookup"><span data-stu-id="97b32-120">[Git]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a><span data-ttu-id="97b32-121">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="97b32-121">Create a storage account</span></span>
<span data-ttu-id="97b32-122">Hozzon létre egy Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="97b32-122">Create an Azure storage account.</span></span> <span data-ttu-id="97b32-123">Az alkalmazás a Tennivalólista elemein tárolására fog használni ezt a fiókot.</span><span class="sxs-lookup"><span data-stu-id="97b32-123">The app will use this account to store the to-do items.</span></span>

1. <span data-ttu-id="97b32-124">Jelentkezzen be a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="97b32-124">Log into the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="97b32-125">Kattintson a **új** a portál bal alsó ikonra, majd kattintson a **adatok + tárolás** > **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="97b32-125">Click the **New** icon on the bottom left of the portal, then click **Data + Storage** > **Storage**.</span></span> <span data-ttu-id="97b32-126">A storage-fiók egy egyedi nevet adjon, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.</span><span class="sxs-lookup"><span data-stu-id="97b32-126">Give the storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Új gomb](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)
   
    <span data-ttu-id="97b32-128">A storage-fiók létrehozásakor a **értesítések** gomb villogjon, egy zöld **sikeres** és a storage-fiók panelje meg nyitva, hogy a létrehozott új erőforráscsoporthoz tartozó megjeleníthető.</span><span class="sxs-lookup"><span data-stu-id="97b32-128">When the storage account has been created, the **Notifications** button will flash a green **SUCCESS** and the storage account's blade is open to show that it belongs to the new resource group you created.</span></span>
3. <span data-ttu-id="97b32-129">A storage-fiók paneljén kattintson **beállítások** > **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="97b32-129">In the storage account's blade, click **Settings** > **Keys**.</span></span> <span data-ttu-id="97b32-130">Az elsődleges elérési kulcsot másolja a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="97b32-130">Copy the primary access key to the clipboard.</span></span>
   
    ![A hozzáférési kulcsot][portal-storage-access-keys]

## <a name="install-modules-and-generate-scaffolding"></a><span data-ttu-id="97b32-132">Modulok telepítése és állványok készítése</span><span class="sxs-lookup"><span data-stu-id="97b32-132">Install modules and generate scaffolding</span></span>
<span data-ttu-id="97b32-133">Ez a szakasz hozzon létre egy új csomópont alkalmazást, és modul-csomagokat adjon az npm segítségével.</span><span class="sxs-lookup"><span data-stu-id="97b32-133">In this section you will create a new Node application and use npm to add module packages.</span></span> <span data-ttu-id="97b32-134">Az alkalmazás fogja használni a [Express] és [Azure] modulok.</span><span class="sxs-lookup"><span data-stu-id="97b32-134">For this application you will use the [Express] and [Azure] modules.</span></span> <span data-ttu-id="97b32-135">Az Express modul modell nézetvezérlő keretet biztosít a csomópont, amíg az Azure modulok kapcsolatot biztosít annak a Table szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="97b32-135">The Express module provides a Model View Controller framework for node, while the Azure modules provides connectivity to the Table service.</span></span>

### <a name="install-express-and-generate-scaffolding"></a><span data-ttu-id="97b32-136">Express telepítése és állványok készítése</span><span class="sxs-lookup"><span data-stu-id="97b32-136">Install express and generate scaffolding</span></span>
1. <span data-ttu-id="97b32-137">A parancssorból hozzon létre egy új könyvtárat, nevű **tasklist** és a könyvtárhoz.</span><span class="sxs-lookup"><span data-stu-id="97b32-137">From the command line, create a new directory named **tasklist** and switch to that directory.</span></span>  
2. <span data-ttu-id="97b32-138">Adja meg a következő parancsot a Express modul telepítése.</span><span class="sxs-lookup"><span data-stu-id="97b32-138">Enter the following command to install the Express module.</span></span>
   
        npm install express-generator@4.2.0 -g
   
    <span data-ttu-id="97b32-139">Attól függően, hogy az operációs rendszer, amelyre a parancs előtt a "sudo" lehet szükség:</span><span class="sxs-lookup"><span data-stu-id="97b32-139">Depending on the operating system, you may need to put 'sudo' before the command:</span></span>
   
        sudo npm install express-generator@4.2.0 -g
   
    <span data-ttu-id="97b32-140">Az eredmény jelenik meg a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="97b32-140">The output appears similar to the following example:</span></span>
   
        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)
   
   > [!NOTE]
   > <span data-ttu-id="97b32-141">A "-g" paraméter globálisan telepíti a modult.</span><span class="sxs-lookup"><span data-stu-id="97b32-141">The '-g' parameter installs the module globally.</span></span> <span data-ttu-id="97b32-142">Ily módon használhatjuk **expressz** web app állványok létrehozásához írja be a további információ nélkül.</span><span class="sxs-lookup"><span data-stu-id="97b32-142">That way, we can use **express** to generate web app scaffolding without having to type in additional path information.</span></span>
   > 
   > 
3. <span data-ttu-id="97b32-143">Az alkalmazás állványok létrehozásához adja meg a **expressz** parancs:</span><span class="sxs-lookup"><span data-stu-id="97b32-143">To create the scaffolding for the application, enter the **express** command:</span></span>
   
        express
   
    <span data-ttu-id="97b32-144">Ez a parancs az alábbi példához hasonló jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="97b32-144">The output of this command appears similar to the following example:</span></span>
   
           create : .
           create : ./package.json
           create : ./app.js
           create : ./public
           create : ./public/images
           create : ./routes
           create : ./routes/index.js
           create : ./routes/users.js
           create : ./public/stylesheets
           create : ./public/stylesheets/style.css
           create : ./views
           create : ./views/index.jade
           create : ./views/layout.jade
           create : ./views/error.jade
           create : ./public/javascripts
           create : ./bin
           create : ./bin/www
   
           install dependencies:
             $ cd . && npm install
   
           run the app:
             $ DEBUG=my-application ./bin/www
   
    <span data-ttu-id="97b32-145">Most már rendelkezik néhány új könyvtárak és fájlok a **tasklist** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="97b32-145">You now have several new directories and files in the **tasklist** directory.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="97b32-146">A kiegészítő modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="97b32-146">Install additional modules</span></span>
<span data-ttu-id="97b32-147">Egy fájlt, amely **expressz** hoz létre az **package.json**.</span><span class="sxs-lookup"><span data-stu-id="97b32-147">One of the files that **express** creates is **package.json**.</span></span> <span data-ttu-id="97b32-148">Ez a fájl modul függőségek listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="97b32-148">This file contains a list of module dependencies.</span></span> <span data-ttu-id="97b32-149">Később amikor az App Service Web Apps alkalmazást telepít központilag, ez a fájl határozza meg melyik modulokat kell telepíteni az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="97b32-149">Later, when you deploy the application to App Service Web Apps, this file determines which modules need to be installed on Azure.</span></span>

<span data-ttu-id="97b32-150">Parancsot a parancssorból, írja be a következő parancsot a leírt moduljainak telepítése a **package.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="97b32-150">From the command-line, enter the following command to install the modules described in the **package.json** file.</span></span> <span data-ttu-id="97b32-151">'Sudo' használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="97b32-151">You may need to use 'sudo'.</span></span>

    npm install

<span data-ttu-id="97b32-152">Ez a parancs az alábbi példához hasonló jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="97b32-152">The output of this command appears similar to the following example:</span></span>

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


<span data-ttu-id="97b32-153">Ezután adja meg a következő paranccsal telepíthető a [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok:</span><span class="sxs-lookup"><span data-stu-id="97b32-153">Next, enter the following command to install the [azure], [node-uuid], [nconf] and [async] modules:</span></span>

    npm install azure-storage node-uuid async nconf --save

<span data-ttu-id="97b32-154">A **--mentése** jelző ezekhez a modulokhoz, hogy bejegyzéseket ad a **package.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="97b32-154">The **--save** flag adds entries for these modules to the **package.json** file.</span></span>

<span data-ttu-id="97b32-155">Ez a parancs az alábbi példához hasonló jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="97b32-155">The output of this command appears similar to the following example:</span></span>

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-the-application"></a><span data-ttu-id="97b32-156">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="97b32-156">Create the application</span></span>
<span data-ttu-id="97b32-157">Most még készen áll az alkalmazás létrehozására.</span><span class="sxs-lookup"><span data-stu-id="97b32-157">Now we're ready to build the application.</span></span>

### <a name="create-a-model"></a><span data-ttu-id="97b32-158">Modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="97b32-158">Create a model</span></span>
<span data-ttu-id="97b32-159">A *modell* olyan objektum, amely az alkalmazás adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="97b32-159">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="97b32-160">Az alkalmazás csak a modell egy feladat objektum, amelyen a tennivalók listájára elem..</span><span class="sxs-lookup"><span data-stu-id="97b32-160">For the application, the only model is a task object, which represents an item in the to-do list.</span></span> <span data-ttu-id="97b32-161">A következő mezők feladatnál:</span><span class="sxs-lookup"><span data-stu-id="97b32-161">Tasks will have the following fields:</span></span>

* <span data-ttu-id="97b32-162">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="97b32-162">PartitionKey</span></span>
* <span data-ttu-id="97b32-163">RowKey</span><span class="sxs-lookup"><span data-stu-id="97b32-163">RowKey</span></span>
* <span data-ttu-id="97b32-164">Name (karakterlánc)</span><span class="sxs-lookup"><span data-stu-id="97b32-164">name (string)</span></span>
* <span data-ttu-id="97b32-165">kategória (karakterlánc)</span><span class="sxs-lookup"><span data-stu-id="97b32-165">category (string)</span></span>
* <span data-ttu-id="97b32-166">Befejezett (logikai)</span><span class="sxs-lookup"><span data-stu-id="97b32-166">completed (Boolean)</span></span>

<span data-ttu-id="97b32-167">**PartitionKey** és **RowKey** tábla kulcsként a Table szolgáltatás által használt.</span><span class="sxs-lookup"><span data-stu-id="97b32-167">**PartitionKey** and **RowKey** are used by the Table Service as table keys.</span></span> <span data-ttu-id="97b32-168">További információkért lásd: [ismertetése a Table szolgáltatás adatmodell](https://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="97b32-168">For more information, see [Understanding the Table Service data model](https://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

1. <span data-ttu-id="97b32-169">Az a **tasklist** könyvtár, hozzon létre egy új könyvtárat nevű **modellek**.</span><span class="sxs-lookup"><span data-stu-id="97b32-169">In the **tasklist** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="97b32-170">Az a **modellek** könyvtár, hozzon létre egy új fájlt **task.js**.</span><span class="sxs-lookup"><span data-stu-id="97b32-170">In the **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="97b32-171">Ez a fájl tartalmazza majd a modellt az alkalmazás által létrehozott feladatok számára.</span><span class="sxs-lookup"><span data-stu-id="97b32-171">This file will contain the model for the tasks created by your application.</span></span>
3. <span data-ttu-id="97b32-172">Elején a **task.js** fájlt, adja hozzá a következő kódot a szükséges kódtárak hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="97b32-172">At the beginning of the **task.js** file, add the following code to reference required libraries:</span></span>
   
        var azure = require('azure-storage');
          var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;
4. <span data-ttu-id="97b32-173">Adja hozzá a következő kódot meghatározására és exportálására a feladatobjektum.</span><span class="sxs-lookup"><span data-stu-id="97b32-173">Add the following code to define and export the Task object.</span></span> <span data-ttu-id="97b32-174">Ez az objektum felelős a tábla kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="97b32-174">This object is responsible for connecting to the table.</span></span>
   
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
5. <span data-ttu-id="97b32-175">Adja hozzá a következő kódot a feladatobjektumokhoz további metódusok meghatározásához a feladatobjektumot, amelyek lehetővé teszik a táblában tárolt adatok interakció:</span><span class="sxs-lookup"><span data-stu-id="97b32-175">Add the following code to define additional methods on the Task object, which allow interactions with data stored in the table:</span></span>
   
        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(this.tableName, query, null, function entitiesQueried(error, result) {
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
6. <span data-ttu-id="97b32-176">Mentse és zárja be a **task.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="97b32-176">Save and close the **task.js** file.</span></span>

### <a name="create-a-controller"></a><span data-ttu-id="97b32-177">A tartományvezérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="97b32-177">Create a controller</span></span>
<span data-ttu-id="97b32-178">A *vezérlő* HTTP-kérelmeket kezeli, és ez a beállítás a HTML-válaszban.</span><span class="sxs-lookup"><span data-stu-id="97b32-178">A *controller* handles HTTP requests and renders the HTML response.</span></span>

1. <span data-ttu-id="97b32-179">Az a **tasklist/útvonalak** könyvtár, hozzon létre egy új fájlt **tasklist.js** , majd nyissa meg szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="97b32-179">In the **tasklist/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="97b32-180">Adja hozzá a következő kódot a **tasklist.js** fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="97b32-180">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="97b32-181">Ez betölti az azure és async modult által használt **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="97b32-181">This loads the azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="97b32-182">Ez határozza meg a **TaskList** függvénynek, amely egy példánya átadása a **feladat** objektum korábban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="97b32-182">This also defines the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>
   
        var azure = require('azure-storage');
        var async = require('async');
   
        module.exports = TaskList;
3. <span data-ttu-id="97b32-183">Adja meg a **TaskList** objektum.</span><span class="sxs-lookup"><span data-stu-id="97b32-183">Define a **TaskList** object.</span></span>
   
        function TaskList(task) {
          this.task = task;
        }
4. <span data-ttu-id="97b32-184">Adja hozzá a következő módszerekkel **TaskList**:</span><span class="sxs-lookup"><span data-stu-id="97b32-184">Add the following methods to **TaskList**:</span></span>
   
        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = new azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },
   
          addTask: function(req,res) {
            var self = this;
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

### <a name="modify-appjs"></a><span data-ttu-id="97b32-185">Az app.js fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="97b32-185">Modify app.js</span></span>
1. <span data-ttu-id="97b32-186">Az a **tasklist** könyvtár, nyissa meg a **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="97b32-186">From the **tasklist** directory, open the **app.js** file.</span></span> <span data-ttu-id="97b32-187">Ez a fájl futtatásával korábban létrejött a **expressz** parancsot.</span><span class="sxs-lookup"><span data-stu-id="97b32-187">This file was created earlier by running the **express** command.</span></span>
2. <span data-ttu-id="97b32-188">A fájl elején adja hozzá az azure-moduljának betöltése, a táblázat nevét, a partíciós kulcs, és állítsa be a tároló hitelesítő adatokat használja ebben a példában a következőket:</span><span class="sxs-lookup"><span data-stu-id="97b32-188">At the beginning of the file, add the following to load the azure module, set the table name, partition key, and set the storage credentials used by this example:</span></span>
   
        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");
   
   > [!NOTE]
   > <span data-ttu-id="97b32-189">nconf fog betölteni a konfigurációs értékek vagy környezeti változók vagy a **config.json** fájlt, amely később hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="97b32-189">nconf will load the configuration values from either environment variables or the **config.json** file, which we will create later.</span></span>
   > 
   > 
3. <span data-ttu-id="97b32-190">Az app.js fájlban görgessen le, ahol megjelenik a következő sort:</span><span class="sxs-lookup"><span data-stu-id="97b32-190">In the app.js file, scroll down to where you see the following line:</span></span>
   
        app.use('/', routes);
        app.use('/users', users);
   
    <span data-ttu-id="97b32-191">A fenti sorok cserélje le az alább látható kódot.</span><span class="sxs-lookup"><span data-stu-id="97b32-191">Replace the above lines with the code shown below.</span></span> <span data-ttu-id="97b32-192">Ez egy példányát inicializálja <strong>feladat</strong> a tárfiók kapcsolattal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="97b32-192">This will initialize an instance of <strong>Task</strong> with a connection to your storage account.</span></span> <span data-ttu-id="97b32-193">Ez átadott a <strong>TaskList</strong>, amelyek segítségével kommunikálnak a Table szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="97b32-193">This is passed to the <strong>TaskList</strong>, which will use it to communicate with the Table service:</span></span>
   
        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
4. <span data-ttu-id="97b32-194">Mentse a **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="97b32-194">Save the **app.js** file.</span></span>

### <a name="modify-the-index-view"></a><span data-ttu-id="97b32-195">Az index nézetről módosítása</span><span class="sxs-lookup"><span data-stu-id="97b32-195">Modify the index view</span></span>
1. <span data-ttu-id="97b32-196">Nyissa meg a **tasklist/views/index.jade** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="97b32-196">Open the **tasklist/views/index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="97b32-197">Cserélje le a fájl teljes tartalmát a következő kódra.</span><span class="sxs-lookup"><span data-stu-id="97b32-197">Replace the entire contents of the file with the following code.</span></span> <span data-ttu-id="97b32-198">Ez határozza meg a nézet jeleníti meg a meglévő feladatokat, és az új feladatok hozzáadása és meglévőket megjelölés befejezettként űrlapot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="97b32-198">This defines a view that displays existing tasks and includes a form for adding new tasks and marking existing ones as completed.</span></span>
   
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
              if (typeof tasks === "undefined")
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
3. <span data-ttu-id="97b32-199">Mentse és zárja be **index.jade** fájlt.</span><span class="sxs-lookup"><span data-stu-id="97b32-199">Save and close **index.jade** file.</span></span>

### <a name="modify-the-global-layout"></a><span data-ttu-id="97b32-200">A globális elrendezés módosítása</span><span class="sxs-lookup"><span data-stu-id="97b32-200">Modify the global layout</span></span>
<span data-ttu-id="97b32-201">A **Views** fájlt a **nézetek** könyvtár más globális sablonjaként **.jade** fájlokat.</span><span class="sxs-lookup"><span data-stu-id="97b32-201">The **layout.jade** file in the **views** directory is a global template for other **.jade** files.</span></span> <span data-ttu-id="97b32-202">Ebben a lépésben módosítja úgy, hogy használjon [Twitter Bootstrap](https://github.com/twbs/bootstrap), ez az egy eszközkészlet, amellyel könnyedén töltött kinézetű webalkalmazás tervezéséhez.</span><span class="sxs-lookup"><span data-stu-id="97b32-202">In this step you will modify it to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking web app.</span></span>

<span data-ttu-id="97b32-203">Töltse le és csomagolja ki a fájlokat a [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="97b32-203">Download and extract the files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="97b32-204">Másolás a **bootstrap.min.css** a fájlt a rendszerindítási **css** mappából a **nyilvános/stíluslapok** az alkalmazás könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="97b32-204">Copy the **bootstrap.min.css** file from the Bootstrap **css** folder into the **public/stylesheets** directory of your application.</span></span>

<span data-ttu-id="97b32-205">Az a **nézetek** mappa, nyissa meg **Views** , és cserélje ki a teljes tartalmát a következőre:</span><span class="sxs-lookup"><span data-stu-id="97b32-205">From the **views** folder, open **layout.jade** and replace the entire contents with the following:</span></span>

    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body.app
        nav.navbar.navbar-default
          div.navbar-header
          a.navbar-brand(href='/') My Tasks
        block content

### <a name="create-a-config-file"></a><span data-ttu-id="97b32-206">Egy konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="97b32-206">Create a config file</span></span>
<span data-ttu-id="97b32-207">Az alkalmazás futtatásához helyileg, azt fogja üzembe Azure Storage hitelesítő adatokat a konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="97b32-207">To run the app locally, we'll put Azure Storage credentials into a config file.</span></span> <span data-ttu-id="97b32-208">Hozzon létre egy fájlt **config.json* * és a következő JSON:</span><span class="sxs-lookup"><span data-stu-id="97b32-208">Create a file named **config.json* *with the following JSON:</span></span>

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

<span data-ttu-id="97b32-209">Cserélje le **tárfióknév** nevű, a tárolási fiók korábban létrehozott, és cserélje le **tárelérési kulcs** a tárfiók elsődleges elérési kulcsát.</span><span class="sxs-lookup"><span data-stu-id="97b32-209">Replace **storage account name** with the name of the storage account you created earlier, and replace **storage access key** with the primary access key for your storage account.</span></span> <span data-ttu-id="97b32-210">Példa:</span><span class="sxs-lookup"><span data-stu-id="97b32-210">For example:</span></span>

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

<span data-ttu-id="97b32-211">Mentse a fájlt *egy könyvtár szintjén magasabb* mint a **tasklist** könyvtárban, ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="97b32-211">Save this file *one directory level higher* than the **tasklist** directory, like this:</span></span>

    parent/
      |-- config.json
      |-- tasklist/

<span data-ttu-id="97b32-212">Ennek az az oka, a konfigurációs fájl ellenőrzése forrás vezérlőbe elkerülése érdekében, ahol előfordulhat, hogy válik nyilvános.</span><span class="sxs-lookup"><span data-stu-id="97b32-212">The reason for doing this is to avoid checking the config file into source control, where it might become public.</span></span> <span data-ttu-id="97b32-213">Azt az alkalmazás telepítése az Azure-ba, amikor használjuk a környezeti változók egy konfigurációs fájl helyett.</span><span class="sxs-lookup"><span data-stu-id="97b32-213">When we deploy the app to Azure, we will use environment variables instead of a config file.</span></span>

## <a name="run-the-application-locally"></a><span data-ttu-id="97b32-214">Az alkalmazás helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="97b32-214">Run the application locally</span></span>
<span data-ttu-id="97b32-215">Az alkalmazás a helyi gépén, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="97b32-215">To test the application on your local machine, perform the following steps:</span></span>

1. <span data-ttu-id="97b32-216">A parancssorból lépjen a **tasklist** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="97b32-216">From the command-line, change directories to the **tasklist** directory.</span></span>
2. <span data-ttu-id="97b32-217">Az alábbi parancs segítségével indítsa el az alkalmazás helyi:</span><span class="sxs-lookup"><span data-stu-id="97b32-217">Use the following command to launch the application locally:</span></span>
   
        npm start
3. <span data-ttu-id="97b32-218">Nyisson meg egy webböngészőt, és navigáljon a http://127.0.0.1:3000.</span><span class="sxs-lookup"><span data-stu-id="97b32-218">Open a web browser and navigate to http://127.0.0.1:3000.</span></span>
   
    <span data-ttu-id="97b32-219">Az alábbi példához hasonló weblap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="97b32-219">A web page similar to the following example appears.</span></span>
   
    ![Egy weblap, egy üres tasklist megjelenítése][node-table-finished]
4. <span data-ttu-id="97b32-221">Hozzon létre egy új teendő, adjon meg egy nevet és a kategória, és kattintson a **elem hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="97b32-221">To create a new to-do item, enter a name and category and click **Add Item**.</span></span> 
5. <span data-ttu-id="97b32-222">Befejeződött, ellenőrizze, feladatok **Complete** kattintson **frissítési feladatok**.</span><span class="sxs-lookup"><span data-stu-id="97b32-222">To mark a task as complete, check **Complete** and click **Update Tasks**.</span></span>
   
    ![Az új elemet a listában a feladatok képe][node-table-list-items]

<span data-ttu-id="97b32-224">Annak ellenére, hogy az alkalmazás helyileg fut, akkor az adatok az Azure Table szolgáltatásban tárolja.</span><span class="sxs-lookup"><span data-stu-id="97b32-224">Even though the application is running locally, it is storing the data in the Azure Table service.</span></span>

## <a name="deploy-your-application-to-azure"></a><span data-ttu-id="97b32-225">Az Azure alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="97b32-225">Deploy your application to Azure</span></span>
<span data-ttu-id="97b32-226">A jelen szakaszban szereplő lépéseket az Azure parancssori eszközök segítségével hozzon létre egy új webalkalmazást az App Service-ben, és a Git segítségével telepítse központilag az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="97b32-226">The steps in this section use the Azure command-line tools to create a new web app in App Service, and then use Git to deploy your application.</span></span> <span data-ttu-id="97b32-227">A műveletek végrehajtásához Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="97b32-227">To perform these steps you must have an Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="97b32-228">Ezeket a lépéseket is segítségével hajtható végre a [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="97b32-228">These steps can also be performed by using the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="97b32-229">Lásd: [és üzembe egy Node.js-webalkalmazás az Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="97b32-229">See [Build and deploy a Node.js web app in Azure App Service].</span></span>
> 
> <span data-ttu-id="97b32-230">Ha ez az első webalkalmazás hozott létre, az Azure portál telepíti az alkalmazást kell használnia.</span><span class="sxs-lookup"><span data-stu-id="97b32-230">If this is the first web app you have created, you must use the Azure Portal to deploy this application.</span></span>
> 
> 

<span data-ttu-id="97b32-231">Első lépésként telepítse a [Azure CLI] írja be a következő parancsot a parancssorból:</span><span class="sxs-lookup"><span data-stu-id="97b32-231">To get started, install the [Azure CLI] by entering the following command from the command line:</span></span>

    npm install azure-cli -g

### <a name="import-publishing-settings"></a><span data-ttu-id="97b32-232">Közzétételi beállítások importálása</span><span class="sxs-lookup"><span data-stu-id="97b32-232">Import publishing settings</span></span>
<span data-ttu-id="97b32-233">Ebben a lépésben az előfizetés kapcsolatos információkat tartalmazó fájl tölti le.</span><span class="sxs-lookup"><span data-stu-id="97b32-233">In this step, you will download a file containing information about your subscription.</span></span>

1. <span data-ttu-id="97b32-234">Írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="97b32-234">Enter the following command:</span></span>
   
        azure login
   
    <span data-ttu-id="97b32-235">Ez a parancs elindítja egy böngészőt, és a letöltési lapra lép.</span><span class="sxs-lookup"><span data-stu-id="97b32-235">This command launches a browser and navigates to the download page.</span></span> <span data-ttu-id="97b32-236">Ha a rendszer kéri, jelentkezzen be az Azure-előfizetéséhez társított fiókot.</span><span class="sxs-lookup"><span data-stu-id="97b32-236">If prompted, log in with the account associated with your Azure subscription.</span></span>
   
    <!-- ![The download page][download-publishing-settings] -->
   
    <span data-ttu-id="97b32-237">A fájl letöltése automatikusan elindul; Ha nem, kattintson a hivatkozásra a lap saját kezűleg letölteni a fájlt elején.</span><span class="sxs-lookup"><span data-stu-id="97b32-237">The file download begins automatically; if it does not, you can click the link at the beginning of the page to manually download the file.</span></span> <span data-ttu-id="97b32-238">Mentse a fájlt, és jegyezze fel a fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="97b32-238">Save the file and note the file path.</span></span>
2. <span data-ttu-id="97b32-239">Adja meg a beállítások importálása a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="97b32-239">Enter the following command to import the settings:</span></span>
   
        azure account import <path-to-file>
   
    <span data-ttu-id="97b32-240">Az előző lépésben adja meg a letöltött közzétételi beállítások fájlját elérési útját és nevét.</span><span class="sxs-lookup"><span data-stu-id="97b32-240">Specify the path and file name of the publishing settings file you downloaded in the previous step.</span></span>
3. <span data-ttu-id="97b32-241">A beállítások importálása, miután törli a közzétételi beállítások fájlja.</span><span class="sxs-lookup"><span data-stu-id="97b32-241">After the settings are imported, delete the publish settings file.</span></span> <span data-ttu-id="97b32-242">Már nem szükséges, és az Azure-előfizetéssel kapcsolatos bizalmas információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="97b32-242">It is no longer needed, and contains sensitive information regarding your Azure subscription.</span></span>

### <a name="create-an-app-service-web-app"></a><span data-ttu-id="97b32-243">Az App Service webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="97b32-243">Create an App Service web app</span></span>
1. <span data-ttu-id="97b32-244">A parancssorból lépjen a **tasklist** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="97b32-244">From the command-line, change directories to the **tasklist** directory.</span></span>
2. <span data-ttu-id="97b32-245">A következő paranccsal létrehozhat egy új webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="97b32-245">Use the following command to create a new web app.</span></span>
   
        azure site create --git
   
    <span data-ttu-id="97b32-246">A webes alkalmazás neve és a hely bekéri.</span><span class="sxs-lookup"><span data-stu-id="97b32-246">You will be prompted for the web app name and location.</span></span> <span data-ttu-id="97b32-247">Adjon meg egy egyedi nevet, és válassza ki az ugyanazon a földrajzi helyen, a Azure Storage-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="97b32-247">Provide a unique name and select the same geographical location as your Azure Storage account.</span></span>
   
    <span data-ttu-id="97b32-248">A `--git` paraméter létrehoz egy Git-tárházat az Azure a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="97b32-248">The `--git` parameter creates a Git repository on Azure for this web app.</span></span> <span data-ttu-id="97b32-249">Inicializálja az aktuális könyvtárban található egy Git-tárház Ha egyik sem létezik, és hozzáad egy [Git távoli] nevű "azure", amely segítségével teheti közzé az alkalmazást az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="97b32-249">It also initializes a Git repository in the current directory if none exists, and adds a [Git remote] named 'azure', which is used to publish the application to Azure.</span></span> <span data-ttu-id="97b32-250">Végül létrehoz egy **web.config** fájl, amely tartalmazza a csomópont-alkalmazások üzemeltetését Azure által használt beállítások.</span><span class="sxs-lookup"><span data-stu-id="97b32-250">Finally, it creates a **web.config** file, which contains settings used by Azure to host node applications.</span></span> <span data-ttu-id="97b32-251">Ha nincs megadva a `--git` paraméter, de a könyvtár tartalmaz egy Git-tárházat, a parancs még mindig létrehoz az azure távoli.</span><span class="sxs-lookup"><span data-stu-id="97b32-251">If you omit the `--git` parameter but the directory contains a Git repository, the command will still create the 'azure' remote.</span></span>
   
    <span data-ttu-id="97b32-252">Ez a parancs befejezése után az alábbihoz hasonló kimenetet fog látni.</span><span class="sxs-lookup"><span data-stu-id="97b32-252">Once this command has completed, you will see output similar to the following.</span></span> <span data-ttu-id="97b32-253">Vegye figyelembe, hogy a sor kezdve **webhely létrejött. helye** a webes alkalmazás URL-CÍMÉT tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="97b32-253">Note that the line beginning with **Website created at** contains the URL for the web app.</span></span>
   
        info:   Executing command site create
        help:   Need a site name
        Name: TableTasklist
        info:   Using location southcentraluswebspace
        info:   Executing `git init`
        info:   Creating default .gitignore file
        info:   Creating a new web site
        info:   Created web site at  tabletasklist.azurewebsites.net
        info:   Initializing repository
        info:   Repository initialized
        info:   Executing `git remote add azure https://username@tabletasklist.azurewebsites.net/TableTasklist.git`
        info:   site create command OK
   
   > [!NOTE]
   > <span data-ttu-id="97b32-254">Ha ez az első App Service web app az előfizetéséhez, utasította a webalkalmazás létrehozása az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="97b32-254">If this is the first App Service web app for your subscription, you will be instructed to use the Azure Portal to create the web app.</span></span> <span data-ttu-id="97b32-255">További információkért lásd: [és üzembe egy Node.js-webalkalmazás az Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="97b32-255">For more information, see [Build and deploy a Node.js web app in Azure App Service].</span></span>
   > 
   > 

### <a name="set-environment-variables"></a><span data-ttu-id="97b32-256">Környezeti változók beállítása</span><span class="sxs-lookup"><span data-stu-id="97b32-256">Set environment variables</span></span>
<span data-ttu-id="97b32-257">Ebben a lépésben az Azure web app konfigurációjához felveszi a környezeti változók.</span><span class="sxs-lookup"><span data-stu-id="97b32-257">In this step, you will add environment variables to your web app configuration on Azure.</span></span>
<span data-ttu-id="97b32-258">A parancssorból írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="97b32-258">From the command line, enter the following:</span></span>

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


<span data-ttu-id="97b32-259">Cserélje le  **<storage account name>**  nevű, a tárolási fiók korábban létrehozott, és cserélje le  **<storage access key>**  a tárfiók elsődleges elérési kulcsát.</span><span class="sxs-lookup"><span data-stu-id="97b32-259">Replace **<storage account name>** with the name of the storage account you created earlier, and replace **<storage access key>** with the primary access key for your storage account.</span></span> <span data-ttu-id="97b32-260">(A megegyező értékeket használja a korábban létrehozott config.json fájlként.)</span><span class="sxs-lookup"><span data-stu-id="97b32-260">(Use the same values as the config.json file that you created earlier.)</span></span>

<span data-ttu-id="97b32-261">Másik lehetőségként a beállíthatja a környezeti változók a [Azure Portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="97b32-261">Alternatively, you can set environment variables in the [Azure Portal](https://portal.azure.com/):</span></span>

1. <span data-ttu-id="97b32-262">Nyissa meg a webalkalmazása panelén kattintva **Tallózás** > **webalkalmazások** > a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="97b32-262">Open the web app's blade by clicking **Browse** > **Web Apps** > your web app name.</span></span>
2. <span data-ttu-id="97b32-263">A webalkalmazás panelen kattintson **összes beállítás** > **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="97b32-263">In your web app's blade, click **All Settings** > **Application Settings**.</span></span>
   
     <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->
3. <span data-ttu-id="97b32-264">Görgessen le a **Alkalmazásbeállítások** szakaszt, és vegye fel a kulcs/érték párok.</span><span class="sxs-lookup"><span data-stu-id="97b32-264">Scroll down to the **App settings** section and add the key/value pairs.</span></span>
   
     ![Alkalmazásbeállítások](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)
4. <span data-ttu-id="97b32-266">Kattintson a **SAVE** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="97b32-266">Click **SAVE**.</span></span>

### <a name="publish-the-application"></a><span data-ttu-id="97b32-267">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="97b32-267">Publish the application</span></span>
<span data-ttu-id="97b32-268">Tegye közzé az alkalmazást, a kód fájlok véglegesítése Git, és majd leküldése azure/fő.</span><span class="sxs-lookup"><span data-stu-id="97b32-268">To publish the app, commit the code files to Git and then push to azure/master.</span></span>

1. <span data-ttu-id="97b32-269">Az üzembe helyezési hitelesítő adatok beállítása.</span><span class="sxs-lookup"><span data-stu-id="97b32-269">Set your deployment credentials.</span></span>
   
        azure site deployment user set <name> <password>
2. <span data-ttu-id="97b32-270">Vegye fel, és véglegesítse az alkalmazásfájlokat.</span><span class="sxs-lookup"><span data-stu-id="97b32-270">Add and commit your application files.</span></span>
   
        git add .
        git commit -m "adding files"
3. <span data-ttu-id="97b32-271">A véglegesítési leküldése az App Service web app:</span><span class="sxs-lookup"><span data-stu-id="97b32-271">Push the commit to the App Service web app:</span></span>
   
        git push azure master
   
    <span data-ttu-id="97b32-272">Használjon **fő** , a cél ágat.</span><span class="sxs-lookup"><span data-stu-id="97b32-272">Use **master** as the target branch.</span></span> <span data-ttu-id="97b32-273">A telepítés végén megjelenik egy utasítást a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="97b32-273">At the end of the deployment, you see a statement similar to the following example:</span></span>
   
        To https://username@tabletasklist.azurewebsites.net/TableTasklist.git
          * [new branch]      master -> master
4. <span data-ttu-id="97b32-274">A leküldéses befejezése után, keresse meg a webes alkalmazás URL-cím által korábban visszaadott a `azure create site` parancsot az alkalmazás megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="97b32-274">Once the push operation has completed, browse to the web app URL returned previously by the `azure create site` command to view your application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97b32-275">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97b32-275">Next steps</span></span>
<span data-ttu-id="97b32-276">Amíg ez a cikk lépéseit írják le, a Table szolgáltatás használatával az adatok tárolására, használhatja [MongoDB](https://mlab.com/azure/).</span><span class="sxs-lookup"><span data-stu-id="97b32-276">While the steps in this article describe using the Table Service to store information, you can also use [MongoDB](https://mlab.com/azure/).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="97b32-277">További források</span><span class="sxs-lookup"><span data-stu-id="97b32-277">Additional resources</span></span>
<span data-ttu-id="97b32-278">[Azure CLI]</span><span class="sxs-lookup"><span data-stu-id="97b32-278">[Azure CLI]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="97b32-279">A változások</span><span class="sxs-lookup"><span data-stu-id="97b32-279">What's changed</span></span>
* <span data-ttu-id="97b32-280">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="97b32-280">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- URLs -->

<span data-ttu-id="97b32-281">[és üzembe egy Node.js-webalkalmazás az Azure App Service]: app-service-web-get-started-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="97b32-281">[Build and deploy a Node.js web app in Azure App Service]: app-service-web-get-started-nodejs.md</span></span>
[Azure Developer Center]: /develop/nodejs/

<span data-ttu-id="97b32-282">[csomópont]: http://nodejs.org</span><span class="sxs-lookup"><span data-stu-id="97b32-282">[node]: http://nodejs.org</span></span>
<span data-ttu-id="97b32-283">[Git]: http://git-scm.com</span><span class="sxs-lookup"><span data-stu-id="97b32-283">[Git]: http://git-scm.com</span></span>
<span data-ttu-id="97b32-284">[Express]: http://expressjs.com</span><span class="sxs-lookup"><span data-stu-id="97b32-284">[Express]: http://expressjs.com</span></span>
[for free]: http://windowsazure.com
<span data-ttu-id="97b32-285">[Git távoli]: http://git-scm.com/docs/git-remote</span><span class="sxs-lookup"><span data-stu-id="97b32-285">[Git remote]: http://git-scm.com/docs/git-remote</span></span>

<span data-ttu-id="97b32-286">[Azure CLI]:../cli-install-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="97b32-286">[Azure CLI]:../cli-install-nodejs.md</span></span>

<span data-ttu-id="97b32-287">[azure]: https://github.com/Azure/azure-sdk-for-node</span><span class="sxs-lookup"><span data-stu-id="97b32-287">[azure]: https://github.com/Azure/azure-sdk-for-node</span></span>
<span data-ttu-id="97b32-288">[csomópont-uuid]: https://www.npmjs.com/package/node-uuid</span><span class="sxs-lookup"><span data-stu-id="97b32-288">[node-uuid]: https://www.npmjs.com/package/node-uuid</span></span>
<span data-ttu-id="97b32-289">[nconf]: https://www.npmjs.com/package/nconf</span><span class="sxs-lookup"><span data-stu-id="97b32-289">[nconf]: https://www.npmjs.com/package/nconf</span></span>
<span data-ttu-id="97b32-290">[aszinkron]: https://www.npmjs.com/package/async</span><span class="sxs-lookup"><span data-stu-id="97b32-290">[async]: https://www.npmjs.com/package/async</span></span>

[Azure Portal]: https://portal.azure.com

[Create and deploy a Node.js application to an Azure Web Site]: app-service-web-get-started-nodejs.md

<!-- Image References -->

[node-table-finished]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_empty.png
[node-table-list-items]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_list.png
[download-publishing-settings]: ./media/storage-nodejs-use-table-storage-web-site/azure-account-download-cli.png
[portal-storage-access-keys]: ./media/storage-nodejs-use-table-storage-web-site/manage-access-keys.png
[app-settings]: ./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png
