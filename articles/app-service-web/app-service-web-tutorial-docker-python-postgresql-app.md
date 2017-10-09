---
title: "aaaBuild egy Docker Python és PostgreSQL webalkalmazást az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooget a Docker Python-alkalmazások használata az Azure-ban, a kapcsolat tooa PostgreSQL-adatbázis használati ideje."
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
editor: 
ms.assetid: 2bada123-ef18-44e5-be71-e16323b20466
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: e594ef9ec8c04ef2bf725e5f998691f3fb8cf815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Az Azure-ban Docker Python és PostgreSQL webalkalmazás létrehozása

Az Azure Web Apps jól skálázható, önálló javítási a webhelyszolgáltató biztosít. Ez az oktatóanyag bemutatja, hogyan toocreate egy alapszintű Docker Python webalkalmazás az Azure-ban. Az alkalmazás tooa PostgreSQL-adatbázishoz fog csatlakozni. Amikor elkészült, konfigurálnia kell egy Python Flask-alkalmazás egy Docker-tároló belül futó [Azure App Service Web Apps](app-service-web-overview.md).

![Docker Python Flask-alkalmazás az Azure App Service-ben](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

A macOS hajtsa végre az alábbi hello lépéseket. Linux és Windows utasításokat a legtöbb esetben hello azonos, de hello különbségek nem részletes ebben az oktatóanyagban.
 
## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

1. [A Git telepítése](https://git-scm.com/)
1. [Telepítse a Pythont](https://www.python.org/downloads/)
1. [Telepítheti és futtathatja PostgreSQL](https://www.postgresql.org/download/)
1. [Telepítse a Docker Community Edition](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Adatbázis létrehozása és helyi PostgreSQL-telepítés sikerességének ellenőrzése

Nyissa meg a hello terminálablakot és `psql postgres` tooconnect tooyour helyi PostgreSQL-kiszolgálón.

```bash
psql postgres
```

Ha a kapcsolódás sikeres, a rendszer fut a PostgreSQL-adatbázisból. Ha nem, győződjön meg arról, hogy elindult-e a helyi PostgresQL-adatbázisban: hello lépéseket követve [letölti - PostgreSQL Core terjesztési](https://www.postgresql.org/download/).

Adatbázis létrehozása *eventregistration* és nevű külön adatbázis-felhasználó beállítása *manager* jelszóval *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```
Típus *\q* tooexit hello PostgreSQL-ügyfél. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Helyi Python Flask-alkalmazás létrehozása

Ebben a lépésben beállíthatja hello helyi Python Flask-projektet.

### <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Nyissa meg hello terminálablakot, és `CD` tooa munkakönyvtárát.  

Futtatási hello következő parancsok tooclone hello minta tárház, majd a toohello *0,1-initialapp* kiadási.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Ez a minta-tárház tartalmaz egy [Flask](http://flask.pocoo.org/) alkalmazás. 

### <a name="run-hello-application"></a>Hello alkalmazás futtatása

> [!NOTE] 
> Egy későbbi lépésben egy Docker-tároló toouse hello éles adatbázissal kiépítésével, egyszerűsíti a folyamatot.

Telepítse a szükséges hello csomagok, és indítsa el a hello alkalmazás.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Amikor hello alkalmazás teljesen betöltődik, valami hasonló toohello a következő üzenet jelenik meg:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

Keresse meg a böngészőben toohttp://127.0.0.1:5000. Kattintson a **regisztrálni!** és tesztfelhasználó létrehozása.

![Helyileg futó Python Flask-alkalmazás](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

hello Flask mintaalkalmazás felhasználói adatok hello adatbázisban tárolja. Ha a felhasználó regisztrálása sikeres, az alkalmazás ír adatok toohello helyi PostgreSQL-adatbázisból.

toostop hello Flask server bármikor, írja be a Terminálszolgáltatások hello Ctrl + C. 

## <a name="create-a-production-postgresql-database"></a>Hozzon létre egy PostgreSQL-adatbázisban

Ebben a lépésben létrehoz egy PostgreSQL-adatbázisban az Azure-ban. Ha az alkalmazás telepített tooAzure, a felhő adatbázist fogja használni.

### <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Biztos, hogy most a folyamatban toouse hello Azure CLI 2.0 toocreate hello erőforrások szükséges toohost a Python-alkalmazás az Azure App Service-ben.  Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat. 

```azurecli
az login 
``` 
   
### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a hello [az csoport létrehozása](/cli/azure/group#create). 

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

hello alábbi példa létrehoz egy erőforráscsoport hello USA nyugati régiója régióban:

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

Használjon hello [az App Service lista-helyek](/cli/azure/appservice#list-locations) Azure CLI parancssori toolist elérhető helyről.

### <a name="create-an-azure-database-for-postgresql-server"></a>Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz

Egy PostgreSQL-kiszolgáló létrehozása hello [az postgres kiszolgáló létrehozni](/cli/azure/documentdb#create) parancsot.

Hello a következő parancsot, helyettesítse egy egyedi kiszolgálónevet a hello  *\<postgresql_name >* helyőrző és egy felhasználónevet hello  *\<admin_username >* helyőrző . hello kiszolgálónevet a PostgreSQL-végpontot részeként használatos (`https://<postgresql_name>.postgres.database.azure.com`), így a hello nevének kell toobe egyedi az Azure-ban minden kiszolgálóra. hello felhasználónév hello kezdeti adatbázis rendszergazdai felhasználói fiók megadása. Biztosan felszólító toopick a felhasználó jelszavát.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

Hello Azure adatbázis PostgreSQL-kiszolgáló létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-hello-azure-database-for-postgresql-server"></a>Hozzon létre egy tűzfalszabályt hello Azure adatbázis PostgreSQL-kiszolgáló

Futtassa az Azure parancssori felület toohello parancs tooallow adatbázist az összes IP-cím a következő hello.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

hello Azure CLI megerősíti, hogy a hello tűzfalszabály létrehozása a kimeneti hasonló toohello a következő példa:

```json
{
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

## <a name="connect-your-python-flask-application-toohello-database"></a>Csatlakozás a Python Flask toohello adatbázis

Ebben a lépésben csatlakoztatja a Python Flask minta alkalmazás toohello Azure adatbázis létrehozott PostgreSQL-kiszolgáló.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Hozzon létre egy üres adatbázist, és állítson be egy új adatbázis-alkalmazás felhasználói

Hozzon létre egy adatbázis-felhasználó tooa egyetlen adatbázist csak. Ezen hitelesítő adatok tooavoid jogosultságot ad a hello Alkalmazáskiszolgáló teljes körű hozzáférési toohello fogja használni.

Csatlakozás toohello adatbázis (felszólítja a rendszergazdai jelszavát).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Hello PostgreSQL CLI hello adatbázis és a felhasználó létrehozása.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```

Típus *\q* tooexit hello PostgreSQL-ügyfél.

### <a name="test-hello-application-locally-against-hello-azure-postgresql-database"></a>Helyi adatbázison hello Azure PostgreSQL hello alkalmazás tesztelése 

Ha visszalép most toohello *app* mappában található hello klónozott Github-tárházban, hello Python Flask-alkalmazást futtathatja hello adatbázis környezeti változók frissítésével.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Amikor hello alkalmazás teljesen betöltődik, valami hasonló toohello a következő üzenet jelenik meg:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

Keresse meg a böngészőben toohttp://127.0.0.1:5000. Kattintson a **regisztrálni!** és egy tesztelési regisztráció létrehozása. Az Azure-ban most ír az toohello-adatbázist.

![Helyileg futó Python Flask-alkalmazás](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-hello-application-from-a-docker-container"></a>Hello alkalmazást futtat egy Docker-tárolójából.

Hello Docker-tároló lemezképet létre.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker visszaigazolja, hogy sikeresen létrejött informatikai hello tároló.

```bash
Successfully built 7548f983a36b
```

Adja hozzá a környezeti változók tooan környezeti változó adatbázisfájl *db.env*. hello app toohello PostgreSQL éles adatbázis az Azure-ban fog csatlakozni.

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

Futtatta belül hello Docker-tároló hello alkalmazását. hello következő parancs hello környezeti változó fájlt adja meg, és leképezi hello alapértelmezett Flask port 5000 toolocal 5000-es port.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

hello korábban látott hasonló toowhat eredménye. Hello kezdeti adatbázis áttelepítés azonban már nincs szüksége a toobe végre, és ezért a rendszer kihagyja.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C tooquit)
```

hello adatbázis már tartalmazza a korábban létrehozott hello regisztrációs.

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-hello-docker-container-tooa-container-registry"></a>Hello Docker tároló tooa tároló beállításjegyzék feltöltése

Ebben a lépésben hello Docker tároló tooa tároló beállításjegyzék feltöltése. Azure-tároló beállításjegyzék fogja használni, de más például Docker Hub népszerű ők is használhatja.

### <a name="create-an-azure-container-registry"></a>Azure Container Registry létrehozása

Cserélje le a következő parancs toocreate tároló beállításjegyzékbeli hello  *\<registry_name >* az Ön által választott egyedi Azure tároló beállításjegyzék néven.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Kimenet
```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-hello-registry-credentials-for-pushing-and-pulling-docker-images"></a>Hello beállításjegyzék hitelesítő adatokat kérdez le, és húzza a Docker képek beolvasása

tooshow beállításjegyzék hitelesítő adatait, először engedélyezze a felügyeleti mód.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Megjelenik a két jelszó. Jegyezze fel a hello felhasználónevet és hello első jelszót.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-tooazure-container-registry"></a>A Docker-tároló tooAzure tároló beállításjegyzék feltöltése

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-hello-docker-python-flask-application-tooazure"></a>Hello Docker Python Flask alkalmazás tooAzure telepítése

Ebben a lépésben a Docker tároló-alapú Python Flask alkalmazás tooAzure App Service telepítése.

### <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

Hozzon létre egy App Service-csomag hello [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) parancsot. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

hello alábbi példa létrehoz egy Linux-alapú App Service-csomag nevű *myAppServicePlan* hello S1 IP-címek segítségével:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Hello App Service-csomag létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json 
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "linux",
  "location": "West US",
  "maximumNumberOfWorkers": 10,
  "name": "myAppServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "S",
    "locations": null,
    "name": "S1",
    "size": "S1",
    "skuCapacity": null,
    "tier": "Standard"
  },
  "status": "Ready",
  "subscription": "00000000-0000-0000-0000-000000000000",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
``` 

### <a name="create-a-web-app"></a>Webalkalmazás létrehozása

A webalkalmazás létrehozása az hello *myAppServicePlan* hello az App Service-csomag [az webalkalmazás létrehozása](/cli/azure/webapp#create) parancsot. 

hello web app ad meg egy üzemeltetési tér toodeploy a kódot, és egy URL-címet biztosít az Ön tooview hello telepített alkalmazás. Toocreate hello webes alkalmazás használata. 

A következő parancsot, hello hello helyébe  *\<alkalmazás_neve >* helyőrzőt egy egyedi alkalmazásnevet. Ez a név része hello alapértelmezett URL-cím hello webalkalmazás, így hello nevének kell toobe egyedi az Azure App Service-ben minden alkalmazások között. 

```azurecli
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

### <a name="configure-hello-database-environment-variables"></a>Hello adatbázis környezeti változók konfigurálása

Hello az oktatóanyagban a korábbi megadott környezeti változók tooconnect tooyour PostgreSQL-adatbázisból.

Az App Service-ben, a környezeti változók beállítása _Alkalmazásbeállítások_ hello segítségével [az webapp appsettings konfiguráció](/cli/azure/webapp/config#set) parancsot. 

hello alábbi példa adja meg adatbázis-kapcsolat adatai hello Alkalmazásbeállítások. Ugyancsak hello *PORT* változó toomap PORTJÁVAL 5000 a Docker-tároló tooreceive HTTP-forgalmat a 80-as PORT.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Docker-tároló telepítés konfigurálása 

App Service automatikusan letölteni és futtatni egy Docker-tároló.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Amikor a hello Docker-tároló frissítésére vagy hello beállításainak módosítása, indítsa újra a hello alkalmazást. Újraindítása biztosítja, hogy minden beállítások érvényesek, és hello legújabb tároló lekért hello beállításjegyzék van.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-toohello-azure-web-app"></a>Keresse meg az Azure-webalkalmazásban toohello 

Keresse meg a telepített toohello web app a webböngésző használatával. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> hello webalkalmazás hosszabb tooload vesz igénybe, mert hello tároló toobe letöltődnek és működni kezdenek hello tároló konfigurációjának módosítása után.

Megjelenik a korábban regisztrált vendégek hello előző lépésben mentett toohello Azure éles adatbázist.

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Gratulálunk!** Futtatja egy Docker tároló-alapú Python Flask-alkalmazás az Azure App Service-ben.

## <a name="update-data-model-and-redeploy"></a>Frissítés adatmodell, és helyezze üzembe újra

Ebben a lépésben a résztvevők tooeach eseményregisztráció hello száma hello vendég modell frissítésével hozzá.

Tekintse meg a hello *0,2-áttelepítési* kiadás az alábbi git parancs hello:

```bash
git checkout tags/0.2-migration
```

Ebben a kiadásban már elvégezték a szükséges módosításokat tooviews, tartományvezérlők és modell hello. Is keresztül létrehozott adatbázis áttelepítés *alembic* (`flask db migrate`). A következő git parancs hello keresztül végrehajtott valamennyi módosítást látható:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>A módosításokat a helyi tesztelése

Futtassa a következő parancsok tootest hello a módosítások helyileg futó hello flask kiszolgáló.

Mac / Linux:
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Keresse meg a böngésző tooview hello módosítások toohttp://127.0.0.1:5000. Hozzon létre egy teszt regisztrációs.

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-tooazure"></a>Módosítások tooAzure közzététele

Hello új docker lemezkép, toohello tároló beállításjegyzék leküldeni, és indítsa újra hello alkalmazást.

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Keresse meg az Azure-webalkalmazásban tooyour, és újra hello új funkcióinak kipróbálásához. Hozzon létre egy másik eseményregisztráció.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask-alkalmazás az Azure App Service-ben](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Az Azure-webalkalmazásban kezelése

Nyissa meg toohello [Azure-portálon](https://portal.azure.com) toosee hello létrehozott webalkalmazás.

Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevére.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-docker-python-postgresql-app/app-resource.png)

Alapértelmezés szerint hello portal jeleníti meg a webalkalmazás **áttekintése** lap. Ezen az oldalon megtekintheti az alkalmazás állapotát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés. hello lapok hello bal oldalán található hello hello különböző konfigurációs lapok megnyithatja megjelenítése.

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Következő lépések

Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name tooyour webalkalmazás.

> [!div class="nextstepaction"] 
> [Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése](app-service-web-tutorial-custom-domain.md)
