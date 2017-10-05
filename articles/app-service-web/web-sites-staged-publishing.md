---
title: "Átmeneti környezet az Azure App Service web Apps beállítása |} Microsoft Docs"
description: "Ismerje meg, előkészített közzététele az Azure App Service web Apps használatával."
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: ca27c55eaaceb3109b1450c550330dfc416fdf55
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a><span data-ttu-id="6dca5-103">Átmeneti környezet az Azure App Service beállítása</span><span class="sxs-lookup"><span data-stu-id="6dca5-103">Set up staging environments in Azure App Service</span></span>
<a name="Overview"></a>

<span data-ttu-id="6dca5-104">A webalkalmazás, webes alkalmazás a Linux, mobil háttér és API-alkalmazás telepítésekor [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), telepíthet egy külön üzembe helyezési pont helyett az alapértelmezett éles tárolóhelyre futtatáskor a **szabványos** vagy **prémium** App Service-csomag mód.</span><span class="sxs-lookup"><span data-stu-id="6dca5-104">When you deploy your web app, web app on Linux, mobile back end, and API app to [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), you can deploy to a separate deployment slot instead of the default production slot when running in the **Standard** or **Premium** App Service plan mode.</span></span> <span data-ttu-id="6dca5-105">Üzembe helyezési saját állomásnevek ténylegesen élő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="6dca5-105">Deployment slots are actually live apps with their own hostnames.</span></span> <span data-ttu-id="6dca5-106">Alkalmazás tartalmát és konfigurációk elemek lecserélhető között két üzembe helyezési, beleértve az éles webalkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="6dca5-106">App content and configurations elements can be swapped between two deployment slots, including the production slot.</span></span> <span data-ttu-id="6dca5-107">A telepített környezet tárolóhelye az alkalmazás telepítése az alábbi előnyökkel jár:</span><span class="sxs-lookup"><span data-stu-id="6dca5-107">Deploying your application to a deployment slot has the following benefits:</span></span>

