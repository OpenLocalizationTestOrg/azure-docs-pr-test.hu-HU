---
title: "aaaCommon felhőalapú szolgáltatás felügyeleti feladatai |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage cloud services – a hello Azure-portálon. Ezekben a példákban hello Azure-portálon."
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
ms.openlocfilehash: ade8a18a7754edbaae4903230c26c009fef63ed7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-cloud-services"></a><span data-ttu-id="1b790-104">TooManage Cloud Services</span><span class="sxs-lookup"><span data-stu-id="1b790-104">How tooManage Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1b790-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1b790-105">Azure portal</span></span>](cloud-services-how-to-manage-portal.md)
> * [<span data-ttu-id="1b790-106">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="1b790-106">Azure classic portal</span></span>](cloud-services-how-to-manage.md)
>
>

<span data-ttu-id="1b790-107">A hello **Felhőszolgáltatások (klasszikus)** területén hello Azure portál, lehet frissíteni a szerepkör-szolgáltatás vagy a központi telepítés, előléptetni egy szakaszos telepítés tooproduction, hivatkozás erőforrások tooyour felhőalapú szolgáltatás, így megtekintheti a hello erőforrás függőségek méretezési erőforrások hello együtt és egy felhőalapú szolgáltatás, vagy a központi telepítés törlése.</span><span class="sxs-lookup"><span data-stu-id="1b790-107">In hello **Cloud Services (classic)** area of hello Azure portal, you can update a service role or a deployment, promote a staged deployment tooproduction, link resources tooyour cloud service so that you can see hello resource dependencies and scale hello resources together, and delete a cloud service or a deployment.</span></span>

