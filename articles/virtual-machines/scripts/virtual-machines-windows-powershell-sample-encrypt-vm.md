---
title: "PowerShell parancsfájl minta - aaaAzure egy Windows virtuális gép titkosítása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – egy Windows virtuális gép titkosítása"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 80c52a0b734a52a051ed30026b294840fd521143
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="b4025-103">Az Azure PowerShell Windows virtuális gépek titkosítása</span><span class="sxs-lookup"><span data-stu-id="b4025-103">Encrypt a Windows virtual machine with Azure PowerShell</span></span>

<span data-ttu-id="b4025-104">Ez a parancsfájl létrehoz egy biztonságos Azure Key Vault, a titkosítási kulcsokat, az Azure Active Directory szolgáltatás egyszerű és a Windows rendszerű virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="b4025-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="b4025-105">virtuális gép hello majd titkosított hello titkosítási kulcs a Key Vault és a szolgáltatás egyszerű hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="b4025-105">hello VM is then encrypted using hello encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b4025-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b4025-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b4025-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b4025-107">Clean up deployment</span></span> 

<span data-ttu-id="b4025-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="b4025-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="b4025-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b4025-109">Script explanation</span></span>

<span data-ttu-id="b4025-110">A parancsfájl a következő parancsok toocreate hello telepítési hello.</span><span class="sxs-lookup"><span data-stu-id="b4025-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="b4025-111">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="b4025-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b4025-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="b4025-112">Command</span></span> | <span data-ttu-id="b4025-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b4025-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b4025-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b4025-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="b4025-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4025-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b4025-116">Új-AzureRmKeyVault</span><span class="sxs-lookup"><span data-stu-id="b4025-116">New-AzureRmKeyVault</span></span>](/powershell/module/azurerm.keyvault/new-azurermkeyvault) | <span data-ttu-id="b4025-117">Az Azure Key Vault toostore biztonságos adatokat például a titkosítási kulcsokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4025-117">Creates an Azure Key Vault toostore secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="b4025-118">-AzureKeyVaultKey</span><span class="sxs-lookup"><span data-stu-id="b4025-118">Add-AzureKeyVaultKey</span></span>](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | <span data-ttu-id="b4025-119">A Key Vault egy titkosítási kulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4025-119">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="b4025-120">Új AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="b4025-120">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="b4025-121">Egy Azure Active Directory szolgáltatás egyszerű toosecurely hitelesítéséhez, és szabályozhatja a hívóbetűk tooencryption hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4025-121">Creates an Azure Active Directory service principal toosecurely authenticate and control access tooencryption keys.</span></span> |
| [<span data-ttu-id="b4025-122">Set-AzureRmKeyVaultAccessPolicy</span><span class="sxs-lookup"><span data-stu-id="b4025-122">Set-AzureRmKeyVaultAccessPolicy</span></span>](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | <span data-ttu-id="b4025-123">Olyan engedélyeket ad a hello Key Vault toogrant hello szolgáltatás egyszerű tooencryption hívóbetűk.</span><span class="sxs-lookup"><span data-stu-id="b4025-123">Sets permissions on hello Key Vault toogrant hello service principal access tooencryption keys.</span></span> |
| [<span data-ttu-id="b4025-124">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="b4025-124">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="b4025-125">Létrehoz egy alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="b4025-125">Creates a subnet configuration.</span></span> <span data-ttu-id="b4025-126">Ebben a konfigurációban használatos hello virtuális hálózat létrehozásának folyamatát.</span><span class="sxs-lookup"><span data-stu-id="b4025-126">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="b4025-127">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="b4025-127">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="b4025-128">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b4025-128">Creates a virtual network.</span></span> |
| [<span data-ttu-id="b4025-129">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="b4025-129">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="b4025-130">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b4025-130">Creates a public IP address.</span></span> |
| [<span data-ttu-id="b4025-131">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="b4025-131">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="b4025-132">Létrehoz egy hálózati biztonsági csoport szabály konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="b4025-132">Creates a network security group rule configuration.</span></span> <span data-ttu-id="b4025-133">Ez a konfiguráció használt toocreate egy NSG-szabály esetén hello NSG jön létre.</span><span class="sxs-lookup"><span data-stu-id="b4025-133">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="b4025-134">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="b4025-134">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="b4025-135">Hálózati biztonsági csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b4025-135">Creates a network security group.</span></span> |
| [<span data-ttu-id="b4025-136">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="b4025-136">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="b4025-137">Lekérdezi az alhálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4025-137">Gets subnet information.</span></span> <span data-ttu-id="b4025-138">Ezt az információt a hálózati illesztő létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="b4025-138">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="b4025-139">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="b4025-139">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="b4025-140">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="b4025-140">Creates a network interface.</span></span> |
| [<span data-ttu-id="b4025-141">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="b4025-141">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="b4025-142">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b4025-142">Creates a VM configuration.</span></span> <span data-ttu-id="b4025-143">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="b4025-143">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="b4025-144">hello konfigurációs szolgál a virtuális gép létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="b4025-144">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="b4025-145">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="b4025-145">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="b4025-146">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="b4025-146">Create a virtual machine.</span></span> |
| [<span data-ttu-id="b4025-147">Get-AzureRmKeyVault</span><span class="sxs-lookup"><span data-stu-id="b4025-147">Get-AzureRmKeyVault</span></span>](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | <span data-ttu-id="b4025-148">A Key Vault hello lekérdezi a szükséges információk</span><span class="sxs-lookup"><span data-stu-id="b4025-148">Gets required information on hello Key Vault</span></span> |
| [<span data-ttu-id="b4025-149">Set-AzureRmVMDiskEncryptionExtension</span><span class="sxs-lookup"><span data-stu-id="b4025-149">Set-AzureRmVMDiskEncryptionExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | <span data-ttu-id="b4025-150">Engedélyezi a titkosítás használatát a virtuális gépek hello szolgáltatás egyszerű hitelesítő adatait, és a titkosítási kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="b4025-150">Enables encryption on a VM using hello service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="b4025-151">Get-AzureRmVmDiskEncryptionStatus</span><span class="sxs-lookup"><span data-stu-id="b4025-151">Get-AzureRmVmDiskEncryptionStatus</span></span>](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | <span data-ttu-id="b4025-152">Virtuális gép titkosítási folyamat hello hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b4025-152">Shows hello status of hello VM encryption process.</span></span> |
| [<span data-ttu-id="b4025-153">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b4025-153">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="b4025-154">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b4025-154">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b4025-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4025-155">Next steps</span></span>

<span data-ttu-id="b4025-156">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b4025-156">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b4025-157">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b4025-157">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
