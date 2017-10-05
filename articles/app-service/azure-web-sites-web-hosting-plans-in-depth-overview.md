---
title: "Az Azure App Service-csomagok részletes áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan App Service-csomagok Azure App Service munka, és hogyan azok előnyeit, a kezelhetőséget."
keywords: "app service, azure app service, méret, méretezhető, app service-csomag, app service ára"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: f97be571d104e3cc1c6ee732886fa7133ba0dc83
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a><span data-ttu-id="70b9d-104">Az Azure App Service díjcsomagjainak részletes áttekintése</span><span class="sxs-lookup"><span data-stu-id="70b9d-104">Azure App Service plans in-depth overview</span></span>

<span data-ttu-id="70b9d-105">App Service-csomagokról az alkalmazások futtatásához használt fizikai erőforrások gyűjteményét képviseli.</span><span class="sxs-lookup"><span data-stu-id="70b9d-105">App Service plans represent the collection of physical resources used to host your apps.</span></span>

<span data-ttu-id="70b9d-106">Az App Service-csomagok a következőket határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="70b9d-106">App Service plans define:</span></span>

- <span data-ttu-id="70b9d-107">A régióban (USA nyugati régiója, USA keleti régiója, stb.)</span><span class="sxs-lookup"><span data-stu-id="70b9d-107">Region (West US, East US, etc.)</span></span>
- <span data-ttu-id="70b9d-108">Méretezési száma (egy, két, három példányt, stb.)</span><span class="sxs-lookup"><span data-stu-id="70b9d-108">Scale count (one, two, three instances, etc.)</span></span>
- <span data-ttu-id="70b9d-109">Példányméretének (Small, Medium, Large)</span><span class="sxs-lookup"><span data-stu-id="70b9d-109">Instance size (Small, Medium, Large)</span></span>
- <span data-ttu-id="70b9d-110">Termékváltozat (Ingyenes, Közös, Alapszintű, Standard, Prémium)</span><span class="sxs-lookup"><span data-stu-id="70b9d-110">SKU (Free, Shared, Basic, Standard, Premium)</span></span>

<span data-ttu-id="70b9d-111">Web Apps, Mobile Apps, az API Apps, függvény alkalmazások (vagy funkciók), a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) összes futtassa az App Service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="70b9d-111">Web Apps, Mobile Apps, API Apps, Function Apps (or Functions), in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) all run in an App Service plan.</span></span>  <span data-ttu-id="70b9d-112">Az ugyanahhoz az előfizetéshez, és a régió alkalmazások oszthatnak meg az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="70b9d-112">Apps in the same subscription, and region can share an App Service plan.</span></span> 

<span data-ttu-id="70b9d-113">Hozzárendelt összes alkalmazást egy **App Service-csomag** a meghatározott erőforrások megosztása.</span><span class="sxs-lookup"><span data-stu-id="70b9d-113">All applications assigned to an **App Service plan** share the resources defined by it.</span></span> <span data-ttu-id="70b9d-114">A megosztás menti a pénz, esetén több alkalmazást az önálló App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="70b9d-114">This sharing saves money when hosting multiple apps in a single App Service plan.</span></span>

<span data-ttu-id="70b9d-115">Az **App Service-csomagok** szintje az **Ingyenes** és **Közös** termékváltozatoktól az **Alapszintű**, **Standard** és **Prémium** változatokig terjed. A magasabb szintű változatok több erőforrást és szolgáltatást biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="70b9d-115">Your **App Service plan** can scale from **Free** and **Shared** SKUs to **Basic**, **Standard**, and **Premium** SKUs giving you access to more resources and features along the way.</span></span>

<span data-ttu-id="70b9d-116">Ha az App Service-csomag **alapvető** SKU vagy újabb verzióját, majd megadhatja a **mérete** , a méretezés, a virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="70b9d-116">If your App Service plan is set to **Basic** SKU or higher, then you can control the **size** and scale count of the VMs.</span></span>