<span data-ttu-id="1b790-108">További információ arról, hogyan tooscale a felhőalapú szolgáltatás elérhető [Itt](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1b790-108">More information about how tooscale your cloud service is available [here](cloud-services-how-to-scale-portal.md).</span></span>

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a><span data-ttu-id="1b790-109">Hogyan: Felhő szerepkör-szolgáltatás vagy a központi telepítés</span><span class="sxs-lookup"><span data-stu-id="1b790-109">How to: Update a cloud service role or deployment</span></span>
<span data-ttu-id="1b790-110">Ha a felhőszolgáltatás tooupdate hello alkalmazáskód kell, akkor **frissítés** hello cloud service panelen.</span><span class="sxs-lookup"><span data-stu-id="1b790-110">If you need tooupdate hello application code for your cloud service, use **Update** on hello cloud service blade.</span></span> <span data-ttu-id="1b790-111">Egyetlen szerepkör vagy az összes szerepkör frissítheti.</span><span class="sxs-lookup"><span data-stu-id="1b790-111">You can update a single role or all roles.</span></span> <span data-ttu-id="1b790-112">tooupdate, feltöltheti egy új service-csomag vagy a szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="1b790-112">tooupdate, you can upload a new service package or service configuration file.</span></span>

1. <span data-ttu-id="1b790-113">A hello [Azure-portálon][Azure portal], válassza ki a kívánt tooupdate hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1b790-113">In hello [Azure portal][Azure portal], select hello cloud service you want tooupdate.</span></span> <span data-ttu-id="1b790-114">Ez a lépés hello cloud service példány paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1b790-114">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="1b790-115">Hello paneljén kattintson hello **frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="1b790-115">In hello blade, click hello **Update** button.</span></span>

    ![Frissítés gomb](./media/cloud-services-how-to-manage-portal/update-button.png)

3. <span data-ttu-id="1b790-117">Hello üzemelő példány frissítése egy új felhőszolgáltatás csomagfájlját (.cspkg) és a szolgáltatás konfigurációs fájlját (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="1b790-117">Update hello deployment with a new service package file (.cspkg) and service configuration file (.cscfg).</span></span>

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. <span data-ttu-id="1b790-119">**Opcionálisan** hello üzemelő példány címkéje és hello storage-fiók frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1b790-119">**Optionally** update hello deployment label and hello storage account.</span></span>
5. <span data-ttu-id="1b790-120">Ha a szerepkörök csak egy szerepkör példánya, válassza ki a hello **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** tooenable hello frissítési tooproceed.</span><span class="sxs-lookup"><span data-stu-id="1b790-120">If any roles have only one role instance, select hello **Deploy even if one or more roles contain a single instance** tooenable hello upgrade tooproceed.</span></span>

    <span data-ttu-id="1b790-121">Azure is csak 99,95 % szolgáltatás rendelkezésre állásának biztosítása a felhőalapú szolgáltatás frissítése közben. Ha szerepkörönként legalább két szerepkörpéldányokat (virtuális gépek).</span><span class="sxs-lookup"><span data-stu-id="1b790-121">Azure can only guarantee 99.95 percent service availability during a cloud service update if each role has at least two role instances (virtual machines).</span></span> <span data-ttu-id="1b790-122">Két szerepkör osztályt egy virtuális gép ügyfél kérelmeket dolgozza fel más hello frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="1b790-122">With two role instances, one virtual machine processes client requests while hello other is updated.</span></span>

6. <span data-ttu-id="1b790-123">Ellenőrizze **indítsa el a központi telepítési** toohave hello frissítés alkalmazása hello csomag hello feltöltés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="1b790-123">Check **Start deployment** toohave hello update applied after hello upload of hello package has finished.</span></span>
7. <span data-ttu-id="1b790-124">Kattintson a **OK** toobegin hello szolgáltatás frissítése.</span><span class="sxs-lookup"><span data-stu-id="1b790-124">Click **OK** toobegin updating hello service.</span></span>

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a><span data-ttu-id="1b790-125">Hogyan: felcserélése központi telepítések toopromote egy szakaszos telepítés tooproduction</span><span class="sxs-lookup"><span data-stu-id="1b790-125">How to: Swap deployments toopromote a staged deployment tooproduction</span></span>
<span data-ttu-id="1b790-126">Ha egy felhőalapú szolgáltatás, a szakasz az új kiadási toodeploy döntse el, és tesztelje az új kiadási a felhőalapú szolgáltatás tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1b790-126">When you decide toodeploy a new release of a cloud service, stage and test your new release in your cloud service staging environment.</span></span> <span data-ttu-id="1b790-127">Használjon **felcserélése** tooswitch hello URL-címek mely hello két központi telepítések tárgyalja, és lépteti elő egy új kiadási tooproduction.</span><span class="sxs-lookup"><span data-stu-id="1b790-127">Use **Swap** tooswitch hello URLs by which hello two deployments are addressed and promote a new release tooproduction.</span></span>

<span data-ttu-id="1b790-128">Hello telepítéseit kicserélheti **Felhőszolgáltatások** lap vagy hello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="1b790-128">You can swap deployments from hello **Cloud Services** page or hello dashboard.</span></span>

1. <span data-ttu-id="1b790-129">A hello [Azure-portálon][Azure portal], válassza ki a kívánt tooupdate hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1b790-129">In hello [Azure portal][Azure portal], select hello cloud service you want tooupdate.</span></span> <span data-ttu-id="1b790-130">Ez a lépés hello cloud service példány paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1b790-130">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="1b790-131">Hello paneljén kattintson hello **felcserélése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1b790-131">In hello blade, click hello **Swap** button.</span></span>

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. <span data-ttu-id="1b790-133">hello következő megerősítési kérés nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="1b790-133">hello following confirmation prompt opens.</span></span>

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. <span data-ttu-id="1b790-135">Miután meggyőződött hello információkat, kattintson a **OK** tooswap hello központi telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="1b790-135">After you verify hello deployment information, click **OK** tooswap hello deployments.</span></span>

    <span data-ttu-id="1b790-136">hello telepítési swap gyorsan mert megváltoztató egyedül hello hello virtuális IP-címek (VIP) hello telepítésekhez.</span><span class="sxs-lookup"><span data-stu-id="1b790-136">hello deployment swap happens quickly because hello only thing that changes is hello virtual IP addresses (VIPs) for hello deployments.</span></span>

    <span data-ttu-id="1b790-137">toosave számítási költségek, a központi telepítés átmeneti, miután ellenőrizte, hogy a várt módon működik-e az éles telepítési hello törölheti.</span><span class="sxs-lookup"><span data-stu-id="1b790-137">toosave compute costs, you can delete hello staging deployment after you verify that your production deployment is working as expected.</span></span>

### <a name="common-questions-about-swapping-deployments"></a><span data-ttu-id="1b790-138">Központi telepítések csere kapcsolatos gyakori kérdésekre</span><span class="sxs-lookup"><span data-stu-id="1b790-138">Common questions about swapping deployments</span></span>

<span data-ttu-id="1b790-139">**Mik azok a telepítések csere hello előfeltételei**</span><span class="sxs-lookup"><span data-stu-id="1b790-139">**What are hello prerequisites for swapping deployments?**</span></span>

<span data-ttu-id="1b790-140">Nincsenek a sikeres telepítés swap két fő előfeltételei:</span><span class="sxs-lookup"><span data-stu-id="1b790-140">There are two key prerequisites for a successful deployment swap:</span></span>

- <span data-ttu-id="1b790-141">Ha azt szeretné, hogy egy statikus IP-cím toouse a termelési aljzat, kell lefoglalni egy a, valamint az átmeneti helyet.</span><span class="sxs-lookup"><span data-stu-id="1b790-141">If you would like toouse a static IP address for your production slot, you must reserve one for your staging slot as well.</span></span> <span data-ttu-id="1b790-142">Ellenkező esetben hello swap meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="1b790-142">Otherwise, hello swap fails.</span></span>

- <span data-ttu-id="1b790-143">A szerepkörök az összes példányát kell futnia, hello swap elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="1b790-143">All instances of your roles must be running before you can perform hello swap.</span></span> <span data-ttu-id="1b790-144">Ellenőrizheti a hello Azure-portálon hello áttekintése panel példányai hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="1b790-144">You can check hello status of your instances in hello overview blade of hello Azure portal.</span></span> <span data-ttu-id="1b790-145">Másik lehetőségként használhatja a hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) Windows PowerShell parancsot.</span><span class="sxs-lookup"><span data-stu-id="1b790-145">Alternatively, you can use hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) command in Windows PowerShell.</span></span>

