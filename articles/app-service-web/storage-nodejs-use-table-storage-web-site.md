---
title: "aaaNode.js web app használatával hello Azure Table szolgáltatás"
description: "Ez az oktatóanyag útmutatást ad meg hogyan toouse hello Azure Table szolgáltatás toostore lévő Azure App Service Web Apps Node.js-alkalmazás adatait."
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
ms.openlocfilehash: f6e08335b4c7f62f7b3994287edd586860cb7135
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-app-using-hello-azure-table-service"></a><span data-ttu-id="5a4a6-103">Hello Azure Table szolgáltatás használata node.js-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="5a4a6-103">Node.js web app using hello Azure Table Service</span></span>
## <a name="overview"></a><span data-ttu-id="5a4a6-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5a4a6-104">Overview</span></span>
<span data-ttu-id="5a4a6-105">Az oktatóanyag bemutatja, hogyan toouse Table szolgáltatás Azure adatkezelés toostore és hozzáférési adatok biztosítja a [csomópont] üzemeltetett alkalmazás [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-105">This tutorial shows you how toouse Table service provided by Azure Data Management toostore and access data from a [node] application hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="5a4a6-106">Ez az oktatóanyag feltételezi, hogy rendelkezik némi tapasztalattal a csomópont használatát és [Git].</span><span class="sxs-lookup"><span data-stu-id="5a4a6-106">This tutorial assumes that you have some prior experience using node and [Git].</span></span>

<span data-ttu-id="5a4a6-107">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-107">You will learn:</span></span>

* <span data-ttu-id="5a4a6-108">Hogyan toouse npm (csomópont Csomagkezelő) tooinstall hello csomópont modulok</span><span class="sxs-lookup"><span data-stu-id="5a4a6-108">How toouse npm (node package manager) tooinstall hello node modules</span></span>
* <span data-ttu-id="5a4a6-109">Hogyan toowork a hello Azure Table szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5a4a6-109">How toowork with hello Azure Table service</span></span>
* <span data-ttu-id="5a4a6-110">Hogyan toouse hello Azure CLI toocreate egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-110">How toouse hello Azure CLI toocreate a web app.</span></span>

<span data-ttu-id="5a4a6-111">Az oktatóanyag utasításait követve egy egyszerű, webalapú fog létrehozni, amely lehetővé teszi a létrehozása, beolvasása és feladatok végrehajtása "Feladatlista" alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-111">By following this tutorial, you will build a simple web-based "to-do list" application that allows creating, retrieving and completing tasks.</span></span> <span data-ttu-id="5a4a6-112">hello feladatok hello Table szolgáltatás vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-112">hello tasks are stored in hello Table service.</span></span>

<span data-ttu-id="5a4a6-113">Alkalmazás hello befejeződött a következő:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-113">Here is hello completed application:</span></span>

![Egy üres tasklist szöveget megjelenítő weblap][node-table-finished]

> [!NOTE]
> <span data-ttu-id="5a4a6-115">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-115">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="5a4a6-116">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-116">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="5a4a6-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5a4a6-117">Prerequisites</span></span>
<span data-ttu-id="5a4a6-118">Hello jelen cikkben lévő utasítások követése, előtt győződjön meg arról, hogy hello következőkkel:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-118">Before following hello instructions in this article, ensure that you have hello following installed:</span></span>

* <span data-ttu-id="5a4a6-119">[csomópont] 0.10.24 verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="5a4a6-119">[node] version 0.10.24 or higher</span></span>
* <span data-ttu-id="5a4a6-120">[Git]</span><span class="sxs-lookup"><span data-stu-id="5a4a6-120">[Git]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a><span data-ttu-id="5a4a6-121">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="5a4a6-121">Create a storage account</span></span>
<span data-ttu-id="5a4a6-122">Hozzon létre egy Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-122">Create an Azure storage account.</span></span> <span data-ttu-id="5a4a6-123">hello alkalmazás fogja használni a fiókot toostore hello Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-123">hello app will use this account toostore hello to-do items.</span></span>

1. <span data-ttu-id="5a4a6-124">Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5a4a6-124">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="5a4a6-125">Kattintson a hello **új** hello portál bal hello alsó ikonra, majd kattintson a **adatok + tárolás** > **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-125">Click hello **New** icon on hello bottom left of hello portal, then click **Data + Storage** > **Storage**.</span></span> <span data-ttu-id="5a4a6-126">Hello tárfiók adjon egy egyedi nevet, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-126">Give hello storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Új gomb](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)
   
    <span data-ttu-id="5a4a6-128">Hello tárfiók létrehozásakor hello **értesítések** gomb villogjon, egy zöld **sikeres** és hello tárolási fiók panelje meg nyitva, hogy tartozik-e új erőforrás toohello tooshow csoportjának, létre.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-128">When hello storage account has been created, hello **Notifications** button will flash a green **SUCCESS** and hello storage account's blade is open tooshow that it belongs toohello new resource group you created.</span></span>
3. <span data-ttu-id="5a4a6-129">Hello tárolási fiók paneljén kattintson **beállítások** > **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-129">In hello storage account's blade, click **Settings** > **Keys**.</span></span> <span data-ttu-id="5a4a6-130">Hello elsődleges elérési kulcs toohello vágólapra másolása.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-130">Copy hello primary access key toohello clipboard.</span></span>
   
    ![A hozzáférési kulcsot][portal-storage-access-keys]

## <a name="install-modules-and-generate-scaffolding"></a><span data-ttu-id="5a4a6-132">Modulok telepítése és állványok készítése</span><span class="sxs-lookup"><span data-stu-id="5a4a6-132">Install modules and generate scaffolding</span></span>
<span data-ttu-id="5a4a6-133">Ez a szakasz hozzon létre egy új csomópont alkalmazást, és npm tooadd modul csomagot.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-133">In this section you will create a new Node application and use npm tooadd module packages.</span></span> <span data-ttu-id="5a4a6-134">Ehhez az alkalmazáshoz használandó hello [Express] és [Azure] modulok.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-134">For this application you will use hello [Express] and [Azure] modules.</span></span> <span data-ttu-id="5a4a6-135">hello Express modul modell nézetvezérlő keretet biztosít a csomópont, miközben hello Azure modulok kapcsolat toohello tábla szolgáltatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-135">hello Express module provides a Model View Controller framework for node, while hello Azure modules provides connectivity toohello Table service.</span></span>

### <a name="install-express-and-generate-scaffolding"></a><span data-ttu-id="5a4a6-136">Express telepítése és állványok készítése</span><span class="sxs-lookup"><span data-stu-id="5a4a6-136">Install express and generate scaffolding</span></span>
1. <span data-ttu-id="5a4a6-137">Hello parancssorból hozzon létre egy új könyvtárat, nevű **tasklist** és kapcsoló toothat könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-137">From hello command line, create a new directory named **tasklist** and switch toothat directory.</span></span>  
2. <span data-ttu-id="5a4a6-138">Adja meg a következő parancs tooinstall hello Express modul hello.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-138">Enter hello following command tooinstall hello Express module.</span></span>
   
        npm install express-generator@4.2.0 -g
   
    <span data-ttu-id="5a4a6-139">Attól függően, hogy hello operációs rendszer szükség lehet a tooput 'sudo' parancs hello előtt:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-139">Depending on hello operating system, you may need tooput 'sudo' before hello command:</span></span>
   
        sudo npm install express-generator@4.2.0 -g
   
    <span data-ttu-id="5a4a6-140">hello eredmény jelenik meg a következő példa hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-140">hello output appears similar toohello following example:</span></span>
   
        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)
   
   > [!NOTE]
   > <span data-ttu-id="5a4a6-141">hello "-g" paraméter hello modul globálisan telepíti.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-141">hello '-g' parameter installs hello module globally.</span></span> <span data-ttu-id="5a4a6-142">Ily módon használhatjuk **expressz** toogenerate web app állványok anélkül, hogy tootype további információt.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-142">That way, we can use **express** toogenerate web app scaffolding without having tootype in additional path information.</span></span>
   > 
   > 
