---
title: "aaaAzure figyelés és a Windows virtuális gépek |} Microsoft Docs"
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
ms.openlocfilehash: 191dc5a30d41c25a9e38f8ec2a32efdc05e03015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="50832-103">Az Azure PowerShell használatával Windows virtuális gépek figyelése</span><span class="sxs-lookup"><span data-stu-id="50832-103">Monitor a Windows Virtual Machine with Azure PowerShell</span></span>

<span data-ttu-id="50832-104">Azure figyelési ügynökök toocollect rendszerindító és Azure virtuális gépeken, a teljesítményadatokat az Azure storage tárolja ezeket az adatokat, és tegye elérhetővé a portálon, a hello Azure PowerShell modul és a hello Azure CLI segítségével az.</span><span class="sxs-lookup"><span data-stu-id="50832-104">Azure monitoring uses agents toocollect boot and performance data from Azure VMs, store this data in Azure storage, and make it accessible through portal, hello Azure PowerShell module, and hello Azure CLI.</span></span> <span data-ttu-id="50832-105">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="50832-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="50832-106">A virtuális gép rendszerindítási diagnosztika engedélyezése</span><span class="sxs-lookup"><span data-stu-id="50832-106">Enable boot diagnostics on a VM</span></span>
> * <span data-ttu-id="50832-107">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="50832-107">View boot diagnostics</span></span>
> * <span data-ttu-id="50832-108">Virtuális gép gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="50832-108">View VM host metrics</span></span>
> * <span data-ttu-id="50832-109">Hello diagnosztika-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="50832-109">Install hello diagnostics extension</span></span>
> * <span data-ttu-id="50832-110">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="50832-110">View VM metrics</span></span>
> * <span data-ttu-id="50832-111">Riasztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="50832-111">Create an alert</span></span>
> * <span data-ttu-id="50832-112">Speciális figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="50832-112">Set up advanced monitoring</span></span>

<span data-ttu-id="50832-113">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="50832-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="50832-114">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="50832-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="50832-115">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="50832-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="50832-116">Ebben az oktatóanyagban toocomplete hello példában rendelkeznie kell egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="50832-116">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="50832-117">Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="50832-117">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="50832-118">Ha hello oktatóanyag dolgozik, cserélje le a hello erőforráscsoportot, a virtuális gép nevét és a helyet, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="50832-118">When working through hello tutorial, replace hello resource group, VM name, and location where needed.</span></span>

## <a name="view-boot-diagnostics"></a><span data-ttu-id="50832-119">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="50832-119">View boot diagnostics</span></span>

<span data-ttu-id="50832-120">Indítsa el a Windows virtuális gépek, mivel hello rendszerindítási diagnosztikai ügynök rögzíti a képernyő kimeneti hibaelhárítási céllal használható.</span><span class="sxs-lookup"><span data-stu-id="50832-120">As Windows virtual machines boot up, hello boot diagnostic agent captures screen output that can be used for troubleshooting purpose.</span></span> <span data-ttu-id="50832-121">Ez a funkció alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="50832-121">This capability is enabled by default.</span></span> <span data-ttu-id="50832-122">hello rögzítése a képernyő pillanatfelvételek tárolódnak az Azure storage-fiók, amely alapértelmezés szerint is létrejön.</span><span class="sxs-lookup"><span data-stu-id="50832-122">hello captured screen shots are stored in an Azure storage account, which is also created by default.</span></span> 

<span data-ttu-id="50832-123">Hello rendszerindítási diagnosztikai adatok hello kaphat [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) parancsot.</span><span class="sxs-lookup"><span data-stu-id="50832-123">You can get hello boot diagnostic data with hello [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) command.</span></span> <span data-ttu-id="50832-124">A következő példa hello, rendszerindítási diagnosztika hello letöltött toohello gyökerébe * c:\* meghajtó.</span><span class="sxs-lookup"><span data-stu-id="50832-124">In hello following example, boot diagnostics are downloaded toohello root of hello *c:\* drive.</span></span> 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a><span data-ttu-id="50832-125">Gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="50832-125">View host metrics</span></span>

<span data-ttu-id="50832-126">Egy Windows virtuális gép a gazdagép dedikált virtuális gépek rendelkezik, amely hatással van az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="50832-126">A Windows VM has a dedicated Host VM in Azure that it interacts with.</span></span> <span data-ttu-id="50832-127">Metrikák automatikusan gyűjtendő hello állomás, és megtekinthetők az hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="50832-127">Metrics are automatically collected for hello Host and can be viewed in hello Azure portal.</span></span>

