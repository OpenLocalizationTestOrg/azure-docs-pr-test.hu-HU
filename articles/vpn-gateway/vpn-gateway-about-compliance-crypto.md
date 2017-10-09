---
title: "aaaAbout titkosítási követelmények és az Azure VPN gatewayek |} Microsoft Docs"
description: "Ez a cikk ismerteti a titkosítási követelményeket és az Azure VPN gatewayek"
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2017
ms.author: yushwang
ms.openlocfilehash: af5f14d66beeea5316218f9788c4ad7876826162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>Kriptográfiai követelményeiről és az Azure VPN gatewayek

A cikk ismerteti, hogyan konfigurálhat Azure VPN-átjárók toosatisfy a létesítmények közötti S2S VPN-alagutat, mind az Azure VNet – VNet kapcsolatokhoz kriptográfiai követelményeinek. 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>Az Azure VPN gatewayek IPsec és az internetes KULCSCSERE házirend paraméterek
Standard IPsec és IKE protokoll titkosítási algoritmusok számos különböző kombinációkban támogatja. Ha az ügyfelek nem kérő titkosítási algoritmusok és a paraméterek megadott kombinációja, az Azure VPN gatewayek alapértelmezett javaslatokat készletének használata. hello alapértelmezett házirend beállítása választott toomaximize együttműködés külső felek VPN-eszközök széles az alapértelmezett beállításokkal. Ennek eredményeképpen hello szabályzatok és javaslatok hello száma nem tér ki az összes lehetséges kombinációjának elérhető titkosítási algoritmusok és a kulcs szintjeiről.

alapértelmezett házirend beállítása az Azure VPN gateway hello dokumentum szerepel hello: [kapcsolatos VPN-eszközök és webhelyek közötti VPN átjáró kapcsolatok IPsec/IKE paramétereinek](vpn-gateway-about-vpn-devices.md).

## <a name="cryptographic-requirements"></a>Titkosítási követelmények
A kommunikációhoz, vagy a titkosítási algoritmusokat és paramétereket általában toocompliance vagy biztonsági követelmények miatt az ügyfelek konfigurálhatja az Azure VPN-átjárók toouse IPsec/IKE egyéni házirendet kriptográfiai specifikus algoritmusok és a kulcs szintjeiről helyett hello Azure alapértelmezett házirend beállítása.

Például hello IKEv2 alapmódú házirendek az Azure VPN gatewayek használják-e csak Diffie-Hellman csoport 2 (1024 bit), mivel az ügyfelek esetleg toospecify IKE, például csoport 14 (2048 bites), a csoport 24 (2048 bites MODP csoport) vagy a ECP (elliptikus használt erősebb csoportok toobe görbe csoportok) 384 vagy 256 bit (csoportos 19 csoport 20, illetve). Hasonló követelményeinek, valamint a tooIPsec gyorsmódú házirendeket alkalmazhat.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Az Azure VPN gatewayek egyéni IPsec/IKE-házirend
Az Azure VPN gatewayek mostantól támogatják az kapcsolatonként, egyéni IPsec/IKE-házirendet. Hely-hely vagy VNet – VNet-kapcsolatot választhat titkosítási algoritmusok egyedi kombinációja IPsec és az internetes KULCSCSERE hello szükséges kulcs erősségét, a a hello a következő példában látható módon:

![IPSec-ike-házirend](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

IPsec/IKE-házirend létrehozása, és alkalmazza a tooa új vagy meglévő kapcsolat. 

### <a name="workflow"></a>Munkafolyamat

1. Hozzon létre hello virtuális hálózatok, a VPN-átjárók és a helyi hálózati átjárók, a kapcsolat topológia, más hogyan toodocuments leírtak alapján.
2. IPsec/IKE-házirend létrehozása
3. Hello házirendet alkalmazhat S2S vagy VNet – VNet kapcsolat létrehozásakor
4. Ha hello kapcsolatot hozott létre, alkalmazása vagy hello házirend tooan meglévő kapcsolat frissítése


## <a name="ipsecike-policy-faq"></a>IPsec/IKE szabályzata – GYIK

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="next-steps"></a>Következő lépések
Lásd: [konfigurálása IPsec/IKE házirend](vpn-gateway-ipsecikepolicy-rm-powershell.md) lépésenkénti egyéni IPsec/IKE-házirend konfigurálása a kapcsolatot.

Lásd még: [csatlakozás több csoportházirend-alapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md) toolearn hello UsePolicyBasedTrafficSelectors beállítás többet.
