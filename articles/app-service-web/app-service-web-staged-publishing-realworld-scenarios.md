---
title: "DevOps környezetek hatékony használata a webalkalmazás |} Microsoft Docs"
description: "Beállítása és kezelése az alkalmazáshoz több fejlesztési környezet üzembe helyezési útmutató"
services: app-service\web
documentationcenter: 
author: sunbuild
manager: yochayk
editor: 
ms.assetid: 16a594dc-61f5-4984-b5ca-9d5abc39fb1e
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/24/2016
ms.author: sumuth
ms.openlocfilehash: 25248411659f6c7b2e386e310428c365c44ea2e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-devops-environments-effectively-for-your-web-apps"></a>DevOps környezetek hatékony használata a webalkalmazások
Ez a cikk bemutatja, hogyan állíthat be és felügyelhet webes alkalmazások központi telepítésének, ha az alkalmazás több verziója különböző környezetekben, például a fejlesztési, illetve átmeneti, minőségi megbízhatósági (QA) és éles. Az alkalmazás minden verziója a telepítés során megadott erre a célra a fejlesztési környezet tekinthető. Például a fejlesztők a a QA környezet tesztelje az alkalmazás minőségének, mielőtt azok küldje le a módosításokat, üzemi környezetben.
Több fejlesztési környezet feladat lehet, mert később szüksége lesz nyomon követheti a kódot, kezelheti az erőforrásokat (számítási, webalkalmazás, adatbázis, gyorsítótár stb.), és kód telepítése környezetek között.

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a>Állítson be egy nem éles környezetben (szakaszban, fejlesztői, QA)
Miután egy éles webalkalmazás működik, és, a a következő lépés, ha egy nem éles környezetben. Üzembe helyezési használatához győződjön meg arról, hogy futtatja, a Standard vagy prémium szintű Azure App Service csomag módban. Üzembe helyezési olyan élő webes alkalmazások, amelyek a saját állomás neve. Webes alkalmazás tartalom és a konfigurációs elemek lecserélhető között két üzembe helyezési, beleértve az éles webalkalmazásra. Központi telepítésekor az alkalmazást egy üzembe helyezési pont, töltse le a következő előnyöket biztosítja:

