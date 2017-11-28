---
title: "Virtuálisgép-bővítmények és a Linux funkcióit |} Microsoft Docs"
description: "Ismerje meg, milyen bővítmények érhetők el, mi akkor adja meg, vagy javítania szerint csoportosítva, az Azure virtuális gépeken."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 8a5b39351f665c51ae7d83f755329e54ff3cf786
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a><span data-ttu-id="6c821-103">Virtuálisgép-bővítmények és a Linux funkcióit</span><span class="sxs-lookup"><span data-stu-id="6c821-103">Virtual machine extensions and features for Linux</span></span>

<span data-ttu-id="6c821-104">Az Azure virtuálisgép-bővítmények kis alkalmazásokat, amelyek telepítés utáni konfigurációs és automatizálási feladatok Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="6c821-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="6c821-105">Például ha egy virtuális gépet a Szoftvertelepítés, a víruskereső védelmet, vagy a Docker konfigurációs igényel, a Virtuálisgép-bővítmény segítségével ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="6c821-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used to complete these tasks.</span></span> <span data-ttu-id="6c821-106">Az Azure Virtuálisgép-bővítmények futtathatja az Azure parancssori felület, PowerShell, az Azure Resource Manager-sablonok és az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="6c821-106">Azure VM extensions can be run using the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal.</span></span> <span data-ttu-id="6c821-107">Bővítmények mellékelhető egy új virtuálisgép-telepítést, vagy bármely létező rendszert futtathat.</span><span class="sxs-lookup"><span data-stu-id="6c821-107">Extensions can be bundled with a new virtual machine deployment, or run against any existing system.</span></span>

<span data-ttu-id="6c821-108">Ez a dokumentum áttekintést nyújt az Virtuálisgép-bővítmények, Azure Virtuálisgép-bővítmények használatára vonatkozó Előfeltételek és útmutatás észleléséhez, kezeléséhez, és távolítsa el a Virtuálisgép-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="6c821-108">This document provides an overview of VM extensions, prerequisites for using Azure VM extensions, and guidance on how to detect, manage, and remove VM extensions.</span></span> <span data-ttu-id="6c821-109">A dokumentum általános információkat tartalmaz a Virtuálisgép-bővítmények érhetők el, mert egyes potenciálisan egyedi konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="6c821-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="6c821-110">Bővítmény felhasználóspecifikus adatainak található minden a dokumentumban a egyéni bővítmény egyedi.</span><span class="sxs-lookup"><span data-stu-id="6c821-110">Extension-specific details can be found in each document specific to the individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="6c821-111">Használati esetek és minták</span><span class="sxs-lookup"><span data-stu-id="6c821-111">Use cases and samples</span></span>

<span data-ttu-id="6c821-112">Számos különböző Azure Virtuálisgép-bővítmények érhetők el, az adott használati eset.</span><span class="sxs-lookup"><span data-stu-id="6c821-112">Several different Azure VM extensions are available, each with a specific use case.</span></span> <span data-ttu-id="6c821-113">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="6c821-113">Some examples are:</span></span>

- <span data-ttu-id="6c821-114">PowerShell kívánt állapot konfiguráció alkalmazása a DSC-bővítményt használ a Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6c821-114">Apply PowerShell Desired State configurations to a virtual machine using the DSC extension for Linux.</span></span> <span data-ttu-id="6c821-115">További információkért lásd: [Azure kívánt állapot konfigurációs kiterjesztése](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span><span class="sxs-lookup"><span data-stu-id="6c821-115">For more information, see [Azure Desired State configuration extension](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span></span>
- <span data-ttu-id="6c821-116">A virtuális gép a Microsoft Monitoring Agent Virtuálisgép-bővítmény a figyelés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6c821-116">Configure monitoring of a virtual machine with the Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="6c821-117">További információkért lásd: [a Linux virtuális gépek figyelése](tutorial-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="6c821-117">For more information, see [How to monitor a Linux VM](tutorial-monitoring.md).</span></span>
- <span data-ttu-id="6c821-118">Az Azure infrastruktúra Datadog kiterjesztésű figyelés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6c821-118">Configure monitoring of your Azure infrastructure with the Datadog extension.</span></span> <span data-ttu-id="6c821-119">További információkért lásd: a [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="6c821-119">For more information, see the [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="6c821-120">A Docker állomás konfigurálása az Azure virtuális gépen a Docker Virtuálisgép-bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="6c821-120">Configure a Docker host on an Azure virtual machine using the Docker VM extension.</span></span> <span data-ttu-id="6c821-121">További információkért lásd: [Docker Virtuálisgép-bővítmény](dockerextension.md).</span><span class="sxs-lookup"><span data-stu-id="6c821-121">For more information, see [Docker VM extension](dockerextension.md).</span></span>

