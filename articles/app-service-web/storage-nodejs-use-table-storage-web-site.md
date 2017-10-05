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
# <a name="nodejs-web-app-using-the-azure-table-service"></a>Az Azure Table Service-t használó Node.js-alapú webalkalmazás
## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan tárolhatja és érheti el az adatok adatkezelés Azure által biztosított Table szolgáltatás segítségével egy [csomópont] üzemeltetett alkalmazás [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások. Ez az oktatóanyag feltételezi, hogy rendelkezik némi tapasztalattal a csomópont használatát és [Git].

Az oktatóanyagban érintett témák köre:

* A csomópont-modulok telepítése (csomópont Csomagkezelő) npm segítségével
* Az Azure Table szolgáltatás használata
* Hogyan webalkalmazás létrehozása az Azure parancssori felület használatával.

Az oktatóanyag utasításait követve egy egyszerű, webalapú fog létrehozni, amely lehetővé teszi a létrehozása, beolvasása és feladatok végrehajtása "Feladatlista" alkalmazást. A Table szolgáltatás tárolja a feladatokat.

A kész alkalmazás a következő:

![Egy üres tasklist szöveget megjelenítő weblap][node-table-finished]

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Mielőtt Ez a cikk utasításait követve ellenőrizze, hogy a következőkkel:

* [csomópont] 0.10.24 verzió vagy újabb
* [Git]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a>Create a storage account
Hozzon létre egy Azure-tárfiókot. Az alkalmazás a Tennivalólista elemein tárolására fog használni ezt a fiókot.

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **új** a portál bal alsó ikonra, majd kattintson a **adatok + tárolás** > **tárolási**. A storage-fiók egy egyedi nevet adjon, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.
   
      ![Új gomb](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)
   
    A storage-fiók létrehozásakor a **értesítések** gomb villogjon, egy zöld **sikeres** és a storage-fiók panelje meg nyitva, hogy a létrehozott új erőforráscsoporthoz tartozó megjeleníthető.
3. A storage-fiók paneljén kattintson **beállítások** > **kulcsok**. Az elsődleges elérési kulcsot másolja a vágólapra.
   
    ![A hozzáférési kulcsot][portal-storage-access-keys]

## <a name="install-modules-and-generate-scaffolding"></a>Modulok telepítése és állványok készítése
Ez a szakasz hozzon létre egy új csomópont alkalmazást, és modul-csomagokat adjon az npm segítségével. Az alkalmazás fogja használni a [Express] és [Azure] modulok. Az Express modul modell nézetvezérlő keretet biztosít a csomópont, amíg az Azure modulok kapcsolatot biztosít annak a Table szolgáltatás.

### <a name="install-express-and-generate-scaffolding"></a>Express telepítése és állványok készítése
1. A parancssorból hozzon létre egy új könyvtárat, nevű **tasklist** és a könyvtárhoz.  
2. Adja meg a következő parancsot a Express modul telepítése.
   
        npm install express-generator@4.2.0 -g
   
    Attól függően, hogy az operációs rendszer, amelyre a parancs előtt a "sudo" lehet szükség:
   
        sudo npm install express-generator@4.2.0 -g
   
    Az eredmény jelenik meg a következőhöz hasonló:
   
        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)
   
   > [!NOTE]
   > A "-g" paraméter globálisan telepíti a modult. Ily módon használhatjuk **expressz** web app állványok létrehozásához írja be a további információ nélkül.
   > 
   > 
3. Az alkalmazás állványok létrehozásához adja meg a **expressz** parancs:
   
        express
   
    Ez a parancs az alábbi példához hasonló jelenik meg:
   
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
   
    Most már rendelkezik néhány új könyvtárak és fájlok a **tasklist** könyvtár.

### <a name="install-additional-modules"></a>A kiegészítő modulok telepítése
Egy fájlt, amely **expressz** hoz létre az **package.json**. Ez a fájl modul függőségek listáját tartalmazza. Később amikor az App Service Web Apps alkalmazást telepít központilag, ez a fájl határozza meg melyik modulokat kell telepíteni az Azure-on.

Parancsot a parancssorból, írja be a következő parancsot a leírt moduljainak telepítése a **package.json** fájlt. 'Sudo' használni szeretne.

    npm install

Ez a parancs az alábbi példához hasonló jelenik meg:

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


