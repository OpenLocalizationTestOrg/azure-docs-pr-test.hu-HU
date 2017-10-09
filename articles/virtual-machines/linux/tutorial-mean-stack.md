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
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a><span data-ttu-id="973a1-103">A MongoDB, az Express, az AngularJS és a Node.js (átlag) verem létrehozni a Linux virtuális gép az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="973a1-103">Create a MongoDB, Express, AngularJS, and Node.js (MEAN) stack on a Linux VM in Azure</span></span>

<span data-ttu-id="973a1-104">Az oktatóanyag bemutatja, hogyan tooimplement a MongoDB, a Express, az AngularJS és a Node.js (átlag) verem a Linux virtuális gép az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="973a1-104">This tutorial shows you how tooimplement a MongoDB, Express, AngularJS, and Node.js (MEAN) stack on a Linux VM in Azure.</span></span> <span data-ttu-id="973a1-105">az Ön által létrehozott hello átlagos verem lehetővé teszi, hogy hozzáadásával, törlésével és egy adatbázisban könyvek listázása.</span><span class="sxs-lookup"><span data-stu-id="973a1-105">hello MEAN stack that you create enables adding, deleting, and listing books in a database.</span></span> <span data-ttu-id="973a1-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="973a1-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="973a1-107">Linux rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="973a1-107">Create a Linux VM</span></span>
> * <span data-ttu-id="973a1-108">A Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="973a1-108">Install Node.js</span></span>
> * <span data-ttu-id="973a1-109">A MongoDB telepítése és hello kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="973a1-109">Install MongoDB and set up hello server</span></span>
> * <span data-ttu-id="973a1-110">Express telepítése és útvonalak toohello kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="973a1-110">Install Express and set up routes toohello server</span></span>
> * <span data-ttu-id="973a1-111">Az AngularJS hello útvonalakat</span><span class="sxs-lookup"><span data-stu-id="973a1-111">Access hello routes with AngularJS</span></span>
> * <span data-ttu-id="973a1-112">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="973a1-112">Run hello application</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="973a1-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="973a1-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="973a1-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="973a1-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="973a1-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="973a1-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>


## <a name="create-a-linux-vm"></a><span data-ttu-id="973a1-116">Linux rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="973a1-116">Create a Linux VM</span></span>

