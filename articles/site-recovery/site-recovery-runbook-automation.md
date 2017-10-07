---
title: aaaAdd Azure Automation runbookjai toorecovery tervek az Azure Site Recovery |} Microsoft Docs
description: "Ismerje meg, hogyan Azure Site Recovery segítségével a helyreállítási terv kiterjesztése az Azure Automation használatával. Ismerje meg, hogyan toocomplete összetett feladatok, helyreállítási tooAzure során."
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
ms.openlocfilehash: 90d517200cec5527e98a0d00da466bace587b70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans"></a><span data-ttu-id="165eb-104">Azure Automation runbookjai toorecovery csomagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="165eb-104">Add Azure Automation runbooks toorecovery plans</span></span>
<span data-ttu-id="165eb-105">Ez a cikk azt ismertetik, hogyan integrálható az Azure Site Recovery Azure Automation toohelp kiterjeszti a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="165eb-105">In this article, we describe how Azure Site Recovery integrates with Azure Automation toohelp you extend your recovery plans.</span></span> <span data-ttu-id="165eb-106">A helyreállítási terv lehet levezényelni a Site Recovery védett virtuális gépek helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="165eb-106">Recovery plans can orchestrate recovery of VMs that are protected with Site Recovery.</span></span> <span data-ttu-id="165eb-107">A helyreállítási terv replikációs tooa másodlagos felhő, valamint az replikációs tooAzure működik.</span><span class="sxs-lookup"><span data-stu-id="165eb-107">Recovery plans work both for replication tooa secondary cloud, and for replication tooAzure.</span></span> <span data-ttu-id="165eb-108">A helyreállítási terv is segít hello helyreállítási **következetesen pontos**, **ismételhető**, és **automatizált**.</span><span class="sxs-lookup"><span data-stu-id="165eb-108">Recovery plans also help make hello recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="165eb-109">Ha a rendszer átadja a virtuális gépek tooAzure, integráció az Azure Automation szolgáltatásban, a helyreállítási terv terjeszti ki.</span><span class="sxs-lookup"><span data-stu-id="165eb-109">If you fail over your VMs tooAzure, integration with Azure Automation extends your recovery plans.</span></span> <span data-ttu-id="165eb-110">Használat tooexecute runbookok, amelyek hatékony automatizálási feladatai kiindulópontjaként.</span><span class="sxs-lookup"><span data-stu-id="165eb-110">You can use it tooexecute runbooks, which offer powerful automation tasks.</span></span>

