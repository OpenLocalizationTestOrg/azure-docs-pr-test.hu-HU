---
title: "Az Azure figyelése és a Windows virtuális gépek |} Microsoft Docs"
description: "Az oktatóanyag - figyelő egy Windows rendszerű virtuális gép az Azure PowerShell használatával"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 42a475092bdd615c23154e5f813861b0acefcf29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="06530-103">Az Azure PowerShell használatával Windows virtuális gépek figyelése</span><span class="sxs-lookup"><span data-stu-id="06530-103">Monitor a Windows Virtual Machine with Azure PowerShell</span></span>

<span data-ttu-id="06530-104">Ügynökök Azure figyelés használ a rendszerindító és teljesítményadatokat gyűjteni Azure virtuális gépeken, ezek az adatok tárolása az Azure storage, és lehetővé teszi a portál, az Azure PowerShell modul és az Azure parancssori felület keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="06530-104">Azure monitoring uses agents to collect boot and performance data from Azure VMs, store this data in Azure storage, and make it accessible through portal, the Azure PowerShell module, and the Azure CLI.</span></span> <span data-ttu-id="06530-105">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="06530-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="06530-106">A virtuális gép rendszerindítási diagnosztika engedélyezése</span><span class="sxs-lookup"><span data-stu-id="06530-106">Enable boot diagnostics on a VM</span></span>
> * <span data-ttu-id="06530-107">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="06530-107">View boot diagnostics</span></span>
> * <span data-ttu-id="06530-108">Virtuális gép gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="06530-108">View VM host metrics</span></span>
> * <span data-ttu-id="06530-109">A diagnosztika-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="06530-109">Install the diagnostics extension</span></span>
> * <span data-ttu-id="06530-110">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="06530-110">View VM metrics</span></span>
> * <span data-ttu-id="06530-111">Riasztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="06530-111">Create an alert</span></span>
> * <span data-ttu-id="06530-112">Speciális figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="06530-112">Set up advanced monitoring</span></span>

<span data-ttu-id="06530-113">Az oktatóanyaghoz az Azure PowerShell-modul 3.6-os vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="06530-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="06530-114">A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="06530-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="06530-115">Ha frissítenie kell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="06530-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="06530-116">A példa az oktatóanyag elvégzéséhez rendelkeznie kell egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="06530-116">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="06530-117">Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="06530-117">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="06530-118">Az oktatóanyag lépéseinek használatakor, cserélje ki az erőforráscsoportot, a virtuális gép nevét és a helyet, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="06530-118">When working through the tutorial, replace the resource group, VM name, and location where needed.</span></span>

## <a name="view-boot-diagnostics"></a><span data-ttu-id="06530-119">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="06530-119">View boot diagnostics</span></span>

<span data-ttu-id="06530-120">Indítsa el a Windows virtuális gépek, a rendszerindítási diagnosztikai ügynök rögzíti a képernyő kimeneti hibaelhárítási céllal használható.</span><span class="sxs-lookup"><span data-stu-id="06530-120">As Windows virtual machines boot up, the boot diagnostic agent captures screen output that can be used for troubleshooting purpose.</span></span> <span data-ttu-id="06530-121">Ez a funkció alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="06530-121">This capability is enabled by default.</span></span> <span data-ttu-id="06530-122">A rögzített képernyőképek tárolódnak az Azure storage-fiók, amely alapértelmezés szerint is létrejön.</span><span class="sxs-lookup"><span data-stu-id="06530-122">The captured screen shots are stored in an Azure storage account, which is also created by default.</span></span> 

<span data-ttu-id="06530-123">A rendszerindítási diagnosztikai adatok kaphat a [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) parancsot.</span><span class="sxs-lookup"><span data-stu-id="06530-123">You can get the boot diagnostic data with the [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) command.</span></span> <span data-ttu-id="06530-124">A következő példában a rendszerindítási diagnosztika gyökerébe letöltődnek a * c:\* meghajtó.</span><span class="sxs-lookup"><span data-stu-id="06530-124">In the following example, boot diagnostics are downloaded to the root of the *c:\* drive.</span></span> 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a><span data-ttu-id="06530-125">Gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="06530-125">View host metrics</span></span>

<span data-ttu-id="06530-126">Egy Windows virtuális gép a gazdagép dedikált virtuális gépek rendelkezik, amely hatással van az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="06530-126">A Windows VM has a dedicated Host VM in Azure that it interacts with.</span></span> <span data-ttu-id="06530-127">Metrikák automatikusan összegyűjtött ahhoz, hogy a gazdagép és az Azure portálon is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="06530-127">Metrics are automatically collected for the Host and can be viewed in the Azure portal.</span></span>

