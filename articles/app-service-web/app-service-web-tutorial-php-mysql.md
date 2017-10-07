---
title: "a PHP és a MySQL webalkalmazás az Azure-ban aaaBuild |} Microsoft Docs"
description: "Megtudhatja, hogyan tooget egy PHP-alkalmazás az Azure-ban működik, a kapcsolat tooa MySQL-adatbázis az Azure-ban."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 14feb4f3-5095-496e-9a40-690e1414bd73
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 07/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 3c050b30e2e2c80d011bed989cd5f8cecac35d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a><span data-ttu-id="e9717-103">Az Azure-ban a PHP és a MySQL webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9717-103">Build a PHP and MySQL web app in Azure</span></span>

<span data-ttu-id="e9717-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e9717-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="e9717-105">Ez az oktatóanyag bemutatja, hogyan toocreate egy PHP webalkalmazás az Azure-ban, és csatlakoztassa tooa MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e9717-105">This tutorial shows how toocreate a PHP web app in Azure and connect it tooa MySQL database.</span></span> <span data-ttu-id="e9717-106">Amikor végzett, konfigurálnia kell egy [Laravel](https://laravel.com/) Azure App Service Web Apps futó alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="e9717-106">When you're finished, you'll have a [Laravel](https://laravel.com/) app running on Azure App Service Web Apps.</span></span>

![PHP-alkalmazás fusson az Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="e9717-108">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="e9717-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e9717-109">MySQL-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e9717-109">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="e9717-110">Csatlakozás egy PHP-alkalmazás tooMySQL</span><span class="sxs-lookup"><span data-stu-id="e9717-110">Connect a PHP app tooMySQL</span></span>
> * <span data-ttu-id="e9717-111">Hello app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="e9717-111">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="e9717-112">Hello adatmodell frissítése, és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="e9717-112">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="e9717-113">Az adatfolyam diagnosztikai naplók az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="e9717-113">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="e9717-114">Az Azure-portálon hello hello alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="e9717-114">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9717-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e9717-115">Prerequisites</span></span>

<span data-ttu-id="e9717-116">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="e9717-116">toocomplete this tutorial:</span></span>

* [<span data-ttu-id="e9717-117">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="e9717-117">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="e9717-118">Telepítse a PHP 5.6.4 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="e9717-118">Install PHP 5.6.4 or above</span></span>](http://php.net/downloads.php)
* [<span data-ttu-id="e9717-119">Szerkesztő telepítése</span><span class="sxs-lookup"><span data-stu-id="e9717-119">Install Composer</span></span>](https://getcomposer.org/doc/00-intro.md)
* <span data-ttu-id="e9717-120">Engedélyezze a következő PHP-bővítmények Laravel igényeinek hello: OpenSSL, OEM-MySQL, Mbstring, jogkivonatokat létrehozó, XML</span><span class="sxs-lookup"><span data-stu-id="e9717-120">Enable hello following PHP extensions Laravel needs: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML</span></span>
* [<span data-ttu-id="e9717-121">Telepítse és indítsa el a MySQL</span><span class="sxs-lookup"><span data-stu-id="e9717-121">Install and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a><span data-ttu-id="e9717-122">Készítse elő a helyi MySQL</span><span class="sxs-lookup"><span data-stu-id="e9717-122">Prepare local MySQL</span></span>

<span data-ttu-id="e9717-123">Ebben a lépésben egy adatbázis létrehozása a helyi MySQL Server ebben az oktatóanyagban a használatra.</span><span class="sxs-lookup"><span data-stu-id="e9717-123">In this step, you create a database in your local MySQL server for your use in this tutorial.</span></span>

### <a name="connect-toolocal-mysql-server"></a><span data-ttu-id="e9717-124">Csatlakoztassa a kiszolgálót toolocal MySQL</span><span class="sxs-lookup"><span data-stu-id="e9717-124">Connect toolocal MySQL server</span></span>

<span data-ttu-id="e9717-125">Egy terminálablakot tooyour helyi MySQL kiszolgálóhoz csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="e9717-125">In a terminal window, connect tooyour local MySQL server.</span></span> <span data-ttu-id="e9717-126">A terminálablakot toorun összes hello parancsokat használhatja az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="e9717-126">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="e9717-127">Ha a számítógép a jelszót, adja meg a jelszót hello hello `root` fiók.</span><span class="sxs-lookup"><span data-stu-id="e9717-127">If you're prompted for a password, enter hello password for hello `root` account.</span></span> <span data-ttu-id="e9717-128">Ha nem emlékszik a rendszergazdafiók jelszavával, lásd: [MySQL: hogyan tooReset hello gyökér szintű jelszó](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="e9717-128">If you don't remember your root account password, see [MySQL: How tooReset hello Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="e9717-129">Ha a parancs sikeresen lefutott, majd MySQL fut a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e9717-129">If your command runs successfully, then your MySQL server is running.</span></span> <span data-ttu-id="e9717-130">Ha nem, győződjön meg arról, hogy elindult-e a helyi MySQL-kiszolgáló által a következő hello [MySQL telepítés utáni lépéseket](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="e9717-130">If not, make sure that your local MySQL server is started by following hello [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database-locally"></a><span data-ttu-id="e9717-131">Helyi adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9717-131">Create a database locally</span></span>

<span data-ttu-id="e9717-132">A hello `mysql` kérdés, hozzon létre egy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="e9717-132">At hello `mysql` prompt, create a database.</span></span>

```sql 
CREATE DATABASE sampledb;
```

<span data-ttu-id="e9717-133">Lépjen ki a kiszolgálói kapcsolatot írja be a `quit`.</span><span class="sxs-lookup"><span data-stu-id="e9717-133">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a><span data-ttu-id="e9717-134">Helyileg egy PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9717-134">Create a PHP app locally</span></span>
<span data-ttu-id="e9717-135">Ebben a lépésben Laravel mintaalkalmazás beolvasása, az adatbázis-kapcsolat konfigurálása, és futtassa helyileg.</span><span class="sxs-lookup"><span data-stu-id="e9717-135">In this step, you get a Laravel sample application, configure its database connection, and run it locally.</span></span> 

### <a name="clone-hello-sample"></a><span data-ttu-id="e9717-136">Klónozás hello minta</span><span class="sxs-lookup"><span data-stu-id="e9717-136">Clone hello sample</span></span>

<span data-ttu-id="e9717-137">A hello terminálablakot `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="e9717-137">In hello terminal window, `cd` tooa working directory.</span></span>

<span data-ttu-id="e9717-138">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="e9717-138">Run hello following command tooclone hello sample repository.</span></span>

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

<span data-ttu-id="e9717-139">`cd`tooyour klónozott könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e9717-139">`cd` tooyour cloned directory.</span></span>
<span data-ttu-id="e9717-140">Telepítse a szükséges hello csomagok.</span><span class="sxs-lookup"><span data-stu-id="e9717-140">Install hello required packages.</span></span>

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a><span data-ttu-id="e9717-141">MySQL-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="e9717-141">Configure MySQL connection</span></span>

<span data-ttu-id="e9717-142">Hello adattár gyökérkönyvtárában, hozzon létre egy fájlt *.env*.</span><span class="sxs-lookup"><span data-stu-id="e9717-142">In hello repository root, create a file named *.env*.</span></span> <span data-ttu-id="e9717-143">Változók hello be a következő másolási hello *.env* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e9717-143">Copy hello following variables into hello *.env* file.</span></span> <span data-ttu-id="e9717-144">Cserélje le a hello  _&lt;root_password >_ helyőrzőt hello MySQL gyökér szintű felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e9717-144">Replace hello _&lt;root_password>_ placeholder with hello MySQL root user's password.</span></span>

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

<span data-ttu-id="e9717-145">Információk arról, hogyan Laravel használja hello _.env_ fájlba, lásd: [Laravel környezet konfigurációjának](https://laravel.com/docs/5.4/configuration#environment-configuration).</span><span class="sxs-lookup"><span data-stu-id="e9717-145">For information on how Laravel uses hello _.env_ file, see [Laravel Environment Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration).</span></span>

### <a name="run-hello-sample-locally"></a><span data-ttu-id="e9717-146">Futtassa helyben a hello minta</span><span class="sxs-lookup"><span data-stu-id="e9717-146">Run hello sample locally</span></span>

<span data-ttu-id="e9717-147">Futtatás [Laravel adatbázis áttelepítések](https://laravel.com/docs/5.4/migrations) toocreate hello táblák hello alkalmazások igényeihez.</span><span class="sxs-lookup"><span data-stu-id="e9717-147">Run [Laravel database migrations](https://laravel.com/docs/5.4/migrations) toocreate hello tables hello application needs.</span></span> <span data-ttu-id="e9717-148">mely táblázatok jönnek létre a hello áttelepítések, keresse meg a hello toosee _adatbázis/áttelepítések_ könyvtárban lévő hello Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="e9717-148">toosee which tables are created in hello migrations, look in hello _database/migrations_ directory in hello Git repository.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="e9717-149">Hozzon létre egy új Laravel alkalmazás kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e9717-149">Generate a new Laravel application key.</span></span>

```bash
php artisan key:generate
```

<span data-ttu-id="e9717-150">Hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e9717-150">Run hello application.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="e9717-151">Keresse meg a túl`http://localhost:8000` a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="e9717-151">Navigate too`http://localhost:8000` in a browser.</span></span> <span data-ttu-id="e9717-152">Hello lapon vennie néhány feladatot.</span><span class="sxs-lookup"><span data-stu-id="e9717-152">Add a few tasks in hello page.</span></span>

![A PHP sikeresen csatlakozik tooMySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="e9717-154">Írja be a PHP-toostop `Ctrl + C` a Terminálszolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="e9717-154">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a><span data-ttu-id="e9717-155">Hozzon létre MySQL az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e9717-155">Create MySQL in Azure</span></span>

<span data-ttu-id="e9717-156">Ebben a lépésben a MySQL-adatbázis létrehozása [MySQL (előzetes verzió) az Azure-adatbázis](/azure/mysql).</span><span class="sxs-lookup"><span data-stu-id="e9717-156">In this step, you create a MySQL database in [Azure Database for MySQL (Preview)](/azure/mysql).</span></span> <span data-ttu-id="e9717-157">Később akkor konfigurálja hello PHP alkalmazás tooconnect toothis adatbázist.</span><span class="sxs-lookup"><span data-stu-id="e9717-157">Later, you configure hello PHP application tooconnect toothis database.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="e9717-158">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="e9717-158">Create a resource group</span></span>

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a><span data-ttu-id="e9717-159">A MySQL-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9717-159">Create a MySQL server</span></span>

<span data-ttu-id="e9717-160">A kiszolgáló létrehozása az Azure-adatbázisban a MySQL (előzetes verzió) hello [az mysql kiszolgáló létrehozni](/cli/azure/mysql/server#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9717-160">Create a server in Azure Database for MySQL (Preview) with hello [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>

<span data-ttu-id="e9717-161">Hello a következő parancsot, helyettesítse a MySQL-kiszolgáló nevét, ahol látható hello  _&lt;mysql_server_name >_ helyőrző (érvényes karakterek: `a-z`, `0-9`, és `-`).</span><span class="sxs-lookup"><span data-stu-id="e9717-161">In hello following command, substitute your MySQL server name where you see hello _&lt;mysql_server_name>_ placeholder (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="e9717-162">Ez a név része hello MySQL kiszolgáló állomásneve (`<mysql_server_name>.database.windows.net`), toobe globálisan egyedi kell.</span><span class="sxs-lookup"><span data-stu-id="e9717-162">This name is part of hello MySQL server's hostname  (`<mysql_server_name>.database.windows.net`), it needs toobe globally unique.</span></span>

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --admin-user adminuser \
    --admin-password MySQLAzure2017
```

<span data-ttu-id="e9717-163">Hello MySQL kiszolgáló akkor jön létre, amikor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="e9717-163">When hello MySQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a><span data-ttu-id="e9717-164">Kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e9717-164">Configure server firewall</span></span>

<span data-ttu-id="e9717-165">Hozzon létre egy tűzfalszabályt a MySQL server tooallow ügyfél kapcsolatok hello segítségével [az mysql-tűzfalszabály létrehozása](/cli/azure/mysql/server/firewall-rule#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9717-165">Create a firewall rule for your MySQL server tooallow client connections by using hello [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span>

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="e9717-166">MySQL (előzetes verzió) Azure-adatbázis jelenleg nem korlátozza a kapcsolatokat csak tooAzure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e9717-166">Azure Database for MySQL (Preview) doesn't currently limit connections only tooAzure services.</span></span> <span data-ttu-id="e9717-167">Mivel a dinamikusan kiosztott IP-címek az Azure-ban,-e jobb tooenable összes IP-címet.</span><span class="sxs-lookup"><span data-stu-id="e9717-167">As IP addresses in Azure are dynamically assigned, it is better tooenable all IP addresses.</span></span> <span data-ttu-id="e9717-168">hello szolgáltatás előzetes verzió van.</span><span class="sxs-lookup"><span data-stu-id="e9717-168">hello service is in preview.</span></span> <span data-ttu-id="e9717-169">Az adatbázis védelmét biztosító jobb módszerek tervbe van véve.</span><span class="sxs-lookup"><span data-stu-id="e9717-169">Better methods for securing your database are planned.</span></span>
>
>

### <a name="connect-tooproduction-mysql-server-locally"></a><span data-ttu-id="e9717-170">Csatlakoztassa tooproduction MySQL a helyi kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="e9717-170">Connect tooproduction MySQL server locally</span></span>

<span data-ttu-id="e9717-171">A hello terminálablakot csatlakozzon a toohello MySQL server az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e9717-171">In hello terminal window, connect toohello MySQL server in Azure.</span></span> <span data-ttu-id="e9717-172">Használja a korábban megadott hello értéket  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="e9717-172">Use hello value you specified previously for _&lt;mysql_server_name>_.</span></span>

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

<span data-ttu-id="e9717-173">Ha a rendszer kéri a jelszót, _$tr0ngPa$ w0rd!_, a megadott hello adatbázis létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="e9717-173">When prompted for a password, use _$tr0ngPa$w0rd!_, which you specified when you created hello database.</span></span>

### <a name="create-a-production-database"></a><span data-ttu-id="e9717-174">Éles adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9717-174">Create a production database</span></span>

<span data-ttu-id="e9717-175">A hello `mysql` kérdés, hozzon létre egy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="e9717-175">At hello `mysql` prompt, create a database.</span></span>

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="e9717-176">Hozzon létre egy felhasználói jogosultságokkal</span><span class="sxs-lookup"><span data-stu-id="e9717-176">Create a user with permissions</span></span>

<span data-ttu-id="e9717-177">Néven adatbázis-felhasználó létrehozása _phpappuser_ , és adjon neki a hello minden jogosultság `sampledb` adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e9717-177">Create a database user called _phpappuser_ and give it all privileges in hello `sampledb` database.</span></span>

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* too'phpappuser';
```

<span data-ttu-id="e9717-178">Lépjen ki a hello kiszolgálókapcsolat beírásával `quit`.</span><span class="sxs-lookup"><span data-stu-id="e9717-178">Exit hello server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="connect-app-tooazure-mysql"></a><span data-ttu-id="e9717-179">Kapcsolódás app tooAzure MySQL</span><span class="sxs-lookup"><span data-stu-id="e9717-179">Connect app tooAzure MySQL</span></span>

<span data-ttu-id="e9717-180">Ebben a lépésben csatlakoztatja hello PHP alkalmazás toohello MySQL adatbázis MySQL (előzetes verzió) Azure-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e9717-180">In this step, you connect hello PHP application toohello MySQL database you created in Azure Database for MySQL (Preview).</span></span>

<a name="devconfig"></a>

### <a name="configure-hello-database-connection"></a><span data-ttu-id="e9717-181">Hello adatbázis-kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e9717-181">Configure hello database connection</span></span>

<span data-ttu-id="e9717-182">Hello adattár gyökérkönyvtárában, hozzon létre egy _. env.production_ változók bele a következő fájl, és másolja hello.</span><span class="sxs-lookup"><span data-stu-id="e9717-182">In hello repository root, create an _.env.production_ file and copy hello following variables into it.</span></span> <span data-ttu-id="e9717-183">Cserélje le a hello helyőrző  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="e9717-183">Replace hello placeholder _&lt;mysql_server_name>_.</span></span>

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
MYSQL_SSL=true
```

<span data-ttu-id="e9717-184">Hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="e9717-184">Save hello changes.</span></span>

> [!TIP]
> <span data-ttu-id="e9717-185">a MySQL kapcsolat adatait, a fájl már ki van zárva a Git-tárház hello toosecure (lásd: _.gitignore_ hello tárház gyökérkönyvtárában).</span><span class="sxs-lookup"><span data-stu-id="e9717-185">toosecure your MySQL connection information, this file is already excluded from hello Git repository (See _.gitignore_ in hello repository root).</span></span> <span data-ttu-id="e9717-186">Később megtudhatja, hogyan tooconfigure környezeti változók App Service tooconnect tooyour adatbázis Azure-adatbázisban a MySQL (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="e9717-186">Later, you learn how tooconfigure environment variables in App Service tooconnect tooyour database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="e9717-187">A környezeti változókat, akkor nem kell hello *.env* fájl az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="e9717-187">With environment variables, you don't need hello *.env* file in App Service.</span></span>
>

### <a name="configure-ssl-certificate"></a><span data-ttu-id="e9717-188">SSL-tanúsítvány konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e9717-188">Configure SSL certificate</span></span>

<span data-ttu-id="e9717-189">Alapértelmezés szerint a MySQL az Azure-adatbázis ügyfelektől érkező SSL-kapcsolatok érvénybe lépteti.</span><span class="sxs-lookup"><span data-stu-id="e9717-189">By default, Azure Database for MySQL enforces SSL connections from clients.</span></span> <span data-ttu-id="e9717-190">tooconnect tooyour MySQL-adatbázis az Azure-ban, kell használnia egy _.pem_ SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="e9717-190">tooconnect tooyour MySQL database in Azure, you must use a _.pem_ SSL certificate.</span></span>

<span data-ttu-id="e9717-191">Nyissa meg _config/database.php_ , és adja hozzá a hello _sslmode_ és _beállítások_ paraméterek túl`connections.mysql`, ahogy az a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="e9717-191">Open _config/database.php_ and add hello _sslmode_ and _options_ parameters too`connections.mysql`, as shown in hello following code.</span></span>

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

<span data-ttu-id="e9717-192">toolearn hogyan toogenerate ez _certificate.pem_, lásd: [konfigurálása az SSL-kapcsolat az alkalmazás toosecurely a MySQL adatbázis tooAzure kapcsolati](../mysql/howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="e9717-192">toolearn how toogenerate this _certificate.pem_, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](../mysql/howto-configure-ssl.md).</span></span>

> [!TIP]
> <span data-ttu-id="e9717-193">hello elérési _/ssl/certificate.pem_ tooan meglévő mutat _certificate.pem_ fájl hello Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="e9717-193">hello path _/ssl/certificate.pem_ points tooan existing _certificate.pem_ file in hello Git repository.</span></span> <span data-ttu-id="e9717-194">Ez a fájl ebben az oktatóanyagban a kényelem valósul meg.</span><span class="sxs-lookup"><span data-stu-id="e9717-194">This file is provided for convenience in this tutorial.</span></span> <span data-ttu-id="e9717-195">A legjobb, meg kell nem hajtsa végre a _.pem_ tanúsítványok forrás vezérlőbe.</span><span class="sxs-lookup"><span data-stu-id="e9717-195">For best practice, you should not commit your _.pem_ certificates into source control.</span></span> 
>

### <a name="test-hello-application-locally"></a><span data-ttu-id="e9717-196">Hello alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="e9717-196">Test hello application locally</span></span>

<span data-ttu-id="e9717-197">Futtassa a Laravel adatbázis áttelepítése az _. env.production_ , környezet toocreate hello táblák fájl a MySQL-adatbázisban az Azure Database hello MySQL (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="e9717-197">Run Laravel database migrations with _.env.production_ as hello environment file toocreate hello tables in your MySQL database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="e9717-198">Ne feledje, hogy _. env.production_ hello kapcsolati információk tooyour MySQL-adatbázis rendelkezik az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e9717-198">Remember that _.env.production_ has hello connection information tooyour MySQL database in Azure.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="e9717-199">_. env.production_ még nem rendelkezik érvényes kulcs.</span><span class="sxs-lookup"><span data-stu-id="e9717-199">_.env.production_ doesn't have a valid application key yet.</span></span> <span data-ttu-id="e9717-200">Az egy új terminál hello létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e9717-200">Generate a new one for it in hello terminal.</span></span>

```bash
php artisan key:generate --env=production --force
```

<span data-ttu-id="e9717-201">Futtassa a hello mintaalkalmazást _. env.production_ hello környezet fájlként.</span><span class="sxs-lookup"><span data-stu-id="e9717-201">Run hello sample application with _.env.production_ as hello environment file.</span></span>

```bash
php artisan serve --env=production
```

<span data-ttu-id="e9717-202">Keresse meg a túl`http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="e9717-202">Navigate too`http://localhost:8000`.</span></span> <span data-ttu-id="e9717-203">Hello értékkel nem jelenik meg hibaüzenet, ha hello PHP-alkalmazások az Azure-ban toohello MySQL-adatbázis kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="e9717-203">If hello page loads without errors, hello PHP application is connecting toohello MySQL database in Azure.</span></span>

<span data-ttu-id="e9717-204">Hello lapon vennie néhány feladatot.</span><span class="sxs-lookup"><span data-stu-id="e9717-204">Add a few tasks in hello page.</span></span>

![A PHP csatlakozás sikeresen tooAzure adatbázis MySQL (előzetes verzió)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="e9717-206">Írja be a PHP-toostop `Ctrl + C` a Terminálszolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="e9717-206">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

### <a name="commit-your-changes"></a><span data-ttu-id="e9717-207">A változtatások véglegesítése a határidő</span><span class="sxs-lookup"><span data-stu-id="e9717-207">Commit your changes</span></span>

<span data-ttu-id="e9717-208">Futtassa a következő Git-parancsok toocommit hello a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="e9717-208">Run hello following Git commands toocommit your changes:</span></span>

```bash
git add .
git commit -m "database.php updates"
```

<span data-ttu-id="e9717-209">Az alkalmazás készen áll a toobe telepítve.</span><span class="sxs-lookup"><span data-stu-id="e9717-209">Your app is ready toobe deployed.</span></span>

## <a name="deploy-tooazure"></a><span data-ttu-id="e9717-210">TooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="e9717-210">Deploy tooAzure</span></span>

<span data-ttu-id="e9717-211">Ebben a lépésben hello PHP MySQL-kompatibilis alkalmazás tooAzure App Service telepítése.</span><span class="sxs-lookup"><span data-stu-id="e9717-211">In this step, you deploy hello MySQL-connected PHP application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="e9717-212">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9717-212">Create an App Service plan</span></span>

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a><span data-ttu-id="e9717-213">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9717-213">Create a web app</span></span>

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)]

### <a name="set-hello-php-version"></a><span data-ttu-id="e9717-214">Hello PHP-verzió beállítása</span><span class="sxs-lookup"><span data-stu-id="e9717-214">Set hello PHP version</span></span>

<span data-ttu-id="e9717-215">Hello segítségével igényel, amely az alkalmazás hello hello PHP verzióját [az webapp konfiguráció](/cli/azure/webapp/config#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9717-215">Set hello PHP version that hello application requires by using hello [az webapp config set](/cli/azure/webapp/config#set) command.</span></span>

<span data-ttu-id="e9717-216">hello következő parancs beállítja a hello PHP verzió too_7.0_.</span><span class="sxs-lookup"><span data-stu-id="e9717-216">hello following command sets hello PHP version too_7.0_.</span></span>

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a><span data-ttu-id="e9717-217">Adatbázis-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e9717-217">Configure database settings</span></span>

<span data-ttu-id="e9717-218">Szerint korábban, csatlakoztathatja tooyour Azure-beli MySQL database App Service környezeti változók használatát.</span><span class="sxs-lookup"><span data-stu-id="e9717-218">As pointed out previously, you can connect tooyour Azure MySQL database using environment variables in App Service.</span></span>

<span data-ttu-id="e9717-219">Az App Service-ben, a környezeti változók beállítása _Alkalmazásbeállítások_ hello segítségével [az webapp appsettings konfiguráció](/cli/azure/webapp/config/appsettings#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9717-219">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span>

<span data-ttu-id="e9717-220">hello következő parancsot konfigurálása hello Alkalmazásbeállítások `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, és `DB_PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="e9717-220">hello following command configures hello app settings `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, and `DB_PASSWORD`.</span></span> <span data-ttu-id="e9717-221">Cserélje le a helyőrzőket hello  _&lt;alkalmazásnév >_ és  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="e9717-221">Replace hello placeholders _&lt;appname>_ and _&lt;mysql_server_name>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

<span data-ttu-id="e9717-222">Használhatja a hello PHP [getenv](http://www.php.net/manual/function.getenv.php) metódus tooaccess hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="e9717-222">You can use hello PHP [getenv](http://www.php.net/manual/function.getenv.php) method tooaccess hello settings.</span></span> <span data-ttu-id="e9717-223">hello Laravel kódot használja egy [env](https://laravel.com/docs/5.4/helpers#method-env) keresztül hello PHP burkoló `getenv`.</span><span class="sxs-lookup"><span data-stu-id="e9717-223">hello Laravel code uses an [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper over hello PHP `getenv`.</span></span> <span data-ttu-id="e9717-224">Például hello MySQL konfigurációját a _config/database.php_ hello a következő kódot a következőképpen néz:</span><span class="sxs-lookup"><span data-stu-id="e9717-224">For example, hello MySQL configuration in _config/database.php_ looks like hello following code:</span></span>

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a><span data-ttu-id="e9717-225">Környezeti változók Laravel konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e9717-225">Configure Laravel environment variables</span></span>

<span data-ttu-id="e9717-226">Laravel kell egy alkalmazás-kulcsot az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="e9717-226">Laravel needs an application key in App Service.</span></span> <span data-ttu-id="e9717-227">Alkalmazás beállításokkal konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="e9717-227">You can configure it with app settings.</span></span>

<span data-ttu-id="e9717-228">Használjon `php artisan` toogenerate egy új alkalmazás kulcs too_.env_ mentés nélkül.</span><span class="sxs-lookup"><span data-stu-id="e9717-228">Use `php artisan` toogenerate a new application key without saving it too_.env_.</span></span>

```bash
php artisan key:generate --show
```

<span data-ttu-id="e9717-229">Hello alkalmazás kulcsát állítsa a hello App Service webalkalmazás hello segítségével [az webapp appsettings konfiguráció](/cli/azure/webapp/config/appsettings#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9717-229">Set hello application key in hello App Service web app by using hello [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span> <span data-ttu-id="e9717-230">Cserélje le a helyőrzőket hello  _&lt;alkalmazásnév >_ és  _&lt;outputofphpartisankey: készítése >_.</span><span class="sxs-lookup"><span data-stu-id="e9717-230">Replace hello placeholders _&lt;appname>_ and _&lt;outputofphpartisankey:generate>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

<span data-ttu-id="e9717-231">`APP_DEBUG="true"`be van állítva Laravel tooreturn hibakeresési adatok hello webalkalmazás központi telepítési hibát észlel.</span><span class="sxs-lookup"><span data-stu-id="e9717-231">`APP_DEBUG="true"` tells Laravel tooreturn debugging information when hello deployed web app encounters errors.</span></span> <span data-ttu-id="e9717-232">Termelési alkalmazások futtatásakor állítsa be úgy túl`false`, amelyhez biztonságosabb.</span><span class="sxs-lookup"><span data-stu-id="e9717-232">When running a production application, set it too`false`, which is more secure.</span></span>

### <a name="set-hello-virtual-application-path"></a><span data-ttu-id="e9717-233">Set hello virtuális alkalmazás elérési útja</span><span class="sxs-lookup"><span data-stu-id="e9717-233">Set hello virtual application path</span></span>

<span data-ttu-id="e9717-234">Hello webalkalmazás hello virtuális alkalmazás elérési útjának beállítása.</span><span class="sxs-lookup"><span data-stu-id="e9717-234">Set hello virtual application path for hello web app.</span></span> <span data-ttu-id="e9717-235">Ez a lépés akkor szükséges, mert hello [Laravel alkalmazás életciklusa](https://laravel.com/docs/5.4/lifecycle) hello kezdődik _nyilvános_ könyvtár hello alkalmazás gyökérkönyvtárában helyett.</span><span class="sxs-lookup"><span data-stu-id="e9717-235">This step is required because hello [Laravel application lifecycle](https://laravel.com/docs/5.4/lifecycle) begins in hello _public_ directory instead of hello application's root directory.</span></span> <span data-ttu-id="e9717-236">Egyéb PHP keretrendszerek, amelynek életciklus start hello gyökérkönyvtárában hello alkalmazás virtuális elérési út kézi konfigurálása nélkül is működik.</span><span class="sxs-lookup"><span data-stu-id="e9717-236">Other PHP frameworks whose lifecycle start in hello root directory can work without manual configuration of hello virtual application path.</span></span>

<span data-ttu-id="e9717-237">Set hello virtuális alkalmazás elérési útja hello segítségével [az erőforrás frissítési](/cli/azure/resource#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9717-237">Set hello virtual application path by using hello [az resource update](/cli/azure/resource#update) command.</span></span> <span data-ttu-id="e9717-238">Cserélje le a hello  _&lt;alkalmazásnév >_ helyőrző.</span><span class="sxs-lookup"><span data-stu-id="e9717-238">Replace hello _&lt;appname>_ placeholder.</span></span>

```azurecli-interactive
az resource update \
    --name web \
    --resource-group myResourceGroup \
    --namespace Microsoft.Web \
    --resource-type config \
    --parent sites/<app_name> \
    --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" \
    --api-version 2015-06-01
```

<span data-ttu-id="e9717-239">Alapértelmezés szerint az Azure App Service mutat hello virtuális alkalmazás gyökérútvonalát (_/_) hello toohello gyökérkönyvtárában telepített alkalmazásfájlok (_sites\wwwroot_).</span><span class="sxs-lookup"><span data-stu-id="e9717-239">By default, Azure App Service points hello root virtual application path (_/_) toohello root directory of hello deployed application files (_sites\wwwroot_).</span></span>

### <a name="configure-a-deployment-user"></a><span data-ttu-id="e9717-240">Üzembe helyező felhasználó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e9717-240">Configure a deployment user</span></span>

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="configure-local-git-deployment"></a><span data-ttu-id="e9717-241">A Git helyi üzemelő példányának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e9717-241">Configure local Git deployment</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)]

### <a name="push-tooazure-from-git"></a><span data-ttu-id="e9717-242">A Git Push tooAzure</span><span class="sxs-lookup"><span data-stu-id="e9717-242">Push tooAzure from Git</span></span>

<span data-ttu-id="e9717-243">Adjon hozzá egy helyi Git-tárház Azure távoli tooyour.</span><span class="sxs-lookup"><span data-stu-id="e9717-243">Add an Azure remote tooyour local Git repository.</span></span>

```bash
git remote add azure <paste_copied_url_here>
```

<span data-ttu-id="e9717-244">Toohello Azure távoli toodeploy hello PHP-alkalmazás leküldési.</span><span class="sxs-lookup"><span data-stu-id="e9717-244">Push toohello Azure remote toodeploy hello PHP application.</span></span> <span data-ttu-id="e9717-245">Korábban hello létrehozása központi felhasználói hello részeként megadott hello jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="e9717-245">You are prompted for hello password you supplied earlier as part of hello creation of hello deployment user.</span></span>

```bash
git push azure master
```

<span data-ttu-id="e9717-246">A telepítés során az Azure App Service a telepítés előrehaladását a Git kommunikál.</span><span class="sxs-lookup"><span data-stu-id="e9717-246">During deployment, Azure App Service communicates its progress with Git.</span></span>

```bash
Counting objects: 3, done.
Delta compression using up too8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

> [!NOTE]
> <span data-ttu-id="e9717-247">Azt tapasztalhatja, hogy telepíti-e a telepítési folyamat hello [szerkesztő](https://getcomposer.org/) csomagok hello végén.</span><span class="sxs-lookup"><span data-stu-id="e9717-247">You may notice that hello deployment process installs [Composer](https://getcomposer.org/) packages at hello end.</span></span> <span data-ttu-id="e9717-248">App Service nem fut ezen automatizálása alapértelmezett üzembe helyezése során, így ez a minta-tárház három további fájlok a a root directory tooenable:</span><span class="sxs-lookup"><span data-stu-id="e9717-248">App Service does not run these automations during default deployment, so this sample repository has three additional files in its root directory tooenable it:</span></span>
>
> - <span data-ttu-id="e9717-249">`.deployment`– Ez a fájl közli az App Service toorun `bash deploy.sh` hello egyéni telepítési parancsfájlként.</span><span class="sxs-lookup"><span data-stu-id="e9717-249">`.deployment` - This file tells App Service toorun `bash deploy.sh` as hello custom deployment script.</span></span>
> - <span data-ttu-id="e9717-250">`deploy.sh`-hello egyéni telepítési parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="e9717-250">`deploy.sh` - hello custom deployment script.</span></span> <span data-ttu-id="e9717-251">Ha hello fájl tekintse át, látni fogja, hogy fusson `php composer.phar install` után `npm install`.</span><span class="sxs-lookup"><span data-stu-id="e9717-251">If you review hello file, you will see that it runs `php composer.phar install` after `npm install`.</span></span>
> - <span data-ttu-id="e9717-252">`composer.phar`-hello szerkesztő Csomagkezelő.</span><span class="sxs-lookup"><span data-stu-id="e9717-252">`composer.phar` - hello Composer package manager.</span></span>
>
> <span data-ttu-id="e9717-253">Ez a megközelítés tooadd bármely lépés tooyour Git-alapú üzembe helyezési tooApp szolgáltatás használhatja.</span><span class="sxs-lookup"><span data-stu-id="e9717-253">You can use this approach tooadd any step tooyour Git-based deployment tooApp Service.</span></span> <span data-ttu-id="e9717-254">További információkért lásd: [egyéni telepítési parancsfájl](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span><span class="sxs-lookup"><span data-stu-id="e9717-254">For more information, see [Custom Deployment Script](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span></span>
>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="e9717-255">Keresse meg az Azure-webalkalmazásban toohello</span><span class="sxs-lookup"><span data-stu-id="e9717-255">Browse toohello Azure web app</span></span>

<span data-ttu-id="e9717-256">Keresse meg a túl`http://<app_name>.azurewebsites.net` és néhány feladatok toohello listát vesznek fel.</span><span class="sxs-lookup"><span data-stu-id="e9717-256">Browse too`http://<app_name>.azurewebsites.net` and add a few tasks toohello list.</span></span>

![PHP-alkalmazás fusson az Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

<span data-ttu-id="e9717-258">Gratulálunk, az Azure App Service-ben, amelyen az adatvezérelt PHP-alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="e9717-258">Congratulations, you're running a data-driven PHP app in Azure App Service.</span></span>

## <a name="update-model-locally-and-redeploy"></a><span data-ttu-id="e9717-259">Frissítési modell helyileg, és helyezze üzembe újra</span><span class="sxs-lookup"><span data-stu-id="e9717-259">Update model locally and redeploy</span></span>

<span data-ttu-id="e9717-260">Ebben a lépésben egy egyszerű változás toohello ellenőrizze `task` adatok modell és hello webalkalmazást, és tegye közzé az hello frissítés tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e9717-260">In this step, you make a simple change toohello `task` data model and hello webapp, and then publish hello update tooAzure.</span></span>

<span data-ttu-id="e9717-261">A hello feladatok esetben módosítani hello alkalmazás, hogy a feladatoknak befejezettként.</span><span class="sxs-lookup"><span data-stu-id="e9717-261">For hello tasks scenario, you modify hello application so that you can mark a task as complete.</span></span>

### <a name="add-a-column"></a><span data-ttu-id="e9717-262">Egy oszlop hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e9717-262">Add a column</span></span>

<span data-ttu-id="e9717-263">A Terminálszolgáltatások hello lépjen a toohello hello Git-tárház gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="e9717-263">In hello terminal, navigate toohello root of hello Git repository.</span></span>

<span data-ttu-id="e9717-264">Egy új adatbázis történő áttelepítés hello készítése `tasks` tábla:</span><span class="sxs-lookup"><span data-stu-id="e9717-264">Generate a new database migration for hello `tasks` table:</span></span>

```bash
php artisan make:migration add_complete_column --table=tasks
```

<span data-ttu-id="e9717-265">Ez a parancs mutatja, hogy hello hello áttelepítési fájl neve, amely akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="e9717-265">This command shows you hello name of hello migration file that's generated.</span></span> <span data-ttu-id="e9717-266">Ez a fájl a _adatbázis/áttelepítések_ , majd nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="e9717-266">Find this file in _database/migrations_ and open it.</span></span>

<span data-ttu-id="e9717-267">Cserélje le a hello `up` hello kód a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="e9717-267">Replace hello `up` method with hello following code:</span></span>

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

<span data-ttu-id="e9717-268">hello előző kód logikai oszlopot ad a hello `tasks` nevű táblát `complete`.</span><span class="sxs-lookup"><span data-stu-id="e9717-268">hello preceding code adds a boolean column in hello `tasks` table called `complete`.</span></span>

<span data-ttu-id="e9717-269">Cserélje le a hello `down` hello hello visszaállítási művelet kódja a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="e9717-269">Replace hello `down` method with hello following code for hello rollback action:</span></span>

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

<span data-ttu-id="e9717-270">A Terminálszolgáltatások hello futtassa Laravel adatbázis áttelepítések toomake hello módosítása hello helyi adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="e9717-270">In hello terminal, run Laravel database migrations toomake hello change in hello local database.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="e9717-271">Hello alapján [Laravel elnevezési](https://laravel.com/docs/5.4/eloquent#defining-models), hello modell `Task` (lásd: _app/Task.php_) toohello leképezhető `tasks` tábla alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="e9717-271">Based on hello [Laravel naming convention](https://laravel.com/docs/5.4/eloquent#defining-models), hello model `Task` (see _app/Task.php_) maps toohello `tasks` table by default.</span></span>

### <a name="update-application-logic"></a><span data-ttu-id="e9717-272">Frissítés az alkalmazáslogikát</span><span class="sxs-lookup"><span data-stu-id="e9717-272">Update application logic</span></span>

<span data-ttu-id="e9717-273">Nyissa meg hello *routes/web.php* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e9717-273">Open hello *routes/web.php* file.</span></span> <span data-ttu-id="e9717-274">hello alkalmazás határozza meg, az útvonalak és üzleti logika itt.</span><span class="sxs-lookup"><span data-stu-id="e9717-274">hello application defines its routes and business logic here.</span></span>

<span data-ttu-id="e9717-275">Hello fájl hello végén adjon hozzá egy útvonalat a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="e9717-275">At hello end of hello file, add a route with hello following code:</span></span>

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

<span data-ttu-id="e9717-276">hello előző kód esetén egy egyszerű frissítés toohello adatmodellhez való átváltással hello értékének `complete`.</span><span class="sxs-lookup"><span data-stu-id="e9717-276">hello preceding code makes a simple update toohello data model by toggling hello value of `complete`.</span></span>

### <a name="update-hello-view"></a><span data-ttu-id="e9717-277">Hello nézet frissítése</span><span class="sxs-lookup"><span data-stu-id="e9717-277">Update hello view</span></span>

<span data-ttu-id="e9717-278">Nyissa meg hello *resources/views/tasks.blade.php* fájlt.</span><span class="sxs-lookup"><span data-stu-id="e9717-278">Open hello *resources/views/tasks.blade.php* file.</span></span> <span data-ttu-id="e9717-279">Hello található `<tr>` nyitó, és cserélje le:</span><span class="sxs-lookup"><span data-stu-id="e9717-279">Find hello `<tr>` opening tag and replace it with:</span></span>

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

<span data-ttu-id="e9717-280">hello megelőző kód módosítások hello sor szín attól függően, hogy hello feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="e9717-280">hello preceding code changes hello row color depending on whether hello task is complete.</span></span>

<span data-ttu-id="e9717-281">A következő sorban hello a következő kód hello közül választhat:</span><span class="sxs-lookup"><span data-stu-id="e9717-281">In hello next line, you have hello following code:</span></span>

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

<span data-ttu-id="e9717-282">Teljes sor hello cserélje le a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="e9717-282">Replace hello entire line with hello following code:</span></span>

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

<span data-ttu-id="e9717-283">hello előző kód hozzáadása, amely hivatkozik, amelyet korábban megadott hello útvonal hello Küldés gomb.</span><span class="sxs-lookup"><span data-stu-id="e9717-283">hello preceding code adds hello submit button that references hello route that you defined earlier.</span></span>

### <a name="test-hello-changes-locally"></a><span data-ttu-id="e9717-284">Helyileg hello változások tesztelése</span><span class="sxs-lookup"><span data-stu-id="e9717-284">Test hello changes locally</span></span>

<span data-ttu-id="e9717-285">Hello Git-tárház hello gyökérkönyvtárából futtassa a hello fejlesztési kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e9717-285">From hello root directory of hello Git repository, run hello development server.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="e9717-286">toosee hello feladat állapotának megváltoztatását, keresse meg a túl`http://localhost:8000` és select hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="e9717-286">toosee hello task status change, navigate too`http://localhost:8000` and select hello checkbox.</span></span>

![A hozzáadott jelölőnégyzetet tootask](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

<span data-ttu-id="e9717-288">Írja be a PHP-toostop `Ctrl + C` a Terminálszolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="e9717-288">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

### <a name="publish-changes-tooazure"></a><span data-ttu-id="e9717-289">Módosítások tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="e9717-289">Publish changes tooAzure</span></span>

<span data-ttu-id="e9717-290">A Terminálszolgáltatások hello futtassa Laravel adatbázis áttelepítése hello éles kapcsolati karakterlánc toomake hello változásáról hello Azure-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="e9717-290">In hello terminal, run Laravel database migrations with hello production connection string toomake hello change in hello Azure database.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="e9717-291">A Git összes hello változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e9717-291">Commit all hello changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

<span data-ttu-id="e9717-292">Egyszer hello `git push` végezze el, akkor keresse meg a toohello Azure web app és tesztelési hello új funkcióit.</span><span class="sxs-lookup"><span data-stu-id="e9717-292">Once hello `git push` is complete, navigate toohello Azure web app and test hello new functionality.</span></span>

![Adatbázis és a modell módosítások közzétéve tooAzure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="e9717-294">Ha olyan feladatokat, hello adatbázisában megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="e9717-294">If you added any tasks, they are retained in hello database.</span></span> <span data-ttu-id="e9717-295">Frissítések toohello adatkulcsokat érintetlenül meglévő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e9717-295">Updates toohello data schema leave existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="e9717-296">Diagnosztikai naplók adatfolyam</span><span class="sxs-lookup"><span data-stu-id="e9717-296">Stream diagnostic logs</span></span>

<span data-ttu-id="e9717-297">Hello PHP-alkalmazás futtatása az Azure App Service-ben, közben kaphat hello konzol naplók védőeszközön tooyour terminál.</span><span class="sxs-lookup"><span data-stu-id="e9717-297">While hello PHP application runs in Azure App Service, you can get hello console logs piped tooyour terminal.</span></span> <span data-ttu-id="e9717-298">Ily módon kaphat hello ugyanazon diagnosztikai üzenetek toohelp hibakeresése az alkalmazáshibák.</span><span class="sxs-lookup"><span data-stu-id="e9717-298">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="e9717-299">streamelés esetén használjon hello toostart napló [az webapp napló végéről](/cli/azure/webapp/log#tail) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e9717-299">toostart log streaming, use hello [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
```

<span data-ttu-id="e9717-300">Napló-adatfolyam megkezdte, miután Azure webalkalmazás hello hello böngésző tooget frissítése néhány webes forgalom.</span><span class="sxs-lookup"><span data-stu-id="e9717-300">Once log streaming has started, refresh hello Azure web app in hello browser tooget some web traffic.</span></span> <span data-ttu-id="e9717-301">Most már megtekintheti a konzol naplók védőeszközön toohello Terminálszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e9717-301">You can now see console logs piped toohello terminal.</span></span> <span data-ttu-id="e9717-302">Ha a konzol naplói nem jelenik meg azonnal, ellenőrzés 30 másodperc múlva.</span><span class="sxs-lookup"><span data-stu-id="e9717-302">If you don't see console logs immediately, check again in 30 seconds.</span></span>

<span data-ttu-id="e9717-303">bármikor, streaming toostop napló típusa `Ctrl` + `C`.</span><span class="sxs-lookup"><span data-stu-id="e9717-303">toostop log streaming at anytime, type `Ctrl`+`C`.</span></span>

> [!TIP]
> <span data-ttu-id="e9717-304">A PHP-alkalmazások használhatják hello standard [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="e9717-304">A PHP application can use hello standard [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello console.</span></span> <span data-ttu-id="e9717-305">hello mintaalkalmazás használja ezt a megközelítést _app/Http/routes.php_.</span><span class="sxs-lookup"><span data-stu-id="e9717-305">hello sample application uses this approach in _app/Http/routes.php_.</span></span>
>
> <span data-ttu-id="e9717-306">Egy webes keretrendszer, [Laravel használ Monolog](https://laravel.com/docs/5.4/errors) hello naplózási szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="e9717-306">As a web framework, [Laravel uses Monolog](https://laravel.com/docs/5.4/errors) as hello logging provider.</span></span> <span data-ttu-id="e9717-307">toosee hogyan tooget Monolog toooutput üzenetek toohello konzol, lásd: [PHP: hogyan toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span><span class="sxs-lookup"><span data-stu-id="e9717-307">toosee how tooget Monolog toooutput messages toohello console, see [PHP: How toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span></span>
>
>

## <a name="manage-hello-azure-web-app"></a><span data-ttu-id="e9717-308">Hello Azure web apphoz kezelése</span><span class="sxs-lookup"><span data-stu-id="e9717-308">Manage hello Azure web app</span></span>

<span data-ttu-id="e9717-309">Nyissa meg toohello [Azure-portálon](https://portal.azure.com) toomanage hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e9717-309">Go toohello [Azure portal](https://portal.azure.com) toomanage hello web app you created.</span></span>

<span data-ttu-id="e9717-310">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.</span><span class="sxs-lookup"><span data-stu-id="e9717-310">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-php-mysql/access-portal.png)

<span data-ttu-id="e9717-312">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="e9717-312">You see your web app's Overview page.</span></span> <span data-ttu-id="e9717-313">Itt például a stop, kezdő, indítsa újra, tallózással keresse meg és delete alapvető felügyeleti feladatokat hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="e9717-313">Here, you can perform basic management tasks like  stop, start, restart, browse, and delete.</span></span>

<span data-ttu-id="e9717-314">hello bal oldali menü lapokat biztosít az alkalmazás konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="e9717-314">hello left menu provides pages for configuring your app.</span></span>

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="e9717-316">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e9717-316">Next steps</span></span>

<span data-ttu-id="e9717-317">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="e9717-317">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e9717-318">MySQL-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e9717-318">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="e9717-319">Csatlakozás egy PHP-alkalmazás tooMySQL</span><span class="sxs-lookup"><span data-stu-id="e9717-319">Connect a PHP app tooMySQL</span></span>
> * <span data-ttu-id="e9717-320">Hello app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="e9717-320">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="e9717-321">Hello adatmodell frissítése, és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="e9717-321">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="e9717-322">Az adatfolyam diagnosztikai naplók az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="e9717-322">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="e9717-323">Az Azure-portálon hello hello alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="e9717-323">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="e9717-324">Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name tooa webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e9717-324">Advance toohello next tutorial toolearn how toomap a custom DNS name tooa web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e9717-325">Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése</span><span class="sxs-lookup"><span data-stu-id="e9717-325">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
