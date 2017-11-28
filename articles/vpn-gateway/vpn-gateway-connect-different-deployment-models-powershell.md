---
title: "Csatlakoztassa a klasszikus virtuális hálózatok tooAzure erőforrás-kezelő Vnetek: PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate klasszikus virtuális hálózatokat és erőforrás-kezelő Vnetek VPN-átjáró és a PowerShell használatával közötti VPN-kapcsolat"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: 8b1cf6ae4becf1829fa99961c5dd09a422fcc1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a><span data-ttu-id="2775b-103">Különböző üzemi modellekből származó virtuális hálózatok összekapcsolása a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="2775b-103">Connect virtual networks from different deployment models using PowerShell</span></span>



<span data-ttu-id="2775b-104">Ez a cikk bemutatja, hogyan tooconnect hagyományos Vneteket tooResource Manager Vnetek tooallow hello hello külön telepítési modellek toocommunicate egymás mellett található erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="2775b-104">This article shows you how tooconnect classic VNets tooResource Manager VNets tooallow hello resources located in hello separate deployment models toocommunicate with each other.</span></span> <span data-ttu-id="2775b-105">hello cikkben leírt lépéseket a PowerShell segítségével, de ez a konfiguráció hello cikk jelöl ki a listában hello Azure-portál használatával is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="2775b-105">hello steps in this article use PowerShell, but you can also create this configuration using hello Azure portal by selecting hello article from this list.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2775b-106">Portál</span><span class="sxs-lookup"><span data-stu-id="2775b-106">Portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="2775b-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2775b-107">PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

<span data-ttu-id="2775b-108">Csatlakozás egy klasszikus virtuális hálózat tooa erőforrás-kezelő virtuális hálózat hasonló tooconnecting egy VNet tooan helyszíni hely.</span><span class="sxs-lookup"><span data-stu-id="2775b-108">Connecting a classic VNet tooa Resource Manager VNet is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="2775b-109">Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat.</span><span class="sxs-lookup"><span data-stu-id="2775b-109">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="2775b-110">Létrehozhat, amelyek különböző előfizetésekhez és különböző régiókban Vnetek közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="2775b-110">You can create a connection between VNets that are in different subscriptions and in different regions.</span></span> <span data-ttu-id="2775b-111">Kapcsolatok tooon helyszíni hálózatokban, már rendelkező Vnetek mindaddig, amíg hello átjáró, amely a konfigurálva a dinamikus- vagy útválasztó-alapú is csatlakoztathatja.</span><span class="sxs-lookup"><span data-stu-id="2775b-111">You can also connect VNets that already have connections tooon-premises networks, as long as hello gateway that they have been configured with is dynamic or route-based.</span></span> <span data-ttu-id="2775b-112">VNet – VNet kapcsolatokhoz kapcsolatos további információkért lásd: hello [VNet – VNet – gyakori kérdések](#faq) hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="2775b-112">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span> 

<span data-ttu-id="2775b-113">Ha a Vnetek hello ugyanabban a régióban, érdemes lehet tooinstead vegye figyelembe, hogy csatlakoztatja őket a Vnetben társviszony-létesítés használatával.</span><span class="sxs-lookup"><span data-stu-id="2775b-113">If your VNets are in hello same region, you may want tooinstead consider connecting them using VNet Peering.</span></span> <span data-ttu-id="2775b-114">A virtuális hálózatok közötti társviszony nem használ VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="2775b-114">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="2775b-115">További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2775b-115">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> 

## <a name="before-beginning"></a><span data-ttu-id="2775b-116">Mielőtt hozzálát</span><span class="sxs-lookup"><span data-stu-id="2775b-116">Before beginning</span></span>

<span data-ttu-id="2775b-117">hello következő lépések végigvezetik Önt hello beállítások szükséges tooconfigure dinamikus- vagy útválasztó-alapú átjáró minden vnet és hello átjárók közötti VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="2775b-117">hello following steps walk you through hello settings necessary tooconfigure a dynamic or route-based gateway for each VNet and create a VPN connection between hello gateways.</span></span> <span data-ttu-id="2775b-118">Ez a konfiguráció nem támogatja a statikus vagy csoportházirend-alapú átjárók.</span><span class="sxs-lookup"><span data-stu-id="2775b-118">This configuration does not support static or policy-based gateways.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="2775b-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2775b-119">Prerequisites</span></span>

* <span data-ttu-id="2775b-120">Mindkét Vnetek létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="2775b-120">Both VNets have already been created.</span></span>
* <span data-ttu-id="2775b-121">hello címtartományát hello Vnetek nem lehetnek átfedésben vannak egymással, vagy átfedésben vannak egyetlen hello tartományok hello átjárók csatlakoztathatók más kapcsolatok esetében.</span><span class="sxs-lookup"><span data-stu-id="2775b-121">hello address ranges for hello VNets do not overlap with each other, or overlap with any of hello ranges for other connections that hello gateways may be connected to.</span></span>
* <span data-ttu-id="2775b-122">Hello legújabb PowerShell-parancsmagok telepítése.</span><span class="sxs-lookup"><span data-stu-id="2775b-122">You have installed hello latest PowerShell cmdlets.</span></span> <span data-ttu-id="2775b-123">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) további információt.</span><span class="sxs-lookup"><span data-stu-id="2775b-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span> <span data-ttu-id="2775b-124">Ellenőrizze, hogy hello szolgáltatás-felügyeleti (SM) és erőforrás-kezelő (RM) parancsmagok hello telepítenie.</span><span class="sxs-lookup"><span data-stu-id="2775b-124">Make sure you install both hello Service Management (SM) and hello Resource Manager (RM) cmdlets.</span></span> 