1. <span data-ttu-id="06530-128">Az Azure portálon kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** erőforrás listájában.</span><span class="sxs-lookup"><span data-stu-id="06530-128">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="06530-129">Kattintson a **metrikák** a virtuális gép panelen, majd válassza ki a gazdagép-metrikák bármelyikét **elérhető** tekintheti meg, hogyan működik-e a gazdagép virtuális.</span><span class="sxs-lookup"><span data-stu-id="06530-129">Click **Metrics** on the VM blade, and then select any of the Host metrics under **Available metrics** to see how the Host VM is performing.</span></span>

    ![Gazdagép-metrikák megtekintése](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a><span data-ttu-id="06530-131">Diagnosztika-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="06530-131">Install diagnostics extension</span></span>

<span data-ttu-id="06530-132">Az alapvető állomás adatok gyűjtése le elérhető, de a részletesebb és Virtuálisgép-specifikus metrika, meg kell telepítenie az Azure diagnostics bővítményt a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="06530-132">The basic host metrics are available, but to see more granular and VM-specific metrics, you to need to install the Azure diagnostics extension on the VM.</span></span> <span data-ttu-id="06530-133">Az Azure diagnostics bővítmény lehetővé teszi, hogy további figyelési és diagnosztikai adatokat beolvasni a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="06530-133">The Azure diagnostics extension allows additional monitoring and diagnostics data to be retrieved from the VM.</span></span> <span data-ttu-id="06530-134">Megtekintheti a metrikák és riasztások alapján hogyan hajtja végre a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="06530-134">You can view these performance metrics and create alerts based on how the VM performs.</span></span> <span data-ttu-id="06530-135">A diagnosztikai bővítmény telepítve van az Azure portálon keresztül az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="06530-135">The diagnostic extension is installed through the Azure portal as follows:</span></span>

1. <span data-ttu-id="06530-136">Az Azure portálon kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** erőforrás listájában.</span><span class="sxs-lookup"><span data-stu-id="06530-136">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="06530-137">Kattintson a **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="06530-137">Click **Diagnosis settings**.</span></span> <span data-ttu-id="06530-138">A lista azt mutatja, hogy *rendszerindítási diagnosztika* már engedélyezve van az előző szakaszából.</span><span class="sxs-lookup"><span data-stu-id="06530-138">The list shows that *Boot diagnostics* are already enabled from the previous section.</span></span> <span data-ttu-id="06530-139">Jelölje be a jelölőnégyzetet a *alapvető metrikák*.</span><span class="sxs-lookup"><span data-stu-id="06530-139">Click the check box for *Basic metrics*.</span></span>
3. <span data-ttu-id="06530-140">Kattintson a **vendégszintű a figyelés bekapcsolható** gombra.</span><span class="sxs-lookup"><span data-stu-id="06530-140">Click the **Enable guest-level monitoring** button.</span></span>

    ![Nézet diagnosztikai metrikák](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a><span data-ttu-id="06530-142">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="06530-142">View VM metrics</span></span>

<span data-ttu-id="06530-143">A virtuális gép mérni, hogy megtekinthetők-e a gazdagép VM metrikák azonos módon tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="06530-143">You can view the VM metrics in the same way that you viewed the host VM metrics:</span></span>

1. <span data-ttu-id="06530-144">Az Azure portálon kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** erőforrás listájában.</span><span class="sxs-lookup"><span data-stu-id="06530-144">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="06530-145">Hogyan működik-e a virtuális gép megtekintéséhez kattintson **metrikák** a virtuális gép panelen, majd válassza ki a diagnosztika mérőszámok alapján bármelyikét **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="06530-145">To see how the VM is performing, click **Metrics** on the VM blade, and then select any of the diagnostics metrics under **Available metrics**.</span></span>

    ![Nézet VM metrikák](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a><span data-ttu-id="06530-147">Riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="06530-147">Create alerts</span></span>

<span data-ttu-id="06530-148">Riasztások adott mérőszámok alapján hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="06530-148">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="06530-149">Riasztások értesíti, amikor az átlagos CPU-használat meghaladja az egy bizonyos küszöb vagy a rendelkezésre álló szabad lemezterületet alá csökken egy adott értékre, például használható.</span><span class="sxs-lookup"><span data-stu-id="06530-149">Alerts can be used to notify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="06530-150">Riasztások jelennek meg az Azure portálon, vagy e-mailben küldhetők el.</span><span class="sxs-lookup"><span data-stu-id="06530-150">Alerts are displayed in the Azure portal or can be sent via email.</span></span> <span data-ttu-id="06530-151">Riasztás generálása Azure Automation-forgatókönyveket vagy Azure Logic Apps is elindítható.</span><span class="sxs-lookup"><span data-stu-id="06530-151">You can also trigger Azure Automation runbooks or Azure Logic Apps in response to alerts being generated.</span></span>

<span data-ttu-id="06530-152">A következő példa az átlagos processzorhasználat riasztást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="06530-152">The following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="06530-153">Az Azure portálon kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** erőforrás listájában.</span><span class="sxs-lookup"><span data-stu-id="06530-153">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="06530-154">Kattintson a **riasztási szabályok** virtuális gép paneljén kattintson a **metrika riasztás hozzáadása** a riasztások panel tetején.</span><span class="sxs-lookup"><span data-stu-id="06530-154">Click **Alert rules** on the VM blade, then click **Add metric alert** across the top of the alerts blade.</span></span>
4. <span data-ttu-id="06530-155">Adjon meg egy **neve** a riasztás például *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="06530-155">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="06530-156">Riasztást vált ki, ha processzor 1.0 meghaladja 5 percig, hagyja a többi alapértelmezett kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="06530-156">To trigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all the other defaults selected.</span></span>
6. <span data-ttu-id="06530-157">Szükség esetén jelölje be a *E-mail-tulajdonosok, közreműködőknek és olvasóknak* e-mail értesítést küldeni.</span><span class="sxs-lookup"><span data-stu-id="06530-157">Optionally, check the box for *Email owners, contributors, and readers* to send email notification.</span></span> <span data-ttu-id="06530-158">Az alapértelmezett művelet is értesítést megjeleníteni a portálon.</span><span class="sxs-lookup"><span data-stu-id="06530-158">The default action is to present a notification in the portal.</span></span>
7. <span data-ttu-id="06530-159">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="06530-159">Click the **OK** button.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="06530-160">Speciális figyelés</span><span class="sxs-lookup"><span data-stu-id="06530-160">Advanced monitoring</span></span> 

<span data-ttu-id="06530-161">Fejlettebb, figyelés, a virtuális gép segítségével teheti [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="06530-161">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="06530-162">Ha még nem tette meg, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) az Operations Management Suite szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="06530-162">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="06530-163">Ha rendelkezik az OMS-portállal, találja a kulcsát és a munkaterület azonosítóját a beállítások panelen.</span><span class="sxs-lookup"><span data-stu-id="06530-163">When you have access to the OMS portal, you can find the workspace key and workspace identifier on the Settings blade.</span></span> <span data-ttu-id="06530-164">Használja a [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand gombra az OMS-bővítmény hozzáadása a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="06530-164">Use the [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand to to add the OMS extension to the VM.</span></span> <span data-ttu-id="06530-165">Frissíti a változó értékét az alábbi minta megfelelően, OMS-munkaterület kulcs és a munkaterület azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="06530-165">Update the variable values in the below sample to reflect you OMS workspace key and workspace Id.</span></span>  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

<span data-ttu-id="06530-166">Néhány perc múlva megtekintheti az új virtuális Gépet az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="06530-166">After a few minutes, you should see the new VM in the OMS workspace.</span></span> 

![OMS panel](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="06530-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06530-168">Next steps</span></span>
<span data-ttu-id="06530-169">Ebben az oktatóanyagban konfigurálva, és tekintse át a virtuális gépek az Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="06530-169">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="06530-170">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="06530-170">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="06530-171">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="06530-171">Create a virtual network</span></span>
> * <span data-ttu-id="06530-172">Egy erőforráscsoport és a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="06530-172">Create a resource group and VM</span></span> 
> * <span data-ttu-id="06530-173">Rendszerindítási diagnosztika a virtuális Gépre engedélyezése</span><span class="sxs-lookup"><span data-stu-id="06530-173">Enable boot diagnostics on the VM</span></span>
> * <span data-ttu-id="06530-174">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="06530-174">View boot diagnostics</span></span>
> * <span data-ttu-id="06530-175">Gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="06530-175">View host metrics</span></span>
> * <span data-ttu-id="06530-176">A diagnosztika-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="06530-176">Install the diagnostics extension</span></span>
> * <span data-ttu-id="06530-177">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="06530-177">View VM metrics</span></span>
> * <span data-ttu-id="06530-178">Riasztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="06530-178">Create an alert</span></span>
> * <span data-ttu-id="06530-179">Speciális figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="06530-179">Set up advanced monitoring</span></span>

<span data-ttu-id="06530-180">A következő oktatóanyag az Azure security Centerrel kapcsolatos további továbblépés.</span><span class="sxs-lookup"><span data-stu-id="06530-180">Advance to the next tutorial to learn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="06530-181">A virtuális gépek biztonságának kezelése</span><span class="sxs-lookup"><span data-stu-id="06530-181">Manage VM security</span></span>](./tutorial-azure-security.md)