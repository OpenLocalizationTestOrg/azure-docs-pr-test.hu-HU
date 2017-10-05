---
title: "Azure Automation-runbook hozzáadása az Azure Site Recovery helyreállítási tervek |} Microsoft Docs"
description: "Ismerje meg, hogyan Azure Site Recovery segítségével a helyreállítási terv kiterjesztése az Azure Automation használatával. Útmutató: Azure történő helyreállítás során az összetett feladatok elvégzéséhez."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 064a6782970b950543f93c24800998c1f104c8df
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a><span data-ttu-id="1372e-104">Azure Automation-runbook hozzáadása a helyreállítási terv</span><span class="sxs-lookup"><span data-stu-id="1372e-104">Add Azure Automation runbooks to recovery plans</span></span>
<span data-ttu-id="1372e-105">Ez a cikk azt ismerteti hogyan integrálható az Azure Site Recovery Azure Automation segítséget a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="1372e-105">In this article, we describe how Azure Site Recovery integrates with Azure Automation to help you extend your recovery plans.</span></span> <span data-ttu-id="1372e-106">A helyreállítási terv lehet levezényelni a Site Recovery védett virtuális gépek helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="1372e-106">Recovery plans can orchestrate recovery of VMs that are protected with Site Recovery.</span></span> <span data-ttu-id="1372e-107">A helyreállítási terv a replikációs másodlagos felhőhöz, és a replikálást az Azure-működik.</span><span class="sxs-lookup"><span data-stu-id="1372e-107">Recovery plans work both for replication to a secondary cloud, and for replication to Azure.</span></span> <span data-ttu-id="1372e-108">A helyreállítási terv is segít a helyreállítási **következetesen pontos**, **ismételhető**, és **automatizált**.</span><span class="sxs-lookup"><span data-stu-id="1372e-108">Recovery plans also help make the recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="1372e-109">A rendszer átadja a virtuális gépek Azure-ba, ha az integráció az Azure Automation szolgáltatásban, a helyreállítási terv terjeszti ki.</span><span class="sxs-lookup"><span data-stu-id="1372e-109">If you fail over your VMs to Azure, integration with Azure Automation extends your recovery plans.</span></span> <span data-ttu-id="1372e-110">A runbookok, amelyek hatékony automatizálási feladatok végrehajtásához használható.</span><span class="sxs-lookup"><span data-stu-id="1372e-110">You can use it to execute runbooks, which offer powerful automation tasks.</span></span>