### <span data-ttu-id="2775b-125"><a name="exampleref"></a>Példabeállítások</span><span class="sxs-lookup"><span data-stu-id="2775b-125"><a name="exampleref"></a>Example settings</span></span>

<span data-ttu-id="2775b-126">Ezen értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothem toobetter hello jelen cikk példái a megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="2775b-126">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

<span data-ttu-id="2775b-127">**Klasszikus virtuális hálózat beállításai**</span><span class="sxs-lookup"><span data-stu-id="2775b-127">**Classic VNet settings**</span></span>

<span data-ttu-id="2775b-128">Virtuális hálózat neve = ClassicVNet</span><span class="sxs-lookup"><span data-stu-id="2775b-128">VNet Name = ClassicVNet</span></span> <br>
<span data-ttu-id="2775b-129">Hely = USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="2775b-129">Location = West US</span></span> <br>
<span data-ttu-id="2775b-130">Virtuális hálózat = 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2775b-130">Virtual Network Address Spaces = 10.0.0.0/24</span></span> <br>
<span data-ttu-id="2775b-131">Alhálózat-1 = 10.0.0.0/27</span><span class="sxs-lookup"><span data-stu-id="2775b-131">Subnet-1 = 10.0.0.0/27</span></span> <br>
<span data-ttu-id="2775b-132">A GatewaySubnet = 10.0.0.32/29</span><span class="sxs-lookup"><span data-stu-id="2775b-132">GatewaySubnet = 10.0.0.32/29</span></span> <br>
<span data-ttu-id="2775b-133">Helyi hálózati név = RMVNetLocal</span><span class="sxs-lookup"><span data-stu-id="2775b-133">Local Network Name = RMVNetLocal</span></span> <br>
<span data-ttu-id="2775b-134">GatewayType = DynamicRouting</span><span class="sxs-lookup"><span data-stu-id="2775b-134">GatewayType = DynamicRouting</span></span>

<span data-ttu-id="2775b-135">**Erőforrás-kezelő VNet beállításait**</span><span class="sxs-lookup"><span data-stu-id="2775b-135">**Resource Manager VNet settings**</span></span>

<span data-ttu-id="2775b-136">Virtuális hálózat neve = RMVNet</span><span class="sxs-lookup"><span data-stu-id="2775b-136">VNet Name = RMVNet</span></span> <br>
<span data-ttu-id="2775b-137">Erőforráscsoport = RG1</span><span class="sxs-lookup"><span data-stu-id="2775b-137">Resource Group = RG1</span></span> <br>
<span data-ttu-id="2775b-138">Virtuális hálózati IP-címterek = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="2775b-138">Virtual Network IP Address Spaces = 192.168.0.0/16</span></span> <br>
<span data-ttu-id="2775b-139">Alhálózat-1 = 192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="2775b-139">Subnet-1 = 192.168.1.0/24</span></span> <br>
<span data-ttu-id="2775b-140">A GatewaySubnet = 192.168.0.0/26</span><span class="sxs-lookup"><span data-stu-id="2775b-140">GatewaySubnet = 192.168.0.0/26</span></span> <br>
<span data-ttu-id="2775b-141">Hely = USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="2775b-141">Location = East US</span></span> <br>
<span data-ttu-id="2775b-142">Átjáró nyilvános IP-név = gwpip</span><span class="sxs-lookup"><span data-stu-id="2775b-142">Gateway public IP name = gwpip</span></span> <br>
<span data-ttu-id="2775b-143">Helyi hálózati átjáró = ClassicVNetLocal</span><span class="sxs-lookup"><span data-stu-id="2775b-143">Local Network Gateway = ClassicVNetLocal</span></span> <br>
<span data-ttu-id="2775b-144">A virtuális hálózati átjáró neve = RMGateway</span><span class="sxs-lookup"><span data-stu-id="2775b-144">Virtual Network Gateway name = RMGateway</span></span> <br>
<span data-ttu-id="2775b-145">Átjáró IP-címzési beállításokat = gwipconfig</span><span class="sxs-lookup"><span data-stu-id="2775b-145">Gateway IP addressing configuration = gwipconfig</span></span>

