---
title: "Windows Virtuálisgép-rendszerképek kiválasztása az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan használhatja az Azure Powershellt közzétevő, az ajánlat, SKU, és verziója a piactér Virtuálisgép-rendszerképek meghatározásához."
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
ms.openlocfilehash: 814ae260123c045d4b6766bf4b312f874cd77068
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a><span data-ttu-id="5e932-103">Az Azure piactéren az Azure PowerShell Windows Virtuálisgép-lemezképek megkeresése</span><span class="sxs-lookup"><span data-stu-id="5e932-103">How to find Windows VM images in the Azure Marketplace with Azure PowerShell</span></span>

<span data-ttu-id="5e932-104">Ez a témakör ismerteti az Azure PowerShell használata az Azure piactéren Virtuálisgép-rendszerképek kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="5e932-104">This topic describes how to use Azure PowerShell to find VM images in the Azure Marketplace.</span></span> <span data-ttu-id="5e932-105">Ezen információk használatával adja meg a Piactéri lemezkép a Windows virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="5e932-105">Use this information to specify a Marketplace image when you create a Windows VM.</span></span>

<span data-ttu-id="5e932-106">Győződjön meg arról, hogy telepített és konfigurált legújabb [Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="5e932-106">Make sure that you installed and configured the latest [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>



## <a name="table-of-commonly-used-windows-images"></a><span data-ttu-id="5e932-107">A gyakran használt Windows-lemezképek tábla</span><span class="sxs-lookup"><span data-stu-id="5e932-107">Table of commonly used Windows images</span></span>
| <span data-ttu-id="5e932-108">Közzétevő neve</span><span class="sxs-lookup"><span data-stu-id="5e932-108">PublisherName</span></span> | <span data-ttu-id="5e932-109">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="5e932-109">Offer</span></span> | <span data-ttu-id="5e932-110">SKU</span><span class="sxs-lookup"><span data-stu-id="5e932-110">Sku</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="5e932-111">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-111">MicrosoftWindowsServer</span></span> |<span data-ttu-id="5e932-112">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-112">WindowsServer</span></span> |<span data-ttu-id="5e932-113">2016-Datacenter</span><span class="sxs-lookup"><span data-stu-id="5e932-113">2016-Datacenter</span></span> |
| <span data-ttu-id="5e932-114">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-114">MicrosoftWindowsServer</span></span> |<span data-ttu-id="5e932-115">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-115">WindowsServer</span></span> |<span data-ttu-id="5e932-116">2016-adatközpont-kiszolgálómag</span><span class="sxs-lookup"><span data-stu-id="5e932-116">2016-Datacenter-Server-Core</span></span> |
| <span data-ttu-id="5e932-117">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-117">MicrosoftWindowsServer</span></span> |<span data-ttu-id="5e932-118">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-118">WindowsServer</span></span> |<span data-ttu-id="5e932-119">2016-adatközpont-az-tárolók</span><span class="sxs-lookup"><span data-stu-id="5e932-119">2016-Datacenter-with-Containers</span></span> |
| <span data-ttu-id="5e932-120">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-120">MicrosoftWindowsServer</span></span> |<span data-ttu-id="5e932-121">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-121">WindowsServer</span></span> |<span data-ttu-id="5e932-122">2016-Nano-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="5e932-122">2016-Nano-Server</span></span> |
| <span data-ttu-id="5e932-123">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-123">MicrosoftWindowsServer</span></span> |<span data-ttu-id="5e932-124">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-124">WindowsServer</span></span> |<span data-ttu-id="5e932-125">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="5e932-125">2012-R2-Datacenter</span></span> |
| <span data-ttu-id="5e932-126">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-126">MicrosoftWindowsServer</span></span> |<span data-ttu-id="5e932-127">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5e932-127">WindowsServer</span></span> |<span data-ttu-id="5e932-128">2008 R2 SP1 CSOMAG</span><span class="sxs-lookup"><span data-stu-id="5e932-128">2008-R2-SP1</span></span> |
| <span data-ttu-id="5e932-129">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="5e932-129">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="5e932-130">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="5e932-130">DynamicsNAV</span></span> |<span data-ttu-id="5e932-131">2017</span><span class="sxs-lookup"><span data-stu-id="5e932-131">2017</span></span> |
| <span data-ttu-id="5e932-132">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="5e932-132">MicrosoftSharePoint</span></span> |<span data-ttu-id="5e932-133">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="5e932-133">MicrosoftSharePointServer</span></span> |<span data-ttu-id="5e932-134">2016</span><span class="sxs-lookup"><span data-stu-id="5e932-134">2016</span></span> |
| <span data-ttu-id="5e932-135">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="5e932-135">MicrosoftSQLServer</span></span> |<span data-ttu-id="5e932-136">SQL2016-WS2016</span><span class="sxs-lookup"><span data-stu-id="5e932-136">SQL2016-WS2016</span></span> |<span data-ttu-id="5e932-137">Enterprise</span><span class="sxs-lookup"><span data-stu-id="5e932-137">Enterprise</span></span> |
| <span data-ttu-id="5e932-138">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="5e932-138">MicrosoftSQLServer</span></span> |<span data-ttu-id="5e932-139">SQL2014SP2-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="5e932-139">SQL2014SP2-WS2012R2</span></span> |<span data-ttu-id="5e932-140">Enterprise</span><span class="sxs-lookup"><span data-stu-id="5e932-140">Enterprise</span></span> |
| <span data-ttu-id="5e932-141">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="5e932-141">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="5e932-142">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="5e932-142">WindowsServerHPCPack</span></span> |<span data-ttu-id="5e932-143">2012R2</span><span class="sxs-lookup"><span data-stu-id="5e932-143">2012R2</span></span> |
| <span data-ttu-id="5e932-144">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="5e932-144">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="5e932-145">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="5e932-145">WindowsServerEssentials</span></span> |<span data-ttu-id="5e932-146">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="5e932-146">WindowsServerEssentials</span></span> |

## <a name="find-specific-images"></a><span data-ttu-id="5e932-147">Adott rendszerképek keresése</span><span class="sxs-lookup"><span data-stu-id="5e932-147">Find specific images</span></span>


<span data-ttu-id="5e932-148">Amikor új virtuális gépet hoz létre az Azure Resource Managerrel, néhány esetben meg kell határoznia egy rendszerképet a következő tulajdonságok kombinációja alapján:</span><span class="sxs-lookup"><span data-stu-id="5e932-148">When creating a new virtual machine with Azure Resource Manager, in some cases you need to specify an image with the combination of the following image properties:</span></span>

* <span data-ttu-id="5e932-149">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="5e932-149">Publisher</span></span>
* <span data-ttu-id="5e932-150">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="5e932-150">Offer</span></span>
* <span data-ttu-id="5e932-151">SKU</span><span class="sxs-lookup"><span data-stu-id="5e932-151">SKU</span></span>

<span data-ttu-id="5e932-152">Például használja ezeket az értékeket a a [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell-parancsmagot, vagy egy erőforrás-sablon, amelyben meg kell adni a virtuális gép létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5e932-152">For example, use these values with the [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell cmdlet, or with a resource group template in which you must specify the type of VM to be created.</span></span>

<span data-ttu-id="5e932-153">Ha meg kell határoznia ezeket az értékeket, futtathatja a [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), és [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) parancsmag megnyitása a képek.</span><span class="sxs-lookup"><span data-stu-id="5e932-153">If you need to determine these values, you can run the [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), and [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets to navigate the images.</span></span> <span data-ttu-id="5e932-154">Meghatározhatja, hogy ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="5e932-154">You determine these values:</span></span>

1. <span data-ttu-id="5e932-155">Listázza a rendszerkép-közzétevőket.</span><span class="sxs-lookup"><span data-stu-id="5e932-155">List the image publishers.</span></span>
2. <span data-ttu-id="5e932-156">Listázza egy adott közzétevő ajánlatait.</span><span class="sxs-lookup"><span data-stu-id="5e932-156">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="5e932-157">Listázza egy adott ajánlathoz tartozó termékváltozatokat.</span><span class="sxs-lookup"><span data-stu-id="5e932-157">For a given offer, list their SKUs.</span></span>

<span data-ttu-id="5e932-158">Először listázza a közzétevőket a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="5e932-158">First, list the publishers with the following commands:</span></span>

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

<span data-ttu-id="5e932-159">Adja meg a kiválasztott közzétevő nevét, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5e932-159">Fill in your chosen publisher name and run the following commands:</span></span>

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="5e932-160">Adja meg a kiválasztott ajánlat nevét, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5e932-160">Fill in your chosen offer name and run the following commands:</span></span>

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="5e932-161">A kimenetét a `Get-AzureRMVMImageSku` parancsot minden olyan információt, meg kell adnia a kép egy új virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="5e932-161">From the output of the `Get-AzureRMVMImageSku` command, you have all the information you need to specify the image for a new virtual machine.</span></span>

<span data-ttu-id="5e932-162">Az alábbi egy teljes példa:</span><span class="sxs-lookup"><span data-stu-id="5e932-162">The following shows a full example:</span></span>

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

<span data-ttu-id="5e932-163">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="5e932-163">Output:</span></span>

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

<span data-ttu-id="5e932-164">A „MicrosoftWindowsServer” közzétevő esetében:</span><span class="sxs-lookup"><span data-stu-id="5e932-164">For the "MicrosoftWindowsServer" publisher:</span></span>

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

<span data-ttu-id="5e932-165">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="5e932-165">Output:</span></span>

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

<span data-ttu-id="5e932-166">A „WindowsServer” ajánlat esetében:</span><span class="sxs-lookup"><span data-stu-id="5e932-166">For the "WindowsServer" offer:</span></span>

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

<span data-ttu-id="5e932-167">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="5e932-167">Output:</span></span>

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

<span data-ttu-id="5e932-168">Ha a listából kimásolja a kiválasztott termékváltozat nevét, akkor rendelkezésére áll a `Set-AzureRMVMSourceImage` PowerShell-parancsmaghoz vagy egy erőforráscsoport sablonjához szükséges összes információ.</span><span class="sxs-lookup"><span data-stu-id="5e932-168">From this list, copy the chosen SKU name, and you have all the information for the `Set-AzureRMVMSourceImage` PowerShell cmdlet or for a resource group template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e932-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e932-169">Next steps</span></span>
<span data-ttu-id="5e932-170">Most már kiválaszthatja azt az adott rendszerképet, amelyet használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="5e932-170">Now you can choose precisely the image you want to use.</span></span> <span data-ttu-id="5e932-171">A virtuális gépek gyors létrehozása csak talált, kép információk segítségével: [Windows virtuális gép létrehozása a PowerShell-lel](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5e932-171">To create a virtual machine quickly by using the image information, which you just found, see [Create a Windows virtual machine with PowerShell](quick-create-powershell.md).</span></span>
