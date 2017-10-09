---
title: az Azure DevTest Labs labor aaaCreate |} Microsoft Docs
description: "Labor létrehozása virtuális gépekhez az Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/30/2017
ms.author: tarcher
ms.openlocfilehash: 2ec5498f10e0cc06a196a71e319e340937abb1a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Labor létrehozása az Azure DevTest Labs szolgáltatásban
A Azure DevTest Labs szolgáltatásban labor hello infrastruktúra, amely magában foglalja az erőforrások csoportja, például, amely lehetővé teszi nagyobb virtuális gépek (VM), ezeket az erőforrásokat kezelése korlátozásai és a kvóták megadásával. Ez a cikk bemutatja, hogyan hello hello Azure-portál használatával labor létrehozásának folyamatán.

## <a name="prerequisites"></a>Előfeltételek
toocreate egy tesztkörnyezetet, lesz szüksége:

* Azure-előfizetés. Azure megvásárlási lehetőségeinek toolearn lásd [hogyan toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) vagy [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/). Hello előfizetés toocreate hello labor hello tulajdonosának kell lennie.

## <a name="steps-toocreate-a-lab-in-azure-devtest-labs"></a>Lépéseket toocreate egy tesztkörnyezetet a Azure DevTest Labs szolgáltatásban
hello lépések bemutatják, hogyan toouse hello Azure portál toocreate egy tesztkörnyezetet a Azure DevTest Labs szolgáltatásban. 

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Főmenüben hello hello bal oldalon válassza ki a **több szolgáltatások** (alján hello hello lista).

    ![További szolgáltatások menüpont](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. Az elérhető szolgáltatások listájából hello **DevTest Labs**.
1. A hello **DevTest Labs** panelen válassza **Hozzáadás**.
   
    ![Labor hozzáadása](./media/devtest-lab-create-lab/add-lab-button.png)

1. A hello **létrehozása a DevTest Lab** panel:
   
    1. Adjon meg egy **tesztkörnyezet nevének** hello új labor számára.
    2. Jelölje be hello **előfizetés** hello amelyiknek tooassociate.
    3. Válassza ki a **hely** mely toostore hello tesztkörnyezetben.
    4. Válassza ki **automatikus leállítási** toospecify Ha tooenable - és paramétereit hello - hello automatikus leállítása az összes hello labor virtuális gépeken. hello automatikus leállítási szolgáltatása lehetővé teszi az főként költségkímélő során megadhatja, ha szeretné a virtuális gép hello tooautomatically le kell állítani. Hello a cikkben leírt hello lépéseket követve hello labor létrehozása után módosíthatja az automatikus leállítási beállítások [egy Azure DevTest Labs szolgáltatásban található, amikor az összes házirend kezeléséhez](./devtest-lab-set-lab-policy.md#set-auto-shutdown).
    5. Válassza ki **PIN-kód tooDashboard** Ha azt szeretné, hogy hello labor tooappear hello portál irányítópultján a parancsikont.
    6. Válassza ki **automatizálási lehetőségek** tooget Azure Resource Manager-sablonok a konfigurációs automatizálásához. 
    7. Kattintson a **Létrehozás** gombra. Kiválasztása után **létrehozása**, hello **DevTest Labs** panelt jeleníti meg. Hello labor létrehozási folyamata hello állapotának végezhet figyelést, ha hello figyeli **értesítések** területen. Ezt követően frissítse a hello lap toosee az újonnan létrehozott hello labor labs hello listájában.  
    
    ![Laborpanel létrehozása](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Következő lépések
A labor létrehozása után az alábbiakban néhány következő lépések tooconsider:

* [Biztonságos hozzáférés tooa labor](devtest-lab-add-devtest-user.md).
* [Laborházirendek megadása](devtest-lab-set-lab-policy.md).
* [Laborsablon létrehozása](devtest-lab-create-template.md).
* [Egyéni összetevők létrehozása a virtuális géphez](devtest-lab-artifact-author.md).
* [Adja hozzá a virtuális gép és az összetevők tooa labor](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).