## <span data-ttu-id="2775b-146"><a name="createsmgw"></a>1. szakasz - konfigurálása hello klasszikus virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="2775b-146"><a name="createsmgw"></a>Section 1 - Configure hello classic VNet</span></span>
### <a name="part-1---download-your-network-configuration-file"></a><span data-ttu-id="2775b-147">1. rész – a hálózati konfigurációs fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="2775b-147">Part 1 - Download your network configuration file</span></span>
1. <span data-ttu-id="2775b-148">Jelentkezzen be tooyour Azure-fiók hello PowerShell-konzolban emelt szintű jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="2775b-148">Log in tooyour Azure account in hello PowerShell console with elevated rights.</span></span> <span data-ttu-id="2775b-149">hello következő parancsmag kéri hello bejelentkezési hitelesítő adatokat az Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="2775b-149">hello following cmdlet prompts you for hello login credentials for your Azure Account.</span></span> <span data-ttu-id="2775b-150">A bejelentkezés után az tölti le a fiókbeállításoknál, hogy-e elérhető tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2775b-150">After logging in, it downloads your account settings so that they are available tooAzure PowerShell.</span></span> <span data-ttu-id="2775b-151">Service Manager PowerShell-parancsmagok toocomplete hello hello konfigurációs ezen része használja.</span><span class="sxs-lookup"><span data-stu-id="2775b-151">You use hello SM PowerShell cmdlets toocomplete this part of hello configuration.</span></span>

  ```powershell
  Add-AzureAccount
  ```
2. <span data-ttu-id="2775b-152">Az Azure-hálózat konfigurációs fájl exportálása hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2775b-152">Export your Azure network configuration file by running hello following command.</span></span> <span data-ttu-id="2775b-153">Hello helyét hello fájl tooexport tooa máshová szükség esetén módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="2775b-153">You can change hello location of hello file tooexport tooa different location if necessary.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. <span data-ttu-id="2775b-154">Nyitott hello tartalmazó .xml-fájlt, hogy a tooedit le azt.</span><span class="sxs-lookup"><span data-stu-id="2775b-154">Open hello .xml file that you downloaded tooedit it.</span></span> <span data-ttu-id="2775b-155">Hello hálózati konfigurációs fájl példa, lásd: hello [hálózati konfigurációs séma](https://msdn.microsoft.com/library/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="2775b-155">For an example of hello network configuration file, see hello [Network Configuration Schema](https://msdn.microsoft.com/library/jj157100.aspx).</span></span>

