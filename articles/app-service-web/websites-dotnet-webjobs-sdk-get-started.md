---
title: az Azure App Service .NET webjobs-feladat aaaCreate |} Microsoft Docs
description: "Hozzon létre egy többrétegű alkalmazást ASP.NET MVC és az Azure használatával. hello első end fut egy webalkalmazást az Azure App Service és hello a webjobs-feladat befejezési futtatása gépet. hello app Entity Framework, SQL-adatbázis, és az Azure storage várólisták és blobokat használ."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: mollybos
ms.assetid: 99cb9917-483a-45f8-a98d-07d19c68c753
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/14/2017
ms.author: glenga
ms.openlocfilehash: d92fc4b81cc0d58f8e900f257e747af56d32b911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-webjob-in-azure-app-service"></a><span data-ttu-id="fdf95-105">.NET WebJobs-feladat létrehozása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="fdf95-105">Create a .NET WebJob in Azure App Service</span></span>
<span data-ttu-id="fdf95-106">Ez az oktatóanyag bemutatja, hogyan toowrite kód egy egyszerű többrétegű ASP.NET MVC 5 alkalmazás esetén hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="fdf95-106">This tutorial shows how toowrite code for a simple multi-tier ASP.NET MVC 5 application that uses hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="fdf95-107">hello célját hello [WebJobs SDK](websites-webjobs-resources.md) közös elvégezhető webjobs-feladat, például képek feldolgozását, a várólista feldolgozása, a RSS összesítési, a fájlkarbantartás, írt toosimplify hello kódot, és e-mailek küldésekor.</span><span class="sxs-lookup"><span data-stu-id="fdf95-107">hello purpose of hello [WebJobs SDK](websites-webjobs-resources.md) is toosimplify hello code you write for common tasks that a WebJob can perform, such as image processing, queue processing, RSS aggregation, file maintenance, and sending emails.</span></span> <span data-ttu-id="fdf95-108">hello WebJobs SDK az Azure Storage és a Service Bus, a feladatütemezés és a hibák kezelése és sok más szabhatják beépített szolgáltatásai rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fdf95-108">hello WebJobs SDK has built-in features for working with Azure Storage and Service Bus, for scheduling tasks and handling errors, and for many other common scenarios.</span></span> <span data-ttu-id="fdf95-109">Emellett rendelkezik bővíthető toobe készült, és nincs olyan [nyílt forráskódú extensions tárháza](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span><span class="sxs-lookup"><span data-stu-id="fdf95-109">In addition, it's designed toobe extensible, and there's an [open source repository for extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span></span>

<span data-ttu-id="fdf95-110">hello mintaalkalmazás egy hirdetőtábla.</span><span class="sxs-lookup"><span data-stu-id="fdf95-110">hello sample application is an advertising bulletin board.</span></span> <span data-ttu-id="fdf95-111">Felhasználók feltöltheti ads lemezképeket, és a háttér folyamat hello képek toothumbnails alakítja.</span><span class="sxs-lookup"><span data-stu-id="fdf95-111">Users can upload images for ads, and a backend process converts hello images toothumbnails.</span></span> <span data-ttu-id="fdf95-112">hello ad a lista lapon látható hello miniatűröket, és hello ad részleteit megjelenítő oldalra hello teljes méretű képet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-112">hello ad list page shows hello thumbnails, and hello ad details page shows hello full-size image.</span></span> <span data-ttu-id="fdf95-113">Íme egy képernyőkép:</span><span class="sxs-lookup"><span data-stu-id="fdf95-113">Here's a screenshot:</span></span>

![Hirdetéslista](./media/websites-dotnet-webjobs-sdk-get-started/list.png)

<span data-ttu-id="fdf95-115">A mintaalkalmazás együttműködve [Azure várólisták](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) és [Azure-blobokat](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage).</span><span class="sxs-lookup"><span data-stu-id="fdf95-115">This sample application works with [Azure queues](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) and [Azure blobs](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage).</span></span> <span data-ttu-id="fdf95-116">hello az oktatóanyag bemutatja, hogyan a toodeploy hello alkalmazás túl[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) és [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).</span><span class="sxs-lookup"><span data-stu-id="fdf95-116">hello tutorial shows how toodeploy hello application too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).</span></span>

## <span data-ttu-id="fdf95-117"><a id="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fdf95-117"><a id="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="fdf95-118">hello oktatóanyag feltételezi, hogy ismeri hogyan rendelkező toowork [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) Visual studióban.</span><span class="sxs-lookup"><span data-stu-id="fdf95-118">hello tutorial assumes that you know how toowork with [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projects in Visual Studio.</span></span>

<span data-ttu-id="fdf95-119">hello oktatóanyag eredetileg készült Visual Studio 2013, de a Visual Studio újabb verzióival használható.</span><span class="sxs-lookup"><span data-stu-id="fdf95-119">hello tutorial was originally written for Visual Studio 2013, but can be used with later versions of Visual Studio.</span></span> <span data-ttu-id="fdf95-120">Ha a Visual Studio 2015-öt vagy 2017 használ, jegyezze fel, hogy mielőtt hello alkalmazás helyi futtatásához, meg kell változtatnia hello `Data Source` hello SQL Server LocalDB kapcsolati karakterláncot a hello Web.config és App.config fájlok egy részének `Data Source=(localdb)\v11.0` túl`Data Source=(LocalDb)\MSSQLLocalDB`.</span><span class="sxs-lookup"><span data-stu-id="fdf95-120">If you are using Visual Studio 2015 or 2017, make note that before you run hello application locally, you must change hello `Data Source` part of hello SQL Server LocalDB connection string in hello Web.config and App.config files from `Data Source=(localdb)\v11.0` too`Data Source=(LocalDb)\MSSQLLocalDB`.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf95-121"><a name="note"></a>Ez az oktatóanyag egy Azure-fiók toocomplete kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="fdf95-121"><a name="note"></a>You must have an Azure account toocomplete this tutorial:</span></span>
>
> * <span data-ttu-id="fdf95-122">Is [nyissa meg az Azure-fiók szabad](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): kapott kreditek, hogy használhatja ki tootry fizetős Azure-szolgáltatásokat, és még azok lejárta után, megtarthatja hello fiókot, és ingyenes, például a webhelyek szolgáltatásokat használja.</span><span class="sxs-lookup"><span data-stu-id="fdf95-122">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): You get credits that you can use tootry out paid Azure services, and even after they're used up, you can keep hello account and use free Azure services, such as Websites.</span></span> <span data-ttu-id="fdf95-123">A hitelkártya soha nem lesz felszámítva, kivéve, ha explicit módon a beállítások módosításához és kérje meg a toobe számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="fdf95-123">Your credit card will never be charged, unless you explicitly change your settings and ask toobe charged.</span></span>
> * <span data-ttu-id="fdf95-124">Is [aktiválása havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): az előfizetés biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-124">You can [activate Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Your subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="fdf95-125">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="fdf95-125">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="fdf95-126">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="fdf95-126">No credit cards required; no commitments.</span></span>
>
>

## <span data-ttu-id="fdf95-127"><a id="learn"></a>Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="fdf95-127"><a id="learn"></a>What you'll learn</span></span>
<span data-ttu-id="fdf95-128">hello az oktatóanyag bemutatja, hogyan toodo hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="fdf95-128">hello tutorial shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="fdf95-129">Engedélyezze a számítógépen az Azure fejlesztési hello Azure SDK telepítése (csak a Visual Studio 2013 és a felhasználók 2015).</span><span class="sxs-lookup"><span data-stu-id="fdf95-129">Enable your machine for Azure development by installing hello Azure SDK (only for Visual Studio 2013 and 2015 users).</span></span>
* <span data-ttu-id="fdf95-130">Hozzon létre egy konzolalkalmazás-projektet, amely automatikusan telepíti az Azure webjobs-feladatot, amikor hello társított webes projekt telepítése.</span><span class="sxs-lookup"><span data-stu-id="fdf95-130">Create a Console Application project that automatically deploys as an Azure WebJob when you deploy hello associated web project.</span></span>
* <span data-ttu-id="fdf95-131">A WebJobs SDK háttérrendszer helyi számítógépen hello fejlesztési tesztelése.</span><span class="sxs-lookup"><span data-stu-id="fdf95-131">Test a WebJobs SDK backend locally on hello development computer.</span></span>
* <span data-ttu-id="fdf95-132">A webjobs-feladatok háttér tooa webalkalmazás alkalmazás közzététele az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="fdf95-132">Publish an application with a WebJobs backend tooa web app in App Service.</span></span>
* <span data-ttu-id="fdf95-133">Fájlok feltöltése, és tárolja őket a hello Azure Blob szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="fdf95-133">Upload files and store them in hello Azure Blob service.</span></span>
* <span data-ttu-id="fdf95-134">Hello Azure WebJobs SDK toowork használata Azure Storage üzenetsorokat és blobokat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-134">Use hello Azure WebJobs SDK toowork with Azure Storage queues and blobs.</span></span>

