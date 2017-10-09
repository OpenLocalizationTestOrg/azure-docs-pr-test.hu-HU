---
title: "aaaInternet irányuló load balancer áttekintése |} Microsoft Docs"
description: "Az internetre irányuló terheléselosztót és a szolgáltatások áttekintése. A terheléselosztó működéséről az Azure virtuális gépek és felhőszolgáltatások használatával."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3514f945d69ec576ed256cdd01069491e3e43936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internet-facing-load-balancer-overview"></a>Az Internet felé néző terhelés terheléselosztó áttekintése

Az Azure terheléselosztó továbbítja a hello nyilvános IP-cím és port száma bejövő forgalom toohello privát IP-cím és port számát hello virtuális gép, és ez fordítva is igaz hello válasz forgalom hello virtuális gépről. Terheléselosztási szabályok lehetővé teszik több virtuális gépek vagy szolgáltatások közötti forgalom toodistribute adott típusú. Például a webes kérelem forgalom terhelését hello elosztva is több webkiszolgálók vagy webes szerepkörök.

Egy felhőalapú szolgáltatás, amely tartalmazza a webes szerepkörök vagy feldolgozói szerepkörök példányai meghatározhatja egy nyilvános végpontot hello szolgáltatás definíciós (.csdef) fájl.

Hello *servicedefinition.csdef* hello végpont-konfiguráció tartalmazza, ha több szerepkör példányát egy webes vagy feldolgozói szerepkör üzembe helyezése, hello terheléselosztó lesz állítva. hello módon tooadd példányok tooyour felhő üzembe helyezése hello példányok száma a hello szolgáltatás konfigurációs fájljában (.csfg) változik.

hello alábbi ábrán egy elosztott terhelésű végpont a webes forgalomban hello nyilvános és titkos TCP-port a 80-as három virtuális gépek által közösen használt. A három virtuális gépek vannak egy elosztott terhelésű készlet.

![Példa nyilvános terheléselosztóra](./media/load-balancer-internet-overview/IC727496.png)

1. ábra – elosztott terhelésű végpont a webes forgalom

Internetes ügyfelek toohello nyilvános IP-cím hello felhőszolgáltatás küldött weblap kérelmek 80-as porton, hello Azure terheléselosztó hello kérelmek hello három virtuális gépek hello elosztott terhelésű készlet között osztja el. A load balancer algoritmusok kapcsolatos további információkért lásd: hello [load balancer áttekintése lapon](load-balancer-overview.md#load-balancer-features).

Alapértelmezés szerint az Azure Load Balancer osztja el a hálózati forgalom több virtuálisgép-példánya között. Beállíthatja úgy is munkamenet-kapcsolatot, további információkért lásd: [terheléselosztó terheléselosztási mód](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Következő lépések

További tudnivalók [belső terheléselosztó](load-balancer-internal-overview.md) toobetter megértse, mely terheléselosztót a felhő üzembe helyezése jobban megfelelnek.

Is [Internet felé néző terheléselosztó létrehozásához](load-balancer-get-started-internet-arm-ps.md) és konfigurálása, hogy milyen típusú [mód](load-balancer-distribution-mode.md) egy meghatározott típusú terheléselosztó hálózati forgalom viselkedését.

Ha az alkalmazásnak tookeep kapcsolatok életben kiszolgálók egy terheléselosztó mögött, akkor is jobban átláthatja kapcsolatos [üresjárati TCP időtúllépési beállítások terheléselosztó](load-balancer-tcp-idle-timeout.md). Az Azure Load Balancer használatakor segítségével toolearn kapcsolat üresjárati működése.
