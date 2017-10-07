---
title: "a virtuálisgép-méretezési alkalmazás aaaDeploy beállítása"
description: "Azure virtuálisgép-méretezési csoportok bővítmények toodepoy egy alkalmazás használatát."
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
ms.openlocfilehash: 5f3988b9511d80370a8be1fc042c21fee212506e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a><span data-ttu-id="29b3d-103">A virtuálisgép-méretezési csoportok az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="29b3d-103">Deploy your application on virtual machine scale sets</span></span>

<span data-ttu-id="29b3d-104">Ez a cikk ismerteti, hogyan tooinstall szoftver hello idő hello léptékű be különböző módokat lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="29b3d-104">This article describes different ways of how tooinstall software at hello time hello scale set is provisioned.</span></span>

<span data-ttu-id="29b3d-105">Érdemes lehet tooreview hello [méretezési beállítása kialakítás áttekintése](virtual-machine-scale-sets-design-overview.md) cikket, amely ismerteti az egyes hello határoz meg küszöbértéket virtuálisgép-méretezési csoportok.</span><span class="sxs-lookup"><span data-stu-id="29b3d-105">You may want tooreview hello [Scale Set Design Overview](virtual-machine-scale-sets-design-overview.md) article, which describes some of hello limits imposed by virtual machine scale sets.</span></span>

## <a name="capture-and-reuse-an-image"></a><span data-ttu-id="29b3d-106">Rögzítheti és újra felhasználhatja a képfájl</span><span class="sxs-lookup"><span data-stu-id="29b3d-106">Capture and reuse an image</span></span>

<span data-ttu-id="29b3d-107">Használhat egy virtuális gépet az Azure tooprepare egy alap-lemezképet a bővített adott meg.</span><span class="sxs-lookup"><span data-stu-id="29b3d-107">You can use a virtual machine you have in Azure tooprepare a base-image for your scale set.</span></span> <span data-ttu-id="29b3d-108">A folyamat egy felügyelt lemezes tárolás, a fiókot is hivatkozni lehessen hello alapjául szolgáló lemezképhez, a méretezési készlet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="29b3d-108">This process creates a managed disk in your storage account, which you can reference as hello base image for your scale set.</span></span> 

<span data-ttu-id="29b3d-109">Hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="29b3d-109">Do hello following steps:</span></span>

1. <span data-ttu-id="29b3d-110">Egy Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="29b3d-110">Create an Azure Virtual Machine</span></span>
   * <span data-ttu-id="29b3d-111">[Linux][linux-vm-create]</span><span class="sxs-lookup"><span data-stu-id="29b3d-111">[Linux][linux-vm-create]</span></span>
   * <span data-ttu-id="29b3d-112">[Windows][windows-vm-create]</span><span class="sxs-lookup"><span data-stu-id="29b3d-112">[Windows][windows-vm-create]</span></span>

2. <span data-ttu-id="29b3d-113">A távoli virtuális gép hello és hello rendszer tooyour tetszőlegesen testreszabása.</span><span class="sxs-lookup"><span data-stu-id="29b3d-113">Remote into hello virtual machine and customize hello system tooyour liking.</span></span>

   <span data-ttu-id="29b3d-114">Ha azt szeretné, az alkalmazás most már telepítheti.</span><span class="sxs-lookup"><span data-stu-id="29b3d-114">If you want, you can install your application now.</span></span> <span data-ttu-id="29b3d-115">Azonban tudnia, hogy az alkalmazás most telepítésével készíthet az alkalmazás bonyolultabb frissítése, mert tooremove szükség lehet az első.</span><span class="sxs-lookup"><span data-stu-id="29b3d-115">However, know that by installing your application now, you may make upgrading your application more complicated because you may need tooremove it first.</span></span> <span data-ttu-id="29b3d-116">Ehelyett használhatja ezt a lépést tooinstall előfeltételeket az alkalmazás esetleg, például egy adott runtime vagy az operációs rendszer szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="29b3d-116">Instead, you can use this step tooinstall any prerequisites your application may need, like a specific runtime or operating system feature.</span></span>

