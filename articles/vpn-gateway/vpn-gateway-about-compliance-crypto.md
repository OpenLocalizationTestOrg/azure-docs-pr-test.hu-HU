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
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a><span data-ttu-id="0eb98-103">Kriptográfiai követelményeiről és az Azure VPN gatewayek</span><span class="sxs-lookup"><span data-stu-id="0eb98-103">About cryptographic requirements and Azure VPN gateways</span></span>

<span data-ttu-id="0eb98-104">A cikk ismerteti, hogyan konfigurálhat Azure VPN-átjárók toosatisfy a létesítmények közötti S2S VPN-alagutat, mind az Azure VNet – VNet kapcsolatokhoz kriptográfiai követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="0eb98-104">This article discusses how you can configure Azure VPN gateways toosatisfy your cryptographic requirements for both cross-premises S2S VPN tunnels and VNet-to-VNet connections within Azure.</span></span> 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a><span data-ttu-id="0eb98-105">Az Azure VPN gatewayek IPsec és az internetes KULCSCSERE házirend paraméterek</span><span class="sxs-lookup"><span data-stu-id="0eb98-105">About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="0eb98-106">Standard IPsec és IKE protokoll titkosítási algoritmusok számos különböző kombinációkban támogatja.</span><span class="sxs-lookup"><span data-stu-id="0eb98-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="0eb98-107">Ha az ügyfelek nem kérő titkosítási algoritmusok és a paraméterek megadott kombinációja, az Azure VPN gatewayek alapértelmezett javaslatokat készletének használata.</span><span class="sxs-lookup"><span data-stu-id="0eb98-107">If customers do not request a specific combination of cryptographic algorithms and parameters, Azure VPN gateways use a set of default proposals.</span></span> <span data-ttu-id="0eb98-108">hello alapértelmezett házirend beállítása választott toomaximize együttműködés külső felek VPN-eszközök széles az alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="0eb98-108">hello default policy sets were chosen toomaximize interoperability with a wide range of third-party VPN devices in default configurations.</span></span> <span data-ttu-id="0eb98-109">Ennek eredményeképpen hello szabályzatok és javaslatok hello száma nem tér ki az összes lehetséges kombinációjának elérhető titkosítási algoritmusok és a kulcs szintjeiről.</span><span class="sxs-lookup"><span data-stu-id="0eb98-109">As a result, hello policies and hello number of proposals cannot cover all possible combinations of available cryptographic algorithms and key strengths.</span></span>

