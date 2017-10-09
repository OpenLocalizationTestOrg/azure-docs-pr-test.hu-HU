---
title: "aaaMigrate Azure IaaS virtuális gépek Azure-régiók közötti |} Microsoft Docs"
description: "Azure Site Recovery toomigrate Azure IaaS virtuális gépeket egy Azure-régiót tooanother használja."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: c84dc77716b8d19969eab60707ed1332ca39b893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a><span data-ttu-id="25854-103">Az Azure Site Recovery Azure-régiók közötti Azure IaaS virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="25854-103">Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery</span></span>
## <a name="overview"></a><span data-ttu-id="25854-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="25854-104">Overview</span></span>
<span data-ttu-id="25854-105">Üdvözli a Site Recovery tooAzure!</span><span class="sxs-lookup"><span data-stu-id="25854-105">Welcome tooAzure Site Recovery!</span></span> <span data-ttu-id="25854-106">Ez a cikk használja, ha azt szeretné, hogy toomigrate Azure virtuális gépek Azure-régiók közötti.</span><span class="sxs-lookup"><span data-stu-id="25854-106">Use this article if you want toomigrate Azure VMs between Azure regions.</span></span> <span data-ttu-id="25854-107">Mielőtt elkezdené, vegye figyelembe, hogy:</span><span class="sxs-lookup"><span data-stu-id="25854-107">Before you start, note that:</span></span>

* <span data-ttu-id="25854-108">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: Azure Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="25854-108">Azure has two different deployment models for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="25854-109">Azure a két portál – hello a klasszikus Azure portálra, hello klasszikus üzembe helyezési modellt támogató, és a két üzembe helyezési modell támogató Azure-portálon hello is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="25854-109">Azure also has two portals – hello Azure classic portal that supports hello classic deployment model, and hello Azure portal with support for both deployment models.</span></span> <span data-ttu-id="25854-110">hello alapvető lépéseit az áttelepítés: hello azonos e Site Recovery konfigurálja az erőforrás-kezelő vagy a klasszikus.</span><span class="sxs-lookup"><span data-stu-id="25854-110">hello basic steps for migration are hello same whether you're configuring Site Recovery in Resource Manager or in classic.</span></span> <span data-ttu-id="25854-111">Azonban a felhasználói felület utasításokat hello és képernyőképeket ebben a cikkben az Azure-portálon hello vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="25854-111">However hello UI instructions and screenshots in this article are relevant for hello Azure portal.</span></span>
* <span data-ttu-id="25854-112">**Jelenleg csak áttelepíthetők egy régió tartozik tooanother. Virtuális gépek az Azure-régió, egy tooanother átveheti, de Ön visszavétel nem újra.**</span><span class="sxs-lookup"><span data-stu-id="25854-112">**Currently you can only migrate from one region tooanother. You can fail over VMs from one Azure region tooanother, but you can't fail them back again.**</span></span>

<span data-ttu-id="25854-113">Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="25854-113">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25854-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="25854-114">Prerequisites</span></span>
<span data-ttu-id="25854-115">Ez mit kell ehhez a központi telepítéshez:</span><span class="sxs-lookup"><span data-stu-id="25854-115">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="25854-116">**Infrastruktúra-szolgáltatási virtuális gépeket**: hello toomigrate kívánt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="25854-116">**IaaS virtual machines**: hello VMs you want toomigrate.</span></span> <span data-ttu-id="25854-117">Telepít át virtuális gépeken való kezelésével azokat a fizikai gépként.</span><span class="sxs-lookup"><span data-stu-id="25854-117">You migrate these VMs by treating them as physical machines.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="25854-118">A központi telepítés lépései</span><span class="sxs-lookup"><span data-stu-id="25854-118">Deployment steps</span></span>
<span data-ttu-id="25854-119">Ez a szakasz ismerteti a hello üzembe helyezés lépései hello új Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="25854-119">This section describes hello deployment steps in hello new Azure portal.</span></span>

1. <span data-ttu-id="25854-120">[Hozzon létre egy tárolót](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="25854-120">[Create a vault](site-recovery-vmware-to-azure.md).</span></span>
2. <span data-ttu-id="25854-121">[Engedélyezze a replikálást](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="25854-121">[Enable replication](site-recovery-vmware-to-azure.md).</span></span> <span data-ttu-id="25854-122">Engedélyezze a hello szeretné, hogy toomigrate, és válassza ki a forrásként Azure virtuális gépek replikációját.</span><span class="sxs-lookup"><span data-stu-id="25854-122">Enable replication for hello VMs you want toomigrate, and choose Azure as source.</span></span> 
3. <span data-ttu-id="25854-123">[A nem tervezett feladatátvétel](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="25854-123">[ Run an unplanned failover](site-recovery-failover.md).</span></span> <span data-ttu-id="25854-124">Kezdeti replikáció befejezése után egy Azure-régiót tooanother futtathatja egy nem tervezett feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="25854-124">After initial replication is complete, you can run an unplanned failover from one Azure region tooanother.</span></span> <span data-ttu-id="25854-125">Szükség esetén a helyreállítási terv létrehozása, és nem tervezett feladatátvétel, toomigrate több virtuális gép régiók között.</span><span class="sxs-lookup"><span data-stu-id="25854-125">Optionally, you can create a recovery plan and run an unplanned failover, toomigrate multiple virtual machines between regions.</span></span> <span data-ttu-id="25854-126">[További](site-recovery-create-recovery-plans.md) helyreállítási tervek.</span><span class="sxs-lookup"><span data-stu-id="25854-126">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25854-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25854-127">Next steps</span></span>
<span data-ttu-id="25854-128">További információ található más fájlreplikációs forgatókönyvek [Mi az Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="25854-128">Learn more about other replication scenarios in [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>