### <a name="part-2--verify-hello-gateway-subnet"></a><span data-ttu-id="2775b-156">Rész 2 - hello átjáróalhálózatot ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2775b-156">Part 2 -Verify hello gateway subnet</span></span>
<span data-ttu-id="2775b-157">A hello **VirtualNetworkSites** elemet, adja hozzá egy átjáró alhálózati tooyour VNet, ha nem már létrehozták.</span><span class="sxs-lookup"><span data-stu-id="2775b-157">In hello **VirtualNetworkSites** element, add a gateway subnet tooyour VNet if one has not already been created.</span></span> <span data-ttu-id="2775b-158">Az hello hálózati konfigurációs fájl használatakor hello átjáróalhálózatot névvel kell ellátni "GatewaySubnet" vagy az Azure nem ismeri fel és nem használható egy átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="2775b-158">When working with hello network configuration file, hello gateway subnet MUST be named "GatewaySubnet" or Azure cannot recognize and use it as a gateway subnet.</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="2775b-159">**Példa**</span><span class="sxs-lookup"><span data-stu-id="2775b-159">**Example:**</span></span>

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="part-3---add-hello-local-network-site"></a><span data-ttu-id="2775b-160">3. rész – hello helyi hálózati hely hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2775b-160">Part 3 - Add hello local network site</span></span>
<span data-ttu-id="2775b-161">hozzáadhat hello helyi hálózati telephely hello tooconnect kívánt erőforrás-kezelő virtuális hálózat toowhich jelöli.</span><span class="sxs-lookup"><span data-stu-id="2775b-161">hello local network site you add represents hello RM VNet toowhich you want tooconnect.</span></span> <span data-ttu-id="2775b-162">Adja hozzá a **LocalNetworkSites** elem toohello fájlt, ha egy már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="2775b-162">Add a **LocalNetworkSites** element toohello file if one doesn't already exist.</span></span> <span data-ttu-id="2775b-163">Ezen a ponton hello a konfigurációban az hello VPNGatewayAddress lehet egy másik érvényes nyilvános IP-címet, mert azt még nem hozta létre az erőforrás-kezelő virtuális hálózat hello hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="2775b-163">At this point in hello configuration, hello VPNGatewayAddress can be any valid public IP address because we haven't yet created hello gateway for hello Resource Manager VNet.</span></span> <span data-ttu-id="2775b-164">Miután létrehozhatunk hello átjáró, azt cserélje le a helyőrző IP-cím hello megfelelő nyilvános IP-címet, amelyet toohello RM átjáró rendelt.</span><span class="sxs-lookup"><span data-stu-id="2775b-164">Once we create hello gateway, we replace this placeholder IP address with hello correct public IP address that has been assigned toohello RM gateway.</span></span>

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-hello-vnet-with-hello-local-network-site"></a><span data-ttu-id="2775b-165">Rész 4 – hello hálózatok társítása hello helyi hálózati telephely</span><span class="sxs-lookup"><span data-stu-id="2775b-165">Part 4 - Associate hello VNet with hello local network site</span></span>
<span data-ttu-id="2775b-166">Ebben a szakaszban adtuk meg, hogy szeretné-e tooconnect hello helyi hálózati telephely hello VNet.</span><span class="sxs-lookup"><span data-stu-id="2775b-166">In this section, we specify hello local network site that you want tooconnect hello VNet to.</span></span> <span data-ttu-id="2775b-167">Ebben az esetben is, amely korábban hivatkozott erőforrás-kezelő VNet hello.</span><span class="sxs-lookup"><span data-stu-id="2775b-167">In this case, it is hello Resource Manager VNet that you referenced earlier.</span></span> <span data-ttu-id="2775b-168">Győződjön meg arról, hogy hello nevének felel meg.</span><span class="sxs-lookup"><span data-stu-id="2775b-168">Make sure hello names match.</span></span> <span data-ttu-id="2775b-169">Ez a lépés nem hoz létre egy átjáró.</span><span class="sxs-lookup"><span data-stu-id="2775b-169">This step does not create a gateway.</span></span> <span data-ttu-id="2775b-170">Meghatározza az hello helyi hálózati átjáró hello fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="2775b-170">It specifies hello local network that hello gateway will connect to.</span></span>

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-hello-file-and-upload"></a><span data-ttu-id="2775b-171">5 - rész mentés hello fájl- és feltöltése</span><span class="sxs-lookup"><span data-stu-id="2775b-171">Part 5 - Save hello file and upload</span></span>
<span data-ttu-id="2775b-172">Hello fájl mentéséhez, majd importálja azt hello a következő parancs futtatásával tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2775b-172">Save hello file, then import it tooAzure by running hello following command.</span></span> <span data-ttu-id="2775b-173">Ellenőrizze, hogy módosítja a környezetben a megfelelő hello fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="2775b-173">Make sure you change hello file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="2775b-174">Látni fogja, hogy sikerült-e hello importálás megjelenítő hasonló eredményt.</span><span class="sxs-lookup"><span data-stu-id="2775b-174">You will see a similar result showing that hello import succeeded.</span></span>

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-hello-gateway"></a><span data-ttu-id="2775b-175">6. rész – hello átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="2775b-175">Part 6 - Create hello gateway</span></span>

<span data-ttu-id="2775b-176">Mielőtt futtatná az ebben a példában, tekintse meg a toohello hálózati konfigurációs fájl a pontos hello nevét, hogy Azure letöltött toosee vár.</span><span class="sxs-lookup"><span data-stu-id="2775b-176">Before running this example, refer toohello network configuration file that you downloaded for hello exact names that Azure expects toosee.</span></span> <span data-ttu-id="2775b-177">hello hálózati konfigurációs fájlt a klasszikus virtuális hálózatok hello értéket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2775b-177">hello network configuration file contains hello values for your classic virtual networks.</span></span> <span data-ttu-id="2775b-178">Egyes esetekben Vnetek hello hálózati konfigurációs fájlban, módosulnak, a klasszikus virtuális hálózat beállítások létrehozásakor klasszikus hello neveit hello Azure-portálon hello üzembe helyezési modellel toohello eltérései miatt.</span><span class="sxs-lookup"><span data-stu-id="2775b-178">Sometimes hello names for classic VNets are changed in hello network configuration file when creating classic VNet settings in hello Azure portal due toohello differences in hello deployment models.</span></span> <span data-ttu-id="2775b-179">Például egy klasszikus virtuális hálózat "Klasszikus virtuális hálózat" nevű és "ClassicRG" nevű erőforráscsoportban létrehozta az Azure portál toocreate hello használva hello neve hello hálózati konfigurációs fájlban lévő akkor konvertált too'Group ClassicRG klasszikus virtuális hálózat ".</span><span class="sxs-lookup"><span data-stu-id="2775b-179">For example, if you used hello Azure portal toocreate a classic VNet named 'Classic VNet' and created it in a resource group named 'ClassicRG', hello name that is contained in hello network configuration file is converted too'Group ClassicRG Classic VNet'.</span></span> <span data-ttu-id="2775b-180">A szóközöket tartalmazó VNet neve hello megadásakor idézőjelekbe hello érték.</span><span class="sxs-lookup"><span data-stu-id="2775b-180">When specifying hello name of a VNet that contains spaces, use quotation marks around hello value.</span></span>


