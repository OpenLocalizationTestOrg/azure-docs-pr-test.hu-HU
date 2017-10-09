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
# <a name="use-devops-environments-effectively-for-your-web-apps"></a><span data-ttu-id="0937d-103">DevOps környezetek hatékony használata a webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="0937d-103">Use DevOps environments effectively for your web apps</span></span>
<span data-ttu-id="0937d-104">Ez a cikk bemutatja, hogyan tooset és webes alkalmazások központi telepítésének kezelése, ha az alkalmazás több verziója különböző környezetekben, például a fejlesztési, illetve átmeneti, minőségi megbízhatósági (QA) és éles.</span><span class="sxs-lookup"><span data-stu-id="0937d-104">This article shows you how tooset up and manage web application deployments when multiple versions of your application are in various environments, such as development, staging, quality assurance (QA), and production.</span></span> <span data-ttu-id="0937d-105">Az alkalmazás minden verziója a telepítés során hello adott célra a fejlesztési környezet tekinthető.</span><span class="sxs-lookup"><span data-stu-id="0937d-105">Each version of your application can be considered as a development environment for hello specific purpose of your deployment process.</span></span> <span data-ttu-id="0937d-106">Például a fejlesztők a QA környezet tootest hello minőségének hello alkalmazás hello ahhoz, azok leküldéses hello módosítások tooproduction.</span><span class="sxs-lookup"><span data-stu-id="0937d-106">For example, developers can use hello QA environment tootest hello quality of hello application before they push hello changes tooproduction.</span></span>
<span data-ttu-id="0937d-107">Több fejlesztési környezet kihívást, mivel szüksége tootrack kódot, kezelheti az erőforrásokat (számítási, webalkalmazás, adatbázis, gyorsítótár stb.), és kód telepítése környezetek között lehet.</span><span class="sxs-lookup"><span data-stu-id="0937d-107">Multiple development environments can be a challenge because you need tootrack code, manage resources (compute, web app, database, cache, etc.), and deploy code across environments.</span></span>

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a><span data-ttu-id="0937d-108">Állítson be egy nem éles környezetben (szakaszban, fejlesztői, QA)</span><span class="sxs-lookup"><span data-stu-id="0937d-108">Set up a non-production environment (stage, dev, QA)</span></span>
<span data-ttu-id="0937d-109">Miután egy éles webalkalmazás működik, és, hello következő lépésre toocreate nem éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0937d-109">After a production web app is up and running, hello next step is toocreate a non-production environment.</span></span> <span data-ttu-id="0937d-110">üzembe helyezési toouse, győződjön meg arról, hogy futnak-hello Standard vagy prémium szintű Azure App Service-csomag módban.</span><span class="sxs-lookup"><span data-stu-id="0937d-110">toouse deployment slots, make sure that you are running in hello Standard or Premium Azure App Service plan mode.</span></span> <span data-ttu-id="0937d-111">Üzembe helyezési olyan élő webes alkalmazások, amelyek a saját állomás neve.</span><span class="sxs-lookup"><span data-stu-id="0937d-111">Deployment slots are live web apps that have their own host names.</span></span> <span data-ttu-id="0937d-112">Webes alkalmazás tartalom és a konfigurációs elemek lecserélhető között két üzembe helyezési, beleértve a hello éles tárolóhelyre.</span><span class="sxs-lookup"><span data-stu-id="0937d-112">Web app content and configuration elements can be swapped between two deployment slots, including hello production slot.</span></span> <span data-ttu-id="0937d-113">Az alkalmazás tooa üzembe helyezési pont telepítésekor a következő előnyöket hello elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="0937d-113">When you deploy your application tooa deployment slot, you get hello following benefits:</span></span>