## <span data-ttu-id="fdf95-135"><a id="contosoads"></a>Alkalmazás-architektúra</span><span class="sxs-lookup"><span data-stu-id="fdf95-135"><a id="contosoads"></a>Application architecture</span></span>
<span data-ttu-id="fdf95-136">hello mintaalkalmazást használ hello [várólista-központú](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-betöltési hello processzorigényes munkáján miniatűrök tooa háttér folyamat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fdf95-136">hello sample application uses hello [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-load hello CPU-intensive work of creating thumbnails tooa backend process.</span></span>

<span data-ttu-id="fdf95-137">hello alkalmazást használó Entity Framework Code First toocreate hello táblák és -hozzáférési hello adatok SQL-adatbázisban tárolja a hirdetéseket.</span><span class="sxs-lookup"><span data-stu-id="fdf95-137">hello app stores ads in a SQL database, using Entity Framework Code First toocreate hello tables and access hello data.</span></span> <span data-ttu-id="fdf95-138">Az egyes hirdetések hello adatbázis két URL-címek tárolja: hello teljes méretű képhez, egyet, hello miniatűr egy.</span><span class="sxs-lookup"><span data-stu-id="fdf95-138">For each ad, hello database stores two URLs: one for hello full-size image and one for hello thumbnail.</span></span>

![Hirdetés tábla](./media/websites-dotnet-webjobs-sdk-get-started/adtable.png)

<span data-ttu-id="fdf95-140">Amikor egy felhasználó feltölt egy képet, hello webalkalmazás hello-lemezképet tárolja egy [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), és hello ad adatokat toohello blob URL-CÍMMEL rendelkező hello adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="fdf95-140">When a user uploads an image, hello web app stores hello image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores hello ad information in hello database with a URL that points toohello blob.</span></span> <span data-ttu-id="fdf95-141">At hello azonos időben, egy üzenet tooan Azure üzenetsor-kezelési ír.</span><span class="sxs-lookup"><span data-stu-id="fdf95-141">At hello same time, it writes a message tooan Azure queue.</span></span> <span data-ttu-id="fdf95-142">Az Azure webjobs-feladat futtató háttérkiszolgáló folyamatban hello WebJobs SDK kérdezze le az új üzenetek hello várólista.</span><span class="sxs-lookup"><span data-stu-id="fdf95-142">In a backend process running as an Azure WebJob, hello WebJobs SDK polls hello queue for new messages.</span></span> <span data-ttu-id="fdf95-143">Egy új üzenet jelenik meg, amikor hello webjobs-feladat létrehoz egy kép miniatűrjét, és a frissítések hello Miniatűr URL-adatbázis mező az adott Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fdf95-143">When a new message appears, hello WebJob creates a thumbnail for that image and updates hello thumbnail URL database field for that ad.</span></span> <span data-ttu-id="fdf95-144">Íme egy diagram bemutatja, hogyan működnek együtt hello alkalmazás hello részei:</span><span class="sxs-lookup"><span data-stu-id="fdf95-144">Here's a diagram that shows how hello parts of hello application interact:</span></span>

![Contoso Ads architektúra](./media/websites-dotnet-webjobs-sdk-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]  
<span data-ttu-id="fdf95-146">hello oktatóanyagban szereplő utasítások alkalmazása tooAzure SDK for .NET 2.7.1-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fdf95-146">hello tutorial instructions apply tooAzure SDK for .NET 2.7.1 or later.</span></span>

## <span data-ttu-id="fdf95-147"><a id="storage"></a>Egy Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdf95-147"><a id="storage"></a>Create an Azure Storage account</span></span>
<span data-ttu-id="fdf95-148">Azure-tárfiók forrásokat biztosít hello felhőben üzenetsor és a blob adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="fdf95-148">An Azure storage account provides resources for storing queue and blob data in hello cloud.</span></span> <span data-ttu-id="fdf95-149">Azt is használják hello WebJobs SDK toostore naplózási adatok hello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="fdf95-149">It's also used by hello WebJobs SDK toostore logging data for hello dashboard.</span></span>

<span data-ttu-id="fdf95-150">Egy valós alkalmazás általában létrehozhat külön az alkalmazásadatok és a naplózási adatok és külön fiókok a Tesztadatok és termelési adatok.</span><span class="sxs-lookup"><span data-stu-id="fdf95-150">In a real-world application, you typically create separate accounts for application data versus logging data and separate accounts for test data versus production data.</span></span> <span data-ttu-id="fdf95-151">Ebben az oktatóanyagban csak egy fiókot fog használni.</span><span class="sxs-lookup"><span data-stu-id="fdf95-151">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="fdf95-152">Nyissa meg hello **Server Explorer** ablak a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="fdf95-152">Open hello **Server Explorer** window in Visual Studio.</span></span>
2. <span data-ttu-id="fdf95-153">Kattintson a jobb gombbal hello **Azure** csomópontra, majd **csatlakozás Azure-előfizetés tooMicrosoft...** .</span><span class="sxs-lookup"><span data-stu-id="fdf95-153">Right-click hello **Azure** node, and then click **Connect tooMicrosoft Azure Subscription...**.</span></span>
   
   ![Csatlakozás tooAzure](./media/websites-dotnet-webjobs-sdk-get-started/connaz.png)

3. <span data-ttu-id="fdf95-155">Jelentkezzen be Azure hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="fdf95-155">Sign in using your Azure credentials.</span></span>
4. <span data-ttu-id="fdf95-156">Kattintson a jobb gombbal **tárolási** hello Azure csomópont, és kattintson a **Storage-fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-156">Right-click **Storage** under hello Azure node, and then click **Create Storage Account**.</span></span>
   
   ![Storage-fiók létrehozása](./media/websites-dotnet-webjobs-sdk-get-started/createstor.png)
   
5. <span data-ttu-id="fdf95-158">A hello **Storage-fiók létrehozása** párbeszédpanelen adja meg hello storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="fdf95-158">In hello **Create Storage Account** dialog, enter a name for hello storage account.</span></span>

    <span data-ttu-id="fdf95-159">hello nevének meg kell egyedinek kell lennie (egyéb Azure storage-fiók hello rendelkezhet ugyanazzal a névvel).</span><span class="sxs-lookup"><span data-stu-id="fdf95-159">hello name must be must be unique (no other Azure storage account can have hello same name).</span></span> <span data-ttu-id="fdf95-160">Ha hello név már használatban van, kap egy alkalommal toochange azt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-160">If hello name you enter is already in use, you'll get a chance toochange it.</span></span>

    <span data-ttu-id="fdf95-161">a tárfiók lesz URL-cím tooaccess hello *{name}*. core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="fdf95-161">hello URL tooaccess your storage account will be *{name}*.core.windows.net.</span></span>
6. <span data-ttu-id="fdf95-162">Set hello **régiót vagy Affinitáscsoportot** legördülő lista toohello régió legközelebbi tooyou.</span><span class="sxs-lookup"><span data-stu-id="fdf95-162">Set hello **Region or Affinity Group** drop-down list toohello region closest tooyou.</span></span>

    <span data-ttu-id="fdf95-163">A beállítással megadható, hogy melyik Azure-adatközpontban fogja futtatni a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="fdf95-163">This setting specifies which Azure datacenter will host your storage account.</span></span> <span data-ttu-id="fdf95-164">Ebben az oktatóanyagban a választott érdekében nem érzékelhető különbség.</span><span class="sxs-lookup"><span data-stu-id="fdf95-164">For this tutorial, your choice won't make a noticeable difference.</span></span> <span data-ttu-id="fdf95-165">Azonban éles web app alkalmazásban keresi a webkiszolgáló és a tárolási fiók toobe hello az ugyanabban a régióban toominimize késést és az adatok kilépési díjakat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-165">However, for a production web app, you want your web server and your storage account toobe in hello same region toominimize latency and data egress charges.</span></span> <span data-ttu-id="fdf95-166">hello web app (amely hoz létre, később) datacenter kell lennie, mint a lehető toohello böngészők hello webalkalmazás elérése a rendelés toominimize késés bezárásához.</span><span class="sxs-lookup"><span data-stu-id="fdf95-166">hello web app (which you'll create later) datacenter should be as close as possible toohello browsers accessing hello web app in order toominimize latency.</span></span>
7. <span data-ttu-id="fdf95-167">Set hello **replikációs** legördülő lista túl**helyileg redundáns**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-167">Set hello **Replication** drop-down list too**Locally redundant**.</span></span>

    <span data-ttu-id="fdf95-168">A georeplikáció engedélyezve van a tárfiók, hello tárolt tartalom esetén meg kell replikált tooa másodlagos adatközpontba tooenable feladatátvételi toothat helyévé hello elsődleges helyen jelentős katasztrófa következik.</span><span class="sxs-lookup"><span data-stu-id="fdf95-168">When geo-replication is enabled for a storage account, hello stored content is replicated tooa secondary datacenter tooenable failover toothat location in case of a major disaster in hello primary location.</span></span> <span data-ttu-id="fdf95-169">A georeplikáció további költségeket vonhat maga után.</span><span class="sxs-lookup"><span data-stu-id="fdf95-169">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="fdf95-170">A vizsgálati és fejlesztői fiókok esetében általában nem érdemes toopay a georeplikációért.</span><span class="sxs-lookup"><span data-stu-id="fdf95-170">For test and development accounts, you generally don't want toopay for geo-replication.</span></span> <span data-ttu-id="fdf95-171">További információ: [Tárfiók létrehozása, kezelése vagy törlése](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="fdf95-171">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>
8. <span data-ttu-id="fdf95-172">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf95-172">Click **Create**.</span></span>

    ![Új tárfiók](./media/websites-dotnet-webjobs-sdk-get-started/newstorage.png)

## <span data-ttu-id="fdf95-174"><a id="download"></a>Hello alkalmazás letöltése</span><span class="sxs-lookup"><span data-stu-id="fdf95-174"><a id="download"></a>Download hello application</span></span>
1. <span data-ttu-id="fdf95-175">Töltse le és csomagolja ki a hello [kész megoldást](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).</span><span class="sxs-lookup"><span data-stu-id="fdf95-175">Download and unzip hello [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).</span></span>
2. <span data-ttu-id="fdf95-176">Indítsa el a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="fdf95-176">Start Visual Studio.</span></span>
3. <span data-ttu-id="fdf95-177">A hello **fájl** menüben válassza **nyissa meg a > Projekt/megoldás**, és keresse meg a letöltött hello megoldás toowhere, majd nyissa meg a megoldásfájlt hello.</span><span class="sxs-lookup"><span data-stu-id="fdf95-177">From hello **File** menu choose **Open > Project/Solution**, navigate toowhere you downloaded hello solution, and then open hello solution file.</span></span>
4. <span data-ttu-id="fdf95-178">Nyomja le a CTRL + SHIFT + B toobuild hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="fdf95-178">Press CTRL+SHIFT+B toobuild hello solution.</span></span>

    <span data-ttu-id="fdf95-179">Alapértelmezés szerint a Visual Studio automatikusan visszaállítja hello NuGet-csomag tartalmát, amely nem szerepel a hello *.zip* fájlt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-179">By default, Visual Studio automatically restores hello NuGet package content, which was not included in hello *.zip* file.</span></span> <span data-ttu-id="fdf95-180">Hello csomagok nem állnak vissza, ha a telepítést, manuálisan is toohello **NuGet-csomagok kezelése megoldáshoz** párbeszédpanel, kattintson a hello **visszaállítása** hello felső jobb oldali gomb.</span><span class="sxs-lookup"><span data-stu-id="fdf95-180">If hello packages don't restore, install them manually by going toohello **Manage NuGet Packages for Solution** dialog and clicking hello **Restore** button at hello top right.</span></span>
5. <span data-ttu-id="fdf95-181">A **Megoldáskezelőben**, győződjön meg arról, hogy **ContosoAdsWeb** hello indítási projekt legyen kijelölve.</span><span class="sxs-lookup"><span data-stu-id="fdf95-181">In **Solution Explorer**, make sure that **ContosoAdsWeb** is selected as hello startup project.</span></span>

## <span data-ttu-id="fdf95-182"><a id="configurestorage"></a>A tárfiók hello alkalmazás toouse konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdf95-182"><a id="configurestorage"></a>Configure hello application toouse your storage account</span></span>
1. <span data-ttu-id="fdf95-183">Nyissa meg a hello alkalmazás *Web.config* fájl hello ContosoAdsWeb projektben.</span><span class="sxs-lookup"><span data-stu-id="fdf95-183">Open hello application *Web.config* file in hello ContosoAdsWeb project.</span></span>

    <span data-ttu-id="fdf95-184">SQL kapcsolati karakterlánc és az Azure tárolási kapcsolati karakterlánc blobokat és üzenetsorokat való munkához hello-fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fdf95-184">hello file contains a SQL connection string and an Azure storage connection string for working with blobs and queues.</span></span>

    <span data-ttu-id="fdf95-185">SQL kapcsolati karakterlánc hello mutat tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fdf95-185">hello SQL connection string points tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

    <span data-ttu-id="fdf95-186">hello tárolási kapcsolati karakterlánc, amely rendelkezik a tárolási fiók nevét és hívóbetűjét hello helyőrzőit példa.</span><span class="sxs-lookup"><span data-stu-id="fdf95-186">hello storage connection string is an example that has placeholders for hello storage account name and access key.</span></span> <span data-ttu-id="fdf95-187">Cseréli le ez egy olyan kapcsolati karakterlánccal, amely rendelkezik hello nevét és a tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="fdf95-187">You'll replace this with a connection string that has hello name and key of your storage account.</span></span>  

    ```
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
    </connectionStrings>
    ```
    <span data-ttu-id="fdf95-188">hello tárolási kapcsolati karakterlánc neve AzureWebJobsStorage hello neve hello WebJobs SDK használja, mert alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="fdf95-188">hello storage connection string is named AzureWebJobsStorage because that's hello name hello WebJobs SDK uses by default.</span></span> <span data-ttu-id="fdf95-189">hello ugyanazzal a névvel itt van használatban, így hello Azure környezetben vannak tooset csak egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="fdf95-189">hello same name is used here so you have tooset only one connection string value in hello Azure environment.</span></span>
2. <span data-ttu-id="fdf95-190">A **Server Explorer**, kattintson a jobb gombbal a tárfiókba hello **tárolási** csomópontra, majd **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-190">In **Server Explorer**, right-click your storage account under hello **Storage** node, and then click **Properties**.</span></span>

    ![Kattintson a Tárfiók tulajdonságai](./media/websites-dotnet-webjobs-sdk-get-started/storppty.png)
3. <span data-ttu-id="fdf95-192">A hello **tulajdonságok** ablak, kattintson a **Tárfiókkulcsok**, és kattintson a hello három pont.</span><span class="sxs-lookup"><span data-stu-id="fdf95-192">In hello **Properties** window, click **Storage Account Keys**, and then click hello ellipsis.</span></span>

    ![Tárfiókkulcsok](./media/websites-dotnet-webjobs-sdk-get-started/stor-account-keys.png)
4. <span data-ttu-id="fdf95-194">Másolás hello **kapcsolati karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-194">Copy hello **Connection String**.</span></span>

    ![Tároló kulcsait párbeszédpanel](./media/websites-dotnet-webjobs-sdk-get-started/cpak.png)