<span data-ttu-id="2775b-181">A következő példa toocreate dinamikus útválasztó átjáró hello használata:</span><span class="sxs-lookup"><span data-stu-id="2775b-181">Use hello following example toocreate a dynamic routing gateway:</span></span>

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

<span data-ttu-id="2775b-182">Hello segítségével ellenőrizheti a hello átjáró hello állapotának **Get-AzureVNetGateway** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2775b-182">You can check hello status of hello gateway by using hello **Get-AzureVNetGateway** cmdlet.</span></span>

## <span data-ttu-id="2775b-183"><a name="creatermgw"></a>2. rész: Hello RM hálózatok átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2775b-183"><a name="creatermgw"></a>Section 2: Configure hello RM VNet gateway</span></span>
<span data-ttu-id="2775b-184">toocreate hello erőforrás-kezelő virtuális hálózat, a VPN-átjáró kövesse az utasításokat követve hello.</span><span class="sxs-lookup"><span data-stu-id="2775b-184">toocreate a VPN gateway for hello RM VNet, follow hello following instructions.</span></span> <span data-ttu-id="2775b-185">Hello klasszikus virtuális hálózat átjáró hello nyilvános IP-cím beolvasása után hello lépéseket addig ne indítsa el.</span><span class="sxs-lookup"><span data-stu-id="2775b-185">Don't start hello steps until after you have retrieved hello public IP address for hello classic VNet's gateway.</span></span> 

1. <span data-ttu-id="2775b-186">Jelentkezzen be tooyour Azure-fiók hello PowerShell-konzolban.</span><span class="sxs-lookup"><span data-stu-id="2775b-186">Log in tooyour Azure account in hello PowerShell console.</span></span> <span data-ttu-id="2775b-187">hello következő parancsmag kéri hello bejelentkezési hitelesítő adatokat az Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="2775b-187">hello following cmdlet prompts you for hello login credentials for your Azure Account.</span></span> <span data-ttu-id="2775b-188">A bejelentkezés után a fiók beállításait, hogy-e elérhető tooAzure PowerShell lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="2775b-188">After logging in, your account settings are downloaded so that they are available tooAzure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  <span data-ttu-id="2775b-189">Az Azure-előfizetések listájának lekérdezése, ha egynél több előfizetéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2775b-189">Get a list of your Azure subscriptions if you have more than one subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
   
  <span data-ttu-id="2775b-190">Adja meg, hogy szeretné-e toouse hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2775b-190">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="2775b-191">A helyi hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2775b-191">Create a local network gateway.</span></span> <span data-ttu-id="2775b-192">Egy virtuális hálózatban hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="2775b-192">In a virtual network, hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="2775b-193">Ebben az esetben a hello helyi hálózati átjáró tooyour klasszikus virtuális hálózatot jelenti.</span><span class="sxs-lookup"><span data-stu-id="2775b-193">In this case, hello local network gateway refers tooyour Classic VNet.</span></span> <span data-ttu-id="2775b-194">Adjon neki egy nevet, amellyel Azure is jelenti tooit és hello terület címelőtag is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="2775b-194">Give it a name by which Azure can refer tooit, and also specify hello address space prefix.</span></span> <span data-ttu-id="2775b-195">Azure hello IP-cím előtagján megadhatja, mely forgalom toosend tooyour helyszíni hely tooidentify használja.</span><span class="sxs-lookup"><span data-stu-id="2775b-195">Azure uses hello IP address prefix you specify tooidentify which traffic toosend tooyour on-premises location.</span></span> <span data-ttu-id="2775b-196">Ha tájékoztatásra van szüksége tooadjust hello itt később, az átjáró létrehozása előtt módosíthatja hello értékek és a futási hello minta újra.</span><span class="sxs-lookup"><span data-stu-id="2775b-196">If you need tooadjust hello information here later, before creating your gateway, you can modify hello values and run hello sample again.</span></span>
   
   <span data-ttu-id="2775b-197">**-Név** tooassign toorefer toohello helyi hálózati átjáró kívánt hello neve.</span><span class="sxs-lookup"><span data-stu-id="2775b-197">**-Name** is hello name you want tooassign toorefer toohello local network gateway.</span></span><br><span data-ttu-id="2775b-198">
   **-AddressPrefix** hello a klasszikus virtuális hálózat-tartomány.</span><span class="sxs-lookup"><span data-stu-id="2775b-198">
   **-AddressPrefix** is hello Address Space for your classic VNet.</span></span><br><span data-ttu-id="2775b-199">
