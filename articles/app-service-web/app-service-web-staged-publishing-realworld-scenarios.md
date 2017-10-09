---
title: "aaaUse DevOps környezetek hatékonyan a webalkalmazáshoz tartozó |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse telepítési tárolóhely mentése tooset és az alkalmazás több fejlesztési környezet kezelése"
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
ms.openlocfilehash: 61a552e735a4ad9769b661d7c988744074ba2962
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-devops-environments-effectively-for-your-web-apps"></a>DevOps környezetek hatékony használata a webalkalmazások
Ez a cikk bemutatja, hogyan tooset és webes alkalmazások központi telepítésének kezelése, ha az alkalmazás több verziója különböző környezetekben, például a fejlesztési, illetve átmeneti, minőségi megbízhatósági (QA) és éles. Az alkalmazás minden verziója a telepítés során hello adott célra a fejlesztési környezet tekinthető. Például a fejlesztők a QA környezet tootest hello minőségének hello alkalmazás hello ahhoz, azok leküldéses hello módosítások tooproduction.
Több fejlesztési környezet kihívást, mivel szüksége tootrack kódot, kezelheti az erőforrásokat (számítási, webalkalmazás, adatbázis, gyorsítótár stb.), és kód telepítése környezetek között lehet.

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a>Állítson be egy nem éles környezetben (szakaszban, fejlesztői, QA)
Miután egy éles webalkalmazás működik, és, hello következő lépésre toocreate nem éles környezetben. üzembe helyezési toouse, győződjön meg arról, hogy futnak-hello Standard vagy prémium szintű Azure App Service-csomag módban. Üzembe helyezési olyan élő webes alkalmazások, amelyek a saját állomás neve. Webes alkalmazás tartalom és a konfigurációs elemek lecserélhető között két üzembe helyezési, beleértve a hello éles tárolóhelyre. Az alkalmazás tooa üzembe helyezési pont telepítésekor a következő előnyöket hello elérhetővé:

