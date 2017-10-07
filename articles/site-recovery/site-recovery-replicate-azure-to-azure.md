---
title: "aaaReplicate alkalmazások (az Azure tooAzure) |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset a replikáció a virtuális gépen fut az Azure-régió, egy Azure-ban túl egy másik régióban."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: fb190dac14419f892a1c6b45a3d991d8005e4bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-virtual-machines-tooanother-azure-region"></a><span data-ttu-id="f8e58-103">Az Azure virtuális gépek tooanother az Azure-régió replikálása</span><span class="sxs-lookup"><span data-stu-id="f8e58-103">Replicate Azure virtual machines tooanother Azure region</span></span>



>[!NOTE]
>
> <span data-ttu-id="f8e58-104">Az Azure virtuális gépek helyreállítási helyreplikálásának jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="f8e58-104">Site Recovery replication for Azure virtual machines is currently in preview.</span></span>

<span data-ttu-id="f8e58-105">Ez a cikk ismerteti, hogyan tooset a replikáció a virtuális gépek futnak a egy Azure-régiót tooanother Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="f8e58-105">This article describes how tooset up replication of virtual machines running in one Azure region tooanother Azure region.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8e58-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f8e58-106">Prerequisites</span></span>

* <span data-ttu-id="f8e58-107">hello a cikk feltételezi, hogy már ismeri a Site Recovery és a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="f8e58-107">hello article assumes that you already know about Site Recovery and Recovery Services Vault.</span></span> <span data-ttu-id="f8e58-108">Egy "Recovery services-tároló" előre létrehozott toohave van szüksége.</span><span class="sxs-lookup"><span data-stu-id="f8e58-108">You need toohave a 'Recovery services vault' pre created.</span></span>

    >[!NOTE]
    >
    > <span data-ttu-id="f8e58-109">Ajánlott hello hello helyre, ahol a virtuális gépek tooreplicate "Recovery services-tároló" létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8e58-109">It is recommended that you create hello 'Recovery services vault' in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="f8e58-110">Ha a célhely "USA középső RÉGIÓJA", hozzon létre például az "USA középső RÉGIÓJA" tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f8e58-110">For example, if your target location is 'Central US', create vault in 'Central US'.</span></span>