Ezután adja meg a következő paranccsal telepíthető a [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok:

    npm install azure-storage node-uuid async nconf --save

A **--mentése** jelző ezekhez a modulokhoz, hogy bejegyzéseket ad a **package.json** fájlt.

Ez a parancs az alábbi példához hasonló jelenik meg:

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-the-application"></a>Az alkalmazás létrehozása
Most még készen áll az alkalmazás létrehozására.

### <a name="create-a-model"></a>Modell létrehozása
A *modell* olyan objektum, amely az alkalmazás adatait jelöli. Az alkalmazás csak a modell egy feladat objektum, amelyen a tennivalók listájára elem.. A következő mezők feladatnál:

* PartitionKey
* RowKey
* Name (karakterlánc)
* kategória (karakterlánc)
* Befejezett (logikai)

**PartitionKey** és **RowKey** tábla kulcsként a Table szolgáltatás által használt. További információkért lásd: [ismertetése a Table szolgáltatás adatmodell](https://msdn.microsoft.com/library/azure/dd179338.aspx).

1. Az a **tasklist** könyvtár, hozzon létre egy új könyvtárat nevű **modellek**.
2. Az a **modellek** könyvtár, hozzon létre egy új fájlt **task.js**. Ez a fájl tartalmazza majd a modellt az alkalmazás által létrehozott feladatok számára.
3. Elején a **task.js** fájlt, adja hozzá a következő kódot a szükséges kódtárak hivatkozik:
   
        var azure = require('azure-storage');
          var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;
4. Adja hozzá a következő kódot meghatározására és exportálására a feladatobjektum. Ez az objektum felelős a tábla kapcsolódik.
   
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
5. Adja hozzá a következő kódot a feladatobjektumokhoz további metódusok meghatározásához a feladatobjektumot, amelyek lehetővé teszik a táblában tárolt adatok interakció:
   
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
6. Mentse és zárja be a **task.js** fájlt.

### <a name="create-a-controller"></a>A tartományvezérlő létrehozása
A *vezérlő* HTTP-kérelmeket kezeli, és ez a beállítás a HTML-válaszban.

1. Az a **tasklist/útvonalak** könyvtár, hozzon létre egy új fájlt **tasklist.js** , majd nyissa meg szövegszerkesztőben.
2. Adja hozzá a következő kódot a **tasklist.js** fájlhoz. Ez betölti az azure és async modult által használt **tasklist.js**. Ez határozza meg a **TaskList** függvénynek, amely egy példánya átadása a **feladat** objektum korábban meghatározott:
   
        var azure = require('azure-storage');
        var async = require('async');
   
        module.exports = TaskList;
3. Adja meg a **TaskList** objektum.
   
        function TaskList(task) {
          this.task = task;
        }
4. Adja hozzá a következő módszerekkel **TaskList**:
   
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

### <a name="modify-appjs"></a>Az app.js fájl módosítása
1. Az a **tasklist** könyvtár, nyissa meg a **app.js** fájlt. Ez a fájl futtatásával korábban létrejött a **expressz** parancsot.
2. A fájl elején adja hozzá az azure-moduljának betöltése, a táblázat nevét, a partíciós kulcs, és állítsa be a tároló hitelesítő adatokat használja ebben a példában a következőket:
   
        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");
   
   > [!NOTE]
   > nconf fog betölteni a konfigurációs értékek vagy környezeti változók vagy a **config.json** fájlt, amely később hozunk létre.
   > 
   > 
3. Az app.js fájlban görgessen le, ahol megjelenik a következő sort:
   
        app.use('/', routes);
        app.use('/users', users);
   
    A fenti sorok cserélje le az alább látható kódot. Ez egy példányát inicializálja <strong>feladat</strong> a tárfiók kapcsolattal rendelkező. Ez átadott a <strong>TaskList</strong>, amelyek segítségével kommunikálnak a Table szolgáltatás:
   
        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
4. Mentse a **app.js** fájlt.

### <a name="modify-the-index-view"></a>Az index nézetről módosítása
1. Nyissa meg a **tasklist/views/index.jade** fájlt egy szövegszerkesztőben.
2. Cserélje le a fájl teljes tartalmát a következő kódra. Ez határozza meg a nézet jeleníti meg a meglévő feladatokat, és az új feladatok hozzáadása és meglévőket megjelölés befejezettként űrlapot tartalmaz.
   
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
3. Mentse és zárja be **index.jade** fájlt.

### <a name="modify-the-global-layout"></a>A globális elrendezés módosítása
A **Views** fájlt a **nézetek** könyvtár más globális sablonjaként **.jade** fájlokat. Ebben a lépésben módosítja úgy, hogy használjon [Twitter Bootstrap](https://github.com/twbs/bootstrap), ez az egy eszközkészlet, amellyel könnyedén töltött kinézetű webalkalmazás tervezéséhez.

Töltse le és csomagolja ki a fájlokat a [Twitter Bootstrap](http://getbootstrap.com/). Másolás a **bootstrap.min.css** a fájlt a rendszerindítási **css** mappából a **nyilvános/stíluslapok** az alkalmazás könyvtárába.

Az a **nézetek** mappa, nyissa meg **Views** , és cserélje ki a teljes tartalmát a következőre:

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

### <a name="create-a-config-file"></a>Egy konfigurációs fájl létrehozása
Az alkalmazás futtatásához helyileg, azt fogja üzembe Azure Storage hitelesítő adatokat a konfigurációs fájlban. Hozzon létre egy fájlt **config.json* * és a következő JSON:

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Cserélje le **tárfióknév** nevű, a tárolási fiók korábban létrehozott, és cserélje le **tárelérési kulcs** a tárfiók elsődleges elérési kulcsát. Példa:

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Mentse a fájlt *egy könyvtár szintjén magasabb* mint a **tasklist** könyvtárban, ehhez hasonló:

    parent/
      |-- config.json
      |-- tasklist/

Ennek az az oka, a konfigurációs fájl ellenőrzése forrás vezérlőbe elkerülése érdekében, ahol előfordulhat, hogy válik nyilvános. Azt az alkalmazás telepítése az Azure-ba, amikor használjuk a környezeti változók egy konfigurációs fájl helyett.

## <a name="run-the-application-locally"></a>Az alkalmazás helyi futtatása
Az alkalmazás a helyi gépén, hajtsa végre az alábbi lépéseket:

1. A parancssorból lépjen a **tasklist** könyvtár.
2. Az alábbi parancs segítségével indítsa el az alkalmazás helyi:
   
        npm start
3. Nyisson meg egy webböngészőt, és navigáljon a http://127.0.0.1:3000.
   
    Az alábbi példához hasonló weblap jelenik meg.
   
    ![Egy weblap, egy üres tasklist megjelenítése][node-table-finished]
4. Hozzon létre egy új teendő, adjon meg egy nevet és a kategória, és kattintson a **elem hozzáadása**. 
5. Befejeződött, ellenőrizze, feladatok **Complete** kattintson **frissítési feladatok**.
   
    ![Az új elemet a listában a feladatok képe][node-table-list-items]

Annak ellenére, hogy az alkalmazás helyileg fut, akkor az adatok az Azure Table szolgáltatásban tárolja.

## <a name="deploy-your-application-to-azure"></a>Az Azure alkalmazás telepítése
A jelen szakaszban szereplő lépéseket az Azure parancssori eszközök segítségével hozzon létre egy új webalkalmazást az App Service-ben, és a Git segítségével telepítse központilag az alkalmazást. A műveletek végrehajtásához Azure-előfizetéssel kell rendelkeznie.

> [!NOTE]
> Ezeket a lépéseket is segítségével hajtható végre a [Azure Portal](https://portal.azure.com/). Lásd: [és üzembe egy Node.js-webalkalmazás az Azure App Service].
> 
> Ha ez az első webalkalmazás hozott létre, az Azure portál telepíti az alkalmazást kell használnia.
> 
> 

Első lépésként telepítse a [Azure CLI] írja be a következő parancsot a parancssorból:

    npm install azure-cli -g

### <a name="import-publishing-settings"></a>Közzétételi beállítások importálása
Ebben a lépésben az előfizetés kapcsolatos információkat tartalmazó fájl tölti le.

1. Írja be a következő parancsot:
   
        azure login
   
    Ez a parancs elindítja egy böngészőt, és a letöltési lapra lép. Ha a rendszer kéri, jelentkezzen be az Azure-előfizetéséhez társított fiókot.
   
    <!-- ![The download page][download-publishing-settings] -->
   
    A fájl letöltése automatikusan elindul; Ha nem, kattintson a hivatkozásra a lap saját kezűleg letölteni a fájlt elején. Mentse a fájlt, és jegyezze fel a fájl elérési útját.
2. Adja meg a beállítások importálása a következő parancsot:
   
        azure account import <path-to-file>
   
    Az előző lépésben adja meg a letöltött közzétételi beállítások fájlját elérési útját és nevét.
3. A beállítások importálása, miután törli a közzétételi beállítások fájlja. Már nem szükséges, és az Azure-előfizetéssel kapcsolatos bizalmas információkat tartalmaznak.

### <a name="create-an-app-service-web-app"></a>Az App Service webalkalmazás létrehozása
1. A parancssorból lépjen a **tasklist** könyvtár.
2. A következő paranccsal létrehozhat egy új webalkalmazást.
   
        azure site create --git
   
    A webes alkalmazás neve és a hely bekéri. Adjon meg egy egyedi nevet, és válassza ki az ugyanazon a földrajzi helyen, a Azure Storage-fiókkal.
   
    A `--git` paraméter létrehoz egy Git-tárházat az Azure a webalkalmazás. Inicializálja az aktuális könyvtárban található egy Git-tárház Ha egyik sem létezik, és hozzáad egy [Git távoli] nevű "azure", amely segítségével teheti közzé az alkalmazást az Azure-bA. Végül létrehoz egy **web.config** fájl, amely tartalmazza a csomópont-alkalmazások üzemeltetését Azure által használt beállítások. Ha nincs megadva a `--git` paraméter, de a könyvtár tartalmaz egy Git-tárházat, a parancs még mindig létrehoz az azure távoli.
   
    Ez a parancs befejezése után az alábbihoz hasonló kimenetet fog látni. Vegye figyelembe, hogy a sor kezdve **webhely létrejött. helye** a webes alkalmazás URL-CÍMÉT tartalmazza.
   
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
   > Ha ez az első App Service web app az előfizetéséhez, utasította a webalkalmazás létrehozása az Azure portál használatával. További információkért lásd: [és üzembe egy Node.js-webalkalmazás az Azure App Service].
   > 
   > 

### <a name="set-environment-variables"></a>Környezeti változók beállítása
Ebben a lépésben az Azure web app konfigurációjához felveszi a környezeti változók.
A parancssorból írja be a következőt:

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


Cserélje le  **<storage account name>**  nevű, a tárolási fiók korábban létrehozott, és cserélje le  **<storage access key>**  a tárfiók elsődleges elérési kulcsát. (A megegyező értékeket használja a korábban létrehozott config.json fájlként.)

Másik lehetőségként a beállíthatja a környezeti változók a [Azure Portal](https://portal.azure.com/):

1. Nyissa meg a webalkalmazása panelén kattintva **Tallózás** > **webalkalmazások** > a webes alkalmazás neve.
2. A webalkalmazás panelen kattintson **összes beállítás** > **Alkalmazásbeállítások**.
   
     <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->
3. Görgessen le a **Alkalmazásbeállítások** szakaszt, és vegye fel a kulcs/érték párok.
   
     ![Alkalmazásbeállítások](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)
4. Kattintson a **SAVE** (Mentés) gombra.

### <a name="publish-the-application"></a>Az alkalmazás közzététele
Tegye közzé az alkalmazást, a kód fájlok véglegesítése Git, és majd leküldése azure/fő.

1. Az üzembe helyezési hitelesítő adatok beállítása.
   
        azure site deployment user set <name> <password>
2. Vegye fel, és véglegesítse az alkalmazásfájlokat.
   
        git add .
        git commit -m "adding files"
3. A véglegesítési leküldése az App Service web app:
   
        git push azure master
   
    Használjon **fő** , a cél ágat. A telepítés végén megjelenik egy utasítást a következőhöz hasonló:
   
        To https://username@tabletasklist.azurewebsites.net/TableTasklist.git
          * [new branch]      master -> master
4. A leküldéses befejezése után, keresse meg a webes alkalmazás URL-cím által korábban visszaadott a `azure create site` parancsot az alkalmazás megtekintéséhez.

## <a name="next-steps"></a>Következő lépések
Amíg ez a cikk lépéseit írják le, a Table szolgáltatás használatával az adatok tárolására, használhatja [MongoDB](https://mlab.com/azure/). 

## <a name="additional-resources"></a>További források
[Azure CLI]

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

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

[Create and deploy a Node.js application to an Azure Web Site]: app-service-web-get-started-nodejs.md

<!-- Image References -->

[node-table-finished]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_empty.png
[node-table-list-items]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_list.png
[download-publishing-settings]: ./media/storage-nodejs-use-table-storage-web-site/azure-account-download-cli.png
[portal-storage-access-keys]: ./media/storage-nodejs-use-table-storage-web-site/manage-access-keys.png
[app-settings]: ./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png
