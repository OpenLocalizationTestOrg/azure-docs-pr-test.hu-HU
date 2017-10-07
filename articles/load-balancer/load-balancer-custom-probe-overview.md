---
title: "aaaLoad terheléselosztó egyéni mintavételt és figyelési állapot |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse egyéni vizsgálat Azure Load Balancer toomonitor példányok terheléselosztó mögött"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3dfcfcd2d5cffa58b160cb38d63acffbd997d452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-load-balancer-probes"></a>A Load Balancer vizsgálatok ismertetése

Az Azure Load Balancer hello funkció toomonitor hello állapotát kiszolgálópéldányok mintavételt használatával biztosít. Egy mintavételt toorespond meghibásodása esetén terheléselosztó leállítja az új kapcsolatok toohello nem megfelelő állapotú példány küldésekor. hello meglévő kapcsolatok nem érinti, és új kapcsolatok küldött toohealthy példányok.

Felhőszolgáltatás szerepköreit (feldolgozói szerepköröket és webes szerepkörök) használja a vendégügynök mintavételi figyelésre. TCP- vagy HTTP egyéni mintavételt terheléselosztó mögött a virtuális gépek használatakor be kell állítani.

## <a name="understand-probe-count-and-timeout"></a>Mintavételi száma és időtúllépés

Mintavételi viselkedés attól függ:

* sikeres mintavételek menüpontban, amelyek lehetővé teszik egy példány toobe üzemidőnek feliratú hello száma.
* sikertelen mintavételek menüpontban, amelyek egy címkével, lefelé példány toobe hello száma.

hello időkorlát és a gyakoriság értéke SuccessFailCount a megállapításához, hogy egy példány megerősített toobe fut, vagy nem működik. A hello Azure-portálon a hello időtúllépés tootwo hello értékének hello gyakoriság van beállítva.

elosztott terhelésű szoftverpéldányok hello mintavételi konfigurációs az a végpont (Ez azt jelenti, hogy egy elosztott terhelésű készlet) nem lehet hello azonos. Ez azt jelenti, hogy nem kell egy másik mintavétel konfigurálása az egyes szerepkör példánya vagy a virtuális gép hello azonos üzemeltetett szolgáltatás egy adott végpont kombinációjához. Például minden példány kell rendelkeznie, azonos helyi portok és időtúllépéseket okoz.

> [!IMPORTANT]
> A Load Balancer mintavételi hello IP-címet 168.63.129.16 használja. A nyilvános IP-cím elősegíti a kommunikációs toointernal platform erőforrások hello bring your-saját-IP-Azure virtuális hálózatot. hello virtuális nyilvános IP-cím 168.63.129.16 minden régióban szolgál, és nem változik. Azt javasoljuk, hogy lehetővé tegye az IP-címet a helyi tűzfal házirendekben. Ez nem tekinthető biztonsági kockázatot jelent, mert csak hello belső Azure platformon is forrás-, hogy a cím az üzenet. Ha ezt nem teszi meg, nem lesznek a különböző helyzetekben, például konfigurálása váratlan viselkedést hello azonos IP-címtartomány 168.63.129.16, és hogy ismétlődő IP-címeket.

## <a name="learn-about-hello-types-of-probes"></a>További tudnivalók mintavételt hello típusai

### <a name="guest-agent-probe"></a>Vendég ügynök mintavétele

Ez a Hálózatfigyelő csak akkor használható Azure Cloud Services. Terheléselosztó használja hello vendégügynök hello virtuális gépen belül, és ezután figyeli, és válaszol egy HTTP 200 OK válasz csak akkor, ha hello példány hello kész állapotban van (Ez azt jelenti, hogy nem szerepel egy másik állapot például elfoglalt, újrahasznosítása vagy leállítása).

