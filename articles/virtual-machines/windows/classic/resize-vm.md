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
# <a name="resize-a-windows-vm-created-in-hello-classic-deployment-model"></a>A Windows virtuális gépek hello klasszikus üzembe helyezési modellel létrehozott átméretezése
Ez a cikk bemutatja, hogyan tooresize egy Windows virtuális gép létrehozása hello klasszikus üzembe helyezési modellel Azure Powershell használatával.

Annak eldöntéséhez, hogy hello képességét tooresize egy virtuális Gépet, nincsenek két fogalom, amelyek szabályozzák hello tartomány méretek elérhető tooresize hello virtuális gép. első hello fogalma-hello régióban, amelyben a virtuális Gépre van telepítve. Virtuálisgép-méretek elérhető régióban hello listája hello szolgáltatások lapon hello Azure-régiókat weblap alatt áll. második hello fogalma-hello fizikai hardver a virtuális gép jelenleg üzemeltet. hello fizikai kiszolgálók virtuális gépeket üzemeltet a fürtök közös fizikai hardver együtt vannak csoportosítva. hello módszert a virtuális gép méretének módosítása eltér attól függően, hogy hello kívánt új Virtuálisgép-méretet támogatja hello hardver fürt hello VM tartalmazó.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Emellett [méretezze át a virtuális gépek hello Resource Manager üzembe helyezési modellel létrehozott](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="add-your-account"></a>A fiók hozzáadása
Klasszikus Azure-erőforrások az Azure PowerShell toowork kell konfigurálni. Kövesse az alábbi tooconfigure Azure PowerShell toomanage hagyományos erőforrások hello lépéseket.

1. Hello PowerShell parancssorába írja be a `Add-AzureAccount` kattintson **Enter**. 
2. Írja be az Azure-előfizetéshez társított hello e-mail címét, és kattintson a **Folytatás**. 
3. Adja meg a fiókhoz tartozó jelszót hello. 
4. Kattintson a **bejelentkezés**. 

## <a name="resize-in-hello-same-hardware-cluster"></a>Átméretezi a hello azonos fürt hardver
tooresize egy virtuális gép tooa terület hello hardver fürt hello virtuális Gépet üzemeltető hajtsa végre a lépéseket követve hello.

1. Futtassa a következő PowerShell-paranccsal toolist hello Virtuálisgép-méretek elérhető hello hardver fürt üzemeltető hello felhőalapú szolgáltatás, amely tartalmazza a virtuális gép hello hello.
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. Futtassa a következő parancsok tooresize hello VM hello.
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Új hardver fürt átméretezése
egy virtuális gép tooa méret nem érhető el a hello hardver fürt üzemeltet tooresize hello VM, hello felhőszolgáltatás és hello felhőalapú szolgáltatás virtuális gépeinek kell újból létre kell hozni. Minden felhőalapú szolgáltatás egyetlen hardver fürtön tárolja, ezért hello felhőalapú szolgáltatás virtuális gépeinek meg kell adni a hardver-fürtökön támogatott mérete. hello következő lépéseket ismerteti, hogyan tooresize törlésével és majd újbóli létrehozása a virtuális gépek hello felhőalapú szolgáltatás.

1. Futtassa a következő PowerShell parancs toolist hello Virtuálisgép-méretek elérhető hello régióban hello. 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. Jegyezze fel az összes konfigurációs beállítások az egyes virtuális gépek hello VM toobe átméretezték, ezért tartalmazó hello felhőszolgáltatásban. 
3. Hello felhőszolgáltatásban hello beállítás tooretain hello lemezek kiválasztása az egyes virtuális gépek összes virtuális gép törlése.
4. Hozza létre újra hello VM toobe átméretezték, ezért hello használata szükséges Virtuálisgép-méretet.
5. Hozza létre újból minden más virtuális gépek, amelyek használatával elérhető ilyenkor hello felhőszolgáltatás hello hardver fürt Virtuálisgép-méretet hello felhőszolgáltatásban.

Egy minta parancsfájlt törlése és újbóli létrehozása egy felhőszolgáltatás, új Virtuálisgép-méretet használó található [Itt](https://github.com/Azure/azure-vm-scripts). 

## <a name="next-steps"></a>Következő lépések
* [Méretezze át a virtuális gépek hello Resource Manager üzembe helyezési modellel létrehozott](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

