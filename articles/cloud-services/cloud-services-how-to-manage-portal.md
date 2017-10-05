---
title: "Cloud service általános felügyeleti feladatai |} Microsoft Docs"
description: "Útmutató az Azure portálon felhőszolgáltatások kezelése. Ezekben a példákban az Azure-portálon."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 4650cebe18153e3b10bbec685a66a590348c99e9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-manage-cloud-services"></a><span data-ttu-id="27242-104">Cloud Services kezelése</span><span class="sxs-lookup"><span data-stu-id="27242-104">How to Manage Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="27242-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="27242-105">Azure portal</span></span>](cloud-services-how-to-manage-portal.md)
> * [<span data-ttu-id="27242-106">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="27242-106">Azure classic portal</span></span>](cloud-services-how-to-manage.md)
>
>

<span data-ttu-id="27242-107">Az a **Felhőszolgáltatások (klasszikus)** területén az Azure portál, a szerepkör-szolgáltatás vagy a központi telepítés frissítése, üzemi szakaszos telepítés előléptetéséhez is erőforrások csatolása a felhőalapú szolgáltatás, hogy tekintse meg az erőforrás-függőségek és az erőforrások együtt méretezhető, és egy felhőalapú szolgáltatás, vagy a központi telepítés törlése.</span><span class="sxs-lookup"><span data-stu-id="27242-107">In the **Cloud Services (classic)** area of the Azure portal, you can update a service role or a deployment, promote a staged deployment to production, link resources to your cloud service so that you can see the resource dependencies and scale the resources together, and delete a cloud service or a deployment.</span></span>

