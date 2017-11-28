---
title: "aaaService kvótái és korlátai Azure Batch |} Microsoft Docs"
description: "További tudnivalók az alapértelmezett Azure Batch kvóták, korlátok és korlátozások és hogyan toorequest kvóta növeli"
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
ms.openlocfilehash: 6035d1c7618cfe97ebca3780e02a4ee34f54e534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="09f2f-103">A Bach szolgáltatás kvótái és korlátozásai</span><span class="sxs-lookup"><span data-stu-id="09f2f-103">Batch service quotas and limits</span></span>

<span data-ttu-id="09f2f-104">Mint az egyéb Azure-szolgáltatásokkal, nincsenek korlátozások bizonyos hello Batch szolgáltatás társított erőforrások.</span><span class="sxs-lookup"><span data-stu-id="09f2f-104">As with other Azure services, there are limits on certain resources associated with hello Batch service.</span></span> <span data-ttu-id="09f2f-105">Ezek a korlátozások számos alapértelmezett kvóták alkalmazása az Azure-ban hello előfizetés vagy a fiók szintjén.</span><span class="sxs-lookup"><span data-stu-id="09f2f-105">Many of these limits are default quotas applied by Azure at hello subscription or account level.</span></span> <span data-ttu-id="09f2f-106">Ez a cikk ismerteti azokat az alapértelmezett beállításokat, és hogyan kérheti a kvótájának növeli.</span><span class="sxs-lookup"><span data-stu-id="09f2f-106">This article discusses those defaults, and how you can request quota increases.</span></span>

<span data-ttu-id="09f2f-107">Vegye figyelembe ezeket a kvótákat a Batch számítási feladatok tervezésekor és bővítésekor.</span><span class="sxs-lookup"><span data-stu-id="09f2f-107">Keep these quotas in mind as you are designing and scaling up your Batch workloads.</span></span> <span data-ttu-id="09f2f-108">Például ha a készlet a megadott számítási csomópontok száma hello cél nem elérése, előfordulhat, hogy elérte hello core kvótakorlátot a Batch-fiók, vagy a regionális virtuális gép magok kvóta az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="09f2f-108">For example, if your pool isn't reaching hello target number of compute nodes you've specified, you might have reached hello core quota limit for your Batch account, or a regional VM cores quota for your subscription.</span></span>

<span data-ttu-id="09f2f-109">Több kötegelt feladatok futtatása egyetlen Batch-fiók, vagy a Batch-fiókok, amelyek a hello munkaterhelésének terjesztése ugyanahhoz az előfizetéshez, de különböző Azure-régiókban.</span><span class="sxs-lookup"><span data-stu-id="09f2f-109">You can run multiple Batch workloads in a single Batch account, or distribute your workloads among Batch accounts that are in hello same subscription, but in different Azure regions.</span></span>

