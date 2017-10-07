---
title: "aaaCreate egy WordPress webalkalmazást az Azure App Service szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy új Azure web app egy WordPress bloghoz hello Azure portál használatával."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 3a95fcb6732c15a8200921ce474b6dde2298dec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a><span data-ttu-id="2bc53-103">WordPress-webalkalmazás létrehozása az Azure App Service szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="2bc53-103">Create a WordPress web app in Azure App Service</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="2bc53-104">Ez az oktatóanyag bemutatja, hogyan toodeploy egy WordPress-bloghoz az Azure piactér hello hely.</span><span class="sxs-lookup"><span data-stu-id="2bc53-104">This tutorial shows how toodeploy a WordPress blog site from hello Azure Marketplace.</span></span>

<span data-ttu-id="2bc53-105">Amikor elkészült, a hello oktatóanyag összekapcsolta a saját WordPress-blogja és hello felhőben futó.</span><span class="sxs-lookup"><span data-stu-id="2bc53-105">When you're done with hello tutorial you'll have your own WordPress blog site up and running in hello cloud.</span></span>

![WordPress-webhely](./media/web-sites-php-web-site-gallery/wpdashboard.png)

<span data-ttu-id="2bc53-107">Az oktatóanyagból a következőket sajátíthatja el:</span><span class="sxs-lookup"><span data-stu-id="2bc53-107">You'll learn:</span></span>

* <span data-ttu-id="2bc53-108">Hogyan toofind egy JSON-sablon a hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="2bc53-108">How toofind an application template in hello Azure Marketplace.</span></span>
* <span data-ttu-id="2bc53-109">Hogyan toocreate egy webalkalmazást az Azure App Service, amely hello sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="2bc53-109">How toocreate a web app in Azure App Service that is based on hello template.</span></span>
* <span data-ttu-id="2bc53-110">Hogyan új hello tooconfigure Azure App Service szolgáltatás beállításainak web app és az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2bc53-110">How tooconfigure Azure App Service settings for hello new web app and database.</span></span>

