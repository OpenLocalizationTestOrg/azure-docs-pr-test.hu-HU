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
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a><span data-ttu-id="b78b1-103">Hibaelhárítás: Az időnként megszakad az Azure--webhelyek közötti VPN</span><span class="sxs-lookup"><span data-stu-id="b78b1-103">Troubleshooting: Azure Site-to-Site VPN disconnects intermittently</span></span>

<span data-ttu-id="b78b1-104">Hello probléma, hogy egy új vagy meglévő Microsoft Azure pont-pont VPN-kapcsolat nincs stabil-e, vagy leválik rendszeresen Ön is szembesülhet.</span><span class="sxs-lookup"><span data-stu-id="b78b1-104">You might experience hello problem that a new or existing Microsoft Azure Point-to-Site VPN connection is not stable or disconnects regularly.</span></span> <span data-ttu-id="b78b1-105">Ez a cikk ismerteti a lépéseket toohelp azonosítani és megoldani a hello hello hiba okának elhárítása.</span><span class="sxs-lookup"><span data-stu-id="b78b1-105">This article provides troubleshoot steps toohelp you identify and resolve hello cause of hello problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="b78b1-106">Hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="b78b1-106">Troubleshooting steps</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="b78b1-107">Előfeltétel-ellenőrzési lépés</span><span class="sxs-lookup"><span data-stu-id="b78b1-107">Prerequisite step</span></span>

<span data-ttu-id="b78b1-108">Az Azure virtuális hálózati átjáró hello típusának ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="b78b1-108">Check hello type of Azure  virtual network gateway:</span></span>