**-GatewayIpAddress** hello klasszikus virtuális hálózat átjáró hello nyilvános IP-címe.</span><span class="sxs-lookup"><span data-stu-id="2775b-199">
**-GatewayIpAddress** is hello public IP address of hello classic VNet's gateway.</span></span> <span data-ttu-id="2775b-200">Győződjön meg arról, hogy toochange hello következő minta tooreflect hello megfelelő IP-címet.</span><span class="sxs-lookup"><span data-stu-id="2775b-200">Be sure toochange hello following sample tooreflect hello correct IP address.</span></span><br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. <span data-ttu-id="2775b-201">Egy nyilvános IP-cím toobe lefoglalt toohello virtuális hálózati átjáró hello erőforrás-kezelő virtuális hálózat a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="2775b-201">Request a public IP address toobe allocated toohello virtual network gateway for hello Resource Manager VNet.</span></span> <span data-ttu-id="2775b-202">Nem adható meg, hogy szeretné-e toouse hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="2775b-202">You can't specify hello IP address that you want toouse.</span></span> <span data-ttu-id="2775b-203">hello IP-cím dinamikusan lefoglalt toohello virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="2775b-203">hello IP address is dynamically allocated toohello virtual network gateway.</span></span> <span data-ttu-id="2775b-204">Azonban ez nem jelenti hello IP-címet érintő módosításait.</span><span class="sxs-lookup"><span data-stu-id="2775b-204">However, this does not mean hello IP address changes.</span></span> <span data-ttu-id="2775b-205">hello egyetlen alkalom hello virtuális hálózati átjáró IP-cím módosításainak mikor van hello átjáró törlődik, és újra létrehozza.</span><span class="sxs-lookup"><span data-stu-id="2775b-205">hello only time hello virtual network gateway IP address changes is when hello gateway is deleted and recreated.</span></span> <span data-ttu-id="2775b-206">Átméretezése, alaphelyzetbe állítása vagy más belső karbantartás vagy frissítés hello átjáró nem módosítja.</span><span class="sxs-lookup"><span data-stu-id="2775b-206">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of hello gateway.</span></span>

  <span data-ttu-id="2775b-207">Ebben a lépésben is hivatott egy változó, amely egy későbbi lépésben lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="2775b-207">In this step, we also set a variable that is used in a later step.</span></span>

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. <span data-ttu-id="2775b-208">Ellenőrizze, hogy a virtuális hálózati átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="2775b-208">Verify that your virtual network has a gateway subnet.</span></span> <span data-ttu-id="2775b-209">Ha nincs átjáró-alhálózat létezik-e, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="2775b-209">If no gateway subnet exists, add one.</span></span> <span data-ttu-id="2775b-210">Ellenőrizze, hogy hello átjáró alhálózatának elnevezése *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="2775b-210">Make sure hello gateway subnet is named *GatewaySubnet*.</span></span>
5. <span data-ttu-id="2775b-211">Hello a következő parancs futtatásával hello átjáróként használt hello alhálózat beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2775b-211">Retrieve hello subnet used for hello gateway by running hello following command.</span></span> <span data-ttu-id="2775b-212">Ebben a lépésben azt is beállíthatja a egy változó toobe hello következő lépésben szükség.</span><span class="sxs-lookup"><span data-stu-id="2775b-212">In this step, we also set a variable toobe used in hello next step.</span></span>
   
   <span data-ttu-id="2775b-213">**-Név** hello az erőforrás-kezelő virtuális hálózat neve.</span><span class="sxs-lookup"><span data-stu-id="2775b-213">**-Name** is hello name of your Resource Manager VNet.</span></span><br><span data-ttu-id="2775b-214">
   **-ResourceGroupName** hello erőforráscsoportban van, hogy hello virtuális hálózat társítva van.</span><span class="sxs-lookup"><span data-stu-id="2775b-214">
