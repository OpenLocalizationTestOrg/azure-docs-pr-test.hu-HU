---
title: "A virtuális hálózati átjáró törlése: PowerShell: klasszikus Azure portál |} Microsoft Docs"
description: "A PowerShell használatával a klasszikus üzembe helyezési modellel virtuális hálózati átjáró törlése."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: cherylmc
ms.openlocfilehash: b1bc18307227a728e2bc8fd95e30fdc1cbdb8c59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a><span data-ttu-id="9fd37-103">A powershellel (klasszikus) virtuális hálózati átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="9fd37-103">Delete a virtual network gateway using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9fd37-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9fd37-104">Resource Manager - Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="9fd37-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="9fd37-105">Resource Manager - PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="9fd37-106">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="9fd37-106">Classic - PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

<span data-ttu-id="9fd37-107">Ez a cikk segítséget nyújt a VPN-átjáró a klasszikus üzembe helyezési modellel törlése a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="9fd37-107">This article helps you delete a VPN gateway in the classic deployment model by using PowerShell.</span></span> <span data-ttu-id="9fd37-108">Után a virtuális hálózati átjáró törölve lett, távolítsa el az elemeket, amelyek már nem használ a hálózati konfigurációs fájl módosításával.</span><span class="sxs-lookup"><span data-stu-id="9fd37-108">After the virtual network gateway has been deleted, modify the network configuration file to remove elements that you are no longer using.</span></span>

##<span data-ttu-id="9fd37-109"><a name="connect"></a>1. lépés: Csatlakozás az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="9fd37-109"><a name="connect"></a>Step 1: Connect to Azure</span></span>

### <a name="1-install-the-latest-powershell-cmdlets"></a><span data-ttu-id="9fd37-110">1. Telepítse a legújabb PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="9fd37-110">1. Install the latest PowerShell cmdlets.</span></span>

<span data-ttu-id="9fd37-111">Töltse le és telepítse a legújabb verziót az Azure Service Management (SM) PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="9fd37-111">Download and install the latest version of the Azure Service Management (SM) PowerShell cmdlets.</span></span> <span data-ttu-id="9fd37-112">További információt [az Azure PowerShell telepítésével és konfigurálásával](/powershell/azure/overview) foglalkozó témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="9fd37-112">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="2-connect-to-your-azure-account"></a><span data-ttu-id="9fd37-113">2. Csatlakozás az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="9fd37-113">2. Connect to your Azure account.</span></span> 

<span data-ttu-id="9fd37-114">Nyissa meg emelt szintű jogosultságokkal a PowerShell konzolt, és csatlakozzon a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="9fd37-114">Open your PowerShell console with elevated rights and connect to your account.</span></span> <span data-ttu-id="9fd37-115">A következő példa segít a kapcsolódásban:</span><span class="sxs-lookup"><span data-stu-id="9fd37-115">Use the following example to help you connect:</span></span>

```powershell
Add-AzureAccount
```

## <span data-ttu-id="9fd37-116"><a name="export"></a>2. lépés: Exportálása és a hálózati konfiguráció megtekintése</span><span class="sxs-lookup"><span data-stu-id="9fd37-116"><a name="export"></a>Step 2: Export and view the network configuration file</span></span>

<span data-ttu-id="9fd37-117">Hozzon létre egy könyvtárat a számítógépén, majd exportálja a hálózati konfigurációs fájlt a könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="9fd37-117">Create a directory on your computer and then export the network configuration file to the directory.</span></span> <span data-ttu-id="9fd37-118">Ezt a fájlt, mindkét nézetben meg a konfigurációs információkat, valamint a hálózati konfiguráció módosításához használja.</span><span class="sxs-lookup"><span data-stu-id="9fd37-118">You use this file to both view the current configuration information, and also to modify the network configuration.</span></span>

