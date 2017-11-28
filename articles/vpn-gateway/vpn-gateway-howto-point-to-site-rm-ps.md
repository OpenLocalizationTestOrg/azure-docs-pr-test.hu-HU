---
title: "Csatlakozás egy számítógép tooan Azure virtuális hálózat használatával a pont-pont és a Tanúsítványalapú hitelesítés: PowerShell |} Microsoft Docs"
description: "Biztonságos kapcsolódás egy számítógép tooyour virtuális hálózatot hozzon létre egy pont – hely típusú VPN gateway-kapcsolatot tanúsítvány alapú hitelesítést használ. Ez a cikk toohello Resource Manager üzembe helyezési modellben vonatkozik, és használja a PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: b962e4b1946a4ae17d4eb2b920ed54437bc26b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-powershell"></a><span data-ttu-id="31995-104">Egy pont – hely kapcsolat tooa virtuális hálózat konfigurálása tanúsítvány hitelesítése: PowerShell</span><span class="sxs-lookup"><span data-stu-id="31995-104">Configure a Point-to-Site connection tooa VNet using certificate authentication: PowerShell</span></span>

<span data-ttu-id="31995-105">Ez a cikk bemutatja, hogyan toocreate egy Vnetet egy pont – hely kapcsolat hello Resource Manager üzembe a modellhez tartozó PowerShell-lel.</span><span class="sxs-lookup"><span data-stu-id="31995-105">This article shows you how toocreate a VNet with a Point-to-Site connection in hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="31995-106">Ez a konfiguráció tanúsítványok tooauthenticate hello csatlakozó ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="31995-106">This configuration uses certificates tooauthenticate hello connecting client.</span></span> <span data-ttu-id="31995-107">Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="31995-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="31995-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31995-108">Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="31995-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31995-109">PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="31995-110">(Klasszikus) Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31995-110">Azure portal (classic)</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

<span data-ttu-id="31995-111">Pont-pont (P2S) VPN-átjáró lehetővé teszi a biztonságos kapcsolat tooyour virtuális hálózat létrehozása az egyéni ügyfél-számítógépről.</span><span class="sxs-lookup"><span data-stu-id="31995-111">A Point-to-Site (P2S) VPN gateway lets you create a secure connection tooyour virtual network from an individual client computer.</span></span> <span data-ttu-id="31995-112">Pont-pont VPN-kapcsolatok akkor hasznos, ha azt szeretné, hogy a virtuális hálózat egy távoli helyről, például amikor, amelyek dolgozzon home vagy konferencia tooconnect tooyour.</span><span class="sxs-lookup"><span data-stu-id="31995-112">Point-to-Site VPN connections are useful when you want tooconnect tooyour VNet from a remote location, such when you are telecommuting from home or a conference.</span></span> <span data-ttu-id="31995-113">P2S VPN esetén is egy hasznos megoldás toouse helyett a telephelyek közötti VPN tooconnect tooa VNet kell csak néhány ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="31995-113">A P2S VPN is also a useful solution toouse instead of a Site-to-Site VPN when you have only a few clients that need tooconnect tooa VNet.</span></span>

<span data-ttu-id="31995-114">P2S használja a Secure Socket Tunneling Protocol (SSTP), amely SSL-alapú VPN-protokoll hello.</span><span class="sxs-lookup"><span data-stu-id="31995-114">P2S uses hello Secure Socket Tunneling Protocol (SSTP), which is an SSL-based VPN protocol.</span></span> <span data-ttu-id="31995-115">P2S VPN-kapcsolatot létesít hello ügyfélszámítógépről elindításával.</span><span class="sxs-lookup"><span data-stu-id="31995-115">A P2S VPN connection is established by starting it from hello client computer.</span></span>