* <span data-ttu-id="6dca5-108">Mielőtt az éles webalkalmazásra a csere, ellenőrizheti egy átmeneti üzembe helyezési tárhelyet az alkalmazások változásairól.</span><span class="sxs-lookup"><span data-stu-id="6dca5-108">You can validate app changes in a staging deployment slot before swapping it with the production slot.</span></span>
* <span data-ttu-id="6dca5-109">Központilag telepíthetők az alkalmazások a tárhely először és csere az éles környezetben biztosítja, hogy a összes példányát a tárolóhely Mielőtt éles környezetben felcserélés folyamatban vannak tárolóhelyspecifikus.</span><span class="sxs-lookup"><span data-stu-id="6dca5-109">Deploying an app to a slot first and swapping it into production ensures that all instances of the slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="6dca5-110">Ez megszünteti állásidő, az alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="6dca5-110">This eliminates downtime when you deploy your app.</span></span> <span data-ttu-id="6dca5-111">A forgalom átirányítása zökkenőmentes-kérelmek nem dobja swap műveletek miatt.</span><span class="sxs-lookup"><span data-stu-id="6dca5-111">The traffic redirection is seamless, and no requests are dropped as a result of swap operations.</span></span> <span data-ttu-id="6dca5-112">A teljes munkafolyamat konfigurálásával automatizálható [automatikus felcserélés](#Auto-Swap) , ha nincs szükség a előtti swap érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="6dca5-112">This entire workflow can be automated by configuring [Auto Swap](#Auto-Swap) when pre-swap validation is not needed.</span></span>
* <span data-ttu-id="6dca5-113">Egy felcserélés után a tárolóhely korábban előkészített alkalmazással most már rendelkezik az előző éles alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="6dca5-113">After a swap, the slot with previously staged app now has the previous production app.</span></span> <span data-ttu-id="6dca5-114">Ha a módosításokat, azokat az éles tárolóhelyre felcserélve nem a várt módon, közvetlenül a "utolsó ismert helyes"nevű hely eléréséhez a azonos virtuális végezhet vissza.</span><span class="sxs-lookup"><span data-stu-id="6dca5-114">If the changes swapped into the production slot are not as you expected, you can perform the same swap immediately to get your "last known good site" back.</span></span>

<span data-ttu-id="6dca5-115">Minden App Service-csomag mód támogatja az üzembe helyezési pontok különböző számú.</span><span class="sxs-lookup"><span data-stu-id="6dca5-115">Each App Service plan mode supports a different number of deployment slots.</span></span> <span data-ttu-id="6dca5-116">Tárolóhely száma megállapítása: az alkalmazás mód támogatja, lásd: [App Service szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="6dca5-116">To find out the number of slots your app's mode supports, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

* <span data-ttu-id="6dca5-117">Ha az alkalmazás több tárolóhelye van, a mód nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="6dca5-117">When your app has multiple slots, you cannot change the mode.</span></span>
* <span data-ttu-id="6dca5-118">Skálázás nem érhető el a nem végleges pont felcserélése.</span><span class="sxs-lookup"><span data-stu-id="6dca5-118">Scaling is not available for non-production slots.</span></span>
* <span data-ttu-id="6dca5-119">Csatolt erőforrás-kezelés nem végleges pont felcserélése nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6dca5-119">Linked resource management is not supported for non-production slots.</span></span> <span data-ttu-id="6dca5-120">Az a [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) csak, úgy kerülheti el a lehetséges hatásairól az éles tárhely ideiglenesen a nem éles tárolóhelyre áthelyezése egy másik App Service csomag módot.</span><span class="sxs-lookup"><span data-stu-id="6dca5-120">In the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) only, you can avoid this potential impact on a production slot by temporarily moving the non-production slot to a different App Service plan mode.</span></span> <span data-ttu-id="6dca5-121">Vegye figyelembe, hogy az nem éles tárhely kell ismét közös azonos módot az éles tárolóhelyre ahhoz, hogy a két felcserélése is.</span><span class="sxs-lookup"><span data-stu-id="6dca5-121">Note that the non-production slot must once again share the same mode with the production slot before you can swap the two slots.</span></span>

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a><span data-ttu-id="6dca5-122">Adja hozzá egy üzembe helyezési tárhelyet</span><span class="sxs-lookup"><span data-stu-id="6dca5-122">Add a deployment slot</span></span>
<span data-ttu-id="6dca5-123">Futnia kell az alkalmazást a **szabványos** vagy **prémium** ahhoz, hogy engedélyezheti a több üzembe helyezési mód.</span><span class="sxs-lookup"><span data-stu-id="6dca5-123">The app must be running in the **Standard** or **Premium** mode in order for you to enable multiple deployment slots.</span></span>

1. <span data-ttu-id="6dca5-124">Az a [Azure Portal](https://portal.azure.com/), nyissa meg az alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="6dca5-124">In the [Azure Portal](https://portal.azure.com/), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="6dca5-125">Válassza ki a **üzembe helyezési** lehetőséget, majd kattintson az **tárhely felvétele**.</span><span class="sxs-lookup"><span data-stu-id="6dca5-125">Choose the **Deployment slots** option, then click **Add Slot**.</span></span>
   
    ![Adja hozzá egy új üzembe helyezési tárhelyet][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > <span data-ttu-id="6dca5-127">Ha az alkalmazás már nem része a **szabványos** vagy **prémium** mód, kapni fog egy üzenetet, a támogatott módok előkészített közzététel engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="6dca5-127">If the app is not already in the **Standard** or **Premium** mode, you will receive a message indicating the supported modes for enabling staged publishing.</span></span> <span data-ttu-id="6dca5-128">Ezen a ponton rendelkezik választhatja **frissítése** , és keresse meg a **méretezési** fülre az alkalmazás a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="6dca5-128">At this point, you have the option to select **Upgrade** and navigate to the **Scale** tab of your app before continuing.</span></span>
   > 
   > 
3. <span data-ttu-id="6dca5-129">Az a **tárhely hozzáadása** panelen nevezze el a tárhely, és válassza ki, hogy egy másik meglévő üzembe helyezési pont az alkalmazáskonfiguráció klónozását.</span><span class="sxs-lookup"><span data-stu-id="6dca5-129">In the **Add a slot** blade, give the slot a name, and select whether to clone app configuration from another existing deployment slot.</span></span> <span data-ttu-id="6dca5-130">Kattintson a pipa jelre a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="6dca5-130">Click the check mark to continue.</span></span>
   
    ![Konfigurációs forrás][ConfigurationSource1]
   
    <span data-ttu-id="6dca5-132">Először ad hozzá egy tárolóhely, csak két lehetősége van: az alapértelmezett tárolóhelyről, éles környezetben, vagy egyáltalán nem klónozott konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="6dca5-132">The first time you add a slot, you will only have two choices: clone configuration from the default slot in production or not at all.</span></span>
    <span data-ttu-id="6dca5-133">A létrehozást követően több üzembe helyezési ponti, lesz klónozza a nem az éles tárhely beállításait:</span><span class="sxs-lookup"><span data-stu-id="6dca5-133">After you have created several slots, you will be able to clone configuration from a slot other than the one in production:</span></span>
   
    ![Konfigurációs forrás][MultipleConfigurationSources]
4. <span data-ttu-id="6dca5-135">Az alkalmazás erőforrás paneljén kattintson **üzembe helyezési**, majd kattintson az erőforráspanelről, hogy a tárhely, metrikákat és csakúgy, mint bármely más alkalmazás-konfigurációs vannak beállítva egy üzembe helyezési tárhelyet.</span><span class="sxs-lookup"><span data-stu-id="6dca5-135">In your app's resource blade, click  **Deployment slots**, then click a deployment slot to open that slot's resource blade, with a set of metrics and configuration just like any other app.</span></span> <span data-ttu-id="6dca5-136">A tárhely neve jelezve, az üzembe helyezési pont megjelenítő a panel tetején látható.</span><span class="sxs-lookup"><span data-stu-id="6dca5-136">The name of the slot is shown at the top of the blade to remind you that you are viewing the deployment slot.</span></span>
   
    ![Központi telepítés tárolóhely cím][StagingTitle]
5. <span data-ttu-id="6dca5-138">Kattintson az alkalmazás URL-CÍMÉT a tárolóhely panelen.</span><span class="sxs-lookup"><span data-stu-id="6dca5-138">Click the app URL in the slot's blade.</span></span> <span data-ttu-id="6dca5-139">Figyelje meg a telepített környezet tárolóhelye saját állomásnév és is egy élő app.</span><span class="sxs-lookup"><span data-stu-id="6dca5-139">Notice the deployment slot has its own hostname and is also a live app.</span></span> <span data-ttu-id="6dca5-140">Az üzembe helyezési pont nyilvános hozzáférés korlátozására, lásd: [App Service Web App – webes hozzáférés letiltása nem éles üzembe helyezési](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span><span class="sxs-lookup"><span data-stu-id="6dca5-140">To limit public access to the deployment slot, see [App Service Web App – block web access to non-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span></span>

<span data-ttu-id="6dca5-141">Nincs tartalom központi telepítési tárolóhely létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="6dca5-141">There is no content after deployment slot creation.</span></span> <span data-ttu-id="6dca5-142">A tárolóhely telepítene egy másik tárház ágat, vagy egy teljesen más tárházba.</span><span class="sxs-lookup"><span data-stu-id="6dca5-142">You can deploy to the slot from a different repository branch, or an altogether different repository.</span></span> <span data-ttu-id="6dca5-143">A tárolóhely konfigurációs is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6dca5-143">You can also change the slot's configuration.</span></span> <span data-ttu-id="6dca5-144">A közzétételi profil vagy a központi telepítési társított hitelesítő adatokat a telepített környezet tárolóhelye tartalom frissítések használja.</span><span class="sxs-lookup"><span data-stu-id="6dca5-144">Use the publish profile or deployment credentials associated with the deployment slot for content updates.</span></span>  <span data-ttu-id="6dca5-145">Például végezheti [ezt a tárolóhelyet a git közzététele](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="6dca5-145">For example, you can [publish to this slot with git](app-service-deploy-local-git.md).</span></span>

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a><span data-ttu-id="6dca5-146">Az üzembe helyezési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="6dca5-146">Configuration for deployment slots</span></span>
<span data-ttu-id="6dca5-147">Klónozza a másik üzembe helyezési pont konfigurációja, a klónozott konfigurációs esetén szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="6dca5-147">When you clone configuration from another deployment slot, the cloned configuration is editable.</span></span> <span data-ttu-id="6dca5-148">Továbbá bizonyos konfigurációs elemek követi a tartalmat a lapozófájl-kapacitás (nem tárolóhely adott) keresztül közben más konfigurációs elemek ugyanaz a tárolóhely marad a lapozófájl-kapacitás (tárolóhely adott) után.</span><span class="sxs-lookup"><span data-stu-id="6dca5-148">Furthermore, some configuration elements will follow the content across a swap (not slot specific) while other configuration elements will stay in the same slot after a swap (slot specific).</span></span> <span data-ttu-id="6dca5-149">A következő listákban konfiguráció szerint változnak, ha Ön felcserélése megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="6dca5-149">The following lists show the configuration that will change when you swap slots.</span></span>

<span data-ttu-id="6dca5-150">**Beállítások, amelyek van cserélve**:</span><span class="sxs-lookup"><span data-stu-id="6dca5-150">**Settings that are swapped**:</span></span>

* <span data-ttu-id="6dca5-151">Általános beállítások - keretrendszer verziója, 32 vagy 64 bites, például webes szoftvercsatornák</span><span class="sxs-lookup"><span data-stu-id="6dca5-151">General settings - such as framework version, 32/64-bit, Web sockets</span></span>
* <span data-ttu-id="6dca5-152">Alkalmazásbeállítások (beállítható úgy, hogy a tárhely anyagot)</span><span class="sxs-lookup"><span data-stu-id="6dca5-152">App settings (can be configured to stick to a slot)</span></span>
* <span data-ttu-id="6dca5-153">Kapcsolati karakterláncok (beállítható úgy, hogy a tárhely anyagot)</span><span class="sxs-lookup"><span data-stu-id="6dca5-153">Connection strings (can be configured to stick to a slot)</span></span>
* <span data-ttu-id="6dca5-154">Kezelőleképezések</span><span class="sxs-lookup"><span data-stu-id="6dca5-154">Handler mappings</span></span>
* <span data-ttu-id="6dca5-155">Megfigyelési és diagnosztikai beállítások</span><span class="sxs-lookup"><span data-stu-id="6dca5-155">Monitoring and diagnostic settings</span></span>
* <span data-ttu-id="6dca5-156">Webjobs-feladatok tartalom</span><span class="sxs-lookup"><span data-stu-id="6dca5-156">WebJobs content</span></span>

<span data-ttu-id="6dca5-157">**Beállítások, amelyek nem van cserélve**:</span><span class="sxs-lookup"><span data-stu-id="6dca5-157">**Settings that are not swapped**:</span></span>

* <span data-ttu-id="6dca5-158">Közzétételi végpontok</span><span class="sxs-lookup"><span data-stu-id="6dca5-158">Publishing endpoints</span></span>
* <span data-ttu-id="6dca5-159">Egyéni tartománynevek</span><span class="sxs-lookup"><span data-stu-id="6dca5-159">Custom Domain Names</span></span>
* <span data-ttu-id="6dca5-160">SSL-tanúsítványok és kötések</span><span class="sxs-lookup"><span data-stu-id="6dca5-160">SSL certificates and bindings</span></span>
* <span data-ttu-id="6dca5-161">Skálázási beállításokat</span><span class="sxs-lookup"><span data-stu-id="6dca5-161">Scale settings</span></span>
* <span data-ttu-id="6dca5-162">Webjobs-feladatok bejegyzéstípusait</span><span class="sxs-lookup"><span data-stu-id="6dca5-162">WebJobs schedulers</span></span>

<span data-ttu-id="6dca5-163">Egy alkalmazás beállítás vagy a kapcsolati karakterlánc (nem cserélhető fel) tárhely anyagot konfigurálása, hozzáférés-a **Alkalmazásbeállítások** paneljén adott tárhely, majd válassza ki a **tárolóhely beállítás** mezőben a konfiguráció a tárolóhely kell odatapadjon elemei.</span><span class="sxs-lookup"><span data-stu-id="6dca5-163">To configure an app setting or connection string to stick to a slot (not swapped), access the **Application Settings** blade for a specific slot, then select the **Slot Setting** box for the configuration elements that should stick the slot.</span></span> <span data-ttu-id="6dca5-164">Vegye figyelembe, hogy jelölés egy konfigurációs elem, ha adott tárolóhely létrehozásáról szóló az elem nem cserélhető között az alkalmazáshoz kapcsolódó összes üzembe helyezési hatását.</span><span class="sxs-lookup"><span data-stu-id="6dca5-164">Note that marking a configuration element as slot specific has the effect of establishing that element as not swappable across all the deployment slots associated with the app.</span></span>

![Tárolóhely-beállítások][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a><span data-ttu-id="6dca5-166">Központi telepítés felcserélése</span><span class="sxs-lookup"><span data-stu-id="6dca5-166">Swap deployment slots</span></span> 
<span data-ttu-id="6dca5-167">Az üzembe helyezési kicserélheti a **áttekintése** vagy **üzembe helyezési** a app erőforráspanelen ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6dca5-167">You can swap deployment slots in the **Overview** or **Deployment slots** view of your app's resource blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6dca5-168">Mielőtt egy üzembe helyezési pont alkalmazás felcserélni a éles környezetben, győződjön meg arról, hogy minden adott nem tárolóhely-beállítások kívánt felcserélés célja, hogy vannak-e konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6dca5-168">Before you swap an app from a deployment slot into production, make sure that all non-slot specific settings are configured exactly as you want to have it in the swap target.</span></span>
> 
> 

1. <span data-ttu-id="6dca5-169">Központi telepítés felcserélése, kattintson a **Swap** gombra a parancssávon az alkalmazás vagy a telepített környezet tárolóhelye parancsra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="6dca5-169">To swap deployment slots, click the **Swap** button in the command bar of the app or in the command bar of a deployment slot.</span></span>
   
    ![Lapozófájl-kapacitás gomb][SwapButtonBar]

2. <span data-ttu-id="6dca5-171">Győződjön meg arról, hogy a lapozófájl-kapacitás forrás- és a lapozófájl-kapacitás cél megfelelően vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="6dca5-171">Make sure that the swap source and swap target are set properly.</span></span> <span data-ttu-id="6dca5-172">A felcserélés célja általában az éles webalkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="6dca5-172">Usually, the swap target is the production slot.</span></span> <span data-ttu-id="6dca5-173">Kattintson a **OK** elvégezni a műveletet.</span><span class="sxs-lookup"><span data-stu-id="6dca5-173">Click **OK** to complete the operation.</span></span> <span data-ttu-id="6dca5-174">A művelet befejezése után az üzembe helyezési rendelkezik lett cserélve.</span><span class="sxs-lookup"><span data-stu-id="6dca5-174">When the operation finishes, the deployment slots have been swapped.</span></span>

    ![Felcserélés befejezése](./media/web-sites-staged-publishing/SwapImmediately.png)

    <span data-ttu-id="6dca5-176">Az a **felcserélés előnézettel** típus felcserélése, lásd: [felcserélés előnézettel (több fázisban swap)](#Multi-Phase).</span><span class="sxs-lookup"><span data-stu-id="6dca5-176">For the **Swap with preview** swap type, see [Swap with preview (multi-phase swap)](#Multi-Phase).</span></span>  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a><span data-ttu-id="6dca5-177">A felcserélés előnézettel (több fázisban swap)</span><span class="sxs-lookup"><span data-stu-id="6dca5-177">Swap with preview (multi-phase swap)</span></span>

<span data-ttu-id="6dca5-178">A felcserélés előnézettel, vagy több fázisban swap leegyszerűsíti a tárolóhely konfigurációs elemeket, például kapcsolati karakterláncok érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="6dca5-178">Swap with preview, or multi-phase swap, simplify validation of slot-specific configuration elements, such as connection strings.</span></span>
<span data-ttu-id="6dca5-179">A kritikus fontosságú munkaterhelésekhez, érvényesíteni szeretné, hogy az alkalmazás az éles tárolóhelyre konfiguráció alkalmazása várt viselkedik, és végezze el az ilyen érvényesítési *előtt* az alkalmazást éles környezetben van cserélve.</span><span class="sxs-lookup"><span data-stu-id="6dca5-179">For mission-critical workloads, you want to validate that the app behaves as expected when the production slot's configuration is applied, and you must perform such validation *before* the app is swapped into production.</span></span> <span data-ttu-id="6dca5-180">Felcserélés előnézettel az alábbiakra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="6dca5-180">Swap with preview is what you need.</span></span>

> [!NOTE]
> <span data-ttu-id="6dca5-181">A felcserélés előnézettel a webalkalmazásokban Linux rendszeren nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6dca5-181">Swap with preview is not supported in web apps on Linux.</span></span>

<span data-ttu-id="6dca5-182">Használatakor a **a felcserélés előnézettel** beállítás (lásd: [telepítési felcserélése](#Swap)), az App Service a következőket teszi:</span><span class="sxs-lookup"><span data-stu-id="6dca5-182">When you use the **Swap with preview** option (see [Swap deployment slots](#Swap)), App Service does the following:</span></span>

- <span data-ttu-id="6dca5-183">Megtartja a céltárolóhelyre változatlan marad, így nem érinti a meglévő terhelése, hogy a bővítőhely (pl. éles környezet).</span><span class="sxs-lookup"><span data-stu-id="6dca5-183">Keeps the destination slot unchanged so existing workload on that slot (e.g. production) is not impacted.</span></span>
- <span data-ttu-id="6dca5-184">A forrás tárolási helyre, beleértve a tárolóhely-specifikus kapcsolati karakterláncok és Alkalmazásbeállítások céltárolóhelyen konfigurációs elemeinek vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="6dca5-184">Applies the configuration elements of the destination slot to the source slot, including the slot-specific connection strings and app settings.</span></span>
- <span data-ttu-id="6dca5-185">A forrás tárolási helyre, a fent említett konfigurációs elemeket a munkavégző folyamatok újraindul.</span><span class="sxs-lookup"><span data-stu-id="6dca5-185">Restarts the worker processes on the source slot using these aforementioned configuration elements.</span></span>
- <span data-ttu-id="6dca5-186">Amikor befejezte a lapozófájl-kapacitás: a warmed létrehozása előtti forrás tárolási helyre helyezi a hangsúlyt a céltárolóhelyre.</span><span class="sxs-lookup"><span data-stu-id="6dca5-186">When you complete the swap: Moves the pre-warmed-up source slot into the destination slot.</span></span> <span data-ttu-id="6dca5-187">A rendeltetési tárolási helyre helyezi át a forrás tárolási helyre, mint egy manuális felcserélés.</span><span class="sxs-lookup"><span data-stu-id="6dca5-187">The destination slot is moved into the source slot as in a manual swap.</span></span>
- <span data-ttu-id="6dca5-188">Ha megszakítja a lapozófájl-kapacitás: újra alkalmazza a forrás tárolási helyre konfigurációs elemet a forrás tárolási helyre.</span><span class="sxs-lookup"><span data-stu-id="6dca5-188">When you cancel the swap: Reapplies the configuration elements of the source slot to the source slot.</span></span>

<span data-ttu-id="6dca5-189">Megtekintheti, pontosan hogyan az alkalmazás működik a céltárolóhelyre konfigurációjával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6dca5-189">You can preview exactly how the app will behave with the destination slot's configuration.</span></span> <span data-ttu-id="6dca5-190">Ellenőrzés befejezése után végezze el a lapozófájl-kapacitás egy külön lépésben.</span><span class="sxs-lookup"><span data-stu-id="6dca5-190">Once you complete validation, you complete the swap in a separate step.</span></span> <span data-ttu-id="6dca5-191">Ez a lépés előnye, hogy a forrás tárolási helyre már tárolóhelyspecifikus a kívánt konfigurációval rendelkezik, és az ügyfelek nem tapasztalnak állásidőt fog tapasztalni.</span><span class="sxs-lookup"><span data-stu-id="6dca5-191">This step has the added advantage that the source slot is already warmed up with the desired configuration, and clients will not experience any downtime.</span></span>  

<span data-ttu-id="6dca5-192">Több fázisban swap érhető el az Azure PowerShell-parancsmagok-példák az Azure PowerShell-parancsmagok telepítési üzembe helyezési ponti szakaszban szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="6dca5-192">Samples for the Azure PowerShell cmdlets available for multi-phase swap are included in the Azure PowerShell cmdlets for deployment slots section.</span></span>

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a><span data-ttu-id="6dca5-193">Az automatikus felcserélés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6dca5-193">Configure Auto Swap</span></span>
<span data-ttu-id="6dca5-194">Az automatikus felcserélés leegyszerűsíti a DevOps helyzetekben, ahol szeretné folyamatosan alkalmazás telepítése az állásidő és nulla hidegindítás a végfelhasználók az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6dca5-194">Auto Swap streamlines DevOps scenarios where you want to continuously deploy your app with zero cold start and zero downtime for end customers of the app.</span></span> <span data-ttu-id="6dca5-195">Ha egy üzembe helyezési pont van konfigurálva az automatikus felcserélés éles környezetben, minden alkalommal, amikor a kód frissítése leküldése a tárolóhely, App Service lesz automatikusan felcserélése az alkalmazás éles környezetben után azt rendelkezik már tárolóhelyspecifikus tárolóhelye.</span><span class="sxs-lookup"><span data-stu-id="6dca5-195">When a deployment slot is configured for Auto Swap into production, every time you push your code update to that slot, App Service will automatically swap the app into production after it has already warmed up in the slot.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6dca5-196">Ha engedélyezi az automatikus felcserélés a tárhely, ellenőrizze, hogy a tárhely konfigurációs pontosan a cél tárolóhely (általában az éles tárolóhelyre) készült konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="6dca5-196">When you enable Auto Swap for a slot, make sure the slot configuration is exactly the configuration intended for the target slot (usually the production slot).</span></span>
> 
> 

> [!NOTE]
> <span data-ttu-id="6dca5-197">Az automatikus felcserélés a webalkalmazásokban Linux rendszeren nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6dca5-197">Auto swap is not supported in web apps on Linux.</span></span>

<span data-ttu-id="6dca5-198">Az automatikus felcserélés konfigurálása tárhely is könnyen.</span><span class="sxs-lookup"><span data-stu-id="6dca5-198">Configuring Auto Swap for a slot is easy.</span></span> <span data-ttu-id="6dca5-199">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6dca5-199">Follow the steps below:</span></span>

1. <span data-ttu-id="6dca5-200">A **üzembe helyezési**, válassza ki, nem éles tárhely és kiválasztható **Alkalmazásbeállítások** , hogy a tárhely erőforrás panelén.</span><span class="sxs-lookup"><span data-stu-id="6dca5-200">In **Deployment Slots**, select a non-production slot, and choose **Application Settings** in that slot's resource blade.</span></span>  
   
    ![][Autoswap1]
2. <span data-ttu-id="6dca5-201">Válassza ki **a** a **automatikus felcserélés**, válassza ki a kívánt cél bővítőhely a **automatikus felcserélés tárolóhely**, és kattintson a **mentése** parancsra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="6dca5-201">Select **On** for **Auto Swap**, select the desired target slot in **Auto Swap Slot**, and click **Save** in the command bar.</span></span> <span data-ttu-id="6dca5-202">Ellenőrizze, hogy a tárhely pontosan a cél tárolóhely szánt konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="6dca5-202">Make sure configuration for the slot is exactly the configuration intended for the target slot.</span></span>
   
    <span data-ttu-id="6dca5-203">A **értesítések** lapon villogjon, egy zöld **sikeres** a művelet végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="6dca5-203">The **Notifications** tab will flash a green **SUCCESS** once the operation is complete.</span></span>
   
    ![][Autoswap2]
   
   > [!NOTE]
   > <span data-ttu-id="6dca5-204">Tesztelje az automatikus felcserélés az alkalmazásra vonatkozóan, először jelölje ki a cél nem éles tárhely **automatikus felcserélés tárolóhely** Ismerkedjen meg a szolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="6dca5-204">To test Auto Swap for your app, you can first select a non-production target slot in **Auto Swap Slot** to become familiar with the feature.</span></span>  
   > 
   > 
3. <span data-ttu-id="6dca5-205">Hajtsa végre egy adott üzembe helyezési pont kódot küldeni.</span><span class="sxs-lookup"><span data-stu-id="6dca5-205">Execute a code push to that deployment slot.</span></span> <span data-ttu-id="6dca5-206">Az automatikus felcserélés rövid idő múlva történjen, és a frissítés jelenik meg a cél a tárhely URL-címen.</span><span class="sxs-lookup"><span data-stu-id="6dca5-206">Auto Swap will happen after a short time and the update will be reflected at your target slot's URL.</span></span>

<a name="Rollback"></a>

## <a name="to-rollback-a-production-app-after-swap"></a><span data-ttu-id="6dca5-207">A visszaállítás után swap éles alkalmazások</span><span class="sxs-lookup"><span data-stu-id="6dca5-207">To rollback a production app after swap</span></span>
<span data-ttu-id="6dca5-208">Ki a hibákat a tárolóhelycsere élesben azonosít, ha visszaállíthatja az üzembe helyezési ponti azok előtti swap állapota az azonos két üzembe helyezési ponti azonnal csere által.</span><span class="sxs-lookup"><span data-stu-id="6dca5-208">If any errors are identified in production after a slot swap, roll the slots back to their pre-swap states by swapping the same two slots immediately.</span></span>

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a><span data-ttu-id="6dca5-209">Egyéni bemelegítési swap előtt</span><span class="sxs-lookup"><span data-stu-id="6dca5-209">Custom warm-up before swap</span></span>
<span data-ttu-id="6dca5-210">Néhány alkalmazás egyéni bemelegítési műveletek lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="6dca5-210">Some apps may require custom warm-up actions.</span></span> <span data-ttu-id="6dca5-211">A `applicationInitialization` konfigurációs elem a Web.config fájlban lehetővé teszi egyéni inicializálási műveletek végrehajtása előtt a kérelem érkezik.</span><span class="sxs-lookup"><span data-stu-id="6dca5-211">The `applicationInitialization` configuration element in web.config allows you to specify custom initialization actions to be performed before a request is received.</span></span> <span data-ttu-id="6dca5-212">A csereművelet végrehajtásához az egyéni bemelegítési várakozik.</span><span class="sxs-lookup"><span data-stu-id="6dca5-212">The swap operation will wait for this custom warm-up to complete.</span></span> <span data-ttu-id="6dca5-213">Íme egy minta web.config töredéket.</span><span class="sxs-lookup"><span data-stu-id="6dca5-213">Here is a sample web.config fragment.</span></span>

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="to-delete-a-deployment-slot"></a><span data-ttu-id="6dca5-214">Egy üzembe helyezési pont törlése</span><span class="sxs-lookup"><span data-stu-id="6dca5-214">To delete a deployment slot</span></span>
<span data-ttu-id="6dca5-215">Egy üzembe helyezési pont paneljén nyissa meg a telepített környezet tárolóhelye panelen, kattintson a **áttekintése** (az alapértelmezett oldal), és kattintson a **törlése** parancsra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="6dca5-215">In the blade for a deployment slot, open the deployment slot's blade, click **Overview** (the default page), and click **Delete** in the command bar.</span></span>  

![Egy üzembe helyezési pont törlése][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a><span data-ttu-id="6dca5-217">Az üzembe helyezési tárhely az Azure PowerShell-parancsmagjai</span><span class="sxs-lookup"><span data-stu-id="6dca5-217">Azure PowerShell cmdlets for deployment slots</span></span>
<span data-ttu-id="6dca5-218">Az Azure PowerShell modul, amely kezelése a Windows PowerShell, hiszen támogatják az kezelése az Azure App Service üzembe helyezési segítségével Azure-parancsmagokat kínál.</span><span class="sxs-lookup"><span data-stu-id="6dca5-218">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell, including support for managing deployment slots in Azure App Service.</span></span>

* <span data-ttu-id="6dca5-219">A telepítése és konfigurálása az Azure PowerShell és az Azure PowerShell hitelesítéséhez az Azure előfizetéssel rendelkező információkért lásd: [telepítése és konfigurálása a Microsoft Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6dca5-219">For information on installing and configuring Azure PowerShell, and on authenticating Azure PowerShell with your Azure subscription, see [How to install and configure Microsoft Azure PowerShell](/powershell/azure/overview).</span></span>  

- - -
### <a name="create-a-web-app"></a><span data-ttu-id="6dca5-220">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dca5-220">Create a web app</span></span>
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a><span data-ttu-id="6dca5-221">Egy üzembe helyezési tárhely létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dca5-221">Create a deployment slot</span></span>
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a><span data-ttu-id="6dca5-222">Indítson el egy felcserélés felülvizsgálati (több fázisban swap) és a cél tárolóhely konfiguráció alkalmazása a forrás tárolási helyre</span><span class="sxs-lookup"><span data-stu-id="6dca5-222">Initiate a swap with review (multi-phase swap) and apply destination slot configuration to source slot</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a><span data-ttu-id="6dca5-223">Megszakítja a függőben lévő felcserélés (felülvizsgálati felcserélés) és a forrás helyezési pont konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="6dca5-223">Cancel a pending swap (swap with review) and restore source slot configuration</span></span>
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a><span data-ttu-id="6dca5-224">Központi telepítés felcserélése</span><span class="sxs-lookup"><span data-stu-id="6dca5-224">Swap deployment slots</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a><span data-ttu-id="6dca5-225">Üzembe helyezési pont törlése</span><span class="sxs-lookup"><span data-stu-id="6dca5-225">Delete deployment slot</span></span>
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a><span data-ttu-id="6dca5-226">Az üzembe helyezési tárhely az Azure parancssori felület (CLI) parancsok</span><span class="sxs-lookup"><span data-stu-id="6dca5-226">Azure Command-Line Interface (Azure CLI) commands for Deployment Slots</span></span>
<span data-ttu-id="6dca5-227">Az Azure parancssori felület platformfüggetlen parancsokat biztosít működik-e az Azure-ral, hiszen támogatják az App Service üzembe helyezési pontok kezelése.</span><span class="sxs-lookup"><span data-stu-id="6dca5-227">The Azure CLI provides cross-platform commands for working with Azure, including support for managing App Service deployment slots.</span></span>

* <span data-ttu-id="6dca5-228">Telepítése és konfigurálása az Azure parancssori felület, beleértve az Azure parancssori felület csatlakoztatása az Azure-előfizetéshez, további információkért lásd: [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6dca5-228">For instructions on installing and configuring the Azure CLI, including information on how to connect Azure CLI to your Azure subscription, see [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="6dca5-229">Az Azure App Service az Azure parancssori felületen használható parancsok listáját, hívja meg a `azure site -h`.</span><span class="sxs-lookup"><span data-stu-id="6dca5-229">To list the commands available for Azure App Service in the Azure CLI, call `azure site -h`.</span></span>

> [!NOTE] 
> <span data-ttu-id="6dca5-230">A [Azure CLI 2.0](https://github.com/Azure/azure-cli) parancsok a üzembe helyezési, lásd: [az App Service web üzembe helyezési pont](/cli/azure/appservice/web/deployment/slot).</span><span class="sxs-lookup"><span data-stu-id="6dca5-230">For [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands for deployment slots, see [az appservice web deployment slot](/cli/azure/appservice/web/deployment/slot).</span></span>

- - -
### <a name="azure-site-list"></a><span data-ttu-id="6dca5-231">az Azure webhely-lista</span><span class="sxs-lookup"><span data-stu-id="6dca5-231">azure site list</span></span>
<span data-ttu-id="6dca5-232">Az aktuális előfizetésben az alkalmazásokkal kapcsolatos információk hívás **azure webhelylista**, az alábbi példa.</span><span class="sxs-lookup"><span data-stu-id="6dca5-232">For information about the apps in the current subscription, call **azure site list**, as in the following example.</span></span>

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a><span data-ttu-id="6dca5-233">az Azure-webhely létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dca5-233">azure site create</span></span>
<span data-ttu-id="6dca5-234">Egy üzembe helyezési tárhely létrehozása, hívja meg a **azure-webhely létrehozása** és adja meg a meglévő alkalmazás és a tárolóhely létrehozásához, az alábbi példában látható módon nevét.</span><span class="sxs-lookup"><span data-stu-id="6dca5-234">To create a deployment slot, call **azure site create** and specify the name of an existing app and the name of the slot to create, as in the following example.</span></span>

`azure site create webappslotstest --slot staging`

<span data-ttu-id="6dca5-235">Az új tárhely verziókövetésének engedélyezéséhez használja a **– git** beállítás, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="6dca5-235">To enable source control for the new slot, use the **--git** option, as in the following example.</span></span>

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a><span data-ttu-id="6dca5-236">az Azure site lapozófájl-kapacitás</span><span class="sxs-lookup"><span data-stu-id="6dca5-236">azure site swap</span></span>
<span data-ttu-id="6dca5-237">Ahhoz, hogy a frissített telepítési az éles alkalmazások tárolóhely, használja a **azure hely swap** parancs a Csere műveletet, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="6dca5-237">To make the updated deployment slot the production app, use the **azure site swap** command to perform a swap operation, as in the following example.</span></span> <span data-ttu-id="6dca5-238">Az éles alkalmazás nem fog tapasztalni bármely állásidő, és nem fog változni hidegindítás.</span><span class="sxs-lookup"><span data-stu-id="6dca5-238">The production app will not experience any down time, nor will it undergo a cold start.</span></span>

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a><span data-ttu-id="6dca5-239">az Azure webhely törlése</span><span class="sxs-lookup"><span data-stu-id="6dca5-239">azure site delete</span></span>
<span data-ttu-id="6dca5-240">Az üzembe helyezési pont, amely már nem szükséges törölheti a **azure webhely törlése** parancs, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="6dca5-240">To delete a deployment slot that is no longer needed, use the **azure site delete** command, as in the following example.</span></span>

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> <span data-ttu-id="6dca5-241">Tekintsen meg működés közben egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6dca5-241">See a web app in action.</span></span> <span data-ttu-id="6dca5-242">[App Service kipróbálása](https://azure.microsoft.com/try/app-service/) azonnal, és hozzon létre egy rövid élettartamú alapszintű alkalmazást – nincs szükség bankkártyára, nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="6dca5-242">[Try App Service](https://azure.microsoft.com/try/app-service/) immediately and create a short-lived starter app—no credit card required, no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6dca5-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6dca5-243">Next Steps</span></span>
<span data-ttu-id="6dca5-244">[Az Azure App Service Web App – webes hozzáférés letiltása nem éles üzembe helyezési](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Bevezetés az App Service Linux](./app-service-linux-intro.md)
[Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="6dca5-244">[Azure App Service Web App – block web access to non-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Introduction to App Service on Linux](./app-service-linux-intro.md)
[Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/)</span></span>

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

