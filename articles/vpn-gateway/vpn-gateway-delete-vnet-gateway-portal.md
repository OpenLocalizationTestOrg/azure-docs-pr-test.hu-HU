---
title: "A virtuális hálózati átjáró törlése: Azure-portálon: erőforrás-kezelő |} Microsoft Docs"
description: "Törölje a virtuális hálózati átjáró, az Azure portál használatával a Resource Manager üzembe helyezési modellben."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: 1d289c09465cb8d5e4bfa569441dffcbf562b3bf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a><span data-ttu-id="80de4-103">Törölje a virtuális hálózati átjáró, a portál használatával</span><span class="sxs-lookup"><span data-stu-id="80de4-103">Delete a virtual network gateway using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="80de4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="80de4-104">Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="80de4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="80de4-105">PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="80de4-106">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="80de4-106">PowerShell (classic)</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

<span data-ttu-id="80de4-107">Többféle különböző megközelítés közül választhat, ha törli a virtuális hálózati átjáró VPN gateway-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="80de4-107">There are a couple of different approaches you can take when you want to delete a virtual network gateway for a VPN gateway configuration.</span></span>

- <span data-ttu-id="80de4-108">Ha teljes tartalmának törlése, és kezdje újra a folyamatot, ahogy a gyorsítás esetében is egy tesztkörnyezetben, törölheti az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="80de4-108">If you want to delete everything and start over, as in the case of a test environment, you can delete the resource group.</span></span> <span data-ttu-id="80de4-109">Ha töröl egy erőforráscsoport, a csoportban lévő összes erőforrást törli.</span><span class="sxs-lookup"><span data-stu-id="80de4-109">When you delete a resource group, it deletes all the resources within the group.</span></span> <span data-ttu-id="80de4-110">Ez a módszer csak akkor javasolt, ha nem szeretné megtartani az erőforrások az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="80de4-110">This is method is only recommended if you don't want to keep any of the resources in the resource group.</span></span> <span data-ttu-id="80de4-111">Ezzel a megközelítéssel csak néhány erőforrásokat külön-külön nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="80de4-111">You can't selectively delete only a few resources using this approach.</span></span>

- <span data-ttu-id="80de4-112">Ha meg szeretné tartani a erőforrások az erőforráscsoportban, a virtuális hálózati átjáró törlése válik kicsit bonyolultabb.</span><span class="sxs-lookup"><span data-stu-id="80de4-112">If you want to keep some of the resources in your resource group, deleting a virtual network gateway becomes slightly more complicated.</span></span> <span data-ttu-id="80de4-113">A virtuális hálózati átjáró törlése előtt először törölnie kell az átjáró függő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="80de4-113">Before you can delete the virtual network gateway, you must first delete any resources that are dependent on the gateway.</span></span> <span data-ttu-id="80de4-114">A szükséges lépések attól függ, a létrehozott kapcsolatok és a tőle függő erőforrások, minden egyes kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="80de4-114">The steps you follow depend on the type of connections that you created and the dependent resources for each connection.</span></span>

## <a name="delete-a-vpn-gateway"></a><span data-ttu-id="80de4-115">VPN Gateway törlése</span><span class="sxs-lookup"><span data-stu-id="80de4-115">Delete a VPN gateway</span></span>

<span data-ttu-id="80de4-116">A virtuális hálózati átjáró törlése, először törölnie kell az egyes erőforrások, amelyek a virtuális hálózati átjáró vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="80de4-116">To delete a virtual network gateway, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="80de4-117">Erőforrások miatt függőségek meghatározott sorrendben kell törölni.</span><span class="sxs-lookup"><span data-stu-id="80de4-117">Resources must be deleted in a certain order due to dependencies.</span></span>

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

<span data-ttu-id="80de4-118">Ezen a ponton a virtuális hálózati átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="80de4-118">At this point, the virtual network gateway is deleted.</span></span> <span data-ttu-id="80de4-119">A következő lépések segítenek a már nem használt erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="80de4-119">The next steps help you delete any resources that are no longer being used.</span></span>

### <a name="to-delete-the-local-network-gateway"></a><span data-ttu-id="80de4-120">A helyi hálózati átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="80de4-120">To delete the local network gateway</span></span>