- Módosítások tooa webalkalmazás egy átmeneti üzembe helyezési tárhelyet is ellenőrzi, mielőtt felcserélni a hello alkalmazás hello éles tárolóhelyre.
- Amikor egy webes tooa tárolóhelye először telepíteni és felcserélni a éles környezetben, hello tárolóhely összes példánya van tárolóhelyspecifikus előtt éles környezetben felcserélés folyamatban. Ez a folyamat nem állásidő, a webes alkalmazás központi telepítésekor. hello forgalom átirányítása zökkenőmentes-kérelmek nem dobja tooswap műveletek miatt. tooautomate a teljes munkafolyamat konfigurálása [automatikus felcserélés](web-sites-staged-publishing.md#configure-auto-swap) , ha nincs szükség a előtti swap érvényesítése.
- Egy felcserélés után hello tárolóhely, amely korábban előkészített hello webes alkalmazás most már rendelkezik hello előző éles webalkalmazás rendelkezik. Ha az éles tárolóhelyre hello felcserélve hello módosítások nem a várt módon, végezhet hello azonos felcserélése azonnal tooget az "utolsó ismert helyes" web app vissza.

tooset fel egy átmeneti üzembe helyezési tárhelyet, lásd: [átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md). Minden környezet tartalmaznia kell a saját erőforrások készletét. Például ha a webalkalmazás egy adatbázist használ, majd üzemi és átmeneti webalkalmazások használja a különböző adatbázisokhoz. Átmeneti fejlesztési környezet erőforrások, például az adatbázis, a tároló vagy a gyorsítótár tooset hozzáadása a átmeneti fejlesztési környezetet.

## <a name="examples-of-using-multiple-development-environments"></a>Példák több fejlesztési környezet
A projekt forrás kód kezelésének legalább két környezet kell követnie: fejlesztési és éles. Ha használja a Tartalomkezelés rendszerek (CMSs), alkalmazás-keretrendszert, stb., hello alkalmazás előfordulhat, hogy támogatja ezt a helyzetet testreszabási nélkül. Az eshetőségre az egyes hello népszerű keretrendszerekre hello a következő szakaszok ismertetik. Nagy mennyiségű kérdések toomind érkeznek, végzett munka során CMS/keretrendszerek, például:

- Hogyan tegye meg bontja hello tartalom különböző környezetek?
- Milyen fájlok módosíthatja az nem befolyásolja a keretrendszer verzióját frissítéseket?
- Környezet / konfigurációk kezelése
- Hogyan kezelheti a modulok, a beépülő modulok és a hello alapvető keretrendszert verziófrissítéseit?

Nincsenek számos módon tooset be több környezet a projekthez. hello következő példák azt szemléltetik, minden egyes alkalmazáshoz több módszert.

### <a name="wordpress"></a>WordPress
Ebben a szakaszban megtudhatja, hogyan tooset be a telepítési munkafolyamat tárolóhely a WordPress alkalmazás. WordPress, például a legtöbb CMS megoldások nem támogatja több fejlesztési környezet testreszabási nélkül. Azure App Service Web Apps szolgáltatása hello rendelkezik néhány szolgáltatásokat, amelyek könnyen toostore konfigurációs beállítások kívül a kódot.

1. Mielőtt létrehozna egy átmeneti helyet, állítsa be az alkalmazás kódja toosupport több környezet. toosupport több környezet WordPress, tooedit kell `wp-config.php` a helyi fejlesztési a web-alkalmazás, és adja hozzá a következő kódot a hello fájl hello elején hello. Ez a folyamat lehetővé teszi az alkalmazás toopick hello megfelelő konfigurációja a hello kijelölt környezete alapján.

    ```
    // Support multiple environments
    // set hello config file based on current environment
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
    // include hello config file if it exists, otherwise WP is going toofail
    require_once $path. $config_file;
    ```

2. Hozzon létre egy webes alkalmazás legfelső szintű nevű mappát `config`, és adja hozzá a hello `wp-config.azure.php` és `wp-config.local.php` fájlokat, amelyek tartalmazzák az Azure-alapú környezetben és a helyi környezet kulcsattribútumokkal.

3. Másolja a következő hello `wp-config.local.php`:

    ```
    <?php
    // MySQL settings
    /** hello name of hello database for WordPress */

    define('DB_NAME', 'yourdatabasename');

    /** MySQL database username */
    define('DB_USER', 'yourdbuser');

    /** MySQL database password */
    define('DB_PASSWORD', 'yourpassword');

    /** MySQL hostname */
    define('DB_HOST', 'localhost');
    /**
     * For developers: WordPress debugging mode.
     * * Change this tootrue tooenable hello display of notices during development.
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

    Hello biztonsági kulcsok beállítása a hello előző kód is segítségkérés tooprevent a web app alatt, megtámadott, ezért egyedi értékeket. Ha toogenerate hello karakterlánc van szüksége a biztonsági kulcsok hello kód szerepel, akkor [lépjen toohello automatikus generátor](https://api.wordpress.org/secret-key/1.1/salt) toocreate új kulcs/érték párok.

4. Másolás hello alábbi kódot `wp-config.azure.php`:

    ```    
    <?php
    // MySQL settings
    /** hello name of hello database for WordPress */

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
    * Change this tootrue tooenable hello display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging tooinvestigate issues without displaying tooend user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on hello page */

    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);

    //Security key settings
    /** If you need toogenerate hello string for security keys mentioned above, you can go hello automatic generator toocreate new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
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
Egy utolsó lépésként tooconfigure hello WordPress alkalmazás relatív elérési utakat. WordPress hello adatbázisában tárolja az URL-cím információkat. Ez a tároló nehezebbé egy környezet tooanother áthelyezése tartalmat. Minden alkalommal, amikor a helyi toostage vagy szakasz tooproduction környezetek védelemről tooupdate hello adatbázis szükséges. egy adatbázis telepítésére, minden alkalommal, amikor telepít egy környezet tooanother, használjon hello a kiváltó problémákat tooreduce hello kockázatát [relatív legfelső szintű hivatkozásokat tartalmaz a beépülő modul](https://wordpress.org/plugins/root-relative-urls/), hello WordPress rendszergazda használatával telepíthető Irányítópult.

Adja hozzá a következő tételek tooyour hello `wp-config.php` előtt hello fájl `That's all, stop editing!` Megjegyzés:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Hello beépülő modul segítségével hello aktiválása `Plugins` WordPress rendszergazda irányítópult menüjében. A WordPress alkalmazás idehivatkozás beállításainak mentése.

#### <a name="hello-final-wp-configphp-file"></a>végső hello `wp-config.php` fájl
A WordPress core változásainak nem lesz hatással a `wp-config.php`, `wp-config.azure.php`, és `wp-config.local.php` fájlokat. Íme egy végleges verziójú hello `wp-config.php` fájlt:

```
<?php
/**
 * hello base configurations of hello WordPress.
 *
 * This file has hello following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get hello MySQL settings from your web host.
 *
 * This file is used by hello wp-config.php creation script during the
 * installation. You don't have toouse hello web web app, you can just copy this file
 * too"wp-config.php" and fill in hello values.
 *
 * @package WordPress
 */

