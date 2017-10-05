---
title: "Azure-felhőszolgáltatás integrálása az Azure CDN |} Microsoft Docs"
description: "Egy felhőszolgáltatás, amely egy integrált Azure CDN-végpont a tartalmat szolgáltat telepítése"
services: cdn, cloud-services
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: tysonn
ms.assetid: b3c0108f-9ec5-43a8-8fd0-40eafbd32637
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f2849fe25fd0d5b3dc26598ffba7591cb7433161
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <span data-ttu-id="d393f-103"><a name="intro"></a>Egy felhőalapú szolgáltatás integrálása az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="d393f-103"><a name="intro"></a> Integrate a cloud service with Azure CDN</span></span>
<span data-ttu-id="d393f-104">Egy felhőalapú szolgáltatás Azure CDN-t, a tartalom a felhőszolgáltatás helyről integrálható.</span><span class="sxs-lookup"><span data-stu-id="d393f-104">A cloud service can be integrated with Azure CDN, serving any content from the cloud service's location.</span></span> <span data-ttu-id="d393f-105">Ez a megközelítés lehetővé teszi a következő előnyökkel jár:</span><span class="sxs-lookup"><span data-stu-id="d393f-105">This approach gives you the following advantages:</span></span>

* <span data-ttu-id="d393f-106">Egyszerűen regisztrálhat és lemezképek, parancsprogramok és a felhőszolgáltatás-projekt címtárakban stíluslapok frissítése</span><span class="sxs-lookup"><span data-stu-id="d393f-106">Easily deploy and update images, scripts, and stylesheets in your cloud service's project directories</span></span>
* <span data-ttu-id="d393f-107">Könnyedén frissíthet a felhőalapú szolgáltatás, például a jQuery vagy rendszerindítási verzió a NuGet-csomagok</span><span class="sxs-lookup"><span data-stu-id="d393f-107">Easily upgrade the NuGet packages in your cloud service, such as jQuery or Bootstrap versions</span></span>
* <span data-ttu-id="d393f-108">A webes alkalmazás és a CDN-kiszolgált tartalom minden az ugyanabban a Visual Studio felületén kezelése</span><span class="sxs-lookup"><span data-stu-id="d393f-108">Manage your Web application and your CDN-served content all from the same Visual Studio interface</span></span>
* <span data-ttu-id="d393f-109">A webes alkalmazás és a CDN-kiszolgált tartalom egyesített telepítési munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="d393f-109">Unified deployment workflow for your Web application and your CDN-served content</span></span>
* <span data-ttu-id="d393f-110">ASP.NET kötegelése és minification integrálása az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="d393f-110">Integrate ASP.NET bundling and minification with Azure CDN</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="d393f-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="d393f-111">What you will learn</span></span>
<span data-ttu-id="d393f-112">Ebből az oktatóanyagból megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="d393f-112">In this tutorial, you will learn how to:</span></span>