5. <span data-ttu-id="fdf95-196">Cserélje le a hello tárolási kapcsolat karakterláncát a hello *Web.config* imént kimásolt kapcsolati karakterláncot hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-196">Replace hello storage connection string in hello *Web.config* file with hello connection string you just copied.</span></span> <span data-ttu-id="fdf95-197">Gondoskodjon róla, hogy minden hello idézőjelek közötti, de nem tartalmazza a beillesztés előtt hello idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="fdf95-197">Make sure you select everything inside hello quotation marks but not including hello quotation marks before pasting.</span></span>
6. <span data-ttu-id="fdf95-198">Nyissa meg hello *App.config* fájl hello ContosoAdsWebJob projektben.</span><span class="sxs-lookup"><span data-stu-id="fdf95-198">Open hello *App.config* file in hello ContosoAdsWebJob project.</span></span>

    <span data-ttu-id="fdf95-199">Ez a fájl két tárolási kapcsolati karakterlánc, az alkalmazásadatok egyet, majd egy naplózási rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fdf95-199">This file has two storage connection strings, one for application data and one for logging.</span></span> <span data-ttu-id="fdf95-200">Külön tárfiókok használata alkalmazásadatok és a naplózás, és használhatja [több tárfiókot adatok](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="fdf95-200">You can use separate storage accounts for application data and logging, and you can use [multiple storage accounts for data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span> <span data-ttu-id="fdf95-201">Ebben az oktatóanyagban egy tárfiókot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="fdf95-201">For this tutorial, you'll use a single storage account.</span></span> <span data-ttu-id="fdf95-202">hello kapcsolati karakterláncok hello tárfiókkulcsok helyőrzőit rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fdf95-202">hello connection strings have placeholders for hello storage account keys.</span></span>

    ```
    <configuration>
        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;"/>
    </connectionStrings>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    </configuration>

    ```

    <span data-ttu-id="fdf95-203">Alapértelmezés szerint hello WebJobs SDK AzureWebJobsStorage és AzureWebJobsDashboard nevű kapcsolati karakterláncok keresi.</span><span class="sxs-lookup"><span data-stu-id="fdf95-203">By default, hello WebJobs SDK looks for connection strings named AzureWebJobsStorage and AzureWebJobsDashboard.</span></span> <span data-ttu-id="fdf95-204">Alternatív megoldásként is [hello kapcsolati karakterláncot tárolni szeretné, és adja át azt kifejezetten toohello azonban `JobHost` objektum](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).</span><span class="sxs-lookup"><span data-stu-id="fdf95-204">As an alternative, you can [store hello connection string however you want and pass it in explicitly toohello `JobHost` object](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).</span></span>
7. <span data-ttu-id="fdf95-205">Cserélje le mindkét tárolási kapcsolati karakterláncok korábban kimásolt hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="fdf95-205">Replace both storage connection strings with hello connection string you copied earlier.</span></span>
8. <span data-ttu-id="fdf95-206">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-206">Save your changes.</span></span>

