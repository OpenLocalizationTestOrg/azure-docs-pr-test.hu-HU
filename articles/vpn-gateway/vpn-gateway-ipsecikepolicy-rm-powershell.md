---
title: "S2S VPN- és VNet – VNet kapcsolatokhoz IPsec/IKE-házirend konfigurálása: Azure Resource Manager: PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan S2S és VNet – VNet kapcsolatokhoz IPsec/IKE-házirend konfigurálása az Azure VPN Gatewayek Azure Resource Manager és a PowerShell használatával."
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
ms.date: 05/12/2017
ms.author: yushwang
ms.openlocfilehash: 798014b6e8d4495db99ef2e2d2ea487ae7d02fd0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a><span data-ttu-id="ee23e-103">S2S VPN- és VNet – VNet kapcsolatokhoz IPsec/IKE-házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ee23e-103">Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections</span></span>

<span data-ttu-id="ee23e-104">Ez a cikk végigvezeti a telephelyek közötti VPN- és VNet – VNet kapcsolatokhoz, a Resource Manager üzembe helyezési modellben és a PowerShell használatával IPsec/IKE-házirendet konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="ee23e-104">This article walks you through the steps to configure IPsec/IKE policy for Site-to-Site VPN or VNet-to-VNet connections using the Resource Manager deployment model and PowerShell.</span></span>

## <span data-ttu-id="ee23e-105"><a name="about"></a>Az Azure VPN gatewayek IPsec és az internetes KULCSCSERE házirend paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee23e-105"><a name="about"></a>About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="ee23e-106">Standard IPsec és IKE protokoll titkosítási algoritmusok számos különböző kombinációkban támogatja.</span><span class="sxs-lookup"><span data-stu-id="ee23e-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="ee23e-107">Tekintse meg [kriptográfiai követelményeiről és az Azure VPN gatewayek](vpn-gateway-about-compliance-crypto.md) hogyan Ez segítségére lehet biztosítása létesítmények közötti és VNet – VNet-kapcsolatot megfelelnek a megfelelőségi és biztonsági.</span><span class="sxs-lookup"><span data-stu-id="ee23e-107">Refer to [About cryptographic requirements and Azure VPN gateways](vpn-gateway-about-compliance-crypto.md) to see how this can help ensuring cross-premises and VNet-to-VNet connectivity satisfy your compliance or security requirements.</span></span>

<span data-ttu-id="ee23e-108">Ez a cikk bemutatja, és hozhat létre és IPsec/IKE-szabályzat beállítása egy új vagy meglévő kapcsolat alkalmazható:</span><span class="sxs-lookup"><span data-stu-id="ee23e-108">This article provides instructions to create and configure an IPsec/IKE policy and apply to a new or existing connection:</span></span>

