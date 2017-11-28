---
title: "Az Azure VPN gateway tooreestablish IPsec bújtatja alaphelyzetbe állítása |} Microsoft Docs"
description: "Ez a cikk végigvezeti az Azure VPN Gateway tooreestablish IPsec-alagutak alaphelyzetbe állítását. hello cikk tooVPN átjárók klasszikus hello és hello Resource Manager üzembe helyezési modellel vonatkozik."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 79d77cb8-d175-4273-93ac-712d7d45b1fe
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: cherylmc
ms.openlocfilehash: 84dd741f0bebd6b18cb235216a68a88da5fe17b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reset-a-vpn-gateway"></a><span data-ttu-id="063d3-104">VPN-átjáró alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="063d3-104">Reset a VPN Gateway</span></span>

<span data-ttu-id="063d3-105">Az Azure VPN Gateway alaphelyzetbe állítása akkor hasznos, ha egy vagy több helyek közötti VPN-alagúton elveszíti a létesítmények közötti VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="063d3-105">Resetting an Azure VPN gateway is helpful if you lose cross-premises VPN connectivity on one or more Site-to-Site VPN tunnels.</span></span> <span data-ttu-id="063d3-106">Ebben a helyzetben a helyszíni VPN-eszközök minden megfelelően működik-e, de nem tudta tooestablish hello Azure VPN gatewayek az IPsec-alagutak.</span><span class="sxs-lookup"><span data-stu-id="063d3-106">In this situation, your on-premises VPN devices are all working correctly, but are not able tooestablish IPsec tunnels with hello Azure VPN gateways.</span></span> <span data-ttu-id="063d3-107">Ez a cikk segítséget nyújt a VPN-átjárót alaphelyzetbe.</span><span class="sxs-lookup"><span data-stu-id="063d3-107">This article helps you reset your VPN gateway.</span></span>

### <a name="what-happens-during-a-reset"></a><span data-ttu-id="063d3-108">Mi történik, amikor egy alaphelyzetbe áll?</span><span class="sxs-lookup"><span data-stu-id="063d3-108">What happens during a reset?</span></span>

<span data-ttu-id="063d3-109">VPN-átjáró két Virtuálisgép-példány fut egy aktív-készenléti állapotban lévő konfigurációs tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="063d3-109">A VPN gateway is composed of two VM instances running in an active-standby configuration.</span></span> <span data-ttu-id="063d3-110">Hello átjáró alaphelyzetbe állításakor újraindítja hello átjáró, és ezután újra alkalmazza hello létesítmények közötti konfigurációk tooit.</span><span class="sxs-lookup"><span data-stu-id="063d3-110">When you reset hello gateway, it reboots hello gateway, and then reapplies hello cross-premises configurations tooit.</span></span> <span data-ttu-id="063d3-111">hello átjáró tartja hello már rendelkezik nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="063d3-111">hello gateway keeps hello public IP address it already has.</span></span> <span data-ttu-id="063d3-112">Ez azt jelenti, hogy nincs szükség Azure VPN Gateway tooupdate hello VPN-útválasztó konfigurációja egy új nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="063d3-112">This means you won’t need tooupdate hello VPN router configuration with a new public IP address for Azure VPN gateway.</span></span>

<span data-ttu-id="063d3-113">Hello parancs tooreset hello átjáró elküldésekor hello jelenlegi aktív példány hello Azure VPN-átjáró azonnali újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="063d3-113">When you issue hello command tooreset hello gateway, hello current active instance of hello Azure VPN gateway is rebooted immediately.</span></span> <span data-ttu-id="063d3-114">Lesz rövid hiány hello aktív példányból (újraindítása folyamatban), toohello készenléti állapotban lévő példány hello feladatátvétel során.</span><span class="sxs-lookup"><span data-stu-id="063d3-114">There will be a brief gap during hello failover from hello active instance (being rebooted), toohello standby instance.</span></span> <span data-ttu-id="063d3-115">hello gap egy percnél kevesebb kell lennie.</span><span class="sxs-lookup"><span data-stu-id="063d3-115">hello gap should be less than one minute.</span></span>