## <span data-ttu-id="fdf95-207"><a id="run"></a>Hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="fdf95-207"><a id="run"></a>Run hello application locally</span></span>
1. <span data-ttu-id="fdf95-208">toostart hello webes előtér hello alkalmazás, nyomja le a CTRL + F5 billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-208">toostart hello web frontend of hello application, press CTRL+F5.</span></span>

    <span data-ttu-id="fdf95-209">hello alapértelmezett böngésző megnyitja toohello kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="fdf95-209">hello default browser opens toohello home page.</span></span> <span data-ttu-id="fdf95-210">(hello webes projekt hajtott végre, mert fut hello kezdőprojektként.)</span><span class="sxs-lookup"><span data-stu-id="fdf95-210">(hello web project runs because you've made it hello startup project.)</span></span>

    ![Contoso Ads kezdőlap](./media/websites-dotnet-webjobs-sdk-get-started/home.png)
2. <span data-ttu-id="fdf95-212">toostart hello webjobs-feladat háttér hello alkalmazás, kattintson a jobb gombbal a hello ContosoAdsWebJob projektre a **Megoldáskezelőben**, és kattintson a **Debug** > **Start új példányt** .</span><span class="sxs-lookup"><span data-stu-id="fdf95-212">toostart hello WebJob backend of hello application, right-click hello ContosoAdsWebJob project in **Solution Explorer**, and then click **Debug** > **Start new instance**.</span></span>

    <span data-ttu-id="fdf95-213">Egy alkalmazás konzolablak megnyílik, és jelző hello WebJobs SDK JobHost objektum megkezdődött toorun naplózási üzeneteket jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-213">A console application window opens and displays logging messages indicating hello WebJobs SDK JobHost object has started toorun.</span></span>

    ![Megjeleníti az adott hello háttér alkalmazás konzolablak fut.](./media/websites-dotnet-webjobs-sdk-get-started/backendrunning.png)
3. <span data-ttu-id="fdf95-215">A böngészőben kattintson **hoznak létre hirdetéseket**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-215">In your browser, click  **Create an Ad**.</span></span>
4. <span data-ttu-id="fdf95-216">Adjon meg néhány tesztadatot, egy kép tooupload válassza ki, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-216">Enter some test data, select an image tooupload, and then click **Create**.</span></span>

    ![Lap létrehozása](./media/websites-dotnet-webjobs-sdk-get-started/create.png)

    <span data-ttu-id="fdf95-218">hello app toohello Index lapra kerül, de hello új hirdetés miniatűrje nem jelenik meg, mert a feldolgozása még nem történt meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-218">hello app goes toohello Index page, but it doesn't show a thumbnail for hello new ad because that processing hasn't happened yet.</span></span>

    <span data-ttu-id="fdf95-219">Eközben a rövid várakozás után naplózási megjelenik egy üzenet hello alkalmazás konzolablakban, hogy a várólista üzenet érkezett, de feldolgozása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="fdf95-219">Meanwhile, after a short wait a logging message in hello console application window shows that a queue message was received and has been processed.</span></span>

    ![Megjeleníti, hogy a várólista-üzenet feldolgozása alkalmazás konzolablak](./media/websites-dotnet-webjobs-sdk-get-started/backendlogs.png)
5. <span data-ttu-id="fdf95-221">Miután meggyőződött arról, hogy a naplózási üzenetek hello alkalmazás konzolablakban hello, hello Index lap toosee hello miniatűrök frissítése.</span><span class="sxs-lookup"><span data-stu-id="fdf95-221">After you see hello logging messages in hello console application window, refresh hello Index page toosee hello thumbnail.</span></span>

    ![Index lap](./media/websites-dotnet-webjobs-sdk-get-started/list.png)
6. <span data-ttu-id="fdf95-223">Kattintson a **részletek** a ad toosee hello teljes méretű kép.</span><span class="sxs-lookup"><span data-stu-id="fdf95-223">Click **Details** for your ad toosee hello full-size image.</span></span>

    ![Részletek lap](./media/websites-dotnet-webjobs-sdk-get-started/details.png)

<span data-ttu-id="fdf95-225">Ön már futott hello alkalmazás a helyi számítógépen, és egy SQL Server adatbázis található a számítógépen, de az üzenetsorok működik, és a blobok használ hello felhő.</span><span class="sxs-lookup"><span data-stu-id="fdf95-225">You've been running hello application on your local computer, and it's using a SQL Server database located on your computer, but it's working with queues and blobs in hello cloud.</span></span> <span data-ttu-id="fdf95-226">A következő szakasz hello fogja futtatni hello alkalmazás hello felhőben felhő adatbázis, valamint a felhő blobokat és üzenetsorokat használ.</span><span class="sxs-lookup"><span data-stu-id="fdf95-226">In hello following section you'll run hello application in hello cloud, using a cloud database as well as cloud blobs and queues.</span></span>  

## <span data-ttu-id="fdf95-227"><a id="runincloud"></a>Hello felhő hello alkalmazást futtat</span><span class="sxs-lookup"><span data-stu-id="fdf95-227"><a id="runincloud"></a>Run hello application in hello cloud</span></span>
<span data-ttu-id="fdf95-228">El kell végeznie a következő lépéseket toorun hello alkalmazás hello felhőben hello:</span><span class="sxs-lookup"><span data-stu-id="fdf95-228">You'll do hello following steps toorun hello application in hello cloud:</span></span>

* <span data-ttu-id="fdf95-229">TooWeb alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="fdf95-229">Deploy tooWeb Apps.</span></span> <span data-ttu-id="fdf95-230">A Visual Studio automatikusan létrehoz egy új webalkalmazást az App Service és az SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-230">Visual Studio automatically creates a new web app in App Service and a SQL Database instance.</span></span>
* <span data-ttu-id="fdf95-231">Hello web app toouse Azure SQL adatbázis és a tárolási fiók beállítása.</span><span class="sxs-lookup"><span data-stu-id="fdf95-231">Configure hello web app toouse your Azure SQL database and storage account.</span></span>

<span data-ttu-id="fdf95-232">Miután létrehozta a hirdetések hello felhőben futtatása során, megtekintheti lesz hello WebJobs SDK irányítópult toosee hello gazdag figyelési funkciókat toooffer rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fdf95-232">After you've created some ads while running in hello cloud, you'll view hello WebJobs SDK dashboard toosee hello rich monitoring features it has toooffer.</span></span>

### <a name="deploy-tooweb-apps"></a><span data-ttu-id="fdf95-233">TooWeb alkalmazások telepítése</span><span class="sxs-lookup"><span data-stu-id="fdf95-233">Deploy tooWeb Apps</span></span>

1. <span data-ttu-id="fdf95-234">Hello böngésző és hello console application ablak bezárása.</span><span class="sxs-lookup"><span data-stu-id="fdf95-234">Close hello browser and hello console application window.</span></span>
2. <span data-ttu-id="fdf95-235">Hello kövesse hello [közzététele az SQL Database tooAzure](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) szakasz.</span><span class="sxs-lookup"><span data-stu-id="fdf95-235">Follow hello steps in hello [Publish tooAzure with SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) section.</span></span>
3. <span data-ttu-id="fdf95-236">Hello központi telepítésének lépései befejezése után folytassa a hátralévő feladatok hello ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="fdf95-236">After you complete hello steps for deploying, continue with hello remaining tasks in this article.</span></span>

### <a name="configure-hello-web-app-toouse-your-azure-sql-database-and-storage-account"></a><span data-ttu-id="fdf95-237">Hello web app toouse Azure SQL adatbázis és a tárolási fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="fdf95-237">Configure hello web app toouse your Azure SQL database and storage account</span></span>
<span data-ttu-id="fdf95-238">Biztonsági szempontból ajánlott túl[ne tegye a bizalmas adatokat például kapcsolati karakterláncok forráskódú adattárakban tárolt fájlok](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span><span class="sxs-lookup"><span data-stu-id="fdf95-238">It's a security best practice too[avoid putting sensitive information such as connection strings in files that are stored in source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span> <span data-ttu-id="fdf95-239">Azure tartalmaz egy módja toodo, amely: kapcsolati karakterlánc és más beállításértékek beállítható hello Azure-környezetéhez, és az ASP.NET konfigurációs API-k automatikusan átvételéhez ezeket az értékeket, az Azure-ban hello alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="fdf95-239">Azure provides a way toodo that: you can set connection string and other setting values in hello Azure environment, and ASP.NET configuration APIs automatically pick up these values when hello app runs in Azure.</span></span> <span data-ttu-id="fdf95-240">Beállíthatja ezeket az értékeket az Azure-ban használatával **Server Explorer**, hello Azure portálon, a Windows PowerShell vagy a platformfüggetlen parancssori felületre hello.</span><span class="sxs-lookup"><span data-stu-id="fdf95-240">You can set these values in Azure by using **Server Explorer**, hello Azure Portal, Windows PowerShell, or hello cross-platform command-line interface.</span></span> <span data-ttu-id="fdf95-241">További információkért lásd: [hogyan alkalmazás karakterláncok és a kapcsolati karakterláncok munkahelyi](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="fdf95-241">For more information, see [How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span>

<span data-ttu-id="fdf95-242">Ebben a szakaszban használhatja **Server Explorer** tooset kapcsolati karakterlánc-értékek az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="fdf95-242">In this section, you use **Server Explorer** tooset connection string values in Azure.</span></span>

1. <span data-ttu-id="fdf95-243">A **Server Explorer**, kattintson a jobb gombbal a web app alatt **Azure > az App Service > {az erőforráscsoport}**, és kattintson a **nézetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-243">In **Server Explorer**, right-click your web app under **Azure > App Service > {your resource group}**, and then click **View Settings**.</span></span>

    <span data-ttu-id="fdf95-244">Hello **Azure Web Apps** ablak nyílik meg a hello **konfigurációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="fdf95-244">hello **Azure Web App** window opens on hello **Configuration** tab.</span></span>
2. <span data-ttu-id="fdf95-245">Hello nevének módosítása hello DefaultConnection kapcsolati karakterlánc toohello nevű úgy döntött, hogy mikor konfigurált SQL-adatbázis hello hello [közzététele az SQL Database tooAzure](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) cikk.</span><span class="sxs-lookup"><span data-stu-id="fdf95-245">Change hello name of hello DefaultConnection connection string toohello name you chose when you configured hello SQL database in hello [Publish tooAzure with SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) article.</span></span>

    <span data-ttu-id="fdf95-246">Azure automatikusan létrejön, ez a kapcsolati karakterlánc létrehozott hello webalkalmazáshoz társított adatbázissal, így már hello megfelelő kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="fdf95-246">Azure automatically created this connection string when you created hello web app with an associated database, so it already has hello right connection string value.</span></span> <span data-ttu-id="fdf95-247">Csak hello neve toowhat keres a kód módosítása.</span><span class="sxs-lookup"><span data-stu-id="fdf95-247">You're changing just hello name toowhat your code is looking for.</span></span>
3. <span data-ttu-id="fdf95-248">Adjon hozzá két új kapcsolati karakterláncokat AzureWebJobsStorage és AzureWebJobsDashboard nevű.</span><span class="sxs-lookup"><span data-stu-id="fdf95-248">Add two new connection strings, named AzureWebJobsStorage and AzureWebJobsDashboard.</span></span> <span data-ttu-id="fdf95-249">Hello adatbázis típusának beállítása túl**egyéni**, és a set hello kapcsolati karakterlánc értékét toohello azonos értéket használta korábban a hello *Web.config* és *App.config* fájlokat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-249">Set hello database type too**Custom**, and set hello connection string value toohello same value that you used earlier for hello *Web.config* and *App.config* files.</span></span> <span data-ttu-id="fdf95-250">(Ügyeljen hello teljes kapcsolati karakterlánc, nem csak a hello elérési kulcsot tartalmaz, és nem jelennek meg a hello idézőjelek között.)</span><span class="sxs-lookup"><span data-stu-id="fdf95-250">(Be sure you include hello entire connection string, not just hello access key, and don't include hello quotation marks.)</span></span>

    <span data-ttu-id="fdf95-251">A kapcsolati karakterláncok hello WebJobs SDK, az alkalmazásadatok egy és egy, a naplózás által használt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-251">These connection strings are used by hello WebJobs SDK, one for application data and one for logging.</span></span> <span data-ttu-id="fdf95-252">Mivel a korábban hello az alkalmazásadatok is használják hello webes előtér-kódját.</span><span class="sxs-lookup"><span data-stu-id="fdf95-252">As you saw earlier, hello one for application data is also used by hello web front-end code.</span></span>
4. <span data-ttu-id="fdf95-253">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf95-253">Click **Save**.</span></span>

    ![Kapcsolati karakterláncok az Azure portálon](./media/websites-dotnet-webjobs-sdk-get-started/azconnstr.png)
5. <span data-ttu-id="fdf95-255">A **Server Explorer**, kattintson a jobb gombbal a hello webalkalmazást, és kattintson a **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-255">In **Server Explorer**, right-click hello web app, and then click **Stop**.</span></span>
6. <span data-ttu-id="fdf95-256">Miután hello webes alkalmazás leáll, kattintson ismét a jobb gombbal hello webalkalmazás, és kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-256">After hello web app stops, right-click hello web app again, and then click **Start**.</span></span>

   <span data-ttu-id="fdf95-257">hello webjobs-feladat automatikusan elindul, amikor a tesz közzé, de az megáll, amikor olyan konfigurációs módosítást hajt végre.</span><span class="sxs-lookup"><span data-stu-id="fdf95-257">hello WebJob automatically starts when you publish, but it stops when you make a configuration change.</span></span> <span data-ttu-id="fdf95-258">toorestart, hello a webalkalmazás újraindítása, vagy indítsa újra a webjobs-feladat hello hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="fdf95-258">toorestart it, you can either restart hello web app or restart hello WebJob in hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="fdf95-259">A konfiguráció módosítása után általában ajánlott toorestart hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdf95-259">It's generally recommended toorestart hello web app after a configuration change.</span></span>
7. <span data-ttu-id="fdf95-260">Frissítse a hello böngészőablakban, amely rendelkezik hello webes alkalmazás URL-CÍMÉT a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="fdf95-260">Refresh hello browser window that has hello web app URL in its address bar.</span></span>

    <span data-ttu-id="fdf95-261">hello kezdőlap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-261">hello home page appears.</span></span>
8. <span data-ttu-id="fdf95-262">Hozzon létre egy Active Directory, úgy, ahogy mikor meg [hello alkalmazás helyileg futtatta](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).</span><span class="sxs-lookup"><span data-stu-id="fdf95-262">Create an ad, as you did when you [ran hello application locally](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).</span></span>

   <span data-ttu-id="fdf95-263">hello Index lapot a miniatűr először nélkül jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-263">hello Index page shows without a thumbnail at first.</span></span>
9. <span data-ttu-id="fdf95-264">Néhány másodperc múlva frissítse a hello lapot, és hello miniatűr jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-264">Refresh hello page after a few seconds and hello thumbnail appears.</span></span>

   <span data-ttu-id="fdf95-265">Hello miniatűrje nem jelenik meg, ha lehetséges, hogy toowait egy percet a hello webjobs-feladat toorestart.</span><span class="sxs-lookup"><span data-stu-id="fdf95-265">If hello thumbnail doesn't appear, you might have toowait a minute or so for hello WebJob toorestart.</span></span> <span data-ttu-id="fdf95-266">Után igénybe, ha még nem látja hello miniatűr hello lapon hello webjobs-feladat frissítésekor előfordulhat, hogy nem indult el automatikusan.</span><span class="sxs-lookup"><span data-stu-id="fdf95-266">If after a while, you still don't see hello thumbnail when you refresh hello page, hello WebJob might not have started automatically.</span></span> <span data-ttu-id="fdf95-267">Ebben az esetben lépjen toohello **alkalmazásszolgáltatások** hello paneljén [Azure-portálon](https://portal.azure.com/), keresse meg a webes alkalmazást, és kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-267">In that case, go toohello **App Services** blade in hello [Azure portal](https://portal.azure.com/), locate your web app, and then click **Start**.</span></span>

### <a name="view-hello-webjobs-sdk-dashboard"></a><span data-ttu-id="fdf95-268">Nézet hello WebJobs SDK irányítópult</span><span class="sxs-lookup"><span data-stu-id="fdf95-268">View hello WebJobs SDK dashboard</span></span>
1. <span data-ttu-id="fdf95-269">A hello [Azure-portálon](https://portal.azure.com/), jelölje be hello **alkalmazásszolgáltatások panel**, keresse meg a webes alkalmazást, és válassza ki **WebJobs**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-269">In hello [Azure portal](https://portal.azure.com/), select hello **App Services blade**, locate your web app, and select **WebJobs**.</span></span>
3. <span data-ttu-id="fdf95-270">Jelölje be hello **naplók** fülre.</span><span class="sxs-lookup"><span data-stu-id="fdf95-270">Select hello **Logs** tab.</span></span>

    ![Naplófájlok lap](./media/websites-dotnet-webjobs-sdk-get-started/log-tab.png)

    <span data-ttu-id="fdf95-272">Egy új böngészőlapon toohello WebJobs SDK irányítópult megnyitása.</span><span class="sxs-lookup"><span data-stu-id="fdf95-272">A new browser tab opens toohello WebJobs SDK dashboard.</span></span> <span data-ttu-id="fdf95-273">hello irányítópult adott webjobs-feladat fut, és a kód funkciók listáját láthatja, hogy hello indított WebJobs SDK hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-273">hello dashboard shows that hello WebJob is running and shows a list of functions in your code that hello WebJobs SDK triggered.</span></span>
4. <span data-ttu-id="fdf95-274">Kattintson az egyik hello funkciók toosee adatait a végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="fdf95-274">Click one of hello functions toosee details about its execution.</span></span>

    ![WebJobs SDK irányítópult](./media/websites-dotnet-webjobs-sdk-get-started/wjdashboardhome.png)

    ![WebJobs SDK irányítópult](./media/websites-dotnet-webjobs-sdk-get-started/wjfunctiondetails.png)

    <span data-ttu-id="fdf95-277">Hello **ismétlési függvény** ezen az oldalon gomb hatására a WebJobs SDK hello keretrendszer toocall hello függvény újra, és biztosít egy alkalommal toochange hello átadott adatok toohello függvény első.</span><span class="sxs-lookup"><span data-stu-id="fdf95-277">hello **Replay Function** button on this page causes hello WebJobs SDK framework toocall hello function again, and it gives you a chance toochange hello data passed toohello function first.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf95-278">Ha végzett tesztelése során, fontolja meg törlését hello webalkalmazás, tárfiókot és az SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-278">When you're finished testing, consider deleting hello web app, storage account, and your SQL Database instance.</span></span> <span data-ttu-id="fdf95-279">hello webalkalmazás szabad, de hello SQL tárfiók és adatbázispéldány keletkeznek költségek (ámbár, minimális toohello kis méret miatt).</span><span class="sxs-lookup"><span data-stu-id="fdf95-279">hello web app is free, but hello SQL storage account and database instance accrue charges (albeit, minimal due toohello small size).</span></span> <span data-ttu-id="fdf95-280">Is ha nem adja meg hello a webalkalmazás fut, bárki, aki rátalál az URL-cím létrehozhat és megtekinthet hirdetéseket.</span><span class="sxs-lookup"><span data-stu-id="fdf95-280">Also, if you leave hello web app running, anyone who finds your URL can create and view ads.</span></span> 
>
>

### <a name="delete-your-web-app"></a><span data-ttu-id="fdf95-281">A webalkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="fdf95-281">Delete your web app</span></span>
<span data-ttu-id="fdf95-282">Hello portálon lépjen toohello **alkalmazásszolgáltatások** panelen keresse meg és jelölje ki a webalkalmazás, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-282">In hello portal, go toohello **App Services** blade, locate and select your web app, and then click **Delete**.</span></span> <span data-ttu-id="fdf95-283">Ha csak tootemporarily megakadályozni, hogy mások hello webalkalmazás-hozzáférése, kattintson a **leállítása** helyette.</span><span class="sxs-lookup"><span data-stu-id="fdf95-283">If you just want tootemporarily prevent others from accessing hello web app, click **Stop** instead.</span></span> <span data-ttu-id="fdf95-284">Ebben az esetben díjakat továbbra is az SQL-adatbázis és a Tárfiók hello tooaccrue.</span><span class="sxs-lookup"><span data-stu-id="fdf95-284">In that case, charges will continue tooaccrue for hello SQL Database and Storage account.</span></span>

### <a name="delete-your-storage-account"></a><span data-ttu-id="fdf95-285">A tárfiók törlése</span><span class="sxs-lookup"><span data-stu-id="fdf95-285">Delete your storage account</span></span>
<span data-ttu-id="fdf95-286">toodelete a tárfiók, lásd: [tárfiók törlése](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="fdf95-286">toodelete your storage account, see [Delete a storage account](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account).</span></span> 

### <a name="delete-your-database"></a><span data-ttu-id="fdf95-287">Az adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="fdf95-287">Delete your database</span></span>
<span data-ttu-id="fdf95-288">toodelete az SQL-adatbázis, lásd: hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="fdf95-288">toodelete your SQL database, see hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) documentation.</span></span>

## <span data-ttu-id="fdf95-289"><a id="create"></a>Teljesen új hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdf95-289"><a id="create"></a>Create hello application from scratch</span></span>
<span data-ttu-id="fdf95-290">Ebben a szakaszban el kell végeznie hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="fdf95-290">In this section you'll do hello following tasks:</span></span>

* <span data-ttu-id="fdf95-291">Hozzon létre webes projektet a Visual Studio megoldás.</span><span class="sxs-lookup"><span data-stu-id="fdf95-291">Create a Visual Studio solution with a web project.</span></span>
* <span data-ttu-id="fdf95-292">Adja hozzá a hello adatok hordozhatóosztálytár-projektjének hello előtér- és a háttérrendszer között megosztott hozzáférési réteg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-292">Add a class library project for hello data access layer that is shared between hello front end and back end.</span></span>
* <span data-ttu-id="fdf95-293">Vegyen fel egy konzolalkalmazás projekt hello háttérkiszolgáló tartalmazó engedélyezve van a webjobs-feladatok telepítés.</span><span class="sxs-lookup"><span data-stu-id="fdf95-293">Add a Console Application project for hello backend, with WebJobs deployment enabled.</span></span>
* <span data-ttu-id="fdf95-294">Adja hozzá a NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="fdf95-294">Add NuGet packages.</span></span>
* <span data-ttu-id="fdf95-295">Állítsa be a projekt hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="fdf95-295">Set project references.</span></span>
* <span data-ttu-id="fdf95-296">Másolja át a kódot és konfigurációs fájljainak együttműködik hello oktatóanyag hello előző szakaszban letöltött hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="fdf95-296">Copy application code and configuration files from hello downloaded application that you worked with in hello previous section of hello tutorial.</span></span>
* <span data-ttu-id="fdf95-297">Tekintse át a hello részei hello kódot, amely együttműködik az Azure-blobokat és üzenetsorokat és WebJobs SDK hello.</span><span class="sxs-lookup"><span data-stu-id="fdf95-297">Review hello parts of hello code that work with Azure blobs and queues and hello WebJobs SDK.</span></span>

### <a name="create-a-visual-studio-solution-with-a-web-project-and-class-library-project"></a><span data-ttu-id="fdf95-298">A Visual Studio megoldás hordozhatóosztálytár-projektjének és webes projektet hoz létre</span><span class="sxs-lookup"><span data-stu-id="fdf95-298">Create a Visual Studio solution with a web project and class library project</span></span>
1. <span data-ttu-id="fdf95-299">A Visual Studio felületén válassza **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-299">In Visual Studio, choose **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="fdf95-300">A hello **új projekt** párbeszédpanelen válasszon **Visual C#** > **webes** > **ASP.NET Web Application (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-300">In hello **New Project** dialog, choose **Visual C#** > **Web** > **ASP.NET Web Application (.NET Framework)**.</span></span>
3. <span data-ttu-id="fdf95-301">Név: hello projekt ContosoAdsWeb, hello megoldás ContosoAdsWebJobsSDK name (hello megoldásnév módosítható, ha a hello kívánja menteni mappában, amelyben hello letöltött megoldás), és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-301">Name hello project ContosoAdsWeb, name hello solution ContosoAdsWebJobsSDK (change hello solution name if you're putting it in hello same folder as hello downloaded solution), and then click **OK**.</span></span>

    ![Új projekt](./media/websites-dotnet-webjobs-sdk-get-started/newproject.png)
4. <span data-ttu-id="fdf95-303">A hello **új ASP.NET-webalkalmazás** párbeszédpanelen válassza hello MVC-sablont, majd **hitelesítés módosítása**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-303">In hello **New ASP.NET Web Application** dialog, choose hello MVC template, and select **Change Authentication**.</span></span>

    ![Hitelesítés módosítása](./media/websites-dotnet-webjobs-sdk-get-started/chgauth.png)
5. <span data-ttu-id="fdf95-305">A hello **hitelesítés módosítása** párbeszédpanelen válasszon **nem hitelesítési**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-305">In hello **Change Authentication** dialog, choose **No Authentication**, and then click **OK**.</span></span>

    ![Nincs hitelesítés](./media/websites-dotnet-webjobs-sdk-get-started/noauth.png)
6. <span data-ttu-id="fdf95-307">A hello **új ASP.NET-webalkalmazás** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-307">In hello **New ASP.NET Web Application** dialog, click **OK**.</span></span>

    <span data-ttu-id="fdf95-308">A Visual Studio létrehozza a hello megoldás és hello webes projektet.</span><span class="sxs-lookup"><span data-stu-id="fdf95-308">Visual Studio creates hello solution and hello web project.</span></span>
7. <span data-ttu-id="fdf95-309">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello megoldás (nem hello projekt), és válassza a **Hozzáadás** > **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-309">In **Solution Explorer**, right-click hello solution (not hello project), and choose **Add** > **New Project**.</span></span>
8. <span data-ttu-id="fdf95-310">A hello **új projekt hozzáadása** párbeszédpanelen válasszon **Visual C#** > **klasszikus Windows asztal** > **Class Library (.NET Keretrendszer)** sablont.</span><span class="sxs-lookup"><span data-stu-id="fdf95-310">In hello **Add New Project** dialog, choose **Visual C#** > **Windows Classic Desktop** > **Class Library (.NET Framework)** template.</span></span>  
9. <span data-ttu-id="fdf95-311">Név hello projekt *ContosoAdsCommon*, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-311">Name hello project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="fdf95-312">A projektnek hello Entity Framework-környezetre és a hello adatmodell, amely mindkét hello előtér és háttér fogja használni.</span><span class="sxs-lookup"><span data-stu-id="fdf95-312">This project will contain hello Entity Framework context and hello data model which both hello front end and back end will use.</span></span> <span data-ttu-id="fdf95-313">Alternatív megoldásként sikerült hello webes projekt hello EF-hez kapcsolódó osztályokat definiálja, és hivatkozhat erre a projektre hello webjobs-feladat projektből.</span><span class="sxs-lookup"><span data-stu-id="fdf95-313">As an alternative, you could define hello EF-related classes in hello web project and reference that project from hello WebJob project.</span></span> <span data-ttu-id="fdf95-314">De a webjobs-feladat projekt majd kell egy tooweb referenciaszerelvények, amelyre nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="fdf95-314">But, then your WebJob project would have a reference tooweb assemblies, which it doesn't need.</span></span>

### <a name="add-a-console-application-project-that-has-webjobs-deployment-enabled"></a><span data-ttu-id="fdf95-315">Egy konzolalkalmazás-projektet, amelyen engedélyezve van a webjobs-feladatok telepítés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fdf95-315">Add a Console Application project that has WebJobs deployment enabled</span></span>
1. <span data-ttu-id="fdf95-316">Kattintson a jobb gombbal a hello webes projekt (nem hello megoldás vagy hello hordozhatóosztálytár-projektjének), és kattintson **Hozzáadás** > **új Azure webjobs-feladat projekt**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-316">Right-click hello web project (not hello solution or hello class library project), and then click **Add** > **New Azure WebJob Project**.</span></span>

    ![Új Azure webjobs-feladat Projekt menü kiválasztása](./media/websites-dotnet-webjobs-sdk-get-started/newawjp.png)
2. <span data-ttu-id="fdf95-318">A hello **adja hozzá a Azure webjobs-feladat** párbeszédpanelen adja meg mindkét hello ContosoAdsWebJob **projekt neve** és hello **webjobs-feladat neve**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-318">In hello **Add Azure WebJob** dialog, enter ContosoAdsWebJob as both hello **Project name** and hello **WebJob name**.</span></span> <span data-ttu-id="fdf95-319">Hagyja **webjobs-feladat futtatása mód** túl beállítása**futtatása folyamatosan**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-319">Leave **WebJob run mode** set too**Run Continuously**.</span></span>
3. <span data-ttu-id="fdf95-320">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf95-320">Click **OK**.</span></span>

   <span data-ttu-id="fdf95-321">Visual Studio létrehoz egy konzolalkalmazást, amely konfigurált toodeploy hello webes projekt telepítése, a webjobs-feladat van.</span><span class="sxs-lookup"><span data-stu-id="fdf95-321">Visual Studio creates a Console application that is configured toodeploy as a WebJob whenever you deploy hello web project.</span></span> <span data-ttu-id="fdf95-322">toodo, hogy végrehajtott feladatok hello projekt létrehozása után a következő hello:</span><span class="sxs-lookup"><span data-stu-id="fdf95-322">toodo that, it performed hello following tasks after creating hello project:</span></span>

   * <span data-ttu-id="fdf95-323">Hozzáadott egy *webjobs-feladat közzététele settings.json* hello webjobs-feladat projekt Tulajdonságok mappában található fájl.</span><span class="sxs-lookup"><span data-stu-id="fdf95-323">Added a *webjob-publish-settings.json* file in hello WebJob project Properties folder.</span></span>
   * <span data-ttu-id="fdf95-324">Hozzáadott egy *webjobs-list.json* hello webes projekt Tulajdonságok mappában található fájl.</span><span class="sxs-lookup"><span data-stu-id="fdf95-324">Added a *webjobs-list.json* file in hello web project Properties folder.</span></span>
   * <span data-ttu-id="fdf95-325">Hello Microsoft.Web.WebJobs.Publish NuGet-csomagja telepítve hello webjobs-feladat projektben.</span><span class="sxs-lookup"><span data-stu-id="fdf95-325">Installed hello Microsoft.Web.WebJobs.Publish NuGet package in hello WebJob project.</span></span>

   <span data-ttu-id="fdf95-326">Változásokkal kapcsolatos további információkért lásd: [hogyan toodeploy webjobs-feladatok Visual Studio használatával](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="fdf95-326">For more information about these changes, see [How toodeploy WebJobs by using Visual Studio](websites-dotnet-deploy-webjobs.md).</span></span>

### <a name="add-nuget-packages"></a><span data-ttu-id="fdf95-327">Adja hozzá a NuGet-csomagok</span><span class="sxs-lookup"><span data-stu-id="fdf95-327">Add NuGet packages</span></span>
<span data-ttu-id="fdf95-328">Új projekt sablon hello webjobs-feladat projekt automatikusan telepíti a hello WebJobs SDK NuGet-csomag [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="fdf95-328">hello new-project template for a WebJob project automatically installs hello WebJobs SDK NuGet package [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) and its dependencies.</span></span>

<span data-ttu-id="fdf95-329">Hello webjobs-feladat projekt automatikusan telepített WebJobs SDK-függőség van hello egyik hello Azure Storage ügyfél könyvtár (SCL).</span><span class="sxs-lookup"><span data-stu-id="fdf95-329">One of hello WebJobs SDK dependencies that is installed automatically in hello WebJob project is hello Azure Storage Client Library (SCL).</span></span> <span data-ttu-id="fdf95-330">Azonban meg kell tooadd azt toohello webes projekt toowork a blobokat és üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-330">However, you need tooadd it toohello web project toowork with blobs and queues.</span></span>

1. <span data-ttu-id="fdf95-331">Nyissa meg hello **NuGet-csomagok kezelése** hello megoldás párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fdf95-331">Open hello **Manage NuGet Packages** dialog for hello solution.</span></span>
2. <span data-ttu-id="fdf95-332">Hello bal oldali ablaktáblában jelöljön ki **telepített csomag**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-332">In hello left pane, select **Installed packages**.</span></span>
3. <span data-ttu-id="fdf95-333">Hello található *Azure Storage* csomagot, majd kattintson a **kezelése**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-333">Find hello *Azure Storage* package, and then click **Manage**.</span></span>
4. <span data-ttu-id="fdf95-334">A hello **projekt kijelölése** jelölését, jelölje be hello **ContosoAdsWeb** jelölőnégyzetet, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-334">In hello **Select Projects** box, select hello **ContosoAdsWeb** check box, and then click **OK**.</span></span>

    <span data-ttu-id="fdf95-335">Mind a három projekt hello Entity Framework toowork használja az SQL-adatbázis adatokkal.</span><span class="sxs-lookup"><span data-stu-id="fdf95-335">All three projects use hello Entity Framework toowork with data in SQL Database.</span></span>
5. <span data-ttu-id="fdf95-336">Hello bal oldali ablaktáblában jelöljön ki **Online**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-336">In hello left pane, select **Online**.</span></span>
6. <span data-ttu-id="fdf95-337">Hello található *EntityFramework* NuGet csomagot, majd telepítse mind a három projektben.</span><span class="sxs-lookup"><span data-stu-id="fdf95-337">Find hello *EntityFramework* NuGet package, and install it in all three projects.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="fdf95-338">A projekt hivatkozásainak beállítása</span><span class="sxs-lookup"><span data-stu-id="fdf95-338">Set project references</span></span>
<span data-ttu-id="fdf95-339">Az internetes és a webjobs-feladat projektek együttműködve hello SQL-adatbázis, így mindkettő kell egy hivatkozás toohello ContosoAdsCommon projektre.</span><span class="sxs-lookup"><span data-stu-id="fdf95-339">Both web and WebJob projects work with hello SQL database, so both need a reference toohello ContosoAdsCommon project.</span></span>

1. <span data-ttu-id="fdf95-340">Hello ContosoAdsWeb projektben állítson be egy hivatkozást toohello ContosoAdsCommon projektre.</span><span class="sxs-lookup"><span data-stu-id="fdf95-340">In hello ContosoAdsWeb project, set a reference toohello ContosoAdsCommon project.</span></span> <span data-ttu-id="fdf95-341">(Kattintson a jobb gombbal a ContosoAdsWeb projektben hello, és kattintson **Hozzáadás** > **hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-341">(Right-click hello ContosoAdsWeb project, and then click **Add** > **Reference**.</span></span> 
2. <span data-ttu-id="fdf95-342">A hello **hivatkozáskezelő** párbeszédpanelen jelölje ki **projektek** > **megoldás** > **ContosoAdsCommon**, és kattintson a **OK**.)</span><span class="sxs-lookup"><span data-stu-id="fdf95-342">In hello **Reference Manager** dialog box, select **Projects** > **Solution** > **ContosoAdsCommon**, and then click **OK**.)</span></span>
   
    <span data-ttu-id="fdf95-343">hello webjobs-feladat projekt hivatkozásainak van szüksége, lemezképek használata és a kapcsolati karakterláncok használata.</span><span class="sxs-lookup"><span data-stu-id="fdf95-343">hello WebJob project needs references for working with images and for accessing connection strings.</span></span>

4. <span data-ttu-id="fdf95-344">Hello ContosoAdsWebJob projektben állítson be egy hivatkozást túl`System.Drawing` és `System.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="fdf95-344">In hello ContosoAdsWebJob project, set a reference too`System.Drawing` and `System.Configuration`.</span></span>

### <a name="add-code-and-configuration-files"></a><span data-ttu-id="fdf95-345">Adja hozzá a kódot és konfigurációs fájlok</span><span class="sxs-lookup"><span data-stu-id="fdf95-345">Add code and configuration files</span></span>
<span data-ttu-id="fdf95-346">Ez az oktatóanyag nem szerepelnek azok hogyan túl[hozzon létre az MVC-vezérlők és nézetek használatával állványok](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), hogyan túl[Entity Framework-kód, amely az SQL Server-adatbázisok írási](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), vagy [hello alapjait aszinkron programozás az ASP.NET 4.5 a](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="fdf95-346">This tutorial does not show how too[create MVC controllers and views using scaffolding](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), how too[write Entity Framework code that works with SQL Server databases](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), or [hello basics of asynchronous programming in ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span> <span data-ttu-id="fdf95-347">Ezért, az marad toodo hello letöltött megoldásból hello új megoldásba történő kód és a konfigurációs fájlok másolása.</span><span class="sxs-lookup"><span data-stu-id="fdf95-347">So, all that remains toodo is copy code and configuration files from hello downloaded solution into hello new solution.</span></span> <span data-ttu-id="fdf95-348">Miután ezt megtenné, hello következő szakaszok bemutatják és ismertetik hello kód legfontosabb részeit.</span><span class="sxs-lookup"><span data-stu-id="fdf95-348">After you do that, hello following sections show and explain key parts of hello code.</span></span>

<span data-ttu-id="fdf95-349">tooadd fájlok tooa projekt vagy egy mappát, kattintson a jobb gombbal hello projekt vagy mappa és kattintson **Hozzáadás** > **meglévő cikk**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-349">tooadd files tooa project or a folder, right-click hello project or folder and click **Add** > **Existing Item**.</span></span> <span data-ttu-id="fdf95-350">Jelölje be hello fájlokat kívánja, majd kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-350">Select hello files you want and click **Add**.</span></span> <span data-ttu-id="fdf95-351">Ha a rendszer rákérdez, hogy kívánja-e tooreplace meglévő fájlokat, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="fdf95-351">If asked whether you want tooreplace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="fdf95-352">Hello ContosoAdsCommon projektben törölje a hello *Class1.cs* fájlt, és a hely hello következő fájlok hello letöltött projektből.</span><span class="sxs-lookup"><span data-stu-id="fdf95-352">In hello ContosoAdsCommon project, delete hello *Class1.cs* file and add in its place hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="fdf95-353">*Ad.cs*</span><span class="sxs-lookup"><span data-stu-id="fdf95-353">*Ad.cs*</span></span>
   * <span data-ttu-id="fdf95-354">*ContosoAdscontext.cs*</span><span class="sxs-lookup"><span data-stu-id="fdf95-354">*ContosoAdscontext.cs*</span></span>
   * <span data-ttu-id="fdf95-355">*BlobInformation.cs*</span><span class="sxs-lookup"><span data-stu-id="fdf95-355">*BlobInformation.cs*</span></span>
2. <span data-ttu-id="fdf95-356">Hello ContosoAdsWeb projektben adja hozzá a következő fájlok hello letöltött projektből hello.</span><span class="sxs-lookup"><span data-stu-id="fdf95-356">In hello ContosoAdsWeb project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="fdf95-357">*Web.config*</span><span class="sxs-lookup"><span data-stu-id="fdf95-357">*Web.config*</span></span>
   * <span data-ttu-id="fdf95-358">*Global.asax.cs*</span><span class="sxs-lookup"><span data-stu-id="fdf95-358">*Global.asax.cs*</span></span>  
   * <span data-ttu-id="fdf95-359">A hello *tartományvezérlők* mappa: *AdController.cs*</span><span class="sxs-lookup"><span data-stu-id="fdf95-359">In hello *Controllers* folder: *AdController.cs*</span></span>
   * <span data-ttu-id="fdf95-360">A hello *Views\Shared* mappa: *_Layout.cshtml* fájl</span><span class="sxs-lookup"><span data-stu-id="fdf95-360">In hello *Views\Shared* folder: *_Layout.cshtml* file</span></span>
   * <span data-ttu-id="fdf95-361">A hello *Views\Home* mappa: *Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fdf95-361">In hello *Views\Home* folder: *Index.cshtml*</span></span>
   * <span data-ttu-id="fdf95-362">A hello *Views\Ad* mappában (először hozza létre hello mappa): öt *.cshtml* fájlok</span><span class="sxs-lookup"><span data-stu-id="fdf95-362">In hello *Views\Ad* folder (create hello folder first): five *.cshtml* files</span></span>
3. <span data-ttu-id="fdf95-363">Hello ContosoAdsWebJob projektben adja hozzá a következő fájlok hello letöltött projektből hello.</span><span class="sxs-lookup"><span data-stu-id="fdf95-363">In hello ContosoAdsWebJob project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="fdf95-364">*App.config* (hello fájl szűrő túl módosítása**minden fájl**)</span><span class="sxs-lookup"><span data-stu-id="fdf95-364">*App.config* (change hello file type filter too**All Files**)</span></span>
   * <span data-ttu-id="fdf95-365">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="fdf95-365">*Program.cs*</span></span>
   * <span data-ttu-id="fdf95-366">*Functions.cs*</span><span class="sxs-lookup"><span data-stu-id="fdf95-366">*Functions.cs*</span></span>

<span data-ttu-id="fdf95-367">Most létre, futtatása és hello az oktatóanyag korábbi utasításai szerint hello alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="fdf95-367">You can now build, run, and deploy hello application as instructed earlier in hello tutorial.</span></span> <span data-ttu-id="fdf95-368">Mielőtt ezt megtenné, azonban leállítása hello webjobs-feladat, amely hello első webalkalmazás telepített továbbra is futtatja.</span><span class="sxs-lookup"><span data-stu-id="fdf95-368">Before you do that, however, stop hello WebJob that is still running in hello first web app you deployed to.</span></span> <span data-ttu-id="fdf95-369">Ellenkező esetben a webjobs-feladat fog helyileg létrehozott üzenetek feldolgozásához vagy hello alkalmazás egy új webalkalmazást futtatja, mivel minden használ hello azonos a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="fdf95-369">Otherwise, that WebJob will process queue messages created locally or by hello app running in a new web app, since all are using hello same storage account.</span></span>

## <span data-ttu-id="fdf95-370"><a id="code"></a>Tekintse át a hello alkalmazás kódja</span><span class="sxs-lookup"><span data-stu-id="fdf95-370"><a id="code"></a>Review hello application code</span></span>
<span data-ttu-id="fdf95-371">hello alábbi szakaszok ismertetik a kód hello kapcsolódó tooworking a hello WebJobs SDK és Azure Storage blobokat és üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-371">hello following sections explain hello code related tooworking with hello WebJobs SDK and Azure Storage blobs and queues.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf95-372">Hello az adott toohello kód WebJobs SDK, nyissa meg a toohello [Program.cs és Functions.cs](#programcs) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="fdf95-372">For hello code specific toohello WebJobs SDK, go toohello [Program.cs and Functions.cs](#programcs) sections.</span></span>
>
>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="fdf95-373">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="fdf95-373">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="fdf95-374">hello Ad.cs fájl megad, egy felsorolást a kategóriákhoz és egy POCO entitásosztályt a hirdetés információihoz.</span><span class="sxs-lookup"><span data-stu-id="fdf95-374">hello Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

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

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="fdf95-375">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="fdf95-375">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="fdf95-376">hello ContosoAdsContext osztály megadja szolgál, hogy hello Ad osztály egy DbSet gyűjteményben, amely Entity Framework egy SQL-adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="fdf95-376">hello ContosoAdsContext class specifies that hello Ad class is used in a DbSet collection, which Entity Framework stores in a SQL database.</span></span>

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

<span data-ttu-id="fdf95-377">hello osztály két konstruktorral rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fdf95-377">hello class has two constructors.</span></span> <span data-ttu-id="fdf95-378">hello először hello webes projekt használja, és annak hello hello Web.config fájl vagy hello Azure futtatókörnyezetben tárolt kapcsolati karakterlánc nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-378">hello first is used by hello web project, and specifies hello name of a connection string that is stored in hello Web.config file or hello Azure runtime environment.</span></span> <span data-ttu-id="fdf95-379">hello második konstruktor lehetővé teszi toopass hello tényleges kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="fdf95-379">hello second constructor enables you toopass in hello actual connection string.</span></span> <span data-ttu-id="fdf95-380">Mivel nem rendelkezik a Web.config fájl, amely van szükség hello webjobs-feladat a projekten.</span><span class="sxs-lookup"><span data-stu-id="fdf95-380">That is needed by hello WebJob project since it doesn't have a Web.config file.</span></span> <span data-ttu-id="fdf95-381">A korábban Ha a kapcsolati karakterlánc tárolásának, és láthatja, hogyan később hello kód keresi hello kapcsolati karakterlánc elindítja hello DbContext osztályt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-381">You saw earlier where this connection string was stored, and you'll see later how hello code retrieves hello connection string when it instantiates hello DbContext class.</span></span>

### <a name="contosoadscommon---blobinformationcs"></a><span data-ttu-id="fdf95-382">ContosoAdsCommon – BlobInformation.cs</span><span class="sxs-lookup"><span data-stu-id="fdf95-382">ContosoAdsCommon - BlobInformation.cs</span></span>
<span data-ttu-id="fdf95-383">Hello `BlobInformation` osztálya: egy kép blob várólista üzenetben használt toostore információt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-383">hello `BlobInformation` class is used toostore information about an image blob in a queue message.</span></span>

        public class BlobInformation
        {
            public Uri BlobUri { get; set; }

            public string BlobName
            {
                get
                {
                    return BlobUri.Segments[BlobUri.Segments.Length - 1];
                }
            }
            public string BlobNameWithoutExtension
            {
                get
                {
                    return Path.GetFileNameWithoutExtension(BlobName);
                }
            }
            public int AdId { get; set; }
        }


### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="fdf95-384">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="fdf95-384">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="fdf95-385">Hello meghívott kód `Application_Start` hoz létre egy *képek* blobtárolót és egy *képek* üzenetsort, amennyiben még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="fdf95-385">Code that is called from hello `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="fdf95-386">Ez biztosítja, hogy valahányszor új tárfiókot használatba hello szükséges blobtároló és üzenetsor automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="fdf95-386">This ensures that whenever you start using a new storage account, hello required blob container and queue are created automatically.</span></span>

<span data-ttu-id="fdf95-387">kód lekérdezi hozzáférés toohello tárfiók hello származó hello hello tárolási kapcsolati karakterlánc használatával *Web.config* fájl- vagy Azure futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fdf95-387">hello code gets access toohello storage account by using hello storage connection string from hello *Web.config* file or Azure runtime environment.</span></span>

        var storageAccount = CloudStorageAccount.Parse
            (ConfigurationManager.ConnectionStrings["AzureWebJobsStorage"].ToString());

<span data-ttu-id="fdf95-388">Ezt követően onnan kapta, hogy a hivatkozás toohello *képek* blobtárolóhoz, létrehozza a hello tárolót, ha még nem létezik, és beállítja a hozzáférési engedélyeket hello új tárolóra.</span><span class="sxs-lookup"><span data-stu-id="fdf95-388">Then, it gets a reference toohello *images* blob container, creates hello container if it doesn't already exist, and sets access permissions on hello new container.</span></span> <span data-ttu-id="fdf95-389">Alapértelmezés szerint az új tárolók csak a tárolási fiók hitelesítő adatait tooaccess BLOB ügyfelek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fdf95-389">By default, new containers allow only clients with storage account credentials tooaccess blobs.</span></span> <span data-ttu-id="fdf95-390">hello webalkalmazás kell hello toobe nyilvános blobok, úgy, hogy az URL-címekkel, hogy pont toohello kép blobok képeket jeleníthessen meg.</span><span class="sxs-lookup"><span data-stu-id="fdf95-390">hello web app needs hello blobs toobe public so that it can display images using URLs that point toohello image blobs.</span></span>

        var blobClient = storageAccount.CreateCloudBlobClient();
        var imagesBlobContainer = blobClient.GetContainerReference("images");
        if (imagesBlobContainer.CreateIfNotExists())
        {
            imagesBlobContainer.SetPermissions(
                new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                });
        }

<span data-ttu-id="fdf95-391">Hasonló kód jogosultságot kap a hivatkozás toohello *thumbnailrequest* várólistára, és létrehoz egy új üzenetsort.</span><span class="sxs-lookup"><span data-stu-id="fdf95-391">Similar code gets a reference toohello *thumbnailrequest* queue and creates a new queue.</span></span> <span data-ttu-id="fdf95-392">Ebben az esetben nincs szükség az engedélyek módosítására.</span><span class="sxs-lookup"><span data-stu-id="fdf95-392">In this case no permissions change is needed.</span></span>

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("thumbnailrequest");
        imagesQueue.CreateIfNotExists();

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="fdf95-393">ContosoAdsWeb – _Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="fdf95-393">ContosoAdsWeb - _Layout.cshtml</span></span>
<span data-ttu-id="fdf95-394">Hello *_Layout.cshtml* fájl hello alkalmazásnév beállítja az hello fejlécében és láblécében, és létrehoz egy "Ads" menübejegyzést.</span><span class="sxs-lookup"><span data-stu-id="fdf95-394">hello *_Layout.cshtml* file sets hello app name in hello header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="fdf95-395">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fdf95-395">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="fdf95-396">Hello *Views\Home\Index.cshtml* fájl kategóriahivatkozásokat jelenít meg hello kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="fdf95-396">hello *Views\Home\Index.cshtml* file displays category links on hello home page.</span></span> <span data-ttu-id="fdf95-397">hello hivatkozások átadják hello egész értéket hello `Category` felsorolás egy lekérdezési karakterlánc változó toohello Ads indexlapnak a.</span><span class="sxs-lookup"><span data-stu-id="fdf95-397">hello links pass hello integer value of hello `Category` enum in a querystring variable toohello Ads Index page.</span></span>

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="fdf95-398">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="fdf95-398">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="fdf95-399">A hello *AdController.cs* fájl, hello konstruktor hívások hello `InitializeStorage` metódus toocreate Azure Storage ügyféloldali kódtár objektumok, amelyek az API-k használata a blobokat és üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-399">In hello *AdController.cs* file, hello constructor calls hello `InitializeStorage` method toocreate Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="fdf95-400">Ezt követően hello kód kap-e a hivatkozási toohello *képek* blobtárolóra, ahogy azt korábban a *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="fdf95-400">Then, hello code gets a reference toohello *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="fdf95-401">Hogy Mindeközben beállít egy alapértelmezett [újrapróbálkozási házirendet](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) megfelelő egy webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdf95-401">While doing that, it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="fdf95-402">hello alapértelmezett exponenciális leállítási újrapróbálkozási házirend le több mint egy perc egy átmeneti hiba miatti ismételt próbálkozás hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdf95-402">hello default exponential backoff retry policy could hang hello web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="fdf95-403">Itt megadott hello újrapróbálkozási házirend minden próbálkozás mentése toothree próbálkozás után három másodperc alatt vár.</span><span class="sxs-lookup"><span data-stu-id="fdf95-403">hello retry policy specified here waits three seconds after each try for up toothree tries.</span></span>

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

<span data-ttu-id="fdf95-404">Hasonló kód jogosultságot kap a hivatkozás toohello *képek* várólista.</span><span class="sxs-lookup"><span data-stu-id="fdf95-404">Similar code gets a reference toohello *images* queue.</span></span>

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("blobnamerequest");

<span data-ttu-id="fdf95-405">A legtöbb hello vezérlőkód jellemzően egy DbContext osztályt használó Entity Framework-adatmodell.</span><span class="sxs-lookup"><span data-stu-id="fdf95-405">Most of hello controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="fdf95-406">Egy kivétel: hello HttpPost `Create` metódus, amely feltölt egy fájlt, és menti a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fdf95-406">An exception is hello HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="fdf95-407">hello biztosít egy [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) toohello metódus objektum.</span><span class="sxs-lookup"><span data-stu-id="fdf95-407">hello model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object toohello method.</span></span>

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

<span data-ttu-id="fdf95-408">Egy fájl tooupload hello felhasználói választásakor hello kód feltölt hello fájlt, egy blobba menti, és frissíti a hirdetés adatbázisrekordot hello toohello blob URL-CÍMMEL.</span><span class="sxs-lookup"><span data-stu-id="fdf95-408">If hello user selected a file tooupload, hello code uploads hello file, saves it in a blob, and updates hello Ad database record with a URL that points toohello blob.</span></span>

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

<span data-ttu-id="fdf95-409">hello kódot, amely hello feltöltés van hello `UploadAndSaveBlobAsync` metódust.</span><span class="sxs-lookup"><span data-stu-id="fdf95-409">hello code that does hello upload is in hello `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="fdf95-410">Létrehoz egy GUID nevet hello blob, a feltöltéseket és az eredményobjektumokat hello fájl, és egy hivatkozási mentve toohello blob adja vissza.</span><span class="sxs-lookup"><span data-stu-id="fdf95-410">It creates a GUID name for hello blob, uploads and saves hello file, and returns a reference toohello saved blob.</span></span>

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

<span data-ttu-id="fdf95-411">Miután a hello HttpPost `Create` metódus feltölt egy blobot és frissítések hello adatbázist, létrehoz egy várólista üzenet tooinform hello háttérfolyamatot, hogy a lemezkép átalakítás tooa miniatűr készen áll-e.</span><span class="sxs-lookup"><span data-stu-id="fdf95-411">After hello HttpPost `Create` method uploads a blob and updates hello database, it creates a queue message tooinform hello back-end process that an image is ready for conversion tooa thumbnail.</span></span>

        BlobInformation blobInfo = new BlobInformation() { AdId = ad.AdId, BlobUri = new Uri(ad.ImageURL) };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        await thumbnailRequestQueue.AddMessageAsync(queueMessage);

<span data-ttu-id="fdf95-412">hello HttpPost kódját hello `Edit` metódus hasonló, azzal a különbséggel, hogy hello felhasználó kiválaszt egy új képfájlt, ha bármely a hirdetés létező blobot törölni kell.</span><span class="sxs-lookup"><span data-stu-id="fdf95-412">hello code for hello HttpPost `Edit` method is similar, except that if hello user selects a new image file, any blobs that already exist for this ad must be deleted.</span></span>

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

<span data-ttu-id="fdf95-413">Itt található blobok törlése, ha töröl egy ad hello kódot:</span><span class="sxs-lookup"><span data-stu-id="fdf95-413">Here is hello code that deletes blobs when you delete an ad:</span></span>

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

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="fdf95-414">ContosoAdsWeb – Views\Ad\Index.cshtml és Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="fdf95-414">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="fdf95-415">Hello *Index.cshtml* fájlt tartalmazó miniatűrt jelenít meg hello hirdetés többi adatát:</span><span class="sxs-lookup"><span data-stu-id="fdf95-415">hello *Index.cshtml* file displays thumbnails with hello other ad data:</span></span>

        <img  src="@Html.Raw(item.ThumbnailURL)" />

<span data-ttu-id="fdf95-416">Hello *Details.cshtml* fájl hello teljes méretű képet jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="fdf95-416">hello *Details.cshtml* file displays hello full-size image:</span></span>

        <img src="@Html.Raw(Model.ImageURL)" />

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="fdf95-417">ContosoAdsWeb – Views\Ad\Create.cshtml és Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="fdf95-417">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="fdf95-418">Hello *Create.cshtml* és *Edit.cshtml* fájlok megadják az űrlap kódolását, hogy a lehetővé teszi, hogy a tartományvezérlő tooget hello hello `HttpPostedFileBase` objektum.</span><span class="sxs-lookup"><span data-stu-id="fdf95-418">hello *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables hello controller tooget hello `HttpPostedFileBase` object.</span></span>

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

<span data-ttu-id="fdf95-419">Egy `<input>` elem jelzi hello böngésző tooprovide egy Fájlkiválasztási párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-419">An `<input>` element tells hello browser tooprovide a file selection dialog.</span></span>

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### <span data-ttu-id="fdf95-420"><a id="programcs"></a>ContosoAdsWebJob - Program.cs</span><span class="sxs-lookup"><span data-stu-id="fdf95-420"><a id="programcs"></a>ContosoAdsWebJob - Program.cs</span></span>
<span data-ttu-id="fdf95-421">Ha hello webjobs-feladat indítása hello `Main` metódushívások hello WebJobs SDK `JobHost.RunAndBlock` metódus toobegin végrehajtása hello aktuális szálon kiváltott funkciók.</span><span class="sxs-lookup"><span data-stu-id="fdf95-421">When hello WebJob starts, hello `Main` method calls hello WebJobs SDK `JobHost.RunAndBlock` method toobegin execution of triggered functions on hello current thread.</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

### <span data-ttu-id="fdf95-422"><a id="generatethumbnail"></a>ContosoAdsWebJob - Functions.cs - GenerateThumbnail módszer</span><span class="sxs-lookup"><span data-stu-id="fdf95-422"><a id="generatethumbnail"></a>ContosoAdsWebJob - Functions.cs - GenerateThumbnail method</span></span>
<span data-ttu-id="fdf95-423">hello WebJobs SDK a metódus meghívja a várólista-üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="fdf95-423">hello WebJobs SDK calls this method when a queue message is received.</span></span> <span data-ttu-id="fdf95-424">hello módszer miniatűr hoz létre, és visszahelyezi a miniatűr hello hello adatbázis URL-címet.</span><span class="sxs-lookup"><span data-stu-id="fdf95-424">hello method creates a thumbnail and puts hello thumbnail URL in hello database.</span></span>

        public static void GenerateThumbnail(
        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,
        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)
        {
            using (Stream output = outputBlob.OpenWrite())
            {
                ConvertImageToThumbnailJPG(input, output);
                outputBlob.Properties.ContentType = "image/jpeg";
            }

            // Entity Framework context class is not thread-safe, so it must
            // be instantiated and disposed within hello function.
            using (ContosoAdsContext db = new ContosoAdsContext())
            {
                var id = blobInfo.AdId;
                Ad ad = db.Ads.Find(id);
                if (ad == null)
                {
                    throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", id.ToString()));
                }
                ad.ThumbnailURL = outputBlob.Uri.ToString();
                db.SaveChanges();
            }
        }

* <span data-ttu-id="fdf95-425">Hello `QueueTrigger` attribútum arra utasítja a WebJobs SDK toocall hello ezt a módszert egy új üzenet hello thumbnailrequest várólista fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="fdf95-425">hello `QueueTrigger` attribute directs hello WebJobs SDK toocall this method when a new message is received on hello thumbnailrequest queue.</span></span>

        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,

    <span data-ttu-id="fdf95-426">Hello `BlobInformation` üdvözlőüzenetére várólista célja a hello automatikusan deszerializált `blobInfo` paraméter.</span><span class="sxs-lookup"><span data-stu-id="fdf95-426">hello `BlobInformation` object in hello queue message is automatically deserialized into hello `blobInfo` parameter.</span></span> <span data-ttu-id="fdf95-427">Hello metódus befejezésekor hello várólista üzenet törlődik.</span><span class="sxs-lookup"><span data-stu-id="fdf95-427">When hello method completes, hello queue message is deleted.</span></span> <span data-ttu-id="fdf95-428">Ha hello módszer nem jár sikerrel, befejezése előtt, a rendszer nem törli a várólista üdvözlőüzenetére; a 10 perces címbérlet lejárta után hello üzenet kiadott toobe újra felvételre és feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="fdf95-428">If hello method fails before completing, hello queue message is not deleted; after a 10-minute lease expires, hello message is released toobe picked up again and processed.</span></span> <span data-ttu-id="fdf95-429">Ebben a sorozatban a nem határozatlan ideig ismétlődik, ha üzenet mindig kivételt okoz.</span><span class="sxs-lookup"><span data-stu-id="fdf95-429">This sequence won't be repeated indefinitely if a message always causes an exception.</span></span> <span data-ttu-id="fdf95-430">5 sikertelen kísérlet tooprocess egy üzenetet, után üdvözlőüzenetére-e az áthelyezett tooa várólista nevű {queuename}-elhalt.</span><span class="sxs-lookup"><span data-stu-id="fdf95-430">After 5 unsuccessful attempts tooprocess a message, hello message is moved tooa queue named {queuename}-poison.</span></span> <span data-ttu-id="fdf95-431">kísérletek maximális száma hello lehet konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="fdf95-431">hello maximum number of attempts is configurable.</span></span>
* <span data-ttu-id="fdf95-432">két hello `Blob` attribútumok adja meg az objektumok kötött tooblobs közül: egy toohello meglévő lemezkép-blob és egy tooa új miniatűr blob, amely hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fdf95-432">hello two `Blob` attributes provide objects that are bound tooblobs: one toohello existing image blob and one tooa new thumbnail blob that hello method creates.</span></span>

        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)

    <span data-ttu-id="fdf95-433">BLOB nevének származhat hello tulajdonságainak `BlobInformation` hello üzenetsorban található üzenetet kapott objektum (`BlobName` és `BlobNameWithoutExtension`).</span><span class="sxs-lookup"><span data-stu-id="fdf95-433">Blob names come from properties of hello `BlobInformation` object received in hello queue message (`BlobName` and `BlobNameWithoutExtension`).</span></span> <span data-ttu-id="fdf95-434">tooget hello működése érdekében a Storage ügyféloldali kódtára hello, hello használható `CloudBlockBlob` osztály toowork a BLOB.</span><span class="sxs-lookup"><span data-stu-id="fdf95-434">tooget hello full functionality of hello Storage Client Library, you can use hello `CloudBlockBlob` class toowork with blobs.</span></span> <span data-ttu-id="fdf95-435">Ha azt szeretné, a toowork írt kódot tooreuse `Stream` objektumok, használhatja a hello `Stream` osztály.</span><span class="sxs-lookup"><span data-stu-id="fdf95-435">If you want tooreuse code that was written toowork with `Stream` objects, you can use hello `Stream` class.</span></span>

<span data-ttu-id="fdf95-436">További információ a toowrite működését, amelyek WebJobs SDK attribútumok használata, tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="fdf95-436">For more information about how toowrite functions that use  WebJobs SDK attributes, see hello following resources:</span></span>

* [<span data-ttu-id="fdf95-437">Hogyan toouse Azure várólista hello WebJobs SDK szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="fdf95-437">How toouse Azure queue storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [<span data-ttu-id="fdf95-438">Hogyan toouse Azure blob-tároló hello WebJobs SDK a</span><span class="sxs-lookup"><span data-stu-id="fdf95-438">How toouse Azure blob storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [<span data-ttu-id="fdf95-439">Hogyan toouse Azure table storage a hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="fdf95-439">How toouse Azure table storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [<span data-ttu-id="fdf95-440">Hogyan toouse Azure Service Bus a hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="fdf95-440">How toouse Azure Service Bus with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-service-bus.md)

> [!NOTE]
> * <span data-ttu-id="fdf95-441">A webalkalmazás több virtuális gép fut, ha több WebJobs egyidejűleg fog futni, és egyes esetekben ez eredményezhet hello azonos adatfeldolgozási első többször.</span><span class="sxs-lookup"><span data-stu-id="fdf95-441">If your web app runs on multiple VMs, multiple WebJobs will be running simultaneously, and in some scenarios this can result in hello same data getting processed multiple times.</span></span> <span data-ttu-id="fdf95-442">Ez nem probléma hello beépített várólista, a blob és a Service Bus-eseményindítók használatakor.</span><span class="sxs-lookup"><span data-stu-id="fdf95-442">This is not a problem if you use hello built-in queue, blob, and Service Bus triggers.</span></span> <span data-ttu-id="fdf95-443">hello SDK biztosítja, hogy a funkciók dolgoz fel csak egyszer minden üzenet vagy a blob.</span><span class="sxs-lookup"><span data-stu-id="fdf95-443">hello SDK ensures that your functions will be processed only once for each message or blob.</span></span>
> * <span data-ttu-id="fdf95-444">További információ tooimplement szabályos leállítást, lásd: [szabályos leállítást](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).</span><span class="sxs-lookup"><span data-stu-id="fdf95-444">For information about how tooimplement graceful shutdown, see [Graceful Shutdown](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).</span></span>
> * <span data-ttu-id="fdf95-445">hello kód hello `ConvertImageToThumbnailJPG` (nincs ábrázolva) metódus osztályokat alkalmaz hello `System.Drawing` névtér az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="fdf95-445">hello code in hello `ConvertImageToThumbnailJPG` method (not shown) uses classes in hello `System.Drawing` namespace for simplicity.</span></span> <span data-ttu-id="fdf95-446">A névtérben lévő osztályok hello azonban a Windows-űrlapokkal való használatra lettek tervezve.</span><span class="sxs-lookup"><span data-stu-id="fdf95-446">However, hello classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="fdf95-447">Windows- vagy ASP.NET-szolgáltatásban való használatuk nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="fdf95-447">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="fdf95-448">További információk a képfeldolgozási beállításokról: [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) (Dinamikus képek létrehozása) és [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na) (A képek átméretezésének részletei).</span><span class="sxs-lookup"><span data-stu-id="fdf95-448">For more information about image processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="fdf95-449">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdf95-449">Next steps</span></span>
<span data-ttu-id="fdf95-450">Ebben az oktatóanyagban megtudhatta, egy egyszerű, a háttérbeli feldolgozás hello WebJobs SDK használó többrétegű alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fdf95-450">In this tutorial, you've seen a simple multi-tier application that uses hello WebJobs SDK for backend processing.</span></span> <span data-ttu-id="fdf95-451">Ez a szakasz néhány ASP.NET Többrétegű alkalmazások és a webjobs-feladatok többet javaslatokat kínál.</span><span class="sxs-lookup"><span data-stu-id="fdf95-451">This section offers some suggestions for learning more about ASP.NET multi-tier applications and WebJobs.</span></span>

### <a name="missing-features"></a><span data-ttu-id="fdf95-452">Hiányzó funkciók</span><span class="sxs-lookup"><span data-stu-id="fdf95-452">Missing features</span></span>
<span data-ttu-id="fdf95-453">hello alkalmazás-bevezető oktatóanyag az egyszerű lett megtartva.</span><span class="sxs-lookup"><span data-stu-id="fdf95-453">hello application has been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="fdf95-454">Egy valós alkalmazás megvalósítása volna [függőségi beszúrást](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) és hello [adattárát és egységét működik minták](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), használja [egy naplózási felület](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), használata[ EF Code First áttelepítést](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage adatok módosításait a modell, és használja [EF-kapcsolati rugalmasságot](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage átmeneti hálózati hibák.</span><span class="sxs-lookup"><span data-stu-id="fdf95-454">In a real-world application you would implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) and hello [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), use [an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data model changes, and use [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage transient network errors.</span></span>

### <a name="scaling-webjobs"></a><span data-ttu-id="fdf95-455">Webjobs-feladatok méretezése</span><span class="sxs-lookup"><span data-stu-id="fdf95-455">Scaling WebJobs</span></span>
<span data-ttu-id="fdf95-456">Webjobs-feladatok egy webalkalmazás hello környezetében futnak, és nem méretezhetők legyenek külön-külön.</span><span class="sxs-lookup"><span data-stu-id="fdf95-456">WebJobs run in hello context of a web app and are not scalable separately.</span></span> <span data-ttu-id="fdf95-457">Például ha egy szokásos webes alkalmazás ezen példányát, a háttérben futó folyamat csak egy példánya van, és néhány hello kiszolgáló-erőforrások (CPU, a memória, a stb.) ellenkező esetben lenne elérhető tooserve webes tartalmat használ.</span><span class="sxs-lookup"><span data-stu-id="fdf95-457">For example, if you have one Standard web app instance, you have only one instance of your background process running, and it is using some of hello server resources (CPU, memory, etc.) that otherwise would be available tooserve web content.</span></span>

<span data-ttu-id="fdf95-458">Ha a forgalom függ a nap vagy hét napja, és hello háttérbeli feldolgozás meg kell toodo megvárhatja, alacsony forgalmú időpontban tudta ütemezni a a webjobs-feladatok toorun.</span><span class="sxs-lookup"><span data-stu-id="fdf95-458">If traffic varies by time of day or day of week, and if hello backend processing you need toodo can wait, you could schedule your WebJobs toorun at low-traffic times.</span></span> <span data-ttu-id="fdf95-459">Ha továbbra is túl magas, az adott megoldáshoz tartozó hello terhelést, hello háttér et futtathatja egy dedikált külön web app alkalmazásban webjobs-feladat erre a célra.</span><span class="sxs-lookup"><span data-stu-id="fdf95-459">If hello load is still too high for that solution, you can run hello backend as a WebJob in a separate web app dedicated for that purpose.</span></span> <span data-ttu-id="fdf95-460">Majd méretezheti a háttér-webalkalmazás egymástól függetlenül az előtérbeli webes alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="fdf95-460">You can then scale your backend web app independently from your frontend web app.</span></span>

<span data-ttu-id="fdf95-461">További információkért lásd: [skálázás WebJobs](websites-webjobs-resources.md#scale).</span><span class="sxs-lookup"><span data-stu-id="fdf95-461">For more information, see [Scaling WebJobs](websites-webjobs-resources.md#scale).</span></span>

### <a name="avoiding-web-app-timeout-shut-downs"></a><span data-ttu-id="fdf95-462">Webes alkalmazás időtúllépés leállítási oszlopánál elkerülése</span><span class="sxs-lookup"><span data-stu-id="fdf95-462">Avoiding web app timeout shut-downs</span></span>
<span data-ttu-id="fdf95-463">toomake meg arról, hogy a webjobs-feladatok mindig fut, és a webalkalmazás összes példányán fut, el kell tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fdf95-463">toomake sure your WebJobs are always running, and running on all instances of your web app, you have tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) feature.</span></span>

### <a name="using-hello-webjobs-sdk-outside-of-webjobs"></a><span data-ttu-id="fdf95-464">Hello kívül WebJobs WebJobs SDK használatával</span><span class="sxs-lookup"><span data-stu-id="fdf95-464">Using hello WebJobs SDK outside of WebJobs</span></span>
<span data-ttu-id="fdf95-465">Hello WebJobs SDK használó toorun nem rendelkezik Azure a webjobs-feladat.</span><span class="sxs-lookup"><span data-stu-id="fdf95-465">A program that uses hello WebJobs SDK doesn't have toorun in Azure in a WebJob.</span></span> <span data-ttu-id="fdf95-466">Helyileg futtathat, és más környezetekben, például egy felhőalapú szolgáltatás feldolgozói szerepkör vagy egy Windows-szolgáltatás is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="fdf95-466">It can run locally, and it can also run in other environments such as a Cloud Service worker role or a Windows service.</span></span> <span data-ttu-id="fdf95-467">Azonban csak érheti el WebJobs SDK irányítópult hello Azure-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdf95-467">However, you can only access hello WebJobs SDK dashboard through an Azure web app.</span></span> <span data-ttu-id="fdf95-468">toouse hello irányítópult tooconnect hello web app toohello storage-fiók beállításával hello AzureWebJobsDashboard kapcsolati karakterlánc hello használata van **konfigurálása** hello klasszikus portál lapján.</span><span class="sxs-lookup"><span data-stu-id="fdf95-468">toouse hello dashboard you have tooconnect hello web app toohello storage account you're using by setting hello AzureWebJobsDashboard connection string on hello **Configure** tab of hello classic portal.</span></span> <span data-ttu-id="fdf95-469">Ezt követően kaphat a toohello irányítópult hello URL-cím a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="fdf95-469">Then, you can get toohello Dashboard by using hello following URL:</span></span>

<span data-ttu-id="fdf95-470">https://{webappname}.SCM.azurewebsites.NET/azurejobs/#/Functions</span><span class="sxs-lookup"><span data-stu-id="fdf95-470">https://{webappname}.scm.azurewebsites.net/azurejobs/#/functions</span></span>

<span data-ttu-id="fdf95-471">További információkért lásd: [irányítópult első hello WebJobs SDK a helyi fejlesztési](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), azonban vegye figyelembe, hogy látható-e egy régi kapcsolati karakterlánc nevét.</span><span class="sxs-lookup"><span data-stu-id="fdf95-471">For more information, see [Getting a dashboard for local development with hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), but note that it shows an old connection string name.</span></span>

### <a name="more-webjobs-documentation"></a><span data-ttu-id="fdf95-472">További WebJobs-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="fdf95-472">More WebJobs documentation</span></span>
<span data-ttu-id="fdf95-473">További információkért lásd: [Azure WebJobs-dokumentáció erőforrások](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="fdf95-473">For more information, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span>