<span data-ttu-id="27242-108">További információk a felhőalapú szolgáltatás méretezése [Itt](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="27242-108">More information about how to scale your cloud service is available [here](cloud-services-how-to-scale-portal.md).</span></span>

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a><span data-ttu-id="27242-109">Hogyan: Felhő szerepkör-szolgáltatás vagy a központi telepítés</span><span class="sxs-lookup"><span data-stu-id="27242-109">How to: Update a cloud service role or deployment</span></span>
<span data-ttu-id="27242-110">Ha az alkalmazás kódjának a felhőszolgáltatás frissíteni kell, **frissítése** a felhőalapú szolgáltatás panelen.</span><span class="sxs-lookup"><span data-stu-id="27242-110">If you need to update the application code for your cloud service, use **Update** on the cloud service blade.</span></span> <span data-ttu-id="27242-111">Egyetlen szerepkör vagy az összes szerepkör frissítheti.</span><span class="sxs-lookup"><span data-stu-id="27242-111">You can update a single role or all roles.</span></span> <span data-ttu-id="27242-112">Frissítéséhez feltöltheti egy új service-csomag vagy a szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="27242-112">To update, you can upload a new service package or service configuration file.</span></span>

1. <span data-ttu-id="27242-113">Az a [Azure-portálon][Azure portal], válassza ki a frissíteni kívánt felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="27242-113">In the [Azure portal][Azure portal], select the cloud service you want to update.</span></span> <span data-ttu-id="27242-114">Ez a lépés a felhőalapú szolgáltatás példány paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="27242-114">This step opens the cloud service instance blade.</span></span>
2. <span data-ttu-id="27242-115">A panelen kattintson a **frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="27242-115">In the blade, click the **Update** button.</span></span>

    ![Frissítés gomb](./media/cloud-services-how-to-manage-portal/update-button.png)

3. <span data-ttu-id="27242-117">Az üzemelő példány frissítése egy új felhőszolgáltatás csomagfájlját (.cspkg) és a szolgáltatás konfigurációs fájlját (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="27242-117">Update the deployment with a new service package file (.cspkg) and service configuration file (.cscfg).</span></span>

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. <span data-ttu-id="27242-119">**Opcionálisan** frissíteni az üzemelő példány címkéje és a storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="27242-119">**Optionally** update the deployment label and the storage account.</span></span>
5. <span data-ttu-id="27242-120">Ha a szerepkörök csak egy szerepkör példánya, válassza ki a **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** ahhoz, hogy a frissítés gombra.</span><span class="sxs-lookup"><span data-stu-id="27242-120">If any roles have only one role instance, select the **Deploy even if one or more roles contain a single instance** to enable the upgrade to proceed.</span></span>

    <span data-ttu-id="27242-121">Azure is csak 99,95 % szolgáltatás rendelkezésre állásának biztosítása a felhőalapú szolgáltatás frissítése közben. Ha szerepkörönként legalább két szerepkörpéldányokat (virtuális gépek).</span><span class="sxs-lookup"><span data-stu-id="27242-121">Azure can only guarantee 99.95 percent service availability during a cloud service update if each role has at least two role instances (virtual machines).</span></span> <span data-ttu-id="27242-122">Két szerepkör osztályt egy virtuális gép ügyfél kérelmeket dolgozza fel a másik van frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="27242-122">With two role instances, one virtual machine processes client requests while the other is updated.</span></span>

6. <span data-ttu-id="27242-123">Ellenőrizze **indítsa el a központi telepítés** szeretné, hogy a frissítés telepítése a csomag a feltöltés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="27242-123">Check **Start deployment** to have the update applied after the upload of the package has finished.</span></span>
7. <span data-ttu-id="27242-124">Kattintson a **OK** megkezdéséhez, a szolgáltatás frissítése.</span><span class="sxs-lookup"><span data-stu-id="27242-124">Click **OK** to begin updating the service.</span></span>

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a><span data-ttu-id="27242-125">Hogyan: felcserélése előléptetése üzemi szakaszos telepítés központi telepítések</span><span class="sxs-lookup"><span data-stu-id="27242-125">How to: Swap deployments to promote a staged deployment to production</span></span>
<span data-ttu-id="27242-126">Ha úgy dönt, hogy egy felhőalapú szolgáltatás, a szakasz az új verziót telepítse és tesztelje a az új verziót a felhőalapú szolgáltatás tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="27242-126">When you decide to deploy a new release of a cloud service, stage and test your new release in your cloud service staging environment.</span></span> <span data-ttu-id="27242-127">Használjon **felcserélése** Váltás az URL-címeket, amellyel a két központi telepítések tárgyalja, és előléptetése üzemi új verziót.</span><span class="sxs-lookup"><span data-stu-id="27242-127">Use **Swap** to switch the URLs by which the two deployments are addressed and promote a new release to production.</span></span>

<span data-ttu-id="27242-128">A központi telepítések kicserélheti a **Felhőszolgáltatások** lap vagy az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="27242-128">You can swap deployments from the **Cloud Services** page or the dashboard.</span></span>

1. <span data-ttu-id="27242-129">Az a [Azure-portálon][Azure portal], válassza ki a frissíteni kívánt felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="27242-129">In the [Azure portal][Azure portal], select the cloud service you want to update.</span></span> <span data-ttu-id="27242-130">Ez a lépés a felhőalapú szolgáltatás példány paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="27242-130">This step opens the cloud service instance blade.</span></span>
2. <span data-ttu-id="27242-131">A panelen kattintson a **felcserélése** gombra.</span><span class="sxs-lookup"><span data-stu-id="27242-131">In the blade, click the **Swap** button.</span></span>

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. <span data-ttu-id="27242-133">A következő megerősítő kérdés megjelenik.</span><span class="sxs-lookup"><span data-stu-id="27242-133">The following confirmation prompt opens.</span></span>

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. <span data-ttu-id="27242-135">Miután meggyőződött a központi telepítési információkat, kattintson a **OK** felcserélni a központi telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="27242-135">After you verify the deployment information, click **OK** to swap the deployments.</span></span>

    <span data-ttu-id="27242-136">A központi telepítés felcserélés gyorsan mert, hogy módosítja a virtuális IP-címek (VIP) a telepítések.</span><span class="sxs-lookup"><span data-stu-id="27242-136">The deployment swap happens quickly because the only thing that changes is the virtual IP addresses (VIPs) for the deployments.</span></span>

    <span data-ttu-id="27242-137">Számítási költségek csökkentése érdekében, hogy a várt módon működik-e az éles telepítési ellenőrzése után törölheti a átmeneti központi telepítési.</span><span class="sxs-lookup"><span data-stu-id="27242-137">To save compute costs, you can delete the staging deployment after you verify that your production deployment is working as expected.</span></span>

### <a name="common-questions-about-swapping-deployments"></a><span data-ttu-id="27242-138">Központi telepítések csere kapcsolatos gyakori kérdésekre</span><span class="sxs-lookup"><span data-stu-id="27242-138">Common questions about swapping deployments</span></span>

<span data-ttu-id="27242-139">**Mik azok a telepítések csere előfeltételei**</span><span class="sxs-lookup"><span data-stu-id="27242-139">**What are the prerequisites for swapping deployments?**</span></span>

<span data-ttu-id="27242-140">Nincsenek a sikeres telepítés swap két fő előfeltételei:</span><span class="sxs-lookup"><span data-stu-id="27242-140">There are two key prerequisites for a successful deployment swap:</span></span>

- <span data-ttu-id="27242-141">Ha szeretné használni az éles tárolóhelyre egy statikus IP-címet, kell lefoglalni egy a, valamint az átmeneti helyet.</span><span class="sxs-lookup"><span data-stu-id="27242-141">If you would like to use a static IP address for your production slot, you must reserve one for your staging slot as well.</span></span> <span data-ttu-id="27242-142">Ellenkező esetben a felcserélés meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="27242-142">Otherwise, the swap fails.</span></span>

- <span data-ttu-id="27242-143">A szerepkörök az összes példányát futnia kell a felcserélés elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="27242-143">All instances of your roles must be running before you can perform the swap.</span></span> <span data-ttu-id="27242-144">Az Azure-portálon a áttekintése panel példányának állapotát ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="27242-144">You can check the status of your instances in the overview blade of the Azure portal.</span></span> <span data-ttu-id="27242-145">Másik lehetőségként használhatja a [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) Windows PowerShell parancsot.</span><span class="sxs-lookup"><span data-stu-id="27242-145">Alternatively, you can use the [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) command in Windows PowerShell.</span></span>

<span data-ttu-id="27242-146">Vegye figyelembe, hogy a vendég operációs rendszer frissítése és a szolgáltatás javító műveleteket is okozhat, központi telepítési címek cseréje sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="27242-146">Note that Guest OS updates and service healing operations can also cause deployment swaps to fail.</span></span> <span data-ttu-id="27242-147">További információkért lásd: [felhőalapú szolgáltatás központi telepítési problémák elhárítása](cloud-services-troubleshoot-deployment-problems.md).</span><span class="sxs-lookup"><span data-stu-id="27242-147">For more information, see [Troubleshoot cloud service deployment problems](cloud-services-troubleshoot-deployment-problems.md).</span></span>

<span data-ttu-id="27242-148">**A lapozófájl-kapacitás negatívan befolyásolja az alkalmazáshoz állásidő? Hogyan tudom kezelje azt?**</span><span class="sxs-lookup"><span data-stu-id="27242-148">**Does a swap incur downtime for my application? How should I handle it?**</span></span>

<span data-ttu-id="27242-149">Az utolsó szakaszban leírt módon egy központi telepítési felcserélés, általában gyors, mert csak egy konfigurációs változásokat az Azure load balancer.</span><span class="sxs-lookup"><span data-stu-id="27242-149">As described in the last section, a deployment swap is typically fast since it is just a configuration change in the Azure load balancer.</span></span> <span data-ttu-id="27242-150">Néhány esetben azonban képes tíz vagy több másodpercre és átmeneti kapcsolati hibákat eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="27242-150">In some cases, however, it can take ten or more seconds and result in transient connection failures.</span></span> <span data-ttu-id="27242-151">Hatása az ügyfelek számára korlátozása érdekében vegye fontolóra [ügyfél újrapróbálkozási logika](../best-practices-retry-general.md).</span><span class="sxs-lookup"><span data-stu-id="27242-151">To limit impact to your customers, consider implementing [client retry logic](../best-practices-retry-general.md).</span></span>

## <a name="how-to-link-a-resource-to-a-cloud-service"></a><span data-ttu-id="27242-152">Hogyan: erőforrás összekapcsolása egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="27242-152">How to: Link a resource to a cloud service</span></span>
<span data-ttu-id="27242-153">Az Azure-portál nem összeköt erőforrások, mint a jelenlegi klasszikus Azure portálon does.</span><span class="sxs-lookup"><span data-stu-id="27242-153">The Azure portal does not link resources together like the current Azure classic portal does.</span></span> <span data-ttu-id="27242-154">Ehelyett alkalmazhatja további erőforrásokat a felhőalapú szolgáltatás által használt ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="27242-154">Instead, deploy additional resources to the same resource group being used by the Cloud Service.</span></span>

## <a name="how-to-delete-deployments-and-a-cloud-service"></a><span data-ttu-id="27242-155">Útmutató: egy felhőalapú szolgáltatás és a központi telepítései törlése</span><span class="sxs-lookup"><span data-stu-id="27242-155">How to: Delete deployments and a cloud service</span></span>
<span data-ttu-id="27242-156">Egy felhőalapú szolgáltatás törlése előtt törölni kell minden meglévő telepítés.</span><span class="sxs-lookup"><span data-stu-id="27242-156">Before you can delete a cloud service, you must delete each existing deployment.</span></span>

<span data-ttu-id="27242-157">Számítási költségek csökkentése érdekében, hogy a várt módon működik-e az éles telepítési ellenőrzése után törölheti a átmeneti központi telepítési.</span><span class="sxs-lookup"><span data-stu-id="27242-157">To save compute costs, you can delete the staging deployment after you verify that your production deployment is working as expected.</span></span> <span data-ttu-id="27242-158">Le lett állítva telepített szerepkör-példányok számítási költségek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="27242-158">You are billed for compute costs for deployed role instances that are stopped.</span></span>

<span data-ttu-id="27242-159">Az alábbi eljárás segítségével törölheti a központi telepítés vagy a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="27242-159">Use the following procedure to delete a deployment or your cloud service.</span></span>

1. <span data-ttu-id="27242-160">Az a [Azure-portálon][Azure portal], válassza ki a törölni kívánt felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="27242-160">In the [Azure portal][Azure portal], select the cloud service you want to delete.</span></span> <span data-ttu-id="27242-161">Ez a lépés a felhőalapú szolgáltatás példány paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="27242-161">This step opens the cloud service instance blade.</span></span>
2. <span data-ttu-id="27242-162">A panelen kattintson a **törlése** gombra.</span><span class="sxs-lookup"><span data-stu-id="27242-162">In the blade, click the **Delete** button.</span></span>

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. <span data-ttu-id="27242-164">A teljes körű felhőalapú szolgáltatás törölheti ellenőrzésével **felhőalapú szolgáltatás, és a központi telepítései** vagy válassza a **éles környezet** vagy a **történő üzembe helyezése**.</span><span class="sxs-lookup"><span data-stu-id="27242-164">You can delete the entire cloud service by checking **Cloud service and its deployments** or choose either the **Production deployment** or the **Staging deployment**.</span></span>

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. <span data-ttu-id="27242-166">Kattintson a **törlése** panel alján.</span><span class="sxs-lookup"><span data-stu-id="27242-166">Click the **Delete** button at the bottom.</span></span>
5. <span data-ttu-id="27242-167">A felhőalapú szolgáltatás törléséhez kattintson **törlése a felhőalapú szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="27242-167">To delete the cloud service, click **Delete cloud service**.</span></span> <span data-ttu-id="27242-168">A megerősítést kérő kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="27242-168">Then, at the confirmation prompt, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="27242-169">A felhőszolgáltatást törölték, és részletes figyelési van beállítva, törölnie kell az adatokat kézzel a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="27242-169">When a cloud service is deleted, and verbose monitoring is configured, you must delete the data manually from your storage account.</span></span> <span data-ttu-id="27242-170">Hol található a metrikák táblák kapcsolatos információkért lásd: [ez](cloud-services-how-to-monitor.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="27242-170">For information about where to find the metrics tables, see [this](cloud-services-how-to-monitor.md) article.</span></span>


## <a name="how-to-find-more-information-about-failed-deployments"></a><span data-ttu-id="27242-171">Hogyan: sikertelen központi telepítéssel kapcsolatban további információt</span><span class="sxs-lookup"><span data-stu-id="27242-171">How to: Find more information about failed deployments</span></span>
<span data-ttu-id="27242-172">A **áttekintése** panel rendelkezik állapotjelző tetején.</span><span class="sxs-lookup"><span data-stu-id="27242-172">The **Overview** blade has a status bar at the top.</span></span> <span data-ttu-id="27242-173">A elemre, ha egy új panel nyílik meg, és bármely hibaadatokat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="27242-173">When you click the bar, a new blade opens and displays any error information.</span></span> <span data-ttu-id="27242-174">Ha az üzemelő példány nem tartalmaz ki a hibákat, az információ panel érték üres.</span><span class="sxs-lookup"><span data-stu-id="27242-174">If the deployment does not contain any errors, the information blade is blank.</span></span>

![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a><span data-ttu-id="27242-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="27242-176">Next steps</span></span>
* <span data-ttu-id="27242-177">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="27242-177">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="27242-178">Megtudhatja, hogyan [felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="27242-178">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="27242-179">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="27242-179">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="27242-180">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="27242-180">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
