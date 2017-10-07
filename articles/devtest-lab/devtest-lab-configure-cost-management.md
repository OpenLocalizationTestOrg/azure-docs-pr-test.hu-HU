---
title: "aaaView hello havi becsült labor költségtrend a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "További információk a hello Azure DevTest Labs havi becsült költség trend diagram."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 47c7dd4cf91b76de74b502c50f05e79cd501ee35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-hello-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Nézet hello havi becsült labor költségtrend a Azure DevTest Labs szolgáltatásban
hello költség kezelése szolgáltatást a DevTest Labs segítségével nyomon követheti a labor hello költségét. Ez a cikk bemutatja, hogyan toouse hello **becsült havi Költségtrend** diagram tooview hello az aktuális naptári hónapra becsült költség-date és hello hello várható befejezési hónap költsége aktuális naptári hónapra. Ebből a cikkből megismerheti, hogyan tooview hello havi becsült költség trend diagram hello Azure-portálon.

## <a name="viewing-hello-monthly-estimated-cost-trend-chart"></a>Hello havi becsült Költségtrend diagram megtekintése
tooview hello havi becsült Költségtrend diagram, kövesse az alábbi lépéseket: 

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
3. Labs hello listában jelölje ki hello kívánt labor.   
4. Hello labor paneljén válassza **beállítások költség**.
5. A hello labor **beállítások költség** panelen válassza **Lab költségtrend**.
6. hello következő képernyőfelvételen látható költség diagram példát. 
   
    ![A diagram költség](./media/devtest-lab-configure-cost-management/graph.png)

Hello **becsült költség** érték hello van az aktuális naptári hónapra becsült költség dátumig. Hello **becsült költség** hello teljes hello becsült költségét aktuális naptári hónapra számítja hello labor költség hello az előző öt nap.

hello költség összegeket toohello következő egész számra felfelé kerekíti. Példa: 

* 5.01 too6 kerekít. 
* 5.50 too6 kerekít.
* 5.99 too6 kerekít.

Azt jelzi, hello diagram felett, hello diagramon látható hello költségek során a rendszer *becsült* használatával költségek [használatalapú fizetés](https://azure.microsoft.com/offers/ms-azr-0003p/) díjakat kínál.
Emellett hello az alábbiakban *nem* költségszámítás hello szerepel:

* CSP és Dreamspark-előfizetés használata jelenleg nem támogatott Azure DevTest Labs használ hello [Azure számlázási API-k](../billing/billing-usage-rate-card-overview.md) toocalculate hello labor költség, amely nem támogatja a kriptográfiai Szolgáltató vagy a Dreamspark-előfizetések.
* Az ajánlatok díjaival számolva. A Microsoft jelenleg nem képes toouse a (lásd az előfizetéshez tartozó), hogy Ön rendelkezik egyeztet Microsoft vagy a Microsoft partnerei ajánlatok díjaival számolva. Használatalapú fizetés díjszabás használjuk.
* A adók
* A kedvezményeket
* A Számlázás pénzneme. Jelenleg hello labor költség csak USD pénznemben jelenik meg.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések
* [Két további dolgot tookeep a költség, a nyomon követése a DevTest Labs szolgáltatásban](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Miért költség küszöbértékek?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Következő lépések
Az alábbiakban néhány dolgot tootry mellett:

* [Labor házirendeket definiálhat az](devtest-lab-set-lab-policy.md) -megtudhatja, hogyan tooset hello használt különféle szabályzatok hogyan használhatók a labor és a virtuális gépek toogovern. 
* [Hozzon létre egyéni lemezkép](devtest-lab-create-template.md) – a virtuális gépek létrehozásakor megad egy talál, amely lehet, vagy egy egyéni lemezképet, vagy egy Piactéri lemezképhez. Ez a cikk bemutatja, hogyan toocreate egyéni kép VHD-fájl.
* [Konfigurálja a piactéren elérhető rendszerkép](devtest-lab-configure-marketplace-images.md) - DevTest Labs támogatja az Azure piactéren elérhető rendszerkép alapján virtuális gépek létrehozását. Ez a cikk bemutatja, hogyan toospecify, amely, ha bármely, az Azure piactéren elérhető rendszerkép lehet egy, amikor a virtuális gépek létrehozásakor használt.
* [Hozzon létre egy virtuális Gépet egy tesztkörnyezetben](devtest-lab-add-vm-with-artifacts.md) -bemutatja, hogyan toocreate alapjául szolgáló lemezképhez a virtuális gép (vagy egyéni vagy piactér), és hogyan toowork együtt a virtuális gépen.