1. <span data-ttu-id="50832-128">Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="50832-128">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="50832-129">Kattintson a **metrikák** a hello VM panelen, majd válassza ki bármelyik hello gazdagép metrikák alapján **elérhető** toosee hogyan hello gazdagép VM hajt végre.</span><span class="sxs-lookup"><span data-stu-id="50832-129">Click **Metrics** on hello VM blade, and then select any of hello Host metrics under **Available metrics** toosee how hello Host VM is performing.</span></span>

    ![Gazdagép-metrikák megtekintése](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a><span data-ttu-id="50832-131">Diagnosztika-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="50832-131">Install diagnostics extension</span></span>

<span data-ttu-id="50832-132">hello alapvető gazdagép metrikák érhetők el, de a részletesebb toosee, és a Virtuálisgép-specifikus metrika, akkor tooneed tooinstall hello Azure diagnostics bővítményt a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="50832-132">hello basic host metrics are available, but toosee more granular and VM-specific metrics, you tooneed tooinstall hello Azure diagnostics extension on hello VM.</span></span> <span data-ttu-id="50832-133">hello Azure diagnostics bővítmény lehetővé teszi, hogy további felügyeleti és diagnosztikai adatok toobe hello VM lekért.</span><span class="sxs-lookup"><span data-stu-id="50832-133">hello Azure diagnostics extension allows additional monitoring and diagnostics data toobe retrieved from hello VM.</span></span> <span data-ttu-id="50832-134">A metrikák megtekintheti, és hozzon létre a riasztások alapján hogyan hello VM hajt végre.</span><span class="sxs-lookup"><span data-stu-id="50832-134">You can view these performance metrics and create alerts based on how hello VM performs.</span></span> <span data-ttu-id="50832-135">hello diagnosztikai telepítve van a bővítmény hello Azure-portálon keresztül az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="50832-135">hello diagnostic extension is installed through hello Azure portal as follows:</span></span>

1. <span data-ttu-id="50832-136">Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="50832-136">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="50832-137">Kattintson a **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="50832-137">Click **Diagnosis settings**.</span></span> <span data-ttu-id="50832-138">hello lista azt mutatja, hogy *rendszerindítási diagnosztika* már engedélyezve van az előző szakasz hello.</span><span class="sxs-lookup"><span data-stu-id="50832-138">hello list shows that *Boot diagnostics* are already enabled from hello previous section.</span></span> <span data-ttu-id="50832-139">Kattintson a megfelelő jelölőnégyzet hello *alapvető metrikák*.</span><span class="sxs-lookup"><span data-stu-id="50832-139">Click hello check box for *Basic metrics*.</span></span>
3. <span data-ttu-id="50832-140">Kattintson a hello **vendégszintű a figyelés bekapcsolható** gombra.</span><span class="sxs-lookup"><span data-stu-id="50832-140">Click hello **Enable guest-level monitoring** button.</span></span>

    ![Nézet diagnosztikai metrikák](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a><span data-ttu-id="50832-142">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="50832-142">View VM metrics</span></span>

<span data-ttu-id="50832-143">Hello VM metrikák hello megtekintheti, hogy megtekinthetők-e hello gazdagép VM metrikák ugyanúgy:</span><span class="sxs-lookup"><span data-stu-id="50832-143">You can view hello VM metrics in hello same way that you viewed hello host VM metrics:</span></span>

1. <span data-ttu-id="50832-144">Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="50832-144">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="50832-145">toosee hogyan hello VM hajt végre, kattintson a **metrikák** a hello VM panelen, majd válassza ki bármelyik hello diagnosztika mérőszámok alapján **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="50832-145">toosee how hello VM is performing, click **Metrics** on hello VM blade, and then select any of hello diagnostics metrics under **Available metrics**.</span></span>

    ![Nézet VM metrikák](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a><span data-ttu-id="50832-147">Riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="50832-147">Create alerts</span></span>

<span data-ttu-id="50832-148">Riasztások adott mérőszámok alapján hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="50832-148">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="50832-149">Riasztás például akkor, amikor az átlagos CPU-használat meghaladja az egy bizonyos küszöb vagy a rendelkezésre álló szabad lemezterületet egy adott értékre, alá süllyed használt toonotify.</span><span class="sxs-lookup"><span data-stu-id="50832-149">Alerts can be used toonotify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="50832-150">Riasztások jelennek meg hello Azure-portálon, vagy e-mailben küldhetők el.</span><span class="sxs-lookup"><span data-stu-id="50832-150">Alerts are displayed in hello Azure portal or can be sent via email.</span></span> <span data-ttu-id="50832-151">A válasz tooalerts generálása Azure Automation-forgatókönyveket vagy Azure Logic Apps is kiválthatják.</span><span class="sxs-lookup"><span data-stu-id="50832-151">You can also trigger Azure Automation runbooks or Azure Logic Apps in response tooalerts being generated.</span></span>

<span data-ttu-id="50832-152">hello alábbi példa riasztást hoz létre az átlagos CPU-használat.</span><span class="sxs-lookup"><span data-stu-id="50832-152">hello following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="50832-153">Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="50832-153">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="50832-154">Kattintson a **riasztási szabályok** hello VM panelen, majd kattintson a **metrika riasztás hozzáadása** keresztül hello hello riasztások panel tetején.</span><span class="sxs-lookup"><span data-stu-id="50832-154">Click **Alert rules** on hello VM blade, then click **Add metric alert** across hello top of hello alerts blade.</span></span>
4. <span data-ttu-id="50832-155">Adjon meg egy **neve** a riasztás például *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="50832-155">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="50832-156">tootrigger riasztást, amikor a processzor 1.0 meghaladja 5 percig, hagyja összes hello többi kijelölt alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="50832-156">tootrigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all hello other defaults selected.</span></span>
6. <span data-ttu-id="50832-157">Szükség esetén jelölje be a hello jelölőnégyzetet *E-mail-tulajdonosok, közreműködőknek és olvasóknak* toosend e-mailben értesítést.</span><span class="sxs-lookup"><span data-stu-id="50832-157">Optionally, check hello box for *Email owners, contributors, and readers* toosend email notification.</span></span> <span data-ttu-id="50832-158">hello alapértelmezett művelete toopresent hello portálon értesítést.</span><span class="sxs-lookup"><span data-stu-id="50832-158">hello default action is toopresent a notification in hello portal.</span></span>
7. <span data-ttu-id="50832-159">Kattintson a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="50832-159">Click hello **OK** button.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="50832-160">Speciális figyelés</span><span class="sxs-lookup"><span data-stu-id="50832-160">Advanced monitoring</span></span> 

<span data-ttu-id="50832-161">Fejlettebb, figyelés, a virtuális gép segítségével teheti [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="50832-161">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="50832-162">Ha még nem tette meg, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) az Operations Management Suite szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="50832-162">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="50832-163">Ha a hozzáférési toohello OMS-portálon, hello munkaterület kulcs és a munkaterület-azonosítót a hello-beállítások panelen található.</span><span class="sxs-lookup"><span data-stu-id="50832-163">When you have access toohello OMS portal, you can find hello workspace key and workspace identifier on hello Settings blade.</span></span> <span data-ttu-id="50832-164">Használjon hello [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand tootooadd hello OMS bővítmény toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="50832-164">Use hello [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand tootooadd hello OMS extension toohello VM.</span></span> <span data-ttu-id="50832-165">Frissítés hello változó értékekkel az alábbi minta tooreflect hello OMS-munkaterület kulcs és a munkaterület azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="50832-165">Update hello variable values in hello below sample tooreflect you OMS workspace key and workspace Id.</span></span>  

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

<span data-ttu-id="50832-166">Néhány perc múlva megtekintheti az új virtuális gép hello OMS-munkaterület hello.</span><span class="sxs-lookup"><span data-stu-id="50832-166">After a few minutes, you should see hello new VM in hello OMS workspace.</span></span> 

![OMS panel](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="50832-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="50832-168">Next steps</span></span>
<span data-ttu-id="50832-169">Ebben az oktatóanyagban konfigurálva, és tekintse át a virtuális gépek az Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="50832-169">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="50832-170">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="50832-170">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="50832-171">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="50832-171">Create a virtual network</span></span>
> * <span data-ttu-id="50832-172">Egy erőforráscsoport és a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="50832-172">Create a resource group and VM</span></span> 
> * <span data-ttu-id="50832-173">Rendszerindítási diagnosztika a virtuális gép hello engedélyezése</span><span class="sxs-lookup"><span data-stu-id="50832-173">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="50832-174">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="50832-174">View boot diagnostics</span></span>
> * <span data-ttu-id="50832-175">Gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="50832-175">View host metrics</span></span>
> * <span data-ttu-id="50832-176">Hello diagnosztika-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="50832-176">Install hello diagnostics extension</span></span>
> * <span data-ttu-id="50832-177">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="50832-177">View VM metrics</span></span>
> * <span data-ttu-id="50832-178">Riasztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="50832-178">Create an alert</span></span>
> * <span data-ttu-id="50832-179">Speciális figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="50832-179">Set up advanced monitoring</span></span>

<span data-ttu-id="50832-180">Az Azure security Centerrel kapcsolatos útmutató toolearn következő toohello előzetes.</span><span class="sxs-lookup"><span data-stu-id="50832-180">Advance toohello next tutorial toolearn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="50832-181">A virtuális gépek biztonságának kezelése</span><span class="sxs-lookup"><span data-stu-id="50832-181">Manage VM security</span></span>](./tutorial-azure-security.md)