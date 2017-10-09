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
ms.openlocfilehash: f8d2e29276efdec7071f2aa0d463b1abd64a5253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a><span data-ttu-id="5c94f-103">S2S VPN- és VNet – VNet kapcsolatokhoz IPsec/IKE-házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5c94f-103">Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections</span></span>

<span data-ttu-id="5c94f-104">Ez a cikk bemutatja, hogyan hello lépéseket tooconfigure IPsec/h.rend hello Resource Manager üzembe helyezési modellben és a PowerShell használatával telephelyek közötti VPN- és VNet – VNet kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="5c94f-104">This article walks you through hello steps tooconfigure IPsec/IKE policy for Site-to-Site VPN or VNet-to-VNet connections using hello Resource Manager deployment model and PowerShell.</span></span>

## <span data-ttu-id="5c94f-105"><a name="about"></a>Az Azure VPN gatewayek IPsec és az internetes KULCSCSERE házirend paraméterek</span><span class="sxs-lookup"><span data-stu-id="5c94f-105"><a name="about"></a>About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="5c94f-106">Standard IPsec és IKE protokoll titkosítási algoritmusok számos különböző kombinációkban támogatja.</span><span class="sxs-lookup"><span data-stu-id="5c94f-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="5c94f-107">Tekintse meg a túl[kriptográfiai követelményeiről és az Azure VPN gatewayek](vpn-gateway-about-compliance-crypto.md) toosee hogyan Ez segítheti a létesítmények közötti és VNet – VNet-kapcsolatot a megfelelőségi és biztonsági követelmények kielégítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5c94f-107">Refer too[About cryptographic requirements and Azure VPN gateways](vpn-gateway-about-compliance-crypto.md) toosee how this can help ensuring cross-premises and VNet-to-VNet connectivity satisfy your compliance or security requirements.</span></span>

<span data-ttu-id="5c94f-108">Ez a cikk utasításokat toocreate biztosít, és IPsec/IKE-szabályzat beállítása és tooa új vagy meglévő kapcsolat alkalmazni:</span><span class="sxs-lookup"><span data-stu-id="5c94f-108">This article provides instructions toocreate and configure an IPsec/IKE policy and apply tooa new or existing connection:</span></span>