**-ResourceGroupName** is hello resource group that hello VNet is associated with.</span></span> <span data-ttu-id="2775b-215">hello átjáró-alhálózatot a virtuális hálózat már léteznie kell, és névvel kell ellátni *GatewaySubnet* toowork megfelelően.</span><span class="sxs-lookup"><span data-stu-id="2775b-215">hello gateway subnet must already exist for this VNet and must be named *GatewaySubnet* toowork properly.</span></span><br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. <span data-ttu-id="2775b-216">Hello átjáró IP-konfiguráció címzési létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2775b-216">Create hello gateway IP addressing configuration.</span></span> <span data-ttu-id="2775b-217">hello átjáró konfigurációs hello alhálózatot és a hello nyilvános IP-cím toouse határoz meg.</span><span class="sxs-lookup"><span data-stu-id="2775b-217">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="2775b-218">A következő minta toocreate hello használata az átjáró konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="2775b-218">Use hello following sample toocreate your gateway configuration.</span></span>

  <span data-ttu-id="2775b-219">Ebben a lépésben hello **- SubnetId** és **- PublicIpAddressId** paramétereket kell átadni hello azonosítóját megadó tulajdonságot hello alhálózatot és IP-cím objektumok, illetve.</span><span class="sxs-lookup"><span data-stu-id="2775b-219">In this step, hello **-SubnetId** and **-PublicIpAddressId** parameters must be passed hello id property from hello subnet, and IP address objects, respectively.</span></span> <span data-ttu-id="2775b-220">Nem használhat egy egyszerű karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2775b-220">You can't use a simple string.</span></span> <span data-ttu-id="2775b-221">Ezek a változók a hello lépés toorequest egy nyilvános IP-cím és lépés tooretrieve hello alhálózati hello.</span><span class="sxs-lookup"><span data-stu-id="2775b-221">These variables are set in hello step toorequest a public IP and hello step tooretrieve hello subnet.</span></span>

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. <span data-ttu-id="2775b-222">Hello erőforrás-kezelő virtuális hálózati átjáró létrehozása hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2775b-222">Create hello Resource Manager virtual network gateway by running hello following command.</span></span> <span data-ttu-id="2775b-223">Hello `-VpnType` kell *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="2775b-223">hello `-VpnType` must be *RouteBased*.</span></span> <span data-ttu-id="2775b-224">Vagy akár többet hello átjáró toocreate 45 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="2775b-224">It can take 45 minutes or more for hello gateway toocreate.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. <span data-ttu-id="2775b-225">Hello VPN-átjáró létrehozása után, másolja a hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="2775b-225">Copy hello public IP address once hello VPN gateway has been created.</span></span> <span data-ttu-id="2775b-226">Használja a klasszikus virtuális hálózat hello helyi hálózati beállításainak konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="2775b-226">You use it when you configure hello local network settings for your Classic VNet.</span></span> <span data-ttu-id="2775b-227">A következő parancsmag tooretrieve hello nyilvános IP-cím hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2775b-227">You can use hello following cmdlet tooretrieve hello public IP address.</span></span> <span data-ttu-id="2775b-228">hello nyilvános IP-cím szerepel, visszatérési hello *IP-cím*.</span><span class="sxs-lookup"><span data-stu-id="2775b-228">hello public IP address is listed in hello return as *IpAddress*.</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-hello-classic-vnet-local-site-settings"></a><span data-ttu-id="2775b-229">3. rész: Hello klasszikus virtuális hálózat helyi webhely beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="2775b-229">Section 3: Modify hello classic VNet local site settings</span></span>

<span data-ttu-id="2775b-230">Ez a szakasz használata hello klasszikus virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="2775b-230">In this section, you work with hello classic VNet.</span></span> <span data-ttu-id="2775b-231">Lecseréli a hello helyőrző IP-címet, amely lesz használt tooconnect toohello erőforrás-kezelő hálózatok átjáró hello helyi helybeállításokat megadásakor használt.</span><span class="sxs-lookup"><span data-stu-id="2775b-231">You replace hello placeholder IP address that you used when specifying hello local site settings that will be used tooconnect toohello Resource Manager VNet gateway.</span></span> 

1. <span data-ttu-id="2775b-232">Hello hálózati konfigurációs fájl exportálása.</span><span class="sxs-lookup"><span data-stu-id="2775b-232">Export hello network configuration file.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. <span data-ttu-id="2775b-233">Egy szövegszerkesztővel, módosíthatók hello érték vpngatewayaddress elemmel.</span><span class="sxs-lookup"><span data-stu-id="2775b-233">Using a text editor, modify hello value for VPNGatewayAddress.</span></span> <span data-ttu-id="2775b-234">Cserélje le hello helyőrző IP-cím erőforrás-kezelő átjáró hello hello nyilvános IP-címét, és mentse a hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="2775b-234">Replace hello placeholder IP address with hello public IP address of hello Resource Manager gateway and then save hello changes.</span></span>

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. <span data-ttu-id="2775b-235">Importálás hello módosítani a hálózati konfigurációs fájl tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2775b-235">Import hello modified network configuration file tooAzure.</span></span>

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <span data-ttu-id="2775b-236"><a name="connect"></a>4. szakasz: Hello átjárók közötti kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="2775b-236"><a name="connect"></a>Section 4: Create a connection between hello gateways</span></span>
<span data-ttu-id="2775b-237">PowerShell hello átjárók közötti kapcsolat létrehozásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="2775b-237">Creating a connection between hello gateways requires PowerShell.</span></span> <span data-ttu-id="2775b-238">Szükség lehet tooadd az Azure-fiók toouse hello klasszikus verziója hello PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="2775b-238">You may need tooadd your Azure Account toouse hello classic version of hello  PowerShell cmdlets.</span></span> <span data-ttu-id="2775b-239">Igen, használjon toodo **Add-AzureAccount**.</span><span class="sxs-lookup"><span data-stu-id="2775b-239">toodo so, use **Add-AzureAccount**.</span></span>

