---
title: "aaaReference navigáláshoz hello Azure-portálon"
description: "App Service Web hello különböző felhasználói élmények közötti hello Szolgáltatáskezelési portál és hello Azure Portal megismerése"
services: app-service
documentationcenter: 
author: jaime-espinosa
manager: erikre
editor: jimbe
ms.assetid: 0cc6a3cc-bd89-4a96-9177-d25f6fb737bb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: jaime-espinosa
ms.openlocfilehash: dcf7c1fc17f9a0c31005ad0f2fd53723d2966058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reference-for-navigating-hello-azure-portal"></a><span data-ttu-id="49c5d-103">Útmutató a Navigálás hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="49c5d-103">Reference for navigating hello Azure portal</span></span>
<span data-ttu-id="49c5d-104">Azure-webhelyek most nevezzük [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="49c5d-104">Azure Websites are now called [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="49c5d-105">Frissítjük a dokumentáció tooreflect mindegyikét hello Azure-portál a neve a változás- és tooprovide utasításokat.</span><span class="sxs-lookup"><span data-stu-id="49c5d-105">We're updating all of our documentation tooreflect this name change and tooprovide instructions for hello Azure Portal.</span></span> <span data-ttu-id="49c5d-106">Amíg a folyamat befejeződik, a webalkalmazásokkal az Azure-portálon hello használatához a dokumentum használhatja útmutatóként.</span><span class="sxs-lookup"><span data-stu-id="49c5d-106">Until that process is done, you can use this document as a guide for working with Web Apps in hello Azure portal.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="hello-future-of-hello-azure-classic-portal"></a><span data-ttu-id="49c5d-107">hello jövőjét a klasszikus Azure portál hello</span><span class="sxs-lookup"><span data-stu-id="49c5d-107">hello future of hello Azure Classic Portal</span></span>
<span data-ttu-id="49c5d-108">Amíg a módosítások a klasszikus Azure portál hello branding hello láthatja, a portál a felváltja hello Azure Portal hello folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="49c5d-108">While you will notice hello branding changes on hello Azure Classic Portal, that portal is in hello process of being replaced by hello Azure Portal.</span></span> <span data-ttu-id="49c5d-109">Hello klasszikus portál van kivezetése, mivel az új fejlesztési hello fókusz toohello Azure Portal van időeltolású.</span><span class="sxs-lookup"><span data-stu-id="49c5d-109">As hello classic portal is being phased out, hello focus for new development is shifting toohello Azure Portal.</span></span> <span data-ttu-id="49c5d-110">A Web Apps minden jövőbeli új funkcióit a hello Azure Portal határozza meg.</span><span class="sxs-lookup"><span data-stu-id="49c5d-110">All upcoming new features for Web Apps will come in hello Azure Portal.</span></span> <span data-ttu-id="49c5d-111">Hello Azure Portal tootake előnyeinek legújabb és legjobb, Web Apps rendelkezik-e toooffer hello használatának megkezdése.</span><span class="sxs-lookup"><span data-stu-id="49c5d-111">Start using hello Azure Portal tootake advantage of hello latest and greatest that Web Apps have toooffer.</span></span>

## <a name="layout-differences-between-hello-azure-classic-portal-and-azure-portal"></a><span data-ttu-id="49c5d-112">Hello klasszikus Azure portál és az Azure portál elrendezés különbségei</span><span class="sxs-lookup"><span data-stu-id="49c5d-112">Layout differences between hello Azure Classic Portal and Azure Portal</span></span>
<span data-ttu-id="49c5d-113">Klasszikus portál hello összes hello Azure services hello bal oldalán találhatók.</span><span class="sxs-lookup"><span data-stu-id="49c5d-113">In hello classic portal, all hello Azure services are listed on hello left hand side.</span></span> <span data-ttu-id="49c5d-114">A klasszikus portálon hello navigációs követi a faszerkezetben, ahol hello szolgáltatás-tól kezdődnek, és keresse meg az egyes elemek.</span><span class="sxs-lookup"><span data-stu-id="49c5d-114">Navigation in hello classic portal follows a tree structure, where you start from hello service and navigate into each element.</span></span> <span data-ttu-id="49c5d-115">Ez a struktúra jól működik, ha független összetevők kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="49c5d-115">This structure works well when managing independent components.</span></span> <span data-ttu-id="49c5d-116">Azonban, melyek Azure épülő alkalmazások összekapcsolt szolgáltatások, és a faszerkezet nem célszerű a szolgáltatások gyűjtemények használata.</span><span class="sxs-lookup"><span data-stu-id="49c5d-116">However, applications built on Azure are a collection of interconnected services, and this tree structure isn't ideal for working with collections of services.</span></span> 

<span data-ttu-id="49c5d-117">hello Azure portál segítségével könnyen toobuild alkalmazások-végpont több szolgáltatás-összetevőivel.</span><span class="sxs-lookup"><span data-stu-id="49c5d-117">hello Azure portal makes it easy toobuild applications end-to-end with components from multiple services.</span></span> <span data-ttu-id="49c5d-118">hello portal formájában rendszerezett *utak*.</span><span class="sxs-lookup"><span data-stu-id="49c5d-118">hello portal is organized as *journeys*.</span></span> <span data-ttu-id="49c5d-119">A *út* sorozata *paneleken*, mely hello tárolói különböző összetevőket.</span><span class="sxs-lookup"><span data-stu-id="49c5d-119">A *journey* is a series of *blades*, which are containers for hello different components.</span></span> <span data-ttu-id="49c5d-120">Például beállítása automatikus skálázást a webes alkalmazás egy *út* amely viszi Több panelen látható módon a következő példa hello: hello **webhelyen** (hogy panel címe még nem frissített toouse panel új terminológia hello), hello **beállítások** panelen, és hello **horizontális felskálázás** panelen.</span><span class="sxs-lookup"><span data-stu-id="49c5d-120">For example, setting up auto-scaling for a web app is a *journey* which takes you several blades as shown in hello following example: hello **web-site** blade (that blade title has not yet been updated toouse hello new terminology), hello **Settings** blade, and hello **Scale out** blade.</span></span> <span data-ttu-id="49c5d-121">Hello példában automatikus skálázás beállítása folyamatban van a CPU-használat toodepend úgy is egy **processzor** panelen.</span><span class="sxs-lookup"><span data-stu-id="49c5d-121">In hello example, auto scaling is being set up toodepend on CPU usage, so there is also a **CPU Percentage** blade.</span></span> <span data-ttu-id="49c5d-122">hello összetevőit hello *paneleken* nevezzük *részek*, amely csempék tűnik.</span><span class="sxs-lookup"><span data-stu-id="49c5d-122">hello components within hello *blades* are called *parts*, which look like tiles.</span></span> 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a><span data-ttu-id="49c5d-123">Navigációs példa: egy webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="49c5d-123">Navigation example: create a web app</span></span>
<span data-ttu-id="49c5d-124">Új webalkalmazások létrehozása a rendszer továbbra is egyszerű módon 1-2-3.</span><span class="sxs-lookup"><span data-stu-id="49c5d-124">Creating new web apps is still as easy as 1-2-3.</span></span> <span data-ttu-id="49c5d-125">a következő kép bemutatja hello klasszikus portál és hello portál egymás melletti toodemonstrate nem sokkal módosított hello több lépésből áll hello tooget szükséges, egy webalkalmazás be és futtatása.</span><span class="sxs-lookup"><span data-stu-id="49c5d-125">hello following image shows hello classic portal and hello portal side-by-side toodemonstrate that not much has changed in hello number of steps needed tooget a web app up and running.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

<span data-ttu-id="49c5d-126">Hello portálon választhat hello leggyakoribb azokból a webalkalmazásokból, például egy WordPress népszerű gyűjtemény alkalmazáson.</span><span class="sxs-lookup"><span data-stu-id="49c5d-126">In hello portal you can choose from hello most common types of web apps, including popular gallery applications like WordPress.</span></span> <span data-ttu-id="49c5d-127">A választható alkalmazások teljes listájának megtekintéséhez keresse fel a hello [Azure piactér].</span><span class="sxs-lookup"><span data-stu-id="49c5d-127">For a full list of available applications, visit hello [Azure Marketplace].</span></span>

<span data-ttu-id="49c5d-128">A webes alkalmazás létrehozásakor megadott URL-címet, az alkalmazásszolgáltatási csomag és a hely csak, mint a klasszikus portálon hello hello portálon.</span><span class="sxs-lookup"><span data-stu-id="49c5d-128">When you create a web app, you specify URL, App Service plan, and location in hello portal just as you do in hello classic portal.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

<span data-ttu-id="49c5d-129">Ezenkívül hello portál lehetővé teszi további közös beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="49c5d-129">In addition, hello portal lets you define other common settings.</span></span> <span data-ttu-id="49c5d-130">Például [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md) egyszerű toosee tétele és a kapcsolódó Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="49c5d-130">For example, [resource groups](../azure-resource-manager/resource-group-overview.md) make it simple toosee and manage related Azure resources.</span></span> 

## <a name="navigation-example-settings-and-features"></a><span data-ttu-id="49c5d-131">Navigációs példa: beállítások és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="49c5d-131">Navigation example: settings and features</span></span>
<span data-ttu-id="49c5d-132">Összes hello beállítások és szolgáltatások logikailag csoportba kerülnek egy egyetlen panelen, amelyből léphet.</span><span class="sxs-lookup"><span data-stu-id="49c5d-132">All hello settings and features are now logically grouped in a single blade, from which you can navigate.</span></span>

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

<span data-ttu-id="49c5d-133">Például, létrehozhat egyéni tartományok kattintva **egyéni tartományok és SSL** a hello **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="49c5d-133">For example, you can create custom domains by clicking **Custom domains and SSL** in hello **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

<span data-ttu-id="49c5d-134">a figyelési riasztások mentése tooset kattintson **kérések és hibák követése** , majd **riasztás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="49c5d-134">tooset up a monitoring alert, click **Requests and errors** and then **Add Alert**.</span></span>

![](./media/app-service-web-app-azure-portal/Monitoring.png)

<span data-ttu-id="49c5d-135">tooenable diagnosztika, kattintson a **diagnosztikai naplók** a hello **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="49c5d-135">tooenable diagnostics, click **Diagnostics logs** in hello **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

<span data-ttu-id="49c5d-136">tooconfigure alkalmazás beállításait, kattintson a **Alkalmazásbeállítások** a hello **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="49c5d-136">tooconfigure application settings, click **Application settings** in hello **Settings** blade.</span></span> 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

<span data-ttu-id="49c5d-137">Eltérő hello márka felhasználónevet, néhány dolog hello portálon átnevezték vagy csoportosított másképp toomake azt könnyebb toofind őket.</span><span class="sxs-lookup"><span data-stu-id="49c5d-137">Other than hello brand name, a few things in hello portal have been renamed or grouped differently toomake it easier toofind them.</span></span> <span data-ttu-id="49c5d-138">Az alábbiakban például van egy képernyőfelvétel a hello megfelelő lap az alkalmazásbeállítások (**konfigurálása**) hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="49c5d-138">For example, below is a screenshot of hello corresponding page for app settings (**Configure**) in hello classic portal.</span></span>

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a><span data-ttu-id="49c5d-139">További források</span><span class="sxs-lookup"><span data-stu-id="49c5d-139">More Resources</span></span>
[Azure Portal]: https://portal.azure.com
[Azure piactér]: /marketplace/

> [!NOTE]
> <span data-ttu-id="49c5d-141">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="49c5d-141">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="49c5d-142">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="49c5d-142">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="49c5d-143">A változások</span><span class="sxs-lookup"><span data-stu-id="49c5d-143">What's changed</span></span>
* <span data-ttu-id="49c5d-144">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="49c5d-144">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

