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
# <a name="nodejs-web-application-using-storage"></a>Storage használata node.js-webalkalmazás
## <a name="overview"></a>Áttekintés
Ebben az oktatóanyagban hello-ben létrehozott alkalmazás a [Express használata Node.js-webalkalmazás] oktatóanyag ki van bővítve hello Microsoft Azure Ügyfélkódtárai használata Node.js toowork management szolgáltatásokkal. Hozzon létre egy webalapú feladatlista alkalmazás, hogy tooAzure telepíthet kiterjeszti az alkalmazást. hello feladatlista lehetővé teszi a felhasználóknak feladatokat beolvasni, adja hozzá az új feladatok és feladatok megjelölése befejezettként.

hello feladat tárolódnak az Azure Storage. Az Azure Storage biztosítja a hibatűrő és magas rendelkezésre állású strukturálatlan adatok tárhelyet. Az Azure Storage számos adatszerkezetek, ahol tárolhatja és érheti adatait tartalmazza. Hello tárolószolgáltatások a hello API-k szereplő hello Azure SDK for Node.js vagy a REST API-kon keresztül is használhatja. További információkért lásd: [tárolása és az adatok elérése az Azure-ban].

Ez az oktatóanyag feltételezi, hogy végrehajtotta-e az hello [Node.js-webalkalmazás] és [expressz Node.js][Express használata Node.js-webalkalmazás] oktatóanyagok.

Hello a következő információkat tartalmazza:

* Hogyan toowork a hello Jade sablon motor
* Hogyan toowork Azure adatok szolgáltatásokhoz

a következő képernyőkép hello befejeződött hello alkalmazás látható:

