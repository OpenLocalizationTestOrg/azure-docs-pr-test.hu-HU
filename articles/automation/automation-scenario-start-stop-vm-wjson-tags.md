---
title: "JSON-formátumú címkék tooschedule Azure VM állapotának aaaUse |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toouse JSON címkék tooautomate hello ütemezéséről a virtuális gép indítási és leállítási karakterláncokat."
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
ms.openlocfilehash: f6bbf1dea1c193e5d1010f12f3b1ed63562f9daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-toocreate-a-schedule-for-azure-vm-startup-and-shutdown"></a><span data-ttu-id="dad39-103">Azure Automation-forgatókönyv: egy ütemezést az Azure virtuális gép indítási és leállítási toocreate használatával a JSON-formátumú címkéket</span><span class="sxs-lookup"><span data-stu-id="dad39-103">Azure Automation scenario: Using JSON-formatted tags toocreate a schedule for Azure VM startup and shutdown</span></span>
<span data-ttu-id="dad39-104">Ügyfelek gyakran szeretné tooschedule hello indítási és virtuális gépek toohelp leállása előfizetés költségek csökkentése vagy üzleti és műszaki követelményeken támogatja.</span><span class="sxs-lookup"><span data-stu-id="dad39-104">Customers often want tooschedule hello startup and shutdown of virtual machines toohelp reduce subscription costs or support business and technical requirements.</span></span>

<span data-ttu-id="dad39-105">hello következő forgatókönyv lehetővé teszi fel az automatikus indítási és leállítási a virtuális gépek tooset nevű ütemezés egy erőforráscsoport szintjén vagy Azure virtuális gépek szintjén címke használatával.</span><span class="sxs-lookup"><span data-stu-id="dad39-105">hello following scenario enables you tooset up automated startup and shutdown of your VMs by using a tag called Schedule at a resource group level or virtual machine level in Azure.</span></span> <span data-ttu-id="dad39-106">Ezt az ütemezést is lehet konfigurálni az vasárnap tooSaturday egy indítási idő-és leállítási ideje.</span><span class="sxs-lookup"><span data-stu-id="dad39-106">This schedule can be configured from Sunday tooSaturday with a startup time and shutdown time.</span></span>

<span data-ttu-id="dad39-107">Néhány, a-kész lehetőség van.</span><span class="sxs-lookup"><span data-stu-id="dad39-107">We do have some out-of-the-box options.</span></span> <span data-ttu-id="dad39-108">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="dad39-108">These include:</span></span>

* <span data-ttu-id="dad39-109">[Virtuálisgép-méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) lehetővé teszik a bejövő vagy kimenő tooscale automatikus skálázási beállításokat.</span><span class="sxs-lookup"><span data-stu-id="dad39-109">[Virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) with autoscale settings that enable you tooscale in or out.</span></span>
* <span data-ttu-id="dad39-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) szolgáltatást, amely rendelkezik beépített funkcióval hello az indítási és leállítási műveletek ütemezés.</span><span class="sxs-lookup"><span data-stu-id="dad39-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) service, which has hello built-in capability of scheduling startup and shutdown operations.</span></span>

<span data-ttu-id="dad39-111">Azonban ezek a beállítások csak támogatja az adott forgatókönyveket, és nem lehet alkalmazott tooinfrastructure,--szolgáltatás (IaaS) virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="dad39-111">However, these options only support specific scenarios and cannot be applied tooinfrastructure-as-a-service (IaaS) VMs.</span></span>

<span data-ttu-id="dad39-112">Hello ütemezés címke alkalmazott tooa erőforráscsoportban, esetén is tartalmaz, amelyeket alkalmazott tooall virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="dad39-112">When hello Schedule tag is applied tooa resource group, it's also applied tooall virtual machines inside that resource group.</span></span> <span data-ttu-id="dad39-113">Ha egy ütemezés is közvetlenül alkalmazott tooa VM, hello utolsó ütemezés előnyt élvez sorrend hello:</span><span class="sxs-lookup"><span data-stu-id="dad39-113">If a schedule is also directly applied tooa VM, hello last schedule takes precedence in hello following order:</span></span>

