---
title: "aaaManage a virtuális gépek Azure PowerShell használatával |} Microsoft Docs"
description: "Ismerje meg, hogy használható-e tooautomate feladatokat a virtuális gépek kezelése a parancsok."
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: e4ca6f098519243a321eac98b6692790fe18c22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a><span data-ttu-id="95c73-103">A virtuális gépek kezelése az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="95c73-103">Manage your virtual machines by using Azure PowerShell</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="95c73-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="95c73-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="95c73-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="95c73-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="95c73-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="95c73-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="95c73-107">Általános PowerShell-parancsok hello Resource Manager modellt használja, lásd: [Itt](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="95c73-107">For common PowerShell commands using hello Resource Manager model, see [here](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="95c73-108">A virtuális gépek minden nap toomanage teszi számos feladatot Azure PowerShell-parancsmagok segítségével automatizálható.</span><span class="sxs-lookup"><span data-stu-id="95c73-108">Many tasks you do each day toomanage your VMs can be automated by using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="95c73-109">Ez a cikk lehetővé teszi az Példaparancsok egyszerűbb feladatokat, és hivatkozások tooarticles, amelyek megjelenítik az összetettebb feladatok hello parancsok.</span><span class="sxs-lookup"><span data-stu-id="95c73-109">This article gives you example commands for simpler tasks, and links tooarticles that show hello commands for more complex tasks.</span></span>

> [!NOTE]
> <span data-ttu-id="95c73-110">Ha még nem telepített és konfigurált Azure PowerShell, még hello cikkben szereplő útmutatást kaphat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="95c73-110">If you haven't installed and configured Azure PowerShell yet, you can get instructions in hello article [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
> 
> 

## <a name="how-toouse-hello-example-commands"></a><span data-ttu-id="95c73-111">Hogyan toouse hello Példaparancsok</span><span class="sxs-lookup"><span data-stu-id="95c73-111">How toouse hello example commands</span></span>
<span data-ttu-id="95c73-112">Néhány hello hello szöveg, amely a környezetnek megfelelő szöveggel parancsok tooreplace lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="95c73-112">You'll need tooreplace some of hello text in hello commands with text that's appropriate for your environment.</span></span> <span data-ttu-id="95c73-113">hello < és > szimbólumok jelzi a szöveg tooreplace van szüksége.</span><span class="sxs-lookup"><span data-stu-id="95c73-113">hello < and > symbols indicate text you need tooreplace.</span></span> <span data-ttu-id="95c73-114">Hello szöveg cseréjekor hello szimbólumok eltávolítása, de hagyja hello ajánlat jelek helyen.</span><span class="sxs-lookup"><span data-stu-id="95c73-114">When you replace hello text, remove hello symbols but leave hello quote marks in place.</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="95c73-115">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="95c73-115">Get a VM</span></span>
<span data-ttu-id="95c73-116">Ez a gyakran használt egyszerű feladat.</span><span class="sxs-lookup"><span data-stu-id="95c73-116">This is a basic task you'll use often.</span></span> <span data-ttu-id="95c73-117">Használja a virtuális gépek tooget információt, feladatokat a virtuális gép vagy kimeneti toostore lekérni egy változóban.</span><span class="sxs-lookup"><span data-stu-id="95c73-117">Use it tooget information about a VM, perform tasks on a VM, or get output toostore in a variable.</span></span>

<span data-ttu-id="95c73-118">tooget információ a virtuális gép, hello futtassa ezt a parancsot, minden hello idézőjelben, beleértve a hello < és > karakter cseréje:</span><span class="sxs-lookup"><span data-stu-id="95c73-118">tooget information about hello VM, run this command, replacing everything in hello quotes, including hello < and > characters:</span></span>

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

<span data-ttu-id="95c73-119">Futtassa egy $vm változóban kimeneti toostore hello:</span><span class="sxs-lookup"><span data-stu-id="95c73-119">toostore hello output in a $vm variable, run:</span></span>

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-tooa-windows-based-vm"></a><span data-ttu-id="95c73-120">Windows-alapú virtuális gép tooa bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="95c73-120">Log on tooa Windows-based VM</span></span>
<span data-ttu-id="95c73-121">Futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="95c73-121">Run these commands:</span></span>

> [!NOTE]
> <span data-ttu-id="95c73-122">Hello virtuális gép és a felhőszolgáltatás neve lekérheti hello hello megjelenítését **Get-AzureVM** parancsot.</span><span class="sxs-lookup"><span data-stu-id="95c73-122">You can get hello virtual machine and cloud service name from hello display of hello **Get-AzureVM** command.</span></span>
> 
> <span data-ttu-id="95c73-123">$svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "< meghajtó és mappa helye toostore letöltött hello RDP-fájlt, például: c:\temp >" $localFile = $localPath + "\" $vmname +".rdp"Get-AzureRemoteDesktopFile - Szolgáltatásnév $svcName-Name $vmName - LocalPath $localFile-elindítása</span><span class="sxs-lookup"><span data-stu-id="95c73-123">$svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "<drive and folder location toostore hello downloaded RDP file, example: c:\temp >" $localFile = $localPath + "\" + $vmname + ".rdp" Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch</span></span>
> 
> 

## <a name="stop-a-vm"></a><span data-ttu-id="95c73-124">Virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="95c73-124">Stop a VM</span></span>
<span data-ttu-id="95c73-125">Futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="95c73-125">Run this command:</span></span>

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> <span data-ttu-id="95c73-126">Használja a paraméter tookeep hello virtuális a IP-cím (VIP) a felhő hello szolgáltatást, abban az esetben, ha az utolsó virtuális gép, amely a felhőszolgáltatás a hello.</span><span class="sxs-lookup"><span data-stu-id="95c73-126">Use this parameter tookeep hello virtual IP (VIP) of hello cloud service in case it's hello last VM in that cloud service.</span></span> <br><br> <span data-ttu-id="95c73-127">Ha hello StayProvisioned paraméter használata esetén a fogjuk továbbra is számlázni hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="95c73-127">If you use hello StayProvisioned parameter, you'll still be billed for hello VM.</span></span>
> 
> 

## <a name="start-a-vm"></a><span data-ttu-id="95c73-128">Virtuális gép elindítása</span><span class="sxs-lookup"><span data-stu-id="95c73-128">Start a VM</span></span>
<span data-ttu-id="95c73-129">Futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="95c73-129">Run this command:</span></span>

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a><span data-ttu-id="95c73-130">Adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="95c73-130">Attach a data disk</span></span>
<span data-ttu-id="95c73-131">Ez a feladat néhány lépést igényel.</span><span class="sxs-lookup"><span data-stu-id="95c73-131">This task requires a few steps.</span></span> <span data-ttu-id="95c73-132">Először használja a hello x Add-AzureDataDisk x parancsmag tooadd hello toohello $vm objektumát.</span><span class="sxs-lookup"><span data-stu-id="95c73-132">First, you use hello ****Add-AzureDataDisk**** cmdlet tooadd hello disk toohello $vm object.</span></span> <span data-ttu-id="95c73-133">Ezután **frissítés-AzureVM** hello VM parancsmag tooupdate hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="95c73-133">Then, you use **Update-AzureVM** cmdlet tooupdate hello configuration of hello VM.</span></span>

<span data-ttu-id="95c73-134">Meg kell toodecide e tooattach új lemez vagy egy tartalmazó adatokat.</span><span class="sxs-lookup"><span data-stu-id="95c73-134">You'll also need toodecide whether tooattach a new disk or one that contains data.</span></span> <span data-ttu-id="95c73-135">Egy új lemez hello parancs hello .vhd fájlt hoz létre, és csatolja.</span><span class="sxs-lookup"><span data-stu-id="95c73-135">For a new disk, hello command creates hello .vhd file and attaches it.</span></span>

<span data-ttu-id="95c73-136">egy új lemezt tooattach futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="95c73-136">tooattach a new disk, run this command:</span></span>

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

<span data-ttu-id="95c73-137">egy meglévő adatlemez tooattach futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="95c73-137">tooattach an existing data disk, run this command:</span></span>

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

<span data-ttu-id="95c73-138">a blob Storage tárolóban, meglévő .vhd fájl tooattach adatlemezek futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="95c73-138">tooattach data disks from an existing .vhd file in blob storage, run this command:</span></span>

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a><span data-ttu-id="95c73-139">Windows-alapú virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="95c73-139">Create a Windows-based VM</span></span>
<span data-ttu-id="95c73-140">új Windows-alapú virtuális gépet az Azure használatát hello utasításait toocreate [Azure PowerShell használata toocreate és előre konfigurálása a Windows-alapú virtuális gépek](create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="95c73-140">toocreate a new Windows-based virtual machine in Azure, use hello instructions in [Use Azure PowerShell toocreate and preconfigure Windows-based virtual machines](create-powershell.md).</span></span> <span data-ttu-id="95c73-141">Ez a témakör lépéseit hello létrehozása az Azure PowerShell paranccsal állítsa be, amely végigvezeti létrehoz egy Windows-alapú virtuális Gépet, amely megfelelően előre beállíthatók:</span><span class="sxs-lookup"><span data-stu-id="95c73-141">This topic steps you through hello creation of an Azure PowerShell command set that creates a Windows-based VM that can be preconfigured:</span></span>

* <span data-ttu-id="95c73-142">Az Active Directory tartományi tagságát.</span><span class="sxs-lookup"><span data-stu-id="95c73-142">With Active Directory domain membership.</span></span>
* <span data-ttu-id="95c73-143">A további lemezek.</span><span class="sxs-lookup"><span data-stu-id="95c73-143">With additional disks.</span></span>
* <span data-ttu-id="95c73-144">Elosztott terhelésű meglévő tagjaként beállítása.</span><span class="sxs-lookup"><span data-stu-id="95c73-144">As a member of an existing load-balanced set.</span></span>
* <span data-ttu-id="95c73-145">Statikus IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="95c73-145">With a static IP address.</span></span>

