---
title: "Azure hálózati figyelő IP folyamat aaaVerify forgalom ellenőrizze - Azure-portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocheck, ha a virtuális gép forgalom tooor engedélyezett vagy megtagadott"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: abf639f36d32f3416dd927e66b635267b746e62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Ha a forgalom engedélyezett vagy megtagadott a tooor IP Flow ellenőrizze a virtuális gép alapján Azure hálózati figyelőt összetevője egy ellenőrzése

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Az Azure REST API-n](network-watcher-check-ip-flow-verify-rest.md)


IP-adatfolyam ellenőrizze, hogy egy funkciója, amely lehetővé teszi tooverify forgalom engedélyezve van a virtuális gép tooor hálózati figyelőt. hello érvényesítési futtathatja a bejövő vagy kimenő forgalmat. Ebben a forgatókönyvben hasznos tooget, hogy a virtuális gép működik tooan külső erőforrás vagy egy másik erőforrás a jelenlegi állapotában. IP-adatfolyam ellenőrzésére használt tooverify, ha a hálózati biztonsági csoport (NSG) szabályok konfigurációja megfelelő-e, és az NSG-szabályok blokkolt adatfolyamok hibaelhárítása. Egy másik oka IP folyamata győződjön meg arról, amelyet a letiltott tooensure forgalmat blokkol megfelelően hello NSG.

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt, vagy hálózati figyelőt meglévő példányát. hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.

## <a name="scenario"></a>Forgatókönyv

Ez a forgatókönyv használ tooverify IP Flow győződjön meg arról, ha egy virtuális gép működik tooanother gép 443-as porton keresztül. Ha megtagadja a hello forgalom, hello biztonsági szabály, amely megtagadja a forgalom adja vissza. toolearn IP Flow ellenőrzéséhez kapcsolatos további információkért látogasson el [IP-adatfolyam ellenőrizze áttekintése](network-watcher-ip-flow-verify-overview.md)

### <a name="run-ip-flow-verify"></a>Futtatási IP-adatfolyam ellenőrzése

Nyissa meg a tooyour hálózati figyelőt, és kattintson a **IP folyamata ellenőrizze**. Válassza ki a hello virtuális gép és a hálózati illesztő kívánt tooverify forgalmát. Adja meg a szűrési további adatokat, és kattintson a **ellenőrizze**.

Miután rákattintott **ellenőrizze**, hello folyamat a megadott hello feltételeknek megfelelő jelölőnégyzet be van jelölve. hello eredménye vagy **engedélyezett hozzáférési** vagy **hozzáférés megtagadva**. Ha a hozzáférés megtagadva hello hálózati biztonsági csoport (NSG), és biztonsági szabály, amely a forgalom blokkolása valósul meg. Ha forgalom hello szolgáltatásmegtagadásos normális működés, majd hello szabály sikeres volt.

> [!NOTE]
> IP-adatfolyam ellenőrizze, hogy megköveteli, hogy a virtuális gép erőforrásához hello le van foglalva.

Ahogy látja, a kép a következő hello, hello kimenő HTTPS-forgalom engedélyezve volt.

![IP-adatfolyam ellenőrzése – áttekintés][1]

Látható kép a következő hello, megváltozott-e a forgalom tooinbound és hello bejövő megváltozott port too123. Forgalom most megtagadva, "Hozzáférés megtagadva" üdvözlőüzenetére valósul meg hello hálózati biztonsági csoport és a biztonsági szabályt, amely megtagadja a hello forgalom együtt.

![IP-adatfolyam eredmények][2]

## <a name="next-steps"></a>Következő lépések

Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello hálózati biztonsági csoport és a biztonsági szabályok definiált.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