<span data-ttu-id="2bc53-111">hello Azure piactér elérhetővé teszi a Microsoft, külső vállalatok és nyílt forráskódú szoftver-kezdeményezések által fejlesztett népszerű webalkalmazások széles skáláját.</span><span class="sxs-lookup"><span data-stu-id="2bc53-111">hello Azure Marketplace makes available a wide range of popular web apps developed by Microsoft, third party companies, and open source software initiatives.</span></span> <span data-ttu-id="2bc53-112">hello webalkalmazások épülnek a legkülönbözőbb népszerű keretrendszerekre, például a [PHP](/develop/nodejs/) ebben a WordPress példában, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), és [Python](/develop/python/), kevés a tooname.</span><span class="sxs-lookup"><span data-stu-id="2bc53-112">hello web apps are built on a wide range of popular frameworks, such as [PHP](/develop/nodejs/) in this WordPress example, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), and [Python](/develop/python/), tooname a few.</span></span> <span data-ttu-id="2bc53-113">egy webalkalmazást az Azure piactéren csak a szükséges szoftver a hello használt böngésző hello hello hello toocreate [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2bc53-113">toocreate a web app from hello Azure Marketplace hello only software you need is hello browser that you use for hello [Azure Portal](https://portal.azure.com/).</span></span> 

<span data-ttu-id="2bc53-114">Ebben az oktatóanyagban üzembe helyezett WordPress webhely hello MySQL adatbázis hello használ.</span><span class="sxs-lookup"><span data-stu-id="2bc53-114">hello WordPress site that you deploy in this tutorial uses MySQL for hello database.</span></span> <span data-ttu-id="2bc53-115">Ha a hello adatbázis SQL adatbázis tooinstead használata, lásd: [Project Nami](http://projectnami.org/).</span><span class="sxs-lookup"><span data-stu-id="2bc53-115">If you wish tooinstead use SQL Database for hello database, see [Project Nami](http://projectnami.org/).</span></span> <span data-ttu-id="2bc53-116">**A Project Nami** hello piactéren keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2bc53-116">**Project Nami** is also available through hello Marketplace.</span></span>

> [!NOTE]
> <span data-ttu-id="2bc53-117">toocomplete ebben az oktatóanyagban a Microsoft Azure-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="2bc53-117">toocomplete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="2bc53-118">Ha nincs fiókja, [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F), vagy [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="2bc53-118">If you don't have an account, you can [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="2bc53-119">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépések tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/).</span><span class="sxs-lookup"><span data-stu-id="2bc53-119">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/).</span></span> <span data-ttu-id="2bc53-120">Itt azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="2bc53-120">There, you can immediately create a short-lived starter web app in App Service—no credit card required, and no commitments.</span></span>
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a><span data-ttu-id="2bc53-121">Válassza ki a WordPresst, és konfigurálja azt az Azure App Service-hez</span><span class="sxs-lookup"><span data-stu-id="2bc53-121">Select WordPress and configure for Azure App Service</span></span>
1. <span data-ttu-id="2bc53-122">Jelentkezzen be toohello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2bc53-122">Log in toohello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="2bc53-123">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2bc53-123">Click **New**.</span></span>
   
    ![Új létrehozása][5]
3. <span data-ttu-id="2bc53-125">Keresse meg a **WordPress**-alkalmazást, majd kattintson a **WordPress** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2bc53-125">Search for **WordPress**, and then click **WordPress**.</span></span> <span data-ttu-id="2bc53-126">Ha a MySQL helyett az SQL-adatbázis toouse, keressen **Project Nami**.</span><span class="sxs-lookup"><span data-stu-id="2bc53-126">If you wish toouse SQL Database instead of MySQL, search for **Project Nami**.</span></span>
   
    ![A listában szereplő WordPress][7]
4. <span data-ttu-id="2bc53-128">Hello hello WordPress alkalmazás leírásának elolvasása, után kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2bc53-128">After reading hello description of hello WordPress app, click **Create**.</span></span>
   
    ![Létrehozás](./media/web-sites-php-web-site-gallery/create.png)
5. <span data-ttu-id="2bc53-130">Adja meg egy nevet a webalkalmazás hello hello **webalkalmazás** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2bc53-130">Enter a name for hello web app in hello **Web app** box.</span></span>
   
    <span data-ttu-id="2bc53-131">A névnek kell lennie egyedi hello azurewebsites.net tartományban, mert hello hello webalkalmazás URL-címe {név} lesznek. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="2bc53-131">This name must be unique in hello azurewebsites.net domain because hello URL of hello web app will be {name}.azurewebsites.net.</span></span> <span data-ttu-id="2bc53-132">Ha hello név nem egyedi, hello szövegmezőben egy piros felkiáltójel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2bc53-132">If hello name you enter isn't unique, a red exclamation mark appears in hello text box.</span></span>
6. <span data-ttu-id="2bc53-133">Ha egynél több előfizetéssel, válassza ki a hello rendelkezik egy használni kívánt toouse.</span><span class="sxs-lookup"><span data-stu-id="2bc53-133">If you have more than one subscription, choose hello one you want toouse.</span></span> 
7. <span data-ttu-id="2bc53-134">Válasszon egy **erőforráscsoportot**, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="2bc53-134">Select a **Resource Group** or create a new one.</span></span>
   
    <span data-ttu-id="2bc53-135">További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2bc53-135">For more information about resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="2bc53-136">Válasszon ki egy **App Service-csomagot/-helyet**, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="2bc53-136">Select an **App Service plan/Location** or create a new one.</span></span>
   
    <span data-ttu-id="2bc53-137">További információk az App Service-csomagokról: [Az Azure App Service-csomagok áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2bc53-137">For more information about App Service plans, see [Azure App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</span></span>    
9. <span data-ttu-id="2bc53-138">Kattintson a **adatbázis**, majd a hello **új MySQL-adatbázis** panelen adja meg a szükséges hello értékeket, a MySQL adatbázis konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="2bc53-138">Click **Database**, and then in hello **New MySQL Database** blade provide hello required values for configuring your MySQL database.</span></span>
   
    <span data-ttu-id="2bc53-139">a.</span><span class="sxs-lookup"><span data-stu-id="2bc53-139">a.</span></span> <span data-ttu-id="2bc53-140">Adjon meg egy új nevet, vagy hello alapértelmezett neve mező maradjon.</span><span class="sxs-lookup"><span data-stu-id="2bc53-140">Enter a new name or leave hello default name.</span></span>
   
    <span data-ttu-id="2bc53-141">b.</span><span class="sxs-lookup"><span data-stu-id="2bc53-141">b.</span></span> <span data-ttu-id="2bc53-142">Hagyja hello **adatbázistípus** túl beállítása**megosztott**.</span><span class="sxs-lookup"><span data-stu-id="2bc53-142">Leave hello **Database Type** set too**Shared**.</span></span>
   
    <span data-ttu-id="2bc53-143">c.</span><span class="sxs-lookup"><span data-stu-id="2bc53-143">c.</span></span> <span data-ttu-id="2bc53-144">Válassza ki a hello ugyanarra a helyre, amelyiket Ön hello választott hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2bc53-144">Choose hello same location as hello one you chose for hello web app.</span></span>
   
    <span data-ttu-id="2bc53-145">d.</span><span class="sxs-lookup"><span data-stu-id="2bc53-145">d.</span></span> <span data-ttu-id="2bc53-146">Válasszon egy tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="2bc53-146">Choose a pricing tier.</span></span> <span data-ttu-id="2bc53-147">A Merkúr (ingyenes tarifacsomag, minimális megengedett kapcsolódással és lemezterülettel) ideális ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="2bc53-147">Mercury (free with minimal allowed connections and disk space) is fine for this tutorial.</span></span>
10. <span data-ttu-id="2bc53-148">A hello **új MySQL-adatbázis** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bc53-148">In hello **New MySQL Database** blade, click **OK**.</span></span> 
11. <span data-ttu-id="2bc53-149">A hello **WordPress** panelen fogadja el a hello jogi feltételeket, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2bc53-149">In hello **WordPress** blade, accept hello legal terms, and then click **Create**.</span></span> 
    
     ![A webalkalmazás konfigurálása](./media/web-sites-php-web-site-gallery/configure.png)
    
     <span data-ttu-id="2bc53-151">Az Azure App Service általában kevesebb, mint egy percen belül hoz létre hello web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2bc53-151">Azure App Service creates hello web app, typically in less than a minute.</span></span> <span data-ttu-id="2bc53-152">Hello portállapon hello tetején hello harang ikonra kattintva figyelemmel követheti hello folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="2bc53-152">You can watch hello progress by clicking hello bell icon at hello top of hello portal page.</span></span>
    
     ![Folyamatjelző](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="2bc53-154">A WordPress-webalkalmazás elindítása és kezelése</span><span class="sxs-lookup"><span data-stu-id="2bc53-154">Launch and manage your WordPress web app</span></span>
1. <span data-ttu-id="2bc53-155">Ha hello webalkalmazás létrehozása befejeződött, nyissa meg a hello Azure Portal toohello erőforráscsoportban, amelyben létrehozta hello alkalmazás, és megtekintheti hello web app és hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2bc53-155">When hello web app creation is finished, navigate in hello Azure Portal toohello resource group in which you created hello application, and you can see hello web app and hello database.</span></span>
   
    <span data-ttu-id="2bc53-156">hello hello villanykörte ikonnal rendelkező további erőforrás van [Application Insights](/services/application-insights/), amely a webalkalmazás figyelési szolgáltatásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="2bc53-156">hello extra resource with hello light bulb icon is [Application Insights](/services/application-insights/), which provides monitoring services for your web app.</span></span>
2. <span data-ttu-id="2bc53-157">A hello **erőforráscsoport** paneljén kattintson hello webalkalmazás sorra.</span><span class="sxs-lookup"><span data-stu-id="2bc53-157">In hello **Resource group** blade, click hello web app line.</span></span>
   
    ![A webalkalmazás konfigurálása](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. <span data-ttu-id="2bc53-159">Hello webalkalmazás panelen kattintson **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="2bc53-159">In hello Web app blade, click **Browse**.</span></span>
   
    ![webhely URL-címe][browse]
4. <span data-ttu-id="2bc53-161">A hello WordPress **üdvözlő** lapon adja meg a WordPress által kért hello konfigurációs információkat, és kattintson a **WordPress telepítése**.</span><span class="sxs-lookup"><span data-stu-id="2bc53-161">In hello WordPress **Welcome** page, enter hello configuration information required by WordPress, and then click **Install WordPress**.</span></span>
   
    ![A WordPress konfigurálása](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. <span data-ttu-id="2bc53-163">Jelentkezzen be a hello létrehozott hello hitelesítő adatok használatával **üdvözlő** lap.</span><span class="sxs-lookup"><span data-stu-id="2bc53-163">Log in using hello credentials you created on hello **Welcome** page.</span></span>  
6. <span data-ttu-id="2bc53-164">Megnyílik a webhelye irányítópultja.</span><span class="sxs-lookup"><span data-stu-id="2bc53-164">Your site Dashboard page opens.</span></span>    
   
    ![WordPress-webhely](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a><span data-ttu-id="2bc53-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2bc53-166">Next steps</span></span>
<span data-ttu-id="2bc53-167">Megtudhatta, hogyan toocreate és üzembe egy PHP webalkalmazást hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="2bc53-167">You've seen how toocreate and deploy a PHP web app from hello gallery.</span></span> <span data-ttu-id="2bc53-168">Az Azure-ban a PHP használatával kapcsolatos további információkért lásd: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="2bc53-168">For more information about using PHP in Azure, see hello [PHP Developer Center](/develop/php/).</span></span>

<span data-ttu-id="2bc53-169">További információ toowork az App Service Web Apps, tekintse meg a hello hivatkozásokat, a bal oldalán (széles böngészőablakok esetén) a hello található hello vagy hello tetején (keskeny böngészőablakok esetén) hello lapját.</span><span class="sxs-lookup"><span data-stu-id="2bc53-169">For more information about how toowork with App Service Web Apps, see hello links on hello left side of hello page (for wide browser windows) or at hello top of hello page (for narrow browser windows).</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="2bc53-170">A változások</span><span class="sxs-lookup"><span data-stu-id="2bc53-170">What's changed</span></span>
* <span data-ttu-id="2bc53-171">A webhelyek tooApp szolgáltatás útmutató toohello módosítást, lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="2bc53-171">For a guide toohello change from Websites tooApp Service, see [Azure App Service and its impact on existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png
