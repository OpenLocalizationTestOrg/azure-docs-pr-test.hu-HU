---
title: "Kriptográfiai követelményeiről és az Azure VPN gatewayek |} Microsoft Docs"
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
ms.openlocfilehash: c789e6c278fc0c58c64f5d96e57f94aee5a6cefc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a><span data-ttu-id="3db6d-103">Kriptográfiai követelményeiről és az Azure VPN gatewayek</span><span class="sxs-lookup"><span data-stu-id="3db6d-103">About cryptographic requirements and Azure VPN gateways</span></span>

<span data-ttu-id="3db6d-104">A cikk ismerteti, hogyan konfigurálhatja az Azure VPN gatewayek a kriptográfiai megfelelnek a létesítmények közötti S2S VPN-alagutat, mind az Azure VNet – VNet kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="3db6d-104">This article discusses how you can configure Azure VPN gateways to satisfy your cryptographic requirements for both cross-premises S2S VPN tunnels and VNet-to-VNet connections within Azure.</span></span> 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a><span data-ttu-id="3db6d-105">Az Azure VPN gatewayek IPsec és az internetes KULCSCSERE házirend paraméterek</span><span class="sxs-lookup"><span data-stu-id="3db6d-105">About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="3db6d-106">Standard IPsec és IKE protokoll titkosítási algoritmusok számos különböző kombinációkban támogatja.</span><span class="sxs-lookup"><span data-stu-id="3db6d-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="3db6d-107">Ha az ügyfelek nem kérő titkosítási algoritmusok és a paraméterek megadott kombinációja, az Azure VPN gatewayek alapértelmezett javaslatokat készletének használata.</span><span class="sxs-lookup"><span data-stu-id="3db6d-107">If customers do not request a specific combination of cryptographic algorithms and parameters, Azure VPN gateways use a set of default proposals.</span></span> <span data-ttu-id="3db6d-108">Az alapértelmezett házirend beállítása azokat a külső VPN-eszközök alapértelmezett beállításokkal való együttműködés maximalizálása volt kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="3db6d-108">The default policy sets were chosen to maximize interoperability with a wide range of third-party VPN devices in default configurations.</span></span> <span data-ttu-id="3db6d-109">Ennek eredményeképpen a szabályzatok és javaslatok számát nem tér ki az összes lehetséges kombinációjának elérhető titkosítási algoritmusok és a kulcs szintjeiről.</span><span class="sxs-lookup"><span data-stu-id="3db6d-109">As a result, the policies and the number of proposals cannot cover all possible combinations of available cryptographic algorithms and key strengths.</span></span>

