---
title: "Klasszikus virtuális hálózatok csatlakoztatása Azure Resource Manager Vnetek: PowerShell |} Microsoft Docs"
description: "Útmutató klasszikus virtuális hálózatokat és erőforrás-kezelő Vnetek VPN-átjáró és a PowerShell használatával közötti VPN-kapcsolat létrehozása"
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
ms.openlocfilehash: 842a32e5304977af92706cdda464286983122247
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a><span data-ttu-id="f30f7-103">Különböző üzemi modellekből származó virtuális hálózatok összekapcsolása a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="f30f7-103">Connect virtual networks from different deployment models using PowerShell</span></span>



<span data-ttu-id="f30f7-104">Ez a cikk bemutatja, hogyan csatlakozzon a hagyományos Vneteket erőforrás-kezelő Vnetek engedélyezi a különálló üzembe helyezési modellel egymással kommunikálni található erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f30f7-104">This article shows you how to connect classic VNets to Resource Manager VNets to allow the resources located in the separate deployment models to communicate with each other.</span></span> <span data-ttu-id="f30f7-105">A cikkben található lépéseket használhatja a PowerShell, de ez a konfiguráció a listáról a cikk kiválasztásával az Azure portál használatával is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-105">The steps in this article use PowerShell, but you can also create this configuration using the Azure portal by selecting the article from this list.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f30f7-106">Portal</span><span class="sxs-lookup"><span data-stu-id="f30f7-106">Portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="f30f7-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f30f7-107">PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

<span data-ttu-id="f30f7-108">A klasszikus virtuális hálózat csatlakozik egy erőforrás-kezelő virtuális hálózat hasonlít egy Vnetet csatlakozik egy helyszíni hely.</span><span class="sxs-lookup"><span data-stu-id="f30f7-108">Connecting a classic VNet to a Resource Manager VNet is similar to connecting a VNet to an on-premises site location.</span></span> <span data-ttu-id="f30f7-109">Mindkét kapcsolattípus egy VPN-átjárót használ a biztonságos alagút IPsec/IKE használatával való kialakításához.</span><span class="sxs-lookup"><span data-stu-id="f30f7-109">Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="f30f7-110">Létrehozhat, amelyek különböző előfizetésekhez és különböző régiókban Vnetek közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-110">You can create a connection between VNets that are in different subscriptions and in different regions.</span></span> <span data-ttu-id="f30f7-111">A helyszíni hálózatokhoz való csatlakozás már rendelkező Vnetek mindaddig, amíg az átjáró, amely a konfigurálva a dinamikus- vagy útválasztó-alapú is csatlakoztathatja.</span><span class="sxs-lookup"><span data-stu-id="f30f7-111">You can also connect VNets that already have connections to on-premises networks, as long as the gateway that they have been configured with is dynamic or route-based.</span></span> <span data-ttu-id="f30f7-112">A virtuális hálózatok közötti kapcsolatokról további információt a cikk végén, a [Virtuális hálózatok közötti kapcsolat – gyakori kérdések](#faq) című részben talál.</span><span class="sxs-lookup"><span data-stu-id="f30f7-112">For more information about VNet-to-VNet connections, see the [VNet-to-VNet FAQ](#faq) at the end of this article.</span></span> 

<span data-ttu-id="f30f7-113">Ha a Vnetek ugyanabban a régióban, érdemes lehet helyette megfontolandó csatlakoztatja őket a Vnetben társviszony-létesítés használatával.</span><span class="sxs-lookup"><span data-stu-id="f30f7-113">If your VNets are in the same region, you may want to instead consider connecting them using VNet Peering.</span></span> <span data-ttu-id="f30f7-114">A virtuális hálózatok közötti társviszony nem használ VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="f30f7-114">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="f30f7-115">További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f30f7-115">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> 

## <a name="before-beginning"></a><span data-ttu-id="f30f7-116">Mielőtt hozzálát</span><span class="sxs-lookup"><span data-stu-id="f30f7-116">Before beginning</span></span>

<span data-ttu-id="f30f7-117">A következő lépések végigvezetik a dinamikus- vagy útválasztó-alapú átjárók konfigurálása az egyes virtuális hálózat és az átjárók közötti VPN-kapcsolat létrehozásához szükséges beállításokat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-117">The following steps walk you through the settings necessary to configure a dynamic or route-based gateway for each VNet and create a VPN connection between the gateways.</span></span> <span data-ttu-id="f30f7-118">Ez a konfiguráció nem támogatja a statikus vagy csoportházirend-alapú átjárók.</span><span class="sxs-lookup"><span data-stu-id="f30f7-118">This configuration does not support static or policy-based gateways.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="f30f7-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f30f7-119">Prerequisites</span></span>

