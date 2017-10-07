---
title: "az erőforrás-házirendek aaaAzure portál |} Microsoft Docs"
description: "Ismerteti, hogyan toouse az Azure portál toocreate és erőforrás-kezelő házirendek kezelése. Szabályzatok alkalmazhatók a hello előfizetés vagy az erőforrás-csoportok."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: ce6413386317e532b63761a24458b85c996af4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-tooassign-and-manage-resource-policies"></a>Az Azure portál tooassign használata és erőforrás-házirendek kezelése
hello Azure-portálon lehetővé teszi, hogy tooassign házirendek tooresource erőforráscsoportok és előfizetések. hello felhasználói felület segítségével könnyen tooselect hello házirend, hogy szeretné, hogy tooassign, és adja meg, hogy a házirend toocustomize hello házirend-beállítások paraméterértékeket. 

egy házirend hello portálon keresztül tooassign, hello házirend-definíció már léteznie kell az előfizetéshez. Az előfizetés rendelkezik, amely készen áll az Ön tooassign tooresource csoportok vagy előfizetések számos beépített házirend-definíciók. Láthatja, hogy a beépített házirendek és hello portál tooassign házirendek használatakor definiált egyéni házirendekben. Egy bevezető toopolicies és hogyan toodefine testreszabott házirendet: [erőforrás házirendek – áttekintés](resource-manager-policy.md).

Összes gyermek-erőforrás által örökölt házirendek. Ezért, ha a házirend alkalmazott tooa erőforráscsoportban, ezt a megfelelő tooall hello erőforrások az erőforráscsoport. Ebben a cikkben hello kifejezés **hatókör** toohello erőforráscsoportba vagy előfizetésbe, hozzárendelt hello házirend hivatkozik. 

Házirendek létrehozása, és az erőforrások (PUT és műveletek javítás) frissítésekor kiértékelése.

## <a name="assign-a-policy"></a>A házirend kijelölése

1. egy olyan erőforráscsoport házirend tooeither tooassign vagy előfizetés, válassza ki, hogy erőforráscsoportba vagy előfizetésbe. Hello beállításaiban, válassza ki a **házirendek**.

   ![Jelölje ki a házirendek](./media/resource-manager-policy-portal/select-policies.png)

2. toocreate egy házirend-hozzárendelést a hatókör kiválasztása **adja hozzá a hozzárendelés**.

   ![hozzárendelés hozzáadása](./media/resource-manager-policy-portal/add-assignment.png)

3. Válassza ki a kívánt tooassign hello házirendet. Akkor is, ha még nem hozzá egyetlen házirend-definíciók tooyour előfizetéshez, lásd: hello beépített házirendek kiosztására használható. A beépített házirendek számos gyakori helyzetek foglalkozik.

   ![Válassza ki a definíció](./media/resource-manager-policy-portal/select-definition.png)

4. A házirend kijelölése után megjelenik a hello házirend leírását, és a házirendhez tartozó paramétereket. Például hello következő kép bemutatja hello **engedélyezett helyek** paraméter, amely korlátozza a hello elérhető helyről hello házirend szükséges.

   ![paraméterek megjelenítése](./media/resource-manager-policy-portal/show-parameters.png)

5. Hello felhasználói felületen válassza ki a hello értékek toospecify hello házirend paraméterek (például a központi telepítéshez használható hello helyek).

   ![Válassza ki a paramétert értékek](./media/resource-manager-policy-portal/select-parameters.png)

6. Adja meg hello más paramétereket. hello hatókör automatikusan hello házirend-hozzárendelés indításakor kijelölt hello panel alapján. Kattintson az **OK** gombra, amikor végzett.

   ![Paraméterek megadása](./media/resource-manager-policy-portal/define-parameters.png)

  Hello házirend hozzárendelt toohello megadott hatókörben.

## <a name="view-policy-assignments"></a>Házirend-hozzárendelések megtekintése

Házirend hozzárendelése után látható az adott hatókörnél házirendek hello listája. Hello **részletek** lapon hello házirend-hozzárendelés összegzését jeleníti meg.

![Részletek megjelenítése](./media/resource-manager-policy-portal/show-details.png)

Hello **hozzárendelési szabály** lapon látható hello JSON hello házirend-definíció.

![hozzárendelési szabály megjelenítése](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a>Egy meglévő házirend-hozzárendelés módosítása

toochange egy házirendet, válassza ki **hozzárendelés szerkesztése** vagy **törlése**

![szerkesztheti és törölheti a hozzárendelés](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a>Rendelje hozzá az egyéni házirendek

Egyéni házirendeket az előfizetés adta meg, ha ezek a házirendek legyenek hello portálon keresztül. Ezek a házirendek vannak végrehajtásával kerüli meg a **[egyéni]**

![Egyéni házirendek](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a>Következő lépések
* toolearn kapcsolatos hello JSON szintaxisának meghatározó házirendek, lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).
* hello házirend séma közzé van téve a [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

