---
title: "aaaCreate üzleti az Azure alkalmazás Azure Active Directory-hitelesítéssel |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy ASP.NET MVC-üzleti alkalmazások az Azure App Service-ben, amely hitelesíti az Azure Active Directoryval"
services: app-service\web, active-directory
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: ad947bdb-4463-43ff-a5e3-91d9b2169b60
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 09/01/2016
ms.author: cephalin
ms.openlocfilehash: 3bcafad78ac0151889b3e336784cc561009f244f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a><span data-ttu-id="826d6-103">Üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="826d6-103">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>
<span data-ttu-id="826d6-104">Ez a cikk bemutatja, hogyan toocreate egy .NET-üzleti alkalmazás [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) hello segítségével [hitelesítési / engedélyezési](../app-service/app-service-authentication-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="826d6-104">This article shows you how toocreate a .NET line-of-business app in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) using hello [Authentication / Authorization](../app-service/app-service-authentication-overview.md) feature.</span></span> <span data-ttu-id="826d6-105">Azt is bemutatja, hogyan toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery címtáradatok hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="826d6-105">It also shows how toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery directory data in hello application.</span></span>

<span data-ttu-id="826d6-106">hello Azure Active Directory-bérlőt, amelyekkel egy csak Azure-címtár is lehet.</span><span class="sxs-lookup"><span data-stu-id="826d6-106">hello Azure Active Directory tenant that you use can be an Azure-only directory.</span></span> <span data-ttu-id="826d6-107">Vagy lehet [a helyszíni Active Directoryból szinkronizált](../active-directory/active-directory-aadconnect.md) toocreate egy egyszeri bejelentkezéses felhasználói élmény biztosítása, amely a helyi és távoli dolgozók.</span><span class="sxs-lookup"><span data-stu-id="826d6-107">Or, it can be [synced with your on-premises Active Directory](../active-directory/active-directory-aadconnect.md) toocreate a single sign-on experience for workers that are on-premises and remote.</span></span> <span data-ttu-id="826d6-108">Ebben a cikkben hello alapértelmezett címtár az Azure-fiókját.</span><span class="sxs-lookup"><span data-stu-id="826d6-108">This article uses hello default directory for your Azure account.</span></span>

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="826d6-109">Mit fog létrehozni</span><span class="sxs-lookup"><span data-stu-id="826d6-109">What you will build</span></span>
<span data-ttu-id="826d6-110">Fog létrehozni egy egyszerű üzleti Create-olvasás-Módosítás-Törlés (CRUD) alkalmazást az App Service Web Apps, hogy nyomon követi a következő funkciók hello elemek használható:</span><span class="sxs-lookup"><span data-stu-id="826d6-110">You will build a simple line-of-business Create-Read-Update-Delete (CRUD) application in App Service Web Apps that tracks work items with hello following features:</span></span>

* <span data-ttu-id="826d6-111">Hitelesíti a felhasználókat az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="826d6-111">Authenticates users against Azure Active Directory</span></span>
* <span data-ttu-id="826d6-112">Lekérdezi a directory-felhasználók és csoportok használatával [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)</span><span class="sxs-lookup"><span data-stu-id="826d6-112">Queries directory users and groups using [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)</span></span>
* <span data-ttu-id="826d6-113">Használja az ASP.NET MVC hello *nem hitelesítési* sablon</span><span class="sxs-lookup"><span data-stu-id="826d6-113">Use hello ASP.NET MVC *No Authentication* template</span></span>

