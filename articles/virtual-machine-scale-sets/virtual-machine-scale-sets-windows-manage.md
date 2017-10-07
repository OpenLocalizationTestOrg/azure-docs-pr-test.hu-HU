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
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>A virtuálisgép-méretezési csoportban lévő virtuális gépek kezelése
Ez a cikk toomanage virtuális gépek a virtuálisgép-méretezési csoportban lévő hello feladatok használja.

Hello feladatokról a virtuálisgép-méretezési csoportban lévő kezelése a legtöbb szükséges, hogy ismeri a hello azonosítója, amelyet az toomanage hello gép. Használhat [Azure erőforrás-kezelő](https://resources.azure.com) egy virtuálisgép-méretezési csoportban lévő toofind hello Példányazonosítója. Az erőforrás-kezelő tooverify hello befejezése hello feladatok állapotának is használja.

Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello Azure PowerShell legújabb verziójának telepítése, az előfizetés kiválasztásával és tooyour fiók bejelentkezés kapcsolatos információkat.

## <a name="display-information-about-a-scale-set"></a>A méretezési kapcsolatos információk megjelenítése
A méretezési csoport, amely egyben hivatkozott tooas hello példányait tartalmazó nézetet kapcsolatos általános információkat kaphat. Másik lehetőségként kaphat konkrétabb információkat, például hello méretezési csoportban lévő hello erőforrások adatait.

Cserélje le a hello hello nevét vagy az erőforráscsoportot és a skála állítsa be, és futtassa a hello parancs értékeket idézőjelek között:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Ehhez hasonló adja vissza:

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

Cserélje le a hello hello nevet, a csoport és a skála erőforráskészlethez értékeket idézőjelek között. Cserélje le  *#*  hello virtuális gép tooget tájékoztatást szeretne kapni, és futtassa azt hello példány azonosítója:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Ez a példa hasonlót adja vissza:

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

## <a name="start-a-virtual-machine-in-a-scale-set"></a>Indítsa el a virtuálisgép-méretezési csoportban lévő
Cserélje le a hello hello nevet, a csoport és a skála erőforráskészlethez értékeket idézőjelek között. Cserélje le  *#*  hello virtuális gép kívánt toostart, és futtassa a hello azonosítóval:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Az erőforrás-kezelőben, láthatja, hogy hello hello példány állapota **futtató**:

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

Hello méretezési készletben hello - InstanceId paraméter nem segítségével is elindítható hello összes virtuális gépet.

## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Méretezési csoportban lévő virtuális gép leállítása
Cserélje le a hello hello nevet, a csoport és a skála erőforráskészlethez értékeket idézőjelek között. Cserélje le  *#*  hello virtuális gép kívánt toostop, és futtassa a hello azonosítóval:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Az erőforrás-kezelőben, láthatja, hogy hello hello példány állapota **felszabadítása**:

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

a virtuális gépek toostop nem felszabadítani azt, használja a hello - StayProvisioned paramétert. Hello hello nem hello - InstanceId paraméter segítségével állítsa be az összes virtuális gép leállítása

## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Indítsa újra a virtuálisgép-méretezési csoportban lévő
Cserélje le a erőforrás csoport és hello méretezési hello nevű értékeket idézőjelek között hello. Cserélje le  *#*  hello virtuális gép kívánt toorestart, és futtassa a hello azonosítóval:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Minden hello virtuális gépek a hello beállítása hello - InstanceId paraméter nem használatával indíthatja el.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>A virtuális gép eltávolítása egy méretezési csoport
Cserélje le a erőforrás csoport és hello méretezési hello nevű értékeket idézőjelek között hello. Cserélje le  *#*  hello virtuális gép kívánt tooremove, és futtassa a hello azonosítóval:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Hello virtuális gép méretezési egyszerre megszüntetheti a nem a hello - InstanceId paraméter használatával.

## <a name="change-hello-capacity-of-a-scale-set"></a>A méretezési hello kapacitás módosítása
Adja hozzá, vagy távolítsa el a virtuális gépeket hello kapacitás hello készlet módosításával. Toochange, set hello kapacitás toowhat toobe-érdemes, és frissítse hello méretezési hello új kapacitással rendelkező kívánt méretezési hello beolvasása. Ezek a parancsok cserélje le a erőforrás csoport és hello méretezési hello nevű értékeket idézőjelek között hello.

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

Virtuális gépek hello méretezési készlet távolítja el, ha hello virtuális gépek hello legmagasabb azonosítók először törlődnek.