1. <span data-ttu-id="b78b1-109">Nyissa meg túl[Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b78b1-109">Go too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b78b1-110">Ellenőrizze a hello **áttekintése** hello virtuális hálózati átjáró hello típus információ oldalán.</span><span class="sxs-lookup"><span data-stu-id="b78b1-110">Check hello **Overview** page of hello virtual network gateway for hello type information.</span></span>
    
    ![hello átjáró hello áttekintése](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a><span data-ttu-id="b78b1-112">Lépés az 1. Ellenőrizze, hogy hello a helyszíni VPN-eszköz van hitelesítve</span><span class="sxs-lookup"><span data-stu-id="b78b1-112">Step 1 Check whether hello on-premises VPN device is validated</span></span>

1. <span data-ttu-id="b78b1-113">Ellenőrizze, hogy használ-e egy [érvényesíteni a VPN-eszköz és az operációs rendszer verziója](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="b78b1-113">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="b78b1-114">Ha a VPN-eszköz hello nincs érvényesítve, előfordulhat, hogy toocontact hello eszköz gyártó toosee bármely kompatibilitási probléma esetén.</span><span class="sxs-lookup"><span data-stu-id="b78b1-114">If hello VPN device is not validated, you may have toocontact hello device manufacturer toosee if there is any compatibility issue.</span></span>
2. <span data-ttu-id="b78b1-115">Győződjön meg arról, hogy hello VPN-eszköz megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="b78b1-115">Make sure that hello VPN device is correctly configured.</span></span> <span data-ttu-id="b78b1-116">További információkért lásd: [eszköz konfigurációs minták szerkesztése](vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="b78b1-116">For more information, see [Editing device configuration samples](vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-check-hello-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a><span data-ttu-id="b78b1-117">2. lépés hello biztonsági társítás beállításokat (a csoportházirend-alapú Azure virtuális hálózati átjárók)</span><span class="sxs-lookup"><span data-stu-id="b78b1-117">Step 2 Check hello Security Association settings(for policy-based Azure virtual network gateways)</span></span>

1. <span data-ttu-id="b78b1-118">Győződjön meg arról, hogy hello virtuális hálózat, alhálózatok és tartományok hello a **helyi hálózati átjáró** definíciót a Microsoft Azure-ban ugyanaz, mint a hello konfigurációs hello a helyszíni VPN-eszközön.</span><span class="sxs-lookup"><span data-stu-id="b78b1-118">Make sure that hello virtual network, subnets and, ranges in hello **Local network gateway** definition in Microsoft Azure are same as hello configuration on hello on-premises VPN device.</span></span>
2. <span data-ttu-id="b78b1-119">Ellenőrizze a biztonsági társítás hello beállítások megfelelő.</span><span class="sxs-lookup"><span data-stu-id="b78b1-119">Verify that hello Security Association settings match.</span></span>

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a><span data-ttu-id="b78b1-120">3. lépés-ellenőrzése átjáró-alhálózatot a felhasználó által definiált útvonalak és hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="b78b1-120">Step 3 Check for User-Defined Routes or Network Security Groups on Gateway Subnet</span></span>

<span data-ttu-id="b78b1-121">Hello átjáró-alhálózatot a felhasználó által megadott útvonal előfordulhat, hogy korlátozzák a forgalmat, és más forgalom engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b78b1-121">A user-defined route on hello gateway subnet may be restricting some traffic and allowing other traffic.</span></span> <span data-ttu-id="b78b1-122">Így megjelenik, hogy hello VPN-kapcsolat megbízhatatlanná néhány adatforgalomhoz és mások helyes.</span><span class="sxs-lookup"><span data-stu-id="b78b1-122">This makes it appear that hello VPN connection is unreliable for some traffic and good for others.</span></span> 

### <a name="step-4-check-hello-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="b78b1-123">4. lépés ellenőrzése "egy VPN-alagút egy alhálózat pár" hello beállítása (Csoportházirend-alapú virtuális hálózati átjárók)</span><span class="sxs-lookup"><span data-stu-id="b78b1-123">Step 4 Check hello "one VPN Tunnel per Subnet Pair" setting (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="b78b1-124">Győződjön meg arról, hogy hello a helyszíni VPN-eszköz beállítása toohave **egy VPN-alagút egy alhálózat pár** csoportházirend-alapú virtuális hálózati átjárók.</span><span class="sxs-lookup"><span data-stu-id="b78b1-124">Make sure that hello on-premises VPN device is set toohave **one VPN tunnel per subnet pair** for policy-based virtual network gateways.</span></span>

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="b78b1-125">5. lépés (a csoportházirend-alapú virtuális hálózati átjárók) a biztonsági társítás korlátozás-ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="b78b1-125">Step 5 Check for Security Association Limitation (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="b78b1-126">hello csoportházirend-alapú virtuális hálózati átjáró legfeljebb 200 alhálózati biztonsági társítás párok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b78b1-126">hello Policy-based virtual network gateway has limit of 200 subnet Security Association pairs.</span></span> <span data-ttu-id="b78b1-127">Ha Azure-beli virtuális hálózat alhálózatok hello száma meg kell szorozni alkalommal hello helyi alhálózatok száma nagyobb, mint 200 az úgy, hogy időnként alhálózatok leválasztása.</span><span class="sxs-lookup"><span data-stu-id="b78b1-127">If hello number of Azure virtual network subnets multiplied times by hello number of local subnets is greater than 200, you see sporadic subnets disconnecting.</span></span>

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="b78b1-128">6. lépés ellenőrizze a helyi VPN-eszköz kapcsolat külső címét</span><span class="sxs-lookup"><span data-stu-id="b78b1-128">Step 6 Check on-premises VPN device external interface address</span></span>

- <span data-ttu-id="b78b1-129">Ha hello az internetre irányuló hello VPN-eszköz IP-címe szerepel-e a hello **helyi hálózati átjáró** definition Azure, szórványos kapcsolat megszakadása tapasztalhatja.</span><span class="sxs-lookup"><span data-stu-id="b78b1-129">If hello Internet facing IP address of hello VPN device is included in hello **Local network gateway** definition in Azure, you may experience sporadic disconnections.</span></span>
- <span data-ttu-id="b78b1-130">hello eszköz külső felületet kell közvetlenül a hello Internet.</span><span class="sxs-lookup"><span data-stu-id="b78b1-130">hello device's external interface must be directly on hello Internet.</span></span> <span data-ttu-id="b78b1-131">Egyetlen hálózati címfordítás (NAT) vagy hello Internet és hello eszköz között tűzfal kell.</span><span class="sxs-lookup"><span data-stu-id="b78b1-131">There should be no Network Address Translation (NAT) or firewall between hello Internet and hello device.</span></span>
-  <span data-ttu-id="b78b1-132">Ha tűzfal fürtszolgáltatás toohave konfigurálja egy virtuális IP-cím, meg kell hello fürt megszüntetése, és tegye elérhetővé a hello VPN-készülék, közvetlen tooa hello az átjáró nyilvános csatoló úgy csatlakozhatnak a.</span><span class="sxs-lookup"><span data-stu-id="b78b1-132">If you configure Firewall Clustering toohave a virtual IP, you must break hello cluster and expose hello VPN appliance directly tooa public interface that hello gateway can interface with.</span></span>

### <a name="step-7-check-whether-hello-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a><span data-ttu-id="b78b1-133">Lépés 7 ellenőrizze, hogy hello a helyszíni VPN-eszköz teljes továbbítási titkosítása engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="b78b1-133">Step 7 Check whether hello on-premises VPN device has Perfect Forward Secrecy enabled</span></span>

<span data-ttu-id="b78b1-134">Hello **teljes továbbítási titkosítása** szolgáltatás hello kapcsolatbontás problémákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="b78b1-134">hello **Perfect Forward Secrecy** feature can cause hello disconnection problems.</span></span> <span data-ttu-id="b78b1-135">Ha hello VPN-eszköz **továbbítását tökéletes** hello szolgáltatás engedélyezve van, tiltsa le.</span><span class="sxs-lookup"><span data-stu-id="b78b1-135">If hello VPN device has **Perfect forward Secrecy** enabled, disable hello feature.</span></span> <span data-ttu-id="b78b1-136">Majd [hello virtuális hálózati átjáró IPSec-házirend frissítése](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span><span class="sxs-lookup"><span data-stu-id="b78b1-136">Then [update hello virtual network gateway IPsec policy](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b78b1-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b78b1-137">Next steps</span></span>

- [<span data-ttu-id="b78b1-138">Pont-pont kapcsolat tooa virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b78b1-138">Configure a Site-to-Site connection tooa virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [<span data-ttu-id="b78b1-139">Telephelyek közötti VPN-kapcsolatok IPsec/IKE-házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b78b1-139">Configure IPsec/IKE policy for Site-to-Site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)

