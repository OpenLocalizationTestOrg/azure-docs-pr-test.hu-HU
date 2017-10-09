---
title: "egy Azure-webhelyek VPN-kapcsolatot, amelyek nem kapcsolódnak aaaTroubleshoot |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot pont-pont VPN-kapcsolatot, amely hirtelen nem működik, és nem kell csatlakoztatni."
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
ms.openlocfilehash: 632c75bfcfb93a532eeead2855d43e6614569a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Hibáinak elhárítása: Egy Azure-webhelyek VPN-kapcsolatot nem lehet kapcsolódni, és nem működik

Miután konfigurált egy a helyszíni hálózat és az Azure virtuális hálózat közötti pont-pont VPN kapcsolatot, a hello VPN-kapcsolat hirtelen leáll, és nem kell csatlakoztatni. Ez a cikk ismerteti a hibaelhárítási lépéseket toohelp a probléma megoldásához. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések

tooresolve hello probléma, először próbálkozzon az túl[alaphelyzetbe állítása hello Azure VPN gateway](vpn-gateway-resetgw-classic.md) és hello alagút a hello a helyszíni VPN-eszköz alaphelyzetbe állítása. Ha hello a probléma továbbra is fennáll, kövesse ezeket hello probléma lépéseket tooidentify hello okát.

### <a name="prerequisite-step"></a>Előfeltétel-ellenőrzési lépés

Hello Azure VPN-átjáró hello típusának ellenőrzése.

1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com).

2. Ellenőrizze a hello **áttekintése** hello VPN-átjáró hello típus információ oldalán.
    
    ![Hello átjáró – áttekintés](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a>1. lépés Ellenőrizze, hogy a rendszer érvényesíti hello a helyszíni VPN-eszköz

1. Ellenőrizze, hogy használ-e egy [érvényesíteni a VPN-eszköz és az operációs rendszer verziója](vpn-gateway-about-vpn-devices.md#devicetable). Ha nem ellenőrzött VPN-eszközhöz hello eszköz, lehetséges, hogy toocontact hello eszköz gyártó toosee kompatibilitási probléma esetén.

2. Győződjön meg arról, hogy hello VPN-eszköz megfelelően van beállítva. További információkért lásd: [eszköz konfigurációs minták szerkesztése](/vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-verify-hello-shared-key"></a>2. lépés Hello megosztott kulcs ellenőrzése

Hasonlítsa össze a megosztott kulcs hello hello a helyszíni VPN eszköz toohello Azure virtuális hálózat VPN toomake meg arról, hogy megfelel-e hello kulcsok. 

tooview hello megosztott kulcs hello Azure VPN-kapcsolatot, a hello a következő módszerek valamelyikével:

**Azure Portal**

1. Nyissa meg toohello VPN-átjáró webhelyek kapcsolat létrehozott.

2. A hello **beállítások** kattintson **megosztott kulcs**.
    
    ![Megosztott kulcs](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

Hello Azure Resource Manager telepítési modell:

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

Hello klasszikus telepítési modell:

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-hello-vpn-peer-ips"></a>3. lépés Hello VPN társ IP-cím ellenőrzése

-   IP-definíciójában hello hello **helyi hálózati átjáró** Azure objektumban meg kell felelnie a hello helyszíni eszköz IP.
-   hello Azure átjáró IP-definíciót, amely hello be van állítva a helyszíni eszközök meg kell felelnie a hello Azure átjáró IP.

### <a name="step-4-check-udr-and-nsgs-on-hello-gateway-subnet"></a>4. lépés Hello átjáróalhálózatot UDR és NSG-k ellenőrzése

Ellenőrizze a távolítsa el a felhasználó által definiált útválasztási (UDR) vagy a hálózati biztonsági csoportokkal (NSG-k) hello átjáró-alhálózat és tesztelje a hello eredménye. Ha hello probléma megoldódott, UDR vagy NSG alkalmazott hello-beállítások érvényesítése.

### <a name="step-5-check-hello-on-premises-vpn-device-external-interface-address"></a>5. lépés Jelölőnégyzet hello a helyszíni VPN-eszköz kapcsolat külső címét

- Ha hello hello VPN-eszköz Internet felé néző IP-címe szerepel hello **helyi hálózati** definition Azure-ban az Ön is szembesülhet szórványos kapcsolat megszakadása.
- hello eszköz külső felületet kell közvetlenül a hello Internet. Egyetlen hálózati címfordítás vagy hello eszköz- és hello az Internet között tűzfal kell.
- tooconfigure tűzfal fürtözési toohave egy virtuális IP-cím, kell hello fürt megszüntetése, és teszi közzé a hello VPN-készülék közvetlen tooa nyilvános csatoló adott hello átjáró úgy csatlakozhatnak a.

### <a name="step-6-verify-that-hello-subnets-match-exactly-azure-policy-based-gateways"></a>6. lépés Győződjön meg arról, hogy hello alhálózatok pontosan megegyezik-e (az Azure csoportházirend-alapú átjárók)

-   Ellenőrizze, hogy megfelel-e hello alhálózatok pontosan hello Azure-beli virtuális hálózat és a helyszíni definícióit hello Azure-beli virtuális hálózat között.
-   Ellenőrizze, hogy hello alhálózatok hello közötti pontosan egyezik-e **helyi hálózati átjáró** és a helyszíni hello a helyszíni hálózati definíciókat.

### <a name="step-7-verify-hello-azure-gateway-health-probe"></a>7. lépés Hello Azure átjáró állapotmintáihoz ellenőrzése

1. Nyissa meg toohello [állapotmintáihoz](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).

2. Kattintson a hello tanúsítványfigyelmeztetés.
3. Ha választ kapnak hello VPN-átjáró megfelelő minősül. Ha nem érkezik válasz, hello átjáró nem lesz kifogástalan, vagy egy NSG hello átjáró-alhálózatot a problémás hello. a következő szöveg hello egy mintaválasz:

    &lt;? XML-verzió = "1.0"? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">elsődleges példány: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / karakterlánc&gt;

### <a name="step-8-check-whether-hello-on-premises-vpn-device-has-hello-perfect-forward-secrecy-feature-enabled"></a>8. lépés. Ellenőrizze, hogy hello a helyszíni VPN-eszköz hello PFS funkció engedélyezve van

hello PFS szolgáltatás kapcsolatbontás problémákat okozhat. Ha hello VPN-eszköz PFS engedélyezve van, tiltsa le a hello szolgáltatást. Hello VPN gateway IPsec-házirendjének frissíteni.

## <a name="next-steps"></a>Következő lépések

-   [Pont-pont kapcsolat tooa virtuális hálózat konfigurálása](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Pont-pont a VPN-kapcsolatok IPsec/IKE-szabályzat beállítása](vpn-gateway-ipsecikepolicy-rm-powershell.md)
