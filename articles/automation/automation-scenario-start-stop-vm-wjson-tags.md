---
title: "JSON-formátumú címkék használata az Azure VM állapotának ütemezése |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan címkék JSON karakterláncok használatával automatizálhatja a virtuális gép indítási és leállítási ütemezését."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: f8d9563318c3afe299cebb7c889874392f114f84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-to-create-a-schedule-for-azure-vm-startup-and-shutdown"></a><span data-ttu-id="a519a-103">Azure Automation-forgatókönyv: JSON-formátumú címkék használatával az Azure virtuális gép indítási és leállítási ütemezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="a519a-103">Azure Automation scenario: Using JSON-formatted tags to create a schedule for Azure VM startup and shutdown</span></span>
<span data-ttu-id="a519a-104">Az ügyfelek gyakran szeretné ütemezni a indításakor és leállásakor előfizetés költségek csökkentése vagy üzleti és műszaki követelményeken támogatja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="a519a-104">Customers often want to schedule the startup and shutdown of virtual machines to help reduce subscription costs or support business and technical requirements.</span></span>

<span data-ttu-id="a519a-105">A következő forgatókönyv az automatikus indítási és leállítási a virtuális gépek beállítása nevű ütemezés egy erőforráscsoport szintjén vagy Azure virtuális gépek szintjén címke használatával teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="a519a-105">The following scenario enables you to set up automated startup and shutdown of your VMs by using a tag called Schedule at a resource group level or virtual machine level in Azure.</span></span> <span data-ttu-id="a519a-106">Az ütemezés konfigurálható vasárnaptól szombatig egy indítási és leállítási ideje.</span><span class="sxs-lookup"><span data-stu-id="a519a-106">This schedule can be configured from Sunday to Saturday with a startup time and shutdown time.</span></span>

<span data-ttu-id="a519a-107">Néhány, a-kész lehetőség van.</span><span class="sxs-lookup"><span data-stu-id="a519a-107">We do have some out-of-the-box options.</span></span> <span data-ttu-id="a519a-108">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="a519a-108">These include:</span></span>

* <span data-ttu-id="a519a-109">[Virtuálisgép-méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) az automatikus skálázási beállításokat, amelyek lehetővé teszik, hogy a bejövő vagy kimenő méretezni.</span><span class="sxs-lookup"><span data-stu-id="a519a-109">[Virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) with autoscale settings that enable you to scale in or out.</span></span>
* <span data-ttu-id="a519a-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) szolgáltatást, amely az ütemezés indítási és leállítási műveletek a beépített alkalmas.</span><span class="sxs-lookup"><span data-stu-id="a519a-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) service, which has the built-in capability of scheduling startup and shutdown operations.</span></span>

<span data-ttu-id="a519a-111">Azonban ezek a beállítások csak támogatja az adott forgatókönyveket és infrastruktúra,--szolgáltatás (IaaS) virtuális gépeket nem lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="a519a-111">However, these options only support specific scenarios and cannot be applied to infrastructure-as-a-service (IaaS) VMs.</span></span>

<span data-ttu-id="a519a-112">Ütemezés címke erőforráscsoporthoz alkalmazása esetén is vonatkozik, amelyeket belül az összes virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a519a-112">When the Schedule tag is applied to a resource group, it's also applied to all virtual machines inside that resource group.</span></span> <span data-ttu-id="a519a-113">Ha ütemezés közvetlenül is vonatkozik egy virtuális Gépet, az utolsó ütemezési elsőbbséget a következő sorrendben:</span><span class="sxs-lookup"><span data-stu-id="a519a-113">If a schedule is also directly applied to a VM, the last schedule takes precedence in the following order:</span></span>