3. <span data-ttu-id="5a4a6-143">toocreate hello állványok hello alkalmazáshoz, adja meg a hello **expressz** parancs:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-143">toocreate hello scaffolding for hello application, enter hello **express** command:</span></span>
   
        express
   
    <span data-ttu-id="5a4a6-144">a parancs kimenetének hello jelenik meg a következő példa hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-144">hello output of this command appears similar toohello following example:</span></span>
   
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
   
           run hello app:
             $ DEBUG=my-application ./bin/www
   
    <span data-ttu-id="5a4a6-145">Most már rendelkezik néhány új könyvtárak és fájlok hello **tasklist** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-145">You now have several new directories and files in hello **tasklist** directory.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="5a4a6-146">A kiegészítő modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="5a4a6-146">Install additional modules</span></span>
<span data-ttu-id="5a4a6-147">Hello egyik fájlok **expressz** hoz létre az **package.json**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-147">One of hello files that **express** creates is **package.json**.</span></span> <span data-ttu-id="5a4a6-148">Ez a fájl modul függőségek listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-148">This file contains a list of module dependencies.</span></span> <span data-ttu-id="5a4a6-149">Később hello alkalmazás tooApp Service Web Apps telepítésekor ez a fájl határozza meg, melyik modulokat kell az Azure telepített toobe.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-149">Later, when you deploy hello application tooApp Service Web Apps, this file determines which modules need toobe installed on Azure.</span></span>