// Support multiple environments
// set hello config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include hello config file if it exists, otherwise WP is going toofail
  require_once $path. $config_file;
}

/** Database Charset toouse in creating database tables. */
define('DB_CHARSET', 'utf8');

/** hello Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path toohello WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>A fejlesztői környezet beállítása
1. Ha már rendelkezik Azure-előfizetése futó WordPress-webalkalmazás, jelentkezzen be toohello [Azure-portálon](http://portal.azure.com), és folytassa a tooyour WordPress webalkalmazást. Ha még nem rendelkezik egy WordPress webalkalmazást, létrehozhat egy hello Azure piactéren. több, lásd: toolearn [WordPress-webalkalmazás létrehozása az Azure App Service](web-sites-php-web-site-gallery.md).
Kattintson a **beállítások** > **üzembe helyezési** > **Hozzáadás** toocreate egy üzembe helyezési pont hello nevű *szakasz*. A telepített környezet tárolóhelye egy másik webes alkalmazást, amely megosztások hello erőforrásokhoz, hello elsődleges webes alkalmazás, amelyet korábban hozott létre.

    ![Szakasz üzembe helyezési tárhely létrehozása](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. MySQL-adatbázis egy másik, azaz hozzáadása `wordpress-stage-db`, tooyour erőforráscsoport, `wordpressapp-group`.

    ![MySQL-adatbázis tooresource csoport hozzáadása](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. A szakasz telepítési tárolóhely toopoint toohello új adatbázis-hello kapcsolati karakterláncok frissítése `wordpress-stage-db`. A termelési webalkalmazás `wordpressprodapp`, és átmeneti web app alkalmazásban `wordpressprodapp-stage`, kell pont toodifferent adatbázisok.

#### <a name="configure-environment-specific-app-settings"></a>Környezetfüggő Alkalmazásbeállítások konfigurálása
A fejlesztők tárolhat kulcs/érték párok-karakterlánc Azure hello konfigurációs adatai, úgynevezett **Alkalmazásbeállítások**, amely a webes alkalmazás van társítva. Futásidőben a webalkalmazások automatikusan az értékek lekérésére, és tegye őket elérhetővé toocode, amelyen a webalkalmazás fut.. Biztonsági szempontból, ennek oka töltött ügyféloldali előnyt bizalmas adatokat, például jelszavakat tartalmazó adatbázis-kapcsolati karakterláncok soha nem jelenik meg a fájlban egyszerű szövegként például `wp-config.php`.

Ez a folyamat, amelynek az ismertetése a következő bekezdések hello, akkor hasznos, mert a fájl és az adatbázis módosítása hello WordPress alkalmazás tartalmazza:

* WordPress verziófrissítés
* Új hozzáadása vagy szerkesztése, vagy frissítse a beépülő modulok
* Új hozzáadása vagy szerkesztése vagy témák frissítése

Az Alkalmazásbeállítások konfigurálása:

* Adatbázis-információ
* Be- / kikapcsolása WordPress naplózás bekapcsolása
* WordPress-biztonsági beállítások

![Wordpress-webalkalmazás-beállításainak](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Győződjön meg arról, hogy hozzáadta a következő Alkalmazásbeállítások a termelési web app és szakaszban aljzat hello. Vegye figyelembe, hogy hello éles web app és az átmeneti webalkalmazás használja-e a különböző adatbázisokhoz.

1. Törölje a jelet hello **tárolóhely beállítás** WP_ENV kivételével az összes hello beállítások paraméter jelölőnégyzetét. Ez a folyamat hello konfigurációs fog felcserélni a webalkalmazás, fájl és az adatbázis. Ha **tárolóhely beállítás** van be van jelölve, hello web app Alkalmazásbeállítások és a kapcsolódási karakterlánc konfigurációs rendszer *nem* környezetek között áthelyezése során a **felcserélése** műveletet. Minden adatbázis-változást visszaállított szolgáltatássablonon nem megszakítja a termelési webalkalmazás.

2. A WebMatrix vagy eszközök egy szerkesztőprogramban, például FTP, Git vagy PhpMyAdmin telepíthet hello helyi fejlesztési környezet web app toohello szakasz webalkalmazáshoz és adatbázishoz.

    ![Webes mátrix közzététele párbeszédpanelen WordPress-webalkalmazás](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. Keresse meg, és az átmeneti webalkalmazás teszteléséhez. Figyelembe véve a frissített toobe hello téma hello webalkalmazás helyére esetben ez átmeneti webalkalmazás hello.

    ![Keresse meg az átmeneti webalkalmazás üzembe helyezési ponti csere előtt](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. Ha az összes megfelelőnek tűnik, kattintson a hello **felcserélése** gombra az átmeneti web app toomove tartalom toohello éles környezetben. Ebben az esetben, ha valamelyik hello web app és az adatbázis hello során környezetek között minden **Swap** műveletet.

    ![A WordPress alkalmazás előzetes módosítások felcserélése](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > Ha adott esetben szükség van az tooonly leküldéses fájlok (nincs adatbázis-frissítés), akkor ellenőrizze **tárolóhely beállítás** az adatbázissal kapcsolatos összes hello *Alkalmazásbeállítások* és *kapcsolódási karakterláncokat beállítások*a hello **a webalkalmazás-beállítások** belül hello hello elvégzése előtt az Azure portál panel **felcserélése**. Ebben az esetben DB_NAME, DB_HOST, DB_PASSWORD, DB_USER és alapértelmezett kapcsolatikarakterlánc-beállításokat kell nem jelennek meg az előzetes módosítások érhesse el egy **felcserélése**. Ekkor, ha végzett a hello **felcserélése** művelet, WordPress webalkalmazást fog rendelkezik hello hello csak fájlok frissítéséhez.
    >
    >

    Ezzel előtt egy **felcserélése**, ez hello éles WordPress webalkalmazást.
    ![Webalkalmazás éles üzembe helyezési ponti csere előtt](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

    Hello után **felcserélése** művelet, hello téma frissítve lett a termelési webalkalmazásban.

    ![Webalkalmazás éles üzembe helyezési ponti csere után](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. Ha vissza tooroll van szüksége, elvégezheti a toohello éles webes **Alkalmazásbeállítások**, hello kattintson **felcserélése** tooswap hello webalkalmazáshoz és adatbázishoz éles toostaging tárolóhelyről gombra. Ne feledje, hogy ha az adatbázis-változások bekerülnek a **felcserélése** műveletet, majd hello telepít tooyour átmeneti webalkalmazás, amikor legközelebb átmeneti webalkalmazáshoz toodeploy hello adatbázis módosítások toohello aktuális adatbázis szükséges. hello aktuális adatbázis hello előző éles adatbázis vagy hello szakasz adatbázisa lehet.

#### <a name="summary"></a>Összefoglalás
Az alábbiakban olvashat egy általánosított folyamat bármely alkalmazás, amely rendelkezik egy adatbázisban:

1. Hello alkalmazás telepíthető a helyi környezet.
2. Környezet-specifikus konfigurációk (helyi és az Azure Web Apps) tartalmazza.
3. A Web Apps beállítása az átmeneti és üzemi környezetekben.
4. Ha már futó Azure éles alkalmazásokban, szinkronizálja a termelési tartalom (a fájlok vagy kódot és az adatbázis) toolocal és átmeneti környezetek.
5. Az alkalmazás a helyi környezet fejlesztéséhez.
6. A termelési webalkalmazás karbantartás vagy zárolt módban helyezze, és szinkronizálja a termelési toostaging és fejlesztői környezetek adatbázis tartalmát.
7. Átmeneti környezet és a vizsgálati toohello telepítése.
8. Tooproduction környezet telepítése.
9. Ismételje meg a 4 – 6.

### <a name="umbraco"></a>Umbraco
Ebben a szakaszban megtudhatja, hogyan hello Umbraco CMS használja az egy egyéni modul toodeploy több DevOps környezetek között. Ez a példa biztosít egy másik módszert toomanaging több fejlesztési környezet.

[Umbraco CMS](http://umbraco.com/) sok fejlesztők által használt népszerű .NET CMS megoldás. Hello biztosít [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul toodeploy fejlesztési toostaging tooproduction környezetből. Visual Studio vagy a WebMatrix használatával könnyen létrehozhat egy helyi fejlesztési környezet egy Umbraco CMS webalkalmazás.

- [A Visual Studio egy Umbraco webalkalmazás létrehozása](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [WebMatrix Umbraco webes alkalmazás létrehozása](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

Ne felejtsen tooremove hello `install` az alkalmazás mappájában, és soha ne töltse fel toostage vagy éles webalkalmazások. Ez az oktatóanyag a WebMatrix használja.

#### <a name="set-up-a-staging-environment"></a>A fejlesztői környezet beállítása
1. Hozzon létre egy üzembe helyezési tárhelyet, amint azt korábban említettük a hello Umbraco CMS webalkalmazás, feltéve, hogy már rendelkezik egy Umbraco CMS web app és futtatásához. Ha nem így tesz, létrehozhat egy hello piactéren.
2. Frissítse a szakasz telepítési tárolóhely toopoint toohello új hello kapcsolati karakterláncot **umbraco-szakasz-db** adatbázis. A termelési web app (umbraositecms-1) és az átmeneti web app (umbracositecms-1-előkészítés) *kell* pont toodifferent adatbázisok.

    ![Frissítse a kapcsolati karakterlánc, web app az új átmeneti adatbázis átmeneti](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. Kattintson a **beolvasása közzétételi beállítások** a hello üzembe helyezési pont **szakasz**. Ez a folyamat tölti le a közzétételi beállítások fájlja, hogy a Visual Studio vagy a WebMatrix igényel toopublish hello helyi fejlesztési web app toohello Azure web app az alkalmazás összes hello információt tároló.

    ![Get közzététele a webalkalmazás átmeneti hello beállítása](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. Nyissa meg a helyi fejlesztési webalkalmazás a WebMatrix vagy a Visual Studio. Ez az oktatóanyag a WebMatrix használja. Először tooimport hello közzététele beállításfájl átmeneti webalkalmazáshoz.

    ![A Web Matrix használatával Umbraco közzétételi beállítások importálása](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. Tekintse át a hello párbeszédpanel módosításokat, és a helyi web app tooyour Azure webalkalmazás telepítése, *umbracositecms-1-szakasz*. Fájlok közvetlen tooyour átmeneti webalkalmazás üzembe helyezésekor, hello fájlok hagyja el `~/app_data/TEMP/` mappa mivel ezeket a fájlokat újra lesznek generálva hello szakasz webalkalmazás első esetén elindult. Is el kell hagynia hello `~/app_data/umbraco.config` fájlt, amely is újra lesznek generálva.

    ![Tekintse át a Publish web matrix változásai](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. Sikeresen közzéteszi hello Umbraco helyi web app toohello átmeneti webalkalmazás, keresse meg a tooyour átmeneti webalkalmazás, és néhány tesztek toorule problémák megoldása során futtassa.

#### <a name="set-up-hello-courier2-deployment-module"></a>Állítson be hello Courier2 telepítési modulja
A hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul, egyszerűen gombbal toopush tartalmat, a stíluslapok és a fejlesztői modulok webalkalmazásból egy átmeneti web app tooa éles. Ez a folyamat csökkenti a termelési webalkalmazás feltörésével, amikor frissítést telepít hello kockázatát.
Vásároljon licencet a Courier2 a hello `*.azurewebsites.net` és az egyéni tartomány (azaz http://abc.com). Hello licenc beszerzését követően a hely hello licenc letöltött (. – Licencszerződés fájlt) a hello `bin` mappát.

![Licencfájl DROP bin mappában](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. [Hello Courier2 csomag](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Tooyour szakasz webalkalmazás, azaz http://umbracocms-site-stage.azurewebsites.net/umbraco, a Bejelentkezés gombra hello **fejlesztői** menüben, majd kattintson **csomagok** > **telepítése helyi csomag**.

    ![Umbraco alkalmazáscsomag-telepítő](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. Töltse fel a hello Courier2 csomag hello installer használatával.

    ![Csomag courier modul feltöltése](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. tooconfigure hello csomag, kell tooupdate hello courier.config fájlt hello **Config** webalkalmazás mappát.

    ```xml
    <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how tooconnect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set hello passwordEncoding tooclear: -->
        <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1.azurewebsites.net</url>
          <user>0</user>
          <!--<login>user@email.com</login> -->
          <!-- <password>user_password</password>-->
          <!-- <passwordEncoding>Clear</passwordEncoding>-->
          </repository>
     </repositories>
     ```

4. A `<repositories>`, adja meg hello éles webhely URL-cím és a felhasználói adatokat.
    Ha hello alapértelmezett Umbraco tagsági szolgáltatót használ, adja hello felügyeleti felhasználói Azonosítóját hello hello &lt;felhasználói&gt; szakasz.
    Ha egy egyéni Umbraco tagsági szolgáltató használ, `<login>`,`<password>` hello Courier2 modul tooconnect toohello éles helyen.
    További részletekért [hello dokumentációját hello Courier2 modul](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).

5. Ehhez hasonlóan hello Courier2 modul telepítése a munkakörnyezeti helyet, és konfigurálja úgy, ahogy az itt látható a megfelelő courier.config fájlban toopoint toohello szakasz webalkalmazás.

    ```xml
     <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how tooconnect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set hello passwordEncoding tooclear: -->
        <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1-stage.azurewebsites.net</url>
          <user>0</user>
          </repository>
     </repositories>
    ```

6. Kattintson a hello **Courier2** hello Umbraco CMS webes alkalmazás irányítópult a lapra, majd **helyek**. Megtekintheti az hello tárház nevét a `courier.config`. Ez a folyamat a termelési és az átmeneti webalkalmazások tegye.

    ![Nézet cél web app tárház](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. toodeploy tartalmat hello átmeneti toohello éles webhelyek, nyissa meg túl**tartalom**, és válasszon ki egy meglévő oldalt, vagy hozzon létre egy új lapot. A webalkalmazásból, ahol hello hello lap neve lesz jelölhető ki meglévő **első lépések – új**, és kattintson a **mentése és közzététele**.

    ![Módosítsa a lap címe és közzététele](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. Kattintson a jobb gombbal hello módosított lap tooview összes hello-beállítások. Kattintson a **Courier** tooopen hello **telepítési** párbeszédpanel megnyitásához. Kattintson a **telepítés** tooinitiate központi telepítés.

    ![Courier modul telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. Tekintse át a hello módosításokat, majd **Folytatás**.

    ![Változások áttekintése Courier-modul és-telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    hello telepítési naplójának jeleníti meg, ha hello telepítése sikeres volt-e.

     ![Courier modulból telepítési naplók megtekintése](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. Keresse meg a termelési web app toosee, ha hello változtatások jelennek meg.

     ![Keresse meg az üzemi webalkalmazás](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

a toolearn hogyan toouse Courier, tekintse át hello dokumentációjában tájékozódhat.

#### <a name="how-tooupgrade-hello-umbraco-cms-version"></a>Hogyan tooupgrade hello Umbraco CMS verziója
Courier fog nem frissít, Umbraco CMS tooanother verziójához súgó. Amikor frissít egy Umbraco CMS verziója, nem kompatibilis az egyéni modulok vagy partnertől modulok keressen, és hello Umbraco alap függvénytárai. Az alábbiakban a gyakorlati tanácsokat:

* Mindig készítsen biztonsági másolatot a web app és az adatbázis frissítése előtt. Az Azure-webalkalmazásokban automatikus biztonsági mentés beállítása a webhelyekhez hello biztonsági mentési funkció használatával, és a hely visszaállítása, hello visszaállítási funkció segítségével szükség esetén. További részletekért lásd: [hogyan kész a webalkalmazás tooback](web-sites-backup.md) és [hogyan toorestore a webalkalmazás](web-sites-restore.md).
* Ellenőrizze, hogy e-csomagok partnerek frissít hello verziójával kompatibilis. Hello csomag letöltési oldalát, tekintse át a hello projekt kompatibilitási Umbraco CMS verziójával.

További részletes információt tooupgrade a webalkalmazást helyileg, [hello általános frissítési útmutatóját lásd](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).

Után a helyi fejlesztési webhely frissítve van, közzététele hello módosítások toohello átmeneti webalkalmazás. Az alkalmazás teszteléséhez. Ha az összes megfelelőnek tűnik, használja a hello **felcserélése** tooswap gombra az átmeneti tárolási hely toohello éles webes alkalmazás. Hello használata esetén **felcserélése** művelet hello módosításokat megtekintheti, amelyek befolyásolják a webalkalmazás-konfigurációban. Ez **felcserélése** művelet cseréje hello webes alkalmazásokat és adatbázisokat. Hello után **felcserélése**, éles web app lesz pont toohello umbraco-szakasz-db adatbázis hello és átmeneti web app lesz pont tooumbraco-termék-db adatbázis hello.

![Lapozófájl-kapacitás preview Umbraco CMS telepítéséhez](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Az alábbiakban a csere hello web app és a hello adatbázis előnyei:

* Visszaállíthatja a webalkalmazás egy másik toohello korábbi verzióját **felcserélése** bármely alkalmazással kapcsolatos problémák esetén.
* A frissítés kell toodeploy fájlok és a webes alkalmazás toohello éles webalkalmazás átmeneti hello-adatbázisok és az adatbázis. Számos elemet lépjen hibás fájlok és adatbázisok központi telepítésekor. Hello segítségével **felcserélése** szolgáltatás tárolóhelyek, azt is csökkentheti az állásidőt a frissítés során és a módosítások központi telepítése során esetlegesen előforduló hibákat hello kockázatok csökkentése.
* Mindent **A / B tesztelés** hello segítségével [tesztelése éles környezetben](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) szolgáltatás.

Ez a példa mutatja, akkor hello rugalmasságot hello platform, ahol hozhat létre az egyéni modulok hasonló tooUmbraco Courier toomanage üzembe helyezett házirendmodul környezetek között.

## <a name="references"></a>Referencia
[Agilis szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md)

[Átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md)

[Hogyan érik el tooblock webes toonon éles üzembe helyezési](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
