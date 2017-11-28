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
# <a name="use-devops-environments-effectively-for-your-web-apps"></a><span data-ttu-id="3dfe3-103">DevOps környezetek hatékony használata a webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="3dfe3-103">Use DevOps environments effectively for your web apps</span></span>
<span data-ttu-id="3dfe3-104">Ez a cikk bemutatja, hogyan állíthat be és felügyelhet webes alkalmazások központi telepítésének, ha az alkalmazás több verziója különböző környezetekben, például a fejlesztési, illetve átmeneti, minőségi megbízhatósági (QA) és éles.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-104">This article shows you how to set up and manage web application deployments when multiple versions of your application are in various environments, such as development, staging, quality assurance (QA), and production.</span></span> <span data-ttu-id="3dfe3-105">Az alkalmazás minden verziója a telepítés során megadott erre a célra a fejlesztési környezet tekinthető.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-105">Each version of your application can be considered as a development environment for the specific purpose of your deployment process.</span></span> <span data-ttu-id="3dfe3-106">Például a fejlesztők a a QA környezet tesztelje az alkalmazás minőségének, mielőtt azok küldje le a módosításokat, üzemi környezetben.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-106">For example, developers can use the QA environment to test the quality of the application before they push the changes to production.</span></span>
<span data-ttu-id="3dfe3-107">Több fejlesztési környezet feladat lehet, mert később szüksége lesz nyomon követheti a kódot, kezelheti az erőforrásokat (számítási, webalkalmazás, adatbázis, gyorsítótár stb.), és kód telepítése környezetek között.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-107">Multiple development environments can be a challenge because you need to track code, manage resources (compute, web app, database, cache, etc.), and deploy code across environments.</span></span>

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a><span data-ttu-id="3dfe3-108">Állítson be egy nem éles környezetben (szakaszban, fejlesztői, QA)</span><span class="sxs-lookup"><span data-stu-id="3dfe3-108">Set up a non-production environment (stage, dev, QA)</span></span>
<span data-ttu-id="3dfe3-109">Miután egy éles webalkalmazás működik, és, a a következő lépés, ha egy nem éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-109">After a production web app is up and running, the next step is to create a non-production environment.</span></span> <span data-ttu-id="3dfe3-110">Üzembe helyezési használatához győződjön meg arról, hogy futtatja, a Standard vagy prémium szintű Azure App Service csomag módban.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-110">To use deployment slots, make sure that you are running in the Standard or Premium Azure App Service plan mode.</span></span> <span data-ttu-id="3dfe3-111">Üzembe helyezési olyan élő webes alkalmazások, amelyek a saját állomás neve.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-111">Deployment slots are live web apps that have their own host names.</span></span> <span data-ttu-id="3dfe3-112">Webes alkalmazás tartalom és a konfigurációs elemek lecserélhető között két üzembe helyezési, beleértve az éles webalkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-112">Web app content and configuration elements can be swapped between two deployment slots, including the production slot.</span></span> <span data-ttu-id="3dfe3-113">Központi telepítésekor az alkalmazást egy üzembe helyezési pont, töltse le a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-113">When you deploy your application to a deployment slot, you get the following benefits:</span></span>

