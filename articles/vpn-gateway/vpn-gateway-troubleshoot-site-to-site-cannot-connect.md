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
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a><span data-ttu-id="2f0fa-103">Hibáinak elhárítása: Egy Azure-webhelyek VPN-kapcsolatot nem lehet kapcsolódni, és nem működik</span><span class="sxs-lookup"><span data-stu-id="2f0fa-103">Troubleshooting: An Azure site-to-site VPN connection cannot connect and stops working</span></span>

<span data-ttu-id="2f0fa-104">Miután konfigurált egy a helyszíni hálózat és az Azure virtuális hálózat közötti pont-pont VPN kapcsolatot, a hello VPN-kapcsolat hirtelen leáll, és nem kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-104">After you configure a site-to-site VPN connection between an on-premises network and an Azure virtual network, hello VPN connection suddenly stops working and cannot be reconnected.</span></span> <span data-ttu-id="2f0fa-105">Ez a cikk ismerteti a hibaelhárítási lépéseket toohelp a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="2f0fa-106">Hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="2f0fa-106">Troubleshooting steps</span></span>

<span data-ttu-id="2f0fa-107">tooresolve hello probléma, először próbálkozzon az túl[alaphelyzetbe állítása hello Azure VPN gateway](vpn-gateway-resetgw-classic.md) és hello alagút a hello a helyszíni VPN-eszköz alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-107">tooresolve hello problem, first try too[reset hello Azure VPN gateway](vpn-gateway-resetgw-classic.md) and reset hello tunnel from hello on-premises VPN device.</span></span> <span data-ttu-id="2f0fa-108">Ha hello a probléma továbbra is fennáll, kövesse ezeket hello probléma lépéseket tooidentify hello okát.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-108">If hello problem persists, follow these steps tooidentify hello cause of hello problem.</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="2f0fa-109">Előfeltétel-ellenőrzési lépés</span><span class="sxs-lookup"><span data-stu-id="2f0fa-109">Prerequisite step</span></span>

<span data-ttu-id="2f0fa-110">Hello Azure VPN-átjáró hello típusának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-110">Check hello type of hello Azure VPN gateway.</span></span>

