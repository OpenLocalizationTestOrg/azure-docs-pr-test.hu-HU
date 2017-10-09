---
title: "a KÖZÉPÉRTÉK aaaCreate verem a Linux virtuális gép az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate a MongoDB, a Express, az AngularJS és a Node.js (átlag) verem a Linux virtuális gép az Azure-ban."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 82a8e34e60d2bb6e6670ee007faa1113ea78b716
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a>A MongoDB, az Express, az AngularJS és a Node.js (átlag) verem létrehozni a Linux virtuális gép az Azure-ban

Az oktatóanyag bemutatja, hogyan tooimplement a MongoDB, a Express, az AngularJS és a Node.js (átlag) verem a Linux virtuális gép az Azure-ban. az Ön által létrehozott hello átlagos verem lehetővé teszi, hogy hozzáadásával, törlésével és egy adatbázisban könyvek listázása. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Linux rendszerű virtuális gép létrehozása
> * A Node.js telepítése
> * A MongoDB telepítése és hello kiszolgáló beállítása
> * Express telepítése és útvonalak toohello kiszolgáló beállítása
> * Az AngularJS hello útvonalakat
> * Hello alkalmazás futtatása

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).


## <a name="create-a-linux-vm"></a>Linux rendszerű virtuális gép létrehozása

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot, és hozzon létre egy Linux virtuális gép hello [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.

hello alábbi példában egy erőforráscsoport nevű hello Azure CLI toocreate *myResourceGroupMEAN* a hello *eastus* helyét. A virtuális gép kerül létrehozásra elnevezett *myVM* az SSH-kulcsok, ha még nem léteznek a kulcs alapértelmezett helye. toouse egy adott kulcsok használata hello--ssh-kulcs-érték lehetőséget állítsa be.

```azurecli-interactive
az group create --name myResourceGroupMEAN --location eastus
az vm create \
    --resource-group myResourceGroupMEAN \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password 'Azure12345678!' \
    --generate-ssh-keys
az vm open-port --port 3300 --resource-group myResourceGroupMEAN --name myVM
```

Hello virtuális gép létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg: 

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroupMEAN/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.72.77.9",
  "resourceGroup": "myResourceGroupMEAN"
}
```
Jegyezze fel a hello `publicIpAddress`. Ez a cím használt tooaccess hello virtuális gép.

Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet. Ellenőrizze, hogy toouse hello megfelelő nyilvános IP-címet. Ebben a példában a fenti az IP cím volt 13.72.77.9.

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a>A Node.js telepítése

[NODE.js](https://nodejs.org/en/) a JavaScript futásidejű Chrome tartozó V8 JavaScript motor épül. NODE.js Express irányítja a hello és AngularJS tartományvezérlők ezen oktatóanyag tooset használatban van.

A virtuális gépet az SSH-megnyitott hello rendszerhéjakba hello telepítse a Node.js.

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-hello-server"></a>A MongoDB telepítése és hello kiszolgáló beállítása
[MongoDB](http://www.mongodb.com) rugalmas, JSON-jellegű dokumentumok tárolja az adatokat. Az adatbázis mezőit dokumentum toodocument eltérhet, és adatszerkezet időbeli módosíthatók. Például az alkalmazás azt hozzá könyv rekordok tooMongoDB, amelyek tartalmazzák a neve, isbn számát, Szerző és lapok száma. 

1. A virtuális gépet az SSH-megnyitott hello rendszerhéjakba hello hello MongoDB kulcs beállítása.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. Frissítse a hello Csomagkezelő hello kulcs.
  
    ```bash
    sudo apt-get update
    ```

3. A MongoDB telepítése.

    ```bash
    sudo apt-get install -y mongodb
    ```

4. Hello kiszolgáló elindítására.

    ```bash
    sudo service mongodb start
    ```

5. Igazolnia kell tooinstall hello [törzs-elemző](https://www.npmjs.com/package/body-parser-json) csomag toohelp velünk JSON átadott kérelmek toohello server hello feldolgozni.

    Hello npm package manager telepítéséhez.

    ```bash
    sudo apt-get install npm
    ```

    Hello törzs elemző telepítéséhez.
    
    ```bash
    sudo npm install body-parser
    ```

6. Hozza létre a *könyvek* , és adja hozzá a fájl tooit nevű *server.js* , amely hello webkiszolgáló hello konfigurációját tartalmazza.

    ```node.js
    var express = require('express');
    var bodyParser = require('body-parser');
    var app = express();
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    require('./apps/routes')(app);
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
        console.log('Server up: http://localhost:' + app.get('port'));
    });
    ```

## <a name="install-express-and-set-up-routes-toohello-server"></a>Express telepítése és útvonalak toohello kiszolgáló beállítása

[Express](https://expressjs.com) van egy minimális igényű és rugalmas Node.js webes alkalmazás-keretrendszer, amely funkciókat biztosít a webes és mobilalkalmazásokhoz. Express ezen oktatóanyag toopass könyv információk tooand a MongoDB adatbázis használatban van. [Mongoose](http://mongoosejs.com) egy egyszerű feladat, a séma-alapú megoldás toomodel biztosít az alkalmazások adatait. Mongoose szerepel ez az oktatóanyag tooprovide hello adatbázis könyv sémát.

1. Telepítse az Express és a mongoose bővítményt.

    ```bash
    sudo npm install express mongoose
    ```

2. A hello *könyvek* mappa, hozza létre a *alkalmazások* , és adja hozzá a nevű fájl *routes.js* hello az express definiált útvonalak.

    ```node.js
    var Book = require('./models/book');
    module.exports = function(app) {
      app.get('/book', function(req, res) {
        Book.find({}, function(err, result) {
          if ( err ) throw err;
          res.json(result);
        });
      }); 
      app.post('/book', function(req, res) {
        var book = new Book( {
          name:req.body.name,
          isbn:req.body.isbn,
          author:req.body.author,
          pages:req.body.pages
        });
        book.save(function(err, result) {
          if ( err ) throw err;
          res.json( {
            message:"Successfully added book",
            book:result
          });
        });
      });
      app.delete("/book/:isbn", function(req, res) {
        Book.findOneAndRemove(req.query, function(err, result) {
          if ( err ) throw err;
          res.json( {
            message: "Successfully deleted hello book",
            book: result
          });
        });
      });
      var path = require('path');
      app.get('*', function(req, res) {
        res.sendfile(path.join(__dirname + '/public', 'index.html'));
      });
    };
    ```

3. A hello *alkalmazások* mappa, hozza létre a *modellek* , és adja hozzá a nevű fájl *book.js* definiált hello könyv modell beállítások használatával.  

    ```node.js
    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);
    var bookSchema = mongoose.Schema( {
      name: String,
      isbn: {type: String, index: true},
      author: String,
      pages: Number
    });
    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema); 
    ```

## <a name="access-hello-routes-with-angularjs"></a>Az AngularJS hello útvonalakat

[AngularJS](https://angularjs.org) webes keretet biztosít a dinamikus nézetek létrehozása a webes alkalmazások. Ebben az oktatóanyagban a weblap AngularJS tooconnect felhasználása expressz és műveleteket a könyv adatbázison.

1. Hello directory módosítsa biztonsági mentése túl*könyvek* (`cd ../..`), és ezután hozza létre a *nyilvános* és nevű fájl hozzáadása *script.js* hello vezérlővel definiált beállítások.

    ```node.js
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http( {
        method: 'GET',
        url: '/book'
      }).then(function successCallback(response) {
        $scope.books = response.data;
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
      $scope.del_book = function(book) {
        $http( {
          method: 'DELETE',
          url: '/book/:isbn',
          params: {'isbn': book.isbn}
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
      $scope.add_book = function() {
        var body = '{ "name": "' + $scope.Name + 
        '", "isbn": "' + $scope.Isbn +
        '", "author": "' + $scope.Author + 
        '", "pages": "' + $scope.Pages + '" }';
        $http({
          method: 'POST',
          url: '/book',
          data: body
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
    });
    ```
    
2. A hello *nyilvános* mappa, hozzon létre egy fájlt *index.html* definiált hello weblapon.

    ```html
    <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td> 
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td> 
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>
            </tr>
            <tr ng-repeat="book in books">
              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

##  <a name="run-hello-application"></a>Hello alkalmazás futtatása

1. Módosítsa biztonsági mentése túl hello könyvtárat*könyvek* (`cd ..`) és hello kiszolgáló indítása a következő parancs futtatásával:

    ```bash
    nodejs server.js
    ```

2. Nyisson meg egy webes böngésző toohello címet, a virtuális gép hello feljegyzett. Például *http://13.72.77.9:3300*. A következő lap hello hasonlót kell megjelennie:

    ![Rendszertöltő rekord](media/tutorial-mean/meanstack-init.png)

3. Hello szövegmezők meg adatot, és kattintson a **Hozzáadás**. Példa:

    ![Rendszertöltő rekord hozzáadása](media/tutorial-mean/meanstack-add.png)

4. Hello lap frissítésével, megtekintheti az ezen a lapon hasonlót:

    ![Lista könyv rekordok](media/tutorial-mean/meanstack-list.png)

5. Sikerült kattintson **törlése** , és távolítsa el a hello rendszertöltő rekord hello adatbázisból.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy webes alkalmazás, amely nyomon követi a rekordok könyv egy létrehozott egy Linux virtuális Gépet a verem. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Linux rendszerű virtuális gép létrehozása
> * A Node.js telepítése
> * A MongoDB telepítése és hello kiszolgáló beállítása
> * Express telepítése és útvonalak toohello kiszolgáló beállítása
> * Az AngularJS hello útvonalakat
> * Hello alkalmazás futtatása

Hogyan előzetes toohello következő útmutató toolearn toosecure webkiszolgálók SSL-tanúsítványokkal.

> [!div class="nextstepaction"]
> [Biztonságos webkiszolgáló SSL](tutorial-secure-web-server.md)
