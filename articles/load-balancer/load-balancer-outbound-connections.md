---
title: "Az Azure-ban kimenő kapcsolatok ismertetése |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan Azure lehetővé teszi a virtuális gépek nyilvános internetes szolgáltatásokkal kommunikálni."
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
ms.openlocfilehash: 03cb14b5710b6dd17599a3c4eab21380c76c2b40
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="understanding-outbound-connections-in-azure"></a>Kimenő kapcsolatok áttekintése az Azure-ban

A virtuális gép (VM) az Azure-végpontok nyilvános IP-címtér az Azure-on kívüli kommunikációt. Ha egy virtuális Gépet egy kimenő folyam lévő nyilvános IP-címtér célra, Azure a virtuális magánhálózati IP-címet rendel egy nyilvános IP-cím, és lehetővé teszi, hogy a forgalom a virtuális gép eléréséhez.

Azure kimenő kapcsolat eléréséhez három különböző módszert biztosít. Minden metódus rendelkezik, saját képességek és megkötések. Tekintse át az egyes módszerek gondosan, és válassza ki, melyik felel meg az igényeinek.

| Forgatókönyv | Módszer | Megjegyzés |
| --- | --- | --- |
| Önálló virtuális gép (nem a terheléselosztóhoz, példány szint nyilvános IP-cím) |Alapértelmezett SNAT |Azure egy nyilvános IP-cím a SNAT társítja. |
| Elosztott terhelésű virtuális gép (nem példány szint nyilvános IP-cím a virtuális gép) |Használ a load balancer SNAT |Azure SNAT használ a Load Balancer egy nyilvános IP-címe |
| Virtuális gép példány szint nyilvános IP-cím (vagy anélkül terheléselosztó) |SNAT nem használatos. |Azure használja a virtuális Géphez rendelt nyilvános IP-cím |

