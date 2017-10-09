---
title: "az affinitáscsoport tooa régióban (klasszikus) Azure virtuális hálózat aaaMigrate |} Microsoft Docs"
description: "Ismerje meg, hogyan toomigrate a virtuális hálózat (klasszikus) kapcsolatot csoportosítani tooa régióban."
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
ms.openlocfilehash: e3a5c0f21e748912e29e2e8d03f4295720e63637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-tooa-region"></a><span data-ttu-id="e470d-103">A virtuális hálózat (klasszikus) át egy affinitáscsoportba tooa terület</span><span class="sxs-lookup"><span data-stu-id="e470d-103">Migrate a virtual network (classic) from an affinity group tooa region</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e470d-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e470d-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="e470d-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="e470d-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="e470d-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello erőforrás-kezelő telepítési modellt használja.</span><span class="sxs-lookup"><span data-stu-id="e470d-106">Microsoft recommends that most new deployments use hello Resource Manager deployment model.</span></span>

<span data-ttu-id="e470d-107">Affinitáscsoportok győződjön meg arról, hogy erőforrásokat ugyanabban az affinitáscsoportban fizikailag közel vannak egymáshoz, ezen erőforrások gyorsabb toocommunicate engedélyezése-kiszolgálók által szolgáltatott hello jött létre.</span><span class="sxs-lookup"><span data-stu-id="e470d-107">Affinity groups ensure that resources created within hello same affinity group are physically hosted by servers that are close together, enabling these resources toocommunicate quicker.</span></span> <span data-ttu-id="e470d-108">Az elmúlt hello a virtuális hálózatok (klasszikus) létrehozására vonatkozó követelmény volt affinitáscsoportokra.</span><span class="sxs-lookup"><span data-stu-id="e470d-108">In hello past, affinity groups were a requirement for creating virtual networks (classic).</span></span> <span data-ttu-id="e470d-109">Ugyanakkor a hello hálózati kezelő szolgáltatás, amely kezeli a virtuális hálózatok (klasszikus) esetleg csak fog működni a fizikai kiszolgálók vagy a méretezési egység belül.</span><span class="sxs-lookup"><span data-stu-id="e470d-109">At that time, hello network manager service that managed virtual networks (classic) could only work within a set of physical servers or scale unit.</span></span> <span data-ttu-id="e470d-110">Az architektúra fejlesztései hello hatókör hálózati felügyeleti tooa régió növekedett.</span><span class="sxs-lookup"><span data-stu-id="e470d-110">Architectural improvements have increased hello scope of network management tooa region.</span></span>

<span data-ttu-id="e470d-111">Architekturális fejlesztéseknek miatt affinitáscsoportok már nem ajánlott, vagy a virtuális hálózatok (klasszikus) szükséges.</span><span class="sxs-lookup"><span data-stu-id="e470d-111">As a result of these architectural improvements, affinity groups are no longer recommended, or required for virtual networks (classic).</span></span> <span data-ttu-id="e470d-112">virtuális hálózatok (klasszikus) affinitáscsoportok hello használata régiók váltotta fel.</span><span class="sxs-lookup"><span data-stu-id="e470d-112">hello use of affinity groups for virtual networks (classic) is replaced by regions.</span></span> <span data-ttu-id="e470d-113">Virtuális hálózatok (klasszikus) régiók társított regionális virtuális hálózatokba nevezzük.</span><span class="sxs-lookup"><span data-stu-id="e470d-113">Virtual networks (classic) that are associated with regions are called regional virtual networks.</span></span>

<span data-ttu-id="e470d-114">Azt javasoljuk, hogy általában nem használ affinitáscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="e470d-114">We recommend that you don't use affinity groups in general.</span></span> <span data-ttu-id="e470d-115">Hello a virtuális hálózati követelményt, vezérelt affinitáscsoportok is fontos toouse tooensure erőforrások, például (klasszikus) számítási és tárolási (klasszikus), kerültek egymást.</span><span class="sxs-lookup"><span data-stu-id="e470d-115">Aside from hello virtual network requirement, affinity groups were also important toouse tooensure resources, such as compute (classic) and storage (classic), were placed near each other.</span></span> <span data-ttu-id="e470d-116">Azonban hello aktuális Azure hálózati architektúra elhelyezési követelménynek már nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="e470d-116">However, with hello current Azure network architecture, these placement requirements are no longer necessary.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e470d-117">Bár továbbra is technikailag lehetséges toocreate egy virtuális hálózatot, amely egy affinitáscsoporthoz tartozik, nincs tehát nincs kényszerítő ok toodo.</span><span class="sxs-lookup"><span data-stu-id="e470d-117">Although it is still technically possible toocreate a virtual network that is associated with an affinity group, there is no compelling reason toodo so.</span></span> <span data-ttu-id="e470d-118">Számos virtuális hálózati szolgáltatás, például a hálózati biztonsági csoportok, csak elérhető regionális virtuális hálózat használatával, és nem érhetők el az affinitáscsoportok társított virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="e470d-118">Many virtual network features, such as network security groups, are only available when using a regional virtual network, and are not available for virtual networks that are associated with affinity groups.</span></span>
> 
> 

## <a name="edit-hello-network-configuration-file"></a><span data-ttu-id="e470d-119">Hello hálózati konfigurációs fájl szerkesztése</span><span class="sxs-lookup"><span data-stu-id="e470d-119">Edit hello network configuration file</span></span>

