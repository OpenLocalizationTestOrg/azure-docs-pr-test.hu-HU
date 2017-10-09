---
title: "a Windows hálózati figyelő ügynök virtuálisgép-bővítmény aaaAzure |} Microsoft Docs"
description: "Hello hálózati figyelő ügynök a Windows virtuális gépet egy virtuálisgép-bővítmény telepítése."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 21298706e462ff32c4d314f9a1ad127074ddf481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a><span data-ttu-id="44938-103">Windows hálózati figyelő ügynök virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="44938-103">Network Watcher Agent virtual machine extension for Windows</span></span>

## <a name="overview"></a><span data-ttu-id="44938-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="44938-104">Overview</span></span>

<span data-ttu-id="44938-105">[Az Azure hálózati figyelőt](https://review.docs.microsoft.com/en-us/azure/network-watcher/) hálózati teljesítmény figyelési, diagnosztikai és elemzési szolgáltatás, amely lehetővé teszi az Azure-hálózatok figyelését.</span><span class="sxs-lookup"><span data-stu-id="44938-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="44938-106">Hálózati figyelő ügynök virtuálisgép-bővítmény hello feltétele néhány hello hálózati figyelőt szolgáltatást az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="44938-106">hello Network Watcher Agent virtual machine extension is a requirement for some of hello Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="44938-107">Ez magában foglalja, igény szerint és egyéb speciális funkciók a hálózati forgalom rögzítése.</span><span class="sxs-lookup"><span data-stu-id="44938-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="44938-108">Ez a dokumentum részletek hello támogatott platformokat, a központi telepítési beállítások hello hálózati figyelő ügynök virtuálisgép-bővítmény Windows.</span><span class="sxs-lookup"><span data-stu-id="44938-108">This document details hello supported platforms and deployment options for hello Network Watcher Agent virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44938-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="44938-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="44938-110">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="44938-110">Operating system</span></span>

<span data-ttu-id="44938-111">Hálózati figyelő ügynök bővítmény hello a Windows is futtathatók a Windows Server 2008 R2, 2012, 2012 R2 és 2016 kiadását.</span><span class="sxs-lookup"><span data-stu-id="44938-111">hello Network Watcher Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span> <span data-ttu-id="44938-112">Vegye figyelembe, hogy a Nano Server hello jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="44938-112">Note that hello Nano Server is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="44938-113">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="44938-113">Internet connectivity</span></span>

<span data-ttu-id="44938-114">Egyes hálózati figyelő ügynök funkcionalitást hello használatához hello cél virtuális gép csatlakoztatott toohello Internet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="44938-114">Some of hello Network Watcher Agent functionality requires that hello target virtual machine be connected toohello Internet.</span></span> <span data-ttu-id="44938-115">Hello képességét tooestablish kimenő kapcsolatok nélkül néhány hello hálózati figyelő ügynök szolgáltatást előfordulhat, hogy hibás működését, vagy már nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="44938-115">Without hello ability tooestablish outgoing connections some of hello Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="44938-116">További részletekért lásd: hello [hálózati figyelőt dokumentáció](../../network-watcher/network-watcher-monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44938-116">For more details, please see hello [Network Watcher documentation](../../network-watcher/network-watcher-monitoring-overview.md).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="44938-117">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="44938-117">Extension schema</span></span>

<span data-ttu-id="44938-118">hello következő JSON látható hello hálózati figyelő ügynök bővítmény hello sémáját.</span><span class="sxs-lookup"><span data-stu-id="44938-118">hello following JSON shows hello schema for hello Network Watcher Agent extension.</span></span> <span data-ttu-id="44938-119">hello bővítmény sem szükséges és nem támogatja a megadott felhasználó által megadott beállításokat jelenleg és az alapértelmezett beállításon támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="44938-119">hello extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a><span data-ttu-id="44938-120">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="44938-120">Property values</span></span>

| <span data-ttu-id="44938-121">Név</span><span class="sxs-lookup"><span data-stu-id="44938-121">Name</span></span> | <span data-ttu-id="44938-122">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="44938-122">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="44938-123">apiVersion</span><span class="sxs-lookup"><span data-stu-id="44938-123">apiVersion</span></span> | <span data-ttu-id="44938-124">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="44938-124">2015-06-15</span></span> |
| <span data-ttu-id="44938-125">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="44938-125">publisher</span></span> | <span data-ttu-id="44938-126">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="44938-126">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="44938-127">type</span><span class="sxs-lookup"><span data-stu-id="44938-127">type</span></span> | <span data-ttu-id="44938-128">NetworkWatcherAgentWindows</span><span class="sxs-lookup"><span data-stu-id="44938-128">NetworkWatcherAgentWindows</span></span> |
| <span data-ttu-id="44938-129">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="44938-129">typeHandlerVersion</span></span> | <span data-ttu-id="44938-130">1.4</span><span class="sxs-lookup"><span data-stu-id="44938-130">1.4</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="44938-131">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="44938-131">Template deployment</span></span>

<span data-ttu-id="44938-132">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="44938-132">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="44938-133">hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello hálózati figyelő ügynöke bővítmény egy Azure Resource Manager sablon telepítése során használható.</span><span class="sxs-lookup"><span data-stu-id="44938-133">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="44938-134">PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="44938-134">PowerShell deployment</span></span>

<span data-ttu-id="44938-135">Hello `Set-AzureRmVMExtension` parancs lehet használt toodeploy hello hálózati figyelő ügynök virtuális gép bővítmény tooan meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="44938-135">hello `Set-AzureRmVMExtension` command can be used toodeploy hello Network Watcher Agent virtual machine extension tooan existing virtual machine.</span></span>

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="44938-136">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="44938-136">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="44938-137">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="44938-137">Troubleshooting</span></span>

<span data-ttu-id="44938-138">A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni az Azure-portálon hello és hello Azure PowerShell modul használatával.</span><span class="sxs-lookup"><span data-stu-id="44938-138">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="44938-139">egy adott virtuális Gépet, a következő parancs használatával futtatási hello kiterjesztéseinek toosee hello telepítési állapota hello Azure PowerShell modul.</span><span class="sxs-lookup"><span data-stu-id="44938-139">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

<span data-ttu-id="44938-140">Bővítmény végrehajtási kimeneti naplózott toofiles hello található a következő könyvtár:</span><span class="sxs-lookup"><span data-stu-id="44938-140">Extension execution output is logged toofiles found in hello following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a><span data-ttu-id="44938-141">Támogatás</span><span class="sxs-lookup"><span data-stu-id="44938-141">Support</span></span>

<span data-ttu-id="44938-142">Ha ez a cikk bármely pontján további segítségre van szüksége, toohello megfigyelő felhasználói útmutató dokumentációjában talál, vagy lépjen kapcsolatba az hello Azure hello szakértői [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="44938-142">If you need more help at any point in this article, you can refer toohello Network Watcher User Guide documentation or contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="44938-143">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="44938-143">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="44938-144">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást.</span><span class="sxs-lookup"><span data-stu-id="44938-144">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="44938-145">Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="44938-145">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