<span data-ttu-id="9fd37-119">Ebben a példában a hálózati konfigurációs fájlt a C:\AzureNet helyre exportálja.</span><span class="sxs-lookup"><span data-stu-id="9fd37-119">In this example, the network configuration file is exported to C:\AzureNet.</span></span>

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="9fd37-120">Nyissa meg a fájlt egy szövegszerkesztőben, és megtekintése a klasszikus virtuális hálózat nevét.</span><span class="sxs-lookup"><span data-stu-id="9fd37-120">Open the file with a text editor and view the name for your classic VNet.</span></span> <span data-ttu-id="9fd37-121">VNet létrehozása az Azure portálon, az Azure által használt teljes név esetén nem látható a portálon.</span><span class="sxs-lookup"><span data-stu-id="9fd37-121">When you create a VNet in the Azure portal, the full name that Azure uses is not visible in the portal.</span></span> <span data-ttu-id="9fd37-122">Például egy VNet neve "ClassicVNet1" az Azure-portálon megjelenő lehetséges, hogy mennyi hosszabb nevet adni a hálózati konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="9fd37-122">For example, a VNet that appears to be named 'ClassicVNet1' in the Azure portal, may have a much longer name in the network configuration file.</span></span> <span data-ttu-id="9fd37-123">A név lehet, hogy alábbihoz hasonló: "Csoport ClassicRG1 ClassicVNet1".</span><span class="sxs-lookup"><span data-stu-id="9fd37-123">The name might look something like: 'Group ClassicRG1 ClassicVNet1'.</span></span> <span data-ttu-id="9fd37-124">Virtuális hálózati nevek felsorolt **"VirtualNetworkSite name ="**.</span><span class="sxs-lookup"><span data-stu-id="9fd37-124">Virtual network names are listed as **'VirtualNetworkSite name ='**.</span></span> <span data-ttu-id="9fd37-125">Amikor a PowerShell-parancsmagok futtatását lévő nevek használata a hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="9fd37-125">Use the names in the network configuration file when running your PowerShell cmdlets.</span></span>

## <span data-ttu-id="9fd37-126"><a name="delete"></a>3. lépés: A virtuális hálózati átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="9fd37-126"><a name="delete"></a>Step 3: Delete the virtual network gateway</span></span>

<span data-ttu-id="9fd37-127">A virtuális hálózati átjáró törlése, a virtuális hálózat az átjárón keresztül létesített összes kapcsolat megszakad.</span><span class="sxs-lookup"><span data-stu-id="9fd37-127">When you delete a virtual network gateway, all connections to the VNet through the gateway are disconnected.</span></span> <span data-ttu-id="9fd37-128">Ha a virtuális hálózat csatlakozik P2S-ügyféllel, akkor le lesz választva figyelmeztetés nélkül.</span><span class="sxs-lookup"><span data-stu-id="9fd37-128">If you have P2S clients connected to the VNet, they will be disconnected without warning.</span></span>

<span data-ttu-id="9fd37-129">Ebben a példában a virtuális hálózati átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="9fd37-129">This example deletes the virtual network gateway.</span></span> <span data-ttu-id="9fd37-130">Győződjön meg arról, hogy a virtuális hálózati teljes nevét használja a hálózati konfigurációs fájlból.</span><span class="sxs-lookup"><span data-stu-id="9fd37-130">Make sure to use the full name of the virtual network from the network configuration file.</span></span>

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

<span data-ttu-id="9fd37-131">Ha sikeres, a visszatérési jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="9fd37-131">If successful, the return shows:</span></span>

```
Status : Successful
```

## <span data-ttu-id="9fd37-132"><a name="modify"></a>4. lépés: Módosítsa a hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="9fd37-132"><a name="modify"></a>Step 4: Modify the network configuration file</span></span>

<span data-ttu-id="9fd37-133">A virtuális hálózati átjáró törlése, a parancsmag nem módosítja a hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="9fd37-133">When you delete a virtual network gateway, the cmdlet does not modify the network configuration file.</span></span> <span data-ttu-id="9fd37-134">Módosítania kell a fájlt, és távolítsa el az elemeket, már nem használt.</span><span class="sxs-lookup"><span data-stu-id="9fd37-134">You need to modify the file to remove the elements that are no longer being used.</span></span> <span data-ttu-id="9fd37-135">Az alábbi szakaszok segítségével módosítsa a letöltött hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="9fd37-135">The following sections help you modify the network configuration file that you downloaded.</span></span>

### <span data-ttu-id="9fd37-136"><a name="lnsref"></a>Helyi hálózati telephelyekre való hivatkozások</span><span class="sxs-lookup"><span data-stu-id="9fd37-136"><a name="lnsref"></a>Local Network Site References</span></span>

<span data-ttu-id="9fd37-137">Helyadatok hivatkozás eltávolításához konfigurációs módosítások elvégzéséhez **ConnectionsToLocalNetwork/LocalNetworkSiteRef**.</span><span class="sxs-lookup"><span data-stu-id="9fd37-137">To remove site reference information, make configuration changes to **ConnectionsToLocalNetwork/LocalNetworkSiteRef**.</span></span> <span data-ttu-id="9fd37-138">A helyi webhelyhez hivatkozás eseményindítók törlése egy alagúton Azure eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="9fd37-138">Removing a local site reference triggers Azure to delete a tunnel.</span></span> <span data-ttu-id="9fd37-139">A létrehozott konfigurációjától függően lehet, hogy nincs egy **LocalNetworkSiteRef** szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="9fd37-139">Depending on the configuration that you created, you may not have a **LocalNetworkSiteRef** listed.</span></span>

