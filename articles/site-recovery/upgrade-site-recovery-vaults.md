---
title: "A Site Recovery-tároló frissítsen az Azure Recovery Services-tároló"
description: "Ismerje meg, hogyan frissítheti az Azure Site Recovery-tároló Recovery Services-tároló"
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
ms.openlocfilehash: fdb33ea0d08353b491f2934fcf885fcb6910b9a2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upgrade-a-site-recovery-vault-to-an-azure-resource-manager-based-recovery-services-vault"></a><span data-ttu-id="b7692-103">A Site Recovery-tároló frissítsen az Azure Resource Manager-alapú Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="b7692-103">Upgrade a Site Recovery vault to an Azure Resource Manager-based Recovery Services vault</span></span>

<span data-ttu-id="b7692-104">Ez a cikk ismerteti, hogyan frissítése a helyreállítási szolgáltatás Azure Resource Manager-alapú tárolók nélkül folyamatban lévő replikáció hatással az Azure Site Recovery-tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="b7692-104">This article describes how to upgrade Azure Site Recovery vaults to Azure Resource Manager-based Recovery Service vaults without any impact on ongoing replication.</span></span> <span data-ttu-id="b7692-105">További információ az Azure erőforrás-kezelő szolgáltatásait és előnyeit: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7692-105">For more information about Azure Resource Manager features and benefits, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="b7692-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b7692-106">Introduction</span></span>
<span data-ttu-id="b7692-107">Recovery Services-tároló egy Azure Resource Manager szerinti erőforrás natív a felhőben a biztonsági mentés és katasztrófa-helyreállítás kezelését.</span><span class="sxs-lookup"><span data-stu-id="b7692-107">A Recovery Services vault is an Azure Resource Manager resource for managing backup and disaster recovery natively in the cloud.</span></span> <span data-ttu-id="b7692-108">Egy egységes tárolóra, használhatja az új Azure-portálon, és azt a felváltja a klasszikus biztonsági mentés és a Site Recovery-tárolók.</span><span class="sxs-lookup"><span data-stu-id="b7692-108">It is a unified vault that you can use in the new Azure portal, and it replaces the classic backup and Site Recovery vaults.</span></span>

<span data-ttu-id="b7692-109">Helyreállítási szolgáltatások tárolók funkciót, beleértve a tömbje kínál:</span><span class="sxs-lookup"><span data-stu-id="b7692-109">Recovery Services vaults offer an array of features, including:</span></span>

* <span data-ttu-id="b7692-110">Az Azure Resource Manager támogatása: védeni, és az Azure Resource Manager-verembe feladatátvételt a virtuális gépek és fizikai gépek.</span><span class="sxs-lookup"><span data-stu-id="b7692-110">Azure Resource Manager support: You can protect and fail over your virtual machines and physical machines into an Azure Resource Manager stack.</span></span>

* <span data-ttu-id="b7692-111">Lemez kizárása: Ha ideiglenes fájlokat vagy a teljes sávszélesség használata nem kívánt magas forgalom adatokat, kötetek lehet kizárni a replikálásból.</span><span class="sxs-lookup"><span data-stu-id="b7692-111">Exclude disk: If you have temp files or high churn data that you don’t want to use all your bandwidth for, you can exclude volumes from replication.</span></span> <span data-ttu-id="b7692-112">Ez a funkció jelenleg engedélyezve van *Azure VMware* és *Hyper-V Azure* és egyéb forgatókönyvek, valamint az időtartam.</span><span class="sxs-lookup"><span data-stu-id="b7692-112">This capability has been enabled currently in *VMware to Azure* and *Hyper-V to Azure* and is extended to other scenarios as well.</span></span>