<span data-ttu-id="0eb98-110">alapértelmezett házirend beállítása az Azure VPN gateway hello dokumentum szerepel hello: [kapcsolatos VPN-eszközök és webhelyek közötti VPN átjáró kapcsolatok IPsec/IKE paramétereinek](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="0eb98-110">hello default policy set for Azure VPN gateway is listed in hello document: [About VPN devices and IPsec/IKE parameters for Site-to-Site VPN Gateway connections](vpn-gateway-about-vpn-devices.md).</span></span>

## <a name="cryptographic-requirements"></a><span data-ttu-id="0eb98-111">Titkosítási követelmények</span><span class="sxs-lookup"><span data-stu-id="0eb98-111">Cryptographic requirements</span></span>
<span data-ttu-id="0eb98-112">A kommunikációhoz, vagy a titkosítási algoritmusokat és paramétereket általában toocompliance vagy biztonsági követelmények miatt az ügyfelek konfigurálhatja az Azure VPN-átjárók toouse IPsec/IKE egyéni házirendet kriptográfiai specifikus algoritmusok és a kulcs szintjeiről helyett hello Azure alapértelmezett házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="0eb98-112">For communications that require specific cryptographic algorithms or parameters, typically due toocompliance or security requirements, customers can now configure their Azure VPN gateways toouse a custom IPsec/IKE policy with specific cryptographic algorithms and key strengths, rather than hello Azure default policy sets.</span></span>

<span data-ttu-id="0eb98-113">Például hello IKEv2 alapmódú házirendek az Azure VPN gatewayek használják-e csak Diffie-Hellman csoport 2 (1024 bit), mivel az ügyfelek esetleg toospecify IKE, például csoport 14 (2048 bites), a csoport 24 (2048 bites MODP csoport) vagy a ECP (elliptikus használt erősebb csoportok toobe görbe csoportok) 384 vagy 256 bit (csoportos 19 csoport 20, illetve).</span><span class="sxs-lookup"><span data-stu-id="0eb98-113">For example, hello IKEv2 main mode policies for Azure VPN gateways utilize only Diffie-Hellman Group 2 (1024 bits), whereas customers may need toospecify stronger groups toobe used in IKE, such as Group 14 (2048-bit), Group 24 (2048-bit MODP Group), or ECP (elliptic curve groups) 256 or 384 bit (Group 19 and Group 20, respectively).</span></span> <span data-ttu-id="0eb98-114">Hasonló követelményeinek, valamint a tooIPsec gyorsmódú házirendeket alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="0eb98-114">Similar requirements apply tooIPsec quick mode policies as well.</span></span>

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a><span data-ttu-id="0eb98-115">Az Azure VPN gatewayek egyéni IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="0eb98-115">Custom IPsec/IKE policy with Azure VPN gateways</span></span>
<span data-ttu-id="0eb98-116">Az Azure VPN gatewayek mostantól támogatják az kapcsolatonként, egyéni IPsec/IKE-házirendet.</span><span class="sxs-lookup"><span data-stu-id="0eb98-116">Azure VPN gateways now support per-connection, custom IPsec/IKE policy.</span></span> <span data-ttu-id="0eb98-117">Hely-hely vagy VNet – VNet-kapcsolatot választhat titkosítási algoritmusok egyedi kombinációja IPsec és az internetes KULCSCSERE hello szükséges kulcs erősségét, a a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="0eb98-117">For a Site-to-Site or VNet-to-VNet connection, you can choose a specific combination of cryptographic algorithms for IPsec and IKE with hello desired key strength, as shown in hello following example:</span></span>

![IPSec-ike-házirend](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

<span data-ttu-id="0eb98-119">IPsec/IKE-házirend létrehozása, és alkalmazza a tooa új vagy meglévő kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0eb98-119">You can create an IPsec/IKE policy and apply tooa new or existing connection.</span></span> 

### <a name="workflow"></a><span data-ttu-id="0eb98-120">Munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="0eb98-120">Workflow</span></span>

1. <span data-ttu-id="0eb98-121">Hozzon létre hello virtuális hálózatok, a VPN-átjárók és a helyi hálózati átjárók, a kapcsolat topológia, más hogyan toodocuments leírtak alapján.</span><span class="sxs-lookup"><span data-stu-id="0eb98-121">Create hello virtual networks, VPN gateways, or local network gateways for your connectivity topology as described in other how-toodocuments</span></span>
2. <span data-ttu-id="0eb98-122">IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb98-122">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="0eb98-123">Hello házirendet alkalmazhat S2S vagy VNet – VNet kapcsolat létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="0eb98-123">You can apply hello policy when you create a S2S or VNet-to-VNet connection</span></span>
4. <span data-ttu-id="0eb98-124">Ha hello kapcsolatot hozott létre, alkalmazása vagy hello házirend tooan meglévő kapcsolat frissítése</span><span class="sxs-lookup"><span data-stu-id="0eb98-124">If hello connection is already created, you can apply or update hello policy tooan existing connection</span></span>


## <a name="ipsecike-policy-faq"></a><span data-ttu-id="0eb98-125">IPsec/IKE szabályzata – GYIK</span><span class="sxs-lookup"><span data-stu-id="0eb98-125">IPsec/IKE policy FAQ</span></span>

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="next-steps"></a><span data-ttu-id="0eb98-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0eb98-126">Next steps</span></span>
<span data-ttu-id="0eb98-127">Lásd: [konfigurálása IPsec/IKE házirend](vpn-gateway-ipsecikepolicy-rm-powershell.md) lépésenkénti egyéni IPsec/IKE-házirend konfigurálása a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="0eb98-127">See [Configure IPsec/IKE policy](vpn-gateway-ipsecikepolicy-rm-powershell.md) for step-by-step instructions on configuring custom IPsec/IKE policy on a connection.</span></span>

<span data-ttu-id="0eb98-128">Lásd még: [csatlakozás több csoportházirend-alapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md) toolearn hello UsePolicyBasedTrafficSelectors beállítás többet.</span><span class="sxs-lookup"><span data-stu-id="0eb98-128">See also [Connect multiple policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) toolearn more about hello UsePolicyBasedTrafficSelectors option.</span></span>
