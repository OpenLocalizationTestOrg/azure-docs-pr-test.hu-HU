---
title: "aaaAzure App Service díjcsomagjainak részletes áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: b384790d9e69b234ca69ac591164c48a4b6ed210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a><span data-ttu-id="00643-104">Az Azure App Service díjcsomagjainak részletes áttekintése</span><span class="sxs-lookup"><span data-stu-id="00643-104">Azure App Service plans in-depth overview</span></span>

<span data-ttu-id="00643-105">App Service-csomagokról képviselő használt fizikai erőforrások toohost hello gyűjteményét az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="00643-105">App Service plans represent hello collection of physical resources used toohost your apps.</span></span>

<span data-ttu-id="00643-106">Az App Service-csomagok a következőket határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="00643-106">App Service plans define:</span></span>

- <span data-ttu-id="00643-107">A régióban (USA nyugati régiója, USA keleti régiója, stb.)</span><span class="sxs-lookup"><span data-stu-id="00643-107">Region (West US, East US, etc.)</span></span>
- <span data-ttu-id="00643-108">Méretezési száma (egy, két, három példányt, stb.)</span><span class="sxs-lookup"><span data-stu-id="00643-108">Scale count (one, two, three instances, etc.)</span></span>
- <span data-ttu-id="00643-109">Példányméretének (Small, Medium, Large)</span><span class="sxs-lookup"><span data-stu-id="00643-109">Instance size (Small, Medium, Large)</span></span>
- <span data-ttu-id="00643-110">Termékváltozat (Ingyenes, Közös, Alapszintű, Standard, Prémium)</span><span class="sxs-lookup"><span data-stu-id="00643-110">SKU (Free, Shared, Basic, Standard, Premium)</span></span>

<span data-ttu-id="00643-111">Web Apps, Mobile Apps, az API Apps, függvény alkalmazások (vagy funkciók), a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) összes futtassa az App Service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="00643-111">Web Apps, Mobile Apps, API Apps, Function Apps (or Functions), in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) all run in an App Service plan.</span></span>  <span data-ttu-id="00643-112">Alkalmazások hello ugyanazt az előfizetést, és régió megoszthatja az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="00643-112">Apps in hello same subscription, and region can share an App Service plan.</span></span> 

<span data-ttu-id="00643-113">Minden alkalmazás hozzárendelt tooan **App Service-csomag** meghatározott hello erőforrások megosztása.</span><span class="sxs-lookup"><span data-stu-id="00643-113">All applications assigned tooan **App Service plan** share hello resources defined by it.</span></span> <span data-ttu-id="00643-114">A megosztás menti a pénz, esetén több alkalmazást az önálló App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="00643-114">This sharing saves money when hosting multiple apps in a single App Service plan.</span></span>

<span data-ttu-id="00643-115">A **App Service-csomag** a méretezhető **szabad** és **megosztott** termékváltozatok túl**alapvető**, **szabványos**, és **Prémium** felkínálva termékváltozatok toomore erőforrások és szolgáltatások mentén hello eléréséhez módon.</span><span class="sxs-lookup"><span data-stu-id="00643-115">Your **App Service plan** can scale from **Free** and **Shared** SKUs too**Basic**, **Standard**, and **Premium** SKUs giving you access toomore resources and features along hello way.</span></span>

<span data-ttu-id="00643-116">Ha az App Service-csomag túl**alapvető** SKU vagy újabb verzióját, majd megadhatja a hello **mérete** , a méretezés hello virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="00643-116">If your App Service plan is set too**Basic** SKU or higher, then you can control hello **size** and scale count of hello VMs.</span></span>

<span data-ttu-id="00643-117">Például ha a csomag konfigurált toouse két "kicsi" példányok hello szabványos szolgáltatási rétegben, terv társított minden alkalmazást futtatnak mindkét esetben.</span><span class="sxs-lookup"><span data-stu-id="00643-117">For example, if your plan is configured toouse two "small" instances in hello standard service tier, all apps that are associated with that plan run on both instances.</span></span> <span data-ttu-id="00643-118">Alkalmazások hozzáférési toohello szabványos réteg funkciókat is.</span><span class="sxs-lookup"><span data-stu-id="00643-118">Apps also have access toohello standard service tier features.</span></span> <span data-ttu-id="00643-119">Alkalmazások futnak terv példányok teljes körűen felügyelt, és magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="00643-119">Plan instances on which apps are running are fully managed and highly available.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00643-120">Hello **SKU** és **méretezési** a hello App Service csomag határozza meg, hogy hello költségek és nincs hello azt az üzemeltetett alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="00643-120">hello **SKU** and **Scale** of hello App Service plan determines hello cost and not hello number of apps hosted in it.</span></span>

