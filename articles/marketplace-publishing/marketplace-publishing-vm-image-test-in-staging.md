---
title: "a virtuális gép hello Piactéri ajánlat aaaTest |} Microsoft Docs"
description: "Ismerje meg, hogyan tootest a virtuális gép rendszerképet az Azure piactér hello."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: ab166d2c3c536810a3a8f48330f0482b9b4e58d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-your-vm-offer-for-hello-azure-marketplace-in-staging"></a>A Virtuálisgép-ajánlat a hello Azure piactér tesztelése az átmeneti
A Termékváltozat "védőfal", ahol tesztelése és ellenőrzése a működését toohello piactér üzembe helyezése előtt privát telepítése azt jelenti, hogy átmeneti. hello SKU tooa felhasználói, akik már telepítették volna átmeneti jelenik meg. A Virtuálisgép-lemezkép leküldött hitelesített toobe toostaging kell lennie.

## <a name="step-1-push-your-offer-toostaging"></a>1. lépés: Az ajánlat toostaging leküldéses
1. A hello **közzététel** lapra, majd **tooStaging leküldéses**.
   
    ![rajz](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Ha hello közzétételi portál értesíti az esetleges hibákat, javítsa ki őket.
3. A hello **ki férhet hozzá az előkészített ajánlatot?** párbeszédpanelen adja meg, hogy szüksége lesz toopreview ajánlatát hello Azure-előfizetések listája hello [Azure betekintő portál](https://portal.azure.com).
   
   > [!NOTE]
   > Virtuális gépek esetén és a megoldás sablonok adjon **nem** típusú kriptográfiai Szolgáltató, DreamSpark- vagy Azure in Open licencprogram engedélyezett előfizetések.
   > 
   > 

    > Virtuális gépek, a hello gombra kattintva esetén **LEKÜLDÉSES tooSTAGING**, a lépéseket követve hello mögött hello helyszín hajtja végre. Képes tooview hello folyamat minden egyes lépést hello közzététel lap hello fogja portal közzététele. Ellenőrizze ezen a lapon rendszeres időközönként (amíg hello állapotánál ELŐKÉSZÍTETT) javítása a átfogóan igénylő hiba információkat.

    > - Először az átmeneti kéréseket, amelyekre toohello hitelesítő csoport hello vhd ellenőrzése. Azonban ha a kérés rendelkezik kapott módosítása csak marketing, majd hello hitelesítő a lépés kimarad.
    > - Hello hitelesítő végrehajtása után a replikáció hello ajánlat indításának összes hello Azure adatközpontjaiban. Általában 24-48hours hello replikációs toocomplete az szükséges, de tooa hét attól függően, hogy hello virtuális merevlemez méretére hello is tarthat. Ha a kérés rendelkezik kapott módosítása csak marketing, majd hello replikációs azonban gyorsabb.
    > - Ha hello replikáció befejeződött, majd hello ajánlat rendelkezésre áll a hello [Azure-portálon](http:/portal.azure.com). Adott idő hello: állapota lesz ELŐKÉSZÍTETT hello a portál közzététele. Egy előkészített ajánlat is elérhetővé válik a hello [Azure-portálon](http:/portal.azure.com) csak a mely hello az ajánlat elő van készítve hello előfizetéshez társított hello e-mail azonosítót.

1. Jelentkezzen be toohello [Azure betekintő portál](https://portal.azure.com) hello segítségével az Azure-előfizetések hello előző lépésben felsorolt.
2. Keresse meg az ajánlatot, és ellenőrizze a VM-lemezkép pontok:
   
   * Győződjön meg arról, hogy tartalom marketing jeleníti meg helyesen hello piactéren.
   * Végpontok közötti hello Virtuálisgép-lemezkép központi telepítése.
     
      ![img-térkép-portál](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> Az ajánlatot marad, amíg tájékoztatja a Microsoftot hello közzétételi portálon keresztül átmeneti [**közzététel** lapon > hello gombra kattintva **"Jóváhagyási tooPush tooProduction lekérő"**] készen toopush abban tooproduction. Ez az az ideális idő toohave keresztül minden felsorolt fog ajánlatát előkészítésekor ellenőrizze a csoport összes tagja.
> 
> átmeneti platform hello tesztelési hello ajánlat egy előnézeti módban készült hello közzétevő. Kifejezetten nem ajánlott a platofrm kereskedelmi célra használja.
> 
> 

## <a name="next-steps"></a>Következő lépések
Az ajánlatot "előkészített", és amelyeket tesztelt, funkciókat, és a tartalom marketing, folytathatja a toohello végső közzététel fázisban **4. lépés**: [központi telepítése a ajánlat toohello piactér](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Lásd még:
* [Első lépések: hogyan toopublish egy ajánlat toohello Azure piactéren](marketplace-publishing-getting-started.md)