<span data-ttu-id="6c821-122">Folyamatokra vonatkozó bővítmények mellett egy egyéni parancsprogramok futtatására szolgáló bővítmény érhető el a Windows és a Linux virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="6c821-122">In addition to process-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="6c821-123">Az egyéni parancsprogramok futtatására szolgáló bővítmény Linux lehetővé teszi, hogy a virtuális gépen futtatandó Bash parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="6c821-123">The Custom Script extension for Linux allows any Bash script to be run on a virtual machine.</span></span> <span data-ttu-id="6c821-124">Egyéni parancsfájlok túl milyen natív Azure tooling biztosíthat beállításokra van szükség Azure központi telepítések tervezése során hasznosak.</span><span class="sxs-lookup"><span data-stu-id="6c821-124">Custom scripts are useful for designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="6c821-125">További információkért lásd: [Linux virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="6c821-125">For more information, see [Linux VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="6c821-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6c821-126">Prerequisites</span></span>

<span data-ttu-id="6c821-127">Minden egyes virtuálisgép-bővítmény előfordulhat, hogy rendelkeznek a saját előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="6c821-127">Each virtual machine extension might have its own set of prerequisites.</span></span> <span data-ttu-id="6c821-128">Például a Docker Virtuálisgép-bővítmény van egy támogatott Linux-disztribúció előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="6c821-128">For instance, the Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="6c821-129">Egyes bővítmények követelményeinek részletes leírást talál a bővítmény-specifikus dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6c821-129">Requirements of individual extensions are detailed in the extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="6c821-130">Azure virtuálisgép-ügynök</span><span class="sxs-lookup"><span data-stu-id="6c821-130">Azure VM agent</span></span>

<span data-ttu-id="6c821-131">Az Azure Virtuálisgép-ügynök kezeli a Azure virtuális gép és az Azure fabric controller közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="6c821-131">The Azure VM agent manages interactions between an Azure virtual machine and the Azure fabric controller.</span></span> <span data-ttu-id="6c821-132">A Virtuálisgép-ügynök telepítése és kezelése az Azure virtuális gépeken, beleértve a Virtuálisgép-bővítmények futtató sok működési jellemzői felelős.</span><span class="sxs-lookup"><span data-stu-id="6c821-132">The VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="6c821-133">Az Azure Virtuálisgép-ügynök telepítve van az Azure piactéren elérhető rendszerkép, és támogatott operációs rendszeren manuálisan telepíthető.</span><span class="sxs-lookup"><span data-stu-id="6c821-133">The Azure VM agent is preinstalled on Azure Marketplace images and can be installed manually on supported operating systems.</span></span>

<span data-ttu-id="6c821-134">Információ a támogatott operációs rendszerek és a telepítési utasításokat: [Azure virtuális gépi ügynöke](../windows/classic/agents-and-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="6c821-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](../windows/classic/agents-and-extensions.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="6c821-135">Virtuálisgép-bővítmények felderítése</span><span class="sxs-lookup"><span data-stu-id="6c821-135">Discover VM extensions</span></span>

<span data-ttu-id="6c821-136">Számos különböző Virtuálisgép-bővítmények állnak rendelkezésre az Azure virtuális gépekkel.</span><span class="sxs-lookup"><span data-stu-id="6c821-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="6c821-137">Teljes listájának megtekintéséhez futtassa a következő parancsot az Azure parancssori felület, a példa cseréje a megfelelő helyre.</span><span class="sxs-lookup"><span data-stu-id="6c821-137">To see a complete list, run the following command with the Azure CLI, replacing the example location with the location of your choice.</span></span>

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a><span data-ttu-id="6c821-138">Futtassa a Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="6c821-138">Run VM extensions</span></span>

<span data-ttu-id="6c821-139">Az Azure virtuálisgép-bővítmények a meglévő virtuális gépeken, amelyek hasznosak, ha konfigurációs módosításokat, vagy állítsa helyre a kapcsolatot egy már telepített virtuális gépen kell futtatható.</span><span class="sxs-lookup"><span data-stu-id="6c821-139">Azure virtual machine extensions can be run on existing virtual machines, which are useful when you need to make configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="6c821-140">Virtuálisgép-bővítmények is Azure Resource Manager sablon központi telepítéseket is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="6c821-140">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="6c821-141">Bővítmények a Resource Manager-sablonok segítségével az Azure virtuális gépek is telepítését és konfigurálását telepítés utáni beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="6c821-141">By using extensions with Resource Manager templates, Azure virtual machines can be deployed and configured without post-deployment intervention.</span></span>

<span data-ttu-id="6c821-142">Az alábbi eljárások segítségével bővítmény futtathat egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6c821-142">The following methods can be used to run an extension against an existing virtual machine.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="6c821-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6c821-143">Azure CLI</span></span>

<span data-ttu-id="6c821-144">Meglévő virtuális gépből ellen is futtatható Azure virtuálisgép-bővítmények használatával a `az vm extension set` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6c821-144">Azure virtual machine extensions can be run against an existing virtual machine by using the `az vm extension set` command.</span></span> <span data-ttu-id="6c821-145">Ebben a példában egy virtuális gép fut az egyéni parancsprogramok futtatására szolgáló bővítmény.</span><span class="sxs-lookup"><span data-stu-id="6c821-145">This example runs the custom script extension against a virtual machine.</span></span>

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

<span data-ttu-id="6c821-146">A parancsfájl az alábbihoz hasonló kimenetet hoz létre:</span><span class="sxs-lookup"><span data-stu-id="6c821-146">The script produces output similar to the following text:</span></span>

```azurecli
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a><span data-ttu-id="6c821-147">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6c821-147">Azure portal</span></span>

<span data-ttu-id="6c821-148">Virtuálisgép-bővítmények egy meglévő virtuális gépet az Azure portálon keresztül is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="6c821-148">VM extensions can be applied to an existing virtual machine through the Azure portal.</span></span> <span data-ttu-id="6c821-149">Ehhez jelölje ki a virtuális gépet, válassza a **bővítmények**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6c821-149">To do so, select the virtual machine, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="6c821-150">Jelölje ki a bővítményt, szeretné, hogy a rendelkezésre álló bővítmények a listából, és kövesse a varázsló utasításait.</span><span class="sxs-lookup"><span data-stu-id="6c821-150">Select the extension you want from the list of available extensions and follow the instructions in the wizard.</span></span>

<span data-ttu-id="6c821-151">A következő kép bemutatja az Azure-portálról Linux egyéni parancsprogramok futtatására szolgáló bővítmény telepítése.</span><span class="sxs-lookup"><span data-stu-id="6c821-151">The following image shows the installation of the Linux Custom Script extension from the Azure portal.</span></span>

![Egyéni parancsfájl-kiterjesztés telepítése](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="6c821-153">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="6c821-153">Azure Resource Manager templates</span></span>

<span data-ttu-id="6c821-154">Virtuálisgép-bővítmények felvehető az Azure Resource Manager-sablon és a sablon a központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6c821-154">VM extensions can be added to an Azure Resource Manager template and executed with the deployment of the template.</span></span> <span data-ttu-id="6c821-155">Egy bővítmény a sablon telepítésekor hozhat létre teljesen konfigurált Azure-környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="6c821-155">When you deploy an extension with a template, you can create fully configured Azure deployments.</span></span> <span data-ttu-id="6c821-156">Például egy Resource Manager-sablon a következő JSON származik.</span><span class="sxs-lookup"><span data-stu-id="6c821-156">For example, the following JSON is taken from a Resource Manager template.</span></span> <span data-ttu-id="6c821-157">A sablon olyan virtuális gépek elosztott terhelésű és az Azure SQL-adatbázis telepíti, és telepíti a .NET Core alkalmazások minden egyes virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="6c821-157">The template deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="6c821-158">A Virtuálisgép-bővítmény gondoskodik a szoftver telepítését.</span><span class="sxs-lookup"><span data-stu-id="6c821-158">The VM extension takes care of the software installation.</span></span>

<span data-ttu-id="6c821-159">További információkért lásd: a teljes [Resource Manager-sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="6c821-159">For more information, see the full [Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

<span data-ttu-id="6c821-160">További információkért lásd: [Azure Resource Manager-sablonok készítése](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span><span class="sxs-lookup"><span data-stu-id="6c821-160">For more information, see [Authoring Azure Resource Manager templates](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="6c821-161">Virtuálisgép-bővítmény adatvédelem</span><span class="sxs-lookup"><span data-stu-id="6c821-161">Secure VM extension data</span></span>

<span data-ttu-id="6c821-162">Jelenik meg a Virtuálisgép-bővítmény, akkor lehet szükség, például a hitelesítő adatokat, a tárfiókok neve és a fiók tárelérési kulcsok bizalmas adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6c821-162">When you're running a VM extension, it may be necessary to include sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="6c821-163">Több Virtuálisgép-bővítmények közé tartozik a védett konfigurációkat, amelyek titkosítja az adatokat, és csak visszafejti a cél virtuális gépen belül.</span><span class="sxs-lookup"><span data-stu-id="6c821-163">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside the target virtual machine.</span></span> <span data-ttu-id="6c821-164">Mindegyik bővítményt egy adott védett konfigurációs séma van, és az egyes részleteit a bővítmény-specifikus dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6c821-164">Each extension has a specific protected configuration schema, and each is detailed in extension-specific documentation.</span></span>

<span data-ttu-id="6c821-165">A következő példa bemutatja az egyéni parancsprogramok futtatására szolgáló bővítmény példányának Linux.</span><span class="sxs-lookup"><span data-stu-id="6c821-165">The following example shows an instance of the Custom Script extension for Linux.</span></span> <span data-ttu-id="6c821-166">Figyelje meg, hogy a parancs végrehajtásának tartalmazza-e a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="6c821-166">Notice that the command to execute includes a set of credentials.</span></span> <span data-ttu-id="6c821-167">Ebben a példában a parancs végrehajtása nem lesznek titkosítva.</span><span class="sxs-lookup"><span data-stu-id="6c821-167">In this example, the command to execute will not be encrypted.</span></span>


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

<span data-ttu-id="6c821-168">Helyezze át a **végrehajtandó parancs** tulajdonságot a **védett** konfigurációs titkosítja a végrehajtási karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="6c821-168">Moving the **command to execute** property to the **protected** configuration secures the execution string.</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="6c821-169">Virtuálisgép-bővítmények hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="6c821-169">Troubleshoot VM extensions</span></span>

<span data-ttu-id="6c821-170">Minden egyes Virtuálisgép-bővítmény hibaelhárítási lépések az adott a kiterjesztést is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="6c821-170">Each VM extension may have troubleshooting steps specific to the extension.</span></span> <span data-ttu-id="6c821-171">Például ha az egyéni parancsprogramok futtatására szolgáló bővítmény használata esetén parancsfájl végrehajtásának részletes adatai található helyileg a virtuális gépen, amelyen a bővítmény futott.</span><span class="sxs-lookup"><span data-stu-id="6c821-171">For example, when you're using the Custom Script extension, script execution details can be found locally on the virtual machine on which the extension was run.</span></span> <span data-ttu-id="6c821-172">Bővítmény-specifikus hibaelhárítási lépéseket részletes leírást talál bővítmény vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="6c821-172">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="6c821-173">A következő hibaelhárítási lépéseket az összes virtuálisgép-bővítmény vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="6c821-173">The following troubleshooting steps apply to all virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="6c821-174">Bővítmény állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="6c821-174">View extension status</span></span>

<span data-ttu-id="6c821-175">Után a virtuális gépek bővítmény futtatva lett a virtuális gépek elleni, a következő paranccsal Azure CLI térjen vissza a bővítmény állapotát.</span><span class="sxs-lookup"><span data-stu-id="6c821-175">After a virtual machine extension has been run against a virtual machine, use the following Azure CLI command to return extension status.</span></span> <span data-ttu-id="6c821-176">Példa paraméterneveknek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="6c821-176">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="6c821-177">A kimeneti néz ki a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="6c821-177">The output looks like the following text:</span></span>

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

<span data-ttu-id="6c821-178">Bővítmény végrehajtási állapotát az Azure portálon is található.</span><span class="sxs-lookup"><span data-stu-id="6c821-178">Extension execution status can also be found in the Azure portal.</span></span> <span data-ttu-id="6c821-179">Egy bővítmény állapotának megtekintéséhez válassza ki a virtuális gépet, kattintson a **bővítmények**, és jelölje ki a kívánt bővítményt.</span><span class="sxs-lookup"><span data-stu-id="6c821-179">To view the status of an extension, select the virtual machine, choose **Extensions**, and select the desired extension.</span></span>

### <a name="rerun-a-vm-extension"></a><span data-ttu-id="6c821-180">Futtassa újra a Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-180">Rerun a VM extension</span></span>

<span data-ttu-id="6c821-181">Előfordulhatnak olyan esetekben, ahol egy virtuálisgép-bővítmény futtatható kell.</span><span class="sxs-lookup"><span data-stu-id="6c821-181">There may be cases in which a virtual machine extension needs to be rerun.</span></span> <span data-ttu-id="6c821-182">Egy bővítmény akkor távolítsa el, majd futtassa újból a kiterjesztés egy végrehajtási módszer az Ön által választott újrafuttathatja.</span><span class="sxs-lookup"><span data-stu-id="6c821-182">You can rerun an extension by removing it, and then rerunning the extension with an execution method of your choice.</span></span> <span data-ttu-id="6c821-183">Egy bővítmény eltávolításához futtassa a következő parancsot az Azure parancssori felület segítségével.</span><span class="sxs-lookup"><span data-stu-id="6c821-183">To remove an extension, run the following command with the Azure CLI.</span></span> <span data-ttu-id="6c821-184">Példa paraméterneveknek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="6c821-184">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

<span data-ttu-id="6c821-185">Az alábbi lépéseket követve az Azure portálon kiterjesztéssel megszüntetheti:</span><span class="sxs-lookup"><span data-stu-id="6c821-185">You can remove an extension by using the following steps in the Azure portal:</span></span>

1. <span data-ttu-id="6c821-186">Válasszon ki egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6c821-186">Select a virtual machine.</span></span>
2. <span data-ttu-id="6c821-187">Válasszon **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="6c821-187">Choose **Extensions**.</span></span>
3. <span data-ttu-id="6c821-188">Jelölje ki a kívánt bővítményt.</span><span class="sxs-lookup"><span data-stu-id="6c821-188">Select the desired extension.</span></span>
4. <span data-ttu-id="6c821-189">Válasszon **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="6c821-189">Choose **Uninstall**.</span></span>

## <a name="common-vm-extension-reference"></a><span data-ttu-id="6c821-190">Virtuálisgép-bővítmény közös hivatkozása</span><span class="sxs-lookup"><span data-stu-id="6c821-190">Common VM extension reference</span></span>
| <span data-ttu-id="6c821-191">Bővítmény neve</span><span class="sxs-lookup"><span data-stu-id="6c821-191">Extension name</span></span> | <span data-ttu-id="6c821-192">Leírás</span><span class="sxs-lookup"><span data-stu-id="6c821-192">Description</span></span> | <span data-ttu-id="6c821-193">További információ</span><span class="sxs-lookup"><span data-stu-id="6c821-193">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6c821-194">Linux rendszeren egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-194">Custom Script extension for Linux</span></span> |<span data-ttu-id="6c821-195">Parancsfájlok futtatásához egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="6c821-195">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="6c821-196">Linux rendszeren egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-196">Custom Script extension for Linux</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="6c821-197">Docker-bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-197">Docker extension</span></span> |<span data-ttu-id="6c821-198">Telepítse a Docker démon távoli Docker parancsok támogatására.</span><span class="sxs-lookup"><span data-stu-id="6c821-198">Install the Docker daemon to support remote Docker commands.</span></span> |[<span data-ttu-id="6c821-199">Docker virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-199">Docker VM extension</span></span>](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="6c821-200">Virtuálisgép-hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-200">VM Access extension</span></span> |<span data-ttu-id="6c821-201">Újra hozzáférést nyerni egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="6c821-201">Regain access to an Azure virtual machine</span></span> |[<span data-ttu-id="6c821-202">Virtuálisgép-hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-202">VM Access extension</span></span>](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| <span data-ttu-id="6c821-203">Azure Diagnostics-bővítményt</span><span class="sxs-lookup"><span data-stu-id="6c821-203">Azure Diagnostics extension</span></span> |<span data-ttu-id="6c821-204">Az Azure Diagnostics kezelése</span><span class="sxs-lookup"><span data-stu-id="6c821-204">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="6c821-205">Azure Diagnostics-bővítményt</span><span class="sxs-lookup"><span data-stu-id="6c821-205">Azure Diagnostics extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="6c821-206">Az Azure virtuális gép hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-206">Azure VM Access extension</span></span> |<span data-ttu-id="6c821-207">Felhasználók és a hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="6c821-207">Manage users and credentials</span></span> |[<span data-ttu-id="6c821-208">Linux virtuális gép hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="6c821-208">VM Access extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
