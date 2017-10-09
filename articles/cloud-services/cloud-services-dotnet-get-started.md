---
title: "az Azure felhőalapú szolgáltatásairól és ASP.NET lépései aaaGet |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy többrétegű alkalmazást ASP.NET MVC és az Azure használatával. hello alkalmazás fut egy felhőszolgáltatás, webes és feldolgozói szerepkörben. Entity Framework, SQL Database és Azure Storage üzenetsorokat és blobokat használ."
services: cloud-services, storage
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: d7aa440d-af4a-4f80-b804-cc46178df4f9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/15/2017
ms.author: adegeo
ms.openlocfilehash: 86271c29b79fad3f01f8ea0e88fd00c7aefc970c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cloud-services-and-aspnet"></a><span data-ttu-id="bc105-105">Ismerkedés az Azure Cloud Services szolgáltatással és az ASP.NET keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="bc105-105">Get started with Azure Cloud Services and ASP.NET</span></span>

## <a name="overview"></a><span data-ttu-id="bc105-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="bc105-106">Overview</span></span>
<span data-ttu-id="bc105-107">Ez az oktatóanyag bemutatja, hogyan toocreate előtér-, az ASP.NET mvc többrétegű .NET-alkalmazásokat, és telepítse azt tooan [Azure-felhőszolgáltatásban](cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="bc105-107">This tutorial shows how toocreate a multi-tier .NET application with an ASP.NET MVC front-end, and deploy it tooan [Azure cloud service](cloud-services-choose-me.md).</span></span> <span data-ttu-id="bc105-108">alkalmazás által használt hello [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [Azure Blob szolgáltatás](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), és hello [Azure Queue szolgáltatás](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span><span class="sxs-lookup"><span data-stu-id="bc105-108">hello application uses [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [Azure Blob service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and hello [Azure Queue service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span></span> <span data-ttu-id="bc105-109">Is [hello Visual Studio-projekt letöltése](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) a hello MSDN Kódgalériából.</span><span class="sxs-lookup"><span data-stu-id="bc105-109">You can [download hello Visual Studio project](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) from hello MSDN Code Gallery.</span></span>

<span data-ttu-id="bc105-110">hello oktatóanyag bemutatja, hogyan toobuild, és futtassa hello alkalmazás helyileg, hogyan toodeploy azt tooAzure, és futtassa a hello felhőalapú, és hogyan toobuild az alapoktól.</span><span class="sxs-lookup"><span data-stu-id="bc105-110">hello tutorial shows you how toobuild and run hello application locally, how toodeploy it tooAzure and run in hello cloud, and how toobuild it from scratch.</span></span> <span data-ttu-id="bc105-111">Indítsa el az alapoktól a felépítést, és ezután teszt hello és telepítés lépéseit, ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="bc105-111">You can start by building from scratch and then do hello test and deploy steps afterward if you prefer.</span></span>

## <a name="contoso-ads-application"></a><span data-ttu-id="bc105-112">Contoso Ads alkalmazás</span><span class="sxs-lookup"><span data-stu-id="bc105-112">Contoso Ads application</span></span>
<span data-ttu-id="bc105-113">hello alkalmazás egy hirdetőtábla.</span><span class="sxs-lookup"><span data-stu-id="bc105-113">hello application is an advertising bulletin board.</span></span> <span data-ttu-id="bc105-114">A felhasználók szöveg megadásával és egy kép feltöltésével hoznak létre hirdetéseket.</span><span class="sxs-lookup"><span data-stu-id="bc105-114">Users create an ad by entering text and uploading an image.</span></span> <span data-ttu-id="bc105-115">Láthatják a hirdetések miniatűr képekkel ellátott listáját, és amikor kiválasztanak egy ad toosee hello részletek láthatják hello teljes méretű kép.</span><span class="sxs-lookup"><span data-stu-id="bc105-115">They can see a list of ads with thumbnail images, and they can see hello full-size image when they select an ad toosee hello details.</span></span>

![Hirdetéslista](./media/cloud-services-dotnet-get-started/list.png)

<span data-ttu-id="bc105-117">hello alkalmazás használ hello [várólista-központú](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-betöltési hello processzorigényes munkáján miniatűrök tooa háttér-folyamat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bc105-117">hello application uses hello [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-load hello CPU-intensive work of creating thumbnails tooa back-end process.</span></span>

## <a name="alternative-architecture-websites-and-webjobs"></a><span data-ttu-id="bc105-118">Alternatív architektúra: Websites és WebJobs</span><span class="sxs-lookup"><span data-stu-id="bc105-118">Alternative architecture: Websites and WebJobs</span></span>
<span data-ttu-id="bc105-119">Ez az oktatóanyag bemutatja, hogyan toorun előtér- és háttér-egy Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="bc105-119">This tutorial shows how toorun both front-end and back-end in an Azure cloud service.</span></span> <span data-ttu-id="bc105-120">A másik lehetőség az előtér-toorun hello egy [Azure-webhelyen](/services/web-sites/) és hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) szolgáltatást (jelenleg előzetes verzió) hello háttér-a.</span><span class="sxs-lookup"><span data-stu-id="bc105-120">An alternative is toorun hello front-end in an [Azure website](/services/web-sites/) and use hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) feature (currently in preview) for hello back-end.</span></span> <span data-ttu-id="bc105-121">Webjobs-feladatok alkalmazó oktatóanyagot, lásd: [Ismerkedés az Azure WebJobs SDK hello](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bc105-121">For a tutorial that uses WebJobs, see [Get Started with hello Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span></span> <span data-ttu-id="bc105-122">Hogyan toochoose hello szolgáltatás, amely legjobban információt adott esetben fér el, a következő témakörben: [Azure Websites, a Cloud Services és a virtuális gépek összevetése](../app-service-web/choose-web-site-cloud-service-vm.md).</span><span class="sxs-lookup"><span data-stu-id="bc105-122">For information about how toochoose hello services that best fit your scenario, see [Azure Websites, Cloud Services, and virtual machines comparison](../app-service-web/choose-web-site-cloud-service-vm.md).</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="bc105-123">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="bc105-123">What you'll learn</span></span>
* <span data-ttu-id="bc105-124">Hogyan tooenable Azure fejlesztési telepítésével, a gép hello Azure SDK-t.</span><span class="sxs-lookup"><span data-stu-id="bc105-124">How tooenable your machine for Azure development by installing hello Azure SDK.</span></span>
* <span data-ttu-id="bc105-125">Hogyan toocreate a Visual Studio cloud service-projekt egy ASP.NET MVC webes és feldolgozói szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="bc105-125">How toocreate a Visual Studio cloud service project with an ASP.NET MVC web role and a worker role.</span></span>
* <span data-ttu-id="bc105-126">Hogyan tootest hello felhőszolgáltatás-projekt helyi, hello Azure storage emulator használatával.</span><span class="sxs-lookup"><span data-stu-id="bc105-126">How tootest hello cloud service project locally, using hello Azure storage emulator.</span></span>
* <span data-ttu-id="bc105-127">Hogyan toopublish hello felhő projekt tooan Azure felhőalapú szolgáltatás, és tesztelése egy Azure storage-fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="bc105-127">How toopublish hello cloud project tooan Azure cloud service and test using an Azure storage account.</span></span>
* <span data-ttu-id="bc105-128">Hogyan tooupload fájlok, illetve hello Azure Blob szolgáltatásban tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="bc105-128">How tooupload files and store them in hello Azure Blob service.</span></span>
* <span data-ttu-id="bc105-129">Hogyan toouse hello Azure Queue szolgáltatás rétegek közötti kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="bc105-129">How toouse hello Azure Queue service for communication between tiers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc105-130">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bc105-130">Prerequisites</span></span>
<span data-ttu-id="bc105-131">hello oktatóanyag feltételezi, hogy tudomásul veszi [alapfogalmaival Azure cloud services](cloud-services-choose-me.md) például *webes szerepkör* és *feldolgozói szerepkör* terminológiát.</span><span class="sxs-lookup"><span data-stu-id="bc105-131">hello tutorial assumes that you understand [basic concepts about Azure cloud services](cloud-services-choose-me.md) such as *web role* and *worker role* terminology.</span></span>  <span data-ttu-id="bc105-132">Azt is feltételezi, hogy tudja, hogyan rendelkező toowork [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) vagy [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) Visual studióban.</span><span class="sxs-lookup"><span data-stu-id="bc105-132">It also assumes that you know how toowork with [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) or [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projects in Visual Studio.</span></span> <span data-ttu-id="bc105-133">hello mintaalkalmazás MVC-t használja, de a legtöbb hello oktatóanyag is vonatkoznak tooWeb űrlapok.</span><span class="sxs-lookup"><span data-stu-id="bc105-133">hello sample application uses MVC, but most of hello tutorial also applies tooWeb Forms.</span></span>

<span data-ttu-id="bc105-134">Hello alkalmazás helyileg Azure-előfizetés nélkül is futtathatja, de egy toodeploy hello alkalmazás toohello felhő lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="bc105-134">You can run hello app locally without an Azure subscription, but you'll need one toodeploy hello application toohello cloud.</span></span> <span data-ttu-id="bc105-135">Ha nincs fiókja, [aktiválhatja az MSDN előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668), vagy [regisztrálhat egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span><span class="sxs-lookup"><span data-stu-id="bc105-135">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span></span>

<span data-ttu-id="bc105-136">hello oktatóanyagban szereplő utasítások vagy a következő termékek hello használata:</span><span class="sxs-lookup"><span data-stu-id="bc105-136">hello tutorial instructions work with either of hello following products:</span></span>

* <span data-ttu-id="bc105-137">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bc105-137">Visual Studio 2013</span></span>
* <span data-ttu-id="bc105-138">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="bc105-138">Visual Studio 2015</span></span>
* <span data-ttu-id="bc105-139">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="bc105-139">Visual Studio 2017</span></span>

<span data-ttu-id="bc105-140">Ha ezek egyikét nincs, a Visual Studio is települ automatikusan hello Azure SDK telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="bc105-140">If you don't have one of these, Visual Studio may be installed automatically when you install hello Azure SDK.</span></span>

## <a name="application-architecture"></a><span data-ttu-id="bc105-141">Alkalmazásarchitektúra</span><span class="sxs-lookup"><span data-stu-id="bc105-141">Application architecture</span></span>
<span data-ttu-id="bc105-142">hello alkalmazást használó Entity Framework Code First toocreate hello táblák és -hozzáférési hello adatok SQL-adatbázisban tárolja a hirdetéseket.</span><span class="sxs-lookup"><span data-stu-id="bc105-142">hello app stores ads in a SQL database, using Entity Framework Code First toocreate hello tables and access hello data.</span></span> <span data-ttu-id="bc105-143">Az egyes hirdetések hello adatbázis tárolók két URL-címet, egy a teljes méretű kép, a másik hello miniatűr hello.</span><span class="sxs-lookup"><span data-stu-id="bc105-143">For each ad, hello database stores two URLs, one for hello full-size image and one for hello thumbnail.</span></span>

![Hirdetés tábla](./media/cloud-services-dotnet-get-started/adtable.png)

<span data-ttu-id="bc105-145">Amikor egy felhasználó feltölt egy képet, hello előtér-futó webes szerepkör tárolja hello lemezképet egy [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), és hello ad adatokat toohello blob URL-CÍMMEL rendelkező hello adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="bc105-145">When a user uploads an image, hello front-end running in a web role stores hello image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores hello ad information in hello database with a URL that points toohello blob.</span></span> <span data-ttu-id="bc105-146">At hello azonos időben, egy üzenet tooan Azure üzenetsor-kezelési ír.</span><span class="sxs-lookup"><span data-stu-id="bc105-146">At hello same time, it writes a message tooan Azure queue.</span></span> <span data-ttu-id="bc105-147">A feldolgozói szerepkör rendszeres időközönként futó háttér-folyamat kérdezze le az új üzenetek hello várólista.</span><span class="sxs-lookup"><span data-stu-id="bc105-147">A back-end process running in a worker role periodically polls hello queue for new messages.</span></span> <span data-ttu-id="bc105-148">Egy új üzenet jelenik meg, amikor hello feldolgozói szerepkör létrehozza a kép miniatűrjét, és a frissítések hello Miniatűr URL-adatbázis mező az adott Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bc105-148">When a new message appears, hello worker role creates a thumbnail for that image and updates hello thumbnail URL database field for that ad.</span></span> <span data-ttu-id="bc105-149">hello alábbi ábra bemutatja hogyan működnek együtt hello hello alkalmazás részei.</span><span class="sxs-lookup"><span data-stu-id="bc105-149">hello following diagram shows how hello parts of hello application interact.</span></span>

![Contoso Ads architektúra](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]

## <a name="download-and-run-hello-completed-solution"></a><span data-ttu-id="bc105-151">Befejeződött hello megoldás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="bc105-151">Download and run hello completed solution</span></span>
1. <span data-ttu-id="bc105-152">Töltse le és csomagolja ki a hello [kész megoldást](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span><span class="sxs-lookup"><span data-stu-id="bc105-152">Download and unzip hello [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span></span>
2. <span data-ttu-id="bc105-153">Indítsa el a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="bc105-153">Start Visual Studio.</span></span>
3. <span data-ttu-id="bc105-154">A hello **fájl** menüben válassza **nyissa meg a projekt**, és keresse meg a letöltött hello megoldás toowhere, majd nyissa meg a megoldásfájlt hello.</span><span class="sxs-lookup"><span data-stu-id="bc105-154">From hello **File** menu choose **Open Project**, navigate toowhere you downloaded hello solution, and then open hello solution file.</span></span>
4. <span data-ttu-id="bc105-155">Nyomja le a CTRL + SHIFT + B toobuild hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="bc105-155">Press CTRL+SHIFT+B toobuild hello solution.</span></span>

    <span data-ttu-id="bc105-156">Alapértelmezés szerint a Visual Studio automatikusan visszaállítja hello NuGet-csomag tartalmát, amely nem szerepel a hello *.zip* fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc105-156">By default, Visual Studio automatically restores hello NuGet package content, which was not included in hello *.zip* file.</span></span> <span data-ttu-id="bc105-157">Hello csomagok nem állnak vissza, ha a telepítést, manuálisan is toohello **NuGet-csomagok kezelése megoldáshoz** párbeszédpanel megnyitásához, majd kattintson a hello **visszaállítása** hello felső jobb oldali gomb.</span><span class="sxs-lookup"><span data-stu-id="bc105-157">If hello packages don't restore, install them manually by going toohello **Manage NuGet Packages for Solution** dialog box and clicking hello **Restore** button at hello top right.</span></span>
5. <span data-ttu-id="bc105-158">A **Megoldáskezelőben**, győződjön meg arról, hogy **ContosoAdsCloudService** hello indítási projekt legyen kijelölve.</span><span class="sxs-lookup"><span data-stu-id="bc105-158">In **Solution Explorer**, make sure that **ContosoAdsCloudService** is selected as hello startup project.</span></span>
6. <span data-ttu-id="bc105-159">Visual Studio 2015-öt használ, vagy a magasabb módosítani a hello SQL Server kapcsolati karakterlánc hello alkalmazásban *Web.config* fájl hello ContosoAdsWeb projekt és hello *ServiceConfiguration.Local.cscfg* hello ContosoAdsCloudService projekt fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc105-159">If you're using Visual Studio 2015 or higher, change hello SQL Server connection string in hello application *Web.config* file of hello ContosoAdsWeb project and in hello *ServiceConfiguration.Local.cscfg* file of hello ContosoAdsCloudService project.</span></span> <span data-ttu-id="bc105-160">Minden esetben módosítsa "(localdb) \v11.0" túl "(localdb) \MSSQLLocalDB".</span><span class="sxs-lookup"><span data-stu-id="bc105-160">In each case, change "(localdb)\v11.0" too"(localdb)\MSSQLLocalDB".</span></span>
7. <span data-ttu-id="bc105-161">Nyomja le a CTRL + F5 toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bc105-161">Press CTRL+F5 toorun hello application.</span></span>

    <span data-ttu-id="bc105-162">Amikor helyileg futtat egy felhőszolgáltatás-projekt, a Visual Studio automatikusan meghívja-e a hello Azure *compute emulator* és az Azure *storage emulator*.</span><span class="sxs-lookup"><span data-stu-id="bc105-162">When you run a cloud service project locally, Visual Studio automatically invokes hello Azure *compute emulator* and Azure *storage emulator*.</span></span> <span data-ttu-id="bc105-163">hello számítási emulátor használ, a számítógép erőforrások toosimulate hello webes és feldolgozói szerepkörök környezeteit.</span><span class="sxs-lookup"><span data-stu-id="bc105-163">hello compute emulator uses your computer's resources toosimulate hello web role and worker role environments.</span></span> <span data-ttu-id="bc105-164">hello storage emulator használ egy [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) adatbázis-toosimulate Azure felhőalapú tárolást.</span><span class="sxs-lookup"><span data-stu-id="bc105-164">hello storage emulator uses a [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database toosimulate Azure cloud storage.</span></span>

    <span data-ttu-id="bc105-165">hello egy felhőszolgáltatás-projekt első futtatásakor szükséges egy percet hello emulátorok toostart fel.</span><span class="sxs-lookup"><span data-stu-id="bc105-165">hello first time you run a cloud service project, it takes a minute or so for hello emulators toostart up.</span></span> <span data-ttu-id="bc105-166">Ha az emulátorok elindulását, hello alapértelmezett böngésző megnyitja toohello alkalmazás kezdőlapját.</span><span class="sxs-lookup"><span data-stu-id="bc105-166">When emulator startup is finished, hello default browser opens toohello application home page.</span></span>

    ![Contoso Ads architektúra](./media/cloud-services-dotnet-get-started/home.png)
8. <span data-ttu-id="bc105-168">Kattintson a **Create an Ad** (Hirdetés létrehozása) gombra.</span><span class="sxs-lookup"><span data-stu-id="bc105-168">Click  **Create an Ad**.</span></span>
9. <span data-ttu-id="bc105-169">Adjon meg néhány tesztadatot, és válassza ki a *.jpg* tooupload lemezképet, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bc105-169">Enter some test data and select a *.jpg* image tooupload, and then click **Create**.</span></span>

    ![Lap létrehozása](./media/cloud-services-dotnet-get-started/create.png)

    <span data-ttu-id="bc105-171">hello app toohello Index lapra kerül, de hello új hirdetés miniatűrje nem jelenik meg, mert a feldolgozása még nem történt meg.</span><span class="sxs-lookup"><span data-stu-id="bc105-171">hello app goes toohello Index page, but it doesn't show a thumbnail for hello new ad because that processing hasn't happened yet.</span></span>
10. <span data-ttu-id="bc105-172">Várjon egy kicsit, majd frissítse a hello Index lap toosee hello miniatűr.</span><span class="sxs-lookup"><span data-stu-id="bc105-172">Wait a moment and then refresh hello Index page toosee hello thumbnail.</span></span>

     ![Index lap](./media/cloud-services-dotnet-get-started/list.png)
11. <span data-ttu-id="bc105-174">Kattintson a **részletek** a ad toosee hello teljes méretű kép.</span><span class="sxs-lookup"><span data-stu-id="bc105-174">Click **Details** for your ad toosee hello full-size image.</span></span>

     ![Részletek lap](./media/cloud-services-dotnet-get-started/details.png)

<span data-ttu-id="bc105-176">Ön már futott hello alkalmazás teljes egészében a helyi számítógépen, a felhő nélküli kapcsolat toohello.</span><span class="sxs-lookup"><span data-stu-id="bc105-176">You've been running hello application entirely on your local computer, with no connection toohello cloud.</span></span> <span data-ttu-id="bc105-177">hello storage emulator hello várólista tárolja, és egy SQL Server Express LocalDB adatbázisban, és hello alkalmazás Blobadatok egy másik LocalDB adatbázisban tárolja az hello ad adatokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-177">hello storage emulator stores hello queue and blob data in a SQL Server Express LocalDB database, and hello application stores hello ad data in another LocalDB database.</span></span> <span data-ttu-id="bc105-178">Entity Framework Code First automatikusan létrehozott hello ad adatbázis hello hello webalkalmazás próbált tooaccess először azt.</span><span class="sxs-lookup"><span data-stu-id="bc105-178">Entity Framework Code First automatically created hello ad database hello first time hello web app tried tooaccess it.</span></span>

<span data-ttu-id="bc105-179">A következő szakasz hello hello megoldás toouse Azure felhőbeli erőforrások üzenetsorok, blobok és hello adatbázis hello felhőben futtatott kell majd konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="bc105-179">In hello following section you'll configure hello solution toouse Azure cloud resources for queues, blobs, and hello application database when it runs in hello cloud.</span></span> <span data-ttu-id="bc105-180">Ha helyileg toocontinue toorun szeretett volna, de felhőalapú tárolási és adatbázis erőforrásainak használatához, megteheti, hogy.</span><span class="sxs-lookup"><span data-stu-id="bc105-180">If you wanted toocontinue toorun locally but use cloud storage and database resources, you could do that.</span></span> <span data-ttu-id="bc105-181">Csak egy függetlenül attól, hogy a kapcsolati karakterláncok, amelyek láthatja beállítása hogyan toodo.</span><span class="sxs-lookup"><span data-stu-id="bc105-181">It's just a matter of setting connection strings, which you'll see how toodo.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="bc105-182">Hello alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="bc105-182">Deploy hello application tooAzure</span></span>
<span data-ttu-id="bc105-183">El kell végeznie a következő lépéseket toorun hello alkalmazás hello felhőben hello:</span><span class="sxs-lookup"><span data-stu-id="bc105-183">You'll do hello following steps toorun hello application in hello cloud:</span></span>

* <span data-ttu-id="bc105-184">Hozzon létre egy Azure-felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bc105-184">Create an Azure cloud service.</span></span>
* <span data-ttu-id="bc105-185">Hozzon létre egy Azure SQL-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="bc105-185">Create an Azure SQL database.</span></span>
* <span data-ttu-id="bc105-186">Hozzon létre egy Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="bc105-186">Create an Azure storage account.</span></span>
* <span data-ttu-id="bc105-187">Konfigurálása hello megoldás toouse az Azure SQL database az Azure-ban futtatott.</span><span class="sxs-lookup"><span data-stu-id="bc105-187">Configure hello solution toouse your Azure SQL database when it runs in Azure.</span></span>
* <span data-ttu-id="bc105-188">Hello megoldás toouse az Azure storage fiók beállítása az Azure-ban futtatott.</span><span class="sxs-lookup"><span data-stu-id="bc105-188">Configure hello solution toouse your Azure storage account when it runs in Azure.</span></span>
* <span data-ttu-id="bc105-189">Hello projekt tooyour Azure felhőalapú szolgáltatás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="bc105-189">Deploy hello project tooyour Azure cloud service.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="bc105-190">Azure-felhőszolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc105-190">Create an Azure cloud service</span></span>
<span data-ttu-id="bc105-191">Azure-felhőszolgáltatás hello környezet hello alkalmazás futni fog.</span><span class="sxs-lookup"><span data-stu-id="bc105-191">An Azure cloud service is hello environment hello application will run in.</span></span>

1. <span data-ttu-id="bc105-192">A böngészőben nyissa meg a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bc105-192">In your browser, open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="bc105-193">Kattintson az **Új > Számítás > Felhőszolgáltatás** elemre.</span><span class="sxs-lookup"><span data-stu-id="bc105-193">Click **New > Compute > Cloud Service**.</span></span>

3. <span data-ttu-id="bc105-194">Hello DNS nevét beviteli mezőbe írja be egy URL-előtagját: hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bc105-194">In hello DNS name input box, enter a URL prefix for hello cloud service.</span></span>

    <span data-ttu-id="bc105-195">Az URL-cím egyedi toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bc105-195">This URL has toobe unique.</span></span>  <span data-ttu-id="bc105-196">Lesz jelenik meg hibaüzenet, ha úgy dönt, hello előtag már használatban van.</span><span class="sxs-lookup"><span data-stu-id="bc105-196">You'll get an error message if hello prefix you choose is already in use.</span></span>
4. <span data-ttu-id="bc105-197">Adjon meg egy új erőforráscsoportot hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bc105-197">Specify a new Resource group for hello  service.</span></span> <span data-ttu-id="bc105-198">Kattintson a **hozzon létre új** és adjon meg egy nevet hello erőforrás csoport beviteli mezőbe, például a CS_contososadsRG.</span><span class="sxs-lookup"><span data-stu-id="bc105-198">Click **Create new** and then type a name in hello Resource group input box, such as CS_contososadsRG.</span></span>

5. <span data-ttu-id="bc105-199">Hello régió, ahol szeretné toodeploy hello alkalmazás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="bc105-199">Choose hello region where you want toodeploy hello application.</span></span>

    <span data-ttu-id="bc105-200">Ez a mező határozza meg, hogy a felhőszolgáltatása melyik adatközpontban fog üzemelni.</span><span class="sxs-lookup"><span data-stu-id="bc105-200">This field specifies which datacenter your cloud service will be hosted in.</span></span> <span data-ttu-id="bc105-201">Termelési alkalmazások esetében válassza hello régió legközelebbi tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="bc105-201">For a production application, you'd choose hello region closest tooyour customers.</span></span> <span data-ttu-id="bc105-202">A jelen oktatóanyag esetében válassza ki a hello régió legközelebbi tooyou.</span><span class="sxs-lookup"><span data-stu-id="bc105-202">For this tutorial, choose hello region closest tooyou.</span></span>
5. <span data-ttu-id="bc105-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bc105-203">Click **Create**.</span></span>

    <span data-ttu-id="bc105-204">A kép a következő hello egy felhőalapú szolgáltatás URL-cím CSvccontosoads.cloudapp.net hello hozza létre.</span><span class="sxs-lookup"><span data-stu-id="bc105-204">In hello following image, a cloud service is created with hello URL CSvccontosoads.cloudapp.net.</span></span>

    ![Új felhőszolgáltatás](./media/cloud-services-dotnet-get-started/newcs.png)

### <a name="create-an-azure-sql-database"></a><span data-ttu-id="bc105-206">Azure SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc105-206">Create an Azure SQL database</span></span>
<span data-ttu-id="bc105-207">Hello alkalmazás futtatásakor hello felhőben, felhőalapú adatbázist fog használni.</span><span class="sxs-lookup"><span data-stu-id="bc105-207">When hello app runs in hello cloud, it will use a cloud-based database.</span></span>

1. <span data-ttu-id="bc105-208">A hello [Azure-portálon](https://portal.azure.com), kattintson a **új > adatbázisok > SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="bc105-208">In hello [Azure portal](https://portal.azure.com), click **New > Databases > SQL Database**.</span></span>
2. <span data-ttu-id="bc105-209">A hello **adatbázisnév** adja meg a *contosoads*.</span><span class="sxs-lookup"><span data-stu-id="bc105-209">In hello **Database Name** box, enter *contosoads*.</span></span>
3. <span data-ttu-id="bc105-210">A hello **erőforráscsoport**, kattintson a **meglévő** és select hello erőforráscsoport hello felhőalapú szolgáltatás használt.</span><span class="sxs-lookup"><span data-stu-id="bc105-210">In hello **Resource group**, click **Use existing** and select hello resource group used for hello cloud service.</span></span>
4. <span data-ttu-id="bc105-211">Hello lemezképet, a következő kattintson **kiszolgáló - kötelező beállítások konfigurálása** és **hozzon létre egy új kiszolgálót**.</span><span class="sxs-lookup"><span data-stu-id="bc105-211">In hello following image, click **Server - Configure required settings** and **Create a new server**.</span></span>

    ![Bújtatási toodatabase kiszolgáló](./media/cloud-services-dotnet-get-started/newdb.png)

    <span data-ttu-id="bc105-213">Azt is megteheti Ha az előfizetés már rendelkezik egy kiszolgálóra, kiválaszthatja, hogy a kiszolgáló hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="bc105-213">Alternatively, if your subscription already has a server, you can select that server from hello drop-down list.</span></span>
5. <span data-ttu-id="bc105-214">A hello **kiszolgálónév** adja meg a *csvccontosodbserver*.</span><span class="sxs-lookup"><span data-stu-id="bc105-214">In hello **Server name** box, enter *csvccontosodbserver*.</span></span>

6. <span data-ttu-id="bc105-215">Adjon meg egy rendszergazdai **Bejelentkezési nevet** és **Jelszót**.</span><span class="sxs-lookup"><span data-stu-id="bc105-215">Enter an administrator **Login Name** and **Password**.</span></span>

    <span data-ttu-id="bc105-216">Ha az **Új kiszolgáló létrehozása** elemet választotta, nem meglévő nevet és jelszót kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="bc105-216">If you selected **Create a new server**, you aren't entering an existing name and password here.</span></span> <span data-ttu-id="bc105-217">Adta meg egy új nevet és jelszót, amely akkor most toouse később hello adatbázis elérésekor.</span><span class="sxs-lookup"><span data-stu-id="bc105-217">You're entering a new name and password that you're defining now toouse later when you access hello database.</span></span> <span data-ttu-id="bc105-218">Ha egy korábban létrehozott server, kéri hello jelszó toohello rendszergazdai felhasználói fiókhoz már létrehozott.</span><span class="sxs-lookup"><span data-stu-id="bc105-218">If you selected a server that you created previously, you'll be prompted for hello password toohello administrative user account you already created.</span></span>
7. <span data-ttu-id="bc105-219">Válasszon azonos hello **hely** hello felhőszolgáltatás választott.</span><span class="sxs-lookup"><span data-stu-id="bc105-219">Choose hello same **Location** that you chose for hello cloud service.</span></span>

    <span data-ttu-id="bc105-220">Ha a hello felhőalapú szolgáltatás és az adatbázis van különböző adatközpontokban (különböző régiókban), a késés mértéke megnő, és hello adatközponton kívül használt sávszélességért fizetnie kell.</span><span class="sxs-lookup"><span data-stu-id="bc105-220">When hello cloud service and database are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside hello data center.</span></span> <span data-ttu-id="bc105-221">Az adatközponton belül használt sávszélesség ingyenes.</span><span class="sxs-lookup"><span data-stu-id="bc105-221">Bandwidth within a data center is free.</span></span>
8. <span data-ttu-id="bc105-222">Ellenőrizze **engedélyezése az azure szolgáltatások tooaccess server**.</span><span class="sxs-lookup"><span data-stu-id="bc105-222">Check **Allow azure services tooaccess server**.</span></span>
9. <span data-ttu-id="bc105-223">Kattintson a **válasszon** hello új kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="bc105-223">Click **Select** for hello new server.</span></span>

    ![Új SQL Database-kiszolgáló](./media/cloud-services-dotnet-get-started/newdbserver.png)
10. <span data-ttu-id="bc105-225">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc105-225">Click **Create**.</span></span>

### <a name="create-an-azure-storage-account"></a><span data-ttu-id="bc105-226">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc105-226">Create an Azure storage account</span></span>
<span data-ttu-id="bc105-227">Azure-tárfiók forrásokat biztosít hello felhőben üzenetsor és a blob adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="bc105-227">An Azure storage account provides resources for storing queue and blob data in hello cloud.</span></span>

<span data-ttu-id="bc105-228">Egy valós alkalmazás esetében általában külön fiókot hozna létre az alkalmazás adatai és a naplózási adatok, illetve a tesztadatok és a termelési adatok számára is.</span><span class="sxs-lookup"><span data-stu-id="bc105-228">In a real-world application, you would typically create separate accounts for application data versus logging data, and separate accounts for test data versus production data.</span></span> <span data-ttu-id="bc105-229">Ebben az oktatóanyagban csak egy fiókot fog használni.</span><span class="sxs-lookup"><span data-stu-id="bc105-229">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="bc105-230">A hello [Azure-portálon](https://portal.azure.com), kattintson a **új > tárolás > tárfiók - blob, a fájl, a tábla, a várólista**.</span><span class="sxs-lookup"><span data-stu-id="bc105-230">In hello [Azure portal](https://portal.azure.com), click **New > Storage > Storage account - blob, file, table, queue**.</span></span>
2. <span data-ttu-id="bc105-231">A hello **neve** mezőbe írjon be egy URL-előtagot.</span><span class="sxs-lookup"><span data-stu-id="bc105-231">In hello **Name** box, enter a URL prefix.</span></span>

    <span data-ttu-id="bc105-232">A hello mező alatt látható előtag plus hello szöveg hello egyedi URL-cím tooyour tárfiók lesz.</span><span class="sxs-lookup"><span data-stu-id="bc105-232">This prefix plus hello text you see under hello box will be hello unique URL tooyour storage account.</span></span> <span data-ttu-id="bc105-233">Ha valaki más hello előtag már használatban van, akkor kell toochoose különböző előtag.</span><span class="sxs-lookup"><span data-stu-id="bc105-233">If hello prefix you enter has already been used by someone else, you'll have toochoose a different prefix.</span></span>
3. <span data-ttu-id="bc105-234">Set hello **telepítési modell** túl*klasszikus*.</span><span class="sxs-lookup"><span data-stu-id="bc105-234">Set hello **Deployment model** too*Classic*.</span></span>

4. <span data-ttu-id="bc105-235">Set hello **replikációs** legördülő lista túl**helyileg redundáns tárolás**.</span><span class="sxs-lookup"><span data-stu-id="bc105-235">Set hello **Replication** drop-down list too**Locally redundant storage**.</span></span>

    <span data-ttu-id="bc105-236">A georeplikáció engedélyezve van a tárfiók, hello tárolt tartalom esetén meg kell replikált tooa másodlagos adatközpontba tooenable feladatátvételi hello elsődleges helyen jelentős katasztrófa esetén.</span><span class="sxs-lookup"><span data-stu-id="bc105-236">When geo-replication is enabled for a storage account, hello stored content is replicated tooa secondary datacenter tooenable failover if a major disaster occurs in hello primary location.</span></span> <span data-ttu-id="bc105-237">A georeplikáció további költségeket vonhat maga után.</span><span class="sxs-lookup"><span data-stu-id="bc105-237">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="bc105-238">A vizsgálati és fejlesztői fiókok esetében általában nem érdemes toopay a georeplikációért.</span><span class="sxs-lookup"><span data-stu-id="bc105-238">For test and development accounts, you generally don't want toopay for geo-replication.</span></span> <span data-ttu-id="bc105-239">További információ: [Tárfiók létrehozása, kezelése vagy törlése](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="bc105-239">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>

5. <span data-ttu-id="bc105-240">A hello **erőforráscsoport**, kattintson a **meglévő** és select hello erőforráscsoport hello felhőalapú szolgáltatás használt.</span><span class="sxs-lookup"><span data-stu-id="bc105-240">In hello **Resource group**, click **Use existing** and select hello resource group used for hello cloud service.</span></span>
6. <span data-ttu-id="bc105-241">Set hello **hely** legördülő lista toohello ugyanabban a régióban hello felhőszolgáltatás számára is választott.</span><span class="sxs-lookup"><span data-stu-id="bc105-241">Set hello **Location** drop-down list toohello same region you chose for hello cloud service.</span></span>

    <span data-ttu-id="bc105-242">Ha a hello felhőalapú szolgáltatás és a tárolási fiók különböző adatközpontokban van (különböző régiókban), a késés mértéke megnő, és hello adatközponton kívül használt sávszélességért fizetnie kell.</span><span class="sxs-lookup"><span data-stu-id="bc105-242">When hello cloud service and storage account are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside hello data center.</span></span> <span data-ttu-id="bc105-243">Az adatközponton belül használt sávszélesség ingyenes.</span><span class="sxs-lookup"><span data-stu-id="bc105-243">Bandwidth within a data center is free.</span></span>

    <span data-ttu-id="bc105-244">Azure-affinitáscsoportok egy erőforrást egy adatközpontban, ami csökkentheti a késés mechanizmus toominimize hello távolságát adja meg.</span><span class="sxs-lookup"><span data-stu-id="bc105-244">Azure affinity groups provide a mechanism toominimize hello distance between resources in a data center, which can reduce latency.</span></span> <span data-ttu-id="bc105-245">A jelen oktatóanyag nem használ affinitáscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-245">This tutorial does not use affinity groups.</span></span> <span data-ttu-id="bc105-246">További információkért lásd: [hogyan tooCreate kapcsolatot csoportosítani az Azure-ban](http://msdn.microsoft.com/library/jj156209.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc105-246">For more information, see [How tooCreate an Affinity Group in Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span></span>
7. <span data-ttu-id="bc105-247">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bc105-247">Click **Create**.</span></span>

    ![Új tárfiók](./media/cloud-services-dotnet-get-started/newstorage.png)

    <span data-ttu-id="bc105-249">Hello kép, a tárfiók létrehozása hello URL-CÍMMEL rendelkező `csvccontosoads.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="bc105-249">In hello image, a storage account is created with hello URL `csvccontosoads.core.windows.net`.</span></span>

### <a name="configure-hello-solution-toouse-your-azure-sql-database-when-it-runs-in-azure"></a><span data-ttu-id="bc105-250">Konfigurálja a az Azure SQL-adatbázis hello megoldás toouse futáskor az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="bc105-250">Configure hello solution toouse your Azure SQL database when it runs in Azure</span></span>
<span data-ttu-id="bc105-251">webes projekt hello és hello feldolgozói szerepkör projekt minden a saját adatbázis-kapcsolati karakterláncot, és minden egyes kell toopoint toohello Azure SQL database az Azure-ban hello alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="bc105-251">hello web project and hello worker role project each has its own database connection string, and each needs toopoint toohello Azure SQL database when hello app runs in Azure.</span></span>

<span data-ttu-id="bc105-252">Fogja használni a [Web.config transzformálása](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) hello webes szerepkör és a felhő környezet szolgáltatásbeállításra hello feldolgozói szerepkör esetében.</span><span class="sxs-lookup"><span data-stu-id="bc105-252">You'll use a [Web.config transform](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) for hello web role and a cloud service environment setting for hello worker role.</span></span>

> [!NOTE]
> <span data-ttu-id="bc105-253">A jelen szakaszban és a következő szakaszban hello tárolt hitelesítő adatokat projektfájlokban.</span><span class="sxs-lookup"><span data-stu-id="bc105-253">In this section and hello next section, you store credentials in project files.</span></span> <span data-ttu-id="bc105-254">[Ne tároljon bizalmas adatokat nyilvános forráskódú adattárakban.](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets)</span><span class="sxs-lookup"><span data-stu-id="bc105-254">[Don't store sensitive data in public source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span>
>
>

1. <span data-ttu-id="bc105-255">Hello ContosoAdsWeb projektben nyissa meg a hello *Web.Release.config* hello alkalmazás átalakítófájlt *Web.config* fájljához, törölje a hello Megjegyzésblokk, amely tartalmaz egy `<connectionStrings>` elem és a Beillesztés a következő kód helyette hello.</span><span class="sxs-lookup"><span data-stu-id="bc105-255">In hello ContosoAdsWeb project, open hello *Web.Release.config* transform file for hello application *Web.config* file, delete hello comment block that contains a `<connectionStrings>` element, and paste hello following code in its place.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="{connectionstring}"
        providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
    </connectionStrings>
    ```

    <span data-ttu-id="bc105-256">Hagyja nyitva a szerkesztéshez hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc105-256">Leave hello file open for editing.</span></span>
2. <span data-ttu-id="bc105-257">A hello [Azure-portálon](https://portal.azure.com), kattintson a **SQL-adatbázisok** hello bal oldali ablaktáblában kattintson ehhez az oktatóanyaghoz létrehozott hello adatbázisra, majd **kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="bc105-257">In hello [Azure portal](https://portal.azure.com), click **SQL Databases** in hello left pane, click hello database you created for this tutorial, and then click **Show connection strings**.</span></span>

    ![Kapcsolati karakterláncok megjelenítése](./media/cloud-services-dotnet-get-started/showcs.png)

    <span data-ttu-id="bc105-259">hello portál megjeleníti a kapcsolati karakterláncokat helyőrzővel helyettesített hello jelszó.</span><span class="sxs-lookup"><span data-stu-id="bc105-259">hello portal displays connection strings, with a placeholder for hello password.</span></span>

    ![Kapcsolati karakterláncok](./media/cloud-services-dotnet-get-started/connstrings.png)
3. <span data-ttu-id="bc105-261">A hello *Web.Release.config* fájl átalakítása, akkor törölje `{connectionstring}` és illessze be a hely hello hello Azure portálról származó ADO.NET kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bc105-261">In hello *Web.Release.config* transform file, delete `{connectionstring}` and paste in its place hello ADO.NET connection string from hello Azure portal.</span></span>
4. <span data-ttu-id="bc105-262">Kapcsolat-karakterláncban hello hello beillesztett *Web.Release.config* fájl átalakítása, cserélje le `{your_password_here}` hello új SQL-adatbázis létrehozott hello jelszóval.</span><span class="sxs-lookup"><span data-stu-id="bc105-262">In hello connection string that you pasted into hello *Web.Release.config* transform file, replace `{your_password_here}` with hello password you created for hello new SQL database.</span></span>
5. <span data-ttu-id="bc105-263">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="bc105-263">Save hello file.</span></span>  
6. <span data-ttu-id="bc105-264">Válassza ki, és másolja a hello kapcsolati karakterláncot (nélkül hello idézőjelek közé foglalt) hello feldolgozói szerepkör projekt konfigurálásának lépéseit a következő hello használható.</span><span class="sxs-lookup"><span data-stu-id="bc105-264">Select and copy hello connection string (without hello surrounding quotation marks) for use in hello following steps for configuring hello worker role project.</span></span>
7. <span data-ttu-id="bc105-265">A **Megoldáskezelőben**, a **szerepkörök** hello felhőszolgáltatás-projekt, kattintson a jobb egérgombbal **ContosoAdsWorker** majd **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="bc105-265">In **Solution Explorer**, under **Roles** in hello cloud service project, right-click **ContosoAdsWorker** and then click **Properties**.</span></span>

    ![Szerepkör tulajdonságai](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)
8. <span data-ttu-id="bc105-267">Kattintson a hello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="bc105-267">Click hello **Settings** tab.</span></span>
9. <span data-ttu-id="bc105-268">Változás **szolgáltatáskonfiguráció** túl**felhő**.</span><span class="sxs-lookup"><span data-stu-id="bc105-268">Change **Service Configuration** too**Cloud**.</span></span>
10. <span data-ttu-id="bc105-269">Jelölje be hello **érték** hello mezőt `ContosoAdsDbConnectionString` beállításával, és illessze be az hello hello az oktatóanyag előző szakaszából másolt hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bc105-269">Select hello **Value** field for hello `ContosoAdsDbConnectionString` setting, and then paste hello connection string that you copied from hello previous section of hello tutorial.</span></span>

     ![A feldolgozói szerepkör adatbázis-kapcsolati karakterlánca](./media/cloud-services-dotnet-get-started/workerdbcs.png)
11. <span data-ttu-id="bc105-271">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-271">Save your changes.</span></span>  

### <a name="configure-hello-solution-toouse-your-azure-storage-account-when-it-runs-in-azure"></a><span data-ttu-id="bc105-272">Konfigurálja a Azure-tárfiókot hello megoldás toouse futáskor az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="bc105-272">Configure hello solution toouse your Azure storage account when it runs in Azure</span></span>
<span data-ttu-id="bc105-273">Az Azure storage kapcsolati karakterláncainak hello webes szerepkör projekt és hello feldolgozói szerepkör projekt hello felhőszolgáltatás-projekt környezeti beállításokban vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="bc105-273">Azure storage account connection strings for both hello web role project and hello worker role project are stored in environment settings in hello cloud service project.</span></span> <span data-ttu-id="bc105-274">Minden olyan projekthez nincs az alkalmazás futásakor, hello helyileg használt beállítások toobe külön készletét, és mikor fusson a hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="bc105-274">For each project, there is a separate set of settings toobe used when hello application runs locally and when it runs in hello cloud.</span></span> <span data-ttu-id="bc105-275">Hello felhőkörnyezet beállításait a webes és feldolgozói szerepkör projektek frissíteni fogja.</span><span class="sxs-lookup"><span data-stu-id="bc105-275">You'll update hello cloud environment settings for both web and worker role projects.</span></span>

1. <span data-ttu-id="bc105-276">A **Megoldáskezelőben**, kattintson a jobb gombbal **ContosoAdsWeb** alatt **szerepkörök** a hello **ContosoAdsCloudService** projektre, és kattintson a **Tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="bc105-276">In **Solution Explorer**, right-click **ContosoAdsWeb** under **Roles** in hello **ContosoAdsCloudService** project, and then click **Properties**.</span></span>

    ![Szerepkör tulajdonságai](./media/cloud-services-dotnet-get-started/roleproperties.png)
2. <span data-ttu-id="bc105-278">Kattintson a hello **beállítások** fülre. A hello **szolgáltatáskonfiguráció** legördülő listán válassza ki **felhő**.</span><span class="sxs-lookup"><span data-stu-id="bc105-278">Click hello **Settings** tab. In hello **Service Configuration** drop-down box, choose **Cloud**.</span></span>

    ![Felhő konfigurálása](./media/cloud-services-dotnet-get-started/sccloud.png)
3. <span data-ttu-id="bc105-280">Jelölje be hello **StorageConnectionString** bejegyzést, és megjelenik egy három pontot (**...** ) hello jobb oldali végén hello gombra.</span><span class="sxs-lookup"><span data-stu-id="bc105-280">Select hello **StorageConnectionString** entry, and you'll see an ellipsis (**...**) button at hello right end of hello line.</span></span> <span data-ttu-id="bc105-281">Kattintson a hello három pont gombra tooopen hello **tárolási fiók kapcsolati karakterlánc létrehozása** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bc105-281">Click hello ellipsis button tooopen hello **Create Storage Account Connection String** dialog box.</span></span>

    ![A Kapcsolati karakterlánc létrehozása mező megnyitása](./media/cloud-services-dotnet-get-started/opencscreate.png)
4. <span data-ttu-id="bc105-283">A hello **tárolási kapcsolati karakterlánc létrehozása** párbeszédpanel, kattintson a **az előfizetés**, válassza ki a korábban létrehozott hello tárfiók, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc105-283">In hello **Create Storage Connection String** dialog box, click **Your subscription**, choose hello storage account that you created earlier, and then click **OK**.</span></span> <span data-ttu-id="bc105-284">Ha még nincs bejelentkezve, a rendszer az Azure-fiókja hitelesítő adatait kéri.</span><span class="sxs-lookup"><span data-stu-id="bc105-284">If you're not already logged in, you'll be prompted for your Azure account credentials.</span></span>

    ![Tárfiók kapcsolati karakterláncának létrehozása](./media/cloud-services-dotnet-get-started/createstoragecs.png)
5. <span data-ttu-id="bc105-286">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-286">Save your changes.</span></span>
6. <span data-ttu-id="bc105-287">Kövesse hello ugyanazt az eljárást, a hello `StorageConnectionString` kapcsolati karakterlánc tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bc105-287">Follow hello same procedure that you used for hello `StorageConnectionString` connection string tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` connection string.</span></span>

    <span data-ttu-id="bc105-288">Ez a kapcsolati karakterlánc naplózásra használható.</span><span class="sxs-lookup"><span data-stu-id="bc105-288">This connection string is used for logging.</span></span>
7. <span data-ttu-id="bc105-289">Kövesse hello ugyanazt az eljárást, a hello **ContosoAdsWeb** szerepkör tooset mindkét kapcsolati karakterláncának hello **ContosoAdsWorker** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="bc105-289">Follow hello same procedure that you used for hello **ContosoAdsWeb** role tooset both connection strings for hello **ContosoAdsWorker** role.</span></span> <span data-ttu-id="bc105-290">Ne feledje tooset **szolgáltatáskonfiguráció** túl**felhő**.</span><span class="sxs-lookup"><span data-stu-id="bc105-290">Don't forget tooset **Service Configuration** too**Cloud**.</span></span>

<span data-ttu-id="bc105-291">Visual Studio felhasználói felületén hello segítségével konfigurált hello beállítások következő fájlok hello ContosoAdsCloudService projektben hello vannak tárolva:</span><span class="sxs-lookup"><span data-stu-id="bc105-291">hello role environment settings that you have configured using hello Visual Studio UI are stored in hello following files in hello ContosoAdsCloudService project:</span></span>

* <span data-ttu-id="bc105-292">*ServiceDefinition.csdef* -hello beállításneveket határozza meg.</span><span class="sxs-lookup"><span data-stu-id="bc105-292">*ServiceDefinition.csdef* - Defines hello setting names.</span></span>
* <span data-ttu-id="bc105-293">*ServiceConfiguration.Cloud.cscfg* – értékeket biztosít hello felhőben hello alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="bc105-293">*ServiceConfiguration.Cloud.cscfg* - Provides values for when hello app runs in hello cloud.</span></span>
* <span data-ttu-id="bc105-294">*ServiceConfiguration.Local.cscfg* – értékeket biztosít amikor hello alkalmazás helyi futtatásához.</span><span class="sxs-lookup"><span data-stu-id="bc105-294">*ServiceConfiguration.Local.cscfg* - Provides values for when hello app runs locally.</span></span>

<span data-ttu-id="bc105-295">Hello ServiceDefinition.csdef például a következő definíciókat hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="bc105-295">For example, hello ServiceDefinition.csdef includes hello following definitions:</span></span>

```xml
<ConfigurationSettings>
    <Setting name="StorageConnectionString" />
    <Setting name="ContosoAdsDbConnectionString" />
</ConfigurationSettings>
```

<span data-ttu-id="bc105-296">És hello *ServiceConfiguration.Cloud.cscfg* fájl ezeket a beállításokat, a Visual Studio megadott hello értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bc105-296">And hello *ServiceConfiguration.Cloud.cscfg* file includes hello values you entered for those settings in Visual Studio.</span></span>

```xml
<Role name="ContosoAdsWorker">
    <Instances count="1" />
    <ConfigurationSettings>
        <Setting name="StorageConnectionString" value="{yourconnectionstring}" />
        <Setting name="ContosoAdsDbConnectionString" value="{yourconnectionstring}" />
        <!-- other settings not shown -->

    </ConfigurationSettings>
    <!-- other settings not shown -->

</Role>
```

<span data-ttu-id="bc105-297">Hello `<Instances>` beállítással a virtuális gépek futó Azure hello feldolgozói szerepkör kódját hello száma.</span><span class="sxs-lookup"><span data-stu-id="bc105-297">hello `<Instances>` setting specifies hello number of virtual machines that Azure will run hello worker role code on.</span></span> <span data-ttu-id="bc105-298">Hello [további lépések](#next-steps) szakasz tartalmaz hivatkozásokat toomore információkat a felhőalapú szolgáltatás, kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="bc105-298">hello [Next steps](#next-steps) section includes links toomore information about scaling out a cloud service,</span></span>

### <a name="deploy-hello-project-tooazure"></a><span data-ttu-id="bc105-299">Hello projekt tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="bc105-299">Deploy hello project tooAzure</span></span>
1. <span data-ttu-id="bc105-300">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **ContosoAdsCloudService** felhőprojektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="bc105-300">In **Solution Explorer**, right-click hello **ContosoAdsCloudService** cloud project and then select **Publish**.</span></span>

   ![Közzététel menü](./media/cloud-services-dotnet-get-started/pubmenu.png)
2. <span data-ttu-id="bc105-302">A hello **jelentkezzen be a** hello lépését **Azure-alkalmazás közzététele** varázsló, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="bc105-302">In hello **Sign in** step of hello **Publish Azure Application** wizard, click **Next**.</span></span>

    ![Bejelentkezés lépés](./media/cloud-services-dotnet-get-started/pubsignin.png)
3. <span data-ttu-id="bc105-304">A hello **beállítások** lépés hello varázsló, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="bc105-304">In hello **Settings** step of hello wizard, click **Next**.</span></span>

    ![Beállítások lépés](./media/cloud-services-dotnet-get-started/pubsettings.png)

    <span data-ttu-id="bc105-306">alapértelmezett beállítások a hello hello **speciális** lapon ez az oktatóanyag rendben.</span><span class="sxs-lookup"><span data-stu-id="bc105-306">hello default settings in hello **Advanced** tab are fine for this tutorial.</span></span> <span data-ttu-id="bc105-307">További információ a Speciális lap hello: [Azure alkalmazás közzététele varázsló](http://msdn.microsoft.com/library/hh535756.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc105-307">For information about hello advanced tab, see [Publish Azure Application Wizard](http://msdn.microsoft.com/library/hh535756.aspx).</span></span>
4. <span data-ttu-id="bc105-308">A hello **összegzés** lépésre, a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="bc105-308">In hello **Summary** step, click **Publish**.</span></span>

    ![Összegzés lépés](./media/cloud-services-dotnet-get-started/pubsummary.png)

   <span data-ttu-id="bc105-310">Hello **Azure tevékenységnapló** ablak a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="bc105-310">hello **Azure Activity Log** window opens in Visual Studio.</span></span>
5. <span data-ttu-id="bc105-311">Hello jobbra mutató nyíl ikon tooexpand hello központi telepítés a Részletek gombra.</span><span class="sxs-lookup"><span data-stu-id="bc105-311">Click hello right arrow icon tooexpand hello deployment details.</span></span>

    <span data-ttu-id="bc105-312">hello telepítési too5 perc vagy több toocomplete is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="bc105-312">hello deployment can take up too5 minutes or more toocomplete.</span></span>

    ![Azure tevékenységnapló ablak](./media/cloud-services-dotnet-get-started/waal.png)
6. <span data-ttu-id="bc105-314">Ha hello központi telepítési állapot befejeződött, kattintson a hello **webes alkalmazás URL-címhez** toostart hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bc105-314">When hello deployment status is complete, click hello **Web app URL** toostart hello application.</span></span>
7. <span data-ttu-id="bc105-315">Most tesztelheti hello app létrehozásával, megtekintésével és szerkesztésével hirdetések, hello alkalmazás helyileg futtatta hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="bc105-315">You can now test hello app by creating, viewing, and editing some ads, as you did when you ran hello application locally.</span></span>

> [!NOTE]
> <span data-ttu-id="bc105-316">Amikor végzett a tesztelési, törölje vagy állítsa le a hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bc105-316">When you're finished testing, delete or stop hello cloud service.</span></span> <span data-ttu-id="bc105-317">Akkor is, ha nem használ hello felhőalapú szolgáltatás, keletkezhetnek költségek, mivel a virtuálisgép-erőforrások vannak lefoglalva számára.</span><span class="sxs-lookup"><span data-stu-id="bc105-317">Even if you're not using hello cloud service, it's accruing charges because virtual machine resources are reserved for it.</span></span> <span data-ttu-id="bc105-318">Ha hagyja, hogy továbbra is fusson, bárki, aki rátalál az URL-címére, létrehozhat és megtekinthet hirdetéseket.</span><span class="sxs-lookup"><span data-stu-id="bc105-318">And if you leave it running, anyone who finds your URL can create and view ads.</span></span> <span data-ttu-id="bc105-319">A hello [Azure-portálon](https://portal.azure.com), toohello lépjen **áttekintése** a felhőszolgáltatás fülre, majd hello **törlése** hello oldal hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="bc105-319">In hello [Azure portal](https://portal.azure.com), go toohello **Overview** tab for your cloud service, and then click hello **Delete** button at hello top of hello page.</span></span> <span data-ttu-id="bc105-320">Ha csak tootemporarily megakadályozni, hogy mások hozzáférni hello webhelyhez, kattintson a **leállítása** helyette.</span><span class="sxs-lookup"><span data-stu-id="bc105-320">If you just want tootemporarily prevent others from accessing hello site, click **Stop** instead.</span></span> <span data-ttu-id="bc105-321">Ebben az esetben díjakat továbbra is tooaccrue.</span><span class="sxs-lookup"><span data-stu-id="bc105-321">In that case, charges will continue tooaccrue.</span></span> <span data-ttu-id="bc105-322">Egy hasonló művelet toodelete hello SQL-adatbázis és a tárolási fiók is hajtsa végre, amikor már nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="bc105-322">You can follow a similar procedure toodelete hello SQL database and storage account when you no longer need them.</span></span>
>
>

## <a name="create-hello-application-from-scratch"></a><span data-ttu-id="bc105-323">Teljesen új hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc105-323">Create hello application from scratch</span></span>
<span data-ttu-id="bc105-324">Ha még nem töltötte le [befejeződött hello alkalmazás](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), most tegye.</span><span class="sxs-lookup"><span data-stu-id="bc105-324">If you haven't already downloaded [hello completed application](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), do that now.</span></span> <span data-ttu-id="bc105-325">Ön fájlokat fog átmásolni hello letöltött projektet hello új projektbe.</span><span class="sxs-lookup"><span data-stu-id="bc105-325">You'll copy files from hello downloaded project into hello new project.</span></span>

<span data-ttu-id="bc105-326">A lépéseket követve hello hello Contoso Ads alkalmazás létrehozása foglal magában:</span><span class="sxs-lookup"><span data-stu-id="bc105-326">Creating hello Contoso Ads application involves hello following steps:</span></span>

* <span data-ttu-id="bc105-327">Hozzon létre egy Visual Studio felhőszolgáltatás-megoldást.</span><span class="sxs-lookup"><span data-stu-id="bc105-327">Create a cloud service Visual Studio solution.</span></span>
* <span data-ttu-id="bc105-328">Frissítse és adja hozzá a NuGet-csomagokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-328">Update and add NuGet packages.</span></span>
* <span data-ttu-id="bc105-329">Állítsa be a projekt hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="bc105-329">Set project references.</span></span>
* <span data-ttu-id="bc105-330">Konfigurálja a kapcsolati karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-330">Configure connection strings.</span></span>
* <span data-ttu-id="bc105-331">Adja hozzá a kódfájlokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-331">Add code files.</span></span>

<span data-ttu-id="bc105-332">Hello megoldás létrehozása után áttekinti hello kód, mely az egyedi toocloud service projektek és Azure blobokat és üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-332">After hello solution is created, you'll review hello code that is unique toocloud service projects and Azure blobs and queues.</span></span>

### <a name="create-a-cloud-service-visual-studio-solution"></a><span data-ttu-id="bc105-333">Visual Studio felhőszolgáltatás-megoldás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc105-333">Create a cloud service Visual Studio solution</span></span>
1. <span data-ttu-id="bc105-334">A Visual Studio felületén válassza **új projekt** a hello **fájl** menü.</span><span class="sxs-lookup"><span data-stu-id="bc105-334">In Visual Studio, choose **New Project** from hello **File** menu.</span></span>
2. <span data-ttu-id="bc105-335">Hello hello bal oldali ablaktáblájában **új projekt** párbeszédpanelen bontsa ki **Visual C#** válassza **felhő** sablonokat, és válassza a hello **Azure Cloud Service** sablont.</span><span class="sxs-lookup"><span data-stu-id="bc105-335">In hello left pane of hello **New Project** dialog box, expand **Visual C#** and choose **Cloud** templates, and then choose hello **Azure Cloud Service** template.</span></span>
3. <span data-ttu-id="bc105-336">Hello projektet és a megoldás ContosoAdsCloudService nevet, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc105-336">Name hello project and solution ContosoAdsCloudService, and then click **OK**.</span></span>

    ![Új projekt](./media/cloud-services-dotnet-get-started/newproject.png)
4. <span data-ttu-id="bc105-338">A hello **új Azure Cloud Service** párbeszédpanelen adjon hozzá egy webes és feldolgozói szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="bc105-338">In hello **New Azure Cloud Service** dialog box, add a web role and a worker role.</span></span> <span data-ttu-id="bc105-339">Hello webes szerepkörnek adja a ContosoAdsWeb, és a hello feldolgozói szerepkör ContosoAdsWorker nevet.</span><span class="sxs-lookup"><span data-stu-id="bc105-339">Name hello web role ContosoAdsWeb, and name hello worker role ContosoAdsWorker.</span></span> <span data-ttu-id="bc105-340">(Használ hello ceruza ikonra hello jobb oldali toochange hello alapértelmezett nevének hello szerepkörök.)</span><span class="sxs-lookup"><span data-stu-id="bc105-340">(Use hello pencil icon in hello right-hand pane toochange hello default names of hello roles.)</span></span>

    ![Új Cloud Service-projekt](./media/cloud-services-dotnet-get-started/newcsproj.png)
5. <span data-ttu-id="bc105-342">Amikor megjelenik a hello **új ASP.NET projekt** párbeszédpanel hello webes szerepkör, válassza ki a hello MVC-sablont, és kattintson **hitelesítés módosítása**.</span><span class="sxs-lookup"><span data-stu-id="bc105-342">When you see hello **New ASP.NET Project** dialog box for hello web role, choose hello MVC template, and then click **Change Authentication**.</span></span>

    ![Hitelesítés módosítása](./media/cloud-services-dotnet-get-started/chgauth.png)
6. <span data-ttu-id="bc105-344">A hello **hitelesítés módosítása** párbeszédpanelen válassza ki **nem hitelesítési**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc105-344">In hello **Change Authentication** dialog box, choose **No Authentication**, and then click **OK**.</span></span>

    ![Nincs hitelesítés](./media/cloud-services-dotnet-get-started/noauth.png)
7. <span data-ttu-id="bc105-346">A hello **új ASP.NET projekt** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc105-346">In hello **New ASP.NET Project** dialog, click **OK**.</span></span>
8. <span data-ttu-id="bc105-347">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello megoldás (nem egy hello projektek), és válassza a **Hozzáadás – új projekt**.</span><span class="sxs-lookup"><span data-stu-id="bc105-347">In **Solution Explorer**, right-click hello solution (not one of hello projects), and choose **Add - New Project**.</span></span>
9. <span data-ttu-id="bc105-348">A hello **új** párbeszédpanelen válassza ki **Windows** alatt **Visual C#** a hello bal oldali ablaktáblán, és kattintson a hello **Class Library** sablon.</span><span class="sxs-lookup"><span data-stu-id="bc105-348">In hello **Add New Project** dialog box, choose **Windows** under **Visual C#** in hello left pane, and then click hello **Class Library** template.</span></span>  
10. <span data-ttu-id="bc105-349">Név hello projekt *ContosoAdsCommon*, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc105-349">Name hello project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="bc105-350">Tooreference hello Entity Framework hello és az adatmodell a webes és feldolgozói szerepkör projekt van szüksége.</span><span class="sxs-lookup"><span data-stu-id="bc105-350">You need tooreference hello Entity Framework context and hello data model from both web and worker role projects.</span></span> <span data-ttu-id="bc105-351">Alternatív megoldásként sikerült hello webes szerepkör projekt hello EF-hez kapcsolódó osztályokat definiálja, és hello feldolgozói szerepkör projekt hivatkozik erre a projektre.</span><span class="sxs-lookup"><span data-stu-id="bc105-351">As an alternative, you could define hello EF-related classes in hello web role project and reference that project from hello worker role project.</span></span> <span data-ttu-id="bc105-352">De hello alternatív módszert használja, a feldolgozói szerepkör projekt kellene egy tooweb referenciaszerelvények, amely nem igényel.</span><span class="sxs-lookup"><span data-stu-id="bc105-352">But in hello alternative approach, your worker role project would have a reference tooweb assemblies that it doesn't need.</span></span>

### <a name="update-and-add-nuget-packages"></a><span data-ttu-id="bc105-353">NuGet-csomagok frissítése és hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bc105-353">Update and add NuGet packages</span></span>
1. <span data-ttu-id="bc105-354">Nyissa meg hello **NuGet-csomagok kezelése** hello megoldás párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bc105-354">Open hello **Manage NuGet Packages** dialog box for hello solution.</span></span>
2. <span data-ttu-id="bc105-355">Hello ablak hello tetején válassza **frissítések**.</span><span class="sxs-lookup"><span data-stu-id="bc105-355">At hello top of hello window, select **Updates**.</span></span>
3. <span data-ttu-id="bc105-356">Keresse meg hello *windowsazure.Storage kifejezésre* csomagot, majd ha hello listában, jelölje ki, majd válassza ki a hello webes és feldolgozói projektek tooupdate legyen, és kattintson **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="bc105-356">Look for hello *WindowsAzure.Storage* package, and if it's in hello list, select it and select hello web and worker projects tooupdate it in, and then click **Update**.</span></span>

    <span data-ttu-id="bc105-357">hello storage ügyféloldali kódtár gyakrabban Visual Studio projektsablonjai, így gyakran tapasztalhatja, hogy hello verziót egy újonnan létrehozott projektben igények toobe frissítése a frissül.</span><span class="sxs-lookup"><span data-stu-id="bc105-357">hello storage client library is updated more frequently than Visual Studio project templates, so you'll often find that hello version in a newly-created project needs toobe updated.</span></span>
4. <span data-ttu-id="bc105-358">Hello ablak hello tetején válassza **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="bc105-358">At hello top of hello window, select **Browse**.</span></span>
5. <span data-ttu-id="bc105-359">Hello található *EntityFramework* NuGet csomagot, majd telepítse mind a három projektben.</span><span class="sxs-lookup"><span data-stu-id="bc105-359">Find hello *EntityFramework* NuGet package, and install it in all three projects.</span></span>
6. <span data-ttu-id="bc105-360">Hello található *Microsoft.WindowsAzure.ConfigurationManager* NuGet csomagot, majd telepítse hello feldolgozói szerepkör projektben.</span><span class="sxs-lookup"><span data-stu-id="bc105-360">Find hello *Microsoft.WindowsAzure.ConfigurationManager* NuGet package, and install it in hello worker role project.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="bc105-361">A projekt hivatkozásainak beállítása</span><span class="sxs-lookup"><span data-stu-id="bc105-361">Set project references</span></span>
1. <span data-ttu-id="bc105-362">Hello ContosoAdsWeb projektben állítson be egy hivatkozást toohello ContosoAdsCommon projektre.</span><span class="sxs-lookup"><span data-stu-id="bc105-362">In hello ContosoAdsWeb project, set a reference toohello ContosoAdsCommon project.</span></span> <span data-ttu-id="bc105-363">Kattintson a jobb gombbal a ContosoAdsWeb projektben hello, és kattintson **hivatkozások** - **hivatkozások hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="bc105-363">Right-click hello ContosoAdsWeb project, and then click **References** - **Add References**.</span></span> <span data-ttu-id="bc105-364">A hello **hivatkozáskezelő** párbeszédpanelen jelölje ki **megoldás – projektek** hello bal oldali ablaktáblában jelöljön ki **ContosoAdsCommon**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc105-364">In hello **Reference Manager** dialog box, select **Solution – Projects** in hello left pane, select **ContosoAdsCommon**, and then click **OK**.</span></span>
2. <span data-ttu-id="bc105-365">Hello ContosoAdsWorker projektben állítson be egy hivatkozást toohello ContosAdsCommon projektre.</span><span class="sxs-lookup"><span data-stu-id="bc105-365">In hello ContosoAdsWorker project, set a reference toohello ContosAdsCommon project.</span></span>

    <span data-ttu-id="bc105-366">ContosoAdsCommon tartalmazza hello Entity Framework adatok adatmodellt és a környezeti osztályt, amely mindkét hello előtér- és használják.</span><span class="sxs-lookup"><span data-stu-id="bc105-366">ContosoAdsCommon will contain hello Entity Framework data model and context class, which will be used by both hello front-end and back-end.</span></span>
3. <span data-ttu-id="bc105-367">Hello ContosoAdsWorker projektben állítson be egy hivatkozást túl`System.Drawing`.</span><span class="sxs-lookup"><span data-stu-id="bc105-367">In hello ContosoAdsWorker project, set a reference too`System.Drawing`.</span></span>

    <span data-ttu-id="bc105-368">Ez a szerelvény hello háttér-tooconvert képek toothumbnails használják.</span><span class="sxs-lookup"><span data-stu-id="bc105-368">This assembly is used by hello back-end tooconvert images toothumbnails.</span></span>

### <a name="configure-connection-strings"></a><span data-ttu-id="bc105-369">Csatlakozási karakterláncok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc105-369">Configure connection strings</span></span>
<span data-ttu-id="bc105-370">Ebben a szakaszban Azure Storage- és SQL-kapcsolati sztringeket fog konfigurálni helyi tesztelés céljából.</span><span class="sxs-lookup"><span data-stu-id="bc105-370">In this section, you configure Azure Storage and SQL connection strings for testing locally.</span></span> <span data-ttu-id="bc105-371">hello korábbi telepítési utasításai hello az oktatóanyag azt ismertetik, hogyan hello kapcsolat tooset karakterláncok a hello alkalmazás futtatásakor hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="bc105-371">hello deployment instructions earlier in hello tutorial explain how tooset up hello connection strings for when hello app runs in hello cloud.</span></span>

1. <span data-ttu-id="bc105-372">Hello ContosoAdsWeb projektre, nyissa meg hello alkalmazás Web.config fájljában és insert hello következő `connectionStrings` elem után hello `configSections` elemet.</span><span class="sxs-lookup"><span data-stu-id="bc105-372">In hello ContosoAdsWeb project, open hello application Web.config file, and insert hello following `connectionStrings` element after hello `configSections` element.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="bc105-373">A Visual Studio 2015 vagy újabb használata esetén cserélje le a „v11.0” elemet az „MSSQLLocalDB” elemre.</span><span class="sxs-lookup"><span data-stu-id="bc105-373">If you're using Visual Studio 2015 or higher, replace "v11.0" with "MSSQLLocalDB".</span></span>
2. <span data-ttu-id="bc105-374">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-374">Save your changes.</span></span>
3. <span data-ttu-id="bc105-375">Hello ContosoAdsCloudService projektben kattintson a jobb gombbal a ContosoAdsWeb **szerepkörök**, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="bc105-375">In hello ContosoAdsCloudService project, right-click ContosoAdsWeb under **Roles**, and then click **Properties**.</span></span>

    ![Szerepkör tulajdonságai](./media/cloud-services-dotnet-get-started/roleproperties.png)
4. <span data-ttu-id="bc105-377">A hello **Conosoadsweb [szerepkör]** tulajdonságai ablakban maradva kattintson hello **beállítások** fülre, majd **beállítás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="bc105-377">In hello **ContosAdsWeb [Role]** properties window, click hello **Settings** tab, and then click **Add Setting**.</span></span>

    <span data-ttu-id="bc105-378">Hagyja **szolgáltatáskonfiguráció** túl beállítása**összes konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="bc105-378">Leave **Service Configuration** set too**All Configurations**.</span></span>
5. <span data-ttu-id="bc105-379">Adjon hozzá egy *StorageConnectionString* névvel ellátott beállítást.</span><span class="sxs-lookup"><span data-stu-id="bc105-379">Add a setting named *StorageConnectionString*.</span></span> <span data-ttu-id="bc105-380">Állítsa be **típus** túl*ConnectionString*, és állítsa be **érték** túl*UseDevelopmentStorage = true*.</span><span class="sxs-lookup"><span data-stu-id="bc105-380">Set **Type** too*ConnectionString*, and set **Value** too*UseDevelopmentStorage=true*.</span></span>

    ![Új kapcsolati karakterlánc](./media/cloud-services-dotnet-get-started/scall.png)
6. <span data-ttu-id="bc105-382">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-382">Save your changes.</span></span>
7. <span data-ttu-id="bc105-383">Hajtsa végre hello azonos eljárás tooadd egy tárolási kapcsolat karakterláncát a hello Contosoadsweb szerepkör tulajdonságaihoz.</span><span class="sxs-lookup"><span data-stu-id="bc105-383">Follow hello same procedure tooadd a storage connection string in hello ContosoAdsWorker role properties.</span></span>
8. <span data-ttu-id="bc105-384">Még tart a hello **ContosoAdsWorker [szerepkör]** tulajdonságai ablakban maradva adja hozzá egy másik kapcsolati karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="bc105-384">Still in hello **ContosoAdsWorker [Role]** properties window, add another connection string:</span></span>

   * <span data-ttu-id="bc105-385">Név: ContosoAdsDbConnectionString</span><span class="sxs-lookup"><span data-stu-id="bc105-385">Name: ContosoAdsDbConnectionString</span></span>
   * <span data-ttu-id="bc105-386">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bc105-386">Type: String</span></span>
   * <span data-ttu-id="bc105-387">Érték: Illessze be hello azonos hello webes szerepkör projekt esetében használt kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bc105-387">Value: Paste hello same connection string you used for hello web role project.</span></span> <span data-ttu-id="bc105-388">(a Visual Studio 2013 a hello a következő példa.</span><span class="sxs-lookup"><span data-stu-id="bc105-388">(hello following example is for Visual Studio 2013.</span></span> <span data-ttu-id="bc105-389">Ne feledje toochange hello adatforrás Ha ezt a példát, és a Visual Studio 2015-ös vagy újabb rendszer használata esetén.)</span><span class="sxs-lookup"><span data-stu-id="bc105-389">Don't forget toochange hello Data Source if you copy this example and you're using Visual Studio 2015 or higher.)</span></span>

       ```
       Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;
       ```

### <a name="add-code-files"></a><span data-ttu-id="bc105-390">Kódfájlok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bc105-390">Add code files</span></span>
<span data-ttu-id="bc105-391">Ebben a szakaszban, átmásolja kódfájlok hello letöltött megoldásból hello új megoldás.</span><span class="sxs-lookup"><span data-stu-id="bc105-391">In this section, you copy code files from hello downloaded solution into hello new solution.</span></span> <span data-ttu-id="bc105-392">hello következő szakaszok bemutatják és ismertetik a kód legfontosabb részeit.</span><span class="sxs-lookup"><span data-stu-id="bc105-392">hello following sections will show and explain key parts of this code.</span></span>

<span data-ttu-id="bc105-393">tooadd fájlok tooa projekt vagy egy mappát, kattintson a jobb gombbal hello projekt vagy mappa és kattintson **Hozzáadás** - **meglévő cikk**.</span><span class="sxs-lookup"><span data-stu-id="bc105-393">tooadd files tooa project or a folder, right-click hello project or folder and click **Add** - **Existing Item**.</span></span> <span data-ttu-id="bc105-394">Válassza ki a hello kívánt fájlokat, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="bc105-394">Select hello files you want and then click **Add**.</span></span> <span data-ttu-id="bc105-395">Ha a rendszer rákérdez, hogy kívánja-e tooreplace meglévő fájlokat, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="bc105-395">If asked whether you want tooreplace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="bc105-396">Hello ContosoAdsCommon projektben törölje a hello *Class1.cs* fájlt, és a hely hello *Ad.cs* és *ContosoAdscontext.cs* hello fájlok letöltése.</span><span class="sxs-lookup"><span data-stu-id="bc105-396">In hello ContosoAdsCommon project, delete hello *Class1.cs* file and add in its place hello *Ad.cs* and *ContosoAdscontext.cs* files from hello downloaded project.</span></span>
2. <span data-ttu-id="bc105-397">Hello ContosoAdsWeb projektben adja hozzá a következő fájlok hello letöltött projektből hello.</span><span class="sxs-lookup"><span data-stu-id="bc105-397">In hello ContosoAdsWeb project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="bc105-398">*Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="bc105-398">*Global.asax.cs*.</span></span>  
   * <span data-ttu-id="bc105-399">A hello *Views\Shared* mappa:  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bc105-399">In hello *Views\Shared* folder: *\_Layout.cshtml*.</span></span>
   * <span data-ttu-id="bc105-400">A hello *Views\Home* mappa: *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bc105-400">In hello *Views\Home* folder: *Index.cshtml*.</span></span>
   * <span data-ttu-id="bc105-401">A hello *tartományvezérlők* mappa: *AdController.cs*.</span><span class="sxs-lookup"><span data-stu-id="bc105-401">In hello *Controllers* folder: *AdController.cs*.</span></span>
   * <span data-ttu-id="bc105-402">A hello *Views\Ad* mappában (először hozza létre hello mappa): öt *.cshtml* fájlokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-402">In hello *Views\Ad* folder (create hello folder first): five *.cshtml* files.</span></span>
3. <span data-ttu-id="bc105-403">Hello ContosoAdsWorker projektben adja hozzá *WorkerRole.cs* hello a letöltött projektet.</span><span class="sxs-lookup"><span data-stu-id="bc105-403">In hello ContosoAdsWorker project, add *WorkerRole.cs* from hello downloaded project.</span></span>

<span data-ttu-id="bc105-404">Most már létrehozhatja és hello alkalmazás futtatásához hello az oktatóanyag korábbi utasításai szerint, és hello alkalmazás fogja használni a helyi adatbázis és a storage emulator erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="bc105-404">You can now build and run hello application as instructed earlier in hello tutorial, and hello app will use local database and storage emulator resources.</span></span>

<span data-ttu-id="bc105-405">hello alábbi szakaszok ismertetik a hello kód a kapcsolódó tooworking hello Azure-környezetéhez, blobokat és üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-405">hello following sections explain hello code related tooworking with hello Azure environment, blobs, and queues.</span></span> <span data-ttu-id="bc105-406">Ez az oktatóanyag nem tartalmazza az hogyan toocreate MVC-vezérlők és nézetek használatával állványok, toowrite Entity Framework-kód, amelyek működése a SQL Server-adatbázisok, vagy az ASP.NET 4.5 aszinkron programozás alapjait hello.</span><span class="sxs-lookup"><span data-stu-id="bc105-406">This tutorial does not explain how toocreate MVC controllers and views using scaffolding, how toowrite Entity Framework code that works with SQL Server databases, or hello basics of asynchronous programming in ASP.NET 4.5.</span></span> <span data-ttu-id="bc105-407">Ezek a témakörök kapcsolatos információkért lásd: a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="bc105-407">For information about these topics, see hello following resources:</span></span>

* [<span data-ttu-id="bc105-408">Bevezetés az MVC 5 használatába</span><span class="sxs-lookup"><span data-stu-id="bc105-408">Get started with MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="bc105-409">Bevezetés az EF 6 és az MVC 5 használatába</span><span class="sxs-lookup"><span data-stu-id="bc105-409">Get started with EF 6 and MVC 5</span></span>](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* <span data-ttu-id="bc105-410">[Bevezetés tooasynchronous programozásba a .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="bc105-410">[Introduction tooasynchronous programming in .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="bc105-411">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="bc105-411">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="bc105-412">hello Ad.cs fájl megad, egy felsorolást a kategóriákhoz és egy POCO entitásosztályt a hirdetés információihoz.</span><span class="sxs-lookup"><span data-stu-id="bc105-412">hello Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

```csharp
public enum Category
{
    Cars,
    [Display(Name="Real Estate")]
    RealEstate,
    [Display(Name = "Free Stuff")]
    FreeStuff
}

public class Ad
{
    public int AdId { get; set; }

    [StringLength(100)]
    public string Title { get; set; }

    public int Price { get; set; }

    [StringLength(1000)]
    [DataType(DataType.MultilineText)]
    public string Description { get; set; }

    [StringLength(1000)]
    [DisplayName("Full-size Image")]
    public string ImageURL { get; set; }

    [StringLength(1000)]
    [DisplayName("Thumbnail")]
    public string ThumbnailURL { get; set; }

    [DataType(DataType.Date)]
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
    public DateTime PostedDate { get; set; }

    public Category? Category { get; set; }
    [StringLength(12)]
    public string Phone { get; set; }
}
```

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="bc105-413">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="bc105-413">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="bc105-414">hello ContosoAdsContext osztály megadja, hogy hello Ad osztály egy DbSet gyűjteményben, amely Entity Framework SQL-adatbázisban tárolja történjen.</span><span class="sxs-lookup"><span data-stu-id="bc105-414">hello ContosoAdsContext class specifies that hello Ad class is used in a DbSet collection, which Entity Framework will store in a SQL database.</span></span>

```csharp
public class ContosoAdsContext : DbContext
{
    public ContosoAdsContext() : base("name=ContosoAdsContext")
    {
    }
    public ContosoAdsContext(string connString)
        : base(connString)
    {
    }
    public System.Data.Entity.DbSet<Ad> Ads { get; set; }
}
```

<span data-ttu-id="bc105-415">hello osztály két konstruktorral rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bc105-415">hello class has two constructors.</span></span> <span data-ttu-id="bc105-416">először hello hello webes projekt használja valamelyiket, és annak hello hello Web.config fájlban tárolt kapcsolati karakterlánc nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="bc105-416">hello first of them is used by hello web project, and specifies hello name of a connection string that is stored in hello Web.config file.</span></span> <span data-ttu-id="bc105-417">hello második konstruktor lehetővé teszi toopass hello tényleges kapcsolati karakterlánc hello feldolgozói szerepkör projekt által használt, mivel nem rendelkezik a Web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc105-417">hello second constructor enables you toopass in hello actual connection string used by hello worker role project, since it doesn't have a Web.config file.</span></span> <span data-ttu-id="bc105-418">A korábban Ha a kapcsolati karakterlánc tárolásának, és láthatja, hogyan később hello kód keresi hello kapcsolati karakterlánc elindítja hello DbContext osztályt.</span><span class="sxs-lookup"><span data-stu-id="bc105-418">You saw earlier where this connection string was stored, and you'll see later how hello code retrieves hello connection string when it instantiates hello DbContext class.</span></span>

### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="bc105-419">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="bc105-419">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="bc105-420">Hello meghívott kód `Application_Start` hoz létre egy *képek* blobtárolót és egy *képek* üzenetsort, amennyiben még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="bc105-420">Code that is called from hello `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="bc105-421">Ez biztosítja, hogy valahányszor új tárfiókot használ, vagy indításakor hello storage emulator használatával új számítógépre, hello szükséges blobtároló és üzenetsor automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="bc105-421">This ensures that whenever you start using a new storage account, or start using hello storage emulator on a new computer, hello required blob container and queue will be created automatically.</span></span>

<span data-ttu-id="bc105-422">kód lekérdezi hozzáférés toohello tárfiók hello származó hello hello tárolási kapcsolati karakterlánc használatával *.cscfg* fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc105-422">hello code gets access toohello storage account by using hello storage connection string from hello *.cscfg* file.</span></span>

```csharp
var storageAccount = CloudStorageAccount.Parse
    (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));
```

<span data-ttu-id="bc105-423">Ezután egy hivatkozás toohello *képek* blobtárolóhoz, létrehozza a hello tárolót, ha még nem létezik, és beállítja a hozzáférési engedélyeket hello új tárolóra.</span><span class="sxs-lookup"><span data-stu-id="bc105-423">Then it gets a reference toohello *images* blob container, creates hello container if it doesn't already exist, and sets access permissions on hello new container.</span></span> <span data-ttu-id="bc105-424">Alapértelmezés szerint új tárolók csak rendelkező ügyfeleknek engedélyezik a tárfiók hitelesítő adatok tooaccess blobokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-424">By default, new containers only allow clients with storage account credentials tooaccess blobs.</span></span> <span data-ttu-id="bc105-425">hello webhely kell hello toobe nyilvános blobok, úgy, hogy az URL-címekkel, hogy pont toohello kép blobok képeket jeleníthessen meg.</span><span class="sxs-lookup"><span data-stu-id="bc105-425">hello website needs hello blobs toobe public so that it can display images using URLs that point toohello image blobs.</span></span>

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
var imagesBlobContainer = blobClient.GetContainerReference("images");
if (imagesBlobContainer.CreateIfNotExists())
{
    imagesBlobContainer.SetPermissions(
        new BlobContainerPermissions
        {
            PublicAccess =BlobContainerPublicAccessType.Blob
        });
}
```

<span data-ttu-id="bc105-426">Hasonló kód jogosultságot kap a hivatkozás toohello *képek* várólistára, és létrehoz egy új üzenetsort.</span><span class="sxs-lookup"><span data-stu-id="bc105-426">Similar code gets a reference toohello *images* queue and creates a new queue.</span></span> <span data-ttu-id="bc105-427">Ebben az esetben nincs szükség az engedélyek módosítására.</span><span class="sxs-lookup"><span data-stu-id="bc105-427">In this case, no permissions change is needed.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
var imagesQueue = queueClient.GetQueueReference("images");
imagesQueue.CreateIfNotExists();
```

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="bc105-428">ContosoAdsWeb – \_Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="bc105-428">ContosoAdsWeb - \_Layout.cshtml</span></span>
<span data-ttu-id="bc105-429">Hello *_Layout.cshtml* fájl hello alkalmazásnév beállítja az hello fejlécében és láblécében, és létrehoz egy "Ads" menübejegyzést.</span><span class="sxs-lookup"><span data-stu-id="bc105-429">hello *_Layout.cshtml* file sets hello app name in hello header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="bc105-430">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="bc105-430">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="bc105-431">Hello *Views\Home\Index.cshtml* fájl kategóriahivatkozásokat jelenít meg hello kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="bc105-431">hello *Views\Home\Index.cshtml* file displays category links on hello home page.</span></span> <span data-ttu-id="bc105-432">hello hivatkozások átadják hello egész értéket hello `Category` felsorolás egy lekérdezési karakterlánc változó toohello Ads indexlapnak a.</span><span class="sxs-lookup"><span data-stu-id="bc105-432">hello links pass hello integer value of hello `Category` enum in a querystring variable toohello Ads Index page.</span></span>

```razor
<li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
<li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
<li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
<li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>
```

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="bc105-433">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="bc105-433">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="bc105-434">A hello *AdController.cs* fájl, hello konstruktor hívások hello `InitializeStorage` metódus toocreate Azure Storage ügyféloldali kódtár objektumok, amelyek az API-k használata a blobokat és üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-434">In hello *AdController.cs* file, hello constructor calls hello `InitializeStorage` method toocreate Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="bc105-435">Ezután hello kód jogosultságot kap a hivatkozás toohello *képek* blobtárolóra, ahogy azt korábban a *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="bc105-435">Then hello code gets a reference toohello *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="bc105-436">Mindeközben beállít egy webalkalmazásokhoz használható alapértelmezett [újrapróbálkozási házirendet](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling).</span><span class="sxs-lookup"><span data-stu-id="bc105-436">While doing that it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="bc105-437">hello alapértelmezett exponenciális leállítási újrapróbálkozási házirend le több mint egy perc egy átmeneti hiba miatti ismételt próbálkozás hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bc105-437">hello default exponential backoff retry policy could hang hello web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="bc105-438">Itt megadott hello újrapróbálkozási házirend minden próbálkozás mentése toothree próbálkozás után három másodperc alatt vár.</span><span class="sxs-lookup"><span data-stu-id="bc105-438">hello retry policy specified here waits three seconds after each try for up toothree tries.</span></span>

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesBlobContainer = blobClient.GetContainerReference("images");
```

<span data-ttu-id="bc105-439">Hasonló kód jogosultságot kap a hivatkozás toohello *képek* várólista.</span><span class="sxs-lookup"><span data-stu-id="bc105-439">Similar code gets a reference toohello *images* queue.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesQueue = queueClient.GetQueueReference("images");
```

<span data-ttu-id="bc105-440">A legtöbb hello vezérlőkód jellemzően egy DbContext osztályt használó Entity Framework-adatmodell.</span><span class="sxs-lookup"><span data-stu-id="bc105-440">Most of hello controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="bc105-441">Egy kivétel: hello HttpPost `Create` metódus, amely feltölt egy fájlt, és menti a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="bc105-441">An exception is hello HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="bc105-442">hello biztosít egy [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) toohello metódus objektum.</span><span class="sxs-lookup"><span data-stu-id="bc105-442">hello model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object toohello method.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<ActionResult> Create(
    [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
    HttpPostedFileBase imageFile)
```

<span data-ttu-id="bc105-443">Egy fájl tooupload hello felhasználói választásakor hello kód feltölt hello fájlt, egy blobba menti, és frissíti a hirdetés adatbázisrekordot hello toohello blob URL-CÍMMEL.</span><span class="sxs-lookup"><span data-stu-id="bc105-443">If hello user selected a file tooupload, hello code uploads hello file, saves it in a blob, and updates hello Ad database record with a URL that points toohello blob.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    blob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = blob.Uri.ToString();
}
```

<span data-ttu-id="bc105-444">hello kódot, amely hello feltöltés van hello `UploadAndSaveBlobAsync` metódust.</span><span class="sxs-lookup"><span data-stu-id="bc105-444">hello code that does hello upload is in hello `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="bc105-445">Létrehoz egy GUID nevet hello blob, a feltöltéseket és az eredményobjektumokat hello fájl, és egy hivatkozási mentve toohello blob adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bc105-445">It creates a GUID name for hello blob, uploads and saves hello file, and returns a reference toohello saved blob.</span></span>

```csharp
private async Task<CloudBlockBlob> UploadAndSaveBlobAsync(HttpPostedFileBase imageFile)
{
    string blobName = Guid.NewGuid().ToString() + Path.GetExtension(imageFile.FileName);
    CloudBlockBlob imageBlob = imagesBlobContainer.GetBlockBlobReference(blobName);
    using (var fileStream = imageFile.InputStream)
    {
        await imageBlob.UploadFromStreamAsync(fileStream);
    }
    return imageBlob;
}
```

<span data-ttu-id="bc105-446">Miután a hello HttpPost `Create` metódus feltölt egy blobot és frissítések hello adatbázist, létrehoz egy várólista üzenet tooinform, hogy egy kép készen áll a átalakítás tooa miniatűr háttér-folyamat.</span><span class="sxs-lookup"><span data-stu-id="bc105-446">After hello HttpPost `Create` method uploads a blob and updates hello database, it creates a queue message tooinform that back-end process that an image is ready for conversion tooa thumbnail.</span></span>

```csharp
string queueMessageString = ad.AdId.ToString();
var queueMessage = new CloudQueueMessage(queueMessageString);
await queue.AddMessageAsync(queueMessage);
```

<span data-ttu-id="bc105-447">hello HttpPost kódját hello `Edit` módszer a hasonló, azzal a különbséggel, hogy ha hello felhasználó kiválaszt egy új képfájlt minden létező blobot törölni kell.</span><span class="sxs-lookup"><span data-stu-id="bc105-447">hello code for hello HttpPost `Edit` method is similar except that if hello user selects a new image file any blobs that already exist must be deleted.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    await DeleteAdBlobsAsync(ad);
    imageBlob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = imageBlob.Uri.ToString();
}
```

<span data-ttu-id="bc105-448">hello következő példa bemutatja, blobok törlése, ha töröl egy ad hello kódját.</span><span class="sxs-lookup"><span data-stu-id="bc105-448">hello next example shows hello code that deletes blobs when you delete an ad.</span></span>

```csharp
private async Task DeleteAdBlobsAsync(Ad ad)
{
    if (!string.IsNullOrWhiteSpace(ad.ImageURL))
    {
        Uri blobUri = new Uri(ad.ImageURL);
        await DeleteAdBlobAsync(blobUri);
    }
    if (!string.IsNullOrWhiteSpace(ad.ThumbnailURL))
    {
        Uri blobUri = new Uri(ad.ThumbnailURL);
        await DeleteAdBlobAsync(blobUri);
    }
}
private static async Task DeleteAdBlobAsync(Uri blobUri)
{
    string blobName = blobUri.Segments[blobUri.Segments.Length - 1];
    CloudBlockBlob blobToDelete = imagesBlobContainer.GetBlockBlobReference(blobName);
    await blobToDelete.DeleteAsync();
}
```

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="bc105-449">ContosoAdsWeb – Views\Ad\Index.cshtml és Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="bc105-449">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="bc105-450">Hello *Index.cshtml* fájlt tartalmazó miniatűrt jelenít meg hello hirdetés többi adatát.</span><span class="sxs-lookup"><span data-stu-id="bc105-450">hello *Index.cshtml* file displays thumbnails with hello other ad data.</span></span>

```razor
<img src="@Html.Raw(item.ThumbnailURL)" />
```

<span data-ttu-id="bc105-451">Hello *Details.cshtml* fájl hello teljes méretű képet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bc105-451">hello *Details.cshtml* file displays hello full-size image.</span></span>

```razor
<img src="@Html.Raw(Model.ImageURL)" />
```

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="bc105-452">ContosoAdsWeb – Views\Ad\Create.cshtml és Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="bc105-452">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="bc105-453">Hello *Create.cshtml* és *Edit.cshtml* fájlok megadják az űrlap kódolását, hogy a lehetővé teszi, hogy a tartományvezérlő tooget hello hello `HttpPostedFileBase` objektum.</span><span class="sxs-lookup"><span data-stu-id="bc105-453">hello *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables hello controller tooget hello `HttpPostedFileBase` object.</span></span>

```razor
@using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))
```

<span data-ttu-id="bc105-454">Egy `<input>` elem jelzi hello böngésző tooprovide egy Fájlkiválasztási párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="bc105-454">An `<input>` element tells hello browser tooprovide a file selection dialog.</span></span>

```razor
<input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />
```

### <a name="contosoadsworker---workerrolecs---onstart-method"></a><span data-ttu-id="bc105-455">ContosoAdsWorker – WorkerRole.cs – OnStart metódus</span><span class="sxs-lookup"><span data-stu-id="bc105-455">ContosoAdsWorker - WorkerRole.cs - OnStart method</span></span>
<span data-ttu-id="bc105-456">hello Azure feldolgozói szerepkör környezet meghívja a hello `OnStart` metódus a hello `WorkerRole` indulásakor hello feldolgozói szerepkör, és meghívja hello `Run` metódusban, amikor hello `OnStart` metódus befejeződik.</span><span class="sxs-lookup"><span data-stu-id="bc105-456">hello Azure worker role environment calls hello `OnStart` method in hello `WorkerRole` class when hello worker role is getting started, and it calls hello `Run` method when hello `OnStart` method finishes.</span></span>

<span data-ttu-id="bc105-457">Hello `OnStart` metódus hello adatbázis-kapcsolati karakterlánc lekérése hello *.cscfg* fájlt, és átadja toohello Entity Framework DbContext osztálynak.</span><span class="sxs-lookup"><span data-stu-id="bc105-457">hello `OnStart` method gets hello database connection string from hello *.cscfg* file and passes it toohello Entity Framework DbContext class.</span></span> <span data-ttu-id="bc105-458">hello SQLClient szolgáltató alapértelmezés szerint van használatban, így hello szolgáltató nincs megadva toobe.</span><span class="sxs-lookup"><span data-stu-id="bc105-458">hello SQLClient provider is used by default, so hello provider does not have toobe specified.</span></span>

```csharp
var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
db = new ContosoAdsContext(dbConnString);
```

<span data-ttu-id="bc105-459">Ezt követően hello metódus lekérdezi a hivatkozás toohello tárfiók, és hello blobtároló és üzenetsor létrehozása, ha azok még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="bc105-459">After that, hello method gets a reference toohello storage account and creates hello blob container and queue if they don't exist.</span></span> <span data-ttu-id="bc105-460">hello tartozó kód hasonló toowhat hello webes szerepkör már látott `Application_Start` metódust.</span><span class="sxs-lookup"><span data-stu-id="bc105-460">hello code for that is similar toowhat you already saw in hello web role `Application_Start` method.</span></span>

### <a name="contosoadsworker---workerrolecs---run-method"></a><span data-ttu-id="bc105-461">ContosoAdsWorker – WorkerRole.cs – Run metódus</span><span class="sxs-lookup"><span data-stu-id="bc105-461">ContosoAdsWorker - WorkerRole.cs - Run method</span></span>
<span data-ttu-id="bc105-462">Hello `Run` metódus lehívásra kerül, ha hello `OnStart` metódus inicializálási feladatának befejezése után.</span><span class="sxs-lookup"><span data-stu-id="bc105-462">hello `Run` method is called when hello `OnStart` method finishes its initialization work.</span></span> <span data-ttu-id="bc105-463">hello metódus elindít egy végtelen ciklust, amely új üzeneteit figyeli, és feldolgozza őket, amikor azok beérkeznek.</span><span class="sxs-lookup"><span data-stu-id="bc105-463">hello method executes an infinite loop that watches for new queue messages and processes them when they arrive.</span></span>

```csharp
public override void Run()
{
    CloudQueueMessage msg = null;

    while (true)
    {
        try
        {
            msg = this.imagesQueue.GetMessage();
            if (msg != null)
            {
                ProcessQueueMessage(msg);
            }
            else
            {
                System.Threading.Thread.Sleep(1000);
            }
        }
        catch (StorageException e)
        {
            if (msg != null && msg.DequeueCount > 5)
            {
                this.imagesQueue.DeleteMessage(msg);
            }
            System.Threading.Thread.Sleep(5000);
        }
    }
}
```

<span data-ttu-id="bc105-464">Hello ciklus egyes ismétlései után található nincs üzenet, ha hello program alvó állapotban marad, a második.</span><span class="sxs-lookup"><span data-stu-id="bc105-464">After each iteration of hello loop, if no queue message was found, hello program sleeps for a second.</span></span> <span data-ttu-id="bc105-465">Ez megakadályozza, hogy a hello feldolgozói szerepkör túl sok CPU idő- és tárolási tranzakciós költségeket halmozzon.</span><span class="sxs-lookup"><span data-stu-id="bc105-465">This prevents hello worker role from incurring excessive CPU time and storage transaction costs.</span></span> <span data-ttu-id="bc105-466">hello Microsoft Ügyféltanácsadói csapatának van egy története egy fejlesztőről, aki elfelejtette tooinclude, tooproduction telepíti, és elment nyaralni.</span><span class="sxs-lookup"><span data-stu-id="bc105-466">hello Microsoft Customer Advisory Team tells a story about a  developer who forgot tooinclude this, deployed tooproduction, and left for vacation.</span></span> <span data-ttu-id="bc105-467">Mire vissza, a figyelmetlensége többe hello szabadsága-nál.</span><span class="sxs-lookup"><span data-stu-id="bc105-467">When he got back, his oversight cost more than hello vacation.</span></span>

<span data-ttu-id="bc105-468">A várólista üzenet tartalma hello néha hibát okoz a feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="bc105-468">Sometimes hello content of a queue message causes an error in processing.</span></span> <span data-ttu-id="bc105-469">Ez a lehetőség egy *az elhalt üzenet*, és ha csak naplózott egy hibát és hello hurok újraindul, akkor feldolgozásával a végtelenségig próbálkozhat tooprocess üzenetet.</span><span class="sxs-lookup"><span data-stu-id="bc105-469">This is called a *poison message*, and if you just logged an error and restarted hello loop, you could endlessly try tooprocess that message.</span></span>  <span data-ttu-id="bc105-470">Ezért hello catch blokk tartalmaz egy if utasítást, amely ellenőrzi a toosee hányszor hello app megpróbálta tooprocess hello aktuális üzenetet, és ha több mint 5-ször volt, üdvözlőüzenetére törli a várólistáról hello.</span><span class="sxs-lookup"><span data-stu-id="bc105-470">Therefore hello catch block includes an if statement that checks toosee how many times hello app has tried tooprocess hello current message, and if it has been more than 5 times, hello message is deleted from hello queue.</span></span>

<span data-ttu-id="bc105-471">`ProcessQueueMessage`akkor lesz meghívva, ha az üzenetsorban található üzenet.</span><span class="sxs-lookup"><span data-stu-id="bc105-471">`ProcessQueueMessage` is called when a queue message is found.</span></span>

```csharp
private void ProcessQueueMessage(CloudQueueMessage msg)
{
    var adId = int.Parse(msg.AsString);
    Ad ad = db.Ads.Find(adId);
    if (ad == null)
    {
        throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", adId.ToString()));
    }

    CloudBlockBlob inputBlob = this.imagesBlobContainer.GetBlockBlobReference(ad.ImageURL);

    string thumbnailName = Path.GetFileNameWithoutExtension(inputBlob.Name) + "thumb.jpg";
    CloudBlockBlob outputBlob = this.imagesBlobContainer.GetBlockBlobReference(thumbnailName);

    using (Stream input = inputBlob.OpenRead())
    using (Stream output = outputBlob.OpenWrite())
    {
        ConvertImageToThumbnailJPG(input, output);
        outputBlob.Properties.ContentType = "image/jpeg";
    }

    ad.ThumbnailURL = outputBlob.Uri.ToString();
    db.SaveChanges();

    this.imagesQueue.DeleteMessage(msg);
}
```

<span data-ttu-id="bc105-472">Ez a kód beolvassa hello adatbázis tooget hello kép URL-címe, hello kép tooa miniatűr alakít, hello miniatűrt egy blobba menti, frissíti hello adatbázist hello miniatűr blob URL- és hello üzenetsor törli.</span><span class="sxs-lookup"><span data-stu-id="bc105-472">This code reads hello database tooget hello image URL, converts hello image tooa thumbnail, saves hello thumbnail in a blob, updates hello database with hello thumbnail blob URL, and deletes hello queue message.</span></span>

> [!NOTE]
> <span data-ttu-id="bc105-473">hello kód hello `ConvertImageToThumbnailJPG` metódus hello System.Drawing névtérben-osztályokat használja az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="bc105-473">hello code in hello `ConvertImageToThumbnailJPG` method uses classes in hello System.Drawing namespace for simplicity.</span></span> <span data-ttu-id="bc105-474">A névtérben lévő osztályok hello azonban a Windows-űrlapokkal való használatra lettek tervezve.</span><span class="sxs-lookup"><span data-stu-id="bc105-474">However, hello classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="bc105-475">Windows- vagy ASP.NET-szolgáltatásban való használatuk nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="bc105-475">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="bc105-476">További információk a képfeldolgozási beállításokról: [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) (Dinamikus képek létrehozása) és [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na) (A képek átméretezésének részletei).</span><span class="sxs-lookup"><span data-stu-id="bc105-476">For more information about image-processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="troubleshooting"></a><span data-ttu-id="bc105-477">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="bc105-477">Troubleshooting</span></span>
<span data-ttu-id="bc105-478">Abban az esetben, ha valami nem működik, ebben az oktatóanyagban hello utasítások követése, az alábbiakban néhány gyakori hibák, és hogyan tooresolve őket.</span><span class="sxs-lookup"><span data-stu-id="bc105-478">In case something doesn't work while you're following hello instructions in this tutorial, here are some common errors and how tooresolve them.</span></span>

### <a name="serviceruntimeroleenvironmentexception"></a><span data-ttu-id="bc105-479">ServiceRuntime.RoleEnvironmentException</span><span class="sxs-lookup"><span data-stu-id="bc105-479">ServiceRuntime.RoleEnvironmentException</span></span>
<span data-ttu-id="bc105-480">Hello `RoleEnvironment` objektum Azure által biztosított, az alkalmazás az Azure-ban való futtatásakor, vagy helyileg a hello Azure compute emulator használatával futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="bc105-480">hello `RoleEnvironment` object is provided by Azure when you run an application in Azure or when you run locally using hello Azure compute emulator.</span></span>  <span data-ttu-id="bc105-481">Ha ez a hiba a helyi futtatás során, akkor győződjön meg arról, hogy hello ContosoAdsCloudService projektet állította hello indítási projektként.</span><span class="sxs-lookup"><span data-stu-id="bc105-481">If you get this error when you're running locally, make sure that you have set hello ContosoAdsCloudService project as hello startup project.</span></span> <span data-ttu-id="bc105-482">Hello projekt toorun hello Azure compute emulator használatával állít be.</span><span class="sxs-lookup"><span data-stu-id="bc105-482">This sets up hello project toorun using hello Azure compute emulator.</span></span>

<span data-ttu-id="bc105-483">Hello alkalmazás által használt hello Azure roleenvironment-et hello dolog tooget hello kapcsolati karakterlánc-értékek hello tárolt *.cscfg* fájlokat, ezért ez a kivétel egy másik oka egy hiányzó kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bc105-483">One of hello things hello application uses hello Azure RoleEnvironment for is tooget hello connection string values that are stored in hello *.cscfg* files, so another cause of this exception is a missing connection string.</span></span> <span data-ttu-id="bc105-484">Győződjön meg arról, hogy létrehozta hello StorageConnectionString beállítást felhő és a helyi konfigurációiban hello ContosoAdsWeb projektben, és mindkét kapcsolati karakterláncot mindkét konfigurációjában létrehozta hello ContosoAdsWorker projektben.</span><span class="sxs-lookup"><span data-stu-id="bc105-484">Make sure that you created hello StorageConnectionString setting for both Cloud and Local configurations in hello ContosoAdsWeb project, and that you created both connection strings for both configurations in hello ContosoAdsWorker project.</span></span> <span data-ttu-id="bc105-485">Ha így tesz egy **található összes** keresési StorageConnectionString hello teljes megoldásban, a kell megjelennie 9 alkalommal 6 fájlban.</span><span class="sxs-lookup"><span data-stu-id="bc105-485">If you do a **Find All** search for StorageConnectionString in hello entire solution, you should see it 9 times in 6 files.</span></span>

### <a name="cannot-override-tooport-xxx-new-port-below-minimum-allowed-value-8080-for-protocol-http"></a><span data-ttu-id="bc105-486">Tooport xxx metódus nem bírálhatja felül.</span><span class="sxs-lookup"><span data-stu-id="bc105-486">Cannot override tooport xxx.</span></span> <span data-ttu-id="bc105-487">Az új port a HTTP protokoll esetében megengedett legalacsonyabb, 8080 érték alatt van</span><span class="sxs-lookup"><span data-stu-id="bc105-487">New port below minimum allowed value 8080 for protocol http</span></span>
<span data-ttu-id="bc105-488">Próbálja meg módosítani a hello hello webes projekt által használt portszámot.</span><span class="sxs-lookup"><span data-stu-id="bc105-488">Try changing hello port number used by hello web project.</span></span> <span data-ttu-id="bc105-489">Kattintson a jobb gombbal a ContosoAdsWeb projektben hello, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="bc105-489">Right-click hello ContosoAdsWeb project, and then click **Properties**.</span></span> <span data-ttu-id="bc105-490">Kattintson a hello **webes** lapra, és módosítsa a portszámot hello hello **projekt URL-címe** beállítást.</span><span class="sxs-lookup"><span data-stu-id="bc105-490">Click hello **Web** tab, and then change hello port number in hello **Project Url** setting.</span></span>

<span data-ttu-id="bc105-491">Alternatív hello probléma megoldására tekintse meg a következő szakasz hello.</span><span class="sxs-lookup"><span data-stu-id="bc105-491">For another alternative that might resolve hello problem, see hello following  section.</span></span>

### <a name="other-errors-when-running-locally"></a><span data-ttu-id="bc105-492">A helyi futtatás során felmerülő egyéb hibák</span><span class="sxs-lookup"><span data-stu-id="bc105-492">Other errors when running locally</span></span>
<span data-ttu-id="bc105-493">Alapértelmezett új cloud service projektek hello Azure compute emulator express toosimulate hello Azure környezetben használják.</span><span class="sxs-lookup"><span data-stu-id="bc105-493">By default new cloud service projects use hello Azure compute emulator express toosimulate hello Azure environment.</span></span> <span data-ttu-id="bc105-494">Ez hello teljes compute emulator egyszerűsített verziója, és bizonyos feltételek hello a teljes emulátor fognak működni, amikor hello express verzió nem.</span><span class="sxs-lookup"><span data-stu-id="bc105-494">This is a lightweight version of hello full compute emulator, and under some conditions hello full emulator will work when hello express version does not.</span></span>  

<span data-ttu-id="bc105-495">toochange hello projekt toouse hello teljes emulátor, kattintson a jobb gombbal a ContosoAdsCloudService projekt hello, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="bc105-495">toochange hello project toouse hello full emulator, right-click hello ContosoAdsCloudService project, and then click **Properties**.</span></span> <span data-ttu-id="bc105-496">A hello **tulajdonságok** ablakban kattintson a hello **webes** fülre, majd hello **teljes emulátor használata** választógombot.</span><span class="sxs-lookup"><span data-stu-id="bc105-496">In hello **Properties** window click hello **Web** tab, and then click hello **Use Full Emulator** radio button.</span></span>

<span data-ttu-id="bc105-497">Rendelés toorun hello alkalmazás hello teljes emulátorral tooopen Visual Studio rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bc105-497">In order toorun hello application with hello full emulator, you have tooopen Visual Studio with administrator privileges.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc105-498">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc105-498">Next steps</span></span>
<span data-ttu-id="bc105-499">hello Contoso Ads alkalmazás kialakítása szándékosan egyszerű-bevezető oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="bc105-499">hello Contoso Ads application has intentionally been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="bc105-500">Például az nem implementálja a következőt [függőségi beszúrást](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) vagy hello [adattárát és egységét működik minták](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), nem [használ felületet a naplózáshoz](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), nemhasznál[ EF Code First áttelepítést](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage adatok modell módosítások vagy [EF-kapcsolati rugalmasságot](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage átmeneti hálózati hibák, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="bc105-500">For example, it doesn't implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) or hello [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), it doesn't [use an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), it doesn't use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data model changes or [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage transient network errors, and so forth.</span></span>

<span data-ttu-id="bc105-501">Az alábbiakban néhány felhőszolgáltatás-alkalmazásokra, amelyek több valós kódolási gyakorlatot alkalmaz, az összetett kevésbé összetett toomore felsorolt bemutatása:</span><span class="sxs-lookup"><span data-stu-id="bc105-501">Here are some cloud service sample applications that demonstrate more real-world coding practices, listed from less complex toomore complex:</span></span>

* <span data-ttu-id="bc105-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span><span class="sxs-lookup"><span data-stu-id="bc105-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span></span> <span data-ttu-id="bc105-503">Hasonlít a tooContoso Ads elvéhez, de több funkciót és több valós kódolási gyakorlatot alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="bc105-503">Similar in concept tooContoso Ads but implements more features and more real-world coding practices.</span></span>
* <span data-ttu-id="bc105-504">[Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36) (Többrétegű Azure-felhőszolgáltatás táblákkal, üzenetsorokkal és blobokkal).</span><span class="sxs-lookup"><span data-stu-id="bc105-504">[Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span></span> <span data-ttu-id="bc105-505">Ismerteti az Azure Storage-táblákat, valamint a blobokat és üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="bc105-505">Introduces Azure Storage tables as well as blobs and queues.</span></span> <span data-ttu-id="bc105-506">A .NET-alapú hello Azure SDK-t egy régebbi verziójú, néhány módosítás toowork hello aktuális verziójával van szükség.</span><span class="sxs-lookup"><span data-stu-id="bc105-506">Based on an older version of hello Azure SDK for .NET, will require some modifications toowork with hello current version.</span></span>
* <span data-ttu-id="bc105-507">[Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649) (A Felhőszolgáltatás alapjai a Microsoft Azure-ban).</span><span class="sxs-lookup"><span data-stu-id="bc105-507">[Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="bc105-508">Gyakorlati tanácsok hello Microsoft Patterns és gyakorlatok csoportja által előállított széles skáláját bemutató, átfogó példák.</span><span class="sxs-lookup"><span data-stu-id="bc105-508">A comprehensive sample demonstrating a wide range of best practices, produced by hello Microsoft Patterns and Practices group.</span></span>

<span data-ttu-id="bc105-509">Hello felhő fejlesztésével kapcsolatos általános információkért lásd: [épület valós Felhőalkalmazások Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span><span class="sxs-lookup"><span data-stu-id="bc105-509">For general information about developing for hello cloud, see [Building Real-World Cloud Apps with Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span></span>

<span data-ttu-id="bc105-510">A videó tooAzure tárolási az ajánlott eljárások és minták című [Microsoft Azure Storage –, ajánlott eljárások és minták](http://channel9.msdn.com/Events/Build/2014/3-628).</span><span class="sxs-lookup"><span data-stu-id="bc105-510">For a video introduction tooAzure Storage best practices and patterns, see [Microsoft Azure Storage – What's New, Best Practices and Patterns](http://channel9.msdn.com/Events/Build/2014/3-628).</span></span>

<span data-ttu-id="bc105-511">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="bc105-511">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="bc105-512">Azure Cloud Services – 1. rész: Bevezetés</span><span class="sxs-lookup"><span data-stu-id="bc105-512">Azure Cloud Services Part 1: Introduction</span></span>](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [<span data-ttu-id="bc105-513">Toomanage Cloud Services</span><span class="sxs-lookup"><span data-stu-id="bc105-513">How toomanage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="bc105-514">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bc105-514">Azure Storage</span></span>](/documentation/services/storage/)
* [<span data-ttu-id="bc105-515">Hogyan toochoose felhő szolgáltató</span><span class="sxs-lookup"><span data-stu-id="bc105-515">How toochoose a cloud service provider</span></span>](https://azure.microsoft.com/overview/choosing-a-cloud-service-provider/)