* [<span data-ttu-id="d393f-113">Az Azure CDN-végpont integrálható a felhőalapú szolgáltatás, és a statikus tartalmat szolgáltat a weblapok Azure CDN</span><span class="sxs-lookup"><span data-stu-id="d393f-113">Integrate an Azure CDN endpoint with your cloud service and serve static content in your Web pages from Azure CDN</span></span>](#deploy)
* [<span data-ttu-id="d393f-114">Adja meg a statikus tartalom gyorsítótár beállításait a felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d393f-114">Configure cache settings for static content in your cloud service</span></span>](#caching)
* [<span data-ttu-id="d393f-115">Az Azure CDN keresztül vezérlő műveletekből tartalmat</span><span class="sxs-lookup"><span data-stu-id="d393f-115">Serve content from controller actions through Azure CDN</span></span>](#controller)
* [<span data-ttu-id="d393f-116">Kötegelve szolgálnak, és minified Azure CDN tartalom megőrzése mellett a felhasználói élmény, a Visual Studio hibakeresési parancsfájl</span><span class="sxs-lookup"><span data-stu-id="d393f-116">Serve bundled and minified content through Azure CDN while preserving the script debugging experience in Visual Studio</span></span>](#bundling)
* [<span data-ttu-id="d393f-117">Konfigurálhatja tartalék CSS és parancsfájlok, amikor az Azure CDN offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="d393f-117">Configure fallback your scripts and CSS when your Azure CDN is offline</span></span>](#fallback)

## <a name="what-you-will-build"></a><span data-ttu-id="d393f-118">Mit fog létrehozni</span><span class="sxs-lookup"><span data-stu-id="d393f-118">What you will build</span></span>
<span data-ttu-id="d393f-119">Fogja telepíteni egy felhőalapú szolgáltatás webes szerepkör az alapértelmezett ASP.NET MVC-sablont, adja hozzá a kódot egy integrált Azure CDN, kép, a tartományvezérlő műveleti eredmény és az alapértelmezett JavaScript és CSS fájlok, például tartalom kiszolgálására és is létrehozható, a tartalék mechanizmusa kötegek és kiszolgálása között abban az esetben, ha a kapcsolat nélküli üzemmódban a CDN konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="d393f-119">You will deploy a cloud service Web role using the default ASP.NET MVC template, add code to serve content from an integrated Azure CDN, such as an image, controller action results, and the default JavaScript and CSS files, and also write code to configure the fallback mechanism for bundles served in the event that the CDN is offline.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="d393f-120">Mit kell</span><span class="sxs-lookup"><span data-stu-id="d393f-120">What you will need</span></span>
<span data-ttu-id="d393f-121">Ez az oktatóanyag előfeltételei a következők:</span><span class="sxs-lookup"><span data-stu-id="d393f-121">This tutorial has the following prerequisites:</span></span>

* <span data-ttu-id="d393f-122">Az aktív [Microsoft Azure-fiók](/account/)</span><span class="sxs-lookup"><span data-stu-id="d393f-122">An active [Microsoft Azure account](/account/)</span></span>
* <span data-ttu-id="d393f-123">A Visual Studio 2015 [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="d393f-123">Visual Studio 2015 with [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span></span>

> [!NOTE]
> <span data-ttu-id="d393f-124">A jelen oktatóanyag elvégzéséhez Azure-fiókra van szükség:</span><span class="sxs-lookup"><span data-stu-id="d393f-124">You need an Azure account to complete this tutorial:</span></span>
> 
> * <span data-ttu-id="d393f-125">Is [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) -kapott kreditek használatával kipróbálhatja a fizetős Azure-szolgáltatásokat, és még azok lejárta után is megtarthatja a fiókot, és használhatja az ingyenes Azure-szolgáltatások, például webhelyekhez.</span><span class="sxs-lookup"><span data-stu-id="d393f-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services, such as Websites.</span></span>
> * <span data-ttu-id="d393f-126">Is [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -az MSDN-előfizetés biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="d393f-126">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a><span data-ttu-id="d393f-127">A felhőalapú szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="d393f-127">Deploy a cloud service</span></span>
<span data-ttu-id="d393f-128">Ebben a szakaszban fogja telepíteni az alapértelmezett ASP.NET MVC alkalmazássablon a Visual Studio 2015 egy felhőalapú szolgáltatás webes szerepkör, és integrálhatja egy új CDN-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="d393f-128">In this section, you will deploy the default ASP.NET MVC application template in Visual Studio 2015 to a cloud service Web role, and then integrate it with a new CDN endpoint.</span></span> <span data-ttu-id="d393f-129">Kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="d393f-129">Follow the instructions below:</span></span>

1. <span data-ttu-id="d393f-130">A Visual Studio 2015, hozzon létre egy új Azure-felhőszolgáltatásban menü címen **fájl > Új > Projekt > felhő > Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="d393f-130">In Visual Studio 2015, create a new Azure cloud service from the menu bar by going to **File > New > Project > Cloud > Azure Cloud Service**.</span></span> <span data-ttu-id="d393f-131">Adjon neki egy nevet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="d393f-131">Give it a name and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. <span data-ttu-id="d393f-132">Válassza ki **ASP.NET webes szerepkör** , és kattintson a  **>**  gombra.</span><span class="sxs-lookup"><span data-stu-id="d393f-132">Select **ASP.NET Web Role** and click the **>** button.</span></span> <span data-ttu-id="d393f-133">Kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="d393f-133">Click OK.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. <span data-ttu-id="d393f-134">Válassza ki **MVC** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="d393f-134">Select **MVC** and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. <span data-ttu-id="d393f-135">Most tegye közzé a webes szerepkör egy Azure felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d393f-135">Now, publish this Web role to an Azure cloud service.</span></span> <span data-ttu-id="d393f-136">Kattintson a jobb gombbal a felhőszolgáltatás-projekt, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="d393f-136">Right-click the cloud service project and select **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. <span data-ttu-id="d393f-137">Ha még nem írta be a Microsoft Azure, kattintson a **fiók hozzáadása...**  legördülő menüből, majd a **vegyen fel egy fiókot** menüpont.</span><span class="sxs-lookup"><span data-stu-id="d393f-137">If you have not yet signed into Microsoft Azure, click the **Add an account...** dropdown and click the **Add an account** menu item.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. <span data-ttu-id="d393f-138">A bejelentkezési oldal jelentkezzen be az Azure-fiók aktiválásához használt Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="d393f-138">In the sign-in page, sign in with the Microsoft account you used to activate your Azure account.</span></span>
7. <span data-ttu-id="d393f-139">Ha be van jelentkezve, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="d393f-139">Once you're signed in, click **Next**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. <span data-ttu-id="d393f-140">Feltételezve, hogy egy felhőalapú szolgáltatás, vagy tárolási fiókja még nem hozott létre, a Visual Studio segítségével hozhat létre is.</span><span class="sxs-lookup"><span data-stu-id="d393f-140">Assuming that you haven't created a cloud service or storage account, Visual Studio will help you create both.</span></span> <span data-ttu-id="d393f-141">Az a **felhőalapú szolgáltatás létrehozása és a fiók** párbeszédpanelen írja be a kívánt szolgáltatás nevét, és válassza ki a kívánt régiót.</span><span class="sxs-lookup"><span data-stu-id="d393f-141">In the **Create Cloud Service and Account** dialog, type the desired service name and select the desired region.</span></span> <span data-ttu-id="d393f-142">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d393f-142">Then, click **Create**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. <span data-ttu-id="d393f-143">A közzétételi beállítások lapon ellenőrizze a konfigurációt, és kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="d393f-143">In the publish settings page, verify the configuration and click **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > <span data-ttu-id="d393f-144">A felhőszolgáltatások a közzétételi folyamat hosszú ideig tart.</span><span class="sxs-lookup"><span data-stu-id="d393f-144">The publishing process for cloud services takes a long time.</span></span> <span data-ttu-id="d393f-145">Az engedélyezéséhez a Web Deploy összes szerepkörök beállítás teheti a felhőalapú szolgáltatás sokkal gyorsabb a webes szerepkörök gyors (de ideiglenes) frissítéseket biztosító hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="d393f-145">The Enable Web Deploy for all roles option can make debugging your cloud service much quicker by providing fast (but temporary) updates to your Web roles.</span></span> <span data-ttu-id="d393f-146">Ez a beállítás további információkért lásd: [közzététele a felhőalapú szolgáltatás, az Azure-eszközökkel](http://msdn.microsoft.com/library/ff683672.aspx).</span><span class="sxs-lookup"><span data-stu-id="d393f-146">For more information on this option, see [Publishing a Cloud Service using the Azure Tools](http://msdn.microsoft.com/library/ff683672.aspx).</span></span>
   > 
   > 
   
    <span data-ttu-id="d393f-147">Ha a **Microsoft Azure tevékenységnapló** mutatja, hogy közzétételi állapotát **befejezve**, létrehozhat egy CDN-végpontot, amely integrálva van a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d393f-147">When the **Microsoft Azure Activity Log** shows that publishing status is **Completed**, you will create a CDN endpoint that's integrated with this cloud service.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="d393f-148">Ha a közzététel után, a központilag telepített a felhőalapú szolgáltatás egy hiba képernyő jelenik meg, valószínű, mert a felhőalapú szolgáltatás telepítése után használja a [vendég operációs rendszer, amely nem tartalmazza a .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span><span class="sxs-lookup"><span data-stu-id="d393f-148">If, after publishing, the deployed cloud service displays an error screen, it's likely because the cloud service you've deployed is using a [guest OS that does not include .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span></span>  <span data-ttu-id="d393f-149">Által a probléma megkerüléséhez [központi telepítése a .NET 4.5.2 az indítási feladatok](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="d393f-149">You can work around this issue by [deploying .NET 4.5.2 as a startup task](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span></span>
   > 
   > 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="d393f-150">Új CDN-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="d393f-150">Create a new CDN profile</span></span>
<span data-ttu-id="d393f-151">A CDN-profil CDN-végpontok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="d393f-151">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="d393f-152">Minden profil egy vagy több CDN-végpontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d393f-152">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="d393f-153">Előfordulhat, hogy több profil használatával rendszerezni szeretné a CDN-végpontokat internetes tartomány, webalkalmazás vagy más feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="d393f-153">You may wish to use multiple profiles to organize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!TIP]
> <span data-ttu-id="d393f-154">Ha már van a CDN-profilt, amely a jelen oktatóanyag használni kívánt, lépjen [hozzon létre egy új CDN-végpont](#create-a-new-cdn-endpoint).</span><span class="sxs-lookup"><span data-stu-id="d393f-154">If you already have a CDN profile that you want to use for this tutorial, proceed to [Create a new CDN endpoint](#create-a-new-cdn-endpoint).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="d393f-155">Új CDN-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="d393f-155">Create a new CDN endpoint</span></span>
<span data-ttu-id="d393f-156">**A tárfiók új CDN-végpont létrehozása**</span><span class="sxs-lookup"><span data-stu-id="d393f-156">**To create a new CDN endpoint for your storage account**</span></span>

1. <span data-ttu-id="d393f-157">Az a [Azure felügyeleti portálon](https://portal.azure.com), keresse meg a CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="d393f-157">In the [Azure Management Portal](https://portal.azure.com), navigate to your CDN profile.</span></span>  <span data-ttu-id="d393f-158">Lehetséges, hogy a profilt az előző lépésben az irányítópulton rögzítette.</span><span class="sxs-lookup"><span data-stu-id="d393f-158">You may have pinned it to the dashboard in the previous step.</span></span>  <span data-ttu-id="d393f-159">Amennyiben nem rögzítette, kattintson **Tallózás** gombra, majd a **CDN-profilok** elemre, végül kattintson arra a profilra, amelyet hozzá szeretne adni a végponthoz.</span><span class="sxs-lookup"><span data-stu-id="d393f-159">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on the profile you plan to add your endpoint to.</span></span>
   
    <span data-ttu-id="d393f-160">Megjelenik a CDN-profil panelje.</span><span class="sxs-lookup"><span data-stu-id="d393f-160">The CDN profile blade appears.</span></span>
   
    ![CDN-profil][cdn-profile-settings]
2. <span data-ttu-id="d393f-162">Kattintson a **Végpont hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="d393f-162">Click the **Add Endpoint** button.</span></span>
   
    ![Végpont hozzáadása gomb][cdn-new-endpoint-button]
   
    <span data-ttu-id="d393f-164">Megjelenik a **Végpont hozzáadása** panel.</span><span class="sxs-lookup"><span data-stu-id="d393f-164">The **Add an endpoint** blade appears.</span></span>
   
    ![Végpont hozzáadása panel][cdn-add-endpoint]
3. <span data-ttu-id="d393f-166">Adja meg a CDN-végpont kívánt nevét a **Név** mezőben.</span><span class="sxs-lookup"><span data-stu-id="d393f-166">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="d393f-167">A rendszer ezt a nevet fogja használni a gyorsítótárazott erőforrások eléréséhez a `<EndpointName>.azureedge.net` tartományban.</span><span class="sxs-lookup"><span data-stu-id="d393f-167">This name will be used to access your cached resources at the domain `<EndpointName>.azureedge.net`.</span></span>
4. <span data-ttu-id="d393f-168">Az a **forrása típusa** legördülő menüből válassza *felhőalapú szolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="d393f-168">In the **Origin type** dropdown, select *Cloud service*.</span></span>  
5. <span data-ttu-id="d393f-169">Az a **forrásállomásnév** legördülő menüben válassza ki a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d393f-169">In the **Origin hostname** dropdown, select your cloud service.</span></span>
6. <span data-ttu-id="d393f-170">Hagyja meg az alapértelmezett értéket **forrás elérési útvonalának**, **forrás állomásfejlécét**, és **protokoll/forrásport**.</span><span class="sxs-lookup"><span data-stu-id="d393f-170">Leave the defaults for **Origin path**, **Origin host header**, and **Protocol/Origin port**.</span></span>  <span data-ttu-id="d393f-171">Meg kell adnia legalább egy protokollt (HTTP vagy HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d393f-171">You must specify at least one protocol (HTTP or HTTPS).</span></span>
7. <span data-ttu-id="d393f-172">Az új végpont létrehozásához kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d393f-172">Click the **Add** button to create the new endpoint.</span></span>
8. <span data-ttu-id="d393f-173">A végpont a létrehozás után megjelenik a profil végpontjainak listájában.</span><span class="sxs-lookup"><span data-stu-id="d393f-173">Once the endpoint is created, it appears in a list of endpoints for the profile.</span></span> <span data-ttu-id="d393f-174">A listanézetben látható a gyorsítótárazott tartalom eléréséhez használható URL-cím, valamint a forrástartomány is.</span><span class="sxs-lookup"><span data-stu-id="d393f-174">The list view shows the URL to use to access cached content, as well as the origin domain.</span></span>
   
    ![CDN-végpont][cdn-endpoint-success]
   
   > [!NOTE]
   > <span data-ttu-id="d393f-176">A végpont nem azonnal lesz használható.</span><span class="sxs-lookup"><span data-stu-id="d393f-176">The endpoint will not immediately be available for use.</span></span>  <span data-ttu-id="d393f-177">A regisztráció a CDN a hálózaton keresztül propagálása 90 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d393f-177">It can take up to 90 minutes for the registration to propagate through the CDN network.</span></span> <span data-ttu-id="d393f-178">A CDN-tartománynevet azonnal használható felhasználók 404 állapotkódot jelenhet meg addig, amíg a tartalom nem érhető el a CDN keresztül.</span><span class="sxs-lookup"><span data-stu-id="d393f-178">Users who try to use the CDN domain name immediately may receive status code 404 until the content is available via the CDN.</span></span>
   > 
   > 

## <a name="test-the-cdn-endpoint"></a><span data-ttu-id="d393f-179">A CDN-végpont tesztelése</span><span class="sxs-lookup"><span data-stu-id="d393f-179">Test the CDN endpoint</span></span>
<span data-ttu-id="d393f-180">A közzétételi állapot esetén **befejezve**, nyisson meg egy böngészőablakot, és navigáljon a  **http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span><span class="sxs-lookup"><span data-stu-id="d393f-180">When the publishing status is **Completed**, open a browser window and navigate to **http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span></span> <span data-ttu-id="d393f-181">A beállítás az URL-cím van:</span><span class="sxs-lookup"><span data-stu-id="d393f-181">In my setup, this URL is:</span></span>

    http://camservice.azureedge.net/Content/bootstrap.css

<span data-ttu-id="d393f-182">A következő forrás URL-címet a CDN-végpontot, amely megfelel:</span><span class="sxs-lookup"><span data-stu-id="d393f-182">Which corresponds to the following origin URL at the CDN endpoint:</span></span>

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

<span data-ttu-id="d393f-183">Kiválasztásakor  **http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, attól függően, a böngésző kérni fogja letölteni, vagy nyissa meg a bootstrap.css, amely innen származik: a közzétett webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d393f-183">When you navigate to **http://*&lt;cdnName>*.azureedge.net/Content/bootstrap.css**, depending on your browser, you will be prompted to download or open the bootstrap.css that came from your published Web app.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

<span data-ttu-id="d393f-184">Hasonló módon érheti el bármely nyilvánosan elérhető URL-  **http://*&lt;serviceName >*.cloudapp.net/** rögtön a CDN-végpont.</span><span class="sxs-lookup"><span data-stu-id="d393f-184">You can similarly access any publicly accessible URL at **http://*&lt;serviceName>*.cloudapp.net/**, straight from your CDN endpoint.</span></span> <span data-ttu-id="d393f-185">Példa:</span><span class="sxs-lookup"><span data-stu-id="d393f-185">For example:</span></span>

* <span data-ttu-id="d393f-186">Egy .js fájlt a setup parancs/Script elérési útjáról</span><span class="sxs-lookup"><span data-stu-id="d393f-186">A .js file from the /Script path</span></span>
* <span data-ttu-id="d393f-187">Minden tartalom fájlt a /Content elérési útja</span><span class="sxs-lookup"><span data-stu-id="d393f-187">Any content file from the /Content path</span></span>
* <span data-ttu-id="d393f-188">Bármely tartományvezérlő/művelet</span><span class="sxs-lookup"><span data-stu-id="d393f-188">Any controller/action</span></span>
* <span data-ttu-id="d393f-189">Ha a lekérdezési karakterlánc engedélyezve van a CDN-végpontot, a lekérdezési karakterláncot tartalmazó összes URL-címe:</span><span class="sxs-lookup"><span data-stu-id="d393f-189">If the query string is enabled at your CDN endpoint, any URL with query strings</span></span>

<span data-ttu-id="d393f-190">Valójában a fenti konfigurációjával tárolhatja a teljes körű felhőalapú szolgáltatás  **http://*&lt;cdnName >*.azureedge.net/**.</span><span class="sxs-lookup"><span data-stu-id="d393f-190">In fact, with the above configuration, you can host the entire cloud service from **http://*&lt;cdnName>*.azureedge.net/**.</span></span> <span data-ttu-id="d393f-191">Ha I navigáljon **http://camservice.azureedge.net/**, otthoni/Index jelenik meg a művelet eredménye.</span><span class="sxs-lookup"><span data-stu-id="d393f-191">If I navigate to **http://camservice.azureedge.net/**, I get the action result from Home/Index.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

<span data-ttu-id="d393f-192">Ez nem jelenti azt, azonban, hogy a rendszer mindig egy teljes körű felhőalapú szolgáltatás Azure CDN keresztül kiszolgálására jó ötlet.</span><span class="sxs-lookup"><span data-stu-id="d393f-192">This does not mean, however, that it's always a good idea to serve an entire cloud service through Azure CDN.</span></span> 

<span data-ttu-id="d393f-193">Statikus kézbesítési optimalizálásával CDN nem feltétlenül felgyorsítása dinamikus eszközöket, amelyek nem jelentenek gyorsítótárazható, kézbesítését vagy nagyon gyakran frissülnek, mivel a CDN-t kell beolvasnia az eszköz új verziójának a forráskiszolgálóról nagyon gyakran.</span><span class="sxs-lookup"><span data-stu-id="d393f-193">A CDN with static delivery optimization does not necessarily speed up delivery of dynamic assets which are not meant to be cached, or are updated very frequently, since the CDN must pull a new version of the asset from the Origin server very often.</span></span> <span data-ttu-id="d393f-194">A jelen esetben engedélyezhető [dinamikus hely Acceleration](cdn-dynamic-site-acceleration.md) CDN-végpont nem gyorsítótárazható dinamikus eszközök kézbesítését felgyorsítása érdekében különböző módszereket használó optimalizálása (DSA).</span><span class="sxs-lookup"><span data-stu-id="d393f-194">For this scenario, you can enable [Dynamic Site Acceleration](cdn-dynamic-site-acceleration.md) optimization (DSA) on your CDN endpoint which uses various techniques to speed up delivery of non-cacheable dynamic assets.</span></span> 

<span data-ttu-id="d393f-195">Ha statikus és dinamikus tartalom vegyesen hellyel rendelkezik, megadhatja a statikus tartalom szolgáltatásához CDN (például általános webes kézbesítés) statikus optimalizálási típusú, és dinamikus tartalom vagy közvetlenül a forráskiszolgálóról szolgáltatásához, vagy a CDN-végpontok DSA optimalizálásával keresztül kapcsolva-eseti alapon.</span><span class="sxs-lookup"><span data-stu-id="d393f-195">If you have a site with a mix of static and dynamic content, you can choose to serve your static content from CDN with a static optimization type (such as general web delivery), and to serve dynamic content either directly from the origin server, or through a CDN endpoint with DSA optimization turned on a case-by-case basis.</span></span> <span data-ttu-id="d393f-196">Ebből a célból már láthatta a CDN-végpont az egyes tartalomfájlok elérését.</span><span class="sxs-lookup"><span data-stu-id="d393f-196">To that end, you have already seen how to access individual content files from the CDN endpoint.</span></span> <span data-ttu-id="d393f-197">I bemutatja, hogyan egy adott vezérlő művelet egy adott CDN-végpont használatával látják el vezérlő műveletek Azure CDN keresztül tartalmat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="d393f-197">I will show you how to serve a specific controller action through a specific CDN endpoint in Serve content from controller actions through Azure CDN.</span></span>

<span data-ttu-id="d393f-198">Ez esetben határozzák meg a felhőszolgáltatás-eseti alapon Azure CDN kiszolgálására mely tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d393f-198">The alternative is to determine which content to serve from Azure CDN on a case-by-case basis in your cloud service.</span></span> <span data-ttu-id="d393f-199">Ebből a célból már láthatta a CDN-végpont az egyes tartalomfájlok elérését.</span><span class="sxs-lookup"><span data-stu-id="d393f-199">To that end, you have already seen how to access individual content files from the CDN endpoint.</span></span> <span data-ttu-id="d393f-200">I bemutatja, hogyan keresztül a CDN-végpont egy adott vezérlő művelet kiszolgálására [vezérlő műveletekből Azure CDN keresztül tartalmat](#controller).</span><span class="sxs-lookup"><span data-stu-id="d393f-200">I will show you how to serve a specific controller action through the CDN endpoint in [Serve content from controller actions through Azure CDN](#controller).</span></span>

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a><span data-ttu-id="d393f-201">A felhőszolgáltatás a statikus fájlok gyorsítótárazási beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d393f-201">Configure caching options for static files in your cloud service</span></span>
<span data-ttu-id="d393f-202">Az Azure CDN integráció a felhőalapú szolgáltatás megadhatja, hogyan szeretne statikus tartalmat, hogy a CDN-végpont gyorsítótárazva.</span><span class="sxs-lookup"><span data-stu-id="d393f-202">With Azure CDN integration in your cloud service, you can specify how you want static content to be cached in the CDN endpoint.</span></span> <span data-ttu-id="d393f-203">Ehhez nyissa meg a *Web.config* a webes szerepkör projekt (pl. WebRole1), és adja hozzá a `<staticContent>` elem `<system.webServer>`.</span><span class="sxs-lookup"><span data-stu-id="d393f-203">To do this, open *Web.config* from your Web role project (e.g. WebRole1) and add a `<staticContent>` element to `<system.webServer>`.</span></span> <span data-ttu-id="d393f-204">Az alábbi XML konfigurálja a gyorsítótár 3 nap múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="d393f-204">The XML below configures the cache to expire in 3 days.</span></span>  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

<span data-ttu-id="d393f-205">Ezután a, a felhőszolgáltatásban található összes statikus fájlok megfigyelheti ugyanaz a szabály a CDN-gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="d393f-205">Once you do this, all static files in your cloud service will observe the same rule in your CDN cache.</span></span> <span data-ttu-id="d393f-206">Részletesebben vezérelhető, a gyorsítótár beállításait, vegye fel a *Web.config* fájlt egy mappába, és ott a beállítások.</span><span class="sxs-lookup"><span data-stu-id="d393f-206">For more granular control of cache settings, add a *Web.config* file into a folder and add your settings there.</span></span> <span data-ttu-id="d393f-207">Például hozzáadhat egy *Web.config* fájlt a *\Content* mappa, és cserélje le a tartalmat a következő XML-kód:</span><span class="sxs-lookup"><span data-stu-id="d393f-207">For example, add a *Web.config* file to the *\Content* folder and replace the content with the following XML:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

<span data-ttu-id="d393f-208">Ez a beállítás hatására az összes statikus fájlok a *\Content* mappát 15 napig gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="d393f-208">This setting causes all static files from the *\Content* folder to be cached for 15 days.</span></span>

<span data-ttu-id="d393f-209">Konfigurálásával kapcsolatos további információ a `<clientCache>` elem, lásd: [Ügyfélgyorsítótár &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span><span class="sxs-lookup"><span data-stu-id="d393f-209">For more information on how to configure the `<clientCache>` element, see [Client Cache &lt;clientCache>](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span></span>

<span data-ttu-id="d393f-210">A [vezérlő műveletekből Azure CDN keresztül tartalmat](#controller), I is bemutatja, hogyan konfigurálja a tartományvezérlő műveleti eredmény-gyorsítótárának beállításait a CDN-gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="d393f-210">In [Serve content from controller actions through Azure CDN](#controller), I will also show you how you can configure cache settings for controller action results in the CDN cache.</span></span>

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a><span data-ttu-id="d393f-211">Az Azure CDN keresztül vezérlő műveletekből tartalmat</span><span class="sxs-lookup"><span data-stu-id="d393f-211">Serve content from controller actions through Azure CDN</span></span>
<span data-ttu-id="d393f-212">Egy felhőalapú szolgáltatás webes szerepkör integrálása Azure CDN, esetén az Azure CDN keresztül vezérlő műveletekből tartalmat viszonylag egyszerű.</span><span class="sxs-lookup"><span data-stu-id="d393f-212">When you integrate a cloud service Web role with Azure CDN, it is relatively easy to serve content from controller actions through the Azure CDN.</span></span> <span data-ttu-id="d393f-213">Eltérő szolgál a felhőalapú szolgáltatás közvetlenül az Azure CDN (egy fent), [Martin Balliauw](https://twitter.com/maartenballiauw) bemutatja, hogyan teheti meg egy visszatöltött a tartományvezérlőre, MemeGenerator [csökkenti a késéseket, a webhely, az Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span><span class="sxs-lookup"><span data-stu-id="d393f-213">Other than serving your cloud service directly through Azure CDN (demonstrated above), [Maarten Balliauw](https://twitter.com/maartenballiauw) shows you how to do it with a fun MemeGenerator controller in [Reducing latency on the web with the Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span></span> <span data-ttu-id="d393f-214">I fog egyszerűen Reprodukálja azt itt.</span><span class="sxs-lookup"><span data-stu-id="d393f-214">I will simply reproduce it here.</span></span>

<span data-ttu-id="d393f-215">Tegyük fel, hogy a felhő szolgáltatás memes generál egy fiatal Horváth Norris-lemezképen alapuló (által fénykép [Alan világos](http://www.flickr.com/photos/alan-light/218493788/)) Ez, például:</span><span class="sxs-lookup"><span data-stu-id="d393f-215">Suppose in your cloud service you want to generate memes based on a young Chuck Norris image (photo by [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) like this:</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

<span data-ttu-id="d393f-216">Rendelkezik egy egyszerű `Index` művelet, amely lehetővé teszi az ügyfelek számára a képet, adja meg a superlatives akkor hoz létre a meme után azok küldje el a műveletet.</span><span class="sxs-lookup"><span data-stu-id="d393f-216">You have a simple `Index` action that allows the customers to specify the superlatives in the image, then generates the meme once they post to the action.</span></span> <span data-ttu-id="d393f-217">Mivel ez Horváth Norris, teheti meg ezen a lapon globálisan menet népszerűvé vált.</span><span class="sxs-lookup"><span data-stu-id="d393f-217">Since it's Chuck Norris, you would expect this page to become wildly popular globally.</span></span> <span data-ttu-id="d393f-218">Ez az Azure CDN félig dinamikus tartalmat szolgáltató jól szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="d393f-218">This is a good example of serving semi-dynamic content with Azure CDN.</span></span>

<span data-ttu-id="d393f-219">Kövesse a fenti beállítása a tartományvezérlő műveleti lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d393f-219">Follow the steps above to setup this controller action:</span></span>

1. <span data-ttu-id="d393f-220">Az a *\Controllers* mappa, hozzon létre egy új .cs fájlt nevű *MemeGeneratorController.cs* , és cserélje le a tartalmat a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="d393f-220">In the *\Controllers* folder, create a new .cs file called *MemeGeneratorController.cs* and replace the content with the following code.</span></span> <span data-ttu-id="d393f-221">Ne felejtse el a kijelölt részét cserélje le a CDN-nevet.</span><span class="sxs-lookup"><span data-stu-id="d393f-221">Be sure to replace the highlighted portion with your CDN name.</span></span>  
   
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;
   
        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
   
                public ActionResult Index()
                {
                    return View();
                }
   
                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }
   
                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }
   
                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }
   
                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }
   
                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
   
                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }
   
                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }
   
                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);
   
                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }
   
                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }
2. <span data-ttu-id="d393f-222">Kattintson a jobb gombbal az alapértelmezett `Index()` műveletet, és kijelölheti **nézet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d393f-222">Right-click in the default `Index()` action and select **Add View**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. <span data-ttu-id="d393f-223">Fogadja el az alábbi beállításokat, majd kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d393f-223">Accept the settings below and click **Add**.</span></span>
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. <span data-ttu-id="d393f-224">Nyissa meg az új *Views\MemeGenerator\Index.cshtml* , és cserélje le a tartalmat a következő egyszerű HTML a superlatives elküldése:</span><span class="sxs-lookup"><span data-stu-id="d393f-224">Open the new *Views\MemeGenerator\Index.cshtml* and replace the content with the following simple HTML for submitting the superlatives:</span></span>
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. <span data-ttu-id="d393f-225">Tegye közzé újra a felhőalapú szolgáltatás, és keresse meg  **http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="d393f-225">Publish the cloud service again and navigate to **http://*&lt;serviceName>*.cloudapp.net/MemeGenerator/Index** in your browser.</span></span>

<span data-ttu-id="d393f-226">Ha küldje el a képernyőn értékeket `/MemeGenerator/Index`, a `Index_Post` tartozó műveleti módszer mutató hivatkozást ad vissza a `Show` megfelelő bemeneti azonosítóval tartozó műveleti módszer.</span><span class="sxs-lookup"><span data-stu-id="d393f-226">When you submit the form values to `/MemeGenerator/Index`, the `Index_Post` action method returns a link to the `Show` action method with the respective input identifier.</span></span> <span data-ttu-id="d393f-227">A hivatkozásra kattintva érhető el a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d393f-227">When you click the link, you reach the following code:</span></span>  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

<span data-ttu-id="d393f-228">Ha a helyi hibakereső van csatlakoztatva, majd elérhetővé válik a helyi átirányítással rendszeres hibakeresési élményt.</span><span class="sxs-lookup"><span data-stu-id="d393f-228">If your local debugger is attached, then you will get the regular debug experience with a local redirect.</span></span> <span data-ttu-id="d393f-229">Ha a felhőszolgáltatásban fut, majd azt fogja átirányítani Önt:</span><span class="sxs-lookup"><span data-stu-id="d393f-229">If it's running in the cloud service, then it will redirect to:</span></span>

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

<span data-ttu-id="d393f-230">A következő forrás URL-címet a CDN-végpontot, amely megfelel:</span><span class="sxs-lookup"><span data-stu-id="d393f-230">Which corresponds to the following origin URL at your CDN endpoint:</span></span>

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


<span data-ttu-id="d393f-231">Ezután a `OutputCacheAttribute` attribútuma a `Generate` adhatja meg, hogyan művelet eredménye gyorsítótárazza, amely az Azure CDN esetében figyelembe vesszük metódust.</span><span class="sxs-lookup"><span data-stu-id="d393f-231">You can then use the `OutputCacheAttribute` attribute on the `Generate` method to specify how the action result should be cached, which Azure CDN will honor.</span></span> <span data-ttu-id="d393f-232">Az alábbi kódot adja meg a gyorsítótár lejárati 1 óra (3600 másodperc).</span><span class="sxs-lookup"><span data-stu-id="d393f-232">The code below specify a cache expiration of 1 hour (3,600 seconds).</span></span>

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

<span data-ttu-id="d393f-233">Hasonlóképpen, szolgálhat bármely tartományvezérlő művelet a tartalmakat a felhőszolgáltatásban keresztül az Azure CDN és a kívánt gyorsítótár-beállítás.</span><span class="sxs-lookup"><span data-stu-id="d393f-233">Likewise, you can serve up content from any controller action in your cloud service through your Azure CDN, with the desired caching option.</span></span>

<span data-ttu-id="d393f-234">A következő szakaszban I bemutatja, hogyan csomagolt és minified parancsfájlok és Azure CDN keresztül CSS kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="d393f-234">In the next section, I will show you how to serve the bundled and minified scripts and CSS through Azure CDN.</span></span>

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a><span data-ttu-id="d393f-235">ASP.NET kötegelése és minification integrálása az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="d393f-235">Integrate ASP.NET bundling and minification with Azure CDN</span></span>
<span data-ttu-id="d393f-236">Parancsfájlok és CSS stíluslapok ritkán módosítása, és az Azure CDN gyorsítótár elsődleges vár.</span><span class="sxs-lookup"><span data-stu-id="d393f-236">Scripts and CSS stylesheets change infrequently and are prime candidates for the Azure CDN cache.</span></span> <span data-ttu-id="d393f-237">A teljes webes szerepkör keresztül az Azure CDN szolgáltató legkönnyebben kötegelése és minification integrálása az Azure CDN szolgáltatás használata.</span><span class="sxs-lookup"><span data-stu-id="d393f-237">Serving the entire Web role through your Azure CDN is the easiest way to integrate bundling and minification with Azure CDN.</span></span> <span data-ttu-id="d393f-238">Azonban előfordulhat, hogy nem kívánt ehhez, I bemutatja, hogyan be erre, miközben megőrzi, mint az ASP.NET kötegelése és minification, a kívánt develper élményt:</span><span class="sxs-lookup"><span data-stu-id="d393f-238">However, as you may not want to do this, I will show you how to do it while preserving the desired develper experience of ASP.NET bundling and minification, such as:</span></span>

* <span data-ttu-id="d393f-239">Nagyszerű hibakeresési mód élmény</span><span class="sxs-lookup"><span data-stu-id="d393f-239">Great debug mode experience</span></span>
* <span data-ttu-id="d393f-240">Egyszerű telepítés</span><span class="sxs-lookup"><span data-stu-id="d393f-240">Streamlined deployment</span></span>
* <span data-ttu-id="d393f-241">Az ügyfelek számára a parancsfájl/CSS verziófrissítések azonnali frissítést</span><span class="sxs-lookup"><span data-stu-id="d393f-241">Immediate updates to clients for script/CSS version upgrades</span></span>
* <span data-ttu-id="d393f-242">Ha a CDN-végpontot nem sikerült tartalék mechanizmus</span><span class="sxs-lookup"><span data-stu-id="d393f-242">Fallback mechanism when your CDN endpoint fails</span></span>
* <span data-ttu-id="d393f-243">Minimalizálása érdekében a kód módosítása</span><span class="sxs-lookup"><span data-stu-id="d393f-243">Minimize code modification</span></span>

<span data-ttu-id="d393f-244">Az a **WebRole1** létrehozott projekt [Azure CDN-végpont integrálása az Azure webhelyén, és a statikus tartalmat szolgáltat a weblapok Azure CDN](#deploy), nyissa meg *App_Start\BundleConfig.cs* és vessen egy pillantást a `bundles.Add()` metódushívások.</span><span class="sxs-lookup"><span data-stu-id="d393f-244">In the **WebRole1** project that you created in [Integrate an Azure CDN endpoint with your Azure website and serve static content in your Web pages from Azure CDN](#deploy), open *App_Start\BundleConfig.cs* and take a look at the `bundles.Add()` method calls.</span></span>

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

<span data-ttu-id="d393f-245">Az első `bundles.Add()` utasítás ad hozzá egy parancsfájl-csomagot a virtuális könyvtár `~/bundles/jquery`.</span><span class="sxs-lookup"><span data-stu-id="d393f-245">The first `bundles.Add()` statement adds a script bundle at the virtual directory `~/bundles/jquery`.</span></span> <span data-ttu-id="d393f-246">Ezután nyissa meg *Views\Shared\_Layout.cshtml* köteg parancsprogramcímkéből megjelenítése hogyan megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d393f-246">Then, open *Views\Shared\_Layout.cshtml* to see how the script bundle tag is rendered.</span></span> <span data-ttu-id="d393f-247">Nem található a következő kódsort Razor kell:</span><span class="sxs-lookup"><span data-stu-id="d393f-247">You should be able to find the following line of Razor code:</span></span>

    @Scripts.Render("~/bundles/jquery")

<span data-ttu-id="d393f-248">Futtatásakor a Razor kódot a Azure webes szerepkör, akkor jelenik meg egy `<script>` címke a parancsfájl köteg a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="d393f-248">When this Razor code is run in the Azure Web role, it will render a `<script>` tag for the script bundle similar to the following:</span></span>

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

<span data-ttu-id="d393f-249">Azonban amikor futtatja azt a Visual Studio beírásával `F5`, azt, ezzel veszélyezteti a kötegben minden parancsfájl külön-külön (a fenti esetben csak egy parancsprogram fájl a kötegben):</span><span class="sxs-lookup"><span data-stu-id="d393f-249">However, when it is run in Visual Studio by typing `F5`, it will render each script file in the bundle individually (in the case above, only one script file is in the bundle):</span></span>

    <script src="/Scripts/jquery-1.10.2.js"></script>

<span data-ttu-id="d393f-250">Ez lehetővé teszi, hogy a fejlesztési környezet JavaScript-kód hibakeresési, miközben csökkenti (kötegelése) egyidejű kapcsolatok és jobb lesz a fájl letöltése teljesítmény (minification) éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d393f-250">This enables you to debug the JavaScript code in your development environment while reducing concurrent client connections (bundling) and improving file download performance (minification) in production.</span></span> <span data-ttu-id="d393f-251">Már egy nagy funkció az Azure CDN integráció megőrzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="d393f-251">It's a great feature to preserve with Azure CDN integration.</span></span> <span data-ttu-id="d393f-252">Továbbá, mivel a megjelenített csomagot már tartalmaz egy automatikusan létrehozott verzió-karakterláncnak, replikálni kívánt funkció, a Nugeten keresztül jQuery verziójára frissít, amikor frissíthető az ügyfél oldalán amint lehetséges.</span><span class="sxs-lookup"><span data-stu-id="d393f-252">Furthermore, since the rendered bundle already contains an automatically generated version string, you want to replicate that functionality so the whenever you update your jQuery version through NuGet, it can be updated at the client side as soon as possible.</span></span>

<span data-ttu-id="d393f-253">Az alábbi lépéseket követve integrációs ASP.NET kötegelése és minification a CDN-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="d393f-253">Follow the steps below to integration ASP.NET bundling and minification with your CDN endpoint.</span></span>

1. <span data-ttu-id="d393f-254">Vissza a *App_Start\BundleConfig.cs*, módosítsa a `bundles.Add()` egy másik használandó módszerek [köteg konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), egy, a CDN-címet adja meg.</span><span class="sxs-lookup"><span data-stu-id="d393f-254">Back in *App_Start\BundleConfig.cs*, modify the `bundles.Add()` methods to use a different [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), one that specifies a CDN address.</span></span> <span data-ttu-id="d393f-255">Ehhez cserélje le a `RegisterBundles` metódus definícióját a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="d393f-255">To do this, replace the `RegisterBundles` method definition with the following code:</span></span>  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="d393f-256">Ügyeljen arra, hogy a csere `<yourCDNName>` az Azure CDN nevével.</span><span class="sxs-lookup"><span data-stu-id="d393f-256">Be sure to replace `<yourCDNName>` with the name of your Azure CDN.</span></span>
   
    <span data-ttu-id="d393f-257">Egyszerű szavakkal állítja `bundles.UseCdn = true` és gondosan kialakított CDN URL-cím hozzá minden egyes csomagot.</span><span class="sxs-lookup"><span data-stu-id="d393f-257">In plain words, you are setting `bundles.UseCdn = true` and added a carefully crafted CDN URL to each bundle.</span></span> <span data-ttu-id="d393f-258">Ha például az első konstruktort a kódban:</span><span class="sxs-lookup"><span data-stu-id="d393f-258">For example, the first constructor in the code:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    <span data-ttu-id="d393f-259">ugyanaz, mint a következő:</span><span class="sxs-lookup"><span data-stu-id="d393f-259">is the same as:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    <span data-ttu-id="d393f-260">Ez a konstruktor közli az ASP.NET kötegelése és minification leképezése egyéni parancsfájlok, amikor helyileg indítja, de a megadott CDN-cím a szóban forgó parancsfájl eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d393f-260">This constructor tells ASP.NET bundling and minification to render individual script files when debugged locally, but use the specified CDN address to access the script in question.</span></span> <span data-ttu-id="d393f-261">Azonban ügyeljen arra, hogy alaposan kialakított CDN URL-címet a két fontos jellemzők:</span><span class="sxs-lookup"><span data-stu-id="d393f-261">However, note two important characteristics with this carefully crafted CDN URL:</span></span>
   
   * <span data-ttu-id="d393f-262">A CDN URL-címhez forrása `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, vagyis a parancsfájl-csomagot a felhőszolgáltatásban valójában a virtuális könyvtár.</span><span class="sxs-lookup"><span data-stu-id="d393f-262">The origin for this CDN URL is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, which is actually the virtual directory of the script bundle in your cloud service.</span></span>
   * <span data-ttu-id="d393f-263">Mivel a CDN konstruktor használ, a CDN script kód a köteg már nem tartalmaz a megjelenített URL-cím automatikusan létrehozott verzió-karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="d393f-263">Since you are using CDN constructor, the CDN script tag for the bundle no longer contains the automatically generated version string in the rendered URL.</span></span> <span data-ttu-id="d393f-264">Manuálisan létre kell hoznia egy egyedi verzió-karakterláncot, minden alkalommal, amikor a parancsfájl-csomag módosítása egy gyorsítótár-tévesztései, az Azure CDN kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="d393f-264">You must manually generate a unique version string every time the script bundle is modified to force a cache miss at your Azure CDN.</span></span> <span data-ttu-id="d393f-265">Egy időben a egyedi verzió-karakterlánca állandónak kell találatot eredményező gyorsítótárbeli kereséseinek, az Azure CDN maximalizálása a csomag telepítése után a központi telepítés élettartama keresztül.</span><span class="sxs-lookup"><span data-stu-id="d393f-265">At the same time, this unique version string must remain constant through the life of the deployment to maximize cache hits at your Azure CDN after the bundle is deployed.</span></span>
   * <span data-ttu-id="d393f-266">A lekérdezési karakterlánc v = < W.X.Y.Z > ponttá a *Properties\AssemblyInfo.cs* a webes szerepkör projekt.</span><span class="sxs-lookup"><span data-stu-id="d393f-266">The query string v=<W.X.Y.Z> pulls from *Properties\AssemblyInfo.cs* in your Web role project.</span></span> <span data-ttu-id="d393f-267">Akkor is, amely tartalmazza a növekvő a szerelvény verziója, minden alkalommal, amikor Azure közzéteszi a telepítési munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="d393f-267">You can have a deployment workflow that includes incrementing the assembly version every time you publish to Azure.</span></span> <span data-ttu-id="d393f-268">Vagy csak módosíthatja *Properties\AssemblyInfo.cs* a projektet a verzió-karakterlánca automatikusan növekvő, minden alkalommal, amikor a build, használhat helyettesítő karaktereket a "*".</span><span class="sxs-lookup"><span data-stu-id="d393f-268">Or, you can just modify *Properties\AssemblyInfo.cs* in your project to automatically increment the version string every time you build, using the wildcard character '*'.</span></span> <span data-ttu-id="d393f-269">Példa:</span><span class="sxs-lookup"><span data-stu-id="d393f-269">For example:</span></span>
     
        <span data-ttu-id="d393f-270">[szerelvény: AssemblyVersion("1.0.0.*")]</span><span class="sxs-lookup"><span data-stu-id="d393f-270">[assembly: AssemblyVersion("1.0.0.*")]</span></span>
     
     <span data-ttu-id="d393f-271">Bármely más stratégia létrehozásakor a központi telepítés során egy egyedi karakterlánc egyszerűsítésére itt fog működni.</span><span class="sxs-lookup"><span data-stu-id="d393f-271">Any other strategy to streamline generating a unique string for the life of a deployment will work here.</span></span>
2. <span data-ttu-id="d393f-272">Ismételt közzététele a felhőalapú szolgáltatás, és elérheti a kezdőlap.</span><span class="sxs-lookup"><span data-stu-id="d393f-272">Republish the cloud service and access the home page.</span></span>
3. <span data-ttu-id="d393f-273">Tekintse meg a lap HTML-kódja.</span><span class="sxs-lookup"><span data-stu-id="d393f-273">View the HTML code for the page.</span></span> <span data-ttu-id="d393f-274">Meg kell látni a CDN URL-cím, egy egyedi verzió-karakterláncot, minden alkalommal, amikor módosításokat ismételt közzététele a felhőalapú szolgáltatáshoz együtt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d393f-274">You should be able to see the CDN URL rendered, with a unique version string every time you republish changes to your cloud service.</span></span> <span data-ttu-id="d393f-275">Példa:</span><span class="sxs-lookup"><span data-stu-id="d393f-275">For example:</span></span>  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. <span data-ttu-id="d393f-276">A Visual Studio hibakeresése a Visual Studio felhőszolgáltatás beírásával `F5`.,</span><span class="sxs-lookup"><span data-stu-id="d393f-276">In Visual Studio, debug the cloud service in Visual Studio by typing `F5`.,</span></span>
5. <span data-ttu-id="d393f-277">Tekintse meg a lap HTML-kódja.</span><span class="sxs-lookup"><span data-stu-id="d393f-277">View the HTML code for the page.</span></span> <span data-ttu-id="d393f-278">Minden egyes külön-külön megjelenítve, hogy akkor is egy egységes élmény a Visual Studio hibakeresési parancsfájl továbbra is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d393f-278">You will still see each script file individually rendered so that you can have a consistent debug experience in Visual Studio.</span></span>  
   
        ...
   
            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>
   
            <script src="/Scripts/modernizr-2.6.2.js"></script>
   
        ...
   
            <script src="/Scripts/jquery-1.10.2.js"></script>
   
            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>
   
        ...   

<a name="fallback"></a>

## <a name="fallback-mechanism-for-cdn-urls"></a><span data-ttu-id="d393f-279">Tartalék mechanizmus CDN URL-címek esetén</span><span class="sxs-lookup"><span data-stu-id="d393f-279">Fallback mechanism for CDN URLs</span></span>
<span data-ttu-id="d393f-280">Azure CDN-végpont bármilyen okból nem sikerül, ha azt szeretné, a weblap kell intelligens ahhoz a forrás webkiszolgálóhoz hozzáférni a megoldásként JavaScript vagy rendszerindítási feltöltését.</span><span class="sxs-lookup"><span data-stu-id="d393f-280">When your Azure CDN endpoint fails for any reason, you want your Web page to be smart enough to access your origin Web server as the fallback option for loading JavaScript or Bootstrap.</span></span> <span data-ttu-id="d393f-281">Súlyos, hogy elveszíti a képek a webhelyen, de sokkal több súlyos stíluslapok és parancsfájlok által biztosított kritikus fontosságú lap funkciókat elveszíti a CDN elérhetetlensége miatt.</span><span class="sxs-lookup"><span data-stu-id="d393f-281">It's serious enough to lose images on your website due to CDN unavailability, but much more severe to lose crucial page functionality provided by your scripts and stylesheets.</span></span>

<span data-ttu-id="d393f-282">A [köteg](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) osztály tartalmazza a tulajdonságot, [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , amely lehetővé teszi, hogy a tartalék mechanizmus a CDN hiba konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d393f-282">The [Bundle](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) class contains a property called [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) that enables you to configure the fallback mechanism for CDN failure.</span></span> <span data-ttu-id="d393f-283">Ez a tulajdonság használatához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d393f-283">To use this property, follow the steps below:</span></span>

1. <span data-ttu-id="d393f-284">Nyissa meg a webes szerepkör projekt *App_Start\BundleConfig.cs*, amelyikhez hozzáadta a CDN URL-cím az egyes [köteg konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), és a következő módosításokat kiemelt tartalék mechanizmus hozzáadása az alapértelmezett csomagokat:</span><span class="sxs-lookup"><span data-stu-id="d393f-284">In your Web role project, open *App_Start\BundleConfig.cs*, where you added a CDN URL in each [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), and make the following highlighted changes to add fallback mechanism to the default bundles:</span></span>  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                  "~/Scripts/bootstrap.js",
                                  "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="d393f-285">Ha `CdnFallbackExpression` értéke nem null értékű, parancsfájl van be a nézetmodellbe, a HTML-e a csomag sikeresen betöltődött, és ha nem, a csomag közvetlenül elérje a származási webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d393f-285">When `CdnFallbackExpression` is not null, script is injected into the HTML to test whether the bundle is loaded successfully and, if not, access the bundle directly from the origin Web server.</span></span> <span data-ttu-id="d393f-286">Ez a tulajdonság kell állítani a JavaScript kifejezéshez teszteli, hogy a megfelelő CDN csomagot megfelelően be van töltve.</span><span class="sxs-lookup"><span data-stu-id="d393f-286">This property needs to be set to a JavaScript expression that tests whether the respective CDN bundle is loaded properly.</span></span> <span data-ttu-id="d393f-287">A kifejezés teszteléséhez minden csomag szükséges a tartalom szerint változik.</span><span class="sxs-lookup"><span data-stu-id="d393f-287">The expression needed to test each bundle differs according to the content.</span></span> <span data-ttu-id="d393f-288">A fenti alapértelmezett csomagokat:</span><span class="sxs-lookup"><span data-stu-id="d393f-288">For the default bundles above:</span></span>
   
   * <span data-ttu-id="d393f-289">`window.jquery`jquery-{version} .js definiálva van</span><span class="sxs-lookup"><span data-stu-id="d393f-289">`window.jquery` is defined in jquery-{version}.js</span></span>
   * <span data-ttu-id="d393f-290">`$.validator`jquery.validate.js definiálva van</span><span class="sxs-lookup"><span data-stu-id="d393f-290">`$.validator` is defined in jquery.validate.js</span></span>
   * <span data-ttu-id="d393f-291">`window.Modernizr`modernizer-{version} .js definiálva van</span><span class="sxs-lookup"><span data-stu-id="d393f-291">`window.Modernizr` is defined in modernizer-{version}.js</span></span>
   * <span data-ttu-id="d393f-292">`$.fn.modal`bootstrap.js definiálva van</span><span class="sxs-lookup"><span data-stu-id="d393f-292">`$.fn.modal` is defined in bootstrap.js</span></span>
     
     <span data-ttu-id="d393f-293">Észrevette, hogy, hogy adott nem állítható be a CdnFallbackExpression a `~/Cointent/css` csomagot.</span><span class="sxs-lookup"><span data-stu-id="d393f-293">You might have noticed that I did not set CdnFallbackExpression for the `~/Cointent/css` bundle.</span></span> <span data-ttu-id="d393f-294">Ez azért, mert jelenleg nincs a [System.Web.Optimization hiba](https://aspnetoptimization.codeplex.com/workitem/104) , amelyek esetében a `<script>` helyett a várt tartalék CSS címkéjének `<link>` címke.</span><span class="sxs-lookup"><span data-stu-id="d393f-294">This is because currently there is a [bug in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) that injects a `<script>` tag for the fallback CSS instead of the expected `<link>` tag.</span></span>
     
     <span data-ttu-id="d393f-295">Nincs, de jó [stílus köteg tartalék](https://github.com/EmberConsultingGroup/StyleBundleFallback) által kínált [decemberéig tanácsadás csoport](https://github.com/EmberConsultingGroup).</span><span class="sxs-lookup"><span data-stu-id="d393f-295">There is, however, a good [Style Bundle Fallback](https://github.com/EmberConsultingGroup/StyleBundleFallback) offered by [Ember Consulting Group](https://github.com/EmberConsultingGroup).</span></span>
2. <span data-ttu-id="d393f-296">CSS a megoldás használatához hozzon létre egy új .cs fájlt a webes szerepkör projekt *App_Start* nevű mappát *StyleBundleExtensions.cs*, és cserélje le a tartalmat a [kód a Githubról](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="d393f-296">To use the workaround for CSS, create a new .cs file in your Web role project's *App_Start* folder called *StyleBundleExtensions.cs*, and replace its content with the [code from GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span></span>
3. <span data-ttu-id="d393f-297">A *App_Start\StyleFundleExtensions.cs*, nevezze át névtere a webes szerepkör nevét (pl. **WebRole1**).</span><span class="sxs-lookup"><span data-stu-id="d393f-297">In *App_Start\StyleFundleExtensions.cs*, rename the namespace to your Web role's name (e.g. **WebRole1**).</span></span>
4. <span data-ttu-id="d393f-298">Lépjen vissza a `App_Start\BundleConfig.cs` , és módosítsa az utolsó `bundles.Add` kiemelt kódot a következő utasítást:</span><span class="sxs-lookup"><span data-stu-id="d393f-298">Go back to `App_Start\BundleConfig.cs` and modify the last `bundles.Add` statement with the following highlighted code:</span></span>  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    <span data-ttu-id="d393f-299">A bővítmény metódus használja ugyanazt a parancsfájl a HTML-KÓDBAN a DOM ellenőrzéséhez az egyező osztály neve, a szabály nevét és a CSS-csomagot, és visszatér az eredeti webkiszolgáló meg, ha az nem található egyezés a szabály értéke.</span><span class="sxs-lookup"><span data-stu-id="d393f-299">This new extension method uses the same idea to inject script in the HTML to check the DOM for the a matching class name, rule name, and rule value defined in the CSS bundle, and falls back to the origin Web server if it fails to find the match.</span></span>
5. <span data-ttu-id="d393f-300">Tegye közzé újra a felhőalapú szolgáltatás, és a kezdőlap elérése.</span><span class="sxs-lookup"><span data-stu-id="d393f-300">Publish the cloud service again and access the home page.</span></span>
6. <span data-ttu-id="d393f-301">Tekintse meg a lap HTML-kódja.</span><span class="sxs-lookup"><span data-stu-id="d393f-301">View the HTML code for the page.</span></span> <span data-ttu-id="d393f-302">Keresse meg injected parancsfájlok a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="d393f-302">You should find injected scripts similar to the following:</span></span>    
   
        ...
   
        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
   
        ...
   
            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
   
        ...

    <span data-ttu-id="d393f-303">Vegye figyelembe, hogy a CSS-köteg injected parancsfájl továbbra is tartalmaz a errant tartós a `CdnFallbackExpression` tulajdonság sorában:</span><span class="sxs-lookup"><span data-stu-id="d393f-303">Note that injected script for the CSS bundle still contains the errant remnant from the `CdnFallbackExpression` property in the line:</span></span>

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    <span data-ttu-id="d393f-304">De óta első része a || kifejezés mindig ad vissza IGAZ (a sorban fölött, amely), a document.write() függvény soha nem fog futni.</span><span class="sxs-lookup"><span data-stu-id="d393f-304">But since the first part of the || expression will always return true (in the line directly above that), the document.write() function will never run.</span></span>

## <a name="more-information"></a><span data-ttu-id="d393f-305">További információ</span><span class="sxs-lookup"><span data-stu-id="d393f-305">More Information</span></span>
* [<span data-ttu-id="d393f-306">Az Azure Content Delivery Network (CDN) áttekintése</span><span class="sxs-lookup"><span data-stu-id="d393f-306">Overview of the Azure Content Delivery Network (CDN)</span></span>](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [<span data-ttu-id="d393f-307">Az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="d393f-307">Using Azure CDN</span></span>](cdn-create-new-endpoint.md)
* [<span data-ttu-id="d393f-308">ASP.NET kötegelése és Minification</span><span class="sxs-lookup"><span data-stu-id="d393f-308">ASP.NET Bundling and Minification</span></span>](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
