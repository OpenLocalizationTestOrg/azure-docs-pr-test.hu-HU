---
title: "Azure rövid útmutató– Azure Automation-fiók létrehozása | Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre Azure Automation-fiókot, és hogyan futtathat runbookot"
services: automation
author: csand-msft
ms.author: csand
ms.date: 12/13/2017
ms.topic: quickstart
ms.service: automation
ms.custom: mvc
ms.openlocfilehash: 5522c56749f9c26f376661636dd62f88017f805e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/10/2018
---
# <a name="create-an-azure-automation-account"></a>Azure Automation-fiók létrehozása

Az Azure Automation-fiókok az Azure-on keresztül hozhatók létre. Ez a módszer egy böngészőalapú felhasználói felületet biztosít az Automation-fiókok és a kapcsolódó erőforrások létrehozásához és konfigurálásához. Ez a rövid útmutató végigvezeti az Automation-fiókok létrehozásának és a fiókokban a runbookok futtatásának lépésein.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes Azure-fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="log-in-to-azure"></a>Jelentkezzen be az Azure-ba

Jelentkezzen be az Azure-ba a https://portal.azure.com címen

## <a name="create-automation-account"></a>Automation-fiók létrehozása

1. Kattintson az Azure bal felső sarkában található **Új** gombra.

1. Válassza a **Monitoring és felügyelet** lehetőséget, majd válassza az **Automation** lehetőséget.

1. Adja meg a fiókinformációkat. Az **Azure-beli futtató fiók létrehozása** területen válassza az **Igen** lehetőséget az Azure-beli hitelesítést leegyszerűsítő összetevők automatikus engedélyezéséhez. Amikor végzett, kattintson a **Létrehozás** gombra az Automation-fiók üzembe helyezésének megkezdéséhez.

    ![Adja meg az Automation-fiók információit az oldalon](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

1. Az Automation-fiók az Azure-irányítópultra van rögzítve. Az üzembe helyezés befejeztével automatikusan megnyílik az Automation-fiók áttekintése.

    ![Az Automation-fiókok áttekintése](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Runbook futtatása

Futtassa az oktatóanyag egyik runbookját.

1. Kattintson a **FOLYAMATOK AUTOMATIZÁLÁSA** területen lévő **Runbookok** lehetőségre. Megjelenik a runbookok listája. Alapértelmezés szerint több runbook is engedélyezett a fiókban.

    ![Automation-fiók runbookok listája](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. Válassza ki az **AzureAutomationTutorialScript** runbookot. Ezzel megnyílik a runbook áttekintési oldala.

    ![A runbook áttekintése](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. Kattintson az **Indítás** gombra, és a **Runbook indítása** oldalon kattintson az **OK** gombra a runbook elindításához.

    ![Runbookfeladat oldal](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. Miután a **Feladat állapota** **Fut** értékre vált, kattintson a **Kimenet** vagy a **Minden napló** lehetőségre a runbookfeladat kimenetének megtekintéséhez. A jelen oktatóanyag runbookjának kimenete az Azure-erőforrások listája.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs rá szükség, törölje az erőforráscsoportot, az Automation-fiókot és az összes kapcsolódó erőforrást. Ehhez válassza ki az Automation-fiók erőforráscsoportját, és kattintson a **Törlés** elemre.

## <a name="next-steps"></a>További lépések

Ebben a rövid útmutatóban üzembe helyezett egy Automation-fiókot, elindított egy runbook feladatot, és megtekintette a feladat részleteit. Ha bővebb információra van szüksége az Azure Automationnel kapcsolatban, folytassa az első runbook létrehozásával foglalkozó oktatóanyaggal.

> [!div class="nextstepaction"]
> [Automation rövid útmutató – Runbook létrehozása](./automation-quickstart-create-runbook.md)
