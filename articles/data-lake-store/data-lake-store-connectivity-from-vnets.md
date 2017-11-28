---
title: "Csatlakozás az Azure Data Lake Store a Vnetek |} Microsoft Docs"
description: "Csatlakozás az Azure Data Lake Store az Azure Vnetekhez"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ff7d28d7b53e872b804788647b1e672fafcf6995
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a><span data-ttu-id="aeb97-103">Az Azure Data Lake Store egy Azure virtuális hálózaton belüli virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="aeb97-103">Access Azure Data Lake Store from VMs within an Azure VNET</span></span>
<span data-ttu-id="aeb97-104">Azure Data Lake Store PaaS szolgáltatás, amely a nyilvános Internet IP-címek.</span><span class="sxs-lookup"><span data-stu-id="aeb97-104">Azure Data Lake Store is a PaaS service that runs on public Internet IP addresses.</span></span> <span data-ttu-id="aeb97-105">Bármely kiszolgáló csatlakozni tud-e a nyilvános internethez általában csatlakozhat, valamint az Azure Data Lake Store-végpontok.</span><span class="sxs-lookup"><span data-stu-id="aeb97-105">Any server that can connect to the public Internet can typically connect to Azure Data Lake Store endpoints as well.</span></span> <span data-ttu-id="aeb97-106">Alapértelmezés szerint az Azure Vnetekhez lévő összes virtuális gép hozzáférhet az interneten, és ezért férhetnek hozzá az Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="aeb97-106">By default, all VMs that are in Azure VNETs can access the Internet and hence can access Azure Data Lake Store.</span></span> <span data-ttu-id="aeb97-107">Azonban úgy is egy VNET és nem rendelkezik Internet-hozzáférés a virtuális gépek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="aeb97-107">However, it is possible to configure VMs in a VNET to not have access to the Internet.</span></span> <span data-ttu-id="aeb97-108">Ilyen virtuális gépek elérése az Azure Data Lake Store történik is.</span><span class="sxs-lookup"><span data-stu-id="aeb97-108">For such VMs, access to Azure Data Lake Store is restricted as well.</span></span> <span data-ttu-id="aeb97-109">Nyilvános Internet-hozzáférés letiltása a virtuális gépek az Azure Vnetekhez végezhető a következő módon használja.</span><span class="sxs-lookup"><span data-stu-id="aeb97-109">Blocking public Internet access for VMs in Azure VNETs can be done using any of the following approach.</span></span>

* <span data-ttu-id="aeb97-110">Úgy konfigurálja a hálózati biztonsági csoportok (NSG)</span><span class="sxs-lookup"><span data-stu-id="aeb97-110">By configuring Network Security Groups (NSG)</span></span>
* <span data-ttu-id="aeb97-111">Úgy konfigurálja a felhasználó definiált útvonalakat (UDR)</span><span class="sxs-lookup"><span data-stu-id="aeb97-111">By configuring User Defined Routes (UDR)</span></span>
* <span data-ttu-id="aeb97-112">Útvonalak BGP (industry standard dinamikus útválasztási protokoll) útján kicserélésével ExpressRoute használatakor, amelyek blokkolják az Internet-hozzáférés</span><span class="sxs-lookup"><span data-stu-id="aeb97-112">By exchanging routes via BGP (industry standard dynamic routing protocol) when ExpressRoute is used that block access to the Internet</span></span>

<span data-ttu-id="aeb97-113">Ebből a cikkből megismerheti, hogyan hozzáférés engedélyezése az Azure Data Lake Store az Azure virtuális gépek, amelyek hozzáférését az erőforrásokhoz a fenti három módszer egyikével korlátozott lesz.</span><span class="sxs-lookup"><span data-stu-id="aeb97-113">In this article, you will learn how to enable access to the Azure Data Lake Store from Azure VMs which have been restricted to access resources using one of the three methods listed above.</span></span>

## <a name="enabling-connectivity-to-azure-data-lake-store-from-vms-with-restricted-connectivity"></a><span data-ttu-id="aeb97-114">Azure Data Lake Store engedélyezése kapcsolat korlátozott kapcsolattal rendelkező virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="aeb97-114">Enabling connectivity to Azure Data Lake Store from VMs with restricted connectivity</span></span>
<span data-ttu-id="aeb97-115">Azure Data Lake Store ilyen virtuális gépek eléréséhez konfigurálnia kell azokat az IP-cím, amennyiben rendelkezésre áll-e az Azure Data Lake Store-fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="aeb97-115">To access Azure Data Lake Store from such VMs, you must configure them to access the IP address where the Azure Data Lake Store account is available.</span></span> <span data-ttu-id="aeb97-116">Az IP-címek a Data Lake Store-fiókok alapján azonosíthatja a fiókok a DNS-nevek feloldása (`<account>.azuredatalakestore.net`).</span><span class="sxs-lookup"><span data-stu-id="aeb97-116">You can identify the IP addresses for your Data Lake Store accounts by resolving the DNS names of your accounts (`<account>.azuredatalakestore.net`).</span></span> <span data-ttu-id="aeb97-117">Ehhez használhatja például a **nslookup**.</span><span class="sxs-lookup"><span data-stu-id="aeb97-117">For this you can use tools such as **nslookup**.</span></span> <span data-ttu-id="aeb97-118">Nyisson meg egy parancssort a számítógépen, és futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="aeb97-118">Open a command prompt on your computer and run the following command.</span></span>

    nslookup mydatastore.azuredatalakestore.net