<span data-ttu-id="063d3-116">Hello kapcsolat hello első újraindítás után nem állítja vissza, ha a probléma hello azonos újra tooreboot hello második Virtuálisgép-példány (hello új aktív átjáró) parancsot.</span><span class="sxs-lookup"><span data-stu-id="063d3-116">If hello connection is not restored after hello first reboot, issue hello same command again tooreboot hello second VM instance (hello new active gateway).</span></span> <span data-ttu-id="063d3-117">Ha két újraindításra hello kért hátsó tooback, nem lesznek némileg hosszabb ideig ahol mindkét Virtuálisgép-példányok (aktív és készenléti) újraindítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="063d3-117">If hello two reboots are requested back tooback, there will be a slightly longer period where both VM instances (active and standby) are being rebooted.</span></span> <span data-ttu-id="063d3-118">Ennek hatására hosszabb hiány hello VPN-kapcsolatot, too2 too4 percig, virtuális gépek toocomplete hello vesznek fel a.</span><span class="sxs-lookup"><span data-stu-id="063d3-118">This will cause a longer gap on hello VPN connectivity, up too2 too4 minutes for VMs toocomplete hello reboots.</span></span>

<span data-ttu-id="063d3-119">Után két újraindításra Ha olyan továbbra is problémákat tapasztal, létesítmények közötti kapcsolatot, nyisson egy támogatási kérést hello Azure-portálon a.</span><span class="sxs-lookup"><span data-stu-id="063d3-119">After two reboots, if you are still experiencing cross-premises connectivity problems, please open a support request from hello Azure portal.</span></span>

## <span data-ttu-id="063d3-120"><a name="before"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="063d3-120"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="063d3-121">Az átjáró visszaállítása előtt ellenőrizze a minden IPsec-webhelyek (közötti S2S) VPN-alagút az alább felsorolt hello kulcsfontosságú tételekről.</span><span class="sxs-lookup"><span data-stu-id="063d3-121">Before you reset your gateway, verify hello key items listed below for each IPsec Site-to-Site (S2S) VPN tunnel.</span></span> <span data-ttu-id="063d3-122">Bármely eltért elemek hello jár S2S VPN-alagutat hello bontja a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="063d3-122">Any mismatch in hello items will result in hello disconnect of S2S VPN tunnels.</span></span> <span data-ttu-id="063d3-123">Ellenőrzése és javítása a helyszíni és az Azure VPN gatewayek hello konfigurációi menti, szükségtelen újraindítások, és a szolgáltatások hello más működő kapcsolatok hello átjárókon.</span><span class="sxs-lookup"><span data-stu-id="063d3-123">Verifying and correcting hello configurations for your on-premises and Azure VPN gateways saves you from unnecessary reboots and disruptions for hello other working connections on hello gateways.</span></span>

<span data-ttu-id="063d3-124">Ellenőrizze a következő elemeket az átjárót alaphelyzetbe állítás előtt hello:</span><span class="sxs-lookup"><span data-stu-id="063d3-124">Verify hello following items before resetting your gateway:</span></span>

* <span data-ttu-id="063d3-125">hello Internet IP-címek (VIP) mindkét hello Azure VPN Gateway és hello a helyszíni VPN-átjáró helyesen vannak-e konfigurálva mindkét hello Azure és a hello a helyszíni VPN-szabályzatok.</span><span class="sxs-lookup"><span data-stu-id="063d3-125">hello Internet IP addresses (VIPs) for both hello Azure VPN gateway and hello on-premises VPN gateway are configured correctly in both hello Azure and hello on-premises VPN policies.</span></span>
* <span data-ttu-id="063d3-126">hello előre megosztott kulcs ugyanazt az Azure és a helyszíni VPN-átjáróként kell hello.</span><span class="sxs-lookup"><span data-stu-id="063d3-126">hello pre-shared key must be hello same on both Azure and on-premises VPN gateways.</span></span>
* <span data-ttu-id="063d3-127">Ha IPsec/IKE konfigurációs, például a titkosításhoz, a kivonatoló algoritmusok és a PFS (teljes továbbítási titkosítása), győződjön meg arról, mindkettő hello Azure és a helyszíni VPN-átjárók rendelkeznek hello ugyanazokat a konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="063d3-127">If you apply specific IPsec/IKE configuration, such as encryption, hashing algorithms, and PFS (Perfect Forward Secrecy), ensure both hello Azure and on-premises VPN gateways have hello same configurations.</span></span>

