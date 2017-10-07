---
title: "az Azure portál toodeploy aaaUse Azure-erőforrások |} Microsoft Docs"
description: "Azure-portál és az Azure Resource Manager toodeploy az erőforrások használatára."
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
ms.openlocfilehash: 5a5217f94c8dfc0c1ebd613903ea3dcbe1197bfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure Portallal
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portál](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

Ez a témakör bemutatja, hogyan toouse hello [Azure-portálon](https://portal.azure.com) rendelkező [Azure Resource Manager](resource-group-overview.md) toodeploy az Azure-erőforrások. az erőforrások kezelése toolearn lásd [kezelése Azure-erőforrások portálon keresztül](resource-group-portal.md).

Nem minden szolgáltatás jelenleg hello portálon vagy az erőforrás-kezelő. Ezek a szolgáltatások toouse hello kell [klasszikus portál](https://manage.windowsazure.com). Minden szolgáltatás hello állapotát, lásd: [az Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Erőforráscsoport létrehozása
1. egy üres erőforráscsoporthoz toocreate kiválasztása **új** > **felügyeleti** > **erőforráscsoport**.
   
    ![üres erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. Adjon neki egy nevet és egy helyet, és szükség esetén válasszon egy előfizetést. Egy hely tooprovide hello erőforráscsoport kell, mivel hello erőforráscsoport hello erőforrások metaadatokat tárol. Megfelelőségi okokból érdemes lehet, hogy a metaadatokat tároló toospecify. Általában javasoljuk, hogy megadja a helyét, amelyben az erőforrások többségét tárolva lesz. Hello segítségével azonos helyen egyszerűbbé teheti a sablont.
   
    ![csoport értékeket](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Erőforrások a piactér telepítése
Miután létrehozott egy erőforráscsoport, a piactér hello erőforrások tooit telepítheti. hello piactér előre definiált megoldást nyújt a gyakori forgatókönyvek.

1. a központi telepítés toostart válassza ki **új** és hello típus erőforrás toodeploy szeretné. Ezután keresse meg a hello erőforrás hello adott verziójához toodeploy szeretné.
   
    ![erőforrás telepítése](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. Ha nem látja azt szeretné, hogy toodeploy hello adott megoldás, kereshet hello piactér azt.
   
    ![a piactéren](./media/resource-group-template-deploy-portal/search-resource.png)
3. Attól függően, hogy a kiválasztott erőforrás hello típusát központi telepítés előtt vonatkozó tulajdonságok tooset gyűjteménye van. Ezek a lehetőségek nem itt látható, mivel azok erőforrástípus függően változhat. Az összes ki kell választania a célként megadott erőforráscsoport. hello alábbi képen látható, hogyan toocreate egy webalkalmazást, és telepítse azt toohello létrehozott erőforráscsoportot.
   
    ![Erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    Másik megoldásként megadhatja, hogy toocreate erőforráscsoport az erőforrások telepítése során. Válassza ki **hozzon létre új** , és nevezze el hello erőforráscsoportot.
   
    ![Új erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-new-group.png)
4. A telepítés megkezdése. hello telepítési eltarthat néhány percig. Amikor hello telepítés véget ért, megjelenik egy értesítés.
   
    ![értesítési nézet](./media/resource-group-template-deploy-portal/view-notification.png)
5. Az erőforrások való telepítése után hello használatával adhat hozzá további erőforrásokat toohello erőforráscsoport **Hozzáadás** hello erőforráscsoport panel parancs.
   
    ![erőforrás hozzáadása](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Az egyéni sablont az erőforrások telepítése
Ha szeretné, hogy a központi telepítés tooexecute, de nem használható hello sablonok hello piactér, létrehozhat egy egyéni sablont, amely meghatározza a megoldás hello infrastruktúra. toolearn sablonok létrehozásával kapcsolatban lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).

1. toodeploy hello portálon keresztül egyéni sablont válasszon **új**, és kezdeni a keresést a **sablon-üzembehelyezés** amíg hello lehetőségek közül választhat.
   
    ![keresési sablon-üzembehelyezés](./media/resource-group-template-deploy-portal/search-template.png)
2. Válassza ki **sablon-üzembehelyezés** hello elérhető erőforrások.
   
    ![Válassza ki a sablon telepítése](./media/resource-group-template-deploy-portal/select-template.png)
3. Miután elindította hello sablon-üzembehelyezés, nyissa meg a hello üres sablont, amelyet a testreszabása.
   
    ![Sablon létrehozása](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    Hello szerkesztő adja hozzá a kívánt toodeploy hello erőforrások definiáló hello JSON szintaxis. Válassza ki **mentése** végzett. A JSON-szintaxis hello írása útmutatóért lásd: [Resource Manager sablonokhoz](resource-manager-template-walkthrough.md).
   
    ![sablon szerkesztése](./media/resource-group-template-deploy-portal/edit-template.png)
4. Vagy már meglévő sablon választhat hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/). Ezek a sablonok hello Közösség közzé. Számos gyakori helyzetek terjed ki, és egy hasonló toowhat toodeploy próbált-sablont hozzá valaki. Kereshet hello sablonok toofind, amelyet a forgatókönyvének megfelelő.
   
    ![Válassza ki a következő gyorsindítási sablonon](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    Hello kijelölt sablon hello szerkesztőben tekintheti meg.
5. Miután megadta az összes hello más értékek, válassza ki a **létrehozása** toodeploy hello sablont. 
   
    ![sablon üzembe helyezése](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-tooyour-account"></a>Mentett tooyour fiók sablonból erőforrások telepítése
hello portál lehetővé teszi, hogy a sablon tooyour Azure-fiók toosave, és telepítse újra később. További információ ezen sablonok, mentett használata [Ismerkedés az Azure-portálon hello magánsablonok](../marketplace-consumer/mytemplates-getstarted.md).

1. toofind a mentett sablonok kiválasztása **Tallózás** > **sablonok**.
   
    ![Keresse meg a sablonok](./media/resource-group-template-deploy-portal/browse-templates.png)
2. A sablonok tooyour fiók mentett hello listájából válassza ki egy kívánja toowork a hello.
   
    ![mentett sablonok](./media/resource-group-template-deploy-portal/saved-templates.png)
3. Válassza ki **telepítés** tooredeploy ez mentette a sablont.
   
    ![mentett sablon üzembe helyezése](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Következő lépések
* tooview naplókat, lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).
* telepítési hibák tootroubleshoot, lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
* tooretrieve egy sablon, egy központi telepítés vagy az erőforráscsoportot, lásd: [Azure Resource Manager sablon exportálása létező erőforrásokból](resource-manager-export-template.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

