---
title: "Egy Azure virtuális hálózat (klasszikus) telepítenek át egy affinitáscsoporthoz egy régiót |} Microsoft Docs"
description: "Megtudhatja, hogyan kell áttelepíteni egy virtuális hálózat (klasszikus) affinitáscsoport egy régiót."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: b9b3bd0f2184ac85261166d5fe2ab67e1bf319d4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-to-a-region"></a><span data-ttu-id="7d119-103">Virtuális hálózat (klasszikus) telepítenek át egy affinitáscsoporthoz terület</span><span class="sxs-lookup"><span data-stu-id="7d119-103">Migrate a virtual network (classic) from an affinity group to a region</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7d119-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7d119-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="7d119-105">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="7d119-105">This article covers using the classic deployment model.</span></span> <span data-ttu-id="7d119-106">A Microsoft azt javasolja, hogy az új telepítések esetén használja a Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="7d119-106">Microsoft recommends that most new deployments use the Resource Manager deployment model.</span></span>

<span data-ttu-id="7d119-107">Affinitáscsoportok győződjön meg arról, hogy erőforrásokat ugyanabban az affinitáscsoportban jött létre, amely közel vannak egymáshoz, ezekhez az erőforrásokhoz való kommunikációhoz gyorsabb engedélyezése kiszolgálók fizikailag működtetnek.</span><span class="sxs-lookup"><span data-stu-id="7d119-107">Affinity groups ensure that resources created within the same affinity group are physically hosted by servers that are close together, enabling these resources to communicate quicker.</span></span> <span data-ttu-id="7d119-108">A múltban a virtuális hálózatok (klasszikus) létrehozására vonatkozó követelmény volt affinitáscsoportokra.</span><span class="sxs-lookup"><span data-stu-id="7d119-108">In the past, affinity groups were a requirement for creating virtual networks (classic).</span></span> <span data-ttu-id="7d119-109">Idő lejárta után a virtuális hálózatok (klasszikus) kezelt hálózati manager szolgáltatás esetleg csak fog működni a fizikai kiszolgálók vagy a méretezési egység belül.</span><span class="sxs-lookup"><span data-stu-id="7d119-109">At that time, the network manager service that managed virtual networks (classic) could only work within a set of physical servers or scale unit.</span></span> <span data-ttu-id="7d119-110">Az architektúra fejlesztései régióban hálózati kezelési hatókör növekedett.</span><span class="sxs-lookup"><span data-stu-id="7d119-110">Architectural improvements have increased the scope of network management to a region.</span></span>

<span data-ttu-id="7d119-111">Architekturális fejlesztéseknek miatt affinitáscsoportok már nem ajánlott, vagy a virtuális hálózatok (klasszikus) szükséges.</span><span class="sxs-lookup"><span data-stu-id="7d119-111">As a result of these architectural improvements, affinity groups are no longer recommended, or required for virtual networks (classic).</span></span> <span data-ttu-id="7d119-112">Virtuális hálózatok (klasszikus) az affinitáscsoportok használatát régiók váltotta fel.</span><span class="sxs-lookup"><span data-stu-id="7d119-112">The use of affinity groups for virtual networks (classic) is replaced by regions.</span></span> <span data-ttu-id="7d119-113">Virtuális hálózatok (klasszikus) régiók társított regionális virtuális hálózatokba nevezzük.</span><span class="sxs-lookup"><span data-stu-id="7d119-113">Virtual networks (classic) that are associated with regions are called regional virtual networks.</span></span>

<span data-ttu-id="7d119-114">Azt javasoljuk, hogy általában nem használ affinitáscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="7d119-114">We recommend that you don't use affinity groups in general.</span></span> <span data-ttu-id="7d119-115">A virtuális hálózati követelmény vezérelt affinitáscsoportok is fontos, hogy használatával győződjön meg arról, például (klasszikus) számítási és tárolási (klasszikus) erőforrásokhoz, kerültek egymást.</span><span class="sxs-lookup"><span data-stu-id="7d119-115">Aside from the virtual network requirement, affinity groups were also important to use to ensure resources, such as compute (classic) and storage (classic), were placed near each other.</span></span> <span data-ttu-id="7d119-116">Azonban az aktuális Azure hálózati architektúra elhelyezési követelménynek már nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="7d119-116">However, with the current Azure network architecture, these placement requirements are no longer necessary.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7d119-117">Bár továbbra is technikailag lehetséges hozzon létre egy virtuális hálózatot, amely egy affinitáscsoporthoz tartozik, nincs ehhez kényszerítő ok.</span><span class="sxs-lookup"><span data-stu-id="7d119-117">Although it is still technically possible to create a virtual network that is associated with an affinity group, there is no compelling reason to do so.</span></span> <span data-ttu-id="7d119-118">Számos virtuális hálózati szolgáltatás, például a hálózati biztonsági csoportok, csak elérhető regionális virtuális hálózat használatával, és nem érhetők el az affinitáscsoportok társított virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="7d119-118">Many virtual network features, such as network security groups, are only available when using a regional virtual network, and are not available for virtual networks that are associated with affinity groups.</span></span>
> 
> 

## <a name="edit-the-network-configuration-file"></a><span data-ttu-id="7d119-119">A hálózati konfigurációs fájl szerkesztése</span><span class="sxs-lookup"><span data-stu-id="7d119-119">Edit the network configuration file</span></span>

1. <span data-ttu-id="7d119-120">A hálózati konfigurációs fájl exportálása.</span><span class="sxs-lookup"><span data-stu-id="7d119-120">Export the network configuration file.</span></span> <span data-ttu-id="7d119-121">A PowerShell vagy az Azure parancssori felület (CLI) 1.0-s hálózati konfigurációs fájl exportálása további tudnivalókért lásd: [hálózati konfigurációs fájl használatával virtuális hálózat konfigurálása](virtual-networks-using-network-configuration-file.md#export).</span><span class="sxs-lookup"><span data-stu-id="7d119-121">To learn how to export a network configuration file using PowerShell or the Azure command-line interface (CLI) 1.0, see [Configure a virtual network using a network configuration file](virtual-networks-using-network-configuration-file.md#export).</span></span>
2. <span data-ttu-id="7d119-122">A hálózati konfigurációs fájl szerkesztésével cseréje **AffinityGroup** rendelkező **hely**.</span><span class="sxs-lookup"><span data-stu-id="7d119-122">Edit the network configuration file, replacing **AffinityGroup** with **Location**.</span></span> <span data-ttu-id="7d119-123">Megad egy Azure [régió](https://azure.microsoft.com/regions) a **hely**.</span><span class="sxs-lookup"><span data-stu-id="7d119-123">You specify an Azure [region](https://azure.microsoft.com/regions) for **Location**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7d119-124">A **hely** a régió, az affinitáscsoport, a virtuális hálózat (klasszikus) társított megadott.</span><span class="sxs-lookup"><span data-stu-id="7d119-124">The **Location** is the region that you specified for the affinity group that is associated with your virtual network (classic).</span></span> <span data-ttu-id="7d119-125">Például, ha a virtuális hálózat (klasszikus) társítva, amely áttelepítésekor, USA nyugati régiója található affinitáscsoport a **hely** USA nyugati régiója kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="7d119-125">For example, if your virtual network (classic) is associated with an affinity group that is located in West US, when you migrate, your **Location** must point to West US.</span></span> 
   > 
   > 
   
    <span data-ttu-id="7d119-126">Szerkessze a következő sorokat az az értékeket a saját cserélje le a hálózati konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="7d119-126">Edit the following lines in your network configuration file, replacing the values with your own:</span></span> 
   
    <span data-ttu-id="7d119-127">**Régi érték:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\></span><span class="sxs-lookup"><span data-stu-id="7d119-127">**Old value:** \<VirtualNetworkSitename="VNetUSWest" AffinityGroup="VNetDemoAG"\></span></span> 
   
    <span data-ttu-id="7d119-128">**Új érték:** \<VirtualNetworkSitename = "VNetUSWest" Location "USA nyugati régiója" =\></span><span class="sxs-lookup"><span data-stu-id="7d119-128">**New value:** \<VirtualNetworkSitename="VNetUSWest" Location="West US"\></span></span>
3. <span data-ttu-id="7d119-129">A módosítások mentéséhez és [importálása](virtual-networks-using-network-configuration-file.md#import) a hálózati konfigurációt, az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="7d119-129">Save your changes and [import](virtual-networks-using-network-configuration-file.md#import) the network configuration to Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="7d119-130">Az áttelepítés esetén nem tapasztalnak állásidőt a szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="7d119-130">This migration does NOT cause any downtime to your services.</span></span>
> 
> 

## <a name="what-to-do-if-you-have-a-vm-classic-in-an-affinity-group"></a><span data-ttu-id="7d119-131">Mi a teendő, ha az affinitáscsoportokban lévő virtuális gép (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="7d119-131">What to do if you have a VM (classic) in an affinity group</span></span>
<span data-ttu-id="7d119-132">VMs (klasszikus), amely jelenleg affinitáscsoportokban nem távolítható el az affinitáscsoport kell.</span><span class="sxs-lookup"><span data-stu-id="7d119-132">VMs (classic) that are currently in an affinity group do not need to be removed from the affinity group.</span></span> <span data-ttu-id="7d119-133">Ha egy virtuális Gépet telepít, a rendszer egy méretezési egység van telepítve.</span><span class="sxs-lookup"><span data-stu-id="7d119-133">Once a VM is deployed, it is deployed to a single scale unit.</span></span> <span data-ttu-id="7d119-134">Affinitáscsoportok lehet korlátozni elérhető Virtuálisgép-méretek egy új virtuális gép üzembe helyezéséhez, de bármely telepített meglévő virtuális gép már korlátozott, hogy a Virtuálisgép-méretek érhető el a méretezési egység, amely a virtuális gép van telepítve.</span><span class="sxs-lookup"><span data-stu-id="7d119-134">Affinity groups can restrict the set of available VM sizes for a new VM deployment, but any existing VM that is deployed is already restricted to the set of VM sizes available in the scale unit in which the VM is deployed.</span></span> <span data-ttu-id="7d119-135">A virtuális gép már telepítve van a méretezési egység, mert a virtuális gép eltávolítása egy affinitáscsoporthoz hatástalan, a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="7d119-135">Because the VM is already deployed to a scale unit, removing a VM from an affinity group has no effect on the VM.</span></span>
