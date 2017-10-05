---
title: "Az Azure Batch kvótái és korlátai szolgáltatás |} Microsoft Docs"
description: "További tudnivalók az alapértelmezett Azure Batch kvóták, korlátozások és megkötések-re, arról, hogyan kérhet kvóta"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3f69ed8d3a985afe07e648e7512a88b25278ced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="b56b7-103">A Bach szolgáltatás kvótái és korlátozásai</span><span class="sxs-lookup"><span data-stu-id="b56b7-103">Batch service quotas and limits</span></span>

<span data-ttu-id="b56b7-104">Mint az egyéb Azure-szolgáltatásokkal, nincsenek korlátozások bizonyos erőforrások, a Batch szolgáltatás társított.</span><span class="sxs-lookup"><span data-stu-id="b56b7-104">As with other Azure services, there are limits on certain resources associated with the Batch service.</span></span> <span data-ttu-id="b56b7-105">Ezek a korlátozások számos alapértelmezett kvóták alkalmazása az Azure-ban az előfizetés vagy a fiók szintjén.</span><span class="sxs-lookup"><span data-stu-id="b56b7-105">Many of these limits are default quotas applied by Azure at the subscription or account level.</span></span> <span data-ttu-id="b56b7-106">Ez a cikk ismerteti azokat az alapértelmezett beállításokat, és hogyan kérheti a kvótájának növeli.</span><span class="sxs-lookup"><span data-stu-id="b56b7-106">This article discusses those defaults, and how you can request quota increases.</span></span>

<span data-ttu-id="b56b7-107">Vegye figyelembe ezeket a kvótákat a Batch számítási feladatok tervezésekor és bővítésekor.</span><span class="sxs-lookup"><span data-stu-id="b56b7-107">Keep these quotas in mind as you are designing and scaling up your Batch workloads.</span></span> <span data-ttu-id="b56b7-108">Például a készlet nem eléri a számítási csomópontok a megadott cél száma, ha előfordulhat, hogy elérte a core kvótát a Batch-fiók, vagy a regionális virtuális gép magok kvóta az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="b56b7-108">For example, if your pool isn't reaching the target number of compute nodes you've specified, you might have reached the core quota limit for your Batch account, or a regional VM cores quota for your subscription.</span></span>

<span data-ttu-id="b56b7-109">Több Batch számítási feladatot is futtathat egyetlen Batch-fiókon, de el is oszthatja a számítási feladatokat ugyanazon előfizetéshez, de különböző Azure-régiókhoz tartozó Batch-fiókok között.</span><span class="sxs-lookup"><span data-stu-id="b56b7-109">You can run multiple Batch workloads in a single Batch account, or distribute your workloads among Batch accounts that are in the same subscription, but in different Azure regions.</span></span>