* <span data-ttu-id="f30f7-120">Mindkét Vnetek létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="f30f7-120">Both VNets have already been created.</span></span>
* <span data-ttu-id="f30f7-121">A Vnetek címtartományát nem átfedésben vannak egymással, vagy átfedésben vannak a tartományt az átjárók csatlakoztathatók más kapcsolatok esetében.</span><span class="sxs-lookup"><span data-stu-id="f30f7-121">The address ranges for the VNets do not overlap with each other, or overlap with any of the ranges for other connections that the gateways may be connected to.</span></span>
* <span data-ttu-id="f30f7-122">A legújabb PowerShell-parancsmagok telepítése.</span><span class="sxs-lookup"><span data-stu-id="f30f7-122">You have installed the latest PowerShell cmdlets.</span></span> <span data-ttu-id="f30f7-123">Lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) további információt.</span><span class="sxs-lookup"><span data-stu-id="f30f7-123">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span> <span data-ttu-id="f30f7-124">Győződjön meg arról, hogy a Service Management (SM) és az erőforrás-kezelő (RM) parancsmagok telepítenie.</span><span class="sxs-lookup"><span data-stu-id="f30f7-124">Make sure you install both the Service Management (SM) and the Resource Manager (RM) cmdlets.</span></span> 

### <span data-ttu-id="f30f7-125"><a name="exampleref"></a>Példabeállítások</span><span class="sxs-lookup"><span data-stu-id="f30f7-125"><a name="exampleref"></a>Example settings</span></span>

<span data-ttu-id="f30f7-126">Ezekkel az értékekkel létrehozhat egy tesztkörnyezetet, vagy a segítségükkel értelmezheti a cikkben szereplő példákat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-126">You can use these values to create a test environment, or refer to them to better understand the examples in this article.</span></span>

<span data-ttu-id="f30f7-127">**Klasszikus virtuális hálózat beállításai**</span><span class="sxs-lookup"><span data-stu-id="f30f7-127">**Classic VNet settings**</span></span>

<span data-ttu-id="f30f7-128">Virtuális hálózat neve = ClassicVNet</span><span class="sxs-lookup"><span data-stu-id="f30f7-128">VNet Name = ClassicVNet</span></span> <br>
<span data-ttu-id="f30f7-129">Hely = USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="f30f7-129">Location = West US</span></span> <br>
<span data-ttu-id="f30f7-130">Virtuális hálózat = 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="f30f7-130">Virtual Network Address Spaces = 10.0.0.0/24</span></span> <br>
<span data-ttu-id="f30f7-131">Alhálózat-1 = 10.0.0.0/27</span><span class="sxs-lookup"><span data-stu-id="f30f7-131">Subnet-1 = 10.0.0.0/27</span></span> <br>
<span data-ttu-id="f30f7-132">A GatewaySubnet = 10.0.0.32/29</span><span class="sxs-lookup"><span data-stu-id="f30f7-132">GatewaySubnet = 10.0.0.32/29</span></span> <br>
<span data-ttu-id="f30f7-133">Helyi hálózati név = RMVNetLocal</span><span class="sxs-lookup"><span data-stu-id="f30f7-133">Local Network Name = RMVNetLocal</span></span> <br>
<span data-ttu-id="f30f7-134">GatewayType = DynamicRouting</span><span class="sxs-lookup"><span data-stu-id="f30f7-134">GatewayType = DynamicRouting</span></span>

<span data-ttu-id="f30f7-135">**Erőforrás-kezelő VNet beállításait**</span><span class="sxs-lookup"><span data-stu-id="f30f7-135">**Resource Manager VNet settings**</span></span>

<span data-ttu-id="f30f7-136">Virtuális hálózat neve = RMVNet</span><span class="sxs-lookup"><span data-stu-id="f30f7-136">VNet Name = RMVNet</span></span> <br>
<span data-ttu-id="f30f7-137">Erőforráscsoport = RG1</span><span class="sxs-lookup"><span data-stu-id="f30f7-137">Resource Group = RG1</span></span> <br>
<span data-ttu-id="f30f7-138">Virtuális hálózati IP-címterek = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="f30f7-138">Virtual Network IP Address Spaces = 192.168.0.0/16</span></span> <br>
<span data-ttu-id="f30f7-139">Alhálózat-1 = 192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="f30f7-139">Subnet-1 = 192.168.1.0/24</span></span> <br>
<span data-ttu-id="f30f7-140">A GatewaySubnet = 192.168.0.0/26</span><span class="sxs-lookup"><span data-stu-id="f30f7-140">GatewaySubnet = 192.168.0.0/26</span></span> <br>
<span data-ttu-id="f30f7-141">Hely = USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="f30f7-141">Location = East US</span></span> <br>
<span data-ttu-id="f30f7-142">Átjáró nyilvános IP-név = gwpip</span><span class="sxs-lookup"><span data-stu-id="f30f7-142">Gateway public IP name = gwpip</span></span> <br>
<span data-ttu-id="f30f7-143">Helyi hálózati átjáró = ClassicVNetLocal</span><span class="sxs-lookup"><span data-stu-id="f30f7-143">Local Network Gateway = ClassicVNetLocal</span></span> <br>
<span data-ttu-id="f30f7-144">A virtuális hálózati átjáró neve = RMGateway</span><span class="sxs-lookup"><span data-stu-id="f30f7-144">Virtual Network Gateway name = RMGateway</span></span> <br>
<span data-ttu-id="f30f7-145">Átjáró IP-címzési beállításokat = gwipconfig</span><span class="sxs-lookup"><span data-stu-id="f30f7-145">Gateway IP addressing configuration = gwipconfig</span></span>

