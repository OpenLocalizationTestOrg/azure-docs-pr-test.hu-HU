---
title: "a Windows virtuális gépek SAP aaaUsing |} Microsoft Docs"
description: "A Windows virtuális gépek (VM) a Microsoft Azure-ban SAP használatáról törlése"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: 1b455be4-c02f-43e3-8d39-c2d5f216e646
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 6c4d8a066a4a8805668e78e67fd2110f2000ee75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>SAP használata a Windows virtuális gépek Azure-ban
A felhőalapú informatika egy olyan széles körben használt kifejezés, amely egyre több fontos hello informatikai iparági belül van való kis vállalatok toolarge és multinacionális cég vállalatok fel. A Microsoft Azure Cloud Services Platform széles skálája új lehetőségeket kínál, amelyek Microsoft hello. Most ügyfelek rendszer képes toorapidly kiépítése és deaktiválás rendelkezés alkalmazás Felhőszolgáltatások, mint, hogy ne korlátozott tootechnical vagy költségvetési korlátozások. Helyett befektetés időt és a hardver infrastruktúra, a vállalatok összpontosíthat hello alkalmazás, az üzleti folyamatokat és az előnye az ügyfelek és felhasználók.

Microsoft Azure virtuális gépekkel Microsoft biztosít az átfogó infrastruktúra (IaaS) szolgáltatás platformként. Az Azure virtuális gépek támogatják az SAP NetWeaver-alapú alkalmazásokat (IaaS). az alábbi hello tanulmányok ismertetik, hogyan tooplan és megvalósítása SAP NetWeaver alapú alkalmazások a Windows virtuális gépek Azure-ban. SAP NetWeaver-alapú alkalmazások valósítja meg a [Linux virtuális gépek](../../linux/classic/sap-get-started.md).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver Azure - magas rendelkezésre ÁLLÁSÚ
cím: Azure - fürtszolgáltatási SAP ASC/SCS példányok használata a Windows Server feladatátvevő fürt SIOS DataKeeper rendelkező Azure NetWeaver aaaSAP

Összefoglalás: "Ez a dokumentum ismerteti, hogyan toouse SIOS DataKeeper tooset Azure SAP ASC/SCS magas rendelkezésre állású konfiguráció. SAP védelme hiba összetevők, például SAP ASC/SCS vagy sorba helyezni replikációs szolgáltatása a Windows Server feladatátvevő fürt konfigurációjának megosztott lemezek igénylő az egyetlen pont. Az SAP-összetevők kell mindenhonnan elérhetőnek egy SAP rendszer hello működését. Ezért a magas rendelkezésre állású funkcióit kell toobe be toomake meg arról, hogy ezek az összetevők egy kiszolgáló vagy egy virtuális gép működése képes elviselni, mivel fürtkonfigurációk Windows operációs rendszer nélküli és a Hyper-V környezetben végzett helyezze el. 2015. augusztus frissítésétől magát az Azure nem adhatók meg a hello Windows szükséges olyan megosztott lemezzel alapú magas rendelkezésre állású konfiguráció szükséges kritikus SAP összetevők. Hello termék által SIOS DataKeeper hello segítségével, azonban az SAP ASC/SCS igény szerint a Windows Server feladatátvevő fürt konfigurációk hello Azure IaaS platformon építhetők. Ez a tanulmány arról szól, lépés-lépés megközelítéssel hogyan tooinstall megosztott lemezzel Windows Server feladatátvevő fürt konfigurációjának által biztosított SIOS Datakeeper az Azure-ban. hello papír alapján meghatározható a konfigurációban szereplő részletek hello Azure, a Windows és az SAP oldalán amelyek hello magas rendelkezésre állású konfiguráció optimális módon működnek. hello papír kiegészítés hello SAP dokumentáció és az SAP képviselő hello elsődleges erőforrások telepítése és a SAP szoftver központi telepítését az adott platformok.

Frissített: 2015. augusztus

[Ez az útmutató letöltése](http://go.microsoft.com/fwlink/?LinkId=613056)