<span data-ttu-id="70b9d-117">Például ha a terv normál rétegben két "kicsi" példányt használatára van konfigurálva, terv társított összes alkalmazás futtassa mindkét-példányokon.</span><span class="sxs-lookup"><span data-stu-id="70b9d-117">For example, if your plan is configured to use two "small" instances in the standard service tier, all apps that are associated with that plan run on both instances.</span></span> <span data-ttu-id="70b9d-118">Alkalmazások is van a standard szint szolgáltatásainak eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="70b9d-118">Apps also have access to the standard service tier features.</span></span> <span data-ttu-id="70b9d-119">Alkalmazások futnak terv példányok teljes körűen felügyelt, és magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="70b9d-119">Plan instances on which apps are running are fully managed and highly available.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="70b9d-120">Az App Service-csomag **Termékváltozat** és **Méretezés** tulajdonsága a költségeket befolyásolja, nem a csomagban üzemeltetett alkalmazások számát.</span><span class="sxs-lookup"><span data-stu-id="70b9d-120">The **SKU** and **Scale** of the App Service plan determines the cost and not the number of apps hosted in it.</span></span>

<span data-ttu-id="70b9d-121">Ez a cikk ismerteti, a kulcs jellemzőit, például a réteg és a méretezhetőség, az App Service-csomagot, és hogyan lépnek játékba, miközben az alkalmazások kezelését.</span><span class="sxs-lookup"><span data-stu-id="70b9d-121">This article explores the key characteristics, such as tier and scale, of an App Service plan and how they come into play while managing your apps.</span></span>

## <a name="apps-and-app-service-plans"></a><span data-ttu-id="70b9d-122">Alkalmazások és az App Service-csomagokról</span><span class="sxs-lookup"><span data-stu-id="70b9d-122">Apps and App Service plans</span></span>

<span data-ttu-id="70b9d-123">Lehet, hogy az App Service alkalmazás társítva van egy adott időpontban csak egy App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="70b9d-123">An app in App Service can be associated with only one App Service plan at any given time.</span></span>

<span data-ttu-id="70b9d-124">Alkalmazások és csomagok is tartalmaznak egy **erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="70b9d-124">Both apps and plans are contained in a **resource group**.</span></span> <span data-ttu-id="70b9d-125">Erőforráscsoport minden szereplő erőforrásra életciklus határa funkcionál.</span><span class="sxs-lookup"><span data-stu-id="70b9d-125">A resource group serves as the lifecycle boundary for every resource that's within it.</span></span> <span data-ttu-id="70b9d-126">Erőforráscsoportok segítségével együtt kezelheti az alkalmazás minden a helyére.</span><span class="sxs-lookup"><span data-stu-id="70b9d-126">You can use resource groups to manage all the pieces of an application together.</span></span>

<span data-ttu-id="70b9d-127">Egyetlen erőforráscsoportként működnek rendelkezhet több App Service-csomagokról, mert rendel a különböző alkalmazások különböző fizikai erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="70b9d-127">Because a single resource group can have multiple App Service plans, you can allocate different apps to different physical resources.</span></span>

<span data-ttu-id="70b9d-128">Például elkülönítheti a erőforrások fejlesztői, tesztelési és éles környezetek között.</span><span class="sxs-lookup"><span data-stu-id="70b9d-128">For example, you can separate resources among dev, test, and production environments.</span></span> <span data-ttu-id="70b9d-129">Éles üzemi pontjának és fejlesztési és tesztelési célú külön környezeteket rendelkező lehetővé teszi az erőforrások elkülönítése.</span><span class="sxs-lookup"><span data-stu-id="70b9d-129">Having separate environments for production and dev/test lets you isolate resources.</span></span> <span data-ttu-id="70b9d-130">Ezzel a módszerrel szemben az alkalmazások új verziójának tesztelése betöltése nem "versenyeznek" az az erőforrásokhoz, az éles alkalmazásokat, amelyek valós ügyfelek szolgál.</span><span class="sxs-lookup"><span data-stu-id="70b9d-130">In this way, load testing against a new version of your apps does not compete for the same resources as your production apps, which are serving real customers.</span></span>

