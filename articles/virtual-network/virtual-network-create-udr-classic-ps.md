---
title: "egy Azure-beli virtuális hálózatra - PowerShell – klasszikus útválasztás aaaControl |} Microsoft Docs"
description: "Megtudhatja, hogyan toocontrol útválasztást a PowerShell használatával Vnetek |} Klasszikus"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: d8d07c16-cbe5-4536-acd6-870269346fe3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 36edf263fb434d5fb13310d4324da20e57f016a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a><span data-ttu-id="07cc5-103">Szabályozhatja az Útválasztás és használni a virtuális készülékeket (klasszikus) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="07cc5-103">Control routing and use virtual appliances (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="07cc5-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="07cc5-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="07cc5-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="07cc5-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="07cc5-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="07cc5-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="07cc5-107">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="07cc5-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="07cc5-108">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="07cc5-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="07cc5-109">Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="07cc5-109">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="07cc5-110">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-resource-manager/resource-manager-deployment-model.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="07cc5-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="07cc5-111">Ez a cikk hello tetején egyik lehetőség kiválasztásával megtekintheti a különféle eszközök dokumentációit hello.</span><span class="sxs-lookup"><span data-stu-id="07cc5-111">You can view hello documentation for different tools by selecting an option at hello top of this article.</span></span> <span data-ttu-id="07cc5-112">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="07cc5-112">This article covers hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="07cc5-113">hello minta Azure PowerShell az alábbi parancsok várt már létrehozott egy egyszerű környezetben a fenti forgatókönyvben hello alapján.</span><span class="sxs-lookup"><span data-stu-id="07cc5-113">hello sample Azure PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="07cc5-114">Ha toorun hello parancsok ebben a dokumentumban megjelenített, hozzon létre hello környezet látható [VNet létrehozása (klasszikus) használatával PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).</span><span class="sxs-lookup"><span data-stu-id="07cc5-114">If you want toorun hello commands as they are displayed in this document, create hello environment shown in [create a VNet (classic) using PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="07cc5-115">Hello UDR hello előtér alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="07cc5-115">Create hello UDR for hello front end subnet</span></span>
<span data-ttu-id="07cc5-116">toocreate hello útválasztási táblázatot és hello előtérben lévő alhálózat alhálózat alapján hello forgatókönyv fenti, a szükséges útvonal kövesse a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="07cc5-116">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="07cc5-117">Futtassa a következő parancs toocreate hello hello előtér-alhálózat egy útválasztási táblázatot:</span><span class="sxs-lookup"><span data-stu-id="07cc5-117">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```powershell
    New-AzureRouteTable -Name UDR-FrontEnd -Location uswest `
    -Label "Route table for front end subnet"
    ```

2. <span data-ttu-id="07cc5-118">Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello háttér-alhálózat (192.168.2.0/24) toohello **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="07cc5-118">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```powershell
    Get-AzureRouteTable UDR-FrontEnd `
    |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. <span data-ttu-id="07cc5-119">Futtatási hello parancs tooassociate hello útvonaltáblában hello a következő **előtér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="07cc5-119">Run hello following command tooassociate hello route table with hello **FrontEnd** subnet:</span></span>

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName FrontEnd `
    -RouteTableName UDR-FrontEnd
    ```

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="07cc5-120">Hello UDR hello háttér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="07cc5-120">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="07cc5-121">toocreate hello útválasztási táblázatot és hello vissza a szükséges útvonal hello forgatókönyv alapján alhálózati végén, végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="07cc5-121">toocreate hello route table and route needed for hello back end subnet based on hello scenario, complete hello following steps:</span></span>

1. <span data-ttu-id="07cc5-122">Futtassa a következő parancs toocreate hello hello háttér-alhálózat egy útválasztási táblázatot:</span><span class="sxs-lookup"><span data-stu-id="07cc5-122">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```powershell
    New-AzureRouteTable -Name UDR-BackEnd `
    -Location uswest `
    -Label "Route table for back end subnet"
    ```

2. <span data-ttu-id="07cc5-123">Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello előtér-alhálózat (192.168.1.0/24) toohello **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="07cc5-123">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```powershell
    Get-AzureRouteTable UDR-BackEnd
    | Set-AzureRoute `
    -RouteName RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 `
    -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

3. <span data-ttu-id="07cc5-124">Futtatási hello parancs tooassociate hello útvonaltáblában hello a következő **háttér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="07cc5-124">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```powershell
    Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
    -SubnetName BackEnd `
    -RouteTableName UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-hello-fw1-vm"></a><span data-ttu-id="07cc5-125">A hello FW1 virtuális gép IP-továbbítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="07cc5-125">Enable IP forwarding on hello FW1 VM</span></span>

<span data-ttu-id="07cc5-126">tooenable IP-továbbítást a hello FW1 VM, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="07cc5-126">tooenable IP forwarding in hello FW1 VM, complete hello following steps:</span></span>

1. <span data-ttu-id="07cc5-127">Futtassa a következő parancs toocheck hello állapotának IP-továbbítás hello:</span><span class="sxs-lookup"><span data-stu-id="07cc5-127">Run hello following command toocheck hello status of IP forwarding:</span></span>

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Get-AzureIPForwarding
    ```

2. <span data-ttu-id="07cc5-128">Futtatási hello következő parancsot a tooenable IP-továbbítást hello *FW1* VM:</span><span class="sxs-lookup"><span data-stu-id="07cc5-128">Run hello following command tooenable IP forwarding for hello *FW1* VM:</span></span>

    ```powershell
    Get-AzureVM -Name FW1 -ServiceName TestRGFW `
    | Set-AzureIPForwarding -Enable
    ```