További információkért lásd: [konfigurálása hello szolgáltatás definíciós fájljának (csdef) állapotteljesítmény](https://msdn.microsoft.com/library/azure/ee758710.aspx) vagy [létrehozásához, egy internetre irányuló terheléselosztót a felhőszolgáltatások](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>A Vendég ügynök mintavétele jelölje példánya sérültként hasznossá?

A HTTP 200 OK toorespond hello vendégügynök meghibásodásakor hello load balancer jelek hello példányához nem válaszol, és a forgalom toothat példány küldése leáll. hello terheléselosztó tooping hello példány továbbra is. Ha hello vendégügynök válaszol egy HTTP 200, hello terheléselosztó újra elküldi forgalom toothat példány.

A webes szerepkör használata esetén hello webhely kód általában a w3wp.exe, nem figyelt hello Azure-hálót vagy vendégügynök fut. Ez azt jelenti, hogy hibák w3wp.exe (például 500-as HTTP-válaszok) nem lesz jelentett toohello vendégügynök, hello terheléselosztó nem lépnek kívül Elforgatás példányhoz.

### <a name="http-custom-probe"></a>Egyéni HTTP-vizsgálatot

hello egyéni HTTP terheléselosztó mintavételi felülbírálások hello alapértelmezett Vendég ügynök mintavétele, ami azt jelenti, hogy a saját egyéni logika toodetermine hello állapotát hello szerepkörpéldányt hozhat létre. hello terheléselosztó-vizsgálat a végpont 15 másodpercenként alapértelmezés szerint. hello példány toobe a hello load balancer elforgatási tekinteni, ha hello határidőn (alapértelmezés szerint 31 másodperc) belül válaszol egy HTTP 200 a.

Ez akkor lehet hasznos, ha azt szeretné, tooimplement load balancer Elforgatás saját logikát tooremove példányok. Például sikertelen eldöntheti, tooremove példány Ha 90 %-os Processzor túllépi ezt az értéket, és nem 200 állapotot ad vissza. Ha webes szerepkörök w3wp.exe, ez azt is jelenti automatikus kap a webhely megfigyeléséhez, mivel a webhely kódban hibák nem 200 állapot toohello terheléselosztói mintavétel ad vissza.

> [!NOTE]
> hello HTTP egyéni mintavételi relatív elérési utakat, és csak a HTTP protokollt támogatja. HTTPS nem támogatott.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Az egyéni HTTP-vizsgálatot, jelölje meg, hogy megsérült példány hasznossá?

* hello HTTP alkalmazás eltérő (például a 403-as, 404 vagy 500) 200-as HTTP-válaszkód adja vissza. Ez az a pozitív visszaigazolás, amely az alkalmazás hello példány azonnal kell tenni nem működik.
* hello HTTP-kiszolgáló nem válaszol minden hello időkorlát után. Attól függően, hogy hello időtúllépési érték, amely be van állítva, ez azt jelentheti, hogy több mintavételi kérések nyissa meg nem érkezik válasz, mielőtt hello mintavételi lekérdezi jelölésű nem fut (Ez azt jelenti, hogy mielőtt SuccessFailCount mintavételt megkapja).
* hello server hello kapcsolaton keresztül a TCP alaphelyzetbe állítása bezárása után.

### <a name="tcp-custom-probe"></a>Egyéni TCP-Hálózatfigyelővel

TCP mintavételt kezdeményeznek kapcsolatot a háromutas kézfogás definiált hello port elvégzésével.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Egy egyéni TCP-Hálózatfigyelővel, jelölje meg, hogy megsérült példány hasznossá?

* hello TCP-kiszolgáló nem válaszol minden hello időkorlát után. Ha hello mintavételi van megjelölve, nem fut sikertelen mintavételi hello száma attól függ, amely nem érkezik válasz nem fut a hello mintavételi megjelölése előtti konfigurált toogo volt kérelmek.
* hello mintavételi kap egy TCP hello szerepkörpéldány alaphelyzetbe.

Egy HTTP állapotmintáihoz vagy a TCP-Hálózatfigyelővel konfigurálásával kapcsolatos további információkért lásd: [létrehozásához egy internetre irányuló terheléselosztót a Resource Manager PowerShell-lel](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Adja hozzá a megfelelő példányok újra üzembe a load balancer Elforgatás

TCP- és HTTP mintavételt kifogástalan minősülnek, és jelölje hello szerepkörpéldányt kifogástalan amennyiben:

* hello terheléselosztó lekérdezi egy pozitív mintavételi hello első alkalommal hello VM indul el.
* hello száma (lásd a korábbiakban) SuccessFailCount sikeres mintavételek menüpontban, amelyek a szükséges toomark hello szerepkörpéldányon kifogástalan hello értékének meghatározása. Ha a szerepkör példánya el lett távolítva, a sikeres, egymást követő mintavételek menüpontban hello száma kell egyenlő vagy SuccessFailCount toomark hello szerepkörpéldányon futó hello értéke meghaladja.

> [!NOTE]
> Ha a szerepkör példánya hello állapotának van ingadozik, hello terheléselosztó már előtt hello szerepkörpéldányt vissza hello kifogástalan állapotban vár. Ez házirend tooprotect hello felhasználói és hello infrastruktúra keresztül történik.

## <a name="use-log-analytics-for-load-balancer"></a>Naplóelemzési használ a Load Balancer

Használhat [analytics keresse meg a terheléselosztó](load-balancer-monitor-log.md) toocheck hello mintavételi egészségügyi állapotát és a mintavételi száma. Naplózási használható terheléselosztó állapot Power bi-ban vagy az Azure Operational Insights tooprovide statisztikája.