<span data-ttu-id="70b9d-131">Ha több csomagokról egyetlen erőforráscsoportként működnek, megadhatja a földrajzi régió átfogó alkalmazás is.</span><span class="sxs-lookup"><span data-stu-id="70b9d-131">When you have multiple plans in a single resource group, you can also define an application that spans geographical regions.</span></span>

<span data-ttu-id="70b9d-132">Például egy magas rendelkezésre állású két régiókban futó alkalmazást tartalmaz legalább két terveket, minden egyes régió egy és minden terv társított egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="70b9d-132">For example, a highly available app running in two regions includes at least two plans, one for each region, and one app associated with each plan.</span></span> <span data-ttu-id="70b9d-133">Ilyen esetben az alkalmazás összes példányát egyetlen erőforráscsoportként működnek majd tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="70b9d-133">In such a situation, all the copies of the app are then contained in a single resource group.</span></span> <span data-ttu-id="70b9d-134">Több csomagokban és több alkalmazás erőforráscsoport rendelkező egyszerűen kezelése, szabályozása és az alkalmazás állapotának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="70b9d-134">Having a resource group with multiple plans and multiple apps makes it easy to manage, control, and view the health of the application.</span></span>

## <a name="create-an-app-service-plan-or-use-existing-one"></a><span data-ttu-id="70b9d-135">Az App Service-csomag létrehozása vagy meglévő ütemezés használata</span><span class="sxs-lookup"><span data-stu-id="70b9d-135">Create an App Service plan or use existing one</span></span>

<span data-ttu-id="70b9d-136">Amikor létrehoz egy alkalmazást, érdemes egy erőforráscsoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70b9d-136">When you create an app, you should consider creating a resource group.</span></span> <span data-ttu-id="70b9d-137">Másrészről Ha az alkalmazás egy nagyobb alkalmazás egy összetevőjét, hozza létre, amely nagyobb alkalmazás számára kíván lefoglalni az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="70b9d-137">On the other hand, if this app is a component for a larger application, create it within the resource group that's allocated for that larger application.</span></span>

<span data-ttu-id="70b9d-138">-E az alkalmazás egy teljesen új alkalmazás vagy nagyobb egy részét, dönthet úgy, hogy egy meglévő csomag használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="70b9d-138">Whether the app is an altogether new application or part of a larger one, you can choose to use an existing plan to host it or create a new one.</span></span> <span data-ttu-id="70b9d-139">Ez a döntés nagyobb kapacitást és várható terhelési adott esetben.</span><span class="sxs-lookup"><span data-stu-id="70b9d-139">This decision is more a question of capacity and expected load.</span></span>

<span data-ttu-id="70b9d-140">Javasoljuk, hogy az alkalmazás egy új App Service-be elkülönítése ütemezésének:</span><span class="sxs-lookup"><span data-stu-id="70b9d-140">We recommend isolating your app into a new App Service plan when:</span></span>

- <span data-ttu-id="70b9d-141">Alkalmazás erőforrás-igényes áll.</span><span class="sxs-lookup"><span data-stu-id="70b9d-141">App is resource-intensive.</span></span>
- <span data-ttu-id="70b9d-142">Az alkalmazás rendelkezik egy meglévő csomagban tárolt más alkalmazásokból különböző méretezési tényező.</span><span class="sxs-lookup"><span data-stu-id="70b9d-142">App has different scaling factors from the other apps hosted in an existing plan.</span></span>
- <span data-ttu-id="70b9d-143">Erőforrás más földrajzi régióban kell alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="70b9d-143">App needs resource in a different geographical region.</span></span>

