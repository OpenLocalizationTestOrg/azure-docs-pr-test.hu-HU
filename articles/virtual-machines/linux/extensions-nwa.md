---
title: "Az Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux |} Microsoft Docs"
description: "A hálózati figyelő ügynök a Linux virtuális gépet egy virtuálisgép-bővítmény telepítése."
services: virtual-machines-linux
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: eaadd531b9e05a54446e61f98584ae9d75470a5f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a><span data-ttu-id="0b61e-103">Hálózati figyelő ügynök virtuálisgép-bővítmény Linux</span><span class="sxs-lookup"><span data-stu-id="0b61e-103">Network Watcher Agent virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="0b61e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b61e-104">Overview</span></span>

<span data-ttu-id="0b61e-105">[Az Azure hálózati figyelőt](https://review.docs.microsoft.com/en-us/azure/network-watcher/) hálózati teljesítmény figyelési, diagnosztikai és elemzési szolgáltatás, amely lehetővé teszi az Azure-hálózatok figyelését.</span><span class="sxs-lookup"><span data-stu-id="0b61e-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="0b61e-106">A hálózati figyelő ügynök virtuálisgép-bővítményt az egyes hálózati figyelőt szolgáltatásokat az Azure virtuális gépeken működik.</span><span class="sxs-lookup"><span data-stu-id="0b61e-106">The Network Watcher Agent virtual machine extension is a requirement for some of the Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="0b61e-107">Ez magában foglalja, igény szerint és egyéb speciális funkciók a hálózati forgalom rögzítése.</span><span class="sxs-lookup"><span data-stu-id="0b61e-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="0b61e-108">Ez a dokumentum részletesen a támogatott platformokról és a Linux hálózati figyelő ügynök virtuálisgép-bővítmény vonatkozó telepítési lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="0b61e-108">This document details the supported platforms and deployment options for the Network Watcher Agent virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b61e-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0b61e-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="0b61e-110">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="0b61e-110">Operating system</span></span>

<span data-ttu-id="0b61e-111">A hálózati figyelő ügynök bővítmény is futtathatók a a Linux terjesztéseket:</span><span class="sxs-lookup"><span data-stu-id="0b61e-111">The Network Watcher Agent extension can be run against these Linux distributions:</span></span>

| <span data-ttu-id="0b61e-112">Disztribúció</span><span class="sxs-lookup"><span data-stu-id="0b61e-112">Distribution</span></span> | <span data-ttu-id="0b61e-113">Verzió</span><span class="sxs-lookup"><span data-stu-id="0b61e-113">Version</span></span> |
|---|---|
| <span data-ttu-id="0b61e-114">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="0b61e-114">Ubuntu</span></span> | <span data-ttu-id="0b61e-115">16.04 LTS, 14.04 LTS és 12.04 LTS</span><span class="sxs-lookup"><span data-stu-id="0b61e-115">16.04 LTS, 14.04 LTS and 12.04 LTS</span></span> |
| <span data-ttu-id="0b61e-116">Debian</span><span class="sxs-lookup"><span data-stu-id="0b61e-116">Debian</span></span> | <span data-ttu-id="0b61e-117">7. és 8</span><span class="sxs-lookup"><span data-stu-id="0b61e-117">7 and 8</span></span> |
| <span data-ttu-id="0b61e-118">RedHat</span><span class="sxs-lookup"><span data-stu-id="0b61e-118">RedHat</span></span> | <span data-ttu-id="0b61e-119">6.x, 7.x és</span><span class="sxs-lookup"><span data-stu-id="0b61e-119">6.x and 7.x</span></span> |
| <span data-ttu-id="0b61e-120">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="0b61e-120">Oracle Linux</span></span> | <span data-ttu-id="0b61e-121">7 x</span><span class="sxs-lookup"><span data-stu-id="0b61e-121">7x</span></span> |
| <span data-ttu-id="0b61e-122">SUSE</span><span class="sxs-lookup"><span data-stu-id="0b61e-122">Suse</span></span> | <span data-ttu-id="0b61e-123">11 és 12</span><span class="sxs-lookup"><span data-stu-id="0b61e-123">11 and 12</span></span> |
| <span data-ttu-id="0b61e-124">OpenSuse</span><span class="sxs-lookup"><span data-stu-id="0b61e-124">OpenSuse</span></span> | <span data-ttu-id="0b61e-125">7.0</span><span class="sxs-lookup"><span data-stu-id="0b61e-125">7.0</span></span> |
| <span data-ttu-id="0b61e-126">CentOS</span><span class="sxs-lookup"><span data-stu-id="0b61e-126">CentOS</span></span> | <span data-ttu-id="0b61e-127">7.0</span><span class="sxs-lookup"><span data-stu-id="0b61e-127">7.0</span></span> |

<span data-ttu-id="0b61e-128">Vegye figyelembe, hogy CoreOS jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="0b61e-128">Note that CoreOS is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="0b61e-129">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="0b61e-129">Internet connectivity</span></span>

<span data-ttu-id="0b61e-130">A hálózati figyelő ügynök funkciók némelyike megköveteli, hogy a cél virtuális gép kapcsolódnia kell az internetre.</span><span class="sxs-lookup"><span data-stu-id="0b61e-130">Some of the Network Watcher Agent functionality requires that the target virtual machine be connected to the Internet.</span></span> <span data-ttu-id="0b61e-131">Kimenő kapcsolatokat képessége nélkül egyes hálózati figyelő ügynök szolgáltatásokat lehet, hogy hibás működését, vagy már nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="0b61e-131">Without the ability to establish outgoing connections some of the Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="0b61e-132">További részletekért lásd: a [hálózati figyelőt dokumentáció](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span><span class="sxs-lookup"><span data-stu-id="0b61e-132">For more details, please see the [Network Watcher documentation](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="0b61e-133">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="0b61e-133">Extension schema</span></span>

<span data-ttu-id="0b61e-134">A következő JSON jeleníti meg a hálózati figyelő ügynök bővítmény sémáját.</span><span class="sxs-lookup"><span data-stu-id="0b61e-134">The following JSON shows the schema for the Network Watcher Agent extension.</span></span> <span data-ttu-id="0b61e-135">A bővítmény sem szükséges és nem támogatja a megadott felhasználó által megadott beállításokat jelenleg és az alapértelmezett beállításon támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="0b61e-135">The extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

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
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a><span data-ttu-id="0b61e-136">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="0b61e-136">Property values</span></span>

| <span data-ttu-id="0b61e-137">Név</span><span class="sxs-lookup"><span data-stu-id="0b61e-137">Name</span></span> | <span data-ttu-id="0b61e-138">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="0b61e-138">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="0b61e-139">apiVersion</span><span class="sxs-lookup"><span data-stu-id="0b61e-139">apiVersion</span></span> | <span data-ttu-id="0b61e-140">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="0b61e-140">2015-06-15</span></span> |
| <span data-ttu-id="0b61e-141">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="0b61e-141">publisher</span></span> | <span data-ttu-id="0b61e-142">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="0b61e-142">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="0b61e-143">type</span><span class="sxs-lookup"><span data-stu-id="0b61e-143">type</span></span> | <span data-ttu-id="0b61e-144">NetworkWatcherAgentLinux</span><span class="sxs-lookup"><span data-stu-id="0b61e-144">NetworkWatcherAgentLinux</span></span> |
| <span data-ttu-id="0b61e-145">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="0b61e-145">typeHandlerVersion</span></span> | <span data-ttu-id="0b61e-146">1.4</span><span class="sxs-lookup"><span data-stu-id="0b61e-146">1.4</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="0b61e-147">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="0b61e-147">Template deployment</span></span>

<span data-ttu-id="0b61e-148">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="0b61e-148">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="0b61e-149">Az előző szakaszban ismertetett JSON-séma segítségével az Azure Resource Manager-sablonok az Azure Resource Manager sablon telepítése során a hálózati figyelő ügynök bővítmény futtatása.</span><span class="sxs-lookup"><span data-stu-id="0b61e-149">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="azure-cli-deployment"></a><span data-ttu-id="0b61e-150">Az Azure CLI-telepítés</span><span class="sxs-lookup"><span data-stu-id="0b61e-150">Azure CLI deployment</span></span>

<span data-ttu-id="0b61e-151">Az Azure CLI segítségével a hálózati figyelő ügynök Virtuálisgép-bővítmény telepítése egy meglévő virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="0b61e-151">The Azure CLI can be used to deploy the Network Watcher Agent VM extension to an existing virtual machine.</span></span>

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="0b61e-152">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="0b61e-152">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="0b61e-153">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="0b61e-153">Troubleshooting</span></span>

<span data-ttu-id="0b61e-154">Bővítmény központi telepítések állapotára vonatkozó lehet adatokat beolvasni az Azure-portálon, és az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="0b61e-154">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure CLI.</span></span> <span data-ttu-id="0b61e-155">A megadott virtuális gépek bővítmények központi telepítési állapotának megtekintéséhez a következő parancsot az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="0b61e-155">To see the deployment state of extensions for a given VM, run the following command using the Azure CLI.</span></span>

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

<span data-ttu-id="0b61e-156">A következő könyvtárban található fájlok kerül a bővítmény végrehajtás kimenetének:</span><span class="sxs-lookup"><span data-stu-id="0b61e-156">Extension execution output is logged to files found in the following directory:</span></span>

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a><span data-ttu-id="0b61e-157">Támogatás</span><span class="sxs-lookup"><span data-stu-id="0b61e-157">Support</span></span>

<span data-ttu-id="0b61e-158">Ha ez a cikk bármely pontján további segítségre van szüksége, tekintse meg a hálózati figyelőt dokumentációját, vagy lépjen kapcsolatba az Azure-szakértők a a [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="0b61e-158">If you need more help at any point in this article, you can refer to the Network Watcher documentation or contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="0b61e-159">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="0b61e-159">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="0b61e-160">Lépjen a [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="0b61e-160">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="0b61e-161">Támogatja az Azure használatával kapcsolatos információkért olvassa el a [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="0b61e-161">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
