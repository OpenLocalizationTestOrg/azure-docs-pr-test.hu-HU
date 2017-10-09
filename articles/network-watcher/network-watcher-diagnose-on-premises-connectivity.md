---
title: "az Azure hálózati figyelőt VPN-átjárón keresztül aaaDiagnose a helyi kapcsolat |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan toodiagnose a helyi kapcsolat Azure hálózati figyelőt erőforrás hibaelhárítás VPN-átjárón keresztül."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 9941c5d1b49bec29062210684dae8653cbdb84b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>A helyi kapcsolat keresztül VPN-átjárók diagnosztizálása

Az Azure VPN Gateway lehetővé teszi, hogy a biztonságos kapcsolat a helyszíni hálózat és az Azure virtuális hálózat között hello igény kielégítéséhez toocreate hibrid megoldás. Mivel a követelmények egyedi, ezért nem hello választott helyszíni VPN-eszközön. Jelenleg csak az Azure támogatja [több VPN-eszközök](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) hello eszközforgalmazók együttműködve folyamatosan érvényesíti. A helyszíni VPN-eszköz konfigurálása előtt tekintse át a hello eszközre vonatkozó konfigurációs beállításokat. Hasonló módon van konfigurálva az Azure VPN Gateway vannak beállítva [IPsec paraméterek támogatott](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) kapcsolatok létrehozásához használt. Jelenleg nincs mód toospecify, vagy válasszon olyan egyedi kombinációja hello Azure VPN Gateway IPsec paramétert. A sikeres kapcsolat a helyszíni és az Azure közötti létrehozásához, hello a helyszíni VPN-eszköz beállításait meg kell felelnie Azure VPN-átjáró által előírt hello IPSec-paramétereket. Ha hello-beállítások szerepelnek a helyes, van egy, a kapcsolat megszakadása és eddig ezek a problémák elhárítása nem trivial és általában tartott hello probléma tooidentify és javítsa ki az óra.

Az Azure hálózati figyelőt hello szolgáltatás elhárításával kapcsolatos tudnivalókat képes toodiagnose gondok a az átjáró és a kapcsolatok és percen belül elegendő információt toomake tájékozott döntést toorectify hello probléma lehet.

## <a name="scenario"></a>Forgatókönyv

Azt szeretné, hogy a pont-pont tooconfigure kapcsolatot az Azure és a helyszíni FortiGate használatával, mint a hello a helyszíni VPN-átjáró. tooachieve ebben az esetben szüksége lesz hello beállítása a következő:

