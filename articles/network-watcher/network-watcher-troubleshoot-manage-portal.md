---
title: "aaaTroubleshoot Azure virtuális hálózati átjáró és a kapcsolatok - PowerShell |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse hello Azure hálózati figyelőt hibaelhárítása a PowerShell-parancsmag"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: bd568d34091209390c57d22475559bdb99ad2c59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Virtuális hálózati átjáró és az Azure hálózati figyelő PowerShell kapcsolatok hibáinak elhárítása

> [!div class="op_single_selector"]
> - [Portál](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban. Ezek a képességek egyik erőforrás hibaelhárítás. Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül. Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Áttekintés

Erőforrás hibakeresési hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához. A kérelem tooresource hibaelhárítása, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni. Ha vizsgálat befejeződött, hello eredmény akkor minősül. Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, ami eltarthat több percig tooreturn eredményt. a hibaelhárítás hello naplófájljainak tárolása a megadott tárfiók egy tárolót.

## <a name="troubleshoot-a-gateway-or-connection"></a>Átjáró vagy csatlakozási hibáinak elhárítása

1. Keresse meg a toohello [Azure-portálon](https://portal.azure.com) kattintson **hálózati** > **hálózati figyelőt** > **VPN diagnosztika**
2. Válassza ki a **előfizetés**, **erőforráscsoport**, és **hely**.
3. Erőforrás hibakeresési hello hello erőforrás állapotának adatait adja vissza. Menti a megfelelő naplók tooa tárfiók toobe felül is. Kattintson a **Tárfiók** tooselect egy tárfiókot.
4. A hello **tárfiókok** panelen válasszon ki egy meglévő tárfiókot használ, vagy kattintson **+ tárfiók** toocreate egy újat.
5. A hello **tárolók** panelen válasszon ki egy meglévő tárolót, vagy kattintson **+ tároló** toocreate egy új tároló. Ha végzett a kattintson **kiválasztása**
6. Válassza ki a hello átjáró és a kapcsolat erőforrások tootroubleshoot, és kattintson a **hibaelhárítás indítása**

Ha több erőforrás van kiválasztva, hibaelhárítási van egyidejű futtatását átjáró-erőforrásokat. Nem futtatható a kapcsolat és a hozzá társított hello átjáró ugyanannyi időt vesz igénybe. Ajánlott tootroubleshoot átjárók először, mert a kapcsolat hibaelhárítási hosszabb folyamat. Az erőforrás-hello VPN diagnosztika futtatása közben **HIBAELHÁRÍTÁSI állapot** oszlop megjelenik egy állapotának **fut**

Amikor végzett, hello állapotmódosítások oszlop túl**kifogástalan** vagy **nem kifogástalan állapotú**.

![teljes hibaelhárítása][2]

## <a name="understanding-hello-results"></a>Hello eredmények ismertetése

A hello **részletek** szakasz hello ablak hello **állapot** lapon állapotát jeleníti meg hello hello utolsó hibaelhárítási futtatási kijelölt hello erőforráson. Hello legújabb diagnosztikai eredmények látható xx perc után utolsó Futtatás hello.

|Tulajdonság  |Leírás  |
|---------|---------|
|Erőforrás     | Hivatkozás toohello erőforrás.        |
|Tárolási elérési útja     |  Elérési út toohello tárfiók és tároló, amely tartalmazza a hello naplók (Ha bármelyik keletkezett hello futtatása során). Ez a beállítás nem maradnak, ha kilép a hello portálon.        |
|Összefoglalás     | Hello erőforrás állapotának összegzését.        |
|Részletek     | Hello erőforrás állapota részletes tájékoztatást.        |
|Legutóbbi futtatás     | utolsó hello idő hello idejű hibaelhárításhoz lett futott.        |


Hello **művelet** lapon általános útmutatást biztosít a hogyan tooresolve hello probléma. Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást. Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.  Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)


## <a name="next-steps"></a>Következő lépések

Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