<span data-ttu-id="00643-121">Ez a cikk ismerteti, hello főbb jellemzőit, például a réteg és az App Service-csomag mérete, és hogyan az alkalmazások kezelését közben játékba lépnek.</span><span class="sxs-lookup"><span data-stu-id="00643-121">This article explores hello key characteristics, such as tier and scale, of an App Service plan and how they come into play while managing your apps.</span></span>

## <a name="apps-and-app-service-plans"></a><span data-ttu-id="00643-122">Alkalmazások és az App Service-csomagokról</span><span class="sxs-lookup"><span data-stu-id="00643-122">Apps and App Service plans</span></span>

<span data-ttu-id="00643-123">Lehet, hogy az App Service alkalmazás társítva van egy adott időpontban csak egy App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="00643-123">An app in App Service can be associated with only one App Service plan at any given time.</span></span>

<span data-ttu-id="00643-124">Alkalmazások és csomagok is tartalmaznak egy **erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="00643-124">Both apps and plans are contained in a **resource group**.</span></span> <span data-ttu-id="00643-125">Erőforráscsoport hello életciklus határ minden erőforráshoz, amely hozzá lesz.</span><span class="sxs-lookup"><span data-stu-id="00643-125">A resource group serves as hello lifecycle boundary for every resource that's within it.</span></span> <span data-ttu-id="00643-126">Használható erőforrás-csoportok toomanage összes hello darab kérelem együtt.</span><span class="sxs-lookup"><span data-stu-id="00643-126">You can use resource groups toomanage all hello pieces of an application together.</span></span>

<span data-ttu-id="00643-127">Mivel egyetlen erőforráscsoportként működnek lehet több App Service-csomagokról, foglalhatja különböző alkalmazások toodifferent fizikai erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="00643-127">Because a single resource group can have multiple App Service plans, you can allocate different apps toodifferent physical resources.</span></span>

<span data-ttu-id="00643-128">Például elkülönítheti a erőforrások fejlesztői, tesztelési és éles környezetek között.</span><span class="sxs-lookup"><span data-stu-id="00643-128">For example, you can separate resources among dev, test, and production environments.</span></span> <span data-ttu-id="00643-129">Éles üzemi pontjának és fejlesztési és tesztelési célú külön környezeteket rendelkező lehetővé teszi az erőforrások elkülönítése.</span><span class="sxs-lookup"><span data-stu-id="00643-129">Having separate environments for production and dev/test lets you isolate resources.</span></span> <span data-ttu-id="00643-130">Ezzel a módszerrel szemben az alkalmazások új verziójának tesztelése betöltése nem "versenyeznek" az hello ugyanaz az erőforrásokhoz, mint az éles alkalmazásokat, amelyek valós ügyfelek szolgál.</span><span class="sxs-lookup"><span data-stu-id="00643-130">In this way, load testing against a new version of your apps does not compete for hello same resources as your production apps, which are serving real customers.</span></span>

<span data-ttu-id="00643-131">Ha több csomagokról egyetlen erőforráscsoportként működnek, megadhatja a földrajzi régió átfogó alkalmazás is.</span><span class="sxs-lookup"><span data-stu-id="00643-131">When you have multiple plans in a single resource group, you can also define an application that spans geographical regions.</span></span>

<span data-ttu-id="00643-132">Például egy magas rendelkezésre állású két régiókban futó alkalmazást tartalmaz legalább két terveket, minden egyes régió egy és minden terv társított egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="00643-132">For example, a highly available app running in two regions includes at least two plans, one for each region, and one app associated with each plan.</span></span> <span data-ttu-id="00643-133">Ilyen esetben minden hello másolatot hello alkalmazás egyetlen erőforráscsoportként működnek majd tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="00643-133">In such a situation, all hello copies of hello app are then contained in a single resource group.</span></span> <span data-ttu-id="00643-134">Több csomagokban és több alkalmazás erőforráscsoport rendelkező teszi könnyen toomanage, a vezérlési és az alkalmazás hello hello állapotának megtekintése.</span><span class="sxs-lookup"><span data-stu-id="00643-134">Having a resource group with multiple plans and multiple apps makes it easy toomanage, control, and view hello health of hello application.</span></span>

