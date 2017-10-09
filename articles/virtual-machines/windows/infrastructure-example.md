---
title: "Azure-infrastruktúra forgatókönyv aaaExample |} Microsoft Docs"
description: "További tudnivalók hello legfontosabb tervezési és megvalósítási irányelvek Azure-ban egy példa infrastruktúra üzembe helyezéséhez."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd6b6e904404bea0b5be37dfce6d60039daae636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a>Windows virtuális gépek Azure-infrastruktúra bemutató példa

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Ez a cikk végigvezeti egy példa infrastruktúrát létrehozására. A Microsoft részletességi összegyűjti az összes hello irányelvek és elnevezési konvenciókat, a rendelkezésre állási csoportok, a virtuális hálózatok és terheléselosztók döntések egyszerű online tároló-infrastruktúrák tervezése, és ténylegesen telepítése az a virtuális gépek (VM).

## <a name="example-workload"></a>Példa munkaterhelés
Az Adventure Works Cycles toobuild egy online áruház-alkalmazás, amely áll az Azure-ban szeretne:

* Két előtér olyan webes réteg hello ügyfelet futtató IIS-kiszolgálókkal
* Két IIS-kiszolgálókkal, adatokhoz és egy alkalmazás szint kérelmek feldolgozása
* Két Microsoft SQL Server-példány AlwaysOn rendelkezésre állási csoportokkal (két SQL-kiszolgáló és a legtöbb csomópont tanúsító) adatbázis-rétegből termék adatok és a rendeléseket tárolásához
* Két felhasználói fiókok és a szállítók egy hitelesítési réteg Active Directory-tartományvezérlők
* Két alhálózat összes hello kiszolgálók találhatók:
  * a webkiszolgálók hello előtér-alhálózat 
  * hello alkalmazás-kiszolgálókat, az SQL-fürt és a tartományvezérlők háttér-alhálózatot

![Alkalmazás-infrastruktúra különböző rétegek ábrája](./media/infrastructure-example/example-tiers.png)

Bejövő biztonságos webes forgalom kell hello webkiszolgálók közötti kiegyensúlyozott ügyfelek Tallózás hello online áruház. Rendezés feldolgozási forgalom hello képernyőn a HTTP-kérések hello webkiszolgálóról kiszolgálók mérlegelendő hello alkalmazáskiszolgálók között. Emellett a hello infrastruktúra úgy kell megtervezni, a magas rendelkezésre állás érdekében.

hello eredményül kapott tervezési tartalmaznia kell:

* Egy Azure-előfizetés és a fiók
* Egyetlen erőforráscsoportként működnek
* Azure Managed Disks
* Két alhálózat virtuális hálózat
* A virtuális gépek hello hasonló szerepet a rendelkezésre állási készletek
* Virtual machines (Virtuális gépek)

Az összes fenti hello kövesse ezeket elnevezési konvenciókat:

* Adventure Works Cycles használ **[informatikai munkaterhelés]-[hely]-[Azure-erőforrás]** előtagjaként
  * Ebben a példában a "**azos**" (Azure online áruház) van hello IT-munkaterhelést neve és a "**használja**" (USA keleti régiója 2) az hello hely
* Virtuális hálózatok használata AZOS-használat-VN**[szám]**
* Rendelkezésre állási készletek használata azos-használata-,-**[szerepkör]**
* Virtuális gép nevét használja azos-használata-vm -**[vmname]**

## <a name="azure-subscriptions-and-accounts"></a>Azure-előfizetések és fiókok
Az Adventure Works Cycles vállalati előfizetésének, az Adventure Works nagyvállalati előfizetéssel, az informatikai munkaterhelés számlázási tooprovide nevű használ.

## <a name="storage"></a>Storage
Az Adventure Works Cycles határozza meg, hogy azok Azure felügyelt lemezeket kell használnia. Virtuális gépek létrehozásakor rendelkezésre álló tár mindkét tárolórétegek használhatók:

* **Standard szintű tárolást** hello webkiszolgálók, alkalmazás-kiszolgálókat, és a tartományvezérlők és az adatlemezek.
* **Prémium szintű storage** hello SQL Server virtuális gépek és az adatlemezek.

## <a name="virtual-network-and-subnets"></a>Virtuális hálózat és alhálózatok
Mivel hello virtuális hálózat nem kell a folyamatban lévő kapcsolat toohello Adventure munkahelyi ciklusok a helyi hálózaton, azok lezárását egy kizárólag felhőalapú virtuális hálózaton.

Hozza létre őket egy kizárólag felhőalapú virtuális hálózatot a hello beállításait hello Azure portál segítségével a következő:

* Neve: AZOS-használat-VN01
* Helye: USA keleti régiója 2. régiója
* Virtuális hálózat címtere: 10.0.0.0/8
* Első alhálózati:
  * Name: előtér
  * Címtér: 10.0.1.0/24
* Második alhálózati:
  * Name: háttér
  * Címtér: 10.0.2.0/24

## <a name="availability-sets"></a>Rendelkezésre állási csoportok
toomaintain magas rendelkezésre állású összes négy rétegből álló az online áruházat, az Adventure Works Cycles vállalat úgy döntött, a négy rendelkezésre állási csoportok:

* **azos-használja-– weblapként** hello webkiszolgálókhoz
* **azos-használat-,-alkalmazás** hello alkalmazáskiszolgálók
* **azos-használat-,-sql** hello SQL Server-kiszolgálók számára
* **azos-használatát-szerint – dc** hello tartományvezérlők

## <a name="virtual-machines"></a>Virtual machines (Virtuális gépek)
Az Adventure Works Cycles úgy döntött, a neveket az Azure virtuális gépek a következő hello:

* **azos-használat-vm-web01** hello első webkiszolgálón
* **azos-használat-vm-web02** hello második webkiszolgáló
* **azos-használat-vm-app01** hello első alkalmazáskiszolgáló
* **azos-használat-vm-app02** hello második alkalmazáskiszolgáló
* **azos-használat-vm-sql01** hello első SQL Server kiszolgáló hello fürt
* **azos-használat-vm-sql02** hello második SQL Server kiszolgáló hello fürt
* **azos-használat-vm-dc01** hello első tartományvezérlő
* **azos-használat-vm-dc02** hello második tartományvezérlő

Itt található hello létrejövő konfigurációt.

![Az Azure-ban telepített alkalmazások végleges infrastruktúra](./media/infrastructure-example/example-config.png)

Ez a konfiguráció a következőket tartalmazza:

* Egy kizárólag felhőalapú virtuális hálózatot, két alhálózattal (előtér- és háttérszolgáltatások)
* Az Azure Managed lemezek a Standard és a Premium lemezek
* Négy rendelkezésre állási csoportok esetében egy, az egyes rétegekhez hello online áruház
* virtuális gépek hello hello négy rétegek
* Egy külső elosztott terhelésű készlet hello Internet toohello webkiszolgálók a HTTPS-alapú webes forgalom
* Belső elosztott terhelésű készlet hello webalkalmazás kiszolgálók toohello kiszolgálók a titkosítatlan webes forgalom
* Egyetlen erőforráscsoportként működnek