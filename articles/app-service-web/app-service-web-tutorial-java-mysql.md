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
# <a name="build-a-java-and-mysql-web-app-in-azure"></a><span data-ttu-id="c6132-103">Az Azure Java és MySQL webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c6132-103">Build a Java and MySQL web app in Azure</span></span>

<span data-ttu-id="c6132-104">Az oktatóanyag bemutatja, hogyan toocreate Java web app az Azure-ban, és csatlakoztassa tooa MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c6132-104">This tutorial shows you how toocreate a Java web app in Azure and connect it tooa MySQL database.</span></span> <span data-ttu-id="c6132-105">Ha elkészült, akkor egy [rugó rendszerindító](https://projects.spring.io/spring-boot/) adattárolásra alkalmazás [MySQL az Azure-adatbázis](https://docs.microsoft.com/azure/mysql/overview) futó [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="c6132-105">When you are finished, you will have a [Spring Boot](https://projects.spring.io/spring-boot/) application storing data in [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) running on [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span></span>

![Java-alkalmazás fusson az Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="c6132-107">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="c6132-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c6132-108">MySQL-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c6132-108">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="c6132-109">A minta app toohello adatbázis csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="c6132-109">Connect a sample app toohello database</span></span>
> * <span data-ttu-id="c6132-110">Hello app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="c6132-110">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="c6132-111">Frissítse és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="c6132-111">Update and redeploy hello app</span></span>
> * <span data-ttu-id="c6132-112">Az adatfolyam diagnosztikai naplók az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="c6132-112">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="c6132-113">Az Azure-portálon hello hello alkalmazás figyelése</span><span class="sxs-lookup"><span data-stu-id="c6132-113">Monitor hello app in hello Azure portal</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c6132-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c6132-114">Prerequisites</span></span>

1. [<span data-ttu-id="c6132-115">Töltse le és telepítse a Git</span><span class="sxs-lookup"><span data-stu-id="c6132-115">Download and install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="c6132-116">Töltse le és telepítse a Java 7 JDK hello vagy újabb</span><span class="sxs-lookup"><span data-stu-id="c6132-116">Download and install hello Java 7 JDK or above</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [<span data-ttu-id="c6132-117">Töltse le, telepítse, és indítsa el a MySQL</span><span class="sxs-lookup"><span data-stu-id="c6132-117">Download, install, and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c6132-118">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="c6132-118">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c6132-119">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="c6132-119">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="c6132-120">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c6132-120">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-local-mysql"></a><span data-ttu-id="c6132-121">Készítse elő a helyi MySQL</span><span class="sxs-lookup"><span data-stu-id="c6132-121">Prepare local MySQL</span></span> 

<span data-ttu-id="c6132-122">Ebben a lépésben adatbázis létrehozása a helyi MySQL-kiszolgálót használja a tesztelési hello alkalmazásban helyileg a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c6132-122">In this step, you create a database in a local MySQL server for use in testing hello app locally on your machine.</span></span>

### <a name="connect-toomysql-server"></a><span data-ttu-id="c6132-123">Csatlakoztassa a kiszolgálót tooMySQL</span><span class="sxs-lookup"><span data-stu-id="c6132-123">Connect tooMySQL server</span></span>

<span data-ttu-id="c6132-124">Egy terminálablakot tooyour helyi MySQL kiszolgálóhoz csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="c6132-124">In a terminal window, connect tooyour local MySQL server.</span></span> <span data-ttu-id="c6132-125">A terminálablakot toorun összes hello parancsokat használhatja az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c6132-125">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="c6132-126">Ha a számítógép a jelszót, adja meg a jelszót hello hello `root` fiók.</span><span class="sxs-lookup"><span data-stu-id="c6132-126">If you're prompted for a password, enter hello password for hello `root` account.</span></span> <span data-ttu-id="c6132-127">Ha nem emlékszik a rendszergazdafiók jelszavával, lásd: [MySQL: hogyan tooReset hello gyökér szintű jelszó](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="c6132-127">If you don't remember your root account password, see [MySQL: How tooReset hello Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="c6132-128">Ha a parancs sikeresen lefutott, majd a MySQL-kiszolgáló már fut.</span><span class="sxs-lookup"><span data-stu-id="c6132-128">If your command runs successfully, then your MySQL server is already running.</span></span> <span data-ttu-id="c6132-129">Ha nem, győződjön meg arról, hogy elindult-e a helyi MySQL-kiszolgáló által a következő hello [MySQL telepítés utáni lépéseket](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="c6132-129">If not, make sure that your local MySQL server is started by following hello [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database"></a><span data-ttu-id="c6132-130">Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="c6132-130">Create a database</span></span> 

<span data-ttu-id="c6132-131">A hello `mysql` kérni, hozzon létre egy adatbázist és a táblát, hello Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="c6132-131">In hello `mysql` prompt, create a database and a table for hello to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

<span data-ttu-id="c6132-132">Lépjen ki a kiszolgálói kapcsolatot írja be a `quit`.</span><span class="sxs-lookup"><span data-stu-id="c6132-132">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="create-and-run-hello-sample-app"></a><span data-ttu-id="c6132-133">Hozzon létre és hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="c6132-133">Create and run hello sample app</span></span> 

<span data-ttu-id="c6132-134">Ebben a lépésben klónozza a mintaalkalmazást rugó rendszerindító, toouse hello helyi MySQL-adatbázis konfigurálja, és futtassa azt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c6132-134">In this step, you clone sample Spring boot app, configure it toouse hello local MySQL database, and run it on your computer.</span></span> 

### <a name="clone-hello-sample"></a><span data-ttu-id="c6132-135">Klónozás hello minta</span><span class="sxs-lookup"><span data-stu-id="c6132-135">Clone hello sample</span></span>

<span data-ttu-id="c6132-136">Hello terminálablakot lépjen a tooa hello minta directory és a klónozott tárház működik.</span><span class="sxs-lookup"><span data-stu-id="c6132-136">In hello terminal window, navigate tooa working directory and clone hello sample repository.</span></span> 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-hello-app-toouse-hello-mysql-database"></a><span data-ttu-id="c6132-137">Hello app toouse hello MySQL-adatbázis konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6132-137">Configure hello app toouse hello MySQL database</span></span>

<span data-ttu-id="c6132-138">Frissítés hello `spring.datasource.password` és értéket *spring-boot-mysql-todo/src/main/resources/application.properties* hello ugyanazon gyökér szintű jelszó használt tooopen hello MySQL parancssorba:</span><span class="sxs-lookup"><span data-stu-id="c6132-138">Update hello `spring.datasource.password` and  value in *spring-boot-mysql-todo/src/main/resources/application.properties* with hello same root password used tooopen hello MySQL prompt:</span></span>

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-hello-sample"></a><span data-ttu-id="c6132-139">Hozza létre és futtasson hello mintát</span><span class="sxs-lookup"><span data-stu-id="c6132-139">Build and run hello sample</span></span>

<span data-ttu-id="c6132-140">Build, és futtassa a hello mintákkal hello Maven burkoló hello tárházban tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="c6132-140">Build and run hello sample using hello Maven wrapper included in hello repo:</span></span>

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

<span data-ttu-id="c6132-141">Nyissa meg a böngésző toohttp://localhost:8080 toosee hello minta működés közben.</span><span class="sxs-lookup"><span data-stu-id="c6132-141">Open your browser toohttp://localhost:8080 toosee in hello sample in action.</span></span> <span data-ttu-id="c6132-142">Toohello tevékenységlistájának hozzáadásakor, használja a következő SQL parancsot az hello MySQL Rákérdezés tooview hello a adataihoz MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="c6132-142">As you add tasks toohello list,  use hello following SQL commands in hello MySQL prompt tooview hello data stored in MySQL.</span></span>

```SQL
use testdb;
select * from todo_item;
```

<span data-ttu-id="c6132-143">Állítsa le a hello alkalmazást úgy, hogy elérte-e `Ctrl` + `C` a Terminálszolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="c6132-143">Stop hello application by hitting `Ctrl`+`C` in hello terminal.</span></span> 

## <a name="create-an-azure-mysql-database"></a><span data-ttu-id="c6132-144">Hozzon létre egy Azure-beli MySQL database</span><span class="sxs-lookup"><span data-stu-id="c6132-144">Create an Azure MySQL database</span></span>

<span data-ttu-id="c6132-145">Ebben a lépésben létrehoz egy [MySQL az Azure-adatbázis](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) példányhoz hello használatával [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c6132-145">In this step, you create an [Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instance using hello [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="c6132-146">Konfigurált hello minta alkalmazás toouse ezt az adatbázist később hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c6132-146">You configure hello sample application toouse this database later on in hello tutorial.</span></span>

<span data-ttu-id="c6132-147">Használjon hello Azure CLI 2.0 egy terminálablakot toocreate hello erőforrások a szükséges toohost a Java-alkalmazások az Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c6132-147">Use hello Azure CLI 2.0 in a terminal window toocreate hello resources needed toohost your Java application in Azure appservice.</span></span> <span data-ttu-id="c6132-148">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="c6132-148">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="c6132-149">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="c6132-149">Create a resource group</span></span>

<span data-ttu-id="c6132-150">Hozzon létre egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c6132-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="c6132-151">Egy Azure erőforráscsoport egy olyan logikai tároló, ahol a kapcsolódó erőforrások, például webalkalmazások, adatbázisok és a storage-fiókok telepítése és kezelése.</span><span class="sxs-lookup"><span data-stu-id="c6132-151">An Azure resource group is a logical container where related resources like web apps, databases, and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="c6132-152">hello alábbi példa létrehoz egy erőforráscsoport hello Észak-Európa régióban:</span><span class="sxs-lookup"><span data-stu-id="c6132-152">hello following example creates a resource group in hello North Europe region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

<span data-ttu-id="c6132-153">toosee hello a lehetséges értékek is használhatja a `--location`, használja a hello [az App Service lista-helyek](/cli/azure/appservice#list-locations) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c6132-153">toosee hello possible values you can use for `--location`, use hello [az appservice list-locations](/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-a-mysql-server"></a><span data-ttu-id="c6132-154">A MySQL-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="c6132-154">Create a MySQL server</span></span>

<span data-ttu-id="c6132-155">A kiszolgáló létrehozása az Azure-adatbázisban a MySQL (előzetes verzió) hello [az mysql kiszolgáló létrehozni](/cli/azure/mysql/server#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c6132-155">Create a server in Azure Database for MySQL (Preview) with hello [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>    
<span data-ttu-id="c6132-156">A saját egyedi MySQL kiszolgáló nevét, ahol látható hello `<mysql_server_name>` helyőrző.</span><span class="sxs-lookup"><span data-stu-id="c6132-156">Substitute your own unique MySQL server name where you see hello `<mysql_server_name>` placeholder.</span></span> <span data-ttu-id="c6132-157">Ez a név része a MySQL-kiszolgáló állomásneve, `<mysql_server_name>.mysql.database.azure.com`, így a toobe globálisan egyedi kell.</span><span class="sxs-lookup"><span data-stu-id="c6132-157">This name is part of your MySQL server's hostname, `<mysql_server_name>.mysql.database.azure.com`, so it needs toobe globally unique.</span></span> <span data-ttu-id="c6132-158">Is `<admin_user>` és `<admin_password>` saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="c6132-158">Also substitute `<admin_user>` and `<admin_password>` with your own values.</span></span>

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

<span data-ttu-id="c6132-159">Hello MySQL kiszolgáló akkor jön létre, amikor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="c6132-159">When hello MySQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="configure-server-firewall"></a><span data-ttu-id="c6132-160">Kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6132-160">Configure server firewall</span></span>

<span data-ttu-id="c6132-161">Hozzon létre egy tűzfalszabályt a MySQL server tooallow ügyfél kapcsolatok hello segítségével [az mysql-tűzfalszabály létrehozása](/cli/azure/mysql/server/firewall-rule#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c6132-161">Create a firewall rule for your MySQL server tooallow client connections by using hello [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span> 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="c6132-162">MySQL (előzetes verzió) Azure-adatbázis automatikusan jelenleg nem engedélyezi a Azure-szolgáltatásokhoz érkező kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="c6132-162">Azure Database for MySQL (Preview) does not currently automatically enable connections from Azure services.</span></span> <span data-ttu-id="c6132-163">Dinamikusan kiosztott IP-címek az Azure-ban, mert már jobb tooenable összes IP-címek most.</span><span class="sxs-lookup"><span data-stu-id="c6132-163">As IP addresses in Azure are dynamically assigned, it is better tooenable all IP addresses for now.</span></span> <span data-ttu-id="c6132-164">Hello szolgáltatás folyamatosan mintájának, biztonságossá tétele az adatbázis jobban módszereinek engedélyezve lesz.</span><span class="sxs-lookup"><span data-stu-id="c6132-164">As hello service continues its preview, better methods for securing your database will be enabled.</span></span>

## <a name="configure-hello-azure-mysql-database"></a><span data-ttu-id="c6132-165">Konfigurálja a hello Azure-beli MySQL database</span><span class="sxs-lookup"><span data-stu-id="c6132-165">Configure hello Azure MySQL database</span></span>

<span data-ttu-id="c6132-166">A hello terminálablakot a számítógépen csatlakozzon a toohello MySQL server az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c6132-166">In hello terminal window on your computer, connect toohello MySQL server in Azure.</span></span> <span data-ttu-id="c6132-167">Használja a korábban megadott hello értéket `<admin_user>` és `<mysql_server_name>`.</span><span class="sxs-lookup"><span data-stu-id="c6132-167">Use hello value you specified previously for `<admin_user>` and `<mysql_server_name>`.</span></span>

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a><span data-ttu-id="c6132-168">Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="c6132-168">Create a database</span></span> 

<span data-ttu-id="c6132-169">A hello `mysql` kérni, hozzon létre egy adatbázist és a táblát, hello Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="c6132-169">In hello `mysql` prompt, create a database and a table for hello to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="c6132-170">Hozzon létre egy felhasználói jogosultságokkal</span><span class="sxs-lookup"><span data-stu-id="c6132-170">Create a user with permissions</span></span>

<span data-ttu-id="c6132-171">Hozzon létre egy adatbázis-felhasználó, és adjon neki a hello minden jogosultság `tododb` adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c6132-171">Create a database user and give it all privileges in hello `tododb` database.</span></span> <span data-ttu-id="c6132-172">Cserélje le a helyőrzőket hello `<Javaapp_user>` és `<Javaapp_password>` a saját egyedi alkalmazás nevével.</span><span class="sxs-lookup"><span data-stu-id="c6132-172">Replace hello placeholders `<Javaapp_user>` and `<Javaapp_password>` with your own unique app name.</span></span>

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* too'<Javaapp_user>';
```

<span data-ttu-id="c6132-173">Lépjen ki a kiszolgálói kapcsolatot írja be a `quit`.</span><span class="sxs-lookup"><span data-stu-id="c6132-173">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="deploy-hello-sample-tooazure-app-service"></a><span data-ttu-id="c6132-174">Hello minta tooAzure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="c6132-174">Deploy hello sample tooAzure App Service</span></span>

<span data-ttu-id="c6132-175">Hozzon létre az Azure App Service-csomag hello **szabad** hello segítségével tarifacsomag [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="c6132-175">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="c6132-176">hello App Service-csomagot határoz meg hello használt fizikai erőforrások toohost az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c6132-176">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="c6132-177">App Service-csomagot tooan hozzárendelt összes alkalmazás ossza meg ezeket az erőforrásokat, így toosave költség több alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="c6132-177">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="c6132-178">Amikor készen áll a hello terv, hello Azure CLI hasonló látható kimeneti toohello a következő példa:</span><span class="sxs-lookup"><span data-stu-id="c6132-178">When hello plan is ready, hello Azure CLI shows similar output toohello following example:</span></span>

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

### <a name="create-an-azure-web-app"></a><span data-ttu-id="c6132-179">Azure-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c6132-179">Create an Azure Web app</span></span>

 <span data-ttu-id="c6132-180">Használjon hello [az webalkalmazás létrehozása](/cli/azure/appservice/web#create) CLI parancs toocreate egy web app-definíciót a hello `myAppServicePlan` App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="c6132-180">Use hello [az webapp create](/cli/azure/appservice/web#create) CLI command toocreate a web app definition in hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="c6132-181">hello web app-definíciót tartalmaz egy URL-cím tooaccess az alkalmazásba, és konfigurálja a több beállítások toodeploy a kód tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c6132-181">hello web app definition provides a URL tooaccess your application with and configures several options toodeploy your code tooAzure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="c6132-182">Helyettesítő hello `<app_name>` helyőrző a saját egyedi alkalmazás nevével.</span><span class="sxs-lookup"><span data-stu-id="c6132-182">Substitute hello `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="c6132-183">A egyedi név része hello alapértelmezett tartomány nevét hello webalkalmazás, így hello nevének kell toobe egyedi az Azure-ban minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="c6132-183">This unique name is part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="c6132-184">Ki kell alakítani az tooyour felhasználók hozzárendelhető egy egyéni tartomány nevét bejegyzés toohello webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c6132-184">You can map a custom domain name entry toohello web app before you expose it tooyour users.</span></span>

<span data-ttu-id="c6132-185">Amikor készen áll a hello web app-definíciót, hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="c6132-185">When hello web app definition is ready, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-java"></a><span data-ttu-id="c6132-186">Java konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6132-186">Configure Java</span></span> 

<span data-ttu-id="c6132-187">Hello Java runtime konfiguráció, amelyet az alkalmazás a hello [az App Service web config frissítés](/cli/azure/appservice/web/config#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c6132-187">Set up hello Java runtime configuration that your app needs with hello  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="c6132-188">hello következő parancsot konfigurálása hello web app toorun a legújabb Java 8 JDK és [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="c6132-188">hello following command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-hello-app-toouse-hello-azure-sql-database"></a><span data-ttu-id="c6132-189">Hello app toouse hello Azure SQL adatbázis konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6132-189">Configure hello app toouse hello Azure SQL database</span></span>

<span data-ttu-id="c6132-190">Mielőtt futtatná hello mintaalkalmazást, hello web app toouse hello Azure-beli MySQL database az Azure-ban létrehozott Alkalmazásbeállítások be.</span><span class="sxs-lookup"><span data-stu-id="c6132-190">Before running hello sample app, set application settings on hello web app toouse hello Azure MySQL database you created in Azure.</span></span> <span data-ttu-id="c6132-191">Ezek a Tulajdonságok kitett toohello webalkalmazás környezeti változóként és hello application.properties belül hello csomagolt webalkalmazás beállított hello érték felülírására.</span><span class="sxs-lookup"><span data-stu-id="c6132-191">These properties are exposed toohello web application as environment variables and override hello values set in hello application.properties inside hello packaged web app.</span></span> 

<span data-ttu-id="c6132-192">Állítsa be az alkalmazás a beállításokat a [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) a hello CLI:</span><span class="sxs-lookup"><span data-stu-id="c6132-192">Set application settings using [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) in hello CLI:</span></span>

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

### <a name="get-ftp-deployment-credentials"></a><span data-ttu-id="c6132-193">FTP telepítési hitelesítő adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="c6132-193">Get FTP deployment credentials</span></span> 
<span data-ttu-id="c6132-194">Az alkalmazás tooAzure App Service többek között az FTP, helyi Git, GitHub, a Visual Studio Team Services és a Bitbucketből különböző módokon telepítheti.</span><span class="sxs-lookup"><span data-stu-id="c6132-194">You can deploy your application tooAzure appservice in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="c6132-195">Ebben a példában az FTP-toodeploy hello. A helyi számítógép tooAzure App Service korábban épülő WAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="c6132-195">For this example, FTP toodeploy hello .WAR file built previously on your local machine tooAzure App Service.</span></span>

<span data-ttu-id="c6132-196">toodetermine toopass mentén egy ftp parancs toohello Web App alkalmazásban használja a milyen hitelesítő adatok [az App Service web deployment lista-közzététel-profilok](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) parancs:</span><span class="sxs-lookup"><span data-stu-id="c6132-196">toodetermine what credentials toopass along in an ftp command toohello Web App, Use [az appservice web deployment list-publishing-profiles](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) command:</span></span> 

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

### <a name="upload-hello-app-using-ftp"></a><span data-ttu-id="c6132-197">A FTP hello alkalmazás feltöltése</span><span class="sxs-lookup"><span data-stu-id="c6132-197">Upload hello app using FTP</span></span>

<span data-ttu-id="c6132-198">A kedvenc FTP eszköz toodeploy hello használata. WAR-fájl toohello */site/wwwroot/webapps* hello vett hello kiszolgálócímet mappájába `URL` hello előző parancs mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c6132-198">Use your favorite FTP tool toodeploy hello .WAR file toohello */site/wwwroot/webapps* folder on hello server address taken from hello `URL` field in hello previous command.</span></span> <span data-ttu-id="c6132-199">Hello meglévő alapértelmezett (ROOT) alkalmazás könyvtár eltávolítása, és cserélje le a meglévő ROOT.war a hello hello. Hello az oktatóanyag során korábban küldje el a hello beépített WAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="c6132-199">Remove hello existing default (ROOT) application directory and replace hello existing ROOT.war with hello .WAR file built in hello earlier in hello tutorial.</span></span>

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

### <a name="test-hello-web-app"></a><span data-ttu-id="c6132-200">Teszt hello webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="c6132-200">Test hello web app</span></span>

<span data-ttu-id="c6132-201">Keresse meg a túl`http://<app_name>.azurewebsites.net/` és néhány feladatok toohello listát vesznek fel.</span><span class="sxs-lookup"><span data-stu-id="c6132-201">Browse too`http://<app_name>.azurewebsites.net/` and add a few tasks toohello list.</span></span> 

![Java-alkalmazás fusson az Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="c6132-203">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="c6132-203">**Congratulations!**</span></span> <span data-ttu-id="c6132-204">Adatalapú Java-alkalmazást használ az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="c6132-204">You're running a data-driven Java app in Azure App Service.</span></span>

## <a name="update-hello-app-and-redeploy"></a><span data-ttu-id="c6132-205">Frissítés hello alkalmazást, és helyezze üzembe újra</span><span class="sxs-lookup"><span data-stu-id="c6132-205">Update hello app and redeploy</span></span>

<span data-ttu-id="c6132-206">Frissítés hello alkalmazás tooinclude egy további oszlopot az hello todo listában milyen nap hello elem lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="c6132-206">Update hello application tooinclude an additional column in hello todo list for what day hello item was created.</span></span> <span data-ttu-id="c6132-207">Rugó rendszerindító kezeli a frissítési hello adatbázisséma meg hello adatok modell módosítás a meglévő üzenetadatbázis-rekordok módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="c6132-207">Spring Boot handles updating hello database schema for you as hello data model changes without altering your existing database records.</span></span>

1. <span data-ttu-id="c6132-208">A helyi számítógépen nyissa meg *src/main/java/com/example/fabrikam/TodoItem.java* , és adja hozzá a következő hello importálja toohello osztály:</span><span class="sxs-lookup"><span data-stu-id="c6132-208">On your local system, open up *src/main/java/com/example/fabrikam/TodoItem.java* and add hello following imports toohello class:</span></span>   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. <span data-ttu-id="c6132-209">Adja hozzá a `String` tulajdonság `timeCreated` túl*src/main/java/com/example/fabrikam/TodoItem.java*, inicializálja azt az időbélyegzőnek a objektum létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="c6132-209">Add a `String` property `timeCreated` too*src/main/java/com/example/fabrikam/TodoItem.java*, initializing it with a timestamp at object creation.</span></span> <span data-ttu-id="c6132-210">Adja hozzá az új hello beolvasókat/Setter elemek `timeCreated` tulajdonság a fájl szerkesztése közben.</span><span class="sxs-lookup"><span data-stu-id="c6132-210">Add getters/setters for hello new `timeCreated` property while you are editing this file.</span></span>

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

3. <span data-ttu-id="c6132-211">Frissítés *src/main/java/com/example/fabrikam/TodoDemoController.java* a hello vonallal `updateTodo` metódus tooset hello időbélyeg:</span><span class="sxs-lookup"><span data-stu-id="c6132-211">Update *src/main/java/com/example/fabrikam/TodoDemoController.java* with a line in hello `updateTodo` method tooset hello timestamp:</span></span>

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. <span data-ttu-id="c6132-212">Új mező hello támogatásáról hello Thymeleaf sablonban.</span><span class="sxs-lookup"><span data-stu-id="c6132-212">Add support for hello new field in hello Thymeleaf template.</span></span> <span data-ttu-id="c6132-213">Frissítés *src/main/resources/templates/index.html* hello timestamp, és egy új toodisplay hello mezőérték hello Timestamp az egyes táblazatsorok adatokat egy új tábla fejléccel.</span><span class="sxs-lookup"><span data-stu-id="c6132-213">Update *src/main/resources/templates/index.html* with a new table header for hello timestamp, and a new field toodisplay hello value of hello timestamp in each table data row.</span></span>

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

5. <span data-ttu-id="c6132-214">Építse újra hello alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="c6132-214">Rebuild hello application:</span></span>

    ```bash
    mvnw clean package 
    ```

6. <span data-ttu-id="c6132-215">FTP-hello frissítve. Hello meglévő eltávolítása előtt, mint WAR *hely/wwwroot/webapps/ROOT* könyvtár és *ROOT.war*, majd a frissített hello feltöltése. Mint ROOT.war WAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="c6132-215">FTP hello updated .WAR as before, removing hello existing *site/wwwroot/webapps/ROOT* directory and *ROOT.war*, then uploading hello updated .WAR file as ROOT.war.</span></span> 

<span data-ttu-id="c6132-216">Hello alkalmazás frissítésekor egy **alkalommal létre** oszlop már látható.</span><span class="sxs-lookup"><span data-stu-id="c6132-216">When you refresh hello app, a **Time Created** column is now visible.</span></span> <span data-ttu-id="c6132-217">Ha hozzáad egy új feladatot, hello alkalmazás automatikusan hello időbélyeg fogja majd feltölteni.</span><span class="sxs-lookup"><span data-stu-id="c6132-217">When you add a new task, hello app will populate hello timestamp automatically.</span></span> <span data-ttu-id="c6132-218">A meglévő feladatokat továbbra is változatlan és a munkahelyi hello alkalmazással annak ellenére, hogy az alapul szolgáló adatmodellt hello megváltozott.</span><span class="sxs-lookup"><span data-stu-id="c6132-218">Your existing tasks remain unchanged and work with hello app even though hello underlying data model has changed.</span></span> 

![Java-alkalmazást egy olyan új oszlop frissült](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a><span data-ttu-id="c6132-220">Diagnosztikai naplók adatfolyam</span><span class="sxs-lookup"><span data-stu-id="c6132-220">Stream diagnostic logs</span></span> 

<span data-ttu-id="c6132-221">A Java-alkalmazás futtatása az Azure App Service-ben, közben kaphat a hello konzol naplók adatcsatornán közvetlenül a Terminálszolgáltatások tooyour.</span><span class="sxs-lookup"><span data-stu-id="c6132-221">While your Java application runs in Azure App Service, you can get hello console logs piped directly tooyour terminal.</span></span> <span data-ttu-id="c6132-222">Ily módon kaphat hello ugyanazon diagnosztikai üzenetek toohelp hibakeresése az alkalmazáshibák.</span><span class="sxs-lookup"><span data-stu-id="c6132-222">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="c6132-223">streamelés esetén használjon hello toostart napló [az webapp napló végéről](/cli/azure/appservice/web/log#tail) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c6132-223">toostart log streaming, use hello [az webapp log tail](/cli/azure/appservice/web/log#tail) command.</span></span>

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="c6132-224">Az Azure-webalkalmazásban kezelése</span><span class="sxs-lookup"><span data-stu-id="c6132-224">Manage your Azure web app</span></span>

<span data-ttu-id="c6132-225">Nyissa meg toohello az Azure portál toosee hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c6132-225">Go toohello Azure portal toosee hello web app you created.</span></span>

<span data-ttu-id="c6132-226">toodo, jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c6132-226">toodo this, sign in too[https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="c6132-227">Hello bal oldali menüben kattintson **App Service**, majd kattintson az Azure-webalkalmazásban hello nevére.</span><span class="sxs-lookup"><span data-stu-id="c6132-227">From hello left menu, click **App Service**, then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-java-mysql/access-portal.png)

<span data-ttu-id="c6132-229">Alapértelmezés szerint a webalkalmazás panelen látható hello **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="c6132-229">By default, your web app's blade shows hello **Overview** page.</span></span> <span data-ttu-id="c6132-230">Ezen az oldalon megtekintheti az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="c6132-230">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="c6132-231">Itt is elvégezheti stop, kezdő, újraindítását és delete felügyeleti feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="c6132-231">Here, you can also perform management tasks like stop, start, restart, and delete.</span></span> <span data-ttu-id="c6132-232">hello lapokon hello bal oldalán hello panelen láthatók hello különböző konfigurációs lapok is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="c6132-232">hello tabs on hello left side of hello blade show hello different configuration pages you can open.</span></span>

![Az App Service panel az Azure Portalon](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

<span data-ttu-id="c6132-234">A lapok hello panelen megjelenítése hello számos nagy szolgáltatás tooyour webes alkalmazás is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="c6132-234">These tabs in hello blade show hello many great features you can add tooyour web app.</span></span> <span data-ttu-id="c6132-235">a következő lista hello hello lehetőségeket néhány következő:</span><span class="sxs-lookup"><span data-stu-id="c6132-235">hello following list gives you just a few of hello possibilities:</span></span>
* <span data-ttu-id="c6132-236">Egyéni DNS-név leképezése</span><span class="sxs-lookup"><span data-stu-id="c6132-236">Map a custom DNS name</span></span>
* <span data-ttu-id="c6132-237">Egyéni SSL-tanúsítvány kötése</span><span class="sxs-lookup"><span data-stu-id="c6132-237">Bind a custom SSL certificate</span></span>
* <span data-ttu-id="c6132-238">Folyamatos üzembe helyezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6132-238">Configure continuous deployment</span></span>
* <span data-ttu-id="c6132-239">Vertikális felskálázás és kibővítés</span><span class="sxs-lookup"><span data-stu-id="c6132-239">Scale up and out</span></span>
* <span data-ttu-id="c6132-240">Felhasználói hitelesítés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c6132-240">Add user authentication</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="c6132-241">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c6132-241">Clean up resources</span></span>

<span data-ttu-id="c6132-242">Ha ezeket az erőforrásokat egy másik az oktatóanyaghoz nincs szüksége (lásd: [további lépések](#next)), törölheti azokat a hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c6132-242">If you don't need these resources for another tutorial (see [Next steps](#next)), you can delete them by running hello following command:</span></span> 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="c6132-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6132-243">Next steps</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c6132-244">MySQL-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c6132-244">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="c6132-245">Csatlakozás egy minta Java-alkalmazás toohello MySQL</span><span class="sxs-lookup"><span data-stu-id="c6132-245">Connect a sample Java app toohello MySQL</span></span>
> * <span data-ttu-id="c6132-246">Hello app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="c6132-246">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="c6132-247">Frissítse és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="c6132-247">Update and redeploy hello app</span></span>
> * <span data-ttu-id="c6132-248">Az adatfolyam diagnosztikai naplók az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="c6132-248">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="c6132-249">Az Azure-portálon hello hello alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="c6132-249">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="c6132-250">Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name toohello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c6132-250">Advance toohello next tutorial toolearn how toomap a custom DNS name toohello app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="c6132-251">Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése</span><span class="sxs-lookup"><span data-stu-id="c6132-251">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