<span data-ttu-id="973a1-117">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot, és hozzon létre egy Linux virtuális gép hello [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="973a1-117">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command and create a Linux VM with hello [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> <span data-ttu-id="973a1-118">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="973a1-118">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="973a1-119">hello alábbi példában egy erőforráscsoport nevű hello Azure CLI toocreate *myResourceGroupMEAN* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="973a1-119">hello following example uses hello Azure CLI toocreate a resource group named *myResourceGroupMEAN* in hello *eastus* location.</span></span> <span data-ttu-id="973a1-120">A virtuális gép kerül létrehozásra elnevezett *myVM* az SSH-kulcsok, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="973a1-120">A VM is created named *myVM* with SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="973a1-121">toouse egy adott kulcsok használata hello--ssh-kulcs-érték lehetőséget állítsa be.</span><span class="sxs-lookup"><span data-stu-id="973a1-121">toouse a specific set of keys, use hello --ssh-key-value option.</span></span>

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

<span data-ttu-id="973a1-122">Hello virtuális gép létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="973a1-122">When hello VM has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

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
<span data-ttu-id="973a1-123">Jegyezze fel a hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="973a1-123">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="973a1-124">Ez a cím használt tooaccess hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="973a1-124">This address is used tooaccess hello VM.</span></span>

<span data-ttu-id="973a1-125">Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="973a1-125">Use hello following command toocreate an SSH session with hello VM.</span></span> <span data-ttu-id="973a1-126">Ellenőrizze, hogy toouse hello megfelelő nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="973a1-126">Make sure toouse hello correct public IP address.</span></span> <span data-ttu-id="973a1-127">Ebben a példában a fenti az IP cím volt 13.72.77.9.</span><span class="sxs-lookup"><span data-stu-id="973a1-127">In our example above our IP address was 13.72.77.9.</span></span>

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a><span data-ttu-id="973a1-128">A Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="973a1-128">Install Node.js</span></span>

<span data-ttu-id="973a1-129">[NODE.js](https://nodejs.org/en/) a JavaScript futásidejű Chrome tartozó V8 JavaScript motor épül.</span><span class="sxs-lookup"><span data-stu-id="973a1-129">[Node.js](https://nodejs.org/en/) is a JavaScript runtime built on Chrome's V8 JavaScript engine.</span></span> <span data-ttu-id="973a1-130">NODE.js Express irányítja a hello és AngularJS tartományvezérlők ezen oktatóanyag tooset használatban van.</span><span class="sxs-lookup"><span data-stu-id="973a1-130">Node.js is used in this tutorial tooset up hello Express routes and AngularJS controllers.</span></span>

<span data-ttu-id="973a1-131">A virtuális gépet az SSH-megnyitott hello rendszerhéjakba hello telepítse a Node.js.</span><span class="sxs-lookup"><span data-stu-id="973a1-131">On hello VM, using hello bash shell that you opened with SSH, install Node.js.</span></span>

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-hello-server"></a><span data-ttu-id="973a1-132">A MongoDB telepítése és hello kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="973a1-132">Install MongoDB and set up hello server</span></span>
<span data-ttu-id="973a1-133">[MongoDB](http://www.mongodb.com) rugalmas, JSON-jellegű dokumentumok tárolja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="973a1-133">[MongoDB](http://www.mongodb.com) stores data in flexible, JSON-like documents.</span></span> <span data-ttu-id="973a1-134">Az adatbázis mezőit dokumentum toodocument eltérhet, és adatszerkezet időbeli módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="973a1-134">Fields in a database can vary from document toodocument and data structure can be changed over time.</span></span> <span data-ttu-id="973a1-135">Például az alkalmazás azt hozzá könyv rekordok tooMongoDB, amelyek tartalmazzák a neve, isbn számát, Szerző és lapok száma.</span><span class="sxs-lookup"><span data-stu-id="973a1-135">For our example application, we are adding book records tooMongoDB that contain book name, isbn number, author, and number of pages.</span></span> 

1. <span data-ttu-id="973a1-136">A virtuális gépet az SSH-megnyitott hello rendszerhéjakba hello hello MongoDB kulcs beállítása.</span><span class="sxs-lookup"><span data-stu-id="973a1-136">On hello VM, using hello bash shell that you opened with SSH, set hello MongoDB key.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. <span data-ttu-id="973a1-137">Frissítse a hello Csomagkezelő hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="973a1-137">Update hello package manager with hello key.</span></span>
  
    ```bash
    sudo apt-get update
    ```

3. <span data-ttu-id="973a1-138">A MongoDB telepítése.</span><span class="sxs-lookup"><span data-stu-id="973a1-138">Install MongoDB.</span></span>

    ```bash
    sudo apt-get install -y mongodb
    ```

4. <span data-ttu-id="973a1-139">Hello kiszolgáló elindítására.</span><span class="sxs-lookup"><span data-stu-id="973a1-139">Start hello server.</span></span>

    ```bash
    sudo service mongodb start
    ```

5. <span data-ttu-id="973a1-140">Igazolnia kell tooinstall hello [törzs-elemző](https://www.npmjs.com/package/body-parser-json) csomag toohelp velünk JSON átadott kérelmek toohello server hello feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="973a1-140">We also need tooinstall hello [body-parser](https://www.npmjs.com/package/body-parser-json) package toohelp us process hello JSON passed in requests toohello server.</span></span>

    <span data-ttu-id="973a1-141">Hello npm package manager telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="973a1-141">Install hello npm package manager.</span></span>

    ```bash
    sudo apt-get install npm
    ```

    <span data-ttu-id="973a1-142">Hello törzs elemző telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="973a1-142">Install hello body parser package.</span></span>
    
    ```bash
    sudo npm install body-parser
    ```

6. <span data-ttu-id="973a1-143">Hozza létre a *könyvek* , és adja hozzá a fájl tooit nevű *server.js* , amely hello webkiszolgáló hello konfigurációját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="973a1-143">Create a folder named *Books* and add a file tooit named *server.js* that contains hello configuration for hello web server.</span></span>

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

## <a name="install-express-and-set-up-routes-toohello-server"></a><span data-ttu-id="973a1-144">Express telepítése és útvonalak toohello kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="973a1-144">Install Express and set up routes toohello server</span></span>

<span data-ttu-id="973a1-145">[Express](https://expressjs.com) van egy minimális igényű és rugalmas Node.js webes alkalmazás-keretrendszer, amely funkciókat biztosít a webes és mobilalkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="973a1-145">[Express](https://expressjs.com) is a minimal and flexible Node.js web application framework that provides features for web and mobile applications.</span></span> <span data-ttu-id="973a1-146">Express ezen oktatóanyag toopass könyv információk tooand a MongoDB adatbázis használatban van.</span><span class="sxs-lookup"><span data-stu-id="973a1-146">Express is used in this tutorial toopass book information tooand from our MongoDB database.</span></span> <span data-ttu-id="973a1-147">[Mongoose](http://mongoosejs.com) egy egyszerű feladat, a séma-alapú megoldás toomodel biztosít az alkalmazások adatait.</span><span class="sxs-lookup"><span data-stu-id="973a1-147">[Mongoose](http://mongoosejs.com) provides a straight-forward, schema-based solution toomodel your application data.</span></span> <span data-ttu-id="973a1-148">Mongoose szerepel ez az oktatóanyag tooprovide hello adatbázis könyv sémát.</span><span class="sxs-lookup"><span data-stu-id="973a1-148">Mongoose is used in this tutorial tooprovide a book schema for hello database.</span></span>

1. <span data-ttu-id="973a1-149">Telepítse az Express és a mongoose bővítményt.</span><span class="sxs-lookup"><span data-stu-id="973a1-149">Install Express and Mongoose.</span></span>

    ```bash
    sudo npm install express mongoose
    ```

2. <span data-ttu-id="973a1-150">A hello *könyvek* mappa, hozza létre a *alkalmazások* , és adja hozzá a nevű fájl *routes.js* hello az express definiált útvonalak.</span><span class="sxs-lookup"><span data-stu-id="973a1-150">In hello *Books* folder, create a folder named *apps* and add a file named *routes.js* with hello express routes defined.</span></span>

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

3. <span data-ttu-id="973a1-151">A hello *alkalmazások* mappa, hozza létre a *modellek* , és adja hozzá a nevű fájl *book.js* definiált hello könyv modell beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="973a1-151">In hello *apps* folder, create a folder named *models* and add a file named *book.js* with hello book model configuration defined.</span></span>  

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

## <a name="access-hello-routes-with-angularjs"></a><span data-ttu-id="973a1-152">Az AngularJS hello útvonalakat</span><span class="sxs-lookup"><span data-stu-id="973a1-152">Access hello routes with AngularJS</span></span>

<span data-ttu-id="973a1-153">[AngularJS](https://angularjs.org) webes keretet biztosít a dinamikus nézetek létrehozása a webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="973a1-153">[AngularJS](https://angularjs.org) provides a web framework for creating dynamic views in your web applications.</span></span> <span data-ttu-id="973a1-154">Ebben az oktatóanyagban a weblap AngularJS tooconnect felhasználása expressz és műveleteket a könyv adatbázison.</span><span class="sxs-lookup"><span data-stu-id="973a1-154">In this tutorial, we use AngularJS tooconnect our web page with Express and perform actions on our book database.</span></span>

1. <span data-ttu-id="973a1-155">Hello directory módosítsa biztonsági mentése túl*könyvek* (`cd ../..`), és ezután hozza létre a *nyilvános* és nevű fájl hozzáadása *script.js* hello vezérlővel definiált beállítások.</span><span class="sxs-lookup"><span data-stu-id="973a1-155">Change hello directory back up too*Books* (`cd ../..`), and then create a folder named *public* and add a file named *script.js* with hello controller configuration defined.</span></span>

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
    
2. <span data-ttu-id="973a1-156">A hello *nyilvános* mappa, hozzon létre egy fájlt *index.html* definiált hello weblapon.</span><span class="sxs-lookup"><span data-stu-id="973a1-156">In hello *public* folder, create a file named *index.html* with hello web page defined.</span></span>

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

##  <a name="run-hello-application"></a><span data-ttu-id="973a1-157">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="973a1-157">Run hello application</span></span>

1. <span data-ttu-id="973a1-158">Módosítsa biztonsági mentése túl hello könyvtárat*könyvek* (`cd ..`) és hello kiszolgáló indítása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="973a1-158">Change hello directory back up too*Books* (`cd ..`) and start hello server by running this command:</span></span>

    ```bash
    nodejs server.js
    ```

2. <span data-ttu-id="973a1-159">Nyisson meg egy webes böngésző toohello címet, a virtuális gép hello feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="973a1-159">Open a web browser toohello address that you recorded for hello VM.</span></span> <span data-ttu-id="973a1-160">Például *http://13.72.77.9:3300*.</span><span class="sxs-lookup"><span data-stu-id="973a1-160">For example, *http://13.72.77.9:3300*.</span></span> <span data-ttu-id="973a1-161">A következő lap hello hasonlót kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="973a1-161">You should see something like hello following page:</span></span>

    ![Rendszertöltő rekord](media/tutorial-mean/meanstack-init.png)

3. <span data-ttu-id="973a1-163">Hello szövegmezők meg adatot, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="973a1-163">Enter data into hello textboxes and click **Add**.</span></span> <span data-ttu-id="973a1-164">Példa:</span><span class="sxs-lookup"><span data-stu-id="973a1-164">For example:</span></span>

    ![Rendszertöltő rekord hozzáadása](media/tutorial-mean/meanstack-add.png)

4. <span data-ttu-id="973a1-166">Hello lap frissítésével, megtekintheti az ezen a lapon hasonlót:</span><span class="sxs-lookup"><span data-stu-id="973a1-166">After refreshing hello page, you should see something like this page:</span></span>

    ![Lista könyv rekordok](media/tutorial-mean/meanstack-list.png)

5. <span data-ttu-id="973a1-168">Sikerült kattintson **törlése** , és távolítsa el a hello rendszertöltő rekord hello adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="973a1-168">You could click **Delete** and remove hello book record from hello database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="973a1-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="973a1-169">Next steps</span></span>

<span data-ttu-id="973a1-170">Ebben az oktatóanyagban egy webes alkalmazás, amely nyomon követi a rekordok könyv egy létrehozott egy Linux virtuális Gépet a verem.</span><span class="sxs-lookup"><span data-stu-id="973a1-170">In this tutorial, you created a web application that keeps track of book records using a MEAN stack on a Linux VM.</span></span> <span data-ttu-id="973a1-171">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="973a1-171">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="973a1-172">Linux rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="973a1-172">Create a Linux VM</span></span>
> * <span data-ttu-id="973a1-173">A Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="973a1-173">Install Node.js</span></span>
> * <span data-ttu-id="973a1-174">A MongoDB telepítése és hello kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="973a1-174">Install MongoDB and set up hello server</span></span>
> * <span data-ttu-id="973a1-175">Express telepítése és útvonalak toohello kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="973a1-175">Install Express and set up routes toohello server</span></span>
> * <span data-ttu-id="973a1-176">Az AngularJS hello útvonalakat</span><span class="sxs-lookup"><span data-stu-id="973a1-176">Access hello routes with AngularJS</span></span>
> * <span data-ttu-id="973a1-177">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="973a1-177">Run hello application</span></span>

<span data-ttu-id="973a1-178">Hogyan előzetes toohello következő útmutató toolearn toosecure webkiszolgálók SSL-tanúsítványokkal.</span><span class="sxs-lookup"><span data-stu-id="973a1-178">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="973a1-179">Biztonságos webkiszolgáló SSL</span><span class="sxs-lookup"><span data-stu-id="973a1-179">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)
