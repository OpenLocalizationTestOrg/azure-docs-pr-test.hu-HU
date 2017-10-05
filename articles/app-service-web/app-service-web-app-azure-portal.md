---
title: "Útmutató az Azure-portálon lépjen a"
description: "Ismerje meg a különböző felhasználói élmények az App Service Web között a Szolgáltatáskezelési portál és az Azure portálon"
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
ms.openlocfilehash: d1ef6e87d82df0840e49412154df40cc937b320c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="reference-for-navigating-the-azure-portal"></a><span data-ttu-id="972e0-103">Útmutató az Azure-portálon lépjen a</span><span class="sxs-lookup"><span data-stu-id="972e0-103">Reference for navigating the Azure portal</span></span>
<span data-ttu-id="972e0-104">Azure-webhelyek most nevezzük [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="972e0-104">Azure Websites are now called [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="972e0-105">A név változásnak, és utasításokkal láthatja el az Azure portál dokumentációban mindegyikét frissítjük.</span><span class="sxs-lookup"><span data-stu-id="972e0-105">We're updating all of our documentation to reflect this name change and to provide instructions for the Azure Portal.</span></span> <span data-ttu-id="972e0-106">Amíg a folyamat befejeződik, használhatja a dokumentum útmutatóként dolgozik a webalkalmazásokkal az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="972e0-106">Until that process is done, you can use this document as a guide for working with Web Apps in the Azure portal.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="the-future-of-the-azure-classic-portal"></a><span data-ttu-id="972e0-107">A jövőben is a klasszikus Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="972e0-107">The future of the Azure Classic Portal</span></span>
<span data-ttu-id="972e0-108">A márkajelzési változásai a klasszikus Azure portálon megfigyelheti, a portál napjainkban folyamatban van a felváltja az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="972e0-108">While you will notice the branding changes on the Azure Classic Portal, that portal is in the process of being replaced by the Azure Portal.</span></span> <span data-ttu-id="972e0-109">A klasszikus portálon van kivezetése, mivel a fókusz az új fejlesztési van időeltolású az Azure-portálra.</span><span class="sxs-lookup"><span data-stu-id="972e0-109">As the classic portal is being phased out, the focus for new development is shifting to the Azure Portal.</span></span> <span data-ttu-id="972e0-110">A Web Apps minden jövőbeli új funkcióit határozza meg az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="972e0-110">All upcoming new features for Web Apps will come in the Azure Portal.</span></span> <span data-ttu-id="972e0-111">Indítsa el az Azure portál használatával, hogy webalkalmazásokat fel kell-e a legújabb és legjobb előnyeinek kihasználása érdekében.</span><span class="sxs-lookup"><span data-stu-id="972e0-111">Start using the Azure Portal to take advantage of the latest and greatest that Web Apps have to offer.</span></span>

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a><span data-ttu-id="972e0-112">A klasszikus Azure portál és az Azure portál elrendezés különbségei</span><span class="sxs-lookup"><span data-stu-id="972e0-112">Layout differences between the Azure Classic Portal and Azure Portal</span></span>
<span data-ttu-id="972e0-113">A klasszikus portálon minden Azure-szolgáltatások bal oldali listája látható.</span><span class="sxs-lookup"><span data-stu-id="972e0-113">In the classic portal, all the Azure services are listed on the left hand side.</span></span> <span data-ttu-id="972e0-114">A klasszikus portálon navigációs követi a faszerkezetben, amelyen a szolgáltatás indítása, és keresse meg az egyes elemek.</span><span class="sxs-lookup"><span data-stu-id="972e0-114">Navigation in the classic portal follows a tree structure, where you start from the service and navigate into each element.</span></span> <span data-ttu-id="972e0-115">Ez a struktúra jól működik, ha független összetevők kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="972e0-115">This structure works well when managing independent components.</span></span> <span data-ttu-id="972e0-116">Azonban, melyek Azure épülő alkalmazások összekapcsolt szolgáltatások, és a faszerkezet nem célszerű a szolgáltatások gyűjtemények használata.</span><span class="sxs-lookup"><span data-stu-id="972e0-116">However, applications built on Azure are a collection of interconnected services, and this tree structure isn't ideal for working with collections of services.</span></span> 

<span data-ttu-id="972e0-117">Az Azure-portálon megkönnyíti az alkalmazások-végpont több szolgáltatás-összetevőivel felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="972e0-117">The Azure portal makes it easy to build applications end-to-end with components from multiple services.</span></span> <span data-ttu-id="972e0-118">A portál formájában rendszerezett *utak*.</span><span class="sxs-lookup"><span data-stu-id="972e0-118">The portal is organized as *journeys*.</span></span> <span data-ttu-id="972e0-119">A *út* sorozata *paneleken*, amely a különböző összetevők tárolói.</span><span class="sxs-lookup"><span data-stu-id="972e0-119">A *journey* is a series of *blades*, which are containers for the different components.</span></span> <span data-ttu-id="972e0-120">Például beállítása automatikus skálázást a webes alkalmazás egy *út* amely viszi Több paneleket az alábbi példában látható módon: a **webhelyen** (hogy panel címe nem még megújult az új panel terminológiát), a **beállítások** panelt, és a **horizontális felskálázás** panelen.</span><span class="sxs-lookup"><span data-stu-id="972e0-120">For example, setting up auto-scaling for a web app is a *journey* which takes you several blades as shown in the following example: the **web-site** blade (that blade title has not yet been updated to use the new terminology), the **Settings** blade, and the **Scale out** blade.</span></span> <span data-ttu-id="972e0-121">A példában automatikus skálázás beállítása folyamatban van a CPU-használat függ, így nincs is egy **processzor** panelen.</span><span class="sxs-lookup"><span data-stu-id="972e0-121">In the example, auto scaling is being set up to depend on CPU usage, so there is also a **CPU Percentage** blade.</span></span> <span data-ttu-id="972e0-122">Az összetevők belül a *paneleken* nevezzük *részek*, amely csempék tűnik.</span><span class="sxs-lookup"><span data-stu-id="972e0-122">The components within the *blades* are called *parts*, which look like tiles.</span></span> 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a><span data-ttu-id="972e0-123">Navigációs példa: egy webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="972e0-123">Navigation example: create a web app</span></span>
<span data-ttu-id="972e0-124">Új webalkalmazások létrehozása a rendszer továbbra is egyszerű módon 1-2-3.</span><span class="sxs-lookup"><span data-stu-id="972e0-124">Creating new web apps is still as easy as 1-2-3.</span></span> <span data-ttu-id="972e0-125">A következő kép bemutatja a klasszikus portál és a portál-párhuzamos annak bemutatásához, hogy nem sokkal kevesebb lépésben a webes alkalmazás eléréséhez szükséges, és fut a megváltozott.</span><span class="sxs-lookup"><span data-stu-id="972e0-125">The following image shows the classic portal and the portal side-by-side to demonstrate that not much has changed in the number of steps needed to get a web app up and running.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

<span data-ttu-id="972e0-126">A portál a leggyakoribb webes alkalmazásokat, akár népszerű gyűjtemény alkalmazások, például WordPress közül választhat.</span><span class="sxs-lookup"><span data-stu-id="972e0-126">In the portal you can choose from the most common types of web apps, including popular gallery applications like WordPress.</span></span> <span data-ttu-id="972e0-127">A választható alkalmazások teljes listájának megtekintéséhez keresse fel a [Azure piactér].</span><span class="sxs-lookup"><span data-stu-id="972e0-127">For a full list of available applications, visit the [Azure Marketplace].</span></span>

<span data-ttu-id="972e0-128">A webes alkalmazás létrehozásakor megadott URL-címet, az alkalmazásszolgáltatási csomag és a hely csak, mint a klasszikus portál a portálon.</span><span class="sxs-lookup"><span data-stu-id="972e0-128">When you create a web app, you specify URL, App Service plan, and location in the portal just as you do in the classic portal.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

<span data-ttu-id="972e0-129">Emellett a portál segítségével más általános beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="972e0-129">In addition, the portal lets you define other common settings.</span></span> <span data-ttu-id="972e0-130">Például [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md) révén egyszerűen áttekinthetők és felügyelhetők a kapcsolódó Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="972e0-130">For example, [resource groups](../azure-resource-manager/resource-group-overview.md) make it simple to see and manage related Azure resources.</span></span> 

## <a name="navigation-example-settings-and-features"></a><span data-ttu-id="972e0-131">Navigációs példa: beállítások és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="972e0-131">Navigation example: settings and features</span></span>
<span data-ttu-id="972e0-132">A beállítások és szolgáltatások logikailag csoportba kerülnek egy egyetlen panelen, amelyből léphet.</span><span class="sxs-lookup"><span data-stu-id="972e0-132">All the settings and features are now logically grouped in a single blade, from which you can navigate.</span></span>

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

<span data-ttu-id="972e0-133">Például, létrehozhat egyéni tartományok kattintva **egyéni tartományok és SSL** a a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="972e0-133">For example, you can create custom domains by clicking **Custom domains and SSL** in the **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

<span data-ttu-id="972e0-134">A figyelési riasztások beállításához kattintson **kérések és hibák követése** , majd **hozzáadása riasztás**.</span><span class="sxs-lookup"><span data-stu-id="972e0-134">To set up a monitoring alert, click **Requests and errors** and then **Add Alert**.</span></span>

![](./media/app-service-web-app-azure-portal/Monitoring.png)

<span data-ttu-id="972e0-135">Diagnosztika engedélyezéséhez kattintson **diagnosztikai naplók** a a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="972e0-135">To enable diagnostics, click **Diagnostics logs** in the **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

<span data-ttu-id="972e0-136">Az alkalmazás beállításainak megadásához kattintson **Alkalmazásbeállítások** a a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="972e0-136">To configure application settings, click **Application settings** in the **Settings** blade.</span></span> 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

<span data-ttu-id="972e0-137">Eltérő márka felhasználónevet néhány dolgot a portálon átnevezték vagy azok megkereséséről könnyebb másképp csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="972e0-137">Other than the brand name, a few things in the portal have been renamed or grouped differently to make it easier to find them.</span></span> <span data-ttu-id="972e0-138">Az alábbiakban például van egy Képernyőkép a beállítások a megfelelő lap (**konfigurálása**) a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="972e0-138">For example, below is a screenshot of the corresponding page for app settings (**Configure**) in the classic portal.</span></span>

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a><span data-ttu-id="972e0-139">További források</span><span class="sxs-lookup"><span data-stu-id="972e0-139">More Resources</span></span>
[Azure Portal]: https://portal.azure.com
<span data-ttu-id="972e0-140">[Azure piactér]: /marketplace/</span><span class="sxs-lookup"><span data-stu-id="972e0-140">[Azure Marketplace]: /marketplace/</span></span>

> [!NOTE]
> <span data-ttu-id="972e0-141">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="972e0-141">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="972e0-142">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="972e0-142">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="972e0-143">A változások</span><span class="sxs-lookup"><span data-stu-id="972e0-143">What's changed</span></span>
* <span data-ttu-id="972e0-144">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="972e0-144">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