## <a name="create-an-app-service-plan-or-use-existing-one"></a><span data-ttu-id="00643-135">Az App Service-csomag létrehozása vagy meglévő ütemezés használata</span><span class="sxs-lookup"><span data-stu-id="00643-135">Create an App Service plan or use existing one</span></span>

<span data-ttu-id="00643-136">Amikor létrehoz egy alkalmazást, érdemes egy erőforráscsoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="00643-136">When you create an app, you should consider creating a resource group.</span></span> <span data-ttu-id="00643-137">Hello ugyanakkor, ha az alkalmazás egy nagyobb alkalmazás egy összetevőjét hozza létre azt az adott nagyobb alkalmazáshoz lefoglalt hello erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="00643-137">On hello other hand, if this app is a component for a larger application, create it within hello resource group that's allocated for that larger application.</span></span>

<span data-ttu-id="00643-138">Hogy hello app egy teljesen új alkalmazás vagy nagyobb egy részét, dönthet úgy toouse egy meglévő terv toohost, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="00643-138">Whether hello app is an altogether new application or part of a larger one, you can choose toouse an existing plan toohost it or create a new one.</span></span> <span data-ttu-id="00643-139">Ez a döntés nagyobb kapacitást és várható terhelési adott esetben.</span><span class="sxs-lookup"><span data-stu-id="00643-139">This decision is more a question of capacity and expected load.</span></span>

<span data-ttu-id="00643-140">Javasoljuk, hogy az alkalmazás egy új App Service-be elkülönítése ütemezésének:</span><span class="sxs-lookup"><span data-stu-id="00643-140">We recommend isolating your app into a new App Service plan when:</span></span>

- <span data-ttu-id="00643-141">Alkalmazás erőforrás-igényes áll.</span><span class="sxs-lookup"><span data-stu-id="00643-141">App is resource-intensive.</span></span>
- <span data-ttu-id="00643-142">Az alkalmazás rendelkezik a hello különböző méretezési tényező más egy meglévő csomagban üzemeltetett alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="00643-142">App has different scaling factors from hello other apps hosted in an existing plan.</span></span>
- <span data-ttu-id="00643-143">Erőforrás más földrajzi régióban kell alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="00643-143">App needs resource in a different geographical region.</span></span>

<span data-ttu-id="00643-144">Ezzel a módszerrel lehet új készletét erőforrásokat lefoglalni az alkalmazás és az alkalmazások nagyobb ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="00643-144">This way you can allocate a new set of resources for your app and gain greater control of your apps.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="00643-145">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="00643-145">Create an App Service plan</span></span>

