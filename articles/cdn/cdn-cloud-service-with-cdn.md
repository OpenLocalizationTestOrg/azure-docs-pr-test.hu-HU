---
title: "egy Azure felhőszolgáltatást Azure CDN aaaIntegrate |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy egy felhőalapú szolgáltatás, amely tartalmat szolgáltat az integrált Azure CDN-végpont"
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
ms.openlocfilehash: f20d60b0b5edc133adf06d010633a15f62e2b8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="5b851-103"><a name="intro"></a>Egy felhőalapú szolgáltatás integrálása az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="5b851-103"><a name="intro"></a> Integrate a cloud service with Azure CDN</span></span>
<span data-ttu-id="5b851-104">Egy felhőalapú szolgáltatás Azure CDN minden tartalom hello felhőalapú szolgáltatás helyről integrálható.</span><span class="sxs-lookup"><span data-stu-id="5b851-104">A cloud service can be integrated with Azure CDN, serving any content from hello cloud service's location.</span></span> <span data-ttu-id="5b851-105">A módszer ad meg a következő előnyök hello:</span><span class="sxs-lookup"><span data-stu-id="5b851-105">This approach gives you hello following advantages:</span></span>

* <span data-ttu-id="5b851-106">Egyszerűen regisztrálhat és lemezképek, parancsprogramok és a felhőszolgáltatás-projekt címtárakban stíluslapok frissítése</span><span class="sxs-lookup"><span data-stu-id="5b851-106">Easily deploy and update images, scripts, and stylesheets in your cloud service's project directories</span></span>
* <span data-ttu-id="5b851-107">Könnyedén frissíthet a felhőalapú szolgáltatás, például a jQuery vagy rendszerindítási verzió a hello NuGet-csomagok</span><span class="sxs-lookup"><span data-stu-id="5b851-107">Easily upgrade hello NuGet packages in your cloud service, such as jQuery or Bootstrap versions</span></span>
* <span data-ttu-id="5b851-108">A webes alkalmazás kezelése és a CDN-kiszolgált tartalmat összes hello ugyanaz a Visual Studio felület</span><span class="sxs-lookup"><span data-stu-id="5b851-108">Manage your Web application and your CDN-served content all from hello same Visual Studio interface</span></span>
* <span data-ttu-id="5b851-109">A webes alkalmazás és a CDN-kiszolgált tartalom egyesített telepítési munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="5b851-109">Unified deployment workflow for your Web application and your CDN-served content</span></span>
* <span data-ttu-id="5b851-110">ASP.NET kötegelése és minification integrálása az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="5b851-110">Integrate ASP.NET bundling and minification with Azure CDN</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5b851-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="5b851-111">What you will learn</span></span>
<span data-ttu-id="5b851-112">Ebből az oktatóanyagból megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="5b851-112">In this tutorial, you will learn how to:</span></span>