1. Virtuális hálózati átjáró - hello Azure VPN Gateway
1. Helyi hálózati átjáró - hello [helyszíni VPN-átjáró (FortiGate)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) Azure felhőben megjelenítésre
1. Webhelyek közötti kapcsolattal (házirendalapú) - [hello VPN Gateway és hello közötti kapcsolat a helyszíni útválasztót](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#createconnection)
1. [FortiGate konfigurálása](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Részletes útmutatást a pont-pont konfiguráció konfigurálásához található ellátogatva: [hozhat létre egy Vnetet hello Azure-portál használatával webhelyek kapcsolattal rendelkező](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Hello kritikus konfigurációs lépések egyikét hello IPSec-kommunikáció paramétereket konfigurálását, a helytelen konfiguráció hello a helyszíni hálózat és az Azure közötti tooloss a kapcsolat részletes útmutatást. Azure VPN Gatewayek jelenleg beállított toosupport hello IPSec-paramétereket az 1. szakasza a következő. Megjegyzés: a korábban említett, ezek a beállítások nem módosíthatók.  Hello az alábbi táblázatban látható, hello titkosítási algoritmusok Azure VPN-átjáró által támogatott rendszer AES256 AES128 és a 3DES.

### <a name="ike-phase-1-setup"></a>IKE-1. fázis beállítása

| **Tulajdonság** | **Házirendalapú** | **RouteBased és Standard vagy nagy teljesítményű VPN gateway** |
| --- | --- | --- |
| IKE verziószám |IKEv1 |IKEv2 |
| Diffie-Hellman Group |2. csoport (1024 bites) |2. csoport (1024 bites) |
| Hitelesítési módszer |Előre megosztott kulcs |Előre megosztott kulcs |
| Titkosítási algoritmusok |AES256 AES128 3DES |AES256 3DES |
| Kivonatoló algoritmus |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| 1. fázisú biztonsági társítás (SA) Élettartam (idő) |28 800 másodperc |10 800 másodperc |

Egy felhasználó nevében, lenne szükséges tooconfigure a FortiGate egy minta konfigurációs található [GitHub](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt). A FortiGate toouse SHA-512 tudtukon kivonatoló algoritmus hello-ként konfigurálva. Ez az algoritmus nincs a támogatott algoritmus a csoportházirend-alapú kapcsolatokhoz, a VPN-kapcsolat működjön.

Ezek a problémák rögzített tootroubleshoot és általában nem intuitív alapvető okait. Ebben az esetben egy támogatási jegy tooget súgójának hello probléma megoldását megnyithatja. De Azure hálózati figyelőt az API elhárításával kapcsolatos tudnivalókat azonosíthatja ezeket a problémákat, saját maga.

## <a name="troubleshooting-using-azure-network-watcher"></a>Hibaelhárítás segítségével Azure hálózati figyelőt

toodiagnose a hálózati kapcsolatot, tooAzure PowerShell csatlakozzon, és kezdeményezzen hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag. Ez a következő parancsmag használatával hello részletei [hibáinak elhárítása a virtuális hálózati átjáró és a kapcsolatok - PowerShell](network-watcher-troubleshoot-manage-powershell.md). Ez a parancsmag toofew perc toocomplete is tarthat.

Hello parancsmag befejeztét követően navigálhat hello parancsmag megadott toohello tárolóhely tooget azoknak a hello probléma és a naplókat. Az Azure hálózati figyelőt hello a következő naplófájlokat tartalmazó zip mappát hoz létre:

![1][1]

Nyitott hello fájl nevű IKEErrors.txt, és megjeleníti hello következő hiba miatt, a problémát jelző helyszíni IKE-beállítás hibás konfigurációja.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

Letölthető részletes információkat hello Scrubbed-wfpdiag.txt hello hiba, mert ebben az esetben az akkor említi, hogy nincs `ERROR_IPSEC_IKE_POLICY_MATCH` , hogy az érdeklődési tooconnection nem működik megfelelően.

Egy másik gyakori hiba a konfiguráció helytelen megosztott kulcsok megadó hello. Hello megelőző például különböző megosztott kulccsal volt megadva, a hello IKEErrors.txt hello hiba a következő látható: `Error: Authentication failed. Check shared key`.

Az Azure hálózati figyelőt hibaelhárítása a szolgáltatás lehetővé teszi a toodiagnose és a VPN-átjáró és a kapcsolat elhárítása egyszerű PowerShell parancsmag hello könnyű. Jelenleg a Microsoft támogatja a következő feltételek diagnosztizálás hello, és kifizetni a további feltétel hozzáadása dolgozik.

### <a name="gateway"></a>Átjáró

| Hibatípus | Ok | Napló|
|---|---|---|
| NoFault | Ha nincs hiba észlelhető. |Igen|
| GatewayNotFound | Nem található átjáró vagy az átjáró nem lett beállítva. |Nem|
| PlannedMaintenance |  Átjárópéldány karbantartás alatt áll.  |Nem|
| UserDrivenUpdate | Amikor a felhasználó frissítése folyamatban van. Ennek oka lehet egy átméretezés. | Nem |
| VipUnResponsive | Hello elsődleges hello átjáró példánya nem érhető el. Ez akkor fordul elő, amikor hello állapotmintáihoz sikertelen. | Nem |
| PlatformInActive | A hello platformmal probléma van. | Nem|
| ServiceNotRunning | hello alapul szolgáló szolgáltatás nem fut. | Nem|
| NoConnectionsFoundForGateway | Hello átjáró kapcsolat nem létezik. Ez a figyelmeztetés csak.| Nem|
| ConnectionsNotConnected | Hello kapcsolatok egyike csatlakoznak. Ez a figyelmeztetés csak.| Igen|
| GatewayCPUUsageExceeded | hello aktuális átjáró használata CPU-használat > 95 %. | Igen |

### <a name="connection"></a>Kapcsolat

| Hibatípus | Ok | Napló|
|---|---|---|
| NoFault | Ha nincs hiba észlelhető. |Igen|
| GatewayNotFound | Nem található átjáró vagy az átjáró nem lett beállítva. |Nem|
| PlannedMaintenance | Átjárópéldány karbantartás alatt áll.  |Nem|
| UserDrivenUpdate | Amikor a felhasználó frissítése folyamatban van. Ennek oka lehet egy átméretezés.  | Nem |
| VipUnResponsive | Hello elsődleges hello átjáró példánya nem érhető el. Akkor történik, ha hello állapotmintáihoz sikertelen lesz. | Nem |
| ConnectionEntityNotFound | Kapcsolat konfigurációja hiányzik. | Nem |
| ConnectionIsMarkedDisconnected | hello kapcsolat meg van jelölve "leválasztott". |Nem|
| ConnectionNotConfiguredOnGateway | hello mögöttes szolgáltatás nincs kapcsolat konfigurálva hello. | Igen |
| ConnectionMarkedStandy | az alapul szolgáló szolgáltatás hello készenléti jelölésű.| Igen|
| Authentication | Előmegosztott kulcs nem megfelelő. | Igen|
| PeerReachability | hello társ átjáró nem érhető el. | Igen|
| IkePolicyMismatch | hello társ átjáró rendelkezik IKE szabályzatok Azure által nem támogatott. | Igen|
| WfpParse hiba | Hiba történt az elemzési hello WFP napló. |Igen|

## <a name="next-steps"></a>Következő lépések

Ismerje meg a PowerShell és az Azure Automation toocheck VPN-átjáró kapcsolat ellátogatva [figyelő VPN-átjárók Azure hálózati figyelőt hibaelhárítás](network-watcher-monitor-with-azure-automation.md)

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
