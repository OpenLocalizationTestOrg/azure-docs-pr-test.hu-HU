---
title: "aaaCreate vagy a PowerShell használatával automatikusan Azure Resource Manager-sablonok labs módosítása |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure Resource Manager-sablon is van a PowerShell toocreate vagy automatikusan labor DevTest labs módosítása"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: tarcher
ms.openlocfilehash: 29c8bc67caaec17b1f8926dde4e5d9d314b06600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Létrehozhat vagy módosíthat labs automatikusan az Azure Resource Manager-sablonok és a PowerShell használatával

DevTest Labs biztosít sok Azure Resource Manager-sablonok és a PowerShell-parancsfájlok, amely gyorsan és automatikusan hozzon létre új labs vagy módosítsa a meglévő labs és majd telepítenie kell ezeket az erőforrásokat.

Ez a cikk segít hello folyamatot, amely használatával a sablonok és a parancsfájlok tooautomate hello létrehozását, módosítását és a labs központi telepítését ismerteti. Ez a cikk azt is bemutatja, hogyan toouse PowerShell tooperform néhány gyakori feladatok a DevTest Labs szolgáltatásban kapcsolatos további információk megkeresésének.

## <a name="step-1-gather-your-templates-and-scripts"></a>1. lépés: A sablonok és a parancsfájlok összegyűjtése
Előre végrehajtott található [Azure Resource Manager-sablonok](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) és [PowerShell-parancsfájlok](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) , a nyilvános [Github-tárházban](https://github.com/Azure/azure-devtestlab). Használja őket-van, vagy testre is szabhatja őket az igényeinek, és tárolja őket a saját [privát Git-tárház](devtest-lab-add-artifact-repo.md). 

## <a name="step-2-modify-your-azure-resource-manager-template"></a>2. lépés: Az Azure Resource Manager-sablon módosítása
Hajtsa végre az hello készítésével [az első Azure Resource Manager-sablon létrehozása](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) Ha soha nem hozott létre egy sablont előtt.

Emellett [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) sok útmutatást és javaslatokat toohelp kínál, amelyek megbízható és könnyű toouse Azure Resource Manager-sablonokat hoz létre. Általában egy változata, hello módszerek egyikét használhatja vagy példák megadott, és módosítsa a sablon az igényeinek.

## <a name="step-3-deploy-resources-with-powershell"></a>3. lépés: Erőforrások az PowerShell telepítése
Miután a sablonok és a parancsfájlok testreszabott, lépésekkel hello szükséges túl[erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). hello a cikk az Azure PowerShell használata az Azure Resource Manager sablonok toodeploy általános információkat az erőforrások tooAzure nyújt.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>Gyakori feladatok elvégzése a DevTest Labs szolgáltatásban a PowerShell használatával
Nincsenek sok kapcsolatos további általános feladatok PowerShell segítségével automatizálható. hello hello dokumentáció a következő szakaszok felsorolják hello lépéseket szükséges tooperform ezeket a feladatokat.

* [Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [Töltse fel a VHD fájlt toolab tárfiók PowerShell használatával](devtest-lab-upload-vhd-using-powershell.md)
* [Adja hozzá egy külső felhasználó tooa labor PowerShell használatával](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [PowerShell-lel labor egyéni szerepkör létrehozása](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Következő lépések
* Megtudhatja, hogyan toocreate egy [privát Git-tárház](devtest-lab-add-artifact-repo.md) tárolására az egyéni sablonok vagy parancsfájlok.
* Fedezze fel hello [Azure Resource Manager sablonok Azure gyors üzembe helyezési sablon gyűjteményből](https://github.com/Azure/azure-quickstart-templates).
