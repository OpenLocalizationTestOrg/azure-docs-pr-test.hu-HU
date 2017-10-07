---
title: "a Node.js és a MongoDB webalkalmazás az Azure-ban aaaBuild |} Microsoft Docs"
description: "Megtudhatja, hogyan tooget egy Node.js-alkalmazás az Azure-ban működik, a kapcsolat tooa Cosmos DB adatbázis MongoDB kapcsolati karakterláncot."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 532251c51ed6f8513e6e366393e889b67a85e5b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a>Az Azure-ban Node.js és a MongoDB webalkalmazás létrehozása

Az Azure Web Apps jól skálázható, önálló javítási a webhelyszolgáltató biztosít. Ez az oktatóanyag bemutatja, hogyan toocreate egy Node.js webalkalmazás az Azure-ban, és csatlakoztassa tooa MongoDB-adatbázist. Amikor elkészült, konfigurálnia kell egy átlagos alkalmazás (MongoDB, Express, AngularJS és Node.js) fut a [Azure App Service](app-service-web-overview.md). Az egyszerűség kedvéért hello mintaalkalmazást használ hello [MEAN.js webes keretrendszer](http://meanjs.org/).

![Az Azure App Service-ben futó MEAN.js alkalmazás](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Ismertetett témák:

> [!div class="checklist"]
> * MongoDB-adatbázis létrehozása az Azure-ban
> * Csatlakozás egy Node.js-alkalmazás tooMongoDB
> * Hello app tooAzure telepítése
> * Hello adatmodell frissítése, és telepítse újra hello alkalmazást
> * Az adatfolyam diagnosztikai naplók az Azure-ból
> * Az Azure-portálon hello hello alkalmazás kezelése

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

1. [A Git telepítése](https://git-scm.com/)
1. [Telepítse a Node.js-t és az NPM-et](https://nodejs.org/)
1. [Telepítse a Gulp.js](http://gulpjs.com/) (által megkövetelt [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))
1. [Telepítheti és futtathatja a MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="test-local-mongodb"></a>Teszt helyi mongodb-Protokolltámogatással

Nyissa meg hello terminálablakot, és `cd` toohello `bin` a MongoDB telepítési könyvtárába. A terminálablakot toorun összes hello parancsokat használhatja az oktatóanyag.

Futtatás `mongo` a hello terminál tooconnect tooyour helyi MongoDB-kiszolgálóhoz.

```bash
mongo
```

Ha a kapcsolódás sikeres, majd a MongoDB adatbázis már fut. Ha nem, győződjön meg arról, hogy a helyi MongoDB adatbázis indította a következő hello készítésével [MongoDB Community Edition telepítése](https://docs.mongodb.com/manual/administration/install-community/). Gyakran, MongoDB telepítve van, de továbbra is szükséges toostart azt futtatásával `mongod`. 

Ha elkészült a MongoDB adatbázis tesztelése, írja be a `Ctrl+C` a Terminálszolgáltatások hello. 

## <a name="create-local-nodejs-app"></a>Helyi Node.js-alkalmazás létrehozása

Ebben a lépésben beállíthatja hello helyi Node.js-projektet.

### <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

A hello terminálablakot `cd` tooa munkakönyvtárát.  

Futtassa a következő parancs tooclone hello minta tárház hello. 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

Ez a minta-tárház hello másolatát tartalmazza [MEAN.js tárház](https://github.com/meanjs/mean). Az App Service módosított toorun (további információkért lásd: hello MEAN.js tárház [információs fájl](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).

### <a name="run-hello-application"></a>Hello alkalmazás futtatása

Futtassa a következő parancsok tooinstall szükséges hello csomagok hello és hello alkalmazás indításához.

```bash
cd meanjs
npm install
npm start
```

Amikor hello alkalmazás teljesen betöltődik, valami hasonló toohello a következő üzenet jelenik meg:

```
--
MEAN.JS - Development Environment

Environment:     development
Server:          http://0.0.0.0:3000
Database:        mongodb://localhost/mean-dev
App version:     0.5.0
MEAN.JS version: 0.5.0
--
```

Keresse meg a böngészőben toohttp://localhost:3000. Kattintson a **regisztráció** a felső menüben hello és tesztfelhasználó létrehozása. 

hello MEAN.js mintaalkalmazás felhasználói adatok hello adatbázisban tárolja. Ha a felhasználó létrehozásának és a bejelentkezés sikeres, majd az alkalmazás írása az adatok toohello helyi MongoDB-adatbázist.

![MEAN.js sikeresen csatlakozik tooMongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

Válassza ki **Admin > kezelése cikkek** tooadd egyes cikkeket.

toostop Node.js bármikor, nyomja le az ENTER `Ctrl+C` a Terminálszolgáltatások hello. 

## <a name="create-production-mongodb"></a>Éles MongoDB létrehozása

Ebben a lépésben az Azure-ban MongoDB adatbázis létrehozása. Ha az alkalmazás telepített tooAzure, használja a felhő adatbázis.

Ez az oktatóanyag használja a MongoDB, [Azure Cosmos DB](/azure/documentdb/). Cosmos DB támogatja a MongoDB-ügyfélkapcsolatokat.

### <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Az Azure-ban hello Azure CLI 2.0 toocreate hello szükséges erőforrásokat toohost az alkalmazás fogja használni. Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

hello alábbi példa létrehoz egy erőforráscsoport hello Nyugat-Európában régióban.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Használjon hello [az App Service lista-helyek](/cli/azure/appservice#list-locations) Azure CLI parancssori toolist elérhető helyről. 

### <a name="create-a-cosmos-db-account"></a>A Cosmos DB-fiók létrehozása

Hozzon létre egy Cosmos DB fiókot hello [az cosmosdb létrehozása](/cli/azure/cosmosdb#create) parancsot.

Helyettesítse hello Cosmos DB egyedi nevet a következő parancsot, a hello  *\<cosmosdb_name >* helyőrző. Ez a név hello Cosmos DB végpont, hello részeként használatos `https://<cosmosdb_name>.documents.azure.com/`, így a hello nevének kell toobe egyedi összes Cosmos DB fiók az Azure-ban. hello neve csak kisbetűket, számokat és hello kötőjel (-) karaktert kell tartalmaznia, és 3 – 50 karakter közé kell esnie.

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

Hello *--kind MongoDB* paraméter lehetővé teszi, hogy a MongoDB-ügyfélkapcsolatokat.

Hello Cosmos DB fiók létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json
{
  "consistencyPolicy":
  {
    "defaultConsistencyLevel": "Session",
    "maxIntervalInSeconds": 5,
    "maxStalenessPrefix": 100
  },
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.com:443/",
  "failoverPolicies": 
  ...
  < Output truncated for readability >
}
```

## <a name="connect-app-tooproduction-mongodb"></a>Kapcsolódás app tooproduction mongodb-Protokolltámogatással

Ebben a lépésben csatlakoztatja a MEAN.js alkalmazás toohello Cosmos DB mintaadatbázis imént létrehozott, a MongoDB-kapcsolati karakterlánc használatával. 

### <a name="retrieve-hello-database-key"></a>Hello adatbázis-kulcs beolvasása

tooconnect toohello Cosmos DB adatbázisban, be kell hello adatbázis kulcsot. Használjon hello [az cosmosdb lista-kulcsok](/cli/azure/cosmosdb#list-keys) parancs tooretrieve hello elsődleges kulcs.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

hello Azure CLI információk hasonló toohello a következő példa látható:

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Másolja a hello értékének `primaryMasterKey`. A következő lépésben hello tájékoztatásra van szüksége.

<a name="devconfig"></a>
### <a name="configure-hello-connection-string-in-your-nodejs-application"></a>Node.js-alkalmazásában hello kapcsolati karakterlánc konfigurálása

Nyissa meg a MEAN.js tárház _config/env/production.js_.

A hello `db` objektumazonosító, hello értékét `uri`:

* Cserélje le a két hello  *\<cosmosdb_name >* helyőrzőt a Cosmos DB adatbázis neve.
* Cserélje le a hello  *\<primary_master_key >* helyőrző hello előző lépésben másolt hello kulccsal.

hello következő kód bemutatja hello `db` objektum:

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

Hello `ssl=true` beállítást azért szükség, mert [Cosmos DB SSL megkövetelése](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 

Mentse a módosításokat.

### <a name="test-hello-application-in-production-mode"></a>Hello alkalmazást tesztelheti éles módban 

Futtassa a következő parancs toominify és köteg parancsfájlok hello éles környezet hello. Ez a folyamat hello éles környezetben szükséges hello fájlokat generál.

```bash
gulp prod
```

Futtassa a következő parancs toouse hello kapcsolati karakterlánc konfigurált hello _config/env/production.js_.

```bash
NODE_ENV=production node server.js
```

`NODE_ENV=production`Beállítja a hello környezeti változó, amely közli a Node.js toorun hello éles környezetben.  `node server.js`elindul a Node.js-kiszolgáló hello `server.js` az adattár gyökérkönyvtárában található. Ez az, hogy be van töltve a Node.js-alkalmazás az Azure-ban. 

Hello app be van töltve, ellenőrizze, hogy fut-e hello éles környezetben toomake:

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

Keresse meg a böngészőben toohttp://localhost:8443. Kattintson a **regisztráció** a felső menüben hello és tesztfelhasználó létrehozása. Ha sikeres felhasználó létrehozása, és jelentkezzen be, majd az alkalmazás ír az toohello Cosmos DB-adatbázist az Azure-ban. 

A Terminálszolgáltatások hello, leállításához írja be a Node.js `Ctrl+C`. 

## <a name="deploy-app-tooazure"></a>Alkalmazás tooAzure telepítése

Ebben a lépésben a MongoDB-csatlakoztatott Node.js-alkalmazás tooAzure App Service telepítése.

### <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

Hozzon létre egy App Service-csomag hello [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) parancsot. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

hello alábbi példakód létrehozza az App Service-csomag nevű _myAppServicePlan_ hello segítségével **szabad** IP-címek:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Hello App Service-csomag létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json 
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
```

### <a name="create-a-web-app"></a>Webalkalmazás létrehozása

A webalkalmazás létrehozása az hello `myAppServicePlan` hello az App Service-csomag [az webalkalmazás létrehozása](/cli/azure/webapp#create) parancsot. 

hello web app ad meg egy üzemeltetési tér toodeploy a kódot, és egy URL-címet biztosít az Ön tooview hello telepített alkalmazás. Toocreate hello webes alkalmazás használata. 

A következő parancsot, hello hello helyébe  *\<alkalmazás_neve >* helyőrzőt egy egyedi alkalmazásnevet. Ez a név csak használja hello alapértelmezett URL-cím hello részeként hello web app alkalmazásban így hello nevének kell toobe egyedi az Azure App Service-ben minden alkalmazások között. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Hello webalkalmazás létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-an-environment-variable"></a>Egy környezeti változó beállítása

A hello oktatóanyag, akkor szoftveresen kötött hello adatbázis-kapcsolati karakterláncot a korábbi _config/env/production.js_. Biztonsági szempontból ajánlott összhangban kívánt tookeep a bizalmas adatokat a Git-tárház kívül. Az Azure-ban futó alkalmazások hanem egy környezeti változó fogja használni.

Az App Service-ben, a környezeti változók beállítása _Alkalmazásbeállítások_ hello segítségével [az webapp config appsettings frissítése](/cli/azure/webapp/config/appsettings#update) parancsot. 

hello alábbi példa konfigurál egy `MONGODB_URI` Alkalmazásbeállítás az Azure web app alkalmazásban. Cserélje le a hello  *\<alkalmazás_neve >*,  *\<cosmosdb_name >*, és  *\<primary_master_key >* helyőrzők.

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

A Node.js-kódnak érheti el az alkalmazás setting `process.env.MONGODB_URI`, ahogy bármely környezeti változó elérésére. 

Most visszavonása a változtatások too_config/env/production.js_ a hello a következő parancsot:

```bash
git checkout -- .
```

Nyissa meg _config/env/production.js_ újra. Vegye figyelembe, hogy hello alapértelmezett MEAN.js alkalmazás már konfigurált toouse hello `MONGODB_URI` létrehozott környezeti változó.

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a>A Git helyi üzemelő példányának konfigurálása 

Használjon hello [beállítva az az webalkalmazás üzembe helyező felhasználó](/cli/azure/webapp/deployment/user#set) toocreate hitelesítő adatokat a központi telepítési parancsot.

Az alkalmazás tooAzure App Service többek között az FTP, a helyi Git, a GitHub, a Visual Studio Team Services és a Bitbucketből különböző módokon telepítheti. Az FTP és a helyi Git esetében szükség a központi telepítés hello server tooauthenticate konfigurált toohave a központi felhasználói. A központi felhasználói fiók szintű pedig különböző Azure-előfizetés fiókjából. Csak akkor kell tooconfigure a központi felhasználói egyszer.

Hello a következő parancsban cserélje le  *\<felhasználónév->* és  *\<jelszó >* új felhasználónevet és jelszót. hello felhasználó nevének egyedinek kell lennie. hello jelszó legalább 8 karakter hosszúságúnak kell lennie, és két a következő három elemek hello: betűket, számokat, szimbólumokat. Ha egy ` 'Conflict'. Details: 409` hiba, a módosítás hello felhasználónév. ` 'Bad Request'. Details: 400` hibaüzenet esetén használjon erősebb jelszót.

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

Rekord hello felhasználónév és jelszó használható a későbbi lépésekben hello alkalmazás központi telepítésekor.

Használjon hello [az webalkalmazás központi telepítési forrás config-helyi-git](/cli/azure/webapp/deployment/source#config-local-git) parancs tooconfigure helyi Git hozzáférés toohello Azure-webalkalmazásban. 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

Hello központi felhasználói van konfigurálva, hello Azure CLI formátuma a következő hello hello telepítés URL-CÍMÉT az Azure webalkalmazás szerepel:

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

Másolja át a hello terminálról kimeneti hello hello következő lépésben lesz használható. 

### <a name="push-tooazure-from-git"></a>A Git Push tooAzure

Adjon hozzá egy helyi Git-tárház Azure távoli tooyour. 

```bash
git remote add azure <paste_copied_url_here> 
```

Azure távoli toodeploy toohello a Node.js-alkalmazás leküldési. A korábbi hello létrehozása központi felhasználói hello részeként megadott hello jelszó bekéri. 

```bash
git push azure master
```

A telepítés során az Azure App Service a telepítés előrehaladását a Git kommunikál.

```bash
Counting objects: 5, done.
Delta compression using up too4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

Azt tapasztalhatja, hogy fut-e a központi telepítési folyamat hello [Gulp](http://gulpjs.com/) után `npm install`. App Service nem Gulp vagy Grunt feladatok futnak a telepítés során, így ez a minta-tárház két további fájlok a a root directory tooenable: 

- _.Deployment_ – Ez a fájl közli az App Service toorun `bash deploy.sh` hello egyéni telepítési parancsfájlként.
- _Deploy.SH_ -hello egyéni telepítési parancsfájl. Ha hello fájl tekintse át, látni fogja, hogy fusson `gulp prod` után `npm install` és `bower install`. 

Ez a megközelítés tooadd bármely lépés tooyour Git-alapú üzembe helyezési használhatja. Újraindítja az Azure-webalkalmazásban bármikor, ha az App Service automatizálási feladatok nem futtassa újra.

### <a name="browse-toohello-azure-web-app"></a>Keresse meg az Azure-webalkalmazásban toohello 

Keresse meg a telepített toohello web app a webböngésző használatával. 

```bash 
http://<app_name>.azurewebsites.net 
``` 

Kattintson a **regisztráció** a felső menüben hello, és hozzon létre egy üres felhasználót. 

Ha sikeres, és automatikusan bejelentkezik hello app toohello felhasználó, akkor az MEAN.js-alkalmazás létrehozása az Azure-ban van kapcsolat toohello (Cosmos DB) MongoDB-adatbázist. 

![Az Azure App Service-ben futó MEAN.js alkalmazás](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Válassza ki **Admin > kezelése cikkek** tooadd egyes cikkeket. 

**Gratulálunk!** Adatalapú Node.js-alkalmazás fut az Azure App Service-ben.

## <a name="update-data-model-and-redeploy"></a>Frissítés adatmodell, és helyezze üzembe újra

Ebben a lépésben hello módosítása `article` adatok modell, és a módosítás tooAzure közzététele.

### <a name="update-hello-data-model"></a>Hello adatmodell frissítése

Nyissa meg _modules/articles/server/models/article.server.model.js_.

A `ArticleSchema`, vegye fel a `String` nevű típus `comment`. Amikor elkészült, a séma kódot kell néznek ki:

```javascript
var ArticleSchema = new Schema({
  ...,
  user: {
    type: Schema.ObjectId,
    ref: 'User'
  },
  comment: {
    type: String,
    default: '',
    trim: true
  }
});
```

### <a name="update-hello-articles-code"></a>Hello cikkek kód frissítése

Hello részeinek frissítése a `articles` code toouse `comment`.

Öt fájl toomodify szüksége van: hello server vezérlő és hello négy ügyfél nézeteket. 

Nyissa meg _modules/articles/server/controllers/articles.server.controller.js_.

A hello `update` működik, vegye fel a hozzárendelés `article.comment`. hello következő kód bemutatja befejeződött hello `update` függvény:

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

Nyissa meg _modules/articles/client/views/view-article.client.view.html_.

Hello záró felett `</section>` címkét, adja hozzá a következő sor toodisplay hello `comment` hello egyéb hello cikk együtt:

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

Nyissa meg _modules/articles/client/views/list-articles.client.view.html_.

Hello záró felett `</a>` címkét, adja hozzá a következő sor toodisplay hello `comment` hello egyéb hello cikk együtt:

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

Nyissa meg _modules/articles/client/views/admin/list-articles.client.view.html_.

Belső hello `<div class="list-group">` elem és fölött hello záró `</a>` címkét, adja hozzá a következő sor toodisplay hello `comment` hello egyéb hello cikk együtt:

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

Nyissa meg _modules/articles/client/views/admin/form-article.client.view.html_.

Hello található `<div class="form-group">` elem, amely hello Küldés gomb, mely dolgozunk tartalmazza:

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

Ez a tag fölött hozzáadása egy másik `<div class="form-group">` elem, amelynek segítségével a hello szerkesztése `comment` mező. Az új elem kell kinéznie:

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a>A módosításokat a helyi tesztelése

Mentse az összes módosítást.

Tesztelje a módosításokat az éles üzemmódhoz újból.

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> Ne feledje, hogy a _config/env/production.js_ vissza lett állítva, és hello `MONGODB_URI` környezeti változó csak akkor állítható be, az Azure web app alkalmazásban, és nem a helyi számítógépen. A konfigurációs fájl hello tekinti meg, ha található-e, hogy hello éles konfigurációs alapértelmezés szerint toouse helyi MongoDB-adatbázist. Ezzel biztosíthatja, hogy nem touch termelési adatok helyben a kódmódosításokat tesztelésekor.

Keresse meg a túl`http://localhost:8443` a böngészőben, és győződjön meg arról, hogy be van jelentkezve.

Válassza ki **Admin > kezelése cikkek**, majd adja hozzá a cikk hello kiválasztásával  **+**  gomb.

Új lásd hello `Comment` szövegmező most.

![A hozzáadott megjegyzés mező tooArticles](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

A Terminálszolgáltatások hello, leállításához írja be a Node.js `Ctrl+C`. 

### <a name="publish-changes-tooazure"></a>Módosítások tooAzure közzététele

A Git a változtatások véglegesítése a határidő, majd továbbítsa azokat hello kód módosítások tooAzure.

```bash
git commit -am "added article comment"
git push azure master
```

Egyszer hello `git push` befejezése, keresse meg az Azure-webalkalmazásban tooyour és hello új funkcióinak kipróbálásához.

![Adatbázis és a modell módosítások közzétéve tooAzure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

Ha korábban hozzáadott olyan cikkek, továbbra is láthatja azokat. A Cosmos DB meglévő adatait nem elvész. Emellett a frissítések toohello adatséma és a meglévő adatok sértetlenek.

## <a name="stream-diagnostic-logs"></a>Diagnosztikai naplók adatfolyam 

A Node.js-alkalmazás futtatása az Azure App Service-ben, közben kaphat hello konzol naplók védőeszközön tooyour terminál. Ily módon kaphat hello ugyanazon diagnosztikai üzenetek toohelp hibakeresése az alkalmazáshibák.

streamelés esetén használjon hello toostart napló [az webapp napló végéről](/cli/azure/webapp/log#tail) parancsot.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

Napló adatfolyam megkezdése, után frissítse az Azure-webalkalmazásban a hello böngésző tooget néhány webes forgalom. Ekkor megjelenik konzolnaplófájlokban adatcsatornán terminál tooyour.

Írja be a bármikor streaming leállítási napló `Ctrl+C`. 

## <a name="manage-your-azure-web-app"></a>Az Azure-webalkalmazásban kezelése

Nyissa meg toohello [Azure-portálon](https://portal.azure.com) toosee hello létrehozott webalkalmazás.

Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevére.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

Alapértelmezés szerint hello portal jeleníti meg a webalkalmazás **áttekintése** lap. Ezen az oldalon megtekintheti az alkalmazás állapotát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés. hello lapok hello bal oldalán található hello hello különböző konfigurációs lapok megnyithatja megjelenítése.

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Következő lépések

Hogy mit tudott:

> [!div class="checklist"]
> * MongoDB-adatbázis létrehozása az Azure-ban
> * Csatlakozás egy Node.js-alkalmazás tooMongoDB
> * Hello app tooAzure telepítése
> * Hello adatmodell frissítése, és telepítse újra hello alkalmazást
> * Stream naplók az Azure tooyour terminál
> * Az Azure-portálon hello hello alkalmazás kezelése

Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name tooyour webalkalmazás.

> [!div class="nextstepaction"] 
> [Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése](app-service-web-tutorial-custom-domain.md)
