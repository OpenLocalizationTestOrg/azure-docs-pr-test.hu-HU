---
title: "aaaScale kvótái és korlátai, a laborban a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale egy tesztkörnyezetet a Azure DevTest Labs szolgáltatásban"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: tarcher
ms.openlocfilehash: 7fb429c0aabdfbce3a4a5aa6d9259daa2ee270d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a>Skála kvótái és korlátai a DevTest Labs szolgáltatásban
Munka a DevTest Labs szolgáltatásban, Észreveheti, hogy nincsenek bizonyos alapértelmezett korlátok toosome Azure-erőforrások, ami kihathat a hello DevTest Labs szolgáltatás. A hivatkozott tooas azok **kvóták**.

> [!NOTE]
> DevTest Labs szolgáltatás hello nem ugyanazok a kvóták. Bármely kvóták léphetnek fel hello a teljes Azure-előfizetés alapértelmezett korlátai.

Minden Azure-erőforrás is használhat, amíg el nem éri a kvótát. Minden előfizetés külön kvóták és a használati előfizetésenként követhető nyomon.

Például az egyes előfizetések rendelkezik 20 magszámra vonatkozó alapértelmezett kvótát. Igen ha virtuális gépeket a tesztlaborban négy magok hoz létre, majd csak létrehozhat öt virtuális gépek. 

[Az Azure előfizetés és szolgáltatásra vonatkozó korlátozások](https://docs.microsoft.com/azure/azure-subscription-service-limits) egy Azure-erőforrások leggyakoribb kvótái hello sorolja fel. egy tesztkörnyezetben leggyakrabban használt erőforrások hello, és amely léphetnek fel a kvótákat, az VM magok, nyilvános IP-címek, hálózati illesztő, kezelt lemezek, RBAC szerepkör-hozzárendelés, és ExpressRoute-Kapcsolatcsoportok.

## <a name="view-your-usage-and-quotas"></a>A használati és a kvóták megtekintése
Ezek a lépések bemutatják, hogyan tooview hello az adott Azure-erőforrások és toosee előfizetésében aktuális kvóták minden kvóta hány százalékát használja.

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Válassza ki **több szolgáltatások**, majd válassza ki **számlázási** hello listából.
1. Hello számlázási panelen válassza ki egy előfizetést.
4. Válassza ki **használati + kvóták**.

   ![Használati és kvóták gomb](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   Használati + kvóták hello panel listája jelenik különböző erőforrások hello kvóta erőforrásonként használt előfizetés és hello százalékosan érhető el.

   ![Kvóták és használat](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a>A kért további erőforrást az előfizetésében
Ha eléri a kvóta kap, hello alapértelmezett korlát előfizetés az erőforrások növelhető mentése tooa maximális száma, a [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](https://docs.microsoft.com/azure/azure-subscription-service-limits).

Ezek a lépések bemutatják, hogyan toorequest a kvóta növeléséhez keresztül hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Válassza ki **több szolgáltatások**, jelölje be **számlázási**, majd válassza ki **használati + kvóták**.
1. A használati hello és kvóták panelen, jelölje ki a hello **kérelem növelése** gombra.

   ![Kérésgombhoz növelése](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. toocomplete és hello kérés elküldéséhez szükséges hello adatok minden három lapon a hello **új támogatja a kérelem** űrlap.

   ![Növelje a kérésűrlapra](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

[Understanding Azure korlátozásai és növekszik](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) lépjen kapcsolatba az Azure támogatási toorequest további információt nyújt a kvóta növelését.



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Következő lépések
* Fedezze fel hello [Office DevTest Labs Azure Resource Manager gyorsindítási sablonok](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
