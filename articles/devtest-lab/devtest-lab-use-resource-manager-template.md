---
title: "aaaView és egy virtuális gép Azure Resource Manager-sablonnal |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure Resource Manager sablon egy virtuális gép toocreate más virtuális gépek"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: tarcher
ms.openlocfilehash: b79f7eb4171793681a0b654e6e72a83191c76bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-virtual-machines-azure-resource-manager-template"></a>A virtuális gép Azure Resource Manager sablon használata

Ha hoz létre egy virtuális gép (VM) a DevTest Labs szolgáltatásban keresztül hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040), megtekintheti a hello Azure Resource Manager sablon hello virtuális gép mentése előtt. hello sablon majd használható egy alap toocreate több tesztlabor virtuális gépek hello ugyanazokat a beállításokat.

Ez a cikk ismerteti, hogyan tooview hello-e Resource Manager-sablon a virtuális gépek létrehozásakor, és hogyan toodeploy az újabb tooautomate létrehozását hello azonos virtuális gép.

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Több virtuális Gépre kiterjedő és single-virtuális gép Resource Manager-sablonok
Két módon toocreate virtuális gépek a DevTest Labs szolgáltatásban a Resource Manager sablonnal: hello Microsoft.DevTestLab/labs/virtualmachines erőforrás kiépítése, vagy hello Microsoft.Commpute/virtualmachines erőforrás kiépítéséhez. Minden olyan más esetekben használható, és különböző engedély szükséges.

- Resource Manager-sablonok, amelyek Microsoft.DevTestLab/labs/virtualmachines erőforrás típusa (a hello "erőforrás" tulajdonságban hello sablon) hozhat létre egyéni labor virtuális gépeken. Minden virtuális gép ezután megjelenik hello DevTest Labs szolgáltatásban virtuális gépek listája egyetlen elemként:

   ![Egyetlen elem hello DevTest Labs szolgáltatásban virtuális gépek listában, virtuális gépek listája](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   A Resource Manager-sablon típusának keresztül hello Azure PowerShell-paranccsal helyezhetők **New-AzureRmResourceGroupDeployment** vagy az Azure CLI parancs hello **az csoport központi telepítésének létrehozása** . Rendszergazdai jogosultságokkal van szükség, tehát a DevTest Labs szolgáltatásban felhasználói szerepkörrel felruházott felhasználók nem hello telepítés végrehajtása. 

- Resource Manager-sablonok, amelyek Microsoft.Compute/virtualmachines erőforrás típusa építhető ki több virtuális gép, egy egyetlen környezet hello DevTest Labs szolgáltatásban virtuális gépek listája:

   ![Egyetlen elem hello DevTest Labs szolgáltatásban virtuális gépek listában, virtuális gépek listája](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Virtuális gépek ugyanabban a környezetben együtt kezelheti hello és megosztás hello azonos életciklussal. DevTest Labs szolgáltatásban felhasználói szerepkörrel felruházott felhasználók használja ezeket a sablonokat, mindaddig, amíg hello rendszergazda úgy konfigurálta a hello labor ily módon tesztkörnyezetek hozhatók létre.

hello a cikk hátralévő része a Resource Manager-sablonok Mirosoft.DevTestLab/labs/virtualmachines használó ismerteti. A tesztlabor rendszergazdák tooautomate tesztlabor virtuális gép létrehozása (például claimable virtuális gépek) és a mesterlemez (például a lemezkép gyári) által használt.

[Ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) sok útmutatást és javaslatokat toohelp kínál, amelyek megbízható és könnyű toouse Azure Resource Manager-sablonokat hoz létre.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Megtekintése és a virtuális gép Resource Manager-sablon mentése
1. Hajtsa végre a hello készítésével [az első virtuális gép létrehozása egy tesztkörnyezetben](devtest-lab-create-first-vm.md) toobegin hoz létre virtuális gépet.
1. Adja meg a virtuális gép hello szükséges információkat, és adja hozzá a virtuális gép kívánt összetevőihez.
1. Hello konfigurálása beállítások ablak hello alján válassza **nézet ARM-sablon**.

   ![Nézet ARM-sablon gomb](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Másolja ki és mentse a hello Resource Manager sablon toouse újabb toocreate egy másik virtuális géphez.

   ![Resource manager sablon toosave későbbi használatra](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Hello Resource Manager-sablon mentése után frissítenie kell hello paraméterek szakaszban hello sablon használata előtt. Létrehozhat egy parameter.json, amely csak hello paraméterek kívül hello tényleges Resource Manager sablon testreszabása. 

![A JSON-fájl használatával paramétereket testreszabása](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

## <a name="deploy-a-resource-manager-template-toocreate-a-vm"></a>A Resource Manager sablon toocreate egy virtuális gép telepítése
Miután a Resource Manager-sablon mentése, és igény szerint testreszabott, használhatja tooautomate Virtuálisgép létrehozásához. [Erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) ismerteti, hogyan toouse Azure PowerShell, a Resource Manager sablonok toodeploy az erőforrások tooAzure. [Erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) ismerteti, hogyan toouse Azure parancssori felület a Resource Manager sablonok toodeploy az erőforrások tooAzure.

> [!NOTE]
> Csak a tesztkörnyezet tulajdonosa engedélyekkel rendelkező felhasználó Azure PowerShell használatával létrehozhat egy Resource Manager-sablon virtuális gépeket. Ha azt szeretné, hogy tooautomate virtuális gép létrehozása egy Resource Manager-sablon használatával, és a felhasználói engedélyek csak rendelkezik, használhatja a hello [ **az tesztlabor virtuális gép létrehozása** parancsot a hello CLI](https://docs.microsoft.com/cli/azure/lab/vm#create).

### <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[virtuális Gépre kiterjedő környezetek létrehozása a Resource Manager-sablonok](devtest-lab-create-environment-from-arm.md).
* További gyors üzembe helyezési Resource Manager-sablonok a DevTest Labs automatizálásához áttekintheti az hello [nyilvános DevTest Labs GitHub-tárház](https://github.com/Azure/azure-quickstart-templates).