## <span data-ttu-id="063d3-128"><a name="portal"></a>Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="063d3-128"><a name="portal"></a>Azure portal</span></span>

<span data-ttu-id="063d3-129">Alaphelyzetbe állíthatja a Resource Manager VPN-átjáró hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="063d3-129">You can reset a Resource Manager VPN gateway using hello Azure portal.</span></span> <span data-ttu-id="063d3-130">Ha azt szeretné, hogy a klasszikus átjáró tooreset, tekintse meg a hello [PowerShell](#resetclassic) lépéseket.</span><span class="sxs-lookup"><span data-stu-id="063d3-130">If you want tooreset a classic gateway, see hello [PowerShell](#resetclassic) steps.</span></span>

### <a name="resource-manager-deployment-model"></a><span data-ttu-id="063d3-131">Resource Manager-alapú üzemi modell</span><span class="sxs-lookup"><span data-stu-id="063d3-131">Resource Manager deployment model</span></span>

1. <span data-ttu-id="063d3-132">Nyissa meg hello [Azure-portálon](https://portal.azure.com) , és keresse meg, hogy szeretné-e tooreset toohello erőforrás-kezelő virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="063d3-132">Open hello [Azure portal](https://portal.azure.com) and navigate toohello Resource Manager virtual network gateway that you want tooreset.</span></span>
2. <span data-ttu-id="063d3-133">Hello virtuális hálózati átjáró hello paneljén kattintson az "Új".</span><span class="sxs-lookup"><span data-stu-id="063d3-133">On hello blade for hello virtual network gateway, click 'Reset'.</span></span>

  ![Alaphelyzetbe állítja a VPN-átjáró panel](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. <span data-ttu-id="063d3-135">A hello panel visszaállítása parancsra hello **alaphelyzetbe** gombra.</span><span class="sxs-lookup"><span data-stu-id="063d3-135">On hello Reset blade, click hello **Reset** button.</span></span>

## <span data-ttu-id="063d3-136"><a name="ps"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="063d3-136"><a name="ps"></a>PowerShell</span></span>

### <a name="resource-manager-deployment-model"></a><span data-ttu-id="063d3-137">Resource Manager-alapú üzemi modell</span><span class="sxs-lookup"><span data-stu-id="063d3-137">Resource Manager deployment model</span></span>

<span data-ttu-id="063d3-138">hello parancsmag, az átjárót alaphelyzetbe állítása **alaphelyzetbe állítása-AzureRmVirtualNetworkGateway**.</span><span class="sxs-lookup"><span data-stu-id="063d3-138">hello cmdlet for resetting a gateway is **Reset-AzureRmVirtualNetworkGateway**.</span></span> <span data-ttu-id="063d3-139">A beállítások visszaállítása elvégzése előtt győződjön meg arról, hogy a legújabb verziójának hello hello [Resource Manager PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="063d3-139">Before performing a reset, make sure you have hello latest version of hello [Resource Manager PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span> <span data-ttu-id="063d3-140">hello alábbi példa alaphelyzetbe állítja a virtuális hálózati átjáró VNet1GW nevű hello TestRG1 erőforráscsoportban:</span><span class="sxs-lookup"><span data-stu-id="063d3-140">hello following example resets a virtual network gateway named VNet1GW in hello TestRG1 resource group:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

<span data-ttu-id="063d3-141">Eredmény:</span><span class="sxs-lookup"><span data-stu-id="063d3-141">Result:</span></span>

<span data-ttu-id="063d3-142">Amikor megjelenik a visszaadandó eredmény, akkor feltételezheti, hello átjáró alaphelyzetbe állítása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="063d3-142">When you receive a return result, you can assume hello gateway reset was successful.</span></span> <span data-ttu-id="063d3-143">Azonban nincs semmi hello visszaadott eredmény, amely explicit módon, hogy hello alaphelyzetbe állítása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="063d3-143">However, there is nothing in hello return result that indicates explicitly that hello reset was successful.</span></span> <span data-ttu-id="063d3-144">Ha azt szeretné, hogy szorosan: hello előzmények toosee toolook pontosan amikor hello átjáró alaphelyzetbe lépett fel, megtekintheti ezt az információt a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="063d3-144">If you want toolook closely at hello history toosee exactly when hello gateway reset occurred, you can view that information in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="063d3-145">Hello portálon lépjen túl**(GatewayName) -> erőforrás állapotának**.</span><span class="sxs-lookup"><span data-stu-id="063d3-145">In hello portal, navigate too**'GatewayName' -> Resource Health**.</span></span>

### <span data-ttu-id="063d3-146"><a name="resetclassic"></a>Klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="063d3-146"><a name="resetclassic"></a>Classic deployment model</span></span>

<span data-ttu-id="063d3-147">hello parancsmag, az átjárót alaphelyzetbe állítása **alaphelyzetbe állítása-AzureVNetGateway**.</span><span class="sxs-lookup"><span data-stu-id="063d3-147">hello cmdlet for resetting a gateway is **Reset-AzureVNetGateway**.</span></span> <span data-ttu-id="063d3-148">A beállítások visszaállítása elvégzése előtt győződjön meg arról, hogy a legújabb verziójának hello hello [Service Management (SM) PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="063d3-148">Before performing a reset, make sure you have hello latest version of hello [Service Management (SM) PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0).</span></span> <span data-ttu-id="063d3-149">hello alábbi példa visszaállítja egy "ContosoVNet" nevű virtuális hálózati átjáró hello:</span><span class="sxs-lookup"><span data-stu-id="063d3-149">hello following example resets hello gateway for a virtual network named "ContosoVNet":</span></span>

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

<span data-ttu-id="063d3-150">Eredmény:</span><span class="sxs-lookup"><span data-stu-id="063d3-150">Result:</span></span>

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <span data-ttu-id="063d3-151"><a name="cli"></a>Az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="063d3-151"><a name="cli"></a>Azure CLI</span></span>

<span data-ttu-id="063d3-152">tooreset hello átjáró használata hello [az hálózati vnet-átjáró alaphelyzetbe](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) parancsot.</span><span class="sxs-lookup"><span data-stu-id="063d3-152">tooreset hello gateway, use hello [az network vnet-gateway reset](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) command.</span></span> <span data-ttu-id="063d3-153">hello alábbi példa alaphelyzetbe állítja a virtuális hálózati átjáró VNet5GW nevű hello TestRG5 erőforráscsoportban:</span><span class="sxs-lookup"><span data-stu-id="063d3-153">hello following example resets a virtual network gateway named VNet5GW in hello TestRG5 resource group:</span></span>

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

<span data-ttu-id="063d3-154">Eredmény:</span><span class="sxs-lookup"><span data-stu-id="063d3-154">Result:</span></span>

<span data-ttu-id="063d3-155">Amikor megjelenik a visszaadandó eredmény, akkor feltételezheti, hello átjáró alaphelyzetbe állítása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="063d3-155">When you receive a return result, you can assume hello gateway reset was successful.</span></span> <span data-ttu-id="063d3-156">Azonban nincs semmi hello visszaadott eredmény, amely explicit módon, hogy hello alaphelyzetbe állítása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="063d3-156">However, there is nothing in hello return result that indicates explicitly that hello reset was successful.</span></span> <span data-ttu-id="063d3-157">Ha azt szeretné, hogy szorosan: hello előzmények toosee toolook pontosan amikor hello átjáró alaphelyzetbe lépett fel, megtekintheti ezt az információt a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="063d3-157">If you want toolook closely at hello history toosee exactly when hello gateway reset occurred, you can view that information in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="063d3-158">Hello portálon lépjen túl**(GatewayName) -> erőforrás állapotának**.</span><span class="sxs-lookup"><span data-stu-id="063d3-158">In hello portal, navigate too**'GatewayName' -> Resource Health**.</span></span>
