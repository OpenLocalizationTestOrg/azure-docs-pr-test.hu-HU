---
title: "aaaBuild egy Java és MySQL webalkalmazást az Azure-ban"
description: "Megtudhatja, hogyan tooget egy Java-alkalmazást, amely összeköti toohello Azure MySQL-adatbázis szolgáltatás működik-e az Azure App Service."
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: 0820ee9c2b7bf8fcaa22287c27a7ab848a1c4927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Az Azure Java és MySQL webalkalmazás létrehozása

Az oktatóanyag bemutatja, hogyan toocreate Java web app az Azure-ban, és csatlakoztassa tooa MySQL-adatbázis. Ha elkészült, akkor egy [rugó rendszerindító](https://projects.spring.io/spring-boot/) adattárolásra alkalmazás [MySQL az Azure-adatbázis](https://docs.microsoft.com/azure/mysql/overview) futó [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).

![Java-alkalmazás fusson az Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * MySQL-adatbázis létrehozása az Azure-ban
> * A minta app toohello adatbázis csatlakoztatása
> * Hello app tooAzure telepítése
> * Frissítse és telepítse újra hello alkalmazást
> * Az adatfolyam diagnosztikai naplók az Azure-ból
> * Az Azure-portálon hello hello alkalmazás figyelése


## <a name="prerequisites"></a>Előfeltételek

1. [Töltse le és telepítse a Git](https://git-scm.com/)
1. [Töltse le és telepítse a Java 7 JDK hello vagy újabb](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Töltse le, telepítse, és indítsa el a MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="prepare-local-mysql"></a>Készítse elő a helyi MySQL 

Ebben a lépésben adatbázis létrehozása a helyi MySQL-kiszolgálót használja a tesztelési hello alkalmazásban helyileg a számítógépen.

### <a name="connect-toomysql-server"></a>Csatlakoztassa a kiszolgálót tooMySQL

Egy terminálablakot tooyour helyi MySQL kiszolgálóhoz csatlakozzon. A terminálablakot toorun összes hello parancsokat használhatja az oktatóanyag.

```bash
mysql -u root -p
```

Ha a számítógép a jelszót, adja meg a jelszót hello hello `root` fiók. Ha nem emlékszik a rendszergazdafiók jelszavával, lásd: [MySQL: hogyan tooReset hello gyökér szintű jelszó](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Ha a parancs sikeresen lefutott, majd a MySQL-kiszolgáló már fut. Ha nem, győződjön meg arról, hogy elindult-e a helyi MySQL-kiszolgáló által a következő hello [MySQL telepítés utáni lépéseket](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database"></a>Adatbázis létrehozása 

A hello `mysql` kérni, hozzon létre egy adatbázist és a táblát, hello Tennivalólista elemein.

```sql
CREATE DATABASE tododb;
```

Lépjen ki a kiszolgálói kapcsolatot írja be a `quit`.

```sql
quit
```

## <a name="create-and-run-hello-sample-app"></a>Hozzon létre és hello mintaalkalmazás futtatása 

Ebben a lépésben klónozza a mintaalkalmazást rugó rendszerindító, toouse hello helyi MySQL-adatbázis konfigurálja, és futtassa azt a számítógépen. 

### <a name="clone-hello-sample"></a>Klónozás hello minta

Hello terminálablakot lépjen a tooa hello minta directory és a klónozott tárház működik. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-hello-app-toouse-hello-mysql-database"></a>Hello app toouse hello MySQL-adatbázis konfigurálása

Frissítés hello `spring.datasource.password` és értéket *spring-boot-mysql-todo/src/main/resources/application.properties* hello ugyanazon gyökér szintű jelszó használt tooopen hello MySQL parancssorba:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-hello-sample"></a>Hozza létre és futtasson hello mintát

Build, és futtassa a hello mintákkal hello Maven burkoló hello tárházban tartalmazza:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Nyissa meg a böngésző toohttp://localhost:8080 toosee hello minta működés közben. Toohello tevékenységlistájának hozzáadásakor, használja a következő SQL parancsot az hello MySQL Rákérdezés tooview hello a adataihoz MySQL hello.

```SQL
use testdb;
select * from todo_item;
```

Állítsa le a hello alkalmazást úgy, hogy elérte-e `Ctrl` + `C` a Terminálszolgáltatások hello. 

## <a name="create-an-azure-mysql-database"></a>Hozzon létre egy Azure-beli MySQL database

Ebben a lépésben létrehoz egy [MySQL az Azure-adatbázis](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) példányhoz hello használatával [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Konfigurált hello minta alkalmazás toouse ezt az adatbázist később hello oktatóanyag.

Használjon hello Azure CLI 2.0 egy terminálablakot toocreate hello erőforrások a szükséges toohost a Java-alkalmazások az Azure App Service. Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat. 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Egy Azure erőforráscsoport egy olyan logikai tároló, ahol a kapcsolódó erőforrások, például webalkalmazások, adatbázisok és a storage-fiókok telepítése és kezelése. 

hello alábbi példa létrehoz egy erőforráscsoport hello Észak-Európa régióban:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

toosee hello a lehetséges értékek is használhatja a `--location`, használja a hello [az App Service lista-helyek](/cli/azure/appservice#list-locations) parancsot.

### <a name="create-a-mysql-server"></a>A MySQL-kiszolgáló létrehozása

A kiszolgáló létrehozása az Azure-adatbázisban a MySQL (előzetes verzió) hello [az mysql kiszolgáló létrehozni](/cli/azure/mysql/server#create) parancsot.    
A saját egyedi MySQL kiszolgáló nevét, ahol látható hello `<mysql_server_name>` helyőrző. Ez a név része a MySQL-kiszolgáló állomásneve, `<mysql_server_name>.mysql.database.azure.com`, így a toobe globálisan egyedi kell. Is `<admin_user>` és `<admin_password>` saját értékekkel.

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

Hello MySQL kiszolgáló akkor jön létre, amikor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Kiszolgáló tűzfal konfigurálása

Hozzon létre egy tűzfalszabályt a MySQL server tooallow ügyfél kapcsolatok hello segítségével [az mysql-tűzfalszabály létrehozása](/cli/azure/mysql/server/firewall-rule#create) parancsot. 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> MySQL (előzetes verzió) Azure-adatbázis automatikusan jelenleg nem engedélyezi a Azure-szolgáltatásokhoz érkező kapcsolatokat. Dinamikusan kiosztott IP-címek az Azure-ban, mert már jobb tooenable összes IP-címek most. Hello szolgáltatás folyamatosan mintájának, biztonságossá tétele az adatbázis jobban módszereinek engedélyezve lesz.

## <a name="configure-hello-azure-mysql-database"></a>Konfigurálja a hello Azure-beli MySQL database

A hello terminálablakot a számítógépen csatlakozzon a toohello MySQL server az Azure-ban. Használja a korábban megadott hello értéket `<admin_user>` és `<mysql_server_name>`.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Adatbázis létrehozása 

A hello `mysql` kérni, hozzon létre egy adatbázist és a táblát, hello Tennivalólista elemein.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Hozzon létre egy felhasználói jogosultságokkal

Hozzon létre egy adatbázis-felhasználó, és adjon neki a hello minden jogosultság `tododb` adatbázis. Cserélje le a helyőrzőket hello `<Javaapp_user>` és `<Javaapp_password>` a saját egyedi alkalmazás nevével.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* too'<Javaapp_user>';
```

Lépjen ki a kiszolgálói kapcsolatot írja be a `quit`.

```sql
quit
```

## <a name="deploy-hello-sample-tooazure-app-service"></a>Hello minta tooAzure App Service telepítése

Hozzon létre az Azure App Service-csomag hello **szabad** hello segítségével tarifacsomag [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) CLI parancsot. hello App Service-csomagot határoz meg hello használt fizikai erőforrások toohost az alkalmazásokat. App Service-csomagot tooan hozzárendelt összes alkalmazás ossza meg ezeket az erőforrásokat, így toosave költség több alkalmazás esetén. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Amikor készen áll a hello terv, hello Azure CLI hasonló látható kimeneti toohello a következő példa:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Azure-webalkalmazás létrehozása

 Használjon hello [az webalkalmazás létrehozása](/cli/azure/appservice/web#create) CLI parancs toocreate egy web app-definíciót a hello `myAppServicePlan` App Service-csomag. hello web app-definíciót tartalmaz egy URL-cím tooaccess az alkalmazásba, és konfigurálja a több beállítások toodeploy a kód tooAzure. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

Helyettesítő hello `<app_name>` helyőrző a saját egyedi alkalmazás nevével. A egyedi név része hello alapértelmezett tartomány nevét hello webalkalmazás, így hello nevének kell toobe egyedi az Azure-ban minden alkalmazások között. Ki kell alakítani az tooyour felhasználók hozzárendelhető egy egyéni tartomány nevét bejegyzés toohello webalkalmazást.

Amikor készen áll a hello web app-definíciót, hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg: 

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

### <a name="configure-java"></a>Java konfigurálása 

Hello Java runtime konfiguráció, amelyet az alkalmazás a hello [az App Service web config frissítés](/cli/azure/appservice/web/config#update) parancsot.

hello következő parancsot konfigurálása hello web app toorun a legújabb Java 8 JDK és [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-hello-app-toouse-hello-azure-sql-database"></a>Hello app toouse hello Azure SQL adatbázis konfigurálása

Mielőtt futtatná hello mintaalkalmazást, hello web app toouse hello Azure-beli MySQL database az Azure-ban létrehozott Alkalmazásbeállítások be. Ezek a Tulajdonságok kitett toohello webalkalmazás környezeti változóként és hello application.properties belül hello csomagolt webalkalmazás beállított hello érték felülírására. 

Állítsa be az alkalmazás a beállításokat a [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) a hello CLI:

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" \
    --resource-group myResourceGroup \
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name  \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL=Javaapp_password \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>FTP telepítési hitelesítő adatainak lekérése 
Az alkalmazás tooAzure App Service többek között az FTP, helyi Git, GitHub, a Visual Studio Team Services és a Bitbucketből különböző módokon telepítheti. Ebben a példában az FTP-toodeploy hello. A helyi számítógép tooAzure App Service korábban épülő WAR-fájlt.

toodetermine toopass mentén egy ftp parancs toohello Web App alkalmazásban használja a milyen hitelesítő adatok [az App Service web deployment lista-közzététel-profilok](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) parancs: 

```azurecli-interactive
az webapp deployment list-publishing-profiles \ 
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" \ 
    --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-hello-app-using-ftp"></a>A FTP hello alkalmazás feltöltése

A kedvenc FTP eszköz toodeploy hello használata. WAR-fájl toohello */site/wwwroot/webapps* hello vett hello kiszolgálócímet mappájába `URL` hello előző parancs mezőbe. Hello meglévő alapértelmezett (ROOT) alkalmazás könyvtár eltávolítása, és cserélje le a meglévő ROOT.war a hello hello. Hello az oktatóanyag során korábban küldje el a hello beépített WAR-fájlt.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected toowaws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-hello-web-app"></a>Teszt hello webalkalmazás

Keresse meg a túl`http://<app_name>.azurewebsites.net/` és néhány feladatok toohello listát vesznek fel. 

![Java-alkalmazás fusson az Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Gratulálunk!** Adatalapú Java-alkalmazást használ az Azure App Service-ben.

## <a name="update-hello-app-and-redeploy"></a>Frissítés hello alkalmazást, és helyezze üzembe újra

Frissítés hello alkalmazás tooinclude egy további oszlopot az hello todo listában milyen nap hello elem lett létrehozva. Rugó rendszerindító kezeli a frissítési hello adatbázisséma meg hello adatok modell módosítás a meglévő üzenetadatbázis-rekordok módosítása nélkül.

1. A helyi számítógépen nyissa meg *src/main/java/com/example/fabrikam/TodoItem.java* , és adja hozzá a következő hello importálja toohello osztály:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Adja hozzá a `String` tulajdonság `timeCreated` túl*src/main/java/com/example/fabrikam/TodoItem.java*, inicializálja azt az időbélyegzőnek a objektum létrehozásakor. Adja hozzá az új hello beolvasókat/Setter elemek `timeCreated` tulajdonság a fájl szerkesztése közben.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. Frissítés *src/main/java/com/example/fabrikam/TodoDemoController.java* a hello vonallal `updateTodo` metódus tooset hello időbélyeg:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Új mező hello támogatásáról hello Thymeleaf sablonban. Frissítés *src/main/resources/templates/index.html* hello timestamp, és egy új toodisplay hello mezőérték hello Timestamp az egyes táblazatsorok adatokat egy új tábla fejléccel.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Építse újra hello alkalmazás:

    ```bash
    mvnw clean package 
    ```

6. FTP-hello frissítve. Hello meglévő eltávolítása előtt, mint WAR *hely/wwwroot/webapps/ROOT* könyvtár és *ROOT.war*, majd a frissített hello feltöltése. Mint ROOT.war WAR-fájlt. 

Hello alkalmazás frissítésekor egy **alkalommal létre** oszlop már látható. Ha hozzáad egy új feladatot, hello alkalmazás automatikusan hello időbélyeg fogja majd feltölteni. A meglévő feladatokat továbbra is változatlan és a munkahelyi hello alkalmazással annak ellenére, hogy az alapul szolgáló adatmodellt hello megváltozott. 

![Java-alkalmazást egy olyan új oszlop frissült](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Diagnosztikai naplók adatfolyam 

A Java-alkalmazás futtatása az Azure App Service-ben, közben kaphat a hello konzol naplók adatcsatornán közvetlenül a Terminálszolgáltatások tooyour. Ily módon kaphat hello ugyanazon diagnosztikai üzenetek toohelp hibakeresése az alkalmazáshibák.

streamelés esetén használjon hello toostart napló [az webapp napló végéről](/cli/azure/appservice/web/log#tail) parancsot.

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Az Azure-webalkalmazásban kezelése

Nyissa meg toohello az Azure portál toosee hello létrehozott webalkalmazás.

toodo, jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com).

Hello bal oldali menüben kattintson **App Service**, majd kattintson az Azure-webalkalmazásban hello nevére.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Alapértelmezés szerint a webalkalmazás panelen látható hello **áttekintése** lap. Ezen az oldalon megtekintheti az alkalmazás állapotát. Itt is elvégezheti stop, kezdő, újraindítását és delete felügyeleti feladatokhoz. hello lapokon hello bal oldalán hello panelen láthatók hello különböző konfigurációs lapok is megnyithatja.

![Az App Service panel az Azure Portalon](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

A lapok hello panelen megjelenítése hello számos nagy szolgáltatás tooyour webes alkalmazás is hozzáadhat. a következő lista hello hello lehetőségeket néhány következő:
* Egyéni DNS-név leképezése
* Egyéni SSL-tanúsítvány kötése
* Folyamatos üzembe helyezés konfigurálása
* Vertikális felskálázás és kibővítés
* Felhasználói hitelesítés hozzáadása

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha ezeket az erőforrásokat egy másik az oktatóanyaghoz nincs szüksége (lásd: [további lépések](#next)), törölheti azokat a hello a következő parancs futtatásával: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Következő lépések

> [!div class="checklist"]
> * MySQL-adatbázis létrehozása az Azure-ban
> * Csatlakozás egy minta Java-alkalmazás toohello MySQL
> * Hello app tooAzure telepítése
> * Frissítse és telepítse újra hello alkalmazást
> * Az adatfolyam diagnosztikai naplók az Azure-ból
> * Az Azure-portálon hello hello alkalmazás kezelése

Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name toohello alkalmazást.

> [!div class="nextstepaction"] 
> [Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése](app-service-web-tutorial-custom-domain.md)
