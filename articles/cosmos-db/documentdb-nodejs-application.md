---
title: "a Node.js webalkalmazás az Azure Cosmos DB aaaBuild |} Microsoft Docs"
description: "A Node.js-oktatóanyag azt ismerteti, hogyan toouse Microsoft Azure Cosmos DB toostore és a hozzáférési adatok Node.js Express-webalkalmazások Azure Websitesban tárolt."
keywords: "Alkalmazásfejlesztés, adatbázis-oktatóanyag, node.js, a node.js-oktatóanyag megismerése"
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 9da9e63b-e76a-434e-96dd-195ce2699ef3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: mimig
ms.openlocfilehash: 31194dccf37eef69d2219b0d8328a88d434f79b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="_Toc395783175"></a>Node.js-webalkalmazás létrehozása az Azure Cosmos DB használatával
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

A Node.js-oktatóanyag bemutatja, hogyan toouse Azure Cosmos DB és hello DocumentDB API toostore és a hozzáférési adatok Node.js Express-alkalmazás az Azure Websitesban tárolt. Olyan egyszerű webalapú teendőkezelő alkalmazást, todo appot fog létrehozni, amellyel feladatokat készíthet, kérhet le, és végezhet el. hello feladatok Azure Cosmos DB JSON-dokumentumokként tárolja. Ez az oktatóanyag bemutatja, hogyan hello létrehozásának és telepítésének hello alkalmazás, és bemutatja, mi történik az összes részlet.