- <span data-ttu-id="0937d-114">Módosítások tooa webalkalmazás egy átmeneti üzembe helyezési tárhelyet is ellenőrzi, mielőtt felcserélni a hello alkalmazás hello éles tárolóhelyre.</span><span class="sxs-lookup"><span data-stu-id="0937d-114">You can validate changes tooa web app in a staging deployment slot before you swap hello app with hello production slot.</span></span>
- <span data-ttu-id="0937d-115">Amikor egy webes tooa tárolóhelye először telepíteni és felcserélni a éles környezetben, hello tárolóhely összes példánya van tárolóhelyspecifikus előtt éles környezetben felcserélés folyamatban.</span><span class="sxs-lookup"><span data-stu-id="0937d-115">When you deploy a web app tooa slot first and swap it into production, all instances of hello slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="0937d-116">Ez a folyamat nem állásidő, a webes alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="0937d-116">This process eliminates downtime when you deploy your web app.</span></span> <span data-ttu-id="0937d-117">hello forgalom átirányítása zökkenőmentes-kérelmek nem dobja tooswap műveletek miatt.</span><span class="sxs-lookup"><span data-stu-id="0937d-117">hello traffic redirection is seamless, and no requests are dropped due tooswap operations.</span></span> <span data-ttu-id="0937d-118">tooautomate a teljes munkafolyamat konfigurálása [automatikus felcserélés](web-sites-staged-publishing.md#configure-auto-swap) , ha nincs szükség a előtti swap érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="0937d-118">tooautomate this entire workflow, configure [Auto Swap](web-sites-staged-publishing.md#configure-auto-swap) when pre-swap validation is not needed.</span></span>
- <span data-ttu-id="0937d-119">Egy felcserélés után hello tárolóhely, amely korábban előkészített hello webes alkalmazás most már rendelkezik hello előző éles webalkalmazás rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0937d-119">After a swap, hello slot that has hello previously staged web app now has hello previous production web app.</span></span> <span data-ttu-id="0937d-120">Ha az éles tárolóhelyre hello felcserélve hello módosítások nem a várt módon, végezhet hello azonos felcserélése azonnal tooget az "utolsó ismert helyes" web app vissza.</span><span class="sxs-lookup"><span data-stu-id="0937d-120">If hello changes swapped into hello production slot are not as you expected, you can perform hello same swap immediately tooget your "last known good" web app back.</span></span>

<span data-ttu-id="0937d-121">tooset fel egy átmeneti üzembe helyezési tárhelyet, lásd: [átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="0937d-121">tooset up a staging deployment slot, see [Set up staging environments for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="0937d-122">Minden környezet tartalmaznia kell a saját erőforrások készletét.</span><span class="sxs-lookup"><span data-stu-id="0937d-122">Every environment should include its own set of resources.</span></span> <span data-ttu-id="0937d-123">Például ha a webalkalmazás egy adatbázist használ, majd üzemi és átmeneti webalkalmazások használja a különböző adatbázisokhoz.</span><span class="sxs-lookup"><span data-stu-id="0937d-123">For example, if your web app uses a database, then both production and staging web apps should use different databases.</span></span> <span data-ttu-id="0937d-124">Átmeneti fejlesztési környezet erőforrások, például az adatbázis, a tároló vagy a gyorsítótár tooset hozzáadása a átmeneti fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="0937d-124">Add staging development environment resources such as database, storage, or cache tooset your staging development environment.</span></span>

## <a name="examples-of-using-multiple-development-environments"></a><span data-ttu-id="0937d-125">Példák több fejlesztési környezet</span><span class="sxs-lookup"><span data-stu-id="0937d-125">Examples of using multiple development environments</span></span>
<span data-ttu-id="0937d-126">A projekt forrás kód kezelésének legalább két környezet kell követnie: fejlesztési és éles.</span><span class="sxs-lookup"><span data-stu-id="0937d-126">Any project should follow source code management with at least two environments: development and production.</span></span> <span data-ttu-id="0937d-127">Ha használja a Tartalomkezelés rendszerek (CMSs), alkalmazás-keretrendszert, stb., hello alkalmazás előfordulhat, hogy támogatja ezt a helyzetet testreszabási nélkül.</span><span class="sxs-lookup"><span data-stu-id="0937d-127">If you use content management systems (CMSs), application frameworks, etc., hello application might not support this scenario without customization.</span></span> <span data-ttu-id="0937d-128">Az eshetőségre az egyes hello népszerű keretrendszerekre hello a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="0937d-128">This eventuality is true for some of hello popular frameworks that are discussed in hello following sections.</span></span> <span data-ttu-id="0937d-129">Nagy mennyiségű kérdések toomind érkeznek, végzett munka során CMS/keretrendszerek, például:</span><span class="sxs-lookup"><span data-stu-id="0937d-129">Lots of questions come toomind when you work with CMS/frameworks, such as:</span></span>

- <span data-ttu-id="0937d-130">Hogyan tegye meg bontja hello tartalom különböző környezetek?</span><span class="sxs-lookup"><span data-stu-id="0937d-130">How do you break hello content out into different environments?</span></span>
- <span data-ttu-id="0937d-131">Milyen fájlok módosíthatja az nem befolyásolja a keretrendszer verzióját frissítéseket?</span><span class="sxs-lookup"><span data-stu-id="0937d-131">What files can you change without affecting framework version updates?</span></span>
- <span data-ttu-id="0937d-132">Környezet / konfigurációk kezelése</span><span class="sxs-lookup"><span data-stu-id="0937d-132">How do you manage configurations per environment?</span></span>
- <span data-ttu-id="0937d-133">Hogyan kezelheti a modulok, a beépülő modulok és a hello alapvető keretrendszert verziófrissítéseit?</span><span class="sxs-lookup"><span data-stu-id="0937d-133">How do you manage version updates for modules, plugins, and hello core framework?</span></span>

<span data-ttu-id="0937d-134">Nincsenek számos módon tooset be több környezet a projekthez.</span><span class="sxs-lookup"><span data-stu-id="0937d-134">There are many ways tooset up multiple environments for your project.</span></span> <span data-ttu-id="0937d-135">hello következő példák azt szemléltetik, minden egyes alkalmazáshoz több módszert.</span><span class="sxs-lookup"><span data-stu-id="0937d-135">hello following examples show one method for each respective application.</span></span>

### <a name="wordpress"></a><span data-ttu-id="0937d-136">WordPress</span><span class="sxs-lookup"><span data-stu-id="0937d-136">WordPress</span></span>
<span data-ttu-id="0937d-137">Ebben a szakaszban megtudhatja, hogyan tooset be a telepítési munkafolyamat tárolóhely a WordPress alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0937d-137">In this section, you will learn how tooset up a deployment workflow by using slots for WordPress.</span></span> <span data-ttu-id="0937d-138">WordPress, például a legtöbb CMS megoldások nem támogatja több fejlesztési környezet testreszabási nélkül.</span><span class="sxs-lookup"><span data-stu-id="0937d-138">WordPress, like most CMS solutions, does not support multiple development environments without customization.</span></span> <span data-ttu-id="0937d-139">Azure App Service Web Apps szolgáltatása hello rendelkezik néhány szolgáltatásokat, amelyek könnyen toostore konfigurációs beállítások kívül a kódot.</span><span class="sxs-lookup"><span data-stu-id="0937d-139">hello Web Apps feature of Azure App Service has a few features that make it easy toostore configuration settings outside your code.</span></span>

1. <span data-ttu-id="0937d-140">Mielőtt létrehozna egy átmeneti helyet, állítsa be az alkalmazás kódja toosupport több környezet.</span><span class="sxs-lookup"><span data-stu-id="0937d-140">Before you create a staging slot, set up your application code toosupport multiple environments.</span></span> <span data-ttu-id="0937d-141">toosupport több környezet WordPress, tooedit kell `wp-config.php` a helyi fejlesztési a web-alkalmazás, és adja hozzá a következő kódot a hello fájl hello elején hello.</span><span class="sxs-lookup"><span data-stu-id="0937d-141">toosupport multiple environments in WordPress, you need tooedit `wp-config.php` on your local development web app and add hello following code at hello beginning of hello file.</span></span> <span data-ttu-id="0937d-142">Ez a folyamat lehetővé teszi az alkalmazás toopick hello megfelelő konfigurációja a hello kijelölt környezete alapján.</span><span class="sxs-lookup"><span data-stu-id="0937d-142">This process will enable your application toopick hello correct configuration based on hello selected environment.</span></span>

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

2. <span data-ttu-id="0937d-143">Hozzon létre egy webes alkalmazás legfelső szintű nevű mappát `config`, és adja hozzá a hello `wp-config.azure.php` és `wp-config.local.php` fájlokat, amelyek tartalmazzák az Azure-alapú környezetben és a helyi környezet kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="0937d-143">Create a folder under web app root called `config`, and add hello `wp-config.azure.php` and `wp-config.local.php` files, which represent your Azure environment and local environment respectively.</span></span>

3. <span data-ttu-id="0937d-144">Másolja a következő hello `wp-config.local.php`:</span><span class="sxs-lookup"><span data-stu-id="0937d-144">Copy hello following in `wp-config.local.php`:</span></span>

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

    <span data-ttu-id="0937d-145">Hello biztonsági kulcsok beállítása a hello előző kód is segítségkérés tooprevent a web app alatt, megtámadott, ezért egyedi értékeket.</span><span class="sxs-lookup"><span data-stu-id="0937d-145">Setting hello security keys as illustrated in hello previous code can help tooprevent your web app from being hacked, so use unique values.</span></span> <span data-ttu-id="0937d-146">Ha toogenerate hello karakterlánc van szüksége a biztonsági kulcsok hello kód szerepel, akkor [lépjen toohello automatikus generátor](https://api.wordpress.org/secret-key/1.1/salt) toocreate új kulcs/érték párok.</span><span class="sxs-lookup"><span data-stu-id="0937d-146">If you need toogenerate hello string for security keys mentioned in hello code, you can [go toohello automatic generator](https://api.wordpress.org/secret-key/1.1/salt) toocreate new key/value pairs.</span></span>

4. <span data-ttu-id="0937d-147">Másolás hello alábbi kódot `wp-config.azure.php`:</span><span class="sxs-lookup"><span data-stu-id="0937d-147">Copy hello following code in `wp-config.azure.php`:</span></span>

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

#### <a name="use-relative-paths"></a><span data-ttu-id="0937d-148">Relatív útvonalakat használni</span><span class="sxs-lookup"><span data-stu-id="0937d-148">Use relative paths</span></span>
<span data-ttu-id="0937d-149">Egy utolsó lépésként tooconfigure hello WordPress alkalmazás relatív elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="0937d-149">One last thing tooconfigure in hello WordPress app is relative paths.</span></span> <span data-ttu-id="0937d-150">WordPress hello adatbázisában tárolja az URL-cím információkat.</span><span class="sxs-lookup"><span data-stu-id="0937d-150">WordPress stores URL information in hello database.</span></span> <span data-ttu-id="0937d-151">Ez a tároló nehezebbé egy környezet tooanother áthelyezése tartalmat.</span><span class="sxs-lookup"><span data-stu-id="0937d-151">This storage makes moving content from one environment tooanother more difficult.</span></span> <span data-ttu-id="0937d-152">Minden alkalommal, amikor a helyi toostage vagy szakasz tooproduction környezetek védelemről tooupdate hello adatbázis szükséges.</span><span class="sxs-lookup"><span data-stu-id="0937d-152">You need tooupdate hello database every time you move from local toostage or stage tooproduction environments.</span></span> <span data-ttu-id="0937d-153">egy adatbázis telepítésére, minden alkalommal, amikor telepít egy környezet tooanother, használjon hello a kiváltó problémákat tooreduce hello kockázatát [relatív legfelső szintű hivatkozásokat tartalmaz a beépülő modul](https://wordpress.org/plugins/root-relative-urls/), hello WordPress rendszergazda használatával telepíthető Irányítópult.</span><span class="sxs-lookup"><span data-stu-id="0937d-153">tooreduce hello risk of issues that can be caused with deploying a database every time you deploy from one environment tooanother, use hello [Relative Root links plugin](https://wordpress.org/plugins/root-relative-urls/), which you can install by using hello WordPress administrator dashboard.</span></span>

<span data-ttu-id="0937d-154">Adja hozzá a következő tételek tooyour hello `wp-config.php` előtt hello fájl `That's all, stop editing!` Megjegyzés:</span><span class="sxs-lookup"><span data-stu-id="0937d-154">Add hello following entries tooyour `wp-config.php` file before hello `That's all, stop editing!` comment:</span></span>

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

<span data-ttu-id="0937d-155">Hello beépülő modul segítségével hello aktiválása `Plugins` WordPress rendszergazda irányítópult menüjében.</span><span class="sxs-lookup"><span data-stu-id="0937d-155">Activate hello plugin through hello `Plugins` menu in WordPress administrator dashboard.</span></span> <span data-ttu-id="0937d-156">A WordPress alkalmazás idehivatkozás beállításainak mentése.</span><span class="sxs-lookup"><span data-stu-id="0937d-156">Save your permalink settings for WordPress app.</span></span>

#### <a name="hello-final-wp-configphp-file"></a><span data-ttu-id="0937d-157">végső hello `wp-config.php` fájl</span><span class="sxs-lookup"><span data-stu-id="0937d-157">hello final `wp-config.php` file</span></span>
<span data-ttu-id="0937d-158">A WordPress core változásainak nem lesz hatással a `wp-config.php`, `wp-config.azure.php`, és `wp-config.local.php` fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0937d-158">Any WordPress core updates will not affect your `wp-config.php`, `wp-config.azure.php`, and `wp-config.local.php` files.</span></span> <span data-ttu-id="0937d-159">Íme egy végleges verziójú hello `wp-config.php` fájlt:</span><span class="sxs-lookup"><span data-stu-id="0937d-159">Here's a final version of hello `wp-config.php` file:</span></span>

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

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="0937d-160">A fejlesztői környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="0937d-160">Set up a staging environment</span></span>
1. <span data-ttu-id="0937d-161">Ha már rendelkezik Azure-előfizetése futó WordPress-webalkalmazás, jelentkezzen be toohello [Azure-portálon](http://portal.azure.com), és folytassa a tooyour WordPress webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0937d-161">If you already have a WordPress web app running on your Azure subscription, sign in toohello [Azure portal](http://portal.azure.com), and then go tooyour WordPress web app.</span></span> <span data-ttu-id="0937d-162">Ha még nem rendelkezik egy WordPress webalkalmazást, létrehozhat egy hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="0937d-162">If you don't have a WordPress web app, you can create one from hello Azure Marketplace.</span></span> <span data-ttu-id="0937d-163">több, lásd: toolearn [WordPress-webalkalmazás létrehozása az Azure App Service](web-sites-php-web-site-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="0937d-163">toolearn more, see [Create a WordPress web app in Azure App Service](web-sites-php-web-site-gallery.md).</span></span>
<span data-ttu-id="0937d-164">Kattintson a **beállítások** > **üzembe helyezési** > **Hozzáadás** toocreate egy üzembe helyezési pont hello nevű *szakasz*.</span><span class="sxs-lookup"><span data-stu-id="0937d-164">Click **Settings** > **Deployment slots** > **Add** toocreate a deployment slot with hello name *stage*.</span></span> <span data-ttu-id="0937d-165">A telepített környezet tárolóhelye egy másik webes alkalmazást, amely megosztások hello erőforrásokhoz, hello elsődleges webes alkalmazás, amelyet korábban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="0937d-165">A deployment slot is another web application that shares hello same resources as hello primary web app that you created previously.</span></span>

    ![Szakasz üzembe helyezési tárhely létrehozása](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. <span data-ttu-id="0937d-167">MySQL-adatbázis egy másik, azaz hozzáadása `wordpress-stage-db`, tooyour erőforráscsoport, `wordpressapp-group`.</span><span class="sxs-lookup"><span data-stu-id="0937d-167">Add another MySQL database, say `wordpress-stage-db`, tooyour resource group, `wordpressapp-group`.</span></span>

    ![MySQL-adatbázis tooresource csoport hozzáadása](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. <span data-ttu-id="0937d-169">A szakasz telepítési tárolóhely toopoint toohello új adatbázis-hello kapcsolati karakterláncok frissítése `wordpress-stage-db`.</span><span class="sxs-lookup"><span data-stu-id="0937d-169">Update hello connection strings for your stage deployment slot toopoint toohello new database, `wordpress-stage-db`.</span></span> <span data-ttu-id="0937d-170">A termelési webalkalmazás `wordpressprodapp`, és átmeneti web app alkalmazásban `wordpressprodapp-stage`, kell pont toodifferent adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="0937d-170">Your production web app, `wordpressprodapp`, and staging web app, `wordpressprodapp-stage`, must point toodifferent databases.</span></span>

#### <a name="configure-environment-specific-app-settings"></a><span data-ttu-id="0937d-171">Környezetfüggő Alkalmazásbeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0937d-171">Configure environment-specific app settings</span></span>
<span data-ttu-id="0937d-172">A fejlesztők tárolhat kulcs/érték párok-karakterlánc Azure hello konfigurációs adatai, úgynevezett **Alkalmazásbeállítások**, amely a webes alkalmazás van társítva.</span><span class="sxs-lookup"><span data-stu-id="0937d-172">Developers can store key/value string pairs in Azure as part of hello configuration information, called **App Settings**, that's associated with a web app.</span></span> <span data-ttu-id="0937d-173">Futásidőben a webalkalmazások automatikusan az értékek lekérésére, és tegye őket elérhetővé toocode, amelyen a webalkalmazás fut..</span><span class="sxs-lookup"><span data-stu-id="0937d-173">At runtime, web apps automatically retrieve these values and make them available toocode that's running in your web app.</span></span> <span data-ttu-id="0937d-174">Biztonsági szempontból, ennek oka töltött ügyféloldali előnyt bizalmas adatokat, például jelszavakat tartalmazó adatbázis-kapcsolati karakterláncok soha nem jelenik meg a fájlban egyszerű szövegként például `wp-config.php`.</span><span class="sxs-lookup"><span data-stu-id="0937d-174">From a security perspective, that is a nice side benefit because sensitive information, such as database connection strings that include passwords, never show up as clear text in a file such as `wp-config.php`.</span></span>

<span data-ttu-id="0937d-175">Ez a folyamat, amelynek az ismertetése a következő bekezdések hello, akkor hasznos, mert a fájl és az adatbázis módosítása hello WordPress alkalmazás tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0937d-175">This process, which is explained in hello following paragraphs, is useful because it includes both file changes and database changes for hello WordPress app:</span></span>

* <span data-ttu-id="0937d-176">WordPress verziófrissítés</span><span class="sxs-lookup"><span data-stu-id="0937d-176">WordPress version upgrade</span></span>
* <span data-ttu-id="0937d-177">Új hozzáadása vagy szerkesztése, vagy frissítse a beépülő modulok</span><span class="sxs-lookup"><span data-stu-id="0937d-177">Add new or edit or upgrade plugins</span></span>
* <span data-ttu-id="0937d-178">Új hozzáadása vagy szerkesztése vagy témák frissítése</span><span class="sxs-lookup"><span data-stu-id="0937d-178">Add new or edit or upgrade themes</span></span>

<span data-ttu-id="0937d-179">Az Alkalmazásbeállítások konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="0937d-179">Configure app settings for:</span></span>

* <span data-ttu-id="0937d-180">Adatbázis-információ</span><span class="sxs-lookup"><span data-stu-id="0937d-180">Database information</span></span>
* <span data-ttu-id="0937d-181">Be- / kikapcsolása WordPress naplózás bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="0937d-181">Turning on/off WordPress logging</span></span>
* <span data-ttu-id="0937d-182">WordPress-biztonsági beállítások</span><span class="sxs-lookup"><span data-stu-id="0937d-182">WordPress security settings</span></span>

![Wordpress-webalkalmazás-beállításainak](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

<span data-ttu-id="0937d-184">Győződjön meg arról, hogy hozzáadta a következő Alkalmazásbeállítások a termelési web app és szakaszban aljzat hello.</span><span class="sxs-lookup"><span data-stu-id="0937d-184">Make sure that you add hello following app settings for your production web app and stage slot.</span></span> <span data-ttu-id="0937d-185">Vegye figyelembe, hogy hello éles web app és az átmeneti webalkalmazás használja-e a különböző adatbázisokhoz.</span><span class="sxs-lookup"><span data-stu-id="0937d-185">Note that hello production web app and staging web app use different databases.</span></span>

1. <span data-ttu-id="0937d-186">Törölje a jelet hello **tárolóhely beállítás** WP_ENV kivételével az összes hello beállítások paraméter jelölőnégyzetét.</span><span class="sxs-lookup"><span data-stu-id="0937d-186">Clear hello **Slot Setting** checkbox for all hello settings parameters except WP_ENV.</span></span> <span data-ttu-id="0937d-187">Ez a folyamat hello konfigurációs fog felcserélni a webalkalmazás, fájl és az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="0937d-187">This process will swap hello configuration for your web app, file content, and database.</span></span> <span data-ttu-id="0937d-188">Ha **tárolóhely beállítás** van be van jelölve, hello web app Alkalmazásbeállítások és a kapcsolódási karakterlánc konfigurációs rendszer *nem* környezetek között áthelyezése során a **felcserélése** műveletet.</span><span class="sxs-lookup"><span data-stu-id="0937d-188">If **Slot Setting** is checked, hello web app’s app settings and connection string configuration will *not* move across environments when doing a **Swap** operation.</span></span> <span data-ttu-id="0937d-189">Minden adatbázis-változást visszaállított szolgáltatássablonon nem megszakítja a termelési webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0937d-189">Any database changes that are present will not break your production web app.</span></span>

2. <span data-ttu-id="0937d-190">A WebMatrix vagy eszközök egy szerkesztőprogramban, például FTP, Git vagy PhpMyAdmin telepíthet hello helyi fejlesztési környezet web app toohello szakasz webalkalmazáshoz és adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="0937d-190">Deploy hello local development environment web app toohello stage web app and database by using WebMatrix or tools of your choice, such as FTP, Git, or PhpMyAdmin.</span></span>

    ![Webes mátrix közzététele párbeszédpanelen WordPress-webalkalmazás](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. <span data-ttu-id="0937d-192">Keresse meg, és az átmeneti webalkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="0937d-192">Browse and test your staging web app.</span></span> <span data-ttu-id="0937d-193">Figyelembe véve a frissített toobe hello téma hello webalkalmazás helyére esetben ez átmeneti webalkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="0937d-193">Considering a scenario where hello theme of hello web app is toobe updated, here is hello staging web app.</span></span>

    ![Keresse meg az átmeneti webalkalmazás üzembe helyezési ponti csere előtt](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. <span data-ttu-id="0937d-195">Ha az összes megfelelőnek tűnik, kattintson a hello **felcserélése** gombra az átmeneti web app toomove tartalom toohello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0937d-195">If all looks good, click hello **Swap** button on your staging web app toomove your content toohello production environment.</span></span> <span data-ttu-id="0937d-196">Ebben az esetben, ha valamelyik hello web app és az adatbázis hello során környezetek között minden **Swap** műveletet.</span><span class="sxs-lookup"><span data-stu-id="0937d-196">In this case, you swap hello web app and hello database across environments during every **Swap** operation.</span></span>

    ![A WordPress alkalmazás előzetes módosítások felcserélése](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > <span data-ttu-id="0937d-198">Ha adott esetben szükség van az tooonly leküldéses fájlok (nincs adatbázis-frissítés), akkor ellenőrizze **tárolóhely beállítás** az adatbázissal kapcsolatos összes hello *Alkalmazásbeállítások* és *kapcsolódási karakterláncokat beállítások*a hello **a webalkalmazás-beállítások** belül hello hello elvégzése előtt az Azure portál panel **felcserélése**.</span><span class="sxs-lookup"><span data-stu-id="0937d-198">If your scenario needs tooonly push files (no database updates), then check **Slot Setting** for all hello database-related *app settings* and *connection strings settings* in hello **Web App Settings** blade within hello Azure portal before doing hello **Swap**.</span></span> <span data-ttu-id="0937d-199">Ebben az esetben DB_NAME, DB_HOST, DB_PASSWORD, DB_USER és alapértelmezett kapcsolatikarakterlánc-beállításokat kell nem jelennek meg az előzetes módosítások érhesse el egy **felcserélése**.</span><span class="sxs-lookup"><span data-stu-id="0937d-199">In this case, DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, and default connection string settings should not show up in preview changes when you do a **Swap**.</span></span> <span data-ttu-id="0937d-200">Ekkor, ha végzett a hello **felcserélése** művelet, WordPress webalkalmazást fog rendelkezik hello hello csak fájlok frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0937d-200">At this time, when you complete hello **Swap** operation, hello WordPress web app will have hello updates files only.</span></span>
    >
    >

    <span data-ttu-id="0937d-201">Ezzel előtt egy **felcserélése**, ez hello éles WordPress webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0937d-201">Before doing a **Swap**, here is hello production WordPress web app.</span></span>
    <span data-ttu-id="0937d-202">![Webalkalmazás éles üzembe helyezési ponti csere előtt](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span><span class="sxs-lookup"><span data-stu-id="0937d-202">![Production web app before swapping slots](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span></span>

    <span data-ttu-id="0937d-203">Hello után **felcserélése** művelet, hello téma frissítve lett a termelési webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0937d-203">After hello **Swap** operation, hello theme has been updated on your production web app.</span></span>

    ![Webalkalmazás éles üzembe helyezési ponti csere után](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. <span data-ttu-id="0937d-205">Ha vissza tooroll van szüksége, elvégezheti a toohello éles webes **Alkalmazásbeállítások**, hello kattintson **felcserélése** tooswap hello webalkalmazáshoz és adatbázishoz éles toostaging tárolóhelyről gombra.</span><span class="sxs-lookup"><span data-stu-id="0937d-205">When you need tooroll back, you can go toohello production web **App Settings**, and click hello **Swap** button tooswap hello web app and database from production toostaging slot.</span></span> <span data-ttu-id="0937d-206">Ne feledje, hogy ha az adatbázis-változások bekerülnek a **felcserélése** műveletet, majd hello telepít tooyour átmeneti webalkalmazás, amikor legközelebb átmeneti webalkalmazáshoz toodeploy hello adatbázis módosítások toohello aktuális adatbázis szükséges.</span><span class="sxs-lookup"><span data-stu-id="0937d-206">Remember that if database changes are included with a **Swap** operation, then hello next time you deploy tooyour staging web app, you need toodeploy hello database changes toohello current database for your staging web app.</span></span> <span data-ttu-id="0937d-207">hello aktuális adatbázis hello előző éles adatbázis vagy hello szakasz adatbázisa lehet.</span><span class="sxs-lookup"><span data-stu-id="0937d-207">hello current database might be hello previous production database or hello stage database.</span></span>

#### <a name="summary"></a><span data-ttu-id="0937d-208">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="0937d-208">Summary</span></span>
<span data-ttu-id="0937d-209">Az alábbiakban olvashat egy általánosított folyamat bármely alkalmazás, amely rendelkezik egy adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="0937d-209">Following is a generalized process for any application that has a database:</span></span>

1. <span data-ttu-id="0937d-210">Hello alkalmazás telepíthető a helyi környezet.</span><span class="sxs-lookup"><span data-stu-id="0937d-210">Install hello application on your local environment.</span></span>
2. <span data-ttu-id="0937d-211">Környezet-specifikus konfigurációk (helyi és az Azure Web Apps) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0937d-211">Include environment-specific configurations (local and Azure Web Apps).</span></span>
3. <span data-ttu-id="0937d-212">A Web Apps beállítása az átmeneti és üzemi környezetekben.</span><span class="sxs-lookup"><span data-stu-id="0937d-212">Set up your staging and production environments for Web Apps.</span></span>
4. <span data-ttu-id="0937d-213">Ha már futó Azure éles alkalmazásokban, szinkronizálja a termelési tartalom (a fájlok vagy kódot és az adatbázis) toolocal és átmeneti környezetek.</span><span class="sxs-lookup"><span data-stu-id="0937d-213">If you have a production application already running on Azure, sync your production content (files/code and database) toolocal and staging environments.</span></span>
5. <span data-ttu-id="0937d-214">Az alkalmazás a helyi környezet fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="0937d-214">Develop your application on your local environment.</span></span>
6. <span data-ttu-id="0937d-215">A termelési webalkalmazás karbantartás vagy zárolt módban helyezze, és szinkronizálja a termelési toostaging és fejlesztői környezetek adatbázis tartalmát.</span><span class="sxs-lookup"><span data-stu-id="0937d-215">Place your production web app under maintenance or locked mode, and sync database content from production toostaging and dev environments.</span></span>
7. <span data-ttu-id="0937d-216">Átmeneti környezet és a vizsgálati toohello telepítése.</span><span class="sxs-lookup"><span data-stu-id="0937d-216">Deploy toohello staging environment and test.</span></span>
8. <span data-ttu-id="0937d-217">Tooproduction környezet telepítése.</span><span class="sxs-lookup"><span data-stu-id="0937d-217">Deploy tooproduction environment.</span></span>
9. <span data-ttu-id="0937d-218">Ismételje meg a 4 – 6.</span><span class="sxs-lookup"><span data-stu-id="0937d-218">Repeat steps 4 through 6.</span></span>

### <a name="umbraco"></a><span data-ttu-id="0937d-219">Umbraco</span><span class="sxs-lookup"><span data-stu-id="0937d-219">Umbraco</span></span>
<span data-ttu-id="0937d-220">Ebben a szakaszban megtudhatja, hogyan hello Umbraco CMS használja az egy egyéni modul toodeploy több DevOps környezetek között.</span><span class="sxs-lookup"><span data-stu-id="0937d-220">In this section, you will learn how hello Umbraco CMS uses a custom module toodeploy across multiple DevOps environments.</span></span> <span data-ttu-id="0937d-221">Ez a példa biztosít egy másik módszert toomanaging több fejlesztési környezet.</span><span class="sxs-lookup"><span data-stu-id="0937d-221">This example provides a different approach toomanaging multiple development environments.</span></span>

<span data-ttu-id="0937d-222">[Umbraco CMS](http://umbraco.com/) sok fejlesztők által használt népszerű .NET CMS megoldás.</span><span class="sxs-lookup"><span data-stu-id="0937d-222">[Umbraco CMS](http://umbraco.com/) is a popular .NET CMS solution that's used by many developers.</span></span> <span data-ttu-id="0937d-223">Hello biztosít [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul toodeploy fejlesztési toostaging tooproduction környezetből.</span><span class="sxs-lookup"><span data-stu-id="0937d-223">It provides hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module toodeploy from development toostaging tooproduction environments.</span></span> <span data-ttu-id="0937d-224">Visual Studio vagy a WebMatrix használatával könnyen létrehozhat egy helyi fejlesztési környezet egy Umbraco CMS webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0937d-224">You can easily create a local development environment for an Umbraco CMS web app by using Visual Studio or WebMatrix.</span></span>

- [<span data-ttu-id="0937d-225">A Visual Studio egy Umbraco webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0937d-225">Create an Umbraco web app with Visual Studio</span></span>](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [<span data-ttu-id="0937d-226">WebMatrix Umbraco webes alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0937d-226">Create an Umbraco web app with WebMatrix</span></span>](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

<span data-ttu-id="0937d-227">Ne felejtsen tooremove hello `install` az alkalmazás mappájában, és soha ne töltse fel toostage vagy éles webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="0937d-227">Always remember tooremove hello `install` folder under your application, and never upload it toostage or production web apps.</span></span> <span data-ttu-id="0937d-228">Ez az oktatóanyag a WebMatrix használja.</span><span class="sxs-lookup"><span data-stu-id="0937d-228">This tutorial uses WebMatrix.</span></span>

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="0937d-229">A fejlesztői környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="0937d-229">Set up a staging environment</span></span>
1. <span data-ttu-id="0937d-230">Hozzon létre egy üzembe helyezési tárhelyet, amint azt korábban említettük a hello Umbraco CMS webalkalmazás, feltéve, hogy már rendelkezik egy Umbraco CMS web app és futtatásához.</span><span class="sxs-lookup"><span data-stu-id="0937d-230">Create a deployment slot as mentioned previously for hello Umbraco CMS web app, assuming you already have an Umbraco CMS web app up and running.</span></span> <span data-ttu-id="0937d-231">Ha nem így tesz, létrehozhat egy hello piactéren.</span><span class="sxs-lookup"><span data-stu-id="0937d-231">If you do not, you can create one from hello Marketplace.</span></span>
2. <span data-ttu-id="0937d-232">Frissítse a szakasz telepítési tárolóhely toopoint toohello új hello kapcsolati karakterláncot **umbraco-szakasz-db** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="0937d-232">Update hello connection string for your stage deployment slot toopoint toohello new **umbraco-stage-db** database.</span></span> <span data-ttu-id="0937d-233">A termelési web app (umbraositecms-1) és az átmeneti web app (umbracositecms-1-előkészítés) *kell* pont toodifferent adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="0937d-233">Your production web app (umbraositecms-1) and staging web app (umbracositecms-1-stage) *must* point toodifferent databases.</span></span>

    ![Frissítse a kapcsolati karakterlánc, web app az új átmeneti adatbázis átmeneti](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. <span data-ttu-id="0937d-235">Kattintson a **beolvasása közzétételi beállítások** a hello üzembe helyezési pont **szakasz**.</span><span class="sxs-lookup"><span data-stu-id="0937d-235">Click **Get Publish settings** for hello deployment slot **stage**.</span></span> <span data-ttu-id="0937d-236">Ez a folyamat tölti le a közzétételi beállítások fájlja, hogy a Visual Studio vagy a WebMatrix igényel toopublish hello helyi fejlesztési web app toohello Azure web app az alkalmazás összes hello információt tároló.</span><span class="sxs-lookup"><span data-stu-id="0937d-236">This process will download a publish settings file that stores all hello information that Visual Studio or WebMatrix requires toopublish your application from hello local development web app toohello Azure web app.</span></span>

    ![Get közzététele a webalkalmazás átmeneti hello beállítása](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. <span data-ttu-id="0937d-238">Nyissa meg a helyi fejlesztési webalkalmazás a WebMatrix vagy a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0937d-238">Open your local development web app in WebMatrix or Visual Studio.</span></span> <span data-ttu-id="0937d-239">Ez az oktatóanyag a WebMatrix használja.</span><span class="sxs-lookup"><span data-stu-id="0937d-239">This tutorial uses WebMatrix.</span></span> <span data-ttu-id="0937d-240">Először tooimport hello közzététele beállításfájl átmeneti webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="0937d-240">First, you need tooimport hello publish settings file for your staging web app.</span></span>

    ![A Web Matrix használatával Umbraco közzétételi beállítások importálása](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. <span data-ttu-id="0937d-242">Tekintse át a hello párbeszédpanel módosításokat, és a helyi web app tooyour Azure webalkalmazás telepítése, *umbracositecms-1-szakasz*.</span><span class="sxs-lookup"><span data-stu-id="0937d-242">Review changes in hello dialog box, and deploy your local web app tooyour Azure web app, *umbracositecms-1-stage*.</span></span> <span data-ttu-id="0937d-243">Fájlok közvetlen tooyour átmeneti webalkalmazás üzembe helyezésekor, hello fájlok hagyja el `~/app_data/TEMP/` mappa mivel ezeket a fájlokat újra lesznek generálva hello szakasz webalkalmazás első esetén elindult.</span><span class="sxs-lookup"><span data-stu-id="0937d-243">When you deploy files directly tooyour staging web app, you will omit files in hello `~/app_data/TEMP/` folder because these files will be regenerated when hello stage web app is first started.</span></span> <span data-ttu-id="0937d-244">Is el kell hagynia hello `~/app_data/umbraco.config` fájlt, amely is újra lesznek generálva.</span><span class="sxs-lookup"><span data-stu-id="0937d-244">You should also omit hello `~/app_data/umbraco.config` file, which will also be regenerated.</span></span>

    ![Tekintse át a Publish web matrix változásai](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. <span data-ttu-id="0937d-246">Sikeresen közzéteszi hello Umbraco helyi web app toohello átmeneti webalkalmazás, keresse meg a tooyour átmeneti webalkalmazás, és néhány tesztek toorule problémák megoldása során futtassa.</span><span class="sxs-lookup"><span data-stu-id="0937d-246">After you successfully publish hello Umbraco local web app toohello staging web app, browse tooyour staging web app, and run a few tests toorule out any issues.</span></span>

#### <a name="set-up-hello-courier2-deployment-module"></a><span data-ttu-id="0937d-247">Állítson be hello Courier2 telepítési modulja</span><span class="sxs-lookup"><span data-stu-id="0937d-247">Set up hello Courier2 deployment module</span></span>
<span data-ttu-id="0937d-248">A hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul, egyszerűen gombbal toopush tartalmat, a stíluslapok és a fejlesztői modulok webalkalmazásból egy átmeneti web app tooa éles.</span><span class="sxs-lookup"><span data-stu-id="0937d-248">With hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module, you can simply right-click toopush content, style sheets, and development modules from a staging web app tooa production web app.</span></span> <span data-ttu-id="0937d-249">Ez a folyamat csökkenti a termelési webalkalmazás feltörésével, amikor frissítést telepít hello kockázatát.</span><span class="sxs-lookup"><span data-stu-id="0937d-249">This process reduces hello risk of breaking your production web app when you deploy an update.</span></span>
<span data-ttu-id="0937d-250">Vásároljon licencet a Courier2 a hello `*.azurewebsites.net` és az egyéni tartomány (azaz http://abc.com).</span><span class="sxs-lookup"><span data-stu-id="0937d-250">Purchase a license for Courier2 for hello `*.azurewebsites.net` domain and your custom domain (say http://abc.com).</span></span> <span data-ttu-id="0937d-251">Hello licenc beszerzését követően a hely hello licenc letöltött (. – Licencszerződés fájlt) a hello `bin` mappát.</span><span class="sxs-lookup"><span data-stu-id="0937d-251">After you purchase hello license, place hello downloaded license (.LIC file) in hello `bin` folder.</span></span>

![Licencfájl DROP bin mappában](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. <span data-ttu-id="0937d-253">[Hello Courier2 csomag](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span><span class="sxs-lookup"><span data-stu-id="0937d-253">[Download hello Courier2 package](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span></span> <span data-ttu-id="0937d-254">Tooyour szakasz webalkalmazás, azaz http://umbracocms-site-stage.azurewebsites.net/umbraco, a Bejelentkezés gombra hello **fejlesztői** menüben, majd kattintson **csomagok** > **telepítése helyi csomag**.</span><span class="sxs-lookup"><span data-stu-id="0937d-254">Sign in tooyour stage web app, say http://umbracocms-site-stage.azurewebsites.net/umbraco, click hello **Developer** menu, and then click **Packages** > **Install local package**.</span></span>

    ![Umbraco alkalmazáscsomag-telepítő](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. <span data-ttu-id="0937d-256">Töltse fel a hello Courier2 csomag hello installer használatával.</span><span class="sxs-lookup"><span data-stu-id="0937d-256">Upload hello Courier2 package by using hello installer.</span></span>

    ![Csomag courier modul feltöltése](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. <span data-ttu-id="0937d-258">tooconfigure hello csomag, kell tooupdate hello courier.config fájlt hello **Config** webalkalmazás mappát.</span><span class="sxs-lookup"><span data-stu-id="0937d-258">tooconfigure hello package, you need tooupdate hello courier.config file under hello **Config** folder of your web app.</span></span>

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

4. <span data-ttu-id="0937d-259">A `<repositories>`, adja meg hello éles webhely URL-cím és a felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="0937d-259">Under `<repositories>`, enter hello production site URL and user information.</span></span>
    <span data-ttu-id="0937d-260">Ha hello alapértelmezett Umbraco tagsági szolgáltatót használ, adja hello felügyeleti felhasználói Azonosítóját hello hello &lt;felhasználói&gt; szakasz.</span><span class="sxs-lookup"><span data-stu-id="0937d-260">If you are using hello default Umbraco membership provider, then add hello ID for hello Administration user in hello &lt;user&gt; section.</span></span>
    <span data-ttu-id="0937d-261">Ha egy egyéni Umbraco tagsági szolgáltató használ, `<login>`,`<password>` hello Courier2 modul tooconnect toohello éles helyen.</span><span class="sxs-lookup"><span data-stu-id="0937d-261">If you are using a custom Umbraco membership provider, use `<login>`,`<password>` in hello Courier2 module tooconnect toohello production site.</span></span>
    <span data-ttu-id="0937d-262">További részletekért [hello dokumentációját hello Courier2 modul](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span><span class="sxs-lookup"><span data-stu-id="0937d-262">For more details, [review hello documentation for hello Courier2 module](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span></span>

5. <span data-ttu-id="0937d-263">Ehhez hasonlóan hello Courier2 modul telepítése a munkakörnyezeti helyet, és konfigurálja úgy, ahogy az itt látható a megfelelő courier.config fájlban toopoint toohello szakasz webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0937d-263">Similarly, install hello Courier2 module on your production site, and configure it toopoint toohello stage web app in its respective courier.config file as shown here.</span></span>

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

6. <span data-ttu-id="0937d-264">Kattintson a hello **Courier2** hello Umbraco CMS webes alkalmazás irányítópult a lapra, majd **helyek**.</span><span class="sxs-lookup"><span data-stu-id="0937d-264">Click hello **Courier2** tab in hello Umbraco CMS web app dashboard, and then click **Locations**.</span></span> <span data-ttu-id="0937d-265">Megtekintheti az hello tárház nevét a `courier.config`.</span><span class="sxs-lookup"><span data-stu-id="0937d-265">You should see hello repository name as mentioned in `courier.config`.</span></span> <span data-ttu-id="0937d-266">Ez a folyamat a termelési és az átmeneti webalkalmazások tegye.</span><span class="sxs-lookup"><span data-stu-id="0937d-266">Do this process on both your production and staging web apps.</span></span>

    ![Nézet cél web app tárház](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. <span data-ttu-id="0937d-268">toodeploy tartalmat hello átmeneti toohello éles webhelyek, nyissa meg túl**tartalom**, és válasszon ki egy meglévő oldalt, vagy hozzon létre egy új lapot.</span><span class="sxs-lookup"><span data-stu-id="0937d-268">toodeploy content from hello staging site toohello production site, go too**Content**, and select an existing page or create a new page.</span></span> <span data-ttu-id="0937d-269">A webalkalmazásból, ahol hello hello lap neve lesz jelölhető ki meglévő **első lépések – új**, és kattintson a **mentése és közzététele**.</span><span class="sxs-lookup"><span data-stu-id="0937d-269">I will select an existing page from my web app where hello title of hello page is **Getting Started – new**, and then click **Save and Publish**.</span></span>

    ![Módosítsa a lap címe és közzététele](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. <span data-ttu-id="0937d-271">Kattintson a jobb gombbal hello módosított lap tooview összes hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="0937d-271">Right-click hello modified page tooview all hello options.</span></span> <span data-ttu-id="0937d-272">Kattintson a **Courier** tooopen hello **telepítési** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="0937d-272">Click **Courier** tooopen hello **Deployment** dialog box.</span></span> <span data-ttu-id="0937d-273">Kattintson a **telepítés** tooinitiate központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="0937d-273">Click **Deploy** tooinitiate deployment.</span></span>

    ![Courier modul telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. <span data-ttu-id="0937d-275">Tekintse át a hello módosításokat, majd **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="0937d-275">Review hello changes, and then click **Continue**.</span></span>

    ![Változások áttekintése Courier-modul és-telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    <span data-ttu-id="0937d-277">hello telepítési naplójának jeleníti meg, ha hello telepítése sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="0937d-277">hello deployment log shows if hello deployment was successful.</span></span>

     ![Courier modulból telepítési naplók megtekintése](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. <span data-ttu-id="0937d-279">Keresse meg a termelési web app toosee, ha hello változtatások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="0937d-279">Browse your production web app toosee if hello changes are reflected.</span></span>

     ![Keresse meg az üzemi webalkalmazás](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

<span data-ttu-id="0937d-281">a toolearn hogyan toouse Courier, tekintse át hello dokumentációjában tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="0937d-281">toolearn more about how toouse Courier, review hello documentation.</span></span>

#### <a name="how-tooupgrade-hello-umbraco-cms-version"></a><span data-ttu-id="0937d-282">Hogyan tooupgrade hello Umbraco CMS verziója</span><span class="sxs-lookup"><span data-stu-id="0937d-282">How tooupgrade hello Umbraco CMS version</span></span>
<span data-ttu-id="0937d-283">Courier fog nem frissít, Umbraco CMS tooanother verziójához súgó.</span><span class="sxs-lookup"><span data-stu-id="0937d-283">Courier will not help you upgrade from one version of Umbraco CMS tooanother.</span></span> <span data-ttu-id="0937d-284">Amikor frissít egy Umbraco CMS verziója, nem kompatibilis az egyéni modulok vagy partnertől modulok keressen, és hello Umbraco alap függvénytárai.</span><span class="sxs-lookup"><span data-stu-id="0937d-284">When you upgrade an Umbraco CMS version, you must check for incompatibilities with your custom modules or modules from partners and hello Umbraco Core libraries.</span></span> <span data-ttu-id="0937d-285">Az alábbiakban a gyakorlati tanácsokat:</span><span class="sxs-lookup"><span data-stu-id="0937d-285">Here are best practices:</span></span>

* <span data-ttu-id="0937d-286">Mindig készítsen biztonsági másolatot a web app és az adatbázis frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="0937d-286">Always back up your web app and database before you upgrade.</span></span> <span data-ttu-id="0937d-287">Az Azure-webalkalmazásokban automatikus biztonsági mentés beállítása a webhelyekhez hello biztonsági mentési funkció használatával, és a hely visszaállítása, hello visszaállítási funkció segítségével szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="0937d-287">On web apps in Azure, you can set up automatic backups for your websites by using hello backup feature and restore your site if needed by using hello restore feature.</span></span> <span data-ttu-id="0937d-288">További részletekért lásd: [hogyan kész a webalkalmazás tooback](web-sites-backup.md) és [hogyan toorestore a webalkalmazás](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="0937d-288">For more details, see [How tooback up your web app](web-sites-backup.md) and [How toorestore your web app](web-sites-restore.md).</span></span>
* <span data-ttu-id="0937d-289">Ellenőrizze, hogy e-csomagok partnerek frissít hello verziójával kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="0937d-289">Check if packages from partners are compatible with hello version you're upgrading to.</span></span> <span data-ttu-id="0937d-290">Hello csomag letöltési oldalát, tekintse át a hello projekt kompatibilitási Umbraco CMS verziójával.</span><span class="sxs-lookup"><span data-stu-id="0937d-290">On hello package's download page, review hello project compatibility with Umbraco CMS version.</span></span>

<span data-ttu-id="0937d-291">További részletes információt tooupgrade a webalkalmazást helyileg, [hello általános frissítési útmutatóját lásd](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span><span class="sxs-lookup"><span data-stu-id="0937d-291">For more details about how tooupgrade your web app locally, [see hello general upgrade guidance](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span></span>

<span data-ttu-id="0937d-292">Után a helyi fejlesztési webhely frissítve van, közzététele hello módosítások toohello átmeneti webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0937d-292">After your local development site is upgraded, publish hello changes toohello staging web app.</span></span> <span data-ttu-id="0937d-293">Az alkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="0937d-293">Test your application.</span></span> <span data-ttu-id="0937d-294">Ha az összes megfelelőnek tűnik, használja a hello **felcserélése** tooswap gombra az átmeneti tárolási hely toohello éles webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0937d-294">If all looks good, use hello **Swap** button tooswap your staging site toohello production web app.</span></span> <span data-ttu-id="0937d-295">Hello használata esetén **felcserélése** művelet hello módosításokat megtekintheti, amelyek befolyásolják a webalkalmazás-konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="0937d-295">When you use hello **Swap** operation, you can view hello changes that will be affected in your web app's configuration.</span></span> <span data-ttu-id="0937d-296">Ez **felcserélése** művelet cseréje hello webes alkalmazásokat és adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="0937d-296">This **Swap** operation swaps hello web apps and databases.</span></span> <span data-ttu-id="0937d-297">Hello után **felcserélése**, éles web app lesz pont toohello umbraco-szakasz-db adatbázis hello és átmeneti web app lesz pont tooumbraco-termék-db adatbázis hello.</span><span class="sxs-lookup"><span data-stu-id="0937d-297">After hello **Swap**, hello production web app will point toohello umbraco-stage-db database, and hello staging web app will point tooumbraco-prod-db database.</span></span>

![Lapozófájl-kapacitás preview Umbraco CMS telepítéséhez](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

<span data-ttu-id="0937d-299">Az alábbiakban a csere hello web app és a hello adatbázis előnyei:</span><span class="sxs-lookup"><span data-stu-id="0937d-299">Here are advantages of swapping both hello web app and hello database:</span></span>

* <span data-ttu-id="0937d-300">Visszaállíthatja a webalkalmazás egy másik toohello korábbi verzióját **felcserélése** bármely alkalmazással kapcsolatos problémák esetén.</span><span class="sxs-lookup"><span data-stu-id="0937d-300">You can roll back toohello previous version of your web app with another **Swap** if there are any application issues.</span></span>
* <span data-ttu-id="0937d-301">A frissítés kell toodeploy fájlok és a webes alkalmazás toohello éles webalkalmazás átmeneti hello-adatbázisok és az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="0937d-301">For an upgrade, you need toodeploy files and databases from hello staging web app toohello production web app and database.</span></span> <span data-ttu-id="0937d-302">Számos elemet lépjen hibás fájlok és adatbázisok központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="0937d-302">Many things can go wrong when you deploy files and databases.</span></span> <span data-ttu-id="0937d-303">Hello segítségével **felcserélése** szolgáltatás tárolóhelyek, azt is csökkentheti az állásidőt a frissítés során és a módosítások központi telepítése során esetlegesen előforduló hibákat hello kockázatok csökkentése.</span><span class="sxs-lookup"><span data-stu-id="0937d-303">By using hello **Swap** feature of slots, we can reduce downtime during an upgrade and reduce hello risk of failures that can occur when you deploy changes.</span></span>
* <span data-ttu-id="0937d-304">Mindent **A / B tesztelés** hello segítségével [tesztelése éles környezetben](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0937d-304">You can do **A/B testing** by using hello [Testing in production](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) feature.</span></span>

<span data-ttu-id="0937d-305">Ez a példa mutatja, akkor hello rugalmasságot hello platform, ahol hozhat létre az egyéni modulok hasonló tooUmbraco Courier toomanage üzembe helyezett házirendmodul környezetek között.</span><span class="sxs-lookup"><span data-stu-id="0937d-305">This example shows you hello flexibility of hello platform where you can build custom modules similar tooUmbraco Courier module toomanage deployment across environments.</span></span>

## <a name="references"></a><span data-ttu-id="0937d-306">Referencia</span><span class="sxs-lookup"><span data-stu-id="0937d-306">References</span></span>
[<span data-ttu-id="0937d-307">Agilis szoftverfejlesztői az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0937d-307">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)

[<span data-ttu-id="0937d-308">Átmeneti környezet az Azure App Service web Apps beállítása</span><span class="sxs-lookup"><span data-stu-id="0937d-308">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)

[<span data-ttu-id="0937d-309">Hogyan érik el tooblock webes toonon éles üzembe helyezési</span><span class="sxs-lookup"><span data-stu-id="0937d-309">How tooblock web access toonon-production deployment slots</span></span>](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
