---
title: "az Azure portál toomanage aaaUse Azure-erőforrások |} Microsoft Docs"
description: "Azure-portál és az Azure Resource Manager toomanage az erőforrások használatára. Bemutatja, hogyan toowork irányítópultok toomonitor erőforrásokkal."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 0c89a197a31c5b6dd03ba457cb4d1fdf9f6d00f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-resources-through-portal"></a>Azure-portálon keresztül erőforrások kezelése
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Portál](resource-group-portal.md) 
> * [REST API](resource-manager-rest-api.md)
> 
> 

Ez a témakör bemutatja, hogyan toouse hello [Azure-portálon](https://portal.azure.com) rendelkező [Azure Resource Manager](resource-group-overview.md) toomanage az Azure-erőforrások. toolearn hello portálon keresztül erőforrások telepítésével kapcsolatban lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).

Nem minden szolgáltatás jelenleg hello portálon vagy az erőforrás-kezelő. Ezek a szolgáltatások toouse hello kell [klasszikus portál](https://manage.windowsazure.com). Minden szolgáltatás hello állapotát, lásd: [az Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="manage-resource-groups"></a>Erőforrás-csoportok kezelése

Egy erőforráscsoport egy olyan tároló, amely egy Azure megoldás kapcsolódó erőforrásokat tárol. hello erőforráscsoport összes hello erőforrások hello megoldás, és csak azokat az erőforrásokat, amelyet az egy csoportként toomanage tartalmazhatnak. Úgy dönt, hogy hogyan tooallocate erőforrások tooresource csoportok alapján hasznossá hello a legtöbb, a szervezet számára legjobb. Általában a megosztás hello erőforrásokat adjon hozzá azonos életciklus toohello azonos erőforráscsoport, így könnyen központi telepítése, frissítése és csoportként törölje őket. 

hello erőforráscsoport hello erőforrások metaadatokat tárol. Ezért amikor hello erőforrásnak a helyét adja meg, meg, hogy a metaadatokat tároló. Megfelelőségi okokból, szükség lehet a tooensure, amely az adott tárolja az adatokat.

1. toosee az előfizetésében szereplő összes hello erőforráscsoport kiválasztása **erőforráscsoportok**.
   
    ![Keresse meg az erőforráscsoport-sablonok](./media/resource-group-portal/browse-groups.png)
2. egy üres erőforráscsoporthoz toocreate kiválasztása **Hozzáadás**.
   
    ![csoport hozzáadása](./media/resource-group-portal/add-resource-group.png)
3. Adjon meg egy új erőforráscsoportot hello helyét és típusát. Kattintson a **Létrehozás** gombra.
   
    ![Erőforráscsoport létrehozása](./media/resource-group-portal/create-empty-group.png)
4. Előfordulhat, hogy tooselect **frissítése** toosee hello nemrég létrehozott erőforráscsoportot.
   
    ![erőforrás-csoport frissítése](./media/resource-group-portal/refresh-resource-groups.png)
5. Az erőforráscsoportok megjelenő toocustomize hello információ kiválasztása **oszlopok**.
   
    ![oszlop testreszabása](./media/resource-group-portal/select-columns.png)
6. Hello oszlopok tooadd, majd válassza ki és **frissítés**.
   
    ![oszlopok hozzáadása](./media/resource-group-portal/add-columns.png)
7. toolearn erőforrások tooyour új erőforráscsoportot, telepítésével kapcsolatban lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).
8. A gyors hozzáférés tooa erőforráscsoport PIN-kód hello panel tooyour irányítópult.
   
    ![PIN-kód erőforráscsoport](./media/resource-group-portal/pin-group.png)
9. hello irányítópult hello erőforráscsoport és az erőforrások jeleníti meg. Kiválaszthatja a hello erőforráscsoportok vagy az erőforrások toonavigate toohello elemek bármelyikét.
   
    ![PIN-kód erőforráscsoport](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Címke erőforrások
Címkék tooresource csoportok alkalmazhat, és erőforrások toologically rendezze az eszközök. A címkék használatáról információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](resource-group-using-tags.md).

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Erőforrások megfigyelése
Amikor kiválaszt egy erőforrást, hello erőforráspanelen alapértelmezett diagramjait és az adott erőforrástípus figyelési táblák mutatja be.

1. Válassza ki az erőforrás és a hirdetmény hello **figyelés** szakasz. Ez magában foglalja, amelyek megfelelő toohello erőforrástípus diagramjait. hello következő kép bemutatja hello alapértelmezett figyelési adatok tárfiókok esetén.
   
    ![figyelési megjelenítése](./media/resource-group-portal/show-monitoring.png)
2. Hello panel tooyour irányítópult szakasz rögzíthető hello három ponttal (…) hello szakasz fölé kiválasztásával. Hello mérete hello szakasz hello panelen testreszabásával, vagy távolítsa el teljesen. hello következő kép bemutatja, hogyan toopin, testre szabhatja, vagy távolítsa el a hello CPU és memória szakasz.
   
    ![PIN-kód szakasz](./media/resource-group-portal/pin-cpu-section.png)
3. Miután hello szakasz toohello irányítópult, látni fogja az összefoglaló hello hello irányítópulton. És lehetőséget azonnal viszi toomore hello adatok részleteit.
   
    ![Irányítópult nézet](./media/resource-group-portal/view-startboard.png)
4. toocompletely testreszabása hello adatok hello portálon keresztül figyelheti, keresse meg a tooyour alapértelmezett irányítópultot, és válassza ki **új irányítópult**.
   
    ![irányítópult](./media/resource-group-portal/dashboard.png)
5. Nevezze el az új irányítópult és csempék húzza hello irányítópult. hello csempék különböző beállítások szerint vannak szűrve.
   
    ![irányítópult](./media/resource-group-portal/create-dashboard.png)
   
     toolearn irányítópultok, használatával kapcsolatban lásd: [létrehozása és az Azure-portálon hello irányítópultok megosztása](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-resources"></a>Erőforrások kezelése
Erőforrás hello panelen látható hello beállítások kezeléséhez hello erőforrás. hello portál megjeleníti a felügyeleti beállításokat az adott erőforráshoz. Hello parancsok hello erőforráspanelen és hello bal oldalán hello tetején megjelenik.

![Erőforrások kezelése](./media/resource-group-portal/manage-resources.png)

Ezek a beállítások műveletek, például a indítása és leállítása a virtuális gép vagy hello virtuális gép tulajdonságainak hello újrakonfigurálása végezheti el.

## <a name="move-resources"></a>Erőforrások áthelyezése
Ha toomove erőforrások tooanother erőforráscsoport vagy egy másik előfizetés van szüksége, tekintse meg [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](resource-group-move-resources.md).

## <a name="lock-resources"></a>Erőforrások zárolása
Zárolhatja előfizetést, erőforráscsoporthoz vagy erőforrás tooprevent más felhasználók véletlen törlése vagy a kritikus erőforrásokat módosítása a szervezetében. További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](resource-group-lock-resources.md).

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Az előfizetés és a költségek megtekintése
Az erőforrások előfizetését és hello összegzett költségeinek kapcsolatos információk is megtekinthetők. Válassza ki **előfizetések** és azt szeretné, hogy toosee hello előfizetés. Csak előfordulhat, hogy egy előfizetés tooselect.

![előfizetést](./media/resource-group-portal/select-subscription.png)

Belül hello előfizetés panelen megjelenik egy írási sebesség.

![Írás gyakorisága](./media/resource-group-portal/burn-rate.png)

És erőforrástípusok szerint költségeket.

![Erőforrás költség](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Sablon exportálása
Miután beállította az erőforráscsoport, érdemes lehet az tooview hello Resource Manager-sablon hello erőforráscsoport. Exportáló hello sablon két előnyökkel jár:

1. Könnyen automatizálható a későbbi központi telepítés alkalmával hello megoldás, mert hello a sablon tartalmazza az összes hello teljes infrastruktúrát.
2. Válhat ismeri a sablon szintaxisát megtekintésével hello JavaScript Object Notation (JSON), amely a megoldás jelöli.

Lépésenkénti útmutatásért lásd: [Azure Resource Manager sablon exportálása létező erőforrásokból](resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Erőforráscsoport és erőforrások törlése
Erőforráscsoport törlésekor az abban szereplő összes hello erőforrást. Törölheti az egyes erőforrások erőforráscsoporton belül is. Erőforráscsoport törlésekor, mert lehetséges, hogy nem erőforrásokat, amelyek csatolt tooit egyéb erőforráscsoportok kívánt tooexercise járjon el. Erőforrás-kezelő nem törli a társított erőforrásokat, de azok nem működnek megfelelően várt hello erőforrások nélkül.

![Csoport törlése](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Következő lépések
* tooview tevékenységi naplóit, lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).
* a központi telepítés tooview részleteit lásd [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
* hello portálon keresztül toodeploy erőforrások lásd [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).
* toomanage hozzáférés tooresources, lásd: [szerepkör hozzárendelések toomanage tooyour Azure-előfizetés erőforrások eléréséhez használjon](../active-directory/role-based-access-control-configure.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