![Képernyőfelvétel a hello jelen Node.js oktatóanyag során létrehozott My Todo List alkalmazás](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

Nem rendelkezik idő toocomplete hello oktatóanyag, és most szeretné, hogy tooget hello teljes megoldás? Nem probléma, hogy megkaphassa a hello teljes megoldást a [GitHub][GitHub]. Csak olvasható hello [információs](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) fájl hogyan toorun hello app kapcsolatos utasításokat.

## <a name="_Toc395783176"></a>Előfeltételek
> [!TIP]
> Ez a Node.js-oktatóanyag feltételezi, hogy rendelkezik némi tapasztalattal a Node.js és az Azure Websites használatát illetően.
> 
> 

Ez a cikk hello utasításait követve, előtt győződjön meg, hogy rendelkezik-e hello következő:

* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

   VAGY

   Egy helyi telepítését teszi hello [Azure Cosmos DB emulátor](local-emulator.md) (csak Windows).
* [Node.js][Node.js]-verzió: 0.10.29-es vagy újabb.
* [Express generátor](http://www.expressjs.com/starter/generator.html) (az `npm install express-generator -g` segítségével telepítheti)
* [Git][Git].

## <a name="_Toc395637761"></a>1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása
Először hozzon létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik fiókkal, vagy használatakor hello Azure Cosmos DB emulátor ehhez az oktatóanyaghoz, ugorjon túl[2. lépés: új Node.js-alkalmazás létrehozása](#_Toc395783178).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>2. lépés: Új Node.js-alkalmazás létrehozása
Most megtanulhatja, hogyan toocreate egy alapszintű Hello World Node.js-projektet hello segítségével [Express](http://expressjs.com/) keretrendszer.

1. Nyissa meg kedvenc terminálját, például a hello Node.js parancssort.
2. Keresse meg, amelyben szeretné toostore hello új alkalmazás toohello könyvtár.
3. Egy új alkalmazást, hello express generátor toogenerate használja **todo**.
   
        express todo
4. Nyissa meg az új **todo** könyvtárat, és telepítse a függőségeket.
   
        cd todo
        npm install
5. Futtassa az új alkalmazást.
   
        npm start
6. Az új alkalmazás megtekintéséhez navigáljon a böngészőben túl[http://localhost: 3000](http://localhost:3000).
   
    ![Node.js megismerése – képernyőfelvétel a hello Hello World alkalmazásról egy böngészőablakban](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    Ezt követően toostop hello alkalmazás, nyomja le a CTRL + C hello terminálablakot, és kattintson a **y** tooterminate hello kötegelt.

## <a name="_Toc395783179"></a>3. lépés: További modulok telepítése
Hello **package.json** fájl egyike hello projekt gyökerében hello létrehozott hello fájlok. Ez a fájl tartalmazza a Node.js-alkalmazáshoz szükséges további modulok listáját. Később az alkalmazás tooAzure webhelyek központi telepítésekor a fájl használt toodetermine melyik modulokat kell toobe Azure toosupport az alkalmazás telepítve. Továbbra is kell tooinstall két további csomagok ehhez az oktatóanyaghoz.

1. Vissza a Terminálszolgáltatások hello telepítése hello **aszinkron** modult az npm.
   
        npm install async --save
2. Telepítse a hello **documentdb** modult az npm. Ez a hello modul, ahol minden hello Azure Cosmos DB magic történik.
   
        npm install documentdb --save
3. A Gyorsellenőrzés hello **package.json** hello alkalmazás fájlt meg kell jelennie hello további modulok. Ez a fájl utasítja az Azure melyik csomagok toodownload, és a telepítse az alkalmazás futtatásakor. Hello az alábbi példa azt kell hasonlítania.
   
        {
          "name": "todo",
          "version": "0.0.0",
          "private": true,
          "scripts": {
            "start": "node ./bin/www"
          },
          "dependencies": {
            "async": "^2.1.4",
            "body-parser": "~1.15.2",
            "cookie-parser": "~1.4.3",
            "debug": "~2.2.0",
            "documentdb": "^1.10.0",
            "express": "~4.14.0",
            "jade": "~1.11.0",
            "morgan": "~1.7.0",
            "serve-favicon": "~2.3.0"
          }
        }
   
    Ez értesíti a Node-ot (majd később az Azure-t) arról, hogy az alkalmazás ezektől a további moduloktól függ.

## <a name="_Toc395783180"></a>4. lépés: Hello Azure Cosmos DB szolgáltatás használata node.js-alkalmazásokban
Amely gondoskodik összes hello kezdeti beállítás és konfiguráció, most hozzuk get le toowhy segítünk, és néhány code Azure Cosmos DB használatával toowrite.

### <a name="create-hello-model"></a>Hello modell létrehozása
1. Hello projektkönyvtárban hozzon létre egy új könyvtárat nevű **modellek** a hello hello package.json fájl könyvtárába.
2. A hello **modellek** könyvtár, hozzon létre egy új fájlt **taskDao.js**. Ezt a fájlt fogja tartalmazni az alkalmazás által létrehozott hello feladatok hello modelljét.
3. Az azonos hello **modellek** könyvtár, hozzon létre egy másik új fájlt **docdbUtils.js**. Ez a fájl néhány hasznos, újrafelhasználható, az alkalmazás minden területén használt kódot tartalmaz majd. 
4. Másolás hello alábbi kódot túl**docdbUtils.js**
   
        var DocumentDBClient = require('documentdb').DocumentClient;
   
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
   
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
   
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
   
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };               
   
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
   
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
   
        module.exports = DocDBUtils;
   
5. Mentse és zárja be a hello **docdbUtils.js** fájlt.
6. Hello hello elején **taskDao.js** fájlt, adja hozzá a következő kód tooreference hello hello **DocumentDBClient** és hello **docdbUtils.js** a fenti létrehozott:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. Ezután lesz kód toodefine hozzáadása és hello feladat objektum exportálása. Ez felelős a feladatobjektum elindításáért, valamint beállítja a hello adatbázis és dokumentumgyűjtemény fogjuk használni.
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. Ezután adja hozzá hello kód toodefine további módszereket követően hello feladatobjektum, amelyek lehetővé teszik az Azure Cosmos DB tárolt adatok interakció.
   
        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
   
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
   
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
   
            find: function (querySpec, callback) {
                var self = this;
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results);
                    }
                });
            },
   
            addItem: function (item, callback) {
                var self = this;
   
                item.date = Date.now();
                item.completed = false;
   
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, doc);
                    }
                });
            },
   
            updateItem: function (itemId, callback) {
                var self = this;
   
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        doc.completed = true;
   
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
   
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
   
            getItem: function (itemId, callback) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };
9. Mentse és zárja be a hello **taskDao.js** fájlt. 

