---
title: "a Site Recovery tárolójából tooan Azure Recovery Services-tároló aaaUpgrade"
description: "Ismerje meg, hogyan tooupgrade egy Azure Site Recovery-tárolóban tooa Recovery Services tároló"
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/31/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: a18a923ee3bad91873e654c9b9b5bf8b83acc123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-site-recovery-vault-tooan-azure-resource-manager-based-recovery-services-vault"></a><span data-ttu-id="b3977-103">Frissítse a Site Recovery tárolójából tooan Azure Resource Manager-alapú Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="b3977-103">Upgrade a Site Recovery vault tooan Azure Resource Manager-based Recovery Services vault</span></span>

<span data-ttu-id="b3977-104">Ez a cikk ismerteti, hogyan tooupgrade Azure Site Recovery-tárolók tooAzure helyreállítási szolgáltatás Resource Manager-alapú tárolók a folyamatban lévő replikáció gyakorolt hatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="b3977-104">This article describes how tooupgrade Azure Site Recovery vaults tooAzure Resource Manager-based Recovery Service vaults without any impact on ongoing replication.</span></span> <span data-ttu-id="b3977-105">További információ az Azure erőforrás-kezelő szolgáltatásait és előnyeit: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b3977-105">For more information about Azure Resource Manager features and benefits, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="b3977-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b3977-106">Introduction</span></span>
<span data-ttu-id="b3977-107">Recovery Services-tároló egy Azure Resource Manager-erőforrás natív módon hello felhőalapú biztonsági mentés és katasztrófa helyreállítási kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b3977-107">A Recovery Services vault is an Azure Resource Manager resource for managing backup and disaster recovery natively in hello cloud.</span></span> <span data-ttu-id="b3977-108">Egy egységes tárolóban, melyekkel hello új Azure-portálon, és azt a felváltja hello klasszikus biztonsági mentés és a Site Recovery-tárolók.</span><span class="sxs-lookup"><span data-stu-id="b3977-108">It is a unified vault that you can use in hello new Azure portal, and it replaces hello classic backup and Site Recovery vaults.</span></span>

<span data-ttu-id="b3977-109">Helyreállítási szolgáltatások tárolók funkciót, beleértve a tömbje kínál:</span><span class="sxs-lookup"><span data-stu-id="b3977-109">Recovery Services vaults offer an array of features, including:</span></span>

* <span data-ttu-id="b3977-110">Az Azure Resource Manager támogatása: védeni, és az Azure Resource Manager-verembe feladatátvételt a virtuális gépek és fizikai gépek.</span><span class="sxs-lookup"><span data-stu-id="b3977-110">Azure Resource Manager support: You can protect and fail over your virtual machines and physical machines into an Azure Resource Manager stack.</span></span>

* <span data-ttu-id="b3977-111">Lemez kizárása: Ha ideiglenes fájlokat vagy nagy forgalom adatokat, hogy nem szeretné, hogy toouse a megfelelő sávszélesség, kötetek lehet kizárni a replikálásból.</span><span class="sxs-lookup"><span data-stu-id="b3977-111">Exclude disk: If you have temp files or high churn data that you don’t want toouse all your bandwidth for, you can exclude volumes from replication.</span></span> <span data-ttu-id="b3977-112">Ez a funkció jelenleg engedélyezve van *VMware tooAzure* és *Hyper-V tooAzure* és ki van bővítve tooother forgatókönyvek is.</span><span class="sxs-lookup"><span data-stu-id="b3977-112">This capability has been enabled currently in *VMware tooAzure* and *Hyper-V tooAzure* and is extended tooother scenarios as well.</span></span>

* <span data-ttu-id="b3977-113">Támogatja a premium és helyileg redundáns tárolás: kiszolgálók most védelméhez a prémium szintű storage fiókokat, amelyek lehetővé teszik a felhasználói tooprotect alkalmazások magasabb bemeneti/kimeneti műveletek száma másodpercenként (IOPS).</span><span class="sxs-lookup"><span data-stu-id="b3977-113">Support for premium and locally redundant storage: You can now protect servers in premium storage accounts that allow customers tooprotect applications with higher input/output operations per second (IOPS).</span></span> <span data-ttu-id="b3977-114">Ez a funkció be van kapcsolva *VMware tooAzure*.</span><span class="sxs-lookup"><span data-stu-id="b3977-114">This capability is currently enabled in *VMware tooAzure*.</span></span>