* <span data-ttu-id="b7692-113">Támogatja a premium és helyileg redundáns tárolás: kiszolgálók most védelméhez a prémium szintű storage, amelyek lehetővé teszik az alkalmazások magasabb védelmét felhasználói fiókok bemeneti/kimeneti műveletek száma másodpercenként (IOPS).</span><span class="sxs-lookup"><span data-stu-id="b7692-113">Support for premium and locally redundant storage: You can now protect servers in premium storage accounts that allow customers to protect applications with higher input/output operations per second (IOPS).</span></span> <span data-ttu-id="b7692-114">Ez a funkció be van kapcsolva *Azure VMware*.</span><span class="sxs-lookup"><span data-stu-id="b7692-114">This capability is currently enabled in *VMware to Azure*.</span></span>

* <span data-ttu-id="b7692-115">Zökkenőmentes élményt első lépések: az első lépéseket élmény úgy tervezték, vész-helyreállítási telepítés megkönnyítése.</span><span class="sxs-lookup"><span data-stu-id="b7692-115">Streamlined getting-started experience: The enhanced getting-started experience has been designed to make disaster-recovery setup easy.</span></span>

* <span data-ttu-id="b7692-116">Biztonsági mentés és helyreállítás felügyeleti azonos tárolóból: most vész-helyreállítási kiszolgálók védelme vagy készítsen biztonsági mentést a terhelés jelentősen csökkentheti a felügyelet azonos tárolójából.</span><span class="sxs-lookup"><span data-stu-id="b7692-116">Backup and Site Recovery management from the same vault: You can now protect servers for disaster recovery or perform backup from the same vault, which can reduce your management overhead significantly.</span></span>