## <span data-ttu-id="f30f7-146"><a name="createsmgw"></a>1. szakasz - a klasszikus virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f30f7-146"><a name="createsmgw"></a>Section 1 - Configure the classic VNet</span></span>
### <a name="part-1---download-your-network-configuration-file"></a><span data-ttu-id="f30f7-147">1. rész – a hálózati konfigurációs fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="f30f7-147">Part 1 - Download your network configuration file</span></span>
1. <span data-ttu-id="f30f7-148">Jelentkezzen be az Azure-fiókjával a PowerShell-konzolban, emelt szintű jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="f30f7-148">Log in to your Azure account in the PowerShell console with elevated rights.</span></span> <span data-ttu-id="f30f7-149">A következő parancsmag kéri a bejelentkezési hitelesítő adatok az Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="f30f7-149">The following cmdlet prompts you for the login credentials for your Azure Account.</span></span> <span data-ttu-id="f30f7-150">A bejelentkezés után letölti a fiók beállításait, hogy elérhetők legyenek az Azure PowerShell számára.</span><span class="sxs-lookup"><span data-stu-id="f30f7-150">After logging in, it downloads your account settings so that they are available to Azure PowerShell.</span></span> <span data-ttu-id="f30f7-151">A Service Manager PowerShell-parancsmagok segítségével végezze el ezt a konfiguráció részét.</span><span class="sxs-lookup"><span data-stu-id="f30f7-151">You use the SM PowerShell cmdlets to complete this part of the configuration.</span></span>

  ```powershell
  Add-AzureAccount
  ```
2. <span data-ttu-id="f30f7-152">Az Azure-hálózat konfigurációs fájl exportálása a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f30f7-152">Export your Azure network configuration file by running the following command.</span></span> <span data-ttu-id="f30f7-153">Módosíthatja a fájlt egy másik helyre, szükség esetén helyét.</span><span class="sxs-lookup"><span data-stu-id="f30f7-153">You can change the location of the file to export to a different location if necessary.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. <span data-ttu-id="f30f7-154">Nyissa meg az .xml fájlt a szerkesztéshez letöltött.</span><span class="sxs-lookup"><span data-stu-id="f30f7-154">Open the .xml file that you downloaded to edit it.</span></span> <span data-ttu-id="f30f7-155">A hálózati konfigurációs fájl példa: a [hálózati konfigurációs séma](https://msdn.microsoft.com/library/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="f30f7-155">For an example of the network configuration file, see the [Network Configuration Schema](https://msdn.microsoft.com/library/jj157100.aspx).</span></span>

### <a name="part-2--verify-the-gateway-subnet"></a><span data-ttu-id="f30f7-156">Rész 2 – az átjáró alhálózatának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f30f7-156">Part 2 -Verify the gateway subnet</span></span>
<span data-ttu-id="f30f7-157">Az a **VirtualNetworkSites** elemet, adja hozzá egy átjáró-alhálózatot a virtuális hálózat, ha egy nem már létezik.</span><span class="sxs-lookup"><span data-stu-id="f30f7-157">In the **VirtualNetworkSites** element, add a gateway subnet to your VNet if one has not already been created.</span></span> <span data-ttu-id="f30f7-158">A hálózati konfigurációs fájl használata, ha az átjáró alhálózatának névvel kell ellátni "GatewaySubnet" vagy az Azure nem ismeri fel és nem használható egy átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="f30f7-158">When working with the network configuration file, the gateway subnet MUST be named "GatewaySubnet" or Azure cannot recognize and use it as a gateway subnet.</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="f30f7-159">**Példa**</span><span class="sxs-lookup"><span data-stu-id="f30f7-159">**Example:**</span></span>

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