<span data-ttu-id="1b790-146">Vegye figyelembe, hogy a vendég operációs rendszer frissítése és a szolgáltatás javító műveleteket is eredményezheti, hogy központi telepítési toofail cseréje.</span><span class="sxs-lookup"><span data-stu-id="1b790-146">Note that Guest OS updates and service healing operations can also cause deployment swaps toofail.</span></span> <span data-ttu-id="1b790-147">További információkért lásd: [felhőalapú szolgáltatás központi telepítési problémák elhárítása](cloud-services-troubleshoot-deployment-problems.md).</span><span class="sxs-lookup"><span data-stu-id="1b790-147">For more information, see [Troubleshoot cloud service deployment problems](cloud-services-troubleshoot-deployment-problems.md).</span></span>

<span data-ttu-id="1b790-148">**A lapozófájl-kapacitás negatívan befolyásolja az alkalmazáshoz állásidő? Hogyan tudom kezelje azt?**</span><span class="sxs-lookup"><span data-stu-id="1b790-148">**Does a swap incur downtime for my application? How should I handle it?**</span></span>

<span data-ttu-id="1b790-149">Hello utolsó szakaszban leírt módon egy központi telepítési felcserélés, általában gyors, mert csak egy hello Azure terheléselosztó a konfiguráció megváltozott.</span><span class="sxs-lookup"><span data-stu-id="1b790-149">As described in hello last section, a deployment swap is typically fast since it is just a configuration change in hello Azure load balancer.</span></span> <span data-ttu-id="1b790-150">Néhány esetben azonban képes tíz vagy több másodpercre és átmeneti kapcsolati hibákat eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="1b790-150">In some cases, however, it can take ten or more seconds and result in transient connection failures.</span></span> <span data-ttu-id="1b790-151">toolimit hatás tooyour ügyfelek, vegye fontolóra [ügyfél újrapróbálkozási logika](../best-practices-retry-general.md).</span><span class="sxs-lookup"><span data-stu-id="1b790-151">toolimit impact tooyour customers, consider implementing [client retry logic](../best-practices-retry-general.md).</span></span>

## <a name="how-to-link-a-resource-tooa-cloud-service"></a><span data-ttu-id="1b790-152">Útmutató: egy erőforrás tooa felhőalapú szolgáltatás csatolása</span><span class="sxs-lookup"><span data-stu-id="1b790-152">How to: Link a resource tooa cloud service</span></span>
<span data-ttu-id="1b790-153">hello Azure-portálon csatolja erőforrások együtt, például does hello aktuális klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="1b790-153">hello Azure portal does not link resources together like hello current Azure classic portal does.</span></span> <span data-ttu-id="1b790-154">Ehelyett a további erőforrások toohello telepíteni ugyanabban az hello felhőalapú szolgáltatás által használt erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="1b790-154">Instead, deploy additional resources toohello same resource group being used by hello Cloud Service.</span></span>

