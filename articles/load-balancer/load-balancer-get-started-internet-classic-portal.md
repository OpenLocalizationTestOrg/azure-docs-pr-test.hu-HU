---
title: "az internetre irányuló aaaCreate terheléselosztó - klasszikus Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Internet felé néző terheléselosztót a klasszikus üzembe helyezési modell használatával hello a klasszikus Azure portálon"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 27b0d5af6e7b493fa94a9dfbfa260483ae95a2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-classic-portal"></a>Internet felé néző terheléselosztó (klasszikus) a klasszikus Azure portálon hello létrehozásához

> [!div class="op_single_selector"]
> * [klasszikus Azure portál](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus. Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal. Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello. Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Internetkapcsolattal rendelkező terheléselosztó beállítása virtuális gépekhez

A sorrend tooload egyenleg hálózati forgalmat a hello Internet közötti hello virtuális gépek egy felhőalapú szolgáltatás elosztott terhelésű készletet kell létrehoznia. Ez az eljárás feltételezi, hogy már létrehozta hello virtuális gépek és, hogy azok minden hello belül ugyanaz a felhőalapú szolgáltatás.

**a virtuális gépek egy elosztott terhelésű készlet tooconfigure**

1. Hello a klasszikus Azure portálon, kattintson **virtuális gépek**, majd kattintson a virtuális gép elosztott terhelésű készlet hello hello nevére.
2. Kattintson a **Végpontok**, majd a **Hozzáadás** lehetőségre.
3. A hello **egy végpont tooa virtuális gép felvétele elosztott** lapján hello jobbra mutató nyílra.
4. A hello **adja meg a hello részleteket hello végpont** lap:

   * A **neve**hello végpont nevét írja be vagy válassza ki a hello nevét a hello közös protokollok előre definiált végpontok.
   * A **protokoll**, igény szerint, válassza ki azt, hogy a végponthoz, az TCP vagy UDP, hello típusú által igényelt hello protokollt.
   * A **nyilvános portot és magánhálózati Port**, írja be a használni kívánt virtuális gép toouse hello igény szerint hello portszámokat. Hello magánhálózati port és a tűzfalszabályok hello virtuális gép tooredirect forgalom oly módon, hogy megfelelő-e az alkalmazás használhat. hello magánhálózati port ugyanaz, mint a nyilvános port hello is hello. Például a webes (HTTP) forgalomnak a végpont, rendelheti port 80 tooboth hello nyilvános és magánhálózati portot.

5. Válassza ki **hozzon létre egy elosztott terhelésű készletet**, majd kattintson a hello jobbra mutató nyílra.
6. A hello **hello elosztott terhelésű készlet konfigurálása** lapon hello elosztott terhelésű készlet nevét, és a mintavételi viselkedését hello Azure terheléselosztó hello értékeket. Terheléselosztó hello mintavételt toodetermine használja, elérhető tooreceive bejövő forgalom hello elosztott terhelésű készlet hello virtuális gépek esetén.
7. Kattintson a hello pipa toocreate hello elosztott terhelésű végpont. Látni fogja **Igen** a hello **elosztott terhelésű készlet nevét** hello oszlopa **végpontok** lap hello virtuális géphez.
8. Hello portálon kattintson **virtuális gépek**, kattintson egy további virtuális gép elosztott terhelésű készlet hello hello nevét, majd **végpontok**, és kattintson a **Hozzáadás**.
9. A hello **egy végpont tooa virtuális gép felvétele elosztott** lapján kattintson **adja hozzá a végpont tooan elosztott terhelésű készlet**, válassza ki az elosztott terhelésű készlet hello hello nevét, és kattintson a hello jobbra mutató nyílra.
10. A hello **adja meg a hello részleteket hello végpont** lapon, írja be a hello végpont nevét, és kattintson a pipa hello.

Hello további virtuális gépek hello elosztott terhelésű készlet esetében ismételje meg a 8 – 10.

## <a name="next-steps"></a>Következő lépések

[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