<span data-ttu-id="3db6d-110">Az alapértelmezett házirend beállítása az Azure VPN-átjáró, a dokumentum szerepel: [kapcsolatos VPN-eszközök és webhelyek közötti VPN átjáró kapcsolatok IPsec/IKE paramétereinek](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="3db6d-110">The default policy set for Azure VPN gateway is listed in the document: [About VPN devices and IPsec/IKE parameters for Site-to-Site VPN Gateway connections](vpn-gateway-about-vpn-devices.md).</span></span>

## <a name="cryptographic-requirements"></a><span data-ttu-id="3db6d-111">Titkosítási követelmények</span><span class="sxs-lookup"><span data-stu-id="3db6d-111">Cryptographic requirements</span></span>
<span data-ttu-id="3db6d-112">A titkosítási algoritmusokat vagy paraméterek igénylő kommunikációhoz általában megfelelőségi és biztonsági követelmények miatt az ügyfelek segítségével mostantól beállíthatja az Azure VPN gatewayek egy egyéni IPsec/IKE házirend használatához az adott kriptográfiai algoritmusok és a kulcs szintjeiről helyett az Azure alapértelmezett házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="3db6d-112">For communications that require specific cryptographic algorithms or parameters, typically due to compliance or security requirements, customers can now configure their Azure VPN gateways to use a custom IPsec/IKE policy with specific cryptographic algorithms and key strengths, rather than the Azure default policy sets.</span></span>

<span data-ttu-id="3db6d-113">Például az Azure VPN gatewayek IKEv2 alapmódú házirendeket használják-e csak Diffie-Hellman csoport 2 (1024 bit), mivel az ügyfelek internetes KULCSCSERE, például csoport 14 (2048 bites), a csoport 24 (2048 bites MODP csoport) vagy a ECP (elliptikus használandó erősebb csoportok megadására is szükség görbe csoportok) 384 vagy 256 bit (csoportos 19 csoport 20, illetve).</span><span class="sxs-lookup"><span data-stu-id="3db6d-113">For example, the IKEv2 main mode policies for Azure VPN gateways utilize only Diffie-Hellman Group 2 (1024 bits), whereas customers may need to specify stronger groups to be used in IKE, such as Group 14 (2048-bit), Group 24 (2048-bit MODP Group), or ECP (elliptic curve groups) 256 or 384 bit (Group 19 and Group 20, respectively).</span></span> <span data-ttu-id="3db6d-114">Hasonló követelmények gyorsmódú házirendeket is érvényesek.</span><span class="sxs-lookup"><span data-stu-id="3db6d-114">Similar requirements apply to IPsec quick mode policies as well.</span></span>

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a><span data-ttu-id="3db6d-115">Az Azure VPN gatewayek egyéni IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="3db6d-115">Custom IPsec/IKE policy with Azure VPN gateways</span></span>
<span data-ttu-id="3db6d-116">Az Azure VPN gatewayek mostantól támogatják az kapcsolatonként, egyéni IPsec/IKE-házirendet.</span><span class="sxs-lookup"><span data-stu-id="3db6d-116">Azure VPN gateways now support per-connection, custom IPsec/IKE policy.</span></span> <span data-ttu-id="3db6d-117">Hely-hely vagy VNet – VNet-kapcsolatot választhat titkosítási algoritmusok egyedi kombinációja IPsec és az internetes KULCSCSERE a kívánt kulcs erősségét és a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="3db6d-117">For a Site-to-Site or VNet-to-VNet connection, you can choose a specific combination of cryptographic algorithms for IPsec and IKE with the desired key strength, as shown in the following example:</span></span>

![IPSec-ike-házirend](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

<span data-ttu-id="3db6d-119">IPsec/IKE-házirend létrehozása, és egy új vagy meglévő kapcsolat alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="3db6d-119">You can create an IPsec/IKE policy and apply to a new or existing connection.</span></span> 

### <a name="workflow"></a><span data-ttu-id="3db6d-120">Munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="3db6d-120">Workflow</span></span>

1. <span data-ttu-id="3db6d-121">A virtuális hálózatok, VPN-átjárók vagy a kapcsolat topológia más útmutatókat a helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="3db6d-121">Create the virtual networks, VPN gateways, or local network gateways for your connectivity topology as described in other how-to documents</span></span>
2. <span data-ttu-id="3db6d-122">IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="3db6d-122">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="3db6d-123">Alkalmazhatja a szabályzatot, amikor egy S2S vagy VNet – VNet-kapcsolatot hoz létre</span><span class="sxs-lookup"><span data-stu-id="3db6d-123">You can apply the policy when you create a S2S or VNet-to-VNet connection</span></span>
4. <span data-ttu-id="3db6d-124">Ha a kapcsolat már létrejött, alkalmazni, vagy egy létező kapcsolatra a házirend frissítése</span><span class="sxs-lookup"><span data-stu-id="3db6d-124">If the connection is already created, you can apply or update the policy to an existing connection</span></span>


## <a name="ipsecike-policy-faq"></a><span data-ttu-id="3db6d-125">IPsec/IKE szabályzata – GYIK</span><span class="sxs-lookup"><span data-stu-id="3db6d-125">IPsec/IKE policy FAQ</span></span>

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="next-steps"></a><span data-ttu-id="3db6d-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3db6d-126">Next steps</span></span>
<span data-ttu-id="3db6d-127">Lásd: [konfigurálása IPsec/IKE házirend](vpn-gateway-ipsecikepolicy-rm-powershell.md) lépésenkénti egyéni IPsec/IKE-házirend konfigurálása a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="3db6d-127">See [Configure IPsec/IKE policy](vpn-gateway-ipsecikepolicy-rm-powershell.md) for step-by-step instructions on configuring custom IPsec/IKE policy on a connection.</span></span>

<span data-ttu-id="3db6d-128">Lásd még: [csatlakozás több csoportházirend-alapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md) további információt a UsePolicyBasedTrafficSelectors lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3db6d-128">See also [Connect multiple policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) to learn more about the UsePolicyBasedTrafficSelectors option.</span></span>