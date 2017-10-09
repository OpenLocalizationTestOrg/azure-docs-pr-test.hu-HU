---
title: "aaaVirtual gép bővítményeket és szolgáltatásokat Linux |} Microsoft Docs"
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
ms.openlocfilehash: e0d2ce794c76815ccc6743e8788ee5d9d931e9a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a><span data-ttu-id="27018-103">Virtuálisgép-bővítmények és a Linux funkcióit</span><span class="sxs-lookup"><span data-stu-id="27018-103">Virtual machine extensions and features for Linux</span></span>

<span data-ttu-id="27018-104">Az Azure virtuálisgép-bővítmények kis alkalmazásokat, amelyek telepítés utáni konfigurációs és automatizálási feladatok Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="27018-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="27018-105">Például ha egy virtuális gépet a Szoftvertelepítés, a víruskereső védelmet, vagy a Docker konfigurációs igényel, a Virtuálisgép-bővítmény is használt toocomplete kell ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="27018-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used toocomplete these tasks.</span></span> <span data-ttu-id="27018-106">Az Azure Virtuálisgép-bővítmények hello Azure CLI PowerShell, Azure Resource Manager sablonok segítségével, és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="27018-106">Azure VM extensions can be run using hello Azure CLI, PowerShell, Azure Resource Manager templates, and hello Azure portal.</span></span> <span data-ttu-id="27018-107">Bővítmények mellékelhető egy új virtuálisgép-telepítést, vagy bármely létező rendszert futtathat.</span><span class="sxs-lookup"><span data-stu-id="27018-107">Extensions can be bundled with a new virtual machine deployment, or run against any existing system.</span></span>

<span data-ttu-id="27018-108">Ez a dokumentum áttekintést Virtuálisgép-bővítmények, Azure Virtuálisgép-bővítmények és útmutatás toodetect, hogyan kezelheti, és távolítsa el a Virtuálisgép-bővítmények használatának előfeltételei.</span><span class="sxs-lookup"><span data-stu-id="27018-108">This document provides an overview of VM extensions, prerequisites for using Azure VM extensions, and guidance on how toodetect, manage, and remove VM extensions.</span></span> <span data-ttu-id="27018-109">A dokumentum általános információkat tartalmaz a Virtuálisgép-bővítmények érhetők el, mert egyes potenciálisan egyedi konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="27018-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="27018-110">Minden egyes dokumentum adott toohello egyedi bővítmény részletei bővítmény-specifikus megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="27018-110">Extension-specific details can be found in each document specific toohello individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="27018-111">Használati esetek és minták</span><span class="sxs-lookup"><span data-stu-id="27018-111">Use cases and samples</span></span>

<span data-ttu-id="27018-112">Számos különböző Azure Virtuálisgép-bővítmények érhetők el, az adott használati eset.</span><span class="sxs-lookup"><span data-stu-id="27018-112">Several different Azure VM extensions are available, each with a specific use case.</span></span> <span data-ttu-id="27018-113">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="27018-113">Some examples are:</span></span>