1. <span data-ttu-id="80de4-121">A **összes erőforrás**, keresse meg a helyi hálózati átjáró társított minden egyes kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="80de4-121">In **All resources**, locate the local network gateways that were associated with each connection.</span></span>
2. <span data-ttu-id="80de4-122">Az a **áttekintése** a helyi hálózati átjáró paneljén kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="80de4-122">On the **Overview** blade for the local network gateway, click **Delete**.</span></span>

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a><span data-ttu-id="80de4-123">A nyilvános IP-cím erőforrás az átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="80de4-123">To delete the Public IP address resource for the gateway</span></span>

1. <span data-ttu-id="80de4-124">A **összes erőforrás**, keresse meg a kapcsolódó az átjáró nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="80de4-124">In **All resources**, locate the Public IP address resource that was associated to the gateway.</span></span> <span data-ttu-id="80de4-125">Ha a virtuális hálózati átjáró aktív-aktív volt, két nyilvános IP-cím jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="80de4-125">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span> 
2. <span data-ttu-id="80de4-126">Az a **áttekintése** a nyilvános IP-cím lapján kattintson **törlése**, majd **Igen** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="80de4-126">On the **Overview** page for the Public IP address, click **Delete**, then **Yes** to confirm.</span></span>

### <a name="to-delete-the-gateway-subnet"></a><span data-ttu-id="80de4-127">Az átjáró-alhálózat törléséhez</span><span class="sxs-lookup"><span data-stu-id="80de4-127">To delete the gateway subnet</span></span>

1. <span data-ttu-id="80de4-128">A **összes erőforrás**, keresse meg a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="80de4-128">In **All resources**, locate the virtual network.</span></span> 
2. <span data-ttu-id="80de4-129">A a **alhálózatok** panelen kattintson a **GatewaySubnet**, majd kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="80de4-129">On the **Subnets** blade, click the **GatewaySubnet**, then click **Delete**.</span></span> 
3. <span data-ttu-id="80de4-130">Kattintson a **Igen** annak ellenőrzéséhez, hogy szeretné-e az átjáró-alhálózat törléséhez.</span><span class="sxs-lookup"><span data-stu-id="80de4-130">Click **Yes** to confirm that you want to delete the gateway subnet.</span></span>

## <span data-ttu-id="80de4-131"><a name="deleterg"></a>Az erőforráscsoport törlésével VPN-átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="80de4-131"><a name="deleterg"></a>Delete a VPN gateway by deleting the resource group</span></span>

<span data-ttu-id="80de4-132">Ha nem aggódik megőrzi az erőforrásokat az erőforráscsoport található, és szeretné elölről, törölheti a teljes erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="80de4-132">If you are not concerned about keeping any of your resources in the resource group and you just want to start over, you can delete an entire resource group.</span></span> <span data-ttu-id="80de4-133">Ez az gyorsan távolítson el minden.</span><span class="sxs-lookup"><span data-stu-id="80de4-133">This is a quick way to remove everything.</span></span> <span data-ttu-id="80de4-134">Az alábbi lépéseket csak a Resource Manager üzembe helyezési modellel vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="80de4-134">The following steps apply only to the Resource Manager deployment model.</span></span>

1. <span data-ttu-id="80de4-135">A **összes erőforrás**, keresse meg az erőforráscsoportot, és kattintson a panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="80de4-135">In **All resources**, locate the resource group and click to open the blade.</span></span>
2. <span data-ttu-id="80de4-136">Kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="80de4-136">Click **Delete**.</span></span> <span data-ttu-id="80de4-137">A Delete panelen megtekintheti a fertőzött erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="80de4-137">On the Delete blade, view the affected resources.</span></span> <span data-ttu-id="80de4-138">Győződjön meg arról, hogy valóban törli az összes ilyen erőforrásról.</span><span class="sxs-lookup"><span data-stu-id="80de4-138">Make sure that you want to delete all of these resources.</span></span> <span data-ttu-id="80de4-139">Ha nem, hajtsa végre a lépéseket a [törli a VPN-átjáró](#deletegw) Ez a cikk tetején.</span><span class="sxs-lookup"><span data-stu-id="80de4-139">If not, use the steps in [Delete a VPN gateway](#deletegw) at the top of this article.</span></span>
3. <span data-ttu-id="80de4-140">A folytatáshoz adja meg az erőforráscsoportot, törölje, majd kattintson a kívánt nevét **törlése**.</span><span class="sxs-lookup"><span data-stu-id="80de4-140">To proceed, type the name of the resource group that you want to delete, then click **Delete**.</span></span>