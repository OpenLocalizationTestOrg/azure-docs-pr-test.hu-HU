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
# <a name="nodejs-web-app-using-hello-azure-table-service"></a>Hello Azure Table szolgáltatás használata node.js-webalkalmazás
## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan toouse Table szolgáltatás Azure adatkezelés toostore és hozzáférési adatok biztosítja a [csomópont] üzemeltetett alkalmazás [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások. Ez az oktatóanyag feltételezi, hogy rendelkezik némi tapasztalattal a csomópont használatát és [Git].

Az oktatóanyagban érintett témák köre:

* Hogyan toouse npm (csomópont Csomagkezelő) tooinstall hello csomópont modulok
* Hogyan toowork a hello Azure Table szolgáltatás
* Hogyan toouse hello Azure CLI toocreate egy webalkalmazást.

Az oktatóanyag utasításait követve egy egyszerű, webalapú fog létrehozni, amely lehetővé teszi a létrehozása, beolvasása és feladatok végrehajtása "Feladatlista" alkalmazást. hello feladatok hello Table szolgáltatás vannak tárolva.

Alkalmazás hello befejeződött a következő:

![Egy üres tasklist szöveget megjelenítő weblap][node-table-finished]

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Hello jelen cikkben lévő utasítások követése, előtt győződjön meg arról, hogy hello következőkkel:

* [csomópont] 0.10.24 verzió vagy újabb
* [Git]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a>Create a storage account
Hozzon létre egy Azure-tárfiókot. hello alkalmazás fogja használni a fiókot toostore hello Tennivalólista elemein.

1. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/).
2. Kattintson a hello **új** hello portál bal hello alsó ikonra, majd kattintson a **adatok + tárolás** > **tárolási**. Hello tárfiók adjon egy egyedi nevet, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.
   
      ![Új gomb](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)
   
    Hello tárfiók létrehozásakor hello **értesítések** gomb villogjon, egy zöld **sikeres** és hello tárolási fiók panelje meg nyitva, hogy tartozik-e új erőforrás toohello tooshow csoportjának, létre.
3. Hello tárolási fiók paneljén kattintson **beállítások** > **kulcsok**. Hello elsődleges elérési kulcs toohello vágólapra másolása.
   
    ![A hozzáférési kulcsot][portal-storage-access-keys]

## <a name="install-modules-and-generate-scaffolding"></a>Modulok telepítése és állványok készítése
Ez a szakasz hozzon létre egy új csomópont alkalmazást, és npm tooadd modul csomagot. Ehhez az alkalmazáshoz használandó hello [Express] és [Azure] modulok. hello Express modul modell nézetvezérlő keretet biztosít a csomópont, miközben hello Azure modulok kapcsolat toohello tábla szolgáltatást biztosít.

### <a name="install-express-and-generate-scaffolding"></a>Express telepítése és állványok készítése
1. Hello parancssorból hozzon létre egy új könyvtárat, nevű **tasklist** és kapcsoló toothat könyvtár.  
2. Adja meg a következő parancs tooinstall hello Express modul hello.
   
        npm install express-generator@4.2.0 -g
   
    Attól függően, hogy hello operációs rendszer szükség lehet a tooput 'sudo' parancs hello előtt:
   
        sudo npm install express-generator@4.2.0 -g
   
    hello eredmény jelenik meg a következő példa hasonló toohello:
   
        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)
   
   > [!NOTE]
   > hello "-g" paraméter hello modul globálisan telepíti. Ily módon használhatjuk **expressz** toogenerate web app állványok anélkül, hogy tootype további információt.
   > 
   > 
3. toocreate hello állványok hello alkalmazáshoz, adja meg a hello **expressz** parancs:
   
        express
   
    a parancs kimenetének hello jelenik meg a következő példa hasonló toohello:
   
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
   
    Most már rendelkezik néhány új könyvtárak és fájlok hello **tasklist** könyvtár.

### <a name="install-additional-modules"></a>A kiegészítő modulok telepítése
Hello egyik fájlok **expressz** hoz létre az **package.json**. Ez a fájl modul függőségek listáját tartalmazza. Később hello alkalmazás tooApp Service Web Apps telepítésekor ez a fájl határozza meg, melyik modulokat kell az Azure telepített toobe.

Hello parancssori, adja meg a következő parancs tooinstall hello modulok hello ismertetett hello **package.json** fájlt. Szükség lehet toouse 'sudo'.

    npm install

