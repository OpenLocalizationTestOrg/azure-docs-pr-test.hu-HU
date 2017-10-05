---
title: "A virtuálisgép-méretezési csoportok alkalmazás telepítése"
description: "A bővítmények számára az alkalmazások depoy használata Azure virtuálisgép-méretezési készlet."
services: virtual-machine-scale-sets
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: fa7d9d3bef4cb326844ede76171e8c566e87116b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a><span data-ttu-id="21a37-103">A virtuálisgép-méretezési csoportok az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="21a37-103">Deploy your application on virtual machine scale sets</span></span>

<span data-ttu-id="21a37-104">Ez a cikk ismerteti a szoftver telepítése a méretezési ki van építve időpontjában különböző módokat.</span><span class="sxs-lookup"><span data-stu-id="21a37-104">This article describes different ways of how to install software at the time the scale set is provisioned.</span></span>

<span data-ttu-id="21a37-105">Előfordulhat, hogy szeretné megtekinteni a [méretezési beállítása kialakítás áttekintése](virtual-machine-scale-sets-design-overview.md) cikket, amely bemutat néhányat a határoz meg küszöbértéket virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="21a37-105">You may want to review the [Scale Set Design Overview](virtual-machine-scale-sets-design-overview.md) article, which describes some of the limits imposed by virtual machine scale sets.</span></span>

## <a name="capture-and-reuse-an-image"></a><span data-ttu-id="21a37-106">Rögzítheti és újra felhasználhatja a képfájl</span><span class="sxs-lookup"><span data-stu-id="21a37-106">Capture and reuse an image</span></span>

<span data-ttu-id="21a37-107">A base-lemezkép előkészítése a scale Azure-ban beállított virtuális gép is használhatja.</span><span class="sxs-lookup"><span data-stu-id="21a37-107">You can use a virtual machine you have in Azure to prepare a base-image for your scale set.</span></span> <span data-ttu-id="21a37-108">A folyamat egy felügyelt lemezes tárolás, a fiókot is hivatkozni lehessen az alapjául szolgáló lemezképhez, a méretezési készlet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="21a37-108">This process creates a managed disk in your storage account, which you can reference as the base image for your scale set.</span></span> 

<span data-ttu-id="21a37-109">Hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21a37-109">Do the following steps:</span></span>

1. <span data-ttu-id="21a37-110">Egy Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="21a37-110">Create an Azure Virtual Machine</span></span>
   * <span data-ttu-id="21a37-111">[Linux][linux-vm-create]</span><span class="sxs-lookup"><span data-stu-id="21a37-111">[Linux][linux-vm-create]</span></span>
   * <span data-ttu-id="21a37-112">[Windows][windows-vm-create]</span><span class="sxs-lookup"><span data-stu-id="21a37-112">[Windows][windows-vm-create]</span></span>

2. <span data-ttu-id="21a37-113">A virtuális gép be távolról és testre szabhatja a tetszőlegesen a rendszer.</span><span class="sxs-lookup"><span data-stu-id="21a37-113">Remote into the virtual machine and customize the system to your liking.</span></span>

   <span data-ttu-id="21a37-114">Ha azt szeretné, az alkalmazás most már telepítheti.</span><span class="sxs-lookup"><span data-stu-id="21a37-114">If you want, you can install your application now.</span></span> <span data-ttu-id="21a37-115">Azonban tudnia, hogy az alkalmazás most telepíti, lehetséges, hogy tegyen bonyolultabb az alkalmazás frissítése, mert szükség lehet, először távolítsa el azt.</span><span class="sxs-lookup"><span data-stu-id="21a37-115">However, know that by installing your application now, you may make upgrading your application more complicated because you may need to remove it first.</span></span> <span data-ttu-id="21a37-116">Ehelyett az ebben a lépésben segítségével telepítenie az előfeltételeket, az alkalmazás esetleg, például egy adott runtime vagy az operációs rendszer szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="21a37-116">Instead, you can use this step to install any prerequisites your application may need, like a specific runtime or operating system feature.</span></span>

3. <span data-ttu-id="21a37-117">Az oktatóanyag a "gép rögzítése" bármelyik [Linux] [ linux-vm-capture] vagy [Windows][windows-vm-capture].</span><span class="sxs-lookup"><span data-stu-id="21a37-117">Follow the "capture a machine" tutorial for either [Linux][linux-vm-capture] or [Windows][windows-vm-capture].</span></span>

