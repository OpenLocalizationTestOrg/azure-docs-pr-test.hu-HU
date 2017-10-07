---
title: "a virtuális gép rendelkezésre állási aaaCreate beállítása az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy felügyelt rendelkezésre állási beállítása, vagy nem felügyelt rendelkezésre állási csoportot a virtuális gépek Azure PowerShell használatával, vagy hello portal hello Resource Manager üzembe helyezési modellben."
keywords: "A rendelkezésre állási csoport"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a3db8659-ace8-4e78-8b8c-1e75c04c042c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eadcdfcd28bb2fa21a4647f207b390c33e022ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a>Növelje virtuális gép rendelkezésre állása Azure rendelkezésre állási csoport létrehozása 
Rendelkezésre állási készletek biztosítanak redundanciát tooyour alkalmazás. Javasoljuk, hogy legalább két virtuális gép rendelkezésre állási csoportba. Ez a konfiguráció biztosítja, hogy vagy a tervezett vagy nem tervezett karbantartási események esetén legalább egy virtuális gép lesz elérhető és igazodhat hello 99,95 % Azure SLA-t. További információkért lásd: hello [SLA-t a virtuális gépek](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Virtuális gépek kell létrehozni hello azonos erőforráscsoporthoz tartozik, mint hello rendelkezésre állási csoportot.
> 

Ha azt szeretné, hogy a virtuális gép toobe részei egy rendelkezésre állási csoportnak, szükség van-e toocreate hello rendelkezésre állási beállítása első, vagy ha létre az első virtuális gép hello készletében. A virtuális Gépet felügyelt lemezek fog használni, ha egy felügyelt rendelkezésre állási csoportot, hello rendelkezésre állási csoportot kell létrehozni.

Létrehozásával és a rendelkezésre állási csoportokkal kapcsolatos további információkért lásd: [hello virtuális gépek rendelkezésre állásának kezelése](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="use-hello-portal-toocreate-an-availability-set-before-creating-your-vms"></a>Hello portál toocreate rendelkezésre állási készlet létrehozása a virtuális gépek előtt használata
1. Hello menüben kattintson **Tallózás** válassza **rendelkezésre állási készletek**.
2. A hello **rendelkezésre állási készletek panel**, kattintson a **Hozzáadás**.
   
    ![Képernyőkép a hello hozzáadása egy új rendelkezésre állási csoport létrehozása gombra.](./media/create-availability-set/add-availability-set.png)
3. A hello **rendelkezésre állási csoport létrehozása** panelen, a készlet teljes hello adatait.
   
    ![Képernyőfelvétel, hogy látható hello tooenter toocreate hello rendelkezésre állási szükséges információk beállítása.](./media/create-availability-set/create-availability-set.png)
   
   * **Név** -hello név számokat, betűket, pontokat, aláhúzásjeleket és kötőjeleket áll 1 – 80 karakter lehet. hello első karakterének betűnek vagy számnak kell lennie. hello utolsó karakter betű, szám vagy aláhúzásjel kell lennie.
   * **Tartományok fault** -tartalék tartományok definiálása hello csoport a virtuális gépek, amelyek egy közös power forrás- és a hálózati kapcsolóhoz. Alapértelmezés szerint a hello virtuális gépek mentése toothree tartalék tartományok között egymástól, és a módosított toobetween 1. és 3 lehet.
   * **Tartományok frissítése** - öt frissítési tartományok alapértelmezés szerint tartoznak, és ez 1 és 20 toobetween állítható. Frissítési tartományok jelzi a virtuális gépek és a mögöttes fizikai hardver, hogy újra kell indítani a hello csoportok ugyanannyi időt vesz igénybe. Például ha azt adja meg a tartományokat, több mint öt virtuális gépek belül egy egyetlen rendelkezésre állási csoportban, hello hatodik virtuális gép konfigurálásakor bekerülnek öt frissítés hello hello első virtuális gépen, azonos frissítési tartományban hello hetedik a hello azonos UD hello második virtuális gép, és így tovább. hello újraindítások hello sorrendje nem lehet egymást követő, de csak egy frissítési tartományt újraindításáról egyszerre.
   * **Előfizetés** -válassza hello előfizetés toouse, ha több van.
   * **Erőforráscsoport** -válasszon egy meglévő erőforráscsoportot hello mutató nyílra, majd válassza az erőforráscsoport hello a legördülő listán. Írja be a nevet is létrehozhat egy új erőforráscsoportot. hello neve is tartalmazhatja hello a következő karaktereket: betűket, számokat, pontokat, kötőjeleket, aláhúzásjeleket és nyitó vagy záró zárójel. hello neve nem végződhet ponttal. Összes hello rendelkezésre állási csoportban lévő virtuális gépek hello kell hello létrehozott toobe erőforráscsoportjában hello rendelkezésre állási csoportot.
   * **Hely** -a hello legördülő listán jelöljön ki egy helyet.
   * **Felügyelt** – Itt adhatja meg *Igen* toocreate egy felügyelt rendelkezésre állási beállítása toouse virtuális gépek, amelyek a felügyelt lemezeket használjanak tárhelyként. Válassza ki **nem** Ha hello hello beállítása lévő virtuális gépek a tárfiók nem felügyelt lemezek használhatók.
   
4. Ha befejezte az adatok hello megadásával, kattintson a **létrehozása**. 

## <a name="use-hello-portal-toocreate-a-virtual-machine-and-an-availability-set-at-hello-same-time"></a>Használja a virtuális gépek és a rendelkezésre állási beállított hello azonos hello portál toocreate idő
Hello portál segítségével új virtuális gép létrehozásakor is létrehozhat egy új rendelkezésre állási hello létrehozása során a virtuális gép hello beállítása hello beállítása az első virtuális gép. Ha a virtuális gép toouse felügyelt lemezeket, egy felügyelt rendelkezésre állási csoport jön létre.

![Képernyőkép a hello folyamat létrehozásához egy új rendelkezésre állási beállítása hello virtuális gép létrehozása során.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-tooan-existing-availability-set-in-hello-portal"></a>Adja hozzá egy új virtuális gép tooan meglévő rendelkezésre állási csoport hello portál
Az egyes további virtuális gépek létrehozásakor kell hello készlet tartozik, győződjön meg arról, hogy létrehozta a hello azonos **erőforráscsoport** és majd a válassza ki a 3. lépésben beállítása rendelkezésre állási hello. 

![Képernyőfelvétel: hogyan tooselect rendelkezésre állási toouse beállítani a virtuális Gépet.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-toocreate-an-availability-set"></a>Használjon PowerShell toocreate rendelkezésre állási beállítása
Ebben a példában hoz létre a rendelkezésre állási készlet elnevezett **myAvailabilitySet** a hello **myResourceGroup** hello erőforráscsoportja **USA nyugati régiója** helyét. Toobe hello létrehozása előtt meg kell első virtuális gép, amely hello készlet lesz.

Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját. Futtatás hello parancs tooinstall követően.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).


Ha a virtuális gépek felügyelt lemezt használ, írja be:

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

Ha a virtuális gépek használ a saját storage-fiókok, írja be:

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

További információkért lásd: [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).

## <a name="troubleshooting"></a>Hibaelhárítás
* Amikor létrehoz egy virtuális Gépet, ha nem kívánt hello rendelkezésre állási csoport hello portálon hello legördülő listában, létrehozott azt egy másik erőforráscsoportban található. Ha nem tudja hello erőforráscsoportot a rendelkezésre állási beállítása, nyissa meg toohello központ menü, és kattintson a Tallózás > rendelkezésre állási készletek toosee beállítja a rendelkezésre állási listáját, és mely erőforrás-csoportok tartoznak.

## <a name="next-steps"></a>Következő lépések
Adja hozzá a további tárhely tooyour VM további hozzáadásával [adatlemez](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

