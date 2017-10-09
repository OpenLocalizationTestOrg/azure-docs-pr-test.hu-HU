---
title: "aaaManage csomagrögzítéseket Azure hálózati figyelőt - Azure-portál |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toomanage hello csomag rögzítése funkció az Azure-portál használatával hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 431b145ee215b8d9421fb2aacdce08a0de90b35e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-hello-portal"></a>Csomag rögzítésekre kezelése Azure hálózati figyelőt hello portál használatával

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Az Azure REST API-n](network-watcher-packet-capture-manage-rest.md)

Hálózati figyelő csomagrögzítéssel lehetővé teszi toocreate rögzítési munkamenetek tootrack forgalom tooand virtuális gépről. Szűrők előírt hello rögzítési munkamenet tooensure csak a kívánt hello forgalom rögzíti. Csomagrögzítéssel segít toodiagnose hálózati rendellenességek észlelését, proaktív és reaktív is. Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez. Képes tooremotely eseményindító csomag rögzíti őket, ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel fut, manuálisan, hello kívánt számítógépet, amely értékes időt takaríthat meg.

Ez a cikk végigvezeti hello csomagrögzítéssel jelenleg elérhető különböző felügyeleti feladatok.

- [**A csomagrögzítéssel indítása**](#start-a-packet-capture)
- [**A csomagrögzítéssel leállítása**](#stop-a-packet-capture)
- [**A csomagrögzítéssel törlése**](#delete-a-packet-capture)
- [**A csomagrögzítéssel letöltése**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Előkészületek

Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:

- Egy példányát a csomagrögzítéssel toocreate kívánt hálózati figyelőt hello régióban
- Hello csomagok rendelkező virtuális gép rögzítése engedélyezett bővítményekhez.

> [!IMPORTANT]
> Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`. A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-hello-portal"></a>Csomag rögzítési ügynök bővítmény hello portálon keresztül

tooinstall hello csomagrögzítéssel Virtuálisgép-ügynök hello portálon, keresse meg a virtuális gép tooyour, kattintson a **bővítmények** > **Hozzáadás** keresse meg a **hálózati figyelő ügynöke Windows**

![ügynök nézet][agent]

## <a name="packet-capture-overview"></a>Csomag rögzítési áttekintése

Keresse meg a toohello [Azure-portálon](https://portal.azure.com) kattintson **hálózati** > **hálózati figyelőt** > **csomag rögzítése**

hello – áttekintés oldalra látható, minden csomag listája függetlenül hello állapot létező rögzíti.

> [!NOTE]
> Csomagrögzítéssel kapcsolat toohello storage-fiók szükséges a 443-as porton keresztül.

![csomag rögzítési áttekintés képernyő][1]

## <a name="start-a-packet-capture"></a>A csomagrögzítéssel indítása

Kattintson a **Hozzáadás** toocreate egy csomagrögzítéssel.

a csomagrögzítéssel lehet definiálni hello tulajdonságok a következők:

**Fő beállítások**

- **Előfizetés** -ezt az értéket hello előfizetés használt, a hálózati figyelőt példánya van szükség minden előfizetésben.
- **Erőforráscsoport** -hello erőforráscsoport alatt célzott hello virtuális gép.
- **Virtuális gép cél** -hello csomagrögzítéssel futtatott hello virtuális gép
- **Csomag rögzítési neve** -e értéke hello csomagrögzítéssel hello nevét.

**Konfigurációjának rögzítése**

- **A Tárfiók** -határozza meg, ha a tárfiók csomagrögzítéssel menti.
- **Fájl** -határozza meg, ha egy csomagrögzítéssel mentett hello virtuális gépen.
- **Storage-fiókok** -hello kiválasztott tárolási fiók toosave hello csomagok rögzítése a. Alapértelmezett helye https://{storage name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription fiókazonosító} /resourcegroups/ {erőforráscsoport name}/providers/microsoft.compute/virtualmachines/{virtual gép neve} / {éé} / {MM} / {NN} / {HH} packetcapture__{MM}_{SS} {XXX} _ .cap. (Csak akkor engedélyezett, ha **tárolási** van kiválasztva)
- **Helyi fájl elérési útját** -hello helyi elérési út egy virtuális gép toosave hello csomagrögzítéssel a. (Csak akkor engedélyezett, ha **fájl** van kiválasztva). Érvényes elérési utat meg kell adni.
- **Csomag maximális bájtokat** -szám hello bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja.
- **Munkamenet maximális bájtokat** – teljes rögzített, hello érték elérésekor hello csomagok rögzítési leállítja bájtok száma.
- **Időkorlát (másodperc)** -hello csomagok rögzítési toostop határidő beállítása. Alapértelmezett érték 18000 másodperc.

> [!NOTE]
> Prémium szintű storage-fiókok jelenleg nem támogatottak a csomag tárolásához rögzíti.

**Szűrés (nem kötelező)**

- **Protokoll** -protokoll toofilter hello csomagok rögzítési hello. hello elérhető értékek a következők: TCP, UDP és bármilyen.
- **Helyi IP-cím** -ezt az értéket szűrők hello csomagok rögzítési toopackets ahol hello helyi IP-cím megegyezik-e a szűrő.
- **Helyi port** -ezt az értéket szűrők hello csomagok rögzítési toopackets ahol hello helyi port megegyezik-e a szűrő.
- **Távoli IP-cím** -ezt az értéket szűrők hello csomagok rögzítési toopackets ahol hello távoli IP megegyezik-e a szűrő.
- **Távoli port** -ezt az értéket szűrők hello csomagok rögzítési toopackets ahol hello távoli port megegyezik-e a szűrő.

> [!NOTE]
> Port és az IP-cím értékek egyetlen értéket, az értéktartományon vagy egy lehet. (Ez azt jelenti, hogy a 80-1024 port) Megadhatja, hogy annyi szűrők, ahányat csak szeretne.

Amikor hello értékek ki van töltve, kattintson a **OK** toocreate hello csomagrögzítéssel.

![a csomagrögzítéssel létrehozása][2]

Hello időkorlátot a beállítása után hello csomagrögzítéssel lejárt, hello csomagrögzítéssel leáll, és áttekintheti. Állítsa le a hello csomagok készítését munkamenetek manuálisan is.

## <a name="delete-a-packet-capture"></a>A csomagrögzítéssel törlése

Hello csomagban rögzítése a nézetet, kattintson a hello **helyi menü** (...), vagy kattintson a jobb gombbal, és kattintson a **törlése** toostop hello csomagrögzítéssel

![A csomagrögzítéssel törlése][3]

> [!NOTE]
> A csomagrögzítéssel törlése nem érinti hello fájl hello tárfiókban.

A rendszer felkéri kattintson a kívánt toodelete hello csomagrögzítéssel tooconfirm **Igen**

![Jóváhagyás][4]

## <a name="stop-a-packet-capture"></a>A csomagrögzítéssel leállítása

Hello csomagban rögzítése a nézetet, kattintson a hello **helyi menü** (...), vagy kattintson a jobb gombbal, és kattintson a **leállítása** toostop hello csomagrögzítéssel

## <a name="download-a-packet-capture"></a>A csomagrögzítéssel letöltése

A csomag rögzítési munkamenet befejezése után hello rögzítési fájl feltöltött tooblob tároló- és tooa helyi fájlt a virtuális gép hello. hello tárolási helye hello csomagrögzítéssel definiálása hello munkamenet létrehozását. Egy eszköz tooaccess ezek fájlok rögzítését, mentett tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/

Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)

Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