Ha nem szeretné, hogy a virtuális gépek végpontok nyilvános IP-címtér az Azure-on kívüli kommunikálni, használhatja a hálózati biztonsági csoportok (NSG) letiltja a hozzáférést. NSG-k használatával részletesen tárgyalt [nyilvános kapcsolódási akadályozza meg, hogy](#preventing-public-connectivity).

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Önálló virtuális gép nem példány szint nyilvános IP-címmel

Ebben a forgatókönyvben a virtuális gép nem része egy Azure Load Balancer címkészletet, és nincs hozzárendelve példány szint nyilvános IP (ILPIP) címe. Ha a virtuális Gépet hoz létre egy kimenő folyam, Azure lefordítja a nyilvános IP-forráscím kimenő folyam titkos forrás IP-címét. A nyilvános IP-cím, a kimenő folyam használt nem konfigurálható, és nem tartoznak bele az előfizetés nyilvános IP-erőforrás korlátját ellen. Azure forrás hálózati cím fordítási (SNAT) használja ezt a funkciót. A nyilvános IP-cím elmúló port segítségével különböztetheti meg egymástól a virtuális gép által az egyes adatfolyamok. SNAT dinamikusan kioszt elmúló port, adatfolyamok jönnek létre. Ebben a környezetben használt SNAT porttartomány nevezzük SNAT portok.

SNAT portjait is nagyon gyorsan kimerítették véges erőforrás. Fontos tudni, hogyan használják fel. Egy SNAT port felhasznált ne egyetlen IP-címre. Több forgalom az azonos IP-címre, az egyes folyamokhoz igényel, egyetlen SNAT port. Ez biztosítja, hogy az adatfolyamok egyedi, amikor származik-e az azonos nyilvános IP-cím az ugyanazon IP-címre. Több adatfolyamok, mindegyik különböző IP-címre használja a cél / egyetlen SNAT port. Az IP-célcím egyedivé teszi a forgalom.

Használhat [Naplóelemzési terheléselosztóhoz](load-balancer-monitor-log.md) és [riasztási eseménynaplójában, figyelje a SNAT port Erőforrásfogyás üzenetek](load-balancer-monitor-log.md#alert-event-log). SNAT port erőforrások elfogytak, amikor a kimenő forgalom sikertelen, amíg a meglévő forgalom által kiadott SNAT portok. Terheléselosztó 4 perces üresjárati időtúllépés VISSZAIGÉNYLÉSE SNAT portokat használ.

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>Virtuális gép elosztott terhelésű és példány szint nyilvános IP-cím

Ebben a forgatókönyvben a virtuális Gépet az Azure Load Balancer beállított része.  A virtuális gép nem rendelkezik a nyilvános IP-cím hozzárendelve. A Load Balancer erőforrás be kell állítani egy szabályt, hogy a nyilvános IP-előtérbeli a háttérkészlet hivatkozásra.  Ha nem tölti ki ezt a konfigurációt, a rendszer nem a fenti szakaszban leírt módon [önálló virtuális gép, és nem példány szint nyilvános IP-cím](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address).

A terheléselosztással rendelkező virtuális Gépet egy kimenő folyam hoz létre, amikor az Azure eszköz a személyes forrás IP-címét a kimenő folyam a nyilvános terheléselosztó előtérbeli nyilvános IP-címére. Azure forrás hálózati cím fordítási (SNAT) használja ezt a funkciót. A Load Balancer nyilvános IP-cím elmúló port segítségével különböztetheti meg egymástól a virtuális gép által az egyes adatfolyamok. SNAT dinamikusan kioszt elmúló port a kimenő forgalom létrehozásakor. Ebben a környezetben használt SNAT porttartomány nevezzük SNAT portok.

SNAT portjait is nagyon gyorsan kimerítették véges erőforrás. Fontos tudni, hogyan használják fel. Egy SNAT port felhasznált ne egyetlen IP-címre. Több forgalom az azonos IP-címre, az egyes folyamokhoz igényel, egyetlen SNAT port. Ez biztosítja, hogy az adatfolyamok egyedi, amikor származik-e az azonos nyilvános IP-cím az ugyanazon IP-címre. Több adatfolyamok, mindegyik különböző IP-címre használja a cél / egyetlen SNAT port. Az IP-célcím egyedivé teszi a forgalom.

Használhat [Naplóelemzési terheléselosztóhoz](load-balancer-monitor-log.md) és [riasztási eseménynaplójában, figyelje a SNAT port Erőforrásfogyás üzenetek](load-balancer-monitor-log.md#alert-event-log). SNAT port erőforrások elfogytak, amikor a kimenő forgalom sikertelen, amíg a meglévő forgalom által kiadott SNAT portok. Terheléselosztó 4 perces üresjárati időtúllépés VISSZAIGÉNYLÉSE SNAT portokat használ.

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>Virtuális gép példány szint nyilvános IP-címmel (vagy anélkül terheléselosztó)

Ebben a forgatókönyvben a virtuális Gépnek legyen egy példány szint nyilvános IP (ILPIP) rendelve. Nem számít, hogy a virtuális gép elosztott terhelésű-e. Egy ILPIP használata esetén a forrás hálózati cím fordítási (SNAT) nem használatos. A virtuális Gépet a ILPIP minden kimenő forgalom használ. Ha az alkalmazás számos kimenő forgalom kezdeményez, és SNAT Erőforrásfogyás tapasztal, érdemes lehet rendelni egy ILPIP SNAT megkötések elkerülése érdekében.

## <a name="discovering-the-public-ip-used-by-a-given-vm"></a>A nyilvános IP-címet használja egy adott virtuális gép felderítése

Számos módon határozza meg a nyilvános IP-forráscím egy kimenő kapcsolat. OpenDNS láthatja a nyilvános IP-címet a virtuális gép egy szolgáltatást biztosít. Az nslookup parancs használata, elküldheti a név myip.opendns.com DNS-lekérdezést a OpenDNS feloldó. A szolgáltatás az IP-forráscím kérés küldése használt adja vissza. A virtuális gépről a következő lekérdezés végrehajtásakor, a rendszer a választ a nyilvános IP-címet használja ezt a virtuális gépet.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Megakadályozza a nyilvános kapcsolódási

Néha a virtuális gépek számára hozzon létre egy kimenő folyam engedélyezett nemkívánatos is, vagy előfordulhat, hogy a követelmény, hogy mely célok elérhető a kimenő forgalom kezeléséhez. Ebben az esetben [hálózati biztonsági csoportok (NSG)](../virtual-network/virtual-networks-nsg.md) kezeléséhez, amelyek a virtuális gép el lehet érni a célhelyre. Amikor egy NSG-t alkalmaz egy elosztott terhelésű virtuális gép, szeretné-e nagy figyelmet fordítani az a [alapértelmezett címkék](../virtual-network/virtual-networks-nsg.md#default-tags) és [alapértelmezett szabályok](../virtual-network/virtual-networks-nsg.md#default-rules).

Győződjön meg arról, hogy a virtuális gép állapotának mintavételi kérések fogadhat Azure Load Balancer. Ha egy NSG blokkolja az egészségügyi mintavételi kérelmeit a AZURE_LOADBALANCER az alapértelmezett címke, a virtuális gép állapotmintáihoz sikertelen lesz, és a virtuális gép van megjelölve. Terheléselosztó leállítja az új forgalom küldése ezt a virtuális Gépet.

## <a name="limitations"></a>Korlátozások

Ha [több (nyilvános) IP-címek társítva a terheléselosztó](load-balancer-multivip-overview.md), bármely, a nyilvános IP-címei kimenő forgalom jelöltségét ellenőrző.

Azure algoritmust használ, a készlet mérete alapján SNAT port számának megállapításához.  Ez jelenleg nem konfigurálható.

Fontos rememember, amely SNAT elérhető portok száma nem fordíthatók le közvetlenül a kapcsolatok száma. Ellenőrizze a fent arról, mikor és hogyan SNAT portok foglal le, és ez kimeríthető erőforrás kezelése.
