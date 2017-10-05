---
title: "Azure-erőforrások telepítése Azure portál használatával |} Microsoft Docs"
description: "Azure-portál és az Azure Resource Manager segítségével az erőforrások telepítése."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 2c98a4aa-8d9f-4a0a-b764-214dbe8ed009
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: a4cac5834c667976b0a2d1f2748e4309a166d16a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure Portallal
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portál](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

Ez a témakör bemutatja, hogyan használja a [Azure-portálon](https://portal.azure.com) rendelkező [Azure Resource Manager](resource-group-overview.md) központi telepítése az Azure-erőforrások. Az erőforrások kezelésével kapcsolatos információkért lásd: [kezelése Azure-erőforrások portálon keresztül](resource-group-portal.md).

Jelenleg nem minden szolgáltatás támogatja, a portál vagy az erőforrás-kezelő. Ezek a szolgáltatások kell használnia a [klasszikus portál](https://manage.windowsazure.com). Minden szolgáltatás állapotát, lásd: [az Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Erőforráscsoport létrehozása
1. Hozzon létre egy üres erőforráscsoportot, válassza ki **új** > **felügyeleti** > **erőforráscsoport**.
   
    ![üres erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. Adjon neki egy nevet és egy helyet, és szükség esetén válasszon egy előfizetést. Meg kell adnia az erőforrásnak a helyét, mert az erőforráscsoport erőforrásokra vonatkozó metaadatokat tárol. Megfelelőségi okokból érdemes lehet, hogy metaadatai tárolási helyének meghatározásához. Általában javasoljuk, hogy megadja a helyét, amelyben az erőforrások többségét tárolva lesz. Ugyanazon a helyen használatával egyszerűbbé teheti a sablont.
   
    ![csoport értékeket](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Erőforrások a piactér telepítése
Miután létrehozott egy erőforráscsoport, a piactérről erőforrások telepítheti azt. A piactér előre definiált megoldást nyújt a gyakori forgatókönyvek.

1. A telepítés elindításához válassza ki a **új** és a telepíteni kívánt erőforrás típusát. Ezután keresse meg az adott verzióját szeretné telepíteni az erőforrás.
   
    ![erőforrás telepítése](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. Ha nem látja az adott megoldás szeretne telepíteni, akkor is kereshet a piactér azt.
   
    ![a piactéren](./media/resource-group-template-deploy-portal/search-resource.png)
3. Attól függően, hogy a kiválasztott erőforrás típusát hogy egy központi telepítés előtt állítsa be a megfelelő tulajdonságok gyűjteménye. Ezek a lehetőségek nem itt látható, mivel azok erőforrástípus függően változhat. Az összes ki kell választania a célként megadott erőforráscsoport. A következő kép bemutatja, hogyan hozzon létre egy webalkalmazást, és telepítse azt a létrehozott erőforráscsoportot.
   
    ![Erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    Azt is megteheti Ha dönt, hogy hozzon létre egy erőforráscsoportot az erőforrások telepítése során. Válassza ki **hozzon létre új** , és nevezze el az erőforráscsoportot.
   
    ![Új erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-new-group.png)
4. A telepítés megkezdése. A központi telepítés eltarthat néhány percig. A telepítés befejezését, megjelenik egy értesítés.
   
    ![értesítési nézet](./media/resource-group-template-deploy-portal/view-notification.png)
5. Az erőforrások való telepítése után adhat hozzá további erőforrásokat az erőforráscsoport segítségével a **Hozzáadás** parancs az erőforráscsoport panel.
   
    ![erőforrás hozzáadása](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Az egyéni sablont az erőforrások telepítése
Ha szeretné a központi telepítés hajtható végre, de nem használja a sablonok a piactéren, létrehozhat egy egyéni sablont, amely meghatározza a megoldás infrastruktúráját. Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).

1. Egy egyéni sablon a portálon keresztül történő üzembe helyezéséhez válassza **új**, és kezdeni a keresést a **sablon-üzembehelyezés** mindaddig, amíg a lehetőségek közül választhat.
   
    ![keresési sablon-üzembehelyezés](./media/resource-group-template-deploy-portal/search-template.png)
2. Válassza ki **sablon-üzembehelyezés** elérhető erőforrások.
   
    ![Válassza ki a sablon telepítése](./media/resource-group-template-deploy-portal/select-template.png)
3. Miután elindította a sablon-üzembehelyezés, nyissa meg az üres sablon testreszabása érhetők el.
   
    ![Sablon létrehozása](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    A szerkesztő vegye fel a JSON-szintaxis, amely meghatározza a telepíteni kívánt erőforrásokat. Válassza ki **mentése** végzett. A JSON-szintaxis írásáról útmutatóért lásd: [Resource Manager sablonokhoz](resource-manager-template-walkthrough.md).
   
    ![sablon szerkesztése](./media/resource-group-template-deploy-portal/edit-template.png)
4. Vagy választhat egy már meglévő sablon, a [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/). Ezeket a sablonokat a Közösség által közzé. Számos gyakori helyzetek terjed ki, és valaki hozzáadta a sablont, amely hasonló mi próbál telepíteni. A sablonok található, amelyet a forgatókönyvének megfelelő kereshet.
   
    ![Válassza ki a következő gyorsindítási sablonon](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    A kiválasztott sablont a szerkesztőben tekintheti meg.
5. Miután megadta a többi érték, válassza ki a **létrehozása** a sablon telepítéséhez. 
   
    ![sablon üzembe helyezése](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Egy sablonból fiókjába erőforrások telepítése
A portál lehetővé teszi egy sablon mentése az Azure-fiókjával, és telepítse újra később. További információ ezen sablonok, mentett használata [Ismerkedés az Azure portálon magánsablonok](../marketplace-consumer/mytemplates-getstarted.md).

1. A mentett sablonok találhatók, válassza ki a **Tallózás** > **sablonok**.
   
    ![Keresse meg a sablonok](./media/resource-group-template-deploy-portal/browse-templates.png)
2. Fiókjába sablonok listájának megtekintéséhez jelölje ki a használni kívánt egy.
   
    ![mentett sablonok](./media/resource-group-template-deploy-portal/saved-templates.png)
3. Válassza ki **telepítés** újratelepíteni ezt a mentett sablont.
   
    ![mentett sablon üzembe helyezése](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Következő lépések
* Naplók megtekintése: [naplózási műveletek a Resource Manager](resource-group-audit.md).
* Telepítési hibák elhárításához lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
* Egy központi telepítés vagy az erőforráscsoport a sablon lekéréséhez lásd: [Azure Resource Manager sablon exportálása létező erőforrásokból](resource-manager-export-template.md).
* Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.