3. <span data-ttu-id="29b3d-117">Az oktatóanyag hello "gép rögzítése" bármelyik [Linux] [ linux-vm-capture] vagy [Windows][windows-vm-capture].</span><span class="sxs-lookup"><span data-stu-id="29b3d-117">Follow hello "capture a machine" tutorial for either [Linux][linux-vm-capture] or [Windows][windows-vm-capture].</span></span>

4. <span data-ttu-id="29b3d-118">Hozzon létre egy [virtuálisgép-méretezési csoport] [ vmss-create] hello rendelkező URI hello előző lépésben rögzített rendszerkép.</span><span class="sxs-lookup"><span data-stu-id="29b3d-118">Create a [Virtual Machine Scale Set][vmss-create] with hello image URI you captured in hello previous step.</span></span>

<span data-ttu-id="29b3d-119">Lemezek kapcsolatos további információkért lásd: [felügyelt lemezekhez – áttekintés](../virtual-machines/windows/managed-disks-overview.md) és [használata adatlemezt csatolni](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="29b3d-119">For more information about disks, see [Managed Disks Overview](../virtual-machines/windows/managed-disks-overview.md) and [Use Attached Data Disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

## <a name="install-when-hello-scale-set-is-provisioned"></a><span data-ttu-id="29b3d-120">Amikor hello méretezési ki van építve telepítése</span><span class="sxs-lookup"><span data-stu-id="29b3d-120">Install when hello scale set is provisioned</span></span>

<span data-ttu-id="29b3d-121">Virtuálisgép-bővítmények lehet tooa virtuálisgép-méretezési csoport alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="29b3d-121">Virtual machine extensions can be applied tooa virtual machine scale set.</span></span> <span data-ttu-id="29b3d-122">Egy virtuálisgép-bővítménnyel testre szabhatja a virtuális gépek hello terjedő skálán állítja be a teljes csoport.</span><span class="sxs-lookup"><span data-stu-id="29b3d-122">With a virtual machine extension, you can customize hello virtual machines in a scale set as a whole group.</span></span> <span data-ttu-id="29b3d-123">További információ a bővítményekről: [virtuálisgép-bővítmények](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="29b3d-123">For more information about extensions, see [Virtual Machine Extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="29b3d-124">Három fő kiterjesztések is használhatja, ha az operációs rendszertől függően Linux- vagy Windows-alapú.</span><span class="sxs-lookup"><span data-stu-id="29b3d-124">There are three main extensions you can use, depending on if your operating system is Linux-based or Windows-based.</span></span>

### <a name="windows"></a><span data-ttu-id="29b3d-125">Windows</span><span class="sxs-lookup"><span data-stu-id="29b3d-125">Windows</span></span>

<span data-ttu-id="29b3d-126">Egy Windows-alapú operációs rendszeren használja vagy hello **egyéni parancsfájl v1.8** kiterjesztés vagy a hello **PowerShell DSC** bővítmény.</span><span class="sxs-lookup"><span data-stu-id="29b3d-126">For a Windows-based operating system, use either hello **Custom Script v1.8** extension, or hello **PowerShell DSC** extension.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="29b3d-127">Egyéni parancsfájl</span><span class="sxs-lookup"><span data-stu-id="29b3d-127">Custom Script</span></span>

<span data-ttu-id="29b3d-128">Egyéni parancsprogramok futtatására szolgáló bővítmény hello hello méretezési csoportban lévő virtuális gép feltünteti parancsfájlt futtat.</span><span class="sxs-lookup"><span data-stu-id="29b3d-128">hello Custom Script extension runs a script on each virtual machine instance in hello scale set.</span></span> <span data-ttu-id="29b3d-129">A konfigurációs fájlban vagy a változó azt jelzi, mely fájlok legyenek letöltött toohello virtuális gépet, majd milyen parancsot futtatja.</span><span class="sxs-lookup"><span data-stu-id="29b3d-129">A config file or variable indicates which files are downloaded toohello virtual machine, and then what command runs.</span></span> <span data-ttu-id="29b3d-130">A telepítő toorun, egy parancsfájlt, egy kötegfájlt, végrehajtható fájlok például sikerült használja.</span><span class="sxs-lookup"><span data-stu-id="29b3d-130">You could use this toorun an installer, a script, a batch file, any executable for example.</span></span>

<span data-ttu-id="29b3d-131">PowerShell egy kivonattáblát hello beállításokat használ.</span><span class="sxs-lookup"><span data-stu-id="29b3d-131">PowerShell uses a hashtable for hello settings.</span></span> <span data-ttu-id="29b3d-132">Ez a példa hello egyéni parancsfájl kiterjesztése toorun egy PowerShell-parancsfájlt, amely telepíti az IIS konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="29b3d-132">This example configures hello custom script extension toorun a PowerShell script that installs IIS.</span></span>

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="29b3d-133">Használjon hello `-ProtectedSetting` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="29b3d-133">Use hello `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

---------


<span data-ttu-id="29b3d-134">Az Azure CLI hello-beállítások egy json-fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="29b3d-134">Azure CLI uses a json file for hello settings.</span></span> <span data-ttu-id="29b3d-135">Ez a példa hello egyéni parancsfájl kiterjesztése toorun egy PowerShell-parancsfájlt, amely telepíti az IIS konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="29b3d-135">This example configures hello custom script extension toorun a PowerShell script that installs IIS.</span></span> <span data-ttu-id="29b3d-136">Mentse a json-fájl, a következő hello _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="29b3d-136">Save hello following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

<span data-ttu-id="29b3d-137">Ezután futtassa az Azure CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="29b3d-137">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="29b3d-138">Használjon hello `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="29b3d-138">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="powershell-dsc"></a><span data-ttu-id="29b3d-139">A PowerShell DSC</span><span class="sxs-lookup"><span data-stu-id="29b3d-139">PowerShell DSC</span></span>

<span data-ttu-id="29b3d-140">Használhatja a PowerShell DSC toocustomize hello méretezési készlet virtuálisgép-példányok.</span><span class="sxs-lookup"><span data-stu-id="29b3d-140">You can use PowerShell DSC toocustomize hello scale set vm instances.</span></span> <span data-ttu-id="29b3d-141">Hello **DSC** bővítmény által közzétett **Microsoft.Powershell** telepíti, és futtatja a megadott hello DSC-konfiguráció minden egyes virtuálisgép-példányon.</span><span class="sxs-lookup"><span data-stu-id="29b3d-141">hello **DSC** extension published by **Microsoft.Powershell** deploys and runs hello provided DSC configuration on each virtual machine instance.</span></span> <span data-ttu-id="29b3d-142">A konfigurációs fájlban vagy a változó közli hello bővítmény ahol *.zip* csomag van, és amely _parancsfájl-függvény_ kombinációja toorun.</span><span class="sxs-lookup"><span data-stu-id="29b3d-142">A config file or variable tells hello extension where *.zip* package is, and which _script-function_ combination toorun.</span></span>

<span data-ttu-id="29b3d-143">PowerShell egy kivonattáblát hello beállításokat használ.</span><span class="sxs-lookup"><span data-stu-id="29b3d-143">PowerShell uses a hashtable for hello settings.</span></span> <span data-ttu-id="29b3d-144">Ebben a példában a DSC-csomagot, amely telepíti az IIS szolgáltatást telepíti.</span><span class="sxs-lookup"><span data-stu-id="29b3d-144">This example deploys a DSC package that installs IIS.</span></span>

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

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="29b3d-145">Használjon hello `-ProtectedSetting` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="29b3d-145">Use hello `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

-----------

<span data-ttu-id="29b3d-146">Az Azure CLI json hello beállításokat használ.</span><span class="sxs-lookup"><span data-stu-id="29b3d-146">Azure CLI uses a json for hello settings.</span></span> <span data-ttu-id="29b3d-147">Ebben a példában a DSC-csomagot, amely telepíti az IIS szolgáltatást telepíti.</span><span class="sxs-lookup"><span data-stu-id="29b3d-147">This example deploys a DSC package that installs IIS.</span></span> <span data-ttu-id="29b3d-148">Mentse a json-fájl, a következő hello _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="29b3d-148">Save hello following json file as _settings.json_.</span></span>

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

<span data-ttu-id="29b3d-149">Ezután futtassa az Azure CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="29b3d-149">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="29b3d-150">Használjon hello `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="29b3d-150">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="linux"></a><span data-ttu-id="29b3d-151">Linux</span><span class="sxs-lookup"><span data-stu-id="29b3d-151">Linux</span></span>

<span data-ttu-id="29b3d-152">Linux használhatja bármelyik hello **egyéni parancsfájl v2.0** kiterjesztést, vagy használjon **felhő inicializálás** létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="29b3d-152">Linux can use either hello **Custom Script v2.0** extension or use **cloud-init** during creation.</span></span>

<span data-ttu-id="29b3d-153">Egyéni parancsfájl olyan egyszerű bővítménnyel, amely letölti a fájlokat toohello virtuálisgép-példánya, és a parancsot futtatja.</span><span class="sxs-lookup"><span data-stu-id="29b3d-153">Custom script is a simple extension that downloads files toohello virtual machine instances, and runs a command.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="29b3d-154">Egyéni parancsfájl</span><span class="sxs-lookup"><span data-stu-id="29b3d-154">Custom Script</span></span>

<span data-ttu-id="29b3d-155">Mentse a json-fájl, a következő hello _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="29b3d-155">Save hello following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

<span data-ttu-id="29b3d-156">Hello Azure CLI tooadd a meglévő virtuálisgép-méretezési csoport kiterjesztése tooan használja.</span><span class="sxs-lookup"><span data-stu-id="29b3d-156">Use hello Azure CLI tooadd this extension tooan existing virtual machine scale set.</span></span> <span data-ttu-id="29b3d-157">Minden virtuális gép hello méretezési automatikusan fut hello virtuáliskapcsoló-kiterjesztés beállítása.</span><span class="sxs-lookup"><span data-stu-id="29b3d-157">Each virtual machine in hello scale set automatically runs hello extension.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="29b3d-158">Használjon hello `--protected-settings` bármely beállítások bizalmas információkat tartalmazó váltani.</span><span class="sxs-lookup"><span data-stu-id="29b3d-158">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

#### <a name="cloud-init"></a><span data-ttu-id="29b3d-159">Felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="29b3d-159">Cloud-Init</span></span>

<span data-ttu-id="29b3d-160">Felhő inicializálás hello méretezési csoport létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="29b3d-160">Cloud-Init is used when hello scale set is created.</span></span> <span data-ttu-id="29b3d-161">Először hozzon létre egy helyi fájlt _felhő-init.txt_ , és adja hozzá a konfigurációs tooit.</span><span class="sxs-lookup"><span data-stu-id="29b3d-161">First, create a local file named _cloud-init.txt_ and add your configuration tooit.</span></span> <span data-ttu-id="29b3d-162">Lásd például: [a gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span><span class="sxs-lookup"><span data-stu-id="29b3d-162">For example, see [this gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span></span>

<span data-ttu-id="29b3d-163">Használjon hello Azure CLI toocreate terjedő skálán beállítása.</span><span class="sxs-lookup"><span data-stu-id="29b3d-163">Use hello Azure CLI toocreate a scale set.</span></span> <span data-ttu-id="29b3d-164">Hello `--custom-data` a mezőbe egy felhő-init parancsfájl hello neve.</span><span class="sxs-lookup"><span data-stu-id="29b3d-164">hello `--custom-data` field accepts hello file name of a cloud-init script.</span></span>

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

## <a name="how-do-i-manage-application-updates"></a><span data-ttu-id="29b3d-165">Hogyan kezelhetem az alkalmazás frissítései?</span><span class="sxs-lookup"><span data-stu-id="29b3d-165">How do I manage application updates?</span></span>

<span data-ttu-id="29b3d-166">Ha az alkalmazás egy bővítmény használatával telepítette, az alter hello bővítmény definíciójának valamilyen módon.</span><span class="sxs-lookup"><span data-stu-id="29b3d-166">If you deployed your application through an extension, alter hello extension definition in some way.</span></span> <span data-ttu-id="29b3d-167">Ez a módosítás hatására hello bővítmény újratelepítése toobe tooall virtuálisgép-példánya.</span><span class="sxs-lookup"><span data-stu-id="29b3d-167">This change causes hello extension toobe redeployed tooall virtual machine instances.</span></span> <span data-ttu-id="29b3d-168">Valami **kell** hello bővítményről, például a hivatkozott fájl átnevezése, ellenkező esetben lehet módosítani az Azure biztosítja, nem a bővítmény hello lásd megváltozott.</span><span class="sxs-lookup"><span data-stu-id="29b3d-168">Something **must** be changed about hello extension, such as renaming a referenced file, otherwise, Azure does not see that hello extension has changed.</span></span>

<span data-ttu-id="29b3d-169">Hello alkalmazás a saját operációs rendszer lemezképén bővíthetőség akkor, ha egy automatikus telepítési folyamat használata alkalmazásfrissítéseket.</span><span class="sxs-lookup"><span data-stu-id="29b3d-169">If you baked hello application into your own operating system image, use an automated deployment pipeline for application updates.</span></span> <span data-ttu-id="29b3d-170">Tervezze meg a gyors egy előkészített méretezési készletben éles környezetben a csere architektúra toofacilitate.</span><span class="sxs-lookup"><span data-stu-id="29b3d-170">Design your architecture toofacilitate rapid swapping of a staged scale set into production.</span></span> <span data-ttu-id="29b3d-171">Ez a módszer jól szemlélteti hello [Azure Spinnaker illesztőprogram munkahelyi](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span><span class="sxs-lookup"><span data-stu-id="29b3d-171">A good example of this approach is hello [Azure Spinnaker driver work](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span></span>

<span data-ttu-id="29b3d-172">[Csomagoló](https://www.packer.io/) és [Terraform](https://www.terraform.io/) támogatási Azure Resource Manager, így is adja meg a képek "kódú" és build őket az Azure, majd használja a méretezési csoportban lévő virtuális merevlemez hello.</span><span class="sxs-lookup"><span data-stu-id="29b3d-172">[Packer](https://www.packer.io/) and [Terraform](https://www.terraform.io/) support Azure Resource Manager, so you can also define your images "as code" and build them in Azure, then use hello VHD in your scale set.</span></span> <span data-ttu-id="29b3d-173">Azonban ez úgy válna piactéren elérhető rendszerkép, ahol bővítmények/egyéni parancsfájlok válnak fontosabb óta, nem közvetlenül kezelheti a piactérről bits problémákat okozhatnak.</span><span class="sxs-lookup"><span data-stu-id="29b3d-173">However, doing so would become problematic for marketplace images, where extensions/custom scripts become more important since you don’t directly manipulate bits from marketplace.</span></span>

## <a name="what-happens-when-a-scale-set-scales-out"></a><span data-ttu-id="29b3d-174">Mi történik, ha egy méretezési méretezik el?</span><span class="sxs-lookup"><span data-stu-id="29b3d-174">What happens when a scale set scales out?</span></span>
<span data-ttu-id="29b3d-175">Egy vagy több virtuális gépek tooa méretezési hozzáadásakor hello alkalmazás automatikusan települ.</span><span class="sxs-lookup"><span data-stu-id="29b3d-175">When you add one or more virtual machines tooa scale set, hello application is automatically installed.</span></span> <span data-ttu-id="29b3d-176">A példa hello méretezési van az definiált bővítmények, számítógépen futnak egy új virtuális gép minden alkalommal létrejön.</span><span class="sxs-lookup"><span data-stu-id="29b3d-176">For example if hello scale set has extensions defined, they run on a new virtual machine each time it is created.</span></span> <span data-ttu-id="29b3d-177">Ha hello méretezési egyéni lemezkép alapul, bármely új virtuális gép hello forrás egyéni lemezkép egy példányát.</span><span class="sxs-lookup"><span data-stu-id="29b3d-177">If hello scale set is based on a custom image, any new virtual machine is a copy of hello source custom image.</span></span> <span data-ttu-id="29b3d-178">Ha hello méretezési készlet virtuális gépeket tároló gazdagépeken, majd lehetséges, hogy indítási kód tooload hello tárolók az egy egyéni parancsprogramok futtatására szolgáló bővítmény.</span><span class="sxs-lookup"><span data-stu-id="29b3d-178">If hello scale set virtual machines are container hosts, then you might have startup code tooload hello containers in a Custom Script Extension.</span></span> <span data-ttu-id="29b3d-179">Vagy egy bővítmény a céllal telepíthet olyan ügynök, amely egy fürt orchestrator, például az Azure Tárolószolgáltatás regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="29b3d-179">Or, an extension might install an agent that registers with a cluster orchestrator, such as Azure Container Service.</span></span>


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a><span data-ttu-id="29b3d-180">Hogyan tegye megkezdik az operációs rendszer frissítése frissítési tartományok között?</span><span class="sxs-lookup"><span data-stu-id="29b3d-180">How do you roll out an OS update across update domains?</span></span>
<span data-ttu-id="29b3d-181">Tegyük fel, hogy tooupdate az operációsrendszer-lemezképek hello virtuálisgép-méretezési futó megtartásával.</span><span class="sxs-lookup"><span data-stu-id="29b3d-181">Suppose you want tooupdate your OS image while keeping hello virtual machine scale set running.</span></span> <span data-ttu-id="29b3d-182">PowerShell és az Azure parancssori felület hello hello virtuálisgép-lemezképeket, egyszerre egy virtuális gép frissíthető.</span><span class="sxs-lookup"><span data-stu-id="29b3d-182">PowerShell and hello Azure CLI can update hello virtual machine images, one virtual machine at a time.</span></span> <span data-ttu-id="29b3d-183">Hello [frissítéséhez egy virtuálisgép-méretezési csoportban](./virtual-machine-scale-sets-upgrade-scale-set.md) cikkben további információkat is biztosít a milyen lehetőségek állnak rendelkezésre tooperform az operációs rendszer frissítése a virtuálisgép-méretezési csoport között.</span><span class="sxs-lookup"><span data-stu-id="29b3d-183">hello [Upgrade a Virtual Machine Scale Set](./virtual-machine-scale-sets-upgrade-scale-set.md) article also provides further information on what options are available tooperform an operating system upgrade across a virtual machine scale set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29b3d-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29b3d-184">Next steps</span></span>

* [<span data-ttu-id="29b3d-185">PowerShell toomanage használja a méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="29b3d-185">Use PowerShell toomanage your scale set.</span></span>](virtual-machine-scale-sets-windows-manage.md)
* [<span data-ttu-id="29b3d-186">A méretezési készlet sablont létrehozni.</span><span class="sxs-lookup"><span data-stu-id="29b3d-186">Create a scale set template.</span></span>](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

