---
title: "Hálózati figyelő ügynök virtuálisgép-bővítmény Linux aaaAzure |} Microsoft Docs"
description: "Hello hálózati figyelő ügynök a Linux virtuális gépet egy virtuálisgép-bővítmény telepítése."
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
ms.openlocfilehash: 84bed132cbda83d0917be490f9a50914578952a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a><span data-ttu-id="9ce71-103">Hálózati figyelő ügynök virtuálisgép-bővítmény Linux</span><span class="sxs-lookup"><span data-stu-id="9ce71-103">Network Watcher Agent virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="9ce71-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9ce71-104">Overview</span></span>

<span data-ttu-id="9ce71-105">[Az Azure hálózati figyelőt](https://review.docs.microsoft.com/en-us/azure/network-watcher/) hálózati teljesítmény figyelési, diagnosztikai és elemzési szolgáltatás, amely lehetővé teszi az Azure-hálózatok figyelését.</span><span class="sxs-lookup"><span data-stu-id="9ce71-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="9ce71-106">Hálózati figyelő ügynök virtuálisgép-bővítmény hello feltétele néhány hello hálózati figyelőt szolgáltatást az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="9ce71-106">hello Network Watcher Agent virtual machine extension is a requirement for some of hello Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="9ce71-107">Ez magában foglalja, igény szerint és egyéb speciális funkciók a hálózati forgalom rögzítése.</span><span class="sxs-lookup"><span data-stu-id="9ce71-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="9ce71-108">Ez a dokumentum részletek hello támogatott platformokat, a központi telepítési beállítások hello hálózati figyelő ügynök virtuálisgép-bővítmény Linux.</span><span class="sxs-lookup"><span data-stu-id="9ce71-108">This document details hello supported platforms and deployment options for hello Network Watcher Agent virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ce71-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ce71-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="9ce71-110">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="9ce71-110">Operating system</span></span>

<span data-ttu-id="9ce71-111">Hálózati figyelő ügynök bővítmény hello is futtathatók a a Linux terjesztéseket:</span><span class="sxs-lookup"><span data-stu-id="9ce71-111">hello Network Watcher Agent extension can be run against these Linux distributions:</span></span>

| <span data-ttu-id="9ce71-112">Disztribúció</span><span class="sxs-lookup"><span data-stu-id="9ce71-112">Distribution</span></span> | <span data-ttu-id="9ce71-113">Verzió</span><span class="sxs-lookup"><span data-stu-id="9ce71-113">Version</span></span> |
|---|---|
| <span data-ttu-id="9ce71-114">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="9ce71-114">Ubuntu</span></span> | <span data-ttu-id="9ce71-115">16.04 LTS, 14.04 LTS és 12.04 LTS</span><span class="sxs-lookup"><span data-stu-id="9ce71-115">16.04 LTS, 14.04 LTS and 12.04 LTS</span></span> |
| <span data-ttu-id="9ce71-116">Debian</span><span class="sxs-lookup"><span data-stu-id="9ce71-116">Debian</span></span> | <span data-ttu-id="9ce71-117">7. és 8</span><span class="sxs-lookup"><span data-stu-id="9ce71-117">7 and 8</span></span> |
| <span data-ttu-id="9ce71-118">RedHat</span><span class="sxs-lookup"><span data-stu-id="9ce71-118">RedHat</span></span> | <span data-ttu-id="9ce71-119">6.x, 7.x és</span><span class="sxs-lookup"><span data-stu-id="9ce71-119">6.x and 7.x</span></span> |
| <span data-ttu-id="9ce71-120">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="9ce71-120">Oracle Linux</span></span> | <span data-ttu-id="9ce71-121">7 x</span><span class="sxs-lookup"><span data-stu-id="9ce71-121">7x</span></span> |
| <span data-ttu-id="9ce71-122">SUSE</span><span class="sxs-lookup"><span data-stu-id="9ce71-122">Suse</span></span> | <span data-ttu-id="9ce71-123">11 és 12</span><span class="sxs-lookup"><span data-stu-id="9ce71-123">11 and 12</span></span> |
| <span data-ttu-id="9ce71-124">OpenSuse</span><span class="sxs-lookup"><span data-stu-id="9ce71-124">OpenSuse</span></span> | <span data-ttu-id="9ce71-125">7.0</span><span class="sxs-lookup"><span data-stu-id="9ce71-125">7.0</span></span> |
| <span data-ttu-id="9ce71-126">CentOS</span><span class="sxs-lookup"><span data-stu-id="9ce71-126">CentOS</span></span> | <span data-ttu-id="9ce71-127">7.0</span><span class="sxs-lookup"><span data-stu-id="9ce71-127">7.0</span></span> |

<span data-ttu-id="9ce71-128">Vegye figyelembe, hogy CoreOS jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="9ce71-128">Note that CoreOS is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="9ce71-129">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="9ce71-129">Internet connectivity</span></span>

<span data-ttu-id="9ce71-130">Egyes hálózati figyelő ügynök funkcionalitást hello használatához hello cél virtuális gép csatlakoztatott toohello Internet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9ce71-130">Some of hello Network Watcher Agent functionality requires that hello target virtual machine be connected toohello Internet.</span></span> <span data-ttu-id="9ce71-131">Hello képességét tooestablish kimenő kapcsolatok nélkül néhány hello hálózati figyelő ügynök szolgáltatást előfordulhat, hogy hibás működését, vagy már nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="9ce71-131">Without hello ability tooestablish outgoing connections some of hello Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="9ce71-132">További részletekért lásd: hello [hálózati figyelőt dokumentáció](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span><span class="sxs-lookup"><span data-stu-id="9ce71-132">For more details, please see hello [Network Watcher documentation](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="9ce71-133">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="9ce71-133">Extension schema</span></span>

<span data-ttu-id="9ce71-134">hello következő JSON látható hello hálózati figyelő ügynök bővítmény hello sémáját.</span><span class="sxs-lookup"><span data-stu-id="9ce71-134">hello following JSON shows hello schema for hello Network Watcher Agent extension.</span></span> <span data-ttu-id="9ce71-135">hello bővítmény sem szükséges és nem támogatja a megadott felhasználó által megadott beállításokat jelenleg és az alapértelmezett beállításon támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="9ce71-135">hello extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="9ce71-136">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="9ce71-136">Property values</span></span>

| <span data-ttu-id="9ce71-137">Név</span><span class="sxs-lookup"><span data-stu-id="9ce71-137">Name</span></span> | <span data-ttu-id="9ce71-138">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="9ce71-138">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="9ce71-139">apiVersion</span><span class="sxs-lookup"><span data-stu-id="9ce71-139">apiVersion</span></span> | <span data-ttu-id="9ce71-140">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="9ce71-140">2015-06-15</span></span> |
| <span data-ttu-id="9ce71-141">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="9ce71-141">publisher</span></span> | <span data-ttu-id="9ce71-142">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="9ce71-142">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="9ce71-143">type</span><span class="sxs-lookup"><span data-stu-id="9ce71-143">type</span></span> | <span data-ttu-id="9ce71-144">NetworkWatcherAgentLinux</span><span class="sxs-lookup"><span data-stu-id="9ce71-144">NetworkWatcherAgentLinux</span></span> |
| <span data-ttu-id="9ce71-145">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="9ce71-145">typeHandlerVersion</span></span> | <span data-ttu-id="9ce71-146">1.4</span><span class="sxs-lookup"><span data-stu-id="9ce71-146">1.4</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="9ce71-147">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="9ce71-147">Template deployment</span></span>

<span data-ttu-id="9ce71-148">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="9ce71-148">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="9ce71-149">hello JSON-séma hello előző szakaszban ismertetett az Azure Resource Manager sablon toorun hello hálózati figyelő ügynöke bővítmény egy Azure Resource Manager sablon telepítése során használható.</span><span class="sxs-lookup"><span data-stu-id="9ce71-149">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="azure-cli-deployment"></a><span data-ttu-id="9ce71-150">Az Azure CLI-telepítés</span><span class="sxs-lookup"><span data-stu-id="9ce71-150">Azure CLI deployment</span></span>

<span data-ttu-id="9ce71-151">hello Azure CLI használt toodeploy hello hálózati figyelő ügynök VM bővítmény tooan meglévő virtuális gép is lehet.</span><span class="sxs-lookup"><span data-stu-id="9ce71-151">hello Azure CLI can be used toodeploy hello Network Watcher Agent VM extension tooan existing virtual machine.</span></span>

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="9ce71-152">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="9ce71-152">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="9ce71-153">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9ce71-153">Troubleshooting</span></span>

<span data-ttu-id="9ce71-154">A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni a hello Azure-portálon, és hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="9ce71-154">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure CLI.</span></span> <span data-ttu-id="9ce71-155">egy adott virtuális Gépet, futtassa a következő parancs használatával hello kiterjesztéseinek toosee hello telepítési állapota hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="9ce71-155">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure CLI.</span></span>

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

<span data-ttu-id="9ce71-156">Bővítmény végrehajtási kimeneti naplózott toofiles hello található a következő könyvtár:</span><span class="sxs-lookup"><span data-stu-id="9ce71-156">Extension execution output is logged toofiles found in hello following directory:</span></span>

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a><span data-ttu-id="9ce71-157">Támogatás</span><span class="sxs-lookup"><span data-stu-id="9ce71-157">Support</span></span>

<span data-ttu-id="9ce71-158">Ha ez a cikk bármely pontján további segítségre van szüksége, tekintse meg a toohello hálózati figyelőt dokumentációját, vagy forduljon hello Azure hello szakértői [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="9ce71-158">If you need more help at any point in this article, you can refer toohello Network Watcher documentation or contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="9ce71-159">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="9ce71-159">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="9ce71-160">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást.</span><span class="sxs-lookup"><span data-stu-id="9ce71-160">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="9ce71-161">Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="9ce71-161">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