<span data-ttu-id="1372e-111">Ha most ismerkedik az Azure Automation, akkor [regisztráljon](https://azure.microsoft.com/services/automation/) és [mintaparancsfájlok letöltése](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="1372e-111">If you are new to Azure Automation, you can [sign up](https://azure.microsoft.com/services/automation/) and [download sample scripts](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="1372e-112">További információt, és megtudhatja, miként kell levezényelni a helyreállítási Azure használatával [helyreállítási tervek](https://azure.microsoft.com/blog/?p=166264), lásd: [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="1372e-112">For more information, and to learn how to orchestrate recovery to Azure by using [recovery plans](https://azure.microsoft.com/blog/?p=166264), see [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span></span>

<span data-ttu-id="1372e-113">Ez a cikk azt ismerteti hogyan integrálható az Azure Automation-forgatókönyv be a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="1372e-113">In this article, we describe how you can integrate Azure Automation runbooks into your recovery plans.</span></span> <span data-ttu-id="1372e-114">Példák segítségével, amely korábban a kézi beavatkozás szükséges alapvető feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="1372e-114">We use examples to automate basic tasks that previously required manual intervention.</span></span> <span data-ttu-id="1372e-115">Azt is ismertetik a többlépéses helyreállítási átalakítása egy kattintással indítható helyreállítási művelet.</span><span class="sxs-lookup"><span data-stu-id="1372e-115">We also describe how to convert a multi-step recovery to a single-click recovery action.</span></span>

## <a name="customize-the-recovery-plan"></a><span data-ttu-id="1372e-116">A helyreállítási terv testreszabása</span><span class="sxs-lookup"><span data-stu-id="1372e-116">Customize the recovery plan</span></span>
1. <span data-ttu-id="1372e-117">Lépjen a **Site Recovery** helyreállítási terv erőforráspanelen.</span><span class="sxs-lookup"><span data-stu-id="1372e-117">Go to the **Site Recovery** recovery plan resource blade.</span></span> <span data-ttu-id="1372e-118">Az ebben a példában a helyreállítási tervben szereplő két virtuális gépek hozzáadott, a helyreállításhoz.</span><span class="sxs-lookup"><span data-stu-id="1372e-118">For this example, the recovery plan has two VMs added to it, for recovery.</span></span> <span data-ttu-id="1372e-119">Egy runbook hozzáadása megkezdéséhez kattintson a **Testreszabás** fülre.</span><span class="sxs-lookup"><span data-stu-id="1372e-119">To begin adding a runbook, click the **Customize** tab.</span></span>

    ![A Testreszabás gombra.](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. <span data-ttu-id="1372e-121">Kattintson a jobb gombbal **csoport 1: Start**, majd válassza ki **post művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1372e-121">Right-click **Group 1: Start**, and then select **Add post action**.</span></span>

    ![Kattintson a jobb gombbal csoport 1: Start, és a post művelet hozzáadása](media/site-recovery-runbook-automation-new/customize-rp.png)

3. <span data-ttu-id="1372e-123">Kattintson a **válasszon egy parancsfájlt**.</span><span class="sxs-lookup"><span data-stu-id="1372e-123">Click **Choose a script**.</span></span>

4. <span data-ttu-id="1372e-124">Az a **műveletének frissítése** panelen, a parancsfájl nevét **Hello World**.</span><span class="sxs-lookup"><span data-stu-id="1372e-124">On the **Update action** blade, name the script **Hello World**.</span></span>

    ![A frissítési művelet panel](media/site-recovery-runbook-automation-new/update-rp.png)

5. <span data-ttu-id="1372e-126">Adja meg az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="1372e-126">Enter an Automation account name.</span></span>
    >[!NOTE]
    > <span data-ttu-id="1372e-127">Az Automation-fiók lehet bármely Azure régióban.</span><span class="sxs-lookup"><span data-stu-id="1372e-127">The Automation account can be in any Azure region.</span></span> <span data-ttu-id="1372e-128">Az Automation-fiók ugyanazt az előfizetést, és az Azure Site Recovery-tárolónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1372e-128">The Automation account must be in the same subscription as the Azure Site Recovery vault.</span></span>

6. <span data-ttu-id="1372e-129">Az Automation-fiók válasszon egy runbookot.</span><span class="sxs-lookup"><span data-stu-id="1372e-129">In your Automation account, select a runbook.</span></span> <span data-ttu-id="1372e-130">Ez a forgatókönyv az első csoport a helyreállítás után a helyreállítási terv végrehajtása közben futtatott parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="1372e-130">This runbook is the script that runs during the execution of the recovery plan, after the recovery of the first group.</span></span>

7. <span data-ttu-id="1372e-131">A parancsfájl mentése, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="1372e-131">To save the script, click **OK**.</span></span> <span data-ttu-id="1372e-132">A parancsfájl hozzáadódik **csoport 1: utáni lépéseket**.</span><span class="sxs-lookup"><span data-stu-id="1372e-132">The script is added to **Group 1: Post-steps**.</span></span>

    ![Követő műveletet csoport 1:Start](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a><span data-ttu-id="1372e-134">A parancsfájl hozzáadása szempontjai</span><span class="sxs-lookup"><span data-stu-id="1372e-134">Considerations for adding a script</span></span>

* <span data-ttu-id="1372e-135">A beállítások **egy lépés törlése** vagy **frissítése a parancsfájl**, kattintson a jobb gombbal a parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="1372e-135">For options to **delete a step** or **update the script**, right-click the script.</span></span>
* <span data-ttu-id="1372e-136">Egy parancsfájlt Azure feladatátvétel során a helyi gépről az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="1372e-136">A script can run on Azure during failover from an on-premises machine to Azure.</span></span> <span data-ttu-id="1372e-137">Is futtatható Azure leállítás, mielőtt elsődleges webhelyeken parancsfájlként feladat-visszavétel során az Azure-ból a helyi géphez.</span><span class="sxs-lookup"><span data-stu-id="1372e-137">It also can run on Azure as a primary-site script before shutdown, during failback from Azure to an on-premises machine.</span></span>
* <span data-ttu-id="1372e-138">Parancsfájl futtatása, a helyreállítási terv környezet esetében.</span><span class="sxs-lookup"><span data-stu-id="1372e-138">When a script runs, it injects a recovery plan context.</span></span> <span data-ttu-id="1372e-139">A következő példa bemutatja, egy környezeti változó:</span><span class="sxs-lookup"><span data-stu-id="1372e-139">The following example shows a context variable:</span></span>

    ```
            {"RecoveryPlanName":"hrweb-recovery",

            "FailoverType":"Test",

            "FailoverDirection":"PrimaryToSecondary",

            "GroupId":"1",

            "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                    "ResourceGroupName":"ContosoRG",

                    "CloudServiceName":"pod02hrweb-Chicago-test",

                    "RoleName":"Fabrikam-Hrweb-frontend-test",

                    "RecoveryPointId":"TimeStamp"}

                    }

            }
    ```

    <span data-ttu-id="1372e-140">A következő táblázat nevének és leírásának minden változó az adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="1372e-140">The following table lists the name and description of each variable in the context.</span></span>

    | <span data-ttu-id="1372e-141">**Változó neve**</span><span class="sxs-lookup"><span data-stu-id="1372e-141">**Variable name**</span></span> | <span data-ttu-id="1372e-142">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="1372e-142">**Description**</span></span> |
    | --- | --- |
    | <span data-ttu-id="1372e-143">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="1372e-143">RecoveryPlanName</span></span> |<span data-ttu-id="1372e-144">A futtatandó terv neve.</span><span class="sxs-lookup"><span data-stu-id="1372e-144">The name of the plan being run.</span></span> <span data-ttu-id="1372e-145">Ez a változó segítségével különböző műveletek a helyreállítási terv neve alapján.</span><span class="sxs-lookup"><span data-stu-id="1372e-145">This variable helps you take different actions based on the recovery plan name.</span></span> <span data-ttu-id="1372e-146">A parancsfájl is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="1372e-146">You also can reuse the script.</span></span> |
    | <span data-ttu-id="1372e-147">FailoverType</span><span class="sxs-lookup"><span data-stu-id="1372e-147">FailoverType</span></span> |<span data-ttu-id="1372e-148">Megadja, hogy a feladatátvételi teszt, tervezett vagy nem tervezett.</span><span class="sxs-lookup"><span data-stu-id="1372e-148">Specifies whether the failover is a test, planned, or unplanned.</span></span> |
    | <span data-ttu-id="1372e-149">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="1372e-149">FailoverDirection</span></span> |<span data-ttu-id="1372e-150">Megadja, hogy helyreállítási elsődleges vagy másodlagos helyhez.</span><span class="sxs-lookup"><span data-stu-id="1372e-150">Specifies whether recovery is to a primary or secondary site.</span></span> |
    | <span data-ttu-id="1372e-151">Csoportazonosító</span><span class="sxs-lookup"><span data-stu-id="1372e-151">GroupID</span></span> |<span data-ttu-id="1372e-152">A helyreállítási terv az számát határozza meg a csomag futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="1372e-152">Identifies the group number in the recovery plan when the plan is running.</span></span> |
    | <span data-ttu-id="1372e-153">VmMap</span><span class="sxs-lookup"><span data-stu-id="1372e-153">VmMap</span></span> |<span data-ttu-id="1372e-154">A csoportban lévő összes virtuális gép tömbjét.</span><span class="sxs-lookup"><span data-stu-id="1372e-154">An array of all VMs in the group.</span></span> |
    | <span data-ttu-id="1372e-155">VMMap kulcs</span><span class="sxs-lookup"><span data-stu-id="1372e-155">VMMap key</span></span> |<span data-ttu-id="1372e-156">Egyedi kulcs (GUID) az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="1372e-156">A unique key (GUID) for each VM.</span></span> <span data-ttu-id="1372e-157">Megegyezik a az Azure Virtual Machine Manager (VMM) azonosítója a virtuális gép szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="1372e-157">It's the same as the Azure Virtual Machine Manager (VMM) ID of the VM, where applicable.</span></span> |
    | <span data-ttu-id="1372e-158">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="1372e-158">SubscriptionId</span></span> |<span data-ttu-id="1372e-159">Az Azure-előfizetése Azonosítóját, amelyben a virtuális gép létrehozása történt.</span><span class="sxs-lookup"><span data-stu-id="1372e-159">The Azure subscription ID in which the VM was created.</span></span> |
    | <span data-ttu-id="1372e-160">RoleName</span><span class="sxs-lookup"><span data-stu-id="1372e-160">RoleName</span></span> |<span data-ttu-id="1372e-161">A helyreállítás alatt álló Azure virtuális gép neve.</span><span class="sxs-lookup"><span data-stu-id="1372e-161">The name of the Azure VM that's being recovered.</span></span> |
    | <span data-ttu-id="1372e-162">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="1372e-162">CloudServiceName</span></span> |<span data-ttu-id="1372e-163">A Azure felhőalapú szolgáltatás neve, amely alatt a virtuális gép létrehozásának.</span><span class="sxs-lookup"><span data-stu-id="1372e-163">The Azure cloud service name under which the VM was created.</span></span> |
    | <span data-ttu-id="1372e-164">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="1372e-164">ResourceGroupName</span></span>|<span data-ttu-id="1372e-165">A Azure erőforráscsoport-név alapján, amely a virtuális gép létrejött.</span><span class="sxs-lookup"><span data-stu-id="1372e-165">The Azure resource group name under which the VM was created.</span></span> |
    | <span data-ttu-id="1372e-166">RecoveryPointId</span><span class="sxs-lookup"><span data-stu-id="1372e-166">RecoveryPointId</span></span>|<span data-ttu-id="1372e-167">Ha a virtuális Gépet helyre lett állítva a időbélyegzőjét.</span><span class="sxs-lookup"><span data-stu-id="1372e-167">The timestamp for when the VM is recovered.</span></span> |

* <span data-ttu-id="1372e-168">Győződjön meg arról, hogy az Automation-fiók rendelkezik-e a következő modult:</span><span class="sxs-lookup"><span data-stu-id="1372e-168">Ensure that the Automation account has the following modules:</span></span>
    * <span data-ttu-id="1372e-169">AzureRM.profile</span><span class="sxs-lookup"><span data-stu-id="1372e-169">AzureRM.profile</span></span>
    * <span data-ttu-id="1372e-170">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="1372e-170">AzureRM.Resources</span></span>
    * <span data-ttu-id="1372e-171">AzureRM.Automation</span><span class="sxs-lookup"><span data-stu-id="1372e-171">AzureRM.Automation</span></span>
    * <span data-ttu-id="1372e-172">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="1372e-172">AzureRM.Network</span></span>
    * <span data-ttu-id="1372e-173">AzureRM.Compute</span><span class="sxs-lookup"><span data-stu-id="1372e-173">AzureRM.Compute</span></span>

<span data-ttu-id="1372e-174">Minden modul kompatibilis verziók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1372e-174">All modules should be of compatible versions.</span></span> <span data-ttu-id="1372e-175">Győződjön meg arról, hogy kompatibilisek-e a minden modul segítségével egyszerűen, hogy a modulok legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="1372e-175">An easy way to ensure that all modules are compatible is to use the latest versions of all the modules.</span></span>

### <a name="access-all-vms-of-the-vmmap-in-a-loop"></a><span data-ttu-id="1372e-176">Az ismétlődő VMMap összes gépek elérésének</span><span class="sxs-lookup"><span data-stu-id="1372e-176">Access all VMs of the VMMap in a loop</span></span>
<span data-ttu-id="1372e-177">Az alábbi kód használatával között a Microsoft VMMap minden virtuális gépeinek hurok:</span><span class="sxs-lookup"><span data-stu-id="1372e-177">Use the following code to loop across all VMs of the Microsoft VMMap:</span></span>

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is to ensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> <span data-ttu-id="1372e-178">Az erőforrás csoport nevét és a szerepkör neve értékek nincsenek megadva, ha a parancsfájl egy-egy rendszerindítási csoport tagjai előtti műveletet.</span><span class="sxs-lookup"><span data-stu-id="1372e-178">The resource group name and role name values are empty when the script is a pre-action to a boot group.</span></span> <span data-ttu-id="1372e-179">Az értékeket a rendszer feltölti, csak akkor, ha az adott csoport a virtuális gép feladatátvételi sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="1372e-179">The values are populated only if the VM of that group succeeds in failover.</span></span> <span data-ttu-id="1372e-180">A parancsfájl a rendszerindítási csoport tagjai a követő művelet.</span><span class="sxs-lookup"><span data-stu-id="1372e-180">The script is a post-action of the boot group.</span></span>

## <a name="use-the-same-automation-runbook-in-multiple-recovery-plans"></a><span data-ttu-id="1372e-181">Használja az ugyanazon Automation-forgatókönyv több helyreállítási tervek</span><span class="sxs-lookup"><span data-stu-id="1372e-181">Use the same Automation runbook in multiple recovery plans</span></span>

<span data-ttu-id="1372e-182">A több helyreállítási tervek egy parancsfájl külső változók segítségével is használhatók.</span><span class="sxs-lookup"><span data-stu-id="1372e-182">You can use a single script in multiple recovery plans by using external variables.</span></span> <span data-ttu-id="1372e-183">Használhat [Azure Automation-változók](../automation/automation-variables.md) paramétereket, amelyek átadhatók egy helyreállítási terv végrehajtásra tárolásához.</span><span class="sxs-lookup"><span data-stu-id="1372e-183">You can use [Azure Automation variables](../automation/automation-variables.md) to store parameters that you can pass for a recovery plan execution.</span></span> <span data-ttu-id="1372e-184">A helyreállítási terv nevének előtagjaként hozzáadása a változót, az egyes változók minden helyreállítási terv hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="1372e-184">By adding the recovery plan name as a prefix to the variable, you can create individual variables for each recovery plan.</span></span> <span data-ttu-id="1372e-185">Ezután használja a változók paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="1372e-185">Then, use the variables as parameters.</span></span> <span data-ttu-id="1372e-186">Egy paraméter módosíthatja a parancsfájl módosítása nélkül, de ilyenkor is megváltoztathatják a parancsfájl működésére.</span><span class="sxs-lookup"><span data-stu-id="1372e-186">You can change a parameter without changing the script, but still change the way the script works.</span></span>

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a><span data-ttu-id="1372e-187">Használhat egy egyszerű karakterlánc változót egy runbook parancsfájl</span><span class="sxs-lookup"><span data-stu-id="1372e-187">Use a simple string variable in a runbook script</span></span>

<span data-ttu-id="1372e-188">Ebben a példában egy parancsfájlt a a hálózati biztonsági csoport (NSG) bemenetből fogad adatokat, és alkalmazza azt a virtuális gépeket a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="1372e-188">In this example, a script takes the input of a Network Security Group (NSG) and applies it to the VMs of a recovery plan.</span></span>

<span data-ttu-id="1372e-189">A parancsfájl észleli a helyreállítási terv fut, a helyreállítási terv környezet használata:</span><span class="sxs-lookup"><span data-stu-id="1372e-189">For the script to detect which recovery plan is running, use the recovery plan context:</span></span>

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

<span data-ttu-id="1372e-190">Egy meglévő NSG alkalmazni, ismernie kell az NSG neve és az NSG erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="1372e-190">To apply an existing NSG, you must know the NSG name and the NSG resource group name.</span></span> <span data-ttu-id="1372e-191">Ezek a változók használják bemeneti adatokat a helyreállítási terv parancsprogramokat.</span><span class="sxs-lookup"><span data-stu-id="1372e-191">Use these variables as inputs for recovery plan scripts.</span></span> <span data-ttu-id="1372e-192">Ehhez hozzon létre két változót az Automation-fiók eszközök.</span><span class="sxs-lookup"><span data-stu-id="1372e-192">To do this, create two variables in the Automation account assets.</span></span> <span data-ttu-id="1372e-193">Adja hozzá a nevét, a helyreállítási terv létrehozása a paramétereinek előtagjaként való a változó nevét.</span><span class="sxs-lookup"><span data-stu-id="1372e-193">Add the name of the recovery plan that you are creating the parameters for as a prefix to the variable name.</span></span>

1. <span data-ttu-id="1372e-194">Hozzon létre egy változót, az NSG neve tárolásához.</span><span class="sxs-lookup"><span data-stu-id="1372e-194">Create a variable to store the NSG name.</span></span> <span data-ttu-id="1372e-195">Előtag a helyreállítási terv neve használatával adja hozzá a változó nevét.</span><span class="sxs-lookup"><span data-stu-id="1372e-195">Add a prefix to the variable name by using the name of the recovery plan.</span></span>

    ![Az NSG neve változó létrehozása](media/site-recovery-runbook-automation-new/var1.png)

2. <span data-ttu-id="1372e-197">Hozzon létre egy változó tárolásához az NSG az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="1372e-197">Create a variable to store the NSG's resource group name.</span></span> <span data-ttu-id="1372e-198">Előtag a helyreállítási terv neve használatával adja hozzá a változó nevét.</span><span class="sxs-lookup"><span data-stu-id="1372e-198">Add a prefix to the variable name by using the name of the recovery plan.</span></span>

    ![Hozzon létre egy NSG-t az erőforráscsoport neve](media/site-recovery-runbook-automation-new/var2.png)


3.  <span data-ttu-id="1372e-200">A parancsfájl a következő hivatkozás kódot használja a változó segítségével:</span><span class="sxs-lookup"><span data-stu-id="1372e-200">In the script, use the following reference code to get the variable values:</span></span>

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  <span data-ttu-id="1372e-201">A runbookban található változók segítségével az NSG-t a hálózati illesztő a virtuális gép feladatátadása vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="1372e-201">Use the variables in the runbook to apply the NSG to the network interface of the failed-over VM:</span></span>

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply the NSG to a network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

<span data-ttu-id="1372e-202">Minden helyreállítási terv létrehozása független változók, hogy a parancsfájl is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="1372e-202">For each recovery plan, create independent variables so that you can reuse the script.</span></span> <span data-ttu-id="1372e-203">Egy előtagot a helyreállítási terv nevének megadásával.</span><span class="sxs-lookup"><span data-stu-id="1372e-203">Add a prefix by using the recovery plan name.</span></span> <span data-ttu-id="1372e-204">Ebben a forgatókönyvben egy teljes, végpontok közötti parancsfájlt talál [egy nyilvános IP- és NSG-t a virtuális gépek hozzáadása a Site Recovery helyreállítási terv teszt feladatátvétele során](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span><span class="sxs-lookup"><span data-stu-id="1372e-204">For a complete, end-to-end script for this scenario, see [Add a public IP and NSG to VMs during test failover of a Site Recovery recovery plan](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span></span>


### <a name="use-a-complex-variable-to-store-more-information"></a><span data-ttu-id="1372e-205">További információk tárolására komplex változók használata</span><span class="sxs-lookup"><span data-stu-id="1372e-205">Use a complex variable to store more information</span></span>

<span data-ttu-id="1372e-206">Vegye figyelembe a nyilvános IP-cím, az adott virtuális gépeken futó bekapcsolása egyetlen parancsfájllal használni kívánt forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="1372e-206">Consider a scenario in which you want a single script to turn on a public IP on specific VMs.</span></span> <span data-ttu-id="1372e-207">Más esetben érdemes alkalmazni a különböző NSG-k (nem az összes virtuális gépeken futó) különböző virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="1372e-207">In another scenario, you might want to apply different NSGs on different VMs (not on all VMs).</span></span> <span data-ttu-id="1372e-208">Egy parancsfájl, amely a helyreállítási terv újrafelhasználható végezheti el.</span><span class="sxs-lookup"><span data-stu-id="1372e-208">You can make a script that is reusable for any recovery plan.</span></span> <span data-ttu-id="1372e-209">Minden helyreállítási terv olyan virtuális gépek száma lehet.</span><span class="sxs-lookup"><span data-stu-id="1372e-209">Each recovery plan can have a variable number of VMs.</span></span> <span data-ttu-id="1372e-210">Például egy SharePoint helyreállítási két elülső végpontja van.</span><span class="sxs-lookup"><span data-stu-id="1372e-210">For example, a SharePoint recovery has two front ends.</span></span> <span data-ttu-id="1372e-211">Egy alapszintű az üzletági (LOB) alkalmazás csak egy előtér rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1372e-211">A basic line-of-business (LOB) application has only one front end.</span></span> <span data-ttu-id="1372e-212">Külön változók minden helyreállítási terv nem hozható létre.</span><span class="sxs-lookup"><span data-stu-id="1372e-212">You cannot create separate variables for each recovery plan.</span></span> 

<span data-ttu-id="1372e-213">A következő példában azt egy új módszerrel, és hozzon létre egy [komplex változó](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) az Azure Automation-fiók eszközök.</span><span class="sxs-lookup"><span data-stu-id="1372e-213">In the following example, we use a new technique and create a [complex variable](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) in the Azure Automation account assets.</span></span> <span data-ttu-id="1372e-214">Ehhez több érték megadásával.</span><span class="sxs-lookup"><span data-stu-id="1372e-214">You do this by specifying multiple values.</span></span> <span data-ttu-id="1372e-215">Azure PowerShell használatával kell végeznie az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1372e-215">You must use Azure PowerShell to complete the following steps:</span></span>

1. <span data-ttu-id="1372e-216">A PowerShellben jelentkezzen be az Azure-előfizetésbe:</span><span class="sxs-lookup"><span data-stu-id="1372e-216">In PowerShell, sign in to your Azure subscription:</span></span>

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. <span data-ttu-id="1372e-217">A paraméterek vannak tárolva, a helyreállítási terv neve segítségével hozzon létre a komplex változó:</span><span class="sxs-lookup"><span data-stu-id="1372e-217">To store the parameters, create the complex variable by using the name of the recovery plan:</span></span>

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. <span data-ttu-id="1372e-218">A komplex változóban **VMDetails** rendszer a virtuális gép Azonosítóját a védett virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="1372e-218">In this complex variable, **VMDetails** is the VM ID for the protected VM.</span></span> <span data-ttu-id="1372e-219">Ahhoz, hogy a virtuális gép azonosítója az Azure portálon, a virtuális gép tulajdonságainak megtekintése.</span><span class="sxs-lookup"><span data-stu-id="1372e-219">To get the VM ID, in the Azure portal, view the VM properties.</span></span> <span data-ttu-id="1372e-220">Az alábbi képernyőfelvételen látható egy változó, amely két virtuális gépek adatait tárolja:</span><span class="sxs-lookup"><span data-stu-id="1372e-220">The following screenshot shows a variable that stores the details of two VMs:</span></span>

    ![A globálisan egyedi Azonosítót használja a virtuális gép azonosítója](media/site-recovery-runbook-automation-new/vmguid.png)

4. <span data-ttu-id="1372e-222">Ez a változó használata a runbookokban.</span><span class="sxs-lookup"><span data-stu-id="1372e-222">Use this variable in your runbook.</span></span> <span data-ttu-id="1372e-223">Ha a megadott virtuális gép GUID Azonosítóját a helyreállítási terv a környezetben található, alkalmazza az NSG-t a virtuális Gépen:</span><span class="sxs-lookup"><span data-stu-id="1372e-223">If the indicated VM GUID is found in the recovery plan context, apply the NSG on the VM:</span></span>

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. <span data-ttu-id="1372e-224">A runbookban a virtuális gépeket a helyreállítási terv környezet ismétlése.</span><span class="sxs-lookup"><span data-stu-id="1372e-224">In your runbook, loop through the VMs of the recovery plan context.</span></span> <span data-ttu-id="1372e-225">Ellenőrizze, hogy a virtuális gép létezik **$VMDetailsObj**.</span><span class="sxs-lookup"><span data-stu-id="1372e-225">Check whether the VM exists in **$VMDetailsObj**.</span></span> <span data-ttu-id="1372e-226">Ha létezik, a változót, alkalmazza az NSG tulajdonságai érhetők el:</span><span class="sxs-lookup"><span data-stu-id="1372e-226">If it exists, access the properties of the variable to apply the NSG:</span></span>

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If the VM exists in the context, this will not b Null
                $VM = $vmMap.$VMID
                # Access the properties of the variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code to apply the NSG properties to the VM
            }
        }
    ```

<span data-ttu-id="1372e-227">A különböző helyreállítási tervek használhatja ugyanazt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="1372e-227">You can use the same script for different recovery plans.</span></span> <span data-ttu-id="1372e-228">Adjon meg más paraméterekkel tárolja, amely megfelel a helyreállítási terv különböző változók értékét.</span><span class="sxs-lookup"><span data-stu-id="1372e-228">Enter different parameters by storing the value that corresponds to a recovery plan in different variables.</span></span>

## <a name="sample-scripts"></a><span data-ttu-id="1372e-229">Mintaszkriptek</span><span class="sxs-lookup"><span data-stu-id="1372e-229">Sample scripts</span></span>

<span data-ttu-id="1372e-230">Az Automation-fiók mintaparancsfájlok telepíteni, kattintson a **az Azure telepítéséhez** gombra.</span><span class="sxs-lookup"><span data-stu-id="1372e-230">To deploy sample scripts to your Automation account, click the **Deploy to Azure** button.</span></span>

<span data-ttu-id="1372e-231">[![Üzembe helyezés az Azure-ban](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span><span class="sxs-lookup"><span data-stu-id="1372e-231">[![Deploy to Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span></span>

<span data-ttu-id="1372e-232">Egy másik példa tekintse meg a következő videó.</span><span class="sxs-lookup"><span data-stu-id="1372e-232">For another example, see the following video.</span></span> <span data-ttu-id="1372e-233">Azt mutatja be helyreállítani egy kétrétegű WordPress alkalmazásból az Azure-bA:</span><span class="sxs-lookup"><span data-stu-id="1372e-233">It demonstrates how to recover a two-tier WordPress application to Azure:</span></span>


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a><span data-ttu-id="1372e-234">További források</span><span class="sxs-lookup"><span data-stu-id="1372e-234">Additional resources</span></span>
* [<span data-ttu-id="1372e-235">Azure Automation szolgáltatást futtató fiók</span><span class="sxs-lookup"><span data-stu-id="1372e-235">Azure Automation service Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)
* [<span data-ttu-id="1372e-236">Azure Automation áttekintése</span><span class="sxs-lookup"><span data-stu-id="1372e-236">Azure Automation overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation – áttekintés")
* [<span data-ttu-id="1372e-237">Azure Automation-mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="1372e-237">Azure Automation sample scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure Automation-mintaparancsfájlok")