1. <span data-ttu-id="a519a-114">Az erőforráscsoporthoz alkalmazott ütemezése</span><span class="sxs-lookup"><span data-stu-id="a519a-114">Schedule applied to a resource group</span></span>
2. <span data-ttu-id="a519a-115">Egy erőforráscsoport és a virtuális gép erőforráscsoportban alkalmazott ütemezése</span><span class="sxs-lookup"><span data-stu-id="a519a-115">Schedule applied to a resource group and virtual machine in the resource group</span></span>
3. <span data-ttu-id="a519a-116">A virtuális gépek alkalmazott ütemezése</span><span class="sxs-lookup"><span data-stu-id="a519a-116">Schedule applied to a virtual machine</span></span>

<span data-ttu-id="a519a-117">Ebben a forgatókönyvben lényegében lép a megadott formátumban JSON karakterláncnak, és hozzáadja azt a értékeként egy címke nevű ütemezés.</span><span class="sxs-lookup"><span data-stu-id="a519a-117">This scenario essentially takes a JSON string with a specified format and adds it as the value for a tag called Schedule.</span></span> <span data-ttu-id="a519a-118">A runbook majd felsorolja az összes erőforráscsoport-sablonok és virtuális gépeket, és az ütemezések azonosítja az egyes virtuális gépek a korábban felsorolt forgatókönyvek alapján.</span><span class="sxs-lookup"><span data-stu-id="a519a-118">Then a runbook lists all resource groups and virtual machines and identifies the schedules for each VM based on the scenarios listed earlier.</span></span> <span data-ttu-id="a519a-119">Ezután végighalad a virtuális gépek, amelyeken a csatolt, és értékeli ki, mit kell tenni.</span><span class="sxs-lookup"><span data-stu-id="a519a-119">Next it loops through the VMs that have schedules attached and evaluates what action should be taken.</span></span> <span data-ttu-id="a519a-120">Például meghatározza, hogy igénylő virtuális gépek leállítása, állítsa le vagy figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="a519a-120">For example, it determines which VMs need to be stopped, shut down, or ignored.</span></span>

