---
title: "átmeneti környezet az Azure App Service web Apps mentése aaaSet |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse előkészített közzététele az Azure App Service web Apps."
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
ms.openlocfilehash: 338424100a20bf823323313fb6699e439f367421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a><span data-ttu-id="e4016-103">Átmeneti környezet az Azure App Service beállítása</span><span class="sxs-lookup"><span data-stu-id="e4016-103">Set up staging environments in Azure App Service</span></span>
<a name="Overview"></a>

<span data-ttu-id="e4016-104">Ha központilag telepíti a webalkalmazás, webes alkalmazás a Linux, mobil háttér és API-alkalmazás túl[App Service](http://go.microsoft.com/fwlink/?LinkId=529714), központilag telepíthető tooa külön üzembe helyezési pont helyett hello alapértelmezett éles tárolóhelyre hello futtatásakor **Standard**vagy **prémium** App Service-csomag mód.</span><span class="sxs-lookup"><span data-stu-id="e4016-104">When you deploy your web app, web app on Linux, mobile back end, and API app too[App Service](http://go.microsoft.com/fwlink/?LinkId=529714), you can deploy tooa separate deployment slot instead of hello default production slot when running in hello **Standard** or **Premium** App Service plan mode.</span></span> <span data-ttu-id="e4016-105">Üzembe helyezési saját állomásnevek ténylegesen élő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e4016-105">Deployment slots are actually live apps with their own hostnames.</span></span> <span data-ttu-id="e4016-106">Alkalmazás tartalmát és konfigurációk elemek lecserélhető között két üzembe helyezési, beleértve a hello éles tárolóhelyre.</span><span class="sxs-lookup"><span data-stu-id="e4016-106">App content and configurations elements can be swapped between two deployment slots, including hello production slot.</span></span> <span data-ttu-id="e4016-107">Az alkalmazás tooa üzembe helyezési pont telepítése a következő előnyöket hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e4016-107">Deploying your application tooa deployment slot has hello following benefits:</span></span>

* <span data-ttu-id="e4016-108">Egy átmeneti üzembe helyezési tárhelyet az alkalmazások változásairól előtt hello éles tárhelyének csere azt is ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="e4016-108">You can validate app changes in a staging deployment slot before swapping it with hello production slot.</span></span>
* <span data-ttu-id="e4016-109">Először telepítése egy tooa tárolóhelye és csere az éles környezetben biztosítja, hogy a hello tárolóhely összes példányát, mielőtt éles környezetben felcserélés folyamatban vannak tárolóhelyspecifikus.</span><span class="sxs-lookup"><span data-stu-id="e4016-109">Deploying an app tooa slot first and swapping it into production ensures that all instances of hello slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="e4016-110">Ez megszünteti állásidő, az alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="e4016-110">This eliminates downtime when you deploy your app.</span></span> <span data-ttu-id="e4016-111">hello forgalom átirányítása zökkenőmentes-kérelmek nem dobja swap műveletek miatt.</span><span class="sxs-lookup"><span data-stu-id="e4016-111">hello traffic redirection is seamless, and no requests are dropped as a result of swap operations.</span></span> <span data-ttu-id="e4016-112">A teljes munkafolyamat konfigurálásával automatizálható [automatikus felcserélés](#Auto-Swap) , ha nincs szükség a előtti swap érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="e4016-112">This entire workflow can be automated by configuring [Auto Swap](#Auto-Swap) when pre-swap validation is not needed.</span></span>
* <span data-ttu-id="e4016-113">A lapozófájl-kapacitás után hello tárolóhely korábban előkészített alkalmazással most már hello előző éles alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e4016-113">After a swap, hello slot with previously staged app now has hello previous production app.</span></span> <span data-ttu-id="e4016-114">Ha az éles tárolóhelyre hello felcserélve hello módosítások nem a várt módon, azonos felcserélése azonnal tooget az "utolsó ismert helyes webhely" biztonsági hello végezheti el.</span><span class="sxs-lookup"><span data-stu-id="e4016-114">If hello changes swapped into hello production slot are not as you expected, you can perform hello same swap immediately tooget your "last known good site" back.</span></span>

<span data-ttu-id="e4016-115">Minden App Service-csomag mód támogatja az üzembe helyezési pontok különböző számú.</span><span class="sxs-lookup"><span data-stu-id="e4016-115">Each App Service plan mode supports a different number of deployment slots.</span></span> <span data-ttu-id="e4016-116">toofind hello helyek száma, az alkalmazás mód is támogat, lásd: [App Service szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="e4016-116">toofind out hello number of slots your app's mode supports, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

* <span data-ttu-id="e4016-117">Ha az alkalmazás még több üzembe helyezési ponti, hello mód nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="e4016-117">When your app has multiple slots, you cannot change hello mode.</span></span>
* <span data-ttu-id="e4016-118">Skálázás nem érhető el a nem végleges pont felcserélése.</span><span class="sxs-lookup"><span data-stu-id="e4016-118">Scaling is not available for non-production slots.</span></span>
* <span data-ttu-id="e4016-119">Csatolt erőforrás-kezelés nem végleges pont felcserélése nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e4016-119">Linked resource management is not supported for non-production slots.</span></span> <span data-ttu-id="e4016-120">A hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) csak, elkerülheti a célgyűjtemény az éles tárhely hello nem éles tárolóhelyre tooa másik App Service csomag mód ideiglenesen áthelyezésével.</span><span class="sxs-lookup"><span data-stu-id="e4016-120">In hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) only, you can avoid this potential impact on a production slot by temporarily moving hello non-production slot tooa different App Service plan mode.</span></span> <span data-ttu-id="e4016-121">Vegye figyelembe, hogy hello nem éles tárolóhelyre ismét közös hello hello éles tárolóhelyre előtt hello két üzembe helyezési ponti kicserélheti azonos módban.</span><span class="sxs-lookup"><span data-stu-id="e4016-121">Note that hello non-production slot must once again share hello same mode with hello production slot before you can swap hello two slots.</span></span>

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a><span data-ttu-id="e4016-122">Adja hozzá egy üzembe helyezési tárhelyet</span><span class="sxs-lookup"><span data-stu-id="e4016-122">Add a deployment slot</span></span>
<span data-ttu-id="e4016-123">hello app futnia kell a hello **szabványos** vagy **prémium** meg tooenable több üzembe helyezési mód sorrendjének.</span><span class="sxs-lookup"><span data-stu-id="e4016-123">hello app must be running in hello **Standard** or **Premium** mode in order for you tooenable multiple deployment slots.</span></span>

1. <span data-ttu-id="e4016-124">A hello [Azure Portal](https://portal.azure.com/), nyissa meg az alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="e4016-124">In hello [Azure Portal](https://portal.azure.com/), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="e4016-125">Válassza ki a hello **üzembe helyezési** lehetőséget, majd kattintson az **tárhely felvétele**.</span><span class="sxs-lookup"><span data-stu-id="e4016-125">Choose hello **Deployment slots** option, then click **Add Slot**.</span></span>
   
    ![Adja hozzá egy új üzembe helyezési tárhelyet][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > <span data-ttu-id="e4016-127">Ha hello alkalmazás még nincs hello **szabványos** vagy **prémium** mód, kapni fog egy üzenetet támogatott hello módok előkészített közzététel engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="e4016-127">If hello app is not already in hello **Standard** or **Premium** mode, you will receive a message indicating hello supported modes for enabling staged publishing.</span></span> <span data-ttu-id="e4016-128">Ezen a ponton rendelkezik hello beállítás tooselect **frissítése** , és keresse meg a toohello **méretezési** fülre az alkalmazás a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="e4016-128">At this point, you have hello option tooselect **Upgrade** and navigate toohello **Scale** tab of your app before continuing.</span></span>
   > 
   > 
3. <span data-ttu-id="e4016-129">A hello **tárhely hozzáadása** panelen adjon hello tárolóhelye egy olyan nevet, és válassza ki e tooclone alkalmazás-konfiguráció egy másik meglévő üzembe helyezési pont.</span><span class="sxs-lookup"><span data-stu-id="e4016-129">In hello **Add a slot** blade, give hello slot a name, and select whether tooclone app configuration from another existing deployment slot.</span></span> <span data-ttu-id="e4016-130">Kattintson a pipa jelre toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="e4016-130">Click hello check mark toocontinue.</span></span>
   
    ![Konfigurációs forrás][ConfigurationSource1]
   
    <span data-ttu-id="e4016-132">hello hozzáadásakor tárhely, csak hogy két választási lehetőség: Klónozott konfigurációs hello alapértelmezett tárolóhelyről éles környezetben, vagy egyáltalán nem.</span><span class="sxs-lookup"><span data-stu-id="e4016-132">hello first time you add a slot, you will only have two choices: clone configuration from hello default slot in production or not at all.</span></span>
    <span data-ttu-id="e4016-133">A létrehozást követően több üzembe helyezési ponti, fogja képes tooclone konfigurációs eltérő hello éles környezetben a tárolóhelyről:</span><span class="sxs-lookup"><span data-stu-id="e4016-133">After you have created several slots, you will be able tooclone configuration from a slot other than hello one in production:</span></span>
   
    ![Konfigurációs forrás][MultipleConfigurationSources]
4. <span data-ttu-id="e4016-135">Az alkalmazás erőforrás paneljén kattintson **üzembe helyezési**, majd kattintson a központi telepítés tárolóhely tooopen, hogy a tárhely erőforrás panelről, metrikákat és csakúgy, mint bármely más alkalmazás-konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="e4016-135">In your app's resource blade, click  **Deployment slots**, then click a deployment slot tooopen that slot's resource blade, with a set of metrics and configuration just like any other app.</span></span> <span data-ttu-id="e4016-136">hello hello tárolóhely neve látható hello panel tooremind hello tetején, hogy a rendszer azért jelenítette hello üzembe helyezési pont.</span><span class="sxs-lookup"><span data-stu-id="e4016-136">hello name of hello slot is shown at hello top of hello blade tooremind you that you are viewing hello deployment slot.</span></span>
   
    ![Központi telepítés tárolóhely cím][StagingTitle]
5. <span data-ttu-id="e4016-138">Kattintson a hello a tárhely panelen hello alkalmazás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="e4016-138">Click hello app URL in hello slot's blade.</span></span> <span data-ttu-id="e4016-139">Figyelje meg hello üzembe helyezési pont saját állomásnév és is egy élő alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e4016-139">Notice hello deployment slot has its own hostname and is also a live app.</span></span> <span data-ttu-id="e4016-140">toolimit nyilvános hozzáférés toohello üzembe helyezési pont, lásd: [App Service Web App – blokk web access toonon éles üzembe helyezési](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span><span class="sxs-lookup"><span data-stu-id="e4016-140">toolimit public access toohello deployment slot, see [App Service Web App – block web access toonon-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span></span>

<span data-ttu-id="e4016-141">Nincs tartalom központi telepítési tárolóhely létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="e4016-141">There is no content after deployment slot creation.</span></span> <span data-ttu-id="e4016-142">Egy másik tárház ágat, vagy egy teljesen más tárház toohello tárolóhely telepítése.</span><span class="sxs-lookup"><span data-stu-id="e4016-142">You can deploy toohello slot from a different repository branch, or an altogether different repository.</span></span> <span data-ttu-id="e4016-143">Hello helyezési pont konfigurációját módosíthatja is.</span><span class="sxs-lookup"><span data-stu-id="e4016-143">You can also change hello slot's configuration.</span></span> <span data-ttu-id="e4016-144">Használjon hello hello üzembe helyezési pont tartalomfrissítéseket társított profil vagy a központi telepítési hitelesítő adatok közzététele.</span><span class="sxs-lookup"><span data-stu-id="e4016-144">Use hello publish profile or deployment credentials associated with hello deployment slot for content updates.</span></span>  <span data-ttu-id="e4016-145">Például végezheti [toothis tárolóhely a git közzététele](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="e4016-145">For example, you can [publish toothis slot with git](app-service-deploy-local-git.md).</span></span>

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a><span data-ttu-id="e4016-146">Az üzembe helyezési konfiguráció</span><span class="sxs-lookup"><span data-stu-id="e4016-146">Configuration for deployment slots</span></span>
<span data-ttu-id="e4016-147">A klón a másik üzembe helyezési pont konfigurációja hello klónozott konfigurációs esetén szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="e4016-147">When you clone configuration from another deployment slot, hello cloned configuration is editable.</span></span> <span data-ttu-id="e4016-148">Továbbá néhány konfigurációs elemek követi hello tartalom a lapozófájl-kapacitás (nem tárolóhely adott) keresztül közben hello azonos tárolóhely a lapozófájl-kapacitás (tárolóhely adott) után más konfigurációs elemek marad.</span><span class="sxs-lookup"><span data-stu-id="e4016-148">Furthermore, some configuration elements will follow hello content across a swap (not slot specific) while other configuration elements will stay in hello same slot after a swap (slot specific).</span></span> <span data-ttu-id="e4016-149">hello következő listákban megjelenítése változnak, ha Ön felcserélése hello konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="e4016-149">hello following lists show hello configuration that will change when you swap slots.</span></span>

<span data-ttu-id="e4016-150">**Beállítások, amelyek van cserélve**:</span><span class="sxs-lookup"><span data-stu-id="e4016-150">**Settings that are swapped**:</span></span>

* <span data-ttu-id="e4016-151">Általános beállítások - keretrendszer verziója, 32 vagy 64 bites, például webes szoftvercsatornák</span><span class="sxs-lookup"><span data-stu-id="e4016-151">General settings - such as framework version, 32/64-bit, Web sockets</span></span>
* <span data-ttu-id="e4016-152">Alkalmazásbeállítások (konfigurált toostick tooa tárolóhely is lehet)</span><span class="sxs-lookup"><span data-stu-id="e4016-152">App settings (can be configured toostick tooa slot)</span></span>
* <span data-ttu-id="e4016-153">Kapcsolati karakterláncok (konfigurált toostick tooa tárolóhely is lehet)</span><span class="sxs-lookup"><span data-stu-id="e4016-153">Connection strings (can be configured toostick tooa slot)</span></span>
* <span data-ttu-id="e4016-154">Kezelőleképezések</span><span class="sxs-lookup"><span data-stu-id="e4016-154">Handler mappings</span></span>
* <span data-ttu-id="e4016-155">Megfigyelési és diagnosztikai beállítások</span><span class="sxs-lookup"><span data-stu-id="e4016-155">Monitoring and diagnostic settings</span></span>
* <span data-ttu-id="e4016-156">Webjobs-feladatok tartalom</span><span class="sxs-lookup"><span data-stu-id="e4016-156">WebJobs content</span></span>

<span data-ttu-id="e4016-157">**Beállítások, amelyek nem van cserélve**:</span><span class="sxs-lookup"><span data-stu-id="e4016-157">**Settings that are not swapped**:</span></span>

* <span data-ttu-id="e4016-158">Közzétételi végpontok</span><span class="sxs-lookup"><span data-stu-id="e4016-158">Publishing endpoints</span></span>
* <span data-ttu-id="e4016-159">Egyéni tartománynevek</span><span class="sxs-lookup"><span data-stu-id="e4016-159">Custom Domain Names</span></span>
* <span data-ttu-id="e4016-160">SSL-tanúsítványok és kötések</span><span class="sxs-lookup"><span data-stu-id="e4016-160">SSL certificates and bindings</span></span>
* <span data-ttu-id="e4016-161">Skálázási beállításokat</span><span class="sxs-lookup"><span data-stu-id="e4016-161">Scale settings</span></span>
* <span data-ttu-id="e4016-162">Webjobs-feladatok bejegyzéstípusait</span><span class="sxs-lookup"><span data-stu-id="e4016-162">WebJobs schedulers</span></span>

<span data-ttu-id="e4016-163">egy beállítás vagy a kapcsolati karakterlánc toostick tooa tárolóhelye (nem cserélhető fel), hozzáférési hello tooconfigure **Alkalmazásbeállítások** panel egy adott tárhely, majd jelölje be hello **tárolóhely beállítás** hello be konfigurációs elemek, amelyek kell odatapadjon hello tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="e4016-163">tooconfigure an app setting or connection string toostick tooa slot (not swapped), access hello **Application Settings** blade for a specific slot, then select hello **Slot Setting** box for hello configuration elements that should stick hello slot.</span></span> <span data-ttu-id="e4016-164">Vegye figyelembe, hogy jelölés egy konfigurációs elem, ha adott tárolóhely létrehozásáról szóló az elem nem cserélhető között hello alkalmazáshoz kapcsolódó összes hello üzembe helyezési hello hatását.</span><span class="sxs-lookup"><span data-stu-id="e4016-164">Note that marking a configuration element as slot specific has hello effect of establishing that element as not swappable across all hello deployment slots associated with hello app.</span></span>

![Tárolóhely-beállítások][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a><span data-ttu-id="e4016-166">Központi telepítés felcserélése</span><span class="sxs-lookup"><span data-stu-id="e4016-166">Swap deployment slots</span></span> 
<span data-ttu-id="e4016-167">Kicserélheti az üzembe helyezési a hello **áttekintése** vagy **üzembe helyezési** a app erőforráspanelen ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e4016-167">You can swap deployment slots in hello **Overview** or **Deployment slots** view of your app's resource blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e4016-168">Mielőtt egy üzembe helyezési pont alkalmazás felcserélni a éles környezetben, győződjön meg arról, hogy az összes nem tárolóhely adott beállításai toohave kívánt hello swap cél azt.</span><span class="sxs-lookup"><span data-stu-id="e4016-168">Before you swap an app from a deployment slot into production, make sure that all non-slot specific settings are configured exactly as you want toohave it in hello swap target.</span></span>
> 
> 

1. <span data-ttu-id="e4016-169">üzembe helyezési tooswap, kattintson a hello **felcserélése** hello parancssáv hello alkalmazás vagy a hello parancssávon, egy üzembe helyezési pont gombra.</span><span class="sxs-lookup"><span data-stu-id="e4016-169">tooswap deployment slots, click hello **Swap** button in hello command bar of hello app or in hello command bar of a deployment slot.</span></span>
   
    ![Lapozófájl-kapacitás gomb][SwapButtonBar]

2. <span data-ttu-id="e4016-171">Győződjön meg arról, hogy hello swap forrás- és a lapozófájl-kapacitás a célkiszolgáló megfelelően vannak-e beállítva.</span><span class="sxs-lookup"><span data-stu-id="e4016-171">Make sure that hello swap source and swap target are set properly.</span></span> <span data-ttu-id="e4016-172">Általában hello felcserélés célja hello éles tárolóhelyre.</span><span class="sxs-lookup"><span data-stu-id="e4016-172">Usually, hello swap target is hello production slot.</span></span> <span data-ttu-id="e4016-173">Kattintson a **OK** toocomplete hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="e4016-173">Click **OK** toocomplete hello operation.</span></span> <span data-ttu-id="e4016-174">Hello művelet befejezése után hello üzembe helyezési rendelkezik lett cserélve.</span><span class="sxs-lookup"><span data-stu-id="e4016-174">When hello operation finishes, hello deployment slots have been swapped.</span></span>

    ![Felcserélés befejezése](./media/web-sites-staged-publishing/SwapImmediately.png)

    <span data-ttu-id="e4016-176">A hello **felcserélés előnézettel** típus felcserélése, lásd: [felcserélés előnézettel (több fázisban swap)](#Multi-Phase).</span><span class="sxs-lookup"><span data-stu-id="e4016-176">For hello **Swap with preview** swap type, see [Swap with preview (multi-phase swap)](#Multi-Phase).</span></span>  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a><span data-ttu-id="e4016-177">A felcserélés előnézettel (több fázisban swap)</span><span class="sxs-lookup"><span data-stu-id="e4016-177">Swap with preview (multi-phase swap)</span></span>

<span data-ttu-id="e4016-178">A felcserélés előnézettel, vagy több fázisban swap leegyszerűsíti a tárolóhely konfigurációs elemeket, például kapcsolati karakterláncok érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="e4016-178">Swap with preview, or multi-phase swap, simplify validation of slot-specific configuration elements, such as connection strings.</span></span>
<span data-ttu-id="e4016-179">A kritikus fontosságú munkaterhelésekhez, azt szeretné, hogy az alkalmazás hello toovalidate hello éles tárolóhelyre konfiguráció alkalmazása várt viselkedik, és végezze el az ilyen érvényesítési *előtt* hello alkalmazást éles környezetben van cserélve.</span><span class="sxs-lookup"><span data-stu-id="e4016-179">For mission-critical workloads, you want toovalidate that hello app behaves as expected when hello production slot's configuration is applied, and you must perform such validation *before* hello app is swapped into production.</span></span> <span data-ttu-id="e4016-180">Felcserélés előnézettel az alábbiakra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="e4016-180">Swap with preview is what you need.</span></span>

> [!NOTE]
> <span data-ttu-id="e4016-181">A felcserélés előnézettel a webalkalmazásokban Linux rendszeren nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e4016-181">Swap with preview is not supported in web apps on Linux.</span></span>

<span data-ttu-id="e4016-182">Hello használata esetén **a felcserélés előnézettel** beállítás (lásd: [telepítési felcserélése](#Swap)), az App Service hello a következő:</span><span class="sxs-lookup"><span data-stu-id="e4016-182">When you use hello **Swap with preview** option (see [Swap deployment slots](#Swap)), App Service does hello following:</span></span>

- <span data-ttu-id="e4016-183">A memóriában tárolja hello cél tárolóhely változatlan ezért meglévő terhelése, hogy a bővítőhely (pl. éles) nem változik.</span><span class="sxs-lookup"><span data-stu-id="e4016-183">Keeps hello destination slot unchanged so existing workload on that slot (e.g. production) is not impacted.</span></span>
- <span data-ttu-id="e4016-184">Hello konfigurációs elemeinek hello tárolóhely toohello forrás céltárolóhelyen, beleértve a hello tárolóhely-specifikus kapcsolati karakterláncok és Alkalmazásbeállítások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e4016-184">Applies hello configuration elements of hello destination slot toohello source slot, including hello slot-specific connection strings and app settings.</span></span>
- <span data-ttu-id="e4016-185">Újraindítja a munkavégző folyamatok hello hello forrás tárolási helyre, a fent említett konfigurációs elemeket.</span><span class="sxs-lookup"><span data-stu-id="e4016-185">Restarts hello worker processes on hello source slot using these aforementioned configuration elements.</span></span>
- <span data-ttu-id="e4016-186">Hello swap befejezésekor: a kurzor hello warmed létrehozása előtti forrás tárolási helyre történő hello céltárolóhelyen.</span><span class="sxs-lookup"><span data-stu-id="e4016-186">When you complete hello swap: Moves hello pre-warmed-up source slot into hello destination slot.</span></span> <span data-ttu-id="e4016-187">hello céltárolóhelyen hello forrás tárolási helyre, mint egy manuális felcserélés áthelyezése történik.</span><span class="sxs-lookup"><span data-stu-id="e4016-187">hello destination slot is moved into hello source slot as in a manual swap.</span></span>
- <span data-ttu-id="e4016-188">Ha megszakítja a hello lapozófájl-kapacitás: hello konfigurációs elemek hello forrás tárolóhely toohello forrás tárolási helyre, újra alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="e4016-188">When you cancel hello swap: Reapplies hello configuration elements of hello source slot toohello source slot.</span></span>

<span data-ttu-id="e4016-189">Megtekintheti, pontosan hogyan hello app működik hello cél a tárhely konfigurációjával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e4016-189">You can preview exactly how hello app will behave with hello destination slot's configuration.</span></span> <span data-ttu-id="e4016-190">Ellenőrzés befejezése után el kell végeznie egy külön lépésben hello lapozófájl-kapacitás.</span><span class="sxs-lookup"><span data-stu-id="e4016-190">Once you complete validation, you complete hello swap in a separate step.</span></span> <span data-ttu-id="e4016-191">Ez a lépés rendelkezik hello előnye, hogy hello forrás tárolási helyre már tárolóhelyspecifikus hello kívánt konfigurációval, és az ügyfelek nem tapasztalnak állásidőt fog tapasztalni.</span><span class="sxs-lookup"><span data-stu-id="e4016-191">This step has hello added advantage that hello source slot is already warmed up with hello desired configuration, and clients will not experience any downtime.</span></span>  

<span data-ttu-id="e4016-192">Hello Azure PowerShell-parancsmagok érhető el több fázisban swap minták hello Azure PowerShell-parancsmagok a központi telepítési üzembe helyezési ponti szakaszban szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="e4016-192">Samples for hello Azure PowerShell cmdlets available for multi-phase swap are included in hello Azure PowerShell cmdlets for deployment slots section.</span></span>

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a><span data-ttu-id="e4016-193">Az automatikus felcserélés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e4016-193">Configure Auto Swap</span></span>
<span data-ttu-id="e4016-194">Az alkalmazás a nulla hidegindítás és állásidő automatikus felcserélés leegyszerűsíti DevOps forgatókönyvek toocontinuously, ahová telepíteni a végfelhasználók hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e4016-194">Auto Swap streamlines DevOps scenarios where you want toocontinuously deploy your app with zero cold start and zero downtime for end customers of hello app.</span></span> <span data-ttu-id="e4016-195">Konfigurálásakor egy üzembe helyezési pont az automatikus felcserélés éles környezetben, minden alkalommal, amikor a kód frissítés toothat aljzat leküldéses App Service lesz automatikusan felcserélése hello alkalmazást éles környezetben után azt rendelkezik már tárolóhelyspecifikus hello tárolóhelye.</span><span class="sxs-lookup"><span data-stu-id="e4016-195">When a deployment slot is configured for Auto Swap into production, every time you push your code update toothat slot, App Service will automatically swap hello app into production after it has already warmed up in hello slot.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e4016-196">Ha engedélyezi az automatikus felcserélés a tárhely, győződjön meg arról hello bővítőhely pontosan hello konfigurációját hello cél tárolóhely (általában hello éles tárolóhelyre) számára készült.</span><span class="sxs-lookup"><span data-stu-id="e4016-196">When you enable Auto Swap for a slot, make sure hello slot configuration is exactly hello configuration intended for hello target slot (usually hello production slot).</span></span>
> 
> 

> [!NOTE]
> <span data-ttu-id="e4016-197">Az automatikus felcserélés a webalkalmazásokban Linux rendszeren nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e4016-197">Auto swap is not supported in web apps on Linux.</span></span>

<span data-ttu-id="e4016-198">Az automatikus felcserélés konfigurálása tárhely is könnyen.</span><span class="sxs-lookup"><span data-stu-id="e4016-198">Configuring Auto Swap for a slot is easy.</span></span> <span data-ttu-id="e4016-199">Kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e4016-199">Follow hello steps below:</span></span>

1. <span data-ttu-id="e4016-200">A **üzembe helyezési**, válassza ki, nem éles tárhely és kiválasztható **Alkalmazásbeállítások** , hogy a tárhely erőforrás panelén.</span><span class="sxs-lookup"><span data-stu-id="e4016-200">In **Deployment Slots**, select a non-production slot, and choose **Application Settings** in that slot's resource blade.</span></span>  
   
    ![][Autoswap1]
2. <span data-ttu-id="e4016-201">Válassza ki **a** a **automatikus felcserélés**, jelölje be hello kívánt cél tárolóhely a **automatikus felcserélés tárolóhely**, és kattintson a **mentése** hello parancs sávon.</span><span class="sxs-lookup"><span data-stu-id="e4016-201">Select **On** for **Auto Swap**, select hello desired target slot in **Auto Swap Slot**, and click **Save** in hello command bar.</span></span> <span data-ttu-id="e4016-202">Ellenőrizze, hogy hello bővítőhely pontosan hello cél tárolóhely szánt hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="e4016-202">Make sure configuration for hello slot is exactly hello configuration intended for hello target slot.</span></span>
   
    <span data-ttu-id="e4016-203">Hello **értesítések** lapon villogjon, egy zöld **sikeres** hello művelet végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="e4016-203">hello **Notifications** tab will flash a green **SUCCESS** once hello operation is complete.</span></span>
   
    ![][Autoswap2]
   
   > [!NOTE]
   > <span data-ttu-id="e4016-204">tootest automatikus felcserélés az alkalmazáshoz, először kiválaszthatja a cél nem éles tárhely **automatikus felcserélés tárolóhely** toobecome ismeri a hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e4016-204">tootest Auto Swap for your app, you can first select a non-production target slot in **Auto Swap Slot** toobecome familiar with hello feature.</span></span>  
   > 
   > 
3. <span data-ttu-id="e4016-205">A kód leküldéses toothat üzembe helyezési pont hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="e4016-205">Execute a code push toothat deployment slot.</span></span> <span data-ttu-id="e4016-206">Az automatikus felcserélés rövid idő múlva történjen, és hello frissítés jelenik meg a cél a tárhely URL-címen.</span><span class="sxs-lookup"><span data-stu-id="e4016-206">Auto Swap will happen after a short time and hello update will be reflected at your target slot's URL.</span></span>

<a name="Rollback"></a>

## <a name="toorollback-a-production-app-after-swap"></a><span data-ttu-id="e4016-207">lapozófájl-kapacitás után éles alkalmazások toorollback</span><span class="sxs-lookup"><span data-stu-id="e4016-207">toorollback a production app after swap</span></span>
<span data-ttu-id="e4016-208">Ki a hibákat a tárolóhelycsere élesben azonosít, ha roll hello üzembe helyezési ponti hátsó tootheir előtti swap állapotok hello azonos két üzembe helyezési ponti azonnal csere által.</span><span class="sxs-lookup"><span data-stu-id="e4016-208">If any errors are identified in production after a slot swap, roll hello slots back tootheir pre-swap states by swapping hello same two slots immediately.</span></span>

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a><span data-ttu-id="e4016-209">Egyéni bemelegítési swap előtt</span><span class="sxs-lookup"><span data-stu-id="e4016-209">Custom warm-up before swap</span></span>
<span data-ttu-id="e4016-210">Néhány alkalmazás egyéni bemelegítési műveletek lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="e4016-210">Some apps may require custom warm-up actions.</span></span> <span data-ttu-id="e4016-211">Hello `applicationInitialization` konfigurációs elem a Web.config fájlban lehetővé teszi, hogy toospecify egyéni inicializálási műveletek toobe hajtani egy kérelem érkezik.</span><span class="sxs-lookup"><span data-stu-id="e4016-211">hello `applicationInitialization` configuration element in web.config allows you toospecify custom initialization actions toobe performed before a request is received.</span></span> <span data-ttu-id="e4016-212">hello cserélő művelet várakozik a egyéni bemelegítési toocomplete.</span><span class="sxs-lookup"><span data-stu-id="e4016-212">hello swap operation will wait for this custom warm-up toocomplete.</span></span> <span data-ttu-id="e4016-213">Íme egy minta web.config töredéket.</span><span class="sxs-lookup"><span data-stu-id="e4016-213">Here is a sample web.config fragment.</span></span>

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="toodelete-a-deployment-slot"></a><span data-ttu-id="e4016-214">toodelete egy üzembe helyezési tárhelyet</span><span class="sxs-lookup"><span data-stu-id="e4016-214">toodelete a deployment slot</span></span>
<span data-ttu-id="e4016-215">Egy üzembe helyezési tárhelyet, nyitott hello telepített környezet tárolóhelye panelen hello paneljén kattintson **áttekintése** (hello alapértelmezett oldal), és kattintson a **törlése** hello parancs sávon.</span><span class="sxs-lookup"><span data-stu-id="e4016-215">In hello blade for a deployment slot, open hello deployment slot's blade, click **Overview** (hello default page), and click **Delete** in hello command bar.</span></span>  

![Egy üzembe helyezési pont törlése][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a><span data-ttu-id="e4016-217">Az üzembe helyezési tárhely az Azure PowerShell-parancsmagjai</span><span class="sxs-lookup"><span data-stu-id="e4016-217">Azure PowerShell cmdlets for deployment slots</span></span>
<span data-ttu-id="e4016-218">Az Azure PowerShell egy modul, amely a parancsmagok toomanage Azure támogatják az Azure App Service üzembe helyezési kezelése Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="e4016-218">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell, including support for managing deployment slots in Azure App Service.</span></span>

* <span data-ttu-id="e4016-219">A telepítése és konfigurálása az Azure PowerShell és az Azure PowerShell hitelesítéséhez az Azure előfizetéssel rendelkező információkért lásd: [hogyan tooinstall, és konfigurálja a Microsoft Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e4016-219">For information on installing and configuring Azure PowerShell, and on authenticating Azure PowerShell with your Azure subscription, see [How tooinstall and configure Microsoft Azure PowerShell](/powershell/azure/overview).</span></span>  

- - -
### <a name="create-a-web-app"></a><span data-ttu-id="e4016-220">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4016-220">Create a web app</span></span>
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a><span data-ttu-id="e4016-221">Egy üzembe helyezési tárhely létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4016-221">Create a deployment slot</span></span>
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-toosource-slot"></a><span data-ttu-id="e4016-222">Indítson el egy felcserélés felülvizsgálati (több fázisban swap) és tárhely – konfiguráció toosource céltárolóhelyen alkalmazása</span><span class="sxs-lookup"><span data-stu-id="e4016-222">Initiate a swap with review (multi-phase swap) and apply destination slot configuration toosource slot</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a><span data-ttu-id="e4016-223">Megszakítja a függőben lévő felcserélés (felülvizsgálati felcserélés) és a forrás helyezési pont konfigurációjának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="e4016-223">Cancel a pending swap (swap with review) and restore source slot configuration</span></span>
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a><span data-ttu-id="e4016-224">Központi telepítés felcserélése</span><span class="sxs-lookup"><span data-stu-id="e4016-224">Swap deployment slots</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a><span data-ttu-id="e4016-225">Üzembe helyezési pont törlése</span><span class="sxs-lookup"><span data-stu-id="e4016-225">Delete deployment slot</span></span>
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a><span data-ttu-id="e4016-226">Az üzembe helyezési tárhely az Azure parancssori felület (CLI) parancsok</span><span class="sxs-lookup"><span data-stu-id="e4016-226">Azure Command-Line Interface (Azure CLI) commands for Deployment Slots</span></span>
<span data-ttu-id="e4016-227">hello Azure CLI-platformok parancsokat biztosít működik-e az Azure-ral, hiszen támogatják az App Service üzembe helyezési pontok kezelése.</span><span class="sxs-lookup"><span data-stu-id="e4016-227">hello Azure CLI provides cross-platform commands for working with Azure, including support for managing App Service deployment slots.</span></span>

* <span data-ttu-id="e4016-228">Az utasításokat a telepítése és konfigurálása az Azure parancssori felület hello, beleértve a vonatkozó tooconnect Azure CLI tooyour Azure-előfizetéssel, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="e4016-228">For instructions on installing and configuring hello Azure CLI, including information on how tooconnect Azure CLI tooyour Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="e4016-229">hello Azure CLI-t, az Azure App Service szolgáltatásban elérhető toolist hello parancsokat hívás `azure site -h`.</span><span class="sxs-lookup"><span data-stu-id="e4016-229">toolist hello commands available for Azure App Service in hello Azure CLI, call `azure site -h`.</span></span>

> [!NOTE] 
> <span data-ttu-id="e4016-230">A [Azure CLI 2.0](https://github.com/Azure/azure-cli) parancsok a üzembe helyezési, lásd: [az App Service web üzembe helyezési pont](/cli/azure/appservice/web/deployment/slot).</span><span class="sxs-lookup"><span data-stu-id="e4016-230">For [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands for deployment slots, see [az appservice web deployment slot](/cli/azure/appservice/web/deployment/slot).</span></span>

- - -
### <a name="azure-site-list"></a><span data-ttu-id="e4016-231">az Azure webhely-lista</span><span class="sxs-lookup"><span data-stu-id="e4016-231">azure site list</span></span>
<span data-ttu-id="e4016-232">Ebben az előfizetésben hello hello alkalmazásokkal kapcsolatos információk hívás **azure webhelylista**, a következő példa hello a.</span><span class="sxs-lookup"><span data-stu-id="e4016-232">For information about hello apps in hello current subscription, call **azure site list**, as in hello following example.</span></span>

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a><span data-ttu-id="e4016-233">az Azure-webhely létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4016-233">azure site create</span></span>
<span data-ttu-id="e4016-234">a telepített környezet tárolóhelye toocreate hívja **azure-webhely létrehozása** és adja meg egy meglévő alkalmazást hello nevét és hello tárolóhely toocreate, mint például a következő hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="e4016-234">toocreate a deployment slot, call **azure site create** and specify hello name of an existing app and hello name of hello slot toocreate, as in hello following example.</span></span>

`azure site create webappslotstest --slot staging`

<span data-ttu-id="e4016-235">hello új tárhely, használjon hello tooenable verziókövetését **– git** lehetőség, mint például a következő hello.</span><span class="sxs-lookup"><span data-stu-id="e4016-235">tooenable source control for hello new slot, use hello **--git** option, as in hello following example.</span></span>

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a><span data-ttu-id="e4016-236">az Azure site lapozófájl-kapacitás</span><span class="sxs-lookup"><span data-stu-id="e4016-236">azure site swap</span></span>
<span data-ttu-id="e4016-237">toomake hello frissített telepítési tárolóhely hello éles alkalmazások, használja a hello **azure hely swap** parancs tooperform csereművelet, mint például a következő hello.</span><span class="sxs-lookup"><span data-stu-id="e4016-237">toomake hello updated deployment slot hello production app, use hello **azure site swap** command tooperform a swap operation, as in hello following example.</span></span> <span data-ttu-id="e4016-238">hello éles alkalmazások nem fog tapasztalni bármely állásidő, és nem fog változni hidegindítás.</span><span class="sxs-lookup"><span data-stu-id="e4016-238">hello production app will not experience any down time, nor will it undergo a cold start.</span></span>

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a><span data-ttu-id="e4016-239">az Azure webhely törlése</span><span class="sxs-lookup"><span data-stu-id="e4016-239">azure site delete</span></span>
<span data-ttu-id="e4016-240">toodelete üzembe helyezési pont, amely már nem szükséges, használjon hello **azure webhely törlése** parancs, mint például a következő hello.</span><span class="sxs-lookup"><span data-stu-id="e4016-240">toodelete a deployment slot that is no longer needed, use hello **azure site delete** command, as in hello following example.</span></span>

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> <span data-ttu-id="e4016-241">Tekintsen meg működés közben egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e4016-241">See a web app in action.</span></span> <span data-ttu-id="e4016-242">[App Service kipróbálása](https://azure.microsoft.com/try/app-service/) azonnal, és hozzon létre egy rövid élettartamú alapszintű alkalmazást – nincs szükség bankkártyára, nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="e4016-242">[Try App Service](https://azure.microsoft.com/try/app-service/) immediately and create a short-lived starter app—no credit card required, no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="e4016-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4016-243">Next Steps</span></span>
<span data-ttu-id="e4016-244">[Az Azure App Service Web App – blokkolása web access toonon éles üzembe helyezési](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[bemutatása tooApp szolgáltatás Linux](./app-service-linux-intro.md)
[Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="e4016-244">[Azure App Service Web App – block web access toonon-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Introduction tooApp Service on Linux](./app-service-linux-intro.md)
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

