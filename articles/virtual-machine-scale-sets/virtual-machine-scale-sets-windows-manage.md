---
title: "A virtuálisgép-méretezési csoportban lévő virtuális gépek kezeléséhez |} Microsoft Docs"
description: "A virtuálisgép-méretezési beállítása az Azure PowerShell használatával a virtuális gépek kezeléséhez."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: d09a020b903e5f43afe03b86c675bcc1eb536cbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="12552-103">A virtuálisgép-méretezési csoportban lévő virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="12552-103">Manage virtual machines in a virtual machine scale set</span></span>
<span data-ttu-id="12552-104">A virtuálisgép-méretezési csoportban lévő virtuális gépek kezeléséhez használja a cikkben a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="12552-104">Use the tasks in this article to manage virtual machines in your virtual machine scale set.</span></span>

<span data-ttu-id="12552-105">A virtuálisgép-méretezési csoportban lévő kezelése a feladatokról a legtöbb szükséges, hogy ismeri-e a Példányazonosító a kezelni kívánt számítógép.</span><span class="sxs-lookup"><span data-stu-id="12552-105">Most of the tasks that involve managing a virtual machine in a scale set require that you know the instance ID of the machine that you want to manage.</span></span> <span data-ttu-id="12552-106">Használhat [Azure erőforrás-kezelő](https://resources.azure.com) méretezési csoportban lévő virtuális gép Példányazonosítója kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="12552-106">You can use [Azure Resource Explorer](https://resources.azure.com) to find the instance ID of a virtual machine in a scale set.</span></span> <span data-ttu-id="12552-107">Az erőforrás-kezelő által végzett feladatok állapotának ellenőrzéséhez is használni.</span><span class="sxs-lookup"><span data-stu-id="12552-107">You also use Resource Explorer to verify the status of the tasks that you finish.</span></span>

<span data-ttu-id="12552-108">Az Azure PowerShell legfrissebb verziójának telepítésével, a kívánt előfizetés kiválasztásával és a fiókjába való bejelentkezéssel kapcsolatos információkért lásd: [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="12552-108">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to your account.</span></span>

## <a name="display-information-about-a-scale-set"></a><span data-ttu-id="12552-109">A méretezési kapcsolatos információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="12552-109">Display information about a scale set</span></span>
<span data-ttu-id="12552-110">A méretezési csoport, amely a példányait tartalmazó nézetet is nevezzük kapcsolatos általános információkat kaphat.</span><span class="sxs-lookup"><span data-stu-id="12552-110">You can get general information about a scale set, which is also referred to as the instance view.</span></span> <span data-ttu-id="12552-111">Másik lehetőségként kaphat konkrétabb információkat, például a méretezési csoportban lévő erőforrások adatait.</span><span class="sxs-lookup"><span data-stu-id="12552-111">Or, you can get more specific information, such as information about the resources in the scale set.</span></span>

<span data-ttu-id="12552-112">Az ajánlatban szereplő értékek cserélje le a nevét vagy az erőforráscsoport és állítsa be, és futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="12552-112">Replace the quoted values with the name or your resource group and scale set and then run the command:</span></span>

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

<span data-ttu-id="12552-113">Ehhez hasonló adja vissza:</span><span class="sxs-lookup"><span data-stu-id="12552-113">It returns something like this:</span></span>

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded

<span data-ttu-id="12552-114">Az ajánlatban szereplő értékek cserélje le a erőforrás csoport és a méretezési készlet nevét.</span><span class="sxs-lookup"><span data-stu-id="12552-114">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="12552-115">Cserélje le  *#*  a virtuális gép, amelyet szeretne információ jelenik meg, és futtassa a példány azonosítója:</span><span class="sxs-lookup"><span data-stu-id="12552-115">Replace *#* with the instance identifier of the virtual machine that you want to get information about and then run it:</span></span>

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="12552-116">Ez a példa hasonlót adja vissza:</span><span class="sxs-lookup"><span data-stu-id="12552-116">It returns something like this example:</span></span>

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded

## <a name="start-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="12552-117">Indítsa el a virtuálisgép-méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="12552-117">Start a virtual machine in a scale set</span></span>
<span data-ttu-id="12552-118">Az ajánlatban szereplő értékek cserélje le a erőforrás csoport és a méretezési készlet nevét.</span><span class="sxs-lookup"><span data-stu-id="12552-118">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="12552-119">Cserélje le  *#*  a virtuális gép, amelyet szeretne elindítani, és futtassa az azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="12552-119">Replace *#* with the identifier of the virtual machine that you want to start and then run it:</span></span>

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="12552-120">Az erőforrás-kezelőben, láthatja, hogy a példány állapota **futtató**:</span><span class="sxs-lookup"><span data-stu-id="12552-120">In Resource Explorer, we can see that the status of the instance is **running**:</span></span>

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

<span data-ttu-id="12552-121">A méretezési készletben - InstanceId paraméter nem használatával is elindítható a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="12552-121">You can start all the virtual machines in the scale set by not using the -InstanceId parameter.</span></span>

## <a name="stop-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="12552-122">Méretezési csoportban lévő virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="12552-122">Stop a virtual machine in a scale set</span></span>
<span data-ttu-id="12552-123">Az ajánlatban szereplő értékek cserélje le a erőforrás csoport és a méretezési készlet nevét.</span><span class="sxs-lookup"><span data-stu-id="12552-123">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="12552-124">Cserélje le  *#*  a virtuális gép leállítása, és futtassa kívánt azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="12552-124">Replace *#* with the identifier of the virtual machine that you want to stop and then run it:</span></span>

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="12552-125">Az erőforrás-kezelőben, láthatja, hogy a példány állapota **felszabadítása**:</span><span class="sxs-lookup"><span data-stu-id="12552-125">In Resource Explorer, we can see that the status of the instance is **deallocated**:</span></span>

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]

<span data-ttu-id="12552-126">Állítsa le a virtuális gépet, és nem felszabadítani, használja a - StayProvisioned paramétert.</span><span class="sxs-lookup"><span data-stu-id="12552-126">To stop a virtual machine and not deallocate it, use the -StayProvisioned parameter.</span></span> <span data-ttu-id="12552-127">A készlet összes virtuális gépet a - InstanceId paraméter nem segítségével állíthatók le.</span><span class="sxs-lookup"><span data-stu-id="12552-127">You can stop all the virtual machines in the set by not using the -InstanceId parameter.</span></span>

## <a name="restart-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="12552-128">Indítsa újra a virtuálisgép-méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="12552-128">Restart a virtual machine in a scale set</span></span>
<span data-ttu-id="12552-129">Az ajánlatban szereplő értékek cserélje le az erőforráscsoport és a méretezési csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="12552-129">Replace the quoted values with the name of your resource group and the scale set.</span></span> <span data-ttu-id="12552-130">Cserélje le  *#*  a virtuális gép, amelyet szeretne újraindítani, és futtassa az azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="12552-130">Replace *#* with the identifier of the virtual machine that you want to restart and then run it:</span></span>

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="12552-131">A készlet összes virtuális gépet a - InstanceId paraméter nem használatával indíthatja újra.</span><span class="sxs-lookup"><span data-stu-id="12552-131">You can restart all the virtual machines in the set by not using the -InstanceId parameter.</span></span>

## <a name="remove-a-virtual-machine-from-a-scale-set"></a><span data-ttu-id="12552-132">A virtuális gép eltávolítása egy méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="12552-132">Remove a virtual machine from a scale set</span></span>
<span data-ttu-id="12552-133">Az ajánlatban szereplő értékek cserélje le az erőforráscsoport és a méretezési csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="12552-133">Replace the quoted values with the name of your resource group and the scale set.</span></span> <span data-ttu-id="12552-134">Cserélje le  *#*  a virtuális gép, amelyet szeretne eltávolítani, és futtassa az azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="12552-134">Replace *#* with the identifier of the virtual machine that you want to remove and then run it:</span></span>  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="12552-135">A virtuális gép méretezési egyszerre is eltávolíthat, ha nem használja a - InstanceId paraméter.</span><span class="sxs-lookup"><span data-stu-id="12552-135">You can remove the virtual machine scale set all at once by not using the -InstanceId parameter.</span></span>

## <a name="change-the-capacity-of-a-scale-set"></a><span data-ttu-id="12552-136">A kapacitás, a méretezési módosítása</span><span class="sxs-lookup"><span data-stu-id="12552-136">Change the capacity of a scale set</span></span>
<span data-ttu-id="12552-137">Adja hozzá, vagy távolítsa el a virtuális gépeket a készlet kapacitásának módosításával.</span><span class="sxs-lookup"><span data-stu-id="12552-137">You can add or remove virtual machines by changing the capacity of the set.</span></span> <span data-ttu-id="12552-138">A méretezési, kapacitás beállítása a kívánt műveleteket kell lennie, és ezután frissítse a méretezési készletben az új kapacitással rendelkező kívánt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="12552-138">Get the scale set that you want to change, set the capacity to what you want it to be, and then update the scale set with the new capacity.</span></span> <span data-ttu-id="12552-139">Ezek a parancsok cserélje le az ajánlatban szereplő értékeket az erőforráscsoport és a méretezési csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="12552-139">In these commands, replace the quoted values with the name of your resource group and the scale set.</span></span>

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

<span data-ttu-id="12552-140">Virtuális gépek a méretezési készlet távolítja el, ha a virtuális gépek legmagasabb azonosítókkal először törlődnek.</span><span class="sxs-lookup"><span data-stu-id="12552-140">If you are removing virtual machines from the scale set, the virtual machines with the highest ids are removed first.</span></span>