### <a name="create-hello-controller"></a>Hello tartományvezérlő létrehozása
1. A hello **útvonalak** a projekt könyvtárában hozzon létre egy új fájlt **tasklist.js**. 
2. Adja hozzá a következő kód túl hello**tasklist.js**. Ez betölti a hello DocumentDBClient és async modult, amely által használt **tasklist.js**. Ez is definiálva hello **TaskList** függvény, amelyet hello példányának **feladat** objektum korábban meghatározott:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. Hozzáadja a toohello **tasklist.js** fájl túl használt hello metódusok hozzáadásával**showTasks, Addtasks**, és **completeTasks**:
   
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
   
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
   
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
   
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
   
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
   
                    res.redirect('/');
                });
            },
   
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
   
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };
4. Mentse és zárja be a hello **tasklist.js** fájlt.

### <a name="add-configjs"></a>A config.js fájl hozzáadása
1. A projektkönyvtárban hozzon létre egy új fájlt **config.js** néven.
2. Adja hozzá a hello túl a következő**config.js**. Ez meghatározza az alkalmazáshoz szükséges konfigurációs beállításokat és értékeket.
   
        var config = {}
   
        config.host = process.env.HOST || "[hello URI value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[hello PRIMARY KEY value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. A hello **config.js** fájl, a frissítés hello értékei HOST és AUTH_KEY hello (kulcsok) panelén hello Azure Cosmos DB fiókjában található hello értékek [Microsoft Azure-portálon](https://portal.azure.com).
4. Mentse és zárja be a hello **config.js** fájlt.

### <a name="modify-appjs"></a>Az app.js fájl módosítása
1. Hello projekt könyvtárában nyissa meg hello **app.js** fájlt. Ez a fájl korábban létrejött hello Express-webalkalmazás létrehozásakor.
2. Adja hozzá a következő kód toohello felső részén hello **app.js**
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. Ez a kód hello konfigurációs fájl toobe használt határozza meg, és tooread értékeket abból néhány változó, amelyeket hamarosan használni fog a eltérő lehet.
4. Cserélje le az alábbi két hello **app.js** fájlt:
   
        app.use('/', index);
        app.use('/users', users); 
   
      a következő kódrészletet hello:
   
        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');
5. Ezek a sorok meghatározzák egy új példányt a **TaskDao** objektum, egy új kapcsolat tooAzure Cosmos DB (hello hello értékekkel olvasni **config.js**) hello feladat objektum inicializálása., majd társítanak a toomethods a **TaskList** vezérlő. 
6. Végül mentse és zárja be a hello **app.js** fájlt, hogy szinte végzett.

## <a name="_Toc395783181"></a>5. lépés: Felhasználói felület létrehozása
Most adjuk a figyelmet toobuilding hello felhasználói felületét, így a felhasználók ténylegesen használatba vehessék az alkalmazást. hello használ létrehozott Express-alkalmazás **Jade** , hello megjelenítési motort. További információ a Jade tekintse meg túl[http://jade-lang.com/](http://jade-lang.com/).

1. Hello **Views** hello fájlban **nézetek** directory globális sablonként szolgál az egyéb **.jade** fájlokat. Ebben a lépésben lesz a módosítás toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), amelyen egy eszközkészlet, így könnyen toodesign egy töltött tetszetős webhelyeket. 
2. Nyissa meg hello **Views** fájl található a hello **nézetek** hello következőre mappa és a név felülírandó hello tartalma:

    ```
    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body
        nav.navbar.navbar-inverse.navbar-fixed-top
          div.navbar-header
            a.navbar-brand(href='#') My Tasks
        block content
        script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
        script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
    ```

    Ez gyakorlatilag megmondja hello **Jade** motor toorender az alkalmazás HTML-kódot, és létrehoz egy **blokk** nevű **tartalom** ahol megadhatja hello elrendezés a tartalomhoz lapok.

    Mentse és zárja be a **layout.jade** fájlt.