1. <span data-ttu-id="2f0fa-111">Nyissa meg toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2f0fa-111">Go toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="2f0fa-112">Ellenőrizze a hello **áttekintése** hello VPN-átjáró hello típus információ oldalán.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-112">Check hello **Overview** page of hello VPN gateway for hello type information.</span></span>
    
    ![Hello átjáró – áttekintés](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a><span data-ttu-id="2f0fa-114">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2f0fa-114">Step 1.</span></span> <span data-ttu-id="2f0fa-115">Ellenőrizze, hogy a rendszer érvényesíti hello a helyszíni VPN-eszköz</span><span class="sxs-lookup"><span data-stu-id="2f0fa-115">Check whether hello on-premises VPN device is validated</span></span>

1. <span data-ttu-id="2f0fa-116">Ellenőrizze, hogy használ-e egy [érvényesíteni a VPN-eszköz és az operációs rendszer verziója](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="2f0fa-116">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="2f0fa-117">Ha nem ellenőrzött VPN-eszközhöz hello eszköz, lehetséges, hogy toocontact hello eszköz gyártó toosee kompatibilitási probléma esetén.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-117">If hello device is not a validated VPN device, you might have toocontact hello device manufacturer toosee if there is a compatibility issue.</span></span>

2. <span data-ttu-id="2f0fa-118">Győződjön meg arról, hogy hello VPN-eszköz megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-118">Make sure that hello VPN device is correctly configured.</span></span> <span data-ttu-id="2f0fa-119">További információkért lásd: [eszköz konfigurációs minták szerkesztése](/vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="2f0fa-119">For more information, see [Edit device configuration samples](/vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-verify-hello-shared-key"></a><span data-ttu-id="2f0fa-120">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2f0fa-120">Step 2.</span></span> <span data-ttu-id="2f0fa-121">Hello megosztott kulcs ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2f0fa-121">Verify hello shared key</span></span>

<span data-ttu-id="2f0fa-122">Hasonlítsa össze a megosztott kulcs hello hello a helyszíni VPN eszköz toohello Azure virtuális hálózat VPN toomake meg arról, hogy megfelel-e hello kulcsok.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-122">Compare hello shared key for hello on-premises VPN device toohello Azure Virtual Network VPN toomake sure that hello keys match.</span></span> 

<span data-ttu-id="2f0fa-123">tooview hello megosztott kulcs hello Azure VPN-kapcsolatot, a hello a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="2f0fa-123">tooview hello shared key for hello Azure VPN connection, use one of hello following methods:</span></span>

<span data-ttu-id="2f0fa-124">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="2f0fa-124">**Azure portal**</span></span>

1. <span data-ttu-id="2f0fa-125">Nyissa meg toohello VPN-átjáró webhelyek kapcsolat létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-125">Go toohello VPN gateway site-to-site connection that you created.</span></span>

2. <span data-ttu-id="2f0fa-126">A hello **beállítások** kattintson **megosztott kulcs**.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-126">In hello **Settings** section, click **Shared key**.</span></span>
    
    ![Megosztott kulcs](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

<span data-ttu-id="2f0fa-128">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="2f0fa-128">**Azure PowerShell**</span></span>

<span data-ttu-id="2f0fa-129">Hello Azure Resource Manager telepítési modell:</span><span class="sxs-lookup"><span data-stu-id="2f0fa-129">For hello Azure Resource Manager deployment model:</span></span>

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

<span data-ttu-id="2f0fa-130">Hello klasszikus telepítési modell:</span><span class="sxs-lookup"><span data-stu-id="2f0fa-130">For hello classic deployment model:</span></span>

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-hello-vpn-peer-ips"></a><span data-ttu-id="2f0fa-131">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2f0fa-131">Step 3.</span></span> <span data-ttu-id="2f0fa-132">Hello VPN társ IP-cím ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2f0fa-132">Verify hello VPN peer IPs</span></span>

-   <span data-ttu-id="2f0fa-133">IP-definíciójában hello hello **helyi hálózati átjáró** Azure objektumban meg kell felelnie a hello helyszíni eszköz IP.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-133">hello IP definition in hello **Local Network Gateway** object in Azure should match hello on-premises device IP.</span></span>
-   <span data-ttu-id="2f0fa-134">hello Azure átjáró IP-definíciót, amely hello be van állítva a helyszíni eszközök meg kell felelnie a hello Azure átjáró IP.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-134">hello Azure gateway IP definition that is set on hello on-premises device should match hello Azure gateway IP.</span></span>

### <a name="step-4-check-udr-and-nsgs-on-hello-gateway-subnet"></a><span data-ttu-id="2f0fa-135">4. lépés</span><span class="sxs-lookup"><span data-stu-id="2f0fa-135">Step 4.</span></span> <span data-ttu-id="2f0fa-136">Hello átjáróalhálózatot UDR és NSG-k ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2f0fa-136">Check UDR and NSGs on hello gateway subnet</span></span>

<span data-ttu-id="2f0fa-137">Ellenőrizze a távolítsa el a felhasználó által definiált útválasztási (UDR) vagy a hálózati biztonsági csoportokkal (NSG-k) hello átjáró-alhálózat és tesztelje a hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-137">Check for and remove user-defined routing (UDR) or Network Security Groups (NSGs) on hello gateway subnet, and then test hello result.</span></span> <span data-ttu-id="2f0fa-138">Ha hello probléma megoldódott, UDR vagy NSG alkalmazott hello-beállítások érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-138">If hello problem is resolved, validate hello settings that UDR or NSG applied.</span></span>

### <a name="step-5-check-hello-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="2f0fa-139">5. lépés</span><span class="sxs-lookup"><span data-stu-id="2f0fa-139">Step 5.</span></span> <span data-ttu-id="2f0fa-140">Jelölőnégyzet hello a helyszíni VPN-eszköz kapcsolat külső címét</span><span class="sxs-lookup"><span data-stu-id="2f0fa-140">Check hello on-premises VPN device external interface address</span></span>

- <span data-ttu-id="2f0fa-141">Ha hello hello VPN-eszköz Internet felé néző IP-címe szerepel hello **helyi hálózati** definition Azure-ban az Ön is szembesülhet szórványos kapcsolat megszakadása.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-141">If hello Internet-facing IP address of hello VPN device is included in hello **Local network** definition in Azure, you might experience sporadic disconnections.</span></span>
- <span data-ttu-id="2f0fa-142">hello eszköz külső felületet kell közvetlenül a hello Internet.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-142">hello device's external interface must be directly on hello Internet.</span></span> <span data-ttu-id="2f0fa-143">Egyetlen hálózati címfordítás vagy hello eszköz- és hello az Internet között tűzfal kell.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-143">There should be no network address translation or firewall between hello Internet and hello device.</span></span>
- <span data-ttu-id="2f0fa-144">tooconfigure tűzfal fürtözési toohave egy virtuális IP-cím, kell hello fürt megszüntetése, és teszi közzé a hello VPN-készülék közvetlen tooa nyilvános csatoló adott hello átjáró úgy csatlakozhatnak a.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-144">tooconfigure firewall clustering toohave a virtual IP, you must break hello cluster and expose hello VPN appliance directly tooa public interface that hello gateway can interface with.</span></span>

### <a name="step-6-verify-that-hello-subnets-match-exactly-azure-policy-based-gateways"></a><span data-ttu-id="2f0fa-145">6. lépés</span><span class="sxs-lookup"><span data-stu-id="2f0fa-145">Step 6.</span></span> <span data-ttu-id="2f0fa-146">Győződjön meg arról, hogy hello alhálózatok pontosan megegyezik-e (az Azure csoportházirend-alapú átjárók)</span><span class="sxs-lookup"><span data-stu-id="2f0fa-146">Verify that hello subnets match exactly (Azure policy-based gateways)</span></span>

-   <span data-ttu-id="2f0fa-147">Ellenőrizze, hogy megfelel-e hello alhálózatok pontosan hello Azure-beli virtuális hálózat és a helyszíni definícióit hello Azure-beli virtuális hálózat között.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-147">Verify that hello subnets match exactly between hello Azure virtual network and on-premises definitions for hello Azure virtual network.</span></span>
-   <span data-ttu-id="2f0fa-148">Ellenőrizze, hogy hello alhálózatok hello közötti pontosan egyezik-e **helyi hálózati átjáró** és a helyszíni hello a helyszíni hálózati definíciókat.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-148">Verify that hello subnets match exactly between hello **Local Network Gateway** and on-premises definitions for hello on-premises network.</span></span>

### <a name="step-7-verify-hello-azure-gateway-health-probe"></a><span data-ttu-id="2f0fa-149">7. lépés</span><span class="sxs-lookup"><span data-stu-id="2f0fa-149">Step 7.</span></span> <span data-ttu-id="2f0fa-150">Hello Azure átjáró állapotmintáihoz ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2f0fa-150">Verify hello Azure gateway health probe</span></span>

1. <span data-ttu-id="2f0fa-151">Nyissa meg toohello [állapotmintáihoz](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).</span><span class="sxs-lookup"><span data-stu-id="2f0fa-151">Go toohello [health probe](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).</span></span>

2. <span data-ttu-id="2f0fa-152">Kattintson a hello tanúsítványfigyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-152">Click through hello certificate warning.</span></span>
3. <span data-ttu-id="2f0fa-153">Ha választ kapnak hello VPN-átjáró megfelelő minősül.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-153">If you receive a response, hello VPN gateway is considered healthy.</span></span> <span data-ttu-id="2f0fa-154">Ha nem érkezik válasz, hello átjáró nem lesz kifogástalan, vagy egy NSG hello átjáró-alhálózatot a problémás hello.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-154">If you don't receive a response, hello gateway might not be healthy or an NSG on hello gateway subnet is causing hello problem.</span></span> <span data-ttu-id="2f0fa-155">a következő szöveg hello egy mintaválasz:</span><span class="sxs-lookup"><span data-stu-id="2f0fa-155">hello following text is a sample response:</span></span>

    <span data-ttu-id="2f0fa-156">&lt;? XML-verzió = "1.0"? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">elsődleges példány: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / karakterlánc&gt;</span><span class="sxs-lookup"><span data-stu-id="2f0fa-156">&lt;?xml version="1.0"?>  <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Primary Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6</string&gt;</span></span>

### <a name="step-8-check-whether-hello-on-premises-vpn-device-has-hello-perfect-forward-secrecy-feature-enabled"></a><span data-ttu-id="2f0fa-157">8. lépés.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-157">Step 8.</span></span> <span data-ttu-id="2f0fa-158">Ellenőrizze, hogy hello a helyszíni VPN-eszköz hello PFS funkció engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="2f0fa-158">Check whether hello on-premises VPN device has hello perfect forward secrecy feature enabled</span></span>

<span data-ttu-id="2f0fa-159">hello PFS szolgáltatás kapcsolatbontás problémákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-159">hello perfect forward secrecy feature can cause disconnection problems.</span></span> <span data-ttu-id="2f0fa-160">Ha hello VPN-eszköz PFS engedélyezve van, tiltsa le a hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-160">If hello VPN device has perfect forward secrecy enabled, disable hello feature.</span></span> <span data-ttu-id="2f0fa-161">Hello VPN gateway IPsec-házirendjének frissíteni.</span><span class="sxs-lookup"><span data-stu-id="2f0fa-161">Then update hello VPN gateway IPsec policy.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f0fa-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f0fa-162">Next steps</span></span>

-   [<span data-ttu-id="2f0fa-163">Pont-pont kapcsolat tooa virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2f0fa-163">Configure a site-to-site connection tooa virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [<span data-ttu-id="2f0fa-164">Pont-pont a VPN-kapcsolatok IPsec/IKE-szabályzat beállítása</span><span class="sxs-lookup"><span data-stu-id="2f0fa-164">Configure an IPsec/IKE policy for site-to-site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)