1. <span data-ttu-id="dad39-114">Ütemezés alkalmazott tooa erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="dad39-114">Schedule applied tooa resource group</span></span>
2. <span data-ttu-id="dad39-115">Adja meg az alkalmazott tooa erőforráscsoport és a virtuális gép hello erőforráscsoportban található</span><span class="sxs-lookup"><span data-stu-id="dad39-115">Schedule applied tooa resource group and virtual machine in hello resource group</span></span>
3. <span data-ttu-id="dad39-116">Ütemezés alkalmazása tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="dad39-116">Schedule applied tooa virtual machine</span></span>

<span data-ttu-id="dad39-117">Ebben a forgatókönyvben alapvetően a megadott formátumban JSON karakterláncnak vesz igénybe, és hozzáadja azt a hello értékeként egy címke nevű ütemezés.</span><span class="sxs-lookup"><span data-stu-id="dad39-117">This scenario essentially takes a JSON string with a specified format and adds it as hello value for a tag called Schedule.</span></span> <span data-ttu-id="dad39-118">Ezután egy runbook felsorolja az összes erőforráscsoport-sablonok és virtuális gépek és hello ütemezések azonosítja az egyes virtuális gépek a korábban felsorolt hello forgatókönyvek alapján.</span><span class="sxs-lookup"><span data-stu-id="dad39-118">Then a runbook lists all resource groups and virtual machines and identifies hello schedules for each VM based on hello scenarios listed earlier.</span></span> <span data-ttu-id="dad39-119">Ezután végighalad a virtuális gépek, amelyeken a csatolt hello, és értékeli ki, mit kell tenni.</span><span class="sxs-lookup"><span data-stu-id="dad39-119">Next it loops through hello VMs that have schedules attached and evaluates what action should be taken.</span></span> <span data-ttu-id="dad39-120">Például meghatározza, hogy igénylő virtuális gépek toobe leállt, állítsa le vagy figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="dad39-120">For example, it determines which VMs need toobe stopped, shut down, or ignored.</span></span>