<span data-ttu-id="5a4a6-150">Hello parancssori, adja meg a következő parancs tooinstall hello modulok hello ismertetett hello **package.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-150">From hello command-line, enter hello following command tooinstall hello modules described in hello **package.json** file.</span></span> <span data-ttu-id="5a4a6-151">Szükség lehet toouse 'sudo'.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-151">You may need toouse 'sudo'.</span></span>

    npm install

<span data-ttu-id="5a4a6-152">a parancs kimenetének hello jelenik meg a következő példa hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-152">hello output of this command appears similar toohello following example:</span></span>

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


<span data-ttu-id="5a4a6-153">Ezután adja meg a következő parancs tooinstall hello hello [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-153">Next, enter hello following command tooinstall hello [azure], [node-uuid], [nconf] and [async] modules:</span></span>

    npm install azure-storage node-uuid async nconf --save

<span data-ttu-id="5a4a6-154">Hello **--mentése** jelző hozzáadja a modulok toohello bejegyzéseket **package.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-154">hello **--save** flag adds entries for these modules toohello **package.json** file.</span></span>

<span data-ttu-id="5a4a6-155">a parancs kimenetének hello jelenik meg a következő példa hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-155">hello output of this command appears similar toohello following example:</span></span>

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-hello-application"></a><span data-ttu-id="5a4a6-156">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-156">Create hello application</span></span>
<span data-ttu-id="5a4a6-157">Most már készen áll a toobuild hello alkalmazást a rendszer.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-157">Now we're ready toobuild hello application.</span></span>

### <a name="create-a-model"></a><span data-ttu-id="5a4a6-158">Modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-158">Create a model</span></span>
<span data-ttu-id="5a4a6-159">A *modell* hello adatait az alkalmazás képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-159">A *model* is an object that represents hello data in your application.</span></span> <span data-ttu-id="5a4a6-160">Hello alkalmazás hello csak modell egy feladat objektum, amely hello feladatlista elem..</span><span class="sxs-lookup"><span data-stu-id="5a4a6-160">For hello application, hello only model is a task object, which represents an item in hello to-do list.</span></span> <span data-ttu-id="5a4a6-161">A következő mezők hello feladatnál:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-161">Tasks will have hello following fields:</span></span>

* <span data-ttu-id="5a4a6-162">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="5a4a6-162">PartitionKey</span></span>
* <span data-ttu-id="5a4a6-163">RowKey</span><span class="sxs-lookup"><span data-stu-id="5a4a6-163">RowKey</span></span>
* <span data-ttu-id="5a4a6-164">Name (karakterlánc)</span><span class="sxs-lookup"><span data-stu-id="5a4a6-164">name (string)</span></span>
* <span data-ttu-id="5a4a6-165">kategória (karakterlánc)</span><span class="sxs-lookup"><span data-stu-id="5a4a6-165">category (string)</span></span>
* <span data-ttu-id="5a4a6-166">Befejezett (logikai)</span><span class="sxs-lookup"><span data-stu-id="5a4a6-166">completed (Boolean)</span></span>

<span data-ttu-id="5a4a6-167">**PartitionKey** és **RowKey** tábla kulcsként hello Table szolgáltatás által használt.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-167">**PartitionKey** and **RowKey** are used by hello Table Service as table keys.</span></span> <span data-ttu-id="5a4a6-168">További információkért lásd: [ismertetése hello Table szolgáltatás adatmodell](https://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="5a4a6-168">For more information, see [Understanding hello Table Service data model](https://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

1. <span data-ttu-id="5a4a6-169">A hello **tasklist** könyvtár, hozzon létre egy új könyvtárat nevű **modellek**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-169">In hello **tasklist** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="5a4a6-170">A hello **modellek** könyvtár, hozzon létre egy új fájlt **task.js**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-170">In hello **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="5a4a6-171">Ezt a fájlt fogja tartalmazni az alkalmazás által létrehozott hello feladatok hello modelljét.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-171">This file will contain hello model for hello tasks created by your application.</span></span>
3. <span data-ttu-id="5a4a6-172">Hello hello elején **task.js** fájlt, adja hozzá a következő kód szükséges tooreference szalagtárak hello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-172">At hello beginning of hello **task.js** file, add hello following code tooreference required libraries:</span></span>
   
        var azure = require('azure-storage');
          var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;
4. <span data-ttu-id="5a4a6-173">Adja hozzá a hello alábbi code toodefine, és hello feladat objektum exportálása.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-173">Add hello following code toodefine and export hello Task object.</span></span> <span data-ttu-id="5a4a6-174">Ez az objektum toohello tábla csatlakozás felelős.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-174">This object is responsible for connecting toohello table.</span></span>
   
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
5. <span data-ttu-id="5a4a6-175">Adja hozzá hello kód toodefine további módszereket követően hello feladatobjektum, amelyek lehetővé teszik az hello táblában tárolt adatok interakció:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-175">Add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in hello table:</span></span>
   
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
6. <span data-ttu-id="5a4a6-176">Mentse és zárja be a hello **task.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-176">Save and close hello **task.js** file.</span></span>

### <a name="create-a-controller"></a><span data-ttu-id="5a4a6-177">A tartományvezérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-177">Create a controller</span></span>
<span data-ttu-id="5a4a6-178">A *vezérlő* HTTP-kérelmeket kezeli, és ez a beállítás hello HTML-válasz.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-178">A *controller* handles HTTP requests and renders hello HTML response.</span></span>

1. <span data-ttu-id="5a4a6-179">A hello **tasklist/útvonalak** könyvtár, hozzon létre egy új fájlt **tasklist.js** , majd nyissa meg szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-179">In hello **tasklist/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="5a4a6-180">Adja hozzá a következő kód túl hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-180">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="5a4a6-181">Ez betölti a hello azure és async modult által használt **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-181">This loads hello azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="5a4a6-182">Ez meghatározza hello **TaskList** függvény, amelyet hello példányának **feladat** objektum korábban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-182">This also defines hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>
   
        var azure = require('azure-storage');
        var async = require('async');
   
        module.exports = TaskList;
3. <span data-ttu-id="5a4a6-183">Adja meg a **TaskList** objektum.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-183">Define a **TaskList** object.</span></span>
   
        function TaskList(task) {
          this.task = task;
        }
4. <span data-ttu-id="5a4a6-184">Adja hozzá a következő módszerek túl hello**TaskList**:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-184">Add hello following methods too**TaskList**:</span></span>
   
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

### <a name="modify-appjs"></a><span data-ttu-id="5a4a6-185">Az app.js fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-185">Modify app.js</span></span>
1. <span data-ttu-id="5a4a6-186">A hello **tasklist** könyvtárába, nyissa meg hello **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-186">From hello **tasklist** directory, open hello **app.js** file.</span></span> <span data-ttu-id="5a4a6-187">Ez a fájl készült korábbi hello **expressz** parancsot.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-187">This file was created earlier by running hello **express** command.</span></span>
2. <span data-ttu-id="5a4a6-188">A hello fájl hello elején tooload hello azure modul, a hello tábla neve, a partíciós kulcs és a set hello tárolási által használt hitelesítő adatok ebben a példában a következő hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-188">At hello beginning of hello file, add hello following tooload hello azure module, set hello table name, partition key, and set hello storage credentials used by this example:</span></span>
   
        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");
   
   > [!NOTE]
   > <span data-ttu-id="5a4a6-189">nconf hello konfigurációs értékeket fogja betölteni a környezeti változók vagy hello **config.json** fájlt, amely később hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-189">nconf will load hello configuration values from either environment variables or hello **config.json** file, which we will create later.</span></span>
   > 
   > 
3. <span data-ttu-id="5a4a6-190">Hello app.js fájlban, görgessen lefelé látja toowhere hello a következő sort:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-190">In hello app.js file, scroll down toowhere you see hello following line:</span></span>
   
        app.use('/', routes);
        app.use('/users', users);
   
    <span data-ttu-id="5a4a6-191">Cserélje le hello sorok fent lent látható módon hello kódra.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-191">Replace hello above lines with hello code shown below.</span></span> <span data-ttu-id="5a4a6-192">Ez egy példányát inicializálja <strong>feladat</strong> kapcsolat tooyour storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-192">This will initialize an instance of <strong>Task</strong> with a connection tooyour storage account.</span></span> <span data-ttu-id="5a4a6-193">Ez átadása toohello <strong>TaskList</strong>, amely fog használni az toocommunicate hello Table szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-193">This is passed toohello <strong>TaskList</strong>, which will use it toocommunicate with hello Table service:</span></span>
   
        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
4. <span data-ttu-id="5a4a6-194">Mentse a hello **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-194">Save hello **app.js** file.</span></span>

### <a name="modify-hello-index-view"></a><span data-ttu-id="5a4a6-195">Hello index nézet módosítása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-195">Modify hello index view</span></span>
1. <span data-ttu-id="5a4a6-196">Nyissa meg hello **tasklist/views/index.jade** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-196">Open hello **tasklist/views/index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="5a4a6-197">Cserélje le a következő kód hello hello hello fájl teljes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-197">Replace hello entire contents of hello file with hello following code.</span></span> <span data-ttu-id="5a4a6-198">Ez határozza meg a nézet jeleníti meg a meglévő feladatokat, és az új feladatok hozzáadása és meglévőket megjelölés befejezettként űrlapot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-198">This defines a view that displays existing tasks and includes a form for adding new tasks and marking existing ones as completed.</span></span>
   
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
3. <span data-ttu-id="5a4a6-199">Mentse és zárja be **index.jade** fájlt.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-199">Save and close **index.jade** file.</span></span>

### <a name="modify-hello-global-layout"></a><span data-ttu-id="5a4a6-200">Hello globális elrendezés módosítása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-200">Modify hello global layout</span></span>
<span data-ttu-id="5a4a6-201">Hello **Views** hello fájlban **nézetek** könyvtár más globális sablonjaként **.jade** fájlokat.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-201">hello **layout.jade** file in hello **views** directory is a global template for other **.jade** files.</span></span> <span data-ttu-id="5a4a6-202">Ebben a lépésben lesz a módosítás toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), amely, így könnyen toodesign töltött kinézetű webalkalmazás eszközkészletre.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-202">In this step you will modify it toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking web app.</span></span>

<span data-ttu-id="5a4a6-203">A hello fájlok letöltéséhez és kibontásához [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="5a4a6-203">Download and extract hello files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="5a4a6-204">Másolás hello **bootstrap.min.css** hello rendszerindítási fájlt **css** hello mappából **nyilvános/stíluslapok** az alkalmazás könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-204">Copy hello **bootstrap.min.css** file from hello Bootstrap **css** folder into hello **public/stylesheets** directory of your application.</span></span>

<span data-ttu-id="5a4a6-205">A hello **nézetek** mappa, nyissa meg **Views** , és cserélje le a hello teljes tartalma hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-205">From hello **views** folder, open **layout.jade** and replace hello entire contents with hello following:</span></span>

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

### <a name="create-a-config-file"></a><span data-ttu-id="5a4a6-206">Egy konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-206">Create a config file</span></span>
<span data-ttu-id="5a4a6-207">hello toorun alkalmazást helyileg, azt fogja üzembe Azure Storage hitelesítő adatokat a konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-207">toorun hello app locally, we'll put Azure Storage credentials into a config file.</span></span> <span data-ttu-id="5a4a6-208">Hozzon létre egy fájlt **config.json* * a következő JSON hello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-208">Create a file named **config.json* *with hello following JSON:</span></span>

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

<span data-ttu-id="5a4a6-209">Cserélje le **tárfióknév** hello tárolási hello nevű fiókra, korábban létrehozott, és cserélje le **tárelérési kulcs** a hello elsődleges hozzáférési kulcsot a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-209">Replace **storage account name** with hello name of hello storage account you created earlier, and replace **storage access key** with hello primary access key for your storage account.</span></span> <span data-ttu-id="5a4a6-210">Példa:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-210">For example:</span></span>

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

<span data-ttu-id="5a4a6-211">Mentse a fájlt *egy könyvtár szintjén magasabb* hello mint **tasklist** könyvtárban, ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-211">Save this file *one directory level higher* than hello **tasklist** directory, like this:</span></span>

    parent/
      |-- config.json
      |-- tasklist/

<span data-ttu-id="5a4a6-212">hello ennek oka, hogy tooavoid hello konfigurációs fájl ellenőrzése a verziókövetés, ahol azt válnak nyilvános.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-212">hello reason for doing this is tooavoid checking hello config file into source control, where it might become public.</span></span> <span data-ttu-id="5a4a6-213">Hello app tooAzure telepítésekor használjuk a környezeti változók egy konfigurációs fájl helyett.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-213">When we deploy hello app tooAzure, we will use environment variables instead of a config file.</span></span>

## <a name="run-hello-application-locally"></a><span data-ttu-id="5a4a6-214">Hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-214">Run hello application locally</span></span>
<span data-ttu-id="5a4a6-215">tootest hello alkalmazás a helyi gépén, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-215">tootest hello application on your local machine, perform hello following steps:</span></span>

1. <span data-ttu-id="5a4a6-216">A parancssori hello, módosítsa a könyvtárakat toohello **tasklist** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-216">From hello command-line, change directories toohello **tasklist** directory.</span></span>
2. <span data-ttu-id="5a4a6-217">A következő parancs toolaunch hello alkalmazás helyi hello használata:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-217">Use hello following command toolaunch hello application locally:</span></span>
   
        npm start
3. <span data-ttu-id="5a4a6-218">Nyisson meg egy webböngészőt, és keresse meg a toohttp://127.0.0.1:3000.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-218">Open a web browser and navigate toohttp://127.0.0.1:3000.</span></span>
   
    <span data-ttu-id="5a4a6-219">A következő példa egy weblap hasonló toohello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-219">A web page similar toohello following example appears.</span></span>
   
    ![Egy weblap, egy üres tasklist megjelenítése][node-table-finished]
4. <span data-ttu-id="5a4a6-221">egy új teendő toocreate adjon meg egy nevet és a kategória, és kattintson a **elem hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-221">toocreate a new to-do item, enter a name and category and click **Add Item**.</span></span> 
5. <span data-ttu-id="5a4a6-222">toomark feladat befejeződött, ellenőrizze, **Complete** kattintson **frissítési feladatok**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-222">toomark a task as complete, check **Complete** and click **Update Tasks**.</span></span>
   
    ![Hello új hello lista elemeire feladatok képe][node-table-list-items]

<span data-ttu-id="5a4a6-224">Annak ellenére, hogy hello alkalmazás helyben fut, akkor van hello Azure Table szolgáltatás hello adatok tárolását.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-224">Even though hello application is running locally, it is storing hello data in hello Azure Table service.</span></span>

## <a name="deploy-your-application-tooazure"></a><span data-ttu-id="5a4a6-225">Az alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="5a4a6-225">Deploy your application tooAzure</span></span>
<span data-ttu-id="5a4a6-226">Ebben a szakaszban található lépéseket hello használja hello Azure parancssori eszközök toocreate egy új webalkalmazást az App Service-ben, és a Git toodeploy az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-226">hello steps in this section use hello Azure command-line tools toocreate a new web app in App Service, and then use Git toodeploy your application.</span></span> <span data-ttu-id="5a4a6-227">tooperform ezeket a lépéseket az Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-227">tooperform these steps you must have an Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="5a4a6-228">Ezeket a lépéseket is hello segítségével hajtható végre [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5a4a6-228">These steps can also be performed by using hello [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="5a4a6-229">Lásd: [és üzembe egy Node.js-webalkalmazás az Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="5a4a6-229">See [Build and deploy a Node.js web app in Azure App Service].</span></span>
> 
> <span data-ttu-id="5a4a6-230">Ha ez hello első webalkalmazás hozott létre, az alkalmazás hello Azure Portal toodeploy kell használnia.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-230">If this is hello first web app you have created, you must use hello Azure Portal toodeploy this application.</span></span>
> 
> 

<span data-ttu-id="5a4a6-231">tooget indult el, telepítse a hello [Azure CLI] hello parancs hello parancssorból a következő beírásával:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-231">tooget started, install hello [Azure CLI] by entering hello following command from hello command line:</span></span>

    npm install azure-cli -g

### <a name="import-publishing-settings"></a><span data-ttu-id="5a4a6-232">Közzétételi beállítások importálása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-232">Import publishing settings</span></span>
<span data-ttu-id="5a4a6-233">Ebben a lépésben az előfizetés kapcsolatos információkat tartalmazó fájl tölti le.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-233">In this step, you will download a file containing information about your subscription.</span></span>

1. <span data-ttu-id="5a4a6-234">Adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-234">Enter hello following command:</span></span>
   
        azure login
   
    <span data-ttu-id="5a4a6-235">Ez a parancs elindítja egy böngészőt, és toohello letöltési lapra lép.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-235">This command launches a browser and navigates toohello download page.</span></span> <span data-ttu-id="5a4a6-236">Ha a rendszer kéri, jelentkezzen be az Azure-előfizetéshez társított hello fiókkal.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-236">If prompted, log in with hello account associated with your Azure subscription.</span></span>
   
    <!-- ![hello download page][download-publishing-settings] -->
   
    <span data-ttu-id="5a4a6-237">hello fájl letöltése automatikusan elindul; Ha nem, kattintson a hello hivatkozás hello toomanually letöltési hello lapozófájl hello elején.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-237">hello file download begins automatically; if it does not, you can click hello link at hello beginning of hello page toomanually download hello file.</span></span> <span data-ttu-id="5a4a6-238">Mentse a hello fájl- és megjegyzés hello fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-238">Save hello file and note hello file path.</span></span>
2. <span data-ttu-id="5a4a6-239">Adja meg a következő parancs tooimport hello beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-239">Enter hello following command tooimport hello settings:</span></span>
   
        azure account import <path-to-file>
   
    <span data-ttu-id="5a4a6-240">Adja meg közzététele beállításfájl letöltött hello előző lépésben hello hello elérési útját és nevét.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-240">Specify hello path and file name of hello publishing settings file you downloaded in hello previous step.</span></span>
3. <span data-ttu-id="5a4a6-241">Után hello tartoznak, törölje a hello közzététele beállításfájl.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-241">After hello settings are imported, delete hello publish settings file.</span></span> <span data-ttu-id="5a4a6-242">Már nem szükséges, és az Azure-előfizetéssel kapcsolatos bizalmas információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-242">It is no longer needed, and contains sensitive information regarding your Azure subscription.</span></span>

### <a name="create-an-app-service-web-app"></a><span data-ttu-id="5a4a6-243">Az App Service webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-243">Create an App Service web app</span></span>
1. <span data-ttu-id="5a4a6-244">A parancssori hello, módosítsa a könyvtárakat toohello **tasklist** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-244">From hello command-line, change directories toohello **tasklist** directory.</span></span>
2. <span data-ttu-id="5a4a6-245">A következő parancs toocreate egy új webalkalmazást hello használata.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-245">Use hello following command toocreate a new web app.</span></span>
   
        azure site create --git
   
    <span data-ttu-id="5a4a6-246">Hello webalkalmazás neve és helye bekéri.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-246">You will be prompted for hello web app name and location.</span></span> <span data-ttu-id="5a4a6-247">Adjon meg egy egyedi nevet, és válassza ki, a Azure Storage-fiókkal azonos földrajzi helyen hello.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-247">Provide a unique name and select hello same geographical location as your Azure Storage account.</span></span>
   
    <span data-ttu-id="5a4a6-248">Hello `--git` paraméter létrehoz egy Git-tárházat az Azure a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-248">hello `--git` parameter creates a Git repository on Azure for this web app.</span></span> <span data-ttu-id="5a4a6-249">Inicializálja a Git-tárház hello aktuális könyvtárban található Ha egyik sem létezik, és hozzáad egy [Git távoli] nevű "azure", amely használt toopublish hello alkalmazás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-249">It also initializes a Git repository in hello current directory if none exists, and adds a [Git remote] named 'azure', which is used toopublish hello application tooAzure.</span></span> <span data-ttu-id="5a4a6-250">Végül létrehoz egy **web.config** fájl, amely tartalmazza az Azure toohost csomópont alkalmazások által használt beállítások.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-250">Finally, it creates a **web.config** file, which contains settings used by Azure toohost node applications.</span></span> <span data-ttu-id="5a4a6-251">Hello kihagyásakor `--git` paraméter, de hello könyvtár tartalmaz egy Git-tárházat, hello parancs továbbra is létrehoz az "azure" hello távoli.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-251">If you omit hello `--git` parameter but hello directory contains a Git repository, hello command will still create hello 'azure' remote.</span></span>
   
    <span data-ttu-id="5a4a6-252">Ez a parancs befejezése után kimeneti hasonló toohello következő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-252">Once this command has completed, you will see output similar toohello following.</span></span> <span data-ttu-id="5a4a6-253">Vegye figyelembe, hogy hello kezdődő **webhely létrejött. helye** hello webalkalmazás hello URL-címet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-253">Note that hello line beginning with **Website created at** contains hello URL for hello web app.</span></span>
   
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
   > <span data-ttu-id="5a4a6-254">Ha ez hello első App Service web app az előfizetéséhez, utasításai toouse hello Azure Portal toocreate hello webalkalmazás fogja.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-254">If this is hello first App Service web app for your subscription, you will be instructed toouse hello Azure Portal toocreate hello web app.</span></span> <span data-ttu-id="5a4a6-255">További információkért lásd: [és üzembe egy Node.js-webalkalmazás az Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="5a4a6-255">For more information, see [Build and deploy a Node.js web app in Azure App Service].</span></span>
   > 
   > 

### <a name="set-environment-variables"></a><span data-ttu-id="5a4a6-256">Környezeti változók beállítása</span><span class="sxs-lookup"><span data-stu-id="5a4a6-256">Set environment variables</span></span>
<span data-ttu-id="5a4a6-257">Ebben a lépésben adhat az Azure-on környezeti változók tooyour webes alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-257">In this step, you will add environment variables tooyour web app configuration on Azure.</span></span>
<span data-ttu-id="5a4a6-258">Hello parancssorból írja be a hello következő:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-258">From hello command line, enter hello following:</span></span>

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


<span data-ttu-id="5a4a6-259">Cserélje le  **<storage account name>**  hello tárolási hello nevű fiókra, korábban létrehozott, és cserélje le  **<storage access key>**  a hello elsődleges hozzáférési kulcsot a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-259">Replace **<storage account name>** with hello name of hello storage account you created earlier, and replace **<storage access key>** with hello primary access key for your storage account.</span></span> <span data-ttu-id="5a4a6-260">(Azonos értékek hello használata korábban létrehozott hello config.json fájl.)</span><span class="sxs-lookup"><span data-stu-id="5a4a6-260">(Use hello same values as hello config.json file that you created earlier.)</span></span>

<span data-ttu-id="5a4a6-261">Másik lehetőségként beállíthatja hello környezeti változók [Azure Portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="5a4a6-261">Alternatively, you can set environment variables in hello [Azure Portal](https://portal.azure.com/):</span></span>

1. <span data-ttu-id="5a4a6-262">Hello webalkalmazása panelén megnyitásához **Tallózás** > **webalkalmazások** > a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-262">Open hello web app's blade by clicking **Browse** > **Web Apps** > your web app name.</span></span>
2. <span data-ttu-id="5a4a6-263">A webalkalmazás panelen kattintson **összes beállítás** > **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-263">In your web app's blade, click **All Settings** > **Application Settings**.</span></span>
   
     <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->
3. <span data-ttu-id="5a4a6-264">Görgessen lefelé toohello **Alkalmazásbeállítások** szakaszt, és adja hozzá a hello kulcs/érték párok.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-264">Scroll down toohello **App settings** section and add hello key/value pairs.</span></span>
   
     ![Alkalmazásbeállítások](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)
4. <span data-ttu-id="5a4a6-266">Kattintson a **SAVE** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-266">Click **SAVE**.</span></span>

### <a name="publish-hello-application"></a><span data-ttu-id="5a4a6-267">Hello alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="5a4a6-267">Publish hello application</span></span>
<span data-ttu-id="5a4a6-268">toopublish hello alkalmazást, és véglegesítse hello kód fájlok tooGit, és majd leküldéses tooazure/fő.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-268">toopublish hello app, commit hello code files tooGit and then push tooazure/master.</span></span>

1. <span data-ttu-id="5a4a6-269">Az üzembe helyezési hitelesítő adatok beállítása.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-269">Set your deployment credentials.</span></span>
   
        azure site deployment user set <name> <password>
2. <span data-ttu-id="5a4a6-270">Vegye fel, és véglegesítse az alkalmazásfájlokat.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-270">Add and commit your application files.</span></span>
   
        git add .
        git commit -m "adding files"
3. <span data-ttu-id="5a4a6-271">Leküldéses hello véglegesítési toohello App Service webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-271">Push hello commit toohello App Service web app:</span></span>
   
        git push azure master
   
    <span data-ttu-id="5a4a6-272">Használjon **fő** hello cél fiókirodai szerint.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-272">Use **master** as hello target branch.</span></span> <span data-ttu-id="5a4a6-273">Hello telepítési hello végén tekintse meg a következő példa egy utasítás hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="5a4a6-273">At hello end of hello deployment, you see a statement similar toohello following example:</span></span>
   
        toohttps://username@tabletasklist.azurewebsites.net/TableTasklist.git
          * [new branch]      master -> master
4. <span data-ttu-id="5a4a6-274">Hello leküldéses befejezése után, keresse meg a toohello webes URL-címet hello által korábban visszaadott `azure create site` tooview parancsot az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5a4a6-274">Once hello push operation has completed, browse toohello web app URL returned previously by hello `azure create site` command tooview your application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a4a6-275">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a4a6-275">Next steps</span></span>
<span data-ttu-id="5a4a6-276">Ebben a cikkben hello lépések írják le, hello Table szolgáltatás toostore információk alapján, amíg használhatja [MongoDB](https://mlab.com/azure/).</span><span class="sxs-lookup"><span data-stu-id="5a4a6-276">While hello steps in this article describe using hello Table Service toostore information, you can also use [MongoDB](https://mlab.com/azure/).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5a4a6-277">További források</span><span class="sxs-lookup"><span data-stu-id="5a4a6-277">Additional resources</span></span>
<span data-ttu-id="5a4a6-278">[Azure CLI]</span><span class="sxs-lookup"><span data-stu-id="5a4a6-278">[Azure CLI]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="5a4a6-279">A változások</span><span class="sxs-lookup"><span data-stu-id="5a4a6-279">What's changed</span></span>
* <span data-ttu-id="5a4a6-280">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="5a4a6-280">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- URLs -->

[és üzembe egy Node.js-webalkalmazás az Azure App Service]: app-service-web-get-started-nodejs.md
[Azure Developer Center]: /develop/nodejs/

[csomópont]: http://nodejs.org
[Git]: http://git-scm.com
[Express]: http://expressjs.com
[for free]: http://windowsazure.com
[Git távoli]: http://git-scm.com/docs/git-remote

[Azure CLI]:../cli-install-nodejs.md

[azure]: https://github.com/Azure/azure-sdk-for-node
[csomópont-uuid]: https://www.npmjs.com/package/node-uuid
[nconf]: https://www.npmjs.com/package/nconf
[aszinkron]: https://www.npmjs.com/package/async

[Azure Portal]: https://portal.azure.com

[Create and deploy a Node.js application tooan Azure Web Site]: app-service-web-get-started-nodejs.md

<!-- Image References -->

[node-table-finished]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_empty.png
[node-table-list-items]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_list.png
[download-publishing-settings]: ./media/storage-nodejs-use-table-storage-web-site/azure-account-download-cli.png
[portal-storage-access-keys]: ./media/storage-nodejs-use-table-storage-web-site/manage-access-keys.png
[app-settings]: ./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png