<span data-ttu-id="a519a-121">Ezek a runbookok hitelesítéshez használja a [Azure-beli futtató fiók](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="a519a-121">These runbooks authenticate by using the [Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>

## <a name="download-the-runbooks-for-the-scenario"></a><span data-ttu-id="a519a-122">Töltse le a runbookok a forgatókönyvhöz</span><span class="sxs-lookup"><span data-stu-id="a519a-122">Download the runbooks for the scenario</span></span>
<span data-ttu-id="a519a-123">Ez a forgatókönyv áll négy PowerShell-munkafolyamati forgatókönyvek tölthet le a [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) vagy a [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) tárház ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="a519a-123">This scenario consists of four PowerShell Workflow runbooks that you can download from the [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) or the [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) repository for this project.</span></span>

| <span data-ttu-id="a519a-124">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="a519a-124">Runbook</span></span> | <span data-ttu-id="a519a-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="a519a-125">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a519a-126">Teszt-ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="a519a-126">Test-ResourceSchedule</span></span> |<span data-ttu-id="a519a-127">Minden virtuális gép ütemezés ellenőrzi, és leállítása vagy indítása attól függően, hogy az ütemezés hajt végre.</span><span class="sxs-lookup"><span data-stu-id="a519a-127">Checks each virtual machine schedule and performs shutdown or startup depending on the schedule.</span></span> |
| <span data-ttu-id="a519a-128">Adja hozzá ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="a519a-128">Add-ResourceSchedule</span></span> |<span data-ttu-id="a519a-129">Az ütemezés címke hozzáadása a virtuális gép vagy az erőforrás-csoportot.</span><span class="sxs-lookup"><span data-stu-id="a519a-129">Adds the Schedule tag to a VM or resource group.</span></span> |
| <span data-ttu-id="a519a-130">Frissítés-ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="a519a-130">Update-ResourceSchedule</span></span> |<span data-ttu-id="a519a-131">Módosítja a meglévő ütemezés címke cserélje le a egy újat.</span><span class="sxs-lookup"><span data-stu-id="a519a-131">Modifies the existing Schedule tag by replacing it with a new one.</span></span> |
| <span data-ttu-id="a519a-132">Remove-ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="a519a-132">Remove-ResourceSchedule</span></span> |<span data-ttu-id="a519a-133">Az ütemezés címke eltávolítása a virtuális gép vagy az erőforrás-csoportot.</span><span class="sxs-lookup"><span data-stu-id="a519a-133">Removes the Schedule tag from a VM or resource group.</span></span> |

## <a name="install-and-configure-this-scenario"></a><span data-ttu-id="a519a-134">A forgatókönyv telepítése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a519a-134">Install and configure this scenario</span></span>
### <a name="install-and-publish-the-runbooks"></a><span data-ttu-id="a519a-135">A forgatókönyv telepítése és közzététele</span><span class="sxs-lookup"><span data-stu-id="a519a-135">Install and publish the runbooks</span></span>
<span data-ttu-id="a519a-136">A runbookok a letöltés után importálhatja azokat az eljárás használatával [létrehozása vagy importálása az Azure Automationben runbook](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span><span class="sxs-lookup"><span data-stu-id="a519a-136">After downloading the runbooks, you can import them by using the procedure in [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span></span>  <span data-ttu-id="a519a-137">Minden runbook közzététele, miután sikeresen importálta az Automation-fiók be.</span><span class="sxs-lookup"><span data-stu-id="a519a-137">Publish each runbook after it has been successfully imported into your Automation account.</span></span>

### <a name="add-a-schedule-to-the-test-resourceschedule-runbook"></a><span data-ttu-id="a519a-138">A Test-ResourceSchedule runbook ütemezés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a519a-138">Add a schedule to the Test-ResourceSchedule runbook</span></span>
<span data-ttu-id="a519a-139">Kövesse az alábbi lépéseket ahhoz, hogy a teszt-ResourceSchedule runbookhoz ütemezést.</span><span class="sxs-lookup"><span data-stu-id="a519a-139">Follow these steps to enable the schedule for the Test-ResourceSchedule runbook.</span></span> <span data-ttu-id="a519a-140">Ez azért, amely ellenőrzi a virtuális gépek kell indítása, leállítása, vagy marad, a runbookot.</span><span class="sxs-lookup"><span data-stu-id="a519a-140">This is the runbook that verifies which virtual machines should be started, shut down, or left as is.</span></span>

1. <span data-ttu-id="a519a-141">Azure-portálról, nyissa meg az Automation-fiók, és kattintson a **Runbookok** csempére.</span><span class="sxs-lookup"><span data-stu-id="a519a-141">From the Azure portal, open your Automation account, and then click the **Runbooks** tile.</span></span>
2. <span data-ttu-id="a519a-142">Az a **teszt-ResourceSchedule** panelen kattintson a **ütemezések** csempére.</span><span class="sxs-lookup"><span data-stu-id="a519a-142">On the **Test-ResourceSchedule** blade, click the **Schedules** tile.</span></span>
3. <span data-ttu-id="a519a-143">Az a **ütemezések** panelen kattintson a **ütemezés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="a519a-143">On the **Schedules** blade, click **Add a schedule**.</span></span>
4. <span data-ttu-id="a519a-144">Az a **ütemezések** panelen válassza **ütemezés kapcsolása a forgatókönyvhöz**.</span><span class="sxs-lookup"><span data-stu-id="a519a-144">On the **Schedules** blade, select **Link a schedule to your runbook**.</span></span> <span data-ttu-id="a519a-145">Válassza ki **hozzon létre egy új ütemezést**.</span><span class="sxs-lookup"><span data-stu-id="a519a-145">Then select **Create a new schedule**.</span></span>
5. <span data-ttu-id="a519a-146">Az a **új ütemezés** panelen típus nevében ezt az ütemezést, például: *HourlyExecution*.</span><span class="sxs-lookup"><span data-stu-id="a519a-146">On the **New schedule** blade, type in the name of this schedule, for example: *HourlyExecution*.</span></span>
6. <span data-ttu-id="a519a-147">Az ütemezés **Start**, állítsa be a kezdési időpont egy óra növelésére.</span><span class="sxs-lookup"><span data-stu-id="a519a-147">For the schedule **Start**, set the start time to an hour increment.</span></span>
7. <span data-ttu-id="a519a-148">Válassza ki **ismétlődési**, és ezután a **megismétlődik minden időköz**, jelölje be **1 óra**.</span><span class="sxs-lookup"><span data-stu-id="a519a-148">Select **Recurrence**, and then for **Recur every interval**, select **1 hour**.</span></span>
8. <span data-ttu-id="a519a-149">Ellenőrizze, hogy **beállíthatja a lejárati idejét** értéke **nem**, és kattintson a **létrehozása** menteni az új ütemezést.</span><span class="sxs-lookup"><span data-stu-id="a519a-149">Verify that **Set expiration** is set to **No**, and then click **Create** to save your new schedule.</span></span>
9. <span data-ttu-id="a519a-150">Az a **ütemterv forgatókönyv** beállítások panelen válassza **paraméterek és futtatási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="a519a-150">On the **Schedule Runbook** options blade, select **Parameters and run settings**.</span></span> <span data-ttu-id="a519a-151">A Test-ResourceSchedule **paraméterek** panelen adja meg az előfizetését a **SubscriptionName** mező.</span><span class="sxs-lookup"><span data-stu-id="a519a-151">In the Test-ResourceSchedule **Parameters** blade, enter the name of your subscription in the **SubscriptionName** field.</span></span>  <span data-ttu-id="a519a-152">Ez az az egyetlen paraméter, amely szükséges a runbookhoz.</span><span class="sxs-lookup"><span data-stu-id="a519a-152">This is the only parameter that's required for the runbook.</span></span>  <span data-ttu-id="a519a-153">Amikor végzett, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a519a-153">When you're finished, click **OK**.</span></span>

<span data-ttu-id="a519a-154">A runbook-ütemezés a következőképpen kell kinéznie kész:</span><span class="sxs-lookup"><span data-stu-id="a519a-154">The runbook schedule should look like the following when it's completed:</span></span>

![Runbook. teszt-ResourceSchedule konfigurált.](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-the-json-string"></a><span data-ttu-id="a519a-156">A JSON-karakterláncban formázása</span><span class="sxs-lookup"><span data-stu-id="a519a-156">Format the JSON string</span></span>
<span data-ttu-id="a519a-157">Ez a megoldás alapvetően vesz egy JSON megadott formátumú karakterláncot, és hozzáadja azt a címke értéke nevű ütemezés.</span><span class="sxs-lookup"><span data-stu-id="a519a-157">This solution basically takes a JSON string with a specified format and adds it as the value for a tag called Schedule.</span></span> <span data-ttu-id="a519a-158">Ezután a runbook összes erőforráscsoport-sablonok és virtuális gépek sorolja fel, és azonosítja az egyes virtuális gépek ütemezések.</span><span class="sxs-lookup"><span data-stu-id="a519a-158">Then a runbook lists all resource groups and virtual machines and identifies the schedules for each virtual machine.</span></span>

<span data-ttu-id="a519a-159">A runbook fut a hurokban, a csatolt rendelkező virtuális gépeket, és ellenőrzi, milyen műveleteket kell végezni.</span><span class="sxs-lookup"><span data-stu-id="a519a-159">The runbook loops over the virtual machines that have schedules attached and checks what actions should be taken.</span></span> <span data-ttu-id="a519a-160">A következő egy példa a megoldások hogyan kell formázni:</span><span class="sxs-lookup"><span data-stu-id="a519a-160">The following is an example of how the solutions should be formatted:</span></span>

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

<span data-ttu-id="a519a-161">Ez a struktúra kapcsolatos részletes információkat a következő:</span><span class="sxs-lookup"><span data-stu-id="a519a-161">Here is some detailed information about this structure:</span></span>

1. <span data-ttu-id="a519a-162">Ez a JSON-struktúra formátuma arra optimalizálták, hogy áthidalni a 256 karakteres korlátozást az Azure-ban egyetlen címke értéke.</span><span class="sxs-lookup"><span data-stu-id="a519a-162">The format of this JSON structure is optimized to work around the 256-character limitation of a single tag value in Azure.</span></span>
2. <span data-ttu-id="a519a-163">*TzId* jelenti. a virtuális gép időzónáját.</span><span class="sxs-lookup"><span data-stu-id="a519a-163">*TzId* represents the time zone of the virtual machine.</span></span> <span data-ttu-id="a519a-164">Ezt az Azonosítót kérhetők le a TimeZoneInfo .NET-osztály használatával egy PowerShell-munkamenet--**[System.TimeZoneInfo]:: GetSystemTimeZones()**.</span><span class="sxs-lookup"><span data-stu-id="a519a-164">This ID can be obtained by using the TimeZoneInfo .NET class in a PowerShell session--**[System.TimeZoneInfo]::GetSystemTimeZones()**.</span></span>

   ![A PowerShell GetSystemTimeZones](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * <span data-ttu-id="a519a-166">Napok vannak egy hat a nulla érték jelölik.</span><span class="sxs-lookup"><span data-stu-id="a519a-166">Weekdays are represented with a numeric value of zero to six.</span></span> <span data-ttu-id="a519a-167">A nulla érték vasárnap.</span><span class="sxs-lookup"><span data-stu-id="a519a-167">The value zero equals Sunday.</span></span>
   * <span data-ttu-id="a519a-168">A kezdési időpontot jelöli a **S** attribútum, értéke pedig egy 24 órás formátumban.</span><span class="sxs-lookup"><span data-stu-id="a519a-168">The start time is represented with the **S** attribute, and its value is in a 24-hour format.</span></span>
   * <span data-ttu-id="a519a-169">A végfelhasználók vagy -leállítás időt jelöli a **E** attribútum, értéke pedig egy 24 órás formátumban.</span><span class="sxs-lookup"><span data-stu-id="a519a-169">The end or shutdown time is represented with the **E** attribute, and its value is in a 24-hour format.</span></span>

     <span data-ttu-id="a519a-170">Ha a **S** és **E** attribútumok minden értéket veheti fel nulla (0), a virtuális gép értékelése során marad a jelenlegi állapotában.</span><span class="sxs-lookup"><span data-stu-id="a519a-170">If the **S** and **E** attributes each have a value of zero (0), the virtual machine will be left in its present state at the time of evaluation.</span></span>
3. <span data-ttu-id="a519a-171">Ha ki szeretné hagyni a hét egy adott napjára értékelése, nem adja hozzá a szakaszt, hogy a hét napjára.</span><span class="sxs-lookup"><span data-stu-id="a519a-171">If you want to skip evaluation for a specific day of the week, don’t add a section for that day of the week.</span></span> <span data-ttu-id="a519a-172">A következő példában csak hétfő ki lesz értékelve, és az egyéb napokat a hét figyelmen kívül hagyja:</span><span class="sxs-lookup"><span data-stu-id="a519a-172">In the following example, only Monday is evaluated, and the other days of the week are ignored:</span></span>

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a><span data-ttu-id="a519a-173">Címke erőforráscsoport-sablonok és virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="a519a-173">Tag resource groups or VMs</span></span>
<span data-ttu-id="a519a-174">Virtuális gépek leállítását, vagy a virtuális gépeket, vagy az erőforráscsoportok, amelyben fontosságúak található címkét kell.</span><span class="sxs-lookup"><span data-stu-id="a519a-174">To shut down VMs, you need to tag either the VMs or the resource groups in which they're located.</span></span> <span data-ttu-id="a519a-175">A ütemezés címkével nem rendelkező virtuális gépek nem értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="a519a-175">Virtual machines that don't have a Schedule tag are not evaluated.</span></span> <span data-ttu-id="a519a-176">Nem, ezért azok elindítva, vagy állítsa le.</span><span class="sxs-lookup"><span data-stu-id="a519a-176">Therefore, they aren't started or shut down.</span></span>

<span data-ttu-id="a519a-177">Ezen megoldás címke erőforráscsoportok vagy a virtuális gépek két módja van.</span><span class="sxs-lookup"><span data-stu-id="a519a-177">There are two ways to tag resource groups or VMs with this solution.</span></span> <span data-ttu-id="a519a-178">Megteheti közvetlenül a portálról.</span><span class="sxs-lookup"><span data-stu-id="a519a-178">You can do it directly from the portal.</span></span> <span data-ttu-id="a519a-179">Vagy az Add-ResourceSchedule, a frissítés-ResourceSchedule és a Remove-ResourceSchedule runbookok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a519a-179">Or you can use the Add-ResourceSchedule, Update-ResourceSchedule, and Remove-ResourceSchedule runbooks.</span></span>

### <a name="tag-through-the-portal"></a><span data-ttu-id="a519a-180">A portálon keresztül címke</span><span class="sxs-lookup"><span data-stu-id="a519a-180">Tag through the portal</span></span>
<span data-ttu-id="a519a-181">Virtuális gép vagy erőforráscsoport címkét a portálon lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="a519a-181">Follow these steps to tag a virtual machine or resource group in the portal:</span></span>

1. <span data-ttu-id="a519a-182">A JSON-karakterláncban egybesimítására, és győződjön meg arról, hogy nincs-e szóközt.</span><span class="sxs-lookup"><span data-stu-id="a519a-182">Flatten the JSON string and verify that there aren't any spaces.</span></span>  <span data-ttu-id="a519a-183">A JSON karakterláncnak kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="a519a-183">Your JSON string should look like this:</span></span>

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. <span data-ttu-id="a519a-184">Válassza ki a **címke** egy virtuális gép vagy erőforrás csoport ütemezés alkalmazása ikonra.</span><span class="sxs-lookup"><span data-stu-id="a519a-184">Select the **Tag** icon for a VM or resource group to apply this schedule.</span></span>

   ![Virtuálisgép-kód beállítását](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. <span data-ttu-id="a519a-186">Címkék vannak definiálva a következő egy kulcs/érték pár.</span><span class="sxs-lookup"><span data-stu-id="a519a-186">Tags are defined following a key/value pair.</span></span> <span data-ttu-id="a519a-187">Típus **ütemezés** a a **kulcs** mezőben, majd illessze be a JSON-karakterláncban a **érték** mező.</span><span class="sxs-lookup"><span data-stu-id="a519a-187">Type **Schedule** in the **Key** field, and then paste the JSON string into the **Value** field.</span></span> <span data-ttu-id="a519a-188">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a519a-188">Click **Save**.</span></span> <span data-ttu-id="a519a-189">Az új címke ekkor meg kell jelennie az erőforrás címkék listájában.</span><span class="sxs-lookup"><span data-stu-id="a519a-189">Your new tag should now appear in the list of tags for your resource.</span></span>

   ![Virtuális gép ütemezés címke](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a><span data-ttu-id="a519a-191">PowerShell-címke</span><span class="sxs-lookup"><span data-stu-id="a519a-191">Tag from PowerShell</span></span>
<span data-ttu-id="a519a-192">Minden importált runbookokat, amely leírja, hogyan hajthat végre a runbookok közvetlenül a PowerShell parancsfájl elején súgó-információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="a519a-192">All imported runbooks contain help information at the beginning of the script that describes how to execute the runbooks directly from PowerShell.</span></span> <span data-ttu-id="a519a-193">Az Add-ScheduleResource és frissítés-ScheduleResource runbookok powershellből hívása.</span><span class="sxs-lookup"><span data-stu-id="a519a-193">You can call the Add-ScheduleResource and Update-ScheduleResource runbooks from PowerShell.</span></span> <span data-ttu-id="a519a-194">Ehhez úgy, hogy a szükséges paramétereket, amelyek lehetővé teszik, hogy létrehozni vagy frissíteni a virtuális gép vagy az erőforrás-csoporton a portálon kívül az ütemezés címke.</span><span class="sxs-lookup"><span data-stu-id="a519a-194">You do this by passing required parameters that enable you to create or update the Schedule tag on a VM or resource group outside of the portal.</span></span>

<span data-ttu-id="a519a-195">Szeretne létrehozni, adja hozzá, és a PowerShell segítségével, először meg kell címkék törlése [állítsa be a PowerShell környezetet az Azure-](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a519a-195">To create, add, and delete tags through PowerShell, you first need to [set up your PowerShell environment for Azure](/powershell/azure/overview).</span></span> <span data-ttu-id="a519a-196">A telepítés befejezése után továbbléphet a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="a519a-196">After you complete the setup, you can proceed with the following steps.</span></span>

### <a name="create-a-schedule-tag-with-powershell"></a><span data-ttu-id="a519a-197">Egy ütemezés címke létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a519a-197">Create a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="a519a-198">Nyisson meg egy PowerShell-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="a519a-198">Open a PowerShell session.</span></span> <span data-ttu-id="a519a-199">A hitelesítést a Futtatás mint fiókkal, és adja meg az előfizetés, kövesse az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="a519a-199">Then use the following example to authenticate with your Run As account and to specify a subscription:</span></span>

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="a519a-200">Adjon meg egy ütemezést kivonatoló táblát.</span><span class="sxs-lookup"><span data-stu-id="a519a-200">Define a schedule hash table.</span></span> <span data-ttu-id="a519a-201">Íme egy példa hogyan kell létrehozni:</span><span class="sxs-lookup"><span data-stu-id="a519a-201">Here is an example of how it should be constructed:</span></span>

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. <span data-ttu-id="a519a-202">A runbook által igényelt paramétereinek megadása.</span><span class="sxs-lookup"><span data-stu-id="a519a-202">Define the parameters that are required by the runbook.</span></span> <span data-ttu-id="a519a-203">A következő példában azt céloz meg egy virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="a519a-203">In the following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    <span data-ttu-id="a519a-204">Ha az erőforráscsoport folyamatban címkézést, távolítsa el a *VMName* paraméter $params kivonatát a tábla az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a519a-204">If you’re tagging a resource group, remove the *VMName* parameter from the $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. <span data-ttu-id="a519a-205">Futtassa az Add-ResourceSchedule runbook ütemezés címke létrehozásához a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="a519a-205">Run the Add-ResourceSchedule runbook with the following parameters to create the Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. <span data-ttu-id="a519a-206">Egy erőforráscsoport vagy a virtuális gép címke frissítéséhez hajtsa végre a **frissítés-ResourceSchedule** runbook a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="a519a-206">To update a resource group or virtual machine tag, execute the **Update-ResourceSchedule** runbook with the following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a><span data-ttu-id="a519a-207">A PowerShell-lel ütemezés címke eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a519a-207">Remove a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="a519a-208">Nyisson meg egy PowerShell-munkamenetet, és futtassa a következő, a futtató fiókhoz hitelesítéséhez, és adja meg az előfizetés:</span><span class="sxs-lookup"><span data-stu-id="a519a-208">Open a PowerShell session and run the following to authenticate with your Run As account and to select and specify a subscription:</span></span>

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="a519a-209">A runbook által igényelt paramétereinek megadása.</span><span class="sxs-lookup"><span data-stu-id="a519a-209">Define the parameters that are required by the runbook.</span></span> <span data-ttu-id="a519a-210">A következő példában azt céloz meg egy virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="a519a-210">In the following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    <span data-ttu-id="a519a-211">Ha szeretne eltávolítani egy címkét egy erőforráscsoportból, távolítsa el a *VMName* paraméter $params kivonatát a tábla az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a519a-211">If you’re removing a tag from a resource group, remove the *VMName* parameter from the $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. <span data-ttu-id="a519a-212">Az ütemezés címke eltávolítása a Remove-ResourceSchedule runbook hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="a519a-212">Execute the Remove-ResourceSchedule runbook to remove the Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. <span data-ttu-id="a519a-213">Egy erőforráscsoport vagy a virtuális gép címke frissítéséhez hajtsa végre a Remove-ResourceSchedule runbook a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="a519a-213">To update a resource group or virtual machine tag, execute the Remove-ResourceSchedule runbook with the following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> <span data-ttu-id="a519a-214">Azt javasoljuk, hogy proaktív nyomon, hogy ezek a runbookok (és a virtuális gép állapota) ellenőrzése, hogy a virtuális gépek vannak épp most állítja le, és ennek megfelelően elindult.</span><span class="sxs-lookup"><span data-stu-id="a519a-214">We recommend that you proactively monitor these runbooks (and the virtual machine states) to verify that your virtual machines are being shut down and started accordingly.</span></span>
>

<span data-ttu-id="a519a-215">Válassza ki, ha a teszt-ResourceSchedule runbook-feladat részletes adatait az Azure portálon, a **feladatok** csempe a runbook.</span><span class="sxs-lookup"><span data-stu-id="a519a-215">To view the details of the Test-ResourceSchedule runbook job in the Azure portal, select the **Jobs** tile of the runbook.</span></span> <span data-ttu-id="a519a-216">A feladat összegzésében megtekinthetők a bemeneti paraméterek, a kimeneti stream, valamint a feladattal kapcsolatos általános információk és a kivételek (ha sor került rájuk).</span><span class="sxs-lookup"><span data-stu-id="a519a-216">The job summary displays the input parameters and the output stream, in addition to general information about the job and any exceptions if they occurred.</span></span>

<span data-ttu-id="a519a-217">A **Feladat összegzése** a kimeneti, figyelmeztető és hibastreamek üzeneteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a519a-217">The **Job Summary** includes messages from the output, warning, and error streams.</span></span> <span data-ttu-id="a519a-218">A forgatókönyv végrehajtásának részletes eredményeinek megtekintéséhez válassza a **Kimenet** csempét.</span><span class="sxs-lookup"><span data-stu-id="a519a-218">Select the **Output** tile to view detailed results from the runbook execution.</span></span>

![Teszt-ResourceSchedule kimeneti](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a><span data-ttu-id="a519a-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a519a-220">Next steps</span></span>
* <span data-ttu-id="a519a-221">A PowerShell-alapú munkafolyamat-runbookok első lépéseit [Az első PowerShell-alapú munkafolyamat-runbookom](automation-first-runbook-textual.md) című témakör ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a519a-221">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md).</span></span>
* <span data-ttu-id="a519a-222">Runbook típusait, és azok előnyeit és korlátozások kapcsolatos további információkért lásd: [Azure Automation-runbook típusok](automation-runbook-types.md).</span><span class="sxs-lookup"><span data-stu-id="a519a-222">To learn more about runbook types, and their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md).</span></span>
* <span data-ttu-id="a519a-223">PowerShell parancsfájl támogatási funkciókkal kapcsolatos további információkért lásd: [natív PowerShell-parancsfájl támogatás az Azure Automationben](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span><span class="sxs-lookup"><span data-stu-id="a519a-223">For more information about PowerShell script support features, see [Native PowerShell script support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span></span>
* <span data-ttu-id="a519a-224">Runbook-naplózás és a kimeneti kapcsolatos további információkért lásd: [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="a519a-224">To learn more about runbook logging and output, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md).</span></span>
* <span data-ttu-id="a519a-225">Egy Azure-beli futtató fiókot, és hogyan kell elvégezni a hitelesítést a runbookok azt kapcsolatos további információkért lásd: [hitelesítéséhez az Azure-beli futtató fiók runbookok](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="a519a-225">To learn more about an Azure Run As account and how to authenticate your runbooks by using it, see [Authenticate runbooks with Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>
