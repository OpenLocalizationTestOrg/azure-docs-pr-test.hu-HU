---
title: "kimenő kapcsolatok aaaUnderstanding az Azure-ban |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan Azure lehetővé teszi a virtuális gépek toocommunicate nyilvános internetes szolgáltatások."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 5/31/2017
ms.author: kumud
ms.openlocfilehash: ffe59971b934a16c9f6f78e3b15869a0072320d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-outbound-connections-in-azure"></a>Kimenő kapcsolatok áttekintése az Azure-ban

A virtuális gép (VM) az Azure-végpontok nyilvános IP-címtér az Azure-on kívüli kommunikációt. Amikor egy virtuális Gépet egy kimenő folyam tooa cél a nyilvános IP-címtér kezdeményez, Azure hello VM privát IP cím tooa nyilvános IP-címet, és lehetővé teszi, hogy a forgalom tooreach hello virtuális gép.

Azure tooachieve kimenő kapcsolat három különböző módszert biztosít. Minden metódus rendelkezik, saját képességek és megkötések. Gondosan tekintse át az egyes módszerek toochoose, mely megfelel az igényeinek.

| Forgatókönyv | Módszer | Megjegyzés |
| --- | --- | --- |
| Önálló virtuális gép (nem a terheléselosztóhoz, példány szint nyilvános IP-cím) |Alapértelmezett SNAT |Azure egy nyilvános IP-cím a SNAT társítja. |
| Elosztott terhelésű virtuális gép (nem példány szint nyilvános IP-cím a virtuális gép) |SNAT hello terheléselosztó használatával |Azure használ a Load Balancer hello nyilvános IP-címe SNAT |
| Virtuális gép példány szint nyilvános IP-cím (vagy anélkül terheléselosztó) |SNAT nem használatos. |Azure hello nyilvános IP-cím hozzárendelése a virtuális gép toohello használ |