1. <span data-ttu-id="e470d-120">Hello hálózati konfigurációs fájl exportálása.</span><span class="sxs-lookup"><span data-stu-id="e470d-120">Export hello network configuration file.</span></span> <span data-ttu-id="e470d-121">Hogyan tooexport a hálózati konfigurációs fájlt a PowerShell használatával, vagy hello Azure parancssori felület (CLI) 1.0-s toolearn lásd [hálózati konfigurációs fájl használatával virtuális hálózat konfigurálása](virtual-networks-using-network-configuration-file.md#export).</span><span class="sxs-lookup"><span data-stu-id="e470d-121">toolearn how tooexport a network configuration file using PowerShell or hello Azure command-line interface (CLI) 1.0, see [Configure a virtual network using a network configuration file](virtual-networks-using-network-configuration-file.md#export).</span></span>
2. <span data-ttu-id="e470d-122">Hello hálózati konfigurációs fájl szerkesztésével cseréje **AffinityGroup** rendelkező **hely**.</span><span class="sxs-lookup"><span data-stu-id="e470d-122">Edit hello network configuration file, replacing **AffinityGroup** with **Location**.</span></span> <span data-ttu-id="e470d-123">Megad egy Azure [régió](https://azure.microsoft.com/regions) a **hely**.</span><span class="sxs-lookup"><span data-stu-id="e470d-123">You specify an Azure [region](https://azure.microsoft.com/regions) for **Location**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e470d-124">Hello **hely** hello régió tartozik, amelyet a virtuális hálózat (klasszikus) társított hello affinitáscsoport.</span><span class="sxs-lookup"><span data-stu-id="e470d-124">hello **Location** is hello region that you specified for hello affinity group that is associated with your virtual network (classic).</span></span> <span data-ttu-id="e470d-125">Például, ha a virtuális hálózat (klasszikus) társítva, amely áttelepítésekor, USA nyugati régiója található affinitáscsoport a **hely** tooWest USA kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="e470d-125">For example, if your virtual network (classic) is associated with an affinity group that is located in West US, when you migrate, your **Location** must point tooWest US.</span></span> 
   > 
   > 
   
    <span data-ttu-id="e470d-126">A következő sorokat a hálózati konfigurációs fájlban, hello értékeket cserélje le a saját hello szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="e470d-126">Edit hello following lines in your network configuration file, replacing hello values with your own:</span></span> 
   
    <span data-ttu-id="e470d-127">**Régi érték:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\></span><span class="sxs-lookup"><span data-stu-id="e470d-127">**Old value:** \<VirtualNetworkSitename="VNetUSWest" AffinityGroup="VNetDemoAG"\></span></span> 
   
    <span data-ttu-id="e470d-128">**Új érték:** \<VirtualNetworkSitename = "VNetUSWest" Location "USA nyugati régiója" =\></span><span class="sxs-lookup"><span data-stu-id="e470d-128">**New value:** \<VirtualNetworkSitename="VNetUSWest" Location="West US"\></span></span>
3. <span data-ttu-id="e470d-129">A módosítások mentéséhez és [importálása](virtual-networks-using-network-configuration-file.md#import) hálózati konfiguráció tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="e470d-129">Save your changes and [import](virtual-networks-using-network-configuration-file.md#import) hello network configuration tooAzure.</span></span>

> [!NOTE]
> <span data-ttu-id="e470d-130">Ez az áttelepítés nem okoz leállási tooyour szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e470d-130">This migration does NOT cause any downtime tooyour services.</span></span>
> 
> 

## <a name="what-toodo-if-you-have-a-vm-classic-in-an-affinity-group"></a><span data-ttu-id="e470d-131">Milyen toodo, ha az affinitáscsoportokban lévő virtuális gép (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="e470d-131">What toodo if you have a VM (classic) in an affinity group</span></span>
<span data-ttu-id="e470d-132">VMs (klasszikus), amelyek jelenleg az affinitáscsoportokban lévő hello affinitás csoportból eltávolított toobe nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e470d-132">VMs (classic) that are currently in an affinity group do not need toobe removed from hello affinity group.</span></span> <span data-ttu-id="e470d-133">Miután a virtuális gép van telepítve, már telepített tooa egy méretezési egység.</span><span class="sxs-lookup"><span data-stu-id="e470d-133">Once a VM is deployed, it is deployed tooa single scale unit.</span></span> <span data-ttu-id="e470d-134">Affinitáscsoportok korlátozhatja az egy új virtuális gép üzembe helyezéséhez elérhető Virtuálisgép-méretek hello készletét, de bármely telepített meglévő virtuális gép már korlátozott toohello beállítása a virtuális gép méretének elérhető hello méretezési egység, mely hello virtuális gép van telepítve.</span><span class="sxs-lookup"><span data-stu-id="e470d-134">Affinity groups can restrict hello set of available VM sizes for a new VM deployment, but any existing VM that is deployed is already restricted toohello set of VM sizes available in hello scale unit in which hello VM is deployed.</span></span> <span data-ttu-id="e470d-135">Hello a virtuális gép már van telepített tooa méretezési egység, mert egy virtuális gép eltávolítása egy affinitáscsoporthoz nincs hatással van a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="e470d-135">Because hello VM is already deployed tooa scale unit, removing a VM from an affinity group has no effect on hello VM.</span></span>
