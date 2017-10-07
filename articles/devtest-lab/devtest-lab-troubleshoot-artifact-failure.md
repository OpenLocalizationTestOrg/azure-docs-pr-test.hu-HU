---
title: "Azure DevTest Labs szolgáltatásban virtuális aaaDiagnose összetevő hibáinak |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot összetevő hibák a DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: tarcher
ms.openlocfilehash: 40b3cea72cf071cc5d9a6d002d309d923c3d3084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-artifact-failures-in-hello-lab"></a>Összetevő hibák diagnosztizálása a hello tesztkörnyezet 
A létrehozást követően összetevő, toosee ellenőrizheti, ha sikeres vagy sikertelen volt. Összetevő-naplókat a DevTest Labs szolgáltatásban információkkal toodiagnose összetevő hibát is használhatja. Többféleképpen néhány hello összetevő naplózási információk a Windows virtuális gépek is megtekinthetők.

> [!NOTE]
> tooensure hibák helyesen azonosítani és ismertetése, fontos, hogy hello összetevő megfelelően épül. Hogyan toocorrectly összeállításához összetevő kapcsolatos információkért lásd: [egyéni összetevők létrehozása](devtest-lab-artifact-author.md). Toosee például lehet egy megfelelően strukturált összetevő, és tekintse meg a [paraméter teszttípust](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) összetevő.

## <a name="troubleshoot-artifact-failures-using-hello-azure-portal"></a>Hello Azure-portál használatával összetevő hibák elhárítása
toouse hello Azure portál toodiagnose összetevő létrehozása során fellépő hibák kövesse az alábbi lépéseket:

1. Az erőforrások hello listájából válassza ki a labor.

2. Válassza ki a Windows virtuális Gépet, amely tartalmazza a kívánt tooinvestigate hello összetevő hello.

3. A bal oldali panelen hello alatt **általános**, válassza a **összetevők**. Meg ezt a virtuális Gépet társított összetevők listáját, jelző hello nevét hello összetevő és annak állapotát.

   ![Összetevő git-tárház példa](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Válassza ki, amely egy állapotát jeleníti meg összetevő **sikertelen**. hello összetevő megnyílik, és egy hello összetevő hibája hello kapcsolatos adatokat tartalmazó bővítmény üzenetet jelenít meg.

   ![Összetevő git-tárház példa](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-hello-vm"></a>Virtuális gép hello belül származó összetevő hibák elhárítása
tooview hello összetevő származó naplók hello virtuális gépekről, kövesse az alábbi lépéseket:

1. Jelentkezzen be a virtuális Gépet, amely tartalmazza azt szeretné, hogy toodiagnose hello összetevő toohello.

2. Keresse meg a tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status "1.9 esetén hello ügyféloldali Bővítmény verziószámát.

   ![Összetevő git-tárház példa](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Nyissa meg hello **állapot** tooview adatait, hogy a segítségével összetevő hibák diagnosztizálása ezt a virtuális gépet.




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések
* [Csatlakozás a virtuális gép tooexisting resource manager-sablon használata az Azure DevTest Labs AD-tartomány](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[egy Git-tárház tooa labor hozzáadása](devtest-lab-add-artifact-repo.md).