### <a name="part-3---add-the-local-network-site"></a><span data-ttu-id="f30f7-160">3. rész – a helyi hálózati hely hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f30f7-160">Part 3 - Add the local network site</span></span>
<span data-ttu-id="f30f7-161">A helyi hálózati telephely hozzáadhat jelenti. az erőforrás-kezelő virtuális hálózatot, amelyhez csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="f30f7-161">The local network site you add represents the RM VNet to which you want to connect.</span></span> <span data-ttu-id="f30f7-162">Adja hozzá a **LocalNetworkSites** elemben, amely a fájl, ha egy már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="f30f7-162">Add a **LocalNetworkSites** element to the file if one doesn't already exist.</span></span> <span data-ttu-id="f30f7-163">Ezen a ponton a konfigurációban az a vpngatewayaddress elemmel lehet egy másik érvényes nyilvános IP-címet, mert azt még nem hozta létre az átjáró a Resource Manager vnet.</span><span class="sxs-lookup"><span data-stu-id="f30f7-163">At this point in the configuration, the VPNGatewayAddress can be any valid public IP address because we haven't yet created the gateway for the Resource Manager VNet.</span></span> <span data-ttu-id="f30f7-164">Az átjáró létrehozhatunk, ha azt cserélje le a helyőrző IP-cím megfelelő nyilvános IP-cím van hozzárendelve az erőforrás-kezelő átjáró.</span><span class="sxs-lookup"><span data-stu-id="f30f7-164">Once we create the gateway, we replace this placeholder IP address with the correct public IP address that has been assigned to the RM gateway.</span></span>

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a><span data-ttu-id="f30f7-165">Rész 4 – a virtuális hálózat társítani a helyi hálózati telephely</span><span class="sxs-lookup"><span data-stu-id="f30f7-165">Part 4 - Associate the VNet with the local network site</span></span>
<span data-ttu-id="f30f7-166">Ebben a szakaszban a Vnetet a csatlakozáshoz használni kívánt helyi hálózati telephely adtuk meg.</span><span class="sxs-lookup"><span data-stu-id="f30f7-166">In this section, we specify the local network site that you want to connect the VNet to.</span></span> <span data-ttu-id="f30f7-167">Ebben az esetben a korábban hivatkozott erőforrás-kezelő virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-167">In this case, it is the Resource Manager VNet that you referenced earlier.</span></span> <span data-ttu-id="f30f7-168">Győződjön meg arról, hogy az megfelel-e.</span><span class="sxs-lookup"><span data-stu-id="f30f7-168">Make sure the names match.</span></span> <span data-ttu-id="f30f7-169">Ez a lépés nem hoz létre egy átjáró.</span><span class="sxs-lookup"><span data-stu-id="f30f7-169">This step does not create a gateway.</span></span> <span data-ttu-id="f30f7-170">Azt adja meg a helyi hálózaton, amelyhez az átjáró csatlakozni fognak.</span><span class="sxs-lookup"><span data-stu-id="f30f7-170">It specifies the local network that the gateway will connect to.</span></span>

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a><span data-ttu-id="f30f7-171">5. rész - mentse a fájlt, és töltse fel</span><span class="sxs-lookup"><span data-stu-id="f30f7-171">Part 5 - Save the file and upload</span></span>
<span data-ttu-id="f30f7-172">Mentse a fájlt, majd importálja az Azure-bA a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f30f7-172">Save the file, then import it to Azure by running the following command.</span></span> <span data-ttu-id="f30f7-173">Győződjön meg arról, hogy a fájl elérési útját a környezet szükség szerint módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="f30f7-173">Make sure you change the file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="f30f7-174">Látni fogja, hogy sikerült-e az importálás megjelenítő hasonló eredményt.</span><span class="sxs-lookup"><span data-stu-id="f30f7-174">You will see a similar result showing that the import succeeded.</span></span>

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a><span data-ttu-id="f30f7-175">6. rész – az átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="f30f7-175">Part 6 - Create the gateway</span></span>

<span data-ttu-id="f30f7-176">Mielőtt futtatná az ebben a példában, tekintse meg a hálózati konfigurációs fájljának letöltött Azure vár, hogy pontos neveit.</span><span class="sxs-lookup"><span data-stu-id="f30f7-176">Before running this example, refer to the network configuration file that you downloaded for the exact names that Azure expects to see.</span></span> <span data-ttu-id="f30f7-177">A hálózati konfigurációs fájlt a klasszikus virtuális hálózatok értékeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f30f7-177">The network configuration file contains the values for your classic virtual networks.</span></span> <span data-ttu-id="f30f7-178">Egyes esetekben a hagyományos Vneteket neve módosult a hálózati konfigurációs fájlban klasszikus VNet beállításait az Azure-portálon az üzembe helyezési modellekről eltérései miatt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f30f7-178">Sometimes the names for classic VNets are changed in the network configuration file when creating classic VNet settings in the Azure portal due to the differences in the deployment models.</span></span> <span data-ttu-id="f30f7-179">Például ha a klasszikus virtuális hálózat neve "Klasszikus VNet", és létrehozta a "ClassicRG" nevű erőforráscsoport létrehozásához használt Azure-portálon, a neve, amely a hálózati konfigurációs fájlban lévő alakítja át "Csoport ClassicRG klasszikus virtuális hálózat".</span><span class="sxs-lookup"><span data-stu-id="f30f7-179">For example, if you used the Azure portal to create a classic VNet named 'Classic VNet' and created it in a resource group named 'ClassicRG', the name that is contained in the network configuration file is converted to 'Group ClassicRG Classic VNet'.</span></span> <span data-ttu-id="f30f7-180">A szóközöket tartalmazó VNet neve megadásakor idézőjelekbe értékét.</span><span class="sxs-lookup"><span data-stu-id="f30f7-180">When specifying the name of a VNet that contains spaces, use quotation marks around the value.</span></span>