* <span data-ttu-id="b3977-115">Zökkenőmentes élményt-bevezető: hello fokozott-bevezető élmény úgy tervezték, könnyen toomake vész-helyreállítási beállítása.</span><span class="sxs-lookup"><span data-stu-id="b3977-115">Streamlined getting-started experience: hello enhanced getting-started experience has been designed toomake disaster-recovery setup easy.</span></span>

* <span data-ttu-id="b3977-116">Biztonsági mentés és helyreállítás management hello azonos tárolóban: most vész-helyreállítási kiszolgálók védelme vagy készítsen biztonsági mentést a hello azonos tárolóban, terhet jelentősen csökkentheti a felügyeleti.</span><span class="sxs-lookup"><span data-stu-id="b3977-116">Backup and Site Recovery management from hello same vault: You can now protect servers for disaster recovery or perform backup from hello same vault, which can reduce your management overhead significantly.</span></span>

<span data-ttu-id="b3977-117">Frissített hello élmény és szolgáltatásokkal kapcsolatban további információkért lásd: hello [tároló, biztonsági mentési és helyreállítási blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span><span class="sxs-lookup"><span data-stu-id="b3977-117">For more information about hello upgraded experience and features, see hello [Storage, Backup, and Recovery blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span></span>

## <a name="salient-features"></a><span data-ttu-id="b3977-118">Következő funkciói</span><span class="sxs-lookup"><span data-stu-id="b3977-118">Salient features</span></span>

* <span data-ttu-id="b3977-119">Ne legyen hatással a folyamatban lévő replikáció: folyamatban lévő replikáció során minden megszakadása nélkül, és frissítés utáni.</span><span class="sxs-lookup"><span data-stu-id="b3977-119">No impact on ongoing replication: Ongoing replications continue without any interruption during and post upgrade.</span></span>

* <span data-ttu-id="b3977-120">További költség nélkül: egy frissített képességek teljes készlete beolvasása minden további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="b3977-120">No additional cost: Get an entire set of updated capabilities at no additional cost.</span></span>

* <span data-ttu-id="b3977-121">Adatvesztés nélküli: mivel ezt a folyamatot a frissítés és az áttelepítés nem, a meglévő replikációs helyreállítási pontok és beállításai változatlanok maradnak során, és hello frissítés után.</span><span class="sxs-lookup"><span data-stu-id="b3977-121">No data loss: Because this process is an upgrade and not a migration, existing replication recovery points and settings remain intact during and after hello upgrade.</span></span>


## <a name="what-happens-during-hello-vault-upgrade"></a><span data-ttu-id="b3977-122">Mi történik, hello tároló frissítés során?</span><span class="sxs-lookup"><span data-stu-id="b3977-122">What happens during hello vault upgrade?</span></span>

<span data-ttu-id="b3977-123">Hello frissítés közben nem hajtható végre műveletek, például egy új kiszolgáló vagy a replikáció a virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="b3977-123">During hello upgrade, you cannot perform operations such as registering a new server or enabling replication for a virtual machine (VM).</span></span> <span data-ttu-id="b3977-124">Műveletek, például az adatok olvasása vagy írása adatok toohello tárolóban, például a védett elemek toohello tároló, folyamatban lévő replikáció megszakításmentes folytatásához.</span><span class="sxs-lookup"><span data-stu-id="b3977-124">Operations that involve reading data from or writing data toohello vault, such as ongoing replication of protected items toohello vault, continue uninterrupted.</span></span>

### <a name="changes-tooautomation-and-tooling-after-hello-upgrade"></a><span data-ttu-id="b3977-125">Módosítások tooautomation és tooling hello frissítés után</span><span class="sxs-lookup"><span data-stu-id="b3977-125">Changes tooautomation and tooling after hello upgrade</span></span>
<span data-ttu-id="b3977-126">Hello tároló típus hello klasszikus üzembe helyezési modell toohello Resource Manager üzembe helyezési modellben frissít, hello meglévő automation vagy tooling tooensure, hogy az továbbra is toowork hello frissítés után frissítse.</span><span class="sxs-lookup"><span data-stu-id="b3977-126">As you upgrade hello vault type from hello classic deployment model toohello Resource Manager deployment model, update hello existing automation or tooling tooensure that it continues toowork after hello upgrade.</span></span>

### <a name="prepare-your-environment-for-hello-upgrade"></a><span data-ttu-id="b3977-127">Hello frissítéshez a környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="b3977-127">Prepare your environment for hello upgrade</span></span>

* [<span data-ttu-id="b3977-128">Telepítse a PowerShell vagy a verziófrissítésre azt tooversion 5-ös vagy újabb</span><span class="sxs-lookup"><span data-stu-id="b3977-128">Install PowerShell or upgrade it tooversion 5 or later</span></span>](https://www.microsoft.com/download/details.aspx?id=50395)
* [<span data-ttu-id="b3977-129">Hello Azure PowerShell MSI legfrissebb verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="b3977-129">Install hello latest version of Azure PowerShell MSI</span></span>](https://github.com/Azure/azure-powershell/releases)
* [<span data-ttu-id="b3977-130">Hello Recovery Services-tároló frissítési parancsprogram letöltése</span><span class="sxs-lookup"><span data-stu-id="b3977-130">Download hello Recovery Services vault upgrade script</span></span>](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a><span data-ttu-id="b3977-131">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b3977-131">Prerequisites</span></span>
<span data-ttu-id="b3977-132">a Site Recovery tooupgrade tárolók tooAzure helyreállítási szolgáltatás Resource Manager-alapú tárolók, hello követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="b3977-132">tooupgrade from Site Recovery vaults tooAzure Resource Manager-based Recovery Service vaults, you must meet hello following requirements:</span></span>

* <span data-ttu-id="b3977-133">Minimális verziója: hello Azure Site Recovery Provider a kiszolgálón telepítve kell lennie 5.1.1700.0 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b3977-133">Minimum agent version: hello version of Azure Site Recovery Provider installed on your server must be 5.1.1700.0 or later.</span></span>

* <span data-ttu-id="b3977-134">Támogatott konfiguráció: a tároló nem lehet konfigurálni a tárolóhálózat (SAN) vagy SQL Server AlwaysOn rendelkezésre állási csoportok.</span><span class="sxs-lookup"><span data-stu-id="b3977-134">Supported configuration: You cannot configure your vault with storage area network (SAN) or SQL Server AlwaysOn Availability Groups.</span></span> <span data-ttu-id="b3977-135">Minden más konfigurációk vannak támogatva.</span><span class="sxs-lookup"><span data-stu-id="b3977-135">All other configurations are supported.</span></span>

    >[!NOTE]
    ><span data-ttu-id="b3977-136">Hello frissítés után tárolási leképezése csak a PowerShell segítségével is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="b3977-136">After hello upgrade, you can manage storage mapping only via PowerShell.</span></span>

* <span data-ttu-id="b3977-137">Támogatott központi telepítési forgatókönyv: A tároló nem lehet hello *VMware tooAzure* a hagyományos telepítési modellt.</span><span class="sxs-lookup"><span data-stu-id="b3977-137">Supported deployment scenario: Your vault shouldn’t be hello *VMware tooAzure* legacy deployment model.</span></span> <span data-ttu-id="b3977-138">Mielőtt továbblépne, először helyezze át a toohello továbbfejlesztett üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="b3977-138">Before you proceed, first move toohello enhanced deployment model.</span></span>

* <span data-ttu-id="b3977-139">A felhasználó által kezdeményezett nem aktív feladatok, például a felügyeleti műveletek sík: mivel az access toohello felügyeleti vezérlősík korlátozott frissítés során, hajtsa végre a felügyeleti vezérlősík műveletet indít el a hello frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="b3977-139">No active user-initiated jobs that involve management plane operations: Because access toohello management plane is restricted during upgrade, complete all your management plane actions before you trigger hello upgrade.</span></span> <span data-ttu-id="b3977-140">Ez a folyamat nem tartalmazza a folyamatban lévő replikáció.</span><span class="sxs-lookup"><span data-stu-id="b3977-140">This process doesn’t include ongoing replication.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="b3977-141">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="b3977-141">Frequently asked questions</span></span>

<span data-ttu-id="b3977-142">**Ez a frissítés hatással van a folyamatban lévő replikáció?**</span><span class="sxs-lookup"><span data-stu-id="b3977-142">**Does this upgrade affect my ongoing replication?**</span></span>

<span data-ttu-id="b3977-143">Nem.</span><span class="sxs-lookup"><span data-stu-id="b3977-143">No.</span></span> <span data-ttu-id="b3977-144">A folyamatban lévő replikáció továbbra is folyamatos, során, és hello frissítés után.</span><span class="sxs-lookup"><span data-stu-id="b3977-144">Your ongoing replication continues uninterrupted during and after hello upgrade.</span></span>

<span data-ttu-id="b3977-145">**Mi történik, toonetwork beállításokat, például a pont-pont VPN és IP-beállítások?**</span><span class="sxs-lookup"><span data-stu-id="b3977-145">**What happens toonetwork settings such as site-to-site VPN and IP settings?**</span></span>

<span data-ttu-id="b3977-146">hello frissítés nem érinti hello hálózati beállításait.</span><span class="sxs-lookup"><span data-stu-id="b3977-146">hello upgrade doesn't affect hello network settings.</span></span> <span data-ttu-id="b3977-147">Az összes Azure-– helyi kapcsolat változatlanok maradnak.</span><span class="sxs-lookup"><span data-stu-id="b3977-147">All Azure-to-on-premises connections remain intact.</span></span>

<span data-ttu-id="b3977-148">**Toomy tárolók mi történik, ha nem tervezze meg a jövőben közelében hello tooupgrade?**</span><span class="sxs-lookup"><span data-stu-id="b3977-148">**What happens toomy vaults if I don’t plan tooupgrade in hello near future?**</span></span>

<span data-ttu-id="b3977-149">Hello régi Azure-portálon a Site Recovery-tároló támogatása szeptember 2017 indítása megszűnnek.</span><span class="sxs-lookup"><span data-stu-id="b3977-149">Support for Site Recovery vault in hello old Azure portal will be deprecated starting September 2017.</span></span> <span data-ttu-id="b3977-150">Határozottan javasoljuk, hogy hello frissítési funkció toomove toohello új portált használja.</span><span class="sxs-lookup"><span data-stu-id="b3977-150">We strongly recommend that you use hello upgrade feature toomove toohello new portal.</span></span>

<span data-ttu-id="b3977-151">**Mi az áttelepítési terv jelent a saját meglévő eszközt használunk erre?**</span><span class="sxs-lookup"><span data-stu-id="b3977-151">**What does this migration plan mean for my existing tooling?**</span></span>  

<span data-ttu-id="b3977-152">A tooling toohello Resource Manager üzembe helyezési modellben frissítése az egyik hello figyelembe kell venni a frissítési csomagok a legfontosabb módosításait.</span><span class="sxs-lookup"><span data-stu-id="b3977-152">Updating your tooling toohello Resource Manager deployment model is one of hello most important changes that you must account for in your upgrade plans.</span></span> <span data-ttu-id="b3977-153">Recovery Services-tárolók hello hello Resource Manager üzembe helyezési modellben alapulnak.</span><span class="sxs-lookup"><span data-stu-id="b3977-153">hello Recovery Services vaults are based on hello Resource Manager deployment model.</span></span> 

<span data-ttu-id="b3977-154">**Mennyi ideig nem hello felügyeleti-vezérlősík állásidő utolsó?**</span><span class="sxs-lookup"><span data-stu-id="b3977-154">**How long does hello management-plane downtime last?**</span></span>

<span data-ttu-id="b3977-155">hello frissítés általában körülbelül 15 too30 percet vesz igénybe, és eltarthat tooa legfeljebb egy óra.</span><span class="sxs-lookup"><span data-stu-id="b3977-155">hello upgrade ordinarily takes about 15 too30 minutes, and it could take up tooa maximum of one hour.</span></span>

<span data-ttu-id="b3977-156">**I vissza lehet vonni a frissítés után?**</span><span class="sxs-lookup"><span data-stu-id="b3977-156">**Can I roll back after upgrading?**</span></span>

<span data-ttu-id="b3977-157">Nem.</span><span class="sxs-lookup"><span data-stu-id="b3977-157">No.</span></span> <span data-ttu-id="b3977-158">FIM hello erőforrások sikeresen frissítette a rollback utasítás nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b3977-158">Rollback is not supported after hello resources have been successfully upgraded.</span></span>

<span data-ttu-id="b3977-159">**Azt is szeretnék ellenőrzi az előfizetés vagy az erőforrások toosee e tudja frissíteni?**</span><span class="sxs-lookup"><span data-stu-id="b3977-159">**Can I validate my subscription or resources toosee whether they can be upgraded?**</span></span>

<span data-ttu-id="b3977-160">Igen.</span><span class="sxs-lookup"><span data-stu-id="b3977-160">Yes.</span></span> <span data-ttu-id="b3977-161">Hello platform által támogatott frissítési lehetőség hello frissítés hello első lépése, hogy hello erőforrások képesek-e a frissítésre toovalidate.</span><span class="sxs-lookup"><span data-stu-id="b3977-161">In hello platform-supported upgrade option, hello first step in hello upgrade is toovalidate that hello resources are capable of an upgrade.</span></span> <span data-ttu-id="b3977-162">Hello érvényesítés meghiúsul, ha megfelelő hibaüzeneteket és figyelmeztetéseket fog kapni.</span><span class="sxs-lookup"><span data-stu-id="b3977-162">If hello validation fails, you will receive appropriate error messages or warnings.</span></span>

<span data-ttu-id="b3977-163">**Hogyan jelenthetem a problémát hello frissítést?**</span><span class="sxs-lookup"><span data-stu-id="b3977-163">**How do I report an issue with hello upgrade?**</span></span>

<span data-ttu-id="b3977-164">Esetleges hibákat tapasztal hello frissítés során, vegye figyelembe a hello műveleti azonosító szerepel-e hello hiba.</span><span class="sxs-lookup"><span data-stu-id="b3977-164">If you experience any failures during hello upgrade, note hello operation ID that's listed in hello error.</span></span> <span data-ttu-id="b3977-165">Microsoft Support proaktív hello probléma megoldását fog működni.</span><span class="sxs-lookup"><span data-stu-id="b3977-165">Microsoft Support will proactively work on resolving hello issue.</span></span> <span data-ttu-id="b3977-166">Forduljon a hello támogatási csapatával az előfizetés-azonosító, a tároló neve és a művelet azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b3977-166">You can also contact hello Support team with your subscription ID, vault name, and operation ID.</span></span> <span data-ttu-id="b3977-167">Támogatási tooresolve hello probléma minél gyorsabban fog működni.</span><span class="sxs-lookup"><span data-stu-id="b3977-167">Support will work tooresolve hello issue as quickly as possible.</span></span> <span data-ttu-id="b3977-168">Nem próbálja meg újra hello művelet csak a kifejezetten utasításai toodo Igen, a Microsoft által.</span><span class="sxs-lookup"><span data-stu-id="b3977-168">Do not retry hello operation unless you are explicitly instructed toodo so by Microsoft.</span></span>

## <a name="run-hello-script"></a><span data-ttu-id="b3977-169">Hello parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="b3977-169">Run hello script</span></span>

<span data-ttu-id="b3977-170">A PowerShellben futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="b3977-170">In PowerShell, run hello following command:</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* <span data-ttu-id="b3977-171">Előfizetés-azonosító: hello hello tárolóban, amely a frissítés társított előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b3977-171">SubscriptionID: hello subscription ID that's associated with hello vault that you're upgrading.</span></span>

* <span data-ttu-id="b3977-172">VaultName: hello tároló, amely a frissítés hello nevére.</span><span class="sxs-lookup"><span data-stu-id="b3977-172">VaultName: hello name of hello vault that you're upgrading.</span></span>

* <span data-ttu-id="b3977-173">Helye: hello tárolóban, amely a frissítés hello helye.</span><span class="sxs-lookup"><span data-stu-id="b3977-173">Location: hello location of hello vault that you're upgrading.</span></span>

* <span data-ttu-id="b3977-174">Indítása. ResourceType: A Site Recovery HyperVRecoveryManagerVault tárolók.</span><span class="sxs-lookup"><span data-stu-id="b3977-174">ResourceType: HyperVRecoveryManagerVault for Site Recovery vaults.</span></span>

* <span data-ttu-id="b3977-175">TargetResourceGroupName: hello erőforráscsoport, amelybe hello elhelyezett tároló toobe frissítve.</span><span class="sxs-lookup"><span data-stu-id="b3977-175">TargetResourceGroupName: hello resource group into which you want hello upgraded vault toobe placed.</span></span> <span data-ttu-id="b3977-176">TargetResourceGroupName lehet egy meglévő erőforráscsoportot az Azure Resource Manager vagy egy újat.</span><span class="sxs-lookup"><span data-stu-id="b3977-176">TargetResourceGroupName can be an existing resource group in Azure Resource Manager or a new one.</span></span> <span data-ttu-id="b3977-177">Ha hello megadott TargetResourceGroupName nem létezik, létrejön hello verziófrissítés részeként a hello azonos hello tárolóval helyen.</span><span class="sxs-lookup"><span data-stu-id="b3977-177">If hello TargetResourceGroupName that's supplied does not exist, it is created as part of hello upgrade in hello same location as hello vault.</span></span> <span data-ttu-id="b3977-178">További információ a hello "Erőforráscsoportok" című szakaszában talál [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="b3977-178">For more information, see hello "Resource groups" section of [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

    >[!NOTE]
    ><span data-ttu-id="b3977-179">Tulajdonos toocertain megkötések erőforrás csoport elnevezési.</span><span class="sxs-lookup"><span data-stu-id="b3977-179">Resource group naming is subject toocertain constraints.</span></span> <span data-ttu-id="b3977-180">tooprevent tároló frissítése sikertelen, gondosan kell, hogy tooobserve hello elnevezési konvenció.</span><span class="sxs-lookup"><span data-stu-id="b3977-180">tooprevent vault upgrade failure, be sure tooobserve hello naming convention carefully.</span></span>
    >
    ><span data-ttu-id="b3977-181">Példa:</span><span class="sxs-lookup"><span data-stu-id="b3977-181">For example:</span></span>
    >
    ><span data-ttu-id="b3977-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 - SubscriptionId 1234-54123-354354-56416-8645 - VaultName gen2dr-helyre "Észak-Európa" - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc</span><span class="sxs-lookup"><span data-stu-id="b3977-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionId 1234-54123-354354-56416-8645 -VaultName gen2dr -Location "north europe" -ResourceType hypervrecoverymanagervault -TargetResourceGroupName abc</span></span>

<span data-ttu-id="b3977-183">Másik lehetőségként a következő parancsfájl hello is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="b3977-183">Alternatively, you can run hello following script.</span></span> <span data-ttu-id="b3977-184">Adja meg a hello értékeket hello kötelező paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="b3977-184">Enter hello values for hello required parameters.</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for hello following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. <span data-ttu-id="b3977-185">PowerShell parancsfájl hello meg tooenter a hitelesítő adatok megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="b3977-185">hello PowerShell script prompts you tooenter your credentials.</span></span> <span data-ttu-id="b3977-186">Kétszer meg kell adnia őket, egyszer hello klasszikus telepítési modell fiókhoz tartozó, egyszer pedig hello Azure Resource Manager-fiókot.</span><span class="sxs-lookup"><span data-stu-id="b3977-186">Enter them twice, once for hello classic deployment model account and once for hello Azure Resource Manager account.</span></span>

2. <span data-ttu-id="b3977-187">Után a megadott hitelesítő adatait, hello parancsfájl egy ellenőrzés toodetermine fut, hogy az infrastruktúra-beállítás megfelel-e hello azt már korábban említettük követelmények.</span><span class="sxs-lookup"><span data-stu-id="b3977-187">After you've entered your credentials, hello script runs a check toodetermine whether your infrastructure setup meets hello previously mentioned requirements.</span></span>

3. <span data-ttu-id="b3977-188">Miután hello Előfeltételek be van jelölve, és megerősítette, hello tároló frissítést felszólító tooproceed áll.</span><span class="sxs-lookup"><span data-stu-id="b3977-188">After hello prerequisites have been checked and confirmed, you are prompted tooproceed with hello vault upgrade.</span></span> <span data-ttu-id="b3977-189">hello frissítési folyamat elindul, a tároló frissítése.</span><span class="sxs-lookup"><span data-stu-id="b3977-189">hello upgrade process starts upgrading your vault.</span></span> <span data-ttu-id="b3977-190">hello teljes frissítés 15 too30 perc toocomplete is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b3977-190">hello entire upgrade can take 15 too30 minutes toocomplete.</span></span>

4. <span data-ttu-id="b3977-191">Miután hello frissítése sikeresen befejeződött, hello frissített tároló hello új Azure-portálon végezheti el.</span><span class="sxs-lookup"><span data-stu-id="b3977-191">After hello upgrade has been completed successfully, you can access hello upgraded vault in hello new Azure portal.</span></span>

## <a name="post-upgrade-vault-management"></a><span data-ttu-id="b3977-192">A frissítés utáni tároló kezelése</span><span class="sxs-lookup"><span data-stu-id="b3977-192">Post-upgrade vault management</span></span>

### <a name="replicate-by-using-azure-site-recovery-in-hello-recovery-services-vault"></a><span data-ttu-id="b3977-193">A Recovery Services-tároló hello Azure Site Recovery segítségével replikálása</span><span class="sxs-lookup"><span data-stu-id="b3977-193">Replicate by using Azure Site Recovery in hello Recovery Services vault</span></span>

* <span data-ttu-id="b3977-194">Most egy régió tartozik tooanother védheti az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="b3977-194">You can now protect your Azure VMs from one region tooanother.</span></span> <span data-ttu-id="b3977-195">További információkért lásd: [Azure virtuális gépek replikálása az Azure Site Recovery régiók közötti](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="b3977-195">For more information, see [Replicate Azure VMs between regions with Azure Site Recovery](site-recovery-azure-to-azure.md).</span></span>

* <span data-ttu-id="b3977-196">VMware virtuális gépek tooAzure replikálása kapcsolatos további információkért lásd: [Site Recovery szolgáltatással replikálja a VMware virtuális gépek tooAzure](vmware-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b3977-196">For more information about replicating VMware VMs tooAzure, see [Replicate VMware VMs tooAzure with Site Recovery](vmware-walkthrough-overview.md).</span></span>

* <span data-ttu-id="b3977-197">Replikálása (VMM nélkül) a Hyper-V virtuális gépek tooAzure kapcsolatos további információkért lásd: [replikálásához a Hyper-V virtuális gépek (VMM nélkül) tooAzure](hyper-v-site-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b3977-197">For more information about replicating Hyper-V VMs (without VMM) tooAzure, see [Replicate Hyper-V virtual machines (without VMM) tooAzure](hyper-v-site-walkthrough-overview.md).</span></span>

* <span data-ttu-id="b3977-198">Hyper-V virtuális gépek (VMM) tooAzure replikálása kapcsolatos további információkért lásd: [replikálásához a Hyper-V virtuális gépek használata a Site Recovery VMM-felhők tooAzure hello Azure-portálon](vmm-to-azure-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b3977-198">For more information about replicating Hyper-V VMs (with VMM) tooAzure, see [Replicate Hyper-V virtual machines in VMM clouds tooAzure using Site Recovery in hello Azure portal](vmm-to-azure-walkthrough-overview.md).</span></span>

* <span data-ttu-id="b3977-199">További információ a Hyper-virtuális gépek (VMM) tooa másodlagos helyre replikál: [replikálásához a Hyper-V virtuális gépek a VMM felhők tooa másodlagos VMM hely használatával hello Azure-portálon](site-recovery-vmm-to-vmm.md).</span><span class="sxs-lookup"><span data-stu-id="b3977-199">For more information about replicating Hyper-VMs (with VMM) tooa secondary site, see [Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site using hello Azure portal](site-recovery-vmm-to-vmm.md).</span></span>

* <span data-ttu-id="b3977-200">További információ a VMware virtuális gépek tooa másodlagos helyre replikál: [replikálja a helyszíni VMware virtuális gépek vagy fizikai kiszolgálók tooa másodlagos hely a klasszikus Azure portálon hello](site-recovery-vmware-to-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="b3977-200">For more information about replicating VMware VMs tooa secondary site, see [Replicate on-premises VMware virtual machines or physical servers tooa secondary site in hello classic Azure portal](site-recovery-vmware-to-vmware.md).</span></span>

### <a name="view-your-replicated-items"></a><span data-ttu-id="b3977-201">A replikált elemek megtekintése</span><span class="sxs-lookup"><span data-stu-id="b3977-201">View your replicated items</span></span>

<span data-ttu-id="b3977-202">hello következő kép bemutatja hello Recovery Services tároló irányítópult megjelenítő lapon kulcs entitások hello tároló.</span><span class="sxs-lookup"><span data-stu-id="b3977-202">hello following image shows hello Recovery Services vault dashboard page that displays key entities for hello vault.</span></span> <span data-ttu-id="b3977-203">hello tárolóban, védett entitások listájának tooview válasszon **Site Recovery** > **replikált elemek**.</span><span class="sxs-lookup"><span data-stu-id="b3977-203">tooview a list of protected entities in hello vault, select **Site Recovery** > **Replicated items**.</span></span>


![Replikált elemek](./media/upgrade-site-recovery-vaults/replicateditems.png)

<span data-ttu-id="b3977-205">hello következő kép bemutatja a replikált elemek és hello megtekintésére hello munkafolyamat **feladatátvételi** parancsot a feladatátvétel kezdeményezése.</span><span class="sxs-lookup"><span data-stu-id="b3977-205">hello following image shows hello workflow for viewing your replicated items and hello **Failover** command for initiating a failover.</span></span>

![Replikált elemek](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a><span data-ttu-id="b3977-207">A replikációs beállítások megtekintése</span><span class="sxs-lookup"><span data-stu-id="b3977-207">View your replication settings</span></span>

<span data-ttu-id="b3977-208">Hello Site Recovery-tárolóban mindegyik védelmi csoporthoz úgy van konfigurálva, a Másolás gyakoriságát, a helyreállítási pontok megőrzésének ideje, a alkalmazás alkalmazáskonzisztens pillanatképek gyakorisága és a más replikációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="b3977-208">In hello Site Recovery vault, each protection group is configured with copy frequency, recovery point retention, frequency of application consistent snapshots, and other replication settings.</span></span> <span data-ttu-id="b3977-209">Replikációs házirend hello Recovery Services-tároló, ezek a beállítások vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b3977-209">In hello Recovery Services vault, these settings are configured as a replication policy.</span></span> <span data-ttu-id="b3977-210">hello házirend hello neve az üdvözlő védelmi csoportot vagy hello hello név *primarycloud_Policy*.</span><span class="sxs-lookup"><span data-stu-id="b3977-210">hello name of hello policy is hello name of hello protection group or hello *primarycloud_Policy*.</span></span>

<span data-ttu-id="b3977-211">További információ a replikációs házirendhez: [VMware tooAzure replikációs házirendjének kezeléséhez](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="b3977-211">For more information about replication policy, see [Manage replication policy for VMware tooAzure](site-recovery-setup-replication-settings-vmware.md).</span></span>
