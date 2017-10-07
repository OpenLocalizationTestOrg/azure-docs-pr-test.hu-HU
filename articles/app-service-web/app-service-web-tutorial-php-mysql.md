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
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Az Azure-ban a PHP és a MySQL webalkalmazás létrehozása

Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás. Ez az oktatóanyag bemutatja, hogyan toocreate egy PHP webalkalmazás az Azure-ban, és csatlakoztassa tooa MySQL-adatbázis. Amikor végzett, konfigurálnia kell egy [Laravel](https://laravel.com/) Azure App Service Web Apps futó alkalmazáshoz.

![PHP-alkalmazás fusson az Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * MySQL-adatbázis létrehozása az Azure-ban
> * Csatlakozás egy PHP-alkalmazás tooMySQL
> * Hello app tooAzure telepítése
> * Hello adatmodell frissítése, és telepítse újra hello alkalmazást
> * Az adatfolyam diagnosztikai naplók az Azure-ból
> * Az Azure-portálon hello hello alkalmazás kezelése

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

* [A Git telepítése](https://git-scm.com/)
* [Telepítse a PHP 5.6.4 vagy újabb](http://php.net/downloads.php)
* [Szerkesztő telepítése](https://getcomposer.org/doc/00-intro.md)
* Engedélyezze a következő PHP-bővítmények Laravel igényeinek hello: OpenSSL, OEM-MySQL, Mbstring, jogkivonatokat létrehozó, XML
* [Telepítse és indítsa el a MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Készítse elő a helyi MySQL

Ebben a lépésben egy adatbázis létrehozása a helyi MySQL Server ebben az oktatóanyagban a használatra.

### <a name="connect-toolocal-mysql-server"></a>Csatlakoztassa a kiszolgálót toolocal MySQL

Egy terminálablakot tooyour helyi MySQL kiszolgálóhoz csatlakozzon. A terminálablakot toorun összes hello parancsokat használhatja az oktatóanyag.

```bash
mysql -u root -p
```

Ha a számítógép a jelszót, adja meg a jelszót hello hello `root` fiók. Ha nem emlékszik a rendszergazdafiók jelszavával, lásd: [MySQL: hogyan tooReset hello gyökér szintű jelszó](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Ha a parancs sikeresen lefutott, majd MySQL fut a kiszolgálón. Ha nem, győződjön meg arról, hogy elindult-e a helyi MySQL-kiszolgáló által a következő hello [MySQL telepítés utáni lépéseket](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database-locally"></a>Helyi adatbázis létrehozása

A hello `mysql` kérdés, hozzon létre egy adatbázist.

```sql 
CREATE DATABASE sampledb;
```

Lépjen ki a kiszolgálói kapcsolatot írja be a `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Helyileg egy PHP-alkalmazás létrehozása
Ebben a lépésben Laravel mintaalkalmazás beolvasása, az adatbázis-kapcsolat konfigurálása, és futtassa helyileg. 

### <a name="clone-hello-sample"></a>Klónozás hello minta

A hello terminálablakot `cd` tooa munkakönyvtárát.

Futtassa a következő parancs tooclone hello minta tárház hello.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

`cd`tooyour klónozott könyvtár.
Telepítse a szükséges hello csomagok.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>MySQL-kapcsolat beállítása

Hello adattár gyökérkönyvtárában, hozzon létre egy fájlt *.env*. Változók hello be a következő másolási hello *.env* fájlt. Cserélje le a hello  _&lt;root_password >_ helyőrzőt hello MySQL gyökér szintű felhasználó jelszavát.

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

Információk arról, hogyan Laravel használja hello _.env_ fájlba, lásd: [Laravel környezet konfigurációjának](https://laravel.com/docs/5.4/configuration#environment-configuration).

### <a name="run-hello-sample-locally"></a>Futtassa helyben a hello minta

Futtatás [Laravel adatbázis áttelepítések](https://laravel.com/docs/5.4/migrations) toocreate hello táblák hello alkalmazások igényeihez. mely táblázatok jönnek létre a hello áttelepítések, keresse meg a hello toosee _adatbázis/áttelepítések_ könyvtárban lévő hello Git-tárházba.

```bash
php artisan migrate
```

Hozzon létre egy új Laravel alkalmazás kulcsot.

```bash
php artisan key:generate
```

Hello alkalmazás futtatásához.

```bash
php artisan serve
```

Keresse meg a túl`http://localhost:8000` a böngészőben. Hello lapon vennie néhány feladatot.

![A PHP sikeresen csatlakozik tooMySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Írja be a PHP-toostop `Ctrl + C` a Terminálszolgáltatások hello.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Hozzon létre MySQL az Azure-ban

Ebben a lépésben a MySQL-adatbázis létrehozása [MySQL (előzetes verzió) az Azure-adatbázis](/azure/mysql). Később akkor konfigurálja hello PHP alkalmazás tooconnect toothis adatbázist.

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>A MySQL-kiszolgáló létrehozása

A kiszolgáló létrehozása az Azure-adatbázisban a MySQL (előzetes verzió) hello [az mysql kiszolgáló létrehozni](/cli/azure/mysql/server#create) parancsot.

Hello a következő parancsot, helyettesítse a MySQL-kiszolgáló nevét, ahol látható hello  _&lt;mysql_server_name >_ helyőrző (érvényes karakterek: `a-z`, `0-9`, és `-`). Ez a név része hello MySQL kiszolgáló állomásneve (`<mysql_server_name>.database.windows.net`), toobe globálisan egyedi kell.

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --admin-user adminuser \
    --admin-password MySQLAzure2017
```

Hello MySQL kiszolgáló akkor jön létre, amikor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

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

### <a name="configure-server-firewall"></a>Kiszolgáló tűzfal konfigurálása

Hozzon létre egy tűzfalszabályt a MySQL server tooallow ügyfél kapcsolatok hello segítségével [az mysql-tűzfalszabály létrehozása](/cli/azure/mysql/server/firewall-rule#create) parancsot.

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> MySQL (előzetes verzió) Azure-adatbázis jelenleg nem korlátozza a kapcsolatokat csak tooAzure szolgáltatások. Mivel a dinamikusan kiosztott IP-címek az Azure-ban,-e jobb tooenable összes IP-címet. hello szolgáltatás előzetes verzió van. Az adatbázis védelmét biztosító jobb módszerek tervbe van véve.
>
>

### <a name="connect-tooproduction-mysql-server-locally"></a>Csatlakoztassa tooproduction MySQL a helyi kiszolgálót

A hello terminálablakot csatlakozzon a toohello MySQL server az Azure-ban. Használja a korábban megadott hello értéket  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

Ha a rendszer kéri a jelszót, _$tr0ngPa$ w0rd!_, a megadott hello adatbázis létrehozásakor.

### <a name="create-a-production-database"></a>Éles adatbázis létrehozása

A hello `mysql` kérdés, hozzon létre egy adatbázist.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Hozzon létre egy felhasználói jogosultságokkal

Néven adatbázis-felhasználó létrehozása _phpappuser_ , és adjon neki a hello minden jogosultság `sampledb` adatbázis.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* too'phpappuser';
```

Lépjen ki a hello kiszolgálókapcsolat beírásával `quit`.

```sql
quit
```

## <a name="connect-app-tooazure-mysql"></a>Kapcsolódás app tooAzure MySQL

Ebben a lépésben csatlakoztatja hello PHP alkalmazás toohello MySQL adatbázis MySQL (előzetes verzió) Azure-adatbázis létrehozása.

<a name="devconfig"></a>

### <a name="configure-hello-database-connection"></a>Hello adatbázis-kapcsolat konfigurálása

Hello adattár gyökérkönyvtárában, hozzon létre egy _. env.production_ változók bele a következő fájl, és másolja hello. Cserélje le a hello helyőrző  _&lt;mysql_server_name >_.

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

Hello módosítások mentéséhez.

> [!TIP]
> a MySQL kapcsolat adatait, a fájl már ki van zárva a Git-tárház hello toosecure (lásd: _.gitignore_ hello tárház gyökérkönyvtárában). Később megtudhatja, hogyan tooconfigure környezeti változók App Service tooconnect tooyour adatbázis Azure-adatbázisban a MySQL (előzetes verzió). A környezeti változókat, akkor nem kell hello *.env* fájl az App Service-ben.
>

### <a name="configure-ssl-certificate"></a>SSL-tanúsítvány konfigurálása

Alapértelmezés szerint a MySQL az Azure-adatbázis ügyfelektől érkező SSL-kapcsolatok érvénybe lépteti. tooconnect tooyour MySQL-adatbázis az Azure-ban, kell használnia egy _.pem_ SSL-tanúsítvány.

Nyissa meg _config/database.php_ , és adja hozzá a hello _sslmode_ és _beállítások_ paraméterek túl`connections.mysql`, ahogy az a következő kód hello.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

toolearn hogyan toogenerate ez _certificate.pem_, lásd: [konfigurálása az SSL-kapcsolat az alkalmazás toosecurely a MySQL adatbázis tooAzure kapcsolati](../mysql/howto-configure-ssl.md).

> [!TIP]
> hello elérési _/ssl/certificate.pem_ tooan meglévő mutat _certificate.pem_ fájl hello Git-tárházban. Ez a fájl ebben az oktatóanyagban a kényelem valósul meg. A legjobb, meg kell nem hajtsa végre a _.pem_ tanúsítványok forrás vezérlőbe. 
>

### <a name="test-hello-application-locally"></a>Hello alkalmazás helyi teszteléséhez

Futtassa a Laravel adatbázis áttelepítése az _. env.production_ , környezet toocreate hello táblák fájl a MySQL-adatbázisban az Azure Database hello MySQL (előzetes verzió). Ne feledje, hogy _. env.production_ hello kapcsolati információk tooyour MySQL-adatbázis rendelkezik az Azure-ban.

```bash
php artisan migrate --env=production --force
```

_. env.production_ még nem rendelkezik érvényes kulcs. Az egy új terminál hello létrehozni.

```bash
php artisan key:generate --env=production --force
```

Futtassa a hello mintaalkalmazást _. env.production_ hello környezet fájlként.

```bash
php artisan serve --env=production
```

Keresse meg a túl`http://localhost:8000`. Hello értékkel nem jelenik meg hibaüzenet, ha hello PHP-alkalmazások az Azure-ban toohello MySQL-adatbázis kapcsolódik.

Hello lapon vennie néhány feladatot.

![A PHP csatlakozás sikeresen tooAzure adatbázis MySQL (előzetes verzió)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Írja be a PHP-toostop `Ctrl + C` a Terminálszolgáltatások hello.

### <a name="commit-your-changes"></a>A változtatások véglegesítése a határidő

Futtassa a következő Git-parancsok toocommit hello a módosításokat:

```bash
git add .
git commit -m "database.php updates"
```

Az alkalmazás készen áll a toobe telepítve.

## <a name="deploy-tooazure"></a>TooAzure telepítése

Ebben a lépésben hello PHP MySQL-kompatibilis alkalmazás tooAzure App Service telepítése.

### <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Webalkalmazás létrehozása

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)]

### <a name="set-hello-php-version"></a>Hello PHP-verzió beállítása

Hello segítségével igényel, amely az alkalmazás hello hello PHP verzióját [az webapp konfiguráció](/cli/azure/webapp/config#set) parancsot.

hello következő parancs beállítja a hello PHP verzió too_7.0_.

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a>Adatbázis-beállítások konfigurálása

Szerint korábban, csatlakoztathatja tooyour Azure-beli MySQL database App Service környezeti változók használatát.

Az App Service-ben, a környezeti változók beállítása _Alkalmazásbeállítások_ hello segítségével [az webapp appsettings konfiguráció](/cli/azure/webapp/config/appsettings#set) parancsot.

hello következő parancsot konfigurálása hello Alkalmazásbeállítások `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, és `DB_PASSWORD`. Cserélje le a helyőrzőket hello  _&lt;alkalmazásnév >_ és  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

Használhatja a hello PHP [getenv](http://www.php.net/manual/function.getenv.php) metódus tooaccess hello beállításait. hello Laravel kódot használja egy [env](https://laravel.com/docs/5.4/helpers#method-env) keresztül hello PHP burkoló `getenv`. Például hello MySQL konfigurációját a _config/database.php_ hello a következő kódot a következőképpen néz:

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

### <a name="configure-laravel-environment-variables"></a>Környezeti változók Laravel konfigurálása

Laravel kell egy alkalmazás-kulcsot az App Service-ben. Alkalmazás beállításokkal konfigurálható.

Használjon `php artisan` toogenerate egy új alkalmazás kulcs too_.env_ mentés nélkül.

```bash
php artisan key:generate --show
```

Hello alkalmazás kulcsát állítsa a hello App Service webalkalmazás hello segítségével [az webapp appsettings konfiguráció](/cli/azure/webapp/config/appsettings#set) parancsot. Cserélje le a helyőrzőket hello  _&lt;alkalmazásnév >_ és  _&lt;outputofphpartisankey: készítése >_.

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"`be van állítva Laravel tooreturn hibakeresési adatok hello webalkalmazás központi telepítési hibát észlel. Termelési alkalmazások futtatásakor állítsa be úgy túl`false`, amelyhez biztonságosabb.

### <a name="set-hello-virtual-application-path"></a>Set hello virtuális alkalmazás elérési útja

Hello webalkalmazás hello virtuális alkalmazás elérési útjának beállítása. Ez a lépés akkor szükséges, mert hello [Laravel alkalmazás életciklusa](https://laravel.com/docs/5.4/lifecycle) hello kezdődik _nyilvános_ könyvtár hello alkalmazás gyökérkönyvtárában helyett. Egyéb PHP keretrendszerek, amelynek életciklus start hello gyökérkönyvtárában hello alkalmazás virtuális elérési út kézi konfigurálása nélkül is működik.

Set hello virtuális alkalmazás elérési útja hello segítségével [az erőforrás frissítési](/cli/azure/resource#update) parancsot. Cserélje le a hello  _&lt;alkalmazásnév >_ helyőrző.

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

Alapértelmezés szerint az Azure App Service mutat hello virtuális alkalmazás gyökérútvonalát (_/_) hello toohello gyökérkönyvtárában telepített alkalmazásfájlok (_sites\wwwroot_).

### <a name="configure-a-deployment-user"></a>Üzembe helyező felhasználó konfigurálása

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="configure-local-git-deployment"></a>A Git helyi üzemelő példányának konfigurálása

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)]

### <a name="push-tooazure-from-git"></a>A Git Push tooAzure

Adjon hozzá egy helyi Git-tárház Azure távoli tooyour.

```bash
git remote add azure <paste_copied_url_here>
```

Toohello Azure távoli toodeploy hello PHP-alkalmazás leküldési. Korábban hello létrehozása központi felhasználói hello részeként megadott hello jelszó megadását kéri.

```bash
git push azure master
```

A telepítés során az Azure App Service a telepítés előrehaladását a Git kommunikál.

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
> Azt tapasztalhatja, hogy telepíti-e a telepítési folyamat hello [szerkesztő](https://getcomposer.org/) csomagok hello végén. App Service nem fut ezen automatizálása alapértelmezett üzembe helyezése során, így ez a minta-tárház három további fájlok a a root directory tooenable:
>
> - `.deployment`– Ez a fájl közli az App Service toorun `bash deploy.sh` hello egyéni telepítési parancsfájlként.
> - `deploy.sh`-hello egyéni telepítési parancsfájl. Ha hello fájl tekintse át, látni fogja, hogy fusson `php composer.phar install` után `npm install`.
> - `composer.phar`-hello szerkesztő Csomagkezelő.
>
> Ez a megközelítés tooadd bármely lépés tooyour Git-alapú üzembe helyezési tooApp szolgáltatás használhatja. További információkért lásd: [egyéni telepítési parancsfájl](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
>

### <a name="browse-toohello-azure-web-app"></a>Keresse meg az Azure-webalkalmazásban toohello

Keresse meg a túl`http://<app_name>.azurewebsites.net` és néhány feladatok toohello listát vesznek fel.

![PHP-alkalmazás fusson az Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

Gratulálunk, az Azure App Service-ben, amelyen az adatvezérelt PHP-alkalmazásokban.

## <a name="update-model-locally-and-redeploy"></a>Frissítési modell helyileg, és helyezze üzembe újra

Ebben a lépésben egy egyszerű változás toohello ellenőrizze `task` adatok modell és hello webalkalmazást, és tegye közzé az hello frissítés tooAzure.

A hello feladatok esetben módosítani hello alkalmazás, hogy a feladatoknak befejezettként.

### <a name="add-a-column"></a>Egy oszlop hozzáadása

A Terminálszolgáltatások hello lépjen a toohello hello Git-tárház gyökérkönyvtárában.

Egy új adatbázis történő áttelepítés hello készítése `tasks` tábla:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Ez a parancs mutatja, hogy hello hello áttelepítési fájl neve, amely akkor jön létre. Ez a fájl a _adatbázis/áttelepítések_ , majd nyissa meg.

Cserélje le a hello `up` hello kód a következő metódust:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

hello előző kód logikai oszlopot ad a hello `tasks` nevű táblát `complete`.

Cserélje le a hello `down` hello hello visszaállítási művelet kódja a következő metódust:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

A Terminálszolgáltatások hello futtassa Laravel adatbázis áttelepítések toomake hello módosítása hello helyi adatbázisban.

```bash
php artisan migrate
```

Hello alapján [Laravel elnevezési](https://laravel.com/docs/5.4/eloquent#defining-models), hello modell `Task` (lásd: _app/Task.php_) toohello leképezhető `tasks` tábla alapértelmezés szerint.

### <a name="update-application-logic"></a>Frissítés az alkalmazáslogikát

Nyissa meg hello *routes/web.php* fájlt. hello alkalmazás határozza meg, az útvonalak és üzleti logika itt.

Hello fájl hello végén adjon hozzá egy útvonalat a következő kód hello:

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

hello előző kód esetén egy egyszerű frissítés toohello adatmodellhez való átváltással hello értékének `complete`.

### <a name="update-hello-view"></a>Hello nézet frissítése

Nyissa meg hello *resources/views/tasks.blade.php* fájlt. Hello található `<tr>` nyitó, és cserélje le:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

hello megelőző kód módosítások hello sor szín attól függően, hogy hello feladat befejeződött.

A következő sorban hello a következő kód hello közül választhat:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Teljes sor hello cserélje le a következő kód hello:

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

hello előző kód hozzáadása, amely hivatkozik, amelyet korábban megadott hello útvonal hello Küldés gomb.

### <a name="test-hello-changes-locally"></a>Helyileg hello változások tesztelése

Hello Git-tárház hello gyökérkönyvtárából futtassa a hello fejlesztési kiszolgáló.

```bash
php artisan serve
```

toosee hello feladat állapotának megváltoztatását, keresse meg a túl`http://localhost:8000` és select hello jelölőnégyzetet.

![A hozzáadott jelölőnégyzetet tootask](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

Írja be a PHP-toostop `Ctrl + C` a Terminálszolgáltatások hello.

### <a name="publish-changes-tooazure"></a>Módosítások tooAzure közzététele

A Terminálszolgáltatások hello futtassa Laravel adatbázis áttelepítése hello éles kapcsolati karakterlánc toomake hello változásáról hello Azure-adatbázishoz.

```bash
php artisan migrate --env=production --force
```

A Git összes hello változtatások véglegesítése a határidő, és majd leküldéses hello kód módosítások tooAzure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Egyszer hello `git push` végezze el, akkor keresse meg a toohello Azure web app és tesztelési hello új funkcióit.

![Adatbázis és a modell módosítások közzétéve tooAzure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

Ha olyan feladatokat, hello adatbázisában megmaradnak. Frissítések toohello adatkulcsokat érintetlenül meglévő adatokat.

## <a name="stream-diagnostic-logs"></a>Diagnosztikai naplók adatfolyam

Hello PHP-alkalmazás futtatása az Azure App Service-ben, közben kaphat hello konzol naplók védőeszközön tooyour terminál. Ily módon kaphat hello ugyanazon diagnosztikai üzenetek toohelp hibakeresése az alkalmazáshibák.

streamelés esetén használjon hello toostart napló [az webapp napló végéről](/cli/azure/webapp/log#tail) parancsot.

```azurecli-interactive
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
```

Napló-adatfolyam megkezdte, miután Azure webalkalmazás hello hello böngésző tooget frissítése néhány webes forgalom. Most már megtekintheti a konzol naplók védőeszközön toohello Terminálszolgáltatások. Ha a konzol naplói nem jelenik meg azonnal, ellenőrzés 30 másodperc múlva.

bármikor, streaming toostop napló típusa `Ctrl` + `C`.

> [!TIP]
> A PHP-alkalmazások használhatják hello standard [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello konzol. hello mintaalkalmazás használja ezt a megközelítést _app/Http/routes.php_.
>
> Egy webes keretrendszer, [Laravel használ Monolog](https://laravel.com/docs/5.4/errors) hello naplózási szolgáltatóként. toosee hogyan tooget Monolog toooutput üzenetek toohello konzol, lásd: [PHP: hogyan toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).
>
>

## <a name="manage-hello-azure-web-app"></a>Hello Azure web apphoz kezelése

Nyissa meg toohello [Azure-portálon](https://portal.azure.com) toomanage hello létrehozott webalkalmazás.

Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevét.

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-php-mysql/access-portal.png)

Megtekintheti a webalkalmazás Áttekintés oldalát. Itt például a stop, kezdő, indítsa újra, tallózással keresse meg és delete alapvető felügyeleti feladatokat hajthat végre.

hello bal oldali menü lapokat biztosít az alkalmazás konfigurálását.

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * MySQL-adatbázis létrehozása az Azure-ban
> * Csatlakozás egy PHP-alkalmazás tooMySQL
> * Hello app tooAzure telepítése
> * Hello adatmodell frissítése, és telepítse újra hello alkalmazást
> * Az adatfolyam diagnosztikai naplók az Azure-ból
> * Az Azure-portálon hello hello alkalmazás kezelése

Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name tooa webalkalmazás.

> [!div class="nextstepaction"]
> [Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése](app-service-web-tutorial-custom-domain.md)
