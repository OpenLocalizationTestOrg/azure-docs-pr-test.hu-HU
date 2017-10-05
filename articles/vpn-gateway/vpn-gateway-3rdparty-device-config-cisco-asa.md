---
title: "Mintakonfiguráció - Cisco ASA eszköz csatlakoztatása az Azure VPN gatewayek |} Microsoft Docs"
description: "Ez a cikk a Cisco ASA eszköz csatlakoztatása az Azure VPN gatewayek minta konfigurációját tartalmazza."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 10466b8928e2cd687f7961a2956b6d60823b82be
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a><span data-ttu-id="6bf59-103">Mintakonfiguráció: Cisco ASA (IKEv2/nem BGP-) eszköz</span><span class="sxs-lookup"><span data-stu-id="6bf59-103">Sample configuration: Cisco ASA device (IKEv2/no BGP)</span></span>
<span data-ttu-id="6bf59-104">Ez a cikk ismerteti az Azure VPN gatewayek Cisco ASA eszközök minta konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="6bf59-104">This article provides sample configurations for Cisco ASA devices connecting to Azure VPN gateways.</span></span>

## <a name="device-at-a-glance"></a><span data-ttu-id="6bf59-105">Egy pillanat alatt eszköz</span><span class="sxs-lookup"><span data-stu-id="6bf59-105">Device at a glance</span></span>

|                        |                                   |
| ---                    | ---                               |
| <span data-ttu-id="6bf59-106">Eszköz gyártója</span><span class="sxs-lookup"><span data-stu-id="6bf59-106">Device vendor</span></span>          | <span data-ttu-id="6bf59-107">Cisco</span><span class="sxs-lookup"><span data-stu-id="6bf59-107">Cisco</span></span>                             |
| <span data-ttu-id="6bf59-108">Eszközmodell</span><span class="sxs-lookup"><span data-stu-id="6bf59-108">Device model</span></span>           | <span data-ttu-id="6bf59-109">ASA (adaptív biztonsági készülék)</span><span class="sxs-lookup"><span data-stu-id="6bf59-109">ASA (Adaptive Security Appliance)</span></span> |
| <span data-ttu-id="6bf59-110">Célverzió</span><span class="sxs-lookup"><span data-stu-id="6bf59-110">Target version</span></span>         | <span data-ttu-id="6bf59-111">8.4+</span><span class="sxs-lookup"><span data-stu-id="6bf59-111">8.4+</span></span>                              |
| <span data-ttu-id="6bf59-112">Tesztelt modellt</span><span class="sxs-lookup"><span data-stu-id="6bf59-112">Tested model</span></span>           | <span data-ttu-id="6bf59-113">5505 ASA</span><span class="sxs-lookup"><span data-stu-id="6bf59-113">ASA 5505</span></span>                          |
| <span data-ttu-id="6bf59-114">Tesztelt verziója</span><span class="sxs-lookup"><span data-stu-id="6bf59-114">Tested version</span></span>         | <span data-ttu-id="6bf59-115">9.2</span><span class="sxs-lookup"><span data-stu-id="6bf59-115">9.2</span></span>                               |
| <span data-ttu-id="6bf59-116">IKE-verzió</span><span class="sxs-lookup"><span data-stu-id="6bf59-116">IKE version</span></span>            | <span data-ttu-id="6bf59-117">**IKEv2**</span><span class="sxs-lookup"><span data-stu-id="6bf59-117">**IKEv2**</span></span>                         |
| <span data-ttu-id="6bf59-118">BGP</span><span class="sxs-lookup"><span data-stu-id="6bf59-118">BGP</span></span>                    | <span data-ttu-id="6bf59-119">**Nem**</span><span class="sxs-lookup"><span data-stu-id="6bf59-119">**No**</span></span>                            |
| <span data-ttu-id="6bf59-120">Az Azure VPN-átjáró típusa</span><span class="sxs-lookup"><span data-stu-id="6bf59-120">Azure VPN gateway type</span></span> | <span data-ttu-id="6bf59-121">**Útválasztó-alapú** VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="6bf59-121">**Route-based** VPN gateway</span></span>       |
|                        |                                   |