* [<span data-ttu-id="5c94f-109">1. rész - munkafolyamat toocreate és IPsec/IKE házirendjének beállítása</span><span class="sxs-lookup"><span data-stu-id="5c94f-109">Part 1 - Workflow toocreate and set IPsec/IKE policy</span></span>](#workflow)
* [<span data-ttu-id="5c94f-110">2. rész - támogatott titkosítási algoritmusok és a kulcs szintjeiről</span><span class="sxs-lookup"><span data-stu-id="5c94f-110">Part 2 - Supported cryptographic algorithms and key strengths</span></span>](#params)
* [<span data-ttu-id="5c94f-111">3. rész – hozzon létre egy új S2S VPN-kapcsolat IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-111">Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>](#crossprem)
* [<span data-ttu-id="5c94f-112">Rész 4 – hozzon létre egy új VNet – VNet-kapcsolatot IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-112">Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>](#vnet2vnet)
* [<span data-ttu-id="5c94f-113">5 - rész kezelése (létrehozása, hozzáadása, eltávolítása) kapcsolat IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-113">Part 5 - Manage (create, add, remove) IPsec/IKE policy for a connection</span></span>](#managepolicy)

> [!IMPORTANT]
> 1. <span data-ttu-id="5c94f-114">Vegye figyelembe, hogy IPsec/IKE házirend csak akkor működik a hello gateway SKU-n a következő:</span><span class="sxs-lookup"><span data-stu-id="5c94f-114">Note that IPsec/IKE policy only works on hello following gateway SKUs:</span></span>
>    * <span data-ttu-id="5c94f-115">***VpnGw1, VpnGw2, VpnGw3*** (útválasztó-alapú)</span><span class="sxs-lookup"><span data-stu-id="5c94f-115">***VpnGw1, VpnGw2, VpnGw3*** (route-based)</span></span>
>    * <span data-ttu-id="5c94f-116">***Standard*** és ***HighPerformance*** (útválasztó-alapú)</span><span class="sxs-lookup"><span data-stu-id="5c94f-116">***Standard*** and ***HighPerformance*** (route-based)</span></span>
> 2. <span data-ttu-id="5c94f-117">Egy adott kapcsolathoz csak ***egy*** házirendet adhat meg.</span><span class="sxs-lookup"><span data-stu-id="5c94f-117">You can only specify ***one*** policy combination for a given connection.</span></span>
> 3. <span data-ttu-id="5c94f-118">Meg kell adnia a internetes KULCSCSERE (alapmód) és a IPsec (gyorsmódú) algoritmusok és a paraméterek.</span><span class="sxs-lookup"><span data-stu-id="5c94f-118">You must specify all algorithms and parameters for both IKE (Main Mode) and IPsec (Quick Mode).</span></span> <span data-ttu-id="5c94f-119">A részleges házirend-megadás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="5c94f-119">Partial policy specification is not allowed.</span></span>
> 4. <span data-ttu-id="5c94f-120">Vegye fel a kapcsolatot a VPN szállító műszaki tooensure hello házirend a helyszíni VPN-eszközök esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="5c94f-120">Consult with your VPN device vendor specifications tooensure hello policy is supported on your on-premises VPN devices.</span></span> <span data-ttu-id="5c94f-121">S2S vagy a VNet – VNet kapcsolatokhoz nem tud Ha hello házirendek nem kompatibilisek.</span><span class="sxs-lookup"><span data-stu-id="5c94f-121">S2S or VNet-to-VNet connections cannot establish if hello policies are incompatible.</span></span>

## <span data-ttu-id="5c94f-122"><a name ="workflow"></a>1. rész - munkafolyamat toocreate és IPsec/IKE házirendjének beállítása</span><span class="sxs-lookup"><span data-stu-id="5c94f-122"><a name ="workflow"></a>Part 1 - Workflow toocreate and set IPsec/IKE policy</span></span>
<span data-ttu-id="5c94f-123">Ez a szakasz ismerteti a hello munkafolyamat toocreate és frissítés IPsec/IKE házirend S2S VPN- vagy a VNet – VNet használ:</span><span class="sxs-lookup"><span data-stu-id="5c94f-123">This section outlines hello workflow toocreate and update IPsec/IKE policy on a S2S VPN or VNet-to-VNet connection:</span></span>
1. <span data-ttu-id="5c94f-124">Virtuális hálózat és VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-124">Create a virtual network and a VPN gateway</span></span>
2. <span data-ttu-id="5c94f-125">Helyi hálózati átjáró a helyi kapcsolat vagy egy másik virtuális hálózati közötti és VNet – VNet-kapcsolatot az átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-125">Create a local network gateway for cross premises connection, or another virtual network and gateway for VNet-to-VNet connection</span></span>
3. <span data-ttu-id="5c94f-126">Hozzon létre egy IPsec/IKE házirendet a kijelölt algoritmusok és paraméterek</span><span class="sxs-lookup"><span data-stu-id="5c94f-126">Create an IPsec/IKE policy with selected algorithms and parameters</span></span>
4. <span data-ttu-id="5c94f-127">(Az IPsec vagy VNet2VNet) kapcsolat létrehozása a hello IPsec/IKE házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-127">Create a connection (IPsec or VNet2VNet) with hello IPsec/IKE policy</span></span>
5. <span data-ttu-id="5c94f-128">Frissítés/hozzáadása egy meglévő kapcsolat az IPsec/IKE házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-128">Add/update/remove an IPsec/IKE policy for an existing connection</span></span>

<span data-ttu-id="5c94f-129">jelen cikkben lévő utasítások hello segít beállítása és konfigurálása IPsec/IKE házirendek hello ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="5c94f-129">hello instructions in this article helps you set up and configure IPsec/IKE policies as shown in hello diagram:</span></span>

![IPSec-ike-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <span data-ttu-id="5c94f-131"><a name ="params"></a>2. rész - támogatott titkosítási algoritmusok és a kulcs szintjeiről</span><span class="sxs-lookup"><span data-stu-id="5c94f-131"><a name ="params"></a>Part 2 - Supported cryptographic algorithms & key strengths</span></span>

<span data-ttu-id="5c94f-132">hello alábbi táblázat hello támogatott titkosítási algoritmusok és a kulcs szintjeiről konfigurálható hello ügyfelek:</span><span class="sxs-lookup"><span data-stu-id="5c94f-132">hello following table lists hello supported cryptographic algorithms and key strengths configurable by hello customers:</span></span>

| <span data-ttu-id="5c94f-133">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="5c94f-133">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="5c94f-134">**Beállítások**</span><span class="sxs-lookup"><span data-stu-id="5c94f-134">**Options**</span></span>    |
| ---  | --- 
| <span data-ttu-id="5c94f-135">IKEv2-titkosítás</span><span class="sxs-lookup"><span data-stu-id="5c94f-135">IKEv2 Encryption</span></span> | <span data-ttu-id="5c94f-136">AES256, AES192, AES128, DES3, DES</span><span class="sxs-lookup"><span data-stu-id="5c94f-136">AES256, AES192, AES128, DES3, DES</span></span>  
| <span data-ttu-id="5c94f-137">IKEv2-integritás</span><span class="sxs-lookup"><span data-stu-id="5c94f-137">IKEv2 Integrity</span></span>  | <span data-ttu-id="5c94f-138">SHA384, MD5, SHA1, SHA256</span><span class="sxs-lookup"><span data-stu-id="5c94f-138">SHA384, SHA256, SHA1, MD5</span></span>  |
| <span data-ttu-id="5c94f-139">DH-csoport</span><span class="sxs-lookup"><span data-stu-id="5c94f-139">DH Group</span></span>         | <span data-ttu-id="5c94f-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span><span class="sxs-lookup"><span data-stu-id="5c94f-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span></span> |
| <span data-ttu-id="5c94f-141">IPsec-titkosítás</span><span class="sxs-lookup"><span data-stu-id="5c94f-141">IPsec Encryption</span></span> | <span data-ttu-id="5c94f-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, Nincs</span><span class="sxs-lookup"><span data-stu-id="5c94f-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None</span></span>    |
| <span data-ttu-id="5c94f-143">IPsec-integritás</span><span class="sxs-lookup"><span data-stu-id="5c94f-143">IPsec Integrity</span></span>  | <span data-ttu-id="5c94f-144">GCMASE256, GCMAES192, GCMAES128, SHA-256, SHA1, MD5</span><span class="sxs-lookup"><span data-stu-id="5c94f-144">GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5</span></span> |
| <span data-ttu-id="5c94f-145">PFS-csoport</span><span class="sxs-lookup"><span data-stu-id="5c94f-145">PFS Group</span></span>        | <span data-ttu-id="5c94f-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Nincs</span><span class="sxs-lookup"><span data-stu-id="5c94f-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None</span></span> 
| <span data-ttu-id="5c94f-147">Gyorsmódú biztonsági társítás élettartama</span><span class="sxs-lookup"><span data-stu-id="5c94f-147">QM SA Lifetime</span></span>   | <span data-ttu-id="5c94f-148">(**Nem kötelező**: alapértelmezett értékek vannak használt Ha nincs megadva)</span><span class="sxs-lookup"><span data-stu-id="5c94f-148">(**Optional**: default values are used if not specified)</span></span><br><span data-ttu-id="5c94f-149">Másodperc (egész szám; **min. 300**/alapértelmezett érték: 27000 másodperc)</span><span class="sxs-lookup"><span data-stu-id="5c94f-149">Seconds (integer; **min. 300**/default 27000 seconds)</span></span><br><span data-ttu-id="5c94f-150">KB (egész szám; **min. 1024**/alapértelmezett érték: 102400000 KB)</span><span class="sxs-lookup"><span data-stu-id="5c94f-150">KBytes (integer; **min. 1024**/default 102400000 KBytes)</span></span>   |
| <span data-ttu-id="5c94f-151">Forgalomválasztó</span><span class="sxs-lookup"><span data-stu-id="5c94f-151">Traffic Selector</span></span> | <span data-ttu-id="5c94f-152">UsePolicyBasedTrafficSelectors ** ($True vagy $False; **Nem kötelező**, alapértelmezett $False Ha nincs megadva)</span><span class="sxs-lookup"><span data-stu-id="5c94f-152">UsePolicyBasedTrafficSelectors** ($True/$False; **Optional**, default $False if not specified)</span></span>    |
|  |  |

> [!IMPORTANT]
> 1. <span data-ttu-id="5c94f-153">**GCMAES mint IPsec titkosítási algoritmus használata esetén ki kell választania azonos GCMAES algoritmus és a kulcshossz hello IPSec-integritásának; például mind a GCMAES128 használatával**</span><span class="sxs-lookup"><span data-stu-id="5c94f-153">**If GCMAES is used as for IPsec Encryption algorithm, you must select hello same GCMAES algorithm and key length for IPsec Integrity; for example, using GCMAES128 for both**</span></span>
> 2. <span data-ttu-id="5c94f-154">IKEv2 alapmódú biztonsági Társítás élettartama hello Azure VPN gatewayek a 28 800 másodperc van rögzítve.</span><span class="sxs-lookup"><span data-stu-id="5c94f-154">IKEv2 Main Mode SA lifetime is fixed at 28,800 seconds on hello Azure VPN gateways</span></span>
> 3. <span data-ttu-id="5c94f-155">"UsePolicyBasedTrafficSelectors" beállítás túl$ True kapcsolaton konfigurálja hello Azure VPN gateway tooconnect toopolicy-alapú VPN tűzfal a helyszínen.</span><span class="sxs-lookup"><span data-stu-id="5c94f-155">Setting "UsePolicyBasedTrafficSelectors" too$True on a connection will configure hello Azure VPN gateway tooconnect toopolicy-based VPN firewall on premises.</span></span> <span data-ttu-id="5c94f-156">Ha engedélyezi az PolicyBasedTrafficSelectors, kell-e a VPN-eszköz van hello meghatározott összes kiegészítve a helyi hálózati (helyi hálózati átjáró) előtagok hello Azure-beli virtuális hálózat előtagokat, és a megfelelő forgalmat választók tooensure nem minden elem közöttiként.</span><span class="sxs-lookup"><span data-stu-id="5c94f-156">If you enable PolicyBasedTrafficSelectors, you need tooensure your VPN device has hello matching traffic selectors defined with all combinations of your on-premises network (local network gateway) prefixes to/from hello Azure virtual network prefixes, instead of any-to-any.</span></span> <span data-ttu-id="5c94f-157">Például ha a helyszíni hálózati előtagok 10.1.0.0/16 és 10.2.0.0/16, és a virtuális hálózati előtagok 192.168.0.0/16 és 172.16.0.0/16, kell a következő forgalmat választók toospecify hello:</span><span class="sxs-lookup"><span data-stu-id="5c94f-157">For example, if your on-premises network prefixes are 10.1.0.0/16 and 10.2.0.0/16, and your virtual network prefixes are 192.168.0.0/16 and 172.16.0.0/16, you need toospecify hello following traffic selectors:</span></span>
>    * <span data-ttu-id="5c94f-158">10.1.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="5c94f-158">10.1.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="5c94f-159">10.1.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="5c94f-159">10.1.0.0/16 <====> 172.16.0.0/16</span></span>
>    * <span data-ttu-id="5c94f-160">10.2.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="5c94f-160">10.2.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="5c94f-161">10.2.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="5c94f-161">10.2.0.0/16 <====> 172.16.0.0/16</span></span>

<span data-ttu-id="5c94f-162">Csoportházirend-alapú forgalom választók kapcsolatos további információkért lásd: [csatlakozás több helyszíni házirendalapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5c94f-162">For more information regarding policy-based traffic selectors, see [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>

<span data-ttu-id="5c94f-163">a következő táblázat hello hello egyéni házirend által támogatott megfelelő Diffie-Hellman csoport hello:</span><span class="sxs-lookup"><span data-stu-id="5c94f-163">hello following table lists hello corresponding Diffie-Hellman Groups supported by hello custom policy:</span></span>

| <span data-ttu-id="5c94f-164">**Diffie-Hellman csoport**</span><span class="sxs-lookup"><span data-stu-id="5c94f-164">**Diffie-Hellman Group**</span></span>  | <span data-ttu-id="5c94f-165">**DH-csoport**</span><span class="sxs-lookup"><span data-stu-id="5c94f-165">**DHGroup**</span></span>              | <span data-ttu-id="5c94f-166">**PFS-csoport**</span><span class="sxs-lookup"><span data-stu-id="5c94f-166">**PFSGroup**</span></span> | <span data-ttu-id="5c94f-167">**A kulcs hossza**</span><span class="sxs-lookup"><span data-stu-id="5c94f-167">**Key length**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5c94f-168">1</span><span class="sxs-lookup"><span data-stu-id="5c94f-168">1</span></span>                         | <span data-ttu-id="5c94f-169">DHGroup1</span><span class="sxs-lookup"><span data-stu-id="5c94f-169">DHGroup1</span></span>                 | <span data-ttu-id="5c94f-170">PFS1</span><span class="sxs-lookup"><span data-stu-id="5c94f-170">PFS1</span></span>         | <span data-ttu-id="5c94f-171">768 bites MODP</span><span class="sxs-lookup"><span data-stu-id="5c94f-171">768-bit MODP</span></span>   |
| <span data-ttu-id="5c94f-172">2</span><span class="sxs-lookup"><span data-stu-id="5c94f-172">2</span></span>                         | <span data-ttu-id="5c94f-173">DHGroup2</span><span class="sxs-lookup"><span data-stu-id="5c94f-173">DHGroup2</span></span>                 | <span data-ttu-id="5c94f-174">PFS2</span><span class="sxs-lookup"><span data-stu-id="5c94f-174">PFS2</span></span>         | <span data-ttu-id="5c94f-175">1024 bites MODP</span><span class="sxs-lookup"><span data-stu-id="5c94f-175">1024-bit MODP</span></span>  |
| <span data-ttu-id="5c94f-176">14</span><span class="sxs-lookup"><span data-stu-id="5c94f-176">14</span></span>                        | <span data-ttu-id="5c94f-177">DHGroup14</span><span class="sxs-lookup"><span data-stu-id="5c94f-177">DHGroup14</span></span><br><span data-ttu-id="5c94f-178">DHGroup2048</span><span class="sxs-lookup"><span data-stu-id="5c94f-178">DHGroup2048</span></span> | <span data-ttu-id="5c94f-179">PFS2048</span><span class="sxs-lookup"><span data-stu-id="5c94f-179">PFS2048</span></span>      | <span data-ttu-id="5c94f-180">2048 bites MODP</span><span class="sxs-lookup"><span data-stu-id="5c94f-180">2048-bit MODP</span></span>  |
| <span data-ttu-id="5c94f-181">19</span><span class="sxs-lookup"><span data-stu-id="5c94f-181">19</span></span>                        | <span data-ttu-id="5c94f-182">ECP256</span><span class="sxs-lookup"><span data-stu-id="5c94f-182">ECP256</span></span>                   | <span data-ttu-id="5c94f-183">ECP256</span><span class="sxs-lookup"><span data-stu-id="5c94f-183">ECP256</span></span>       | <span data-ttu-id="5c94f-184">256 bites ECP</span><span class="sxs-lookup"><span data-stu-id="5c94f-184">256-bit ECP</span></span>    |
| <span data-ttu-id="5c94f-185">20</span><span class="sxs-lookup"><span data-stu-id="5c94f-185">20</span></span>                        | <span data-ttu-id="5c94f-186">ECP384</span><span class="sxs-lookup"><span data-stu-id="5c94f-186">ECP384</span></span>                   | <span data-ttu-id="5c94f-187">ECP284</span><span class="sxs-lookup"><span data-stu-id="5c94f-187">ECP284</span></span>       | <span data-ttu-id="5c94f-188">384 bites ECP</span><span class="sxs-lookup"><span data-stu-id="5c94f-188">384-bit ECP</span></span>    |
| <span data-ttu-id="5c94f-189">24</span><span class="sxs-lookup"><span data-stu-id="5c94f-189">24</span></span>                        | <span data-ttu-id="5c94f-190">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="5c94f-190">DHGroup24</span></span>                | <span data-ttu-id="5c94f-191">PFS24</span><span class="sxs-lookup"><span data-stu-id="5c94f-191">PFS24</span></span>        | <span data-ttu-id="5c94f-192">2048 bites MODP</span><span class="sxs-lookup"><span data-stu-id="5c94f-192">2048-bit MODP</span></span>  |

<span data-ttu-id="5c94f-193">Tekintse meg a túl[RFC3526](https://tools.ietf.org/html/rfc3526) és [RFC5114](https://tools.ietf.org/html/rfc5114) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="5c94f-193">Refer too[RFC3526](https://tools.ietf.org/html/rfc3526) and [RFC5114](https://tools.ietf.org/html/rfc5114) for more details.</span></span>

## <span data-ttu-id="5c94f-194"><a name ="crossprem"></a>3. rész – hozzon létre egy új S2S VPN-kapcsolat IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-194"><a name ="crossprem"></a>Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>

<span data-ttu-id="5c94f-195">Ez a szakasz végigvezeti hello S2S VPN-kapcsolat létrehozása az IPsec/IKE házirendjével.</span><span class="sxs-lookup"><span data-stu-id="5c94f-195">This section walks you through hello steps of creating a S2S VPN connection with an IPsec/IKE policy.</span></span> <span data-ttu-id="5c94f-196">hello lépések hello kapcsolat létrehozása hello ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="5c94f-196">hello following steps create hello connection as shown in hello diagram:</span></span>

![s2s-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

<span data-ttu-id="5c94f-198">Lásd: [S2S VPN-kapcsolatot](vpn-gateway-create-site-to-site-rm-powershell.md) részletesebb lépésenkénti S2S VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5c94f-198">See [Create a S2S VPN connection](vpn-gateway-create-site-to-site-rm-powershell.md) for more detailed step-by-step instructions for creating a S2S VPN connection.</span></span>

### <span data-ttu-id="5c94f-199"><a name="before"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5c94f-199"><a name="before"></a>Before you begin</span></span>

* <span data-ttu-id="5c94f-200">Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="5c94f-200">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="5c94f-201">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5c94f-201">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5c94f-202">Hello Azure Resource Manager PowerShell-parancsmagjainak telepítése.</span><span class="sxs-lookup"><span data-stu-id="5c94f-202">Install hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="5c94f-203">Lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="5c94f-203">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <span data-ttu-id="5c94f-204"><a name="createvnet1"></a>1. lépés – hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-204"><a name="createvnet1"></a>Step 1 - Create hello virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="5c94f-205">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="5c94f-205">1. Declare your variables</span></span>

<span data-ttu-id="5c94f-206">Ehhez a gyakorlathoz először is deklarálni kell a változókat.</span><span class="sxs-lookup"><span data-stu-id="5c94f-206">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="5c94f-207">Lehet, hogy tooreplace hello értékeket a saját üzemi konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="5c94f-207">Be sure tooreplace hello values with your own when configuring for production.</span></span>

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="5c94f-208">2. Csatlakozás tooyour előfizetés, és hozzon létre egy új erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="5c94f-208">2. Connect tooyour subscription and create a new resource group</span></span>

<span data-ttu-id="5c94f-209">Váltson át tooPowerShell mód toouse hello erőforrás-kezelő parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="5c94f-209">Make sure you switch tooPowerShell mode toouse hello Resource Manager cmdlets.</span></span> <span data-ttu-id="5c94f-210">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="5c94f-210">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="5c94f-211">Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="5c94f-211">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="5c94f-212">A következő minta toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="5c94f-212">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="5c94f-213">3. Hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-213">3. Create hello virtual network, VPN gateway, and local network gateway</span></span>

<span data-ttu-id="5c94f-214">a következő minta hello hello virtuális hálózat, TestVNet1, hoz létre három alhálózatok és hello VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="5c94f-214">hello following sample creates hello virtual network, TestVNet1, with three subnets, and hello VPN gateway.</span></span> <span data-ttu-id="5c94f-215">Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen.</span><span class="sxs-lookup"><span data-stu-id="5c94f-215">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="5c94f-216">Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="5c94f-216">If you name it something else, your gateway creation fails.</span></span>

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

### <span data-ttu-id="5c94f-217"><a name="s2sconnection"></a>2. lépés - az IPsec/IKE házirendjével S2S VPN-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-217"><a name="s2sconnection"></a>Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="5c94f-218">1. IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-218">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="5c94f-219">a következő mintaparancsfájl hello házirendet hoz létre IPsec/IKE hello algoritmusok és a paraméterek a következő:</span><span class="sxs-lookup"><span data-stu-id="5c94f-219">hello following sample script creates an IPsec/IKE policy with hello following algorithms and parameters:</span></span>

* <span data-ttu-id="5c94f-220">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="5c94f-220">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="5c94f-221">IPsec: AES256, SHA-256, PFS24, 2048KB & SA élettartama 7200 másodperc</span><span class="sxs-lookup"><span data-stu-id="5c94f-221">IPsec: AES256, SHA256, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

<span data-ttu-id="5c94f-222">Ha GCMAES használja az IPsec, használnia kell hello azonos GCMAES algoritmus és a kulcshossz IPsec titkosításhoz és integritását, például:</span><span class="sxs-lookup"><span data-stu-id="5c94f-222">If you use GCMAES for IPsec, you must use hello same GCMAES algorithm and key length for both IPsec encryption and integrity, for example:</span></span>

* <span data-ttu-id="5c94f-223">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="5c94f-223">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="5c94f-224">IPsec: **GCMAES256, GCMAES256**, PFS24, 2048 KB & SA élettartama 7200 másodperc</span><span class="sxs-lookup"><span data-stu-id="5c94f-224">IPsec: **GCMAES256, GCMAES256**, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-hello-ipsecike-policy"></a><span data-ttu-id="5c94f-225">2. IPsec-/ h.rend hello hello S2S VPN-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-225">2. Create hello S2S VPN connection with hello IPsec/IKE policy</span></span>

<span data-ttu-id="5c94f-226">S2S VPN-kapcsolat létrehozásához, és a korábban létrehozott hello IPsec/IKE házirend alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="5c94f-226">Create an S2S VPN connection and apply hello IPsec/IKE policy created earlier.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="5c94f-227">Opcionálisan hozzáadhat "-UsePolicyBasedTrafficSelectors $True" toohello kapcsolat létrehozása parancsmag tooenable Azure VPN gateway tooconnect toopolicy-alapú VPN-eszközök a helyszínen, fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="5c94f-227">You can optionally add "-UsePolicyBasedTrafficSelectors $True" toohello create connection cmdlet tooenable Azure VPN gateway tooconnect toopolicy-based VPN devices on premises, as described above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c94f-228">Miután egy IPsec-/ h.rend kapcsolat van megadva, hello Azure VPN gateway csak elküldi vagy hello IPsec/IKE javaslat a megadott titkosítási algoritmusok és a kulcs szintjeiről a adott kapcsolat elfogadása.</span><span class="sxs-lookup"><span data-stu-id="5c94f-228">Once an IPsec/IKE policy is specified on a connection, hello Azure VPN gateway will only send or accept hello IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="5c94f-229">Győződjön meg arról, a helyszíni VPN-eszköz kapcsolat hello használ, vagy fogadja hello pontos házirend kombináció, ellenkező esetben nem főkiszolgálójával hello S2S VPN-alagúton.</span><span class="sxs-lookup"><span data-stu-id="5c94f-229">Make sure your on-premises VPN device for hello connection uses or accepts hello exact policy combination, otherwise hello S2S VPN tunnel will not establish.</span></span>


## <span data-ttu-id="5c94f-230"><a name ="vnet2vnet"></a>Rész 4 – hozzon létre egy új VNet – VNet-kapcsolatot IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-230"><a name ="vnet2vnet"></a>Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>

<span data-ttu-id="5c94f-231">hello VNet – VNet kapcsolat létrehozásának olyan IPsec/IKE házirend lépésekre hasonló toothat egy S2S VPN-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5c94f-231">hello steps of creating a VNet-to-VNet connection with an IPsec/IKE policy are similar toothat of a S2S VPN connection.</span></span> <span data-ttu-id="5c94f-232">hello következő minta parancsfájlokat hello kapcsolat létrehozása hello ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="5c94f-232">hello following sample scripts create hello connection as shown in hello diagram:</span></span>

![v2v-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

<span data-ttu-id="5c94f-234">Lásd: [VNet – VNet-kapcsolatot](vpn-gateway-vnet-vnet-rm-ps.md) részletes lépéseket a VNet – VNet-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5c94f-234">See [Create a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) for more detailed steps for creating a VNet-to-VNet connection.</span></span> <span data-ttu-id="5c94f-235">Meg kell adnia a [3. rész](#crossprem) toocreate hello VPN Gateway és TestVNet1 konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="5c94f-235">You must complete [Part 3](#crossprem) toocreate and configure TestVNet1 and hello VPN Gateway.</span></span>

### <span data-ttu-id="5c94f-236"><a name="createvnet2"></a>1. lépés – hello második virtuális hálózat és a VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-236"><a name="createvnet2"></a>Step 1 - Create hello second virtual network and VPN gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="5c94f-237">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="5c94f-237">1. Declare your variables</span></span>

<span data-ttu-id="5c94f-238">Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="5c94f-238">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

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

#### <a name="2-create-hello-second-virtual-network-and-vpn-gateway-in-hello-new-resource-group"></a><span data-ttu-id="5c94f-239">2. Hello második virtuális hálózat és a VPN-átjáró hello új erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-239">2. Create hello second virtual network and VPN gateway in hello new resource group</span></span>

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

### <a name="step-2---create-a-vnet-tovnet-connection-with-hello-ipsecike-policy"></a><span data-ttu-id="5c94f-240">2. lépés - a VNet-toVNet kapcsolat létrehozása a hello IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-240">Step 2 - Create a VNet-toVNet connection with hello IPsec/IKE policy</span></span>

<span data-ttu-id="5c94f-241">Hasonló toohello S2S VPN-kapcsolat IPsec/IKE-házirend létrehozása, akkor alkalmazza a toopolicy toohello új kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="5c94f-241">Similar toohello S2S VPN connection, create an IPsec/IKE policy then apply toopolicy toohello new connection.</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="5c94f-242">1. IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-242">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="5c94f-243">a következő mintaparancsfájl hello különböző IPsec/IKE-házirendet hoz hello algoritmusok és a paraméterek a következő:</span><span class="sxs-lookup"><span data-stu-id="5c94f-243">hello following sample script creates a different IPsec/IKE policy with hello following algorithms and parameters:</span></span>
* <span data-ttu-id="5c94f-244">IKEv2: Az AES128, SHA1, DHGroup14</span><span class="sxs-lookup"><span data-stu-id="5c94f-244">IKEv2: AES128, SHA1, DHGroup14</span></span>
* <span data-ttu-id="5c94f-245">IPsec: GCMAES128, GCMAES128, PFS14, SA élettartama 7200 másodperc & 4096KB</span><span class="sxs-lookup"><span data-stu-id="5c94f-245">IPsec: GCMAES128, GCMAES128, PFS14, SA Lifetime 7200 seconds & 4096KB</span></span>

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 4096
```

#### <a name="2-create-vnet-to-vnet-connections-with-hello-ipsecike-policy"></a><span data-ttu-id="5c94f-246">2. VNet – VNet kapcsolatokhoz hello IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c94f-246">2. Create VNet-to-VNet connections with hello IPsec/IKE policy</span></span>

<span data-ttu-id="5c94f-247">VNet – VNet-kapcsolatot, és létrehozott hello IPsec/IKE házirend alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="5c94f-247">Create a VNet-to-VNet connection and apply hello IPsec/IKE policy you created.</span></span> <span data-ttu-id="5c94f-248">Ebben a példában mindkét átjárók vannak hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5c94f-248">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="5c94f-249">Így lehetséges toocreate, és mindkét-kapcsolatok konfigurálása hello hello ugyanazon IPsec/IKE házirend ugyanazon PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="5c94f-249">So it is possible toocreate and configure both connections with hello same IPsec/IKE policy in hello same PowerShell session.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> <span data-ttu-id="5c94f-250">Miután egy IPsec-/ h.rend kapcsolat van megadva, hello Azure VPN gateway csak elküldi vagy hello IPsec/IKE javaslat a megadott titkosítási algoritmusok és a kulcs szintjeiről a adott kapcsolat elfogadása.</span><span class="sxs-lookup"><span data-stu-id="5c94f-250">Once an IPsec/IKE policy is specified on a connection, hello Azure VPN gateway will only send or accept hello IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="5c94f-251">Győződjön meg arról, hogy hello IPsec-házirendek a mindkét kapcsolatok vannak hello azonos, ellenkező esetben a VNet – VNet-kapcsolatot nem fogja létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5c94f-251">Make sure hello IPsec policies for both connections are hello same, otherwise the VNet-to-VNet connection will not establish.</span></span>

<span data-ttu-id="5c94f-252">A lépések elvégzése után hello kapcsolatot néhány perc múlva, és a következő hálózati topológia látható módon hello kezdete hello kell:</span><span class="sxs-lookup"><span data-stu-id="5c94f-252">After completing these steps, hello connection is established in a few minutes, and you will have hello following network topology as shown in hello beginning:</span></span>

![IPSec-ike-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <span data-ttu-id="5c94f-254"><a name ="managepolicy"></a>Rész 5 - kapcsolat frissítés IPsec/IKE-házirend</span><span class="sxs-lookup"><span data-stu-id="5c94f-254"><a name ="managepolicy"></a>Part 5 - Update IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="5c94f-255">hello utolsó szakasza bemutatja, hogyan toomanage IPsec/h.rend létező S2S vagy VNet – VNet kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5c94f-255">hello last section shows you how toomanage IPsec/IKE policy for an existing S2S or VNet-to-VNet connection.</span></span> <span data-ttu-id="5c94f-256">az alábbi hello a gyakorlatban végigvezeti hello műveleteket a kapcsolatot a következő:</span><span class="sxs-lookup"><span data-stu-id="5c94f-256">hello exercise below walks you through hello following operations on a connection:</span></span>

1. <span data-ttu-id="5c94f-257">Hello IPsec/IKE házirend kapcsolódási megjelenítése</span><span class="sxs-lookup"><span data-stu-id="5c94f-257">Show hello IPsec/IKE policy of a connection</span></span>
2. <span data-ttu-id="5c94f-258">Adja hozzá vagy hello IPsec/IKE házirend tooa kapcsolat frissítése</span><span class="sxs-lookup"><span data-stu-id="5c94f-258">Add or update hello IPsec/IKE policy tooa connection</span></span>
3. <span data-ttu-id="5c94f-259">A kapcsolat hello IPsec/IKE házirend eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5c94f-259">Remove hello IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="5c94f-260">hello ugyanazokat a lépéseket alkalmazása tooboth S2S és VNet – VNet kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="5c94f-260">hello same steps apply tooboth S2S and VNet-to-VNet connections.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c94f-261">IPsec-/ h.rend támogatott *szabványos* és *HighPerformance* csak VPN-átjárók útválasztó-alapú.</span><span class="sxs-lookup"><span data-stu-id="5c94f-261">IPsec/IKE policy is supported on *Standard* and *HighPerformance* route-based VPN gateways only.</span></span> <span data-ttu-id="5c94f-262">Alapszintű átjáró hello SKU vagy hello házirendalapú VPN-átjáró nem működik.</span><span class="sxs-lookup"><span data-stu-id="5c94f-262">It does not work on hello Basic gateway SKU or hello policy-based VPN gateway.</span></span>

#### <a name="1-show-hello-ipsecike-policy-of-a-connection"></a><span data-ttu-id="5c94f-263">1. Hello IPsec/IKE házirend kapcsolódási megjelenítése</span><span class="sxs-lookup"><span data-stu-id="5c94f-263">1. Show hello IPsec/IKE policy of a connection</span></span>

<span data-ttu-id="5c94f-264">hello a következő példa bemutatja, hogyan tooget hello kapcsolaton konfigurált IPsec/IKE-szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="5c94f-264">hello following example shows how tooget hello IPsec/IKE policy configured on a connection.</span></span> <span data-ttu-id="5c94f-265">hello parancsfájlok továbbra is a fenti hello gyakorlatokat.</span><span class="sxs-lookup"><span data-stu-id="5c94f-265">hello scripts also continue from hello exercises above.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="5c94f-266">hello utolsó parancs hello hello kapcsolaton konfigurált aktuális IPsec/IKE házirend sorolja fel, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="5c94f-266">hello last command lists hello current IPsec/IKE policy configured on hello connection, if there is any.</span></span> <span data-ttu-id="5c94f-267">a következő minta kimenet hello hello kapcsolat van:</span><span class="sxs-lookup"><span data-stu-id="5c94f-267">hello following sample output is for hello connection:</span></span>

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

<span data-ttu-id="5c94f-268">Ha nincs konfigurált IPsec/IKE házirend, hello parancs (PS > $connection6.policy) egy üres visszatérési lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="5c94f-268">If there is no IPsec/IKE policy configured, hello command (PS> $connection6.policy) gets an empty return.</span></span> <span data-ttu-id="5c94f-269">IPsec/IKE hello kapcsolat nincs konfigurálva, azonban, hogy nincs-e egyéni IPsec/IKE házirend nem jelenti.</span><span class="sxs-lookup"><span data-stu-id="5c94f-269">It does not mean IPsec/IKE is not configured on hello connection, but that there is no custom IPsec/IKE policy.</span></span> <span data-ttu-id="5c94f-270">hello tényleges kapcsolat a helyszíni VPN-eszköz és a hello Azure VPN gateway egyezteti hello alapértelmezett házirendet használja.</span><span class="sxs-lookup"><span data-stu-id="5c94f-270">hello actual connection uses hello default policy negotiated between your on-premises VPN device and hello Azure VPN gateway.</span></span>

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a><span data-ttu-id="5c94f-271">2. A kapcsolat egy IPsec/IKE szabályzat hozzáadásakor vagy módosításakor</span><span class="sxs-lookup"><span data-stu-id="5c94f-271">2. Add or update an IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="5c94f-272">hello lépések tooadd egy új házirendet, vagy a kapcsolat egy meglévő házirenddel is frissítés hello ugyanaz: hozzon létre egy új szabályzatot, akkor alkalmazza az új házirend toohello hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5c94f-272">hello steps tooadd a new policy or update an existing policy on a connection are hello same: create a new policy then apply hello new policy toohello connection.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup None -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

<span data-ttu-id="5c94f-273">tooenable "UsePolicyBasedTrafficSelectors" Ha tooan csatlakozás a helyi csoportházirend-alapú VPN-eszköz hozzáadása hello "-UsePolicyBaseTrafficSelectors" paraméter toohello parancsmagot, vagy állítsa be túl$ False toodisable hello lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="5c94f-273">tooenable "UsePolicyBasedTrafficSelectors" when connecting tooan on-premises policy-based VPN device, add hello "-UsePolicyBaseTrafficSelectors" parameter toohello cmdlet, or set it too$False toodisable hello option:</span></span>

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

<span data-ttu-id="5c94f-274">Hello kapcsolat kaphat újra toocheck hello házirend frissítése után.</span><span class="sxs-lookup"><span data-stu-id="5c94f-274">You can get hello connection again toocheck if hello policy is updated.</span></span>

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="5c94f-275">Hello kimenete hello utolsó sora, ahogy az alábbi példa hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5c94f-275">You should see hello output from hello last line, as shown in hello following example:</span></span>

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

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a><span data-ttu-id="5c94f-276">3. Távolítsa el az IPsec/IKE házirendet a kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="5c94f-276">3. Remove an IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="5c94f-277">Kapcsolat hello egyéni házirendet eltávolítja, ha hello Azure VPN gateway visszaállítja-e a háttérben toohello [alapértelmezett IPsec/IKE javaslatok listájának](vpn-gateway-about-vpn-devices.md) és újbóli a egyeztetést végez, a helyszíni VPN-eszközön újra.</span><span class="sxs-lookup"><span data-stu-id="5c94f-277">Once you remove hello custom policy from a connection, hello Azure VPN gateway reverts back toohello [default list of IPsec/IKE proposals](vpn-gateway-about-vpn-devices.md) and renegotiates again with your on-premises VPN device.</span></span>

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

<span data-ttu-id="5c94f-278">Használhatja ugyanazon parancsfájl toocheck hello hello házirend hello kapcsolatról el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="5c94f-278">You can use hello same script toocheck if hello policy has been removed from hello connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c94f-279">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c94f-279">Next steps</span></span>

<span data-ttu-id="5c94f-280">Lásd: [csatlakozás több helyszíni házirendalapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md) csoportházirend-alapú forgalom választók kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="5c94f-280">See [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) for more details regarding policy-based traffic selectors.</span></span>

<span data-ttu-id="5c94f-281">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="5c94f-281">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="5c94f-282">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c94f-282">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