<span data-ttu-id="f30f7-181">Az alábbi példát követve hozzon létre egy dinamikus útválasztási:</span><span class="sxs-lookup"><span data-stu-id="f30f7-181">Use the following example to create a dynamic routing gateway:</span></span>

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

<span data-ttu-id="f30f7-182">Az átjáró állapotának használatával ellenőrizheti a **Get-AzureVNetGateway** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f30f7-182">You can check the status of the gateway by using the **Get-AzureVNetGateway** cmdlet.</span></span>

## <span data-ttu-id="f30f7-183"><a name="creatermgw"></a>2. szakasz: Az erőforrás-kezelő hálózatok átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f30f7-183"><a name="creatermgw"></a>Section 2: Configure the RM VNet gateway</span></span>
<span data-ttu-id="f30f7-184">Az erőforrás-kezelő vnet VPN-átjáró létrehozásához kövesse az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-184">To create a VPN gateway for the RM VNet, follow the following instructions.</span></span> <span data-ttu-id="f30f7-185">Ne indítsa el a lépéseket, amíg a nyilvános IP-címet a klasszikus virtuális hálózat átjáróként beolvasása után.</span><span class="sxs-lookup"><span data-stu-id="f30f7-185">Don't start the steps until after you have retrieved the public IP address for the classic VNet's gateway.</span></span> 

1. <span data-ttu-id="f30f7-186">Jelentkezzen be az Azure-fiókjával a PowerShell-konzolban.</span><span class="sxs-lookup"><span data-stu-id="f30f7-186">Log in to your Azure account in the PowerShell console.</span></span> <span data-ttu-id="f30f7-187">A következő parancsmag kéri a bejelentkezési hitelesítő adatok az Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="f30f7-187">The following cmdlet prompts you for the login credentials for your Azure Account.</span></span> <span data-ttu-id="f30f7-188">A bejelentkezés után a fiók beállításait, hogy elérhetők az Azure PowerShell letöltése.</span><span class="sxs-lookup"><span data-stu-id="f30f7-188">After logging in, your account settings are downloaded so that they are available to Azure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  <span data-ttu-id="f30f7-189">Az Azure-előfizetések listájának lekérdezése, ha egynél több előfizetéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f30f7-189">Get a list of your Azure subscriptions if you have more than one subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
   
  <span data-ttu-id="f30f7-190">Válassza ki a használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="f30f7-190">Specify the subscription that you want to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="f30f7-191">A helyi hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f30f7-191">Create a local network gateway.</span></span> <span data-ttu-id="f30f7-192">Virtuális hálózatokban a helyi hálózati átjáró általában a helyszínt jelenti.</span><span class="sxs-lookup"><span data-stu-id="f30f7-192">In a virtual network, the local network gateway typically refers to your on-premises location.</span></span> <span data-ttu-id="f30f7-193">A helyi hálózati átjáró ebben az esetben a klasszikus virtuális hálózat hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f30f7-193">In this case, the local network gateway refers to your Classic VNet.</span></span> <span data-ttu-id="f30f7-194">Adjon neki egy nevet, amellyel Azure is hivatkozik rá, és is adja meg a címelőtag terület.</span><span class="sxs-lookup"><span data-stu-id="f30f7-194">Give it a name by which Azure can refer to it, and also specify the address space prefix.</span></span> <span data-ttu-id="f30f7-195">Az Azure a megadott IP-címelőtagot a helyszíni helyre küldendő adatforgalom azonosítására használja.</span><span class="sxs-lookup"><span data-stu-id="f30f7-195">Azure uses the IP address prefix you specify to identify which traffic to send to your on-premises location.</span></span> <span data-ttu-id="f30f7-196">Ha az adatok itt később, úgy, hogy az átjáró létrehozása előtt kell módosítsa az értékeket, és futtassa újból a minta.</span><span class="sxs-lookup"><span data-stu-id="f30f7-196">If you need to adjust the information here later, before creating your gateway, you can modify the values and run the sample again.</span></span>
   
   <span data-ttu-id="f30f7-197">**-Név** utal, hogy a helyi hálózati átjáró hozzárendelni kívánt név.</span><span class="sxs-lookup"><span data-stu-id="f30f7-197">**-Name** is the name you want to assign to refer to the local network gateway.</span></span><br><span data-ttu-id="f30f7-198">
   **-AddressPrefix** a klasszikus virtuális hálózat a címtér van.</span><span class="sxs-lookup"><span data-stu-id="f30f7-198">
   **-AddressPrefix** is the Address Space for your classic VNet.</span></span><br><span data-ttu-id="f30f7-199">
