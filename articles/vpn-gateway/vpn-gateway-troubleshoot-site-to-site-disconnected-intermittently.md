---
title: "az Azure--webhelyek közötti VPN időnként megszakad aaaTroubleshoot |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot hello probléma merült fel a melyik rendszeresen leválasztása hello telephelyek közötti VPN-kapcsolatot."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/21/2017
ms.author: genli
ms.openlocfilehash: 1ce3c4ff9d8f650312e45f33b760ebcc6597fc13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a>Hibaelhárítás: Az időnként megszakad az Azure--webhelyek közötti VPN

Hello probléma, hogy egy új vagy meglévő Microsoft Azure pont-pont VPN-kapcsolat nincs stabil-e, vagy leválik rendszeresen Ön is szembesülhet. Ez a cikk ismerteti a lépéseket toohelp azonosítani és megoldani a hello hello hiba okának elhárítása. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések

### <a name="prerequisite-step"></a>Előfeltétel-ellenőrzési lépés

Az Azure virtuális hálózati átjáró hello típusának ellenőrzése:

1. Nyissa meg túl[Azure-portálon](https://portal.azure.com).
2. Ellenőrizze a hello **áttekintése** hello virtuális hálózati átjáró hello típus információ oldalán.
    
    ![hello átjáró hello áttekintése](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a>Lépés az 1. Ellenőrizze, hogy hello a helyszíni VPN-eszköz van hitelesítve

1. Ellenőrizze, hogy használ-e egy [érvényesíteni a VPN-eszköz és az operációs rendszer verziója](vpn-gateway-about-vpn-devices.md#devicetable). Ha a VPN-eszköz hello nincs érvényesítve, előfordulhat, hogy toocontact hello eszköz gyártó toosee bármely kompatibilitási probléma esetén.
2. Győződjön meg arról, hogy hello VPN-eszköz megfelelően van beállítva. További információkért lásd: [eszköz konfigurációs minták szerkesztése](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-check-hello-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a>2. lépés hello biztonsági társítás beállításokat (a csoportházirend-alapú Azure virtuális hálózati átjárók)

1. Győződjön meg arról, hogy hello virtuális hálózat, alhálózatok és tartományok hello a **helyi hálózati átjáró** definíciót a Microsoft Azure-ban ugyanaz, mint a hello konfigurációs hello a helyszíni VPN-eszközön.
2. Ellenőrizze a biztonsági társítás hello beállítások megfelelő.

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a>3. lépés-ellenőrzése átjáró-alhálózatot a felhasználó által definiált útvonalak és hálózati biztonsági csoportok

Hello átjáró-alhálózatot a felhasználó által megadott útvonal előfordulhat, hogy korlátozzák a forgalmat, és más forgalom engedélyezéséhez. Így megjelenik, hogy hello VPN-kapcsolat megbízhatatlanná néhány adatforgalomhoz és mások helyes. 

### <a name="step-4-check-hello-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a>4. lépés ellenőrzése "egy VPN-alagút egy alhálózat pár" hello beállítása (Csoportházirend-alapú virtuális hálózati átjárók)

Győződjön meg arról, hogy hello a helyszíni VPN-eszköz beállítása toohave **egy VPN-alagút egy alhálózat pár** csoportházirend-alapú virtuális hálózati átjárók.

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a>5. lépés (a csoportházirend-alapú virtuális hálózati átjárók) a biztonsági társítás korlátozás-ellenőrzés

hello csoportházirend-alapú virtuális hálózati átjáró legfeljebb 200 alhálózati biztonsági társítás párok rendelkezik. Ha Azure-beli virtuális hálózat alhálózatok hello száma meg kell szorozni alkalommal hello helyi alhálózatok száma nagyobb, mint 200 az úgy, hogy időnként alhálózatok leválasztása.

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a>6. lépés ellenőrizze a helyi VPN-eszköz kapcsolat külső címét

- Ha hello az internetre irányuló hello VPN-eszköz IP-címe szerepel-e a hello **helyi hálózati átjáró** definition Azure, szórványos kapcsolat megszakadása tapasztalhatja.
- hello eszköz külső felületet kell közvetlenül a hello Internet. Egyetlen hálózati címfordítás (NAT) vagy hello Internet és hello eszköz között tűzfal kell.
-  Ha tűzfal fürtszolgáltatás toohave konfigurálja egy virtuális IP-cím, meg kell hello fürt megszüntetése, és tegye elérhetővé a hello VPN-készülék, közvetlen tooa hello az átjáró nyilvános csatoló úgy csatlakozhatnak a.

### <a name="step-7-check-whether-hello-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a>Lépés 7 ellenőrizze, hogy hello a helyszíni VPN-eszköz teljes továbbítási titkosítása engedélyezve van

Hello **teljes továbbítási titkosítása** szolgáltatás hello kapcsolatbontás problémákat okozhat. Ha hello VPN-eszköz **továbbítását tökéletes** hello szolgáltatás engedélyezve van, tiltsa le. Majd [hello virtuális hálózati átjáró IPSec-házirend frissítése](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).

## <a name="next-steps"></a>Következő lépések

- [Pont-pont kapcsolat tooa virtuális hálózat konfigurálása](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Telephelyek közötti VPN-kapcsolatok IPsec/IKE-házirend konfigurálása](vpn-gateway-ipsecikepolicy-rm-powershell.md)

