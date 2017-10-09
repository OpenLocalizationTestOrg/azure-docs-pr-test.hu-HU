---
title: "az AWS tooAzure virtuális gépek aaaMigrate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toomigrate virtuális gépen fut az Azure Site Recovery segítségével Amazon Web Services (AWS) tooAzure."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: c99b781ec9cca5b8f9a847d3fc48408062b120b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-tooazure-with-azure-site-recovery"></a><span data-ttu-id="c6002-103">Az Azure Site Recovery Amazon Web Services (AWS) tooAzure lévő virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c6002-103">Migrate virtual machines in Amazon Web Services (AWS) tooAzure with Azure Site Recovery</span></span>

<span data-ttu-id="c6002-104">Ez a cikk ismerteti, hogyan toomigrate AWS Windows-példányok tooAzure virtuális gépek hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c6002-104">This article describes how toomigrate AWS Windows instances tooAzure virtual machines with hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="c6002-105">Az áttelepítés akkor hatékonyan a feladatátvételt az AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c6002-105">Migration is effectively a failover from AWS tooAzure.</span></span> <span data-ttu-id="c6002-106">Feladat-visszavételi gépek tooAWS nem lehet, és nincs folyamatban lévő replikáció van.</span><span class="sxs-lookup"><span data-stu-id="c6002-106">You can't failback machines tooAWS, and there's no ongoing replication.</span></span> <span data-ttu-id="c6002-107">Ez a cikk hello hello Azure-portálon az áttelepítési lépéseit mutatja be, és hello utasításokat alapuló [replikálni a fizikai gép tooAzure](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="c6002-107">This article describes hello steps for migration in hello Azure portal, and are based on hello instructions for [replicating a physical machine tooAzure](site-recovery-vmware-to-azure.md).</span></span>

<span data-ttu-id="c6002-108">Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="c6002-108">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="c6002-109">Támogatott operációs rendszerek</span><span class="sxs-lookup"><span data-stu-id="c6002-109">Supported operating systems</span></span>

<span data-ttu-id="c6002-110">A Site Recovery hello a következő operációs rendszereket futtató használt toomigrate EC2 példányok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="c6002-110">Site Recovery can be used toomigrate EC2 instances running any of hello following operating systems:</span></span>

- <span data-ttu-id="c6002-111">Windows (csak a 64 bites)</span><span class="sxs-lookup"><span data-stu-id="c6002-111">Windows(64 bit only)</span></span>
    - <span data-ttu-id="c6002-112">Windows Server 2008 R2 SP1 + (Citrix PV illesztőprogramok vagy csak az AWS PV illesztőprogramok.</span><span class="sxs-lookup"><span data-stu-id="c6002-112">Windows Server 2008 R2 SP1+ (Citrix PV drivers or AWS PV drivers only.</span></span> <span data-ttu-id="c6002-113">**RedHat PV illesztőprogramok futó példányok nem támogatottak**) Windows Server 2012 Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="c6002-113">**Instances running RedHat PV drivers are not supported**) Windows Server 2012 Windows Server 2012 R2</span></span>
- <span data-ttu-id="c6002-114">Linux (csak a 64 bites)</span><span class="sxs-lookup"><span data-stu-id="c6002-114">Linux (64 bit only)</span></span>
    - <span data-ttu-id="c6002-115">Red Hat Enterprise Linux 6.7 (csak HVM virtuális példány)</span><span class="sxs-lookup"><span data-stu-id="c6002-115">Red Hat Enterprise Linux 6.7 (HVM virtualized instances only)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6002-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c6002-116">Prerequisites</span></span>

<span data-ttu-id="c6002-117">Ez mit kell ehhez a központi telepítéshez:</span><span class="sxs-lookup"><span data-stu-id="c6002-117">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="c6002-118">**Konfigurációs kiszolgáló**: az Amazon EC2 virtuális gép Windows Server 2012 R2 rendszerű hello konfigurációs kiszolgálón van telepítve.</span><span class="sxs-lookup"><span data-stu-id="c6002-118">**Configuration server**: An Amazon EC2 VM running Windows Server 2012 R2 is deployed as hello configuration server.</span></span> <span data-ttu-id="c6002-119">Alapértelmezés szerint hello más Azure Site Recovery (a folyamatkiszolgáló és fő célkiszolgáló) ezeket az összetevőket hello konfigurációs kiszolgáló telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="c6002-119">By default, hello other Azure Site Recovery components (process server and master target server) are installed when you deploy hello configuration server.</span></span> <span data-ttu-id="c6002-120">Ez a cikk hello hello Azure-portálon az áttelepítési lépéseit mutatja be, és hello utasításokat alapuló [további](site-recovery-components.md)</span><span class="sxs-lookup"><span data-stu-id="c6002-120">This article describes hello steps for migration in hello Azure portal, and are based on hello instructions for  [Learn more](site-recovery-components.md)</span></span>

* <span data-ttu-id="c6002-121">**EC2 példányok**: hello Amazon EC2 virtuális gépek példányok, amelyet az toomigrate.</span><span class="sxs-lookup"><span data-stu-id="c6002-121">**EC2 instances**: hello Amazon EC2 virtual machines instances that you want toomigrate.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="c6002-122">A központi telepítés lépései</span><span class="sxs-lookup"><span data-stu-id="c6002-122">Deployment steps</span></span>

1. <span data-ttu-id="c6002-123">Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c6002-123">Create a Recovery Services vault.</span></span>
2. <span data-ttu-id="c6002-124">hello biztonsági csoport a EC2 példányok rendelkeznie kell beállított szabályokat tooallow kommunikációs hello EC2 példányt, amelyet toomigrate és hello példány tervezett toodeploy hello konfigurációs kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="c6002-124">hello Security Group of your EC2 instances should have rules configured tooallow communication between hello EC2 instance that you want toomigrate, and hello instance on which you plan toodeploy hello Configuration Server.</span></span>

3. <span data-ttu-id="c6002-125">Hello azonos Amazon virtuális Magánfelhő a EC2 példányként telepítse az automatikus rendszer-Helyreállítás konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c6002-125">On hello same Amazon Virtual Private Cloud as your EC2 instances, deploy an ASR configuration server.</span></span> <span data-ttu-id="c6002-126">Tekintse meg a hello VMware / fizikai konfigurációs kiszolgáló telepítési követelményei tooAzure előfeltételeit.</span><span class="sxs-lookup"><span data-stu-id="c6002-126">Refer hello VMware / Physical tooAzure prerequisites for configuration server deployment requirements.</span></span>

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  <span data-ttu-id="c6002-128">Amikor a konfigurációs kiszolgáló AWS üzembe helyezve, és a Recovery Services-tároló regisztrált, megtekintheti az hello konfigurációs kiszolgáló és a Site Recovery-infrastruktúra folyamatkiszolgáló alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="c6002-128">Once your configuration server is deployed in AWS and registered with your Recovery Services vault, you should see hello configuration server and process server under Site Recovery infrastructure as shown below:</span></span>

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. <span data-ttu-id="c6002-130">Hello konfigurációs kiszolgáló telepítése után (is igénybe vehet fel too15 minustes azt tooappear), ellenőrizze, hogy képes legyen kommunikálni hello virtuális gépeket, hogy szeretné-e toomigrate.</span><span class="sxs-lookup"><span data-stu-id="c6002-130">After you've deployed hello configuration server (it might take up too15 minustes for it tooappear), validate that it can communicate with hello VMs that you want toomigrate.</span></span>

6. <span data-ttu-id="c6002-131">[Replikációs beállítások megadása](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="c6002-131">[Set up replication settings](site-recovery-setup-replication-settings-vmware.md).</span></span>

7. <span data-ttu-id="c6002-132">Engedélyezze a replikálást: engedélyezze a hello toomigrate kívánt virtuális gépek replikációját.</span><span class="sxs-lookup"><span data-stu-id="c6002-132">Enable replication: Enable replication for hello VMs you want toomigrate.</span></span> <span data-ttu-id="c6002-133">Hello EC2 példányok hello magánhálózati IP-címek, hello EC2 konzolról csomagját segítségével felderíthetők.</span><span class="sxs-lookup"><span data-stu-id="c6002-133">You can discover hello EC2 instances using hello private IP addresses, which you can get from hello EC2 console.</span></span>

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. <span data-ttu-id="c6002-135">Hello EC2 példányokat a védett és hello replikációs tooAzure befejeződése után [feladatátvételi teszt futtatása](site-recovery-test-failover-to-azure.md) toovalidate az Azure-ban az alkalmazás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="c6002-135">Once hello EC2 instances have been protected and hello replication tooAzure is complete, [run a Test Failover](site-recovery-test-failover-to-azure.md) toovalidate your application's performance in Azure.</span></span>

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. <span data-ttu-id="c6002-137">Az AWS tooAzure feladatátvétel futtatása az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="c6002-137">Run a Failover from AWS tooAzure for each VM.</span></span> <span data-ttu-id="c6002-138">Szükség esetén a helyreállítási terv létrehozása, és a feladatátvétel, toomigrate több virtuális gép futtatása az AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c6002-138">Optionally, you can create a recovery plan and run a Failover, toomigrate multiple virtual machines from AWS tooAzure.</span></span> <span data-ttu-id="c6002-139">[További](site-recovery-create-recovery-plans.md) helyreállítási tervek.</span><span class="sxs-lookup"><span data-stu-id="c6002-139">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

10. <span data-ttu-id="c6002-140">Az áttelepítés feladatátvevő toocommit nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="c6002-140">For migration, you don't need toocommit a failover.</span></span> <span data-ttu-id="c6002-141">Ehelyett hello az áttelepítés végrehajtásához a beállítást választja, minden gép toomigrate kívánja.</span><span class="sxs-lookup"><span data-stu-id="c6002-141">Instead, you select hello Complete Migration option for each machine you want toomigrate.</span></span> <span data-ttu-id="c6002-142">az áttelepítés végrehajtásához művelet hello hello áttelepítési folyamat befejezését, hello gép replikálása eltávolítja, és leállítja a Site Recovery hello gép számlázási.</span><span class="sxs-lookup"><span data-stu-id="c6002-142">hello Complete Migration action finishes up hello migration process, removes replication for hello machine, and stops Site Recovery billing for hello machine.</span></span>

    ![Migrate (Áttelepítés)](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a><span data-ttu-id="c6002-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6002-144">Next steps</span></span>

- <span data-ttu-id="c6002-145">[Készítse elő az áttelepített gépek tooenable replikációs](site-recovery-azure-to-azure-after-migration.md) tooanother régió vész-helyreállítási igényekre.</span><span class="sxs-lookup"><span data-stu-id="c6002-145">[Prepare migrated machines tooenable replication](site-recovery-azure-to-azure-after-migration.md) tooanother region for disaster recovery needs.</span></span>
- <span data-ttu-id="c6002-146">Gondoskodjon számítási feladatai védelméről az [Azure virtuális gépek replikálásával](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="c6002-146">Start protecting your workloads by [replicating Azure virtual machines.](site-recovery-azure-to-azure.md)</span></span>