1. <span data-ttu-id="2775b-240">Hello PowerShell-konzolban a megosztott kulcs beállítása.</span><span class="sxs-lookup"><span data-stu-id="2775b-240">In hello PowerShell console, set your shared key.</span></span> <span data-ttu-id="2775b-241">Hello parancsmagok futtatása előtt tekintse meg a toohello hálózati konfigurációs fájl a pontos hello nevét, hogy Azure letöltött toosee vár.</span><span class="sxs-lookup"><span data-stu-id="2775b-241">Before running hello cmdlets, refer toohello network configuration file that you downloaded for hello exact names that Azure expects toosee.</span></span> <span data-ttu-id="2775b-242">A szóközöket tartalmazó VNet neve hello megadásakor idézőjelekbe egyetlen hello érték.</span><span class="sxs-lookup"><span data-stu-id="2775b-242">When specifying hello name of a VNet that contains spaces, use single quotation marks around hello value.</span></span><br><br><span data-ttu-id="2775b-243">A következő példában **- VNetName** hello hello neve klasszikus virtuális hálózatot és **- LocalNetworkSiteName** hello helyi hálózati telephelyhez megadott hello neve.</span><span class="sxs-lookup"><span data-stu-id="2775b-243">In following example, **-VNetName** is hello name of hello classic VNet and **-LocalNetworkSiteName** is hello name you specified for hello local network site.</span></span> <span data-ttu-id="2775b-244">Hello **- SharedKey** érték, amely Ön hozza létre, és adja meg.</span><span class="sxs-lookup"><span data-stu-id="2775b-244">hello **-SharedKey** is a value that you generate and specify.</span></span> <span data-ttu-id="2775b-245">Hello példában használtuk "abc123", de Ön hozza létre és összetettebb használja.</span><span class="sxs-lookup"><span data-stu-id="2775b-245">In hello example, we used 'abc123', but you can generate and use something more complex.</span></span> <span data-ttu-id="2775b-246">fontos dolog, hogy az itt megadott hello érték hello hello azonos értéket adjon meg a következő lépésben hello a kapcsolat létrehozásakor kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2775b-246">hello important thing is that hello value you specify here must be hello same value that you specify in hello next step when you create your connection.</span></span> <span data-ttu-id="2775b-247">megjelennek az visszatérési hello **állapota: sikeres**.</span><span class="sxs-lookup"><span data-stu-id="2775b-247">hello return should show **Status: Successful**.</span></span>

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. <span data-ttu-id="2775b-248">Hello VPN-kapcsolat létrehozása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2775b-248">Create hello VPN connection by running hello following commands:</span></span>
   
  <span data-ttu-id="2775b-249">Hello változók megadása.</span><span class="sxs-lookup"><span data-stu-id="2775b-249">Set hello variables.</span></span>

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  <span data-ttu-id="2775b-250">Hello kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2775b-250">Create hello connection.</span></span> <span data-ttu-id="2775b-251">Figyelje meg, hogy hello **- ConnectionType** IPsec, nem Vnet2Vnet van.</span><span class="sxs-lookup"><span data-stu-id="2775b-251">Notice that hello **-ConnectionType** is IPsec, not Vnet2Vnet.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a><span data-ttu-id="2775b-252">Szakasz 5: Ellenőrizze a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="2775b-252">Section 5: Verify your connections</span></span>

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a><span data-ttu-id="2775b-253">a klasszikus virtuális hálózat tooyour erőforrás-kezelő virtuális hálózat tooverify hello kapcsolat</span><span class="sxs-lookup"><span data-stu-id="2775b-253">tooverify hello connection from your classic VNet tooyour Resource Manager VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="2775b-254">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2775b-254">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="2775b-255">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2775b-255">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a><span data-ttu-id="2775b-256">az erőforrás-kezelő virtuális hálózat tooyour tooverify hello kapcsolatot klasszikus virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="2775b-256">tooverify hello connection from your Resource Manager VNet tooyour classic VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="2775b-257">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2775b-257">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="2775b-258">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2775b-258">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="2775b-259"><a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="2775b-259"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

