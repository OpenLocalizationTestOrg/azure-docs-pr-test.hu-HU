---
title: "az Azure-alapú környezetben az Oracle katasztrófa utáni helyreállítása aaaOverview |} Microsoft Docs"
description: "Oracle Database 12c adatbázis az Azure környezetben vészhelyreállítás"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/2/2017
ms.author: rclaus
ms.openlocfilehash: 1fa69e1ba044b46b27695fec92fd9ca82df796f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a>Oracle Database 12c adatbázis Azure környezetben vészhelyreállítás

## <a name="assumptions"></a>Előfeltételek

- Oracle Data Guard tervezési és az Azure-környezetek rendelkezik.


## <a name="goals"></a>Célok
- A kialakítási hello topológia és konfigurációt, amely a vész-helyreállítási követelményeinek megfelelően.

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a>1. forgatókönyv: Elsődleges és a vész-Helyreállítási helyeken az Azure-on

Az ügyfél rendelkezik egy Oracle adatbázis-készlet mentése hello elsődleges helyen. A vész-Helyreállítási hely van egy másik régióban. hello ügyfél Oracle Data Guard használja a helyek közötti gyors helyreállítás. hello elsődleges helyet is rendelkezik, egy másodlagos adatbázis jelentéskészítéshez és egyéb felhasználásra. 

### <a name="topology"></a>topológia

Íme az Azure beállítása hello összefoglalása:

- Két hely (elsődleges hely és a vész-Helyreállítási hely)
- Két virtuális hálózatok
- Két adatbázisokkal Oracle Data Guard (elsődleges és készenléti állapot)
- Két Oracle adatbázis Golden kapu vagy Data Guard (csak az elsődleges hely)
- Két alkalmazás szolgáltatást, egy elsődleges és egy hello vész-Helyreállítási helyen
- Egy *rendelkezésre állási csoport* hello elsődleges helyen adatbázis és a alkalmazás service szolgál
- Minden helyen, amely korlátozza a hozzáférést toohello magánhálózati, és csak a bejelentkezés egy rendszergazda lehetővé teszi egy jumpbox
- A jumpbox, a alkalmazás service, az adatbázis és a VPN-átjáró külön alhálózatokon
- NSG kényszeríti az alkalmazás és az adatbázis alhálózatok

![Hello DR topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a>2. forgatókönyv: Helyszíni elsődleges hely és az Azure-on vész-Helyreállítási hely

Az ügyfél rendelkezik egy helyszíni Oracle adatbázis-beállítások (elsődleges helyen). A vész-Helyreállítási hely van, az Azure-on. Oracle Data Guard szolgál a helyek közötti gyors helyreállítás. hello elsődleges helyet is rendelkezik, egy másodlagos adatbázis jelentéskészítéshez és egyéb felhasználásra. 

Kétféleképpen a telepítés.

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-hello-firewall"></a>1. módszer: A helyszíni és az Azure, a hello tűzfal nyitott TCP-portok igénylő közti közvetlen kapcsolatokon keresztül 

Közvetlen kapcsolatok nem ajánlott, mivel ezek az hello TCP-portok toohello globális kívül.

#### <a name="topology"></a>topológia

Az alábbiakban olvashat az Azure beállítása hello összefoglalása:

- Egy vész-Helyreállítási hely 
- Egy virtuális hálózat
- A Data Guard (aktív) egy Oracle-adatbázishoz
- A vész-Helyreállítási hello helyen egy alkalmazásszolgáltatás
- Egy jumpbox, amely korlátozza a hozzáférést toohello magánhálózati, és csak lehetővé teszi a bejelentkezés egy rendszergazda
- A jumpbox, a alkalmazás service, az adatbázis és a VPN-átjáró külön alhálózatokon
- NSG kényszeríti az alkalmazás és az adatbázis alhálózatok
- Az NSG házirendszabály/tooallow bejövő TCP-port 1521 (vagy egy felhasználó által definiált port)
- Az NSG házirendszabály/toorestrict csak hello IP-cím/címek helyszíni (DB vagy alkalmazás) tooaccess hello virtuális hálózat

![Hello DR topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a>2. módszer: Telephelyek közötti VPN
Telephelyek közötti VPN jobb megközelítés. VPN-en beállításával kapcsolatos további információkért lásd: [virtuális hálózat létrehozása a parancssori felület használatával telephelyek közötti VPN-kapcsolattal rendelkező](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).

#### <a name="topology"></a>topológia

Az alábbiakban olvashat az Azure beállítása hello összefoglalása:

- Egy vész-Helyreállítási hely 
- Egy virtuális hálózat 
- A Data Guard (aktív) egy Oracle-adatbázishoz
- A vész-Helyreállítási hello helyen egy alkalmazásszolgáltatás
- Egy jumpbox, amely korlátozza a hozzáférést toohello magánhálózati, és csak lehetővé teszi a bejelentkezés egy rendszergazda
- Külön alhálózatokon vannak a jumpbox, a szolgáltatás, az adatbázis és a VPN-átjáró
- NSG kényszeríti az alkalmazás és az adatbázis alhálózatok
- A helyszíni és az Azure közötti pont-pont VPN-kapcsolat

![Hello DR topológia oldalát bemutató képernyőkép](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a>További olvasnivaló

- [Tervezése és megvalósítása Oracle-adatbázishoz az Azure](oracle-design.md)
- [Oracle Data Guard konfigurálása](configure-oracle-dataguard.md)
- [Oracle Golden kapu beállítása](configure-oracle-golden-gate.md)
- [Oracle biztonsági mentés és helyreállítás](oracle-backup-recovery.md)


## <a name="next-steps"></a>Következő lépések

- [Oktatóanyag: Hozzon létre magas rendelkezésre állású virtuális gépek](../../linux/create-cli-complete.md)
- [Virtuális gép telepítése az Azure parancssori felület minták felfedezése](../../linux/cli-samples.md)