**-GatewayIpAddress** a klasszikus virtuális hálózatot átjáró nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="f30f7-199">
**-GatewayIpAddress** is the public IP address of the classic VNet's gateway.</span></span> <span data-ttu-id="f30f7-200">Ne feledje módosítani a következő példával, hogy tükrözze a megfelelő IP-címet.</span><span class="sxs-lookup"><span data-stu-id="f30f7-200">Be sure to change the following sample to reflect the correct IP address.</span></span><br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. <span data-ttu-id="f30f7-201">Egy nyilvános IP-címet a virtuális hálózati átjáró számára engedélyezett az erőforrás-kezelő vnet kérelmet.</span><span class="sxs-lookup"><span data-stu-id="f30f7-201">Request a public IP address to be allocated to the virtual network gateway for the Resource Manager VNet.</span></span> <span data-ttu-id="f30f7-202">A használni kívánt IP-címet nem adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="f30f7-202">You can't specify the IP address that you want to use.</span></span> <span data-ttu-id="f30f7-203">Az IP-címet a virtuális hálózati átjáró dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="f30f7-203">The IP address is dynamically allocated to the virtual network gateway.</span></span> <span data-ttu-id="f30f7-204">Ez azonban nem jelenti azt, hogy az IP-cím változik.</span><span class="sxs-lookup"><span data-stu-id="f30f7-204">However, this does not mean the IP address changes.</span></span> <span data-ttu-id="f30f7-205">A csak a virtuális hálózati átjáró IP-cím módosításai, amikor az átjáró törölni, majd újból.</span><span class="sxs-lookup"><span data-stu-id="f30f7-205">The only time the virtual network gateway IP address changes is when the gateway is deleted and recreated.</span></span> <span data-ttu-id="f30f7-206">Az átméretezés, alaphelyzetbe állítása vagy más belső karbantartás vagy frissítés az átjáró nem változik.</span><span class="sxs-lookup"><span data-stu-id="f30f7-206">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of the gateway.</span></span>

  <span data-ttu-id="f30f7-207">Ebben a lépésben is hivatott egy változó, amely egy későbbi lépésben lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="f30f7-207">In this step, we also set a variable that is used in a later step.</span></span>

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. <span data-ttu-id="f30f7-208">Ellenőrizze, hogy a virtuális hálózati átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="f30f7-208">Verify that your virtual network has a gateway subnet.</span></span> <span data-ttu-id="f30f7-209">Ha nincs átjáró-alhálózat létezik-e, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="f30f7-209">If no gateway subnet exists, add one.</span></span> <span data-ttu-id="f30f7-210">Győződjön meg arról, hogy az átjáró alhálózatának elnevezése *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="f30f7-210">Make sure the gateway subnet is named *GatewaySubnet*.</span></span>
5. <span data-ttu-id="f30f7-211">Az alhálózat, a következő parancs futtatásával az átjáróként használt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f30f7-211">Retrieve the subnet used for the gateway by running the following command.</span></span> <span data-ttu-id="f30f7-212">Ebben a lépésben azt is állítsa be a következő lépésben használandó változó.</span><span class="sxs-lookup"><span data-stu-id="f30f7-212">In this step, we also set a variable to be used in the next step.</span></span>
   
   <span data-ttu-id="f30f7-213">**-Név** az erőforrás-kezelő virtuális hálózat neve.</span><span class="sxs-lookup"><span data-stu-id="f30f7-213">**-Name** is the name of your Resource Manager VNet.</span></span><br><span data-ttu-id="f30f7-214">
   **-ResourceGroupName** az erőforráscsoport, amelyhez a virtuális hálózat társítva van.</span><span class="sxs-lookup"><span data-stu-id="f30f7-214">