## <a name="how-to-delete-deployments-and-a-cloud-service"></a><span data-ttu-id="1b790-155">Útmutató: egy felhőalapú szolgáltatás és a központi telepítései törlése</span><span class="sxs-lookup"><span data-stu-id="1b790-155">How to: Delete deployments and a cloud service</span></span>
<span data-ttu-id="1b790-156">Egy felhőalapú szolgáltatás törlése előtt törölni kell minden meglévő telepítés.</span><span class="sxs-lookup"><span data-stu-id="1b790-156">Before you can delete a cloud service, you must delete each existing deployment.</span></span>

<span data-ttu-id="1b790-157">toosave számítási költségek, a központi telepítés átmeneti, miután ellenőrizte, hogy a várt módon működik-e az éles telepítési hello törölheti.</span><span class="sxs-lookup"><span data-stu-id="1b790-157">toosave compute costs, you can delete hello staging deployment after you verify that your production deployment is working as expected.</span></span> <span data-ttu-id="1b790-158">Le lett állítva telepített szerepkör-példányok számítási költségek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="1b790-158">You are billed for compute costs for deployed role instances that are stopped.</span></span>

<span data-ttu-id="1b790-159">A következő eljárás toodelete hello használja, a központi telepítés vagy a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1b790-159">Use hello following procedure toodelete a deployment or your cloud service.</span></span>

1. <span data-ttu-id="1b790-160">A hello [Azure-portálon][Azure portal], válassza ki a kívánt toodelete hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1b790-160">In hello [Azure portal][Azure portal], select hello cloud service you want toodelete.</span></span> <span data-ttu-id="1b790-161">Ez a lépés hello cloud service példány paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1b790-161">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="1b790-162">Hello paneljén kattintson hello **törlése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1b790-162">In hello blade, click hello **Delete** button.</span></span>

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. <span data-ttu-id="1b790-164">Törölheti a hello teljes felhőalapú szolgáltatás ellenőrzésével **felhőalapú szolgáltatás, és a központi telepítései** , vagy válasszon vagy hello **éles környezet** vagy hello **történő üzembe helyezése**.</span><span class="sxs-lookup"><span data-stu-id="1b790-164">You can delete hello entire cloud service by checking **Cloud service and its deployments** or choose either hello **Production deployment** or hello **Staging deployment**.</span></span>

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. <span data-ttu-id="1b790-166">Kattintson a hello **törlése** hello alsó gombra.</span><span class="sxs-lookup"><span data-stu-id="1b790-166">Click hello **Delete** button at hello bottom.</span></span>
5. <span data-ttu-id="1b790-167">toodelete hello felhőalapú szolgáltatás, kattintson a **törlése a felhőalapú szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="1b790-167">toodelete hello cloud service, click **Delete cloud service**.</span></span> <span data-ttu-id="1b790-168">Ezután hello megerősítő kérdés, kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="1b790-168">Then, at hello confirmation prompt, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="1b790-169">A felhőszolgáltatást törölték, és részletes figyelési van beállítva, akkor törölni kell hello adatok manuálisan a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="1b790-169">When a cloud service is deleted, and verbose monitoring is configured, you must delete hello data manually from your storage account.</span></span> <span data-ttu-id="1b790-170">Ha toofind hello metrikák táblák kapcsolatos információkért lásd: [ez](cloud-services-how-to-monitor.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1b790-170">For information about where toofind hello metrics tables, see [this](cloud-services-how-to-monitor.md) article.</span></span>


## <a name="how-to-find-more-information-about-failed-deployments"></a><span data-ttu-id="1b790-171">Hogyan: sikertelen központi telepítéssel kapcsolatban további információt</span><span class="sxs-lookup"><span data-stu-id="1b790-171">How to: Find more information about failed deployments</span></span>
<span data-ttu-id="1b790-172">Hello **áttekintése** panel rendelkezik állapotjelző hello tetején.</span><span class="sxs-lookup"><span data-stu-id="1b790-172">hello **Overview** blade has a status bar at hello top.</span></span> <span data-ttu-id="1b790-173">Ha hello sáv gombra kattint, egy új panel megnyílik, és bármely hibaadatokat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="1b790-173">When you click hello bar, a new blade opens and displays any error information.</span></span> <span data-ttu-id="1b790-174">Ha hello központi telepítési tartalmaz ki a hibákat, hello információk panel nem üres.</span><span class="sxs-lookup"><span data-stu-id="1b790-174">If hello deployment does not contain any errors, hello information blade is blank.</span></span>

![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a><span data-ttu-id="1b790-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b790-176">Next steps</span></span>
* <span data-ttu-id="1b790-177">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1b790-177">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="1b790-178">Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1b790-178">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="1b790-179">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1b790-179">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="1b790-180">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1b790-180">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
