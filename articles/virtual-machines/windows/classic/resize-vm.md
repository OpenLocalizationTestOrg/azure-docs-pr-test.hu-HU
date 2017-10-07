---
title: "egy Windows virtuális gép hello klasszikus üzembe helyezési modellel - Azure aaaResize |} Microsoft Docs"
description: "A Windows hello klasszikus üzembe helyezési modellel, az Azure Powershell használatával létrehozott virtuális gépek méretét."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 39fab14431e2351c9515b0611e850eccfec7a798
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm-created-in-hello-classic-deployment-model"></a><span data-ttu-id="fc48b-103">A Windows virtuális gépek hello klasszikus üzembe helyezési modellel létrehozott átméretezése</span><span class="sxs-lookup"><span data-stu-id="fc48b-103">Resize a Windows VM created in hello classic deployment model</span></span>
<span data-ttu-id="fc48b-104">Ez a cikk bemutatja, hogyan tooresize egy Windows virtuális gép létrehozása hello klasszikus üzembe helyezési modellel Azure Powershell használatával.</span><span class="sxs-lookup"><span data-stu-id="fc48b-104">This article shows you how tooresize a Windows VM, created in hello classic deployment model using Azure Powershell.</span></span>

<span data-ttu-id="fc48b-105">Annak eldöntéséhez, hogy hello képességét tooresize egy virtuális Gépet, nincsenek két fogalom, amelyek szabályozzák hello tartomány méretek elérhető tooresize hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fc48b-105">When considering hello ability tooresize a VM, there are two concepts which control hello range of sizes available tooresize hello virtual machine.</span></span> <span data-ttu-id="fc48b-106">első hello fogalma-hello régióban, amelyben a virtuális Gépre van telepítve.</span><span class="sxs-lookup"><span data-stu-id="fc48b-106">hello first concept is hello region in which your VM is deployed.</span></span> <span data-ttu-id="fc48b-107">Virtuálisgép-méretek elérhető régióban hello listája hello szolgáltatások lapon hello Azure-régiókat weblap alatt áll.</span><span class="sxs-lookup"><span data-stu-id="fc48b-107">hello list of VM sizes available in region is under hello Services tab of hello Azure Regions web page.</span></span> <span data-ttu-id="fc48b-108">második hello fogalma-hello fizikai hardver a virtuális gép jelenleg üzemeltet.</span><span class="sxs-lookup"><span data-stu-id="fc48b-108">hello second concept is hello physical hardware currently hosting your VM.</span></span> <span data-ttu-id="fc48b-109">hello fizikai kiszolgálók virtuális gépeket üzemeltet a fürtök közös fizikai hardver együtt vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="fc48b-109">hello physical servers hosting VMs are grouped together in clusters of common physical hardware.</span></span> <span data-ttu-id="fc48b-110">hello módszert a virtuális gép méretének módosítása eltér attól függően, hogy hello kívánt új Virtuálisgép-méretet támogatja hello hardver fürt hello VM tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="fc48b-110">hello method of changing a VM size differs depending on if hello desired new VM size is supported by hello hardware cluster currently hosting hello VM.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="fc48b-111">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fc48b-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fc48b-112">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="fc48b-112">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="fc48b-113">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="fc48b-113">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="fc48b-114">Emellett [méretezze át a virtuális gépek hello Resource Manager üzembe helyezési modellel létrehozott](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fc48b-114">You can also [resize a VM created in hello Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="add-your-account"></a><span data-ttu-id="fc48b-115">A fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fc48b-115">Add your account</span></span>
<span data-ttu-id="fc48b-116">Klasszikus Azure-erőforrások az Azure PowerShell toowork kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="fc48b-116">You must configure Azure PowerShell toowork with classic Azure resources.</span></span> <span data-ttu-id="fc48b-117">Kövesse az alábbi tooconfigure Azure PowerShell toomanage hagyományos erőforrások hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="fc48b-117">Follow hello steps below tooconfigure Azure PowerShell toomanage classic resources.</span></span>

1. <span data-ttu-id="fc48b-118">Hello PowerShell parancssorába írja be a `Add-AzureAccount` kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="fc48b-118">At hello PowerShell prompt, type `Add-AzureAccount` and click **Enter**.</span></span> 
2. <span data-ttu-id="fc48b-119">Írja be az Azure-előfizetéshez társított hello e-mail címét, és kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="fc48b-119">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="fc48b-120">Adja meg a fiókhoz tartozó jelszót hello.</span><span class="sxs-lookup"><span data-stu-id="fc48b-120">Type in hello password for your account.</span></span> 
4. <span data-ttu-id="fc48b-121">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fc48b-121">Click **Sign in**.</span></span> 

## <a name="resize-in-hello-same-hardware-cluster"></a><span data-ttu-id="fc48b-122">Átméretezi a hello azonos fürt hardver</span><span class="sxs-lookup"><span data-stu-id="fc48b-122">Resize in hello same hardware cluster</span></span>
<span data-ttu-id="fc48b-123">tooresize egy virtuális gép tooa terület hello hardver fürt hello virtuális Gépet üzemeltető hajtsa végre a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="fc48b-123">tooresize a VM tooa size available in hello hardware cluster hosting hello VM, perform hello following steps.</span></span>

1. <span data-ttu-id="fc48b-124">Futtassa a következő PowerShell-paranccsal toolist hello Virtuálisgép-méretek elérhető hello hardver fürt üzemeltető hello felhőalapú szolgáltatás, amely tartalmazza a virtuális gép hello hello.</span><span class="sxs-lookup"><span data-stu-id="fc48b-124">Run hello following PowerShell command toolist hello VM sizes available in hello hardware cluster hosting hello cloud service which contains hello VM.</span></span>
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. <span data-ttu-id="fc48b-125">Futtassa a következő parancsok tooresize hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="fc48b-125">Run hello following commands tooresize hello VM.</span></span>
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a><span data-ttu-id="fc48b-126">Új hardver fürt átméretezése</span><span class="sxs-lookup"><span data-stu-id="fc48b-126">Resize on a new hardware cluster</span></span>
<span data-ttu-id="fc48b-127">egy virtuális gép tooa méret nem érhető el a hello hardver fürt üzemeltet tooresize hello VM, hello felhőszolgáltatás és hello felhőalapú szolgáltatás virtuális gépeinek kell újból létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="fc48b-127">tooresize a VM tooa size not available in hello hardware cluster hosting hello VM, hello cloud service and all VMs in hello cloud service must be recreated.</span></span> <span data-ttu-id="fc48b-128">Minden felhőalapú szolgáltatás egyetlen hardver fürtön tárolja, ezért hello felhőalapú szolgáltatás virtuális gépeinek meg kell adni a hardver-fürtökön támogatott mérete.</span><span class="sxs-lookup"><span data-stu-id="fc48b-128">Each cloud service is hosted on a single hardware cluster so all VMs in hello cloud service must be a size that is supported on a hardware cluster.</span></span> <span data-ttu-id="fc48b-129">hello következő lépéseket ismerteti, hogyan tooresize törlésével és majd újbóli létrehozása a virtuális gépek hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fc48b-129">hello following steps will describe how tooresize a VM by deleting and then recreating hello cloud service.</span></span>

1. <span data-ttu-id="fc48b-130">Futtassa a következő PowerShell parancs toolist hello Virtuálisgép-méretek elérhető hello régióban hello.</span><span class="sxs-lookup"><span data-stu-id="fc48b-130">Run hello following PowerShell command toolist hello VM sizes available in hello region.</span></span> 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. <span data-ttu-id="fc48b-131">Jegyezze fel az összes konfigurációs beállítások az egyes virtuális gépek hello VM toobe átméretezték, ezért tartalmazó hello felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="fc48b-131">Make note of all configuration settings for each VM in hello cloud service which contains hello VM toobe resized.</span></span> 
3. <span data-ttu-id="fc48b-132">Hello felhőszolgáltatásban hello beállítás tooretain hello lemezek kiválasztása az egyes virtuális gépek összes virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="fc48b-132">Delete all VMs in hello cloud service selecting hello option tooretain hello disks for each VM.</span></span>
4. <span data-ttu-id="fc48b-133">Hozza létre újra hello VM toobe átméretezték, ezért hello használata szükséges Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="fc48b-133">Recreate hello VM toobe resized using hello desired VM size.</span></span>
5. <span data-ttu-id="fc48b-134">Hozza létre újból minden más virtuális gépek, amelyek használatával elérhető ilyenkor hello felhőszolgáltatás hello hardver fürt Virtuálisgép-méretet hello felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="fc48b-134">Recreate all other VMs which were in hello cloud service using a VM size available in hello hardware cluster now hosting hello cloud service.</span></span>

<span data-ttu-id="fc48b-135">Egy minta parancsfájlt törlése és újbóli létrehozása egy felhőszolgáltatás, új Virtuálisgép-méretet használó található [Itt](https://github.com/Azure/azure-vm-scripts).</span><span class="sxs-lookup"><span data-stu-id="fc48b-135">A sample script for deleting and recreating a cloud service using a new VM size can be found [here](https://github.com/Azure/azure-vm-scripts).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fc48b-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fc48b-136">Next steps</span></span>
* <span data-ttu-id="fc48b-137">[Méretezze át a virtuális gépek hello Resource Manager üzembe helyezési modellel létrehozott](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fc48b-137">[Resize a VM created in hello Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

