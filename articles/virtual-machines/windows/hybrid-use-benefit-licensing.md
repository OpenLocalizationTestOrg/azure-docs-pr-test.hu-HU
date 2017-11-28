---
title: "Hibrid használata juttatása Windows Server és a Windows ügyfél aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan toomaximize a Windows Software Assurance előnyöket toobring helyszíni tooAzure licencek"
services: virtual-machines-windows
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 5/26/2017
ms.author: xujing
ms.openlocfilehash: f24487320a60132aaf766a31f3e6f3726d4a3bd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-hybrid-use-benefit-for-windows-server-and-windows-client"></a><span data-ttu-id="c1673-103">Azure hibrid használata juttatása Windows Server és a Windows ügyfél</span><span class="sxs-lookup"><span data-stu-id="c1673-103">Azure Hybrid Use Benefit for Windows Server and Windows Client</span></span>
<span data-ttu-id="c1673-104">Software Assurance rendelkező ügyfelek, Azure hibrid használata juttatás lehetővé teszi toouse a helyszíni Windows Server és a Windows Client licencek és futtassa a Windows virtuális gépek Azure költséghatékony.</span><span class="sxs-lookup"><span data-stu-id="c1673-104">For customers with Software Assurance, Azure Hybrid Use Benefit allows you toouse your on-premises Windows Server and Windows Client licenses and run Windows virtual machines in Azure at a reduced cost.</span></span> <span data-ttu-id="c1673-105">Azure hibrid használata juttatás for Windows Server magában foglalja a Windows Server 2008R2, a Windows Server 2012, a Windows Server 2012 R2 rendszerben és a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="c1673-105">Azure Hybrid Use Benefit for Windows Server includes Windows Server 2008R2, Windows Server 2012, Windows Server 2012R2, and Windows Server 2016.</span></span> <span data-ttu-id="c1673-106">Azure hibrid használata előnye a Windows-ügyfél a Windows 10 tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1673-106">Azure Hybrid Use Benefit for Windows Client includes Windows 10.</span></span> <span data-ttu-id="c1673-107">További információkért lásd: hello [Azure hibrid használata juttatás licencelési oldal](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span><span class="sxs-lookup"><span data-stu-id="c1673-107">For more information, please see hello [Azure Hybrid Use Benefit licensing page](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span></span>

>[!IMPORTANT]
><span data-ttu-id="c1673-108">Az Azure hibrid használata előnyeit a Windows ügyfél jelenleg előzetes hello Azure piactér hello Windows 10-lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="c1673-108">Azure Hybrid Use Benefits for Windows Client is currently in Preview using hello Windows 10 image in hello Azure Marketplace.</span></span> <span data-ttu-id="c1673-109">Csak a vállalati felhasználók a Windows 10 nagyvállalati E3/E5 egyes felhasználókra vagy a Windows VDA (bővítmény felhasználói előfizetési licencet vagy előfizetési licenceket) felhasználónkénti ("jogosult licencek") jogosultak.</span><span class="sxs-lookup"><span data-stu-id="c1673-109">Only Enterprise customers with Windows 10 Enterprise E3/E5 per user or Windows VDA per user (User Subscription Licenses or Add-on User Subscription Licenses) (“Qualifying Licenses”) are eligible.</span></span>
>
>

## <a name="ways-toouse-azure-hybrid-use-benefit"></a><span data-ttu-id="c1673-110">Többféleképpen toouse Azure hibrid használata juttatás</span><span class="sxs-lookup"><span data-stu-id="c1673-110">Ways toouse Azure Hybrid Use Benefit</span></span>
<span data-ttu-id="c1673-111">Többféle különböző módokon toodeploy Windows virtuális gépek a hello Azure hibrid használata előnye:</span><span class="sxs-lookup"><span data-stu-id="c1673-111">There are a couple of different ways toodeploy Windows VMs with hello Azure Hybrid Use Benefit:</span></span>

