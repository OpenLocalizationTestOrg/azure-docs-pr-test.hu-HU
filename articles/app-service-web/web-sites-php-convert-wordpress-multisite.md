---
title: aaaConvert WordPress tooMultisite az Azure App Service-ben
description: "Megtudhatja, hogyan tootake WordPress-webalkalmazás létrehozása az Azure-ban hello gyűjtemény révén, és konvertálja tooWordPress a többhelyes telepítés"
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
ms.openlocfilehash: 1153f0a8043de875f081704cd0a124776758878c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-wordpress-toomultisite-in-azure-app-service"></a><span data-ttu-id="c61d4-103">Alakítsa át a WordPress tooMultisite az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="c61d4-103">Convert WordPress tooMultisite in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="c61d4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c61d4-104">Overview</span></span>
<span data-ttu-id="c61d4-105">*Által [Ben Lobaugh][ben-lobaugh], [Microsoft nyílt technológiák Inc.][ms-open-tech]*</span><span class="sxs-lookup"><span data-stu-id="c61d4-105">*By [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*</span></span>

<span data-ttu-id="c61d4-106">Ebből az oktatóanyagból megtudhatja, hogyan tootake WordPress-webalkalmazás létrehozott Azure-ban és telepítése egy WordPress többhelyes be azt a konvertálás hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="c61d4-106">In this tutorial, you will learn how tootake an existing WordPress web app created through hello gallery in Azure and convert it into a WordPress Multisite install.</span></span> <span data-ttu-id="c61d4-107">Emellett megtudhatja, hogyan tooassign egy egyéni tartomány tooeach a hello alwebhelyek belül a telepítést.</span><span class="sxs-lookup"><span data-stu-id="c61d4-107">Additionally, you will learn how tooassign a custom domain tooeach of hello subsites within your install.</span></span>

<span data-ttu-id="c61d4-108">Feltételezzük, hogy rendelkezik-e a WordPress egy meglévő telepítését.</span><span class="sxs-lookup"><span data-stu-id="c61d4-108">It is assumed that you have an existing installation of WordPress.</span></span> <span data-ttu-id="c61d4-109">Ha nem így tesz, adjon követve hello [egy WordPress-webhely létrehozása az Azure-ban hello gyűjteményből][website-from-gallery].</span><span class="sxs-lookup"><span data-stu-id="c61d4-109">If you do not, please follow hello guidance provided in [Create a WordPress web site from hello gallery in Azure][website-from-gallery].</span></span>

<span data-ttu-id="c61d4-110">Egy meglévő WordPress konvertálása egyetlen hely telepítési tooMultisite általában meglehetősen egyszerű, és hello Itt a kezdeti lépések származhat rögtön hello [hozzon létre egy hálózati] [ wordpress-codex-create-a-network] oldalon, hello [WordPress kódex](http://codex.wordpress.org).</span><span class="sxs-lookup"><span data-stu-id="c61d4-110">Converting an existing WordPress single site install tooMultisite is generally fairly simple, and many of hello initial steps here come straight from hello [Create A Network][wordpress-codex-create-a-network] page on hello [WordPress Codex](http://codex.wordpress.org).</span></span>

<span data-ttu-id="c61d4-111">Lássunk neki.</span><span class="sxs-lookup"><span data-stu-id="c61d4-111">Let's get started.</span></span>

## <a name="allow-multisite"></a><span data-ttu-id="c61d4-112">Többhelyes konfiguráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c61d4-112">Allow Multisite</span></span>
<span data-ttu-id="c61d4-113">A többhelyes telepítés tooenable keresztül hello először `wp-config.php` hello fájl **WP\_engedélyezése\_a többhelyes telepítés** konstans.</span><span class="sxs-lookup"><span data-stu-id="c61d4-113">You first need tooenable Multisite through hello `wp-config.php` file with hello **WP\_ALLOW\_MULTISITE** constant.</span></span> <span data-ttu-id="c61d4-114">A webes alkalmazás fájljai két módszer tooedit nincsenek: hello első az FTP és a Git segítségével második hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="c61d4-114">There are two methods tooedit your web app files: hello first is through FTP, and hello second through Git.</span></span> <span data-ttu-id="c61d4-115">Ha nem ismeri, hogyan toosetup mindkét eljárás, tekintse meg a következő oktatóanyagok toohello:</span><span class="sxs-lookup"><span data-stu-id="c61d4-115">If you are unfamiliar with how toosetup either of these methods, please refer toohello following tutorials:</span></span>

* <span data-ttu-id="c61d4-116">[A MySQL és az FTP-PHP-webhely][website-w-mysql-and-ftp-ftp-setup]</span><span class="sxs-lookup"><span data-stu-id="c61d4-116">[PHP web site with MySQL and FTP][website-w-mysql-and-ftp-ftp-setup]</span></span>
* <span data-ttu-id="c61d4-117">[PHP-webhely MySQL és Git][website-w-mysql-and-git-git-setup]</span><span class="sxs-lookup"><span data-stu-id="c61d4-117">[PHP web site with MySQL and Git][website-w-mysql-and-git-git-setup]</span></span>

<span data-ttu-id="c61d4-118">Nyissa meg hello `wp-config.php` hello szerkesztővel, a fájlt, és adja hozzá hello következő fent hello `/* That's all, stop editing! Happy blogging. */` sor.</span><span class="sxs-lookup"><span data-stu-id="c61d4-118">Open hello `wp-config.php` file with hello editor of your choosing and add hello following above hello `/* That's all, stop editing! Happy blogging. */` line.</span></span>

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

<span data-ttu-id="c61d4-119">Lehet, hogy toosave hello fájlt, és töltse fel hátsó toohello server!</span><span class="sxs-lookup"><span data-stu-id="c61d4-119">Be sure toosave hello file and upload it back toohello server!</span></span>

## <a name="network-setup"></a><span data-ttu-id="c61d4-120">Hálózati beállítás</span><span class="sxs-lookup"><span data-stu-id="c61d4-120">Network Setup</span></span>
<span data-ttu-id="c61d4-121">Jelentkezzen be toohello *wp-rendszergazda* , és a webes alkalmazást területe kell megjelennie az hello egy új cikket **eszközök** nevű menü **hálózati beállítás**.</span><span class="sxs-lookup"><span data-stu-id="c61d4-121">Log in toohello *wp-admin* area of your web app and you should see a new item under hello **Tools** menu called **Network Setup**.</span></span> <span data-ttu-id="c61d4-122">Kattintson a **hálózati beállítás** , és töltse ki a hálózat hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="c61d4-122">Click **Network Setup** and fill in hello details of your network.</span></span>

![Hálózati beállítási képernyője][wordpress-network-setup]

<span data-ttu-id="c61d4-124">Ez az oktatóanyag használja hello *alkönyvtár* séma helyet, mert mindig működnek, és azt szeretné állítani minden aloldal egyéni tartományok hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="c61d4-124">This tutorial uses hello *Sub-directories* site schema because it should always work, and we will be setting up custom domains for each subsite later in hello tutorial.</span></span> <span data-ttu-id="c61d4-125">Azonban lehetséges toosetup altartomány telepíteni, ha leképez egy tartományba hello keresztül kell [Azure Portal](https://portal.azure.com) és helyettesítő DNS beállításának megfelelően.</span><span class="sxs-lookup"><span data-stu-id="c61d4-125">However, it should be possible toosetup a subdomain install if you map a domain through hello [Azure Portal](https://portal.azure.com) and setup wildcard DNS properly.</span></span>

<span data-ttu-id="c61d4-126">További információ a altartomány vs alkönyvtár beállítások: hello [többhelyes hálózati típusú] [ wordpress-codex-types-of-networks] hello WordPress kódex foglalkozó.</span><span class="sxs-lookup"><span data-stu-id="c61d4-126">For more information on sub-domain vs sub-directory setups see hello [Types of multisite network][wordpress-codex-types-of-networks] article on hello WordPress Codex.</span></span>

## <a name="enable-hello-network"></a><span data-ttu-id="c61d4-127">Hello hálózati engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c61d4-127">Enable hello Network</span></span>
<span data-ttu-id="c61d4-128">hello hálózati most hello adatbázisban van konfigurálva, de egy további lépés tooenable hello hálózati funkciókat.</span><span class="sxs-lookup"><span data-stu-id="c61d4-128">hello network is now configured in hello database, but there is one remaining step tooenable hello network functionality.</span></span> <span data-ttu-id="c61d4-129">Hello véglegesítése `wp-config.php` beállításait és győződjön meg arról `web.config` megfelelően irányítja a helyekhez.</span><span class="sxs-lookup"><span data-stu-id="c61d4-129">Finalize hello `wp-config.php` settings and ensure `web.config` properly routes each site.</span></span>

<span data-ttu-id="c61d4-130">Hello kattintás után **telepítése** hello gombjára *hálózati beállítás* lapon WordPress megpróbál tooupdate hello `wp-config.php` és `web.config` fájlokat.</span><span class="sxs-lookup"><span data-stu-id="c61d4-130">After clicking hello **Install** button on hello *Network Setup* page, WordPress will attempt tooupdate hello `wp-config.php` and `web.config` files.</span></span> <span data-ttu-id="c61d4-131">Azonban mindig ellenőrizze hello fájlok tooensure hello frissítése sikerült-e.</span><span class="sxs-lookup"><span data-stu-id="c61d4-131">However, you should always check hello files tooensure hello updates were successful.</span></span> <span data-ttu-id="c61d4-132">Ha nem, ezen a képernyőn megjelennek hello szükséges frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="c61d4-132">If not, this screen will present you with hello necessary updates.</span></span> <span data-ttu-id="c61d4-133">Módosítsa és mentse a hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="c61d4-133">Edit and save hello files.</span></span>

<span data-ttu-id="c61d4-134">Elvégzése után ezeket a frissítéseket, szüksége lesz a kimenő toolog és a naplófájlok hello wp-rendszergazda irányítópult vissza.</span><span class="sxs-lookup"><span data-stu-id="c61d4-134">After making these updates you will need toolog out and log back into hello wp-admin dashboard.</span></span>

<span data-ttu-id="c61d4-135">Most kell egy további menü hello admin sávon feliratú **saját webhelyek**.</span><span class="sxs-lookup"><span data-stu-id="c61d4-135">There should now be an additional menu on hello admin bar labeled **My Sites**.</span></span> <span data-ttu-id="c61d4-136">Ebben a menüben toocontrol lehetővé teszi az új hálózati keresztül hello **hálózati rendszergazda** irányítópult.</span><span class="sxs-lookup"><span data-stu-id="c61d4-136">This menu allows you toocontrol your new network through hello **Network Admin** dashboard.</span></span>

## <a name="adding-custom-domains"></a><span data-ttu-id="c61d4-137">Egyéni tartományok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c61d4-137">Adding custom domains</span></span>
<span data-ttu-id="c61d4-138">Hello [WordPress MU tartomány leképezése] [ wordpress-plugin-wordpress-mu-domain-mapping] beépülő modul segítségével kezelheti tooadd egyéni tartományok tooany hely a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="c61d4-138">hello [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] plugin makes it a breeze tooadd custom domains tooany site in your network.</span></span> <span data-ttu-id="c61d4-139">Ahhoz, hogy hello beépülő modul toooperate megfelelően kell toodo néhány további beállítást, a portál hello, valamint a tartományregisztrálónál.</span><span class="sxs-lookup"><span data-stu-id="c61d4-139">In order for hello plugin toooperate properly, you need toodo some additional setup on hello Portal, and also at your domain registrar.</span></span>

## <a name="enable-domain-mapping-toohello-web-app"></a><span data-ttu-id="c61d4-140">Tartomány leképezése toohello webalkalmazás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c61d4-140">Enable domain mapping toohello web app</span></span>
<span data-ttu-id="c61d4-141">Hello **szabad** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) terv üzemmódja nem támogatja az egyéni tartományok tooWeb alkalmazások hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="c61d4-141">hello **Free** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plan mode does not support adding custom domains tooWeb Apps.</span></span> <span data-ttu-id="c61d4-142">Szüksége lesz tooswitch túl**megosztott** vagy **szabványos** mód.</span><span class="sxs-lookup"><span data-stu-id="c61d4-142">You will need tooswitch too**Shared** or **Standard** mode.</span></span> <span data-ttu-id="c61d4-143">toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="c61d4-143">toodo this:</span></span>

* <span data-ttu-id="c61d4-144">Jelentkezzen be Azure Portal toohello, és keresse meg a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c61d4-144">Log in toohello Azure Portal and locate your web app.</span></span> 
* <span data-ttu-id="c61d4-145">Kattintson a hello **vertikális felskálázás** lapján **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="c61d4-145">Click on hello **Scale up** tab in **Settings**.</span></span>
* <span data-ttu-id="c61d4-146">A **általános**, válassza *megosztott* vagy *STANDARD*</span><span class="sxs-lookup"><span data-stu-id="c61d4-146">Under **General**, select either *SHARED* or *STANDARD*</span></span>
* <span data-ttu-id="c61d4-147">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="c61d4-147">Click **Save**</span></span>

<span data-ttu-id="c61d4-148">Előfordulhat, hogy egy üzenet jelenik meg tooverify hello módosítása és a webalkalmazás most fel Önnek a költség, attól függően, használati és egyéb konfigurációkészletet hello tudomásul.</span><span class="sxs-lookup"><span data-stu-id="c61d4-148">You may receive a message asking you tooverify hello change and acknowledge your web app may now incur a cost, depending upon usage and hello other configuration you set.</span></span>

<span data-ttu-id="c61d4-149">Tooprocess hello új beállítások, néhány másodpercig tart most az egy időben toostart beállítása a tartományban van.</span><span class="sxs-lookup"><span data-stu-id="c61d4-149">It takes a few seconds tooprocess hello new settings, so now is a good time toostart setting up your domain.</span></span>

## <a name="verify-your-domain"></a><span data-ttu-id="c61d4-150">Ellenőrizze a tartományt</span><span class="sxs-lookup"><span data-stu-id="c61d4-150">Verify your domain</span></span>
<span data-ttu-id="c61d4-151">Azure Web Apps toomap egy tartományi toohello hely lehetővé teszi a, mielőtt először tooverify, hogy rendelkezik-e hello engedélyezési toomap hello tartomány.</span><span class="sxs-lookup"><span data-stu-id="c61d4-151">Before Azure Web Apps will allow you toomap a domain toohello site, you first need tooverify that you have hello authorization toomap hello domain.</span></span> <span data-ttu-id="c61d4-152">toodo Igen, hozzá kell adnia egy új CNAME rekord tooyour DNS-bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="c61d4-152">toodo so, you must add a new CNAME record tooyour DNS entry.</span></span>

* <span data-ttu-id="c61d4-153">Jelentkezzen be tooyour tartomány DNS-kezelő</span><span class="sxs-lookup"><span data-stu-id="c61d4-153">Log in tooyour domain's DNS manager</span></span>
* <span data-ttu-id="c61d4-154">Hozzon létre egy új CNAME *awverify*</span><span class="sxs-lookup"><span data-stu-id="c61d4-154">Create a new CNAME *awverify*</span></span>
* <span data-ttu-id="c61d4-155">Pont *awverify* túl*awverify. YOUR_DOMAIN.azurewebsites.NET*</span><span class="sxs-lookup"><span data-stu-id="c61d4-155">Point *awverify* too*awverify.YOUR_DOMAIN.azurewebsites.net*</span></span>

<span data-ttu-id="c61d4-156">Előfordulhat, hogy teljes körű érvénybe hello DNS módosítások toogo némi időt vesz igénybe, így hello lépések nem működnek, ha nyissa meg kávészünetet, ellenőrizze, majd térjen vissza, és próbálja meg újból.</span><span class="sxs-lookup"><span data-stu-id="c61d4-156">It may take some time for hello DNS changes toogo into full effect, so if hello following steps do not work immediately, go make a cup of coffee, then come back and try again.</span></span>

## <a name="add-hello-domain-toohello-web-app"></a><span data-ttu-id="c61d4-157">Hello tartomány toohello webalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c61d4-157">Add hello domain toohello web app</span></span>
<span data-ttu-id="c61d4-158">Kattintson a webalkalmazás visszatérési tooyour hello Azure-portálon keresztül **beállítások**, és kattintson a **egyéni tartományok és SSL**.</span><span class="sxs-lookup"><span data-stu-id="c61d4-158">Return tooyour web app through hello Azure Portal, click **Settings**, and then click **Custom domains and SSL**.</span></span>

<span data-ttu-id="c61d4-159">Ha hello *SSL-beállítások* vannak jelenik meg, láthatja hello mezők hol kívánja tooassign tooyour webalkalmazás hello tartományok bemeneteket.</span><span class="sxs-lookup"><span data-stu-id="c61d4-159">When hello *SSL settings* are displayed, you will see hello fields where you will input all hello domains which you wish tooassign tooyour web app.</span></span> <span data-ttu-id="c61d4-160">Ha egy tartomány nem szerepel, azt nem elérhetők leképezés belül WordPress, függetlenül attól, milyen hello tartományi DNS beállítása.</span><span class="sxs-lookup"><span data-stu-id="c61d4-160">If a domain is not listed here, it will not be available for mapping inside WordPress, regardless of how hello domain DNS is setup.</span></span>

![Egyéni tartományok párbeszédpanel kezelése][wordpress-manage-domains]

<span data-ttu-id="c61d4-162">Írja be a tartomány hello szövegmezőbe, után Azure ellenőrzi, hogy hello korábban létrehozott CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="c61d4-162">After typing your domain into hello text box, Azure will verify hello CNAME record you created previously.</span></span> <span data-ttu-id="c61d4-163">Ha hello DNS propagálása nem teljes, egy piros jelző jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c61d4-163">If hello DNS has not fully propagated, a red indicator will show.</span></span> <span data-ttu-id="c61d4-164">Ha sikeres volt, egy zöld pipa jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c61d4-164">If it was successful, you will see a green checkmark.</span></span> 

<span data-ttu-id="c61d4-165">Jegyezze fel az IP-cím szerepel a listában hello párbeszédpanel hello alján hello.</span><span class="sxs-lookup"><span data-stu-id="c61d4-165">Take note of hello IP Address listed at hello bottom of hello dialog.</span></span> <span data-ttu-id="c61d4-166">A tartomány szüksége lesz a toosetup hello rekord.</span><span class="sxs-lookup"><span data-stu-id="c61d4-166">You will need this toosetup hello A record for your domain.</span></span>

## <a name="setup-hello-domain-a-record"></a><span data-ttu-id="c61d4-167">A telepítő hello tartomány A rekord</span><span class="sxs-lookup"><span data-stu-id="c61d4-167">Setup hello domain A record</span></span>
<span data-ttu-id="c61d4-168">Hello más lépéseket volt sikeres, ha Ön most jogosult hello tartomány tooyour Azure web apphoz keresztül DNS A rekord.</span><span class="sxs-lookup"><span data-stu-id="c61d4-168">If hello other steps were successful, you may now assign hello domain tooyour Azure web app through a DNS A record.</span></span> 

<span data-ttu-id="c61d4-169">Fontos fontos toonote itt, a Azure-webalkalmazásokban azonban CNAME és az A rekordok elfogadja, *kell* egy A rekord tooenable megfelelő tartomány leképezése használja.</span><span class="sxs-lookup"><span data-stu-id="c61d4-169">It is important toonote here that Azure web apps accept both CNAME and A records, however you *must* use an A record tooenable proper domain mapping.</span></span> <span data-ttu-id="c61d4-170">Egy olyan CNAME REKORDOT nem továbbítható tooanother CNAME, vagyis mi Azure létre az Ön YOUR_DOMAIN.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="c61d4-170">A CNAME cannot be forwarded tooanother CNAME, which is what Azure created for you with YOUR_DOMAIN.azurewebsites.net.</span></span>

<span data-ttu-id="c61d4-171">Az előző lépésben hello hello IP-címet használja, térjen vissza a tooyour DNS-kezelő és a telepítő hello egy rekord toopoint toothat IP-cím.</span><span class="sxs-lookup"><span data-stu-id="c61d4-171">Using hello IP address from hello previous step, return tooyour DNS manager and setup hello A record toopoint toothat IP.</span></span>

## <a name="install-and-setup-hello-plugin"></a><span data-ttu-id="c61d4-172">Telepítéséhez és beállításához hello beépülő modul</span><span class="sxs-lookup"><span data-stu-id="c61d4-172">Install and setup hello plugin</span></span>
<span data-ttu-id="c61d4-173">WordPress többhelyes környezet jelenleg nem rendelkezik olyan beépített módszerrel toomap egyéni tartományok.</span><span class="sxs-lookup"><span data-stu-id="c61d4-173">WordPress Multisite currently does not have a built-in method toomap custom domains.</span></span> <span data-ttu-id="c61d4-174">Van azonban egy beépülő modul nevű [WordPress MU tartomány leképezése] [ wordpress-plugin-wordpress-mu-domain-mapping] hello funkció, amely ad meg.</span><span class="sxs-lookup"><span data-stu-id="c61d4-174">However, there is a plugin called [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] that adds hello functionality for you.</span></span> <span data-ttu-id="c61d4-175">Jelentkezzen be a webhely toohello hálózati rendszergazda része, és telepítse a hello **WordPress MU tartomány leképezése** beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="c61d4-175">Log in toohello Network Admin portion of your site and install hello **WordPress MU Domain Mapping** plugin.</span></span>

<span data-ttu-id="c61d4-176">Miután telepítése és aktiválása hello beépülő modul, látogasson el a **beállítások** > **tartomány leképezése** tooconfigure hello beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="c61d4-176">After installing and activating hello plugin, visit **Settings** > **Domain Mapping** tooconfigure hello plugin.</span></span> <span data-ttu-id="c61d4-177">Hello első szövegmezőben *kiszolgáló IP-címe*, bemeneti hello toosetup használt IP-cím hello hello tartomány egy olyan rekordot.</span><span class="sxs-lookup"><span data-stu-id="c61d4-177">In hello first textbox, *Server IP Address*, input hello IP Address you used toosetup hello A record for hello domain.</span></span> <span data-ttu-id="c61d4-178">Adja meg *beállítások* felügyelni (hello alapértelmezett gyakran rendben), majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c61d4-178">Set any *Domain Options* you desire (hello defaults are often fine) and click **Save**.</span></span>

## <a name="map-hello-domain"></a><span data-ttu-id="c61d4-179">Hello tartományban</span><span class="sxs-lookup"><span data-stu-id="c61d4-179">Map hello domain</span></span>
<span data-ttu-id="c61d4-180">A Microsoft hello **irányítópult** hello hely toomap hello tartomány kívánja.</span><span class="sxs-lookup"><span data-stu-id="c61d4-180">Visit hello **Dashboard** for hello site you wish toomap hello domain to.</span></span> <span data-ttu-id="c61d4-181">Kattintson a **eszközök** > **tartomány leképezése** és típus hello új tartomány hello szövegmezőben, majd kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c61d4-181">Click on **Tools** > **Domain Mapping** and type hello new domain into hello textbox and click **Add**.</span></span>

<span data-ttu-id="c61d4-182">Alapértelmezés szerint a hello új tartományt egy átírt toohello automatikusan létrehozott hely tartomány lesz.</span><span class="sxs-lookup"><span data-stu-id="c61d4-182">By default, hello new domain will be rewritten toohello autogenerated site domain.</span></span> <span data-ttu-id="c61d4-183">Ha szeretné toohave új tartomány összes elküldött forgalom toohello, ellenőrizze a hello *ebben a blogban elsődleges tartományt* mezőben mentés előtt.</span><span class="sxs-lookup"><span data-stu-id="c61d4-183">If you want toohave all traffic sent toohello new domain, check hello *Primary domain for this blog* box before saving.</span></span> <span data-ttu-id="c61d4-184">Tartományok tooa hely korlátlan számú adhat hozzá, de csak egy elsődleges lehet.</span><span class="sxs-lookup"><span data-stu-id="c61d4-184">You can add an unlimited number of domains tooa site, but  only one can be primary.</span></span>

## <a name="do-it-again"></a><span data-ttu-id="c61d4-185">Hajtsa végre újra</span><span class="sxs-lookup"><span data-stu-id="c61d4-185">Do it again</span></span>
<span data-ttu-id="c61d4-186">Az Azure Web Apps tooadd tartományok tooa webalkalmazás korlátlan számú engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c61d4-186">Azure Web Apps allow you tooadd an unlimited number of domains tooa web app.</span></span> <span data-ttu-id="c61d4-187">tooadd tooexecute hello szüksége lesz egy másik tartomány **ellenőrizze a tartományt** és **hello tartomány rekord telepítő** részekben az egyes tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="c61d4-187">tooadd another domain you will need tooexecute hello **Verify your domain** and **Setup hello domain A record** sections for each domain.</span></span>    

> [!NOTE]
> <span data-ttu-id="c61d4-188">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="c61d4-188">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c61d4-189">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="c61d4-189">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="c61d4-190">A változások</span><span class="sxs-lookup"><span data-stu-id="c61d4-190">What's changed</span></span>
* <span data-ttu-id="c61d4-191">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="c61d4-191">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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