Ha nem szeretné, hogy a virtuális gép toocommunicate végpontokon nyilvános IP-címtér az Azure-on kívüli, a hálózati biztonsági csoportok (NSG) tooblock hozzáférést is használhatja. NSG-k használatával részletesen tárgyalt [nyilvános kapcsolódási akadályozza meg, hogy](#preventing-public-connectivity).

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Önálló virtuális gép nem példány szint nyilvános IP-címmel

Ebben a forgatókönyvben hello virtuális gép nem része egy Azure Load Balancer címkészletet, és nincs hozzárendelve tooit példány szint nyilvános IP (ILPIP) címe. Hello VM egy kimenő folyam hoz létre, amikor az Azure eszköz hello kimenő folyam tooa nyilvános IP-forráscím hello titkos forrás IP-címe. hello a kimenő folyam használt nyilvános IP-cím nem konfigurálható, és nem számít elleni hello előfizetés nyilvános IP-erőforrás korlátját. Azure ezt a funkciót használja a forrás hálózati cím fordítási (SNAT) tooperform. Hello nyilvános IP-cím elmúló port hello virtuális gép által használt toodistinguish egyes adatfolyamok. SNAT dinamikusan kioszt elmúló port, adatfolyamok jönnek létre. Ebben a környezetben használt SNAT hello elmúló port hivatkozott tooas SNAT portok.

SNAT portjait is nagyon gyorsan kimerítették véges erőforrás. Fontos fontos toounderstand hogyan használják fel. Egy SNAT port felhasznált / folyamat tooa egyetlen IP-címre. A több adatfolyamok toohello azonos IP-címre, egyes SNAT egyetlen portot használ fel. Ez biztosítja, hogy hello adatfolyamok egyedi when származik hello azonos nyilvános IP-cím toohello azonos cél IP-címét. Több adatfolyamok, minden tooa különböző célként megadott IP-cím / cél egy SNAT portot használ. IP-célcím hello egyedivé hello adatfolyamok.

Használhat [Naplóelemzési terheléselosztóhoz](load-balancer-monitor-log.md) és [riasztási eseménynaplók toomonitor SNAT port Erőforrásfogyás üzenetek](load-balancer-monitor-log.md#alert-event-log). SNAT port erőforrások elfogytak, amikor a kimenő forgalom sikertelen, amíg a meglévő forgalom által kiadott SNAT portok. Terheléselosztó 4 perces üresjárati időtúllépés VISSZAIGÉNYLÉSE SNAT portokat használ.

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>Virtuális gép elosztott terhelésű és példány szint nyilvános IP-cím

Ebben a forgatókönyvben a hello VM egy Azure Load Balancer készlet részét képezi.  hello virtuális gép nem rendelkezik nyilvános IP-címnek tooit rendelve. szabály toolink hello nyilvános IP időtúllépést hello háttérkészlet a Load Balancer erőforrás hello kell konfigurálni.  Ha nem tölti ki ezt a konfigurációt, hello viselkedés van-e a részben leírt hello fent a [önálló virtuális gép, és nem példány szint nyilvános IP-cím](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address).

Amikor hello terhelésű virtuális gép létrehoz egy kimenő folyam, Azure több hello hello kimenő folyam toohello nyilvános IP-címe hello nyilvános terheléselosztó előtérbeli magánhálózati forrás IP-címe. Azure ezt a funkciót használja a forrás hálózati cím fordítási (SNAT) tooperform. Hello Terheléselosztójának nyilvános IP-cím elmúló port hello virtuális gép által használt toodistinguish egyes adatfolyamok. SNAT dinamikusan kioszt elmúló port a kimenő forgalom létrehozásakor. Ebben a környezetben használt SNAT hello elmúló port hivatkozott tooas SNAT portok.

SNAT portjait is nagyon gyorsan kimerítették véges erőforrás. Fontos fontos toounderstand hogyan használják fel. Egy SNAT port felhasznált / folyamat tooa egyetlen IP-címre. A több adatfolyamok toohello azonos IP-címre, egyes SNAT egyetlen portot használ fel. Ez biztosítja, hogy hello adatfolyamok egyedi when származik hello azonos nyilvános IP-cím toohello azonos cél IP-címét. Több adatfolyamok, minden tooa különböző célként megadott IP-cím / cél egy SNAT portot használ. IP-célcím hello egyedivé hello adatfolyamok.

Használhat [Naplóelemzési terheléselosztóhoz](load-balancer-monitor-log.md) és [riasztási eseménynaplók toomonitor SNAT port Erőforrásfogyás üzenetek](load-balancer-monitor-log.md#alert-event-log). SNAT port erőforrások elfogytak, amikor a kimenő forgalom sikertelen, amíg a meglévő forgalom által kiadott SNAT portok. Terheléselosztó 4 perces üresjárati időtúllépés VISSZAIGÉNYLÉSE SNAT portokat használ.

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>Virtuális gép példány szint nyilvános IP-címmel (vagy anélkül terheléselosztó)

Ebben a forgatókönyvben hello VM egy példány szint nyilvános IP (ILPIP) tooit rendelve van. Nem számít, hogy az hello virtuális gép elosztott terhelésű vagy nem. Egy ILPIP használata esetén a forrás hálózati cím fordítási (SNAT) nem használatos. virtuális gép hello hello ILPIP minden kimenő forgalom használ. Ha az alkalmazás számos kimenő forgalom kezdeményez, és SNAT Erőforrásfogyás tapasztal, érdemes lehet rendelni egy ILPIP tooavoid SNAT megkötések.

## <a name="discovering-hello-public-ip-used-by-a-given-vm"></a>Hello nyilvános IP-címet használja egy adott virtuális gép felderítése

Számos módon toodetermine hello nyilvános IP-forráscím egy kimenő kapcsolat. OpenDNS biztosít egy szolgáltatás, amely meg tudja jeleníteni a virtuális gép nyilvános IP-cím hello. Hello az nslookup parancs használata, elküldheti a DNS-lekérdezések hello neve myip.opendns.com toohello OpenDNS feloldó. hello szolgáltatás hello forrás IP-címe, de a használt toosend hello lekérdezés adja vissza. Lekérdezés után a virtuális gép hello hajtható végre, hello válasz esetén hello nyilvános IP-címet használja ezt a virtuális gépet.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Megakadályozza a nyilvános kapcsolódási

Néha nemkívánatos is a virtuális gép toobe engedélyezett toocreate egy kimenő folyam, vagy előfordulhat, hogy a kimenő folyamatokkal elérhetőségének mely célok követelmény toomanage. Ebben az esetben [hálózati biztonsági csoportok (NSG)](../virtual-network/virtual-networks-nsg.md) toomanage hello célja, hogy hello méretű képes elérni. Ha alkalmazzon NSG tooa terhelésű virtuális gép, toopay figyelmet toohello kell [alapértelmezett címkék](../virtual-network/virtual-networks-nsg.md#default-tags) és [alapértelmezett szabályok](../virtual-network/virtual-networks-nsg.md#default-rules).

Győződjön meg arról, hogy hello VM állapotának mintavételi kérések Azure Load Balancer fogadhat. Ha egy NSG blokkok állapotfigyelő mintavételi kérelmeinek hello AZURE_LOADBALANCER az alapértelmezett címke, a virtuális gép állapotmintáihoz sikertelen lesz, és hello virtuális gép van megjelölve. Terheléselosztó leállítja az új adatfolyamok toothat VM küldésekor.

## <a name="limitations"></a>Korlátozások

Ha [több (nyilvános) IP-címek társítva a terheléselosztó](load-balancer-multivip-overview.md), bármely, a nyilvános IP-címei kimenő forgalom jelöltségét ellenőrző.

Azure egy algoritmus toodetermine hello SNAT elérhető portok száma hello hello készlet mérete alapján használja.  Ez jelenleg nem konfigurálható.

Fontos, hogy SNAT elérhető portok száma hello toorememember nem jelenti azt, közvetlenül a kapcsolatok toonumber. Tekintse meg fent a részletekért során, és hogyan SNAT portok foglal le, és hogyan toomanage kimeríthető ehhez az erőforráshoz.