> [!TIP]
> <span data-ttu-id="00643-146">Ha az App Service-környezetek, áttekintheti a hello dokumentáció adott tooApp Service-környezetek innen: [egy App Service Environment-környezetben az App Service-csomag létrehozása](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span><span class="sxs-lookup"><span data-stu-id="00643-146">If you have an App Service Environment, you can review hello documentation specific tooApp Service Environments here: [Create an App Service plan in an App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span></span>

<span data-ttu-id="00643-147">Üres App Service-csomagot is létrehozhat, az App Service-csomag tallózási élménynek hello vagy alkalmazás létrehozásának részeként.</span><span class="sxs-lookup"><span data-stu-id="00643-147">You can create an empty App Service plan from hello App Service plan browse experience or as part of app creation.</span></span>

<span data-ttu-id="00643-148">A hello [Azure-portálon](https://portal.azure.com), kattintson a **új** > **Web + mobil**, majd válassza ki **Web App** vagy más App Service alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="00643-148">In hello [Azure portal](https://portal.azure.com), click **New** > **Web + mobile**, and then select **Web App** or other App Service app kind.</span></span>

![Alkalmazás létrehozása az Azure-portálon hello.][createWebApp]

<span data-ttu-id="00643-150">Ezután válassza ki vagy hello App Service-csomag hello új alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="00643-150">You can then select or create hello App Service plan for hello new app.</span></span>

 ![Az App Service-csomag létrehozása.][createASP]

<span data-ttu-id="00643-152">toocreate App Service-csomagot, kattintson a **[+] hozzon létre új**, típus hello **App Service-csomag** nevet, és válasszon egy megfelelő **hely**.</span><span class="sxs-lookup"><span data-stu-id="00643-152">toocreate an App Service plan, click **[+] Create New**, type hello **App Service plan** name, and then select an appropriate **Location**.</span></span> <span data-ttu-id="00643-153">Kattintson a **tarifacsomag**, majd válassza ki egy megfelelő tarifacsomagot hello szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="00643-153">Click **Pricing tier**, and then select an appropriate pricing tier for hello service.</span></span> <span data-ttu-id="00643-154">Válassza ki **összes** több, mint az beállítások árképzési tooview **szabad** és **megosztott**.</span><span class="sxs-lookup"><span data-stu-id="00643-154">Select **View all** tooview more pricing options, such as **Free** and **Shared**.</span></span> <span data-ttu-id="00643-155">Hello tarifacsomag kiválasztása után kattintson a hello **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="00643-155">After you have selected hello pricing tier, click hello **Select** button.</span></span>

## <a name="move-an-app-tooa-different-app-service-plan"></a><span data-ttu-id="00643-156">Az alkalmazás tooa másik App Service-csomag áthelyezése</span><span class="sxs-lookup"><span data-stu-id="00643-156">Move an app tooa different App Service plan</span></span>

<span data-ttu-id="00643-157">Az alkalmazás tooa másik App Service-csomag hello mozgathatja [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="00643-157">You can move an app tooa different App Service plan in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="00643-158">Alkalmazások között áthelyezheti tervek mindaddig, amíg hello csomagok vannak hello ugyanazt az erőforráscsoportot és földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="00643-158">You can move apps between plans as long as hello plans are in hello same resource group and geographical region.</span></span>

<span data-ttu-id="00643-159">az alkalmazás tooanother csomag toomove:</span><span class="sxs-lookup"><span data-stu-id="00643-159">toomove an app tooanother plan:</span></span>

- <span data-ttu-id="00643-160">Keresse meg, hogy szeretné-e toomove toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="00643-160">Navigate toohello app that you want toomove.</span></span>
- <span data-ttu-id="00643-161">A hello **menü**, keressen a hello **App Service-csomag** szakasz.</span><span class="sxs-lookup"><span data-stu-id="00643-161">In hello **Menu**, look for hello **App Service Plan** section.</span></span>
- <span data-ttu-id="00643-162">Válassza ki **módosítás App Service-csomag** toostart hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="00643-162">Select **Change App Service plan** toostart hello process.</span></span>

<span data-ttu-id="00643-163">**App Service-csomag módosítása** megnyílik hello **App Service-csomag** választó.</span><span class="sxs-lookup"><span data-stu-id="00643-163">**Change App Service plan** opens hello **App Service plan** selector.</span></span> <span data-ttu-id="00643-164">Ezen a ponton választhatja ki egy meglévő terv toomove be ezt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="00643-164">At this point, you can pick an existing plan toomove this app into.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00643-165">a rendszer hello a következő feltételek alapján szűri hello jelölje be az alkalmazásszolgáltatási csomag felhasználói felületén:</span><span class="sxs-lookup"><span data-stu-id="00643-165">hello select App Service plan UI is filtered by hello following criteria:</span></span>
> - <span data-ttu-id="00643-166">Létezik hello belül azonos erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="00643-166">Exists within hello same Resource Group</span></span>
> - <span data-ttu-id="00643-167">Hello létezik azonos földrajzi régióban</span><span class="sxs-lookup"><span data-stu-id="00643-167">Exists in hello same Geographical Region</span></span>
> - <span data-ttu-id="00643-168">Létezik hello belül azonos webtárhely</span><span class="sxs-lookup"><span data-stu-id="00643-168">Exists within hello same Webspace</span></span>
>
> <span data-ttu-id="00643-169">Egy webtárhely egy logikai szerkezet belül App Service, amely meghatározza a kiszolgáló erőforrások csoportosítása.</span><span class="sxs-lookup"><span data-stu-id="00643-169">A Webspace is a logical construct within App Service which defines a grouping of server resources.</span></span> <span data-ttu-id="00643-170">Egy földrajzi régiót (például az USA nyugati régiója) sorrendben tooallocate ügyfelek használja az App Service számos Webspaces tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="00643-170">A Geographical region (such as West US) contains many Webspaces in order tooallocate customers using App Service.</span></span> <span data-ttu-id="00643-171">Jelenleg az App Service-erőforrások nem képes toobe Webspaces között.</span><span class="sxs-lookup"><span data-stu-id="00643-171">Currently, App Service resources aren’t able toobe moved between Webspaces.</span></span>
>

![App Service-csomag választó.][change]

<span data-ttu-id="00643-173">Minden egyes tervnek a saját IP-címek.</span><span class="sxs-lookup"><span data-stu-id="00643-173">Each plan has its own pricing tier.</span></span> <span data-ttu-id="00643-174">Egy hely áthelyezése a ingyenes szint tooa Standard szintű, például lehetővé teszi a tooit toouse hello szolgáltatásokat és erőforrásokat a hello Standard csomagra hozzárendelt összes alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="00643-174">For example, moving a site from a Free tier tooa Standard tier, enables all apps assigned tooit toouse hello features and resources of hello Standard tier.</span></span>

## <a name="clone-an-app-tooa-different-app-service-plan"></a><span data-ttu-id="00643-175">Az alkalmazás tooa másik App Service-csomag klónozása</span><span class="sxs-lookup"><span data-stu-id="00643-175">Clone an app tooa different App Service plan</span></span>

<span data-ttu-id="00643-176">Ha azt szeretné, hogy toomove hello app tooa más régióban, egy másik app-e a Klónozás.</span><span class="sxs-lookup"><span data-stu-id="00643-176">If you want toomove hello app tooa different region, one alternative is app cloning.</span></span> <span data-ttu-id="00643-177">A Klónozás teszi az alkalmazás másolatát egy új vagy meglévő App Service-csomag bármely régióban.</span><span class="sxs-lookup"><span data-stu-id="00643-177">Cloning makes a copy of your app in a new or existing App Service plan in any region.</span></span>

<span data-ttu-id="00643-178">Található **Klónozott alkalmazás** a hello **Fejlesztőeszközök** hello menü részét.</span><span class="sxs-lookup"><span data-stu-id="00643-178">You can find **Clone App** in hello **Development Tools** section of hello menu.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00643-179">A Klónozás rendelkezik néhány címen olvashat [Azure App Service-alkalmazást az Azure-portál használatával Klónozás](../app-service-web/app-service-web-app-cloning-portal.md).</span><span class="sxs-lookup"><span data-stu-id="00643-179">Cloning has some limitations that you can read about at [Azure App Service App cloning using Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).</span></span>

## <a name="scale-an-app-service-plan"></a><span data-ttu-id="00643-180">Az App Service-csomag vertikális</span><span class="sxs-lookup"><span data-stu-id="00643-180">Scale an App Service plan</span></span>

<span data-ttu-id="00643-181">Nincsenek három módon tooscale terv:</span><span class="sxs-lookup"><span data-stu-id="00643-181">There are three ways tooscale a plan:</span></span>

- <span data-ttu-id="00643-182">**Hello terv tartozó IP-címek**.</span><span class="sxs-lookup"><span data-stu-id="00643-182">**Change hello plan’s pricing tier**.</span></span> <span data-ttu-id="00643-183">Egy alapszintű hello réteg terv konvertált tooStandard, és minden alkalmazás hozzárendelt tooit toouse hello funkcióit hello Standard csomagra.</span><span class="sxs-lookup"><span data-stu-id="00643-183">A plan in hello Basic tier can be converted tooStandard, and all apps assigned tooit toouse hello features of hello Standard tier.</span></span>
- <span data-ttu-id="00643-184">**Hello csomag példányméretének módosítása**.</span><span class="sxs-lookup"><span data-stu-id="00643-184">**Change hello plan’s instance size**.</span></span> <span data-ttu-id="00643-185">Tegyük fel a terv hello alapszintű rétegben, amely használja a kisméretű példányok módosított toouse nagy példányok is lehet.</span><span class="sxs-lookup"><span data-stu-id="00643-185">As an example, a plan in hello Basic tier that uses small instances can be changed toouse large instances.</span></span> <span data-ttu-id="00643-186">Minden alkalmazás társított terv most hello további memória és a példány mérete nagyobb ajánlatok hello Processzor-erőforrások használhatja.</span><span class="sxs-lookup"><span data-stu-id="00643-186">All apps that are associated with that plan now can use hello additional memory and CPU resources that hello larger instance size offers.</span></span>
- <span data-ttu-id="00643-187">**Hello terv példányszám módosítása**.</span><span class="sxs-lookup"><span data-stu-id="00643-187">**Change hello plan’s instance count**.</span></span> <span data-ttu-id="00643-188">Például a Standard csomag kimenő toothree példányok méretezett lehet méretezett too10 példányok.</span><span class="sxs-lookup"><span data-stu-id="00643-188">For example, a Standard plan that's scaled out toothree instances can be scaled too10 instances.</span></span> <span data-ttu-id="00643-189">A prémium tervének kiterjeszthető too20 példányok (tulajdonos tooavailability).</span><span class="sxs-lookup"><span data-stu-id="00643-189">A Premium plan can be scaled out too20 instances (subject tooavailability).</span></span> <span data-ttu-id="00643-190">Minden alkalmazás társított terv most hello további memória és a példányok száma nagyobb ajánlatok hello Processzor-erőforrások használhatja.</span><span class="sxs-lookup"><span data-stu-id="00643-190">All apps that are associated with that plan now can use hello additional memory and CPU resources that hello larger instance count offers.</span></span>

<span data-ttu-id="00643-191">Módosíthatja az árképzési szint és a példány mérete hello kattintva hello **vertikális Felskálázás** elem hello alkalmazás vagy a hello App Service-csomag beállításai alatt.</span><span class="sxs-lookup"><span data-stu-id="00643-191">You can change hello pricing tier and instance size by clicking hello **Scale Up** item under settings for either hello app or hello App Service plan.</span></span> <span data-ttu-id="00643-192">Módosítások toohello App Service-csomag alkalmazása, és hatással rajta futó összes alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="00643-192">Changes apply toohello App Service plan and affect all apps that it hosts.</span></span>

 ![Állítsa be az értékeket tooscale be az alkalmazást.][pricingtier]

## <a name="app-service-plan-cleanup"></a><span data-ttu-id="00643-194">Az alkalmazásszolgáltatási csomag karbantartása</span><span class="sxs-lookup"><span data-stu-id="00643-194">App Service plan cleanup</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00643-195">**App Service-csomagok** , amelyek nem alkalmazásokkal kapcsolatos toothem továbbra is függő díj terheli, mivel azok tooreserve hello számítási kapacitás.</span><span class="sxs-lookup"><span data-stu-id="00643-195">**App Service plans** that have no apps associated toothem still incur charges since they continue tooreserve hello compute capacity.</span></span>

<span data-ttu-id="00643-196">tooavoid váratlan költségek, a törölt hello utolsó alkalmazás az App Service-csomag tárolva hello eredményül kapott üres App Service-csomag is törlődik.</span><span class="sxs-lookup"><span data-stu-id="00643-196">tooavoid unexpected charges, when hello last app hosted in an App Service plan is deleted, hello resulting empty App Service plan is also deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="00643-197">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="00643-197">Summary</span></span>

<span data-ttu-id="00643-198">App Service-csomagokról funkciók és kapacitás szempontjából lehet megosztani az alkalmazások csoportját képviselik.</span><span class="sxs-lookup"><span data-stu-id="00643-198">App Service plans represent a set of features and capacity that you can share across your apps.</span></span> <span data-ttu-id="00643-199">App Service-csomagok biztosít rugalmasságot tooallocate meghatározott alkalmazások tooa erőforráskészlethez hello és tovább optimalizálhatja az Azure erőforrás-használat.</span><span class="sxs-lookup"><span data-stu-id="00643-199">App Service plans give you hello flexibility tooallocate specific apps tooa set of resources and further optimize your Azure resource utilization.</span></span> <span data-ttu-id="00643-200">Így ha toosave pénzt takaríthat meg a tesztkörnyezet megoszthatja a tervet az több alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="00643-200">This way, if you want toosave money on your testing environment, you can share a plan across multiple apps.</span></span> <span data-ttu-id="00643-201">Több régiók és tervek skálázással azt az éles környezetben is kihasználhatja átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="00643-201">You can also maximize throughput for your production environment by scaling it across multiple regions and plans.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="00643-202">A változások</span><span class="sxs-lookup"><span data-stu-id="00643-202">What's changed</span></span>

- <span data-ttu-id="00643-203">Webhelyek tooApp szolgáltatás útmutató toohello módosítást, lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="00643-203">For a guide toohello change from Websites tooApp Service, see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