```
<Gateway>
   <ConnectionsToLocalNetwork>
     <LocalNetworkSiteRef name="D1BFC9CB_Site2">
       <Connection type="IPsec" />
     </LocalNetworkSiteRef>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

<span data-ttu-id="9fd37-140">Példa:</span><span class="sxs-lookup"><span data-stu-id="9fd37-140">Example:</span></span>

```
<Gateway>
   <ConnectionsToLocalNetwork>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

###<span data-ttu-id="9fd37-141"><a name="lns"></a>Helyi hálózati helyek</span><span class="sxs-lookup"><span data-stu-id="9fd37-141"><a name="lns"></a>Local Network Sites</span></span>

<span data-ttu-id="9fd37-142">Távolítsa el a helyi telephely, amely már nem használ.</span><span class="sxs-lookup"><span data-stu-id="9fd37-142">Remove any local sites that you are no longer using.</span></span> <span data-ttu-id="9fd37-143">A létrehozott konfigurációjától függően lehetséges, hogy nincs egy **LocalNetworkSite** szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="9fd37-143">Depending on the configuration you created, it is possible that you don't have a **LocalNetworkSite** listed.</span></span>

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
  <LocalNetworkSite name="Site3">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>57.179.18.164</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

<span data-ttu-id="9fd37-144">Ebben a példában csak Site3 eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="9fd37-144">In this example, we removed only Site3.</span></span>

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

### <span data-ttu-id="9fd37-145"><a name="clientaddresss"></a>Ügyfél címkészlete</span><span class="sxs-lookup"><span data-stu-id="9fd37-145"><a name="clientaddresss"></a>Client AddressPool</span></span>

<span data-ttu-id="9fd37-146">Ha a virtuális hálózat kellett P2S kapcsolatot, akkor egy **VPNClientAddressPool**.</span><span class="sxs-lookup"><span data-stu-id="9fd37-146">If you had a P2S connection to your VNet, you will have a **VPNClientAddressPool**.</span></span> <span data-ttu-id="9fd37-147">Távolítsa el az ügyfél-címkészleteket, amelyek a virtuális hálózati átjáró törölt felelnek meg.</span><span class="sxs-lookup"><span data-stu-id="9fd37-147">Remove the client address pools that correspond to the virtual network gateway that you deleted.</span></span>

```
<Gateway>
    <VPNClientAddressPool>
      <AddressPrefix>10.1.0.0/24</AddressPrefix>
    </VPNClientAddressPool>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

<span data-ttu-id="9fd37-148">Példa:</span><span class="sxs-lookup"><span data-stu-id="9fd37-148">Example:</span></span>

```
<Gateway>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

### <span data-ttu-id="9fd37-149"><a name="gwsub"></a>A GatewaySubnet</span><span class="sxs-lookup"><span data-stu-id="9fd37-149"><a name="gwsub"></a>GatewaySubnet</span></span>

<span data-ttu-id="9fd37-150">Törölje a **GatewaySubnet** , amely megfelel a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="9fd37-150">Delete the **GatewaySubnet** that corresponds to the VNet.</span></span>

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
   <Subnet name="GatewaySubnet">
     <AddressPrefix>10.11.1.0/29</AddressPrefix>
   </Subnet>
 </Subnets>
```

<span data-ttu-id="9fd37-151">Példa:</span><span class="sxs-lookup"><span data-stu-id="9fd37-151">Example:</span></span>

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
 </Subnets>
```

## <span data-ttu-id="9fd37-152"><a name="upload"></a>5. lépés: A hálózati konfigurációs fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="9fd37-152"><a name="upload"></a>Step 5: Upload the network configuration file</span></span>

<span data-ttu-id="9fd37-153">A módosítások mentéséhez és a hálózati konfigurációs fájl feltöltése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="9fd37-153">Save your changes and upload the network configuration file to Azure.</span></span> <span data-ttu-id="9fd37-154">Győződjön meg arról, hogy a fájl elérési útját a környezet szükség szerint módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="9fd37-154">Make sure you change the file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="9fd37-155">Ha sikeres, a visszatérési valami hasonló ebben a példában látható:</span><span class="sxs-lookup"><span data-stu-id="9fd37-155">If successful, the return shows something similar to this example:</span></span>

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded