---
title: "az ügyfél virtuális hálózat az Azure RemoteApp telepített portok és URL-címek toowhitelist aaaList |} Microsoft Docs"
description: "Ismerje meg, melyik portokra és URL-címek tooconfigure az Azure RemoteApp-kommunikációra lesz szüksége."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: 5a001ff7-14c9-47fa-9b39-78fd5a5f0250
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 039866f7b64ac763ca833d66031ade3def1d3543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="list-of-ports-and-urls-toopermit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a><span data-ttu-id="2a176-103">Lista toopermit hozzáférési portok és URL-címek az Azure RemoteApp telepített ügyfelek virtuális hálózati</span><span class="sxs-lookup"><span data-stu-id="2a176-103">List of Ports and URLs toopermit access for Azure RemoteApp Deployed in customer Virtual Network</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2a176-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="2a176-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="2a176-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="2a176-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="2a176-106">Központi telepítése egy virtuális hálózatot (VNET) az Azure RemoteApp felhőalapú vagy hibrid gyűjteményt, tekintse át a következő portinformációkat hello.</span><span class="sxs-lookup"><span data-stu-id="2a176-106">If you are deploying an Azure RemoteApp cloud or hybrid collection in a virtual network (VNET), review hello following port information.</span></span> <span data-ttu-id="2a176-107">További információk a virtuális hálózatokon, [virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2a176-107">For more information on virtual networks, read [Virtual Network Overview](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="2a176-108">Ha létrehozott egy hálózati biztonsági csoport (NSG) korlátozása a forgalom toohello virtuális hálózati erőforrásokhoz a gyűjteményben, ellenőrizze, a következő portok hello elérhető és engedélyezett hello biztonsági házirendek hello virtuális hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2a176-108">If you have created a network security group (NSG) restricting traffic toohello virtual network resources in your collection, make sure hello following ports are accessible and allowed through hello security policies on hello virtual network.</span></span> <span data-ttu-id="2a176-109">Hálózati biztonsági csoportokkal kapcsolatos további információkért olvassa el a [Mi az a hálózati biztonsági csoport? (NSG) ](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="2a176-109">For more information on network security groups, read [What is a Network Security Group? (NSG)](../virtual-network/virtual-networks-nsg.md).</span></span>

## <a name="azure-remoteapp-subnet-needs-access-toothese-endpoints-and-urls"></a><span data-ttu-id="2a176-110">Azure RemoteApp-alhálózatban kell hozzáférést toothese végpontok és URL-címek:</span><span class="sxs-lookup"><span data-stu-id="2a176-110">Azure RemoteApp subnet needs access toothese endpoints and URLs:</span></span>
* <span data-ttu-id="2a176-111">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="2a176-111">*.servicebus.windows.net</span></span>
* <span data-ttu-id="2a176-112">*. servicebus.net</span><span class="sxs-lookup"><span data-stu-id="2a176-112">*.servicebus.net</span></span>
* <span data-ttu-id="2a176-113">https://*.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="2a176-113">https://*.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="2a176-114">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="2a176-114">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="2a176-115">https://*RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="2a176-115">https://*remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="2a176-116">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="2a176-116">https://*.core.windows.net</span></span>  
* <span data-ttu-id="2a176-117">Kimenő: TCP: TCP: 443, 9351, 9352, 10101-10175</span><span class="sxs-lookup"><span data-stu-id="2a176-117">Outbound: TCP: TCP: 443, 9351, 9352, 10101-10175</span></span> 
* <span data-ttu-id="2a176-118">Nem kötelező – UDP: 10201-10275</span><span class="sxs-lookup"><span data-stu-id="2a176-118">Optional – UDP: 10201-10275</span></span>  

## <a name="azure-remoteapp-clients-need-access-toothese-endpoints-and-urls"></a><span data-ttu-id="2a176-119">Az Azure RemoteApp-ügyfelek toothese végpontok és URL-címeket kell elérni:</span><span class="sxs-lookup"><span data-stu-id="2a176-119">Azure RemoteApp clients need access toothese endpoints and URLs:</span></span>
<span data-ttu-id="2a176-120">I jelenti stb hello asztali számítógépek, eszközök, amelyet az ügyfelek által használható tooconnect toohello alkalmazások üzembe helyezése a hello Azure RemoteApp-gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="2a176-120">By clients I mean hello desktops, devices etc. that people use tooconnect toohello apps deployed in hello Azure RemoteApp collection.</span></span>

* <span data-ttu-id="2a176-121">https://telemetry.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="2a176-121">https://telemetry.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="2a176-122">https://*.RemoteApp.windowsazure.com (hello választható UDP-portok vannak ehhez a címhez)</span><span class="sxs-lookup"><span data-stu-id="2a176-122">https://*.remoteapp.windowsazure.com (hello optional UDP ports are for this address)</span></span> 
* <span data-ttu-id="2a176-123">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="2a176-123">https://login.windows.net</span></span>  
* <span data-ttu-id="2a176-124">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="2a176-124">https://login.microsoftonline.com</span></span>  
* <span data-ttu-id="2a176-125">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="2a176-125">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="2a176-126">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="2a176-126">https://*.core.windows.net</span></span>  
* <span data-ttu-id="2a176-127">Kimenő: TCP: 443</span><span class="sxs-lookup"><span data-stu-id="2a176-127">Outbound: TCP: 443</span></span>  
* <span data-ttu-id="2a176-128">Választható - UDP: 3391</span><span class="sxs-lookup"><span data-stu-id="2a176-128">Optional - UDP: 3391</span></span> 