**-ResourceGroupName** is the resource group that the VNet is associated with.</span></span> <span data-ttu-id="f30f7-215">Az átjáró-alhálózatot a virtuális hálózat már léteznie kell, és névvel kell ellátni *GatewaySubnet* megfelelően működjön.</span><span class="sxs-lookup"><span data-stu-id="f30f7-215">The gateway subnet must already exist for this VNet and must be named *GatewaySubnet* to work properly.</span></span><br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. <span data-ttu-id="f30f7-216">Hozzon létre átjáró IP-címzési konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="f30f7-216">Create the gateway IP addressing configuration.</span></span> <span data-ttu-id="f30f7-217">Az átjáró konfigurációja meghatározza az alhálózatot és a használandó nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="f30f7-217">The gateway configuration defines the subnet and the public IP address to use.</span></span> <span data-ttu-id="f30f7-218">A következő minta használatával hozza létre az átjáró konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="f30f7-218">Use the following sample to create your gateway configuration.</span></span>

  <span data-ttu-id="f30f7-219">Ebben a lépésben a **- SubnetId** és **- PublicIpAddressId** paramétereket kell átadni azonosítóját megadó tulajdonságot az alhálózatot és IP-cím objektumok, illetve.</span><span class="sxs-lookup"><span data-stu-id="f30f7-219">In this step, the **-SubnetId** and **-PublicIpAddressId** parameters must be passed the id property from the subnet, and IP address objects, respectively.</span></span> <span data-ttu-id="f30f7-220">Nem használhat egy egyszerű karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f30f7-220">You can't use a simple string.</span></span> <span data-ttu-id="f30f7-221">Ezek a változók van beállítva a lépésben a kérelem egy nyilvános IP-cím és a lépés az alhálózat beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f30f7-221">These variables are set in the step to request a public IP and the step to retrieve the subnet.</span></span>

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. <span data-ttu-id="f30f7-222">Az erőforrás-kezelő virtuális hálózati átjáró létrehozása a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f30f7-222">Create the Resource Manager virtual network gateway by running the following command.</span></span> <span data-ttu-id="f30f7-223">A `-VpnType` kell *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="f30f7-223">The `-VpnType` must be *RouteBased*.</span></span> <span data-ttu-id="f30f7-224">Vagy akár többet az átjáró létrehozása 45 percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-224">It can take 45 minutes or more for the gateway to create.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. <span data-ttu-id="f30f7-225">Másolja át a nyilvános IP-cím, a VPN-átjáró létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="f30f7-225">Copy the public IP address once the VPN gateway has been created.</span></span> <span data-ttu-id="f30f7-226">Használja a klasszikus virtuális hálózat helyi hálózati beállításainak konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="f30f7-226">You use it when you configure the local network settings for your Classic VNet.</span></span> <span data-ttu-id="f30f7-227">A következő parancsmag használatával a nyilvános IP-cím beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f30f7-227">You can use the following cmdlet to retrieve the public IP address.</span></span> <span data-ttu-id="f30f7-228">A nyilvános IP-cím szerepel, a visszatérési *IP-cím*.</span><span class="sxs-lookup"><span data-stu-id="f30f7-228">The public IP address is listed in the return as *IpAddress*.</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-the-classic-vnet-local-site-settings"></a><span data-ttu-id="f30f7-229">3. szakasz: A klasszikus virtuális hálózat helyi webhely beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="f30f7-229">Section 3: Modify the classic VNet local site settings</span></span>

<span data-ttu-id="f30f7-230">Ebben a szakaszban dolgozik a klasszikus virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="f30f7-230">In this section, you work with the classic VNet.</span></span> <span data-ttu-id="f30f7-231">Lecseréli a helyőrző IP-címet, amely a helyi beállításokat telepíthet, amelyek a Resource Manager virtuális hálózat átjárón megadásakor használatos.</span><span class="sxs-lookup"><span data-stu-id="f30f7-231">You replace the placeholder IP address that you used when specifying the local site settings that will be used to connect to the Resource Manager VNet gateway.</span></span> 

1. <span data-ttu-id="f30f7-232">A hálózati konfigurációs fájl exportálása.</span><span class="sxs-lookup"><span data-stu-id="f30f7-232">Export the network configuration file.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. <span data-ttu-id="f30f7-233">Az érték egy szövegszerkesztővel, módosíthatók vpngatewayaddress elemmel.</span><span class="sxs-lookup"><span data-stu-id="f30f7-233">Using a text editor, modify the value for VPNGatewayAddress.</span></span> <span data-ttu-id="f30f7-234">Cserélje le a helyőrző IP-címet az erőforrás-kezelő átjáró nyilvános IP-címét, és mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-234">Replace the placeholder IP address with the public IP address of the Resource Manager gateway and then save the changes.</span></span>

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. <span data-ttu-id="f30f7-235">Importálja a módosított hálózati konfigurációs fájlt az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="f30f7-235">Import the modified network configuration file to Azure.</span></span>

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <span data-ttu-id="f30f7-236"><a name="connect"></a>4. szakasz: Az átjárók közötti kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f30f7-236"><a name="connect"></a>Section 4: Create a connection between the gateways</span></span>
<span data-ttu-id="f30f7-237">Az átjárók közötti kapcsolat létrehozása a PowerShell szükséges.</span><span class="sxs-lookup"><span data-stu-id="f30f7-237">Creating a connection between the gateways requires PowerShell.</span></span> <span data-ttu-id="f30f7-238">Szükség lehet a PowerShell-parancsmagok klasszikus verzióját használja az Azure-fiók hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="f30f7-238">You may need to add your Azure Account to use the classic version of the  PowerShell cmdlets.</span></span> <span data-ttu-id="f30f7-239">Ehhez használja **Add-AzureAccount**.</span><span class="sxs-lookup"><span data-stu-id="f30f7-239">To do so, use **Add-AzureAccount**.</span></span>