> [!NOTE]
> 1. <span data-ttu-id="6bf59-122">Az alábbi konfigurációs Cisco ASA eszköz csatlakozik az Azure **útválasztó-alapú** házirenddel egyéni IPsec/IKE "UserPolicyBasedTrafficSelectors" beállítást, a VPN-átjáró [Ez a cikk](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="6bf59-122">The configuration below connects a Cisco ASA device to an Azure **route-based** VPN gateway using custom IPsec/IKE policy with "UserPolicyBasedTrafficSelectors" option, as described in [this article](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>
> 2. <span data-ttu-id="6bf59-123">ASA eszközök használatára van szükség **IKEv2** a hozzáférési lista-konfigurációkat, nem VTI-alapú.</span><span class="sxs-lookup"><span data-stu-id="6bf59-123">It requires ASA devices to use **IKEv2** with access-list-based configurations, not VTI-based.</span></span>
> 3. <span data-ttu-id="6bf59-124">Tekintse meg a VPN szállító műszaki győződjön meg arról, a házirend a helyszíni VPN-eszközök esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="6bf59-124">Please consult with your VPN device vendor specifications to ensure the policy is supported on your on-premises VPN devices.</span></span>

## <a name="vpn-device-requirements"></a><span data-ttu-id="6bf59-125">VPN-eszköz követelményei</span><span class="sxs-lookup"><span data-stu-id="6bf59-125">VPN device requirements</span></span>
<span data-ttu-id="6bf59-126">Az Azure VPN gatewayek szabványos IPsec/IKE protokoll csomagok használatával szeretne létrehozni a S2S VPN-alagutat.</span><span class="sxs-lookup"><span data-stu-id="6bf59-126">Azure VPN gateways use standard IPsec/IKE protocol suites to establish S2S VPN tunnels.</span></span> <span data-ttu-id="6bf59-127">Tekintse meg [kapcsolatos VPN-eszközök](vpn-gateway-about-vpn-devices.md) részletes IPsec/IKE protokoll paraméterek és alapértelmezett titkosítási algoritmusok az Azure VPN gatewayek.</span><span class="sxs-lookup"><span data-stu-id="6bf59-127">Refer to [About VPN devices](vpn-gateway-about-vpn-devices.md) for the detailed IPsec/IKE protocol parameters and default cryptographic algorithms for Azure VPN gateways.</span></span> <span data-ttu-id="6bf59-128">Opcionálisan megadhat a titkosítási algoritmusok és egy adott kapcsolat kulcs szintjeiről pontos kombinációja a leírtak szerint [titkosítási követelményekkel kapcsolatos](vpn-gateway-about-compliance-crypto.md).</span><span class="sxs-lookup"><span data-stu-id="6bf59-128">You can optionally specify the exact combination of cryptographic algorithms and key strengths for a specific connection as described in [About cryptographic requirements](vpn-gateway-about-compliance-crypto.md).</span></span> <span data-ttu-id="6bf59-129">Ha olyan titkosítási algoritmusok és a kulcs szintjeiről egyedi kombinációja, ellenőrizze, hogy a VPN-eszközök használatát a vonatkozó előírásokat.</span><span class="sxs-lookup"><span data-stu-id="6bf59-129">If you select a specific combination of cryptographic algorithms and key strengths, please make sure you use the corresponding specifications on your VPN devices.</span></span>

## <a name="single-vpn-tunnel"></a><span data-ttu-id="6bf59-130">Egy VPN-alagút</span><span class="sxs-lookup"><span data-stu-id="6bf59-130">Single VPN tunnel</span></span>
<span data-ttu-id="6bf59-131">Ez a topológia áll az Azure VPN gateway és a helyszíni VPN-eszköz között egyetlen S2S VPN-alagúton.</span><span class="sxs-lookup"><span data-stu-id="6bf59-131">This topology consists of a single S2S VPN tunnel between an Azure VPN gateway and your on-premises VPN device.</span></span> <span data-ttu-id="6bf59-132">A VPN-alagúton keresztül nem kötelező beállítani a BGP Protokollt.</span><span class="sxs-lookup"><span data-stu-id="6bf59-132">You can optionally configure BGP across the VPN tunnel.</span></span>

![az egyalagutas](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

<span data-ttu-id="6bf59-134">Tekintse meg [egyalagutas telepítő](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) hozhat létre az Azure-konfigurációjának részletes, lépésenkénti útmutatást.</span><span class="sxs-lookup"><span data-stu-id="6bf59-134">Refer to [Single tunnel setup](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) for detailed, step-by-step instructions to build the Azure configurations.</span></span>

### <a name="network-and-vpn-gateway-information"></a><span data-ttu-id="6bf59-135">Hálózati és a VPN-átjáró adatai</span><span class="sxs-lookup"><span data-stu-id="6bf59-135">Network and VPN gateway information</span></span>
<span data-ttu-id="6bf59-136">Ez a szakasz a paraméterek listázása az alábbi minta.</span><span class="sxs-lookup"><span data-stu-id="6bf59-136">This section list the parameters for the this sample.</span></span>

| <span data-ttu-id="6bf59-137">**A paraméter**</span><span class="sxs-lookup"><span data-stu-id="6bf59-137">**Parameter**</span></span>                | <span data-ttu-id="6bf59-138">**Érték**</span><span class="sxs-lookup"><span data-stu-id="6bf59-138">**Value**</span></span>                    |
| ---                          | ---                          |
| <span data-ttu-id="6bf59-139">VNet-címelőtagjait</span><span class="sxs-lookup"><span data-stu-id="6bf59-139">VNet address prefixes</span></span>        | <span data-ttu-id="6bf59-140">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6bf59-140">10.11.0.0/16</span></span><br><span data-ttu-id="6bf59-141">10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6bf59-141">10.12.0.0/16</span></span> |
| <span data-ttu-id="6bf59-142">Az Azure VPN gateway IP-címet</span><span class="sxs-lookup"><span data-stu-id="6bf59-142">Azure VPN gateway IP</span></span>         | <span data-ttu-id="6bf59-143">Azure_Gateway_Public_IP</span><span class="sxs-lookup"><span data-stu-id="6bf59-143">Azure_Gateway_Public_IP</span></span>      |
| <span data-ttu-id="6bf59-144">A helyszíni címelőtagokat</span><span class="sxs-lookup"><span data-stu-id="6bf59-144">On-premises address prefixes</span></span> | <span data-ttu-id="6bf59-145">10.51.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6bf59-145">10.51.0.0/16</span></span><br><span data-ttu-id="6bf59-146">10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6bf59-146">10.52.0.0/16</span></span> |
| <span data-ttu-id="6bf59-147">A helyszíni VPN-eszköz IP-címet</span><span class="sxs-lookup"><span data-stu-id="6bf59-147">On-premises VPN device IP</span></span>    | <span data-ttu-id="6bf59-148">OnPrem_Device_Public_IP</span><span class="sxs-lookup"><span data-stu-id="6bf59-148">OnPrem_Device_Public_IP</span></span>     |
| <span data-ttu-id="6bf59-149">* VNet BGP ASN</span><span class="sxs-lookup"><span data-stu-id="6bf59-149">*VNet BGP ASN</span></span>                | <span data-ttu-id="6bf59-150">65010</span><span class="sxs-lookup"><span data-stu-id="6bf59-150">65010</span></span>                        |
| <span data-ttu-id="6bf59-151">* Az azure BGP-Társgép IP-</span><span class="sxs-lookup"><span data-stu-id="6bf59-151">*Azure BGP peer IP</span></span>           | <span data-ttu-id="6bf59-152">10.12.255.30</span><span class="sxs-lookup"><span data-stu-id="6bf59-152">10.12.255.30</span></span>                 |
| <span data-ttu-id="6bf59-153">* A helyszíni BGP ASN</span><span class="sxs-lookup"><span data-stu-id="6bf59-153">*On-premises BGP ASN</span></span>         | <span data-ttu-id="6bf59-154">65050</span><span class="sxs-lookup"><span data-stu-id="6bf59-154">65050</span></span>                        |
| <span data-ttu-id="6bf59-155">* A helyszíni BGP-Társgép IP-</span><span class="sxs-lookup"><span data-stu-id="6bf59-155">*On-premises BGP peer IP</span></span>     | <span data-ttu-id="6bf59-156">10.52.255.254</span><span class="sxs-lookup"><span data-stu-id="6bf59-156">10.52.255.254</span></span>                |
|                              |                              |

* <span data-ttu-id="6bf59-157">(*) Választható paraméterek: a BGP csak.</span><span class="sxs-lookup"><span data-stu-id="6bf59-157">(*) Optional parameters for BGP only.</span></span>

### <a name="ipsecike-policy--parameters"></a><span data-ttu-id="6bf59-158">IPsec-/ h.rend & paraméterek</span><span class="sxs-lookup"><span data-stu-id="6bf59-158">IPsec/IKE policy & parameters</span></span>

<span data-ttu-id="6bf59-159">Az alábbi táblázat az IPsec vagy internetes KULCSCSERE-algoritmusok és a mintában használt paraméterek.</span><span class="sxs-lookup"><span data-stu-id="6bf59-159">The table below lists the IPsec/IKE algorithms and parameters used in the sample.</span></span> <span data-ttu-id="6bf59-160">Győződjön meg arról, hogy a fent felsorolt összes algoritmusok a VPN-eszköz modellek és belsővezérlőprogram-verziók által támogatott VPN műszaki további részleteket.</span><span class="sxs-lookup"><span data-stu-id="6bf59-160">Please consult your VPN device specifications to make sure all algorithms listed above are supported by your VPN device models and firmware versions.</span></span>

| <span data-ttu-id="6bf59-161">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="6bf59-161">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="6bf59-162">**Érték**</span><span class="sxs-lookup"><span data-stu-id="6bf59-162">**Value**</span></span>                            |
| ---              | ---                                  |
| <span data-ttu-id="6bf59-163">IKEv2-titkosítás</span><span class="sxs-lookup"><span data-stu-id="6bf59-163">IKEv2 Encryption</span></span> | <span data-ttu-id="6bf59-164">AES256</span><span class="sxs-lookup"><span data-stu-id="6bf59-164">AES256</span></span>                               |
| <span data-ttu-id="6bf59-165">IKEv2-integritás</span><span class="sxs-lookup"><span data-stu-id="6bf59-165">IKEv2 Integrity</span></span>  | <span data-ttu-id="6bf59-166">SHA384</span><span class="sxs-lookup"><span data-stu-id="6bf59-166">SHA384</span></span>                               |
| <span data-ttu-id="6bf59-167">DH-csoport</span><span class="sxs-lookup"><span data-stu-id="6bf59-167">DH Group</span></span>         | <span data-ttu-id="6bf59-168">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="6bf59-168">DHGroup24</span></span>                            |
| <span data-ttu-id="6bf59-169">IPsec-titkosítás</span><span class="sxs-lookup"><span data-stu-id="6bf59-169">IPsec Encryption</span></span> | <span data-ttu-id="6bf59-170">AES256</span><span class="sxs-lookup"><span data-stu-id="6bf59-170">AES256</span></span>                               |
| <span data-ttu-id="6bf59-171">IPsec-integritás</span><span class="sxs-lookup"><span data-stu-id="6bf59-171">IPsec Integrity</span></span>  | <span data-ttu-id="6bf59-172">SHA1</span><span class="sxs-lookup"><span data-stu-id="6bf59-172">SHA1</span></span>                                 |
| <span data-ttu-id="6bf59-173">PFS-csoport</span><span class="sxs-lookup"><span data-stu-id="6bf59-173">PFS Group</span></span>        | <span data-ttu-id="6bf59-174">PFS24</span><span class="sxs-lookup"><span data-stu-id="6bf59-174">PFS24</span></span>                                |
| <span data-ttu-id="6bf59-175">Gyorsmódú biztonsági társítás élettartama</span><span class="sxs-lookup"><span data-stu-id="6bf59-175">QM SA Lifetime</span></span>   | <span data-ttu-id="6bf59-176">7200 másodperc</span><span class="sxs-lookup"><span data-stu-id="6bf59-176">7200 seconds</span></span>                         |
| <span data-ttu-id="6bf59-177">Forgalomválasztó</span><span class="sxs-lookup"><span data-stu-id="6bf59-177">Traffic Selector</span></span> | <span data-ttu-id="6bf59-178">UsePolicyBasedTrafficSelectors $True</span><span class="sxs-lookup"><span data-stu-id="6bf59-178">UsePolicyBasedTrafficSelectors $True</span></span> |
| <span data-ttu-id="6bf59-179">Előre megosztott kulcs</span><span class="sxs-lookup"><span data-stu-id="6bf59-179">Pre-Shared Key</span></span>   | <span data-ttu-id="6bf59-180">PreSharedKey</span><span class="sxs-lookup"><span data-stu-id="6bf59-180">PreSharedKey</span></span>                         |
|                  |                                      |

- <span data-ttu-id="6bf59-181">(*) Néhány eszközön IPSec-integritásának "null", ha a GCM-AES IPsec titkosítási algoritmusként használt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6bf59-181">(*) On some device, IPsec integrity must be "null" if GCM-AES is used as IPsec encryption algorithm.</span></span>

### <a name="device-notes"></a><span data-ttu-id="6bf59-182">Eszköz megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6bf59-182">Device notes</span></span>

>[!NOTE]
>
> 1. <span data-ttu-id="6bf59-183">IKEv2 támogatás szükséges ASA 8.4 verzió vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="6bf59-183">IKEv2 support requires ASA version 8.4 and above.</span></span>
> 2. <span data-ttu-id="6bf59-184">(Túl csoport 5) nagyobb DH és a PFS csoport támogatási ASA verziója szükséges 9.x.</span><span class="sxs-lookup"><span data-stu-id="6bf59-184">Higher DH and PFS group support (beyond Group 5) requires ASA version 9.x.</span></span>
> 3. <span data-ttu-id="6bf59-185">IPsec-titkosítást az SHA-256, SHA-384-et, SHA-512 támogatási AES-GCM- és IPSec-integritási ASA verziója szükséges 9.x újabb ASA hardverre; 5505 ASA, 5510, 5520, 5540, 5550, amelyek 5580 **nem** támogatott.</span><span class="sxs-lookup"><span data-stu-id="6bf59-185">IPsec encryption with AES-GCM and IPsec integrity with SHA-256, SHA-384, SHA-512 support requires ASA version 9.x on newer ASA hardware; ASA 5505, 5510, 5520, 5540, 5550, 5580 are **not** supported.</span></span> <span data-ttu-id="6bf59-186">(Ellenőrizze a szállító specifikációk megerősítéséhez.)</span><span class="sxs-lookup"><span data-stu-id="6bf59-186">(Please check the vendor specifications to confirm.)</span></span>
>


### <a name="sample-device-configurations"></a><span data-ttu-id="6bf59-187">A minta eszköz-konfigurációk</span><span class="sxs-lookup"><span data-stu-id="6bf59-187">Sample device configurations</span></span>
<span data-ttu-id="6bf59-188">Az alábbi parancsfájl egy minta-konfigurációt a topológia és a fenti paraméterek alapján biztosít.</span><span class="sxs-lookup"><span data-stu-id="6bf59-188">The script below provides a sample configuration based on the topology and parameters listed above.</span></span> <span data-ttu-id="6bf59-189">Az S2S VPN-alagút konfiguráció a következő részből áll:</span><span class="sxs-lookup"><span data-stu-id="6bf59-189">The S2S VPN tunnel configuration consists of the following parts:</span></span>

1. <span data-ttu-id="6bf59-190">Felületek & útvonalak</span><span class="sxs-lookup"><span data-stu-id="6bf59-190">Interfaces & routes</span></span>
2. <span data-ttu-id="6bf59-191">Hozzáférés-listák</span><span class="sxs-lookup"><span data-stu-id="6bf59-191">Access lists</span></span>
3. <span data-ttu-id="6bf59-192">IKE-házirend és a paraméterek (1. fázis vagy alapmódú)</span><span class="sxs-lookup"><span data-stu-id="6bf59-192">IKE policy and parameters (Phase 1 or Main Mode)</span></span>
4. <span data-ttu-id="6bf59-193">IPSec-házirend és a paraméterek (2. szakasza vagy gyorsmódú)</span><span class="sxs-lookup"><span data-stu-id="6bf59-193">IPsec policy and parameters (Phase 2 or Quick Mode)</span></span>
5. <span data-ttu-id="6bf59-194">Más paramétereket (TCP MSS hántoló, stb.)</span><span class="sxs-lookup"><span data-stu-id="6bf59-194">Other parameters (TCP MSS clamping, etc.)</span></span>

>[!IMPORTANT] 
><span data-ttu-id="6bf59-195">Győződjön meg arról, hogy az alább felsorolt, a további konfigurálás elvégzésére, és cserélje le a helyőrzőket a tényleges értékek:</span><span class="sxs-lookup"><span data-stu-id="6bf59-195">Please make sure you complete the additional configuration listed below and replace the placeholders with the actual values:</span></span>
> 
> - <span data-ttu-id="6bf59-196">Kapcsolat az illesztők kívül és belül is konfigurációja</span><span class="sxs-lookup"><span data-stu-id="6bf59-196">Interface configuration for both inside and outside interfaces</span></span>
> - <span data-ttu-id="6bf59-197">A vállalaton belüli és személyes és a külső/nyilvános hálózatokhoz útvonalak</span><span class="sxs-lookup"><span data-stu-id="6bf59-197">Routes for your inside/private and outside/public networks</span></span>
> - <span data-ttu-id="6bf59-198">Biztosítsa, hogy az összes nevét és házirend számok egyedi az eszközön</span><span class="sxs-lookup"><span data-stu-id="6bf59-198">Ensure all names and policy numbers are unique on the device</span></span>
> - <span data-ttu-id="6bf59-199">Győződjön meg arról, a titkosítási algoritmusok támogatottak az eszközön</span><span class="sxs-lookup"><span data-stu-id="6bf59-199">Ensure the cryptographic algorithms are supported on your device</span></span>
> - <span data-ttu-id="6bf59-200">Cserélje le a következő helyen tartozó felhasználók számára a tényleges értékek</span><span class="sxs-lookup"><span data-stu-id="6bf59-200">Replace the following place holders with the actual values</span></span>
>   - <span data-ttu-id="6bf59-201">A kapcsolat nevét kívül: "külső"</span><span class="sxs-lookup"><span data-stu-id="6bf59-201">Outside interface name: "outside"</span></span>
>   - <span data-ttu-id="6bf59-202">Azure_Gateway_Public_IP</span><span class="sxs-lookup"><span data-stu-id="6bf59-202">Azure_Gateway_Public_IP</span></span>
>   - <span data-ttu-id="6bf59-203">OnPrem_Device_Public_IP</span><span class="sxs-lookup"><span data-stu-id="6bf59-203">OnPrem_Device_Public_IP</span></span>
>   - <span data-ttu-id="6bf59-204">IKE Pre_Shared_Key</span><span class="sxs-lookup"><span data-stu-id="6bf59-204">IKE Pre_Shared_Key</span></span>
>   - <span data-ttu-id="6bf59-205">Virtuális hálózat és a helyi hálózati átjáró nevek (VNetName, LNGName)</span><span class="sxs-lookup"><span data-stu-id="6bf59-205">VNet and local network gateway names (VNetName, LNGName)</span></span>
>   - <span data-ttu-id="6bf59-206">Virtuális hálózat és a helyszíni hálózati címelőtagokat</span><span class="sxs-lookup"><span data-stu-id="6bf59-206">VNet and on-premises network address prefixes</span></span>
>   - <span data-ttu-id="6bf59-207">Megfelelő netmasks</span><span class="sxs-lookup"><span data-stu-id="6bf59-207">Proper netmasks</span></span>

#### <a name="sample-configuration"></a><span data-ttu-id="6bf59-208">Mintakonfiguráció</span><span class="sxs-lookup"><span data-stu-id="6bf59-208">Sample configuration</span></span>

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec protposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a><span data-ttu-id="6bf59-209">Egyszerű hibakeresési parancsok</span><span class="sxs-lookup"><span data-stu-id="6bf59-209">Simple debugging commands</span></span>

<span data-ttu-id="6bf59-210">Az alábbiakban néhány ASA parancsok hibakeresési célra:</span><span class="sxs-lookup"><span data-stu-id="6bf59-210">Here are some ASA commands for debugging purposes:</span></span>

1. <span data-ttu-id="6bf59-211">Az IPsec és az internetes KULCSCSERE SA megjelenítése</span><span class="sxs-lookup"><span data-stu-id="6bf59-211">Show the IPsec and IKE SA's</span></span>
    - <span data-ttu-id="6bf59-212">"megjelenítése titkosítási ipsec biztonsági társítás"</span><span class="sxs-lookup"><span data-stu-id="6bf59-212">"show crypto ipsec sa"</span></span>
    - <span data-ttu-id="6bf59-213">"titkosítási megjelenítése ikev2 rendszergazdai (SA)"</span><span class="sxs-lookup"><span data-stu-id="6bf59-213">"show crypto ikev2 sa"</span></span>
2. <span data-ttu-id="6bf59-214">Belépés hibakeresési mód – ez kaphat nagyon zajos a konzolon</span><span class="sxs-lookup"><span data-stu-id="6bf59-214">Entering debug mode - this can get very noisy on the console</span></span>
    - <span data-ttu-id="6bf59-215">"hibakeresési titkosítási ikev2 platform <level>"</span><span class="sxs-lookup"><span data-stu-id="6bf59-215">"debug crypto ikev2 platform <level>"</span></span>
    - <span data-ttu-id="6bf59-216">"titkosítási ikev2 protokoll debug <level>"</span><span class="sxs-lookup"><span data-stu-id="6bf59-216">"debug crypto ikev2 protocol <level>"</span></span>
3. <span data-ttu-id="6bf59-217">Lista aktuális konfigurációk</span><span class="sxs-lookup"><span data-stu-id="6bf59-217">List current configurations</span></span>
    - <span data-ttu-id="6bf59-218">"futtatása show" – aktuális konfigurációk láthatók az eszközön; a konfigurációs listát meghatározott részeit a különféle alárendelt parancsok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6bf59-218">"show run" - shows the current configurations on the device; you can use the various sub-commands to list specific parts of the configuration.</span></span> <span data-ttu-id="6bf59-219">Például "futtatása megjelenítése titkosítási", "Futtatás megjelenítése access-list", "Futtatás megjelenítése alagút-csoport" stb.</span><span class="sxs-lookup"><span data-stu-id="6bf59-219">E.g., "show run crypto", "show run access-list", "show run tunnel-group", etc.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6bf59-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6bf59-220">Next steps</span></span>
<span data-ttu-id="6bf59-221">A létesítmények közötti és a virtuális hálózatok közötti aktív-aktív kapcsolatok konfigurálásának lépéseit lásd: [Aktív-aktív VPN Gateway-átjárók konfigurálása létesítmények közötti és virtuális hálózatok közötti kapcsolatokhoz](vpn-gateway-activeactive-rm-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6bf59-221">See [Configuring Active-Active VPN Gateways for Cross-Premises and VNet-to-VNet Connections](vpn-gateway-activeactive-rm-powershell.md) for steps to configure active-active cross-premises and VNet-to-VNet connections.</span></span>