<span data-ttu-id="aeb97-119">A kimenet az alábbihoz hasonló.</span><span class="sxs-lookup"><span data-stu-id="aeb97-119">The output resembles the following.</span></span> <span data-ttu-id="aeb97-120">Az érték alapján **cím** tulajdonság a Data Lake Store-fiókjához társított IP-címét.</span><span class="sxs-lookup"><span data-stu-id="aeb97-120">The value against **Address** property is the IP address associated with your Data Lake Store account.</span></span>

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a><span data-ttu-id="aeb97-121">Virtuális gépek NSG szolgáltatással kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="aeb97-121">Enabling connectivity from VMs restricted by using NSG</span></span>
<span data-ttu-id="aeb97-122">Használatakor egy NSG-szabály Internet-hozzáférés letiltása, majd létrehozhat egy másik NSG-t, amely lehetővé teszi a hozzáférést a Data Lake Store IP-cím.</span><span class="sxs-lookup"><span data-stu-id="aeb97-122">When a NSG rule is used to block access to the Internet, then you can create another NSG that allows access to the Data Lake Store IP Address.</span></span> <span data-ttu-id="aeb97-123">További információt az NSG-szabályok [Mi az a hálózati biztonsági csoport?](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="aeb97-123">More information on NSG rules is available at [What is a Network Security Group?](../virtual-network/virtual-networks-nsg.md).</span></span> <span data-ttu-id="aeb97-124">További információ az NSG-k létrehozásához [kezelése az Azure portál használatával NSG-k](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="aeb97-124">For instructions on how to create NSGs see [How to manage NSGs using the Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span></span>

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a><span data-ttu-id="aeb97-125">Virtuális gépek szolgáltatással UDR vagy ExpressRoute kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="aeb97-125">Enabling connectivity from VMs restricted by using UDR or ExpressRoute</span></span>
<span data-ttu-id="aeb97-126">Útvonalak udr-EK vagy a BGP-kicserélt útvonalakat, Internet-hozzáférés letiltása használata esetén egy különös útvonalat kell konfigurálni, hogy az ilyen alhálózatban lévő virtuális gépek hozzáférhessenek a Data Lake Store-végpontok.</span><span class="sxs-lookup"><span data-stu-id="aeb97-126">When routes, either UDRs or BGP-exchanged routes, are used to block access to the Internet, a special route needs to be configured so that VMs in such subnets can access Data Lake Store endpoints.</span></span> <span data-ttu-id="aeb97-127">További információkért lásd: [Mik azok a felhasználó által megadott útvonalak?](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aeb97-127">For more information, see [What are User Defined Routes?](../virtual-network/virtual-networks-udr-overview.md).</span></span> <span data-ttu-id="aeb97-128">Udr-EK létrehozásával kapcsolatos utasításokért lásd: [létrehozása udr a Resource Manager-EK](../virtual-network/virtual-network-create-udr-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="aeb97-128">For instructions on creating UDRs, see [Create UDRs in Resource Manager](../virtual-network/virtual-network-create-udr-arm-ps.md).</span></span>

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a><span data-ttu-id="aeb97-129">Virtuális gépek szolgáltatással ExpressRoute kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="aeb97-129">Enabling connectivity from VMs restricted by using ExpressRoute</span></span>
<span data-ttu-id="aeb97-130">Ha ExpressRoute-kapcsolatcsoportot van konfigurálva, a helyszíni kiszolgálók elérhető Data Lake Store nyilvános társviszony keresztül.</span><span class="sxs-lookup"><span data-stu-id="aeb97-130">When an ExpressRoute circuit is configured, the on-premises servers can access Data Lake Store through public peering.</span></span> <span data-ttu-id="aeb97-131">További részleteket a nyilvános társviszony található ExpressRoute konfigurálása [ExpressRoute – gyakori kérdések](../expressroute/expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="aeb97-131">More details on configuring ExpressRoute for public peering is available at [ExpressRoute FAQs](../expressroute/expressroute-faqs.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="aeb97-132">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="aeb97-132">See also</span></span>
* [<span data-ttu-id="aeb97-133">Az Azure Data Lake Store áttekintése</span><span class="sxs-lookup"><span data-stu-id="aeb97-133">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="aeb97-134">Azure Data Lake Store-ban tárolt adatok védelme</span><span class="sxs-lookup"><span data-stu-id="aeb97-134">Securing data stored in Azure Data Lake Store</span></span>](data-lake-store-security-overview.md)