<span data-ttu-id="09f2f-110">Ha azt tervezi, hogy a kötegben toorun termelési számítási feladatokhoz, szükség lehet tooincrease egy vagy több hello kvóták fenti hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="09f2f-110">If you plan toorun production workloads in Batch, you may need tooincrease one or more of hello quotas above hello default.</span></span> <span data-ttu-id="09f2f-111">Ha azt szeretné, hogy tooraise egy kvótát, az online megnyithatja [ügyfél-támogatási kérelem](#increase-a-quota) díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="09f2f-111">If you want tooraise a quota, you can open an online [customer support request](#increase-a-quota) at no charge.</span></span>

> [!NOTE]
> <span data-ttu-id="09f2f-112">A kvóta legfeljebb, nem kapacitás garancia.</span><span class="sxs-lookup"><span data-stu-id="09f2f-112">A quota is a credit limit, not a capacity guarantee.</span></span> <span data-ttu-id="09f2f-113">Ha nagyméretű lemezkapacitási igényekről, forduljon az Azure támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="09f2f-113">If you have large-scale capacity needs, please contact Azure support.</span></span>
> 
> 

## <a name="resource-quotas"></a><span data-ttu-id="09f2f-114">Erőforráskvóták</span><span class="sxs-lookup"><span data-stu-id="09f2f-114">Resource quotas</span></span>
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a><span data-ttu-id="09f2f-115">Kvótákat felhasználói előfizetési módban</span><span class="sxs-lookup"><span data-stu-id="09f2f-115">Quotas in user subscription mode</span></span>

<span data-ttu-id="09f2f-116">Tárolókészlet foglalási módban a túl a Batch-fiókhoz**felhasználói előfizetési**, a Batch-virtuális gépek és más erőforrások, például a storage-fiókok, közvetlenül az előfizetésében jönnek létre, amikor egy alkalmazáskészlet jön létre.</span><span class="sxs-lookup"><span data-stu-id="09f2f-116">For a Batch account with pool allocation mode set too**user subscription**, Batch VMs and other resources, such as storage accounts, are created directly in your subscription when a pool is created.</span></span> <span data-ttu-id="09f2f-117">hello Azure Batch magok kvóta nem érvényes ebben a módban létrehozott tooan fiók.</span><span class="sxs-lookup"><span data-stu-id="09f2f-117">hello Azure Batch cores quota does not apply tooan account created in this mode.</span></span> <span data-ttu-id="09f2f-118">Ehelyett hello kvóták regionális számítási maggal és más erőforrásokhoz az előfizetésben érvényesek.</span><span class="sxs-lookup"><span data-stu-id="09f2f-118">Instead, hello quotas in your subscription for regional compute cores and other resources are applied.</span></span> <span data-ttu-id="09f2f-119">További információ a ezek mely százalékértékénél kéri [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="09f2f-119">Learn more about these quotas in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="09f2f-120">Erőforrás kihasználtsága egy olyan fiók, létre felhasználói előfizetési módban tervezésekor a következő kötegelt erőforrások (a hozzáadása toocompute magok) Megjegyzés hello minden 40 Linux virtuális gépek, vagy szükségesek 20 Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="09f2f-120">When planning resource usage for an account created in user subscription mode, note hello following Batch resources (in addition toocompute cores) are required for every 40 Linux VMs, or 20 Windows VMs:</span></span>

| <span data-ttu-id="09f2f-121">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="09f2f-121">Resource</span></span> | <span data-ttu-id="09f2f-122">Kvóta</span><span class="sxs-lookup"><span data-stu-id="09f2f-122">Quota</span></span> | <span data-ttu-id="09f2f-123">Szolgáltató</span><span class="sxs-lookup"><span data-stu-id="09f2f-123">Provider</span></span> |
| --- | ---| --- |
| <span data-ttu-id="09f2f-124">Egy tárfiók</span><span class="sxs-lookup"><span data-stu-id="09f2f-124">One storage account</span></span> | <span data-ttu-id="09f2f-125">Tárfiókok</span><span class="sxs-lookup"><span data-stu-id="09f2f-125">Storage Accounts</span></span> | <span data-ttu-id="09f2f-126">Microsoft.Storage</span><span class="sxs-lookup"><span data-stu-id="09f2f-126">Microsoft.Storage</span></span> |
| <span data-ttu-id="09f2f-127">Egy nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="09f2f-127">One public IP address</span></span> | <span data-ttu-id="09f2f-128">Nyilvános IP-címek</span><span class="sxs-lookup"><span data-stu-id="09f2f-128">Public IP Addresses</span></span> | <span data-ttu-id="09f2f-129">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="09f2f-129">Microsoft.Network</span></span> | 
| <span data-ttu-id="09f2f-130">Egy virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="09f2f-130">One virtual network</span></span> | <span data-ttu-id="09f2f-131">Virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="09f2f-131">Virtual Networks</span></span> | <span data-ttu-id="09f2f-132">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="09f2f-132">Microsoft.Network</span></span> | 
| <span data-ttu-id="09f2f-133">Egy hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="09f2f-133">One network security group</span></span> | <span data-ttu-id="09f2f-134">Network Security Groups (Hálózati biztonsági csoportok)</span><span class="sxs-lookup"><span data-stu-id="09f2f-134">Network Security Groups</span></span> | <span data-ttu-id="09f2f-135">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="09f2f-135">Microsoft.Network</span></span> | 
| <span data-ttu-id="09f2f-136">Egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="09f2f-136">One virtual machine scale set</span></span> | <span data-ttu-id="09f2f-137">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="09f2f-137">Virtual Machine Scale Sets</span></span> | <span data-ttu-id="09f2f-138">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="09f2f-138">Microsoft.Compute</span></span> | 
| <span data-ttu-id="09f2f-139">Egy terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="09f2f-139">One load balancer</span></span> | <span data-ttu-id="09f2f-140">Terheléselosztók</span><span class="sxs-lookup"><span data-stu-id="09f2f-140">Load Balancers</span></span> | <span data-ttu-id="09f2f-141">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="09f2f-141">Microsoft.Network</span></span> | 

<span data-ttu-id="09f2f-142">hello magok kvóta regionális szinten, vagy a virtuális gép termékcsalád kell set függően toohello Virtuálisgép-méretet a Batch-készlet vagy készletek szükséges:</span><span class="sxs-lookup"><span data-stu-id="09f2f-142">hello cores quota at a regional level or per VM family should be set according toohello VM size required for your Batch pool or pools:</span></span>

| <span data-ttu-id="09f2f-143">Kvóta</span><span class="sxs-lookup"><span data-stu-id="09f2f-143">Quota</span></span> | <span data-ttu-id="09f2f-144">Szolgáltató</span><span class="sxs-lookup"><span data-stu-id="09f2f-144">Provider</span></span> |
| --- | ---- |
| <span data-ttu-id="09f2f-145">Teljes regionális magok</span><span class="sxs-lookup"><span data-stu-id="09f2f-145">Total Regional Cores</span></span> | <span data-ttu-id="09f2f-146">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="09f2f-146">Microsoft.Compute</span></span> |
| <span data-ttu-id="09f2f-147">…</span><span class="sxs-lookup"><span data-stu-id="09f2f-147">…</span></span> <span data-ttu-id="09f2f-148">Családbiztonsági magok</span><span class="sxs-lookup"><span data-stu-id="09f2f-148">Family Cores</span></span> | <span data-ttu-id="09f2f-149">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="09f2f-149">Microsoft.Compute</span></span> |



## <a name="other-limits"></a><span data-ttu-id="09f2f-150">Egyéb korlátozások</span><span class="sxs-lookup"><span data-stu-id="09f2f-150">Other limits</span></span>
| <span data-ttu-id="09f2f-151">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="09f2f-151">**Resource**</span></span> | <span data-ttu-id="09f2f-152">**Felső korlát**</span><span class="sxs-lookup"><span data-stu-id="09f2f-152">**Maximum Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="09f2f-153">[Egyidejű feladatok](batch-parallel-node-tasks.md) egyes számítási csomópontjain</span><span class="sxs-lookup"><span data-stu-id="09f2f-153">[Concurrent tasks](batch-parallel-node-tasks.md) per compute node</span></span> |<span data-ttu-id="09f2f-154">csomópont magok száma 4 x</span><span class="sxs-lookup"><span data-stu-id="09f2f-154">4 x number of node cores</span></span> |
| <span data-ttu-id="09f2f-155">[Alkalmazások](batch-application-packages.md) / Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="09f2f-155">[Applications](batch-application-packages.md) per Batch account</span></span> |<span data-ttu-id="09f2f-156">20</span><span class="sxs-lookup"><span data-stu-id="09f2f-156">20</span></span> |
| <span data-ttu-id="09f2f-157">Alkalmazáscsomagok alkalmazásonként</span><span class="sxs-lookup"><span data-stu-id="09f2f-157">Application packages per application</span></span> |<span data-ttu-id="09f2f-158">40</span><span class="sxs-lookup"><span data-stu-id="09f2f-158">40</span></span> |
| <span data-ttu-id="09f2f-159">Csomag mérete (minden)</span><span class="sxs-lookup"><span data-stu-id="09f2f-159">Application package size (each)</span></span> |<span data-ttu-id="09f2f-160">KB. 195GB<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="09f2f-160">Approx. 195GB<sup>1</sup></span></span> |
| <span data-ttu-id="09f2f-161">Maximális kezdő tevékenység mérete</span><span class="sxs-lookup"><span data-stu-id="09f2f-161">Maximum start task size</span></span> | <span data-ttu-id="09f2f-162">32768 karakterek<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="09f2f-162">32768 characters<sup>2</sup></span></span> |

<span data-ttu-id="09f2f-163"><sup>1</sup> blob maximális blokkméretének azure tárolási kapacitása</span><span class="sxs-lookup"><span data-stu-id="09f2f-163"><sup>1</sup> Azure Storage limit for maximum block blob size</span></span><br /><span data-ttu-id="09f2f-164">
<sup>2</sup> erőforrás fájlok és a környezeti változók</span><span class="sxs-lookup"><span data-stu-id="09f2f-164">
<sup>2</sup> Includes resource files and environment variables</span></span>

## <a name="view-batch-quotas"></a><span data-ttu-id="09f2f-165">Kötegelt kvóták megtekintése</span><span class="sxs-lookup"><span data-stu-id="09f2f-165">View Batch quotas</span></span>
<span data-ttu-id="09f2f-166">Tekintse meg a Batch-fiók kvótákat hello [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="09f2f-166">View your Batch account quotas in hello [Azure portal][portal].</span></span>

1. <span data-ttu-id="09f2f-167">Válassza ki **Batch-fiókok** hello portálon, majd válassza ki kíváncsiak vagyunk hello Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="09f2f-167">Select **Batch accounts** in hello portal, then select hello Batch account you're interested in.</span></span>
2. <span data-ttu-id="09f2f-168">Válassza ki **tulajdonságok** hello kötegelt fiók menü panelen.</span><span class="sxs-lookup"><span data-stu-id="09f2f-168">Select **Properties** on hello Batch account's menu blade.</span></span>
3. <span data-ttu-id="09f2f-169">hello tulajdonságok panelen megjeleníti hello **kvóták** jelenleg alkalmazott toohello Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="09f2f-169">hello Properties blade displays hello **quotas** currently applied toohello Batch account</span></span>
   
    ![Batch-fiók kvóták][account_quotas]

<span data-ttu-id="09f2f-171">Batch-fiók felhasználói előfizetési módban létrehozott nézet hello kapcsolatos hello Azure Portal előfizetés kvótáját.</span><span class="sxs-lookup"><span data-stu-id="09f2f-171">For a Batch account created in user subscription mode, view hello related subscription quotas in hello Azure Portal.</span></span>

1. <span data-ttu-id="09f2f-172">Válassza ki **előfizetések**, és válassza ki a Batch-fiókhoz hello alkalmaz hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="09f2f-172">Select **Subscriptions**, and select hello subscription you are using for hello Batch account.</span></span>

2. <span data-ttu-id="09f2f-173">A hello **előfizetés** panelen válassza **használati + kvóták**.</span><span class="sxs-lookup"><span data-stu-id="09f2f-173">On hello **Subscription** blade, select **Usage + quotas**.</span></span>



## <a name="increase-a-quota"></a><span data-ttu-id="09f2f-174">A kvóta növeléséhez</span><span class="sxs-lookup"><span data-stu-id="09f2f-174">Increase a quota</span></span>
<span data-ttu-id="09f2f-175">Kövesse ezeket a kvóta növeléséhez a Batch-fiók vagy a feliratkozás hello lépéseket toorequest [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="09f2f-175">Follow these steps toorequest a quota increase for your Batch account or your subscription using hello [Azure portal][portal].</span></span> <span data-ttu-id="09f2f-176">hello kvótájának növelését függ hello tárolókészlet foglalási mód a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="09f2f-176">hello type of quota increase depends on hello pool allocation mode of your Batch account.</span></span>

### <a name="increase-a-batch-cores-quota"></a><span data-ttu-id="09f2f-177">A kötegelt magok kvóta növelése</span><span class="sxs-lookup"><span data-stu-id="09f2f-177">Increase a Batch cores quota</span></span> 

<span data-ttu-id="09f2f-178">Ha a Batch-fiókhoz készült **a Batch szolgáltatás** mód, kövesse ezeket a lépéseket toorequest a kötegelt magok kvóta növelését:</span><span class="sxs-lookup"><span data-stu-id="09f2f-178">If your Batch account was created in **Batch service** mode, follow these steps toorequest a Batch cores quota increase:</span></span>

1. <span data-ttu-id="09f2f-179">Jelölje be hello **súgó + támogatás** csempét a portál irányítópultján, vagy a kérdőjel hello (**?**) hello portál jobb felső sarkában hello.</span><span class="sxs-lookup"><span data-stu-id="09f2f-179">Select hello **Help + support** tile on your portal dashboard, or hello question mark (**?**) in hello upper-right corner of hello portal.</span></span>
2. <span data-ttu-id="09f2f-180">Válassza ki **új támogatja a kérelem** > **alapjai**.</span><span class="sxs-lookup"><span data-stu-id="09f2f-180">Select **New support request** > **Basics**.</span></span>
3. <span data-ttu-id="09f2f-181">A hello **alapjai** panel:</span><span class="sxs-lookup"><span data-stu-id="09f2f-181">On hello **Basics** blade:</span></span>
   
    <span data-ttu-id="09f2f-182">a.</span><span class="sxs-lookup"><span data-stu-id="09f2f-182">a.</span></span> <span data-ttu-id="09f2f-183">**Típusú** > **kvóta**</span><span class="sxs-lookup"><span data-stu-id="09f2f-183">**Issue Type** > **Quota**</span></span>
   
    <span data-ttu-id="09f2f-184">b.</span><span class="sxs-lookup"><span data-stu-id="09f2f-184">b.</span></span> <span data-ttu-id="09f2f-185">Válassza ki előfizetését.</span><span class="sxs-lookup"><span data-stu-id="09f2f-185">Select your subscription.</span></span>
   
    <span data-ttu-id="09f2f-186">c.</span><span class="sxs-lookup"><span data-stu-id="09f2f-186">c.</span></span> <span data-ttu-id="09f2f-187">**Kvóta típusa** > **kötegelt**</span><span class="sxs-lookup"><span data-stu-id="09f2f-187">**Quota type** > **Batch**</span></span>
   
    <span data-ttu-id="09f2f-188">d.</span><span class="sxs-lookup"><span data-stu-id="09f2f-188">d.</span></span> <span data-ttu-id="09f2f-189">**Támogatás megléte** > **kvóta támogatásához -**</span><span class="sxs-lookup"><span data-stu-id="09f2f-189">**Support plan** > **Quota support - Included**</span></span>
   
    <span data-ttu-id="09f2f-190">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="09f2f-190">Click **Next**.</span></span>
4. <span data-ttu-id="09f2f-191">A hello **probléma** panel:</span><span class="sxs-lookup"><span data-stu-id="09f2f-191">On hello **Problem** blade:</span></span>
   
    <span data-ttu-id="09f2f-192">a.</span><span class="sxs-lookup"><span data-stu-id="09f2f-192">a.</span></span> <span data-ttu-id="09f2f-193">Válassza ki a **súlyossági** tooyour szerint [üzletmenetre gyakorolt hatás][support_sev].</span><span class="sxs-lookup"><span data-stu-id="09f2f-193">Select a **Severity** according tooyour [business impact][support_sev].</span></span>
   
    <span data-ttu-id="09f2f-194">b.</span><span class="sxs-lookup"><span data-stu-id="09f2f-194">b.</span></span> <span data-ttu-id="09f2f-195">A **részletek**, adja meg minden egyes toochange hello Batch-fiók nevét és hello új korlát kívánt kvótát.</span><span class="sxs-lookup"><span data-stu-id="09f2f-195">In **Details**, specify each quota you want toochange, hello Batch account name, and hello new limit.</span></span>
   
    <span data-ttu-id="09f2f-196">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="09f2f-196">Click **Next**.</span></span>
5. <span data-ttu-id="09f2f-197">A hello **elérhetőségi adatai** panel:</span><span class="sxs-lookup"><span data-stu-id="09f2f-197">On hello **Contact information** blade:</span></span>
   
    <span data-ttu-id="09f2f-198">a.</span><span class="sxs-lookup"><span data-stu-id="09f2f-198">a.</span></span> <span data-ttu-id="09f2f-199">Válassza ki a **elsődleges kapcsolattartási módszert**.</span><span class="sxs-lookup"><span data-stu-id="09f2f-199">Select a **Preferred contact method**.</span></span>
   
    <span data-ttu-id="09f2f-200">b.</span><span class="sxs-lookup"><span data-stu-id="09f2f-200">b.</span></span> <span data-ttu-id="09f2f-201">Győződjön meg arról, és írja be a szükséges hello kapcsolattartási adatait.</span><span class="sxs-lookup"><span data-stu-id="09f2f-201">Verify and enter hello required contact details.</span></span>
   
    <span data-ttu-id="09f2f-202">Kattintson a **létrehozása** toosubmit hello támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="09f2f-202">Click **Create** toosubmit hello support request.</span></span>

<span data-ttu-id="09f2f-203">Ha a támogatási kérelmet küldött, az Azure támogatási kapcsolatba lép Önnel.</span><span class="sxs-lookup"><span data-stu-id="09f2f-203">Once you've submitted your support request, Azure support will contact you.</span></span> <span data-ttu-id="09f2f-204">Vegye figyelembe, hogy hello kérelem befejezése is eltarthat too2 munkanapos határidejűek.</span><span class="sxs-lookup"><span data-stu-id="09f2f-204">Note that completing hello request can take up too2 business days.</span></span>

### <a name="increase-a-subscription-cores-quota"></a><span data-ttu-id="09f2f-205">Egy mag előfizetési kvóta növeléséhez</span><span class="sxs-lookup"><span data-stu-id="09f2f-205">Increase a subscription cores quota</span></span>

<span data-ttu-id="09f2f-206">Ha a Batch-fiókhoz készült **felhasználói előfizetési** módot, és át kell további területi vagy virtuális gép termékcsalád mag, a kvóta kérelem növelje az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="09f2f-206">If your Batch account was created in **user subscription** mode and you need additional regional or VM family cores, request a quota increase in your subscription.</span></span> <span data-ttu-id="09f2f-207">Útmutató: [erőforrás-kezelő core kvóta növelése kérelmek](../azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="09f2f-207">For steps, see [Resource Manager core quota increase requests](../azure-supportability/resource-manager-core-quotas-request.md).</span></span>



## <a name="related-topics"></a><span data-ttu-id="09f2f-208">Kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="09f2f-208">Related topics</span></span>
* [<span data-ttu-id="09f2f-209">Hello Azure portál használata az Azure Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="09f2f-209">Create an Azure Batch account using hello Azure portal</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="09f2f-210">Azure Batch funkcióinak áttekintése</span><span class="sxs-lookup"><span data-stu-id="09f2f-210">Azure Batch feature overview</span></span>](batch-api-basics.md)
* [<span data-ttu-id="09f2f-211">Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések</span><span class="sxs-lookup"><span data-stu-id="09f2f-211">Azure subscription and service limits, quotas, and constraints</span></span>](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