- <span data-ttu-id="27018-114">PowerShell kívánt állapot konfigurációk tooa virtuális gépek DSC-bővítményt hello Linux alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="27018-114">Apply PowerShell Desired State configurations tooa virtual machine using hello DSC extension for Linux.</span></span> <span data-ttu-id="27018-115">További információkért lásd: [Azure kívánt állapot konfigurációs kiterjesztése](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span><span class="sxs-lookup"><span data-stu-id="27018-115">For more information, see [Azure Desired State configuration extension](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span></span>
- <span data-ttu-id="27018-116">A virtuális gép a Microsoft Monitoring Agent Virtuálisgép-bővítmény hello figyelés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="27018-116">Configure monitoring of a virtual machine with hello Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="27018-117">További információkért lásd: [hogyan toomonitor Linux virtuális gép](tutorial-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="27018-117">For more information, see [How toomonitor a Linux VM](tutorial-monitoring.md).</span></span>
- <span data-ttu-id="27018-118">Az Azure infrastruktúra hello Datadog bővítmény a figyelés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="27018-118">Configure monitoring of your Azure infrastructure with hello Datadog extension.</span></span> <span data-ttu-id="27018-119">További információkért lásd: hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="27018-119">For more information, see hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="27018-120">A Docker állomás konfigurálása Azure virtuális géphez hello Docker Virtuálisgép-bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="27018-120">Configure a Docker host on an Azure virtual machine using hello Docker VM extension.</span></span> <span data-ttu-id="27018-121">További információkért lásd: [Docker Virtuálisgép-bővítmény](dockerextension.md).</span><span class="sxs-lookup"><span data-stu-id="27018-121">For more information, see [Docker VM extension](dockerextension.md).</span></span>

<span data-ttu-id="27018-122">Továbbá tooprocess-specifikus bővítmények, egy egyéni parancsprogramok futtatására szolgáló bővítmény érhető el a Windows és Linux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="27018-122">In addition tooprocess-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="27018-123">hello Linux rendszeren egyéni parancsprogramok futtatására szolgáló bővítmény lehetővé teszi, hogy bármely Bash parancsfájl toobe futtasson egy virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="27018-123">hello Custom Script extension for Linux allows any Bash script toobe run on a virtual machine.</span></span> <span data-ttu-id="27018-124">Egyéni parancsfájlok túl milyen natív Azure tooling biztosíthat beállításokra van szükség Azure központi telepítések tervezése során hasznosak.</span><span class="sxs-lookup"><span data-stu-id="27018-124">Custom scripts are useful for designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="27018-125">További információkért lásd: [Linux virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="27018-125">For more information, see [Linux VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="27018-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="27018-126">Prerequisites</span></span>

<span data-ttu-id="27018-127">Minden egyes virtuálisgép-bővítmény előfordulhat, hogy rendelkeznek a saját előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="27018-127">Each virtual machine extension might have its own set of prerequisites.</span></span> <span data-ttu-id="27018-128">Például hello Docker Virtuálisgép-bővítmény van egy támogatott Linux-disztribúció előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="27018-128">For instance, hello Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="27018-129">Egyes bővítmények követelményeinek részletes leírást talál hello bővítmény vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="27018-129">Requirements of individual extensions are detailed in hello extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="27018-130">Azure virtuálisgép-ügynök</span><span class="sxs-lookup"><span data-stu-id="27018-130">Azure VM agent</span></span>

<span data-ttu-id="27018-131">hello Azure Virtuálisgép-ügynök kezeli a hello Azure fabric controller és az Azure virtuális gépek közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="27018-131">hello Azure VM agent manages interactions between an Azure virtual machine and hello Azure fabric controller.</span></span> <span data-ttu-id="27018-132">hello Virtuálisgép-ügynök telepítése és kezelése az Azure virtuális gépeken, beleértve a Virtuálisgép-bővítmények futtató sok működési jellemzői felelős.</span><span class="sxs-lookup"><span data-stu-id="27018-132">hello VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="27018-133">hello Azure Virtuálisgép-ügynök telepítve van az Azure piactéren elérhető rendszerkép és támogatott operációs rendszeren manuálisan telepíthető.</span><span class="sxs-lookup"><span data-stu-id="27018-133">hello Azure VM agent is preinstalled on Azure Marketplace images and can be installed manually on supported operating systems.</span></span>

<span data-ttu-id="27018-134">Információ a támogatott operációs rendszerek és a telepítési utasításokat: [Azure virtuális gépi ügynöke](../windows/classic/agents-and-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="27018-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](../windows/classic/agents-and-extensions.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="27018-135">Virtuálisgép-bővítmények felderítése</span><span class="sxs-lookup"><span data-stu-id="27018-135">Discover VM extensions</span></span>

<span data-ttu-id="27018-136">Számos különböző Virtuálisgép-bővítmények állnak rendelkezésre az Azure virtuális gépekkel.</span><span class="sxs-lookup"><span data-stu-id="27018-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="27018-137">toosee teljes listáját, futtassa hello hello Azure CLI-parancsot a következő, hello példa lecserélését hello megfelelő helyre.</span><span class="sxs-lookup"><span data-stu-id="27018-137">toosee a complete list, run hello following command with hello Azure CLI, replacing hello example location with hello location of your choice.</span></span>

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a><span data-ttu-id="27018-138">Futtassa a Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="27018-138">Run VM extensions</span></span>

<span data-ttu-id="27018-139">Az Azure virtuálisgép-bővítmények futtatható meglévő virtuális gépeken, amelyek hasznosak, ha kell toomake konfigurációs módosításokat, vagy állítsa helyre a kapcsolatot egy már telepített virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="27018-139">Azure virtual machine extensions can be run on existing virtual machines, which are useful when you need toomake configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="27018-140">Virtuálisgép-bővítmények is Azure Resource Manager sablon központi telepítéseket is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="27018-140">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="27018-141">Bővítmények a Resource Manager-sablonok segítségével az Azure virtuális gépek is telepítését és konfigurálását telepítés utáni beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="27018-141">By using extensions with Resource Manager templates, Azure virtual machines can be deployed and configured without post-deployment intervention.</span></span>

<span data-ttu-id="27018-142">a következő módszerek hello használt toorun egy bővítmény egy meglévő virtuális gépek ellen lehet.</span><span class="sxs-lookup"><span data-stu-id="27018-142">hello following methods can be used toorun an extension against an existing virtual machine.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="27018-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="27018-143">Azure CLI</span></span>

<span data-ttu-id="27018-144">Az Azure virtuálisgép-bővítmények is futtathatja egy meglévő virtuális gépet hello segítségével `az vm extension set` parancsot.</span><span class="sxs-lookup"><span data-stu-id="27018-144">Azure virtual machine extensions can be run against an existing virtual machine by using hello `az vm extension set` command.</span></span> <span data-ttu-id="27018-145">Ebben a példában egy virtuális gép fut hello egyéni parancsprogramok futtatására szolgáló bővítmény.</span><span class="sxs-lookup"><span data-stu-id="27018-145">This example runs hello custom script extension against a virtual machine.</span></span>

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

<span data-ttu-id="27018-146">hello parancsfájl előállított kimeneti hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="27018-146">hello script produces output similar toohello following text:</span></span>

```azurecli
info:    Executing command vm extension set
+ Looking up hello VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a><span data-ttu-id="27018-147">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="27018-147">Azure portal</span></span>

<span data-ttu-id="27018-148">Virtuálisgép-bővítmények alkalmazott tooan meglévő virtuális gép hello Azure-portálon keresztül lehet.</span><span class="sxs-lookup"><span data-stu-id="27018-148">VM extensions can be applied tooan existing virtual machine through hello Azure portal.</span></span> <span data-ttu-id="27018-149">toodo Igen, jelölje ki a hello virtuális gépet, válassza a **bővítmények**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="27018-149">toodo so, select hello virtual machine, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="27018-150">Szeretné, hogy a rendelkezésre álló bővítmények hello listából, és hello hello varázsló utasításait követve hello bővítmény kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="27018-150">Select hello extension you want from hello list of available extensions and follow hello instructions in hello wizard.</span></span>

<span data-ttu-id="27018-151">hello következő kép bemutatja hello hello Linux egyéni parancsprogramok futtatására szolgáló bővítmény hello Azure-portálon való telepítését.</span><span class="sxs-lookup"><span data-stu-id="27018-151">hello following image shows hello installation of hello Linux Custom Script extension from hello Azure portal.</span></span>

![Egyéni parancsfájl-kiterjesztés telepítése](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="27018-153">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="27018-153">Azure Resource Manager templates</span></span>

<span data-ttu-id="27018-154">Virtuálisgép-bővítmények hozzáadott tooan Azure Resource Manager-sablon és hello sablon hello telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="27018-154">VM extensions can be added tooan Azure Resource Manager template and executed with hello deployment of hello template.</span></span> <span data-ttu-id="27018-155">Egy bővítmény a sablon telepítésekor hozhat létre teljesen konfigurált Azure-környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="27018-155">When you deploy an extension with a template, you can create fully configured Azure deployments.</span></span> <span data-ttu-id="27018-156">Például egy Resource Manager-sablon a következő JSON hello származik.</span><span class="sxs-lookup"><span data-stu-id="27018-156">For example, hello following JSON is taken from a Resource Manager template.</span></span> <span data-ttu-id="27018-157">hello sablon olyan virtuális gépek elosztott terhelésű és az Azure SQL-adatbázis telepíti, és telepíti a .NET Core alkalmazások minden egyes virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="27018-157">hello template deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="27018-158">Virtuálisgép-bővítmény hello hello Szoftvertelepítés gondoskodik.</span><span class="sxs-lookup"><span data-stu-id="27018-158">hello VM extension takes care of hello software installation.</span></span>

<span data-ttu-id="27018-159">További információkért tekintse meg a teljes hello [Resource Manager-sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="27018-159">For more information, see hello full [Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

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

<span data-ttu-id="27018-160">További információkért lásd: [Azure Resource Manager-sablonok készítése](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span><span class="sxs-lookup"><span data-stu-id="27018-160">For more information, see [Authoring Azure Resource Manager templates](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="27018-161">Virtuálisgép-bővítmény adatvédelem</span><span class="sxs-lookup"><span data-stu-id="27018-161">Secure VM extension data</span></span>

<span data-ttu-id="27018-162">Ha egy Virtuálisgép-bővítmény, szükséges tooinclude bizalmas információk, például a hitelesítő adatokat, a tárfiókok neve és a fiók tárelérési kulcsok lehet.</span><span class="sxs-lookup"><span data-stu-id="27018-162">When you're running a VM extension, it may be necessary tooinclude sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="27018-163">Több Virtuálisgép-bővítmények közé tartozik a védett konfigurációkat, amelyek titkosítja az adatokat, és csak visszafejti hello cél virtuális gépen belül.</span><span class="sxs-lookup"><span data-stu-id="27018-163">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside hello target virtual machine.</span></span> <span data-ttu-id="27018-164">Mindegyik bővítményt egy adott védett konfigurációs séma van, és az egyes részleteit a bővítmény-specifikus dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="27018-164">Each extension has a specific protected configuration schema, and each is detailed in extension-specific documentation.</span></span>

<span data-ttu-id="27018-165">a következő példa hello hello egyéni parancsprogramok futtatására szolgáló bővítmény példányának Linux jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="27018-165">hello following example shows an instance of hello Custom Script extension for Linux.</span></span> <span data-ttu-id="27018-166">Figyelje meg, hogy hello parancs tooexecute hitelesítő adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="27018-166">Notice that hello command tooexecute includes a set of credentials.</span></span> <span data-ttu-id="27018-167">Ebben a példában hello parancs tooexecute nem lesznek titkosítva.</span><span class="sxs-lookup"><span data-stu-id="27018-167">In this example, hello command tooexecute will not be encrypted.</span></span>


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

<span data-ttu-id="27018-168">Áthelyezése hello **parancs tooexecute** tulajdonság toohello **védett** konfigurációs hello végrehajtási karakterlánc biztonságossá tételére.</span><span class="sxs-lookup"><span data-stu-id="27018-168">Moving hello **command tooexecute** property toohello **protected** configuration secures hello execution string.</span></span>

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

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="27018-169">Virtuálisgép-bővítmények hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="27018-169">Troubleshoot VM extensions</span></span>

<span data-ttu-id="27018-170">Minden egyes Virtuálisgép-bővítmény hibaelhárítási lépéseket adott toohello bővítmény is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="27018-170">Each VM extension may have troubleshooting steps specific toohello extension.</span></span> <span data-ttu-id="27018-171">Például ha az egyéni parancsprogramok futtatására szolgáló bővítmény hello használata esetén parancsfájl végrehajtásának részletes adatai található helyileg hello bővítmény futtatása hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="27018-171">For example, when you're using hello Custom Script extension, script execution details can be found locally on hello virtual machine on which hello extension was run.</span></span> <span data-ttu-id="27018-172">Bővítmény-specifikus hibaelhárítási lépéseket részletes leírást talál bővítmény vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="27018-172">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="27018-173">a következő hibaelhárítási lépéseket hello tooall virtuálisgép-bővítmények alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="27018-173">hello following troubleshooting steps apply tooall virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="27018-174">Bővítmény állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="27018-174">View extension status</span></span>

<span data-ttu-id="27018-175">A virtuálisgép-bővítmény futtatása a virtuális gépek ellen, után használja az hello Azure CLI parancs tooreturn bővítmény állapota a következő.</span><span class="sxs-lookup"><span data-stu-id="27018-175">After a virtual machine extension has been run against a virtual machine, use hello following Azure CLI command tooreturn extension status.</span></span> <span data-ttu-id="27018-176">Példa paraméterneveknek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="27018-176">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="27018-177">hello kimenete a következő szöveg hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="27018-177">hello output looks like hello following text:</span></span>

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

<span data-ttu-id="27018-178">Bővítmény végrehajtási állapotát hello Azure-portálon is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="27018-178">Extension execution status can also be found in hello Azure portal.</span></span> <span data-ttu-id="27018-179">egy bővítmény, jelölje be hello virtuális gép, tooview hello állapotának kiválasztása **bővítmények**, és válassza ki a kívánt bővítmény hello.</span><span class="sxs-lookup"><span data-stu-id="27018-179">tooview hello status of an extension, select hello virtual machine, choose **Extensions**, and select hello desired extension.</span></span>

### <a name="rerun-a-vm-extension"></a><span data-ttu-id="27018-180">Futtassa újra a Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-180">Rerun a VM extension</span></span>

<span data-ttu-id="27018-181">Előfordulhat, hogy esetekben, ahol a virtuálisgép-bővítmény kell toobe futtassa újra.</span><span class="sxs-lookup"><span data-stu-id="27018-181">There may be cases in which a virtual machine extension needs toobe rerun.</span></span> <span data-ttu-id="27018-182">Egy bővítmény akkor távolítsa el, majd futtassa újból a hello bővítmény egy végrehajtási módszer az Ön által választott újrafuttathatja.</span><span class="sxs-lookup"><span data-stu-id="27018-182">You can rerun an extension by removing it, and then rerunning hello extension with an execution method of your choice.</span></span> <span data-ttu-id="27018-183">egy bővítmény tooremove hello Azure CLI-parancsot a következő hello futtatásához.</span><span class="sxs-lookup"><span data-stu-id="27018-183">tooremove an extension, run hello following command with hello Azure CLI.</span></span> <span data-ttu-id="27018-184">Példa paraméterneveknek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="27018-184">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

<span data-ttu-id="27018-185">Kiterjesztéssel megszüntetheti a lépések az Azure-portálon hello hello használata:</span><span class="sxs-lookup"><span data-stu-id="27018-185">You can remove an extension by using hello following steps in hello Azure portal:</span></span>

1. <span data-ttu-id="27018-186">Válasszon ki egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="27018-186">Select a virtual machine.</span></span>
2. <span data-ttu-id="27018-187">Válasszon **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="27018-187">Choose **Extensions**.</span></span>
3. <span data-ttu-id="27018-188">Jelölje ki a kívánt hello bővítményt.</span><span class="sxs-lookup"><span data-stu-id="27018-188">Select hello desired extension.</span></span>
4. <span data-ttu-id="27018-189">Válasszon **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="27018-189">Choose **Uninstall**.</span></span>

## <a name="common-vm-extension-reference"></a><span data-ttu-id="27018-190">Virtuálisgép-bővítmény közös hivatkozása</span><span class="sxs-lookup"><span data-stu-id="27018-190">Common VM extension reference</span></span>
| <span data-ttu-id="27018-191">Bővítmény neve</span><span class="sxs-lookup"><span data-stu-id="27018-191">Extension name</span></span> | <span data-ttu-id="27018-192">Leírás</span><span class="sxs-lookup"><span data-stu-id="27018-192">Description</span></span> | <span data-ttu-id="27018-193">További információ</span><span class="sxs-lookup"><span data-stu-id="27018-193">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="27018-194">Linux rendszeren egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-194">Custom Script extension for Linux</span></span> |<span data-ttu-id="27018-195">Parancsfájlok futtatásához egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="27018-195">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="27018-196">Linux rendszeren egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-196">Custom Script extension for Linux</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="27018-197">Docker-bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-197">Docker extension</span></span> |<span data-ttu-id="27018-198">Telepítse a hello Docker démon toosupport távoli Docker parancsok.</span><span class="sxs-lookup"><span data-stu-id="27018-198">Install hello Docker daemon toosupport remote Docker commands.</span></span> |[<span data-ttu-id="27018-199">Docker virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-199">Docker VM extension</span></span>](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="27018-200">Virtuálisgép-hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-200">VM Access extension</span></span> |<span data-ttu-id="27018-201">Hozzáférés tooan Azure virtuális gép helyreállításához</span><span class="sxs-lookup"><span data-stu-id="27018-201">Regain access tooan Azure virtual machine</span></span> |[<span data-ttu-id="27018-202">Virtuálisgép-hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-202">VM Access extension</span></span>](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| <span data-ttu-id="27018-203">Azure Diagnostics-bővítményt</span><span class="sxs-lookup"><span data-stu-id="27018-203">Azure Diagnostics extension</span></span> |<span data-ttu-id="27018-204">Az Azure Diagnostics kezelése</span><span class="sxs-lookup"><span data-stu-id="27018-204">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="27018-205">Azure Diagnostics-bővítményt</span><span class="sxs-lookup"><span data-stu-id="27018-205">Azure Diagnostics extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="27018-206">Az Azure virtuális gép hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-206">Azure VM Access extension</span></span> |<span data-ttu-id="27018-207">Felhasználók és a hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="27018-207">Manage users and credentials</span></span> |[<span data-ttu-id="27018-208">Linux virtuális gép hozzáférési bővítmény</span><span class="sxs-lookup"><span data-stu-id="27018-208">VM Access extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