a parancs kimenetének hello jelenik meg a következő példa hasonló toohello:

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


Ezután adja meg a következő parancs tooinstall hello hello [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok:

    npm install azure-storage node-uuid async nconf --save

Hello **--mentése** jelző hozzáadja a modulok toohello bejegyzéseket **package.json** fájlt.

a parancs kimenetének hello jelenik meg a következő példa hasonló toohello:

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-hello-application"></a>Hello alkalmazás létrehozása
Most már készen áll a toobuild hello alkalmazást a rendszer.

### <a name="create-a-model"></a>Modell létrehozása
A *modell* hello adatait az alkalmazás képviselő objektum. Hello alkalmazás hello csak modell egy feladat objektum, amely hello feladatlista elem.. A következő mezők hello feladatnál:

* PartitionKey
* RowKey
* Name (karakterlánc)
* kategória (karakterlánc)
* Befejezett (logikai)

**PartitionKey** és **RowKey** tábla kulcsként hello Table szolgáltatás által használt. További információkért lásd: [ismertetése hello Table szolgáltatás adatmodell](https://msdn.microsoft.com/library/azure/dd179338.aspx).

1. A hello **tasklist** könyvtár, hozzon létre egy új könyvtárat nevű **modellek**.
2. A hello **modellek** könyvtár, hozzon létre egy új fájlt **task.js**. Ezt a fájlt fogja tartalmazni az alkalmazás által létrehozott hello feladatok hello modelljét.
3. Hello hello elején **task.js** fájlt, adja hozzá a következő kód szükséges tooreference szalagtárak hello:
   
        var azure = require('azure-storage');
          var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;
4. Adja hozzá a hello alábbi code toodefine, és hello feladat objektum exportálása. Ez az objektum toohello tábla csatlakozás felelős.
   
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
5. Adja hozzá hello kód toodefine további módszereket követően hello feladatobjektum, amelyek lehetővé teszik az hello táblában tárolt adatok interakció:
   
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
6. Mentse és zárja be a hello **task.js** fájlt.

### <a name="create-a-controller"></a>A tartományvezérlő létrehozása
A *vezérlő* HTTP-kérelmeket kezeli, és ez a beállítás hello HTML-válasz.

1. A hello **tasklist/útvonalak** könyvtár, hozzon létre egy új fájlt **tasklist.js** , majd nyissa meg szövegszerkesztőben.
2. Adja hozzá a következő kód túl hello**tasklist.js**. Ez betölti a hello azure és async modult által használt **tasklist.js**. Ez meghatározza hello **TaskList** függvény, amelyet hello példányának **feladat** objektum korábban meghatározott:
   
        var azure = require('azure-storage');
        var async = require('async');
   
        module.exports = TaskList;
3. Adja meg a **TaskList** objektum.
   
        function TaskList(task) {
          this.task = task;
        }
4. Adja hozzá a következő módszerek túl hello**TaskList**:
   
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
1. A hello **tasklist** könyvtárába, nyissa meg hello **app.js** fájlt. Ez a fájl készült korábbi hello **expressz** parancsot.
2. A hello fájl hello elején tooload hello azure modul, a hello tábla neve, a partíciós kulcs és a set hello tárolási által használt hitelesítő adatok ebben a példában a következő hello hozzáadása:
   
        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");
   
   > [!NOTE]
   > nconf hello konfigurációs értékeket fogja betölteni a környezeti változók vagy hello **config.json** fájlt, amely később hozunk létre.
   > 
   > 
3. Hello app.js fájlban, görgessen lefelé látja toowhere hello a következő sort:
   
        app.use('/', routes);
        app.use('/users', users);
   
    Cserélje le hello sorok fent lent látható módon hello kódra. Ez egy példányát inicializálja <strong>feladat</strong> kapcsolat tooyour storage-fiók. Ez átadása toohello <strong>TaskList</strong>, amely fog használni az toocommunicate hello Table szolgáltatás:
   
        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
4. Mentse a hello **app.js** fájlt.

### <a name="modify-hello-index-view"></a>Hello index nézet módosítása
1. Nyissa meg hello **tasklist/views/index.jade** fájlt egy szövegszerkesztőben.
2. Cserélje le a következő kód hello hello hello fájl teljes tartalmát. Ez határozza meg a nézet jeleníti meg a meglévő feladatokat, és az új feladatok hozzáadása és meglévőket megjelölés befejezettként űrlapot tartalmaz.
   
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

### <a name="modify-hello-global-layout"></a>Hello globális elrendezés módosítása
Hello **Views** hello fájlban **nézetek** könyvtár más globális sablonjaként **.jade** fájlokat. Ebben a lépésben lesz a módosítás toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), amely, így könnyen toodesign töltött kinézetű webalkalmazás eszközkészletre.

A hello fájlok letöltéséhez és kibontásához [Twitter Bootstrap](http://getbootstrap.com/). Másolás hello **bootstrap.min.css** hello rendszerindítási fájlt **css** hello mappából **nyilvános/stíluslapok** az alkalmazás könyvtárába.

A hello **nézetek** mappa, nyissa meg **Views** , és cserélje le a hello teljes tartalma hello alábbira:

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
hello toorun alkalmazást helyileg, azt fogja üzembe Azure Storage hitelesítő adatokat a konfigurációs fájlban. Hozzon létre egy fájlt **config.json* * a következő JSON hello:

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Cserélje le **tárfióknév** hello tárolási hello nevű fiókra, korábban létrehozott, és cserélje le **tárelérési kulcs** a hello elsődleges hozzáférési kulcsot a tárfiók. Példa:

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Mentse a fájlt *egy könyvtár szintjén magasabb* hello mint **tasklist** könyvtárban, ehhez hasonló:

    parent/
      |-- config.json
      |-- tasklist/

hello ennek oka, hogy tooavoid hello konfigurációs fájl ellenőrzése a verziókövetés, ahol azt válnak nyilvános. Hello app tooAzure telepítésekor használjuk a környezeti változók egy konfigurációs fájl helyett.

## <a name="run-hello-application-locally"></a>Hello alkalmazás helyileg történő futtatása
tootest hello alkalmazás a helyi gépén, hajtsa végre a lépéseket követve hello:

1. A parancssori hello, módosítsa a könyvtárakat toohello **tasklist** könyvtár.
2. A következő parancs toolaunch hello alkalmazás helyi hello használata:
   
        npm start
3. Nyisson meg egy webböngészőt, és keresse meg a toohttp://127.0.0.1:3000.
   
    A következő példa egy weblap hasonló toohello jelenik meg.
   
    ![Egy weblap, egy üres tasklist megjelenítése][node-table-finished]
4. egy új teendő toocreate adjon meg egy nevet és a kategória, és kattintson a **elem hozzáadása**. 
5. toomark feladat befejeződött, ellenőrizze, **Complete** kattintson **frissítési feladatok**.
   
    ![Hello új hello lista elemeire feladatok képe][node-table-list-items]

Annak ellenére, hogy hello alkalmazás helyben fut, akkor van hello Azure Table szolgáltatás hello adatok tárolását.

## <a name="deploy-your-application-tooazure"></a>Az alkalmazás tooAzure telepítése
Ebben a szakaszban található lépéseket hello használja hello Azure parancssori eszközök toocreate egy új webalkalmazást az App Service-ben, és a Git toodeploy az alkalmazás. tooperform ezeket a lépéseket az Azure-előfizetéssel kell rendelkeznie.

> [!NOTE]
> Ezeket a lépéseket is hello segítségével hajtható végre [Azure Portal](https://portal.azure.com/). Lásd: [és üzembe egy Node.js-webalkalmazás az Azure App Service].
> 
> Ha ez hello első webalkalmazás hozott létre, az alkalmazás hello Azure Portal toodeploy kell használnia.
> 
> 

tooget indult el, telepítse a hello [Azure CLI] hello parancs hello parancssorból a következő beírásával:

    npm install azure-cli -g

### <a name="import-publishing-settings"></a>Közzétételi beállítások importálása
Ebben a lépésben az előfizetés kapcsolatos információkat tartalmazó fájl tölti le.

1. Adja meg a következő parancs hello:
   
        azure login
   
    Ez a parancs elindítja egy böngészőt, és toohello letöltési lapra lép. Ha a rendszer kéri, jelentkezzen be az Azure-előfizetéshez társított hello fiókkal.
   
    <!-- ![hello download page][download-publishing-settings] -->
   
    hello fájl letöltése automatikusan elindul; Ha nem, kattintson a hello hivatkozás hello toomanually letöltési hello lapozófájl hello elején. Mentse a hello fájl- és megjegyzés hello fájl elérési útját.
2. Adja meg a következő parancs tooimport hello beállítások hello:
   
        azure account import <path-to-file>
   
    Adja meg közzététele beállításfájl letöltött hello előző lépésben hello hello elérési útját és nevét.
3. Után hello tartoznak, törölje a hello közzététele beállításfájl. Már nem szükséges, és az Azure-előfizetéssel kapcsolatos bizalmas információkat tartalmaznak.

### <a name="create-an-app-service-web-app"></a>Az App Service webalkalmazás létrehozása
1. A parancssori hello, módosítsa a könyvtárakat toohello **tasklist** könyvtár.
2. A következő parancs toocreate egy új webalkalmazást hello használata.
   
        azure site create --git
   
    Hello webalkalmazás neve és helye bekéri. Adjon meg egy egyedi nevet, és válassza ki, a Azure Storage-fiókkal azonos földrajzi helyen hello.
   
    Hello `--git` paraméter létrehoz egy Git-tárházat az Azure a webalkalmazás. Inicializálja a Git-tárház hello aktuális könyvtárban található Ha egyik sem létezik, és hozzáad egy [Git távoli] nevű "azure", amely használt toopublish hello alkalmazás tooAzure. Végül létrehoz egy **web.config** fájl, amely tartalmazza az Azure toohost csomópont alkalmazások által használt beállítások. Hello kihagyásakor `--git` paraméter, de hello könyvtár tartalmaz egy Git-tárházat, hello parancs továbbra is létrehoz az "azure" hello távoli.
   
    Ez a parancs befejezése után kimeneti hasonló toohello következő jelenik meg. Vegye figyelembe, hogy hello kezdődő **webhely létrejött. helye** hello webalkalmazás hello URL-címet tartalmaz.
   
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
   > Ha ez hello első App Service web app az előfizetéséhez, utasításai toouse hello Azure Portal toocreate hello webalkalmazás fogja. További információkért lásd: [és üzembe egy Node.js-webalkalmazás az Azure App Service].
   > 
   > 

### <a name="set-environment-variables"></a>Környezeti változók beállítása
Ebben a lépésben adhat az Azure-on környezeti változók tooyour webes alkalmazás konfigurálása.
Hello parancssorból írja be a hello következő:

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


Cserélje le  **<storage account name>**  hello tárolási hello nevű fiókra, korábban létrehozott, és cserélje le  **<storage access key>**  a hello elsődleges hozzáférési kulcsot a tárfiók. (Azonos értékek hello használata korábban létrehozott hello config.json fájl.)

Másik lehetőségként beállíthatja hello környezeti változók [Azure Portal](https://portal.azure.com/):

1. Hello webalkalmazása panelén megnyitásához **Tallózás** > **webalkalmazások** > a webes alkalmazás neve.
2. A webalkalmazás panelen kattintson **összes beállítás** > **Alkalmazásbeállítások**.
   
     <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->
3. Görgessen lefelé toohello **Alkalmazásbeállítások** szakaszt, és adja hozzá a hello kulcs/érték párok.
   
     ![Alkalmazásbeállítások](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)
4. Kattintson a **SAVE** (Mentés) gombra.

### <a name="publish-hello-application"></a>Hello alkalmazás közzététele
toopublish hello alkalmazást, és véglegesítse hello kód fájlok tooGit, és majd leküldéses tooazure/fő.

1. Az üzembe helyezési hitelesítő adatok beállítása.
   
        azure site deployment user set <name> <password>
2. Vegye fel, és véglegesítse az alkalmazásfájlokat.
   
        git add .
        git commit -m "adding files"
3. Leküldéses hello véglegesítési toohello App Service webalkalmazás:
   
        git push azure master
   
    Használjon **fő** hello cél fiókirodai szerint. Hello telepítési hello végén tekintse meg a következő példa egy utasítás hasonló toohello:
   
        toohttps://username@tabletasklist.azurewebsites.net/TableTasklist.git
          * [new branch]      master -> master
4. Hello leküldéses befejezése után, keresse meg a toohello webes URL-címet hello által korábban visszaadott `azure create site` tooview parancsot az alkalmazás.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben hello lépések írják le, hello Table szolgáltatás toostore információk alapján, amíg használhatja [MongoDB](https://mlab.com/azure/). 

## <a name="additional-resources"></a>További források
[Azure CLI]

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

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
