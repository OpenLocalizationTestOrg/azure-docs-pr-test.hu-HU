---
title: "aaaGet a Magánsablonok használatába |} Microsoft Docs"
description: "Hozzáadása, kezelése és megosztása magánsablonok hello Azure-portálon, a hello Azure CLI vagy a PowerShell használatával."
services: marketplace-customer
documentationcenter: 
author: VybavaRamadoss
manager: asimm
editor: 
tags: marketplace, azure-resource-manager
keywords: 
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar
ms.openlocfilehash: 1fe2c6422f62a98f7ae9ba5c61b9639d993f0bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-private-templates-on-hello-azure-portal"></a>Ismerkedés az Azure portál hello Magánsablonok
Egy [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) sablon olyan deklaratív sablonok használt toodefine a központi telepítés. Hello erőforrások toodeploy megoldás határozza meg, és adja meg a paramétereket és változókat, amelyek lehetővé teszik a különböző környezetekhez tooinput értékeket. hello sablon JSON és áll kifejezés, amelynek segítségével az üzemelő példány tooconstruct értékeit.

Hello új használható **sablonok** hello képesség [Azure Portal](https://portal.azure.com) együtt hello **Microsoft.Gallery** erőforrás-szolgáltató a hello [ Az Azure piactér](https://azure.microsoft.com/marketplace/) tooenable felhasználók toocreate, kezelését, és saját könyvtárukból származó magánsablonok telepítését.

Ez a dokumentum útmutatást nyújt a hozzáadása, kezelése és oszthat **sablon** használatával hello Azure portálon.

## <a name="guidance"></a>Útmutatás
hello következő javaslatok segítségével teljes körű kihasználása **sablonok** a megoldásaival való munka során:

* A **sablonok** beágyazott erőforrások, amelyek egy Resource Manager-sablont, illetve további metaadatokat tartalmaznak. Az hasonlóan viselkednek, mint hello piactér tooan elemére. hello legfontosabb különbség, hogy a személyes elemet, megakadályozását toohello piactér elemei.
* Hello **sablonok** könyvtár hasznos segítséget nyújt a felhasználóknak, akik toocustomize a telepítések.
* A **sablonok** egyszerű Azure-beli tárházat biztosítanak a felhasználóknak.
* Kezdje egy meglévő Resource Manager-sablonnal. Keresse meg a kívánt sablont a [GitHubon](https://github.com/Azure/azure-quickstart-templates), vagy [exportálja a sablont](../azure-resource-manager/resource-manager-export-template.md) egy meglévő erőforráscsoportból.
* **Sablonok** kapcsolt toohello-felhasználó, aki közzéteszi őket. hello közzétevő neve látható tooeveryone, aki rendelkezik olvasási jogosultsággal tooit.
* A **sablonok** a Resource Managerhez tartozó erőforrások, amelyeket közzététel után nem lehet átnevezni.

## <a name="add-a-template-resource"></a>Sablonerőforrás hozzáadása
Két módon toocreate van egy **sablon** erőforrás hello Azure-portálon.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>1. módszer: Új sablonerőforrás létrehozása már futó erőforráscsoportból
1. Keresse meg a tooan meglévő erőforráscsoportot hello Azure portálon. A **Beállítások** menüben válassza a **Sablon exportálása** lehetőséget.
2. Ha hello Resource Manager sablon exportálása, a hello **sablon mentése** gomb toosave azt toohello **sablonok** tárház. A Sablon exportálása funkcióról részletes leírást [itt](../azure-resource-manager/resource-manager-export-template.md) talál.
   <br /><br />
   ![Erőforráscsoport exportálása](media/rg-export-portal1.PNG)  <br />
3. Jelölje be hello **tooTemplate mentése** parancsgombra.
   <br /><br />
4. Adja meg a következő információ hello:
   
   * Név – hello sablonobjektum neve (Megjegyzés: az Azure Resource Manager-alapú neve. Így minden vonatkozó elnevezési korlátozás érvényes rá, és létrehozását követően nem módosítható).
   * Leírás – hello sablon rövid ismertetése.
     
     ![Sablon mentése](media/save-template-portal1.PNG)  <br />
5. Kattintson a **Save** (Mentés) gombra.
   
   > [!NOTE]
   > hello sablon exportálása panelen értesítések jelennek meg hello exportált Resource Manager sablon hibákkal rendelkezik, de továbbra is meg fogja tudni toosave a Resource Manager sablon toohello sablonok. Győződjön meg arról, hogy ellenőrizze, és ki a Resource Manager sablon kapcsolatos problémák megoldása ismételt üzembe helyezéssel hello Resource Manager sablon exportálása előtt.
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a>2. módszer: Új sablonerőforrás hozzáadása tallózással
Azt is megteheti egy új **sablon** használatával hello + Hozzáadás parancsgomb az alapoktól **Tallózás > sablonok**. Szüksége lesz tooprovide nevét, leírását és a Resource Manager-sablon JSON hello.

![Sablon hozzáadása](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> A Microsoft.Gallery egy bérlőalapú Azure-erőforrás-szolgáltató. hello sablonerőforrás az toohello kapcsolt felhasználó, aki létrehozta. A feltételekhez tooany előfizetéshez nincs. Előfizetés csak a sablon telepítésekor kiválasztott toobe kell.
> 
> 

## <a name="view-template-resources"></a>Sablonerőforrás megtekintése
Minden **sablonok** elérhető tooyou látható a következő **Tallózás > sablonok**. Itt az Ön által létrehozott **sablonok**, valamint az Önnel különböző szintű engedélyekkel megosztott sablonok egyaránt megjelennek. További részletek a hello [hozzáférés-vezérlés](#access-control-for-a-tenant-resource-provider) az alábbi szakasz.

![Sablon megtekintése](media/view-template-portal1.PNG)  <br />

Hello részleteit megtekintheti a **sablon** hello lista elemére kattintva.

![Sablon megtekintése](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Sablonerőforrás módosítása
Hello szerkesztési folyamatának is kezdeményezhető a **sablon** hello Tallózás listában hello elemre jobb gombbal kattint rá vagy hello Szerkesztés parancsgombot kiválasztásával.

![Sablon szerkesztése](media/edit-template-portal1a.PNG)  <br />

Szerkesztheti a hello leírást vagy a Resource Manager-sablon szövegét. Hello neve nem szerkeszthető, mert ez az erőforrás-kezelő erőforrás neve. Hello Resource Manager-sablon JSON szerkesztésekor Microsoft ellenőrzi, hogy-e érvényes JSON tooensure. Válasszon **OK** , majd **mentése** toosave a módosított sablon.

![Sablon szerkesztése](media/edit-template-portal2a.PNG)  <br />

Egyszer hello **sablon** mentett megerősítési értesítés jelenik meg.

![Sablon szerkesztése](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Sablonerőforrás üzembe helyezése
Bármely **sablon** üzembe helyezhető, amelyhez **olvasási** engedélyekkel rendelkezik. hello üzembe helyezési folyamat elindítja hello standard Azure-sablon üzembe helyezési paneljét. Töltse ki a hello Resource Manager sablon paraméterek tooproceed hello telepítési hello értékeit.

![Sablon üzembe helyezése](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Sablonerőforrás megosztása
A **sablonerőforrást** megoszthatja kollégáival. Megosztás hasonlóan működik, hasonlóképpen túl[szerepkör-hozzárendelés az Azure-erőforrásoknál](../active-directory/role-based-access-control-configure.md). Hello **sablon** tulajdonos tooother kommunikáló felhasználók számára is biztosít engedélyeket a sablonerőforrás. hello személyek vagy csoportok személyek hello megosztott **sablon** pedig képes toosee hello Resource Manager-sablon és a hozzá tartozó katalógusbeli tulajdonságok is.

### <a name="access-control-for-hello-microsoftgallery-resources"></a>Hello Microsoft.Gallery erőforrások hozzáférés-vezérlés
| Szerepkör | Engedélyek |
| --- | --- |
| Tulajdonos |Lehetővé teszi, hogy a megosztás beleértve hello sablonerőforrás teljes hozzáférés |
| Olvasó |Lehetővé teszi az olvasási és Execute(Deploy) hello sablonerőforrás |
| Közreműködő |Lehetővé teszi az hello sablonerőforrás szerkesztését és törlését. Felhasználó nem hello sablon megosztása másokkal |

Válassza ki **megosztás** hello Tallózás elem jobb gombbal kattintva vagy az adott elem hello megtekintési paneljén. Ekkor elindul a Megosztás folyamat.

![Sablon megosztása](media/share-template-portal1a.png)  <br />

 Válasszon egy szerepkör és egy felhasználó vagy csoport tooprovide hozzáférés tooa adott **sablon**. hello szerepkörök választhatók: tulajdonos, olvasó és közreműködő. További részletek a hello [hozzáférés-vezérlés](#access-control-for-a-tenant-resource-provider) fenti szakaszban.

![Sablon megosztása](media/share-template-portal2b.png)  <br />

![Sablon megosztása](media/share-template-portal3b.png)  <br />

Kattintson a **Kijelölés**, majd az **OK** gombra. Most már megtekintheti hello felhasználók vagy csoportok hozzáadott toohello erőforrás.

![Sablon megosztása](media/share-template-portal4b.png)  <br />

> [!NOTE]
> A sablon csak meg lehet osztani a felhasználók és csoportok hello azonos Azure Active Directory-bérlő. Ha meg megosztani a sablont, amely nincs az Ön bérelt szolgáltatásának e-mail címmel, meghívót küldi hello felhasználói toojoin hello bérlői vendégként kéri.
> 
> 

## <a name="next-steps"></a>Következő lépések
* toolearn Resource Manager-sablonok létrehozásával kapcsolatban lásd: [sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md)
* a Resource Manager-sablonokban használható toounderstand hello függvények lásd [sablonfüggvények](../azure-resource-manager/resource-group-template-functions.md)
* A sablonok kialakításával kapcsolatos útmutatásért lásd: [Best practices for designing Azure Resource Manager templates](../azure-resource-manager/best-practices-resource-manager-design-templates.md) (Azure Resource Manager-sablonok tervezésének ajánlott eljárásai)