<span data-ttu-id="70b9d-144">Ezzel a módszerrel lehet új készletét erőforrásokat lefoglalni az alkalmazás és az alkalmazások nagyobb ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="70b9d-144">This way you can allocate a new set of resources for your app and gain greater control of your apps.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="70b9d-145">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="70b9d-145">Create an App Service plan</span></span>

> [!TIP]
> <span data-ttu-id="70b9d-146">Ha az App Service-környezetek, megtekintheti az adott dokumentációjában App Service Environment-környezetek innen: [egy App Service Environment-környezetben az App Service-csomag létrehozása](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span><span class="sxs-lookup"><span data-stu-id="70b9d-146">If you have an App Service Environment, you can review the documentation specific to App Service Environments here: [Create an App Service plan in an App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span></span>

<span data-ttu-id="70b9d-147">Üres App Service-csomagot is létrehozhat, az App Service-csomag tallózási élménynek vagy alkalmazás létrehozásának részeként.</span><span class="sxs-lookup"><span data-stu-id="70b9d-147">You can create an empty App Service plan from the App Service plan browse experience or as part of app creation.</span></span>

<span data-ttu-id="70b9d-148">A a [Azure-portálon](https://portal.azure.com), kattintson a **új** > **Web + mobil**, majd válassza ki **webalkalmazás** vagy más App Service alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="70b9d-148">In the [Azure portal](https://portal.azure.com), click **New** > **Web + mobile**, and then select **Web App** or other App Service app kind.</span></span>

![Alkalmazás létrehozása az Azure portálon.][createWebApp]

<span data-ttu-id="70b9d-150">Ezután válassza ki vagy hozzon létre az új alkalmazás az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="70b9d-150">You can then select or create the App Service plan for the new app.</span></span>

 ![Az App Service-csomag létrehozása.][createASP]

<span data-ttu-id="70b9d-152">Az App Service-csomag létrehozásához kattintson a **[+] hozzon létre új**, típusa a **App Service-csomag** nevet, és válasszon egy megfelelő **hely**.</span><span class="sxs-lookup"><span data-stu-id="70b9d-152">To create an App Service plan, click **[+] Create New**, type the **App Service plan** name, and then select an appropriate **Location**.</span></span> <span data-ttu-id="70b9d-153">Kattintson a **tarifacsomag**, majd válassza ki a megfelelő tarifacsomagot a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="70b9d-153">Click **Pricing tier**, and then select an appropriate pricing tier for the service.</span></span> <span data-ttu-id="70b9d-154">Válassza ki **összes** több, mint az beállítások árképzési nézetre **szabad** és **megosztott**.</span><span class="sxs-lookup"><span data-stu-id="70b9d-154">Select **View all** to view more pricing options, such as **Free** and **Shared**.</span></span> <span data-ttu-id="70b9d-155">Miután kiválasztotta a tarifacsomagot, kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="70b9d-155">After you have selected the pricing tier, click the **Select** button.</span></span>

## <a name="move-an-app-to-a-different-app-service-plan"></a><span data-ttu-id="70b9d-156">Az alkalmazások áthelyezése egy másik App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="70b9d-156">Move an app to a different App Service plan</span></span>

<span data-ttu-id="70b9d-157">Az alkalmazások áthelyezése egy másik App Service-csomag a a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="70b9d-157">You can move an app to a different App Service plan in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="70b9d-158">Mindaddig, amíg a ugyanazt az erőforráscsoportot és a földrajzi régióban találhatók a csomagok, alkalmazások áthelyezhet tervek.</span><span class="sxs-lookup"><span data-stu-id="70b9d-158">You can move apps between plans as long as the plans are in the same resource group and geographical region.</span></span>

<span data-ttu-id="70b9d-159">Az alkalmazások áthelyezése másik terv:</span><span class="sxs-lookup"><span data-stu-id="70b9d-159">To move an app to another plan:</span></span>

- <span data-ttu-id="70b9d-160">Nyissa meg az alkalmazást, amelynek át szeretné helyezni.</span><span class="sxs-lookup"><span data-stu-id="70b9d-160">Navigate to the app that you want to move.</span></span>
- <span data-ttu-id="70b9d-161">Az a **menü**, keresse meg a **App Service-csomag** szakasz.</span><span class="sxs-lookup"><span data-stu-id="70b9d-161">In the **Menu**, look for the **App Service Plan** section.</span></span>
- <span data-ttu-id="70b9d-162">Válassza ki **módosítás App Service-csomag** a folyamat elindításához.</span><span class="sxs-lookup"><span data-stu-id="70b9d-162">Select **Change App Service plan** to start the process.</span></span>

<span data-ttu-id="70b9d-163">**App Service-csomag módosítása** megnyitja a **App Service-csomag** választó.</span><span class="sxs-lookup"><span data-stu-id="70b9d-163">**Change App Service plan** opens the **App Service plan** selector.</span></span> <span data-ttu-id="70b9d-164">Ezen a ponton választhatja ki a meglévő séma ezzel az alkalmazással történő áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="70b9d-164">At this point, you can pick an existing plan to move this app into.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="70b9d-165">A rendszer a következő feltételek alapján szűri a válassza ki az App Service-csomag felhasználói felület:</span><span class="sxs-lookup"><span data-stu-id="70b9d-165">The select App Service plan UI is filtered by the following criteria:</span></span>
> - <span data-ttu-id="70b9d-166">Az ugyanazon erőforráscsoporton belül van</span><span class="sxs-lookup"><span data-stu-id="70b9d-166">Exists within the same Resource Group</span></span>
> - <span data-ttu-id="70b9d-167">Ugyanabban a földrajzi régióban található</span><span class="sxs-lookup"><span data-stu-id="70b9d-167">Exists in the same Geographical Region</span></span>
> - <span data-ttu-id="70b9d-168">Az azonos webtárhely belül van</span><span class="sxs-lookup"><span data-stu-id="70b9d-168">Exists within the same Webspace</span></span>
>
> <span data-ttu-id="70b9d-169">Egy webtárhely egy logikai szerkezet belül App Service, amely meghatározza a kiszolgáló erőforrások csoportosítása.</span><span class="sxs-lookup"><span data-stu-id="70b9d-169">A Webspace is a logical construct within App Service which defines a grouping of server resources.</span></span> <span data-ttu-id="70b9d-170">Egy földrajzi régiót (például az USA nyugati régiója) használja az App Service ügyfelek lefoglalásához sok Webspaces tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="70b9d-170">A Geographical region (such as West US) contains many Webspaces in order to allocate customers using App Service.</span></span> <span data-ttu-id="70b9d-171">Jelenleg az App Service-erőforrások nem helyezhetők át Webspaces tudni.</span><span class="sxs-lookup"><span data-stu-id="70b9d-171">Currently, App Service resources aren’t able to be moved between Webspaces.</span></span>
>

![App Service-csomag választó.][change]

<span data-ttu-id="70b9d-173">Minden egyes tervnek a saját IP-címek.</span><span class="sxs-lookup"><span data-stu-id="70b9d-173">Each plan has its own pricing tier.</span></span> <span data-ttu-id="70b9d-174">Például egy webhely áthelyezése egy ingyenes szint a Standard szintű, hogy lehetővé teszi, hogy a szolgáltatások és erőforrások Standard csomagra hozzárendelt összes alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="70b9d-174">For example, moving a site from a Free tier to a Standard tier, enables all apps assigned to it to use the features and resources of the Standard tier.</span></span>

## <a name="clone-an-app-to-a-different-app-service-plan"></a><span data-ttu-id="70b9d-175">Egy alkalmazás egy másik App Service-csomagra klónozása</span><span class="sxs-lookup"><span data-stu-id="70b9d-175">Clone an app to a different App Service plan</span></span>

<span data-ttu-id="70b9d-176">Ha azt szeretné, az alkalmazás áthelyezése egy másik régióban, egy alternatív-e az alkalmazás a Klónozás.</span><span class="sxs-lookup"><span data-stu-id="70b9d-176">If you want to move the app to a different region, one alternative is app cloning.</span></span> <span data-ttu-id="70b9d-177">A Klónozás teszi az alkalmazás másolatát egy új vagy meglévő App Service-csomag bármely régióban.</span><span class="sxs-lookup"><span data-stu-id="70b9d-177">Cloning makes a copy of your app in a new or existing App Service plan in any region.</span></span>

<span data-ttu-id="70b9d-178">Található **Klónozott alkalmazás** a a **Fejlesztőeszközök** a menü részét.</span><span class="sxs-lookup"><span data-stu-id="70b9d-178">You can find **Clone App** in the **Development Tools** section of the menu.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="70b9d-179">A Klónozás rendelkezik néhány címen olvashat [Azure App Service-alkalmazást az Azure-portál használatával Klónozás](../app-service-web/app-service-web-app-cloning-portal.md).</span><span class="sxs-lookup"><span data-stu-id="70b9d-179">Cloning has some limitations that you can read about at [Azure App Service App cloning using Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).</span></span>

## <a name="scale-an-app-service-plan"></a><span data-ttu-id="70b9d-180">Az App Service-csomag vertikális</span><span class="sxs-lookup"><span data-stu-id="70b9d-180">Scale an App Service plan</span></span>

<span data-ttu-id="70b9d-181">A csomag skálázása három módja van:</span><span class="sxs-lookup"><span data-stu-id="70b9d-181">There are three ways to scale a plan:</span></span>

- <span data-ttu-id="70b9d-182">**Módosítsa a csomag csomagazonosítóját tarifacsomag**.</span><span class="sxs-lookup"><span data-stu-id="70b9d-182">**Change the plan’s pricing tier**.</span></span> <span data-ttu-id="70b9d-183">Standard, és minden alkalmazás rendelt Standard csomagra szolgáltatásait is használni a csomag az alapszintű rétegben alakíthatók ki.</span><span class="sxs-lookup"><span data-stu-id="70b9d-183">A plan in the Basic tier can be converted to Standard, and all apps assigned to it to use the features of the Standard tier.</span></span>
- <span data-ttu-id="70b9d-184">**A csomag példányméretének módosítása**.</span><span class="sxs-lookup"><span data-stu-id="70b9d-184">**Change the plan’s instance size**.</span></span> <span data-ttu-id="70b9d-185">Tegyük fel az alapszintű rétegben, amely használja a kisméretű példányok terv nagy példányok használandó módosítható.</span><span class="sxs-lookup"><span data-stu-id="70b9d-185">As an example, a plan in the Basic tier that uses small instances can be changed to use large instances.</span></span> <span data-ttu-id="70b9d-186">Minden alkalmazás társított terv most használhatja a további memória és a CPU-erőforrást, amely nagyobb példányméretének nyújt.</span><span class="sxs-lookup"><span data-stu-id="70b9d-186">All apps that are associated with that plan now can use the additional memory and CPU resources that the larger instance size offers.</span></span>
- <span data-ttu-id="70b9d-187">**Módosítsa a terv példányszám**.</span><span class="sxs-lookup"><span data-stu-id="70b9d-187">**Change the plan’s instance count**.</span></span> <span data-ttu-id="70b9d-188">Például a Standard tervet készíteni, amely három alkalmazáspéldányra horizontálisan is méretezhető 10 példányok.</span><span class="sxs-lookup"><span data-stu-id="70b9d-188">For example, a Standard plan that's scaled out to three instances can be scaled to 10 instances.</span></span> <span data-ttu-id="70b9d-189">A prémium tervének kiterjeszthető 20 példányokhoz (rendelkezésre állási) feltételei érvényesek.</span><span class="sxs-lookup"><span data-stu-id="70b9d-189">A Premium plan can be scaled out to 20 instances (subject to availability).</span></span> <span data-ttu-id="70b9d-190">Minden alkalmazás társított terv most használhatja a további memória és a Processzor-erőforrások kínál a nagyobb a példányok száma.</span><span class="sxs-lookup"><span data-stu-id="70b9d-190">All apps that are associated with that plan now can use the additional memory and CPU resources that the larger instance count offers.</span></span>

<span data-ttu-id="70b9d-191">Az árképzési szint és a példány mérete kattintva módosíthatja a **vertikális Felskálázás** elem alatt az alkalmazás vagy az App Service-csomag beállításai.</span><span class="sxs-lookup"><span data-stu-id="70b9d-191">You can change the pricing tier and instance size by clicking the **Scale Up** item under settings for either the app or the App Service plan.</span></span> <span data-ttu-id="70b9d-192">Változások az App Service-csomag vonatkoznak, és hatással rajta futó összes alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="70b9d-192">Changes apply to the App Service plan and affect all apps that it hosts.</span></span>

 ![Méretezést kívánó alkalmazás értékeinek beállításához.][pricingtier]

## <a name="app-service-plan-cleanup"></a><span data-ttu-id="70b9d-194">Az alkalmazásszolgáltatási csomag karbantartása</span><span class="sxs-lookup"><span data-stu-id="70b9d-194">App Service plan cleanup</span></span>

> [!IMPORTANT]
> <span data-ttu-id="70b9d-195">**App Service-csomagok** , amelyek nem találhatók alkalmazások hozzájuk társított továbbra is függő díj terheli, mivel azok továbbra is számítási kapacitás lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="70b9d-195">**App Service plans** that have no apps associated to them still incur charges since they continue to reserve the compute capacity.</span></span>

<span data-ttu-id="70b9d-196">Az utolsó az App Service-csomag tárolva alkalmazást törlik váratlan díjak elkerülése az eredményül kapott üres App Service-csomag is törlődik.</span><span class="sxs-lookup"><span data-stu-id="70b9d-196">To avoid unexpected charges, when the last app hosted in an App Service plan is deleted, the resulting empty App Service plan is also deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="70b9d-197">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="70b9d-197">Summary</span></span>

<span data-ttu-id="70b9d-198">App Service-csomagokról funkciók és kapacitás szempontjából lehet megosztani az alkalmazások csoportját képviselik.</span><span class="sxs-lookup"><span data-stu-id="70b9d-198">App Service plans represent a set of features and capacity that you can share across your apps.</span></span> <span data-ttu-id="70b9d-199">App Service-csomagokról rugalmasan, a konkrét alkalmazásokat rendelhet erőforrások olyan készletét, és tovább optimalizálhatja az Azure erőforrás-használat.</span><span class="sxs-lookup"><span data-stu-id="70b9d-199">App Service plans give you the flexibility to allocate specific apps to a set of resources and further optimize your Azure resource utilization.</span></span> <span data-ttu-id="70b9d-200">Ezzel a módszerrel pénzt takaríthatnak meg a tesztelési környezetben, a kívánt is megoszthatja a csomag több alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="70b9d-200">This way, if you want to save money on your testing environment, you can share a plan across multiple apps.</span></span> <span data-ttu-id="70b9d-201">Több régiók és tervek skálázással azt az éles környezetben is kihasználhatja átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="70b9d-201">You can also maximize throughput for your production environment by scaling it across multiple regions and plans.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="70b9d-202">A változások</span><span class="sxs-lookup"><span data-stu-id="70b9d-202">What's changed</span></span>

- <span data-ttu-id="70b9d-203">A módosítás a websites szolgáltatásról az App Service-re, váltásról: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="70b9d-203">For a guide to the change from Websites to App Service, see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