- Ellenőrizheti egy átmeneti üzembe helyezési tárhelyet a webalkalmazás módosítása előtt felcserélni a az alkalmazás és az éles webalkalmazásra.
- Amikor először üzembe egy webalkalmazást tárhely és felcserélni a éles környezetben, a tárhely minden példányát vannak tárolóhelyspecifikus előtt éles környezetben felcserélés folyamatban. Ez a folyamat nem állásidő, a webes alkalmazás központi telepítésekor. A forgalom átirányítása zökkenőmentes, és kérelmek nem dobja swap műveletek miatt. A teljes munkafolyamat automatizálása, konfigurálja a [automatikus felcserélés](web-sites-staged-publishing.md#configure-auto-swap) , ha nincs szükség a előtti swap érvényesítése.
- Egy felcserélés után a tárolóhely, amely a korábban előkészített webes alkalmazás most már rendelkezik az előző éles web app rendelkezik. Ha a módosításokat, azokat az éles tárolóhelyre felcserélve nem a várt módon, végezheti el a azonos virtuális azonnal lekérni a "legutóbbi ismert helyes" webes alkalmazás biztonsági.

Egy átmeneti üzembe helyezési pont beállításához tekintse meg a [átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md). Minden környezet tartalmaznia kell a saját erőforrások készletét. Például ha a webalkalmazás egy adatbázist használ, majd üzemi és átmeneti webalkalmazások használja a különböző adatbázisokhoz. Adja hozzá az átmeneti fejlesztési környezet erőforrások, például az adatbázis, a tároló vagy a gyorsítótár a átmeneti fejlesztési környezet beállítása.

## <a name="examples-of-using-multiple-development-environments"></a>Példák több fejlesztési környezet
A projekt forrás kód kezelésének legalább két környezet kell követnie: fejlesztési és éles. Használatakor a tartalom felügyeleti rendszerekkel (CMSs), alkalmazás-keretrendszert, stb., az alkalmazás nem támogathatja az ebben a forgatókönyvben testreszabási nélkül. Ez eshetőségre az egyes a népszerű keretrendszereket az alábbiakban ismertetett. Nagy mennyiségű kérdések tudomást szem előtt végzett munka során CMS/keretrendszerek, például:

- Hogyan tegye meg bontja a tartalom különböző környezetek?
- Milyen fájlok módosíthatja az nem befolyásolja a keretrendszer verzióját frissítéseket?
- Környezet / konfigurációk kezelése
- Hogyan kezelheti a modulok, a beépülő modulok és az alapvető keretrendszert verziófrissítéseit?

Számos módon állítsa be a projekthez több környezet. A következő példák azt szemléltetik, minden egyes alkalmazáshoz több módszert.

### <a name="wordpress"></a>WordPress
Ebben a szakaszban, megtudhatja, hogyan állíthat be a telepítési munkafolyamat WordPress tárhelyek használatával. WordPress, például a legtöbb CMS megoldások nem támogatja több fejlesztési környezet testreszabási nélkül. Az Azure App Service Web Apps szolgáltatásának szolgáltatásait, amelyek megkönnyítik a tárolni a konfigurációs beállításokat a kód kívül van.

1. Mielőtt létrehozna egy átmeneti helyet, beállítása az alkalmazás kódjában több környezetek támogatására. A WordPress több környezetek támogatására, módosítania kell `wp-config.php` a helyi fejlesztési a web-alkalmazás, és adja hozzá a következő kódot a fájl elején. Ez a folyamat lehetővé teszi az alkalmazás számára válassza ki a megfelelő konfigurációt a kiválasztott környezet alapján.

    ```
    // Support multiple environments
    // set the config file based on current environment
    if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
    // local development
     $config_file = 'config/wp-config.local.php';
    }
    elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
    //single file for all azure development environments
     $config_file = 'config/wp-config.azure.php';
    }
    $path = dirname(__FILE__). '/';
    if (file_exists($path. $config_file)) {
    // include the config file if it exists, otherwise WP is going to fail
    require_once $path. $config_file;
    ```

2. Hozzon létre egy webes alkalmazás legfelső szintű nevű mappát `config`, és adja hozzá a `wp-config.azure.php` és `wp-config.local.php` fájlokat, amelyek tartalmazzák az Azure-alapú környezetben és a helyi környezet kulcsattribútumokkal.

3. Másolja a következő `wp-config.local.php`:

    ```
    <?php
    // MySQL settings
    /** The name of the database for WordPress */

    define('DB_NAME', 'yourdatabasename');

    /** MySQL database username */
    define('DB_USER', 'yourdbuser');

    /** MySQL database password */
    define('DB_PASSWORD', 'yourpassword');

    /** MySQL hostname */
    define('DB_HOST', 'localhost');
    /**
     * For developers: WordPress debugging mode.
     * * Change this to true to enable the display of notices during development.
     * It is strongly recommended that plugin and theme developers use WP_DEBUG
     * in their development environments.
     */
    define('WP_DEBUG', true);

    //Security key settings
    define('AUTH_KEY', 'put your unique phrase here');
    define('SECURE_AUTH_KEY','put your unique phrase here');
    define('LOGGED_IN_KEY','put your unique phrase here');
    define('NONCE_KEY', 'put your unique phrase here');
    define('AUTH_SALT', 'put your unique phrase here');
    define('SECURE_AUTH_SALT', 'put your unique phrase here');
    define('LOGGED_IN_SALT', 'put your unique phrase here');
    define('NONCE_SALT', 'put your unique phrase here');

    /**
     * WordPress Database Table prefix.
     *
     * You can have multiple installations in one database if you give each a unique
     * prefix. Only numbers, letters, and underscores please!
     */
    $table_prefix = 'wp_';
    ```

    Ezért a kulcsok szemléltetett módon az előző kód segítségével megakadályozhatja, hogy a webalkalmazás a alatt, megtámadott biztonsági beállításokat, egyedi értékeket használja. Ha szeretné létrehozni a karakterláncot a biztonsági kulcsok szerepel a kódot is [keresse fel az automatikus készítő](https://api.wordpress.org/secret-key/1.1/salt) új kulcs/érték párok létrehozásához.

4. Másolja az alábbi kódot a `wp-config.azure.php`:

    ```    
    <?php
    // MySQL settings
    /** The name of the database for WordPress */

    define('DB_NAME', getenv('DB_NAME'));

    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));

    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));

    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));

    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */

    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);

    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));

    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
    ```

#### <a name="use-relative-paths"></a>Relatív útvonalakat használni
A WordPress alkalmazás konfigurálása egy dolog relatív elérési utakat. WordPress URL-cím információkat tárol az adatbázisban. Ez a tároló lehetővé teszi a másikra nehezebb a környezetek mozgó tartalmat. Frissítenie kell az adatbázis minden alkalommal, amikor a védelemről helyi szakaszban, vagy az éles környezetekben való szakaszban. A problémákat, amelyek oka lehet az adatbázis telepítése minden alkalommal, amikor az egyik környezetből a másikba telepít a kockázatok csökkentése érdekében használja a [relatív legfelső szintű hivatkozásokat tartalmaz a beépülő modul](https://wordpress.org/plugins/root-relative-urls/), a rendszergazda WordPress Irányítópult segítségével telepíthető.

A következő bejegyzés hozzáadása a `wp-config.php` előtt a fájl a `That's all, stop editing!` Megjegyzés:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

A beépülő modul keresztül aktiválja a `Plugins` WordPress rendszergazda irányítópult menüjében. A WordPress alkalmazás idehivatkozás beállításainak mentése.

#### <a name="the-final-wp-configphp-file"></a>A végleges `wp-config.php` fájl
A WordPress core változásainak nem lesz hatással a `wp-config.php`, `wp-config.azure.php`, és `wp-config.local.php` fájlokat. Ez a végleges verziójú a `wp-config.php` fájlt:

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>A fejlesztői környezet beállítása
1. Ha már rendelkezik Azure-előfizetése futó WordPress-webalkalmazás, jelentkezzen be a [Azure-portálon](http://portal.azure.com), és keresse meg a WordPress webalkalmazás. Ha még nem rendelkezik egy WordPress webalkalmazást, létrehozhat egy, az Azure piactérről. További tudnivalókért lásd: [WordPress-webalkalmazás létrehozása az Azure App Service](web-sites-php-web-site-gallery.md).
Kattintson a **beállítások** > **üzembe helyezési** > **Hozzáadás** egy üzembe helyezési tárhely létrehozása a nevű *szakasz*. A telepített környezet tárolóhelye egy másik webes alkalmazást osztja meg ugyanazokat az erőforrásokat, mint az elsődleges webes alkalmazás, amelyet korábban hozott létre.

    ![Szakasz üzembe helyezési tárhely létrehozása](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. MySQL-adatbázis egy másik, azaz hozzáadása `wordpress-stage-db`, az erőforráscsoportba `wordpressapp-group`.

    ![A MySQL-adatbázis hozzáadása erőforráscsoport](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. A szakasz üzembe helyezési pont pontot az új adatbázishoz tartozó kapcsolati karakterláncainak frissítéséhez `wordpress-stage-db`. A termelési webalkalmazás `wordpressprodapp`, és átmeneti web app alkalmazásban `wordpressprodapp-stage`, a különböző adatbázisokhoz kell mutatnia.

#### <a name="configure-environment-specific-app-settings"></a>Környezetfüggő Alkalmazásbeállítások konfigurálása
Fejlesztők tárolhatja kulcs/érték párok-karakterláncot az Azure-ban a konfigurációs adatai, úgynevezett **Alkalmazásbeállítások**, a webes alkalmazás, amely van társítva. Futásidőben a webalkalmazások automatikusan az értékek lekérésére, és azok számára elérhetővé tenni a webalkalmazásban futó kód. Biztonsági szempontból, ennek oka töltött ügyféloldali előnyt bizalmas adatokat, például jelszavakat tartalmazó adatbázis-kapcsolati karakterláncok soha nem jelenik meg a fájlban egyszerű szövegként például `wp-config.php`.

Ez a folyamat, amelynek az ismertetése az alábbi bekezdésekben, akkor hasznos, mert a fájl és az adatbázis módosítása a WordPress alkalmazás tartalmazza:

* WordPress verziófrissítés
* Új hozzáadása vagy szerkesztése, vagy frissítse a beépülő modulok
* Új hozzáadása vagy szerkesztése vagy témák frissítése

Az Alkalmazásbeállítások konfigurálása:

* Adatbázis-információ
* Be- / kikapcsolása WordPress naplózás bekapcsolása
* WordPress-biztonsági beállítások

![Wordpress-webalkalmazás-beállításainak](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Győződjön meg arról, hogy a termelési web app és szakaszban aljzat adja hozzá a következő beállításokkal. Vegye figyelembe, hogy az éles web app és az átmeneti webalkalmazás használja-e a különböző adatbázisokhoz.

1. Törölje a jelet a **tárolóhely beállítás** WP_ENV kivételével minden beállítások paraméterek jelölőnégyzetét. Ez a folyamat a konfigurációs fog felcserélni a webalkalmazás, fájl és az adatbázis. Ha **tárolóhely beállítás** be van jelölve, a webes alkalmazás Alkalmazásbeállítások és kapcsolati karakterlánc-konfiguráció *nem* környezetek között áthelyezése során a **felcserélése** műveletet. Minden adatbázis-változást visszaállított szolgáltatássablonon nem megszakítja a termelési webalkalmazás.

2. A helyi fejlesztési környezet webalkalmazás telepítése az a szakasz web app és az adatbázis WebMatrix vagy az Ön által választott, például FTP, Git vagy PhpMyAdmin eszközök használatával.

    ![Webes mátrix közzététele párbeszédpanelen WordPress-webalkalmazás](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. Keresse meg, és az átmeneti webalkalmazás teszteléséhez. A forgatókönyv a téma a webes alkalmazás esetén frissítenie kell, figyelembe véve itt található az átmeneti webalkalmazás.

    ![Keresse meg az átmeneti webalkalmazás üzembe helyezési ponti csere előtt](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. Ha az összes megfelelőnek tűnik, kattintson a **felcserélése** helyezze át a tartalmat az éles környezet átmeneti webalkalmazás gombjára. Ebben az esetben, ha valamelyik a web app és az adatbázis során környezetek között minden **Swap** műveletet.

    ![A WordPress alkalmazás előzetes módosítások felcserélése](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > Ha a forgatókönyv csak leküldéses fájlok (nincs adatbázis-frissítés), majd ellenőrizze **tárolóhely beállítás** az összes az adatbázissal kapcsolatos *Alkalmazásbeállítások* és *való kapcsolódási karakterláncokat beállítások* a a **a webalkalmazás-beállítások** panel az Azure portálon előtt ezzel a **felcserélése**. Ebben az esetben DB_NAME, DB_HOST, DB_PASSWORD, DB_USER és alapértelmezett kapcsolatikarakterlánc-beállításokat kell nem jelennek meg az előzetes módosítások érhesse el egy **felcserélése**. Ekkor, ha végzett a **felcserélése** műveletet, a WordPress webalkalmazás csak a frissítések fájlokat fog rendelkezni.
    >
    >

    Ezzel előtt egy **felcserélése**, ez a termelési WordPress webalkalmazást.
    ![Webalkalmazás éles üzembe helyezési ponti csere előtt](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

    Után a **felcserélése** műveletet, a téma frissítve lett a termelési webalkalmazásban.

    ![Webalkalmazás éles üzembe helyezési ponti csere után](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. Kell visszaállítania, amikor a termelési webes lépjen **Alkalmazásbeállítások**, és kattintson a **Swap** felcserélni a webalkalmazás és az adatbázis nem éles környezetben való átmeneti helyet gombra. Ne feledje, hogy ha az adatbázis-változások bekerülnek a **felcserélése** műveletet, majd a következő alkalommal központi telepítése az átmeneti webalkalmazáshoz is telepíteni kell az adatbázis-változást az átmeneti webalkalmazás az aktuális adatbázisra. Az aktuális adatbázisban az előző éles adatbázis vagy a szakasz adatbázisa lehet.

#### <a name="summary"></a>Összefoglalás
Az alábbiakban olvashat egy általánosított folyamat bármely alkalmazás, amely rendelkezik egy adatbázisban:

1. Az alkalmazások telepítése a helyi környezet.
2. Környezet-specifikus konfigurációk (helyi és az Azure Web Apps) tartalmazza.
3. A Web Apps beállítása az átmeneti és üzemi környezetekben.
4. Ha már futó Azure éles alkalmazásokban, szinkronizálja a helyi és átmeneti környezetek éles tartalom (a fájlok vagy kódot és az adatbázis).
5. Az alkalmazás a helyi környezet fejlesztéséhez.
6. A termelési webalkalmazás karbantartás vagy zárolt módban helyezze, és átmeneti és fejlesztői környezetben az éles adatbázis-tartalom szinkronizálása.
7. Központi telepítése az átmeneti környezet és a vizsgálat.
8. Központi telepítése éles környezetben.
9. Ismételje meg a 4 – 6.

### <a name="umbraco"></a>Umbraco
Ebben a szakaszban megtudhatja, hogyan a Umbraco CMS egyéni modul telepítéséhez használt több DevOps környezetek között. Ez a példa több fejlesztési környezet kezelése különböző megközelítését ismerteti.

[Umbraco CMS](http://umbraco.com/) sok fejlesztők által használt népszerű .NET CMS megoldás. Biztosítja a [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul fejlesztési átmeneti üzemi környezetbe való központi telepítése. Visual Studio vagy a WebMatrix használatával könnyen létrehozhat egy helyi fejlesztési környezet egy Umbraco CMS webalkalmazás.

- [A Visual Studio egy Umbraco webalkalmazás létrehozása](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [WebMatrix Umbraco webes alkalmazás létrehozása](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

Mindig ne felejtse el eltávolítani a `install` az alkalmazás mappájában, és soha ne töltse fel szakasza vagy éles webalkalmazások. Ez az oktatóanyag a WebMatrix használja.

#### <a name="set-up-a-staging-environment"></a>A fejlesztői környezet beállítása
1. Hozzon létre egy üzembe helyezési tárhelyet, amint azt korábban említettük a Umbraco CMS webalkalmazás, feltéve, hogy már rendelkezik egy Umbraco CMS web app és futtatásához. Ha nem így tesz, létrehozhat egy, a piactéren.
2. Frissítse a kapcsolati karakterláncot a szakasz üzembe helyezési pont úgy, hogy az új mutasson **umbraco-szakasz-db** adatbázis. A termelési web app (umbraositecms-1) és az átmeneti web app (umbracositecms-1-előkészítés) *kell* a különböző adatbázisokhoz mutasson.

    ![Frissítse a kapcsolati karakterlánc, web app az új átmeneti adatbázis átmeneti](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. Kattintson a **beolvasása közzétételi beállítások** tartozó telepített környezet tárolóhelye **szakasz**. Ez a folyamat letölti egy közzétételi beállítások fájlja, amely tartalmazza a Visual Studio vagy a WebMatrix van szüksége, az alkalmazás a helyi fejlesztési webalkalmazásból az Azure web app közzétételét összes információt.

    ![Get beállítás az átmeneti webes alkalmazás közzététele](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. Nyissa meg a helyi fejlesztési webalkalmazás a WebMatrix vagy a Visual Studio. Ez az oktatóanyag a WebMatrix használja. Először a közzétételi beállítások fájlja a átmeneti webalkalmazás importálásához.

    ![A Web Matrix használatával Umbraco közzétételi beállítások importálása](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. Tekintse át a módosításokat a párbeszédpanelen, és a helyi webalkalmazás telepítése az Azure web app alkalmazásban az *umbracositecms-1-szakasz*. Fájlok közvetlenül az átmeneti webalkalmazás való telepítésekor, a fájlok hagyja el a `~/app_data/TEMP/` mappa, mert ezek a fájlok rendszer újra létrehozza, amikor a szakasz webalkalmazás első lépések. Is kell kihagyja a `~/app_data/umbraco.config` fájlt, amely is újra lesznek generálva.

    ![Tekintse át a Publish web matrix változásai](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. Miután a Umbraco helyi web app az átmeneti a webes alkalmazás sikeres közzétételéhez, keresse meg az átmeneti webes alkalmazás, és bármely miatt néhány tesztek futtatása.

#### <a name="set-up-the-courier2-deployment-module"></a>Állítsa be a Courier2 telepítési modulja
Az a [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul, akkor egyszerűen a jobb gombbal leküldéses tartalmat, a stíluslapok és a fejlesztői modulok egy éles a webes alkalmazás egy átmeneti webalkalmazásból. Ez az eljárás csökkenti a kockázata, hogy az éles webalkalmazás feltörésével, amikor frissítést telepít.
Vásároljon licencet a a Courier2 a `*.azurewebsites.net` és az egyéni tartomány (azaz http://abc.com). Miután Ön megvásárolta a licencet, helyezze a letöltött licenc (. – Licencszerződés fájlt) az a `bin` mappát.

![Licencfájl DROP bin mappában](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. [A Courier2 csomag](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Jelentkezzen be a szakasz a webes alkalmazás, azaz http://umbracocms-site-stage.azurewebsites.net/umbraco, kattintson a **fejlesztői** menüben, majd kattintson **csomagok** > **helyi telepítőcsomag**.

    ![Umbraco alkalmazáscsomag-telepítő](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. A telepítő használatával töltse fel a Courier2 csomag.

    ![Csomag courier modul feltöltése](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. A csomag konfigurálásához courier.config fájlt frissíteni kell a **Config** webalkalmazás mappát.

    ```xml
    <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
        <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1.azurewebsites.net</url>
          <user>0</user>
          <!--<login>user@email.com</login> -->
          <!-- <password>user_password</password>-->
          <!-- <passwordEncoding>Clear</passwordEncoding>-->
          </repository>
     </repositories>
     ```

4. A `<repositories>`, adja meg az üzemi webhely URL-cím és a felhasználói adatokat.
    Ha az alapértelmezett Umbraco tagsági szolgáltatót használ, adja meg a felügyeleti felhasználót azonosító a &lt;felhasználói&gt; szakasz.
    Ha egy egyéni Umbraco tagsági szolgáltató használ, `<login>`,`<password>` csatlakozni a munkakörnyezeti helyet a Courier2 modulban.
    További részletekért [tekintse meg a dokumentációt a Courier2 modul](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).

5. Hasonlóképpen a munkakörnyezeti helyet a Courier2 modul telepítése, és konfigurálja úgy, hogy a megfelelő courier.config fájlban, amint az itt látható a szakasz a webes alkalmazás mutasson.

    ```xml
     <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
        <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1-stage.azurewebsites.net</url>
          <user>0</user>
          </repository>
     </repositories>
    ```

6. Kattintson a **Courier2** az Umbraco CMS webes alkalmazás irányítópult fülre, majd **helyek**. Láthatja a tárház nevét, ahogyan az `courier.config`. Ez a folyamat a termelési és az átmeneti webalkalmazások tegye.

    ![Nézet cél web app tárház](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. A munkakörnyezeti helyet a átmeneti helyről tartalom központi telepítéséhez keresse fel **tartalom**, és válasszon ki egy meglévő oldalt, vagy hozzon létre egy új lapot. A webalkalmazásból, ahol a lap neve lesz jelölhető ki meglévő **első lépések – új**, és kattintson a **mentése és közzététele**.

    ![Módosítsa a lap címe és közzététele](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. Kattintson a jobb gombbal a módosított lap használatával jeleníthetők meg a beállításokat. Kattintson a **Courier** megnyitásához a **telepítési** párbeszédpanel megnyitásához. Kattintson a **telepítés** központi telepítésének indítására.

    ![Courier modul telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. Tekintse át a módosításokat, majd a **Folytatás**.

    ![Változások áttekintése Courier-modul és-telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    A telepítési naplójának jeleníti meg, ha a telepítés sikeres volt-e.

     ![Courier modulból telepítési naplók megtekintése](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. Keresse meg az üzemi webes alkalmazás megtekintéséhez, ha a változtatások jelennek meg.

     ![Keresse meg az üzemi webalkalmazás](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Courier használatával kapcsolatos további tudnivalókért tekintse meg a dokumentációt.

#### <a name="how-to-upgrade-the-umbraco-cms-version"></a>A Umbraco CMS verzió frissítése
Courier fog nem súgó Umbraco CMS egyik verzióról a másikra frissít. Amikor frissít egy Umbraco CMS verziója, ki kell választani az nem kompatibilis az egyéni modulok vagy partnerek és a Umbraco alap függvénytárai modulokat. Az alábbiakban a gyakorlati tanácsokat:

* Mindig készítsen biztonsági másolatot a web app és az adatbázis frissítése előtt. Az Azure-webalkalmazásokban a webhelyek a biztonsági mentési szolgáltatás segítségével állítsa be automatikus biztonsági mentésekhez, és a hely visszaállítása, ha szükséges a visszaállítási funkcióval. További részletekért lásd: [biztonsági mentése a webalkalmazás](web-sites-backup.md) és [visszaállítása a webalkalmazás](web-sites-restore.md).
* Ellenőrizze, hogy e-csomagok partnerek frissít verziójával kompatibilis. A csomag letöltési oldalát, tekintse át a projekt kompatibilitási Umbraco CMS verziójával.

A webalkalmazás helyileg, frissítésével kapcsolatos további részletekért [az általános frissítési útmutatóját lásd](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).

Hely frissítése után a helyi fejlesztési, a változások közzétételére a átmeneti webalkalmazás. Az alkalmazás teszteléséhez. Ha összes megfelelőnek tűnik, a **Swap** felcserélni a átmeneti helyet a termelési web app az gombra. Ha a **felcserélése** műveletet, megtekintheti a módosításokat, amelyek befolyásolják a webalkalmazás-konfigurációban. Ez **felcserélése** művelet cseréje a webalkalmazások és adatbázisokat. Miután a **felcserélése**, a umbraco-szakasz-db adatbázis mutasson az éles web app és az átmeneti web app umbraco-termék-db adatbázis mutasson.

![Lapozófájl-kapacitás preview Umbraco CMS telepítéséhez](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Az alábbiakban a csere, mind a web app és az adatbázis előnyei:

* Segítségével visszaállíthatja az előző verziójára egy másik webalkalmazás **felcserélése** bármely alkalmazással kapcsolatos problémák esetén.
* Frissítés esetén kell telepítenie a fájlok és az átmeneti web app az éles a webes alkalmazás-adatbázisok és adatbázis. Számos elemet lépjen hibás fájlok és adatbázisok központi telepítésekor. Használatával a **felcserélése** szolgáltatás tárolóhelyek, azt is csökkentheti az állásidőt a frissítés során és a módosítások központi telepítése során esetlegesen előforduló hibák.
* Mindent **A / B tesztelés** használatával a [tesztelése éles környezetben](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) szolgáltatás.

Ez a példa bemutatja, a rugalmasságot, a platform ahol hozhat létre a központi telepítés kezelése környezetek között Umbraco Courier modul hasonló az egyéni modulok.

## <a name="references"></a>Referencia
[Agilis szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md)

[Átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md)

[Nem éles üzembe helyezési web access blokkolása](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