- <span data-ttu-id="3dfe3-114">Ellenőrizheti egy átmeneti üzembe helyezési tárhelyet a webalkalmazás módosítása előtt felcserélni a az alkalmazás és az éles webalkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-114">You can validate changes to a web app in a staging deployment slot before you swap the app with the production slot.</span></span>
- <span data-ttu-id="3dfe3-115">Amikor először üzembe egy webalkalmazást tárhely és felcserélni a éles környezetben, a tárhely minden példányát vannak tárolóhelyspecifikus előtt éles környezetben felcserélés folyamatban.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-115">When you deploy a web app to a slot first and swap it into production, all instances of the slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="3dfe3-116">Ez a folyamat nem állásidő, a webes alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-116">This process eliminates downtime when you deploy your web app.</span></span> <span data-ttu-id="3dfe3-117">A forgalom átirányítása zökkenőmentes, és kérelmek nem dobja swap műveletek miatt.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-117">The traffic redirection is seamless, and no requests are dropped due to swap operations.</span></span> <span data-ttu-id="3dfe3-118">A teljes munkafolyamat automatizálása, konfigurálja a [automatikus felcserélés](web-sites-staged-publishing.md#configure-auto-swap) , ha nincs szükség a előtti swap érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-118">To automate this entire workflow, configure [Auto Swap](web-sites-staged-publishing.md#configure-auto-swap) when pre-swap validation is not needed.</span></span>
- <span data-ttu-id="3dfe3-119">Egy felcserélés után a tárolóhely, amely a korábban előkészített webes alkalmazás most már rendelkezik az előző éles web app rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-119">After a swap, the slot that has the previously staged web app now has the previous production web app.</span></span> <span data-ttu-id="3dfe3-120">Ha a módosításokat, azokat az éles tárolóhelyre felcserélve nem a várt módon, végezheti el a azonos virtuális azonnal lekérni a "legutóbbi ismert helyes" webes alkalmazás biztonsági.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-120">If the changes swapped into the production slot are not as you expected, you can perform the same swap immediately to get your "last known good" web app back.</span></span>

<span data-ttu-id="3dfe3-121">Egy átmeneti üzembe helyezési pont beállításához tekintse meg a [átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-121">To set up a staging deployment slot, see [Set up staging environments for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="3dfe3-122">Minden környezet tartalmaznia kell a saját erőforrások készletét.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-122">Every environment should include its own set of resources.</span></span> <span data-ttu-id="3dfe3-123">Például ha a webalkalmazás egy adatbázist használ, majd üzemi és átmeneti webalkalmazások használja a különböző adatbázisokhoz.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-123">For example, if your web app uses a database, then both production and staging web apps should use different databases.</span></span> <span data-ttu-id="3dfe3-124">Adja hozzá az átmeneti fejlesztési környezet erőforrások, például az adatbázis, a tároló vagy a gyorsítótár a átmeneti fejlesztési környezet beállítása.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-124">Add staging development environment resources such as database, storage, or cache to set your staging development environment.</span></span>

## <a name="examples-of-using-multiple-development-environments"></a><span data-ttu-id="3dfe3-125">Példák több fejlesztési környezet</span><span class="sxs-lookup"><span data-stu-id="3dfe3-125">Examples of using multiple development environments</span></span>
<span data-ttu-id="3dfe3-126">A projekt forrás kód kezelésének legalább két környezet kell követnie: fejlesztési és éles.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-126">Any project should follow source code management with at least two environments: development and production.</span></span> <span data-ttu-id="3dfe3-127">Használatakor a tartalom felügyeleti rendszerekkel (CMSs), alkalmazás-keretrendszert, stb., az alkalmazás nem támogathatja az ebben a forgatókönyvben testreszabási nélkül.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-127">If you use content management systems (CMSs), application frameworks, etc., the application might not support this scenario without customization.</span></span> <span data-ttu-id="3dfe3-128">Ez eshetőségre az egyes a népszerű keretrendszereket az alábbiakban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-128">This eventuality is true for some of the popular frameworks that are discussed in the following sections.</span></span> <span data-ttu-id="3dfe3-129">Nagy mennyiségű kérdések tudomást szem előtt végzett munka során CMS/keretrendszerek, például:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-129">Lots of questions come to mind when you work with CMS/frameworks, such as:</span></span>

- <span data-ttu-id="3dfe3-130">Hogyan tegye meg bontja a tartalom különböző környezetek?</span><span class="sxs-lookup"><span data-stu-id="3dfe3-130">How do you break the content out into different environments?</span></span>
- <span data-ttu-id="3dfe3-131">Milyen fájlok módosíthatja az nem befolyásolja a keretrendszer verzióját frissítéseket?</span><span class="sxs-lookup"><span data-stu-id="3dfe3-131">What files can you change without affecting framework version updates?</span></span>
- <span data-ttu-id="3dfe3-132">Környezet / konfigurációk kezelése</span><span class="sxs-lookup"><span data-stu-id="3dfe3-132">How do you manage configurations per environment?</span></span>
- <span data-ttu-id="3dfe3-133">Hogyan kezelheti a modulok, a beépülő modulok és az alapvető keretrendszert verziófrissítéseit?</span><span class="sxs-lookup"><span data-stu-id="3dfe3-133">How do you manage version updates for modules, plugins, and the core framework?</span></span>

<span data-ttu-id="3dfe3-134">Számos módon állítsa be a projekthez több környezet.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-134">There are many ways to set up multiple environments for your project.</span></span> <span data-ttu-id="3dfe3-135">A következő példák azt szemléltetik, minden egyes alkalmazáshoz több módszert.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-135">The following examples show one method for each respective application.</span></span>

### <a name="wordpress"></a><span data-ttu-id="3dfe3-136">WordPress</span><span class="sxs-lookup"><span data-stu-id="3dfe3-136">WordPress</span></span>
<span data-ttu-id="3dfe3-137">Ebben a szakaszban, megtudhatja, hogyan állíthat be a telepítési munkafolyamat WordPress tárhelyek használatával.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-137">In this section, you will learn how to set up a deployment workflow by using slots for WordPress.</span></span> <span data-ttu-id="3dfe3-138">WordPress, például a legtöbb CMS megoldások nem támogatja több fejlesztési környezet testreszabási nélkül.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-138">WordPress, like most CMS solutions, does not support multiple development environments without customization.</span></span> <span data-ttu-id="3dfe3-139">Az Azure App Service Web Apps szolgáltatásának szolgáltatásait, amelyek megkönnyítik a tárolni a konfigurációs beállításokat a kód kívül van.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-139">The Web Apps feature of Azure App Service has a few features that make it easy to store configuration settings outside your code.</span></span>

1. <span data-ttu-id="3dfe3-140">Mielőtt létrehozna egy átmeneti helyet, beállítása az alkalmazás kódjában több környezetek támogatására.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-140">Before you create a staging slot, set up your application code to support multiple environments.</span></span> <span data-ttu-id="3dfe3-141">A WordPress több környezetek támogatására, módosítania kell `wp-config.php` a helyi fejlesztési a web-alkalmazás, és adja hozzá a következő kódot a fájl elején.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-141">To support multiple environments in WordPress, you need to edit `wp-config.php` on your local development web app and add the following code at the beginning of the file.</span></span> <span data-ttu-id="3dfe3-142">Ez a folyamat lehetővé teszi az alkalmazás számára válassza ki a megfelelő konfigurációt a kiválasztott környezet alapján.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-142">This process will enable your application to pick the correct configuration based on the selected environment.</span></span>

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

2. <span data-ttu-id="3dfe3-143">Hozzon létre egy webes alkalmazás legfelső szintű nevű mappát `config`, és adja hozzá a `wp-config.azure.php` és `wp-config.local.php` fájlokat, amelyek tartalmazzák az Azure-alapú környezetben és a helyi környezet kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-143">Create a folder under web app root called `config`, and add the `wp-config.azure.php` and `wp-config.local.php` files, which represent your Azure environment and local environment respectively.</span></span>

3. <span data-ttu-id="3dfe3-144">Másolja a következő `wp-config.local.php`:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-144">Copy the following in `wp-config.local.php`:</span></span>

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

    <span data-ttu-id="3dfe3-145">Ezért a kulcsok szemléltetett módon az előző kód segítségével megakadályozhatja, hogy a webalkalmazás a alatt, megtámadott biztonsági beállításokat, egyedi értékeket használja.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-145">Setting the security keys as illustrated in the previous code can help to prevent your web app from being hacked, so use unique values.</span></span> <span data-ttu-id="3dfe3-146">Ha szeretné létrehozni a karakterláncot a biztonsági kulcsok szerepel a kódot is [keresse fel az automatikus készítő](https://api.wordpress.org/secret-key/1.1/salt) új kulcs/érték párok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-146">If you need to generate the string for security keys mentioned in the code, you can [go to the automatic generator](https://api.wordpress.org/secret-key/1.1/salt) to create new key/value pairs.</span></span>

4. <span data-ttu-id="3dfe3-147">Másolja az alábbi kódot a `wp-config.azure.php`:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-147">Copy the following code in `wp-config.azure.php`:</span></span>

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

#### <a name="use-relative-paths"></a><span data-ttu-id="3dfe3-148">Relatív útvonalakat használni</span><span class="sxs-lookup"><span data-stu-id="3dfe3-148">Use relative paths</span></span>
<span data-ttu-id="3dfe3-149">A WordPress alkalmazás konfigurálása egy dolog relatív elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-149">One last thing to configure in the WordPress app is relative paths.</span></span> <span data-ttu-id="3dfe3-150">WordPress URL-cím információkat tárol az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-150">WordPress stores URL information in the database.</span></span> <span data-ttu-id="3dfe3-151">Ez a tároló lehetővé teszi a másikra nehezebb a környezetek mozgó tartalmat.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-151">This storage makes moving content from one environment to another more difficult.</span></span> <span data-ttu-id="3dfe3-152">Frissítenie kell az adatbázis minden alkalommal, amikor a védelemről helyi szakaszban, vagy az éles környezetekben való szakaszban.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-152">You need to update the database every time you move from local to stage or stage to production environments.</span></span> <span data-ttu-id="3dfe3-153">A problémákat, amelyek oka lehet az adatbázis telepítése minden alkalommal, amikor az egyik környezetből a másikba telepít a kockázatok csökkentése érdekében használja a [relatív legfelső szintű hivatkozásokat tartalmaz a beépülő modul](https://wordpress.org/plugins/root-relative-urls/), a rendszergazda WordPress Irányítópult segítségével telepíthető.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-153">To reduce the risk of issues that can be caused with deploying a database every time you deploy from one environment to another, use the [Relative Root links plugin](https://wordpress.org/plugins/root-relative-urls/), which you can install by using the WordPress administrator dashboard.</span></span>

<span data-ttu-id="3dfe3-154">A következő bejegyzés hozzáadása a `wp-config.php` előtt a fájl a `That's all, stop editing!` Megjegyzés:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-154">Add the following entries to your `wp-config.php` file before the `That's all, stop editing!` comment:</span></span>

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

<span data-ttu-id="3dfe3-155">A beépülő modul keresztül aktiválja a `Plugins` WordPress rendszergazda irányítópult menüjében.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-155">Activate the plugin through the `Plugins` menu in WordPress administrator dashboard.</span></span> <span data-ttu-id="3dfe3-156">A WordPress alkalmazás idehivatkozás beállításainak mentése.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-156">Save your permalink settings for WordPress app.</span></span>

#### <a name="the-final-wp-configphp-file"></a><span data-ttu-id="3dfe3-157">A végleges `wp-config.php` fájl</span><span class="sxs-lookup"><span data-stu-id="3dfe3-157">The final `wp-config.php` file</span></span>
<span data-ttu-id="3dfe3-158">A WordPress core változásainak nem lesz hatással a `wp-config.php`, `wp-config.azure.php`, és `wp-config.local.php` fájlokat.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-158">Any WordPress core updates will not affect your `wp-config.php`, `wp-config.azure.php`, and `wp-config.local.php` files.</span></span> <span data-ttu-id="3dfe3-159">Ez a végleges verziójú a `wp-config.php` fájlt:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-159">Here's a final version of the `wp-config.php` file:</span></span>

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

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="3dfe3-160">A fejlesztői környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="3dfe3-160">Set up a staging environment</span></span>
1. <span data-ttu-id="3dfe3-161">Ha már rendelkezik Azure-előfizetése futó WordPress-webalkalmazás, jelentkezzen be a [Azure-portálon](http://portal.azure.com), és keresse meg a WordPress webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-161">If you already have a WordPress web app running on your Azure subscription, sign in to the [Azure portal](http://portal.azure.com), and then go to your WordPress web app.</span></span> <span data-ttu-id="3dfe3-162">Ha még nem rendelkezik egy WordPress webalkalmazást, létrehozhat egy, az Azure piactérről.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-162">If you don't have a WordPress web app, you can create one from the Azure Marketplace.</span></span> <span data-ttu-id="3dfe3-163">További tudnivalókért lásd: [WordPress-webalkalmazás létrehozása az Azure App Service](web-sites-php-web-site-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-163">To learn more, see [Create a WordPress web app in Azure App Service](web-sites-php-web-site-gallery.md).</span></span>
<span data-ttu-id="3dfe3-164">Kattintson a **beállítások** > **üzembe helyezési** > **Hozzáadás** egy üzembe helyezési tárhely létrehozása a nevű *szakasz*.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-164">Click **Settings** > **Deployment slots** > **Add** to create a deployment slot with the name *stage*.</span></span> <span data-ttu-id="3dfe3-165">A telepített környezet tárolóhelye egy másik webes alkalmazást osztja meg ugyanazokat az erőforrásokat, mint az elsődleges webes alkalmazás, amelyet korábban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-165">A deployment slot is another web application that shares the same resources as the primary web app that you created previously.</span></span>

    ![Szakasz üzembe helyezési tárhely létrehozása](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. <span data-ttu-id="3dfe3-167">MySQL-adatbázis egy másik, azaz hozzáadása `wordpress-stage-db`, az erőforráscsoportba `wordpressapp-group`.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-167">Add another MySQL database, say `wordpress-stage-db`, to your resource group, `wordpressapp-group`.</span></span>

    ![A MySQL-adatbázis hozzáadása erőforráscsoport](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. <span data-ttu-id="3dfe3-169">A szakasz üzembe helyezési pont pontot az új adatbázishoz tartozó kapcsolati karakterláncainak frissítéséhez `wordpress-stage-db`.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-169">Update the connection strings for your stage deployment slot to point to the new database, `wordpress-stage-db`.</span></span> <span data-ttu-id="3dfe3-170">A termelési webalkalmazás `wordpressprodapp`, és átmeneti web app alkalmazásban `wordpressprodapp-stage`, a különböző adatbázisokhoz kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-170">Your production web app, `wordpressprodapp`, and staging web app, `wordpressprodapp-stage`, must point to different databases.</span></span>

#### <a name="configure-environment-specific-app-settings"></a><span data-ttu-id="3dfe3-171">Környezetfüggő Alkalmazásbeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3dfe3-171">Configure environment-specific app settings</span></span>
<span data-ttu-id="3dfe3-172">Fejlesztők tárolhatja kulcs/érték párok-karakterláncot az Azure-ban a konfigurációs adatai, úgynevezett **Alkalmazásbeállítások**, a webes alkalmazás, amely van társítva.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-172">Developers can store key/value string pairs in Azure as part of the configuration information, called **App Settings**, that's associated with a web app.</span></span> <span data-ttu-id="3dfe3-173">Futásidőben a webalkalmazások automatikusan az értékek lekérésére, és azok számára elérhetővé tenni a webalkalmazásban futó kód.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-173">At runtime, web apps automatically retrieve these values and make them available to code that's running in your web app.</span></span> <span data-ttu-id="3dfe3-174">Biztonsági szempontból, ennek oka töltött ügyféloldali előnyt bizalmas adatokat, például jelszavakat tartalmazó adatbázis-kapcsolati karakterláncok soha nem jelenik meg a fájlban egyszerű szövegként például `wp-config.php`.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-174">From a security perspective, that is a nice side benefit because sensitive information, such as database connection strings that include passwords, never show up as clear text in a file such as `wp-config.php`.</span></span>

<span data-ttu-id="3dfe3-175">Ez a folyamat, amelynek az ismertetése az alábbi bekezdésekben, akkor hasznos, mert a fájl és az adatbázis módosítása a WordPress alkalmazás tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-175">This process, which is explained in the following paragraphs, is useful because it includes both file changes and database changes for the WordPress app:</span></span>

* <span data-ttu-id="3dfe3-176">WordPress verziófrissítés</span><span class="sxs-lookup"><span data-stu-id="3dfe3-176">WordPress version upgrade</span></span>
* <span data-ttu-id="3dfe3-177">Új hozzáadása vagy szerkesztése, vagy frissítse a beépülő modulok</span><span class="sxs-lookup"><span data-stu-id="3dfe3-177">Add new or edit or upgrade plugins</span></span>
* <span data-ttu-id="3dfe3-178">Új hozzáadása vagy szerkesztése vagy témák frissítése</span><span class="sxs-lookup"><span data-stu-id="3dfe3-178">Add new or edit or upgrade themes</span></span>

<span data-ttu-id="3dfe3-179">Az Alkalmazásbeállítások konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-179">Configure app settings for:</span></span>

* <span data-ttu-id="3dfe3-180">Adatbázis-információ</span><span class="sxs-lookup"><span data-stu-id="3dfe3-180">Database information</span></span>
* <span data-ttu-id="3dfe3-181">Be- / kikapcsolása WordPress naplózás bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="3dfe3-181">Turning on/off WordPress logging</span></span>
* <span data-ttu-id="3dfe3-182">WordPress-biztonsági beállítások</span><span class="sxs-lookup"><span data-stu-id="3dfe3-182">WordPress security settings</span></span>

![Wordpress-webalkalmazás-beállításainak](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

<span data-ttu-id="3dfe3-184">Győződjön meg arról, hogy a termelési web app és szakaszban aljzat adja hozzá a következő beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-184">Make sure that you add the following app settings for your production web app and stage slot.</span></span> <span data-ttu-id="3dfe3-185">Vegye figyelembe, hogy az éles web app és az átmeneti webalkalmazás használja-e a különböző adatbázisokhoz.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-185">Note that the production web app and staging web app use different databases.</span></span>

1. <span data-ttu-id="3dfe3-186">Törölje a jelet a **tárolóhely beállítás** WP_ENV kivételével minden beállítások paraméterek jelölőnégyzetét.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-186">Clear the **Slot Setting** checkbox for all the settings parameters except WP_ENV.</span></span> <span data-ttu-id="3dfe3-187">Ez a folyamat a konfigurációs fog felcserélni a webalkalmazás, fájl és az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-187">This process will swap the configuration for your web app, file content, and database.</span></span> <span data-ttu-id="3dfe3-188">Ha **tárolóhely beállítás** be van jelölve, a webes alkalmazás Alkalmazásbeállítások és kapcsolati karakterlánc-konfiguráció *nem* környezetek között áthelyezése során a **felcserélése** műveletet.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-188">If **Slot Setting** is checked, the web app’s app settings and connection string configuration will *not* move across environments when doing a **Swap** operation.</span></span> <span data-ttu-id="3dfe3-189">Minden adatbázis-változást visszaállított szolgáltatássablonon nem megszakítja a termelési webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-189">Any database changes that are present will not break your production web app.</span></span>

2. <span data-ttu-id="3dfe3-190">A helyi fejlesztési környezet webalkalmazás telepítése az a szakasz web app és az adatbázis WebMatrix vagy az Ön által választott, például FTP, Git vagy PhpMyAdmin eszközök használatával.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-190">Deploy the local development environment web app to the stage web app and database by using WebMatrix or tools of your choice, such as FTP, Git, or PhpMyAdmin.</span></span>

    ![Webes mátrix közzététele párbeszédpanelen WordPress-webalkalmazás](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. <span data-ttu-id="3dfe3-192">Keresse meg, és az átmeneti webalkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-192">Browse and test your staging web app.</span></span> <span data-ttu-id="3dfe3-193">A forgatókönyv a téma a webes alkalmazás esetén frissítenie kell, figyelembe véve itt található az átmeneti webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-193">Considering a scenario where the theme of the web app is to be updated, here is the staging web app.</span></span>

    ![Keresse meg az átmeneti webalkalmazás üzembe helyezési ponti csere előtt](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. <span data-ttu-id="3dfe3-195">Ha az összes megfelelőnek tűnik, kattintson a **felcserélése** helyezze át a tartalmat az éles környezet átmeneti webalkalmazás gombjára.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-195">If all looks good, click the **Swap** button on your staging web app to move your content to the production environment.</span></span> <span data-ttu-id="3dfe3-196">Ebben az esetben, ha valamelyik a web app és az adatbázis során környezetek között minden **Swap** műveletet.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-196">In this case, you swap the web app and the database across environments during every **Swap** operation.</span></span>

    ![A WordPress alkalmazás előzetes módosítások felcserélése](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > <span data-ttu-id="3dfe3-198">Ha a forgatókönyv csak leküldéses fájlok (nincs adatbázis-frissítés), majd ellenőrizze **tárolóhely beállítás** az összes az adatbázissal kapcsolatos *Alkalmazásbeállítások* és *való kapcsolódási karakterláncokat beállítások* a a **a webalkalmazás-beállítások** panel az Azure portálon előtt ezzel a **felcserélése**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-198">If your scenario needs to only push files (no database updates), then check **Slot Setting** for all the database-related *app settings* and *connection strings settings* in the **Web App Settings** blade within the Azure portal before doing the **Swap**.</span></span> <span data-ttu-id="3dfe3-199">Ebben az esetben DB_NAME, DB_HOST, DB_PASSWORD, DB_USER és alapértelmezett kapcsolatikarakterlánc-beállításokat kell nem jelennek meg az előzetes módosítások érhesse el egy **felcserélése**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-199">In this case, DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, and default connection string settings should not show up in preview changes when you do a **Swap**.</span></span> <span data-ttu-id="3dfe3-200">Ekkor, ha végzett a **felcserélése** műveletet, a WordPress webalkalmazás csak a frissítések fájlokat fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-200">At this time, when you complete the **Swap** operation, the WordPress web app will have the updates files only.</span></span>
    >
    >

    <span data-ttu-id="3dfe3-201">Ezzel előtt egy **felcserélése**, ez a termelési WordPress webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-201">Before doing a **Swap**, here is the production WordPress web app.</span></span>
    <span data-ttu-id="3dfe3-202">![Webalkalmazás éles üzembe helyezési ponti csere előtt](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span><span class="sxs-lookup"><span data-stu-id="3dfe3-202">![Production web app before swapping slots](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span></span>

    <span data-ttu-id="3dfe3-203">Után a **felcserélése** műveletet, a téma frissítve lett a termelési webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-203">After the **Swap** operation, the theme has been updated on your production web app.</span></span>

    ![Webalkalmazás éles üzembe helyezési ponti csere után](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. <span data-ttu-id="3dfe3-205">Kell visszaállítania, amikor a termelési webes lépjen **Alkalmazásbeállítások**, és kattintson a **Swap** felcserélni a webalkalmazás és az adatbázis nem éles környezetben való átmeneti helyet gombra.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-205">When you need to roll back, you can go to the production web **App Settings**, and click the **Swap** button to swap the web app and database from production to staging slot.</span></span> <span data-ttu-id="3dfe3-206">Ne feledje, hogy ha az adatbázis-változások bekerülnek a **felcserélése** műveletet, majd a következő alkalommal központi telepítése az átmeneti webalkalmazáshoz is telepíteni kell az adatbázis-változást az átmeneti webalkalmazás az aktuális adatbázisra.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-206">Remember that if database changes are included with a **Swap** operation, then the next time you deploy to your staging web app, you need to deploy the database changes to the current database for your staging web app.</span></span> <span data-ttu-id="3dfe3-207">Az aktuális adatbázisban az előző éles adatbázis vagy a szakasz adatbázisa lehet.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-207">The current database might be the previous production database or the stage database.</span></span>

#### <a name="summary"></a><span data-ttu-id="3dfe3-208">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="3dfe3-208">Summary</span></span>
<span data-ttu-id="3dfe3-209">Az alábbiakban olvashat egy általánosított folyamat bármely alkalmazás, amely rendelkezik egy adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-209">Following is a generalized process for any application that has a database:</span></span>

1. <span data-ttu-id="3dfe3-210">Az alkalmazások telepítése a helyi környezet.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-210">Install the application on your local environment.</span></span>
2. <span data-ttu-id="3dfe3-211">Környezet-specifikus konfigurációk (helyi és az Azure Web Apps) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-211">Include environment-specific configurations (local and Azure Web Apps).</span></span>
3. <span data-ttu-id="3dfe3-212">A Web Apps beállítása az átmeneti és üzemi környezetekben.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-212">Set up your staging and production environments for Web Apps.</span></span>
4. <span data-ttu-id="3dfe3-213">Ha már futó Azure éles alkalmazásokban, szinkronizálja a helyi és átmeneti környezetek éles tartalom (a fájlok vagy kódot és az adatbázis).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-213">If you have a production application already running on Azure, sync your production content (files/code and database) to local and staging environments.</span></span>
5. <span data-ttu-id="3dfe3-214">Az alkalmazás a helyi környezet fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-214">Develop your application on your local environment.</span></span>
6. <span data-ttu-id="3dfe3-215">A termelési webalkalmazás karbantartás vagy zárolt módban helyezze, és átmeneti és fejlesztői környezetben az éles adatbázis-tartalom szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-215">Place your production web app under maintenance or locked mode, and sync database content from production to staging and dev environments.</span></span>
7. <span data-ttu-id="3dfe3-216">Központi telepítése az átmeneti környezet és a vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-216">Deploy to the staging environment and test.</span></span>
8. <span data-ttu-id="3dfe3-217">Központi telepítése éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-217">Deploy to production environment.</span></span>
9. <span data-ttu-id="3dfe3-218">Ismételje meg a 4 – 6.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-218">Repeat steps 4 through 6.</span></span>

### <a name="umbraco"></a><span data-ttu-id="3dfe3-219">Umbraco</span><span class="sxs-lookup"><span data-stu-id="3dfe3-219">Umbraco</span></span>
<span data-ttu-id="3dfe3-220">Ebben a szakaszban megtudhatja, hogyan a Umbraco CMS egyéni modul telepítéséhez használt több DevOps környezetek között.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-220">In this section, you will learn how the Umbraco CMS uses a custom module to deploy across multiple DevOps environments.</span></span> <span data-ttu-id="3dfe3-221">Ez a példa több fejlesztési környezet kezelése különböző megközelítését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-221">This example provides a different approach to managing multiple development environments.</span></span>

<span data-ttu-id="3dfe3-222">[Umbraco CMS](http://umbraco.com/) sok fejlesztők által használt népszerű .NET CMS megoldás.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-222">[Umbraco CMS](http://umbraco.com/) is a popular .NET CMS solution that's used by many developers.</span></span> <span data-ttu-id="3dfe3-223">Biztosítja a [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul fejlesztési átmeneti üzemi környezetbe való központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-223">It provides the [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module to deploy from development to staging to production environments.</span></span> <span data-ttu-id="3dfe3-224">Visual Studio vagy a WebMatrix használatával könnyen létrehozhat egy helyi fejlesztési környezet egy Umbraco CMS webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-224">You can easily create a local development environment for an Umbraco CMS web app by using Visual Studio or WebMatrix.</span></span>

- [<span data-ttu-id="3dfe3-225">A Visual Studio egy Umbraco webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3dfe3-225">Create an Umbraco web app with Visual Studio</span></span>](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [<span data-ttu-id="3dfe3-226">WebMatrix Umbraco webes alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3dfe3-226">Create an Umbraco web app with WebMatrix</span></span>](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

<span data-ttu-id="3dfe3-227">Mindig ne felejtse el eltávolítani a `install` az alkalmazás mappájában, és soha ne töltse fel szakasza vagy éles webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-227">Always remember to remove the `install` folder under your application, and never upload it to stage or production web apps.</span></span> <span data-ttu-id="3dfe3-228">Ez az oktatóanyag a WebMatrix használja.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-228">This tutorial uses WebMatrix.</span></span>

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="3dfe3-229">A fejlesztői környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="3dfe3-229">Set up a staging environment</span></span>
1. <span data-ttu-id="3dfe3-230">Hozzon létre egy üzembe helyezési tárhelyet, amint azt korábban említettük a Umbraco CMS webalkalmazás, feltéve, hogy már rendelkezik egy Umbraco CMS web app és futtatásához.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-230">Create a deployment slot as mentioned previously for the Umbraco CMS web app, assuming you already have an Umbraco CMS web app up and running.</span></span> <span data-ttu-id="3dfe3-231">Ha nem így tesz, létrehozhat egy, a piactéren.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-231">If you do not, you can create one from the Marketplace.</span></span>
2. <span data-ttu-id="3dfe3-232">Frissítse a kapcsolati karakterláncot a szakasz üzembe helyezési pont úgy, hogy az új mutasson **umbraco-szakasz-db** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-232">Update the connection string for your stage deployment slot to point to the new **umbraco-stage-db** database.</span></span> <span data-ttu-id="3dfe3-233">A termelési web app (umbraositecms-1) és az átmeneti web app (umbracositecms-1-előkészítés) *kell* a különböző adatbázisokhoz mutasson.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-233">Your production web app (umbraositecms-1) and staging web app (umbracositecms-1-stage) *must* point to different databases.</span></span>

    ![Frissítse a kapcsolati karakterlánc, web app az új átmeneti adatbázis átmeneti](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. <span data-ttu-id="3dfe3-235">Kattintson a **beolvasása közzétételi beállítások** tartozó telepített környezet tárolóhelye **szakasz**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-235">Click **Get Publish settings** for the deployment slot **stage**.</span></span> <span data-ttu-id="3dfe3-236">Ez a folyamat letölti egy közzétételi beállítások fájlja, amely tartalmazza a Visual Studio vagy a WebMatrix van szüksége, az alkalmazás a helyi fejlesztési webalkalmazásból az Azure web app közzétételét összes információt.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-236">This process will download a publish settings file that stores all the information that Visual Studio or WebMatrix requires to publish your application from the local development web app to the Azure web app.</span></span>

    ![Get beállítás az átmeneti webes alkalmazás közzététele](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. <span data-ttu-id="3dfe3-238">Nyissa meg a helyi fejlesztési webalkalmazás a WebMatrix vagy a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-238">Open your local development web app in WebMatrix or Visual Studio.</span></span> <span data-ttu-id="3dfe3-239">Ez az oktatóanyag a WebMatrix használja.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-239">This tutorial uses WebMatrix.</span></span> <span data-ttu-id="3dfe3-240">Először a közzétételi beállítások fájlja a átmeneti webalkalmazás importálásához.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-240">First, you need to import the publish settings file for your staging web app.</span></span>

    ![A Web Matrix használatával Umbraco közzétételi beállítások importálása](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. <span data-ttu-id="3dfe3-242">Tekintse át a módosításokat a párbeszédpanelen, és a helyi webalkalmazás telepítése az Azure web app alkalmazásban az *umbracositecms-1-szakasz*.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-242">Review changes in the dialog box, and deploy your local web app to your Azure web app, *umbracositecms-1-stage*.</span></span> <span data-ttu-id="3dfe3-243">Fájlok közvetlenül az átmeneti webalkalmazás való telepítésekor, a fájlok hagyja el a `~/app_data/TEMP/` mappa, mert ezek a fájlok rendszer újra létrehozza, amikor a szakasz webalkalmazás első lépések.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-243">When you deploy files directly to your staging web app, you will omit files in the `~/app_data/TEMP/` folder because these files will be regenerated when the stage web app is first started.</span></span> <span data-ttu-id="3dfe3-244">Is kell kihagyja a `~/app_data/umbraco.config` fájlt, amely is újra lesznek generálva.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-244">You should also omit the `~/app_data/umbraco.config` file, which will also be regenerated.</span></span>

    ![Tekintse át a Publish web matrix változásai](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. <span data-ttu-id="3dfe3-246">Miután a Umbraco helyi web app az átmeneti a webes alkalmazás sikeres közzétételéhez, keresse meg az átmeneti webes alkalmazás, és bármely miatt néhány tesztek futtatása.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-246">After you successfully publish the Umbraco local web app to the staging web app, browse to your staging web app, and run a few tests to rule out any issues.</span></span>

#### <a name="set-up-the-courier2-deployment-module"></a><span data-ttu-id="3dfe3-247">Állítsa be a Courier2 telepítési modulja</span><span class="sxs-lookup"><span data-stu-id="3dfe3-247">Set up the Courier2 deployment module</span></span>
<span data-ttu-id="3dfe3-248">Az a [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul, akkor egyszerűen a jobb gombbal leküldéses tartalmat, a stíluslapok és a fejlesztői modulok egy éles a webes alkalmazás egy átmeneti webalkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-248">With the [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module, you can simply right-click to push content, style sheets, and development modules from a staging web app to a production web app.</span></span> <span data-ttu-id="3dfe3-249">Ez az eljárás csökkenti a kockázata, hogy az éles webalkalmazás feltörésével, amikor frissítést telepít.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-249">This process reduces the risk of breaking your production web app when you deploy an update.</span></span>
<span data-ttu-id="3dfe3-250">Vásároljon licencet a a Courier2 a `*.azurewebsites.net` és az egyéni tartomány (azaz http://abc.com).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-250">Purchase a license for Courier2 for the `*.azurewebsites.net` domain and your custom domain (say http://abc.com).</span></span> <span data-ttu-id="3dfe3-251">Miután Ön megvásárolta a licencet, helyezze a letöltött licenc (. – Licencszerződés fájlt) az a `bin` mappát.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-251">After you purchase the license, place the downloaded license (.LIC file) in the `bin` folder.</span></span>

![Licencfájl DROP bin mappában](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. <span data-ttu-id="3dfe3-253">[A Courier2 csomag](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-253">[Download the Courier2 package](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span></span> <span data-ttu-id="3dfe3-254">Jelentkezzen be a szakasz a webes alkalmazás, azaz http://umbracocms-site-stage.azurewebsites.net/umbraco, kattintson a **fejlesztői** menüben, majd kattintson **csomagok** > **helyi telepítőcsomag**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-254">Sign in to your stage web app, say http://umbracocms-site-stage.azurewebsites.net/umbraco, click the **Developer** menu, and then click **Packages** > **Install local package**.</span></span>

    ![Umbraco alkalmazáscsomag-telepítő](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. <span data-ttu-id="3dfe3-256">A telepítő használatával töltse fel a Courier2 csomag.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-256">Upload the Courier2 package by using the installer.</span></span>

    ![Csomag courier modul feltöltése](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. <span data-ttu-id="3dfe3-258">A csomag konfigurálásához courier.config fájlt frissíteni kell a **Config** webalkalmazás mappát.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-258">To configure the package, you need to update the courier.config file under the **Config** folder of your web app.</span></span>

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

4. <span data-ttu-id="3dfe3-259">A `<repositories>`, adja meg az üzemi webhely URL-cím és a felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-259">Under `<repositories>`, enter the production site URL and user information.</span></span>
    <span data-ttu-id="3dfe3-260">Ha az alapértelmezett Umbraco tagsági szolgáltatót használ, adja meg a felügyeleti felhasználót azonosító a &lt;felhasználói&gt; szakasz.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-260">If you are using the default Umbraco membership provider, then add the ID for the Administration user in the &lt;user&gt; section.</span></span>
    <span data-ttu-id="3dfe3-261">Ha egy egyéni Umbraco tagsági szolgáltató használ, `<login>`,`<password>` csatlakozni a munkakörnyezeti helyet a Courier2 modulban.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-261">If you are using a custom Umbraco membership provider, use `<login>`,`<password>` in the Courier2 module to connect to the production site.</span></span>
    <span data-ttu-id="3dfe3-262">További részletekért [tekintse meg a dokumentációt a Courier2 modul](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-262">For more details, [review the documentation for the Courier2 module](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span></span>

5. <span data-ttu-id="3dfe3-263">Hasonlóképpen a munkakörnyezeti helyet a Courier2 modul telepítése, és konfigurálja úgy, hogy a megfelelő courier.config fájlban, amint az itt látható a szakasz a webes alkalmazás mutasson.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-263">Similarly, install the Courier2 module on your production site, and configure it to point to the stage web app in its respective courier.config file as shown here.</span></span>

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

6. <span data-ttu-id="3dfe3-264">Kattintson a **Courier2** az Umbraco CMS webes alkalmazás irányítópult fülre, majd **helyek**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-264">Click the **Courier2** tab in the Umbraco CMS web app dashboard, and then click **Locations**.</span></span> <span data-ttu-id="3dfe3-265">Láthatja a tárház nevét, ahogyan az `courier.config`.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-265">You should see the repository name as mentioned in `courier.config`.</span></span> <span data-ttu-id="3dfe3-266">Ez a folyamat a termelési és az átmeneti webalkalmazások tegye.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-266">Do this process on both your production and staging web apps.</span></span>

    ![Nézet cél web app tárház](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. <span data-ttu-id="3dfe3-268">A munkakörnyezeti helyet a átmeneti helyről tartalom központi telepítéséhez keresse fel **tartalom**, és válasszon ki egy meglévő oldalt, vagy hozzon létre egy új lapot.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-268">To deploy content from the staging site to the production site, go to **Content**, and select an existing page or create a new page.</span></span> <span data-ttu-id="3dfe3-269">A webalkalmazásból, ahol a lap neve lesz jelölhető ki meglévő **első lépések – új**, és kattintson a **mentése és közzététele**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-269">I will select an existing page from my web app where the title of the page is **Getting Started – new**, and then click **Save and Publish**.</span></span>

    ![Módosítsa a lap címe és közzététele](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. <span data-ttu-id="3dfe3-271">Kattintson a jobb gombbal a módosított lap használatával jeleníthetők meg a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-271">Right-click the modified page to view all the options.</span></span> <span data-ttu-id="3dfe3-272">Kattintson a **Courier** megnyitásához a **telepítési** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-272">Click **Courier** to open the **Deployment** dialog box.</span></span> <span data-ttu-id="3dfe3-273">Kattintson a **telepítés** központi telepítésének indítására.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-273">Click **Deploy** to initiate deployment.</span></span>

    ![Courier modul telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. <span data-ttu-id="3dfe3-275">Tekintse át a módosításokat, majd a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-275">Review the changes, and then click **Continue**.</span></span>

    ![Változások áttekintése Courier-modul és-telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    <span data-ttu-id="3dfe3-277">A telepítési naplójának jeleníti meg, ha a telepítés sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-277">The deployment log shows if the deployment was successful.</span></span>

     ![Courier modulból telepítési naplók megtekintése](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. <span data-ttu-id="3dfe3-279">Keresse meg az üzemi webes alkalmazás megtekintéséhez, ha a változtatások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-279">Browse your production web app to see if the changes are reflected.</span></span>

     ![Keresse meg az üzemi webalkalmazás](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

<span data-ttu-id="3dfe3-281">Courier használatával kapcsolatos további tudnivalókért tekintse meg a dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-281">To learn more about how to use Courier, review the documentation.</span></span>

#### <a name="how-to-upgrade-the-umbraco-cms-version"></a><span data-ttu-id="3dfe3-282">A Umbraco CMS verzió frissítése</span><span class="sxs-lookup"><span data-stu-id="3dfe3-282">How to upgrade the Umbraco CMS version</span></span>
<span data-ttu-id="3dfe3-283">Courier fog nem súgó Umbraco CMS egyik verzióról a másikra frissít.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-283">Courier will not help you upgrade from one version of Umbraco CMS to another.</span></span> <span data-ttu-id="3dfe3-284">Amikor frissít egy Umbraco CMS verziója, ki kell választani az nem kompatibilis az egyéni modulok vagy partnerek és a Umbraco alap függvénytárai modulokat.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-284">When you upgrade an Umbraco CMS version, you must check for incompatibilities with your custom modules or modules from partners and the Umbraco Core libraries.</span></span> <span data-ttu-id="3dfe3-285">Az alábbiakban a gyakorlati tanácsokat:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-285">Here are best practices:</span></span>

* <span data-ttu-id="3dfe3-286">Mindig készítsen biztonsági másolatot a web app és az adatbázis frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-286">Always back up your web app and database before you upgrade.</span></span> <span data-ttu-id="3dfe3-287">Az Azure-webalkalmazásokban a webhelyek a biztonsági mentési szolgáltatás segítségével állítsa be automatikus biztonsági mentésekhez, és a hely visszaállítása, ha szükséges a visszaállítási funkcióval.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-287">On web apps in Azure, you can set up automatic backups for your websites by using the backup feature and restore your site if needed by using the restore feature.</span></span> <span data-ttu-id="3dfe3-288">További részletekért lásd: [biztonsági mentése a webalkalmazás](web-sites-backup.md) és [visszaállítása a webalkalmazás](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-288">For more details, see [How to back up your web app](web-sites-backup.md) and [How to restore your web app](web-sites-restore.md).</span></span>
* <span data-ttu-id="3dfe3-289">Ellenőrizze, hogy e-csomagok partnerek frissít verziójával kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-289">Check if packages from partners are compatible with the version you're upgrading to.</span></span> <span data-ttu-id="3dfe3-290">A csomag letöltési oldalát, tekintse át a projekt kompatibilitási Umbraco CMS verziójával.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-290">On the package's download page, review the project compatibility with Umbraco CMS version.</span></span>

<span data-ttu-id="3dfe3-291">A webalkalmazás helyileg, frissítésével kapcsolatos további részletekért [az általános frissítési útmutatóját lásd](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-291">For more details about how to upgrade your web app locally, [see the general upgrade guidance](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span></span>

<span data-ttu-id="3dfe3-292">Hely frissítése után a helyi fejlesztési, a változások közzétételére a átmeneti webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-292">After your local development site is upgraded, publish the changes to the staging web app.</span></span> <span data-ttu-id="3dfe3-293">Az alkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-293">Test your application.</span></span> <span data-ttu-id="3dfe3-294">Ha összes megfelelőnek tűnik, a **Swap** felcserélni a átmeneti helyet a termelési web app az gombra.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-294">If all looks good, use the **Swap** button to swap your staging site to the production web app.</span></span> <span data-ttu-id="3dfe3-295">Ha a **felcserélése** műveletet, megtekintheti a módosításokat, amelyek befolyásolják a webalkalmazás-konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-295">When you use the **Swap** operation, you can view the changes that will be affected in your web app's configuration.</span></span> <span data-ttu-id="3dfe3-296">Ez **felcserélése** művelet cseréje a webalkalmazások és adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-296">This **Swap** operation swaps the web apps and databases.</span></span> <span data-ttu-id="3dfe3-297">Miután a **felcserélése**, a umbraco-szakasz-db adatbázis mutasson az éles web app és az átmeneti web app umbraco-termék-db adatbázis mutasson.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-297">After the **Swap**, the production web app will point to the umbraco-stage-db database, and the staging web app will point to umbraco-prod-db database.</span></span>

![Lapozófájl-kapacitás preview Umbraco CMS telepítéséhez](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

<span data-ttu-id="3dfe3-299">Az alábbiakban a csere, mind a web app és az adatbázis előnyei:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-299">Here are advantages of swapping both the web app and the database:</span></span>

* <span data-ttu-id="3dfe3-300">Segítségével visszaállíthatja az előző verziójára egy másik webalkalmazás **felcserélése** bármely alkalmazással kapcsolatos problémák esetén.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-300">You can roll back to the previous version of your web app with another **Swap** if there are any application issues.</span></span>
* <span data-ttu-id="3dfe3-301">Frissítés esetén kell telepítenie a fájlok és az átmeneti web app az éles a webes alkalmazás-adatbázisok és adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-301">For an upgrade, you need to deploy files and databases from the staging web app to the production web app and database.</span></span> <span data-ttu-id="3dfe3-302">Számos elemet lépjen hibás fájlok és adatbázisok központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-302">Many things can go wrong when you deploy files and databases.</span></span> <span data-ttu-id="3dfe3-303">Használatával a **felcserélése** szolgáltatás tárolóhelyek, azt is csökkentheti az állásidőt a frissítés során és a módosítások központi telepítése során esetlegesen előforduló hibák.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-303">By using the **Swap** feature of slots, we can reduce downtime during an upgrade and reduce the risk of failures that can occur when you deploy changes.</span></span>
* <span data-ttu-id="3dfe3-304">Mindent **A / B tesztelés** használatával a [tesztelése éles környezetben](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-304">You can do **A/B testing** by using the [Testing in production](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) feature.</span></span>

<span data-ttu-id="3dfe3-305">Ez a példa bemutatja, a rugalmasságot, a platform ahol hozhat létre a központi telepítés kezelése környezetek között Umbraco Courier modul hasonló az egyéni modulok.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-305">This example shows you the flexibility of the platform where you can build custom modules similar to Umbraco Courier module to manage deployment across environments.</span></span>

## <a name="references"></a><span data-ttu-id="3dfe3-306">Referencia</span><span class="sxs-lookup"><span data-stu-id="3dfe3-306">References</span></span>
[<span data-ttu-id="3dfe3-307">Agilis szoftverfejlesztői az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3dfe3-307">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)

[<span data-ttu-id="3dfe3-308">Átmeneti környezet az Azure App Service web Apps beállítása</span><span class="sxs-lookup"><span data-stu-id="3dfe3-308">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)

[<span data-ttu-id="3dfe3-309">Nem éles üzembe helyezési web access blokkolása</span><span class="sxs-lookup"><span data-stu-id="3dfe3-309">How to block web access to non-production deployment slots</span></span>](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