* [<span data-ttu-id="ee23e-109">1 - munkafolyamat létrehozása és IPsec/IKE házirend beállítása. rész</span><span class="sxs-lookup"><span data-stu-id="ee23e-109">Part 1 - Workflow to create and set IPsec/IKE policy</span></span>](#workflow)
* [<span data-ttu-id="ee23e-110">2. rész - támogatott titkosítási algoritmusok és a kulcs szintjeiről</span><span class="sxs-lookup"><span data-stu-id="ee23e-110">Part 2 - Supported cryptographic algorithms and key strengths</span></span>](#params)
* [<span data-ttu-id="ee23e-111">3. rész – hozzon létre egy új S2S VPN-kapcsolat IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="ee23e-111">Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>](#crossprem)
* [<span data-ttu-id="ee23e-112">Rész 4 – hozzon létre egy új VNet – VNet-kapcsolatot IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="ee23e-112">Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>](#vnet2vnet)
* [<span data-ttu-id="ee23e-113">5 - rész kezelése (létrehozása, hozzáadása, eltávolítása) kapcsolat IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="ee23e-113">Part 5 - Manage (create, add, remove) IPsec/IKE policy for a connection</span></span>](#managepolicy)

> [!IMPORTANT]
> 1. <span data-ttu-id="ee23e-114">Vegye figyelembe, hogy IPsec/IKE házirend csak akkor működik a a következő átjáró-termékváltozat:</span><span class="sxs-lookup"><span data-stu-id="ee23e-114">Note that IPsec/IKE policy only works on the following gateway SKUs:</span></span>
>    * <span data-ttu-id="ee23e-115">***VpnGw1, VpnGw2, VpnGw3*** (útválasztó-alapú)</span><span class="sxs-lookup"><span data-stu-id="ee23e-115">***VpnGw1, VpnGw2, VpnGw3*** (route-based)</span></span>
>    * <span data-ttu-id="ee23e-116">***Standard*** és ***HighPerformance*** (útválasztó-alapú)</span><span class="sxs-lookup"><span data-stu-id="ee23e-116">***Standard*** and ***HighPerformance*** (route-based)</span></span>
> 2. <span data-ttu-id="ee23e-117">Egy adott kapcsolathoz csak ***egy*** házirendet adhat meg.</span><span class="sxs-lookup"><span data-stu-id="ee23e-117">You can only specify ***one*** policy combination for a given connection.</span></span>
> 3. <span data-ttu-id="ee23e-118">Meg kell adnia a internetes KULCSCSERE (alapmód) és a IPsec (gyorsmódú) algoritmusok és a paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ee23e-118">You must specify all algorithms and parameters for both IKE (Main Mode) and IPsec (Quick Mode).</span></span> <span data-ttu-id="ee23e-119">A részleges házirend-megadás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="ee23e-119">Partial policy specification is not allowed.</span></span>
> 4. <span data-ttu-id="ee23e-120">Vegye fel a kapcsolatot VPN szállító műszaki győződjön meg arról, a házirend a helyszíni VPN-eszközök esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="ee23e-120">Consult with your VPN device vendor specifications to ensure the policy is supported on your on-premises VPN devices.</span></span> <span data-ttu-id="ee23e-121">S2S vagy a VNet – VNet kapcsolatokhoz a nem tud, ha a házirendek nem kompatibilisek.</span><span class="sxs-lookup"><span data-stu-id="ee23e-121">S2S or VNet-to-VNet connections cannot establish if the policies are incompatible.</span></span>

## <span data-ttu-id="ee23e-122"><a name ="workflow"></a>1 - munkafolyamat létrehozása és IPsec/IKE házirend beállítása. rész</span><span class="sxs-lookup"><span data-stu-id="ee23e-122"><a name ="workflow"></a>Part 1 - Workflow to create and set IPsec/IKE policy</span></span>
<span data-ttu-id="ee23e-123">Ez a szakasz ismerteti a munkafolyamat létrehozásához, és a S2S VPN- vagy a VNet – VNet kapcsolat IPsec/IKE-házirend frissítése:</span><span class="sxs-lookup"><span data-stu-id="ee23e-123">This section outlines the workflow to create and update IPsec/IKE policy on a S2S VPN or VNet-to-VNet connection:</span></span>
1. <span data-ttu-id="ee23e-124">Virtuális hálózat és VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-124">Create a virtual network and a VPN gateway</span></span>
2. <span data-ttu-id="ee23e-125">Helyi hálózati átjáró a helyi kapcsolat vagy egy másik virtuális hálózati közötti és VNet – VNet-kapcsolatot az átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-125">Create a local network gateway for cross premises connection, or another virtual network and gateway for VNet-to-VNet connection</span></span>
3. <span data-ttu-id="ee23e-126">Hozzon létre egy IPsec/IKE házirendet a kijelölt algoritmusok és paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee23e-126">Create an IPsec/IKE policy with selected algorithms and parameters</span></span>
4. <span data-ttu-id="ee23e-127">Az IPsec/IKE házirendet (IPsec vagy VNet2VNet) kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-127">Create a connection (IPsec or VNet2VNet) with the IPsec/IKE policy</span></span>
5. <span data-ttu-id="ee23e-128">Frissítés/hozzáadása egy meglévő kapcsolat az IPsec/IKE házirend</span><span class="sxs-lookup"><span data-stu-id="ee23e-128">Add/update/remove an IPsec/IKE policy for an existing connection</span></span>

<span data-ttu-id="ee23e-129">A jelen cikkben lévő utasítások segít beállítása és konfigurálása IPsec/IKE házirendek az ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="ee23e-129">The instructions in this article helps you set up and configure IPsec/IKE policies as shown in the diagram:</span></span>

![IPSec-ike-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <span data-ttu-id="ee23e-131"><a name ="params"></a>2. rész - támogatott titkosítási algoritmusok és a kulcs szintjeiről</span><span class="sxs-lookup"><span data-stu-id="ee23e-131"><a name ="params"></a>Part 2 - Supported cryptographic algorithms & key strengths</span></span>

<span data-ttu-id="ee23e-132">Az alábbi táblázat a támogatott titkosítási algoritmusok és a kulcs szintjeiről konfigurálható az ügyfelek:</span><span class="sxs-lookup"><span data-stu-id="ee23e-132">The following table lists the supported cryptographic algorithms and key strengths configurable by the customers:</span></span>

| <span data-ttu-id="ee23e-133">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="ee23e-133">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="ee23e-134">**Beállítások**</span><span class="sxs-lookup"><span data-stu-id="ee23e-134">**Options**</span></span>    |
| ---  | --- 
| <span data-ttu-id="ee23e-135">IKEv2-titkosítás</span><span class="sxs-lookup"><span data-stu-id="ee23e-135">IKEv2 Encryption</span></span> | <span data-ttu-id="ee23e-136">AES256, AES192, AES128, DES3, DES</span><span class="sxs-lookup"><span data-stu-id="ee23e-136">AES256, AES192, AES128, DES3, DES</span></span>  
| <span data-ttu-id="ee23e-137">IKEv2-integritás</span><span class="sxs-lookup"><span data-stu-id="ee23e-137">IKEv2 Integrity</span></span>  | <span data-ttu-id="ee23e-138">SHA384, MD5, SHA1, SHA256</span><span class="sxs-lookup"><span data-stu-id="ee23e-138">SHA384, SHA256, SHA1, MD5</span></span>  |
| <span data-ttu-id="ee23e-139">DH-csoport</span><span class="sxs-lookup"><span data-stu-id="ee23e-139">DH Group</span></span>         | <span data-ttu-id="ee23e-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span><span class="sxs-lookup"><span data-stu-id="ee23e-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span></span> |
| <span data-ttu-id="ee23e-141">IPsec-titkosítás</span><span class="sxs-lookup"><span data-stu-id="ee23e-141">IPsec Encryption</span></span> | <span data-ttu-id="ee23e-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, Nincs</span><span class="sxs-lookup"><span data-stu-id="ee23e-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None</span></span>    |
| <span data-ttu-id="ee23e-143">IPsec-integritás</span><span class="sxs-lookup"><span data-stu-id="ee23e-143">IPsec Integrity</span></span>  | <span data-ttu-id="ee23e-144">GCMASE256, GCMAES192, GCMAES128, SHA-256, SHA1, MD5</span><span class="sxs-lookup"><span data-stu-id="ee23e-144">GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5</span></span> |
| <span data-ttu-id="ee23e-145">PFS-csoport</span><span class="sxs-lookup"><span data-stu-id="ee23e-145">PFS Group</span></span>        | <span data-ttu-id="ee23e-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Nincs</span><span class="sxs-lookup"><span data-stu-id="ee23e-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None</span></span> 
| <span data-ttu-id="ee23e-147">Gyorsmódú biztonsági társítás élettartama</span><span class="sxs-lookup"><span data-stu-id="ee23e-147">QM SA Lifetime</span></span>   | <span data-ttu-id="ee23e-148">(**Nem kötelező**: alapértelmezett értékek vannak használt Ha nincs megadva)</span><span class="sxs-lookup"><span data-stu-id="ee23e-148">(**Optional**: default values are used if not specified)</span></span><br><span data-ttu-id="ee23e-149">Másodperc (egész szám; **min. 300**/alapértelmezett érték: 27000 másodperc)</span><span class="sxs-lookup"><span data-stu-id="ee23e-149">Seconds (integer; **min. 300**/default 27000 seconds)</span></span><br><span data-ttu-id="ee23e-150">KB (egész szám; **min. 1024**/alapértelmezett érték: 102400000 KB)</span><span class="sxs-lookup"><span data-stu-id="ee23e-150">KBytes (integer; **min. 1024**/default 102400000 KBytes)</span></span>   |
| <span data-ttu-id="ee23e-151">Forgalomválasztó</span><span class="sxs-lookup"><span data-stu-id="ee23e-151">Traffic Selector</span></span> | <span data-ttu-id="ee23e-152">UsePolicyBasedTrafficSelectors ** ($True vagy $False; **Nem kötelező**, alapértelmezett $False Ha nincs megadva)</span><span class="sxs-lookup"><span data-stu-id="ee23e-152">UsePolicyBasedTrafficSelectors** ($True/$False; **Optional**, default $False if not specified)</span></span>    |
|  |  |

> [!IMPORTANT]
> 1. <span data-ttu-id="ee23e-153">**GCMAES mint IPsec titkosítási algoritmus használata esetén ki kell választania a azonos GCMAES algoritmus és a kulcshossz IPsec sértetlenségét; például mind a GCMAES128 használatával**</span><span class="sxs-lookup"><span data-stu-id="ee23e-153">**If GCMAES is used as for IPsec Encryption algorithm, you must select the same GCMAES algorithm and key length for IPsec Integrity; for example, using GCMAES128 for both**</span></span>
> 2. <span data-ttu-id="ee23e-154">Az IKEv2 fő módú biztonsági hozzárendelés élettartama 28 800 másodpercen van rögzítve az Azure VPN-átjárókon</span><span class="sxs-lookup"><span data-stu-id="ee23e-154">IKEv2 Main Mode SA lifetime is fixed at 28,800 seconds on the Azure VPN gateways</span></span>
> 3. <span data-ttu-id="ee23e-155">$True "UsePolicyBasedTrafficSelectors" beállítást a kapcsolat konfigurálja az Azure VPN gateway házirendalapú VPN-tűzfal a helyszíni való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="ee23e-155">Setting "UsePolicyBasedTrafficSelectors" to $True on a connection will configure the Azure VPN gateway to connect to policy-based VPN firewall on premises.</span></span> <span data-ttu-id="ee23e-156">Ha engedélyezi a PolicyBasedTrafficSelectors, szeretné-e ellenőrizze a megfelelő forgalmat választók meghatározott összes kiegészítve a helyszíni hálózati belőle az Azure-beli virtuális hálózat előtagok (helyi hálózati átjáró) előtagok ahelyett, hogy rendelkezik-e a VPN-eszköz bármely elem közöttiként.</span><span class="sxs-lookup"><span data-stu-id="ee23e-156">If you enable PolicyBasedTrafficSelectors, you need to ensure your VPN device has the matching traffic selectors defined with all combinations of your on-premises network (local network gateway) prefixes to/from the Azure virtual network prefixes, instead of any-to-any.</span></span> <span data-ttu-id="ee23e-157">Például ha a helyszíni hálózati előtagok a 10.1.0.0/16 és a 10.2.0.0/16, a virtuális hálózati előtagok pedig 192.168.0.0/16 és 172.16.0.0/16, az alábbi forgalomválasztókat kell megadnia:</span><span class="sxs-lookup"><span data-stu-id="ee23e-157">For example, if your on-premises network prefixes are 10.1.0.0/16 and 10.2.0.0/16, and your virtual network prefixes are 192.168.0.0/16 and 172.16.0.0/16, you need to specify the following traffic selectors:</span></span>
>    * <span data-ttu-id="ee23e-158">10.1.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ee23e-158">10.1.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="ee23e-159">10.1.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ee23e-159">10.1.0.0/16 <====> 172.16.0.0/16</span></span>
>    * <span data-ttu-id="ee23e-160">10.2.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ee23e-160">10.2.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="ee23e-161">10.2.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ee23e-161">10.2.0.0/16 <====> 172.16.0.0/16</span></span>

<span data-ttu-id="ee23e-162">Csoportházirend-alapú forgalom választók kapcsolatos további információkért lásd: [csatlakozás több helyszíni házirendalapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ee23e-162">For more information regarding policy-based traffic selectors, see [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>

<span data-ttu-id="ee23e-163">A következő táblázat felsorolja a megfelelő Diffie-Hellman csoport, az egyéni házirend által támogatott:</span><span class="sxs-lookup"><span data-stu-id="ee23e-163">The following table lists the corresponding Diffie-Hellman Groups supported by the custom policy:</span></span>

| <span data-ttu-id="ee23e-164">**Diffie-Hellman csoport**</span><span class="sxs-lookup"><span data-stu-id="ee23e-164">**Diffie-Hellman Group**</span></span>  | <span data-ttu-id="ee23e-165">**DH-csoport**</span><span class="sxs-lookup"><span data-stu-id="ee23e-165">**DHGroup**</span></span>              | <span data-ttu-id="ee23e-166">**PFS-csoport**</span><span class="sxs-lookup"><span data-stu-id="ee23e-166">**PFSGroup**</span></span> | <span data-ttu-id="ee23e-167">**A kulcs hossza**</span><span class="sxs-lookup"><span data-stu-id="ee23e-167">**Key length**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ee23e-168">1</span><span class="sxs-lookup"><span data-stu-id="ee23e-168">1</span></span>                         | <span data-ttu-id="ee23e-169">DHGroup1</span><span class="sxs-lookup"><span data-stu-id="ee23e-169">DHGroup1</span></span>                 | <span data-ttu-id="ee23e-170">PFS1</span><span class="sxs-lookup"><span data-stu-id="ee23e-170">PFS1</span></span>         | <span data-ttu-id="ee23e-171">768 bites MODP</span><span class="sxs-lookup"><span data-stu-id="ee23e-171">768-bit MODP</span></span>   |
| <span data-ttu-id="ee23e-172">2</span><span class="sxs-lookup"><span data-stu-id="ee23e-172">2</span></span>                         | <span data-ttu-id="ee23e-173">DHGroup2</span><span class="sxs-lookup"><span data-stu-id="ee23e-173">DHGroup2</span></span>                 | <span data-ttu-id="ee23e-174">PFS2</span><span class="sxs-lookup"><span data-stu-id="ee23e-174">PFS2</span></span>         | <span data-ttu-id="ee23e-175">1024 bites MODP</span><span class="sxs-lookup"><span data-stu-id="ee23e-175">1024-bit MODP</span></span>  |
| <span data-ttu-id="ee23e-176">14</span><span class="sxs-lookup"><span data-stu-id="ee23e-176">14</span></span>                        | <span data-ttu-id="ee23e-177">DHGroup14</span><span class="sxs-lookup"><span data-stu-id="ee23e-177">DHGroup14</span></span><br><span data-ttu-id="ee23e-178">DHGroup2048</span><span class="sxs-lookup"><span data-stu-id="ee23e-178">DHGroup2048</span></span> | <span data-ttu-id="ee23e-179">PFS2048</span><span class="sxs-lookup"><span data-stu-id="ee23e-179">PFS2048</span></span>      | <span data-ttu-id="ee23e-180">2048 bites MODP</span><span class="sxs-lookup"><span data-stu-id="ee23e-180">2048-bit MODP</span></span>  |
| <span data-ttu-id="ee23e-181">19</span><span class="sxs-lookup"><span data-stu-id="ee23e-181">19</span></span>                        | <span data-ttu-id="ee23e-182">ECP256</span><span class="sxs-lookup"><span data-stu-id="ee23e-182">ECP256</span></span>                   | <span data-ttu-id="ee23e-183">ECP256</span><span class="sxs-lookup"><span data-stu-id="ee23e-183">ECP256</span></span>       | <span data-ttu-id="ee23e-184">256 bites ECP</span><span class="sxs-lookup"><span data-stu-id="ee23e-184">256-bit ECP</span></span>    |
| <span data-ttu-id="ee23e-185">20</span><span class="sxs-lookup"><span data-stu-id="ee23e-185">20</span></span>                        | <span data-ttu-id="ee23e-186">ECP384</span><span class="sxs-lookup"><span data-stu-id="ee23e-186">ECP384</span></span>                   | <span data-ttu-id="ee23e-187">ECP284</span><span class="sxs-lookup"><span data-stu-id="ee23e-187">ECP284</span></span>       | <span data-ttu-id="ee23e-188">384 bites ECP</span><span class="sxs-lookup"><span data-stu-id="ee23e-188">384-bit ECP</span></span>    |
| <span data-ttu-id="ee23e-189">24</span><span class="sxs-lookup"><span data-stu-id="ee23e-189">24</span></span>                        | <span data-ttu-id="ee23e-190">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="ee23e-190">DHGroup24</span></span>                | <span data-ttu-id="ee23e-191">PFS24</span><span class="sxs-lookup"><span data-stu-id="ee23e-191">PFS24</span></span>        | <span data-ttu-id="ee23e-192">2048 bites MODP</span><span class="sxs-lookup"><span data-stu-id="ee23e-192">2048-bit MODP</span></span>  |

<span data-ttu-id="ee23e-193">További részletekért lásd: [RFC3526](https://tools.ietf.org/html/rfc3526) és [RFC5114](https://tools.ietf.org/html/rfc5114).</span><span class="sxs-lookup"><span data-stu-id="ee23e-193">Refer to [RFC3526](https://tools.ietf.org/html/rfc3526) and [RFC5114](https://tools.ietf.org/html/rfc5114) for more details.</span></span>

## <span data-ttu-id="ee23e-194"><a name ="crossprem"></a>3. rész – hozzon létre egy új S2S VPN-kapcsolat IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="ee23e-194"><a name ="crossprem"></a>Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>

<span data-ttu-id="ee23e-195">Ez a szakasz bemutatja, hogyan hozzon létre egy S2S VPN-kapcsolatot az IPsec/IKE házirendjével.</span><span class="sxs-lookup"><span data-stu-id="ee23e-195">This section walks you through the steps of creating a S2S VPN connection with an IPsec/IKE policy.</span></span> <span data-ttu-id="ee23e-196">Az alábbi lépéseket a kapcsolat létrehozása, az ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="ee23e-196">The following steps create the connection as shown in the diagram:</span></span>

![s2s-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

<span data-ttu-id="ee23e-198">Lásd: [S2S VPN-kapcsolatot](vpn-gateway-create-site-to-site-rm-powershell.md) részletesebb lépésenkénti S2S VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ee23e-198">See [Create a S2S VPN connection](vpn-gateway-create-site-to-site-rm-powershell.md) for more detailed step-by-step instructions for creating a S2S VPN connection.</span></span>

### <span data-ttu-id="ee23e-199"><a name="before"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ee23e-199"><a name="before"></a>Before you begin</span></span>

* <span data-ttu-id="ee23e-200">Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="ee23e-200">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="ee23e-201">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ee23e-201">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ee23e-202">Az Azure Resource Manager PowerShell-parancsmagjainak telepítése.</span><span class="sxs-lookup"><span data-stu-id="ee23e-202">Install the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="ee23e-203">Lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview) a PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="ee23e-203">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

### <span data-ttu-id="ee23e-204"><a name="createvnet1"></a>1. lépés – a virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-204"><a name="createvnet1"></a>Step 1 - Create the virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="ee23e-205">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="ee23e-205">1. Declare your variables</span></span>

<span data-ttu-id="ee23e-206">Ehhez a gyakorlathoz először is deklarálni kell a változókat.</span><span class="sxs-lookup"><span data-stu-id="ee23e-206">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="ee23e-207">Az éles konfigurációhoz ne felejtse el ezeket az értékeket a saját értékeire cserélni.</span><span class="sxs-lookup"><span data-stu-id="ee23e-207">Be sure to replace the values with your own when configuring for production.</span></span>

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="ee23e-208">2. Csatlakozás az előfizetéshez, és hozzon létre egy új erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="ee23e-208">2. Connect to your subscription and create a new resource group</span></span>

<span data-ttu-id="ee23e-209">A Resource Manager parancsmagjainak használatához váltson át PowerShell módba.</span><span class="sxs-lookup"><span data-stu-id="ee23e-209">Make sure you switch to PowerShell mode to use the Resource Manager cmdlets.</span></span> <span data-ttu-id="ee23e-210">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="ee23e-210">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="ee23e-211">Nyissa meg a PowerShell konzolt, és csatlakozzon a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="ee23e-211">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="ee23e-212">A következő minta segíthet a kapcsolódásban:</span><span class="sxs-lookup"><span data-stu-id="ee23e-212">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="ee23e-213">3. A virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-213">3. Create the virtual network, VPN gateway, and local network gateway</span></span>

<span data-ttu-id="ee23e-214">Az alábbi minta létrehoz a virtuális hálózat, TestVNet1 három alhálózatokon, és a VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="ee23e-214">The following sample creates the virtual network, TestVNet1, with three subnets, and the VPN gateway.</span></span> <span data-ttu-id="ee23e-215">Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen.</span><span class="sxs-lookup"><span data-stu-id="ee23e-215">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="ee23e-216">Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="ee23e-216">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <span data-ttu-id="ee23e-217"><a name="s2sconnection"></a>2. lépés - az IPsec/IKE házirendjével S2S VPN-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-217"><a name="s2sconnection"></a>Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="ee23e-218">1. IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-218">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="ee23e-219">Az alábbi mintaparancsfájl házirendet hoz létre IPsec/IKE a következő algoritmusokat és a Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ee23e-219">The following sample script creates an IPsec/IKE policy with the following algorithms and parameters:</span></span>

* <span data-ttu-id="ee23e-220">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="ee23e-220">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="ee23e-221">IPsec: AES256, SHA-256, PFS24, 2048KB & SA élettartama 7200 másodperc</span><span class="sxs-lookup"><span data-stu-id="ee23e-221">IPsec: AES256, SHA256, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

<span data-ttu-id="ee23e-222">Ha GCMAES használja az IPsec, kell használnia az azonos GCMAES algoritmus és a kulcshossz IPsec titkosításhoz és integritását, például:</span><span class="sxs-lookup"><span data-stu-id="ee23e-222">If you use GCMAES for IPsec, you must use the same GCMAES algorithm and key length for both IPsec encryption and integrity, for example:</span></span>

* <span data-ttu-id="ee23e-223">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="ee23e-223">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="ee23e-224">IPsec: **GCMAES256, GCMAES256**, PFS24, 2048 KB & SA élettartama 7200 másodperc</span><span class="sxs-lookup"><span data-stu-id="ee23e-224">IPsec: **GCMAES256, GCMAES256**, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

#### <a name="2-create-the-s2s-vpn-connection-with-the-ipsecike-policy"></a><span data-ttu-id="ee23e-225">2. Az S2S VPN-kapcsolat létrehozása az IPsec/IKE irányelvnek</span><span class="sxs-lookup"><span data-stu-id="ee23e-225">2. Create the S2S VPN connection with the IPsec/IKE policy</span></span>

<span data-ttu-id="ee23e-226">S2S VPN-kapcsolat létrehozásához, és alkalmazza a korábban létrehozott IPsec/IKE-házirendet.</span><span class="sxs-lookup"><span data-stu-id="ee23e-226">Create an S2S VPN connection and apply the IPsec/IKE policy created earlier.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="ee23e-227">Opcionálisan hozzáadhat "-UsePolicyBasedTrafficSelectors $True" a létrehozás kapcsolat parancsmagnak Azure VPN-átjáró házirendalapú VPN-eszközök a helyszínen, csatlakozni engedélyezése a fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="ee23e-227">You can optionally add "-UsePolicyBasedTrafficSelectors $True" to the create connection cmdlet to enable Azure VPN gateway to connect to policy-based VPN devices on premises, as described above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee23e-228">Miután egy IPsec-/ h.rend kapcsolat van megadva, az Azure VPN gateway csak elküldi vagy elfogadja a IPsec/IKE-a megadott titkosítási algoritmusok és a kulcs szintjeiről a adott kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ee23e-228">Once an IPsec/IKE policy is specified on a connection, the Azure VPN gateway will only send or accept the IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="ee23e-229">Győződjön meg arról, hogy a kapcsolat a helyszíni VPN-eszköz használ, vagy fogadja el a pontos házirend kombináció, ellenkező esetben az S2S VPN-alagút fog létrehozni a.</span><span class="sxs-lookup"><span data-stu-id="ee23e-229">Make sure your on-premises VPN device for the connection uses or accepts the exact policy combination, otherwise the S2S VPN tunnel will not establish.</span></span>


## <span data-ttu-id="ee23e-230"><a name ="vnet2vnet"></a>Rész 4 – hozzon létre egy új VNet – VNet-kapcsolatot IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="ee23e-230"><a name ="vnet2vnet"></a>Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>

<span data-ttu-id="ee23e-231">Hozzon létre egy VNet – VNet-kapcsolatot az IPsec/IKE házirendjével hasonlóak az S2S VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ee23e-231">The steps of creating a VNet-to-VNet connection with an IPsec/IKE policy are similar to that of a S2S VPN connection.</span></span> <span data-ttu-id="ee23e-232">A következő minta parancsfájlokat hozza létre a kapcsolat, az ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="ee23e-232">The following sample scripts create the connection as shown in the diagram:</span></span>

![v2v-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

<span data-ttu-id="ee23e-234">Lásd: [VNet – VNet-kapcsolatot](vpn-gateway-vnet-vnet-rm-ps.md) részletes lépéseket a VNet – VNet-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ee23e-234">See [Create a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) for more detailed steps for creating a VNet-to-VNet connection.</span></span> <span data-ttu-id="ee23e-235">Meg kell adnia a [3. rész](#crossprem) létrehozása és TestVNet1 és a VPN-átjáró konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ee23e-235">You must complete [Part 3](#crossprem) to create and configure TestVNet1 and the VPN Gateway.</span></span>

### <span data-ttu-id="ee23e-236"><a name="createvnet2"></a>1. lépés – a második virtuális hálózat és a VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-236"><a name="createvnet2"></a>Step 1 - Create the second virtual network and VPN gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="ee23e-237">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="ee23e-237">1. Declare your variables</span></span>

<span data-ttu-id="ee23e-238">Ne felejtse el az értékeket olyanokra cserélni, amelyeket a saját konfigurációjához kíván használni.</span><span class="sxs-lookup"><span data-stu-id="ee23e-238">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-the-second-virtual-network-and-vpn-gateway-in-the-new-resource-group"></a><span data-ttu-id="ee23e-239">2. A második virtuális hálózat és a VPN-átjárót az új erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-239">2. Create the second virtual network and VPN gateway in the new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-the-ipsecike-policy"></a><span data-ttu-id="ee23e-240">2. lépés - a VNet-toVNet kapcsolatot létrehozni az IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="ee23e-240">Step 2 - Create a VNet-toVNet connection with the IPsec/IKE policy</span></span>

<span data-ttu-id="ee23e-241">Hasonló a S2S VPN-kapcsolat IPsec/IKE-házirend létrehozása, akkor a házirend az új kapcsolat alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="ee23e-241">Similar to the S2S VPN connection, create an IPsec/IKE policy then apply to policy to the new connection.</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="ee23e-242">1. IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-242">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="ee23e-243">Az alábbi mintaparancsfájl egy másik IPsec/IKE-házirendet hoz létre a következő algoritmusokat és a Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ee23e-243">The following sample script creates a different IPsec/IKE policy with the following algorithms and parameters:</span></span>
* <span data-ttu-id="ee23e-244">IKEv2: Az AES128, SHA1, DHGroup14</span><span class="sxs-lookup"><span data-stu-id="ee23e-244">IKEv2: AES128, SHA1, DHGroup14</span></span>
* <span data-ttu-id="ee23e-245">IPsec: GCMAES128, GCMAES128, PFS14, SA élettartama 7200 másodperc & 4096KB</span><span class="sxs-lookup"><span data-stu-id="ee23e-245">IPsec: GCMAES128, GCMAES128, PFS14, SA Lifetime 7200 seconds & 4096KB</span></span>

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 4096
```

#### <a name="2-create-vnet-to-vnet-connections-with-the-ipsecike-policy"></a><span data-ttu-id="ee23e-246">2. VNet – VNet kapcsolatokhoz a IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee23e-246">2. Create VNet-to-VNet connections with the IPsec/IKE policy</span></span>

<span data-ttu-id="ee23e-247">VNet – VNet-kapcsolatot, és a létrehozott IPsec/IKE házirend alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="ee23e-247">Create a VNet-to-VNet connection and apply the IPsec/IKE policy you created.</span></span> <span data-ttu-id="ee23e-248">Ebben a példában két átjáró ugyanahhoz az előfizetéshez vannak.</span><span class="sxs-lookup"><span data-stu-id="ee23e-248">In this example, both gateways are in the same subscription.</span></span> <span data-ttu-id="ee23e-249">Így a létrehozása és konfigurálása mindkét kapcsolatok az ugyanazon IPsec/IKE-házirendet a PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="ee23e-249">So it is possible to create and configure both connections with the same IPsec/IKE policy in the same PowerShell session.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> <span data-ttu-id="ee23e-250">Miután egy IPsec-/ h.rend kapcsolat van megadva, az Azure VPN gateway csak elküldi vagy elfogadja a IPsec/IKE-a megadott titkosítási algoritmusok és a kulcs szintjeiről a adott kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ee23e-250">Once an IPsec/IKE policy is specified on a connection, the Azure VPN gateway will only send or accept the IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="ee23e-251">Ellenőrizze, hogy az IPsec-házirendek mindkét kapcsolatok megegyeznek, ellenkező esetben a VNet – VNet-kapcsolatot nem fogja létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ee23e-251">Make sure the IPsec policies for both connections are the same, otherwise the VNet-to-VNet connection will not establish.</span></span>

<span data-ttu-id="ee23e-252">A lépések elvégzése után a kapcsolat néhány perc múlva, és a következő hálózati topológia fog, ahogy az a kezdő:</span><span class="sxs-lookup"><span data-stu-id="ee23e-252">After completing these steps, the connection is established in a few minutes, and you will have the following network topology as shown in the beginning:</span></span>

![IPSec-ike-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <span data-ttu-id="ee23e-254"><a name ="managepolicy"></a>Rész 5 - kapcsolat frissítés IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="ee23e-254"><a name ="managepolicy"></a>Part 5 - Update IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="ee23e-255">Az utolsó szakasza bemutatja, hogyan meglévő S2S vagy VNet – VNet kapcsolat IPsec/IKE házirendjének kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="ee23e-255">The last section shows you how to manage IPsec/IKE policy for an existing S2S or VNet-to-VNet connection.</span></span> <span data-ttu-id="ee23e-256">Az alábbiakban a gyakorlatban végigvezeti a kapcsolat a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="ee23e-256">The exercise below walks you through the following operations on a connection:</span></span>

1. <span data-ttu-id="ee23e-257">Az IPsec/IKE házirend kapcsolódási megjelenítése</span><span class="sxs-lookup"><span data-stu-id="ee23e-257">Show the IPsec/IKE policy of a connection</span></span>
2. <span data-ttu-id="ee23e-258">Szabályzat hozzáadásakor vagy módosításakor a IPsec/IKE kapcsolathoz</span><span class="sxs-lookup"><span data-stu-id="ee23e-258">Add or update the IPsec/IKE policy to a connection</span></span>
3. <span data-ttu-id="ee23e-259">Távolítsa el az IPsec/IKE-házirendet a kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="ee23e-259">Remove the IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="ee23e-260">Ugyanezek a lépések S2S és a VNet – VNet kapcsolatokhoz vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="ee23e-260">The same steps apply to both S2S and VNet-to-VNet connections.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee23e-261">IPsec-/ h.rend támogatott *szabványos* és *HighPerformance* csak VPN-átjárók útválasztó-alapú.</span><span class="sxs-lookup"><span data-stu-id="ee23e-261">IPsec/IKE policy is supported on *Standard* and *HighPerformance* route-based VPN gateways only.</span></span> <span data-ttu-id="ee23e-262">Az alapszintű átjáró-Termékváltozat vagy a csoportházirend-alapú VPN-átjáró nem működik.</span><span class="sxs-lookup"><span data-stu-id="ee23e-262">It does not work on the Basic gateway SKU or the policy-based VPN gateway.</span></span>

#### <a name="1-show-the-ipsecike-policy-of-a-connection"></a><span data-ttu-id="ee23e-263">1. Az IPsec/IKE házirend kapcsolódási megjelenítése</span><span class="sxs-lookup"><span data-stu-id="ee23e-263">1. Show the IPsec/IKE policy of a connection</span></span>

<span data-ttu-id="ee23e-264">A következő példa bemutatja, hogyan beolvasni a kapcsolat konfigurált IPsec/IKE-szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="ee23e-264">The following example shows how to get the IPsec/IKE policy configured on a connection.</span></span> <span data-ttu-id="ee23e-265">A parancsfájlok továbbra is a fenti gyakorlatokat.</span><span class="sxs-lookup"><span data-stu-id="ee23e-265">The scripts also continue from the exercises above.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="ee23e-266">Az utolsó parancs megjeleníti az aktuális IPsec/IKE-házirendet, a kapcsolat konfigurálva, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="ee23e-266">The last command lists the current IPsec/IKE policy configured on the connection, if there is any.</span></span> <span data-ttu-id="ee23e-267">A következő minta kimenete a kapcsolathoz:</span><span class="sxs-lookup"><span data-stu-id="ee23e-267">The following sample output is for the connection:</span></span>

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

<span data-ttu-id="ee23e-268">Ha nincs IPsec/IKE szabályzat konfigurálva, a parancs (PS > $connection6.policy) egy üres visszatérési lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="ee23e-268">If there is no IPsec/IKE policy configured, the command (PS> $connection6.policy) gets an empty return.</span></span> <span data-ttu-id="ee23e-269">IPsec/IKE a kapcsolat nincs konfigurálva, azonban, hogy nincs-e egyéni IPsec/IKE házirend nem jelenti.</span><span class="sxs-lookup"><span data-stu-id="ee23e-269">It does not mean IPsec/IKE is not configured on the connection, but that there is no custom IPsec/IKE policy.</span></span> <span data-ttu-id="ee23e-270">A tényleges kapcsolat használja az alapértelmezett házirendet, a helyszíni VPN-eszköz és az Azure VPN gateway között.</span><span class="sxs-lookup"><span data-stu-id="ee23e-270">The actual connection uses the default policy negotiated between your on-premises VPN device and the Azure VPN gateway.</span></span>

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a><span data-ttu-id="ee23e-271">2. A kapcsolat egy IPsec/IKE szabályzat hozzáadásakor vagy módosításakor</span><span class="sxs-lookup"><span data-stu-id="ee23e-271">2. Add or update an IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="ee23e-272">Adjon hozzá egy új házirendet, vagy a kapcsolat egy meglévő házirend frissítése lépései megegyeznek: hozzon létre egy új szabályzatot, akkor alkalmazza az új szabályzat a kapcsolatra.</span><span class="sxs-lookup"><span data-stu-id="ee23e-272">The steps to add a new policy or update an existing policy on a connection are the same: create a new policy then apply the new policy to the connection.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup None -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

<span data-ttu-id="ee23e-273">A helyi csoportházirend-alapú VPN-eszközön való csatlakozáskor "UsePolicyBasedTrafficSelectors" engedélyezéséhez vegye fel a "-UsePolicyBaseTrafficSelectors" paramétert a parancsmaghoz, vagy állítsa az értékét $False a beállítás letiltása:</span><span class="sxs-lookup"><span data-stu-id="ee23e-273">To enable "UsePolicyBasedTrafficSelectors" when connecting to an on-premises policy-based VPN device, add the "-UsePolicyBaseTrafficSelectors" parameter to the cmdlet, or set it to $False to disable the option:</span></span>

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

<span data-ttu-id="ee23e-274">A kapcsolat újra kereséséhez frissül, ha a házirend kérheti le.</span><span class="sxs-lookup"><span data-stu-id="ee23e-274">You can get the connection again to check if the policy is updated.</span></span>

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="ee23e-275">A kimenet utolsó sora, a következő példában látható módon kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="ee23e-275">You should see the output from the last line, as shown in the following example:</span></span>

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : GCMAES128
IpsecIntegrity      : GCMAES128
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14--
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a><span data-ttu-id="ee23e-276">3. Távolítsa el az IPsec/IKE házirendet a kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="ee23e-276">3. Remove an IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="ee23e-277">Amennyiben a kapcsolat az egyéni házirendet eltávolítja, az Azure VPN gateway visszavált a [alapértelmezett IPsec/IKE javaslatok listájának](vpn-gateway-about-vpn-devices.md) és újbóli a egyeztetést végez, a helyszíni VPN-eszköz újra.</span><span class="sxs-lookup"><span data-stu-id="ee23e-277">Once you remove the custom policy from a connection, the Azure VPN gateway reverts back to the [default list of IPsec/IKE proposals](vpn-gateway-about-vpn-devices.md) and renegotiates again with your on-premises VPN device.</span></span>

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

<span data-ttu-id="ee23e-278">Ellenőrizze, hogy ha a házirend el lett távolítva a kapcsolati használhatja ugyanazt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="ee23e-278">You can use the same script to check if the policy has been removed from the connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee23e-279">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee23e-279">Next steps</span></span>

<span data-ttu-id="ee23e-280">Lásd: [csatlakozás több helyszíni házirendalapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md) csoportházirend-alapú forgalom választók kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="ee23e-280">See [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) for more details regarding policy-based traffic selectors.</span></span>

<span data-ttu-id="ee23e-281">Miután a kapcsolat létrejött, hozzáadhat virtuális gépeket a virtuális hálózataihoz.</span><span class="sxs-lookup"><span data-stu-id="ee23e-281">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="ee23e-282">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ee23e-282">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>