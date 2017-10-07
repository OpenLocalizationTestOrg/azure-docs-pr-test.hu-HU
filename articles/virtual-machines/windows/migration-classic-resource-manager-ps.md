---
title: a PowerShell-lel Manager aaaMigrate tooResource |} Microsoft Docs
description: "Ez a cikk végigvezeti hello platform által támogatott áttelepítési IaaS-erőforrások, például a virtuális gépek (VM), a virtuális hálózatokon (Vnetek) és a storage-fiókok klasszikus tooAzure Resource Managerrel (ARM) az Azure PowerShell-parancsok segítségével"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: b5b2987da292f1c241be71a354b0c2e1a96a07c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-powershell"></a>IaaS-erőforrásokra át klasszikus tooAzure erőforrás-kezelő Azure PowerShell használatával
Ezen lépések bemutatják, hogyan toouse Azure PowerShell-parancsok toomigrate infrastruktúra hello klasszikus telepítési modell toohello Azure Resource Manager telepítési modellből a szolgáltató (IaaS) erőforrásként.

Ha azt szeretné, is áttelepítheti erőforrások hello segítségével [Azure parancssori felület (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).

* A támogatott áttelepítési forgatókönyvek háttér, lásd: [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének Platform által támogatott](migration-classic-resource-manager-overview.md).
* Részletes útmutatást és egy áttelepítési forgatókönyv: [műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](migration-classic-resource-manager-deep-dive.md).
* [A leggyakoribb áttelepítési hibák áttekintése](migration-classic-resource-manager-errors.md)

<br>
Ez a folyamatábra tooidentify hello sorrendet, amelyben lépést kell végrehajtani az áttelepítési folyamat során toobe

![Képernyőkép a hello áttelepítés lépései](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a>1. lépés: Az áttelepítés tervezése
Az alábbiakban néhány gyakorlati tanácsok, amely értékeli a klasszikus tooResource Manager áttelepítése IaaS-erőforrásokra, javasoljuk:

* Olvassa végig hello [támogatott és nem támogatott a szolgáltatásnak és konfigurálásnak](migration-classic-resource-manager-overview.md). Ha még nem támogatott konfigurációk vagy szolgáltatások használó virtuális gépek, azt javasoljuk, várja meg a hello konfiguráció/szolgáltatás támogatási toobe jelentette be. Azt is megteheti Ha azt az igényeinek megfelelő, távolítsa el a szolgáltatást, vagy konfigurációs tooenable áttelepítést kilépni.
* Ha olyan parancsfájlok, amelyek központi telepítése az infrastruktúra és az alkalmazások ma rendelkezik automatikus, próbálja toocreate egy hasonló vizsgálat beállítása az áttelepítés ezen parancsfájlok használatával. Másik lehetőségként állíthat be minta környezetek hello Azure-portál használatával.

> [!IMPORTANT]
> Alkalmazásátjárót jelenleg nem támogatottak az áttelepítést a klasszikus tooResource Manager. toomigrate Alkalmazásátjáró, klasszikus virtuális hálózat hello átjáró előkészítési művelet toomove hello hálózati futtatása előtt távolítsa el. Hello áttelepítés befejezése után újra hello átjáró az Azure Resource Manager.
>
>Csatlakozás tooExpressRoute Kapcsolatcsoportok egy másik előfizetésben található ExpressRoute-átjárók nem telepíthetők át automatikusan. Ilyen esetekben hello ExpressRoute-átjáró eltávolítása, telepítse át a virtuális hálózati hello, és hozza létre újra a hello átjáró. Ellenőrizze a [áttelepítése ExpressRoute áramkörök és társított virtuális hálózatokat hello klasszikus toohello Resource Manager üzembe helyezési modellben](../../expressroute/expressroute-migration-classic-resource-manager.md) további információt.
>
>

## <a name="step-2-install-hello-latest-version-of-azure-powershell"></a>2. lépés: Hello Azure PowerShell legújabb verziójának telepítése
Nincsenek a két fő lehetőség közül választhat tooinstall Azure PowerShell: [PowerShell-galériában](https://www.powershellgallery.com/profiles/azure-sdk/) vagy [Webplatform-telepítővel (WebPI)](http://aka.ms/webpi-azps). WebPI havi frissítések kap. PowerShell-galériában folyamatosan frissítések kap. Ez a cikk az Azure PowerShell verzió 2.1.0 alapul.

A telepítési utasításokért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-hello-subscription-in-azure-portal"></a>3. lépés: Ellenőrizze, hogy a rendszergazda hello előfizetés Azure-portálon
tooperform az áttelepítés meg kell adni a hello hello előfizetés társadminisztrátoraként [Azure-portálon](https://portal.azure.com).

1. Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben válassza ki a **előfizetés**. Ha nem látja, válassza ki a **további szolgáltatások**.
3. Megfelelő hello bejegyzés található, akkor nézze meg hello **saját SZEREPKÖR** mező. Egy közös rendszergazda hello értéket kell _fiókadminisztrátor_.

Ha nem tudja tooadd társadminisztrátorának, majd lépjen kapcsolatba a szolgáltatás-rendszergazdai vagy társadminisztrátori hello előfizetés tooget saját kezűleg a hozzá tartozó.   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>4. lépés: Állítsa be az előfizetéshez, és regisztráljon áttelepítése
Először egy PowerShell-parancssorba. Az áttelepítéshez, mindkét klasszikus környezet szüksége tooset és erőforrás-kezelő.

Jelentkezzen be a fiók tooyour hello Resource Manager modellt.

```powershell
    Login-AzureRmAccount
```

Hello az elérhető előfizetések beolvasása hello a következő parancs használatával:

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

Állítsa be az Azure-előfizetéshez hello az aktuális munkamenet. Ez a példa beállítása hello alapértelmezett előfizetés neve túl**saját Azure-előfizetés**. A saját cserélje le a hello példa előfizetés nevét.

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Regisztrációs egy egyszeri lépést, de egyszer a migrálás megkezdése előtt kell tennie. Ha nem regisztrálja, hello a következő hibaüzenet jelenik meg:
>
> *BadRequest: Az előfizetés nincs regisztrálva az áttelepítéshez.*
>
>

Hello áttelepítési erőforrás-szolgáltató regisztrálása hello a következő parancs használatával:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Kis türelmet hello regisztrációs toofinish öt percet. Hello jóváhagyási hello állapotának hello a következő parancs használatával ellenőrizheti:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Győződjön meg arról, hogy van-e RegistrationState `Registered` folytatás előtt.

Most jelentkezzen be a klasszikus modellt hello tooyour fiókjával.

```powershell
    Add-AzureAccount
```

Hello az elérhető előfizetések beolvasása hello a következő parancs használatával:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Állítsa be az Azure-előfizetéshez hello az aktuális munkamenet. Ez a példa beállítja hello alapértelmezett előfizetés túl**saját Azure-előfizetés**. A saját cserélje le a hello példa előfizetés nevét.

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a>5. lépés: Ellenőrizze, hogy elegendő Azure Resource Manager virtuális gép magok a hello Azure-régió, a jelenlegi üzemelő példány vagy virtuális hálózaton
A következő PowerShell parancs toocheck hello aktuális magok száma rendelkezik az Azure Resource Manager hello is használhatja. toolearn kvótákat, az alapvető kapcsolatos további információkért lásd: [korlátozásai és hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).

Ez a példa hello hello rendelkezésre állását ellenőrzi. **USA nyugati régiója** régióban. A saját cserélje le a hello például régió neve.

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-toomigrate-your-iaas-resources"></a>6. lépés: Futtassa a parancsokat toomigrate az IaaS-erőforrások
> [!NOTE]
> Az itt ismertetett összes hello művelet idempotent. Ha a probléma nem támogatott szolgáltatása vagy konfigurációs hiba, azt javasoljuk, hogy újra hello előkészítése, megszakítása vagy véglegesítése a műveletet. hello platform hello művelet újra próbálkozik.
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>6.1. lépés: 1. lehetőség – (nem része virtuális hálózatnak) felhőszolgáltatás a virtuális gépek áttelepítése
Hello listájának beszerzése felhőszolgáltatások hello a következő parancs használatával, majd mentse hello felhőalapú szolgáltatás, amelyet az toomigrate. Ha hello virtuális gépek hello a felhőszolgáltatásban található egy virtuális hálózatot, vagy webes vagy feldolgozói szerepköröket rendelkeznek, hello parancs hibaüzenetet ad vissza.

```powershell
    Get-AzureService | ft Servicename
```

Hello telepítési neve hello felhőszolgáltatás beolvasása. Ebben a példában hello szolgáltatás neve megkülönbözteti a **saját szolgáltatás**. Cserélje le a saját szolgáltatásnév hello példa szolgáltatás neve.

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Készítse elő a hello virtuális gépek áttelepítésre hello felhőszolgáltatásban. A két beállítások toochoose rendelkezik.

* **1. lehetőség. Hello virtuális gépek tooa platform által létrehozott virtuális hálózat áttelepítése**

    Először ellenőrzi, hogy hello felhőalapú szolgáltatás a következő parancsok hello segítségével telepíthet át:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    hello előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés. Ha az ellenőrzés nem jelez hibát, majd továbbléphet toohello **Prepare** . lépés:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **2. lehetőség. Tooan meglévő virtuális hálózat hello Resource Manager üzembe helyezési modellel áttelepítése**

    Ebben a példában beállítása az erőforráscsoport neve túl hello**myResourceGroup**, virtuális hálózat neve túl hello**myVirtualNetwork** és alhálózati név túl hello**mySubNet**. Hello nevek hello példában cserélje le a saját erőforrások hello nevét.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Először ellenőrzi, hogy áttelepítheti a virtuális hálózat hello hello a következő parancs használatával:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    hello előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés. Ha az ellenőrzés nem jelez hibát, majd folytassa a hello előkészítési lépés a következő:

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

Után hello előkészítési művelet sikeres hello megelőző beállítások valamelyikével, a lekérdezés hello áttelepítési állapotának hello virtuális gépek. Győződjön meg arról, hogy vannak-e a hello `Prepared` állapotát.

Ez a példa beállítása virtuális gép neve túl hello**myVM**. Hello példa neve cserélje le a saját virtuális gép nevét.

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

A PowerShell vagy a hello Azure portál segítségével erőforrások előkészített hello hello konfigurációjának ellenőrzése. Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a>6.1. lépés: 2. lehetőség – a virtuális hálózatban lévő virtuális gépek áttelepítése

toomigrate virtuális gépek virtuális hálózatban, hello virtuális hálózat áttelepítése. hello virtuális gépek automatikusan áttelepíti a hello virtuális hálózattal. Mentse hello virtuális hálózatot, hogy szeretné-e toomigrate.
> [!NOTE]
> [Egyetlen klasszikus virtuális gép áttelepítése](migrate-single-classic-to-resource-manager.md) hozzon létre egy új erőforrás-kezelő virtuális gép hello (az operációs rendszer és data) virtuális merevlemezfájlokat hello virtuális gép használatával felügyelt lemezzel.
<br>

> [!NOTE]
> hello virtuálishálózat-név eltérhet hello megjelenő új portált. hello új Azure-portál megjeleníti hello néven `[vnet-name]` hello tényleges virtuális hálózati név típusú, de `Group [resource-group-name] [vnet-name]`. Mielőtt áttelepítené a keresési tényleges virtuálishálózat-név hello hello paranccsal `Get-AzureVnetSite | Select -Property Name` , illetve hello régi Azure-portálon. 

Ez a példa beállítása hello virtuálishálózat-név túl**myVnet**. A saját cserélje le a hello példa a virtuális hálózat nevére.

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Hello virtuális hálózat nem támogatott konfigurációjú webes vagy feldolgozói szerepköröket, vagy a virtuális gépeket tartalmaz, ha egy érvényesítési hibaüzenet kap.
>
>

Először ellenőrzi, hogy a virtuális hálózati hello hello a következő parancs használatával telepíthet át:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

hello előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés. Ha az ellenőrzés nem jelez hibát, majd folytassa a hello előkészítési lépés a következő:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

A virtuális gépek Azure PowerShell vagy az Azure-portálon hello segítségével előkészített hello hello konfigurációjának ellenőrzése. Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a>A tárfiók lépés 6.2 áttelepítése
Ha elkészült hello virtuális gépeinek áttelepítését, ajánlott hello tárfiókok az áttelepítést.

Hello tárfiók az áttelepítés előtt hajtson végre megelőző előfeltétel-ellenőrzések:

* **Klasszikus, amelynek lemezek hello storage-fiókban tárolt virtuális gépek áttelepítése**

    Minden hello klasszikus virtuális gépek lemezei RoleName és DiskName tulajdonságainak hello tárfiók parancs megelőző adja vissza. RoleName hello neve hello virtuális gép toowhich lemez van csatolva. Ha parancs megelőző lemezek majd győződjön meg arról, hogy ezek a lemezek vannak csatolva hozzá virtuális gépek toowhich települnek át áttelepítése előtt hello storage-fiók.
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* **Hello storage-fiókban tárolt választani klasszikus virtuális gépek lemezei törlése**

    Található, nem csatolt klasszikus virtuális gépek lemezei hello tárolt fiók használatával a következő parancsot:

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    Ha a fenti parancs beolvasása lemezek törölje ezeket a következő parancs használatával lemezeket:

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* **Hello storage-fiókban tárolt Virtuálisgép-rendszerképek törlése**

    Előző parancs adja vissza minden hello Virtuálisgép-rendszerképek hello storage-fiókban tárolt operációsrendszer-lemezzel.
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     Adatlemezek hello storage-fiókban tárolt összes hello Virtuálisgép-rendszerképek parancs megelőző adja vissza.
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    Törölje az előző parancs használata parancsok fent által visszaadott összes hello VM lemezképet:
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

Minden tárfiók az áttelepítés ellenőrzése hello a következő parancs használatával. Ebben a példában hello tárfiókneve **myStorageAccount**. Hello példa neve helyére a saját tárfiók hello nevét.

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

Következő lépés az tooPrepare hello storage-fiók áttelepítése

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Az Azure PowerShell vagy az Azure-portálon hello segítségével tárfiók előkészített hello hello konfigurációjának ellenőrzése. Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Következő lépések
* [IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítésének áttekintése](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Parancssori felület toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének védelmével kapcsolatos közösségi eszközök](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [A leggyakoribb áttelepítési hibák áttekintése](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Felülvizsgálati hello legtöbb kapcsolatos gyakori kérdések a klasszikus tooAzure erőforrás-kezelő áttelepítése IaaS-erőforrásokra](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