* <span data-ttu-id="f8e58-111">Ha hálózati biztonsági csoportok (NSG) szabályok vagy tűzfal proxy toocontrol hozzáférés toooutbound internetkapcsolattal az Azure virtuális gépek hello használ, győződjön meg arról, adott meg az engedélyezett hello szükséges URL-címek vagy IP-címek.</span><span class="sxs-lookup"><span data-stu-id="f8e58-111">If you are using Network Security Groups (NSG) rules or firewall proxy toocontrol access toooutbound internet connectivity on hello Azure VMs, ensure that you whitelist hello required URLs or IPs.</span></span> <span data-ttu-id="f8e58-112">Tekintse meg a túl[hálózati útmutató](./site-recovery-azure-to-azure-networking-guidance.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="f8e58-112">Refer too[Networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md) for more details.</span></span>

* <span data-ttu-id="f8e58-113">Ha egy expressroute-on vagy a helyszíni és hello közötti VPN-kapcsolat az Azure-adatforrásról hajtsa végre az [hely kapcsolatos szempontok az Azure tooon helyszíni ExpressRoute / VPN-konfiguráció](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="f8e58-113">If you have an ExpressRoute or a VPN connection between on-premises and hello source location in Azure, follow [Site Recovery Considerations for Azure tooon-premises ExpressRoute / VPN configuration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) document.</span></span>

* <span data-ttu-id="f8e58-114">Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable Azure virtuális gép replikációját.</span><span class="sxs-lookup"><span data-stu-id="f8e58-114">Your Azure user account needs toohave certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of an Azure virtual machine.</span></span>

* <span data-ttu-id="f8e58-115">Az Azure-előfizetéssel kell engedélyezett toocreate virtuális gépek toouse vész-Helyreállítási régió, érdemes hello célhelyet.</span><span class="sxs-lookup"><span data-stu-id="f8e58-115">Your Azure subscription should be enabled toocreate VMs in hello target location you want toouse as DR region.</span></span> <span data-ttu-id="f8e58-116">Felveheti a kapcsolatot támogatási tooenable hello szükséges kvótát.</span><span class="sxs-lookup"><span data-stu-id="f8e58-116">You can contact support tooenable hello required quota.</span></span>

## <a name="enable-replication-from-azure-site-recovery-vault"></a><span data-ttu-id="f8e58-117">Engedélyezze a replikálást az Azure Site Recovery-tároló</span><span class="sxs-lookup"><span data-stu-id="f8e58-117">Enable replication from Azure Site Recovery vault</span></span>
<span data-ttu-id="f8e58-118">Az ezen az ábrán azt replikálja a hello "Kelet-Ázsia" Azure-beli hely toohello "Délkelet-Ázsia" helyen futó virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="f8e58-118">For this illustration, we will replicate VMs running  in hello ‘East Asia’ Azure location toohello ‘South East Asia’ location.</span></span> <span data-ttu-id="f8e58-119">hello lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="f8e58-119">hello steps are as follows:</span></span>

 <span data-ttu-id="f8e58-120">Kattintson a **+ replikálás** hello tároló tooenable replikációs hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="f8e58-120">Click **+Replicate** in hello vault tooenable replication for hello virtual machines.</span></span>

1. <span data-ttu-id="f8e58-121">**Forrás:** toohello pont eredetű hello gépek, amelyek ebben az esetben hivatkozik **Azure**.</span><span class="sxs-lookup"><span data-stu-id="f8e58-121">**Source:** It refers toohello point of origin of hello machines which in this case is **Azure**.</span></span>

2. <span data-ttu-id="f8e58-122">**A forrás helye:** hello ahonnan szeretné tooprotect a virtuális gépek Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="f8e58-122">**Source location:** It is hello Azure region from where you want tooprotect your virtual machines.</span></span> <span data-ttu-id="f8e58-123">Az ezen az ábrán hello forráshely "Kelet-Ázsia" lesz</span><span class="sxs-lookup"><span data-stu-id="f8e58-123">For this illustration, hello source location will be 'East Asia'</span></span>

3. <span data-ttu-id="f8e58-124">**Telepítési modell:** hello forrásgépek Azure-telepítés típusának toohello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f8e58-124">**Deployment model:** It refers toohello Azure deployment model of hello source machines.</span></span> <span data-ttu-id="f8e58-125">Kiválaszthatja, hogy vagy a klasszikus vagy erőforrás-kezelő és az adott modell toohello tartozó gépek listája védelemre hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="f8e58-125">You can select either classic or resource manager and machines belonging toohello specific model will be listed for protection in hello next step.</span></span>

      >[!NOTE]
      >
      > <span data-ttu-id="f8e58-126">Csak a klasszikus virtuális gépek replikálása, és végezze el a helyreállítást a klasszikus virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="f8e58-126">You can only replicate a classic virtual machine and recover it as a classic virtual machine.</span></span> <span data-ttu-id="f8e58-127">Az erőforrás-kezelő virtuális gépként nem lehet helyreállítani.</span><span class="sxs-lookup"><span data-stu-id="f8e58-127">You cannot recover it as a Resource Manager virtual machine.</span></span>

4. <span data-ttu-id="f8e58-128">**Erőforráscsoport:** azt a hello erőforrás csoport toowhich a forrás virtuális gépek tartozik.</span><span class="sxs-lookup"><span data-stu-id="f8e58-128">**Resource Group:** It’s hello resource group toowhich your source virtual machines belong.</span></span> <span data-ttu-id="f8e58-129">Hello kijelölt erőforráscsoportba tartozó összes hello virtuális gépek védelemre hello következő lépésben jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="f8e58-129">All hello VMs under hello selected resource group will be listed for protection in hello next step.</span></span>

    ![A replikáció engedélyezése](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

<span data-ttu-id="f8e58-131">A **virtuális gépek > Válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné.</span><span class="sxs-lookup"><span data-stu-id="f8e58-131">In **Virtual Machines > Select virtual machines**, click and select each machine you want tooreplicate.</span></span> <span data-ttu-id="f8e58-132">Csak olyan gépeket választhat, amelyeken használható a replikáció funkció.</span><span class="sxs-lookup"><span data-stu-id="f8e58-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="f8e58-133">Ezután kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="f8e58-133">Then click OK.</span></span>
    <span data-ttu-id="f8e58-134">![A replikáció engedélyezése](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span><span class="sxs-lookup"><span data-stu-id="f8e58-134">![Enable replication](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span></span>


<span data-ttu-id="f8e58-135">A beállítások című szakaszban konfigurálhatja a cél hely tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f8e58-135">Under Settings section you can configure target site properties</span></span>

1. <span data-ttu-id="f8e58-136">**Cél helye:** Ez az hello hely, ahol a forrás virtuális gép adatait a rendszer replikálja.</span><span class="sxs-lookup"><span data-stu-id="f8e58-136">**Target Location:**  This is hello location where your source virtual machine data will be replicated.</span></span> <span data-ttu-id="f8e58-137">Attól függően, a kijelölt gépekhez helyét, a Site Recovery nyújtják, akkor hello megfelelő célrégiók listája.</span><span class="sxs-lookup"><span data-stu-id="f8e58-137">Depending upon your selected machines location, Site Recovery will provide you hello list of suitable target regions.</span></span>

    > [!TIP]
    > <span data-ttu-id="f8e58-138">Ajánlott tookeep célhelye megegyezik a helyreállítási frissítésétől services-tároló.</span><span class="sxs-lookup"><span data-stu-id="f8e58-138">It is recommended tookeep target location same as of your recovery services vault.</span></span>

2. <span data-ttu-id="f8e58-139">**Cél-erőforráscsoport:** hello erőforrás csoport toowhich a replikált virtuális gépek fog tartozni. Alapértelmezés szerint az ASR létrehoz egy új erőforráscsoportot hello cél régióban "automatikus" utótaggal rendelkező nevű.</span><span class="sxs-lookup"><span data-stu-id="f8e58-139">**Target resource group :** It is hello resource group toowhich all your replicated virtual machines will belong.By default ASR will create a new resource group in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="f8e58-140">Abban az esetben, ha erőforráscsoport hozta létre az ASR már létezik, a fogja használni. Másik lehetőségként toocustomize, ahogy az alábbi hello szakasz.</span><span class="sxs-lookup"><span data-stu-id="f8e58-140">In case resource group created by ASR already exist, it will be reused.You can also choose toocustomize it as shown in hello section below.</span></span>    
3. <span data-ttu-id="f8e58-141">**Cél virtuális hálózat:** alapértelmezés szerint az ASR létrehoz egy új virtuális hálózat hello cél régióban "automatikus" utótaggal rendelkező nevű.</span><span class="sxs-lookup"><span data-stu-id="f8e58-141">**Target Virtual Network:** By default, ASR will create a new virtual network in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="f8e58-142">A csatlakoztatott tooyour Forráshálózat lesz, és minden jövőbeli védelmi fog történni.</span><span class="sxs-lookup"><span data-stu-id="f8e58-142">This will be mapped tooyour source network and will be used for any future protection.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f8e58-143">[Ellenőrizze a hálózati részleteket](site-recovery-network-mapping-azure-to-azure.md) tooknow további információk a hálózat leképezését.</span><span class="sxs-lookup"><span data-stu-id="f8e58-143">[Check networking details](site-recovery-network-mapping-azure-to-azure.md) tooknow more about network mapping.</span></span>

4. <span data-ttu-id="f8e58-144">**Cél Storage-fiókok:** alapértelmezés szerint az ASR hello új cél tárfiók a forrás virtuális gép tárolókonfiguráció mimicking hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f8e58-144">**Target Storage accounts:** By default, ASR will create hello new target storage account mimicking your source VM storage configuration.</span></span> <span data-ttu-id="f8e58-145">Abban az esetben, ha már az ASR által létrehozott tárfiók létezik, a fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f8e58-145">In case storage account created by ASR already exist, it will be reused.</span></span>

5. <span data-ttu-id="f8e58-146">**Storage-fiókok gyorsítótárazása:** ASR gyorsítótárazása nevű hello forrás régióban külön tárfiókot kell.</span><span class="sxs-lookup"><span data-stu-id="f8e58-146">**Cache Storage accounts:** ASR needs extra storage account called cache storage in hello source region.</span></span> <span data-ttu-id="f8e58-147">Az összes hello történik hello a forrás virtuális gépek követni, és ezeket toohello célhelyet replikálása előtt toocache tárfiók küldött módosítások.</span><span class="sxs-lookup"><span data-stu-id="f8e58-147">All hello changes happening on hello source VMs are tracked and sent toocache storage account before replicating those toohello target location.</span></span>

6. <span data-ttu-id="f8e58-148">**A rendelkezésre állási csoporthoz:** alapértelmezés szerint az ASR létrehoz egy új rendelkezésre állási hello cél régióban "automatikus" utótaggal rendelkező név megadva.</span><span class="sxs-lookup"><span data-stu-id="f8e58-148">**Availability set :** By default, ASR will create a new availability set in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="f8e58-149">Abban az esetben, ha az ASR már létrehozta a rendelkezésre állási csoport létezik, a fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f8e58-149">In case availability set created by ASR already exist, it will be reused.</span></span>

7.  <span data-ttu-id="f8e58-150">**Replikációs házirend:** azt határozza meg a helyreállítási pont megőrzési előzményeit és az alkalmazás alkalmazáskonzisztens pillanatkép gyakorisága hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="f8e58-150">**Replication Policy:** It defines hello settings for recovery point retention history and app consistent snapshot frequency.</span></span> <span data-ttu-id="f8e58-151">Alapértelmezés szerint az ASR beállításokkal hozza létre egy új replikációs házirendet alapértelmezett 24 órányi a helyreállítási pontok megőrzésének ideje és a "60 percig app alkalmazáskonzisztens pillanatkép gyakorisága.</span><span class="sxs-lookup"><span data-stu-id="f8e58-151">By default, ASR will create a new replication policy with default settings of ‘24 hours’ for recovery point retention and ’60 minutes’ for app consistent snapshot frequency.</span></span>

    ![A replikáció engedélyezése](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a><span data-ttu-id="f8e58-153">Tároló erőforrásait testreszabása</span><span class="sxs-lookup"><span data-stu-id="f8e58-153">Customize target resources</span></span>

<span data-ttu-id="f8e58-154">Abban az esetben, ha azt szeretné, hogy toochange hello alapértelmezett értékeket használják az automatikus rendszer-Helyreállítás, igényei szerint hello-beállítások módosításával.</span><span class="sxs-lookup"><span data-stu-id="f8e58-154">In case you want toochange hello defaults used by ASR, you can change hello settings based on your needs.</span></span>

1. <span data-ttu-id="f8e58-155">**Testreszabása:** kattintson rá a toochange hello ASR által használt alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="f8e58-155">**Customize:** Click it toochange hello defaults used by ASR.</span></span>

2. <span data-ttu-id="f8e58-156">**Cél-erőforráscsoport:** hello erőforráscsoport választhat célhelyet hello hello előfizetésen belül a meglévő összes hello erőforráscsoportok hello listáját.</span><span class="sxs-lookup"><span data-stu-id="f8e58-156">**Target resource group :**  You can select hello resource group from hello list of all hello resource groups existing in hello target location within hello subscription.</span></span>

3. <span data-ttu-id="f8e58-157">**Cél virtuális hálózat:** hello célhelyet hello lista összes hello virtuális hálózat található.</span><span class="sxs-lookup"><span data-stu-id="f8e58-157">**Target Virtual Network:** You can find hello list of all hello virtual network in hello target location.</span></span>

4. <span data-ttu-id="f8e58-158">**A rendelkezésre állási csoporthoz:** rendelkezésre állási készletek beállítások toohello virtuális gépek rendelkezésre állási forrás régióban egy részét képező csak akkor adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="f8e58-158">**Availability set :** You can only add availability sets settings toohello virtual machines which are a part of availability in source region.</span></span>

5. <span data-ttu-id="f8e58-159">**Cél Storage-fiókok:**</span><span class="sxs-lookup"><span data-stu-id="f8e58-159">**Target Storage accounts:**</span></span>

<span data-ttu-id="f8e58-160">![Engedélyezze a replikálást](./media/site-recovery-replicate-azure-to-azure/customize.PNG) kattintson a **tároló-erőforrás létrehozása** és a replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f8e58-160">![Enable replication](./media/site-recovery-replicate-azure-to-azure/customize.PNG) Click on **Create target resource** and Enable Replication</span></span>


<span data-ttu-id="f8e58-161">Amennyiben a védett virtuális gépek ellenőrizheti, a virtuális gépek állapotát hello állapotának **replikált elemek**</span><span class="sxs-lookup"><span data-stu-id="f8e58-161">Once virtual machines are protected you can check hello status of VMs health under **Replicated items**</span></span>

>[!NOTE]
><span data-ttu-id="f8e58-162">Hello során idő a kezdeti replikáció nem sikerült annak a lehetősége, hogy állapot ideje toorefresh vesz igénybe, és nem jelenik meg a folyamatban van egy kis ideig.</span><span class="sxs-lookup"><span data-stu-id="f8e58-162">During hello time of initial replication there could a possibility that status takes time toorefresh and you don't see progress for some time.</span></span> <span data-ttu-id="f8e58-163">Kattinthat a frissítés gomb hello hello panel tooget hello legfrissebb állapotának hello tetején.</span><span class="sxs-lookup"><span data-stu-id="f8e58-163">You can click hello Refresh button on hello top of hello blade tooget hello latest status.</span></span>
>

![A replikáció engedélyezése](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a><span data-ttu-id="f8e58-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8e58-165">Next steps</span></span>
- <span data-ttu-id="f8e58-166">[További](site-recovery-test-failover-to-azure.md) feladatátvételi teszt futtatása.</span><span class="sxs-lookup"><span data-stu-id="f8e58-166">[Learn more](site-recovery-test-failover-to-azure.md) about running a test failover.</span></span>
- <span data-ttu-id="f8e58-167">[További](site-recovery-failover.md) kapcsolatos különféle a feladatátvételeket, és hogyan toorun őket.</span><span class="sxs-lookup"><span data-stu-id="f8e58-167">[Learn more](site-recovery-failover.md) about different types of failovers, and how toorun them.</span></span>
- <span data-ttu-id="f8e58-168">További információ [helyreállítási tervek segítségével](site-recovery-create-recovery-plans.md) tooreduce RTO.</span><span class="sxs-lookup"><span data-stu-id="f8e58-168">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) tooreduce RTO.</span></span>
- <span data-ttu-id="f8e58-169">További információ [újbóli védelméhez az Azure virtuális gépek](site-recovery-how-to-reprotect.md) feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="f8e58-169">Learn more about [reprotecting Azure  VMs](site-recovery-how-to-reprotect.md) after failover.</span></span>
