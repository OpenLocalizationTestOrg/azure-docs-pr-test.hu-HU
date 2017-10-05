---
title: "WordPress átalakítása többhelyes környezet az Azure App Service-ben"
description: "Egy meglévő WordPress-webalkalmazás létrehozása az Azure katalógusában révén ismerje és átalakíthatja a WordPress többhelyes telepítés"
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: fe52dbf4-179c-42f1-adf9-d6a9af920c39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4a15fb5e97d2ca57e5883c07651c372c54021c92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a><span data-ttu-id="ac131-103">WordPress átalakítása többhelyes környezet az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="ac131-103">Convert WordPress to Multisite in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="ac131-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ac131-104">Overview</span></span>
<span data-ttu-id="ac131-105">*Által [Ben Lobaugh][ben-lobaugh], [Microsoft nyílt technológiák Inc.][ms-open-tech]*</span><span class="sxs-lookup"><span data-stu-id="ac131-105">*By [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*</span></span>

<span data-ttu-id="ac131-106">Ebben az oktatóanyagban, megtudhatja, hogyan egy meglévő WordPress-webalkalmazás létrehozása az Azure katalógusában révén és átalakíthatja egy WordPress többhelyes telepítést.</span><span class="sxs-lookup"><span data-stu-id="ac131-106">In this tutorial, you will learn how to take an existing WordPress web app created through the gallery in Azure and convert it into a WordPress Multisite install.</span></span> <span data-ttu-id="ac131-107">Emellett megtudhatja, hogyan rendelhető hozzá egyéni tartományt a telepítés belül alwebhelyek mindegyikének.</span><span class="sxs-lookup"><span data-stu-id="ac131-107">Additionally, you will learn how to assign a custom domain to each of the subsites within your install.</span></span>

<span data-ttu-id="ac131-108">Feltételezzük, hogy rendelkezik-e a WordPress egy meglévő telepítését.</span><span class="sxs-lookup"><span data-stu-id="ac131-108">It is assumed that you have an existing installation of WordPress.</span></span> <span data-ttu-id="ac131-109">Ha nem, kövesse az útmutatást [egy WordPress-webhely létrehozása az Azure-ban a gyűjteményből][website-from-gallery].</span><span class="sxs-lookup"><span data-stu-id="ac131-109">If you do not, please follow the guidance provided in [Create a WordPress web site from the gallery in Azure][website-from-gallery].</span></span>

<span data-ttu-id="ac131-110">Egy meglévő WordPress konvertálása a többhelyes környezet egyetlen hely telepítésének általában meglehetősen egyszerű, és a kezdeti lépések származhat rögtön a [hozzon létre egy hálózati] [ wordpress-codex-create-a-network] lapot a [WordPress kódex](http://codex.wordpress.org).</span><span class="sxs-lookup"><span data-stu-id="ac131-110">Converting an existing WordPress single site install to Multisite is generally fairly simple, and many of the initial steps here come straight from the [Create A Network][wordpress-codex-create-a-network] page on the [WordPress Codex](http://codex.wordpress.org).</span></span>

<span data-ttu-id="ac131-111">Lássunk neki.</span><span class="sxs-lookup"><span data-stu-id="ac131-111">Let's get started.</span></span>

## <a name="allow-multisite"></a><span data-ttu-id="ac131-112">Többhelyes konfiguráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ac131-112">Allow Multisite</span></span>
<span data-ttu-id="ac131-113">Először engedélyeznie kell a többhelyes telepítés keresztül a `wp-config.php` fájlt a **WP\_engedélyezése\_TÖBBHELYES** konstans.</span><span class="sxs-lookup"><span data-stu-id="ac131-113">You first need to enable Multisite through the `wp-config.php` file with the **WP\_ALLOW\_MULTISITE** constant.</span></span> <span data-ttu-id="ac131-114">Két módszer a webes alkalmazás fájlok szerkesztésére: az egyik FTP és a második és a Git segítségével.</span><span class="sxs-lookup"><span data-stu-id="ac131-114">There are two methods to edit your web app files: the first is through FTP, and the second through Git.</span></span> <span data-ttu-id="ac131-115">Ha nincs tisztában a hogyan egyik módszert sem állíthatja, olvassa el az alábbi oktatóanyagok:</span><span class="sxs-lookup"><span data-stu-id="ac131-115">If you are unfamiliar with how to setup either of these methods, please refer to the following tutorials:</span></span>

* <span data-ttu-id="ac131-116">[A MySQL és az FTP-PHP-webhely][website-w-mysql-and-ftp-ftp-setup]</span><span class="sxs-lookup"><span data-stu-id="ac131-116">[PHP web site with MySQL and FTP][website-w-mysql-and-ftp-ftp-setup]</span></span>
* <span data-ttu-id="ac131-117">[PHP-webhely MySQL és Git][website-w-mysql-and-git-git-setup]</span><span class="sxs-lookup"><span data-stu-id="ac131-117">[PHP web site with MySQL and Git][website-w-mysql-and-git-git-setup]</span></span>

<span data-ttu-id="ac131-118">Nyissa meg a `wp-config.php` a szerkesztőben a fájlt, és adja hozzá a következő fenti a `/* That's all, stop editing! Happy blogging. */` sor.</span><span class="sxs-lookup"><span data-stu-id="ac131-118">Open the `wp-config.php` file with the editor of your choosing and add the following above the `/* That's all, stop editing! Happy blogging. */` line.</span></span>

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

<span data-ttu-id="ac131-119">Mindenképpen mentse a fájlt, és töltse fel a kiszolgálóhoz!</span><span class="sxs-lookup"><span data-stu-id="ac131-119">Be sure to save the file and upload it back to the server!</span></span>

## <a name="network-setup"></a><span data-ttu-id="ac131-120">Hálózati beállítás</span><span class="sxs-lookup"><span data-stu-id="ac131-120">Network Setup</span></span>
<span data-ttu-id="ac131-121">Jelentkezzen be a *wp-rendszergazda* területén a webalkalmazást, és meg kell jelennie az új cikket a **eszközök** nevű menü **hálózati beállítás**.</span><span class="sxs-lookup"><span data-stu-id="ac131-121">Log in to the *wp-admin* area of your web app and you should see a new item under the **Tools** menu called **Network Setup**.</span></span> <span data-ttu-id="ac131-122">Kattintson a **hálózati beállítás** , és töltse ki a hálózat adatait.</span><span class="sxs-lookup"><span data-stu-id="ac131-122">Click **Network Setup** and fill in the details of your network.</span></span>

![Hálózati beállítási képernyője][wordpress-network-setup]

<span data-ttu-id="ac131-124">Ez az oktatóanyag használja a *alkönyvtár* séma helyet, mert mindig működnek, és azt szeretné állítani minden aloldal egyéni tartományok az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="ac131-124">This tutorial uses the *Sub-directories* site schema because it should always work, and we will be setting up custom domains for each subsite later in the tutorial.</span></span> <span data-ttu-id="ac131-125">Azonban lehetővé kell tenni altartomány telepíteni, ha leképez egy tartományhoz, a telepítő a [Azure Portal](https://portal.azure.com) és helyettesítő DNS beállításának megfelelően.</span><span class="sxs-lookup"><span data-stu-id="ac131-125">However, it should be possible to setup a subdomain install if you map a domain through the [Azure Portal](https://portal.azure.com) and setup wildcard DNS properly.</span></span>

<span data-ttu-id="ac131-126">További információ a altartomány vs alkönyvtár beállítások: a [többhelyes hálózati típusú] [ wordpress-codex-types-of-networks] a WordPress kódex foglalkozó.</span><span class="sxs-lookup"><span data-stu-id="ac131-126">For more information on sub-domain vs sub-directory setups see the [Types of multisite network][wordpress-codex-types-of-networks] article on the WordPress Codex.</span></span>

## <a name="enable-the-network"></a><span data-ttu-id="ac131-127">A hálózati engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ac131-127">Enable the Network</span></span>
<span data-ttu-id="ac131-128">A hálózati konfigurálva van az adatbázisban, de egy további lépéssel engedélyezheti a hálózati funkcióit.</span><span class="sxs-lookup"><span data-stu-id="ac131-128">The network is now configured in the database, but there is one remaining step to enable the network functionality.</span></span> <span data-ttu-id="ac131-129">Véglegesítse a `wp-config.php` beállításait és győződjön meg arról `web.config` megfelelően irányítja a helyekhez.</span><span class="sxs-lookup"><span data-stu-id="ac131-129">Finalize the `wp-config.php` settings and ensure `web.config` properly routes each site.</span></span>

<span data-ttu-id="ac131-130">Kattintás után a **telepítése** gombra a *hálózati beállítás* lapon WordPress megpróbálja frissíteni a `wp-config.php` és `web.config` fájlokat.</span><span class="sxs-lookup"><span data-stu-id="ac131-130">After clicking the **Install** button on the *Network Setup* page, WordPress will attempt to update the `wp-config.php` and `web.config` files.</span></span> <span data-ttu-id="ac131-131">Azonban a frissítése sikerült-e a fájlokat mindig ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="ac131-131">However, you should always check the files to ensure the updates were successful.</span></span> <span data-ttu-id="ac131-132">Ha nem, ezen a képernyőn megjelennek a szükséges frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="ac131-132">If not, this screen will present you with the necessary updates.</span></span> <span data-ttu-id="ac131-133">Módosítsa és mentse a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="ac131-133">Edit and save the files.</span></span>

<span data-ttu-id="ac131-134">Elvégzése után ezeket a frissítéseket, szüksége lesz a jelentkezzen ki, majd jelentkezzen be a wp-rendszergazda irányítópult biztonsági.</span><span class="sxs-lookup"><span data-stu-id="ac131-134">After making these updates you will need to log out and log back into the wp-admin dashboard.</span></span>

<span data-ttu-id="ac131-135">Most kell egy további menü feliratú admin menüsávon **saját webhelyek**.</span><span class="sxs-lookup"><span data-stu-id="ac131-135">There should now be an additional menu on the admin bar labeled **My Sites**.</span></span> <span data-ttu-id="ac131-136">Ebben a menüben lehetővé teszi, hogy az új hálózati keresztül vezérlését a **hálózati rendszergazda** irányítópult.</span><span class="sxs-lookup"><span data-stu-id="ac131-136">This menu allows you to control your new network through the **Network Admin** dashboard.</span></span>

## <a name="adding-custom-domains"></a><span data-ttu-id="ac131-137">Egyéni tartományok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ac131-137">Adding custom domains</span></span>
<span data-ttu-id="ac131-138">A [WordPress MU tartomány leképezése] [ wordpress-plugin-wordpress-mu-domain-mapping] beépülő modul segítségével egy kezelheti az egyéni tartományok hozzáadása a hálózat valamelyik helyéhez.</span><span class="sxs-lookup"><span data-stu-id="ac131-138">The [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] plugin makes it a breeze to add custom domains to any site in your network.</span></span> <span data-ttu-id="ac131-139">Ahhoz, hogy a beépülő modul megfelelően működjenek kell tennie néhány további beállítást, a portál, valamint a tartományregisztrálónál.</span><span class="sxs-lookup"><span data-stu-id="ac131-139">In order for the plugin to operate properly, you need to do some additional setup on the Portal, and also at your domain registrar.</span></span>

## <a name="enable-domain-mapping-to-the-web-app"></a><span data-ttu-id="ac131-140">A webalkalmazás tartomány hozzárendelésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ac131-140">Enable domain mapping to the web app</span></span>
<span data-ttu-id="ac131-141">A **szabad** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) üzemmódja nem támogatja az egyéni tartományok hozzáadása webalkalmazások terv.</span><span class="sxs-lookup"><span data-stu-id="ac131-141">The **Free** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plan mode does not support adding custom domains to Web Apps.</span></span> <span data-ttu-id="ac131-142">Váltson át kell **megosztott** vagy **szabványos** mód.</span><span class="sxs-lookup"><span data-stu-id="ac131-142">You will need to switch to **Shared** or **Standard** mode.</span></span> <span data-ttu-id="ac131-143">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ac131-143">To do this:</span></span>

* <span data-ttu-id="ac131-144">Jelentkezzen be az Azure portálon, és keresse meg a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ac131-144">Log in to the Azure Portal and locate your web app.</span></span> 
* <span data-ttu-id="ac131-145">Kattintson a **vertikális felskálázás** lapján **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="ac131-145">Click on the **Scale up** tab in **Settings**.</span></span>
* <span data-ttu-id="ac131-146">A **általános**, válassza *megosztott* vagy *STANDARD*</span><span class="sxs-lookup"><span data-stu-id="ac131-146">Under **General**, select either *SHARED* or *STANDARD*</span></span>
* <span data-ttu-id="ac131-147">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="ac131-147">Click **Save**</span></span>

<span data-ttu-id="ac131-148">Egy üzenet jelenhet kéri, hogy ellenőrizze a módosítást, és megerősíti, hogy a webalkalmazás most merülhet fel, a költség, attól függően, használatának és beállíthatja a további konfigurálást.</span><span class="sxs-lookup"><span data-stu-id="ac131-148">You may receive a message asking you to verify the change and acknowledge your web app may now incur a cost, depending upon usage and the other configuration you set.</span></span>

<span data-ttu-id="ac131-149">Feldolgozni az új beállítások néhány másodpercet vesz igénybe a tartomány megkezdték egy időben most van.</span><span class="sxs-lookup"><span data-stu-id="ac131-149">It takes a few seconds to process the new settings, so now is a good time to start setting up your domain.</span></span>

## <a name="verify-your-domain"></a><span data-ttu-id="ac131-150">Ellenőrizze a tartományt</span><span class="sxs-lookup"><span data-stu-id="ac131-150">Verify your domain</span></span>
<span data-ttu-id="ac131-151">Mielőtt Azure Web Apps lehetővé teszi, hogy a hely rendelni egy tartományt, először ellenőrizze, hogy rendelkezik-e a tartomány hozzárendelését a engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ac131-151">Before Azure Web Apps will allow you to map a domain to the site, you first need to verify that you have the authorization to map the domain.</span></span> <span data-ttu-id="ac131-152">Ehhez hozzá kell adnia egy új CNAME rekordot a DNS-bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="ac131-152">To do so, you must add a new CNAME record to your DNS entry.</span></span>

* <span data-ttu-id="ac131-153">Jelentkezzen be a tartomány DNS-kezelő</span><span class="sxs-lookup"><span data-stu-id="ac131-153">Log in to your domain's DNS manager</span></span>
* <span data-ttu-id="ac131-154">Hozzon létre egy új CNAME *awverify*</span><span class="sxs-lookup"><span data-stu-id="ac131-154">Create a new CNAME *awverify*</span></span>
* <span data-ttu-id="ac131-155">Pont *awverify* való *awverify. YOUR_DOMAIN.azurewebsites.NET*</span><span class="sxs-lookup"><span data-stu-id="ac131-155">Point *awverify* to *awverify.YOUR_DOMAIN.azurewebsites.net*</span></span>

<span data-ttu-id="ac131-156">Ez eltarthat egy ideig, a DNS módosítások teljes érvénybe lépjen, így ha az alábbi lépéseket nem működnek, nyissa meg kávészünetet, ellenőrizze, majd térjen vissza, és próbálja meg újból.</span><span class="sxs-lookup"><span data-stu-id="ac131-156">It may take some time for the DNS changes to go into full effect, so if the following steps do not work immediately, go make a cup of coffee, then come back and try again.</span></span>

## <a name="add-the-domain-to-the-web-app"></a><span data-ttu-id="ac131-157">A tartomány hozzáadása a webalkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="ac131-157">Add the domain to the web app</span></span>
<span data-ttu-id="ac131-158">Térjen vissza a webalkalmazás az Azure portálon keresztül, kattintson a **beállítások**, és kattintson a **egyéni tartományok és SSL**.</span><span class="sxs-lookup"><span data-stu-id="ac131-158">Return to your web app through the Azure Portal, click **Settings**, and then click **Custom domains and SSL**.</span></span>

<span data-ttu-id="ac131-159">Ha a *SSL-beállítások* vannak jelenik meg, megjelenik a mezőket, amelyen a webalkalmazás hozzárendelni kívánt összes tartományát bemeneteket.</span><span class="sxs-lookup"><span data-stu-id="ac131-159">When the *SSL settings* are displayed, you will see the fields where you will input all the domains which you wish to assign to your web app.</span></span> <span data-ttu-id="ac131-160">Ha egy tartomány nem szerepel, azt nem elérhetők leképezés belül WordPress, függetlenül attól, hogy a tartomány DNS beállítása.</span><span class="sxs-lookup"><span data-stu-id="ac131-160">If a domain is not listed here, it will not be available for mapping inside WordPress, regardless of how the domain DNS is setup.</span></span>

![Egyéni tartományok párbeszédpanel kezelése][wordpress-manage-domains]

<span data-ttu-id="ac131-162">Írja be a tartomány, a szövegmezőbe, után Azure ellenőrzi, hogy a korábban létrehozott CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="ac131-162">After typing your domain into the text box, Azure will verify the CNAME record you created previously.</span></span> <span data-ttu-id="ac131-163">Ha a DNS propagálása nem teljes, egy piros jelző jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ac131-163">If the DNS has not fully propagated, a red indicator will show.</span></span> <span data-ttu-id="ac131-164">Ha sikeres volt, egy zöld pipa jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ac131-164">If it was successful, you will see a green checkmark.</span></span> 

<span data-ttu-id="ac131-165">Jegyezze fel az IP-cím szerepel a párbeszédpanel alján.</span><span class="sxs-lookup"><span data-stu-id="ac131-165">Take note of the IP Address listed at the bottom of the dialog.</span></span> <span data-ttu-id="ac131-166">Szüksége lesz a tartomány az A rekord beállítása.</span><span class="sxs-lookup"><span data-stu-id="ac131-166">You will need this to setup the A record for your domain.</span></span>

## <a name="setup-the-domain-a-record"></a><span data-ttu-id="ac131-167">A tartomány egy olyan rekordot beállítása</span><span class="sxs-lookup"><span data-stu-id="ac131-167">Setup the domain A record</span></span>
<span data-ttu-id="ac131-168">Ha a többi lépés sikeres volt, előfordulhat, hogy most rendel a tartományhoz az Azure-webalkalmazásban a DNS A rekord keresztül.</span><span class="sxs-lookup"><span data-stu-id="ac131-168">If the other steps were successful, you may now assign the domain to your Azure web app through a DNS A record.</span></span> 

<span data-ttu-id="ac131-169">Itt megjegyezni, hogy az Azure web apps fogadja el CNAME és az A rekordok, azonban fontos, *kell* egy A rekordot használja a megfelelő tartomány hozzárendelésének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ac131-169">It is important to note here that Azure web apps accept both CNAME and A records, however you *must* use an A record to enable proper domain mapping.</span></span> <span data-ttu-id="ac131-170">Egy olyan CNAME REKORDOT nem továbbítható egy másik CNAME Ez mit Azure létre az Ön YOUR_DOMAIN.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="ac131-170">A CNAME cannot be forwarded to another CNAME, which is what Azure created for you with YOUR_DOMAIN.azurewebsites.net.</span></span>

<span data-ttu-id="ac131-171">Az előző lépésben az IP-címet használja, a DNS-kezelő lépjen vissza, és az A rekord ponthoz, hogy az IP-beállítása.</span><span class="sxs-lookup"><span data-stu-id="ac131-171">Using the IP address from the previous step, return to your DNS manager and setup the A record to point to that IP.</span></span>

## <a name="install-and-setup-the-plugin"></a><span data-ttu-id="ac131-172">Telepítse és állítsa be a beépülő modul</span><span class="sxs-lookup"><span data-stu-id="ac131-172">Install and setup the plugin</span></span>
<span data-ttu-id="ac131-173">WordPress többhelyes környezet jelenleg nem rendelkezik beépített módszert a egyéni tartományokat.</span><span class="sxs-lookup"><span data-stu-id="ac131-173">WordPress Multisite currently does not have a built-in method to map custom domains.</span></span> <span data-ttu-id="ac131-174">Van azonban egy beépülő modul nevű [WordPress MU tartomány leképezése] [ wordpress-plugin-wordpress-mu-domain-mapping] a funkciót, amely ad meg.</span><span class="sxs-lookup"><span data-stu-id="ac131-174">However, there is a plugin called [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] that adds the functionality for you.</span></span> <span data-ttu-id="ac131-175">Jelentkezzen be a webhely, a hálózati rendszergazda része, és telepítse a **WordPress MU tartomány leképezése** beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="ac131-175">Log in to the Network Admin portion of your site and install the **WordPress MU Domain Mapping** plugin.</span></span>

<span data-ttu-id="ac131-176">Miután telepítette, és a beépülő modul aktiválása, látogasson el **beállítások** > **tartomány leképezése** a beépülő modul konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ac131-176">After installing and activating the plugin, visit **Settings** > **Domain Mapping** to configure the plugin.</span></span> <span data-ttu-id="ac131-177">Az első szövegmezőjének *kiszolgáló IP-címe*, adjon meg az IP-cím, a tartomány az A rekord beállítására használatos.</span><span class="sxs-lookup"><span data-stu-id="ac131-177">In the first textbox, *Server IP Address*, input the IP Address you used to setup the A record for the domain.</span></span> <span data-ttu-id="ac131-178">Adja meg *beállítások* meg desire (az alapértelmezett beállításokat gyakran rendben), majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ac131-178">Set any *Domain Options* you desire (the defaults are often fine) and click **Save**.</span></span>

## <a name="map-the-domain"></a><span data-ttu-id="ac131-179">A tartomány hozzárendelését</span><span class="sxs-lookup"><span data-stu-id="ac131-179">Map the domain</span></span>
<span data-ttu-id="ac131-180">Látogasson el a **irányítópult** kívánja a tartomány hozzárendelését a helyhez.</span><span class="sxs-lookup"><span data-stu-id="ac131-180">Visit the **Dashboard** for the site you wish to map the domain to.</span></span> <span data-ttu-id="ac131-181">Kattintson a **eszközök** > **tartomány leképezése** és az új tartomány írja be a szövegmezőbe, majd kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ac131-181">Click on **Tools** > **Domain Mapping** and type the new domain into the textbox and click **Add**.</span></span>

<span data-ttu-id="ac131-182">Alapértelmezés szerint az új tartomány automatikusan létrehozott hely tartományhoz felülíródik.</span><span class="sxs-lookup"><span data-stu-id="ac131-182">By default, the new domain will be rewritten to the autogenerated site domain.</span></span> <span data-ttu-id="ac131-183">Ha azt szeretné, hogy az új tartományhoz, a jelölőnégyzet küldött összes forgalom a *ebben a blogban elsődleges tartományt* mezőben mentés előtt.</span><span class="sxs-lookup"><span data-stu-id="ac131-183">If you want to have all traffic sent to the new domain, check the *Primary domain for this blog* box before saving.</span></span> <span data-ttu-id="ac131-184">Tartományok korlátlan számú hozzá egy helyhez, de csak egy elsődleges lehet.</span><span class="sxs-lookup"><span data-stu-id="ac131-184">You can add an unlimited number of domains to a site, but  only one can be primary.</span></span>

## <a name="do-it-again"></a><span data-ttu-id="ac131-185">Hajtsa végre újra</span><span class="sxs-lookup"><span data-stu-id="ac131-185">Do it again</span></span>
<span data-ttu-id="ac131-186">Az Azure Web Apps lehetővé teszi a tartományok korlátlan számú hozzáadása a webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ac131-186">Azure Web Apps allow you to add an unlimited number of domains to a web app.</span></span> <span data-ttu-id="ac131-187">Szüksége lesz a hajtható végre egy másik tartomány hozzáadása a **ellenőrizze a tartományt** és **beállítása a tartomány egy olyan rekordot** részekben az egyes tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="ac131-187">To add another domain you will need to execute the **Verify your domain** and **Setup the domain A record** sections for each domain.</span></span>    

> [!NOTE]
> <span data-ttu-id="ac131-188">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="ac131-188">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="ac131-189">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="ac131-189">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="ac131-190">A változások</span><span class="sxs-lookup"><span data-stu-id="ac131-190">What's changed</span></span>
* <span data-ttu-id="ac131-191">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="ac131-191">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png