<span data-ttu-id="165eb-111">Ha új tooAzure Automation, akkor [regisztráljon](https://azure.microsoft.com/services/automation/) és [mintaparancsfájlok letöltése](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="165eb-111">If you are new tooAzure Automation, you can [sign up](https://azure.microsoft.com/services/automation/) and [download sample scripts](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="165eb-112">További információk és toolearn hogyan tooorchestrate helyreállítási tooAzure használatával [helyreállítási tervek](https://azure.microsoft.com/blog/?p=166264), lásd: [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="165eb-112">For more information, and toolearn how tooorchestrate recovery tooAzure by using [recovery plans](https://azure.microsoft.com/blog/?p=166264), see [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span></span>

<span data-ttu-id="165eb-113">Ez a cikk azt ismerteti hogyan integrálható az Azure Automation-forgatókönyv be a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="165eb-113">In this article, we describe how you can integrate Azure Automation runbooks into your recovery plans.</span></span> <span data-ttu-id="165eb-114">Példák tooautomate alapvető műveleteket, amelyek korábban a kézi beavatkozás szükséges használjuk.</span><span class="sxs-lookup"><span data-stu-id="165eb-114">We use examples tooautomate basic tasks that previously required manual intervention.</span></span> <span data-ttu-id="165eb-115">Azt is leírják, hogyan tooconvert egy többlépéses helyreállítási tooa kattintással helyreállítási művelet.</span><span class="sxs-lookup"><span data-stu-id="165eb-115">We also describe how tooconvert a multi-step recovery tooa single-click recovery action.</span></span>

## <a name="customize-hello-recovery-plan"></a><span data-ttu-id="165eb-116">Hello helyreállítási terv testreszabása</span><span class="sxs-lookup"><span data-stu-id="165eb-116">Customize hello recovery plan</span></span>
1. <span data-ttu-id="165eb-117">Nyissa meg toohello **Site Recovery** helyreállítási terv erőforráspanelen.</span><span class="sxs-lookup"><span data-stu-id="165eb-117">Go toohello **Site Recovery** recovery plan resource blade.</span></span> <span data-ttu-id="165eb-118">Az ebben a példában a hello helyreállítási tervben szereplő két virtuális gépek hozzáadott tooit, a helyreállításhoz.</span><span class="sxs-lookup"><span data-stu-id="165eb-118">For this example, hello recovery plan has two VMs added tooit, for recovery.</span></span> <span data-ttu-id="165eb-119">egy runbook hozzáadása toobegin kattintson hello **Testreszabás** fülre.</span><span class="sxs-lookup"><span data-stu-id="165eb-119">toobegin adding a runbook, click hello **Customize** tab.</span></span>

    ![Hello Testreszabás gombra.](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. <span data-ttu-id="165eb-121">Kattintson a jobb gombbal **csoport 1: Start**, majd válassza ki **post művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="165eb-121">Right-click **Group 1: Start**, and then select **Add post action**.</span></span>

    ![Kattintson a jobb gombbal csoport 1: Start, és a post művelet hozzáadása](media/site-recovery-runbook-automation-new/customize-rp.png)

3. <span data-ttu-id="165eb-123">Kattintson a **válasszon egy parancsfájlt**.</span><span class="sxs-lookup"><span data-stu-id="165eb-123">Click **Choose a script**.</span></span>

4. <span data-ttu-id="165eb-124">A hello **műveletének frissítése** panelen, hello parancsfájlt **Hello World**.</span><span class="sxs-lookup"><span data-stu-id="165eb-124">On hello **Update action** blade, name hello script **Hello World**.</span></span>

    ![hello frissítési művelet panel](media/site-recovery-runbook-automation-new/update-rp.png)

5. <span data-ttu-id="165eb-126">Adja meg az Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="165eb-126">Enter an Automation account name.</span></span>
    >[!NOTE]
    > <span data-ttu-id="165eb-127">Automation-fiók hello bármely Azure régióban lehet.</span><span class="sxs-lookup"><span data-stu-id="165eb-127">hello Automation account can be in any Azure region.</span></span> <span data-ttu-id="165eb-128">Automation-fiók hello hello kell lennie és hello Azure Site Recovery-tárolónak ugyanabban az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="165eb-128">hello Automation account must be in hello same subscription as hello Azure Site Recovery vault.</span></span>

6. <span data-ttu-id="165eb-129">Az Automation-fiók válasszon egy runbookot.</span><span class="sxs-lookup"><span data-stu-id="165eb-129">In your Automation account, select a runbook.</span></span> <span data-ttu-id="165eb-130">Ez a runbook végrehajtásakor hello hello helyreállítási terv, hello első csoport hello helyreállítás után futó hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="165eb-130">This runbook is hello script that runs during hello execution of hello recovery plan, after hello recovery of hello first group.</span></span>

7. <span data-ttu-id="165eb-131">toosave hello parancsfájl, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="165eb-131">toosave hello script, click **OK**.</span></span> <span data-ttu-id="165eb-132">hello parancsfájl túl kerül**csoport 1: utáni lépéseket**.</span><span class="sxs-lookup"><span data-stu-id="165eb-132">hello script is added too**Group 1: Post-steps**.</span></span>

    ![Követő műveletet csoport 1:Start](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a><span data-ttu-id="165eb-134">A parancsfájl hozzáadása szempontjai</span><span class="sxs-lookup"><span data-stu-id="165eb-134">Considerations for adding a script</span></span>

* <span data-ttu-id="165eb-135">A beállítások megtekintéséhez túl**egy lépés törlése** vagy **hello parancsfájl frissítése**, kattintson a jobb gombbal a hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="165eb-135">For options too**delete a step** or **update hello script**, right-click hello script.</span></span>
* <span data-ttu-id="165eb-136">A parancsfájl futtatásához Azure feladatátvétel során a egy a helyszíni gépen tooAzure.</span><span class="sxs-lookup"><span data-stu-id="165eb-136">A script can run on Azure during failover from an on-premises machine tooAzure.</span></span> <span data-ttu-id="165eb-137">Is futtatható Azure leállítás, mielőtt elsődleges webhelyeken parancsfájlként Azure tooan a helyi számítógépen, a feladat-visszavétel során.</span><span class="sxs-lookup"><span data-stu-id="165eb-137">It also can run on Azure as a primary-site script before shutdown, during failback from Azure tooan on-premises machine.</span></span>
* <span data-ttu-id="165eb-138">Parancsfájl futtatása, a helyreállítási terv környezet esetében.</span><span class="sxs-lookup"><span data-stu-id="165eb-138">When a script runs, it injects a recovery plan context.</span></span> <span data-ttu-id="165eb-139">hello a következő példa bemutatja, egy környezeti változó:</span><span class="sxs-lookup"><span data-stu-id="165eb-139">hello following example shows a context variable:</span></span>

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

    <span data-ttu-id="165eb-140">hello a következő táblázat felsorolja a hello nevét és leírását mindegyik változónak hello a környezetben.</span><span class="sxs-lookup"><span data-stu-id="165eb-140">hello following table lists hello name and description of each variable in hello context.</span></span>

    | <span data-ttu-id="165eb-141">**Változó neve**</span><span class="sxs-lookup"><span data-stu-id="165eb-141">**Variable name**</span></span> | <span data-ttu-id="165eb-142">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="165eb-142">**Description**</span></span> |
    | --- | --- |
    | <span data-ttu-id="165eb-143">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="165eb-143">RecoveryPlanName</span></span> |<span data-ttu-id="165eb-144">hello neve hello terv futtatása.</span><span class="sxs-lookup"><span data-stu-id="165eb-144">hello name of hello plan being run.</span></span> <span data-ttu-id="165eb-145">Ez a változó segítségével különböző műveletek hello helyreállítási terv neve alapján.</span><span class="sxs-lookup"><span data-stu-id="165eb-145">This variable helps you take different actions based on hello recovery plan name.</span></span> <span data-ttu-id="165eb-146">Hello parancsfájlt is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="165eb-146">You also can reuse hello script.</span></span> |
    | <span data-ttu-id="165eb-147">FailoverType</span><span class="sxs-lookup"><span data-stu-id="165eb-147">FailoverType</span></span> |<span data-ttu-id="165eb-148">Megadja, hogy hello feladatátvételi teszt, tervezett vagy nem tervezett.</span><span class="sxs-lookup"><span data-stu-id="165eb-148">Specifies whether hello failover is a test, planned, or unplanned.</span></span> |
    | <span data-ttu-id="165eb-149">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="165eb-149">FailoverDirection</span></span> |<span data-ttu-id="165eb-150">Megadja, hogy helyreállítási tooa elsődleges vagy másodlagos helyhez.</span><span class="sxs-lookup"><span data-stu-id="165eb-150">Specifies whether recovery is tooa primary or secondary site.</span></span> |
    | <span data-ttu-id="165eb-151">Csoportazonosító</span><span class="sxs-lookup"><span data-stu-id="165eb-151">GroupID</span></span> |<span data-ttu-id="165eb-152">Hello csoportazonosító azonosítja hello helyreállítási terv hello terv fut-e.</span><span class="sxs-lookup"><span data-stu-id="165eb-152">Identifies hello group number in hello recovery plan when hello plan is running.</span></span> |
    | <span data-ttu-id="165eb-153">VmMap</span><span class="sxs-lookup"><span data-stu-id="165eb-153">VmMap</span></span> |<span data-ttu-id="165eb-154">Hello csoportban lévő összes virtuális gép tömbjét.</span><span class="sxs-lookup"><span data-stu-id="165eb-154">An array of all VMs in hello group.</span></span> |
    | <span data-ttu-id="165eb-155">VMMap kulcs</span><span class="sxs-lookup"><span data-stu-id="165eb-155">VMMap key</span></span> |<span data-ttu-id="165eb-156">Egyedi kulcs (GUID) az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="165eb-156">A unique key (GUID) for each VM.</span></span> <span data-ttu-id="165eb-157">Fennállt – azonos Azure Virtual Machine Manager (VMM) azonosítója hello hello hello a virtuális gép, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="165eb-157">It's hello same as hello Azure Virtual Machine Manager (VMM) ID of hello VM, where applicable.</span></span> |
    | <span data-ttu-id="165eb-158">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="165eb-158">SubscriptionId</span></span> |<span data-ttu-id="165eb-159">hello Azure-előfizetése Azonosítóját, amelyben a virtuális gép hello hozták létre.</span><span class="sxs-lookup"><span data-stu-id="165eb-159">hello Azure subscription ID in which hello VM was created.</span></span> |
    | <span data-ttu-id="165eb-160">RoleName</span><span class="sxs-lookup"><span data-stu-id="165eb-160">RoleName</span></span> |<span data-ttu-id="165eb-161">hello Azure virtuális Gépen, amelyik a helyreállítandó hello neve.</span><span class="sxs-lookup"><span data-stu-id="165eb-161">hello name of hello Azure VM that's being recovered.</span></span> |
    | <span data-ttu-id="165eb-162">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="165eb-162">CloudServiceName</span></span> |<span data-ttu-id="165eb-163">hello Azure felhőalapú szolgáltatás neve alapján, amely a virtuális gép hello hozták létre.</span><span class="sxs-lookup"><span data-stu-id="165eb-163">hello Azure cloud service name under which hello VM was created.</span></span> |
    | <span data-ttu-id="165eb-164">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="165eb-164">ResourceGroupName</span></span>|<span data-ttu-id="165eb-165">hello Azure erőforráscsoport-név alapján mely hello VM lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="165eb-165">hello Azure resource group name under which hello VM was created.</span></span> |
    | <span data-ttu-id="165eb-166">RecoveryPointId</span><span class="sxs-lookup"><span data-stu-id="165eb-166">RecoveryPointId</span></span>|<span data-ttu-id="165eb-167">Ha a virtuális gép hello helyre lett állítva hello időbélyegzőjét.</span><span class="sxs-lookup"><span data-stu-id="165eb-167">hello timestamp for when hello VM is recovered.</span></span> |

* <span data-ttu-id="165eb-168">Győződjön meg arról, hogy hello Automation-fiók rendelkezik a következő modulok hello:</span><span class="sxs-lookup"><span data-stu-id="165eb-168">Ensure that hello Automation account has hello following modules:</span></span>
    * <span data-ttu-id="165eb-169">AzureRM.profile</span><span class="sxs-lookup"><span data-stu-id="165eb-169">AzureRM.profile</span></span>
    * <span data-ttu-id="165eb-170">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="165eb-170">AzureRM.Resources</span></span>
    * <span data-ttu-id="165eb-171">AzureRM.Automation</span><span class="sxs-lookup"><span data-stu-id="165eb-171">AzureRM.Automation</span></span>
    * <span data-ttu-id="165eb-172">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="165eb-172">AzureRM.Network</span></span>
    * <span data-ttu-id="165eb-173">AzureRM.Compute</span><span class="sxs-lookup"><span data-stu-id="165eb-173">AzureRM.Compute</span></span>

<span data-ttu-id="165eb-174">Minden modul kompatibilis verziók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="165eb-174">All modules should be of compatible versions.</span></span> <span data-ttu-id="165eb-175">Egy egyszerű módot tooensure kompatibilis összes modulokra toouse hello a legfrissebb verziója minden hello modulok.</span><span class="sxs-lookup"><span data-stu-id="165eb-175">An easy way tooensure that all modules are compatible is toouse hello latest versions of all hello modules.</span></span>

### <a name="access-all-vms-of-hello-vmmap-in-a-loop"></a><span data-ttu-id="165eb-176">Minden virtuális gépeinek hello VMMap ismétlődő eléréséhez</span><span class="sxs-lookup"><span data-stu-id="165eb-176">Access all VMs of hello VMMap in a loop</span></span>
<span data-ttu-id="165eb-177">Kód tooloop hello Microsoft VMMap az összes virtuális gépek között a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="165eb-177">Use hello following code tooloop across all VMs of hello Microsoft VMMap:</span></span>

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is tooensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> <span data-ttu-id="165eb-178">hello erőforrás csoport nevét és a szerepkör neve értékek nincsenek megadva, ha hello parancsfájl egy előtti művelet tooa rendszerindítási csoport.</span><span class="sxs-lookup"><span data-stu-id="165eb-178">hello resource group name and role name values are empty when hello script is a pre-action tooa boot group.</span></span> <span data-ttu-id="165eb-179">hello értékek a rendszer feltölti, csak akkor, ha hello az adott csoport a virtuális gép feladatátvételi sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="165eb-179">hello values are populated only if hello VM of that group succeeds in failover.</span></span> <span data-ttu-id="165eb-180">hello parancsfájl hello rendszerindítási csoport tagjai a követő művelet.</span><span class="sxs-lookup"><span data-stu-id="165eb-180">hello script is a post-action of hello boot group.</span></span>

## <a name="use-hello-same-automation-runbook-in-multiple-recovery-plans"></a><span data-ttu-id="165eb-181">Használja az azonos hello több helyreállítási tervek az Automation-forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="165eb-181">Use hello same Automation runbook in multiple recovery plans</span></span>

<span data-ttu-id="165eb-182">A több helyreállítási tervek egy parancsfájl külső változók segítségével is használhatók.</span><span class="sxs-lookup"><span data-stu-id="165eb-182">You can use a single script in multiple recovery plans by using external variables.</span></span> <span data-ttu-id="165eb-183">Használhat [Azure Automation-változók](../automation/automation-variables.md) toostore paramétereket, amelyek átadhatók egy helyreállítási terv végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="165eb-183">You can use [Azure Automation variables](../automation/automation-variables.md) toostore parameters that you can pass for a recovery plan execution.</span></span> <span data-ttu-id="165eb-184">Adja hozzá a hello helyreállítási terv neve előtag toohello változóként, minden helyreállítási terv egyes változók is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="165eb-184">By adding hello recovery plan name as a prefix toohello variable, you can create individual variables for each recovery plan.</span></span> <span data-ttu-id="165eb-185">Ezt követően változókkal hello paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="165eb-185">Then, use hello variables as parameters.</span></span> <span data-ttu-id="165eb-186">Módosíthatja egy paraméter hello parancsfájl módosítása nélkül, de továbbra is módosítás hello módon hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="165eb-186">You can change a parameter without changing hello script, but still change hello way hello script works.</span></span>

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a><span data-ttu-id="165eb-187">Használhat egy egyszerű karakterlánc változót egy runbook parancsfájl</span><span class="sxs-lookup"><span data-stu-id="165eb-187">Use a simple string variable in a runbook script</span></span>

<span data-ttu-id="165eb-188">Ebben a példában egy parancsfájlt a hálózati biztonsági csoport (NSG) hello bemenetből fogad adatokat, és alkalmazza azt toohello virtuális gépeket a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="165eb-188">In this example, a script takes hello input of a Network Security Group (NSG) and applies it toohello VMs of a recovery plan.</span></span>

<span data-ttu-id="165eb-189">Hello parancsfájl toodetect melyik helyreállítási tervhez fut, a hello helyreállítási terv környezet használata:</span><span class="sxs-lookup"><span data-stu-id="165eb-189">For hello script toodetect which recovery plan is running, use hello recovery plan context:</span></span>

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

<span data-ttu-id="165eb-190">egy meglévő NSG tooapply, ismernie kell hello NSG neve és hello NSG az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="165eb-190">tooapply an existing NSG, you must know hello NSG name and hello NSG resource group name.</span></span> <span data-ttu-id="165eb-191">Ezek a változók használják bemeneti adatokat a helyreállítási terv parancsprogramokat.</span><span class="sxs-lookup"><span data-stu-id="165eb-191">Use these variables as inputs for recovery plan scripts.</span></span> <span data-ttu-id="165eb-192">toodo, hozzon létre két változót hello az Automation-fiók eszközök.</span><span class="sxs-lookup"><span data-stu-id="165eb-192">toodo this, create two variables in hello Automation account assets.</span></span> <span data-ttu-id="165eb-193">Adja hozzá a hello nevét hello helyreállítási terv létrehozása hello paramétereinek előtag toohello változó neveként.</span><span class="sxs-lookup"><span data-stu-id="165eb-193">Add hello name of hello recovery plan that you are creating hello parameters for as a prefix toohello variable name.</span></span>

1. <span data-ttu-id="165eb-194">Hozzon létre egy változó toostore hello NSG neve.</span><span class="sxs-lookup"><span data-stu-id="165eb-194">Create a variable toostore hello NSG name.</span></span> <span data-ttu-id="165eb-195">Vegyen fel egy előtag toohello változónevet hello helyreállítási terv hello neve.</span><span class="sxs-lookup"><span data-stu-id="165eb-195">Add a prefix toohello variable name by using hello name of hello recovery plan.</span></span>

    ![Az NSG neve változó létrehozása](media/site-recovery-runbook-automation-new/var1.png)

2. <span data-ttu-id="165eb-197">Hozzon létre egy változó toostore hello NSG az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="165eb-197">Create a variable toostore hello NSG's resource group name.</span></span> <span data-ttu-id="165eb-198">Vegyen fel egy előtag toohello változónevet hello helyreállítási terv hello neve.</span><span class="sxs-lookup"><span data-stu-id="165eb-198">Add a prefix toohello variable name by using hello name of hello recovery plan.</span></span>

    ![Hozzon létre egy NSG-t az erőforráscsoport neve](media/site-recovery-runbook-automation-new/var2.png)


3.  <span data-ttu-id="165eb-200">Hello parancsfájlban használja a következő hivatkozás kód tooget hello változók értékeinek hello:</span><span class="sxs-lookup"><span data-stu-id="165eb-200">In hello script, use hello following reference code tooget hello variable values:</span></span>

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  <span data-ttu-id="165eb-201">Hello runbook tooapply hello NSG toohello hálózati adapterének hello használata hello változók átvevő VM:</span><span class="sxs-lookup"><span data-stu-id="165eb-201">Use hello variables in hello runbook tooapply hello NSG toohello network interface of hello failed-over VM:</span></span>

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply hello NSG tooa network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

<span data-ttu-id="165eb-202">Minden helyreállítási terv hozza létre a független változók hello parancsfájlt is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="165eb-202">For each recovery plan, create independent variables so that you can reuse hello script.</span></span> <span data-ttu-id="165eb-203">Egy előtagot hello helyreállítási terv nevének megadásával.</span><span class="sxs-lookup"><span data-stu-id="165eb-203">Add a prefix by using hello recovery plan name.</span></span> <span data-ttu-id="165eb-204">Ebben a forgatókönyvben egy teljes, végpontok közötti parancsfájlt talál [a Site Recovery helyreállítási terv teszt feladatátvétele során adja hozzá a nyilvános IP-cím és NSG tooVMs](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span><span class="sxs-lookup"><span data-stu-id="165eb-204">For a complete, end-to-end script for this scenario, see [Add a public IP and NSG tooVMs during test failover of a Site Recovery recovery plan](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span></span>


### <a name="use-a-complex-variable-toostore-more-information"></a><span data-ttu-id="165eb-205">Használja a komplex változó toostore további információk</span><span class="sxs-lookup"><span data-stu-id="165eb-205">Use a complex variable toostore more information</span></span>

<span data-ttu-id="165eb-206">Fontolja meg egy olyan forgatókönyvet, amelyben egy adott virtuális gépeken nyilvános IP-egyetlen parancsfájl tooturn szeretné.</span><span class="sxs-lookup"><span data-stu-id="165eb-206">Consider a scenario in which you want a single script tooturn on a public IP on specific VMs.</span></span> <span data-ttu-id="165eb-207">Más esetben érdemes lehet a tooapply különböző NSG-k (nem az összes virtuális gépeken futó) különböző gépeken.</span><span class="sxs-lookup"><span data-stu-id="165eb-207">In another scenario, you might want tooapply different NSGs on different VMs (not on all VMs).</span></span> <span data-ttu-id="165eb-208">Egy parancsfájl, amely a helyreállítási terv újrafelhasználható végezheti el.</span><span class="sxs-lookup"><span data-stu-id="165eb-208">You can make a script that is reusable for any recovery plan.</span></span> <span data-ttu-id="165eb-209">Minden helyreállítási terv olyan virtuális gépek száma lehet.</span><span class="sxs-lookup"><span data-stu-id="165eb-209">Each recovery plan can have a variable number of VMs.</span></span> <span data-ttu-id="165eb-210">Például egy SharePoint helyreállítási két elülső végpontja van.</span><span class="sxs-lookup"><span data-stu-id="165eb-210">For example, a SharePoint recovery has two front ends.</span></span> <span data-ttu-id="165eb-211">Egy alapszintű az üzletági (LOB) alkalmazás csak egy előtér rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="165eb-211">A basic line-of-business (LOB) application has only one front end.</span></span> <span data-ttu-id="165eb-212">Külön változók minden helyreállítási terv nem hozható létre.</span><span class="sxs-lookup"><span data-stu-id="165eb-212">You cannot create separate variables for each recovery plan.</span></span> 

<span data-ttu-id="165eb-213">A következő példa hello, hogy egy új módszerrel, és hozzon létre egy [komplex változó](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) hello Azure Automation-fiók eszközökbe.</span><span class="sxs-lookup"><span data-stu-id="165eb-213">In hello following example, we use a new technique and create a [complex variable](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) in hello Azure Automation account assets.</span></span> <span data-ttu-id="165eb-214">Ehhez több érték megadásával.</span><span class="sxs-lookup"><span data-stu-id="165eb-214">You do this by specifying multiple values.</span></span> <span data-ttu-id="165eb-215">A következő lépéseket az Azure PowerShell toocomplete hello kell használnia:</span><span class="sxs-lookup"><span data-stu-id="165eb-215">You must use Azure PowerShell toocomplete hello following steps:</span></span>

1. <span data-ttu-id="165eb-216">PowerShell, a bejelentkezés tooyour Azure-előfizetés:</span><span class="sxs-lookup"><span data-stu-id="165eb-216">In PowerShell, sign in tooyour Azure subscription:</span></span>

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. <span data-ttu-id="165eb-217">toostore hello paraméterek, hello komplex változó hello neve hello helyreállítási terv létrehozása:</span><span class="sxs-lookup"><span data-stu-id="165eb-217">toostore hello parameters, create hello complex variable by using hello name of hello recovery plan:</span></span>

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. <span data-ttu-id="165eb-218">A komplex változóban **VMDetails** hello Virtuálisgép-azonosító hello a virtuális gép védett.</span><span class="sxs-lookup"><span data-stu-id="165eb-218">In this complex variable, **VMDetails** is hello VM ID for hello protected VM.</span></span> <span data-ttu-id="165eb-219">tooget hello virtuális gép azonosítója az Azure-portálon hello hello virtuális gép tulajdonságainak megtekintése.</span><span class="sxs-lookup"><span data-stu-id="165eb-219">tooget hello VM ID, in hello Azure portal, view hello VM properties.</span></span> <span data-ttu-id="165eb-220">hello következő képernyőfelvétel egy változó, amely tárolja a két virtuális gépek hello részleteit:</span><span class="sxs-lookup"><span data-stu-id="165eb-220">hello following screenshot shows a variable that stores hello details of two VMs:</span></span>

    ![Használja a hello GUID hello virtuális gép azonosítója](media/site-recovery-runbook-automation-new/vmguid.png)

4. <span data-ttu-id="165eb-222">Ez a változó használata a runbookokban.</span><span class="sxs-lookup"><span data-stu-id="165eb-222">Use this variable in your runbook.</span></span> <span data-ttu-id="165eb-223">Hello feltüntetve VM GUID hello helyreállítási tervnek a környezetben található, ha telepíteni hello NSG hello VM:</span><span class="sxs-lookup"><span data-stu-id="165eb-223">If hello indicated VM GUID is found in hello recovery plan context, apply hello NSG on hello VM:</span></span>

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. <span data-ttu-id="165eb-224">A runbookban ismétlése hello virtuális gépeinek hello helyreállítási tervnek a környezetben.</span><span class="sxs-lookup"><span data-stu-id="165eb-224">In your runbook, loop through hello VMs of hello recovery plan context.</span></span> <span data-ttu-id="165eb-225">Ellenőrizze, hogy létezik-e virtuális gép hello az **$VMDetailsObj**.</span><span class="sxs-lookup"><span data-stu-id="165eb-225">Check whether hello VM exists in **$VMDetailsObj**.</span></span> <span data-ttu-id="165eb-226">Ha létezik, hello változó tooapply hello NSG hello tulajdonságai érhetők el:</span><span class="sxs-lookup"><span data-stu-id="165eb-226">If it exists, access hello properties of hello variable tooapply hello NSG:</span></span>

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If hello VM exists in hello context, this will not b Null
                $VM = $vmMap.$VMID
                # Access hello properties of hello variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code tooapply hello NSG properties toohello VM
            }
        }
    ```

<span data-ttu-id="165eb-227">Használhatja ugyanazt a parancsfájlt hello különböző helyreállítási tervek.</span><span class="sxs-lookup"><span data-stu-id="165eb-227">You can use hello same script for different recovery plans.</span></span> <span data-ttu-id="165eb-228">Adjon meg más paraméterekkel hello érték, amely megfelel a különböző változók tooa helyreállítási tervben tárolja.</span><span class="sxs-lookup"><span data-stu-id="165eb-228">Enter different parameters by storing hello value that corresponds tooa recovery plan in different variables.</span></span>

## <a name="sample-scripts"></a><span data-ttu-id="165eb-229">Mintaszkriptek</span><span class="sxs-lookup"><span data-stu-id="165eb-229">Sample scripts</span></span>

<span data-ttu-id="165eb-230">toodeploy minta parancsfájlok tooyour Automation-fiókot, kattintson a hello **tooAzure telepítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="165eb-230">toodeploy sample scripts tooyour Automation account, click hello **Deploy tooAzure** button.</span></span>

<span data-ttu-id="165eb-231">[![TooAzure telepítése](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span><span class="sxs-lookup"><span data-stu-id="165eb-231">[![Deploy tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span></span>

<span data-ttu-id="165eb-232">Egy másik példa tekintse meg a következő videó hello.</span><span class="sxs-lookup"><span data-stu-id="165eb-232">For another example, see hello following video.</span></span> <span data-ttu-id="165eb-233">Bemutatja, hogyan toorecover egy kétrétegű WordPress alkalmazás tooAzure:</span><span class="sxs-lookup"><span data-stu-id="165eb-233">It demonstrates how toorecover a two-tier WordPress application tooAzure:</span></span>


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a><span data-ttu-id="165eb-234">További források</span><span class="sxs-lookup"><span data-stu-id="165eb-234">Additional resources</span></span>
* [<span data-ttu-id="165eb-235">Azure Automation szolgáltatást futtató fiók</span><span class="sxs-lookup"><span data-stu-id="165eb-235">Azure Automation service Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)
* [<span data-ttu-id="165eb-236">Azure Automation áttekintése</span><span class="sxs-lookup"><span data-stu-id="165eb-236">Azure Automation overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation – áttekintés")
* [<span data-ttu-id="165eb-237">Azure Automation-mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="165eb-237">Azure Automation sample scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure Automation-mintaparancsfájlok")