* [<span data-ttu-id="5b851-113">Az Azure CDN-végpont integrálható a felhőalapú szolgáltatás, és a statikus tartalmat szolgáltat a weblapok Azure CDN</span><span class="sxs-lookup"><span data-stu-id="5b851-113">Integrate an Azure CDN endpoint with your cloud service and serve static content in your Web pages from Azure CDN</span></span>](#deploy)
* [<span data-ttu-id="5b851-114">Adja meg a statikus tartalom gyorsítótár beállításait a felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5b851-114">Configure cache settings for static content in your cloud service</span></span>](#caching)
* [<span data-ttu-id="5b851-115">Az Azure CDN keresztül vezérlő műveletekből tartalmat</span><span class="sxs-lookup"><span data-stu-id="5b851-115">Serve content from controller actions through Azure CDN</span></span>](#controller)
* [<span data-ttu-id="5b851-116">Szolgálnak kötegelve, és Azure CDN tartalom minified hello parancsprogram-hibakeresés a Visual Studio élmény megőrzésével</span><span class="sxs-lookup"><span data-stu-id="5b851-116">Serve bundled and minified content through Azure CDN while preserving hello script debugging experience in Visual Studio</span></span>](#bundling)
* [<span data-ttu-id="5b851-117">Konfigurálhatja tartalék CSS és parancsfájlok, amikor az Azure CDN offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="5b851-117">Configure fallback your scripts and CSS when your Azure CDN is offline</span></span>](#fallback)

## <a name="what-you-will-build"></a><span data-ttu-id="5b851-118">Mit fog létrehozni</span><span class="sxs-lookup"><span data-stu-id="5b851-118">What you will build</span></span>
<span data-ttu-id="5b851-119">Fogja telepíteni egy felhőalapú szolgáltatás webes szerepkör hello alapértelmezett ASP.NET MVC-sablont használ, kép, a tartományvezérlő műveleti eredmény és hello alapértelmezett JavaScript és CSS fájlok, például egy integrált Azure CDN kód tooserve tartalom hozzáadása és is írhatnak a kód tooconfigure hello tartalék mechanizmus hello eseményben kiszolgált adott CDN hello csomagok offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="5b851-119">You will deploy a cloud service Web role using hello default ASP.NET MVC template, add code tooserve content from an integrated Azure CDN, such as an image, controller action results, and hello default JavaScript and CSS files, and also write code tooconfigure hello fallback mechanism for bundles served in hello event that hello CDN is offline.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="5b851-120">Mit kell</span><span class="sxs-lookup"><span data-stu-id="5b851-120">What you will need</span></span>
<span data-ttu-id="5b851-121">Ez az oktatóanyag a következő előfeltételek hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="5b851-121">This tutorial has hello following prerequisites:</span></span>

* <span data-ttu-id="5b851-122">Az aktív [Microsoft Azure-fiók](/account/)</span><span class="sxs-lookup"><span data-stu-id="5b851-122">An active [Microsoft Azure account](/account/)</span></span>
* <span data-ttu-id="5b851-123">A Visual Studio 2015 [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="5b851-123">Visual Studio 2015 with [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)</span></span>

> [!NOTE]
> <span data-ttu-id="5b851-124">Ez az oktatóanyag kell egy Azure-fiók toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5b851-124">You need an Azure account toocomplete this tutorial:</span></span>
> 
> * <span data-ttu-id="5b851-125">Is [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) -jóváírásokat kap használhatja ki tootry fizetős Azure-szolgáltatásokat, és még azok lejárta után is megtarthatja hello fiókot és használhatja az ingyenes Azure-szolgáltatások, például webhelyekhez.</span><span class="sxs-lookup"><span data-stu-id="5b851-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use tootry out paid Azure services, and even after they're used up you can keep hello account and use free Azure services, such as Websites.</span></span>
> * <span data-ttu-id="5b851-126">Is [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -az MSDN-előfizetés biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="5b851-126">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a><span data-ttu-id="5b851-127">A felhőalapú szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="5b851-127">Deploy a cloud service</span></span>
<span data-ttu-id="5b851-128">Ebben a szakaszban fogja telepíteni hello alapértelmezett ASP.NET MVC alkalmazássablon a Visual Studio 2015 tooa felhőalapú szolgáltatás webes szerepkör, és integrálhatja egy új CDN-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="5b851-128">In this section, you will deploy hello default ASP.NET MVC application template in Visual Studio 2015 tooa cloud service Web role, and then integrate it with a new CDN endpoint.</span></span> <span data-ttu-id="5b851-129">Kövesse az alábbi hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="5b851-129">Follow hello instructions below:</span></span>

1. <span data-ttu-id="5b851-130">A Visual Studio 2015, hozzon létre egy új Azure-felhőszolgáltatásban hello menüsorában túl címen**fájl > Új > Projekt > felhő > Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="5b851-130">In Visual Studio 2015, create a new Azure cloud service from hello menu bar by going too**File > New > Project > Cloud > Azure Cloud Service**.</span></span> <span data-ttu-id="5b851-131">Adjon neki egy nevet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b851-131">Give it a name and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. <span data-ttu-id="5b851-132">Válassza ki **ASP.NET webes szerepkör** hello kattintson  **>**  gombra.</span><span class="sxs-lookup"><span data-stu-id="5b851-132">Select **ASP.NET Web Role** and click hello **>** button.</span></span> <span data-ttu-id="5b851-133">Kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="5b851-133">Click OK.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. <span data-ttu-id="5b851-134">Válassza ki **MVC** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b851-134">Select **MVC** and click **OK**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. <span data-ttu-id="5b851-135">Most tegye közzé a webes szerepkör tooan Azure felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5b851-135">Now, publish this Web role tooan Azure cloud service.</span></span> <span data-ttu-id="5b851-136">Kattintson a jobb gombbal a hello felhőszolgáltatás-projekt, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="5b851-136">Right-click hello cloud service project and select **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. <span data-ttu-id="5b851-137">Ha még nem írta be a Microsoft Azure, kattintson a hello **fiók hozzáadása...**  legördülő menüből, majd kattintson a hello **vegyen fel egy fiókot** menüpont.</span><span class="sxs-lookup"><span data-stu-id="5b851-137">If you have not yet signed into Microsoft Azure, click hello **Add an account...** dropdown and click hello **Add an account** menu item.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. <span data-ttu-id="5b851-138">Hello bejelentkezési oldal, a bejelentkezés hello tooactivate az Azure-fiókjával használt Microsoft-fiók.</span><span class="sxs-lookup"><span data-stu-id="5b851-138">In hello sign-in page, sign in with hello Microsoft account you used tooactivate your Azure account.</span></span>
7. <span data-ttu-id="5b851-139">Ha be van jelentkezve, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5b851-139">Once you're signed in, click **Next**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. <span data-ttu-id="5b851-140">Feltételezve, hogy egy felhőalapú szolgáltatás, vagy tárolási fiókja még nem hozott létre, a Visual Studio segítségével hozhat létre is.</span><span class="sxs-lookup"><span data-stu-id="5b851-140">Assuming that you haven't created a cloud service or storage account, Visual Studio will help you create both.</span></span> <span data-ttu-id="5b851-141">A hello **felhőalapú szolgáltatás létrehozása és a fiók** párbeszédpanel, hello kívánt szolgáltatás nevét, és jelölje be hello kívánt régiót.</span><span class="sxs-lookup"><span data-stu-id="5b851-141">In hello **Create Cloud Service and Account** dialog, type hello desired service name and select hello desired region.</span></span> <span data-ttu-id="5b851-142">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5b851-142">Then, click **Create**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. <span data-ttu-id="5b851-143">Hello a beállítások lap közzététele, ellenőrizze hello konfigurációját, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="5b851-143">In hello publish settings page, verify hello configuration and click **Publish**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > <span data-ttu-id="5b851-144">a felhőszolgáltatások hello a közzétételi folyamat hosszú ideig tart.</span><span class="sxs-lookup"><span data-stu-id="5b851-144">hello publishing process for cloud services takes a long time.</span></span> <span data-ttu-id="5b851-145">minden szerepkörök beállítás engedélyezéséhez a Web Deploy hello teheti a felhőalapú szolgáltatás sokkal gyorsabb, adja meg a webes szerepkörök gyors (de ideiglenes) frissítések tooyour hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="5b851-145">hello Enable Web Deploy for all roles option can make debugging your cloud service much quicker by providing fast (but temporary) updates tooyour Web roles.</span></span> <span data-ttu-id="5b851-146">Ez a beállítás további információkért lásd: [közzététele a felhőalapú szolgáltatás hello Azure eszközökkel](http://msdn.microsoft.com/library/ff683672.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b851-146">For more information on this option, see [Publishing a Cloud Service using hello Azure Tools](http://msdn.microsoft.com/library/ff683672.aspx).</span></span>
   > 
   > 
   
    <span data-ttu-id="5b851-147">Ha hello **Microsoft Azure tevékenységnapló** mutatja, hogy közzétételi állapotát **befejezve**, létrehozhat egy CDN-végpontot, amely integrálva van a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5b851-147">When hello **Microsoft Azure Activity Log** shows that publishing status is **Completed**, you will create a CDN endpoint that's integrated with this cloud service.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="5b851-148">Ha a közzététel után, a telepített hello felhőalapú szolgáltatás egy hiba képernyő jelenik meg, valószínű, mert hello felhőalapú szolgáltatás telepítése után használja a [vendég operációs rendszer, amely nem tartalmazza a .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span><span class="sxs-lookup"><span data-stu-id="5b851-148">If, after publishing, hello deployed cloud service displays an error screen, it's likely because hello cloud service you've deployed is using a [guest OS that does not include .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).</span></span>  <span data-ttu-id="5b851-149">Által a probléma megkerüléséhez [központi telepítése a .NET 4.5.2 az indítási feladatok](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="5b851-149">You can work around this issue by [deploying .NET 4.5.2 as a startup task](../cloud-services/cloud-services-dotnet-install-dotnet.md).</span></span>
   > 
   > 

## <a name="create-a-new-cdn-profile"></a><span data-ttu-id="5b851-150">Új CDN-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b851-150">Create a new CDN profile</span></span>
<span data-ttu-id="5b851-151">A CDN-profil CDN-végpontok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="5b851-151">A CDN profile is a collection of CDN endpoints.</span></span>  <span data-ttu-id="5b851-152">Minden profil egy vagy több CDN-végpontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5b851-152">Each profile contains one or more CDN endpoints.</span></span>  <span data-ttu-id="5b851-153">Kezdésként érdemes lehet toouse több profilok tooorganize a CDN-végpontokat internetes tartomány, webalkalmazás vagy más feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="5b851-153">You may wish toouse multiple profiles tooorganize your CDN endpoints by internet domain, web application, or some other criteria.</span></span>

> [!TIP]
> <span data-ttu-id="5b851-154">Ha már rendelkezik, amelyet az toouse ebben az oktatóanyagban a CDN-profil, a folytatáshoz túl[hozzon létre egy új CDN-végpont](#create-a-new-cdn-endpoint).</span><span class="sxs-lookup"><span data-stu-id="5b851-154">If you already have a CDN profile that you want toouse for this tutorial, proceed too[Create a new CDN endpoint](#create-a-new-cdn-endpoint).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a><span data-ttu-id="5b851-155">Új CDN-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b851-155">Create a new CDN endpoint</span></span>
<span data-ttu-id="5b851-156">**új CDN-végpont a tárfiók toocreate**</span><span class="sxs-lookup"><span data-stu-id="5b851-156">**toocreate a new CDN endpoint for your storage account**</span></span>

1. <span data-ttu-id="5b851-157">A hello [Azure felügyeleti portálon](https://portal.azure.com), keresse meg a tooyour CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="5b851-157">In hello [Azure Management Portal](https://portal.azure.com), navigate tooyour CDN profile.</span></span>  <span data-ttu-id="5b851-158">Előfordulhat, hogy rendelkezik rögzítette azt toohello irányítópult hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="5b851-158">You may have pinned it toohello dashboard in hello previous step.</span></span>  <span data-ttu-id="5b851-159">Ha nem, akkor megtalálja kattintva **Tallózás**, majd **CDN-profilra**, és kattintson a hello-profil tervezi tooadd a végponthoz.</span><span class="sxs-lookup"><span data-stu-id="5b851-159">If you not, you can find it by clicking **Browse**, then **CDN profiles**, and clicking on hello profile you plan tooadd your endpoint to.</span></span>
   
    <span data-ttu-id="5b851-160">CDN-profil panelje hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5b851-160">hello CDN profile blade appears.</span></span>
   
    ![CDN-profil][cdn-profile-settings]
2. <span data-ttu-id="5b851-162">Kattintson a hello **végpont hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="5b851-162">Click hello **Add Endpoint** button.</span></span>
   
    ![Végpont hozzáadása gomb][cdn-new-endpoint-button]
   
    <span data-ttu-id="5b851-164">Hello **végpont hozzáadása** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5b851-164">hello **Add an endpoint** blade appears.</span></span>
   
    ![Végpont hozzáadása panel][cdn-add-endpoint]
3. <span data-ttu-id="5b851-166">Adja meg a CDN-végpont kívánt nevét a **Név** mezőben.</span><span class="sxs-lookup"><span data-stu-id="5b851-166">Enter a **Name** for this CDN endpoint.</span></span>  <span data-ttu-id="5b851-167">Ez a név a gyorsítótárazott erőforrások hello tartomány lesz használt tooaccess `<EndpointName>.azureedge.net`.</span><span class="sxs-lookup"><span data-stu-id="5b851-167">This name will be used tooaccess your cached resources at hello domain `<EndpointName>.azureedge.net`.</span></span>
4. <span data-ttu-id="5b851-168">A hello **forrása típusa** legördülő menüből válassza *felhőalapú szolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="5b851-168">In hello **Origin type** dropdown, select *Cloud service*.</span></span>  
5. <span data-ttu-id="5b851-169">A hello **forrásállomásnév** legördülő menüben válassza ki a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5b851-169">In hello **Origin hostname** dropdown, select your cloud service.</span></span>
6. <span data-ttu-id="5b851-170">Hagyja meg az alapértelmezett hello beállításait **forrás elérési útvonalának**, **forrás állomásfejlécét**, és **protokoll/forrásport**.</span><span class="sxs-lookup"><span data-stu-id="5b851-170">Leave hello defaults for **Origin path**, **Origin host header**, and **Protocol/Origin port**.</span></span>  <span data-ttu-id="5b851-171">Meg kell adnia legalább egy protokollt (HTTP vagy HTTPS).</span><span class="sxs-lookup"><span data-stu-id="5b851-171">You must specify at least one protocol (HTTP or HTTPS).</span></span>
7. <span data-ttu-id="5b851-172">Kattintson a hello **Hozzáadás** gomb toocreate hello új végpont.</span><span class="sxs-lookup"><span data-stu-id="5b851-172">Click hello **Add** button toocreate hello new endpoint.</span></span>
8. <span data-ttu-id="5b851-173">Hello végpont létrehozását követően hello-profil végpontjainak listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5b851-173">Once hello endpoint is created, it appears in a list of endpoints for hello profile.</span></span> <span data-ttu-id="5b851-174">hello listanézetben látható, hello URL-cím toouse tooaccess gyorsítótárazott tartalmat, valamint hello forrástartományt.</span><span class="sxs-lookup"><span data-stu-id="5b851-174">hello list view shows hello URL toouse tooaccess cached content, as well as hello origin domain.</span></span>
   
    ![CDN-végpont][cdn-endpoint-success]
   
   > [!NOTE]
   > <span data-ttu-id="5b851-176">hello végpont nem azonnal lesz használható.</span><span class="sxs-lookup"><span data-stu-id="5b851-176">hello endpoint will not immediately be available for use.</span></span>  <span data-ttu-id="5b851-177">Hello regisztrációs toopropagate hello CDN a hálózaton keresztül too90 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="5b851-177">It can take up too90 minutes for hello registration toopropagate through hello CDN network.</span></span> <span data-ttu-id="5b851-178">Felhasználók, akik toouse hello CDN-tartománynevet azonnal próbálkozzon előfordulhat, hogy fogadni a 404 állapotkódot, amíg hello tartalom hello CDN keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="5b851-178">Users who try toouse hello CDN domain name immediately may receive status code 404 until hello content is available via hello CDN.</span></span>
   > 
   > 

## <a name="test-hello-cdn-endpoint"></a><span data-ttu-id="5b851-179">Teszt hello CDN-végpont</span><span class="sxs-lookup"><span data-stu-id="5b851-179">Test hello CDN endpoint</span></span>
<span data-ttu-id="5b851-180">Közzétételi állapot hello esetén **befejezve**, nyisson meg egy böngészőablakot, és keresse meg a túl**http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span><span class="sxs-lookup"><span data-stu-id="5b851-180">When hello publishing status is **Completed**, open a browser window and navigate too**http://<cdnName>*.azureedge.net/Content/bootstrap.css**.</span></span> <span data-ttu-id="5b851-181">A beállítás az URL-cím van:</span><span class="sxs-lookup"><span data-stu-id="5b851-181">In my setup, this URL is:</span></span>

    http://camservice.azureedge.net/Content/bootstrap.css

<span data-ttu-id="5b851-182">Amely megfelel a következő forrás URL-cím hello CDN-végpont toohello:</span><span class="sxs-lookup"><span data-stu-id="5b851-182">Which corresponds toohello following origin URL at hello CDN endpoint:</span></span>

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

<span data-ttu-id="5b851-183">Kiválasztásakor túl**http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, attól függően, hogy a böngészőben fog az rákérdezéses toodownload vagy nyitott hello bootstrap.css, a közzétett webalkalmazás származik.</span><span class="sxs-lookup"><span data-stu-id="5b851-183">When you navigate too**http://*&lt;cdnName>*.azureedge.net/Content/bootstrap.css**, depending on your browser, you will be prompted toodownload or open hello bootstrap.css that came from your published Web app.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

<span data-ttu-id="5b851-184">Hasonló módon érheti el bármely nyilvánosan elérhető URL-  **http://*&lt;serviceName >*.cloudapp.net/** rögtön a CDN-végpont.</span><span class="sxs-lookup"><span data-stu-id="5b851-184">You can similarly access any publicly accessible URL at **http://*&lt;serviceName>*.cloudapp.net/**, straight from your CDN endpoint.</span></span> <span data-ttu-id="5b851-185">Példa:</span><span class="sxs-lookup"><span data-stu-id="5b851-185">For example:</span></span>

* <span data-ttu-id="5b851-186">Egy .js fájl hello/Script elérési útjáról</span><span class="sxs-lookup"><span data-stu-id="5b851-186">A .js file from hello /Script path</span></span>
* <span data-ttu-id="5b851-187">Bármely tartalomfájl hello /Content az elérési út</span><span class="sxs-lookup"><span data-stu-id="5b851-187">Any content file from hello /Content path</span></span>
* <span data-ttu-id="5b851-188">Bármely tartományvezérlő/művelet</span><span class="sxs-lookup"><span data-stu-id="5b851-188">Any controller/action</span></span>
* <span data-ttu-id="5b851-189">Ha a CDN-végpontot, a lekérdezési karakterláncokat tartalmazó valamennyi URL-cím jelenleg engedélyezett hello lekérdezési karakterlánc</span><span class="sxs-lookup"><span data-stu-id="5b851-189">If hello query string is enabled at your CDN endpoint, any URL with query strings</span></span>

<span data-ttu-id="5b851-190">Valójában a fenti konfigurációs hello, tárolhatja hello teljes körű felhőalapú szolgáltatás  **http://*&lt;cdnName >*.azureedge.net/**. Ha I túl**http://camservice.azureedge.net/ ** kapok hello művelet eredménye a kezdőlap/Index.</span><span class="sxs-lookup"><span data-stu-id="5b851-190">In fact, with hello above configuration, you can host hello entire cloud service from **http://*&lt;cdnName>*.azureedge.net/**. If I navigate too**http://camservice.azureedge.net/**, I get hello action result from Home/Index.</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

<span data-ttu-id="5b851-191">Ez nem jelenti azt, azonban, hogy a rendszer mindig egy jó ötlet tooserve egy teljes körű felhőalapú szolgáltatás Azure CDN keresztül.</span><span class="sxs-lookup"><span data-stu-id="5b851-191">This does not mean, however, that it's always a good idea tooserve an entire cloud service through Azure CDN.</span></span> 

<span data-ttu-id="5b851-192">A statikus kézbesítési optimalizálásával CDN nem feltétlenül felgyorsítása dinamikus eszközöket, amelyek nem jelentenek gyorsítótárazott toobe, vagy nagyon gyakran frissülnek, mivel a hello CDN kell lekéréses hello eszközök új verziójának hello forráskiszolgálóról nagyon gyakran kézbesítését.</span><span class="sxs-lookup"><span data-stu-id="5b851-192">A CDN with static delivery optimization does not necessarily speed up delivery of dynamic assets which are not meant toobe cached, or are updated very frequently, since hello CDN must pull a new version of hello asset from hello Origin server very often.</span></span> <span data-ttu-id="5b851-193">A jelen esetben engedélyezhető [dinamikus hely Acceleration](cdn-dynamic-site-acceleration.md) CDN-végpont nem gyorsítótárazható dinamikus eszközök kézbesítését be különböző módszereket toospeed használó optimalizálása (DSA).</span><span class="sxs-lookup"><span data-stu-id="5b851-193">For this scenario, you can enable [Dynamic Site Acceleration](cdn-dynamic-site-acceleration.md) optimization (DSA) on your CDN endpoint which uses various techniques toospeed up delivery of non-cacheable dynamic assets.</span></span> 

<span data-ttu-id="5b851-194">Ha statikus és dinamikus tartalom vegyesen hellyel rendelkezik, választhat tooserve a CDN (például általános webes kézbesítés) statikus optimalizálási típusú statikus tartalmat, és a tooserve dinamikus tartalmat közvetlenül a hello eredeti kiszolgálóra vagy egy CDN keresztül DSA optimalizálásával végpont-eseti alapon van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="5b851-194">If you have a site with a mix of static and dynamic content, you can choose tooserve your static content from CDN with a static optimization type (such as general web delivery), and tooserve dynamic content either directly from hello origin server, or through a CDN endpoint with DSA optimization turned on a case-by-case basis.</span></span> <span data-ttu-id="5b851-195">toothat célból már láthatta hogyan tooaccess egyes tartalom fájljainak hello CDN-végpontot.</span><span class="sxs-lookup"><span data-stu-id="5b851-195">toothat end, you have already seen how tooaccess individual content files from hello CDN endpoint.</span></span> <span data-ttu-id="5b851-196">I bemutatja, hogyan tooserve egy adott vezérlő művelet a megadott CDN-végpontot keresztül kiszolgálására tartalom vezérlő műveletekből Azure CDN keresztül.</span><span class="sxs-lookup"><span data-stu-id="5b851-196">I will show you how tooserve a specific controller action through a specific CDN endpoint in Serve content from controller actions through Azure CDN.</span></span>

<span data-ttu-id="5b851-197">hello alternatív toodetermine, amely a felhőszolgáltatás-eseti alapon Azure CDN tooserve tartalom.</span><span class="sxs-lookup"><span data-stu-id="5b851-197">hello alternative is toodetermine which content tooserve from Azure CDN on a case-by-case basis in your cloud service.</span></span> <span data-ttu-id="5b851-198">toothat célból már láthatta hogyan tooaccess egyes tartalom fájljainak hello CDN-végpontot.</span><span class="sxs-lookup"><span data-stu-id="5b851-198">toothat end, you have already seen how tooaccess individual content files from hello CDN endpoint.</span></span> <span data-ttu-id="5b851-199">I bemutatja, hogyan tooserve egy adott vezérlő művelet keresztül hello a CDN-végpont [vezérlő műveletekből Azure CDN keresztül tartalmat](#controller).</span><span class="sxs-lookup"><span data-stu-id="5b851-199">I will show you how tooserve a specific controller action through hello CDN endpoint in [Serve content from controller actions through Azure CDN](#controller).</span></span>

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a><span data-ttu-id="5b851-200">A felhőszolgáltatás a statikus fájlok gyorsítótárazási beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5b851-200">Configure caching options for static files in your cloud service</span></span>
<span data-ttu-id="5b851-201">Az Azure CDN integráció a felhőalapú szolgáltatás megadhatja a statikus tartalom toobe tárolja a CDN-végpont hello módját.</span><span class="sxs-lookup"><span data-stu-id="5b851-201">With Azure CDN integration in your cloud service, you can specify how you want static content toobe cached in hello CDN endpoint.</span></span> <span data-ttu-id="5b851-202">toodo a, nyissa meg *Web.config* a webes szerepkör projekt (pl. WebRole1), és adja hozzá a `<staticContent>` elem túl`<system.webServer>`.</span><span class="sxs-lookup"><span data-stu-id="5b851-202">toodo this, open *Web.config* from your Web role project (e.g. WebRole1) and add a `<staticContent>` element too`<system.webServer>`.</span></span> <span data-ttu-id="5b851-203">az alábbi XML hello hello gyorsítótár tooexpire azt a 3 nap.</span><span class="sxs-lookup"><span data-stu-id="5b851-203">hello XML below configures hello cache tooexpire in 3 days.</span></span>  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

<span data-ttu-id="5b851-204">Ezután a, a felhőszolgáltatásban található összes statikus fájlok megfigyelheti hello ugyanaz a szabály a CDN-gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="5b851-204">Once you do this, all static files in your cloud service will observe hello same rule in your CDN cache.</span></span> <span data-ttu-id="5b851-205">Részletesebben vezérelhető, a gyorsítótár beállításait, vegye fel a *Web.config* fájlt egy mappába, és ott a beállítások.</span><span class="sxs-lookup"><span data-stu-id="5b851-205">For more granular control of cache settings, add a *Web.config* file into a folder and add your settings there.</span></span> <span data-ttu-id="5b851-206">Például hozzáadhat egy *Web.config* toohello fájl *\Content* mappa, és cserélje le a következő XML hello hello tartalom:</span><span class="sxs-lookup"><span data-stu-id="5b851-206">For example, add a *Web.config* file toohello *\Content* folder and replace hello content with hello following XML:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

<span data-ttu-id="5b851-207">Ez a beállítás feldolgozza az összes statikus fájlt hello *\Content* mappa toobe 15 napig.</span><span class="sxs-lookup"><span data-stu-id="5b851-207">This setting causes all static files from hello *\Content* folder toobe cached for 15 days.</span></span>

<span data-ttu-id="5b851-208">További információt a tooconfigure hello `<clientCache>` elem, lásd: [Ügyfélgyorsítótár &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span><span class="sxs-lookup"><span data-stu-id="5b851-208">For more information on how tooconfigure hello `<clientCache>` element, see [Client Cache &lt;clientCache>](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).</span></span>

<span data-ttu-id="5b851-209">A [vezérlő műveletekből Azure CDN keresztül tartalmat](#controller), I is bemutatja, hogyan konfigurálhatja a tartományvezérlő műveleti eredmény-gyorsítótárának beállításait a hello CDN gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="5b851-209">In [Serve content from controller actions through Azure CDN](#controller), I will also show you how you can configure cache settings for controller action results in hello CDN cache.</span></span>

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a><span data-ttu-id="5b851-210">Az Azure CDN keresztül vezérlő műveletekből tartalmat</span><span class="sxs-lookup"><span data-stu-id="5b851-210">Serve content from controller actions through Azure CDN</span></span>
<span data-ttu-id="5b851-211">Egy felhőalapú szolgáltatás webes szerepkör integrálása Azure CDN, esetén viszonylag egyszerű tooserve tartalom vezérlő műveletekből hello Azure CDN keresztül.</span><span class="sxs-lookup"><span data-stu-id="5b851-211">When you integrate a cloud service Web role with Azure CDN, it is relatively easy tooserve content from controller actions through hello Azure CDN.</span></span> <span data-ttu-id="5b851-212">A felhőbeli szolgáltató eltérő szolgáltatás közvetlenül az Azure CDN (egy fent), [Martin Balliauw](https://twitter.com/maartenballiauw) bemutatja, hogyan toodo azt egy visszatöltött MemeGenerator tartományvezérlőre [csökkenti a késéseket hello Azure CDN hello weben ](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span><span class="sxs-lookup"><span data-stu-id="5b851-212">Other than serving your cloud service directly through Azure CDN (demonstrated above), [Maarten Balliauw](https://twitter.com/maartenballiauw) shows you how toodo it with a fun MemeGenerator controller in [Reducing latency on hello web with hello Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN).</span></span> <span data-ttu-id="5b851-213">I fog egyszerűen Reprodukálja azt itt.</span><span class="sxs-lookup"><span data-stu-id="5b851-213">I will simply reproduce it here.</span></span>

<span data-ttu-id="5b851-214">Tegyük fel, hogy a felhőszolgáltatásban található egy fiatal Horváth Norris lemezképen alapuló toogenerate memes kívánt (által fénykép [Alan világos](http://www.flickr.com/photos/alan-light/218493788/)) Ez, például:</span><span class="sxs-lookup"><span data-stu-id="5b851-214">Suppose in your cloud service you want toogenerate memes based on a young Chuck Norris image (photo by [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) like this:</span></span>

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

<span data-ttu-id="5b851-215">Rendelkezik egy egyszerű `Index` műveletet, amely lehetővé teszi az hello ügyfelek toospecify hello superlatives hello kép, majd állít elő hello meme azok utáni toohello művelet után.</span><span class="sxs-lookup"><span data-stu-id="5b851-215">You have a simple `Index` action that allows hello customers toospecify hello superlatives in hello image, then generates hello meme once they post toohello action.</span></span> <span data-ttu-id="5b851-216">Mivel ez Horváth Norris, teheti meg a lap toobecome menet népszerű globálisan.</span><span class="sxs-lookup"><span data-stu-id="5b851-216">Since it's Chuck Norris, you would expect this page toobecome wildly popular globally.</span></span> <span data-ttu-id="5b851-217">Ez az Azure CDN félig dinamikus tartalmat szolgáltató jól szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="5b851-217">This is a good example of serving semi-dynamic content with Azure CDN.</span></span>

<span data-ttu-id="5b851-218">Lépésekkel hello toosetup fent a vezérlő művelet:</span><span class="sxs-lookup"><span data-stu-id="5b851-218">Follow hello steps above toosetup this controller action:</span></span>

1. <span data-ttu-id="5b851-219">A hello *\Controllers* mappa, hozzon létre egy új .cs fájlt nevű *MemeGeneratorController.cs* és a következő kód csere hello hello tartalom.</span><span class="sxs-lookup"><span data-stu-id="5b851-219">In hello *\Controllers* folder, create a new .cs file called *MemeGeneratorController.cs* and replace hello content with hello following code.</span></span> <span data-ttu-id="5b851-220">Lehet, hogy tooreplace hello kiemelt része a CDN-névvel.</span><span class="sxs-lookup"><span data-stu-id="5b851-220">Be sure tooreplace hello highlighted portion with your CDN name.</span></span>  
   
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
   
                    if (Debugger.IsAttached) // Preserve hello debug experience
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
2. <span data-ttu-id="5b851-221">Kattintson a jobb gombbal a hello alapértelmezett `Index()` műveletet, és kijelölheti **nézet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5b851-221">Right-click in hello default `Index()` action and select **Add View**.</span></span>
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. <span data-ttu-id="5b851-222">Fogadja el az alábbi hello beállításokat, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5b851-222">Accept hello settings below and click **Add**.</span></span>
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. <span data-ttu-id="5b851-223">Megnyitás új hello *Views\MemeGenerator\Index.cshtml* , és cserélje hello tartalom a következő egyszerű HTML hello superlatives küldésére hello:</span><span class="sxs-lookup"><span data-stu-id="5b851-223">Open hello new *Views\MemeGenerator\Index.cshtml* and replace hello content with hello following simple HTML for submitting hello superlatives:</span></span>
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. <span data-ttu-id="5b851-224">Tegye közzé újra hello felhőalapú szolgáltatás, és keresse meg a túl**http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="5b851-224">Publish hello cloud service again and navigate too**http://*&lt;serviceName>*.cloudapp.net/MemeGenerator/Index** in your browser.</span></span>

<span data-ttu-id="5b851-225">Ha hello űrlap értékek elküldését, túl`/MemeGenerator/Index`, hello `Index_Post` tartozó műveleti módszer adja vissza a hivatkozás toohello `Show` hello megfelelő bemeneti azonosítóval tartozó műveleti módszer.</span><span class="sxs-lookup"><span data-stu-id="5b851-225">When you submit hello form values too`/MemeGenerator/Index`, hello `Index_Post` action method returns a link toohello `Show` action method with hello respective input identifier.</span></span> <span data-ttu-id="5b851-226">Amikor hello hivatkozásra kattint, a következő kód hello érhető el:</span><span class="sxs-lookup"><span data-stu-id="5b851-226">When you click hello link, you reach hello following code:</span></span>  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve hello debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

<span data-ttu-id="5b851-227">Ha a helyi hibakereső van csatlakoztatva, majd elérhetővé válik a helyi átirányítást hello rendszeres hibakeresési élményt.</span><span class="sxs-lookup"><span data-stu-id="5b851-227">If your local debugger is attached, then you will get hello regular debug experience with a local redirect.</span></span> <span data-ttu-id="5b851-228">Ha hello felhőszolgáltatásban fut, majd azt fogja átirányítani Önt:</span><span class="sxs-lookup"><span data-stu-id="5b851-228">If it's running in hello cloud service, then it will redirect to:</span></span>

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

<span data-ttu-id="5b851-229">Amely megfelel a következő forrás URL-címet a CDN-végpont toohello:</span><span class="sxs-lookup"><span data-stu-id="5b851-229">Which corresponds toohello following origin URL at your CDN endpoint:</span></span>

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


<span data-ttu-id="5b851-230">Hello segítségével `OutputCacheAttribute` hello attribútum `Generate` metódus toospecify hogyan hello művelet eredménye gyorsítótárazza, amely Azure CDN megtörténni.</span><span class="sxs-lookup"><span data-stu-id="5b851-230">You can then use hello `OutputCacheAttribute` attribute on hello `Generate` method toospecify how hello action result should be cached, which Azure CDN will honor.</span></span> <span data-ttu-id="5b851-231">hello kódot adja meg a gyorsítótár lejárati 1 óra (3600 másodperc).</span><span class="sxs-lookup"><span data-stu-id="5b851-231">hello code below specify a cache expiration of 1 hour (3,600 seconds).</span></span>

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

<span data-ttu-id="5b851-232">Ehhez hasonlóan, szolgálhat bármely tartományvezérlő művelet a tartalmakat a felhőszolgáltatásban keresztül az Azure CDN szükséges hello gyorsítótárazási kapcsolóval.</span><span class="sxs-lookup"><span data-stu-id="5b851-232">Likewise, you can serve up content from any controller action in your cloud service through your Azure CDN, with hello desired caching option.</span></span>

<span data-ttu-id="5b851-233">A következő szakaszban hello I bemutatja, hogyan tooserve hello kötegelve, és a parancsfájlokat és Azure CDN keresztül CSS minified.</span><span class="sxs-lookup"><span data-stu-id="5b851-233">In hello next section, I will show you how tooserve hello bundled and minified scripts and CSS through Azure CDN.</span></span>

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a><span data-ttu-id="5b851-234">ASP.NET kötegelése és minification integrálása az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="5b851-234">Integrate ASP.NET bundling and minification with Azure CDN</span></span>
<span data-ttu-id="5b851-235">Parancsfájlok és CSS stíluslapok ritkán módosítsa, majd hello Azure CDN gyorsítótár elsődleges vár.</span><span class="sxs-lookup"><span data-stu-id="5b851-235">Scripts and CSS stylesheets change infrequently and are prime candidates for hello Azure CDN cache.</span></span> <span data-ttu-id="5b851-236">Szolgál hello teljes webes szerepkört az Azure CDN keresztül hello legegyszerűbb módja toointegrate kötegelése és az Azure CDN minification.</span><span class="sxs-lookup"><span data-stu-id="5b851-236">Serving hello entire Web role through your Azure CDN is hello easiest way toointegrate bundling and minification with Azure CDN.</span></span> <span data-ttu-id="5b851-237">Azonban előfordulhat, hogy nem kívánt toodo ez, I bemutatja, hogyan toodo azt megőrzése mellett hello szükséges develper élmény ASP.NET kötegelése és minification, többek között:</span><span class="sxs-lookup"><span data-stu-id="5b851-237">However, as you may not want toodo this, I will show you how toodo it while preserving hello desired develper experience of ASP.NET bundling and minification, such as:</span></span>

* <span data-ttu-id="5b851-238">Nagyszerű hibakeresési mód élmény</span><span class="sxs-lookup"><span data-stu-id="5b851-238">Great debug mode experience</span></span>
* <span data-ttu-id="5b851-239">Egyszerű telepítés</span><span class="sxs-lookup"><span data-stu-id="5b851-239">Streamlined deployment</span></span>
* <span data-ttu-id="5b851-240">A parancsfájl/CSS verziófrissítések azonnali frissítést tooclients</span><span class="sxs-lookup"><span data-stu-id="5b851-240">Immediate updates tooclients for script/CSS version upgrades</span></span>
* <span data-ttu-id="5b851-241">Ha a CDN-végpontot nem sikerült tartalék mechanizmus</span><span class="sxs-lookup"><span data-stu-id="5b851-241">Fallback mechanism when your CDN endpoint fails</span></span>
* <span data-ttu-id="5b851-242">Minimalizálása érdekében a kód módosítása</span><span class="sxs-lookup"><span data-stu-id="5b851-242">Minimize code modification</span></span>

<span data-ttu-id="5b851-243">A hello **WebRole1** létrehozott projekt [Azure CDN-végpont integrálása az Azure webhelyén, és a statikus tartalmat szolgáltat a weblapok Azure CDN](#deploy), nyissa meg *App_Start\ BundleConfig.cs* és vessen egy pillantást hello `bundles.Add()` metódushívások.</span><span class="sxs-lookup"><span data-stu-id="5b851-243">In hello **WebRole1** project that you created in [Integrate an Azure CDN endpoint with your Azure website and serve static content in your Web pages from Azure CDN](#deploy), open *App_Start\BundleConfig.cs* and take a look at hello `bundles.Add()` method calls.</span></span>

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

<span data-ttu-id="5b851-244">először hello `bundles.Add()` utasítás ad hozzá egy parancsfájl köteg hello virtuális könyvtár `~/bundles/jquery`.</span><span class="sxs-lookup"><span data-stu-id="5b851-244">hello first `bundles.Add()` statement adds a script bundle at hello virtual directory `~/bundles/jquery`.</span></span> <span data-ttu-id="5b851-245">Ezután nyissa meg *Views\Shared\_Layout.cshtml* toosee hogyan hello parancsfájlkód köteg lett megjelenítve.</span><span class="sxs-lookup"><span data-stu-id="5b851-245">Then, open *Views\Shared\_Layout.cshtml* toosee how hello script bundle tag is rendered.</span></span> <span data-ttu-id="5b851-246">Meg kell tudni toofind hello Razor kódsort a következő:</span><span class="sxs-lookup"><span data-stu-id="5b851-246">You should be able toofind hello following line of Razor code:</span></span>

    @Scripts.Render("~/bundles/jquery")

<span data-ttu-id="5b851-247">Futtatásakor a Razor kódot hello Azure webes szerepkör, akkor jelenik meg egy `<script>` hello címkéjének köteg hasonló toohello következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="5b851-247">When this Razor code is run in hello Azure Web role, it will render a `<script>` tag for hello script bundle similar toohello following:</span></span>

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

<span data-ttu-id="5b851-248">Azonban azt futása közben a Visual Studio beírásával `F5`, külön-külön, ezzel veszélyezteti a hello kötegben minden parancsfájl (hello fenti esetben csak egy parancsfájl van hello csomagot):</span><span class="sxs-lookup"><span data-stu-id="5b851-248">However, when it is run in Visual Studio by typing `F5`, it will render each script file in hello bundle individually (in hello case above, only one script file is in hello bundle):</span></span>

    <script src="/Scripts/jquery-1.10.2.js"></script>

<span data-ttu-id="5b851-249">Ez lehetővé teszi a fejlesztési környezet toodebug hello JavaScript-kód közben csökkentése (kötegelése) egyidejű kapcsolatok és jobb lesz a fájl letöltése teljesítmény (minification) éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5b851-249">This enables you toodebug hello JavaScript code in your development environment while reducing concurrent client connections (bundling) and improving file download performance (minification) in production.</span></span> <span data-ttu-id="5b851-250">Az Azure CDN integráció egy nagyszerű szolgáltatás toopreserve.</span><span class="sxs-lookup"><span data-stu-id="5b851-250">It's a great feature toopreserve with Azure CDN integration.</span></span> <span data-ttu-id="5b851-251">Ezenkívül megjelenítve hello csomagot már tartalmaz egy automatikusan létrehozott verzió-karakterláncnak, mivel azt szeretné, tooreplicate, amely funkciót úgy hello frissítése jQuery keresztül NuGet, amikor frissíthető hello ügyfél oldalán, amint lehetséges.</span><span class="sxs-lookup"><span data-stu-id="5b851-251">Furthermore, since hello rendered bundle already contains an automatically generated version string, you want tooreplicate that functionality so hello whenever you update your jQuery version through NuGet, it can be updated at hello client side as soon as possible.</span></span>

<span data-ttu-id="5b851-252">Kövesse az alábbi toointegration ASP.NET kötegelése és a CDN-végponthoz minification hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5b851-252">Follow hello steps below toointegration ASP.NET bundling and minification with your CDN endpoint.</span></span>

1. <span data-ttu-id="5b851-253">Vissza a *App_Start\BundleConfig.cs*, hello módosítása `bundles.Add()` módszerek toouse egy másik [köteg konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), egy, a CDN-címet adja meg.</span><span class="sxs-lookup"><span data-stu-id="5b851-253">Back in *App_Start\BundleConfig.cs*, modify hello `bundles.Add()` methods toouse a different [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), one that specifies a CDN address.</span></span> <span data-ttu-id="5b851-254">toodo, ez a név felülírandó hello `RegisterBundles` metódus-definíció hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="5b851-254">toodo this, replace hello `RegisterBundles` method definition with hello following code:</span></span>  
   
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
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you're
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    <span data-ttu-id="5b851-255">Lehet, hogy tooreplace `<yourCDNName>` az Azure CDN hello nevére.</span><span class="sxs-lookup"><span data-stu-id="5b851-255">Be sure tooreplace `<yourCDNName>` with hello name of your Azure CDN.</span></span>
   
    <span data-ttu-id="5b851-256">Egyszerű szavakkal állítja `bundles.UseCdn = true` és gondosan kialakított CDN URL-cím tooeach köteg hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="5b851-256">In plain words, you are setting `bundles.UseCdn = true` and added a carefully crafted CDN URL tooeach bundle.</span></span> <span data-ttu-id="5b851-257">Például hello első konstruktor hello kódban:</span><span class="sxs-lookup"><span data-stu-id="5b851-257">For example, hello first constructor in hello code:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    <span data-ttu-id="5b851-258">ugyanaz, mint a rendszer hello:</span><span class="sxs-lookup"><span data-stu-id="5b851-258">is hello same as:</span></span>
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    <span data-ttu-id="5b851-259">Ez a konstruktor közli az ASP.NET kötegelése és minification egyéni parancsfájlok toorender, amikor indítja helyi, de használja hello CDN cím tooaccess hello parancsfájl az adott meg.</span><span class="sxs-lookup"><span data-stu-id="5b851-259">This constructor tells ASP.NET bundling and minification toorender individual script files when debugged locally, but use hello specified CDN address tooaccess hello script in question.</span></span> <span data-ttu-id="5b851-260">Azonban ügyeljen arra, hogy alaposan kialakított CDN URL-címet a két fontos jellemzők:</span><span class="sxs-lookup"><span data-stu-id="5b851-260">However, note two important characteristics with this carefully crafted CDN URL:</span></span>
   
   * <span data-ttu-id="5b851-261">a CDN URL-címhez hello forrása: `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, vagyis tényleges hello virtuális könyvtár hello parancsfájl köteg a felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="5b851-261">hello origin for this CDN URL is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, which is actually hello virtual directory of hello script bundle in your cloud service.</span></span>
   * <span data-ttu-id="5b851-262">CDN konstruktor használ, mivel a hello CDN script kód a hello köteg már nem tartalmaz URL-cím megjelenítése hello automatikusan generált hello verzió-karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="5b851-262">Since you are using CDN constructor, hello CDN script tag for hello bundle no longer contains hello automatically generated version string in hello rendered URL.</span></span> <span data-ttu-id="5b851-263">Manuálisan létre kell hoznia egy egyedi verzió-karakterláncot, minden alkalommal, amikor hello parancsfájl köteg, az Azure CDN teljesíti a gyorsítótár módosított tooforce.</span><span class="sxs-lookup"><span data-stu-id="5b851-263">You must manually generate a unique version string every time hello script bundle is modified tooforce a cache miss at your Azure CDN.</span></span> <span data-ttu-id="5b851-264">At hello azonos időben, a egyedi verzió-karakterlánca állandónak kell keresztül hello telepítési toomaximize találatot eredményező gyorsítótárbeli kereséseinek, az Azure CDN hello élettartama hello köteg telepítése után.</span><span class="sxs-lookup"><span data-stu-id="5b851-264">At hello same time, this unique version string must remain constant through hello life of hello deployment toomaximize cache hits at your Azure CDN after hello bundle is deployed.</span></span>
   * <span data-ttu-id="5b851-265">lekérdezési karakterlánc v hello = < W.X.Y.Z > ponttá a *Properties\AssemblyInfo.cs* a webes szerepkör projekt.</span><span class="sxs-lookup"><span data-stu-id="5b851-265">hello query string v=<W.X.Y.Z> pulls from *Properties\AssemblyInfo.cs* in your Web role project.</span></span> <span data-ttu-id="5b851-266">Akkor is, amely tartalmazza a növekvő hello szerelvény verziója, minden alkalommal, amikor tooAzure közzéteszi a telepítési munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="5b851-266">You can have a deployment workflow that includes incrementing hello assembly version every time you publish tooAzure.</span></span> <span data-ttu-id="5b851-267">Vagy csak módosíthatja *Properties\AssemblyInfo.cs* a projekt tooautomatically növekmény hello verzió-karakterlánc minden alkalommal, amikor a build, használatával hello helyettesítő karakter "*".</span><span class="sxs-lookup"><span data-stu-id="5b851-267">Or, you can just modify *Properties\AssemblyInfo.cs* in your project tooautomatically increment hello version string every time you build, using hello wildcard character '*'.</span></span> <span data-ttu-id="5b851-268">Példa:</span><span class="sxs-lookup"><span data-stu-id="5b851-268">For example:</span></span>
     
        <span data-ttu-id="5b851-269">[szerelvény: AssemblyVersion("1.0.0.*")]</span><span class="sxs-lookup"><span data-stu-id="5b851-269">[assembly: AssemblyVersion("1.0.0.*")]</span></span>
     
     <span data-ttu-id="5b851-270">Más stratégia toostreamline egy egyedi karakterlánc hello során a központi telepítés létrehozásakor itt fog működni.</span><span class="sxs-lookup"><span data-stu-id="5b851-270">Any other strategy toostreamline generating a unique string for hello life of a deployment will work here.</span></span>
2. <span data-ttu-id="5b851-271">Hello felhőalapú szolgáltatás, és hozzáférést hello kezdőlap közzé.</span><span class="sxs-lookup"><span data-stu-id="5b851-271">Republish hello cloud service and access hello home page.</span></span>
3. <span data-ttu-id="5b851-272">Nézet hello hello oldal HTML-kódja.</span><span class="sxs-lookup"><span data-stu-id="5b851-272">View hello HTML code for hello page.</span></span> <span data-ttu-id="5b851-273">Meg kell tudni toosee hello CDN URL-cím, egy egyedi verzió-karakterláncot, minden alkalommal, amikor újra közzé módosítások tooyour felhőalapú szolgáltatás együtt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5b851-273">You should be able toosee hello CDN URL rendered, with a unique version string every time you republish changes tooyour cloud service.</span></span> <span data-ttu-id="5b851-274">Példa:</span><span class="sxs-lookup"><span data-stu-id="5b851-274">For example:</span></span>  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. <span data-ttu-id="5b851-275">A Visual Studio hibakeresési írja be a Visual Studio felhőszolgáltatás hello `F5`.,</span><span class="sxs-lookup"><span data-stu-id="5b851-275">In Visual Studio, debug hello cloud service in Visual Studio by typing `F5`.,</span></span>
5. <span data-ttu-id="5b851-276">Nézet hello hello oldal HTML-kódja.</span><span class="sxs-lookup"><span data-stu-id="5b851-276">View hello HTML code for hello page.</span></span> <span data-ttu-id="5b851-277">Minden egyes külön-külön megjelenítve, hogy akkor is egy egységes élmény a Visual Studio hibakeresési parancsfájl továbbra is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="5b851-277">You will still see each script file individually rendered so that you can have a consistent debug experience in Visual Studio.</span></span>  
   
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

## <a name="fallback-mechanism-for-cdn-urls"></a><span data-ttu-id="5b851-278">Tartalék mechanizmus CDN URL-címek esetén</span><span class="sxs-lookup"><span data-stu-id="5b851-278">Fallback mechanism for CDN URLs</span></span>
<span data-ttu-id="5b851-279">Azure CDN-végpont bármilyen okból nem sikerül, ha azt szeretné, a weblap toobe intelligens elég tooaccess a származási webkiszolgáló JavaScript vagy rendszerindítási feltöltését hello megoldásként.</span><span class="sxs-lookup"><span data-stu-id="5b851-279">When your Azure CDN endpoint fails for any reason, you want your Web page toobe smart enough tooaccess your origin Web server as hello fallback option for loading JavaScript or Bootstrap.</span></span> <span data-ttu-id="5b851-280">Elég súlyos toolose képek a webhelyen tooCDN elérhetetlensége, de a parancsfájlok és a stíluslapok által biztosított sokkal több súlyos toolose kritikus fontosságú lap funkciókat miatt.</span><span class="sxs-lookup"><span data-stu-id="5b851-280">It's serious enough toolose images on your website due tooCDN unavailability, but much more severe toolose crucial page functionality provided by your scripts and stylesheets.</span></span>

<span data-ttu-id="5b851-281">Hello [köteg](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) osztály tartalmazza a tulajdonságot, [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , amely lehetővé teszi tooconfigure hello tartalék mechanizmus a CDN-hiba.</span><span class="sxs-lookup"><span data-stu-id="5b851-281">hello [Bundle](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) class contains a property called [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) that enables you tooconfigure hello fallback mechanism for CDN failure.</span></span> <span data-ttu-id="5b851-282">toouse ezt a tulajdonságot, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5b851-282">toouse this property, follow hello steps below:</span></span>

1. <span data-ttu-id="5b851-283">A webes szerepkör projektben nyissa meg *App_Start\BundleConfig.cs*, amelyikhez hozzáadta a CDN URL-cím az egyes [köteg konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), és ellenőrizze a kijelölt hello következő tooadd tartalék mechanizmus toohello változik alapértelmezett csomagokat:</span><span class="sxs-lookup"><span data-stu-id="5b851-283">In your Web role project, open *App_Start\BundleConfig.cs*, where you added a CDN URL in each [Bundle constructor](http://msdn.microsoft.com/library/jj646464.aspx), and make hello following highlighted changes tooadd fallback mechanism toohello default bundles:</span></span>  
   
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
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
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
   
    <span data-ttu-id="5b851-284">Ha `CdnFallbackExpression` értéke nem null értékű, parancsfájl van be a nézetmodellbe hello HTML tootest e hello csomag sikeresen betöltődött és, ha nem, érheti el hello köteg közvetlenül hello származási webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5b851-284">When `CdnFallbackExpression` is not null, script is injected into hello HTML tootest whether hello bundle is loaded successfully and, if not, access hello bundle directly from hello origin Web server.</span></span> <span data-ttu-id="5b851-285">Ez a tulajdonság kell toobe beállítási tooa JavaScript kifejezés, amely teszteli, hogy hello megfelelő CDN csomagot megfelelően be van töltve.</span><span class="sxs-lookup"><span data-stu-id="5b851-285">This property needs toobe set tooa JavaScript expression that tests whether hello respective CDN bundle is loaded properly.</span></span> <span data-ttu-id="5b851-286">hello kifejezés szükséges tootest minden alkalmazáscsomag eltér függően toohello tartalmat.</span><span class="sxs-lookup"><span data-stu-id="5b851-286">hello expression needed tootest each bundle differs according toohello content.</span></span> <span data-ttu-id="5b851-287">A fenti hello alapértelmezett csomagokat:</span><span class="sxs-lookup"><span data-stu-id="5b851-287">For hello default bundles above:</span></span>
   
   * <span data-ttu-id="5b851-288">`window.jquery`jquery-{version} .js definiálva van</span><span class="sxs-lookup"><span data-stu-id="5b851-288">`window.jquery` is defined in jquery-{version}.js</span></span>
   * <span data-ttu-id="5b851-289">`$.validator`jquery.validate.js definiálva van</span><span class="sxs-lookup"><span data-stu-id="5b851-289">`$.validator` is defined in jquery.validate.js</span></span>
   * <span data-ttu-id="5b851-290">`window.Modernizr`modernizer-{version} .js definiálva van</span><span class="sxs-lookup"><span data-stu-id="5b851-290">`window.Modernizr` is defined in modernizer-{version}.js</span></span>
   * <span data-ttu-id="5b851-291">`$.fn.modal`bootstrap.js definiálva van</span><span class="sxs-lookup"><span data-stu-id="5b851-291">`$.fn.modal` is defined in bootstrap.js</span></span>
     
     <span data-ttu-id="5b851-292">Észrevette, hogy, hogy I nincs beállítva CdnFallbackExpression a hello `~/Cointent/css` csomagot.</span><span class="sxs-lookup"><span data-stu-id="5b851-292">You might have noticed that I did not set CdnFallbackExpression for hello `~/Cointent/css` bundle.</span></span> <span data-ttu-id="5b851-293">Ez azért, mert jelenleg nincs egy [System.Web.Optimization hiba](https://aspnetoptimization.codeplex.com/workitem/104) , amelyek esetében a `<script>` helyett hello tartalék CSS várt hello címkéjének `<link>` címke.</span><span class="sxs-lookup"><span data-stu-id="5b851-293">This is because currently there is a [bug in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) that injects a `<script>` tag for hello fallback CSS instead of hello expected `<link>` tag.</span></span>
     
     <span data-ttu-id="5b851-294">Nincs, de jó [stílus köteg tartalék](https://github.com/EmberConsultingGroup/StyleBundleFallback) által kínált [decemberéig tanácsadás csoport](https://github.com/EmberConsultingGroup).</span><span class="sxs-lookup"><span data-stu-id="5b851-294">There is, however, a good [Style Bundle Fallback](https://github.com/EmberConsultingGroup/StyleBundleFallback) offered by [Ember Consulting Group](https://github.com/EmberConsultingGroup).</span></span>
2. <span data-ttu-id="5b851-295">toouse hello megoldás a CSS, hozzon létre egy új .cs-fájlt a webes szerepkör projekt *App_Start* nevű mappát *StyleBundleExtensions.cs*, és cserélje le benne lévő tartalom hello [a kód GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="5b851-295">toouse hello workaround for CSS, create a new .cs file in your Web role project's *App_Start* folder called *StyleBundleExtensions.cs*, and replace its content with hello [code from GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).</span></span>
3. <span data-ttu-id="5b851-296">A *App_Start\StyleFundleExtensions.cs*, nevezze át a hello névtér tooyour webes szerepkör nevét (pl. **WebRole1**).</span><span class="sxs-lookup"><span data-stu-id="5b851-296">In *App_Start\StyleFundleExtensions.cs*, rename hello namespace tooyour Web role's name (e.g. **WebRole1**).</span></span>
4. <span data-ttu-id="5b851-297">Lépjen vissza túl`App_Start\BundleConfig.cs` és hello utolsó módosítása `bundles.Add` hello kiemelt kódot a következő utasítást:</span><span class="sxs-lookup"><span data-stu-id="5b851-297">Go back too`App_Start\BundleConfig.cs` and modify hello last `bundles.Add` statement with hello following highlighted code:</span></span>  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    <span data-ttu-id="5b851-298">A bővítmény metódus használ hello azonos ötlet tooinject parancsfájl a hello HTML toocheck hello DOM hello egyező osztály neve, a szabály nevét és hello CSS csomagot, és visszatér hátsó toohello származási webkiszolgáló Ha toofind hello egyeztetés sikertelen a megadott szabály értékét.</span><span class="sxs-lookup"><span data-stu-id="5b851-298">This new extension method uses hello same idea tooinject script in hello HTML toocheck hello DOM for hello a matching class name, rule name, and rule value defined in hello CSS bundle, and falls back toohello origin Web server if it fails toofind hello match.</span></span>
5. <span data-ttu-id="5b851-299">Tegye közzé újra hello felhőalapú szolgáltatás és az access hello kezdőlap.</span><span class="sxs-lookup"><span data-stu-id="5b851-299">Publish hello cloud service again and access hello home page.</span></span>
6. <span data-ttu-id="5b851-300">Nézet hello hello oldal HTML-kódja.</span><span class="sxs-lookup"><span data-stu-id="5b851-300">View hello HTML code for hello page.</span></span> <span data-ttu-id="5b851-301">Keresse meg injected parancsfájlok hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="5b851-301">You should find injected scripts similar toohello following:</span></span>    
   
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

    <span data-ttu-id="5b851-302">A CSS-köteg hello injected parancsfájl továbbra is tartalmaz hello errant tartós hello a `CdnFallbackExpression` tulajdonság hello sorban:</span><span class="sxs-lookup"><span data-stu-id="5b851-302">Note that injected script for hello CSS bundle still contains hello errant remnant from hello `CdnFallbackExpression` property in hello line:</span></span>

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    <span data-ttu-id="5b851-303">De óta hello hello első része. kifejezés mindig ad vissza IGAZ (a hello sorban fölött, amely), hello document.write() függvény soha nem fog futni.</span><span class="sxs-lookup"><span data-stu-id="5b851-303">But since hello first part of hello || expression will always return true (in hello line directly above that), hello document.write() function will never run.</span></span>

## <a name="more-information"></a><span data-ttu-id="5b851-304">További információ</span><span class="sxs-lookup"><span data-stu-id="5b851-304">More Information</span></span>
* [<span data-ttu-id="5b851-305">Hello Azure Content Delivery Network (CDN) áttekintése</span><span class="sxs-lookup"><span data-stu-id="5b851-305">Overview of hello Azure Content Delivery Network (CDN)</span></span>](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [<span data-ttu-id="5b851-306">Az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="5b851-306">Using Azure CDN</span></span>](cdn-create-new-endpoint.md)
* [<span data-ttu-id="5b851-307">ASP.NET kötegelése és Minification</span><span class="sxs-lookup"><span data-stu-id="5b851-307">ASP.NET Bundling and Minification</span></span>](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
