---
title: "a virtuálisgép-méretezési csoportban lévő virtuális gépek aaaManage |} Microsoft Docs"
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
ms.openlocfilehash: 7d848729c0fc708bd596b61feb528cf4bf4bafd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="93986-103">A virtuálisgép-méretezési csoportban lévő virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="93986-103">Manage virtual machines in a virtual machine scale set</span></span>
<span data-ttu-id="93986-104">Ez a cikk toomanage virtuális gépek a virtuálisgép-méretezési csoportban lévő hello feladatok használja.</span><span class="sxs-lookup"><span data-stu-id="93986-104">Use hello tasks in this article toomanage virtual machines in your virtual machine scale set.</span></span>

<span data-ttu-id="93986-105">Hello feladatokról a virtuálisgép-méretezési csoportban lévő kezelése a legtöbb szükséges, hogy ismeri a hello azonosítója, amelyet az toomanage hello gép.</span><span class="sxs-lookup"><span data-stu-id="93986-105">Most of hello tasks that involve managing a virtual machine in a scale set require that you know hello instance ID of hello machine that you want toomanage.</span></span> <span data-ttu-id="93986-106">Használhat [Azure erőforrás-kezelő](https://resources.azure.com) egy virtuálisgép-méretezési csoportban lévő toofind hello Példányazonosítója.</span><span class="sxs-lookup"><span data-stu-id="93986-106">You can use [Azure Resource Explorer](https://resources.azure.com) toofind hello instance ID of a virtual machine in a scale set.</span></span> <span data-ttu-id="93986-107">Az erőforrás-kezelő tooverify hello befejezése hello feladatok állapotának is használja.</span><span class="sxs-lookup"><span data-stu-id="93986-107">You also use Resource Explorer tooverify hello status of hello tasks that you finish.</span></span>

<span data-ttu-id="93986-108">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello Azure PowerShell legújabb verziójának telepítése, az előfizetés kiválasztásával és tooyour fiók bejelentkezés kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="93986-108">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooyour account.</span></span>

## <a name="display-information-about-a-scale-set"></a><span data-ttu-id="93986-109">A méretezési kapcsolatos információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="93986-109">Display information about a scale set</span></span>
<span data-ttu-id="93986-110">A méretezési csoport, amely egyben hivatkozott tooas hello példányait tartalmazó nézetet kapcsolatos általános információkat kaphat.</span><span class="sxs-lookup"><span data-stu-id="93986-110">You can get general information about a scale set, which is also referred tooas hello instance view.</span></span> <span data-ttu-id="93986-111">Másik lehetőségként kaphat konkrétabb információkat, például hello méretezési csoportban lévő hello erőforrások adatait.</span><span class="sxs-lookup"><span data-stu-id="93986-111">Or, you can get more specific information, such as information about hello resources in hello scale set.</span></span>

<span data-ttu-id="93986-112">Cserélje le a hello hello nevét vagy az erőforráscsoportot és a skála állítsa be, és futtassa a hello parancs értékeket idézőjelek között:</span><span class="sxs-lookup"><span data-stu-id="93986-112">Replace hello quoted values with hello name or your resource group and scale set and then run hello command:</span></span>

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

<span data-ttu-id="93986-113">Ehhez hasonló adja vissza:</span><span class="sxs-lookup"><span data-stu-id="93986-113">It returns something like this:</span></span>

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

<span data-ttu-id="93986-114">Cserélje le a hello hello nevet, a csoport és a skála erőforráskészlethez értékeket idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="93986-114">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="93986-115">Cserélje le  *#*  hello virtuális gép tooget tájékoztatást szeretne kapni, és futtassa azt hello példány azonosítója:</span><span class="sxs-lookup"><span data-stu-id="93986-115">Replace *#* with hello instance identifier of hello virtual machine that you want tooget information about and then run it:</span></span>

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="93986-116">Ez a példa hasonlót adja vissza:</span><span class="sxs-lookup"><span data-stu-id="93986-116">It returns something like this example:</span></span>

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

## <a name="start-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="93986-117">Indítsa el a virtuálisgép-méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="93986-117">Start a virtual machine in a scale set</span></span>
<span data-ttu-id="93986-118">Cserélje le a hello hello nevet, a csoport és a skála erőforráskészlethez értékeket idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="93986-118">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="93986-119">Cserélje le  *#*  hello virtuális gép kívánt toostart, és futtassa a hello azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="93986-119">Replace *#* with hello identifier of hello virtual machine that you want toostart and then run it:</span></span>

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="93986-120">Az erőforrás-kezelőben, láthatja, hogy hello hello példány állapota **futtató**:</span><span class="sxs-lookup"><span data-stu-id="93986-120">In Resource Explorer, we can see that hello status of hello instance is **running**:</span></span>

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

<span data-ttu-id="93986-121">Hello méretezési készletben hello - InstanceId paraméter nem segítségével is elindítható hello összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="93986-121">You can start all hello virtual machines in hello scale set by not using hello -InstanceId parameter.</span></span>

## <a name="stop-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="93986-122">Méretezési csoportban lévő virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="93986-122">Stop a virtual machine in a scale set</span></span>
<span data-ttu-id="93986-123">Cserélje le a hello hello nevet, a csoport és a skála erőforráskészlethez értékeket idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="93986-123">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="93986-124">Cserélje le  *#*  hello virtuális gép kívánt toostop, és futtassa a hello azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="93986-124">Replace *#* with hello identifier of hello virtual machine that you want toostop and then run it:</span></span>

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="93986-125">Az erőforrás-kezelőben, láthatja, hogy hello hello példány állapota **felszabadítása**:</span><span class="sxs-lookup"><span data-stu-id="93986-125">In Resource Explorer, we can see that hello status of hello instance is **deallocated**:</span></span>

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

<span data-ttu-id="93986-126">a virtuális gépek toostop nem felszabadítani azt, használja a hello - StayProvisioned paramétert.</span><span class="sxs-lookup"><span data-stu-id="93986-126">toostop a virtual machine and not deallocate it, use hello -StayProvisioned parameter.</span></span> <span data-ttu-id="93986-127">Hello hello nem hello - InstanceId paraméter segítségével állítsa be az összes virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="93986-127">You can stop all hello virtual machines in hello set by not using hello -InstanceId parameter.</span></span>

## <a name="restart-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="93986-128">Indítsa újra a virtuálisgép-méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="93986-128">Restart a virtual machine in a scale set</span></span>
<span data-ttu-id="93986-129">Cserélje le a erőforrás csoport és hello méretezési hello nevű értékeket idézőjelek között hello.</span><span class="sxs-lookup"><span data-stu-id="93986-129">Replace hello quoted values with hello name of your resource group and hello scale set.</span></span> <span data-ttu-id="93986-130">Cserélje le  *#*  hello virtuális gép kívánt toorestart, és futtassa a hello azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="93986-130">Replace *#* with hello identifier of hello virtual machine that you want toorestart and then run it:</span></span>

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="93986-131">Minden hello virtuális gépek a hello beállítása hello - InstanceId paraméter nem használatával indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="93986-131">You can restart all hello virtual machines in hello set by not using hello -InstanceId parameter.</span></span>

## <a name="remove-a-virtual-machine-from-a-scale-set"></a><span data-ttu-id="93986-132">A virtuális gép eltávolítása egy méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="93986-132">Remove a virtual machine from a scale set</span></span>
<span data-ttu-id="93986-133">Cserélje le a erőforrás csoport és hello méretezési hello nevű értékeket idézőjelek között hello.</span><span class="sxs-lookup"><span data-stu-id="93986-133">Replace hello quoted values with hello name of your resource group and hello scale set.</span></span> <span data-ttu-id="93986-134">Cserélje le  *#*  hello virtuális gép kívánt tooremove, és futtassa a hello azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="93986-134">Replace *#* with hello identifier of hello virtual machine that you want tooremove and then run it:</span></span>  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="93986-135">Hello virtuális gép méretezési egyszerre megszüntetheti a nem a hello - InstanceId paraméter használatával.</span><span class="sxs-lookup"><span data-stu-id="93986-135">You can remove hello virtual machine scale set all at once by not using hello -InstanceId parameter.</span></span>

## <a name="change-hello-capacity-of-a-scale-set"></a><span data-ttu-id="93986-136">A méretezési hello kapacitás módosítása</span><span class="sxs-lookup"><span data-stu-id="93986-136">Change hello capacity of a scale set</span></span>
<span data-ttu-id="93986-137">Adja hozzá, vagy távolítsa el a virtuális gépeket hello kapacitás hello készlet módosításával.</span><span class="sxs-lookup"><span data-stu-id="93986-137">You can add or remove virtual machines by changing hello capacity of hello set.</span></span> <span data-ttu-id="93986-138">Toochange, set hello kapacitás toowhat toobe-érdemes, és frissítse hello méretezési hello új kapacitással rendelkező kívánt méretezési hello beolvasása.</span><span class="sxs-lookup"><span data-stu-id="93986-138">Get hello scale set that you want toochange, set hello capacity toowhat you want it toobe, and then update hello scale set with hello new capacity.</span></span> <span data-ttu-id="93986-139">Ezek a parancsok cserélje le a erőforrás csoport és hello méretezési hello nevű értékeket idézőjelek között hello.</span><span class="sxs-lookup"><span data-stu-id="93986-139">In these commands, replace hello quoted values with hello name of your resource group and hello scale set.</span></span>

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

<span data-ttu-id="93986-140">Virtuális gépek hello méretezési készlet távolítja el, ha hello virtuális gépek hello legmagasabb azonosítók először törlődnek.</span><span class="sxs-lookup"><span data-stu-id="93986-140">If you are removing virtual machines from hello scale set, hello virtual machines with hello highest ids are removed first.</span></span>