<span data-ttu-id="b56b7-110">Ha szeretné futtatni a termelési számítási feladatokhoz kötegben, szükség lehet egy vagy több, a kvóták fenti az alapértelmezett növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="b56b7-110">If you plan to run production workloads in Batch, you may need to increase one or more of the quotas above the default.</span></span> <span data-ttu-id="b56b7-111">Ha azt szeretné, a kvóta emelése, az online megnyithatja [ügyfél-támogatási kérelem](#increase-a-quota) díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="b56b7-111">If you want to raise a quota, you can open an online [customer support request](#increase-a-quota) at no charge.</span></span>

> [!NOTE]
> <span data-ttu-id="b56b7-112">A kvóta legfeljebb, nem kapacitás garancia.</span><span class="sxs-lookup"><span data-stu-id="b56b7-112">A quota is a credit limit, not a capacity guarantee.</span></span> <span data-ttu-id="b56b7-113">Ha nagyméretű lemezkapacitási igényekről, forduljon az Azure támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="b56b7-113">If you have large-scale capacity needs, please contact Azure support.</span></span>
> 
> 

## <a name="resource-quotas"></a><span data-ttu-id="b56b7-114">Erőforráskvóták</span><span class="sxs-lookup"><span data-stu-id="b56b7-114">Resource quotas</span></span>
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a><span data-ttu-id="b56b7-115">Kvótákat felhasználói előfizetési módban</span><span class="sxs-lookup"><span data-stu-id="b56b7-115">Quotas in user subscription mode</span></span>

<span data-ttu-id="b56b7-116">Tárolókészlet foglalási módban a Batch-fiókhoz **felhasználói előfizetési**, a Batch-virtuális gépek és más erőforrások, például a storage-fiókok, közvetlenül az előfizetésében jönnek létre, amikor egy alkalmazáskészlet jön létre.</span><span class="sxs-lookup"><span data-stu-id="b56b7-116">For a Batch account with pool allocation mode set to **user subscription**, Batch VMs and other resources, such as storage accounts, are created directly in your subscription when a pool is created.</span></span> <span data-ttu-id="b56b7-117">Az Azure Batch magok kvóta nem vonatkozik az ebben a módban létrehozott fiók.</span><span class="sxs-lookup"><span data-stu-id="b56b7-117">The Azure Batch cores quota does not apply to an account created in this mode.</span></span> <span data-ttu-id="b56b7-118">Ehelyett az előfizetéshez tartozó területi kvóták számítási mag, és más erőforrások alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="b56b7-118">Instead, the quotas in your subscription for regional compute cores and other resources are applied.</span></span> <span data-ttu-id="b56b7-119">További információ a ezek mely százalékértékénél kéri [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="b56b7-119">Learn more about these quotas in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="b56b7-120">Egy olyan fiók felhasználói előfizetési módban létrehozott erőforrás-használat tervezésekor vegye figyelembe a következő kötegelt erőforrások (számítási magok) mellett minden 40 Linux virtuális gépet, vagy szükségesek 20 Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="b56b7-120">When planning resource usage for an account created in user subscription mode, note the following Batch resources (in addition to compute cores) are required for every 40 Linux VMs, or 20 Windows VMs:</span></span>

| <span data-ttu-id="b56b7-121">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="b56b7-121">Resource</span></span> | <span data-ttu-id="b56b7-122">Kvóta</span><span class="sxs-lookup"><span data-stu-id="b56b7-122">Quota</span></span> | <span data-ttu-id="b56b7-123">Szolgáltató</span><span class="sxs-lookup"><span data-stu-id="b56b7-123">Provider</span></span> |
| --- | ---| --- |
| <span data-ttu-id="b56b7-124">Egy tárfiók</span><span class="sxs-lookup"><span data-stu-id="b56b7-124">One storage account</span></span> | <span data-ttu-id="b56b7-125">Tárfiókok</span><span class="sxs-lookup"><span data-stu-id="b56b7-125">Storage Accounts</span></span> | <span data-ttu-id="b56b7-126">Microsoft.Storage</span><span class="sxs-lookup"><span data-stu-id="b56b7-126">Microsoft.Storage</span></span> |
| <span data-ttu-id="b56b7-127">Egy nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="b56b7-127">One public IP address</span></span> | <span data-ttu-id="b56b7-128">Nyilvános IP-címek</span><span class="sxs-lookup"><span data-stu-id="b56b7-128">Public IP Addresses</span></span> | <span data-ttu-id="b56b7-129">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="b56b7-129">Microsoft.Network</span></span> | 
| <span data-ttu-id="b56b7-130">Egy virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="b56b7-130">One virtual network</span></span> | <span data-ttu-id="b56b7-131">Virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="b56b7-131">Virtual Networks</span></span> | <span data-ttu-id="b56b7-132">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="b56b7-132">Microsoft.Network</span></span> | 
| <span data-ttu-id="b56b7-133">Egy hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="b56b7-133">One network security group</span></span> | <span data-ttu-id="b56b7-134">Network Security Groups (Hálózati biztonsági csoportok)</span><span class="sxs-lookup"><span data-stu-id="b56b7-134">Network Security Groups</span></span> | <span data-ttu-id="b56b7-135">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="b56b7-135">Microsoft.Network</span></span> | 
| <span data-ttu-id="b56b7-136">Egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="b56b7-136">One virtual machine scale set</span></span> | <span data-ttu-id="b56b7-137">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="b56b7-137">Virtual Machine Scale Sets</span></span> | <span data-ttu-id="b56b7-138">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="b56b7-138">Microsoft.Compute</span></span> | 
| <span data-ttu-id="b56b7-139">Egy terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="b56b7-139">One load balancer</span></span> | <span data-ttu-id="b56b7-140">Terheléselosztók</span><span class="sxs-lookup"><span data-stu-id="b56b7-140">Load Balancers</span></span> | <span data-ttu-id="b56b7-141">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="b56b7-141">Microsoft.Network</span></span> | 

<span data-ttu-id="b56b7-142">Regionális szinten, vagy a virtuális gép termékcsalád magok kvóta a Virtuálisgép-méretet, a Batch-készlet vagy készletek szükséges megfelelően kell beállítani:</span><span class="sxs-lookup"><span data-stu-id="b56b7-142">The cores quota at a regional level or per VM family should be set according to the VM size required for your Batch pool or pools:</span></span>

| <span data-ttu-id="b56b7-143">Kvóta</span><span class="sxs-lookup"><span data-stu-id="b56b7-143">Quota</span></span> | <span data-ttu-id="b56b7-144">Szolgáltató</span><span class="sxs-lookup"><span data-stu-id="b56b7-144">Provider</span></span> |
| --- | ---- |
| <span data-ttu-id="b56b7-145">Teljes regionális magok</span><span class="sxs-lookup"><span data-stu-id="b56b7-145">Total Regional Cores</span></span> | <span data-ttu-id="b56b7-146">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="b56b7-146">Microsoft.Compute</span></span> |
| <span data-ttu-id="b56b7-147">…</span><span class="sxs-lookup"><span data-stu-id="b56b7-147">…</span></span> <span data-ttu-id="b56b7-148">Családbiztonsági magok</span><span class="sxs-lookup"><span data-stu-id="b56b7-148">Family Cores</span></span> | <span data-ttu-id="b56b7-149">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="b56b7-149">Microsoft.Compute</span></span> |



## <a name="other-limits"></a><span data-ttu-id="b56b7-150">Egyéb korlátozások</span><span class="sxs-lookup"><span data-stu-id="b56b7-150">Other limits</span></span>
| <span data-ttu-id="b56b7-151">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="b56b7-151">**Resource**</span></span> | <span data-ttu-id="b56b7-152">**Felső korlát**</span><span class="sxs-lookup"><span data-stu-id="b56b7-152">**Maximum Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="b56b7-153">[Egyidejű feladatok](batch-parallel-node-tasks.md) egyes számítási csomópontjain</span><span class="sxs-lookup"><span data-stu-id="b56b7-153">[Concurrent tasks](batch-parallel-node-tasks.md) per compute node</span></span> |<span data-ttu-id="b56b7-154">csomópont magok száma 4 x</span><span class="sxs-lookup"><span data-stu-id="b56b7-154">4 x number of node cores</span></span> |
| <span data-ttu-id="b56b7-155">[Alkalmazások](batch-application-packages.md) / Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b56b7-155">[Applications](batch-application-packages.md) per Batch account</span></span> |<span data-ttu-id="b56b7-156">20</span><span class="sxs-lookup"><span data-stu-id="b56b7-156">20</span></span> |
| <span data-ttu-id="b56b7-157">Alkalmazáscsomagok alkalmazásonként</span><span class="sxs-lookup"><span data-stu-id="b56b7-157">Application packages per application</span></span> |<span data-ttu-id="b56b7-158">40</span><span class="sxs-lookup"><span data-stu-id="b56b7-158">40</span></span> |
| <span data-ttu-id="b56b7-159">Csomag mérete (minden)</span><span class="sxs-lookup"><span data-stu-id="b56b7-159">Application package size (each)</span></span> |<span data-ttu-id="b56b7-160">KB. 195GB<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="b56b7-160">Approx. 195GB<sup>1</sup></span></span> |
| <span data-ttu-id="b56b7-161">Maximális kezdő tevékenység mérete</span><span class="sxs-lookup"><span data-stu-id="b56b7-161">Maximum start task size</span></span> | <span data-ttu-id="b56b7-162">32768 karakterek<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="b56b7-162">32768 characters<sup>2</sup></span></span> |

<span data-ttu-id="b56b7-163"><sup>1</sup> blob maximális blokkméretének azure tárolási kapacitása</span><span class="sxs-lookup"><span data-stu-id="b56b7-163"><sup>1</sup> Azure Storage limit for maximum block blob size</span></span><br /><span data-ttu-id="b56b7-164">
<sup>2</sup> erőforrás fájlok és a környezeti változók</span><span class="sxs-lookup"><span data-stu-id="b56b7-164">
<sup>2</sup> Includes resource files and environment variables</span></span>

## <a name="view-batch-quotas"></a><span data-ttu-id="b56b7-165">Kötegelt kvóták megtekintése</span><span class="sxs-lookup"><span data-stu-id="b56b7-165">View Batch quotas</span></span>
<span data-ttu-id="b56b7-166">A kötegelt fiók kvótákat megtekintése a [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="b56b7-166">View your Batch account quotas in the [Azure portal][portal].</span></span>

1. <span data-ttu-id="b56b7-167">Válassza ki **Batch-fiókok** a portálon, majd válassza ki a Batch-fiók kíváncsiak vagyunk.</span><span class="sxs-lookup"><span data-stu-id="b56b7-167">Select **Batch accounts** in the portal, then select the Batch account you're interested in.</span></span>
2. <span data-ttu-id="b56b7-168">Válassza ki **tulajdonságok** a Batch-fiók menü panelen.</span><span class="sxs-lookup"><span data-stu-id="b56b7-168">Select **Properties** on the Batch account's menu blade.</span></span>
3. <span data-ttu-id="b56b7-169">A Tulajdonságok panelen megjeleníti a **kvóták** jelenleg hozzárendelve a Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="b56b7-169">The Properties blade displays the **quotas** currently applied to the Batch account</span></span>
   
    ![Batch-fiók kvóták][account_quotas]

<span data-ttu-id="b56b7-171">Batch-fiók felhasználói előfizetési módban létrehozott tekintse meg a kapcsolódó előfizetés kvóták az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b56b7-171">For a Batch account created in user subscription mode, view the related subscription quotas in the Azure Portal.</span></span>

1. <span data-ttu-id="b56b7-172">Válassza ki **előfizetések**, és válassza ki az előfizetést, a Batch-fiókot használ.</span><span class="sxs-lookup"><span data-stu-id="b56b7-172">Select **Subscriptions**, and select the subscription you are using for the Batch account.</span></span>

2. <span data-ttu-id="b56b7-173">Az a **előfizetés** panelen válassza **használati + kvóták**.</span><span class="sxs-lookup"><span data-stu-id="b56b7-173">On the **Subscription** blade, select **Usage + quotas**.</span></span>



## <a name="increase-a-quota"></a><span data-ttu-id="b56b7-174">A kvóta növeléséhez</span><span class="sxs-lookup"><span data-stu-id="b56b7-174">Increase a quota</span></span>
<span data-ttu-id="b56b7-175">Kövesse ezeket a lépéseket, kérje a kvóta növeléséhez a Batch-fiók, illetve az előfizetés használata a [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="b56b7-175">Follow these steps to request a quota increase for your Batch account or your subscription using the [Azure portal][portal].</span></span> <span data-ttu-id="b56b7-176">Kvótájának növelését típusa attól függ, hogy a tárolókészlet foglalási mód a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b56b7-176">The type of quota increase depends on the pool allocation mode of your Batch account.</span></span>

### <a name="increase-a-batch-cores-quota"></a><span data-ttu-id="b56b7-177">A kötegelt magok kvóta növelése</span><span class="sxs-lookup"><span data-stu-id="b56b7-177">Increase a Batch cores quota</span></span> 

<span data-ttu-id="b56b7-178">Ha a Batch-fiókhoz készült **a Batch szolgáltatás** mód, kövesse ezeket a lépéseket, kérje a kötegelt magok kvóta növelése:</span><span class="sxs-lookup"><span data-stu-id="b56b7-178">If your Batch account was created in **Batch service** mode, follow these steps to request a Batch cores quota increase:</span></span>

1. <span data-ttu-id="b56b7-179">Válassza ki a **súgó + támogatás** csempét a portál irányítópultján, vagy a kérdőjel (**?**) a portál jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="b56b7-179">Select the **Help + support** tile on your portal dashboard, or the question mark (**?**) in the upper-right corner of the portal.</span></span>
2. <span data-ttu-id="b56b7-180">Válassza ki **új támogatja a kérelem** > **alapjai**.</span><span class="sxs-lookup"><span data-stu-id="b56b7-180">Select **New support request** > **Basics**.</span></span>
3. <span data-ttu-id="b56b7-181">Az a **alapjai** panel:</span><span class="sxs-lookup"><span data-stu-id="b56b7-181">On the **Basics** blade:</span></span>
   
    <span data-ttu-id="b56b7-182">a.</span><span class="sxs-lookup"><span data-stu-id="b56b7-182">a.</span></span> <span data-ttu-id="b56b7-183">**Típusú** > **kvóta**</span><span class="sxs-lookup"><span data-stu-id="b56b7-183">**Issue Type** > **Quota**</span></span>
   
    <span data-ttu-id="b56b7-184">b.</span><span class="sxs-lookup"><span data-stu-id="b56b7-184">b.</span></span> <span data-ttu-id="b56b7-185">Válassza ki előfizetését.</span><span class="sxs-lookup"><span data-stu-id="b56b7-185">Select your subscription.</span></span>
   
    <span data-ttu-id="b56b7-186">c.</span><span class="sxs-lookup"><span data-stu-id="b56b7-186">c.</span></span> <span data-ttu-id="b56b7-187">**Kvóta típusa** > **kötegelt**</span><span class="sxs-lookup"><span data-stu-id="b56b7-187">**Quota type** > **Batch**</span></span>
   
    <span data-ttu-id="b56b7-188">d.</span><span class="sxs-lookup"><span data-stu-id="b56b7-188">d.</span></span> <span data-ttu-id="b56b7-189">**Támogatás megléte** > **kvóta támogatásához -**</span><span class="sxs-lookup"><span data-stu-id="b56b7-189">**Support plan** > **Quota support - Included**</span></span>
   
    <span data-ttu-id="b56b7-190">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b56b7-190">Click **Next**.</span></span>
4. <span data-ttu-id="b56b7-191">Az a **probléma** panel:</span><span class="sxs-lookup"><span data-stu-id="b56b7-191">On the **Problem** blade:</span></span>
   
    <span data-ttu-id="b56b7-192">a.</span><span class="sxs-lookup"><span data-stu-id="b56b7-192">a.</span></span> <span data-ttu-id="b56b7-193">Válassza ki a **súlyossági** megfelelően a [üzletmenetre gyakorolt hatás][support_sev].</span><span class="sxs-lookup"><span data-stu-id="b56b7-193">Select a **Severity** according to your [business impact][support_sev].</span></span>
   
    <span data-ttu-id="b56b7-194">b.</span><span class="sxs-lookup"><span data-stu-id="b56b7-194">b.</span></span> <span data-ttu-id="b56b7-195">A **részletek**, adja meg minden egyes módosítani kívánt kvótát, a Batch-fiók nevét és az új korlát.</span><span class="sxs-lookup"><span data-stu-id="b56b7-195">In **Details**, specify each quota you want to change, the Batch account name, and the new limit.</span></span>
   
    <span data-ttu-id="b56b7-196">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b56b7-196">Click **Next**.</span></span>
5. <span data-ttu-id="b56b7-197">Az a **elérhetőségi adatai** panel:</span><span class="sxs-lookup"><span data-stu-id="b56b7-197">On the **Contact information** blade:</span></span>
   
    <span data-ttu-id="b56b7-198">a.</span><span class="sxs-lookup"><span data-stu-id="b56b7-198">a.</span></span> <span data-ttu-id="b56b7-199">Válassza ki a **elsődleges kapcsolattartási módszert**.</span><span class="sxs-lookup"><span data-stu-id="b56b7-199">Select a **Preferred contact method**.</span></span>
   
    <span data-ttu-id="b56b7-200">b.</span><span class="sxs-lookup"><span data-stu-id="b56b7-200">b.</span></span> <span data-ttu-id="b56b7-201">Győződjön meg arról, és írja be a szükséges kapcsolattartási adatait.</span><span class="sxs-lookup"><span data-stu-id="b56b7-201">Verify and enter the required contact details.</span></span>
   
    <span data-ttu-id="b56b7-202">Kattintson a**Create** (Létrehozás) gombra a támogatási kérelem elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="b56b7-202">Click **Create** to submit the support request.</span></span>

<span data-ttu-id="b56b7-203">Ha a támogatási kérelmet küldött, az Azure támogatási kapcsolatba lép Önnel.</span><span class="sxs-lookup"><span data-stu-id="b56b7-203">Once you've submitted your support request, Azure support will contact you.</span></span> <span data-ttu-id="b56b7-204">Ne feledje, hogy a kérelem befejezése telhet legfeljebb 2 munkanapos határidejűek.</span><span class="sxs-lookup"><span data-stu-id="b56b7-204">Note that completing the request can take up to 2 business days.</span></span>

### <a name="increase-a-subscription-cores-quota"></a><span data-ttu-id="b56b7-205">Egy mag előfizetési kvóta növeléséhez</span><span class="sxs-lookup"><span data-stu-id="b56b7-205">Increase a subscription cores quota</span></span>

<span data-ttu-id="b56b7-206">Ha a Batch-fiókhoz készült **felhasználói előfizetési** módot, és át kell további területi vagy virtuális gép termékcsalád mag, a kvóta kérelem növelje az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="b56b7-206">If your Batch account was created in **user subscription** mode and you need additional regional or VM family cores, request a quota increase in your subscription.</span></span> <span data-ttu-id="b56b7-207">Útmutató: [erőforrás-kezelő core kvóta növelése kérelmek](../azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="b56b7-207">For steps, see [Resource Manager core quota increase requests](../azure-supportability/resource-manager-core-quotas-request.md).</span></span>



## <a name="related-topics"></a><span data-ttu-id="b56b7-208">Kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="b56b7-208">Related topics</span></span>
* [<span data-ttu-id="b56b7-209">Az Azure portál használata az Azure Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b56b7-209">Create an Azure Batch account using the Azure portal</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="b56b7-210">Azure Batch funkcióinak áttekintése</span><span class="sxs-lookup"><span data-stu-id="b56b7-210">Azure Batch feature overview</span></span>](batch-api-basics.md)
* [<span data-ttu-id="b56b7-211">Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések</span><span class="sxs-lookup"><span data-stu-id="b56b7-211">Azure subscription and service limits, quotas, and constraints</span></span>](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
