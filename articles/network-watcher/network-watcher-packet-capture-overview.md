---
title: "az Azure hálózati figyelőt aaaIntroduction tooPacket rögzítési |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt csomag rögzítése funkció áttekintése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2ce01b391b9c1a1c19aa29c8620628c55586df03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toovariable-packet-capture-in-azure-network-watcher"></a>Bevezetés toovariable csomagrögzítéssel Azure hálózati figyelőt

Hálózati figyelő változó csomagrögzítéssel lehetővé teszi a virtuális gép toocreate csomag rögzítési munkamenetek tootrack forgalom tooand. Csomagrögzítéssel segít toodiagnose hálózati rendellenességeket mindkét reaktív és proactivity. Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez.

Csomagrögzítéssel a virtuálisgép-bővítmény hálózati figyelőt keresztül távolról elindult. Ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel manuálisan virtuális gépen futó hello szükséges, amely értékes időt takaríthat meg. Csomagrögzítéssel hello portál, a PowerShell, a CLI vagy a REST API-n keresztül is elindítható. Például hogyan csomagrögzítéssel is elindítható a virtuális gép a riasztások van. Szűrők a következők: megadott hello rögzítési munkamenet tooensure a kívánt toomonitor forgalom rögzítése. 5 rekordos (protokoll, helyi IP-cím, távoli IP-cím, helyi port és távoli port) alapuló szűrők információkat. hello rögzített adatok hello helyi lemez vagy egy tárolási blob tárolja. A 10 csomag rögzítési munkamenetek régiónként korlátozva van. Ezt a határt csak toohello munkamenetek vonatkozik, és nem felel meg a csomag rögzítési fájlok mentése helyileg hello VM vagy a storage-fiók toohello.

> [!IMPORTANT]
> Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`. A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).

tooreduce hello információkat rögzíti a tooonly hello információt csomag rögzítési munkamenet hello alábbi beállítások érhetők el:

**Konfigurációjának rögzítése**

|Tulajdonság|Leírás|
|---|---|
|**Maximális bájtokat csomagot (bájt)** | szám hello bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja. szám hello bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja. Ha csak hello IPv4-fejléc – kell jelző 34 Itt |
|**Maximális bájtokat munkamenet (bájt)** | Az adott bájtok teljes száma a rendszer rögzíti, hello érték elérésekor hello munkamenete véget nem ér.|
|**Időkorlát (másodperc)** | Készletek hello csomag időkorlát munkamenet rögzítése. hello alapérték 18000 másodperceken vagy 5 óra.|

**Szűrés (nem kötelező)**

|Tulajdonság|Leírás|
|---|---|
|**Protocol (Protokoll)** | hello protokoll toofilter hello csomagok rögzíti. hello elérhető értékek a következők: TCP és UDP összes.|
|**Helyi IP-cím** | Ez az érték szűrők hello csomagok rögzítési toopackets, ahol hello helyi IP-cím megegyezik-e a szűrő.|
|**Helyi port** | Az érték szűrők hello csomag ahol hello helyi port megegyezik-e a szűrő toopackets rögzíti.|
|**Távoli IP-cím** | Az érték szűrők hello csomag ahol hello távoli IP megegyezik-e a szűrő toopackets rögzíti.|
|**Távoli port** | Az érték szűrők hello csomag ahol hello távoli port megegyezik-e a szűrő toopackets rögzíti.|

### <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan kezelheti csomag rögzítésekre hello portálon keresztül ellátogatva [kezelése az Azure-portálon hello csomagrögzítéssel](network-watcher-packet-capture-manage-portal.md) vagy a PowerShell-lel ellátogatva [csomag rögzítése kezelése a PowerShell használatával](network-watcher-packet-capture-manage-powershell.md).

Megtudhatja, hogyan toocreate proaktív csomag rögzíti látogasson el a virtuális gép riasztásain alapuló [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













