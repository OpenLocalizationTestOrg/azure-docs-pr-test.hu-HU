---
title: "Windows virtuális gép aaaSelect lemezképek az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure PowerSHell toodetermine hello közzétevő, az ajánlat, SKU és verziója a piactér Virtuálisgép-lemezképek."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 752edcd0935f5141832e49503ae800ea0145e219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-windows-vm-images-in-hello-azure-marketplace-with-azure-powershell"></a><span data-ttu-id="9aaa2-103">Hogyan toofind Windows virtuális gép az Azure piactérről az Azure PowerShell hello lemezképek</span><span class="sxs-lookup"><span data-stu-id="9aaa2-103">How toofind Windows VM images in hello Azure Marketplace with Azure PowerShell</span></span>

<span data-ttu-id="9aaa2-104">Ez a témakör ismerteti, hogyan toouse Azure PowerShell toofind VM hello Azure piactér a lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-104">This topic describes how toouse Azure PowerShell toofind VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="9aaa2-105">Ezen információk toospecify Piactéri rendszerkép használja, a Windows virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-105">Use this information toospecify a Marketplace image when you create a Windows VM.</span></span>

<span data-ttu-id="9aaa2-106">Győződjön meg arról, hogy telepítve, és hello legújabb konfigurált [Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="9aaa2-106">Make sure that you installed and configured hello latest [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>



## <a name="table-of-commonly-used-windows-images"></a><span data-ttu-id="9aaa2-107">A gyakran használt Windows-lemezképek tábla</span><span class="sxs-lookup"><span data-stu-id="9aaa2-107">Table of commonly used Windows images</span></span>
| <span data-ttu-id="9aaa2-108">Közzétevő neve</span><span class="sxs-lookup"><span data-stu-id="9aaa2-108">PublisherName</span></span> | <span data-ttu-id="9aaa2-109">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="9aaa2-109">Offer</span></span> | <span data-ttu-id="9aaa2-110">SKU</span><span class="sxs-lookup"><span data-stu-id="9aaa2-110">Sku</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9aaa2-111">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-111">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9aaa2-112">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-112">WindowsServer</span></span> |<span data-ttu-id="9aaa2-113">2016-Datacenter</span><span class="sxs-lookup"><span data-stu-id="9aaa2-113">2016-Datacenter</span></span> |
| <span data-ttu-id="9aaa2-114">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-114">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9aaa2-115">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-115">WindowsServer</span></span> |<span data-ttu-id="9aaa2-116">2016-adatközpont-kiszolgálómag</span><span class="sxs-lookup"><span data-stu-id="9aaa2-116">2016-Datacenter-Server-Core</span></span> |
| <span data-ttu-id="9aaa2-117">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-117">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9aaa2-118">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-118">WindowsServer</span></span> |<span data-ttu-id="9aaa2-119">2016-adatközpont-az-tárolók</span><span class="sxs-lookup"><span data-stu-id="9aaa2-119">2016-Datacenter-with-Containers</span></span> |
| <span data-ttu-id="9aaa2-120">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-120">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9aaa2-121">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-121">WindowsServer</span></span> |<span data-ttu-id="9aaa2-122">2016-Nano-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9aaa2-122">2016-Nano-Server</span></span> |
| <span data-ttu-id="9aaa2-123">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-123">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9aaa2-124">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-124">WindowsServer</span></span> |<span data-ttu-id="9aaa2-125">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="9aaa2-125">2012-R2-Datacenter</span></span> |
| <span data-ttu-id="9aaa2-126">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-126">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9aaa2-127">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-127">WindowsServer</span></span> |<span data-ttu-id="9aaa2-128">2008 R2 SP1 CSOMAG</span><span class="sxs-lookup"><span data-stu-id="9aaa2-128">2008-R2-SP1</span></span> |
| <span data-ttu-id="9aaa2-129">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="9aaa2-129">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="9aaa2-130">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="9aaa2-130">DynamicsNAV</span></span> |<span data-ttu-id="9aaa2-131">2017</span><span class="sxs-lookup"><span data-stu-id="9aaa2-131">2017</span></span> |
| <span data-ttu-id="9aaa2-132">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="9aaa2-132">MicrosoftSharePoint</span></span> |<span data-ttu-id="9aaa2-133">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-133">MicrosoftSharePointServer</span></span> |<span data-ttu-id="9aaa2-134">2016</span><span class="sxs-lookup"><span data-stu-id="9aaa2-134">2016</span></span> |
| <span data-ttu-id="9aaa2-135">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-135">MicrosoftSQLServer</span></span> |<span data-ttu-id="9aaa2-136">SQL2016-WS2016</span><span class="sxs-lookup"><span data-stu-id="9aaa2-136">SQL2016-WS2016</span></span> |<span data-ttu-id="9aaa2-137">Enterprise</span><span class="sxs-lookup"><span data-stu-id="9aaa2-137">Enterprise</span></span> |
| <span data-ttu-id="9aaa2-138">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="9aaa2-138">MicrosoftSQLServer</span></span> |<span data-ttu-id="9aaa2-139">SQL2014SP2-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="9aaa2-139">SQL2014SP2-WS2012R2</span></span> |<span data-ttu-id="9aaa2-140">Enterprise</span><span class="sxs-lookup"><span data-stu-id="9aaa2-140">Enterprise</span></span> |
| <span data-ttu-id="9aaa2-141">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="9aaa2-141">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="9aaa2-142">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="9aaa2-142">WindowsServerHPCPack</span></span> |<span data-ttu-id="9aaa2-143">2012R2</span><span class="sxs-lookup"><span data-stu-id="9aaa2-143">2012R2</span></span> |
| <span data-ttu-id="9aaa2-144">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="9aaa2-144">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="9aaa2-145">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="9aaa2-145">WindowsServerEssentials</span></span> |<span data-ttu-id="9aaa2-146">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="9aaa2-146">WindowsServerEssentials</span></span> |

## <a name="find-specific-images"></a><span data-ttu-id="9aaa2-147">Adott rendszerképek keresése</span><span class="sxs-lookup"><span data-stu-id="9aaa2-147">Find specific images</span></span>


<span data-ttu-id="9aaa2-148">Amikor egy új virtuális gép létrehozása az Azure Resource Manager eszközzel, bizonyos esetekben van szüksége toospecify lemezkép hello kombinációja hello következő lemezkép tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-148">When creating a new virtual machine with Azure Resource Manager, in some cases you need toospecify an image with hello combination of hello following image properties:</span></span>

* <span data-ttu-id="9aaa2-149">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="9aaa2-149">Publisher</span></span>
* <span data-ttu-id="9aaa2-150">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="9aaa2-150">Offer</span></span>
* <span data-ttu-id="9aaa2-151">SKU</span><span class="sxs-lookup"><span data-stu-id="9aaa2-151">SKU</span></span>

<span data-ttu-id="9aaa2-152">Például használni ezeket az értékeket hello [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell-parancsmagot, vagy egy erőforrás-sablon, amelyben meg kell adnia a létrehozott virtuális gép toobe hello típusát.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-152">For example, use these values with hello [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell cmdlet, or with a resource group template in which you must specify hello type of VM toobe created.</span></span>

<span data-ttu-id="9aaa2-153">Ha toodetermine kell ezeket az értékeket, futtathatja hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), és [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) parancsmagok toonavigate hello képek.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-153">If you need toodetermine these values, you can run hello [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), and [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets toonavigate hello images.</span></span> <span data-ttu-id="9aaa2-154">Meghatározhatja, hogy ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-154">You determine these values:</span></span>

1. <span data-ttu-id="9aaa2-155">Lista hello kép közzétevők.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-155">List hello image publishers.</span></span>
2. <span data-ttu-id="9aaa2-156">Listázza egy adott közzétevő ajánlatait.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-156">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="9aaa2-157">Listázza egy adott ajánlathoz tartozó termékváltozatokat.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-157">For a given offer, list their SKUs.</span></span>

<span data-ttu-id="9aaa2-158">Először listában hello közzétevők a hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-158">First, list hello publishers with hello following commands:</span></span>

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

<span data-ttu-id="9aaa2-159">Adja meg a kiválasztott közzétevő neve, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-159">Fill in your chosen publisher name and run hello following commands:</span></span>

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="9aaa2-160">Adja meg a kiválasztott ajánlat nevét, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-160">Fill in your chosen offer name and run hello following commands:</span></span>

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="9aaa2-161">Hello hello kimenetét a `Get-AzureRMVMImageSku` parancsot egy új virtuális gép toospecify hello lemezkép szükséges összes hello információt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-161">From hello output of hello `Get-AzureRMVMImageSku` command, you have all hello information you need toospecify hello image for a new virtual machine.</span></span>

<span data-ttu-id="9aaa2-162">hello következő teljes példa látható:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-162">hello following shows a full example:</span></span>

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

<span data-ttu-id="9aaa2-163">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-163">Output:</span></span>

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

<span data-ttu-id="9aaa2-164">Hello "MicrosoftWindowsServer" közzétevő:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-164">For hello "MicrosoftWindowsServer" publisher:</span></span>

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="9aaa2-165">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-165">Output:</span></span>

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

<span data-ttu-id="9aaa2-166">Hello "WindowsServer" szolgáltatásokat:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-166">For hello "WindowsServer" offer:</span></span>

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="9aaa2-167">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9aaa2-167">Output:</span></span>

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

<span data-ttu-id="9aaa2-168">A listáról, másolja a kiválasztott termékváltozat hello, és rendelkezik az összes hello adatot hello `Set-AzureRMVMSourceImage` PowerShell-parancsmagot, vagy egy erőforrás-sablon.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-168">From this list, copy hello chosen SKU name, and you have all hello information for hello `Set-AzureRMVMSourceImage` PowerShell cmdlet or for a resource group template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9aaa2-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9aaa2-169">Next steps</span></span>
<span data-ttu-id="9aaa2-170">Most kiválaszthatja, hogy pontosan hello kép toouse szeretné.</span><span class="sxs-lookup"><span data-stu-id="9aaa2-170">Now you can choose precisely hello image you want toouse.</span></span> <span data-ttu-id="9aaa2-171">toocreate egy virtuális gép hello kép információk, csak talált, amely segítségével gyorsan lásd [Windows virtuális gép létrehozása a PowerShell-lel](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9aaa2-171">toocreate a virtual machine quickly by using hello image information, which you just found, see [Create a Windows virtual machine with PowerShell](quick-create-powershell.md).</span></span>