<span data-ttu-id="826d6-114">Ha az üzleti alkalmazás az Azure szerepköralapú hozzáférés-vezérlést (RBAC) van szüksége, tekintse meg [tovább](#next).</span><span class="sxs-lookup"><span data-stu-id="826d6-114">If you need role-based access control (RBAC) for your line-of-business app in Azure, see [Next Step](#next).</span></span>

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="826d6-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="826d6-115">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="826d6-116">Meg kell hello toocomplete követően ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="826d6-116">You need hello following toocomplete this tutorial:</span></span>

* <span data-ttu-id="826d6-117">Az Azure Active Directory-bérlő különböző csoportok felhasználóival</span><span class="sxs-lookup"><span data-stu-id="826d6-117">An Azure Active Directory tenant with users in various groups</span></span>
* <span data-ttu-id="826d6-118">Engedélyek toocreate alkalmazások hello Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="826d6-118">Permissions toocreate applications on hello Azure Active Directory tenant</span></span>
* <span data-ttu-id="826d6-119">A Visual Studio 2013 Update 4 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="826d6-119">Visual Studio 2013 Update 4 or later</span></span>
* [<span data-ttu-id="826d6-120">Az Azure SDK 2.8.1-es verziójának vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="826d6-120">Azure SDK 2.8.1 or later</span></span>](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-tooazure"></a><span data-ttu-id="826d6-121">Létrehozhat és telepíthet egy webes alkalmazás tooAzure</span><span class="sxs-lookup"><span data-stu-id="826d6-121">Create and deploy a web app tooAzure</span></span>
1. <span data-ttu-id="826d6-122">A Visual Studio eszközből a kattintson **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="826d6-122">From Visual Studio, click **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="826d6-123">Válassza ki **ASP.NET Web Application**, nevezze el a projektet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="826d6-123">Select **ASP.NET Web Application**, name your project, and click **OK**.</span></span>
3. <span data-ttu-id="826d6-124">SELECT hello **MVC** sablont, majd módosítsa túl hello hitelesítési**nem hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="826d6-124">Select hello **MVC** template, then change hello authentication too**No Authentication**.</span></span> <span data-ttu-id="826d6-125">Győződjön meg arról, hogy **gazdagépet a felhő hello** van kiválasztva, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="826d6-125">Make sure **Host in hello Cloud** is selected and click **OK**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. <span data-ttu-id="826d6-126">A hello **App Service létrehozása** párbeszédpanel, kattintson a **vegyen fel egy fiókot** (, majd **vegyen fel egy fiókot** hello a legördülő) az Azure-fiók tooyour toolog.</span><span class="sxs-lookup"><span data-stu-id="826d6-126">In hello **Create App Service** dialog, click **Add an account** (and then **Add an account** in hello dropdown) toolog in tooyour Azure account.</span></span>
5. <span data-ttu-id="826d6-127">Miután bejelentkezett a webalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="826d6-127">Once logged in configure your web app.</span></span> <span data-ttu-id="826d6-128">Hozzon létre egy erőforráscsoportot és egy új App Service-csomag megfelelő hello kattintva **új** gombra.</span><span class="sxs-lookup"><span data-stu-id="826d6-128">Create a resource group and a new App Service plan by clicking hello respective **New** button.</span></span> <span data-ttu-id="826d6-129">Kattintson a **további Azure-szolgáltatások felfedezés** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="826d6-129">Click **Explore additional Azure services** toocontinue.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. <span data-ttu-id="826d6-130">A hello **szolgáltatások** lapra, majd  **+**  tooadd az alkalmazás SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="826d6-130">In hello **Services** tab, click **+** tooadd a SQL Database for your app.</span></span> 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. <span data-ttu-id="826d6-131">A **SQL-adatbázis beállítása**, kattintson a **új** toocreate egy SQL Server-példányt.</span><span class="sxs-lookup"><span data-stu-id="826d6-131">In **Configure SQL Database**, click **New** toocreate a SQL Server instance.</span></span>
8. <span data-ttu-id="826d6-132">A **SQL-kiszolgáló konfigurálása**, konfigurálja az SQL Server-példányt.</span><span class="sxs-lookup"><span data-stu-id="826d6-132">In **Configure SQL Server**, configure your SQL Server instance.</span></span> <span data-ttu-id="826d6-133">Kattintson a **OK**, **OK**, és **létrehozása** tookick ki hello létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="826d6-133">Then, click **OK**, **OK**, and **Create** tookick off hello app creation in Azure.</span></span>
9. <span data-ttu-id="826d6-134">A **Azure App Service-tevékenység**, láthatja, mikor hello létrehozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="826d6-134">In **Azure App Service Activity**, you can see when hello app creation is finished.</span></span> <span data-ttu-id="826d6-135">Kattintson a  **közzététel &lt;* appname*> toothis webalkalmazás most **, majd kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="826d6-135">Click **Publish &lt;*appname*> toothis Web App now**, then click **Publish**.</span></span> 
   
    <span data-ttu-id="826d6-136">Miután befejezte a Visual Studio, hello nyílik alkalmazás közzététele hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="826d6-136">Once Visual Studio finishes, it opens hello publish app in hello browser.</span></span> 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a><span data-ttu-id="826d6-137">Hitelesítés és a directory hozzáférés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="826d6-137">Configure authentication and directory access</span></span>
1. <span data-ttu-id="826d6-138">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="826d6-138">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="826d6-139">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások** > **&lt;*appname*> ** > **hitelesítési / engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="826d6-139">From hello left menu, click **App Services** > **&lt;*appname*>** > **Authentication / Authorization**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. <span data-ttu-id="826d6-140">Azure Active Directory-hitelesítés bekapcsolása kattintva **a** > **Azure Active Directory** > **Express** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="826d6-140">Turn on Azure Active Directory authentication by clicking **On** > **Azure Active Directory** > **Express** > **OK**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. <span data-ttu-id="826d6-141">Kattintson a **mentése** hello parancs sávon.</span><span class="sxs-lookup"><span data-stu-id="826d6-141">Click **Save** in hello command bar.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    <span data-ttu-id="826d6-142">Miután hello hitelesítési beállításainak mentése sikeresen befejeződött, próbálja a Navigálás tooyour app újra hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="826d6-142">Once hello authentication settings are saved successfully, try navigating tooyour app again in hello browser.</span></span> <span data-ttu-id="826d6-143">Az alapértelmezett beállítások a teljes alkalmazás hello hitelesítés kikényszerítéséhez.</span><span class="sxs-lookup"><span data-stu-id="826d6-143">Your default settings enforce authentication on hello whole app.</span></span> <span data-ttu-id="826d6-144">Ha még nem jelentkezett, átirányított tooa bejelentkezési képernyő áll.</span><span class="sxs-lookup"><span data-stu-id="826d6-144">If you aren't already logged in, you are redirected tooa login screen.</span></span> <span data-ttu-id="826d6-145">Miután bejelentkezett, megjelenik az HTTPS által védett alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="826d6-145">Once logged in, you see your app secured by HTTPS.</span></span> <span data-ttu-id="826d6-146">A következő lépésben tooenable access toodirectory adatokat.</span><span class="sxs-lookup"><span data-stu-id="826d6-146">Next, you need tooenable access toodirectory data.</span></span> 
5. <span data-ttu-id="826d6-147">Keresse meg a toohello [klasszikus portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="826d6-147">Navigate toohello [classic portal](https://manage.windowsazure.com).</span></span>
6. <span data-ttu-id="826d6-148">Hello bal oldali menüben kattintson **Active Directory** > **alapértelmezett címtárat** > **alkalmazások**  >   **&lt;* appname*> **.</span><span class="sxs-lookup"><span data-stu-id="826d6-148">From hello left menu, click **Active Directory** > **Default Directory** > **Applications** > **&lt;*appname*>**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    <span data-ttu-id="826d6-149">Ez az App Service létrehozott meg tooenable hello engedélyezési hello Azure Active Directory-alkalmazás / hitelesítési funkció.</span><span class="sxs-lookup"><span data-stu-id="826d6-149">This is hello Azure Active Directory application that App Service created for you tooenable hello Authorization / Authentication feature.</span></span>
7. <span data-ttu-id="826d6-150">Kattintson a **felhasználók** és **csoportok** toomake, hogy rendelkezik-egyes felhasználók és csoportok hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="826d6-150">Click **Users** and **Groups** toomake sure that you have some users and groups in hello directory.</span></span> <span data-ttu-id="826d6-151">Ha nem, néhány teszt felhasználók és csoportok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="826d6-151">If not, create a few test users and groups.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. <span data-ttu-id="826d6-152">Kattintson a **konfigurálása** tooconfigure ezt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="826d6-152">Click **Configure** tooconfigure this application.</span></span>
9. <span data-ttu-id="826d6-153">Görgessen lefelé toohello **kulcsok** szakaszt, és a kulcs hozzáadása egy duration kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="826d6-153">Scroll down toohello **Keys** section and add a key by selecting a duration.</span></span> <span data-ttu-id="826d6-154">Kattintson a **delegált engedélyek** válassza **címtáradatok olvasása**.</span><span class="sxs-lookup"><span data-stu-id="826d6-154">Then, click **Delegated Permissions** and select **Read directory data**.</span></span> 
   <span data-ttu-id="826d6-155">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="826d6-155">Click **Save**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. <span data-ttu-id="826d6-156">Miután a beállítások lesznek mentve, görgessen toohello készítsen biztonsági másolatot **kulcsok** szakaszt, és kattintson a hello **másolása** gomb toocopy hello ügyfélkulcsot.</span><span class="sxs-lookup"><span data-stu-id="826d6-156">Once your settings are saved, scroll back up toohello **Keys** section and click hello **Copy** button toocopy hello client key.</span></span> 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="826d6-157">Ha most elhagyni a lapot, akkor ez az ügyfél kulcs soha újra tudja tooaccess.</span><span class="sxs-lookup"><span data-stu-id="826d6-157">If you navigate away from this page now, you won't be able tooaccess this client key ever again.</span></span>
    > 
    > 
11. <span data-ttu-id="826d6-158">A következő lépésben tooconfigure a webalkalmazás a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="826d6-158">Next, you need tooconfigure your web app with this key.</span></span> <span data-ttu-id="826d6-159">Jelentkezzen be toohello [Azure erőforrás-kezelő](https://resources.azure.com) az Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="826d6-159">Log in toohello [Azure Resource Explorer](https://resources.azure.com) with your Azure account.</span></span>
12. <span data-ttu-id="826d6-160">Hello hello oldal tetején, kattintson a **olvasási/írási** hello Azure erőforrás-kezelő toomake változásairól.</span><span class="sxs-lookup"><span data-stu-id="826d6-160">At hello top of hello page, click **Read/Write** toomake changes in hello Azure Resource Explorer.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. <span data-ttu-id="826d6-161">Hello hitelesítési beállításai az alkalmazások, előfizetések helyen található >  **&lt;* subscriptionname*> ** > **resourceGroups**  >   **&lt;* resourcegroupname*> ** > **szolgáltatók** > **Microsoft.Web**  >  **helyek** > **&lt;*appname*> ** > **config**  >  **authsettings**.</span><span class="sxs-lookup"><span data-stu-id="826d6-161">Find hello authentication settings for your app, located at subscriptions > **&lt;*subscriptionname*>** > **resourceGroups** > **&lt;*resourcegroupname*>** > **providers** > **Microsoft.Web** > **sites** > **&lt;*appname*>** > **config** > **authsettings**.</span></span>
14. <span data-ttu-id="826d6-162">Kattintson a **Szerkesztés** gombra.</span><span class="sxs-lookup"><span data-stu-id="826d6-162">Click **Edit**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. <span data-ttu-id="826d6-163">Hello ablaktábla szerkesztése, állítson be hello `clientSecret` és `additionalLoginParams` tulajdonságok az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="826d6-163">In hello editing pane, set hello `clientSecret` and `additionalLoginParams` properties as follows.</span></span>
    
        ...
        "clientSecret": "<client key from hello Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. <span data-ttu-id="826d6-164">Kattintson a **Put** : hello felső toosubmit a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="826d6-164">Click **Put** at hello top toosubmit your changes.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. <span data-ttu-id="826d6-165">Mostantól, ha hello engedélyezési jogkivonatot tooaccess hello Azure Active Directory Graph API, egyszerűen lépjen tootest  **https://&lt;*appname*> a.azurewebsites.net/.auth/me** a böngésző.</span><span class="sxs-lookup"><span data-stu-id="826d6-165">Now, tootest if you have hello authorization token tooaccess hello Azure Active Directory Graph API, just navigate to **https://&lt;*appname*>.azurewebsites.net/.auth/me** in your browser.</span></span> <span data-ttu-id="826d6-166">Ha Ön mindent megfelelően konfigurálva, megjelenik hello `access_token` hello JSON-választ a tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="826d6-166">If you configured everything correctly, you should see hello `access_token` property in hello JSON response.</span></span>
    
    <span data-ttu-id="826d6-167">Hello `~/.auth/me` URL-címet felügyeli App Service hitelesítés / engedélyezés toogive meg minden hello vonatkozó információkat tooyour hitelesített munkamenet.</span><span class="sxs-lookup"><span data-stu-id="826d6-167">hello `~/.auth/me` URL path is managed by App Service Authentication / Authorization toogive you all hello information related tooyour authenticated session.</span></span> <span data-ttu-id="826d6-168">További információkért lásd: [hitelesítési és engedélyezési az Azure App Service](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="826d6-168">For more information, see [Authentication and authorization in Azure App Service](../app-service/app-service-authentication-overview.md).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="826d6-169">Hello `access_token` rendelkezik egy olyan lejárati időt.</span><span class="sxs-lookup"><span data-stu-id="826d6-169">hello `access_token` has an expiration period.</span></span> <span data-ttu-id="826d6-170">Azonban App Service hitelesítés / engedélyezés a token frissítési funkciókat kínál `~/.auth/refresh`.</span><span class="sxs-lookup"><span data-stu-id="826d6-170">However, App Service Authentication / Authorization provides token refresh functionality with `~/.auth/refresh`.</span></span> <span data-ttu-id="826d6-171">További információt a toouse, lásd: [szolgáltatás Token Alkalmazásáruházból](https://cgillum.tech/2016/03/07/app-service-token-store/).</span><span class="sxs-lookup"><span data-stu-id="826d6-171">For more information on how toouse it, see [App Service Token Store](https://cgillum.tech/2016/03/07/app-service-token-store/).</span></span>
    > 
    > 

<span data-ttu-id="826d6-172">A következő fogja végrehajtani valamit a címtáradatok hasznos.</span><span class="sxs-lookup"><span data-stu-id="826d6-172">Next, you will do something useful with directory data.</span></span>

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-tooyour-app"></a><span data-ttu-id="826d6-173">Üzleti funkció tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="826d6-173">Add line-of-business functionality tooyour app</span></span>
<span data-ttu-id="826d6-174">Ezután hozzon létre egy egyszerű CRUD munkahelyi elemek Rendszerleállási események követése.</span><span class="sxs-lookup"><span data-stu-id="826d6-174">Now, you create a simple CRUD work items tracker.</span></span>  

1. <span data-ttu-id="826d6-175">Hello ~\Models mappában hozzon létre egy WorkItem.cs nevű osztályt, és cserélje le `public class WorkItem {...}` a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="826d6-175">In hello ~\Models folder, create a class file called WorkItem.cs, and replace `public class WorkItem {...}` with hello following code:</span></span>
   
     <span data-ttu-id="826d6-176">használatával System.ComponentModel.DataAnnotations;</span><span class="sxs-lookup"><span data-stu-id="826d6-176">using System.ComponentModel.DataAnnotations;</span></span>
   
     <span data-ttu-id="826d6-177">WorkItem {nyilvános osztály</span><span class="sxs-lookup"><span data-stu-id="826d6-177">public class WorkItem   {</span></span>
   
         [Key]
         public int ItemID { get; set; }
         public string AssignedToID { get; set; }
         public string AssignedToName { get; set; }
         public string Description { get; set; }
         public WorkItemStatus Status { get; set; }
     <span data-ttu-id="826d6-178">}</span><span class="sxs-lookup"><span data-stu-id="826d6-178">}</span></span>
   
     <span data-ttu-id="826d6-179">nyilvános enum WorkItemStatus {</span><span class="sxs-lookup"><span data-stu-id="826d6-179">public enum WorkItemStatus   {</span></span>
   
         Open,
         Investigating,
         Resolved,
         Closed
     <span data-ttu-id="826d6-180">}</span><span class="sxs-lookup"><span data-stu-id="826d6-180">}</span></span>
2. <span data-ttu-id="826d6-181">Hello projekt toomake az új modell elérhető toohello állványok programot a Visual Studio build.</span><span class="sxs-lookup"><span data-stu-id="826d6-181">Build hello project toomake your new model accessible toohello scaffolding logic in Visual Studio.</span></span>
3. <span data-ttu-id="826d6-182">Egy új generált elem hozzáadása a `WorkItemsController` toohello ~\Controllers mappa (kattintson a jobb gombbal **tartományvezérlők**, pont túl**Hozzáadás**, és válassza ki **új generált elem**).</span><span class="sxs-lookup"><span data-stu-id="826d6-182">Add a new scaffolded item `WorkItemsController` toohello ~\Controllers folder (right-click **Controllers**, point too**Add**, and select **New scaffolded item**).</span></span> 
4. <span data-ttu-id="826d6-183">Válassza ki **MVC 5 Controller nézetek, entitás-keretrendszer használatával** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="826d6-183">Select **MVC 5 Controller with views, using Entity Framework** and click **Add**.</span></span>
5. <span data-ttu-id="826d6-184">SELECT hello modell létrehozott, majd kattintson a  **+**  , majd **Hozzáadás** tooadd egy adatkörnyezetet, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="826d6-184">Select hello model that you created, then click **+** and then **Add** tooadd a data context, and then click **Add**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. <span data-ttu-id="826d6-185">A ~\Views\WorkItems\Create.cshtml (az automatikusan generált elem), megkeresi a hello `Html.BeginForm` segédmetódus, és győződjön meg a kijelölt változtatások hello:</span><span class="sxs-lookup"><span data-stu-id="826d6-185">In ~\Views\WorkItems\Create.cshtml (an automatically scaffolded item), find hello `Html.BeginForm` helper method and make hello following highlighted changes:</span></span>  
   
   <pre class="prettyprint">
   @model WebApplication1.Models.WorkItem
   
   @{
    ViewBag.Title = &quot;Create&quot;;
   }
   
   &lt;h2&gt;Create&lt;/h2&gt;
   
   @using (Html.BeginForm(<mark>&quot;Create&quot;, &quot;WorkItems&quot;, FormMethod.Post, new { id = &quot;main-form&quot; }</mark>)) 
   {
    @Html.AntiForgeryToken()
   
    &lt;div class=&quot;form-horizontal&quot;&gt;
        &lt;h4&gt;WorkItem&lt;/h4&gt;
        &lt;hr /&gt;
        @Html.ValidationSummary(true, &quot;&quot;, new { @class = &quot;text-danger&quot; })
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToID, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToID, new { htmlAttributes = new { @class = &quot;form-control&quot;<mark>, @type = &quot;hidden&quot;</mark> } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToID, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToName, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToName, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToName, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Description, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.Description, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.Description, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EnumDropDownListFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;form-control&quot; })
                @Html.ValidationMessageFor(model =&gt; model.Status, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            &lt;div class=&quot;col-md-offset-2 col-md-10&quot;&gt;
                &lt;input type=&quot;submit&quot; value=&quot;Create&quot; class=&quot;btn btn-default&quot;<mark> id=&quot;submit-button&quot;</mark> /&gt;
            &lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;
   }
   
   &lt;div&gt;
    @Html.ActionLink(&quot;Back tooList&quot;, &quot;Index&quot;)
   &lt;/div&gt;
   
   @section Scripts {
    @Scripts.Render(&quot;~/bundles/jqueryval&quot;)
    <mark>&lt;script&gt;
        // People/Group Picker Code
        var maxResultsPerPage = 14;
        var input = document.getElementById(&quot;AssignedToName&quot;);
   
        // Access token from request header, and tenantID from claims identity
        var token = &quot;@Request.Headers[&quot;X-MS-TOKEN-AAD-ACCESS-TOKEN&quot;]&quot;;
        var tenant =&quot;@(System.Security.Claims.ClaimsPrincipal.Current.Claims
                        .Where(c => c.Type == &quot;http://schemas.microsoft.com/identity/claims/tenantid&quot;)
                        .Select(c => c.Value).SingleOrDefault())&quot;;
   
        var picker = new AadPicker(maxResultsPerPage, input, token, tenant);
   
        // Submit hello selected user/group toobe asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   <span data-ttu-id="826d6-186">Vegye figyelembe, hogy `token` és `tenant` hello által használt `AadPicker` objektum toomake Azure Active Directory Graph API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="826d6-186">Note that `token` and `tenant` are used by hello `AadPicker` object toomake Azure Active Directory Graph API calls.</span></span> <span data-ttu-id="826d6-187">Fogja hozzáadni `AadPicker` később.</span><span class="sxs-lookup"><span data-stu-id="826d6-187">You'll add `AadPicker` later.</span></span>     
   
   > [!NOTE]
   > <span data-ttu-id="826d6-188">Is kaphat `token` és `tenant` hello ügyfél oldalán a `~/.auth/me`, de ez egy további kiszolgálóra hívás lenne.</span><span class="sxs-lookup"><span data-stu-id="826d6-188">You can just as well get `token` and `tenant` from hello client side with `~/.auth/me`, but that would be an additional server call.</span></span> <span data-ttu-id="826d6-189">Példa:</span><span class="sxs-lookup"><span data-stu-id="826d6-189">For example:</span></span>
   > 
   > <span data-ttu-id="826d6-190">$.ajax ({adattípus: "json" URL-címe: "/.auth/me", a sikeres: függvény (adatok) {var token adatok [0] = .access_token; var bérlői adatok [0] .user_claims .find = (c = > c.typ === "http://schemas.microsoft.com/identity/claims/tenantid") .val;}});</span><span class="sxs-lookup"><span data-stu-id="826d6-190">$.ajax({ dataType: "json", url: "/.auth/me", success: function (data) { var token = data[0].access_token; var tenant = data[0].user_claims .find(c => c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val; } });</span></span>
   > 
   > 
7. <span data-ttu-id="826d6-191">Hello végez azonos módosításokat ~ \Views\WorkItems\Edit.cshtml.</span><span class="sxs-lookup"><span data-stu-id="826d6-191">Make hello same changes with ~\Views\WorkItems\Edit.cshtml.</span></span>
8. <span data-ttu-id="826d6-192">Hello `AadPicker` objektum van definiálva egy parancsprogram, hogy kell-e tooadd tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="826d6-192">hello `AadPicker` object is defined in a script that you need tooadd tooyour project.</span></span> <span data-ttu-id="826d6-193">Jobb gombbal az hello ~\Scripts mappa túl**Hozzáadás**, és kattintson a **JavaScript-fájl**.</span><span class="sxs-lookup"><span data-stu-id="826d6-193">Right-click hello ~\Scripts folder, point too**Add**, and click **JavaScript file**.</span></span> <span data-ttu-id="826d6-194">Típus `AadPickerLibrary` hello fájlnevet, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="826d6-194">Type `AadPickerLibrary` for hello filename and click **OK**.</span></span>
9. <span data-ttu-id="826d6-195">Másolja a tartalmat hello [Itt](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) történő ~ \Scripts\AadPickerLibrary.js.</span><span class="sxs-lookup"><span data-stu-id="826d6-195">Copy hello content from [here](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) into ~\Scripts\AadPickerLibrary.js.</span></span>
   
   <span data-ttu-id="826d6-196">Hello parancsfájlban hello `AadPicker` hívások objektum [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) a felhasználókat és csoportokat, amelyek megfelelnek a hello bemeneti toosearch.</span><span class="sxs-lookup"><span data-stu-id="826d6-196">In hello script, hello `AadPicker` object calls [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) toosearch for users and groups that match hello input.</span></span>  
10. <span data-ttu-id="826d6-197">~\Scripts\AadPickerLibrary.js is használ az hello [jQuery felhasználói felület Autocomplete widget](https://jqueryui.com/autocomplete/).</span><span class="sxs-lookup"><span data-stu-id="826d6-197">~\Scripts\AadPickerLibrary.js also uses hello [jQuery UI Autocomplete widget](https://jqueryui.com/autocomplete/).</span></span> <span data-ttu-id="826d6-198">Így kell tooadd jQuery felhasználói felület tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="826d6-198">So you need tooadd jQuery UI tooyour project.</span></span> <span data-ttu-id="826d6-199">Kattintson a jobb gombbal a projektre a, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="826d6-199">Right-click your project in and click **Manage NuGet Packages**.</span></span>
11. <span data-ttu-id="826d6-200">A NuGet-Csomagkezelő hello, kattintson a Tallózás gombra, típus **jquery-ui** hello keresősávban, és kattintson a **jQuery.UI.Combined**.</span><span class="sxs-lookup"><span data-stu-id="826d6-200">In hello NuGet Package Manager, click Browse, type **jquery-ui** in hello search bar, and click **jQuery.UI.Combined**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. <span data-ttu-id="826d6-201">Hello jobb oldali ablaktáblában kattintson **telepítése**, majd kattintson a **OK** tooproceed.</span><span class="sxs-lookup"><span data-stu-id="826d6-201">In hello right pane, click **Install**, then click **OK** tooproceed.</span></span>
13. <span data-ttu-id="826d6-202">Nyissa meg a ~\App_Start\BundleConfig.cs, és hajtsa végre a kijelölt változtatások hello:</span><span class="sxs-lookup"><span data-stu-id="826d6-202">Open ~\App_Start\BundleConfig.cs and make hello following highlighted changes:</span></span>  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
        // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
        bundles.Add(new ScriptBundle(&quot;~/bundles/modernizr&quot;).Include(
                    &quot;~/Scripts/modernizr-*&quot;));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/bootstrap&quot;).Include(
                    &quot;~/Scripts/bootstrap.js&quot;,
                    &quot;~/Scripts/respond.js&quot;));
    
        bundles.Add(new StyleBundle(&quot;~/Content/css&quot;).Include(
                    &quot;~/Content/bootstrap.css&quot;,
                    &quot;~/Content/site.css&quot;<mark>,
                    &quot;~/Content/themes/base/jquery-ui.css&quot;</mark>));
    }
    </pre>
    
    <span data-ttu-id="826d6-203">Nincsenek további performant módon toomanage JavaScript és CSS fájlok az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="826d6-203">There are more performant ways toomanage JavaScript and CSS files in your app.</span></span> <span data-ttu-id="826d6-204">Azonban az egyszerűség kedvéért csak fog toopiggyback a hello csomagok, amelyek minden nézet vannak betöltve.</span><span class="sxs-lookup"><span data-stu-id="826d6-204">However, for simplicity you're just going toopiggyback on hello bundles that are loaded with every view.</span></span>
14. <span data-ttu-id="826d6-205">Végezetül az ~ \Global.asax, adja hozzá a következő kódsort hello hello `Application_Start()` metódust.</span><span class="sxs-lookup"><span data-stu-id="826d6-205">Finally, in ~\Global.asax, add hello following line of code in hello `Application_Start()` method.</span></span> <span data-ttu-id="826d6-206">`Ctrl`+`.`minden elnevezési névfeloldási lévő hiba túl javítást.</span><span class="sxs-lookup"><span data-stu-id="826d6-206">`Ctrl`+`.` on each naming resolution error too fix it.</span></span>
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > <span data-ttu-id="826d6-207">Mivel hello alapértelmezett MVC-sablont használ a kódsort szüksége <code>[ValidateAntiForgeryToken]</code> decoration egyes hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="826d6-207">You need this line of code because hello default MVC template uses <code>[ValidateAntiForgeryToken]</code> decoration on some of hello actions.</span></span> <span data-ttu-id="826d6-208">Lejáró leírt toohello viselkedéstől [Brock Allen](https://twitter.com/BrockLAllen) : [MVC 4, AntiForgeryToken és jogcímek](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) a HTTP POST sikertelenek lehetnek hamisításgátló jogkivonat érvényesítésére, mert:</span><span class="sxs-lookup"><span data-stu-id="826d6-208">Due toohello behavior described by [Brock Allen](https://twitter.com/BrockLAllen) at [MVC 4, AntiForgeryToken and Claims](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) your HTTP POST may fail anti-forgery token validation because:</span></span>
    > 
    > * <span data-ttu-id="826d6-209">Az Azure Active Directory nem küldi hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, amelyre azonban szükség van alapértelmezés szerint hello hamisításgátló jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="826d6-209">Azure Active Directory does not send hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, which is required by default by hello anti-forgery token.</span></span>
    > * <span data-ttu-id="826d6-210">Ha Azure Active Directory szinkronizálása megtörtént-e az AD FS directory, AD FS hello megbízhatósági alapértelmezés szerint nem jogcímet küld ki hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider vagy, bár manuálisan konfigurálhatja az AD FS toosend Ezt a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="826d6-210">If Azure Active Directory is directory synced with AD FS, hello AD FS trust by default does not send hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider claim either, although you can manually configure AD FS toosend this claim.</span></span>
    > 
    > <span data-ttu-id="826d6-211">`ClaimTypes.NameIdentifies`Adja meg a hello jogcím `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, amely Azure Active Directory fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="826d6-211">`ClaimTypes.NameIdentifies` specifies hello claim `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, which Azure Active Directory does supply.</span></span>  
    > 
    > 
15. <span data-ttu-id="826d6-212">Most tegye közzé a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="826d6-212">Now, publish your changes.</span></span> <span data-ttu-id="826d6-213">Kattintson jobb gombbal a projektre, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="826d6-213">Right-click your project and click **Publish**.</span></span>
16. <span data-ttu-id="826d6-214">Kattintson a **beállítások**, győződjön meg arról, hogy van egy kapcsolati karakterlánc tooyour SQL-adatbázis, válassza ki **frissítés adatbázis** toomake hello változás történt a modell, és kattintson a **közzététel** .</span><span class="sxs-lookup"><span data-stu-id="826d6-214">Click **Settings**, make sure there is a connection string tooyour SQL Database, select **Update Database** toomake hello schema changes for your model, and click **Publish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. <span data-ttu-id="826d6-215">Hello böngészőben nyissa meg a toohttps: / /&lt;*appname*>.azurewebsites.net/workitems, és kattintson **hozzon létre új**.</span><span class="sxs-lookup"><span data-stu-id="826d6-215">In hello browser, navigate toohttps://&lt;*appname*>.azurewebsites.net/workitems and click **Create New**.</span></span>
18. <span data-ttu-id="826d6-216">Kattintson a hello **AssignedToName** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="826d6-216">Click in hello **AssignedToName** box.</span></span> <span data-ttu-id="826d6-217">A legördülő listában az Azure Active Directory-bérlő felhasználók és csoportok ekkor megjelenik.</span><span class="sxs-lookup"><span data-stu-id="826d6-217">You should now see users and groups from your Azure Active Directory tenant in a dropdown.</span></span> <span data-ttu-id="826d6-218">Írja be a toofilter, vagy használja a hello `Up` vagy `Down` kulcs, vagy kattintson a tooselect hello felhasználó vagy csoport.</span><span class="sxs-lookup"><span data-stu-id="826d6-218">You can type toofilter, or use hello `Up` or `Down` key or click tooselect hello user or group.</span></span> 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. <span data-ttu-id="826d6-219">Kattintson a **létrehozása** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="826d6-219">Click **Create** toosave hello changes.</span></span> <span data-ttu-id="826d6-220">Kattintson a **szerkesztése** létrehozott hello munka elem tooobserve ugyanez a viselkedés hello.</span><span class="sxs-lookup"><span data-stu-id="826d6-220">Then, click **Edit** on hello created work item tooobserve hello same behavior.</span></span>

<span data-ttu-id="826d6-221">Congrats most már fut egy sor üzleti alkalmazás Azure directory elérésével!</span><span class="sxs-lookup"><span data-stu-id="826d6-221">Congrats, you are now running a line-of-business app in Azure with directory access!</span></span> <span data-ttu-id="826d6-222">Nincs sokkal több hello Graph API teheti.</span><span class="sxs-lookup"><span data-stu-id="826d6-222">There's a lot more you can do with hello Graph API.</span></span> <span data-ttu-id="826d6-223">Lásd: [Azure AD Graph API-referencia](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).</span><span class="sxs-lookup"><span data-stu-id="826d6-223">See [Azure AD Graph API reference](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).</span></span>

<a name="next"></a>

## <a name="next-step"></a><span data-ttu-id="826d6-224">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="826d6-224">Next Step</span></span>
<span data-ttu-id="826d6-225">Ha az üzleti alkalmazás az azure szerepköralapú hozzáférés-vezérlést (RBAC) van szüksége, tekintse meg [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) a hello Azure Active Directory csapat egy mintát.</span><span class="sxs-lookup"><span data-stu-id="826d6-225">If you need role-based access control (RBAC) for your line-of-business app in azure, see [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) for a sample from hello Azure Active Directory team.</span></span> <span data-ttu-id="826d6-226">Megmutatja, hogyan tooenable szerepkörök az Azure Active Directory-alkalmazás, és majd engedélyezése a felhasználók hello `[Authorize]` decoration.</span><span class="sxs-lookup"><span data-stu-id="826d6-226">It shows you how tooenable roles for your Azure Active Directory application, and then authorize users with hello `[Authorize]` decoration.</span></span>

<span data-ttu-id="826d6-227">Ha a sor üzleti alkalmazás tooon helyszíni adatokhoz kell használni, lásd: [hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service](web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="826d6-227">If your line-of-business app needs access tooon-premises data, see [Access on-premises resources using hybrid connections in Azure App Service](web-sites-hybrid-connection-get-started.md).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="826d6-228">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="826d6-228">Further resources</span></span>
* [<span data-ttu-id="826d6-229">Hitelesítési és engedélyezési az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="826d6-229">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)
* [<span data-ttu-id="826d6-230">A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="826d6-230">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="826d6-231">Üzleti alkalmazás létrehozása az Azure-ban az AD FS-hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="826d6-231">Create a line-of-business app in Azure with AD FS authentication</span></span>](web-sites-dotnet-lob-application-adfs.md)
* [<span data-ttu-id="826d6-232">App Service hitelesítés és hello Azure AD Graph API</span><span class="sxs-lookup"><span data-stu-id="826d6-232">App Service Auth and hello Azure AD Graph API</span></span>](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [<span data-ttu-id="826d6-233">A Microsoft Azure Active Directory-példák és dokumentáció</span><span class="sxs-lookup"><span data-stu-id="826d6-233">Microsoft Azure Active Directory Samples and Documentation</span></span>](https://github.com/AzureADSamples)
* [<span data-ttu-id="826d6-234">Az Azure Active Directory támogatott jogkivonat és jogcímtípusok</span><span class="sxs-lookup"><span data-stu-id="826d6-234">Azure Active Directory Supported Token and Claim Types</span></span>](http://msdn.microsoft.com/library/azure/dn195587.aspx)