<span data-ttu-id="b7692-117">A frissített élmény és a szolgáltatásokkal kapcsolatos további információkért lásd: a [tároló, biztonsági mentési és helyreállítási blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span><span class="sxs-lookup"><span data-stu-id="b7692-117">For more information about the upgraded experience and features, see the [Storage, Backup, and Recovery blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span></span>

## <a name="salient-features"></a><span data-ttu-id="b7692-118">Következő funkciói</span><span class="sxs-lookup"><span data-stu-id="b7692-118">Salient features</span></span>

* <span data-ttu-id="b7692-119">Ne legyen hatással a folyamatban lévő replikáció: folyamatban lévő replikáció során minden megszakadása nélkül, és frissítés utáni.</span><span class="sxs-lookup"><span data-stu-id="b7692-119">No impact on ongoing replication: Ongoing replications continue without any interruption during and post upgrade.</span></span>

* <span data-ttu-id="b7692-120">További költség nélkül: egy frissített képességek teljes készlete beolvasása minden további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="b7692-120">No additional cost: Get an entire set of updated capabilities at no additional cost.</span></span>

* <span data-ttu-id="b7692-121">Adatvesztés nélküli: mivel ezt a folyamatot a frissítés és az áttelepítés nem, a meglévő replikációs helyreállítási pontok és beállításai változatlanok maradnak során, és a frissítés után.</span><span class="sxs-lookup"><span data-stu-id="b7692-121">No data loss: Because this process is an upgrade and not a migration, existing replication recovery points and settings remain intact during and after the upgrade.</span></span>


## <a name="what-happens-during-the-vault-upgrade"></a><span data-ttu-id="b7692-122">Mi történik, a tároló frissítés során?</span><span class="sxs-lookup"><span data-stu-id="b7692-122">What happens during the vault upgrade?</span></span>

<span data-ttu-id="b7692-123">A frissítés közben nem hajtható végre műveletek, például egy új kiszolgáló vagy a replikáció a virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="b7692-123">During the upgrade, you cannot perform operations such as registering a new server or enabling replication for a virtual machine (VM).</span></span> <span data-ttu-id="b7692-124">Műveletek, például az adatok írása a tárolóval, például a folyamatban lévő replikáció a tárolóba, a védett elemek vagy adatok olvasása megszakításmentes folytatásához.</span><span class="sxs-lookup"><span data-stu-id="b7692-124">Operations that involve reading data from or writing data to the vault, such as ongoing replication of protected items to the vault, continue uninterrupted.</span></span>

### <a name="changes-to-automation-and-tooling-after-the-upgrade"></a><span data-ttu-id="b7692-125">Automatizálási és a frissítés után tooling módosítása</span><span class="sxs-lookup"><span data-stu-id="b7692-125">Changes to automation and tooling after the upgrade</span></span>
<span data-ttu-id="b7692-126">A tároló típusa a klasszikus telepítési modellből a Resource Manager üzembe helyezési modellel frissít, frissítse a meglévő automatizációk vagy tooling annak érdekében, hogy az tovább működni a frissítés után.</span><span class="sxs-lookup"><span data-stu-id="b7692-126">As you upgrade the vault type from the classic deployment model to the Resource Manager deployment model, update the existing automation or tooling to ensure that it continues to work after the upgrade.</span></span>

### <a name="prepare-your-environment-for-the-upgrade"></a><span data-ttu-id="b7692-127">A frissítés a környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="b7692-127">Prepare your environment for the upgrade</span></span>

* [<span data-ttu-id="b7692-128">Telepítse a PowerShell, vagy frissítse az 5-ös vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="b7692-128">Install PowerShell or upgrade it to version 5 or later</span></span>](https://www.microsoft.com/download/details.aspx?id=50395)
* [<span data-ttu-id="b7692-129">Telepítse a legújabb verziót az Azure PowerShell MSI</span><span class="sxs-lookup"><span data-stu-id="b7692-129">Install the latest version of Azure PowerShell MSI</span></span>](https://github.com/Azure/azure-powershell/releases)
* [<span data-ttu-id="b7692-130">A Recovery Services-tároló frissítési parancsfájl letöltése</span><span class="sxs-lookup"><span data-stu-id="b7692-130">Download the Recovery Services vault upgrade script</span></span>](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a><span data-ttu-id="b7692-131">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b7692-131">Prerequisites</span></span>
<span data-ttu-id="b7692-132">A Site Recovery-tárolóhoz frissíthet helyreállítási szolgáltatás Azure Resource Manager-alapú tárolók, a következő követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="b7692-132">To upgrade from Site Recovery vaults to Azure Resource Manager-based Recovery Service vaults, you must meet the following requirements:</span></span>

* <span data-ttu-id="b7692-133">Minimális verziója: az Azure Site Recovery Provider a kiszolgálón telepítve kell lennie 5.1.1700.0 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b7692-133">Minimum agent version: The version of Azure Site Recovery Provider installed on your server must be 5.1.1700.0 or later.</span></span>

* <span data-ttu-id="b7692-134">Támogatott konfiguráció: a tároló nem lehet konfigurálni a tárolóhálózat (SAN) vagy SQL Server AlwaysOn rendelkezésre állási csoportok.</span><span class="sxs-lookup"><span data-stu-id="b7692-134">Supported configuration: You cannot configure your vault with storage area network (SAN) or SQL Server AlwaysOn Availability Groups.</span></span> <span data-ttu-id="b7692-135">Minden más konfigurációk vannak támogatva.</span><span class="sxs-lookup"><span data-stu-id="b7692-135">All other configurations are supported.</span></span>

    >[!NOTE]
    ><span data-ttu-id="b7692-136">A frissítés után tárolási leképezése csak a PowerShell segítségével is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="b7692-136">After the upgrade, you can manage storage mapping only via PowerShell.</span></span>

* <span data-ttu-id="b7692-137">Támogatott központi telepítési forgatókönyv: A tároló nem lehet a *Azure VMware* a hagyományos telepítési modellt.</span><span class="sxs-lookup"><span data-stu-id="b7692-137">Supported deployment scenario: Your vault shouldn’t be the *VMware to Azure* legacy deployment model.</span></span> <span data-ttu-id="b7692-138">Mielőtt továbblépne, először helyezze át a továbbfejlesztett üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="b7692-138">Before you proceed, first move to the enhanced deployment model.</span></span>

* <span data-ttu-id="b7692-139">A felhasználó által kezdeményezett nem aktív feladatok, például a felügyeleti műveletek sík: mert a frissítés során a felügyeleti vezérlősík elérése történik, hajtsa végre a felügyeleti vezérlősík műveletet indít el a frissítés előtt.</span><span class="sxs-lookup"><span data-stu-id="b7692-139">No active user-initiated jobs that involve management plane operations: Because access to the management plane is restricted during upgrade, complete all your management plane actions before you trigger the upgrade.</span></span> <span data-ttu-id="b7692-140">Ez a folyamat nem tartalmazza a folyamatban lévő replikáció.</span><span class="sxs-lookup"><span data-stu-id="b7692-140">This process doesn’t include ongoing replication.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="b7692-141">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="b7692-141">Frequently asked questions</span></span>

<span data-ttu-id="b7692-142">**Ez a frissítés hatással van a folyamatban lévő replikáció?**</span><span class="sxs-lookup"><span data-stu-id="b7692-142">**Does this upgrade affect my ongoing replication?**</span></span>

<span data-ttu-id="b7692-143">Nem.</span><span class="sxs-lookup"><span data-stu-id="b7692-143">No.</span></span> <span data-ttu-id="b7692-144">A folyamatban lévő replikáció továbbra is folyamatos, során, és a frissítés után.</span><span class="sxs-lookup"><span data-stu-id="b7692-144">Your ongoing replication continues uninterrupted during and after the upgrade.</span></span>

<span data-ttu-id="b7692-145">**Mi történik a hálózati beállítások, például a pont-pont VPN és IP-beállítások?**</span><span class="sxs-lookup"><span data-stu-id="b7692-145">**What happens to network settings such as site-to-site VPN and IP settings?**</span></span>

<span data-ttu-id="b7692-146">A frissítés nincs hatással a hálózati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="b7692-146">The upgrade doesn't affect the network settings.</span></span> <span data-ttu-id="b7692-147">Az összes Azure-– helyi kapcsolat változatlanok maradnak.</span><span class="sxs-lookup"><span data-stu-id="b7692-147">All Azure-to-on-premises connections remain intact.</span></span>

<span data-ttu-id="b7692-148">**Mi történik a tárolók, ha szeretnék nem kívánja frissíteni a közeljövőben?**</span><span class="sxs-lookup"><span data-stu-id="b7692-148">**What happens to my vaults if I don’t plan to upgrade in the near future?**</span></span>

<span data-ttu-id="b7692-149">A régi Azure-portálon a Site Recovery-tároló támogatása szeptember 2017 indítása megszűnnek.</span><span class="sxs-lookup"><span data-stu-id="b7692-149">Support for Site Recovery vault in the old Azure portal will be deprecated starting September 2017.</span></span> <span data-ttu-id="b7692-150">Határozottan javasoljuk, hogy a frissítési szolgáltatás használatával helyezze át az új portál.</span><span class="sxs-lookup"><span data-stu-id="b7692-150">We strongly recommend that you use the upgrade feature to move to the new portal.</span></span>

<span data-ttu-id="b7692-151">**Mi az áttelepítési terv jelent a saját meglévő eszközt használunk erre?**</span><span class="sxs-lookup"><span data-stu-id="b7692-151">**What does this migration plan mean for my existing tooling?**</span></span>  

<span data-ttu-id="b7692-152">A Resource Manager üzembe helyezési modellben a tooling frissítése az egyik legfontosabb módosításait, figyelembe kell venni a frissítési tervek.</span><span class="sxs-lookup"><span data-stu-id="b7692-152">Updating your tooling to the Resource Manager deployment model is one of the most important changes that you must account for in your upgrade plans.</span></span> <span data-ttu-id="b7692-153">A Recovery Services-tárolók a Resource Manager üzembe helyezési modellel alapulnak.</span><span class="sxs-lookup"><span data-stu-id="b7692-153">The Recovery Services vaults are based on the Resource Manager deployment model.</span></span> 

<span data-ttu-id="b7692-154">**A felügyeleti-vezérlősík állásidő mennyi ideig does utolsó?**</span><span class="sxs-lookup"><span data-stu-id="b7692-154">**How long does the management-plane downtime last?**</span></span>

<span data-ttu-id="b7692-155">A frissítés általában a körülbelül 15 – 30 percet vesz igénybe, és legfeljebb egy órával eltarthat.</span><span class="sxs-lookup"><span data-stu-id="b7692-155">The upgrade ordinarily takes about 15 to 30 minutes, and it could take up to a maximum of one hour.</span></span>

<span data-ttu-id="b7692-156">**I vissza lehet vonni a frissítés után?**</span><span class="sxs-lookup"><span data-stu-id="b7692-156">**Can I roll back after upgrading?**</span></span>

<span data-ttu-id="b7692-157">Nem.</span><span class="sxs-lookup"><span data-stu-id="b7692-157">No.</span></span> <span data-ttu-id="b7692-158">Miután az erőforrások frissítése sikeresen befejeződött a rollback utasítás nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b7692-158">Rollback is not supported after the resources have been successfully upgraded.</span></span>

<span data-ttu-id="b7692-159">**Az előfizetés vagy erőforrások megjelenítéséhez, hogy azok frissítése is ellenőrzi?**</span><span class="sxs-lookup"><span data-stu-id="b7692-159">**Can I validate my subscription or resources to see whether they can be upgraded?**</span></span>

<span data-ttu-id="b7692-160">Igen.</span><span class="sxs-lookup"><span data-stu-id="b7692-160">Yes.</span></span> <span data-ttu-id="b7692-161">A platform által támogatott frissítési lehetőséget a frissítés első lépése, hogy ellenőrizze, hogy az erőforrások képesek-e a frissítésre.</span><span class="sxs-lookup"><span data-stu-id="b7692-161">In the platform-supported upgrade option, the first step in the upgrade is to validate that the resources are capable of an upgrade.</span></span> <span data-ttu-id="b7692-162">Ha az érvényesítés meghiúsul, a megfelelő hibaüzeneteket és figyelmeztetéseket fog kapni.</span><span class="sxs-lookup"><span data-stu-id="b7692-162">If the validation fails, you will receive appropriate error messages or warnings.</span></span>

<span data-ttu-id="b7692-163">**Hogyan jelenthetem a problémát a frissítést?**</span><span class="sxs-lookup"><span data-stu-id="b7692-163">**How do I report an issue with the upgrade?**</span></span>

<span data-ttu-id="b7692-164">A frissítés során a esetleges hibákat tapasztal, vegye figyelembe a műveleti azonosító szerepel-e a hibát.</span><span class="sxs-lookup"><span data-stu-id="b7692-164">If you experience any failures during the upgrade, note the operation ID that's listed in the error.</span></span> <span data-ttu-id="b7692-165">Microsoft Support proaktív módon fog működni a probléma megoldását.</span><span class="sxs-lookup"><span data-stu-id="b7692-165">Microsoft Support will proactively work on resolving the issue.</span></span> <span data-ttu-id="b7692-166">Forduljon a támogatási csoportot, az előfizetés-azonosító, a tároló neve és a művelet azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b7692-166">You can also contact the Support team with your subscription ID, vault name, and operation ID.</span></span> <span data-ttu-id="b7692-167">Támogatás a probléma megoldásához minél gyorsabban fog működni.</span><span class="sxs-lookup"><span data-stu-id="b7692-167">Support will work to resolve the issue as quickly as possible.</span></span> <span data-ttu-id="b7692-168">Nem próbálja meg újra a műveletet csak akkor vannak explicit módon kifejezetten kéri a Microsoft által.</span><span class="sxs-lookup"><span data-stu-id="b7692-168">Do not retry the operation unless you are explicitly instructed to do so by Microsoft.</span></span>

## <a name="run-the-script"></a><span data-ttu-id="b7692-169">A parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="b7692-169">Run the script</span></span>

<span data-ttu-id="b7692-170">A PowerShell a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b7692-170">In PowerShell, run the following command:</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* <span data-ttu-id="b7692-171">Előfizetés-azonosító: Az előfizetés-azonosító, amely a tárolóra, frissítés van társítva.</span><span class="sxs-lookup"><span data-stu-id="b7692-171">SubscriptionID: The subscription ID that's associated with the vault that you're upgrading.</span></span>

* <span data-ttu-id="b7692-172">VaultName: A tárolóra, frissítés neve.</span><span class="sxs-lookup"><span data-stu-id="b7692-172">VaultName: The name of the vault that you're upgrading.</span></span>

* <span data-ttu-id="b7692-173">Hely: A tárolóra, frissítés helye.</span><span class="sxs-lookup"><span data-stu-id="b7692-173">Location: The location of the vault that you're upgrading.</span></span>

* <span data-ttu-id="b7692-174">Indítása. ResourceType: A Site Recovery HyperVRecoveryManagerVault tárolók.</span><span class="sxs-lookup"><span data-stu-id="b7692-174">ResourceType: HyperVRecoveryManagerVault for Site Recovery vaults.</span></span>

* <span data-ttu-id="b7692-175">TargetResourceGroupName: A szeretné helyezni a frissített tároló erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="b7692-175">TargetResourceGroupName: The resource group into which you want the upgraded vault to be placed.</span></span> <span data-ttu-id="b7692-176">TargetResourceGroupName lehet egy meglévő erőforráscsoportot az Azure Resource Manager vagy egy újat.</span><span class="sxs-lookup"><span data-stu-id="b7692-176">TargetResourceGroupName can be an existing resource group in Azure Resource Manager or a new one.</span></span> <span data-ttu-id="b7692-177">Ha a megadott TargetResourceGroupName nem létezik, és a tárolónak ugyanazon a helyen a frissítés részeként létrehozták.</span><span class="sxs-lookup"><span data-stu-id="b7692-177">If the TargetResourceGroupName that's supplied does not exist, it is created as part of the upgrade in the same location as the vault.</span></span> <span data-ttu-id="b7692-178">További információkért lásd: a "Erőforráscsoportok" szakaszában [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="b7692-178">For more information, see the "Resource groups" section of [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

    >[!NOTE]
    ><span data-ttu-id="b7692-179">Az erőforrás-csoport elnevezési bizonyos megkötéseknek.</span><span class="sxs-lookup"><span data-stu-id="b7692-179">Resource group naming is subject to certain constraints.</span></span> <span data-ttu-id="b7692-180">Tároló frissítés nem sikerülne elkerülése érdekében ügyeljen arra, hogy az elnevezési gondosan figyelje meg.</span><span class="sxs-lookup"><span data-stu-id="b7692-180">To prevent vault upgrade failure, be sure to observe the naming convention carefully.</span></span>
    >
    ><span data-ttu-id="b7692-181">Példa:</span><span class="sxs-lookup"><span data-stu-id="b7692-181">For example:</span></span>
    >
    ><span data-ttu-id="b7692-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 - SubscriptionId 1234-54123-354354-56416-8645 - VaultName gen2dr-helyre "Észak-Európa" - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc</span><span class="sxs-lookup"><span data-stu-id="b7692-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionId 1234-54123-354354-56416-8645 -VaultName gen2dr -Location "north europe" -ResourceType hypervrecoverymanagervault -TargetResourceGroupName abc</span></span>

<span data-ttu-id="b7692-183">Ehelyett futtathatja a következő parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b7692-183">Alternatively, you can run the following script.</span></span> <span data-ttu-id="b7692-184">Adja meg az értékeket a szükséges paramétereket.</span><span class="sxs-lookup"><span data-stu-id="b7692-184">Enter the values for the required parameters.</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for the following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. <span data-ttu-id="b7692-185">A PowerShell-parancsfájlt a hitelesítő adatok megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="b7692-185">The PowerShell script prompts you to enter your credentials.</span></span> <span data-ttu-id="b7692-186">Kétszer meg kell adnia őket, egyszer a klasszikus üzembe helyezési modell fiókhoz tartozó és egyszer az Azure Resource Manager-fiók.</span><span class="sxs-lookup"><span data-stu-id="b7692-186">Enter them twice, once for the classic deployment model account and once for the Azure Resource Manager account.</span></span>

2. <span data-ttu-id="b7692-187">Után a megadott hitelesítő adatait, a parancsfájl fut egy ellenőrzést annak meghatározásához, hogy az infrastruktúra telepítése megfelel-e a korábban leírt követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="b7692-187">After you've entered your credentials, the script runs a check to determine whether your infrastructure setup meets the previously mentioned requirements.</span></span>

3. <span data-ttu-id="b7692-188">Miután az Előfeltételek be van jelölve, és megerősítette, kéri a tároló frissítés folytatásához.</span><span class="sxs-lookup"><span data-stu-id="b7692-188">After the prerequisites have been checked and confirmed, you are prompted to proceed with the vault upgrade.</span></span> <span data-ttu-id="b7692-189">A frissítési folyamat elindul, a tároló frissítése.</span><span class="sxs-lookup"><span data-stu-id="b7692-189">The upgrade process starts upgrading your vault.</span></span> <span data-ttu-id="b7692-190">A teljes frissítés 15-30 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b7692-190">The entire upgrade can take 15 to 30 minutes to complete.</span></span>

4. <span data-ttu-id="b7692-191">Miután a frissítés sikeresen befejeződött, a frissített tárolót az új Azure-portálon végezheti el.</span><span class="sxs-lookup"><span data-stu-id="b7692-191">After the upgrade has been completed successfully, you can access the upgraded vault in the new Azure portal.</span></span>

## <a name="post-upgrade-vault-management"></a><span data-ttu-id="b7692-192">A frissítés utáni tároló kezelése</span><span class="sxs-lookup"><span data-stu-id="b7692-192">Post-upgrade vault management</span></span>

### <a name="replicate-by-using-azure-site-recovery-in-the-recovery-services-vault"></a><span data-ttu-id="b7692-193">A Recovery Services-tároló az Azure Site Recovery segítségével replikálása</span><span class="sxs-lookup"><span data-stu-id="b7692-193">Replicate by using Azure Site Recovery in the Recovery Services vault</span></span>

* <span data-ttu-id="b7692-194">Másik egy régióban most megvédheti az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="b7692-194">You can now protect your Azure VMs from one region to another.</span></span> <span data-ttu-id="b7692-195">További információkért lásd: [Azure virtuális gépek replikálása az Azure Site Recovery régiók közötti](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="b7692-195">For more information, see [Replicate Azure VMs between regions with Azure Site Recovery](site-recovery-azure-to-azure.md).</span></span>

* <span data-ttu-id="b7692-196">További információ a VMware virtuális gépek replikálása Azure-bA: [VMware virtuális gépek replikálása az Azure Site Recovery szolgáltatással](vmware-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7692-196">For more information about replicating VMware VMs to Azure, see [Replicate VMware VMs to Azure with Site Recovery](vmware-walkthrough-overview.md).</span></span>

* <span data-ttu-id="b7692-197">További információ a (VMM nélkül) a Hyper-V virtuális gépek replikálása Azure-bA: [replikálásához a Hyper-V virtuális gépek (VMM nélkül) az Azure-bA](hyper-v-site-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7692-197">For more information about replicating Hyper-V VMs (without VMM) to Azure, see [Replicate Hyper-V virtual machines (without VMM) to Azure](hyper-v-site-walkthrough-overview.md).</span></span>

* <span data-ttu-id="b7692-198">(A VMM-mel) a Hyper-V virtuális gépek replikálása Azure-bA kapcsolatos további információkért lásd: [replikálásához a Hyper-V virtuális gépek VMM-felhőkben az Azure Site Recovery használata az Azure portálon](vmm-to-azure-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7692-198">For more information about replicating Hyper-V VMs (with VMM) to Azure, see [Replicate Hyper-V virtual machines in VMM clouds to Azure using Site Recovery in the Azure portal](vmm-to-azure-walkthrough-overview.md).</span></span>

* <span data-ttu-id="b7692-199">További információ a Hyper-virtuális gépek (a VMM-mel) replikálása egy másodlagos hely: [replikálásához a Hyper-V virtuális gépek VMM-felhőkben VMM másodlagos hely az Azure portál használatával](site-recovery-vmm-to-vmm.md).</span><span class="sxs-lookup"><span data-stu-id="b7692-199">For more information about replicating Hyper-VMs (with VMM) to a secondary site, see [Replicate Hyper-V virtual machines in VMM clouds to a secondary VMM site using the Azure portal](site-recovery-vmm-to-vmm.md).</span></span>

* <span data-ttu-id="b7692-200">További információ a VMware virtuális gépek replikálása másodlagos helyre: [replikálja a helyszíni VMware virtuális gépek vagy fizikai kiszolgálók egy másodlagos helyre, a klasszikus Azure portálon](site-recovery-vmware-to-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="b7692-200">For more information about replicating VMware VMs to a secondary site, see [Replicate on-premises VMware virtual machines or physical servers to a secondary site in the classic Azure portal](site-recovery-vmware-to-vmware.md).</span></span>

### <a name="view-your-replicated-items"></a><span data-ttu-id="b7692-201">A replikált elemek megtekintése</span><span class="sxs-lookup"><span data-stu-id="b7692-201">View your replicated items</span></span>

<span data-ttu-id="b7692-202">A következő kép bemutatja a Recovery Services tároló irányítópult-oldalon, amely megjeleníti a tároló kulcs entitások.</span><span class="sxs-lookup"><span data-stu-id="b7692-202">The following image shows the Recovery Services vault dashboard page that displays key entities for the vault.</span></span> <span data-ttu-id="b7692-203">Válassza ki, ha a védett entitás listáját a tárolóban, **Site Recovery** > **replikált elemek**.</span><span class="sxs-lookup"><span data-stu-id="b7692-203">To view a list of protected entities in the vault, select **Site Recovery** > **Replicated items**.</span></span>


![Replikált elemek](./media/upgrade-site-recovery-vaults/replicateditems.png)

<span data-ttu-id="b7692-205">A következő kép bemutatja a munkafolyamat a replikált elemek megtekintéséhez és a **feladatátvételi** parancsot a feladatátvétel kezdeményezése.</span><span class="sxs-lookup"><span data-stu-id="b7692-205">The following image shows the workflow for viewing your replicated items and the **Failover** command for initiating a failover.</span></span>

![Replikált elemek](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a><span data-ttu-id="b7692-207">A replikációs beállítások megtekintése</span><span class="sxs-lookup"><span data-stu-id="b7692-207">View your replication settings</span></span>

<span data-ttu-id="b7692-208">A Site Recovery-tárolóban mindegyik védelmi csoporthoz úgy van konfigurálva, a Másolás gyakoriságát, a helyreállítási pontok megőrzésének ideje, a alkalmazás alkalmazáskonzisztens pillanatképek gyakorisága és a más replikációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="b7692-208">In the Site Recovery vault, each protection group is configured with copy frequency, recovery point retention, frequency of application consistent snapshots, and other replication settings.</span></span> <span data-ttu-id="b7692-209">A Recovery Services-tároló ezek a beállítások vannak konfigurálva replikációs házirend.</span><span class="sxs-lookup"><span data-stu-id="b7692-209">In the Recovery Services vault, these settings are configured as a replication policy.</span></span> <span data-ttu-id="b7692-210">A házirend neve a védelmi csoport neve vagy a *primarycloud_Policy*.</span><span class="sxs-lookup"><span data-stu-id="b7692-210">The name of the policy is the name of the protection group or the *primarycloud_Policy*.</span></span>

<span data-ttu-id="b7692-211">További információ a replikációs házirendhez: [replikációs házirend kezelése az Azure-bA VMware](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="b7692-211">For more information about replication policy, see [Manage replication policy for VMware to Azure](site-recovery-setup-replication-settings-vmware.md).</span></span>