1. <span data-ttu-id="c1673-112">A virtuális gépek telepítése [adott piactéren elérhető rendszerkép](#deploy-a-vm-using-the-azure-marketplace) , amelyek az Azure hibrid használata juttatás – Windows Server 2016-os, a Windows Server 2012 R2 rendszerben, a Windows Server 2012 és Windows Server 2008SP1 előre konfigurált.</span><span class="sxs-lookup"><span data-stu-id="c1673-112">You can deploy VMs from [specific Marketplace images](#deploy-a-vm-using-the-azure-marketplace) that are pre-configured with Azure Hybrid Use Benefit - Windows Server 2016, Windows Server 2012R2, Windows Server 2012 and Windows Server 2008SP1.</span></span>
2. <span data-ttu-id="c1673-113">Is [feltöltése egy egyéni VM](#upload-a-windows-vhd) és [telepítése a Resource Manager-sablon használatával](#deploy-a-vm-via-resource-manager) vagy [Azure PowerShell](#detailed-powershell-deployment-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="c1673-113">You can [upload a custom VM](#upload-a-windows-vhd) and [deploy using a Resource Manager template](#deploy-a-vm-via-resource-manager) or [Azure PowerShell](#detailed-powershell-deployment-walkthrough).</span></span>

## <a name="deploy-a-vm-using-hello-azure-marketplace"></a><span data-ttu-id="c1673-114">A virtuális gépek hello Azure piactér telepítése</span><span class="sxs-lookup"><span data-stu-id="c1673-114">Deploy a VM using hello Azure Marketplace</span></span>
<span data-ttu-id="c1673-115">Következő lemezképek találhatók hello Azure hibrid használata juttatás előre konfiguráltan piactér: Windows Server 2016-ot, a Windows Server 2012 R2 rendszerben, a Windows Server 2012 és Windows Server 2008SP1.</span><span class="sxs-lookup"><span data-stu-id="c1673-115">Following images are available in hello Marketplace pre-configured with Azure Hybrid Use Benefit: Windows Server 2016, Windows Server 2012R2, Windows Server 2012 and Windows Server 2008SP1.</span></span> <span data-ttu-id="c1673-116">Ezeket a lemezképeket is telepíthető közvetlenül a hello Azure-portálon, a Resource Manager-sablonok vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c1673-116">These images can be deployed directly from hello Azure portal, Resource Manager templates, or Azure PowerShell.</span></span>

<span data-ttu-id="c1673-117">Ezeket a lemezképeket, közvetlenül a hello Azure-portál telepítése.</span><span class="sxs-lookup"><span data-stu-id="c1673-117">You can deploy these images directly from hello Azure portal.</span></span> <span data-ttu-id="c1673-118">A Resource Manager-sablonok és az Azure PowerShell használatra hello lista megtekintése a képek az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c1673-118">For use in Resource Manager templates and with Azure PowerShell, view hello list of images as follows:</span></span>

<span data-ttu-id="c1673-119">A Windows Server:</span><span class="sxs-lookup"><span data-stu-id="c1673-119">For Windows Server:</span></span>
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
- <span data-ttu-id="c1673-120">2016-Datacenter verziót 2016.127.20170406 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="c1673-120">2016-Datacenter version 2016.127.20170406 or above</span></span>

- <span data-ttu-id="c1673-121">4.127.20170406 2012-R2 – Datacenter verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="c1673-121">2012-R2-Datacenter version 4.127.20170406 or above</span></span>

- <span data-ttu-id="c1673-122">3.127.20170406 2012 – Datacenter verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="c1673-122">2012-Datacenter version 3.127.20170406 or above</span></span>

- <span data-ttu-id="c1673-123">2.127.20170406 2008 R2-SP1 verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="c1673-123">2008-R2-SP1 version 2.127.20170406 or above</span></span>

<span data-ttu-id="c1673-124">Windows-ügyfél:</span><span class="sxs-lookup"><span data-stu-id="c1673-124">For Windows Client:</span></span>
```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "Windows-HUB"
```

## <a name="upload-a-windows-server-vhd"></a><span data-ttu-id="c1673-125">A Windows Server VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="c1673-125">Upload a Windows Server VHD</span></span>
<span data-ttu-id="c1673-126">egy Windows Server virtuális Gépet az Azure-ban toodeploy, először toocreate virtuális Merevlemezt, amely tartalmazza az alap Windows-buildet.</span><span class="sxs-lookup"><span data-stu-id="c1673-126">toodeploy a Windows Server VM in Azure, you first need toocreate a VHD that contains your base Windows build.</span></span> <span data-ttu-id="c1673-127">Ehhez a virtuális Merevlemezhez elő kell megfelelően készíteni Sysprep feltöltés előtt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c1673-127">This VHD must be appropriately prepared via Sysprep before you upload it tooAzure.</span></span> <span data-ttu-id="c1673-128">Is [további hello VHD követelményeiről és a Sysprep folyamat](upload-generalized-managed.md) és [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="c1673-128">You can [read more about hello VHD requirements and Sysprep process](upload-generalized-managed.md) and [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span> <span data-ttu-id="c1673-129">Készítsen biztonsági másolatot hello VM a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="c1673-129">Back up hello VM before running Sysprep.</span></span> 

<span data-ttu-id="c1673-130">Győződjön meg arról, hogy [telepített és konfigurált hello legújabb Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c1673-130">Make sure you have [installed and configured hello latest Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="c1673-131">Miután előkészítette a VHD-t, töltse fel a hello VHD tooyour Azure Storage-fiókra hello `Add-AzureRmVhd` parancsmag az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c1673-131">Once you have prepared your VHD, upload hello VHD tooyour Azure Storage account using hello `Add-AzureRmVhd` cmdlet as follows:</span></span>

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> <span data-ttu-id="c1673-132">Microsoft SQL Server, SharePoint Server és Dynamics is használhatja a frissítési garancia licencelési.</span><span class="sxs-lookup"><span data-stu-id="c1673-132">Microsoft SQL Server, SharePoint Server, and Dynamics can also utilize your Software Assurance licensing.</span></span> <span data-ttu-id="c1673-133">Szüksége van az tooprepare hello Windows kiszolgálói lemezképet az alkalmazás-összetevők telepítése és ennek megfelelően biztosító licenckulcsot, akkor hello lemez kép tooAzure feltöltése.</span><span class="sxs-lookup"><span data-stu-id="c1673-133">You still need tooprepare hello Windows Server image by installing your application components and providing license keys accordingly, then uploading hello disk image tooAzure.</span></span> <span data-ttu-id="c1673-134">Tekintse át a Sysprep fut, mint az alkalmazás megfelelő dokumentációjában hello [telepítése az SQL Server Sysprep használatával szempontjai](https://msdn.microsoft.com/library/ee210754.aspx) vagy [a SharePoint Server 2016 referencia lemezkép (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1673-134">Review hello appropriate documentation for running Sysprep with your application, such as [Considerations for Installing SQL Server using Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) or [Build a SharePoint Server 2016 Reference Image (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).</span></span>
>
>

<span data-ttu-id="c1673-135">Akkor is olvashat további tudnivalókat [hello VHD tooAzure folyamat feltöltése](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)</span><span class="sxs-lookup"><span data-stu-id="c1673-135">You can also read more about [uploading hello VHD tooAzure process](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)</span></span>


## <a name="deploy-a-vm-via-resource-manager-template"></a><span data-ttu-id="c1673-136">A virtuális gép keresztül Resource Manager-sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="c1673-136">Deploy a VM via Resource Manager Template</span></span>
<span data-ttu-id="c1673-137">A Resource Manager-sablonok, egy további paramétere belül `licenseType` adható meg.</span><span class="sxs-lookup"><span data-stu-id="c1673-137">Within your Resource Manager templates, an additional parameter for `licenseType` can be specified.</span></span> <span data-ttu-id="c1673-138">További tudnivalók [Azure Resource Manager sablonok készítése](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c1673-138">You can read more about [authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="c1673-139">Miután a virtuális merevlemez feltöltése tooAzure, szerkesztése, Resource Manager sablon tooinclude hello licenctípus hello részét szolgáltató számítási és a szokásos módon sablon üzembe helyezése:</span><span class="sxs-lookup"><span data-stu-id="c1673-139">Once you have your VHD uploaded tooAzure, edit you Resource Manager template tooinclude hello license type as part of hello compute provider and deploy your template as normal:</span></span>

<span data-ttu-id="c1673-140">A Windows Server:</span><span class="sxs-lookup"><span data-stu-id="c1673-140">For Windows Server:</span></span>
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

<span data-ttu-id="c1673-141">A Windows ügyfél toouse Azure piactér lemezképpel csak:</span><span class="sxs-lookup"><span data-stu-id="c1673-141">For Windows Client toouse with Azure Marketplace Image only:</span></span>
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a><span data-ttu-id="c1673-142">Telepítse a virtuális Gépet keresztül PowerShell gyors üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="c1673-142">Deploy a VM via PowerShell quickstart</span></span>
<span data-ttu-id="c1673-143">A Windows Server virtuális gép PowerShell használatával való telepítésekor, egy további paramétere van `-LicenseType`.</span><span class="sxs-lookup"><span data-stu-id="c1673-143">When deploying your Windows Server VM via PowerShell, you have an additional parameter for `-LicenseType`.</span></span> <span data-ttu-id="c1673-144">Miután a virtuális merevlemez feltöltése tooAzure, létrehozhat egy virtuális gép használatával `New-AzureRmVM` és adja meg az alábbiak szerint hello licencelési típusát:</span><span class="sxs-lookup"><span data-stu-id="c1673-144">Once you have your VHD uploaded tooAzure, you create a VM using `New-AzureRmVM` and specify hello licensing type as follows:</span></span>

<span data-ttu-id="c1673-145">A Windows Server:</span><span class="sxs-lookup"><span data-stu-id="c1673-145">For Windows Server:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

<span data-ttu-id="c1673-146">A Windows ügyfél toouse Azure piactér lemezképpel csak:</span><span class="sxs-lookup"><span data-stu-id="c1673-146">For Windows Client toouse with Azure Marketplace Image only:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

<span data-ttu-id="c1673-147">Is [olvassa el a virtuális gépek az Azure PowerShell telepítése részletes bemutatóért](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) az alábbi, vagy olvasási leíró útmutató túl hello-e más lépéseket[Windows virtuális gép létrehozása erőforrás-kezelő és a PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c1673-147">You can [read a more detailed walkthrough on deploying a VM in Azure via PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) below, or read a more descriptive guide on hello different steps too[create a Windows VM using Resource Manager and PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="verify-your-vm-is-utilizing-hello-licensing-benefit"></a><span data-ttu-id="c1673-148">Ellenőrizze, hogy a virtuális gép hello licencelési előnye van használata</span><span class="sxs-lookup"><span data-stu-id="c1673-148">Verify your VM is utilizing hello licensing benefit</span></span>
<span data-ttu-id="c1673-149">A virtuális gép hello PowerShell vagy a Resource Manager üzembe helyezési módszer használatával telepített, akkor ellenőrizze hello licenc típusa a `Get-AzureRmVM` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c1673-149">Once you have deployed your VM through either hello PowerShell or Resource Manager deployment method, verify hello license type with `Get-AzureRmVM` as follows:</span></span>

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="c1673-150">a kimeneti hello van hasonló toohello a következő példa a Windows Server:</span><span class="sxs-lookup"><span data-stu-id="c1673-150">hello output is similar toohello following example for Windows Server:</span></span>

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

<span data-ttu-id="c1673-151">A kimeneti ellentétben azokkal a következő virtuális gép telepítése nélkül Azure hibrid használata juttatás licencelési, például rögtön hello Azure-katalógus alapján telepített virtuális gép hello:</span><span class="sxs-lookup"><span data-stu-id="c1673-151">This output contrasts with hello following VM deployed without Azure Hybrid Use Benefit licensing, such as a VM deployed straight from hello Azure Gallery:</span></span>

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a><span data-ttu-id="c1673-152">Részletes PowerShell telepítési forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="c1673-152">Detailed PowerShell deployment walkthrough</span></span>
<span data-ttu-id="c1673-153">hello következő részletes PowerShell lépéseket megjelenítése a virtuális gépek teljes körű telepítésére.</span><span class="sxs-lookup"><span data-stu-id="c1673-153">hello following detailed PowerShell steps show a full deployment of a VM.</span></span> <span data-ttu-id="c1673-154">További környezet el tudja olvasni toohello tényleges parancsmagok és a különböző összetevők létrehozása [erőforrás-kezelő és a PowerShell használatával Windows virtuális gép létrehozása](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c1673-154">You can read more context as toohello actual cmdlets and different components being created in [Create a Windows VM using Resource Manager and PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="c1673-155">Az erőforráscsoport, a tárfiók és a virtuális hálózat létrehozásának lépéseit, majd adja meg a virtuális Gépet, és végül hozza létre a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="c1673-155">You step through creating your resource group, storage account, and virtual networking, then define your VM and finally create your VM.</span></span>

<span data-ttu-id="c1673-156">Először biztonságos hitelesítő adatok beszerzése, állítsa be a helyét, és az erőforráscsoport neve:</span><span class="sxs-lookup"><span data-stu-id="c1673-156">First, securely obtain credentials, set a location, and resource group name:</span></span>

```powershell
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "myResourceGroup"
```

<span data-ttu-id="c1673-157">Hozzon létre egy nyilvános IP-cím:</span><span class="sxs-lookup"><span data-stu-id="c1673-157">Create a public IP:</span></span>

```powershell
$publicIPName = "myPublicIP"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName `
    -Location $location -AllocationMethod "Dynamic"
```

<span data-ttu-id="c1673-158">Adja meg az alhálózat, a hálózati adapter és a virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="c1673-158">Define your subnet, NIC, and VNET:</span></span>

```powershell
$subnetName = "mySubnet"
$nicName = "myNIC"
$vnetName = "myVnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location `
    -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

<span data-ttu-id="c1673-159">A virtuális gép neve, és hozzon létre egy Virtuálisgép-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="c1673-159">Name your VM and create a VM config:</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

<span data-ttu-id="c1673-160">Adja meg az operációs rendszer:</span><span class="sxs-lookup"><span data-stu-id="c1673-160">Define your OS:</span></span>

```powershell
$computerName = "myVM"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="c1673-161">A hálózati adapter toohello VM hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="c1673-161">Add your NIC toohello VM:</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="c1673-162">Adja meg a storage-fiók toouse hello:</span><span class="sxs-lookup"><span data-stu-id="c1673-162">Define hello storage account toouse:</span></span>

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

<span data-ttu-id="c1673-163">Töltse fel a Merevlemezen, megfelelően előkészítve, és csatlakoztassa a virtuális gép tooyour használatra:</span><span class="sxs-lookup"><span data-stu-id="c1673-163">Upload your VHD, suitably prepared, and attach tooyour VM for use:</span></span>

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

<span data-ttu-id="c1673-164">Végezetül hozza létre a virtuális Gépet, és adja meg a licencelési típus tooutilize Azure hibrid használata juttatás hello:</span><span class="sxs-lookup"><span data-stu-id="c1673-164">Finally, create your VM and define hello licensing type tooutilize Azure Hybrid Use Benefit:</span></span>

<span data-ttu-id="c1673-165">A Windows Server:</span><span class="sxs-lookup"><span data-stu-id="c1673-165">For Windows Server:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a><span data-ttu-id="c1673-166">A virtuálisgép-méretezési keresztül Resource Manager-sablon beállítása telepítése</span><span class="sxs-lookup"><span data-stu-id="c1673-166">Deploy a virtual machine scale set via Resource Manager template</span></span>
<span data-ttu-id="c1673-167">A VMSS Resource Manager-sablonok, egy további paramétere belül `licenseType` meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="c1673-167">Within your VMSS Resource Manager templates, an additional parameter for `licenseType` must be specified.</span></span> <span data-ttu-id="c1673-168">További tudnivalók [Azure Resource Manager sablonok készítése](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c1673-168">You can read more about [authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="c1673-169">A Resource Manager sablon tooinclude hello licenseType tulajdonság hello méretezési készlet virtualMachineProfile részeként szerkesztése és a szokásos módon sablon telepítése – lásd: 2016 Windows Server lemezképet használja az alábbi példában:</span><span class="sxs-lookup"><span data-stu-id="c1673-169">Edit your Resource Manager template tooinclude hello licenseType property as part of hello scale set’s virtualMachineProfile and deploy your template as normal - see example below using 2016 Windows Server image:</span></span>


```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```

> [!NOTE]
> <span data-ttu-id="c1673-170">Egy virtuálisgép-méretezési AHUB előnyt PowerShell és egyéb SDK eszközök beállítása üzembe helyezése támogatása hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="c1673-170">Support for deploying a virtual machine scale set with AHUB benefits through PowerShell and other SDK tools is coming soon.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="c1673-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1673-171">Next steps</span></span>
<span data-ttu-id="c1673-172">Tudjon meg többet az [Azure hibrid használata juttatás licencelési](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span><span class="sxs-lookup"><span data-stu-id="c1673-172">Read more about [Azure Hybrid Use Benefit licensing](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span></span>

<span data-ttu-id="c1673-173">További információ [Resource Manager-sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c1673-173">Learn more about [using Resource Manager templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="c1673-174">További információ [Azure hibrid használata előnyök és az Azure Site Recovery ellenőrizze áttelepítése alkalmazások tooAzure költséghatékonyabb még](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).</span><span class="sxs-lookup"><span data-stu-id="c1673-174">Learn more about [Azure Hybrid Use Benefit and Azure Site Recovery make migrating applications tooAzure even more cost-effective](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).</span></span>