![hello befejeződött az internet explorer weblapot](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Tárolási hitelesítő adatok beállítása a Web.config fájlban
Át kell adnia a tároló hitelesítő adatait tooaccess Azure Storage. Ez való hello web.config Alkalmazásbeállítások használatával történik.
hello web.config beállítások átadása pedig környezeti változók tooNode, amely majd olvassák hello Azure SDK-t.

> [!NOTE]
> Tárolási csak szolgálnak, ha hello alkalmazás telepített tooAzure. Hello emulátorban futtatásakor hello alkalmazás a hello storage emulator használja.
>
>

Hajtsa végre a következő lépéseket tooretrieve hello tárfiók hitelesítő adatainak hello, és vegye fel őket toohello web.config beállítást:

1. Még nincs nyitva, hogy kezdődnie hello Azure PowerShell hello **Start** kibontásával menü **minden program és az Azure-**, kattintson a jobb gombbal **Azure PowerShell**, majd válassza ki a  **Futtatás rendszergazdaként**.
2. Módosítsa a könyvtárakat toohello mappát, amely tartalmazza az alkalmazás. Például a C:\\csomópont\\tasklist\\WebRole1.
3. Adja meg a következő parancsmag tooretrieve hello tárfiókadatok hello hello Azure Powershell-ablakban:

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   hello előző parancsmag beolvassa hello listában tárfiókok és az üzemeltetett szolgáltatás társított kulcsait.

   > [!NOTE]
   > Hello Azure SDK tárfiók létrehozása, ha a szolgáltatás telepítése, mert a tárfiók már léteznie kell a hello előző útmutatók az alkalmazás központi telepítése.
   >
   >
4. Nyissa meg hello **ServiceDefinition.csdef** Ha hello alkalmazás telepített tooAzure használt hello környezet beállításokat tartalmazó fájlt:

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. INSERT hello következő letiltása a **környezet** elemet, és a {TÁRFIÓK} és {TÁRELÉRÉSI kulcs} hello fióknévvel és hello elsődleges kulcsának hello toouse szánt központi telepítés:

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![hello web.cloud.config fájl tartalma](./media/table-storage-cloud-service-nodejs/node37.png)

6. Hello fájlt mentse és zárja be a Jegyzettömböt.

### <a name="install-additional-modules"></a>A kiegészítő modulok telepítése
1. Használjon hello a következő parancs tooinstall hello [azure], [csomópont-uuid], [nconf] és [aszinkron] modulok helyileg, valamint toosave bejegyzése számukra toohello **package.json** fájlt:

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  a parancs kimenetének hello hasonló toohello következő kell megjelennie:

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

## <a name="using-hello-table-service-in-a-node-application"></a>Hello Table szolgáltatás használata node.js-alkalmazásokban
Ebben a szakaszban hello alapszintű hello által létrehozott alkalmazás **expressz** parancs hozzáadásával ki van bővítve a **task.js** a tevékenységek hello modellt tartalmazó fájl. Módosítsa a meglévő hello **app.js** fájlt, és hozzon létre egy új **tasklist.js** fájlt, amely hello modellt használ.

### <a name="create-hello-model"></a>Hello modell létrehozása
1. A hello **WebRole1** könyvtár, hozzon létre egy új könyvtárat nevű **modellek**.
2. A hello **modellek** könyvtár, hozzon létre egy új fájlt **task.js**. Ez a fájl tartalmazza az alkalmazás által létrehozott hello feladatok hello modelljét.
3. Hello hello elején **task.js** fájlt, adja hozzá a következő kód szükséges tooreference szalagtárak hello:

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. Ezután adja hozzá a kódot toodefine és hello feladat objektum exportálása. hello feladatobjektum toohello tábla csatlakozás felelős.

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

5. Ezután adja hozzá hello kód toodefine további módszereket követően hello feladatobjektum, amelyek lehetővé teszik az hello táblában tárolt adatok interakció:

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

6. Mentse és zárja be a hello **task.js** fájlt.

### <a name="create-hello-controller"></a>Hello tartományvezérlő létrehozása
1. A hello **WebRole1/útvonalak** könyvtár, hozzon létre egy új fájlt **tasklist.js** , majd nyissa meg szövegszerkesztőben.
2. Adja hozzá a következő kód túl hello**tasklist.js**. Ez a kód betölti hello azure és a modulok aszinkron által használt **tasklist.js** és hello meghatározása **TaskList** függvény, amelyet hello példányának **feladat** azt objektum korábban meghatározott:

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. Hozzáadja a toohello **tasklist.js** fájl túl használt hello metódusok hozzáadásával**showTasks**, **Addtasks**, és **completeTasks**:

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

4. Mentse a hello **tasklist.js** fájlt.

### <a name="modify-appjs"></a>Az app.js fájl módosítása
1. A hello **WebRole1** könyvtárába, nyissa meg hello **app.js** fájlt egy szövegszerkesztőben.
2. Elején hello hello fájlt, adja hozzá a következő tooload hello azure modul hello és hello tábla nevét és a partíciós kulcs beállítása:

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. Hello app.js fájlban, görgessen lefelé látja toowhere hello a következő sort:

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    Cserélje le a következő kód hello sorok megelőző hello. Ez a kód egy példányát inicializálja <strong>feladat</strong> kapcsolat tooyour storage-fiók. Hello <strong>feladat</strong> toohello átadása <strong>TaskList</strong>, melyik használ az toocommunicate hello tábla szolgáltatással:

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. Mentse a hello **app.js** fájlt.

### <a name="modify-hello-index-view"></a>Hello index nézet módosítása
1. Módosítsa a könyvtárakat toohello **nézetek** könyvtárhoz, és nyissa meg hello **index.jade** fájlt egy szövegszerkesztőben.
2. Cserélje le a hello hello tartalmát **index.jade** hello kód a következő fájl. Ezzel a kóddal hello nézetet meglévő feladatok megjelenítéséhez határozza meg, és határozza meg az új feladatok hozzáadása és meglévőket megjelölés befejezettként űrlap.

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

3. Mentse és zárja be **index.jade** fájlt.

### <a name="modify-hello-global-layout"></a>Hello globális elrendezés módosítása
Hello **Views** hello fájlban **nézetek** directory globális sablonként szolgál az egyéb **.jade** fájlokat. Ebben a lépésben módosítsa a hello **Views** toouse fájl [Twitter Bootstrap](https://github.com/twbs/bootstrap), amelyen egy eszközkészlet, így könnyen toodesign egy töltött tetszetős webhelyeket.

1. A hello fájlok letöltéséhez és kibontásához [Twitter Bootstrap](http://getbootstrap.com/). Másolás hello **bootstrap.min.css** hello fájlt **bootstrap\\eloszlás\\css** mappa toohello **nyilvános\\stíluslapok** az tasklist alkalmazás könyvtár.
2. A hello **nézetek** mappa, nyissa meg hello **Views** fájlt a szövegszerkesztőben, és cserélje ki hello tartalmát hello következőre:

    DOCTYPE html html központi cím cím hivatkozás = (rel = "xsl", href='/stylesheets/bootstrap.min.css) hivatkozás (rel = "xsl", href='/stylesheets/style.css) body.app nav.navbar.navbar alapértelmezett div.navbar-fejléc a.navbar-brand(href='/') a  Feladatok blokkolja a tartalmat

3. Mentse a hello **Views** fájlt.

### <a name="running-hello-application-in-hello-emulator"></a>Hello emulátor hello alkalmazás fut
A következő parancs toostart hello alkalmazás hello emulátorban hello használata.

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

hello böngészőben nyílik meg, és megjeleníti a következő lap hello:

![A webes lapozható című saját feladatlista feladatok és a mezők tooadd tartalmazó tábla egy új feladatot.](./media/table-storage-cloud-service-nodejs/node44.png)

Hello űrlap tooadd elemek használja, vagy távolítsa el a meglévő elemeket befejezettként jelöli meg őket.

## <a name="publishing-hello-application-tooazure"></a>Közzétételi hello alkalmazás tooAzure
Windows PowerShell-ablakban hello hívja a következő parancsmag tooredeploy hello az üzemeltetett szolgáltatás tooAzure.

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

Cserélje le **myuniquename** ehhez az alkalmazáshoz, egyedi névvel. Cserélje le **datacentername** hello nevet, egy Azure-adatközponthoz, például a **USA nyugati régiója**.

Hello telepítés befejezése után, a válasz hasonló toohello következő kell megjelennie:

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

Hello megadásával **-elindítása** hello előző parancsmag, hello böngésző beállítás megnyílik, és megjeleníti az alkalmazás Azure-beli közzététel befejezésekor.

![Hello saját feladatlista lapot megjelenítő böngészőablak. hello URL-cím jelzi, hello lap most alatt az Azure-on.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Leállítása és az alkalmazás törlése
Után az alkalmazás telepítéséhez, érdemes lehet toodisable, így költségek elkerülése vagy létrehozhatja és más alkalmazások belül hello központi telepítése ingyenes próbaverziós időszak.

Az Azure a webesszerepkör-példányok esetében óránként számol fel díjat a felhasznált kiszolgálóidő után.
Kiszolgálói felhasznált után az alkalmazás van telepítve, akkor is, ha a példányok nem futnak, és hello leállt állapotban van.

hello következő lépések bemutatják, hogyan toostop és törli az alkalmazást.

1. Hello Windows PowerShell-ablakban állítsa le a hello szolgáltatás központi telepítése a következő parancsmag hello hello előző szakaszban létrehozott:

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   Hello szolgáltatás leállítása eltarthat néhány percig. Hello szolgáltatás leáll, amikor megjelenik egy üzenet, amely azt jelzi, hogy leállt.

2. toodelete hello szolgáltatást, a következő parancsmag hívás hello:

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   Amikor a rendszer kéri, adja meg a **Y** toodelete hello szolgáltatást.

   Hello szolgáltatás törlése eltarthat néhány percig. Hello szolgáltatás törlését követően kapni fog egy üzenet, amely azt jelzi, hogy törölve lett-e a hello szolgáltatást.

[Express használata Node.js-webalkalmazás]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[tárolása és az adatok elérése az Azure-ban]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-webalkalmazás]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