4. <span data-ttu-id="21a37-118">Hozzon létre egy [virtuálisgép-méretezési csoport] [ vmss-create] a lemezképpel az előző lépésben rögzített URI.</span><span class="sxs-lookup"><span data-stu-id="21a37-118">Create a [Virtual Machine Scale Set][vmss-create] with the image URI you captured in the previous step.</span></span>

<span data-ttu-id="21a37-119">Lemezek kapcsolatos további információkért lásd: [felügyelt lemezekhez – áttekintés](../virtual-machines/windows/managed-disks-overview.md) és [használata adatlemezt csatolni](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="21a37-119">For more information about disks, see [Managed Disks Overview](../virtual-machines/windows/managed-disks-overview.md) and [Use Attached Data Disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

## <a name="install-when-the-scale-set-is-provisioned"></a><span data-ttu-id="21a37-120">Ha a méretezési ki van építve telepítése</span><span class="sxs-lookup"><span data-stu-id="21a37-120">Install when the scale set is provisioned</span></span>

<span data-ttu-id="21a37-121">A virtuálisgép-méretezési csoport virtuálisgép-bővítmények alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="21a37-121">Virtual machine extensions can be applied to a virtual machine scale set.</span></span> <span data-ttu-id="21a37-122">Egy virtuálisgép-bővítménnyel testre szabhatja a virtuális gépek terjedő skálán állítja be a teljes csoport.</span><span class="sxs-lookup"><span data-stu-id="21a37-122">With a virtual machine extension, you can customize the virtual machines in a scale set as a whole group.</span></span> <span data-ttu-id="21a37-123">További információ a bővítményekről: [virtuálisgép-bővítmények](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="21a37-123">For more information about extensions, see [Virtual Machine Extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="21a37-124">Három fő kiterjesztések is használhatja, ha az operációs rendszertől függően Linux- vagy Windows-alapú.</span><span class="sxs-lookup"><span data-stu-id="21a37-124">There are three main extensions you can use, depending on if your operating system is Linux-based or Windows-based.</span></span>

### <a name="windows"></a><span data-ttu-id="21a37-125">Windows</span><span class="sxs-lookup"><span data-stu-id="21a37-125">Windows</span></span>

<span data-ttu-id="21a37-126">Használja a Windows-alapú operációs rendszer esetén a **egyéni parancsfájl v1.8** kiterjesztést, vagy a **PowerShell DSC** bővítmény.</span><span class="sxs-lookup"><span data-stu-id="21a37-126">For a Windows-based operating system, use either the **Custom Script v1.8** extension, or the **PowerShell DSC** extension.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="21a37-127">Egyéni parancsfájl</span><span class="sxs-lookup"><span data-stu-id="21a37-127">Custom Script</span></span>

<span data-ttu-id="21a37-128">Az egyéni parancsprogramok futtatására szolgáló bővítmény parancsfájlt futtat a méretezési csoportban lévő egyes virtuálisgép-példányt.</span><span class="sxs-lookup"><span data-stu-id="21a37-128">The Custom Script extension runs a script on each virtual machine instance in the scale set.</span></span> <span data-ttu-id="21a37-129">A konfigurációs fájlban vagy a változó azt jelzi, mely fájlokat szeretne letölteni a virtuális géphez, majd milyen parancsot futtatja.</span><span class="sxs-lookup"><span data-stu-id="21a37-129">A config file or variable indicates which files are downloaded to the virtual machine, and then what command runs.</span></span> <span data-ttu-id="21a37-130">Telepítőhöz, egy parancsfájlt, egy kötegfájlt, végrehajtható fájlok például futtatásához használhatja.</span><span class="sxs-lookup"><span data-stu-id="21a37-130">You could use this to run an installer, a script, a batch file, any executable for example.</span></span>

<span data-ttu-id="21a37-131">PowerShell egy kivonattáblát a beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="21a37-131">PowerShell uses a hashtable for the settings.</span></span> <span data-ttu-id="21a37-132">Ebben a példában az egyéni parancsprogramok futtatására szolgáló bővítmény futtatni egy PowerShell-parancsfájlt, amely telepíti az IIS konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="21a37-132">This example configures the custom script extension to run a PowerShell script that installs IIS.</span></span>

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="21a37-133">Használja a `-ProtectedSetting` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="21a37-133">Use the `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

---------


<span data-ttu-id="21a37-134">Az Azure parancssori felület a beállításokat egy json-fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="21a37-134">Azure CLI uses a json file for the settings.</span></span> <span data-ttu-id="21a37-135">Ebben a példában az egyéni parancsprogramok futtatására szolgáló bővítmény futtatni egy PowerShell-parancsfájlt, amely telepíti az IIS konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="21a37-135">This example configures the custom script extension to run a PowerShell script that installs IIS.</span></span> <span data-ttu-id="21a37-136">A következő json fájl mentése másként _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="21a37-136">Save the following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

<span data-ttu-id="21a37-137">Ezután futtassa az Azure CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="21a37-137">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="21a37-138">Használja a `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="21a37-138">Use the `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="powershell-dsc"></a><span data-ttu-id="21a37-139">A PowerShell DSC</span><span class="sxs-lookup"><span data-stu-id="21a37-139">PowerShell DSC</span></span>

<span data-ttu-id="21a37-140">PowerShell DSC segítségével testre szabhatja a méretezési készlet virtuálisgép-példányok.</span><span class="sxs-lookup"><span data-stu-id="21a37-140">You can use PowerShell DSC to customize the scale set vm instances.</span></span> <span data-ttu-id="21a37-141">A **DSC** bővítmény által közzétett **Microsoft.Powershell** telepíti és futtatja a megadott DSC-konfiguráció minden egyes virtuálisgép-példányt.</span><span class="sxs-lookup"><span data-stu-id="21a37-141">The **DSC** extension published by **Microsoft.Powershell** deploys and runs the provided DSC configuration on each virtual machine instance.</span></span> <span data-ttu-id="21a37-142">A konfigurációs fájlban vagy a változó közli a bővítmény ahol *.zip* csomag van, és amely _parancsfájl-függvény_ kombinációja futtatásához.</span><span class="sxs-lookup"><span data-stu-id="21a37-142">A config file or variable tells the extension where *.zip* package is, and which _script-function_ combination to run.</span></span>

<span data-ttu-id="21a37-143">PowerShell egy kivonattáblát a beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="21a37-143">PowerShell uses a hashtable for the settings.</span></span> <span data-ttu-id="21a37-144">Ebben a példában a DSC-csomagot, amely telepíti az IIS szolgáltatást telepíti.</span><span class="sxs-lookup"><span data-stu-id="21a37-144">This example deploys a DSC package that installs IIS.</span></span>

```powershell
# Setup extension configuration hashtable variable
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="21a37-145">Használja a `-ProtectedSetting` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="21a37-145">Use the `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

-----------

<span data-ttu-id="21a37-146">Az Azure CLI json a beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="21a37-146">Azure CLI uses a json for the settings.</span></span> <span data-ttu-id="21a37-147">Ebben a példában a DSC-csomagot, amely telepíti az IIS szolgáltatást telepíti.</span><span class="sxs-lookup"><span data-stu-id="21a37-147">This example deploys a DSC package that installs IIS.</span></span> <span data-ttu-id="21a37-148">A következő json fájl mentése másként _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="21a37-148">Save the following json file as _settings.json_.</span></span>

```json
{
  "wmfVersion": "latest",
  "configuration": {
    "url": "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip",
    "script": "configure-http.ps1",
    "function": "WebsiteTest"
  }
}
```

<span data-ttu-id="21a37-149">Ezután futtassa az Azure CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="21a37-149">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="21a37-150">Használja a `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="21a37-150">Use the `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="linux"></a><span data-ttu-id="21a37-151">Linux</span><span class="sxs-lookup"><span data-stu-id="21a37-151">Linux</span></span>

<span data-ttu-id="21a37-152">Linux használhatja a **egyéni parancsfájl v2.0** kiterjesztést, vagy használjon **felhő inicializálás** létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="21a37-152">Linux can use either the **Custom Script v2.0** extension or use **cloud-init** during creation.</span></span>

<span data-ttu-id="21a37-153">Egyéni parancsfájl olyan egyszerű bővítménnyel, amely letölti a fájlokat a virtuálisgép-példánya, és a parancsot futtatja.</span><span class="sxs-lookup"><span data-stu-id="21a37-153">Custom script is a simple extension that downloads files to the virtual machine instances, and runs a command.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="21a37-154">Egyéni parancsfájl</span><span class="sxs-lookup"><span data-stu-id="21a37-154">Custom Script</span></span>

<span data-ttu-id="21a37-155">A következő json fájl mentése másként _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="21a37-155">Save the following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

<span data-ttu-id="21a37-156">Az Azure parancssori felület használatával adja hozzá ezt a bővítményt egy meglévő virtuálisgép-méretezési csoport.</span><span class="sxs-lookup"><span data-stu-id="21a37-156">Use the Azure CLI to add this extension to an existing virtual machine scale set.</span></span> <span data-ttu-id="21a37-157">A méretezési készletben automatikusan a virtuális gépeken futtatja a bővítmény.</span><span class="sxs-lookup"><span data-stu-id="21a37-157">Each virtual machine in the scale set automatically runs the extension.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="21a37-158">Használja a `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="21a37-158">Use the `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

#### <a name="cloud-init"></a><span data-ttu-id="21a37-159">Felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="21a37-159">Cloud-Init</span></span>

<span data-ttu-id="21a37-160">Felhő inicializálás a méretezési csoport létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="21a37-160">Cloud-Init is used when the scale set is created.</span></span> <span data-ttu-id="21a37-161">Először hozzon létre egy helyi fájlt _felhő-init.txt_ , és a konfigurációs felvétele.</span><span class="sxs-lookup"><span data-stu-id="21a37-161">First, create a local file named _cloud-init.txt_ and add your configuration to it.</span></span> <span data-ttu-id="21a37-162">Lásd például: [a gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span><span class="sxs-lookup"><span data-stu-id="21a37-162">For example, see [this gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span></span>

<span data-ttu-id="21a37-163">Az Azure parancssori felület használatával hozzon létre egy méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="21a37-163">Use the Azure CLI to create a scale set.</span></span> <span data-ttu-id="21a37-164">A `--custom-data` mezőben fogadja el a felhő inicializálás parancsfájl nevét.</span><span class="sxs-lookup"><span data-stu-id="21a37-164">The `--custom-data` field accepts the file name of a cloud-init script.</span></span>

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

## <a name="how-do-i-manage-application-updates"></a><span data-ttu-id="21a37-165">Hogyan kezelhetem az alkalmazás frissítései?</span><span class="sxs-lookup"><span data-stu-id="21a37-165">How do I manage application updates?</span></span>

<span data-ttu-id="21a37-166">Ha telepítette az alkalmazást egy bővítményével, módosítsa a bővítmény definíciójának valamilyen módon.</span><span class="sxs-lookup"><span data-stu-id="21a37-166">If you deployed your application through an extension, alter the extension definition in some way.</span></span> <span data-ttu-id="21a37-167">Ez a módosítás hatására a bővítményt a virtuálisgép-példányok újratelepítése.</span><span class="sxs-lookup"><span data-stu-id="21a37-167">This change causes the extension to be redeployed to all virtual machine instances.</span></span> <span data-ttu-id="21a37-168">Valami **kell** a bővítmény, például a hivatkozott fájl átnevezése, ellenkező esetben lehet módosítani az Azure biztosítja nem tekintse meg a kiterjesztése megváltozott.</span><span class="sxs-lookup"><span data-stu-id="21a37-168">Something **must** be changed about the extension, such as renaming a referenced file, otherwise, Azure does not see that the extension has changed.</span></span>

<span data-ttu-id="21a37-169">Ha Ön az alkalmazás a saját operációs rendszer lemezképén bővíthetőség, használjon egy automatikus telepítési folyamat alkalmazás frissítések.</span><span class="sxs-lookup"><span data-stu-id="21a37-169">If you baked the application into your own operating system image, use an automated deployment pipeline for application updates.</span></span> <span data-ttu-id="21a37-170">Tervezze meg a architektúra lehetővé teszi egy előkészített méretezési készletben éles környezetben gyors csere.</span><span class="sxs-lookup"><span data-stu-id="21a37-170">Design your architecture to facilitate rapid swapping of a staged scale set into production.</span></span> <span data-ttu-id="21a37-171">Ez a módszer egy jó példa a [Azure Spinnaker illesztőprogram munkahelyi](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span><span class="sxs-lookup"><span data-stu-id="21a37-171">A good example of this approach is the [Azure Spinnaker driver work](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span></span>

<span data-ttu-id="21a37-172">[Csomagoló](https://www.packer.io/) és [Terraform](https://www.terraform.io/) támogatási Azure Resource Manager így is adja meg a képek "kódú" és build őket az Azure, majd használja a virtuális merevlemez a méretezési csoportban lévő.</span><span class="sxs-lookup"><span data-stu-id="21a37-172">[Packer](https://www.packer.io/) and [Terraform](https://www.terraform.io/) support Azure Resource Manager, so you can also define your images "as code" and build them in Azure, then use the VHD in your scale set.</span></span> <span data-ttu-id="21a37-173">Azonban ez úgy válna piactéren elérhető rendszerkép, ahol bővítmények/egyéni parancsfájlok válnak fontosabb óta, nem közvetlenül kezelheti a piactérről bits problémákat okozhatnak.</span><span class="sxs-lookup"><span data-stu-id="21a37-173">However, doing so would become problematic for marketplace images, where extensions/custom scripts become more important since you don’t directly manipulate bits from marketplace.</span></span>

## <a name="what-happens-when-a-scale-set-scales-out"></a><span data-ttu-id="21a37-174">Mi történik, ha egy méretezési méretezik el?</span><span class="sxs-lookup"><span data-stu-id="21a37-174">What happens when a scale set scales out?</span></span>
<span data-ttu-id="21a37-175">Amikor egy vagy több virtuális gépeket ad hozzá egy méretezési, az alkalmazás automatikusan települ.</span><span class="sxs-lookup"><span data-stu-id="21a37-175">When you add one or more virtual machines to a scale set, the application is automatically installed.</span></span> <span data-ttu-id="21a37-176">A példa a méretezési van az definiált bővítmények, számítógépen futnak egy új virtuális gép minden alkalommal létrejön.</span><span class="sxs-lookup"><span data-stu-id="21a37-176">For example if the scale set has extensions defined, they run on a new virtual machine each time it is created.</span></span> <span data-ttu-id="21a37-177">Ha a méretezési egyéni lemezkép alapul, bármely új virtuális gép a forrás egyéni lemezkép egy példányát.</span><span class="sxs-lookup"><span data-stu-id="21a37-177">If the scale set is based on a custom image, any new virtual machine is a copy of the source custom image.</span></span> <span data-ttu-id="21a37-178">Esetén a méretezési készlet virtuális gépeket tároló állomások, majd lehetséges, hogy a tárolókat az egy egyéni parancsprogramok futtatására szolgáló bővítmény betöltése indítás kódot.</span><span class="sxs-lookup"><span data-stu-id="21a37-178">If the scale set virtual machines are container hosts, then you might have startup code to load the containers in a Custom Script Extension.</span></span> <span data-ttu-id="21a37-179">Vagy egy bővítmény a céllal telepíthet olyan ügynök, amely egy fürt orchestrator, például az Azure Tárolószolgáltatás regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="21a37-179">Or, an extension might install an agent that registers with a cluster orchestrator, such as Azure Container Service.</span></span>


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a><span data-ttu-id="21a37-180">Hogyan tegye megkezdik az operációs rendszer frissítése frissítési tartományok között?</span><span class="sxs-lookup"><span data-stu-id="21a37-180">How do you roll out an OS update across update domains?</span></span>
<span data-ttu-id="21a37-181">Tegyük fel, hogy az operációsrendszer-lemezképek frissítéséhez a virtuálisgép-méretezési futó megtartásával.</span><span class="sxs-lookup"><span data-stu-id="21a37-181">Suppose you want to update your OS image while keeping the virtual machine scale set running.</span></span> <span data-ttu-id="21a37-182">PowerShell és az Azure parancssori felület frissítheti a virtuális gép képfájljait, egyszerre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="21a37-182">PowerShell and the Azure CLI can update the virtual machine images, one virtual machine at a time.</span></span> <span data-ttu-id="21a37-183">A [frissítéséhez egy virtuálisgép-méretezési csoportban](./virtual-machine-scale-sets-upgrade-scale-set.md) cikkben további információkat is biztosít a milyen beállítások érhetők el operációs rendszer verziófrissítést végrehajtani egy virtuálisgép-méretezési csoport között.</span><span class="sxs-lookup"><span data-stu-id="21a37-183">The [Upgrade a Virtual Machine Scale Set](./virtual-machine-scale-sets-upgrade-scale-set.md) article also provides further information on what options are available to perform an operating system upgrade across a virtual machine scale set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21a37-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21a37-184">Next steps</span></span>

* [<span data-ttu-id="21a37-185">A PowerShell szolgáltatás használatával kezelheti a méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="21a37-185">Use PowerShell to manage your scale set.</span></span>](virtual-machine-scale-sets-windows-manage.md)
* [<span data-ttu-id="21a37-186">A méretezési készlet sablont létrehozni.</span><span class="sxs-lookup"><span data-stu-id="21a37-186">Create a scale set template.</span></span>](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