<span data-ttu-id="dad39-121">Ezek a runbookok hitelesítést hello [Azure-beli futtató fiók](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="dad39-121">These runbooks authenticate by using hello [Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>

## <a name="download-hello-runbooks-for-hello-scenario"></a><span data-ttu-id="dad39-122">Töltse le a hello runbookokat hello a forgatókönyvhöz</span><span class="sxs-lookup"><span data-stu-id="dad39-122">Download hello runbooks for hello scenario</span></span>
<span data-ttu-id="dad39-123">Ez a forgatókönyv áll tölthető le: hello négy PowerShell munkafolyamat-forgatókönyvekről [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) vagy hello [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) tárház ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="dad39-123">This scenario consists of four PowerShell Workflow runbooks that you can download from hello [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) or hello [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) repository for this project.</span></span>

| <span data-ttu-id="dad39-124">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="dad39-124">Runbook</span></span> | <span data-ttu-id="dad39-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="dad39-125">Description</span></span> |
| --- | --- |
| <span data-ttu-id="dad39-126">Teszt-ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="dad39-126">Test-ResourceSchedule</span></span> |<span data-ttu-id="dad39-127">Minden virtuális gép ütemezés ellenőrzi, és leállítása vagy indítása hello ütemezés attól függően hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="dad39-127">Checks each virtual machine schedule and performs shutdown or startup depending on hello schedule.</span></span> |
| <span data-ttu-id="dad39-128">Adja hozzá ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="dad39-128">Add-ResourceSchedule</span></span> |<span data-ttu-id="dad39-129">Hello ütemezés címke tooa VM vagy erőforrás csoport hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="dad39-129">Adds hello Schedule tag tooa VM or resource group.</span></span> |
| <span data-ttu-id="dad39-130">Frissítés-ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="dad39-130">Update-ResourceSchedule</span></span> |<span data-ttu-id="dad39-131">Cserélje le a egy új hello meglévő ütemezés címke módosítja.</span><span class="sxs-lookup"><span data-stu-id="dad39-131">Modifies hello existing Schedule tag by replacing it with a new one.</span></span> |
| <span data-ttu-id="dad39-132">Remove-ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="dad39-132">Remove-ResourceSchedule</span></span> |<span data-ttu-id="dad39-133">Hello ütemezés címke eltávolítása a virtuális gép vagy az erőforrás-csoportot.</span><span class="sxs-lookup"><span data-stu-id="dad39-133">Removes hello Schedule tag from a VM or resource group.</span></span> |

## <a name="install-and-configure-this-scenario"></a><span data-ttu-id="dad39-134">A forgatókönyv telepítése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dad39-134">Install and configure this scenario</span></span>
### <a name="install-and-publish-hello-runbooks"></a><span data-ttu-id="dad39-135">Telepítse és runbookokat hello közzététele</span><span class="sxs-lookup"><span data-stu-id="dad39-135">Install and publish hello runbooks</span></span>
<span data-ttu-id="dad39-136">Runbookokat hello a letöltés után importálhatja azokat a hello eljárással [létrehozása vagy importálása az Azure Automationben runbook](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span><span class="sxs-lookup"><span data-stu-id="dad39-136">After downloading hello runbooks, you can import them by using hello procedure in [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span></span>  <span data-ttu-id="dad39-137">Minden runbook közzététele, miután sikeresen importálta az Automation-fiók be.</span><span class="sxs-lookup"><span data-stu-id="dad39-137">Publish each runbook after it has been successfully imported into your Automation account.</span></span>

### <a name="add-a-schedule-toohello-test-resourceschedule-runbook"></a><span data-ttu-id="dad39-138">Egy ütemezés toohello teszt-ResourceSchedule runbook hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dad39-138">Add a schedule toohello Test-ResourceSchedule runbook</span></span>
<span data-ttu-id="dad39-139">Kövesse a lépéseket tooenable hello ütemezés hello teszt-ResourceSchedule runbook.</span><span class="sxs-lookup"><span data-stu-id="dad39-139">Follow these steps tooenable hello schedule for hello Test-ResourceSchedule runbook.</span></span> <span data-ttu-id="dad39-140">Ez az hello runbookot, amely ellenőrzi a virtuális gépek el kell indítani, állítsa le, vagy a bal oldalon, mert a.</span><span class="sxs-lookup"><span data-stu-id="dad39-140">This is hello runbook that verifies which virtual machines should be started, shut down, or left as is.</span></span>

1. <span data-ttu-id="dad39-141">Az Azure-portálon hello, nyissa meg az Automation-fiók, és kattintson a hello **Runbookok** csempére.</span><span class="sxs-lookup"><span data-stu-id="dad39-141">From hello Azure portal, open your Automation account, and then click hello **Runbooks** tile.</span></span>
2. <span data-ttu-id="dad39-142">A hello **teszt-ResourceSchedule** panelen hello kattintson **ütemezések** csempére.</span><span class="sxs-lookup"><span data-stu-id="dad39-142">On hello **Test-ResourceSchedule** blade, click hello **Schedules** tile.</span></span>
3. <span data-ttu-id="dad39-143">A hello **ütemezések** panelen kattintson a **ütemezés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="dad39-143">On hello **Schedules** blade, click **Add a schedule**.</span></span>
4. <span data-ttu-id="dad39-144">A hello **ütemezések** panelen válassza **ütemezés tooyour runbook hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="dad39-144">On hello **Schedules** blade, select **Link a schedule tooyour runbook**.</span></span> <span data-ttu-id="dad39-145">Válassza ki **hozzon létre egy új ütemezést**.</span><span class="sxs-lookup"><span data-stu-id="dad39-145">Then select **Create a new schedule**.</span></span>
5. <span data-ttu-id="dad39-146">A hello **új ütemezés** panelen hello nevében típusú ezt az ütemezést, például: *HourlyExecution*.</span><span class="sxs-lookup"><span data-stu-id="dad39-146">On hello **New schedule** blade, type in hello name of this schedule, for example: *HourlyExecution*.</span></span>
6. <span data-ttu-id="dad39-147">Hello ütemezés **Start**, hello kezdési idő tooan óra növekmény beállítása.</span><span class="sxs-lookup"><span data-stu-id="dad39-147">For hello schedule **Start**, set hello start time tooan hour increment.</span></span>
7. <span data-ttu-id="dad39-148">Válassza ki **ismétlődési**, és ezután a **megismétlődik minden időköz**, jelölje be **1 óra**.</span><span class="sxs-lookup"><span data-stu-id="dad39-148">Select **Recurrence**, and then for **Recur every interval**, select **1 hour**.</span></span>
8. <span data-ttu-id="dad39-149">Ellenőrizze, hogy **beállíthatja a lejárati idejét** értéke túl**nem**, és kattintson a **létrehozása** toosave az új ütemezést.</span><span class="sxs-lookup"><span data-stu-id="dad39-149">Verify that **Set expiration** is set too**No**, and then click **Create** toosave your new schedule.</span></span>
9. <span data-ttu-id="dad39-150">A hello **ütemterv forgatókönyv** beállítások panelen válassza **paraméterek és futtatási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="dad39-150">On hello **Schedule Runbook** options blade, select **Parameters and run settings**.</span></span> <span data-ttu-id="dad39-151">A Test-ResourceSchedule hello **paraméterek** panelen adja meg az előfizetés hello nevét a hello **SubscriptionName** mező.</span><span class="sxs-lookup"><span data-stu-id="dad39-151">In hello Test-ResourceSchedule **Parameters** blade, enter hello name of your subscription in hello **SubscriptionName** field.</span></span>  <span data-ttu-id="dad39-152">Ez az hello egyetlen paraméter, amely runbook hello szükséges.</span><span class="sxs-lookup"><span data-stu-id="dad39-152">This is hello only parameter that's required for hello runbook.</span></span>  <span data-ttu-id="dad39-153">Amikor végzett, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="dad39-153">When you're finished, click **OK**.</span></span>

<span data-ttu-id="dad39-154">runbook-ütemezés hello alábbihoz hasonló hello kész:</span><span class="sxs-lookup"><span data-stu-id="dad39-154">hello runbook schedule should look like hello following when it's completed:</span></span>

![Runbook. teszt-ResourceSchedule konfigurált.](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-hello-json-string"></a><span data-ttu-id="dad39-156">JSON-karakterlánc formátuma hello</span><span class="sxs-lookup"><span data-stu-id="dad39-156">Format hello JSON string</span></span>
<span data-ttu-id="dad39-157">Ez a megoldás alapvetően vesz egy JSON megadott formátumú karakterlánc, és hozzáadja azt egy címke hello értékként nevű ütemezés.</span><span class="sxs-lookup"><span data-stu-id="dad39-157">This solution basically takes a JSON string with a specified format and adds it as hello value for a tag called Schedule.</span></span> <span data-ttu-id="dad39-158">Egy runbook összes erőforráscsoport-sablonok és virtuális gépek sorolja fel, és azonosítja az egyes virtuális gépek hello ütemezések.</span><span class="sxs-lookup"><span data-stu-id="dad39-158">Then a runbook lists all resource groups and virtual machines and identifies hello schedules for each virtual machine.</span></span>

<span data-ttu-id="dad39-159">hello runbook fut a hurokban, amelyek csatolt hello virtuális gépeket, és ellenőrzi, milyen műveleteket kell végezni.</span><span class="sxs-lookup"><span data-stu-id="dad39-159">hello runbook loops over hello virtual machines that have schedules attached and checks what actions should be taken.</span></span> <span data-ttu-id="dad39-160">hello az alábbiakban látható egy példa hogyan hello megoldások formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="dad39-160">hello following is an example of how hello solutions should be formatted:</span></span>

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

<span data-ttu-id="dad39-161">Ez a struktúra kapcsolatos részletes információkat a következő:</span><span class="sxs-lookup"><span data-stu-id="dad39-161">Here is some detailed information about this structure:</span></span>

1. <span data-ttu-id="dad39-162">Ez a JSON-struktúra hello formátuma nem optimalizált toowork körül hello 256 karakteres korlátozása egy egyetlen címke az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="dad39-162">hello format of this JSON structure is optimized toowork around hello 256-character limitation of a single tag value in Azure.</span></span>
2. <span data-ttu-id="dad39-163">*TzId* jelöli hello hello virtuális gép időzónáját.</span><span class="sxs-lookup"><span data-stu-id="dad39-163">*TzId* represents hello time zone of hello virtual machine.</span></span> <span data-ttu-id="dad39-164">Ezt az Azonosítót kérhetők le hello TimeZoneInfo .NET osztály használatával egy PowerShell-munkamenet--**[System.TimeZoneInfo]:: GetSystemTimeZones()**.</span><span class="sxs-lookup"><span data-stu-id="dad39-164">This ID can be obtained by using hello TimeZoneInfo .NET class in a PowerShell session--**[System.TimeZoneInfo]::GetSystemTimeZones()**.</span></span>

   ![A PowerShell GetSystemTimeZones](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * <span data-ttu-id="dad39-166">Hétköznapokon érték nulla toosix jelölik.</span><span class="sxs-lookup"><span data-stu-id="dad39-166">Weekdays are represented with a numeric value of zero toosix.</span></span> <span data-ttu-id="dad39-167">hello nulla érték vasárnap.</span><span class="sxs-lookup"><span data-stu-id="dad39-167">hello value zero equals Sunday.</span></span>
   * <span data-ttu-id="dad39-168">hello kezdési ideje képviselt a hello **S** attribútum, értéke pedig egy 24 órás formátumban.</span><span class="sxs-lookup"><span data-stu-id="dad39-168">hello start time is represented with hello **S** attribute, and its value is in a 24-hour format.</span></span>
   * <span data-ttu-id="dad39-169">hello vége vagy -leállítás idő képviselt a hello **E** attribútum, értéke pedig egy 24 órás formátumban.</span><span class="sxs-lookup"><span data-stu-id="dad39-169">hello end or shutdown time is represented with hello **E** attribute, and its value is in a 24-hour format.</span></span>

     <span data-ttu-id="dad39-170">Ha hello **S** és **E** attribútumok minden értéket veheti fel nulla (0), hello virtuális gép marad a jelenlegi állapotában a kiértékelési hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="dad39-170">If hello **S** and **E** attributes each have a value of zero (0), hello virtual machine will be left in its present state at hello time of evaluation.</span></span>
3. <span data-ttu-id="dad39-171">Ha azt szeretné, hogy egy adott napjára hello hét tooskip értékelési, nem szakasz hozzáadása hello hét adott napjaira vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="dad39-171">If you want tooskip evaluation for a specific day of hello week, don’t add a section for that day of hello week.</span></span> <span data-ttu-id="dad39-172">Hello a következő példában csak hétfő kiértékeli, és hello más hello hét napjai figyelmen kívül hagyja:</span><span class="sxs-lookup"><span data-stu-id="dad39-172">In hello following example, only Monday is evaluated, and hello other days of hello week are ignored:</span></span>

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a><span data-ttu-id="dad39-173">Címke erőforráscsoport-sablonok és virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="dad39-173">Tag resource groups or VMs</span></span>
<span data-ttu-id="dad39-174">tooshut le a virtuális gépek, kell tootag hello virtuális gépek vagy a hello erőforráscsoportok, amelyben fontosságúak található.</span><span class="sxs-lookup"><span data-stu-id="dad39-174">tooshut down VMs, you need tootag either hello VMs or hello resource groups in which they're located.</span></span> <span data-ttu-id="dad39-175">A ütemezés címkével nem rendelkező virtuális gépek nem értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="dad39-175">Virtual machines that don't have a Schedule tag are not evaluated.</span></span> <span data-ttu-id="dad39-176">Nem, ezért azok elindítva, vagy állítsa le.</span><span class="sxs-lookup"><span data-stu-id="dad39-176">Therefore, they aren't started or shut down.</span></span>

<span data-ttu-id="dad39-177">Nincsenek két módon tootag erőforráscsoportok vagy virtuális gépek ezen megoldás.</span><span class="sxs-lookup"><span data-stu-id="dad39-177">There are two ways tootag resource groups or VMs with this solution.</span></span> <span data-ttu-id="dad39-178">Azt közvetlenül a hello portálon teheti meg.</span><span class="sxs-lookup"><span data-stu-id="dad39-178">You can do it directly from hello portal.</span></span> <span data-ttu-id="dad39-179">Vagy hello Add-ResourceSchedule, a frissítés-ResourceSchedule és a Remove-ResourceSchedule runbookok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="dad39-179">Or you can use hello Add-ResourceSchedule, Update-ResourceSchedule, and Remove-ResourceSchedule runbooks.</span></span>

### <a name="tag-through-hello-portal"></a><span data-ttu-id="dad39-180">Címke hello portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="dad39-180">Tag through hello portal</span></span>
<span data-ttu-id="dad39-181">Ezen lépések tootag egy virtuális gép vagy erőforráscsoport hello portálon kövesse:</span><span class="sxs-lookup"><span data-stu-id="dad39-181">Follow these steps tootag a virtual machine or resource group in hello portal:</span></span>

1. <span data-ttu-id="dad39-182">Egybesimítására hello JSON karakterláncát, és győződjön meg arról, hogy nincs-e szóközt.</span><span class="sxs-lookup"><span data-stu-id="dad39-182">Flatten hello JSON string and verify that there aren't any spaces.</span></span>  <span data-ttu-id="dad39-183">A JSON karakterláncnak kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="dad39-183">Your JSON string should look like this:</span></span>

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. <span data-ttu-id="dad39-184">Jelölje be hello **címke** ikonja egy virtuális gép vagy az erőforrás csoport tooapply ezt az ütemezést.</span><span class="sxs-lookup"><span data-stu-id="dad39-184">Select hello **Tag** icon for a VM or resource group tooapply this schedule.</span></span>

   ![Virtuálisgép-kód beállítását](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. <span data-ttu-id="dad39-186">Címkék vannak definiálva a következő egy kulcs/érték pár.</span><span class="sxs-lookup"><span data-stu-id="dad39-186">Tags are defined following a key/value pair.</span></span> <span data-ttu-id="dad39-187">Típus **ütemezés** a hello **kulcs** mezőben, és JSON-karakterláncban hello majd beillesztheti hello **érték** mező.</span><span class="sxs-lookup"><span data-stu-id="dad39-187">Type **Schedule** in hello **Key** field, and then paste hello JSON string into hello **Value** field.</span></span> <span data-ttu-id="dad39-188">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="dad39-188">Click **Save**.</span></span> <span data-ttu-id="dad39-189">Az új címke ekkor meg kell jelennie az erőforrás címkék hello listáját.</span><span class="sxs-lookup"><span data-stu-id="dad39-189">Your new tag should now appear in hello list of tags for your resource.</span></span>

   ![Virtuális gép ütemezés címke](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a><span data-ttu-id="dad39-191">PowerShell-címke</span><span class="sxs-lookup"><span data-stu-id="dad39-191">Tag from PowerShell</span></span>
<span data-ttu-id="dad39-192">Minden importált runbookokat hello parancsfájl, amely leírja, hogyan a tooexecute közvetlenül a PowerShell runbookokat hello hello elején súgó-információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="dad39-192">All imported runbooks contain help information at hello beginning of hello script that describes how tooexecute hello runbooks directly from PowerShell.</span></span> <span data-ttu-id="dad39-193">Add-ScheduleResource és frissítés-ScheduleResource runbookokat hello powershellből hívása.</span><span class="sxs-lookup"><span data-stu-id="dad39-193">You can call hello Add-ScheduleResource and Update-ScheduleResource runbooks from PowerShell.</span></span> <span data-ttu-id="dad39-194">Ehhez a virtuális gép vagy az erőforrás-csoporton hello portálon kívül az toocreate vagy frissítés hello ütemezés címkét, amelyek lehetővé teszik a szükséges paraméterek átadása.</span><span class="sxs-lookup"><span data-stu-id="dad39-194">You do this by passing required parameters that enable you toocreate or update hello Schedule tag on a VM or resource group outside of hello portal.</span></span>

<span data-ttu-id="dad39-195">toocreate, adja hozzá, és a PowerShell segítségével címkék törlése, először túl[állítsa be a PowerShell környezetet az Azure-](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dad39-195">toocreate, add, and delete tags through PowerShell, you first need too[set up your PowerShell environment for Azure](/powershell/azure/overview).</span></span> <span data-ttu-id="dad39-196">Hello telepítő befejezése után az alábbi lépésekkel hello lépne.</span><span class="sxs-lookup"><span data-stu-id="dad39-196">After you complete hello setup, you can proceed with hello following steps.</span></span>

### <a name="create-a-schedule-tag-with-powershell"></a><span data-ttu-id="dad39-197">Egy ütemezés címke létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="dad39-197">Create a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="dad39-198">Nyisson meg egy PowerShell-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="dad39-198">Open a PowerShell session.</span></span> <span data-ttu-id="dad39-199">Kövesse a következő példa tooauthenticate a Futtatás mint fiók és toospecify előfizetés hello:</span><span class="sxs-lookup"><span data-stu-id="dad39-199">Then use hello following example tooauthenticate with your Run As account and toospecify a subscription:</span></span>

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="dad39-200">Adjon meg egy ütemezést kivonatoló táblát.</span><span class="sxs-lookup"><span data-stu-id="dad39-200">Define a schedule hash table.</span></span> <span data-ttu-id="dad39-201">Íme egy példa hogyan kell létrehozni:</span><span class="sxs-lookup"><span data-stu-id="dad39-201">Here is an example of how it should be constructed:</span></span>

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. <span data-ttu-id="dad39-202">Hello runbook által igényelt hello paraméterek megadása.</span><span class="sxs-lookup"><span data-stu-id="dad39-202">Define hello parameters that are required by hello runbook.</span></span> <span data-ttu-id="dad39-203">A következő példa hello azt céloz meg egy virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="dad39-203">In hello following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    <span data-ttu-id="dad39-204">Ha egy erőforráscsoport folyamatban címkézést, távolítsa el a hello *VMName* hello $params kivonatoló paramétert tábla az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="dad39-204">If you’re tagging a resource group, remove hello *VMName* parameter from hello $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. <span data-ttu-id="dad39-205">Futtassa a következő paraméterek toocreate hello ütemezés címke hello hello Add-ResourceSchedule runbook:</span><span class="sxs-lookup"><span data-stu-id="dad39-205">Run hello Add-ResourceSchedule runbook with hello following parameters toocreate hello Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. <span data-ttu-id="dad39-206">egy erőforrás csoport vagy a virtuális gép címke tooupdate hello hajtható végre **frissítés-ResourceSchedule** runbook a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="dad39-206">tooupdate a resource group or virtual machine tag, execute hello **Update-ResourceSchedule** runbook with hello following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a><span data-ttu-id="dad39-207">A PowerShell-lel ütemezés címke eltávolítása</span><span class="sxs-lookup"><span data-stu-id="dad39-207">Remove a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="dad39-208">Nyisson meg egy PowerShell-munkamenetet, és futtassa a következő tooauthenticate a Futtatás mint fiók és tooselect hello, és adjon meg egy előfizetési:</span><span class="sxs-lookup"><span data-stu-id="dad39-208">Open a PowerShell session and run hello following tooauthenticate with your Run As account and tooselect and specify a subscription:</span></span>

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="dad39-209">Hello runbook által igényelt hello paraméterek megadása.</span><span class="sxs-lookup"><span data-stu-id="dad39-209">Define hello parameters that are required by hello runbook.</span></span> <span data-ttu-id="dad39-210">A következő példa hello azt céloz meg egy virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="dad39-210">In hello following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    <span data-ttu-id="dad39-211">Ha szeretne eltávolítani egy címkét egy erőforráscsoportból, távolítsa el a hello *VMName* hello $params kivonatoló paramétert tábla az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="dad39-211">If you’re removing a tag from a resource group, remove hello *VMName* parameter from hello $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. <span data-ttu-id="dad39-212">Hello Remove-ResourceSchedule runbook tooremove hello ütemezés címke hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="dad39-212">Execute hello Remove-ResourceSchedule runbook tooremove hello Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. <span data-ttu-id="dad39-213">erőforrás csoport vagy a virtuális gép címkét, tooupdate hello Remove-ResourceSchedule runbook a következő paraméterek hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="dad39-213">tooupdate a resource group or virtual machine tag, execute hello Remove-ResourceSchedule runbook with hello following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> <span data-ttu-id="dad39-214">Azt javasoljuk, hogy proaktív nyomon, hogy ezek a runbookok (és hello virtuális gép állapota) a virtuális gépek éppen tooverify állítsa le, és ennek megfelelően elindult.</span><span class="sxs-lookup"><span data-stu-id="dad39-214">We recommend that you proactively monitor these runbooks (and hello virtual machine states) tooverify that your virtual machines are being shut down and started accordingly.</span></span>
>

<span data-ttu-id="dad39-215">hello Azure-portálon válassza hello tooview hello részletek hello teszt-ResourceSchedule runbook feladat **feladatok** csempe hello runbook.</span><span class="sxs-lookup"><span data-stu-id="dad39-215">tooview hello details of hello Test-ResourceSchedule runbook job in hello Azure portal, select hello **Jobs** tile of hello runbook.</span></span> <span data-ttu-id="dad39-216">hello feladat összegzése hello bemeneti paraméterek megjelenítése és hello kimeneti adatfolyam, továbbá hello feladat toogeneral információ és a kivételek Ha sor került ilyenre.</span><span class="sxs-lookup"><span data-stu-id="dad39-216">hello job summary displays hello input parameters and hello output stream, in addition toogeneral information about hello job and any exceptions if they occurred.</span></span>

<span data-ttu-id="dad39-217">Hello **feladat összegzése** hello kimeneti, figyelmeztető és adatfolyamok üzeneteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dad39-217">hello **Job Summary** includes messages from hello output, warning, and error streams.</span></span> <span data-ttu-id="dad39-218">Jelölje be hello **kimeneti** tooview csempe részletes eredményeinek hello a runbook végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="dad39-218">Select hello **Output** tile tooview detailed results from hello runbook execution.</span></span>

![Teszt-ResourceSchedule kimeneti](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a><span data-ttu-id="dad39-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dad39-220">Next steps</span></span>
* <span data-ttu-id="dad39-221">a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md).</span><span class="sxs-lookup"><span data-stu-id="dad39-221">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md).</span></span>
* <span data-ttu-id="dad39-222">További információ az runbooktípusokkal, és azok előnyeit és -korlátozások toolearn lásd [Azure Automation-runbook típusok](automation-runbook-types.md).</span><span class="sxs-lookup"><span data-stu-id="dad39-222">toolearn more about runbook types, and their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md).</span></span>
* <span data-ttu-id="dad39-223">PowerShell parancsfájl támogatási funkciókkal kapcsolatos további információkért lásd: [natív PowerShell-parancsfájl támogatás az Azure Automationben](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span><span class="sxs-lookup"><span data-stu-id="dad39-223">For more information about PowerShell script support features, see [Native PowerShell script support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span></span>
* <span data-ttu-id="dad39-224">További információ a runbook-naplózás és a kimeneti, toolearn lásd [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="dad39-224">toolearn more about runbook logging and output, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md).</span></span>
* <span data-ttu-id="dad39-225">További információk az Azure-beli futtató fiókot, és hogyan tooauthenticate a runbookok használatával, lásd: toolearn [hitelesítéséhez az Azure-beli futtató fiók runbookok](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="dad39-225">toolearn more about an Azure Run As account and how tooauthenticate your runbooks by using it, see [Authenticate runbooks with Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>