![Csatlakozás egy számítógép tooan Azure VNet - pont – hely kapcsolat diagramja](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

<span data-ttu-id="31995-117">Pont-pont tanúsítvány hitelesítési kapcsolatok hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="31995-117">Point-to-Site certificate authentication connections require hello following:</span></span>

* <span data-ttu-id="31995-118">Útvonalalapú VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="31995-118">A RouteBased VPN gateway.</span></span>
* <span data-ttu-id="31995-119">hello nyilvános kulcsát (.cer-fájl) egy legfelső szintű tanúsítvány, amely feltöltött tooAzure.</span><span class="sxs-lookup"><span data-stu-id="31995-119">hello public key (.cer file) for a root certificate, which is uploaded tooAzure.</span></span> <span data-ttu-id="31995-120">Hello tanúsítványt a feltöltést követően megbízható tanúsítvány minősül, és használják a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="31995-120">Once hello certificate is uploaded, it is considered a trusted certificate and is used for authentication.</span></span>
* <span data-ttu-id="31995-121">Hello legfelső szintű tanúsítvány által létrehozott és telepített minden egyes ügyfélszámítógépen toohello VNet csatlakozó ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="31995-121">A client certificate that is generated from hello root certificate and installed on each client computer that will connect toohello VNet.</span></span> <span data-ttu-id="31995-122">A rendszer ezt a tanúsítványt használja ügyfélhitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="31995-122">This certificate is used for client authentication.</span></span>
* <span data-ttu-id="31995-123">A VPN-ügyfél konfigurációs csomagja.</span><span class="sxs-lookup"><span data-stu-id="31995-123">A VPN client configuration package.</span></span> <span data-ttu-id="31995-124">hello VPN-ügyfélcsomag konfigurációs hello ügyfél tooconnect toohello VNet hello szükséges információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="31995-124">hello VPN client configuration package contains hello necessary information for hello client tooconnect toohello VNet.</span></span> <span data-ttu-id="31995-125">hello csomag hello meglévő VPN-ügyfél, amely natív toohello Windows operációs rendszer konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="31995-125">hello package configures hello existing VPN client that is native toohello Windows operating system.</span></span> <span data-ttu-id="31995-126">Minden ügyfél hello konfigurációs csomag használatával kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="31995-126">Each client that connects must be configured using hello configuration package.</span></span>

<span data-ttu-id="31995-127">A pont–hely kapcsolatok nem igényelnek VPN-eszközt vagy helyszíni nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="31995-127">Point-to-Site connections do not require a VPN device or an on-premises public-facing IP address.</span></span> <span data-ttu-id="31995-128">hello VPN-kapcsolaton keresztül SSTP (Secure Socket Tunneling Protocol) jön létre.</span><span class="sxs-lookup"><span data-stu-id="31995-128">hello VPN connection is created over SSTP (Secure Socket Tunneling Protocol).</span></span> <span data-ttu-id="31995-129">Hello kiszolgáló oldalán 1.0-s, 1.1-es és 1.2-es SSTP verziója támogatott.</span><span class="sxs-lookup"><span data-stu-id="31995-129">On hello server side, we support SSTP versions 1.0, 1.1, and 1.2.</span></span> <span data-ttu-id="31995-130">hello ügyfél úgy dönt, hogy melyik verzió toouse.</span><span class="sxs-lookup"><span data-stu-id="31995-130">hello client decides which version toouse.</span></span> <span data-ttu-id="31995-131">Windows 8.1 és újabb kiadások esetén az SSTP alapértelmezés szerint az 1.2 verziót használja.</span><span class="sxs-lookup"><span data-stu-id="31995-131">For Windows 8.1 and above, SSTP uses 1.2 by default.</span></span> 

<span data-ttu-id="31995-132">Pont – hely kapcsolatok kapcsolatos további információkért lásd: hello [pont-pont – gyakori kérdések](#faq) hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="31995-132">For more information about Point-to-Site connections, see hello [Point-to-Site FAQ](#faq) at hello end of this article.</span></span>

## <a name="before-beginning"></a><span data-ttu-id="31995-133">Mielőtt hozzálát</span><span class="sxs-lookup"><span data-stu-id="31995-133">Before beginning</span></span>

* <span data-ttu-id="31995-134">Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="31995-134">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="31995-135">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="31995-135">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="31995-136">Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="31995-136">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="31995-137">PowerShell-parancsmagok telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="31995-137">For more information about installing PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <span data-ttu-id="31995-138"><a name="example"></a>Példaértékek</span><span class="sxs-lookup"><span data-stu-id="31995-138"><a name="example"></a>Example values</span></span>

<span data-ttu-id="31995-139">Hello példa értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothese értékek toobetter hello jelen cikk példái a megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="31995-139">You can use hello example values toocreate a test environment, or refer toothese values toobetter understand hello examples in this article.</span></span> <span data-ttu-id="31995-140">Hello változók szakaszban hivatott [1](#declare) hello cikk.</span><span class="sxs-lookup"><span data-stu-id="31995-140">We set hello variables in section [1](#declare) of hello article.</span></span> <span data-ttu-id="31995-141">Hello lépésekkel egy útmutató, és hello értékeket használja őket módosítása nélkül, vagy módosítsa őket tooreflect a környezetben.</span><span class="sxs-lookup"><span data-stu-id="31995-141">You can either use hello steps as a walk-through and use hello values without changing them, or change them tooreflect your environment.</span></span> 

* <span data-ttu-id="31995-142">**Név: VNet1**</span><span class="sxs-lookup"><span data-stu-id="31995-142">**Name: VNet1**</span></span>
* <span data-ttu-id="31995-143">**Címtartomány: 192.168.0.0/16** és **10.254.0.0/16**</span><span class="sxs-lookup"><span data-stu-id="31995-143">**Address space: 192.168.0.0/16** and **10.254.0.0/16**</span></span><br><span data-ttu-id="31995-144">Az ebben a példában használjuk, amely ezt a konfigurációt az több címterek egynél több címet terület tooillustrate.</span><span class="sxs-lookup"><span data-stu-id="31995-144">For this example, we use more than one address space tooillustrate that this configuration works with multiple address spaces.</span></span> <span data-ttu-id="31995-145">Azonban nem kötelező több címtartományt megadni ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="31995-145">However, multiple address spaces are not required for this configuration.</span></span>
* <span data-ttu-id="31995-146">**Alhálózat neve: FrontEnd**</span><span class="sxs-lookup"><span data-stu-id="31995-146">**Subnet name: FrontEnd**</span></span>
  * <span data-ttu-id="31995-147">**Alhálózati címtartomány: 192.168.1.0/24**</span><span class="sxs-lookup"><span data-stu-id="31995-147">**Subnet address range: 192.168.1.0/24**</span></span>
* <span data-ttu-id="31995-148">**Alhálózat neve: BackEnd**</span><span class="sxs-lookup"><span data-stu-id="31995-148">**Subnet name: BackEnd**</span></span>
  * <span data-ttu-id="31995-149">**Alhálózati címtartomány: 10.254.1.0/24**</span><span class="sxs-lookup"><span data-stu-id="31995-149">**Subnet address range: 10.254.1.0/24**</span></span>
* <span data-ttu-id="31995-150">**Alhálózat neve: GatewaySubnet**</span><span class="sxs-lookup"><span data-stu-id="31995-150">**Subnet name: GatewaySubnet**</span></span><br><span data-ttu-id="31995-151">hello alhálózati név *GatewaySubnet* hello VPN gateway toowork esetén kötelező.</span><span class="sxs-lookup"><span data-stu-id="31995-151">hello Subnet name *GatewaySubnet* is mandatory for hello VPN gateway toowork.</span></span>
  * <span data-ttu-id="31995-152">**Átjáró-alhálózat címtartománya: 192.168.200.0/24**</span><span class="sxs-lookup"><span data-stu-id="31995-152">**GatewaySubnet address range: 192.168.200.0/24**</span></span> 
* <span data-ttu-id="31995-153">**VPN-ügyfelek címkészlete: 172.16.201.0/24**</span><span class="sxs-lookup"><span data-stu-id="31995-153">**VPN client address pool: 172.16.201.0/24**</span></span><br><span data-ttu-id="31995-154">A VPN-ügyfelek toohello VNet a pont-pont kapcsolattal csatlakozó fogadása hello Magánhálózati ügyfélcímkészlete IP-címet.</span><span class="sxs-lookup"><span data-stu-id="31995-154">VPN clients that connect toohello VNet using this Point-to-Site connection receive an IP address from hello VPN client address pool.</span></span>
* <span data-ttu-id="31995-155">**Előfizetés:** Ha egynél több előfizetéssel, győződjön meg arról, hogy rendelkezik a megfelelő hello.</span><span class="sxs-lookup"><span data-stu-id="31995-155">**Subscription:** If you have more than one subscription, verify that you are using hello correct one.</span></span>
* <span data-ttu-id="31995-156">**Erőforráscsoport: TestRG**</span><span class="sxs-lookup"><span data-stu-id="31995-156">**Resource Group: TestRG**</span></span>
* <span data-ttu-id="31995-157">**Hely: East US**</span><span class="sxs-lookup"><span data-stu-id="31995-157">**Location: East US**</span></span>
* <span data-ttu-id="31995-158">**DNS-kiszolgáló: IP-cím** hello DNS-kiszolgáló, amelyet az toouse a névfeloldáshoz.</span><span class="sxs-lookup"><span data-stu-id="31995-158">**DNS Server: IP address** of hello DNS server that you want toouse for name resolution.</span></span>
* <span data-ttu-id="31995-159">**Átjáró neve: Vnet1GW**</span><span class="sxs-lookup"><span data-stu-id="31995-159">**GW Name: Vnet1GW**</span></span>
* <span data-ttu-id="31995-160">**Nyilvános IP-név: VNet1GWPIP**</span><span class="sxs-lookup"><span data-stu-id="31995-160">**Public IP name: VNet1GWPIP**</span></span>
* <span data-ttu-id="31995-161">**VPN típusa: RouteBased**</span><span class="sxs-lookup"><span data-stu-id="31995-161">**VpnType: RouteBased**</span></span> 

## <span data-ttu-id="31995-162"><a name="declare"></a>1. Bejelentkezés és a változók beállítása</span><span class="sxs-lookup"><span data-stu-id="31995-162"><a name="declare"></a>1. Log in and set variables</span></span>

<span data-ttu-id="31995-163">Ebben a szakaszban be, és ehhez a konfigurációhoz használt hello értékek deklarálható.</span><span class="sxs-lookup"><span data-stu-id="31995-163">In this section, you log in and declare hello values used for this configuration.</span></span> <span data-ttu-id="31995-164">hello deklarált értékek hello mintaparancsfájlok használatosak.</span><span class="sxs-lookup"><span data-stu-id="31995-164">hello declared values are used in hello sample scripts.</span></span> <span data-ttu-id="31995-165">Hello értékek tooreflect módosítsa a saját környezetben.</span><span class="sxs-lookup"><span data-stu-id="31995-165">Change hello values tooreflect your own environment.</span></span> <span data-ttu-id="31995-166">Vagy használhat deklarált értékek hello és végrehajtania hello lépéseket, mint egy gyakorlatot.</span><span class="sxs-lookup"><span data-stu-id="31995-166">Or, you can use hello declared values and go through hello steps as an exercise.</span></span>

1. <span data-ttu-id="31995-167">Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="31995-167">Open your PowerShell console with elevated privileges, and log in tooyour Azure account.</span></span> <span data-ttu-id="31995-168">Ez a parancsmag kéri hello bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="31995-168">This cmdlet prompts you for hello login credentials.</span></span> <span data-ttu-id="31995-169">A bejelentkezés után az tölti le a fiókbeállításoknál, hogy-e elérhető tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="31995-169">After logging in, it downloads your account settings so that they are available tooAzure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```
2. <span data-ttu-id="31995-170">Olvassa be az Azure-előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="31995-170">Get a list of your Azure subscriptions.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
3. <span data-ttu-id="31995-171">Adja meg, hogy szeretné-e toouse hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="31995-171">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. <span data-ttu-id="31995-172">Deklarálja, hogy szeretné-e toouse hello változók.</span><span class="sxs-lookup"><span data-stu-id="31995-172">Declare hello variables that you want toouse.</span></span> <span data-ttu-id="31995-173">Hello használja, a következő mintát, és hello értékeket a saját, amikor erre szükség van.</span><span class="sxs-lookup"><span data-stu-id="31995-173">Use hello following sample, substituting hello values for your own when necessary.</span></span>

  ```powershell
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $DNS = "10.1.1.3"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <span data-ttu-id="31995-174"><a name="ConfigureVNet"></a>2. Virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="31995-174"><a name="ConfigureVNet"></a>2. Configure a VNet</span></span>

1. <span data-ttu-id="31995-175">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="31995-175">Create a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. <span data-ttu-id="31995-176">Hozzon létre hello alhálózati beállítások hello virtuális hálózathoz, azok elnevezési *előtér*, *háttér*, és *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="31995-176">Create hello subnet configurations for hello virtual network, naming them *FrontEnd*, *BackEnd*, and *GatewaySubnet*.</span></span> <span data-ttu-id="31995-177">Ezeket az előtagokat hello meg deklarált VNet címtér részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="31995-177">These prefixes must be part of hello VNet address space that you declared.</span></span>

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. <span data-ttu-id="31995-178">Hello virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="31995-178">Create hello virtual network.</span></span>

  <span data-ttu-id="31995-179">Ebben a példában a hello DNS-kiszolgáló nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="31995-179">In this example, hello DNS server is optional.</span></span> <span data-ttu-id="31995-180">Az érték megadásával nem jön létre új DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="31995-180">Specifying a value does not create a new DNS server.</span></span> <span data-ttu-id="31995-181">hello DNS kiszolgáló IP-cím megadott kell egy DNS-kiszolgáló, amely képes névfeloldásra hello hello erőforrásokhoz való kapcsolódás esetén.</span><span class="sxs-lookup"><span data-stu-id="31995-181">hello DNS server IP address that you specify should be a DNS server that can resolve hello names for hello resources you are connecting to.</span></span> <span data-ttu-id="31995-182">Ebben a példában a magánhálózati IP-cím használtuk, de valószínű, hogy ez nem hello IP-címet a DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="31995-182">For this example, we used a private IP address, but it is likely that this is not hello IP address of your DNS server.</span></span> <span data-ttu-id="31995-183">Lehet, hogy toouse a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="31995-183">Be sure toouse your own values.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
  ```
4. <span data-ttu-id="31995-184">Adja meg a létrehozott virtuális hálózat hello hello változók.</span><span class="sxs-lookup"><span data-stu-id="31995-184">Specify hello variables for hello virtual network you created.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. <span data-ttu-id="31995-185">Egy VPN Gateway-nek rendelkeznie kell nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="31995-185">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="31995-186">Először igényelnie hello IP-cím erőforrás, és ezután tooit hivatkozni, a virtuális hálózati átjáró létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="31995-186">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="31995-187">hello IP-cím dinamikusan toohello erőforrás van hozzárendelve, hello VPN-átjáró létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="31995-187">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="31995-188">A VPN Gateway jelenleg csak a *Dinamikus* nyilvános IP-cím lefoglalását támogatja.</span><span class="sxs-lookup"><span data-stu-id="31995-188">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="31995-189">Nem kérheti statikus IP-cím hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="31995-189">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="31995-190">Azonban ez nem jelenti azt, hogy hello IP-cím hozzá van rendelve tooyour VPN-átjáró után módosítja.</span><span class="sxs-lookup"><span data-stu-id="31995-190">However, it doesn't mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="31995-191">hello egyetlen alkalom hello nyilvános IP-cím módosításainak mikor van hello átjáró törlődik, és újból létrehozza.</span><span class="sxs-lookup"><span data-stu-id="31995-191">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="31995-192">Nem módosul átméretezés, alaphelyzetbe állítás, illetve a VPN Gateway belső karbantartása/frissítése során.</span><span class="sxs-lookup"><span data-stu-id="31995-192">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

  <span data-ttu-id="31995-193">Kérjen egy dinamikusan hozzárendelt nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="31995-193">Request a dynamically assigned public IP address.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <span data-ttu-id="31995-194"><a name="creategateway"></a>3. Hello VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="31995-194"><a name="creategateway"></a>3. Create hello VPN gateway</span></span>

<span data-ttu-id="31995-195">Konfigurálja, és a Vnethez tartozó hello virtuális hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="31995-195">Configure and create hello virtual network gateway for your VNet.</span></span>

* <span data-ttu-id="31995-196">Hello *- GatewayType* kell **Vpn** és hello *– VpnType* kell **RouteBased**.</span><span class="sxs-lookup"><span data-stu-id="31995-196">hello *-GatewayType* must be **Vpn** and hello *-VpnType* must be **RouteBased**.</span></span>
* <span data-ttu-id="31995-197">VPN-átjáró is eltarthat, too45 perc toocomplete, attól függően, hogy hello [átjáró-termékváltozat](vpn-gateway-about-vpn-gateway-settings.md) választja.</span><span class="sxs-lookup"><span data-stu-id="31995-197">A VPN gateway can take up too45 minutes toocomplete, depending on hello [gateway sku](vpn-gateway-about-vpn-gateway-settings.md) you select.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 `
```

## <span data-ttu-id="31995-198"><a name="addresspool"></a>4. Hello Magánhálózati ügyfélcímkészlete hozzáadása</span><span class="sxs-lookup"><span data-stu-id="31995-198"><a name="addresspool"></a>4. Add hello VPN client address pool</span></span>

<span data-ttu-id="31995-199">Miután hello VPN-átjáró végzett a létrehozással, hello Magánhálózati ügyfélcímkészlete is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="31995-199">After hello VPN gateway finishes creating, you can add hello VPN client address pool.</span></span> <span data-ttu-id="31995-200">hello Magánhálózati ügyfélcímkészlete, amelyből a hello VPN-ügyfelek IP-címet kap, kapcsolódáskor hello tartományon.</span><span class="sxs-lookup"><span data-stu-id="31995-200">hello VPN client address pool is hello range from which hello VPN clients receive an IP address when connecting.</span></span> <span data-ttu-id="31995-201">Használjon egy privát IP-címtartományt, amely nem fedi át hello helyszíni helyre történő csatlakozás vagy hello tooconnect a kívánt virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="31995-201">Use a private IP address range that does not overlap with hello on-premises location that you connect from, or with hello VNet that you want tooconnect to.</span></span> <span data-ttu-id="31995-202">Ebben a példában hello Magánhálózati ügyfélcímkészlete van deklarálva, egy [változó](#declare) 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="31995-202">In this example, hello VPN client address pool is declared as a [variable](#declare) in Step 1.</span></span>

```powershell
$Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <span data-ttu-id="31995-203"><a name="Certificates"></a>5. Tanúsítványok előállítása</span><span class="sxs-lookup"><span data-stu-id="31995-203"><a name="Certificates"></a>5. Generate certificates</span></span>

<span data-ttu-id="31995-204">Tanúsítványok pont-pont VPN Azure tooauthenticate VPN-ügyfelek által használt.</span><span class="sxs-lookup"><span data-stu-id="31995-204">Certificates are used by Azure tooauthenticate VPN clients for Point-to-Site VPNs.</span></span> <span data-ttu-id="31995-205">Hello nyilvánoskulcs-adatokat a hello legfelső szintű tanúsítvány tooAzure feltöltése.</span><span class="sxs-lookup"><span data-stu-id="31995-205">You upload hello public key information of hello root certificate tooAzure.</span></span> <span data-ttu-id="31995-206">nyilvános kulcs hello majd minősül "megbízható".</span><span class="sxs-lookup"><span data-stu-id="31995-206">hello public key is then considered 'trusted'.</span></span> <span data-ttu-id="31995-207">Ügyféltanúsítványok kell kell hello megbízható legfelső szintű tanúsítvány jön létre, és csak utána települ a hello tanúsítványok-aktuális felhasználó/személyes tanúsítványtárolóba minden egyes ügyfélszámítógépre.</span><span class="sxs-lookup"><span data-stu-id="31995-207">Client certificates must be generated from hello trusted root certificate, and then installed on each client computer in hello Certificates-Current User/Personal certificate store.</span></span> <span data-ttu-id="31995-208">hello tanúsítvány használt tooauthenticate hello ügyfél, amikor kezdeményezik a kapcsolat toohello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="31995-208">hello certificate is used tooauthenticate hello client when it initiates a connection toohello VNet.</span></span> 

<span data-ttu-id="31995-209">Önaláírt tanúsítványok használata esetén azokat megadott paraméterekkel kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="31995-209">If you use self-signed certificates, they must be created using specific parameters.</span></span> <span data-ttu-id="31995-210">Létrehozhat egy önaláírt tanúsítványt a hello utasításokat követve [PowerShell és Windows 10](vpn-gateway-certificates-point-to-site.md), vagy ha nincs Windows 10, [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).</span><span class="sxs-lookup"><span data-stu-id="31995-210">You can create a self-signed certificate using hello instructions for [PowerShell and Windows 10](vpn-gateway-certificates-point-to-site.md), or, if you don't have Windows 10, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).</span></span> <span data-ttu-id="31995-211">Fontos lépéseket hello hello utasítások önaláírt legfelső szintű tanúsítványok és az ügyféltanúsítványok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="31995-211">It's important that you follow hello steps in hello instructions when generating self-signed root certificates and client certificates.</span></span> <span data-ttu-id="31995-212">Ellenkező esetben létrehozhat hello tanúsítvány nem lesz kompatibilis a P2S-kapcsolatok, és hibaüzenet jelenik meg a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="31995-212">Otherwise, hello certificates you generate will not be compatible with P2S connections and you will receive a connection error.</span></span>

### <span data-ttu-id="31995-213"><a name="cer"></a>1. Hello .cer fájl hello legfelső szintű tanúsítvány beszerzése</span><span class="sxs-lookup"><span data-stu-id="31995-213"><a name="cer"></a>1. Obtain hello .cer file for hello root certificate</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <span data-ttu-id="31995-214"><a name="generate"></a>2. Ügyféltanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="31995-214"><a name="generate"></a>2. Generate a client certificate</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <span data-ttu-id="31995-215"><a name="upload"></a>6. Hello legfelső szintű tanúsítvány nyilvános kulcs adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="31995-215"><a name="upload"></a>6. Upload hello root certificate public key information</span></span>

<span data-ttu-id="31995-216">Ellenőrizze, hogy a VPN-átjáró létrehozása befejeződött-e.</span><span class="sxs-lookup"><span data-stu-id="31995-216">Verify that your VPN gateway has finished creating.</span></span> <span data-ttu-id="31995-217">Miután befejezte, hello .cer fájlját (hello nyilvánoskulcs-adatokat tartalmazó) a megbízható legfelső szintű tanúsítvány tooAzure feltölthet.</span><span class="sxs-lookup"><span data-stu-id="31995-217">Once it has completed, you can upload hello .cer file (which contains hello public key information) for a trusted root certificate tooAzure.</span></span> <span data-ttu-id="31995-218">A.cer fájl a feltöltést követően Azure használható tooauthenticate ügyfelek, amelyek telepítették a hello megbízható legfelső szintű tanúsítvány által létrehozott ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="31995-218">Once a.cer file is uploaded, Azure can use it tooauthenticate clients that have installed a client certificate generated from hello trusted root certificate.</span></span> <span data-ttu-id="31995-219">Szükség esetén további megbízható legfelső szintű tanúsítvány fájlok - tooa összesen 20 - később fel feltölthet.</span><span class="sxs-lookup"><span data-stu-id="31995-219">You can upload additional trusted root certificate files - up tooa total of 20 - later, if needed.</span></span>

1. <span data-ttu-id="31995-220">A tanúsítvány neve hello érték helyett a saját hello változó deklarálható.</span><span class="sxs-lookup"><span data-stu-id="31995-220">Declare hello variable for your certificate name, replacing hello value with your own.</span></span>

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. <span data-ttu-id="31995-221">Cserélje le hello fájl elérési útját a saját, és futtassa a hello parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="31995-221">Replace hello file path with your own, and then run hello cmdlets.</span></span>

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```
3. <span data-ttu-id="31995-222">Hello nyilvánoskulcs-adatokat tooAzure feltöltése.</span><span class="sxs-lookup"><span data-stu-id="31995-222">Upload hello public key information tooAzure.</span></span> <span data-ttu-id="31995-223">Hello tanúsítvány adatait a feltöltést követően Azure úgy ítéli meg, ez toobe megbízható főtanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="31995-223">Once hello certificate information is uploaded, Azure considers this toobe a trusted root certificate.</span></span>

   ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
  ```

## <span data-ttu-id="31995-224"><a name="clientconfig"></a>7. Hello VPN-ügyfélcsomag konfigurációs letöltése</span><span class="sxs-lookup"><span data-stu-id="31995-224"><a name="clientconfig"></a>7. Download hello VPN client configuration package</span></span>

<span data-ttu-id="31995-225">tooconnect tooa egy pont – hely VPN hálózatok, minden ügyfél telepítenie kell egy konfigurációs ügyfélcsomagot, amely hello beállításokkal konfigurálja a hello natív VPN-ügyfél és a szükséges tooconnect toohello virtuális hálózati fájlokat.</span><span class="sxs-lookup"><span data-stu-id="31995-225">tooconnect tooa VNet using a Point-to-Site VPN, each client must install a client configuration package that configures hello native VPN client with hello settings and files that are necessary tooconnect toohello virtual network.</span></span> <span data-ttu-id="31995-226">hello VPN-ügyfélcsomag konfigurációs hello natív Windows VPN-ügyfél konfigurálja, egy másik VPN-ügyfél nem telepít.</span><span class="sxs-lookup"><span data-stu-id="31995-226">hello VPN client configuration package configures hello native Windows VPN client, it doesn't install a new or different VPN client.</span></span> 

<span data-ttu-id="31995-227">Minden egyes ügyfélszámítógépre csomag azonos VPN-ügyfél konfigurációja hello mindaddig, amíg hello verzióegyezéseket hello architektúra hello ügyfél használhatja.</span><span class="sxs-lookup"><span data-stu-id="31995-227">You can use hello same VPN client configuration package on each client computer, as long as hello version matches hello architecture for hello client.</span></span> <span data-ttu-id="31995-228">Ügyfél által támogatott operációs rendszerek hello listájáért lásd: hello [pont – hely kapcsolatok gyakran ismételt kérdések](#faq) hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="31995-228">For hello list of client operating systems that are supported, see hello [Point-to-Site connections FAQ](#faq) at hello end of this article.</span></span>

1. <span data-ttu-id="31995-229">Hello átjáró létrehozása után létrehozhat és hello ügyfél konfigurációs csomag.</span><span class="sxs-lookup"><span data-stu-id="31995-229">After hello gateway has been created, you can generate and download hello client configuration package.</span></span> <span data-ttu-id="31995-230">Ez a példa hello csomagot tölti le a 64 bites ügyfeleken.</span><span class="sxs-lookup"><span data-stu-id="31995-230">This example downloads hello package for 64-bit clients.</span></span> <span data-ttu-id="31995-231">Ha azt szeretné, hogy toodownload hello 32 bites ügyfél, a "Amd64" lecserélése "x86".</span><span class="sxs-lookup"><span data-stu-id="31995-231">If you want toodownload hello 32-bit client, replace 'Amd64' with 'x86'.</span></span> <span data-ttu-id="31995-232">Emellett letöltheti hello VPN-ügyfél hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="31995-232">You can also download hello VPN client by using hello Azure portal.</span></span>

  ```powershell
  Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
  -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
  ```
2. <span data-ttu-id="31995-233">Másolással illessze be a visszaadott tooa webes böngésző toodownload hello csomag, ügyelve tooremove hello ajánlatok hello hivatkozás körülvevő hello hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="31995-233">Copy and paste hello link that is returned tooa web browser toodownload hello package, taking care tooremove hello quotes surrounding hello link.</span></span> 
3. <span data-ttu-id="31995-234">Töltse le, és telepítse a hello csomag hello ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="31995-234">Download and install hello package on hello client computer.</span></span> <span data-ttu-id="31995-235">Ha megjelenik a SmartScreen egy előugró ablaka, kattintson a **További információ**, majd a **Futtatás mindenképpen** elemre.</span><span class="sxs-lookup"><span data-stu-id="31995-235">If you see a SmartScreen popup, click **More info**, then **Run anyway**.</span></span> <span data-ttu-id="31995-236">Egyéb ügyfélszámítógépeire hello csomag tooinstall is mentheti.</span><span class="sxs-lookup"><span data-stu-id="31995-236">You can also save hello package tooinstall on other client computers.</span></span>
4. <span data-ttu-id="31995-237">Hello ügyfélszámítógépen nyissa meg túl**hálózati beállítások** kattintson **VPN**.</span><span class="sxs-lookup"><span data-stu-id="31995-237">On hello client computer, navigate too**Network Settings** and click **VPN**.</span></span> <span data-ttu-id="31995-238">VPN-kapcsolat hello hello csatlakozó virtuális hálózati hello nevét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="31995-238">hello VPN connection shows hello name of hello virtual network that it connects to.</span></span>

## <span data-ttu-id="31995-239"><a name="clientcertificate"></a>8. Exportált ügyféltanúsítvány telepítése</span><span class="sxs-lookup"><span data-stu-id="31995-239"><a name="clientcertificate"></a>8. Install an exported client certificate</span></span>

<span data-ttu-id="31995-240">Ha azt szeretné, hogy egy P2S toocreate kapcsolat eltérő hello ügyfélszámítógépről egy használt toogenerate hello ügyféltanúsítványokat, tooinstall ügyféltanúsítvány szükséges.</span><span class="sxs-lookup"><span data-stu-id="31995-240">If you want toocreate a P2S connection from a client computer other than hello one you used toogenerate hello client certificates, you need tooinstall a client certificate.</span></span> <span data-ttu-id="31995-241">Ügyfél-tanúsítvány telepítése, úgy kell hello jelszó hello ügyfél tanúsítvány exportálása során létrejött.</span><span class="sxs-lookup"><span data-stu-id="31995-241">When installing a client certificate, you need hello password that was created when hello client certificate was exported.</span></span> <span data-ttu-id="31995-242">Ez általában csak egy függetlenül attól, hogy duplán hello tanúsítványt, és telepíti azt.</span><span class="sxs-lookup"><span data-stu-id="31995-242">Typically, it is just a matter of double-clicking hello certificate and installing it.</span></span>

<span data-ttu-id="31995-243">Ellenőrizze, hogy hello ügyféltanúsítvány egy .pfx együtt hello teljes láncát (amely hello alapértelmezett) típusúként lett exportálva.</span><span class="sxs-lookup"><span data-stu-id="31995-243">Make sure hello client certificate was exported as a .pfx along with hello entire certificate chain (which is hello default).</span></span> <span data-ttu-id="31995-244">Ellenkező esetben hello legfelső szintű tanúsítvány adatait nincs jelen hello ügyfélszámítógépen, és hello ügyfél megfelelően nem fogja tudni tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="31995-244">Otherwise, hello root certificate information isn't present on hello client computer and hello client won't be able tooauthenticate properly.</span></span> <span data-ttu-id="31995-245">További információkért lásd az [exportált ügyféltanúsítványok telepítését](vpn-gateway-certificates-point-to-site.md#install) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="31995-245">For more information, see [Install an exported client certificate](vpn-gateway-certificates-point-to-site.md#install).</span></span> 

## <span data-ttu-id="31995-246"><a name="connect"></a>9. Csatlakozás tooAzure</span><span class="sxs-lookup"><span data-stu-id="31995-246"><a name="connect"></a>9. Connect tooAzure</span></span>

1. <span data-ttu-id="31995-247">tooconnect tooyour VNet hello ügyfélszámítógépen nyissa meg a tooVPN kapcsolatok, és keresse meg a létrehozott hello VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="31995-247">tooconnect tooyour VNet, on hello client computer, navigate tooVPN connections and locate hello VPN connection that you created.</span></span> <span data-ttu-id="31995-248">Hello azonos nevet a virtuális hálózatnak nevezik.</span><span class="sxs-lookup"><span data-stu-id="31995-248">It is named hello same name as your virtual network.</span></span> <span data-ttu-id="31995-249">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="31995-249">Click **Connect**.</span></span> <span data-ttu-id="31995-250">Előugró üzenet jelenhet meg, hogy toousing hello tanúsítvány hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="31995-250">A pop-up message may appear that refers toousing hello certificate.</span></span> <span data-ttu-id="31995-251">Kattintson a **Folytatás** toouse emelt szintű jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="31995-251">Click **Continue** toouse elevated privileges.</span></span> 
2. <span data-ttu-id="31995-252">A hello **kapcsolat** állapotlapon, kattintson a **Connect** toostart hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="31995-252">On hello **Connection** status page, click **Connect** toostart hello connection.</span></span> <span data-ttu-id="31995-253">Ha megjelenik egy **tanúsítvány kiválasztása** képernyőn, győződjön meg arról, hogy hello ügyfél tanúsítvány ábrázoló, amelyet az toouse tooconnect egy hello.</span><span class="sxs-lookup"><span data-stu-id="31995-253">If you see a **Select Certificate** screen, verify that hello client certificate showing is hello one that you want toouse tooconnect.</span></span> <span data-ttu-id="31995-254">Ha nem, hello nyílra tooselect hello megfelelő tanúsítványt használjon, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="31995-254">If it is not, use hello drop-down arrow tooselect hello correct certificate, and then click **OK**.</span></span>

  ![VPN-ügyfél kapcsolódik tooAzure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. <span data-ttu-id="31995-256">A kapcsolat létrejött.</span><span class="sxs-lookup"><span data-stu-id="31995-256">Your connection is established.</span></span>

  ![A kapcsolat létrejött](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-p2s-connections"></a><span data-ttu-id="31995-258">Pont–hely kapcsolatok hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="31995-258">Troubleshooting P2S connections</span></span>

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <span data-ttu-id="31995-259"><a name="verify"></a>10. A kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="31995-259"><a name="verify"></a>10. Verify your connection</span></span>

1. <span data-ttu-id="31995-260">tooverify, hogy a VPN-kapcsolatot az aktív, nyisson meg egy rendszergazda jogú parancssort, és futtassa *ipconfig/all*.</span><span class="sxs-lookup"><span data-stu-id="31995-260">tooverify that your VPN connection is active, open an elevated command prompt, and run *ipconfig/all*.</span></span>
2. <span data-ttu-id="31995-261">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="31995-261">View hello results.</span></span> <span data-ttu-id="31995-262">Láthatja, hogy hello IP-cím kapott hello hello pont-pont Magánhálózati Ügyfélcímkészlete a konfigurációban megadott címek egyikét.</span><span class="sxs-lookup"><span data-stu-id="31995-262">Notice that hello IP address you received is one of hello addresses within hello Point-to-Site VPN Client Address Pool that you specified in your configuration.</span></span> <span data-ttu-id="31995-263">hello eredményei hasonló toothis példa:</span><span class="sxs-lookup"><span data-stu-id="31995-263">hello results are similar toothis example:</span></span>

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

## <span data-ttu-id="31995-264"><a name="connectVM"></a>Csatlakoztassa tooa virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="31995-264"><a name="connectVM"></a>Connect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <span data-ttu-id="31995-265"><a name="addremovecert"></a>Főtanúsítvány hozzáadása vagy eltávolítása</span><span class="sxs-lookup"><span data-stu-id="31995-265"><a name="addremovecert"></a>Add or remove a root certificate</span></span>

<span data-ttu-id="31995-266">A megbízható főtanúsítványokat felveheti vagy el is távolíthatja az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="31995-266">You can add and remove trusted root certificates from Azure.</span></span> <span data-ttu-id="31995-267">Ha eltávolít egy legfelső szintű tanúsítványt, hello legfelső szintű tanúsítványt generált tanúsítvánnyal rendelkező ügyfelek nem tudják hitelesíteni magukat, és nem fogja tudni tooconnect.</span><span class="sxs-lookup"><span data-stu-id="31995-267">When you remove a root certificate, clients that have a certificate generated from hello root certificate can't authenticate and won't be able tooconnect.</span></span> <span data-ttu-id="31995-268">Ha szeretné, hogy egy ügyfél tooauthenticate, és csatlakozni tud kell tooinstall (feltöltött) tooAzure megbízható legfelső szintű tanúsítványokat létre egy új ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="31995-268">If you want a client tooauthenticate and connect, you need tooinstall a new client certificate generated from a root certificate that is trusted (uploaded) tooAzure.</span></span>

### <span data-ttu-id="31995-269"><a name="addtrustedroot"></a>a megbízható legfelső szintű tanúsítvány tooadd</span><span class="sxs-lookup"><span data-stu-id="31995-269"><a name="addtrustedroot"></a>tooadd a trusted root certificate</span></span>

<span data-ttu-id="31995-270">Másolatot too20 legfelső szintű tanúsítvány .cer fájlok tooAzure adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="31995-270">You can add up too20 root certificate .cer files tooAzure.</span></span> <span data-ttu-id="31995-271">hello alábbi lépéseit súgó legfelső szintű tanúsítvány hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="31995-271">hello following steps help you add a root certificate:</span></span>

#### <span data-ttu-id="31995-272"><a name="certmethod1"></a>1. módszer:</span><span class="sxs-lookup"><span data-stu-id="31995-272"><a name="certmethod1"></a>Method 1</span></span>

<span data-ttu-id="31995-273">Ez a hello leghatékonyabb módszer tooupload egy legfelső szintű tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="31995-273">This is hello most efficient method tooupload a root certificate.</span></span>

1. <span data-ttu-id="31995-274">Hello .cer fájl tooupload előkészítése:</span><span class="sxs-lookup"><span data-stu-id="31995-274">Prepare hello .cer file tooupload:</span></span>

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert3.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
  ```
2. <span data-ttu-id="31995-275">Hello-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="31995-275">Upload hello file.</span></span> <span data-ttu-id="31995-276">Egyszerre csak egy fájlt tölthet fel.</span><span class="sxs-lookup"><span data-stu-id="31995-276">You can only upload one file at a time.</span></span>

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
  ```

3. <span data-ttu-id="31995-277">tooverify adott hello tanúsítványfájl feltöltött:</span><span class="sxs-lookup"><span data-stu-id="31995-277">tooverify that hello certificate file uploaded:</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

#### <span data-ttu-id="31995-278"><a name="certmethod2"></a>2. módszer:</span><span class="sxs-lookup"><span data-stu-id="31995-278"><a name="certmethod2"></a>Method 2</span></span>

<span data-ttu-id="31995-279">Ez a módszer akkor metódus 1-nél több lépésből áll, de van hello ugyanazt az eredményt.</span><span class="sxs-lookup"><span data-stu-id="31995-279">This method is has more steps than Method 1, but has hello same result.</span></span> <span data-ttu-id="31995-280">Szerepel abban az esetben szüksége tooview hello tanúsítványának adatait.</span><span class="sxs-lookup"><span data-stu-id="31995-280">It is included in case you need tooview hello certificate data.</span></span>

1. <span data-ttu-id="31995-281">Hozzon létre, és készítse elő a hello új legfelső szintű tanúsítvány tooadd tooAzure.</span><span class="sxs-lookup"><span data-stu-id="31995-281">Create and prepare hello new root certificate tooadd tooAzure.</span></span> <span data-ttu-id="31995-282">Hello nyilvános kulcsának exportálásához, mint egy Base-64 kódolású X.509 (. CER), majd nyissa meg szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="31995-282">Export hello public key as a Base-64 encoded X.509 (.CER) and open it with a text editor.</span></span> <span data-ttu-id="31995-283">Hello értékek, másolása, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="31995-283">Copy hello values, as shown in hello following example:</span></span>

  ![tanúsítvány](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > <span data-ttu-id="31995-285">Ha hello Tanúsítványadatok másol, győződjön meg arról, hogy egy folyamatos sorba kocsivissza és soremelés nélkül hello szöveg másolása.</span><span class="sxs-lookup"><span data-stu-id="31995-285">When copying hello certificate data, make sure that you copy hello text as one continuous line without carriage returns or line feeds.</span></span> <span data-ttu-id="31995-286">Szükség lehet toomodify a nézeten belül hello text editor too'Show szimbólum/megjelenítése összes karakter toosee hello kocsivissza értéket ad vissza, és hírcsatornák sor.</span><span class="sxs-lookup"><span data-stu-id="31995-286">You may need toomodify your view in hello text editor too'Show Symbol/Show all characters' toosee hello carriage returns and line feeds.</span></span>
  >
  >

2. <span data-ttu-id="31995-287">Adja meg a hello tanúsítvány és kulcs adatai változóként.</span><span class="sxs-lookup"><span data-stu-id="31995-287">Specify hello certificate name and key information as a variable.</span></span> <span data-ttu-id="31995-288">Cserélje le a saját hello az alábbi példa hello információkat:</span><span class="sxs-lookup"><span data-stu-id="31995-288">Replace hello information with your own, as shown in hello following example:</span></span>

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. <span data-ttu-id="31995-289">Hello új legfelső szintű tanúsítvány hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="31995-289">Add hello new root certificate.</span></span> <span data-ttu-id="31995-290">Egyszerre csak egy tanúsítványt adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="31995-290">You can only add one certificate at a time.</span></span>

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. <span data-ttu-id="31995-291">Hello új tanúsítvány helyesen lett-e hozzáadva a következő példa hello segítségével ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="31995-291">You can verify that hello new certificate was added correctly by using hello following example:</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <span data-ttu-id="31995-292"><a name="removerootcert"></a>egy legfelső szintű tanúsítványt tooremove</span><span class="sxs-lookup"><span data-stu-id="31995-292"><a name="removerootcert"></a>tooremove a root certificate</span></span>

1. <span data-ttu-id="31995-293">Hello változó deklarálható.</span><span class="sxs-lookup"><span data-stu-id="31995-293">Declare hello variables.</span></span>

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. <span data-ttu-id="31995-294">Távolítsa el a hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="31995-294">Remove hello certificate.</span></span>

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. <span data-ttu-id="31995-295">A következő példa tooverify, hogy a tanúsítvány hello használata hello sikeresen el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="31995-295">Use hello following example tooverify that hello certificate was removed successfully.</span></span>

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <span data-ttu-id="31995-296"><a name="revoke"></a>Ügyféltanúsítvány visszavonása</span><span class="sxs-lookup"><span data-stu-id="31995-296"><a name="revoke"></a>Revoke a client certificate</span></span>

<span data-ttu-id="31995-297">Az ügyféltanúsítványokat vissza lehet vonni.</span><span class="sxs-lookup"><span data-stu-id="31995-297">You can revoke client certificates.</span></span> <span data-ttu-id="31995-298">hello tanúsítvány-visszavonási lista lehetővé teszi a tooselectively visszautasítja a pont – hely kapcsolat egyedi ügyféltanúsítványok alapján.</span><span class="sxs-lookup"><span data-stu-id="31995-298">hello certificate revocation list allows you tooselectively deny Point-to-Site connectivity based on individual client certificates.</span></span> <span data-ttu-id="31995-299">Ez a folyamat eltér a megbízható főtanúsítvány eltávolításától.</span><span class="sxs-lookup"><span data-stu-id="31995-299">This is different than removing a trusted root certificate.</span></span> <span data-ttu-id="31995-300">Ha eltávolítja a megbízható legfelső szintű tanúsítvány .cer az Azure-ból, azt minden tanúsítványt generált/aláírt hello visszavont legfelső szintű tanúsítvány hello hozzáférés visszavonása.</span><span class="sxs-lookup"><span data-stu-id="31995-300">If you remove a trusted root certificate .cer from Azure, it revokes hello access for all client certificates generated/signed by hello revoked root certificate.</span></span> <span data-ttu-id="31995-301">Ügyfél-tanúsítvány visszavonásával, ahelyett, hogy a főtanúsítvány hello, lehetővé teszi, hogy hello más is létrehozott, hello legfelső szintű tanúsítvány toocontinue toobe hitelesítéshez használt tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="31995-301">Revoking a client certificate, rather than hello root certificate, allows hello other certificates that were generated from hello root certificate toocontinue toobe used for authentication.</span></span>

<span data-ttu-id="31995-302">hello általános gyakorlat toouse hello legfelső szintű tanúsítvány toomanage hozzáférés csapat vagy szervezet szinten egyéni felhasználók számára a minden részletre kiterjedő hozzáférés-vezérléshez visszavont ügyféltanúsítványok használata során.</span><span class="sxs-lookup"><span data-stu-id="31995-302">hello common practice is toouse hello root certificate toomanage access at team or organization levels, while using revoked client certificates for fine-grained access control on individual users.</span></span>

### <span data-ttu-id="31995-303"><a name="revokeclientcert"></a>toorevoke ügyféltanúsítványt</span><span class="sxs-lookup"><span data-stu-id="31995-303"><a name="revokeclientcert"></a>toorevoke a client certificate</span></span>

1. <span data-ttu-id="31995-304">Hello ügyfél tanúsítványának ujjlenyomata beolvasása.</span><span class="sxs-lookup"><span data-stu-id="31995-304">Retrieve hello client certificate thumbprint.</span></span> <span data-ttu-id="31995-305">További információkért lásd: [hogyan tooretrieve hello tanúsítvány ujjlenyomata](https://msdn.microsoft.com/library/ms734695.aspx).</span><span class="sxs-lookup"><span data-stu-id="31995-305">For more information, see [How tooretrieve hello Thumbprint of a Certificate](https://msdn.microsoft.com/library/ms734695.aspx).</span></span>
2. <span data-ttu-id="31995-306">Másolja a hello információk tooa szövegszerkesztőben, és úgy, hogy egy folyamatos karakterláncként, távolítsa el az összes szóközöket.</span><span class="sxs-lookup"><span data-stu-id="31995-306">Copy hello information tooa text editor and remove all spaces so that it is a continuous string.</span></span> <span data-ttu-id="31995-307">Ez a karakterlánc a következő lépésben hello van deklarálva, egy változó.</span><span class="sxs-lookup"><span data-stu-id="31995-307">This string is declared as a variable in hello next step.</span></span>
3. <span data-ttu-id="31995-308">Hello változó deklarálható.</span><span class="sxs-lookup"><span data-stu-id="31995-308">Declare hello variables.</span></span> <span data-ttu-id="31995-309">Győződjön meg arról, hogy toodeclare hello ujjlenyomat lekért hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="31995-309">Make sure toodeclare hello thumbprint you retrieved in hello previous step.</span></span>

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. <span data-ttu-id="31995-310">Adja hozzá a hello ujjlenyomat toohello visszavont tanúsítványok listáját.</span><span class="sxs-lookup"><span data-stu-id="31995-310">Add hello thumbprint toohello list of revoked certificates.</span></span> <span data-ttu-id="31995-311">Megjelenik az "Succeeded" hello ujjlenyomat hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="31995-311">You see "Succeeded" when hello thumbprint has been added.</span></span>

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. <span data-ttu-id="31995-312">Győződjön meg arról, hogy hello ujjlenyomat hozzá lett adva toohello visszavont tanúsítványok listáját.</span><span class="sxs-lookup"><span data-stu-id="31995-312">Verify that hello thumbprint was added toohello certificate revocation list.</span></span>

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. <span data-ttu-id="31995-313">Hello ujjlenyomat hozzáadását követően hello tanúsítvány már nem használt tooconnect lehet.</span><span class="sxs-lookup"><span data-stu-id="31995-313">After hello thumbprint has been added, hello certificate can no longer be used tooconnect.</span></span> <span data-ttu-id="31995-314">Ügyfelek, amelyek ezzel a tanúsítvánnyal tooconnect kap üzenetet kap arról, hogy hello tanúsítvány hatályát veszti.</span><span class="sxs-lookup"><span data-stu-id="31995-314">Clients that try tooconnect using this certificate receive a message saying that hello certificate is no longer valid.</span></span>

### <span data-ttu-id="31995-315"><a name="reinstateclientcert"></a>tooreinstate ügyféltanúsítványt</span><span class="sxs-lookup"><span data-stu-id="31995-315"><a name="reinstateclientcert"></a>tooreinstate a client certificate</span></span>

<span data-ttu-id="31995-316">Ügyféltanúsítvány visszaállíthatja hello ujjlenyomat eltávolítása a visszavont ügyféltanúsítványok hello listája.</span><span class="sxs-lookup"><span data-stu-id="31995-316">You can reinstate a client certificate by removing hello thumbprint from hello list of revoked client certificates.</span></span>

1. <span data-ttu-id="31995-317">Hello változó deklarálható.</span><span class="sxs-lookup"><span data-stu-id="31995-317">Declare hello variables.</span></span> <span data-ttu-id="31995-318">Ellenőrizze, hogy deklarálhatja hello megfelelő, amelyet az tooreinstate hello-tanúsítványának ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="31995-318">Make sure you declare hello correct thumbprint for hello certificate that you want tooreinstate.</span></span>

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. <span data-ttu-id="31995-319">Távolítsa el a hello tanúsítvány ujjlenyomata a hello visszavont tanúsítványok listáját.</span><span class="sxs-lookup"><span data-stu-id="31995-319">Remove hello certificate thumbprint from hello certificate revocation list.</span></span>

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. <span data-ttu-id="31995-320">Ellenőrizze, hogy hello ujjlenyomata a rendszer eltávolítja a hello lista visszavonva.</span><span class="sxs-lookup"><span data-stu-id="31995-320">Check if hello thumbprint is removed from hello revoked list.</span></span>

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <span data-ttu-id="31995-321"><a name="faq"></a>Pont–hely kapcsolatok – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="31995-321"><a name="faq"></a>Point-to-Site FAQ</span></span>

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="31995-322">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31995-322">Next steps</span></span>
<span data-ttu-id="31995-323">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="31995-323">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="31995-324">További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="31995-324">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span> <span data-ttu-id="31995-325">További információ a hálózati és a virtuális gépek toounderstand lásd: [Azure és a Linux virtuális gép hálózati áttekintés](../virtual-machines/linux/azure-vm-network-overview.md).</span><span class="sxs-lookup"><span data-stu-id="31995-325">toounderstand more about networking and virtual machines, see [Azure and Linux VM network overview](../virtual-machines/linux/azure-vm-network-overview.md).</span></span>