1. <span data-ttu-id="f30f7-240">A PowerShell-konzolban a megosztott kulcs beállítása.</span><span class="sxs-lookup"><span data-stu-id="f30f7-240">In the PowerShell console, set your shared key.</span></span> <span data-ttu-id="f30f7-241">Az parancsmagok futtatása előtt tekintse meg a hálózati konfigurációs fájljának letöltött Azure vár, hogy pontos neveit.</span><span class="sxs-lookup"><span data-stu-id="f30f7-241">Before running the cmdlets, refer to the network configuration file that you downloaded for the exact names that Azure expects to see.</span></span> <span data-ttu-id="f30f7-242">A szóközöket tartalmazó VNet neve megadásakor idézőjelekbe egyetlen érték.</span><span class="sxs-lookup"><span data-stu-id="f30f7-242">When specifying the name of a VNet that contains spaces, use single quotation marks around the value.</span></span><br><br><span data-ttu-id="f30f7-243">A következő példában **- VNetName** neve a klasszikus virtuális hálózaton, és **- LocalNetworkSiteName** a helyi hálózati telephelyhez megadott neve.</span><span class="sxs-lookup"><span data-stu-id="f30f7-243">In following example, **-VNetName** is the name of the classic VNet and **-LocalNetworkSiteName** is the name you specified for the local network site.</span></span> <span data-ttu-id="f30f7-244">A **- SharedKey** érték, amely Ön hozza létre, és adja meg.</span><span class="sxs-lookup"><span data-stu-id="f30f7-244">The **-SharedKey** is a value that you generate and specify.</span></span> <span data-ttu-id="f30f7-245">A példában használtuk "abc123", de Ön hozza létre és összetettebb használja.</span><span class="sxs-lookup"><span data-stu-id="f30f7-245">In the example, we used 'abc123', but you can generate and use something more complex.</span></span> <span data-ttu-id="f30f7-246">Figyeljen arra, hogy az itt megadott érték ugyanazt az értéket a következő lépés a kapcsolat létrehozásakor meg kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f30f7-246">The important thing is that the value you specify here must be the same value that you specify in the next step when you create your connection.</span></span> <span data-ttu-id="f30f7-247">Meg kell jelennie a visszatérési **állapota: sikeres**.</span><span class="sxs-lookup"><span data-stu-id="f30f7-247">The return should show **Status: Successful**.</span></span>

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. <span data-ttu-id="f30f7-248">A VPN-kapcsolat létrehozása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f30f7-248">Create the VPN connection by running the following commands:</span></span>
   
  <span data-ttu-id="f30f7-249">Állítsa be a változókat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-249">Set the variables.</span></span>

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  <span data-ttu-id="f30f7-250">Hozza létre a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f30f7-250">Create the connection.</span></span> <span data-ttu-id="f30f7-251">Figyelje meg, hogy a **- ConnectionType** IPsec, nem Vnet2Vnet van.</span><span class="sxs-lookup"><span data-stu-id="f30f7-251">Notice that the **-ConnectionType** is IPsec, not Vnet2Vnet.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a><span data-ttu-id="f30f7-252">Szakasz 5: Ellenőrizze a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="f30f7-252">Section 5: Verify your connections</span></span>

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a><span data-ttu-id="f30f7-253">A kapcsolat a klasszikus virtuális hálózat és az erőforrás-kezelő virtuális hálózat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f30f7-253">To verify the connection from your classic VNet to your Resource Manager VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="f30f7-254">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f30f7-254">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="f30f7-255">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f30f7-255">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a><span data-ttu-id="f30f7-256">A kapcsolat az erőforrás-kezelő virtuális hálózat és a klasszikus virtuális hálózat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f30f7-256">To verify the connection from your Resource Manager VNet to your classic VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="f30f7-257">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f30f7-257">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="f30f7-258">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f30f7-258">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="f30f7-259"><a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="f30f7-259"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

