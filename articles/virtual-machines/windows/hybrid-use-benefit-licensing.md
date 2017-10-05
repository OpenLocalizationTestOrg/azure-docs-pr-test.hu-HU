---
title: "A Windows Server és a Windows ügyfél Azure hibrid használata juttatás |} Microsoft Docs"
description: "Útmutató a Windows Software Assurance előnyeit, hogy helyszíni licencek Azure maximalizálása"
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
ms.openlocfilehash: 210635624946ddb293427167e9d476c377bcc9b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-hybrid-use-benefit-for-windows-server-and-windows-client"></a>Azure hibrid használata juttatása Windows Server és a Windows ügyfél
Software Assurance rendelkező ügyfelek Azure hibrid használja juttatás lehetővé teszi a helyszíni Windows Server és a Windows Client licencek használja, és futtassa a Windows virtuális gépek Azure-ban költséghatékony. Azure hibrid használata juttatás for Windows Server magában foglalja a Windows Server 2008R2, a Windows Server 2012, a Windows Server 2012 R2 rendszerben és a Windows Server 2016. Azure hibrid használata előnye a Windows-ügyfél a Windows 10 tartalmazza. További információkért lásd: a [Azure hibrid használata juttatás licencelési oldal](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

>[!IMPORTANT]
>Az Azure hibrid használata előnyeit a Windows ügyfél jelenleg előzetes a Windows 10-lemezkép használata az Azure piactéren. Csak a vállalati felhasználók a Windows 10 nagyvállalati E3/E5 egyes felhasználókra vagy a Windows VDA (bővítmény felhasználói előfizetési licencet vagy előfizetési licenceket) felhasználónkénti ("jogosult licencek") jogosultak.
>
>

## <a name="ways-to-use-azure-hybrid-use-benefit"></a>Azure hibrid használja juttatás használatának módjai
Van több különböző módon központi telepítése a Windows-alapú virtuális gépek Azure hibrid használata előnyökkel:

1. A virtuális gépek telepítése [adott piactéren elérhető rendszerkép](#deploy-a-vm-using-the-azure-marketplace) , amelyek az Azure hibrid használata juttatás – Windows Server 2016-os, a Windows Server 2012 R2 rendszerben, a Windows Server 2012 és Windows Server 2008SP1 előre konfigurált.
2. Is [feltöltése egy egyéni VM](#upload-a-windows-vhd) és [telepítése a Resource Manager-sablon használatával](#deploy-a-vm-via-resource-manager) vagy [Azure PowerShell](#detailed-powershell-deployment-walkthrough).

## <a name="deploy-a-vm-using-the-azure-marketplace"></a>A virtuális gépek az Azure piactéren telepítése
Következő lemezképek találhatók a piactér előre konfigurálva van az Azure hibrid használata juttatás: Windows Server 2016-ot, a Windows Server 2012 R2 rendszerben, a Windows Server 2012 és Windows Server 2008SP1. Ezek a rendszerképek közvetlenül az Azure Portal webhelyről, illetve Resource Manager-sablonok vagy az Azure PowerShell használatával helyezhetők üzembe.

Telepítheti ezeket a lemezképeket, közvetlenül az Azure portálról. A Resource Manager-sablonok és az Azure PowerShell használatra listájának megtekintéséhez képek az alábbiak szerint:

A Windows Server:
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
- 2016-Datacenter verziót 2016.127.20170406 vagy újabb

- 4.127.20170406 2012-R2 – Datacenter verzió vagy újabb

- 3.127.20170406 2012 – Datacenter verzió vagy újabb

- 2.127.20170406 2008 R2-SP1 verzió vagy újabb

Windows-ügyfél:
```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "Windows-HUB"
```

## <a name="upload-a-windows-server-vhd"></a>A Windows Server VHD feltöltése
Az Azure-ban a Windows Server virtuális gép üzembe helyezéséhez először az alap Windows-buildet tartalmazó virtuális merevlemez létrehozása. Ehhez a virtuális Merevlemezhez kell megfelelően készíteni a Sysprep segítségével az Azure-ba való feltöltés előtt. Is [további tudnivalókat a virtuális merevlemez követelményeiről és a Sysprep folyamat](upload-generalized-managed.md) és [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Készítsen biztonsági másolatot a virtuális Gépet a Sysprep futtatása előtt. 

Győződjön meg arról, hogy [telepítette és konfigurálta a legújabb Azure PowerShell](/powershell/azure/overview). Miután előkészítette a VHD-t, a VHD-fájlt feltölti az Azure Storage segítségével a `Add-AzureRmVhd` parancsmag az alábbiak szerint:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server és Dynamics is használhatja a frissítési garancia licencelési. Továbbra is elő kell készíteni a Windows Server kép az alkalmazás-összetevők telepítése és ennek megfelelően biztosító licenckulcsot, akkor a lemezképet feltöltése az Azure-bA. Tekintse át a Sysprep futtatása az alkalmazással, például a megfelelő dokumentációja [telepítése az SQL Server Sysprep használatával szempontjai](https://msdn.microsoft.com/library/ee210754.aspx) vagy [a SharePoint Server 2016 referencia lemezkép (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).
>
>

Akkor is olvashat további tudnivalókat [a virtuális merevlemez feltöltése az Azure folyamat](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)


## <a name="deploy-a-vm-via-resource-manager-template"></a>A virtuális gép keresztül Resource Manager-sablon üzembe helyezése
A Resource Manager-sablonok, egy további paramétere belül `licenseType` adható meg. További tudnivalók [Azure Resource Manager sablonok készítése](../../resource-group-authoring-templates.md). Miután a virtuális merevlemez feltöltése az Azure-ba, szerkesztése, a licenc típusa a számítási szolgáltató részeként és a szokásos módon sablon üzembe helyezése a Resource Manager-sablon:

A Windows Server:
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

Windows-ügyfél használata csak Azure Piactéri lemezképhez:
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>Telepítse a virtuális Gépet keresztül PowerShell gyors üzembe helyezés
A Windows Server virtuális gép PowerShell használatával való telepítésekor, egy további paramétere van `-LicenseType`. Miután a virtuális merevlemez feltöltése az Azure-bA rendelkezik, hozzon létre egy virtuális gép használatával `New-AzureRmVM` és az alábbiak szerint adja meg a licencelési típusát:

A Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

Windows-ügyfél használata csak Azure Piactéri lemezképhez:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

Is [olvassa el a virtuális gépek az Azure PowerShell telepítése részletes bemutatóért](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) az alábbi, vagy olvassa el a különböző lépéseket leíró útmutató [Windows virtuális gép létrehozása erőforrás-kezelő és a PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Ellenőrizze, hogy a virtuális Gépet a licencelési előnye van használata
A virtuális gép üzembe helyezett keresztül vagy a PowerShell vagy a Resource Manager üzembe helyezési módszer, ellenőrizze a licenc típusa a `Get-AzureRmVM` az alábbiak szerint:

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

A Windows Server az alábbi példához hasonló kimenete:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Ezzel szemben a hagyományosabb kimeneti a a következő virtuális Géphez nem Azure hibrid használata juttatás licencelési, például a virtuális gépek közvetlenül az Azure katalógusából telepített:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a>Részletes PowerShell telepítési forgatókönyv
A következő részletes PowerShell lépések bemutatják, a virtuális gépek teljes körű telepítésére. A tényleges parancsmagok és a különböző összetevők létrehozása több környezet el tudja olvasni [erőforrás-kezelő és a PowerShell használatával Windows virtuális gép létrehozása](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Az erőforráscsoport, a tárfiók és a virtuális hálózat létrehozásának lépéseit, majd adja meg a virtuális Gépet, és végül hozza létre a virtuális Gépet.

Először biztonságos hitelesítő adatok beszerzése, állítsa be a helyét, és az erőforráscsoport neve:

```powershell
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "myResourceGroup"
```

Hozzon létre egy nyilvános IP-cím:

```powershell
$publicIPName = "myPublicIP"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName `
    -Location $location -AllocationMethod "Dynamic"
```

Adja meg az alhálózat, a hálózati adapter és a virtuális hálózat:

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

A virtuális gép neve, és hozzon létre egy Virtuálisgép-konfiguráció:

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Adja meg az operációs rendszer:

```powershell
$computerName = "myVM"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Vegye fel a hálózati Kártyát a virtuális Gépet:

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Adja meg a storage-fiók használata:

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

Töltse fel a Merevlemezen, megfelelően előkészítve, és csatlakoztassa a virtuális gép számára:

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Végezetül hozza létre a virtuális Gépet, és Azure hibrid használata juttatás magukat, hogy a licencelési típust:

A Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a>A virtuálisgép-méretezési keresztül Resource Manager-sablon beállítása telepítése
A VMSS Resource Manager-sablonok, egy további paramétere belül `licenseType` meg kell adni. További tudnivalók [Azure Resource Manager sablonok készítése](../../resource-group-authoring-templates.md). A Resource Manager sablon tartalmazza a licenseType tulajdonságot a méretezési virtualMachineProfile részeként és a szokásos módon sablon telepítése – tekintse meg a 2016 Windows Server lemezképet használja az alábbi példában szerkesztése:


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
> Egy virtuálisgép-méretezési AHUB előnyt PowerShell és egyéb SDK eszközök beállítása üzembe helyezése támogatása hamarosan elérhető.
>

## <a name="next-steps"></a>Következő lépések
Tudjon meg többet az [Azure hibrid használata juttatás licencelési](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

További információ [Resource Manager-sablonok használatával](../../azure-resource-manager/resource-group-overview.md).

További információ [Azure hibrid használata előnyök és az Azure Site Recovery ellenőrizze áttelepítése alkalmazások az Azure-bA költséghatékonyabb még](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).