3. Most nyissa meg a hello **index.jade** fájlt, az alkalmazás által használható, és cserélje ki hello fájl tartalma hello hello következőre hello megtekintése:
   
        extends layout
        block content
           h1 #{title}
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
                     td #{task.name}
                     td #{task.category}
                     - var date  = new Date(task.date);
                     - var day   = date.getDate();
                     - var month = date.getMonth() + 1;
                     - var year  = date.getFullYear();
                     td #{month + "/" + day + "/" + year}
                     td
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn.btn-primary(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             .form-group
               label(for="name") Item Name:
               input.form-control(name="name", type="textbox")
             .form-group
               label(for="category") Item Category:
               input.form-control(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   

Ez kibővíti az elrendezést, és tartalmat biztosít hello **tartalom** imént látott hello helyőrző **Views** korábbi fájlt.
   
Ebben az elrendezésben két HTML-űrlapot hoztunk létre.

hello első űrlap az adatok és a gomb használatával kérdezhetjük túl elhelyezésével tooupdate elemek táblát tartalmaz**/completetask** metódusának.
    
hello második űrlap két beviteli mezőt és egy gombot, amely lehetővé teszi új elem toocreate elhelyezésével túl tartalmaz**/addtask** metódusának.

Csak az alkalmazás toowork szükséges.

## <a name="_Toc395783181"></a>6. lépés: Az alkalmazás helyileg történő futtatása
1. tootest hello alkalmazás a helyi számítógépen futni `npm start` a hello terminál toostart az alkalmazást, majd frissítse a [http://localhost: 3000](http://localhost:3000) webböngészőben. hello lap most hello az alábbi képen hasonlóan kell kinéznie:
   
    ![Képernyőfelvétel a hello My ToDo List alkalmazásról egy böngészőablakban](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > Ha hello Views vagy hello index.jade fájl hello francia kapcsolatos hibaüzenetet kap, akkor győződjön meg arról, hogy mindkét fájl első két sora hello bal oldali indokolt, szóközök nélkül. Ha hello első két sor elé, távolítsa el őket, mentse a fájlt, majd frissítse a böngészőt. 

2. Hello elemet, az elem neve és a kategória mező tooenter egy új feladatot használja, és kattintson a **elem hozzáadása**. Ez egy új dokumentumot hoz létre az Azure Cosmos DB-ben a megadott tulajdonságokkal. 
3. hello oldal ekkor frissül, az újonnan létrehozott elemet a teendőlistában hello toodisplay hello.
   
    ![Képernyőfelvétel a hello alkalmazás hello teendőlista új eleméről](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. toocomplete egy feladatot, egyszerűen jelölje be a hello teljes oszlopban hello jelölőnégyzetet, és kattintson a **feladatok frissítése**. Ezzel frissíti a már létrehozott hello dokumentum.

5. toostop hello alkalmazás, nyomja le a CTRL + C hello terminálablakot, és kattintson a **Y** tooterminate hello kötegelt.

## <a name="_Toc395783182"></a>7. lépés: Az alkalmazás fejlesztési projekt tooAzure webhelyek központi telepítése
1. Ha még nem tette meg, engedélyezzen egy Git-tárházat az Azure Websites számára. Hogyan találhat útmutatót toodo ezt a hello [helyi Git-telepítésének tooAzure App Service](../app-service-web/app-service-deploy-local-git.md) témakör.
2. Adja hozzá Azure-webhelyét távoli Git-elemként.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Telepítsen toohello távoli küldésével.
   
        git push azure master
4. Néhány másodpercen belül git befejezi a webalkalmazás közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!

    Gratulálunk! Ebben az esetben az első Node.js Express-webalkalmazását Azure Cosmos DB használatával építve, és közzétette azt tooAzure webhelyek.

    Ha szeretné, hogy toodownload, vagy tekintse meg a teljes referenciaalkalmazás toohello ehhez az oktatóanyaghoz, le is tölthetők: [GitHub][GitHub].

## <a name="_Toc395637775"></a>Következő lépések

* Szeretné, hogy tooperform méretezés és teljesítmény Azure Cosmos DB tesztelték? Tekintse meg a következőt: [Teljesítmény- és mérettesztelés az Azure Cosmos DB használatával](performance-testing.md)
* Ismerje meg, hogyan túl[figyelése Azure Cosmos DB fiók](monitor-accounts.md).
* A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).
* Fedezze fel hello [Azure Cosmos DB dokumentáció](https://docs.microsoft.com/azure/documentdb/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

